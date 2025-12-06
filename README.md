<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Live Russia – Форум</title>

<style>
body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: #121212;
    color: #eaeaea;
}
header {
    background: #1e1e1e;
    padding: 15px;
    font-size: 20px;
    text-align: center;
    border-bottom: 1px solid #333;
}
main {
    max-width: 900px;
    margin: auto;
    padding: 20px;
}
button {
    background: #2b2b2b;
    color: white;
    border: 1px solid #444;
    padding: 8px 12px;
    border-radius: 6px;
}
button:hover { background: #3b3b3b; cursor: pointer; }

.card {
    background: #1c1c1c;
    padding: 15px;
    border-radius: 8px;
    margin-bottom: 15px;
    border: 1px solid #2b2b2b;
}
.input {
    width: 100%;
    padding: 10px;
    margin-top: 5px;
    background: #222;
    border: 1px solid #444;
    color: white;
    border-radius: 6px;
}
.topic-title { font-size: 18px; font-weight: bold; color: #4aa3ff; cursor: pointer; }
.reply { margin-top: 10px; padding: 10px; background: #181818; border-radius: 6px; border: 1px solid #333; }
.role-admin { color: #ff4e4e; }
.role-mod { color: #6fa8ff; }
.role-user { color: #5bd45b; }
.role-guest { color: #bdbdbd; }
</style>
</head>

<body>

<header>
    Live Russia — Форум (локальная версия)
</header>

<main>

<!-- Блок логина -->
<div id="loginBlock" class="card">
    <h3>Войти</h3>
    <input id="loginName" class="input" placeholder="Введите ник">
    <button onclick="login()">Войти</button>
</div>

<!-- Профиль -->
<div id="profileBlock" class="card" style="display:none;">
    <h3>Профиль</h3>
    <p>Ник: <span id="profName"></span></p>
    <p>Роль: <span id="profRole"></span></p>
    <button onclick="logout()">Выйти</button>
</div>

<!-- Создание темы -->
<div id="createTopicBlock" class="card" style="display:none;">
    <h3>Создать тему</h3>
    <input id="topicTitle" class="input" placeholder="Заголовок темы">
    <textarea id="topicText" class="input" style="height: 100px;" placeholder="Текст темы"></textarea>
    <button onclick="createTopic()">Опубликовать</button>
</div>

<!-- Список тем -->
<div class="card">
    <h3>Темы</h3>
    <div id="topicsList"></div>
</div>

<!-- Просмотр темы -->
<div id="viewTopic" class="card" style="display:none;"></div>

</main>

<script>
// ======= LOCAL DB =======
let db = JSON.parse(localStorage.getItem("forumDB") || `{
    "user": null,
    "topics": []
}`);

function save() {
    localStorage.setItem("forumDB", JSON.stringify(db));
}

// ======= ROLES =======
function getRoleColor(role) {
    return {
        admin: "role-admin",
        moderator: "role-mod",
        user: "role-user",
        guest: "role-guest"
    }[role] || "role-guest";
}

// ======= LOGIN =======
function login() {
    const name = document.getElementById("loginName").value.trim();
    if(name.length < 2) return alert("Ник слишком короткий");

    let role = "user";
    if(name === "Admin") role = "admin";
    else if(name.startsWith("Mod_")) role = "moderator";

    db.user = { name, role };
    save();
    renderAll();
}

function logout() {
    db.user = null;
    save();
    renderAll();
}

// ======= CREATE TOPIC =======
function createTopic() {
    if(!db.user || !["user","moderator","admin"].includes(db.user.role))
        return alert("У вас нет прав создавать темы");

    const title = topicTitle.value.trim();
    const text = topicText.value.trim();
    if (!title || !text) return;

    db.topics.push({
        id: Date.now(),
        author: db.user.name,
        role: db.user.role,
        title,
        text,
        replies: []
    });
    save();
    topicTitle.value = "";
    topicText.value = "";
    renderAll();
}

// ======= VIEW TOPIC =======
function openTopic(id) {
    const topic = db.topics.find(t => t.id === id);
    if (!topic) return;

    let html = `
        <h2>${topic.title}</h2>
        <p><b class="${getRoleColor(topic.role)}">${topic.author}</b>: ${topic.text}</p>
        <hr>
        <h3>Ответы:</h3>
    `;

    topic.replies.forEach(r => {
        html += `
        <div class="reply">
            <b class="${getRoleColor(r.role)}">${r.author}</b>: ${r.text}
        </div>`;
    });

    if(db.user && ["user","moderator","admin"].includes(db.user.role)) {
        html += `
            <textarea id="replyText" class="input" placeholder="Ваш ответ"></textarea>
            <button onclick="replyTopic(${id})">Ответить</button>
        `;
    }

    html += `<button style="margin-top:10px;" onclick="closeTopic()">Назад</button>`;

    viewTopic.innerHTML = html;
    viewTopic.style.display = "block";
}

// ======= REPLY TO TOPIC =======
function replyTopic(id) {
    const topic = db.topics.find(t => t.id === id);
    const txt = replyText.value.trim();
    if (!txt) return;

    topic.replies.push({
        author: db.user.name,
        role: db.user.role,
        text: txt
    });

    save();
    openTopic(id);
}

function closeTopic() {
    viewTopic.style.display = "none";
}

// ======= RENDER =======
function renderTopics() {
    topicsList.innerHTML = "";
    db.topics.forEach(t => {
        topicsList.innerHTML += `
            <div class="card" style="margin-bottom:10px;">
                <div class="topic-title" onclick="openTopic(${t.id})">${t.title}</div>
                <div><b class="${getRoleColor(t.role)}">${t.author}</b></div>
            </div>
        `;
    });
}

function renderAll() {
    if (db.user) {
        loginBlock.style.display = "none";
        profileBlock.style.display = "block";
        profName.textContent = db.user.name;
        profRole.textContent = db.user.role;

        if(["user","moderator","admin"].includes(db.user.role))
            createTopicBlock.style.display = "block";
        else
            createTopicBlock.style.display = "none";

    } else {
        loginBlock.style.display = "block";
        profileBlock.style.display = "none";
        createTopicBlock.style.display = "none";
    }

    renderTopics();
}

renderAll();
</script>

</body>
</html>