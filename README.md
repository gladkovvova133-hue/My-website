<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>Анонимные вопросы + Панель Carlo</title>
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
.read-btn {
    background: #28a745;
    margin-top: 5px;
    font-size: 12px;
}
.read-btn:hover { background: #218838; }
.reply-btn {
    background: #ffc107;
    margin-top: 5px;
    font-size: 12px;
    color: black;
}
.reply-btn:hover { background: #e0a800; }
.reply-text {
    margin-top:5px;
    font-style: italic;
    color: #00ff80;
}
</style>
</head>
<body>

<div class="container" id="userPanel">
    <h2>Анонимный вопрос админам</h2>
    <input id="nickInput" placeholder="Ваш ник (необязательно)">
    <textarea id="question" placeholder="Введите свой вопрос..."></textarea>
    <button onclick="sendQuestion()">Отправить</button>
    <div id="msg"></div>
</div>

<div class="container" id="adminPanel">
    <h2>Панель админа (Carlo)</h2>
    <div id="questionsList"></div>
</div>

<script>
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
    let nick = document.getElementById("nickInput").value.trim();
    let msg = document.getElementById("msg");
    if(!q) { msg.innerHTML = "Введите вопрос!"; msg.style.color="#ff4d4d"; return; }

    let db = loadDB();
    db.push({ question: q, date: new Date().toLocaleString(), read: false, reply: "", nick: nick || "Аноним" });
    saveDB(db);

    msg.innerHTML = "Вопрос отправлен!";
    msg.style.color="#00ff80";
    document.getElementById("question").value="";
    document.getElementById("nickInput").value="";
    showQuestions();
}

// === Админ-панель ===
function showQuestions() {
    let nick = localStorage.getItem("nick");
    if(!nick || nick !== "Carlo"){
        document.getElementById("adminPanel").innerHTML = "<h2>Нет доступа</h2>";
        return;
    }

    let db = loadDB();
    let container = document.getElementById("questionsList");
    container.innerHTML = '';

    db.forEach((q,i)=>{
        let div = document.createElement('div');
        div.className='question';
        div.innerHTML=`<strong>${q.date}</strong> | От: <strong>${q.nick}</strong><br>${q.question}<br>Статус: ${q.read ? "Прочитано" : "Непрочитано"}`;
        if(q.reply) {
            let replyDiv = document.createElement('div');
            replyDiv.className='reply-text';
            replyDiv.innerText="Ответ: "+q.reply;
            div.appendChild(replyDiv);
        }

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

        container.appendChild(div);
    });
}

// === Настройка пользователя для теста ===
if(!localStorage.getItem("nick")) localStorage.setItem("nick","Carlo");

// Показываем сразу вопросы для админа
showQuestions();
</script>

</body>
</html>