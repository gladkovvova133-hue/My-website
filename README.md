<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Live Russia — Главная</title>
  <style>
    /* Общие стили */
    body {
      margin: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: #0d1117;
      color: #ffffff;
      transition: background 0.3s;
    }

    a { text-decoration: none; }

    /* Header */
    header {
      background: linear-gradient(90deg,#ff0000,#0047ab);
      padding: 20px;
      text-align: center;
      font-size: 2em;
      font-weight: bold;
      letter-spacing: 1px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.5);
    }

    /* Navigation */
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
      font-weight: bold;
      padding: 8px 15px;
      border-radius: 5px;
      transition: 0.3s;
      cursor: pointer;
    }

    nav a:hover {
      background: #58a6ff33;
      color: #58a6ff;
    }

    /* Hero */
    .hero {
      text-align: center;
      padding: 80px 20px;
      background: url('https://i.imgur.com/7plVG7M.jpeg') center/cover no-repeat;
      color: white;
      text-shadow: 2px 2px 8px rgba(0,0,0,0.8);
      transition: all 0.3s;
    }

    .hero h1 { font-size: 3em; margin-bottom: 10px; }
    .hero p { font-size: 1.3em; }

    /* Sections */
    section {
      padding: 50px 20px;
      max-width: 900px;
      margin: auto;
      display: none;
      animation: fadeIn 0.5s;
    }

    section.active { display: block; }

    h2 {
      color: #58a6ff;
      border-bottom: 2px solid #58a6ff;
      display: inline-block;
      margin-bottom: 20px;
    }

    .card {
      background-color: #161b22;
      border: 1px solid #30363d;
      border-radius: 15px;
      padding: 25px;
      margin-bottom: 20px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.4);
      transition: transform 0.3s;
    }

    .card:hover { transform: translateY(-5px); }

    /* Contacts */
    #contacts a {
      color: #1da1f2;
      display: block;
      margin: 10px 0;
      font-size: 1.1em;
    }

    #contacts a:hover { text-decoration: underline; }

    /* Footer */
    footer {
      text-align: center;
      padding: 15px;
      background-color: #0d1117;
      border-top: 1px solid #30363d;
      font-size: 0.9em;
      color: #8b949e;
    }

    /* Анимация появления */
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(20px);}
      to { opacity: 1; transform: translateY(0);}
    }
  </style>
</head>
<body>

<header>LIVE RUSSIA COMMUNITY</header>

<nav>
  <a onclick="showSection('home')">Главная</a>
  <a href="https://forum.liverussia.online/index.php" target="_blank">Форум</a>
  <a onclick="showSection('about')">О проекте</a>
  <a onclick="showSection('contacts')">Контакты</a>
  <a onclick="showSection('news')">Новости</a>
</nav>

<!-- Hero -->
<div class="hero" id="home-section">
  <h1>Welcome to Live Russia</h1>
  <p>Be with us, play live Russia!</p>
</div>

<!-- About -->
<section id="about">
  <h2>About Us</h2>
  <div class="card">
    Live Russia — уникальный проект с русскими и английскими серверами, командой администрации и игроками, которые ценят проект.
  </div>
</section>

<!-- Contacts -->
<section id="contacts">
  <h2>Контакты</h2>
  <div class="card">
    <a href="https://t.me/Carlo_Litvinenko" target="_blank">Telegram:https://t.me/CarloLitvinenko </a>
    <a href="https://https://discord.gg/Swmz3kR9" target="_blank"<a href= почта  "gladkovvova133@gmail.com"</a>
  </div>
</section>

<!-- News -->
<section id="news">
  <h2>Новости проекта</h2>
  <div class="card">
    Скоро будет обновление семей, подробности в <a href= "https://t.me/CarloLitvinenko" target="_blank">Telegram</a>
  </div>
</section>

<footer>© 2025 Live Russia | Created by Carlo</footer>

<script>
  // Скрыть все секции кроме home по умолчанию
  document.addEventListener('DOMContentLoaded', function() {
    showSection('home');
  });

  function showSection(sectionId) {
    const sections = ['about','contacts','news'];
    sections.forEach(id => {
      document.getElementById(id).classList.remove('active');
    });

    if(sectionId === 'home'){
      document.getElementById('home-section').style.display = 'block';
    } else {
      document.getElementById('home-section').style.display = 'none';
      document.getElementById(sectionId).classList.add('active');
    }
  }
</script>

</body>
</html>