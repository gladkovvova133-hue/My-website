<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Live Russia — Главная</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background-color: #0d1117;
      color: white;
    }
    header {
      background: linear-gradient(90deg, #ff0000, #0047ab);
      padding: 20px;
      text-align: center;
      font-size: 2em;
      font-weight: bold;
      letter-spacing: 1px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.5);
    }
    nav {
      background-color: #161b22;
      display: flex;
      justify-content: center;
      gap: 20px;
      padding: 15px 0;
      border-bottom: 1px solid #30363d;
    }
    nav a {
      color: white;
      text-decoration: none;
      font-weight: bold;
      transition: 0.3s;
    }
    nav a:hover {
      color: #58a6ff;
    }
    .hero {
      text-align: center;
      padding: 80px 20px;
      background: url('https://i.imgur.com/7plVG7M.jpeg') center/cover no-repeat;
      color: white;
      text-shadow: 2px 2px 5px rgba(0,0,0,0.8);
    }
    .hero h1 {
      font-size: 3em;
      margin-bottom: 10px;
    }
    .hero p {
      font-size: 1.3em;
    }
    section {
      padding: 50px 20px;
      max-width: 900px;
      margin: auto;
    }
    h2 {
      color: #58a6ff;
      border-bottom: 2px solid #58a6ff;
      display: inline-block;
      margin-bottom: 20px;
    }
    .card {
      background-color: #161b22;
      border: 1px solid #30363d;
      border-radius: 10px;
      padding: 20px;
      margin-bottom: 20px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.4);
    }
    footer {
      text-align: center;
      padding: 15px;
      background-color: #0d1117;
      border-top: 1px solid #30363d;
      font-size: 0.9em;
      color: #8b949e;
    }
  </style>
</head>
<body>
  <header>LIVE RUSSIA COMMUNITY</header>
  <nav>
    <a href="#">Главная</a>
    <nav>
  <a href="#">Главная</a>
  <a href="https://forum.liverussia.online/index.php" target="_blank">Форум</a>
  <a href="#">О проекте</a>
  <a href="#">Контакты</a>
</nav>
    <a href="#">О проекте</a>
    <a href="#">Контакты</a>
  </nav>

  <div class="hero">
    <h1>Добро пожаловать в Live Russia</h1>
    <p>Играй, развивайся, будь частью сообщества!</p>
  </div>

  <section>
    <h2>Новости проекта</h2>
    <div class="card">
      <h3>🔥 Обновление 3.5 — Новый сезон!</h3>
      <p>Теперь доступен новый город, система миссий и уникальные автомобили. Проверь свой гараж прямо сейчас!</p>
    </div>
    <div class="card">
      <h3>🚓 Набор в администрацию</h3>
      <p>Хотите стать частью команды и помогать игрокам? Заполните анкету на форуме в разделе "Набор".</p>
    </div>

    <h2>О нас</h2>
    <div class="card">
      <p>Live Russia — это уникальный игровой проект, объединяющий тысячи игроков по всей России и СНГ.  
      Мы предлагаем реалистичную атмосферу, ролевую систему и дружное сообщество!</p>
    </div>
  </section>

  <footer>
    © 2025 Live Russia | Сделано сообществом игроков
  </footer>
</body>
</html>