<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Live Russia — Главная</title>
  <style>
    body { margin:0; font-family: Arial; background:#0d1117; color:white; }
    header { background: linear-gradient(90deg,#ff0000,#0047ab); padding:20px; text-align:center; font-size:2em; font-weight:bold; }
    nav { background:#161b22; display:flex; justify-content:center; gap:20px; padding:15px 0; }
    nav a { color:white; text-decoration:none; font-weight:bold; cursor:pointer; }
    nav a:hover { color:#58a6ff; }
    .hero { text-align:center; padding:50px 20px; background:url('https://i.imgur.com/7plVG7M.jpeg') center/cover no-repeat; color:white; text-shadow:2px 2px 5px rgba(0,0,0,0.8); }
    .hero h1 { font-size:3em; margin-bottom:10px; }
    section { padding:50px 20px; max-width:900px; margin:auto; }
    h2 { color:#58a6ff; border-bottom:2px solid #58a6ff; display:inline-block; margin-bottom:20px; }
    .card { background:#161b22; border:1px solid #30363d; border-radius:10px; padding:20px; margin-bottom:20px; }
    #contacts { display:none; background:#161b22; border:1px solid #30363d; border-radius:10px; padding:20px; max-width:600px; margin:20px auto; }
    #contacts a { color:#1da1f2; display:block; margin:10px 0; text-decoration:none; }
    #contacts a:hover { text-decoration:underline; }
    footer { text-align:center; padding:15px; background:#0d1117; color:#8b949e; margin-top:30px; }
    button { background:#0d1117; color:white; border:1px solid #30363d; padding:8px 12px; border-radius:5px; cursor:pointer; font-weight:bold; }
    button:hover { background:#161b22; }
  </style>
</head>
<body>
  <header>LIVE RUSSIA COMMUNITY</header>

  <nav>
    <a onclick="showHome()">Главная</a>
    <a href="https://forum.liverussia.online/index.php" target="_blank">Форум</a>
    <a onclick="showAbout()">О проекте</a>
    <a onclick="showContacts()">Контакты</a>
  </nav>

  <div class="hero" id="home">
    <h1>Welcome to Live Russia</h1>
    <p>Be with us, play live Russia!</p>
  </div>

  <section id="about" style="display:none;">
    <h2>About Us</h2>
    <div class="card">
      Live Russia — уникальный проект с русскими и английскими серверами, командой администрации и игроками, которые ценят проект.
    </div>
  </section>

  <div id="contacts">
    <h2>Контакты</h2>
    <a href="https://t.me/Carlo_Litvinenko" target="_blank">Telegram: @Carlo_Litvinenko</a>
    <a href="https://discord.gg/abcd1234" target="_blank">Discord: Live Russia</a>
    <a href="mailto:example@mail.com">Email: example@mail.com</a>
  </div>

  <section id="news">
    <h2>Новости проекта</h2>
    <div class="card">
      Скоро будет обновление семей, подробности в <a href="https://t.me/CarloLitvinenko" target="_blank">Telegram</a>
    </div>
  </section>

  <footer>© 2025 Live Russia | Created by Carlo</footer>

  <script>
    function showContacts() {
      document.getElementById('contacts').style.display = 'block';
      document.getElementById('about').style.display = 'none';
      document.getElementById('home').style.display = 'none';
      document.getElementById('news').style.display = 'none';
    }
    function showHome() {
      document.getElementById('home').style.display = 'block';
      document.getElementById('news').style.display = 'block';
      document.getElementById('about').style.display = 'none';
      document.getElementById('contacts').style.display = 'none';
    }
    function showAbout() {
      document.getElementById('about').style.display = 'block';
      document.getElementById('home').style.display = 'none';
      document.getElementById('contacts').style.display = 'none';
      document.getElementById('news').style.display = 'none';
    }
  </script>
</body>
</html>