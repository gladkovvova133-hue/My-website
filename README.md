<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Live Russia Forum</title>

<style>
body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: #0c0c0c;
    color: #e6e6e6;
}

/* HEADER */
header {
    background: linear-gradient(90deg, #1a1a1a, #111);
    padding: 22px;
    text-align: center;
    font-size: 24px;
    font-weight: bold;
    box-shadow: 0 4px 15px rgba(0,0,0,0.45);
    letter-spacing: 1px;
}

/* CARDS */
.card {
    background: #131313;
    border: 1px solid #222;
    padding: 22px;
    margin: 20px auto;
    border-radius: 12px;
    max-width: 600px;
    box-shadow: 0 0 18px rgba(0,0,0,0.55);
    transition: 0.2s;
}
.card:hover {
    border-color: #4aa3ff;
}

/* INPUTS */
.input {
    width: 100%;
    background: #1b1b1b;
    border: 1px solid #333;
    padding: 12px;
    margin-top: 10px;
    color: #fff;
    border-radius: 8px;
    font-size: 15px;
}
.input:focus {
    outline: none;
    border-color: #4aa3ff;
}

/* BUTTON */
button {
    background: #303030;
    color: white;
    border: none;
    padding: 12px 16px;
    border-radius: 8px;
    margin-top: 12px;
    font-size: 16px;
    font-weight: bold;
    transition: 0.2s;
}
button:hover {
    background: #4aa3ff;
    cursor: pointer;
}

/* TOPICS */
.topic-title {
    font-size: 18px;
    font-weight: bold;
    color: #4aa3ff;
    cursor: pointer;
    margin-bottom: 3px;
}
.reply {
    padding: 10px;
    background: #111;
    border: 1px solid #2f2f2f;
    border-radius: 8px;
    margin-top: 10px;
}

/* ROLES COLORS */
.role-admin { color: #ff4e4e; }
.role-moderator { color: #6fa8ff; }
.role-user { color: #5bd45b; }
.role-guest { color: #bdbdbd; }

/* ROLE PANEL */
.role-panel {
    margin-top: 15px;
    padding: 15px;
    border: 1px solid #333;
    border-radius: 8px;
    background: #0f0f0f;
}
</style>
</head>

<body>

<header>Live Russia — Форум</header>

<main>

<!-- РЕГИСТРАЦИЯ -->
<div id="registerBlock" class="card">
    <h2>Регистрация</h2>
    <input id="regNick" class="input" placeholder="Ваш ник">
    <input id="regPass" type="password" class="input" placeholder="Пароль">
    <button onclick="registerUser()">Зарегистрироваться</button>
</div>

<!-- ПРОФИЛЬ -->
<div id="profileBlock" class="card" style="display:none;">
    <h2>Профиль</h2>
    <p>Ник: <span id="pName"></span></p>
    <p>Роль: <span id="pRole"></span></p>
    <button onclick="logout()">Выйти</button>
</div>

<!-- ВЫДАЧА РОЛЕЙ — доступ только Carlo -->
<div id="roleBlock" class="card" style="display:none;">
    <h2>Управление ролями</h2>
    <div class="role-panel">
        <input id="roleNick" class="input" placeholder="Кому выдать роль">
        <select id="roleSelect" class="input">
            <option value="user">User</option>
            <option value="moderator">Moderator</option>
            <option value="admin">Admin</option>
        </select>
        <button onclick="giveRole()">Выдать</button>
    </div>
</div>

<!-- СОЗДАНИЕ ТЕМ -->
<div id="createTopicBlock" class="card" style="display:none;">
    <h2>Создать тему</h2>
    <input id="topicTitle" class="input" placeholder="Название темы">
    <textarea id="topicText" class="input" placeholder="Текст темы" style="height:100px;"></textarea>
    <button onclick="createTopic()">Опубликовать</button>
</div>

<!-- СПИСОК ТЕМ -->
<div class="card">
    <h2>Темы</h2>
    <div id="topicsList"></div>
</div>

<!-- ПРОСМОТР ТЕМЫ -->
<div id="viewTopic" class="card" style="display:none;"></div>

</main>


<script>
// ======== БАЗА ========
let db = JSON.parse(localStorage.getItem("forumDB") || `{
    "users": [],
    "current": null,
    "topics": []
}`);

function save() {
    localStorage.setItem("forumDB", JSON.stringify(db));
}

// ======== РОЛИ ========
function roleColor(r) {
    return {
        admin: "role-admin",
        moderator: "role-moderator",
        user: "role-user",
        guest: "role-guest"
    }[r] || "role-guest";
}

// ======== ПОЛЬЗОВАТЕЛЬ ========
function me() {
    return db.users.find(u => u.nick === db.current);
}

// ======== РЕГИСТРАЦИЯ ========
function registerUser() {
    const nick = document.getElementById("regNick").value.trim();
    const pass = document.getElementById("regPass").value.trim();

    if (!nick || !pass) return alert("Введите ник и пароль");
    if (nick.length < 2) return alert("Ник слишком короткий");
    if (nick.length > 16) return alert("Ник слишком длиный");

    if (db.users.find(u => u.nick.toLowerCase() === nick.toLowerCase()))
        return alert("Этот ник уже занят!");

    let role = "user";
    if (nick === "Carlo") role = "admin";

    db.users.push({
        nick,
        pass: btoa(pass),
        role
    });

    db.current = nick;
    save();
    render();
}

// ======== ВЫХОД ========
function logout() {
    db.current = null;
    save();
    render();
}

// ======== ВЫДАЧА РОЛИ ========
function giveRole() {
    const nick = document.getElementById("roleNick").value.trim();
    const user = db.users.find(u => u.nick === nick);
    if (!user) return alert("Пользователь не найден");

    user.role = document.getElementById("roleSelect").value;
    save();
    alert("Роль изменена");
    render();
}

// ======== СОЗДАНИЕ ТЕМЫ ========
function createTopic() {
    const user = me();
    const title = document.getElementById("topicTitle").value.trim();
    const text = document.getElementById("topicText").value.trim();

    if (!title || !text) return;

    db.topics.push({
        id: Date.now(),
        title,
        text,
        author: user.nick,
        role: user.role,
        replies: []
    });

    save();
    renderTopics();
}

// ======== ВИД ТЕМЫ ========
function openTopic(id) {
    const t = db.topics.find(x => x.id === id);
    if (!t) return;

    let html = `
        <h2>${t.title}</h2>
        <p><b class="${roleColor(t.role)}">${t.author}</b>: ${t.text}</p>
        <hr><h3>Ответы:</h3>
    `;

    t.replies.forEach(r => {
        html += `
            <div class="reply">
                <b class="${roleColor(r.role)}">${r.author}</b>: ${r.text}
            </div>
        `;
    });

    if (me()) {
        html += `
            <textarea id="replyText" class="input" placeholder="Ваш ответ"></textarea>
            <button onclick="replyTopic(${id})">Ответить</button>
        `;
    }

    html += `<button onclick="closeTopic()" style="margin-top:10px;">Назад</button>`;
    document.getElementById("viewTopic").innerHTML = html;
    document.getElementById("viewTopic").style.display = "block";
}

function closeTopic() {
    document.getElementById("viewTopic").style.display = "none";
}

// ======== ОТВЕТ ========
function replyTopic(id) {
    const t = db.topics.find(x => x.id === id);
    const text = document.getElementById("replyText").value.trim();
    if (!text) return;

    t.replies.push({
        author: me().nick,
        role: me().role,
        text
    });

    save();
    openTopic(id);
}

// ======== ОТРИСОВКА СПИСКА ТЕМ ========
function renderTopics() {
    const box = document.getElementById("topicsList");
    box.innerHTML = "";

    db.topics.forEach(t => {
        box.innerHTML += `
            <div class="card" style="margin-bottom:10px;">
                <div class="topic-title" onclick="openTopic(${t.id})">${t.title}</div>
                <div><b class="${roleColor(t.role)}">${t.author}</b></div>
            </div>
        `;
    });
}

// ======== ГЛАВНАЯ ОТРИСОВКА ========
function render() {
    const user = me();

    if (!user) {
        document.getElementById("registerBlock").style.display = "block";
        document.getElementById("profileBlock").style.display = "none";
        document.getElementById("createTopicBlock").style.display = "none";
        document.getElementById("roleBlock").style.display = "none";
    } else {
        document.getElementById("registerBlock").style.display = "none";
        document.getElementById("profileBlock").style.display = "block";

        document.getElementById("pName").textContent = user.nick;
        document.getElementById("pRole").textContent = user.role;

        document.getElementById("createTopicBlock").style.display = "block";

        document.getElementById("roleBlock").style.display =
            user.nick === "Carlo" ? "block" : "none";
    }

    renderTopics();
}

render();
</script>

</body>
</html>