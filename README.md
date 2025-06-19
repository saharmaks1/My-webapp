DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Игровой Интерфейс</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #2a004f, #000033); /* Синий и фиолетовый градиент */
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            position: relative;
            overflow: hidden;
            color: white; /* Для читабельности текста на темном фоне */
        }

        /* Звезды на фоне */
        .star {
            position: absolute;
            background-color: white;
            border-radius: 50%;
            opacity: 0;
            animation: twinkle 5s infinite ease-in-out alternate;
        }

        @keyframes twinkle {
            0%, 100% { opacity: 0; transform: scale(0.5); }
            50% { opacity: 1; transform: scale(1.2); }
        }

        /* Размеры и позиции звезд можно генерировать динамически или задать вручную */
        .star:nth-child(1) { top: 10%; left: 20%; width: 2px; height: 2px; animation-delay: 0s; }
        .star:nth-child(2) { top: 5%; left: 70%; width: 3px; height: 3px; animation-delay: 1s; }
        .star:nth-child(3) { top: 30%; left: 80%; width: 1px; height: 1px; animation-delay: 2s; }
        .star:nth-child(4) { top: 60%; left: 10%; width: 2px; height: 2px; animation-delay: 0.5s; }
        .star:nth-child(5) { top: 80%; left: 90%; width: 3px; height: 3px; animation-delay: 1.5s; }
        .star:nth-child(6) { top: 45%; left: 40%; width: 1px; height: 1px; animation-delay: 2.5s; }
        .star:nth-child(7) { top: 25%; left: 50%; width: 2px; height: 2px; animation-delay: 0.8s; }
        .star:nth-child(8) { top: 70%; left: 30%; width: 3px; height: 3px; animation-delay: 1.8s; }

        .container {
            width: 100%;
            height: 100%;
            position: relative;
            z-index: 1; /* Чтобы элементы были над звездами */
        }

        /* Основные игровые элементы */
        .game-elements > * {
            transition: opacity 0.3s ease, transform 0.3s ease;
        }

        .game-elements.hidden > * {
            opacity: 0;
            transform: scale(0.9);
            pointer-events: none;
        }

        /* Верхний правый угол - Уровень */
        .top-right {
            position: absolute;
            top: 20px;
            right: 20px;
            background-color: #4CAF50;
            color: white;
            padding: 10px 20px;
            border-radius: 5px;
            font-size: 1.2em;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            z-index: 10;
        }

        /* Правый нижний угол - Кнопка "Играть" */
        .bottom-right {
            position: absolute;
            bottom: 20px;
            right: 20px;
            background-color: #FFEB3B;
            color: #333;
            padding: 15px 30px;
            border-radius: 50px;
            font-size: 1.5em;
            font-weight: bold;
            text-decoration: none;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            transition: background-color 0.3s ease;
            z-index: 10;
            white-space: nowrap;
            display: inline-block; /* Важно для избежания обрезки */
        }

        .bottom-right:hover {
            background-color: #FFC107;
        }

        /* Левый нижний угол - Скины и Улучшения */
        .bottom-left-buttons {
            position: absolute;
            bottom: 20px;
            left: 20px;
            display: flex;
            flex-direction: row;
            gap: 15px;
            z-index: 10;
        }

        .bottom-left-buttons button {
            background-color: #2196F3;
            color: white;
            padding: 12px 25px;
            border: none;
            border-radius: 5px;
            font-size: 1.1em;
            cursor: pointer;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            transition: background-color 0.3s ease;
            white-space: nowrap;
        }

        .bottom-left-buttons button:hover {
            background-color: #1976D2;
        }

        /* Верхний левый угол - Шестеренка (Настройки) */
        .top-left-settings {
            position: absolute;
            top: 20px;
            left: 20px;
            background-color: #607D8B;
            color: white;
            padding: 10px;
            border-radius: 5px;
            font-size: 1.8em;
            cursor: pointer;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            transition: background-color 0.3s ease;
            z-index: 10;
        }

        .top-left-settings:hover {
            background-color: #455A64;
        }

        /* Бандит */
        .gangster-image {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 250px;
            height: auto;
            max-width: 70%;
            border-radius: 10px;
            box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
            z-index: 5;
            object-fit: contain;
        }

        /* Секция настроек */
        .settings-section {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(240, 240, 240, 0.95);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 20;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s ease;
        }

        .settings-section.visible {
            opacity: 1;
            pointer-events: auto;
        }

        .settings-header {
            position: absolute;
            top: 20px;
            right: 20px;
            font-size: 2em;
            color: #333;
        }

        .settings-buttons {
            position: absolute;
            bottom: 20px;
            left: 20px;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .settings-buttons button {
            background-color: #607D8B;
            color: white;
            padding: 15px 30px;
            border: none;
            border-radius: 8px;
            font-size: 1.2em;
            cursor: pointer;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            transition: background-color 0.3s ease;
            width: 200px;
            text-align: left;
        }

        .settings-buttons button:hover {
            background-color: #455A64;
        }

        .sub-buttons {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-left: 30px;
            margin-top: 10px;
            opacity: 0;
            max-height: 0;
            overflow: hidden;
            transition: opacity 0.3s ease, max-height 0.3s ease;
        }

        .sub-buttons.visible {
            opacity: 1;
            max-height: 200px;
        }

        .sub-buttons button {
            background-color: #9E9E9E;
            padding: 10px 20px;
            font-size: 1em;
            width: 170px;
        }

        .sub-buttons button:hover {
            background-color: #757575;
        }
    </style>
</head>
<body>
    <div class="star"></div>
    <div class="star"></div>
    <div class="star"></div>
    <div class="star"></div>
    <div class="star"></div>
    <div class="star"></div>
    <div class="star"></div>
    <div class="star"></div>

    <div class="container">
        <div class="game-elements" id="gameElements">
            <div class="top-right">Уровень: 1</div>
            <a href="#" class="bottom-right">Играть</a>
            <div class="bottom-left-buttons">
                <button>Скины</button>
                <button>Улучшения</button>
            </div>
            <div class="top-left-settings" id="settingsButton">⚙️</div>
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/d4/Cartoon_Gangster_Boss_PNG_Image.png/640px-Cartoon_Gangster_Boss_PNG_Image.png" alt="Gangster" class="gangster-image">
        </div>

        <div class="settings-section" id="settingsSection">
            <div class="settings-header">Настройки</div>
            <div class="settings-buttons">
                <button id="backButton">Назад</button>
                <button id="themesButton">Выбор тем</button>
                <div class="sub-buttons" id="themesSubButtons">
                    <button>Тёмная</button>
                    <button>Чёрная</button>
                </div>
                <button id="languageButton">Выбор языка</button>
                <div class="sub-buttons" id="languageSubButtons">
                    <button>English</button>
                    <button>Русский</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        const settingsButton = document.getElementById('settingsButton');
        const settingsSection = document.getElementById('settingsSection');
        const gameElements = document.getElementById('gameElements');
        const backButton = document.getElementById('backButton');
        const themesButton = document.getElementById('themesButton');
        const languageButton = document.getElementById('languageButton');
        const themesSubButtons = document.getElementById('themesSubButtons');
        const languageSubButtons = document.getElementById('languageSubButtons');

        settingsButton.addEventListener('click', () => {
            gameElements.classList.add('hidden');
            setTimeout(() => {
                gameElements.style.display = 'none';
                settingsSection.style.display = 'flex';
                setTimeout(() => {
                    settingsSection.classList.add('visible');
                }, 10);
            }, 300);
        });

        backButton.addEventListener('click', () => {
            settingsSection.classList.remove('visible');
            themesSubButtons.classList.remove('visible');
            languageSubButtons.classList.remove('visible');
            setTimeout(() => {
                settingsSection.style.display = 'none';
                gameElements.style.display = 'block';
                setTimeout(() => {
                    gameElements.classList.remove('hidden');
                }, 10);
            }, 300);
        });

        themesButton.addEventListener('click', () => {
            themesSubButtons.classList.toggle('visible');
            languageSubButtons.classList.remove('visible');
        });

        languageButton.addEventListener('click', () => {
            languageSubButtons.classList.toggle('visible');
            themesSubButtons.classList.remove('visible');
        });

        themesSubButtons.querySelectorAll('button').forEach(button => {
            button.addEventListener('click', () => {
                alert('Выбрана тема: ' + button.textContent);
            });
        });

        languageSubButtons.querySelectorAll('button').forEach(button => {
            button.addEventListener('click', () => {
                alert('Выбран язык: ' + button.textContent);
            });
        });

    </script>
</body>
</html>
