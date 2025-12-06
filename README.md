<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>Регистрация</title>
<style>
body {
    font-family: Arial;
    background: #0f1115;
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
#regBox {
    background: #1a1d23;
    padding: 20px;
    width: 320px;
    border-radius: 12px;
}
input {
    width: 100%;
    padding: 10px;
    margin-top: 10px;
    border-radius: 6px;
    border: none;
}
button {
    width: 100%;
    margin-top: 15px;
    padding: 12px;
    background: #0d6efd;
    border: none;
    color: white;
    border-radius: 8px;
    cursor: pointer;
    font-size: 16px;
}
button:hover {
    background: #0b5cd4;
}
#err {
    color: #ff4d4d;
    margin-top: 10px;
}
</style>
</head>
<body>

<div id="regBox">
    <h2>Регистрация</h2>
    <input id="nick" placeholder="Введите ник">
    <input id="pass" type="password" placeholder="Введите пароль">
   <button onclick="register()">Зарегистрироваться</button>
    <div id="err"></div>
</div>

<script>
function loadDB() {
    let data = localStorage.getItem("forumDB");
    if (!data) {
        return { users: [], current: null };
    }
    try {
        return JSON.parse(data);
    } catch {
        return { users: [], current: null };
    }
}

function saveDB(db) {
    localStorage.setItem("forumDB", JSON.stringify(db));
}

function register() {
    let nick = document.getElementById("nick").value.trim();
    let pass = document.getElementById("pass").value.trim();
    let error = document.getElementById("err");

    if (!nick || !pass) {
        error.innerHTML = "Заполните все поля!";
        return;
    }

    let db = loadDB();

    // Проверка на занятый ник
    if (db.users.some(u => u.nick.toLowerCase() === nick.toLowerCase())) {
        error.innerHTML = "Этот ник уже зарегистрирован!";
        return;
    }

    // Создаём пользователя
    let newUser = {
        nick: nick,
        pass: pass,
        role: nick === "Carlo" ? "admin" : "user"
    };

    db.users.push(newUser);
    db.current = newUser.nick;
    saveDB(db);

    alert("Регистрация успешна! Добро пожаловать, " + nick);
}
</script>

</body>
</html>