<!DOCTYPE html>
<html lang="ru">
<head>
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
        margin: 0;
    }
    #box {
        background: #1a1d23;
        padding: 25px;
        width: 330px;
        border-radius: 12px;
    }
    h2 {
        margin-top: 0;
        text-align: center;
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
        text-align: center;
        min-height: 20px;
    }
    #switch {
        margin-top: 15px;
        text-align: center;
        cursor: pointer;
        color: #9aa4b2;
    }
    #switch:hover {
        color: white;
    }
</style>
</head>
<body>

<div id="box">

    <h2 id="title">Регистрация</h2>

    <input id="nick" placeholder="Никнейм">
    <input id="pass" type="password" placeholder="Пароль">

    <button id="actionBtn" onclick="register()">Зарегистрироваться</button>

    <div id="err"></div>

    <div id="switch" onclick="toggleMode()">У меня уже есть аккаунт</div>
</div>

<script>
    function loadDB() {
        let data = localStorage.getItem("forumDB");
        if (!data) return { users: [], current: null };
        try { return JSON.parse(data); } catch { return { users: [], current: null }; }
    }

    function saveDB(db) {
        localStorage.setItem("forumDB", JSON.stringify(db));
    }

    let mode = "reg";

    function toggleMode() {
        let title = document.getElementById("title");
        let btn = document.getElementById("actionBtn");
        let sw = document.getElementById("switch");
        let err = document.getElementById("err");

        err.innerHTML = "";

        if (mode === "reg") {
            mode = "login";
            title.innerHTML = "Авторизация";
            btn.innerHTML = "Войти";
            btn.setAttribute("onclick", "login()");
            sw.innerHTML = "У меня нет аккаунта";
        } else {
            mode = "reg";
            title.innerHTML = "Регистрация";
            btn.innerHTML = "Зарегистрироваться";
            btn.setAttribute("onclick", "register()");
            sw.innerHTML = "У меня уже есть аккаунт";
        }
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

        if (db.users.some(u => u.nick.toLowerCase() === nick.toLowerCase())) {
            error.innerHTML = "Этот ник уже занят!";
            return;
        }

        let newUser = {
            nick: nick,
            pass: pass,
            role: nick === "Carlo" ? "admin" : "user"
        };

        db.users.push(newUser);
        db.current = newUser.nick;

        saveDB(db);

        // Переход на главную
        window.location.href = "index.html";
    }

    function login() {
        let nick = document.getElementById("nick").value.trim();
        let pass = document.getElementById("pass").value.trim();
        let error = document.getElementById("err");

        let db = loadDB();
        let user = db.users.find(u => u.nick.toLowerCase() === nick.toLowerCase());

        if (!user) {
            error.innerHTML = "Пользователь не найден!";
            return;
        }

        if (user.pass !== pass) {
            error.innerHTML = "Неверный пароль!";
            return;
        }

        db.current = user.nick;
        saveDB(db);

        // Переход на главную
        window.location.href = "index.html";
    }
</script>

</body>
</html>