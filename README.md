<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Мой стильный сайт</title>
    <style>
        /* Общие стили */
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 0;
            background: linear-gradient(120deg, #f0f8ff, #d0e8ff);
            color: #333;
            text-align: center;
        }

        /* Заголовок */
        h1 {
            margin-top: 80px;
            font-size: 56px;
            color: #1a73e8;
            animation: fadeIn 2s ease-in;
        }

        /* Подзаголовок */
        p {
            font-size: 20px;
            color: #555;
            margin: 20px 0;
            animation: fadeIn 3s ease-in;
        }

        /* Кнопки */
        .buttons a {
            display: inline-block;
            margin: 15px;
            padding: 12px 30px;
            font-size: 18px;
            color: #fff;
            background-color: #1a73e8;
            border-radius: 30px;
            text-decoration: none;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(26, 115, 232, 0.4);
        }

        .buttons a:hover {
            background-color: #155ab6;
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(26, 115, 232, 0.6);
        }

        /* Footer */
        .footer {
            margin-top: 60px;
            font-size: 14px;
            color: #888;
        }

        /* Анимация появления */
        @keyframes fadeIn {
            from {opacity: 0;}
            to {opacity: 1;}
        }
    </style>
</head>
<body>
    <h1>Привет, это мой стильный сайт!</h1>
    <p>Добро пожаловать! Здесь можно разместить ссылки и любую информацию.</p>

    <div class="buttons">
        <a href="https://www.google.com" target="_blank">Перейти на Google</a>
        <a href="https://gladkovvova133-hue.github.io/My-website/" target="_blank">На главную страницу</a>
    </div>

    <div class="footer">
        © 2025 Мой стильный сайт
    </div>
</body>
</html>
