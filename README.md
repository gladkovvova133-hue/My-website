<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Мой сайт</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(135deg, #1e90ff, #00bfff);
      color: white;
      text-align: center;
      padding: 40px;
      min-height: 100vh;
    }
    .card {
      background: rgba(255, 255, 255, 0.1);
      padding: 30px;
      border-radius: 20px;
      max-width: 600px;
      margin: auto;
      box-shadow: 0 4px 20px rgba(0,0,0,0.2);
    }
    input, textarea {
      width: 90%;
      margin: 10px 0;
      padding: 10px;
      border: none;
      border-radius: 10px;
      font-size: 16px;
    }
    button {
      background: #ffcc00;
      border: none;
      padding: 10px 20px;
      border-radius: 10px;
      font-size: 16px;
      cursor: pointer;
      margin-top: 10px;
    }
    button:hover {
      background: #ffaa00;
    }
    h1 {
      margin-bottom: 20px;
    }
  </style>
</head>
<body>
  <div class="card">
    <h1>Мой персональный сайт</h1>
    <input id="name" type="text" placeholder="Введите ваше имя"><br>
    <textarea id="about" rows="5" placeholder="Расскажите о себе"></textarea><br>
    <input id="contact" type="text" placeholder="Ваш контакт (email, Telegram и т.д.)"><br>
    <button onclick="saveData()">💾 Сохранить</button>

    <div id="output" style="margin-top: 30px; text-align: left;"></div>
  </div>

  <script>
    function saveData() {
      const name = document.getElementById('name').value;
      const about = document.getElementById('about').value;
      const contact = document.getElementById('contact').value;
      
      document.getElementById('output').innerHTML = `
        <h2>${name || 'Без имени'}</h2>
        <p>${about || 'Информация не заполнена.'}</p>
        <p><b>Контакты:</b> ${contact || '—'}</p>
      `;
    }
  </script>
</body>
</html>