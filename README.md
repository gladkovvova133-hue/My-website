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
    background: #0f0f0f;
    color: #e6e6e6;
}

/* HEADER */
header {
    background: #1a1a1a;
    padding: 18px;
    text-align: center;
    font-size: 22px;
    font-weight: bold;
    box-shadow: 0 3px 12px rgba(0,0,0,0.4);
}

/* CARDS */
.card {
    background: #161616;
    border: 1px solid #2b2b2b;
    padding: 20px;
    margin: 20px auto;
    border-radius: 10px;
    max-width: 600px;
    box-shadow: 0 0 18px rgba(0,0,0,0.5);
}

/* INPUTS */
.input {
    width: 100%;
    background: #1d1d1d;
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
    background: #333;
    color: white;
    border: none;
    padding: 12px 16px;
    border-radius: 8px;
    margin-top: 10px;
    font-size: 15px;
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
.role-mod { color: #6fa8ff; }
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
        <button onclick="giveRole()">Выдать роль</button>
    </div>
</div>

<!-- СОЗДАНИЕ ТЕМЫ -->
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
        moderator: "role-mod",
        user: "role-user",
        guest: "role-guest"
    }[r] || "role-guest";
}

// ======== РЕГИСТРАЦИЯ ========
function registerUser() {
    const nick = regNick.value.trim();
    const pass = regPass.value;

    if (!nick || !pass) return alert("Введите ник и пароль");
    if (nick.length < 2) return alert("Ник слишком короткий");

    // проверка дубликатов
    if (db.users.find(u => u.nick.toLowerCase() === nick.toLowerCase()))
        return alert("Этот ник уже занят!");

    // Карло = супер-админ
    let role = "user";
    if (nick === "Carlo") role = "admin";

    db.users.push({
        nick,
        pass: btoa(pass), // простая шифровка
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

// ======== ОПРЕДЕЛЕНИЕ ТЕКУЩЕГО ПОЛЬЗОВАТЕЛЯ ========
function me() {
    return db.users.find(u => u.nick === db.current);
}

// ======== ВЫДАЧА РОЛИ ========
function giveRole() {
    const user = db.users.find(u => u.nick === roleNick.value.trim());
    if (!user) return alert("Пользователь не найден");

    const newRole = roleSelect.value;
    user.role = newRole;

    save();
    alert("Роль выдана");
    render();
}

// ======== ТЕМА: СОЗДАНИЕ ========
function createTopic() {
    const user = me();
    if (!user || !["user","moderator","admin"].includes(user.role))
        return alert("Нет прав");

    const title = topicTitle.value.trim();
    const text = topicText.value.trim();
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
    topicTitle.value = "";
    topicText.value = "";
    renderTopics();
}

// ======== ТЕМА: ОТКРЫТЬ ========
function openTopic(id) {
    const t = db.topics.find(x => x.id === id);
    if (!t) return;

    let html = `
        <h2>${t.title}</h2>
        <p><b class="${roleColor(t.role)}">${t.author}</b>: ${t.text}</p>
        <hr>
        <h3>Ответы:</h3>
    `;

    t.replies.forEach(r => {
        html += `
            <div class="reply">
                <b class="${roleColor(r.role)}">${r.author}</b>: ${r.text}
            </div>
        `;
    });

    if (me() && ["user","moderator","admin"].includes(me().role)) {
        html += `
            <textarea id="replyText" class="input" placeholder="Ваш ответ"></textarea>
            <button onclick="replyTopic(${id})">Ответить</button>
        `;
    }

    html += `<button style="margin-top:10px;" onclick="closeTopic()">Назад</button>`;

    viewTopic.innerHTML = html;
    viewTopic.style.display = "block";
}

function closeTopic() {
    viewTopic.style.display = "none";
}

// ======== ТЕМА: ОТВЕТ ========
function replyTopic(id) {
    const t = db.topics.find(x => x.id === id);
    const user = me();
    const txt = replyText.value.trim();
    if (!txt) return;

    t.replies.push({
        author: user.nick,
        role: user.role,
        text: txt
    });

    save();
    openTopic(id);
}

// ======== ОТРИСОВКА ТЕМ ========
function renderTopics() {
    topicsList.innerHTML = "";
    db.topics.forEach(t => {
        topicsList.innerHTML += `
            <div class="card" style="margin-bottom:10px;">
                <div class="topic-title" onclick="openTopic(${t.id})">${t.title}</div>
                <div><b class="${roleColor(t.role)}">${t.author}</b></div>
            </div>
        `;
    });
}

// ======== ГЛАВНАЯ РЕНДЕР ========
function render() {
    const user = me();

    if (!user) {
        registerBlock.style.display = "block";
        profileBlock.style.display = "none";
        createTopicBlock.style.display = "none";
        roleBlock.style.display = "none";
    } else {
        registerBlock.style.display = "none";
        profileBlock.style.display = "block";
        pName.textContent = user.nick;
        pRole.textContent = user.role;

        createTopicBlock.style.display = "block";

        // Карло = выдача ролей
        roleBlock.style.display = user.nick === "Carlo" ? "block" : "none";
    }

    renderTopics();
}

render();
</script>

</body>
</html>