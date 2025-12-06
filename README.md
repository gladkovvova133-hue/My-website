<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>Регистрация + Анонимные вопросы</title>
<style>
body {
    font-family: Arial;
    background: #0f1115;
    color: white;
    display: flex;
    justify-content: center;
    align-items: flex-start;
    min-height: 100vh;
    padding-top: 50px;
    margin: 0;
}
.container {
    background: #1a1d23;
    padding: 25px;
    border-radius: 12px;
    width: 450px;
    margin: 10px;
}
input, textarea {
    width: 100%;
    border-radius: 6px;
    border: none;
    padding: 10px;
    margin-bottom: 10px;
}
textarea { height: 80px; resize: none; }
button {
    width: 100%;
    padding: 12px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    background: #0d6efd;
    color: white;
    font-size: 16px;
    margin-bottom: 10px;
}
button:hover { background: #0b5cd4; }
#msg { margin-top: 10px; min-height: 20px; }
.question {
    background: #2a2d35;
    padding: 10px;
    border-radius: 6px;
    margin-top: 8px;
}
.read-btn { background: #28a745; margin-top: 5px; font-size: 12px; }
.read-btn:hover { background: #218838; }
.reply-btn { background: #ffc107; margin-top: 5px; font-size: 12px; color: black; }
.reply-btn:hover { background: #e0a800; }
.reply-text { margin-top:5px; font-style: italic; color: #00ff80; }
.hidden { display: none; }
</style>
</head>
<body>

<!-- === Регистрация / Вход === -->
<div class="container" id="regPanel">
    <h2>Регистрация / Вход</h2>
    <input id="regNick" placeholder="Ваш ник">
    <input id="regPass" type="password" placeholder="Пароль">
    <button onclick="registerUser()">Зарегистрироваться</button>
    <button onclick="loginUser()">Войти</button>
    <div id="msg"></div>
</div>

<!-- === Форма вопросов (видна после входа) === -->
<div class="container hidden" id="userPanel">
    <h2>Анонимный вопрос админам</h2>
    <textarea id="question" placeholder="Введите свой вопрос..."></textarea>
    <button onclick="sendQuestion()">Отправить</button>
</div>

<!-- === Панель админа (Carlo) === -->
<div class="container hidden" id="adminPanel">
    <h2>Панель админа (Carlo)</h2>
    <div id="questionsList"></div>
</div>

<script>
// === Работа с пользователями ===
function loadUsers() {
    let data = localStorage.getItem("usersDB");
    if(!data) return [];
    try { return JSON.parse(data); } catch { return []; }
}
function saveUsers(db) {
    localStorage.setItem("usersDB", JSON.stringify(db));
}

function registerUser() {
    let nick = document.getElementById("regNick").value.trim();
    let pass = document.getElementById("regPass").value.trim();
    let msg = document.getElementById("msg");
    if(!nick || !pass){ msg.innerHTML="Введите ник и пароль!"; msg.style.color="#ff4d4d"; return; }

    let users = loadUsers();
    if(users.some(u=>u.nick.toLowerCase()===nick.toLowerCase())) {
        msg.innerHTML="Ник уже занят!"; msg.style.color="#ff4d4d"; return;
    }

    users.push({nick, pass});
    saveUsers(users);
    localStorage.setItem("nick", nick);
    msg.innerHTML="Регистрация успешна!";
    msg.style.color="#00ff80";
    openQuestionPanel();
}

function loginUser() {
    let nick = document.getElementById("regNick").value.trim();
    let pass = document.getElementById("regPass").value.trim();
    let msg = document.getElementById("msg");
    let users = loadUsers();
    let user = users.find(u=>u.nick.toLowerCase()===nick.toLowerCase());

    if(!user){ msg.innerHTML="Пользователь не найден!"; msg.style.color="#ff4d4d"; return; }
    if(user.pass!==pass){ msg.innerHTML="Неверный пароль!"; msg.style.color="#ff4d4d"; return; }

    localStorage.setItem("nick", nick);
    msg.innerHTML="Вход успешен!";
    msg.style.color="#00ff80";
    openQuestionPanel();
}

// === Показ панели вопросов и админа ===
function openQuestionPanel() {
    document.getElementById("regPanel").classList.add("hidden");
    document.getElementById("userPanel").classList.remove("hidden");
    showQuestions();
}

// === Работа с вопросами ===
function loadDB() {
    let data = localStorage.getItem("questionsDB");
    if(!data) return [];
    try { return JSON.parse(data); } catch { return []; }
}
function saveDB(db) {
    localStorage.setItem("questionsDB", JSON.stringify(db));
}

function sendQuestion() {
    let q = document.getElementById("question").value.trim();
    let nick = localStorage.getItem("nick") || "Аноним";
    if(!q) return;
    let db = loadDB();
    db.push({ question: q, date: new Date().toLocaleString(), read:false, reply:"", nick });
    saveDB(db);
    document.getElementById("question").value="";
    showQuestions();
}

// === Панель админа ===
function showQuestions() {
    let nick = localStorage.getItem("nick");
    let adminPanel = document.getElementById("adminPanel");
    if(nick==="Carlo"){
        adminPanel.classList.remove("hidden");
    }

    let db = loadDB();
    let container = document.getElementById("questionsList");
    container.innerHTML = '';

    db.forEach((q,i)=>{
        let div = document.createElement('div');
        div.className='question';
        div.innerHTML=`<strong>${q.date}</strong> | От: <strong>${q.nick}</strong><br>${q.question}<br>Статус: ${q.read ? "Прочитано" : "Непрочитано"}`;
        if(q.reply){
            let replyDiv = document.createElement('div');
            replyDiv.className='reply-text';
            replyDiv.innerText="Ответ: "+q.reply;
            div.appendChild(replyDiv);
        }

        if(nick==="Carlo"){
            if(!q.read){
                let readBtn = document.createElement('button');
                readBtn.className='read-btn';
                readBtn.innerText='Отметить как прочитано';
                readBtn.onclick = ()=>{
                    db[i].read=true;
                    saveDB(db);
                    showQuestions();
                };
                div.appendChild(readBtn);
            }
            if(!q.reply){
                let input = document.createElement('input');
                input.className='reply';
                input.placeholder='Ваш ответ...';
                let replyBtn = document.createElement('button');
                replyBtn.className='reply-btn';
                replyBtn.innerText='Ответить';
                replyBtn.onclick = ()=>{
                    let answer = input.value.trim();
                    if(answer){
                        db[i].reply = answer;
                        db[i].read = true;
                        saveDB(db);
                        showQuestions();
                    }
                };
                div.appendChild(input);
                div.appendChild(replyBtn);
            }
        }
        container.appendChild(div);
    });
}

// === Автопроверка входа ===
if(localStorage.getItem("nick")){
    openQuestionPanel();
}
</script>

</body>
</html>