<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RPG Character Sheet</title>
    <style>
        :root {
            --bg-dark: #121212;
            --card-dark: #1e1e1e;
            --text-light: #e0e0e0;
            --accent-red: #ff5555;
            --accent-blue: #5555ff;
            --accent-orange: #ffaa55;
            --accent-purple: #aa55ff;
            --accent-teal: #55ffaa;
            --accent-pink: #ff55aa;
            --save-bg: #2a2a2a;
            --health-bg: #331111;
            --temp-health-bg: #333311;
            --abilities-bg: #113322;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 20px;
            background-color: var(--bg-dark);
            color: var(--text-light);
            min-height: 100vh;
            line-height: 1.6;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 15px;
        }

        .tab-buttons {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .tab-btn {
            padding: 10px 20px;
            background: var(--card-dark);
            border: none;
            border-radius: 6px;
            color: var(--text-light);
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s;
            white-space: nowrap;
        }

        .tab-btn:hover {
            background: rgba(255, 255, 255, 0.1);
            transform: translateY(-2px);
        }

        .tab-btn.active {
            background: var(--accent-blue);
            box-shadow: 0 4px 8px rgba(85, 85, 255, 0.3);
        }

        .tab-content {
            display: none;
            animation: fadeIn 0.3s ease-in;
        }

        .tab-content.active {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .main-content {
            display: grid;
            grid-template-columns: 1fr 300px;
            gap: 20px;
        }

        @media (max-width: 768px) {
            .main-content {
                grid-template-columns: 1fr;
            }
            
            .tab-buttons {
                flex-direction: column;
                align-items: center;
            }
            
            .tab-btn {
                width: 100%;
                max-width: 300px;
            }
        }

        .header-section {
            margin-bottom: 30px;
            padding: 25px;
            background: var(--card-dark);
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }

        @media (max-width: 600px) {
            .header-section {
                grid-template-columns: 1fr;
            }
        }

        .character-name {
            grid-column: 1 / -1;
            margin-bottom: 15px;
        }

        h1 {
            color: var(--text-light);
            text-shadow: 0 0 10px rgba(255, 255, 255, 0.2);
            margin-bottom: 20px;
            text-align: center;
            font-size: 2.5em;
        }

        .info-group {
            text-align: left;
        }

        .info-label {
            font-size: 14px;
            opacity: 0.7;
            margin-bottom: 5px;
            color: var(--text-light);
        }

        .info-value {
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 15px;
            padding: 8px 12px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 8px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            transition: border-color 0.3s;
        }

        .text-input {
            width: 100%;
            padding: 10px;
            background: transparent;
            border: none;
            border-bottom: 1px solid rgba(255, 255, 255, 0.3);
            color: var(--text-light);
            font-size: 16px;
            transition: all 0.3s;
        }

        .text-input:focus {
            outline: none;
            border-color: var(--accent-blue);
            box-shadow: 0 2px 4px rgba(85, 85, 255, 0.2);
        }

        .stats-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .stat-box {
            background: var(--card-dark);
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
            transition: transform 0.3s, box-shadow 0.3s;
            border: 1px solid rgba(255, 255, 255, 0.1);
            display: flex;
            flex-direction: column;
        }

        .stat-box:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.4);
        }

        .stat-name {
            font-weight: bold;
            font-size: 18px;
            margin-bottom: 15px;
            color: var(--text-light);
            text-align: center;
        }

        .stat-value {
            font-size: 32px;
            font-weight: bold;
            margin: 15px 0;
            text-shadow: 0 0 8px currentColor;
            text-align: center;
            transition: all 0.3s;
        }

        .stat-controls {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-bottom: 10px;
        }

        .stat-btn {
            width: 40px;
            height: 40px;
            font-size: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
        }

        .stat-btn:hover {
            transform: scale(1.1);
        }

        .save-box {
            background: var(--save-bg);
            border-radius: 8px;
            padding: 10px;
            margin-top: 15px;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .save-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 10px;
        }

        .save-label {
            font-size: 14px;
            opacity: 0.8;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .save-checkbox {
            width: 18px;
            height: 18px;
            cursor: pointer;
        }

        .save-value {
            font-weight: bold;
            font-size: 18px;
            min-width: 30px;
            text-align: center;
        }

        .save-controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        .save-btn {
            width: 30px;
            height: 30px;
            font-size: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            transition: all 0.3s;
        }

        .save-btn:hover {
            transform: scale(1.1);
        }

        button {
            padding: 8px 15px;
            font-size: 14px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: bold;
            color: white;
            background: rgba(255, 255, 255, 0.1);
            margin-top: 5px;
        }

        button:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: translateY(-2px);
        }

        .main-btn {
            padding: 10px 20px;
            margin-top: 10px;
            background: linear-gradient(145deg, #333, #222);
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
        }

        /* Цветовые стили для характеристик */
        .strength-btn { 
            background: var(--accent-red);
            color: white;
        }
        .agility-btn { 
            background: var(--accent-blue);
            color: white;
        }
        .constitution-btn { 
            background: var(--accent-orange);
            color: white;
        }
        .wisdom-btn { 
            background: var(--accent-purple);
            color: white;
        }
        .intelligence-btn { 
            background: var(--accent-teal);
            color: var(--bg-dark);
        }
        .charisma-btn { 
            background: var(--accent-pink);
            color: white;
        }

        .stat-value.strength { color: var(--accent-red); }
        .stat-value.agility { color: var(--accent-blue); }
        .stat-value.constitution { color: var(--accent-orange); }
        .stat-value.wisdom { color: var(--accent-purple); }
        .stat-value.intelligence { color: var(--accent-teal); }
        .stat-value.charisma { color: var(--accent-pink); }

        /* Стили для инвентаря */
        .inventory-section {
            margin: 30px 0;
            padding: 25px;
            background: var(--card-dark);
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
            text-align: left;
        }

        .inventory-title {
            font-size: 20px;
            font-weight: bold;
            margin-bottom: 15px;
            color: var(--accent-orange);
            text-align: center;
        }

        .inventory-row {
            margin-bottom: 20px;
        }

        .inventory-label {
            font-size: 16px;
            margin-bottom: 8px;
            display: block;
            color: var(--text-light);
            opacity: 0.9;
        }

        .inventory-input {
            width: 100%;
            padding: 12px;
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 8px;
            color: var(--text-light);
            font-size: 16px;
            transition: all 0.3s;
        }

        .inventory-input:focus {
            outline: none;
            border-color: var(--accent-blue);
            box-shadow: 0 2px 4px rgba(85, 85, 255, 0.2);
        }

        /* Боковая панель */
        .sidebar {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        @media (max-width: 768px) {
            .sidebar {
                order: -1;
            }
        }

        .side-box {
            background: var(--card-dark);
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.1);
            text-align: center;
        }

        .side-title {
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 15px;
            color: var(--accent-teal);
        }

        .side-value {
            font-size: 36px;
            font-weight: bold;
            margin: 15px 0;
            transition: all 0.3s;
        }

        .side-controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        .side-btn {
            width: 40px;
            height: 40px;
            font-size: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            background: rgba(255, 255, 255, 0.1);
            color: var(--text-light);
            transition: all 0.3s;
        }

        .side-btn:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: scale(1.1);
        }

        .armor-value { color: var(--accent-orange); }
        .initiative-value { color: var(--accent-purple); }
        .speed-value { color: var(--accent-pink); }

        /* Секция здоровья */
        .health-section {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-bottom: 30px;
        }

        @media (max-width: 600px) {
            .health-section {
                grid-template-columns: 1fr;
            }
        }

        .health-box {
            background: var(--card-dark);
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.1);
            text-align: center;
        }

        .health-title {
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 15px;
        }

        .current-health {
            color: var(--accent-red);
        }

        .temp-health {
            color: var(--accent-orange);
        }

        .health-value {
            font-size: 32px;
            font-weight: bold;
            margin: 15px 0;
            transition: all 0.3s;
        }

        .health-controls {
            display: flex;
            justify-content: center;
            gap: 10px;
            flex-wrap: wrap;
        }

        .health-btn {
            width: 40px;
            height: 40px;
            font-size: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
        }

        .health-btn:hover {
            transform: scale(1.1);
        }

        .health-input {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 8px;
            color: var(--text-light);
            font-size: 16px;
            text-align: center;
            transition: all 0.3s;
        }

        .health-input:focus {
            outline: none;
            border-color: var(--accent-blue);
            box-shadow: 0 2px 4px rgba(85, 85, 255, 0.2);
        }

        /* Стили для вкладки персональных черт */
        .traits-section {
            background: var(--card-dark);
            border-radius: 12px;
            padding: 25px;
            margin-bottom: 30px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
        }

        .trait-row {
            margin-bottom: 20px;
        }

        .trait-input {
            width: 100%;
            padding: 12px;
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 8px;
            color: var(--text-light);
            font-size: 16px;
            transition: all 0.3s;
        }

        .trait-input:focus {
            outline: none;
            border-color: var(--accent-blue);
            box-shadow: 0 2px 4px rgba(85, 85, 255, 0.2);
        }

        /* Стили для вкладки способностей */
        .abilities-section {
            background: var(--card-dark);
            border-radius: 12px;
            padding: 25px;
            margin-bottom: 30px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
        }

        .ability-row {
            margin-bottom: 20px;
            padding: 15px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 8px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .ability-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
            flex-wrap: wrap;
            gap: 10px;
        }

        .ability-name {
            font-weight: bold;
            font-size: 18px;
            flex: 1;
            min-width: 200px;
            padding: 10px;
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 6px;
            color: var(--text-light);
        }

        .ability-controls {
            display: flex;
            gap: 10px;
        }

        .ability-description {
            width: 100%;
            padding: 12px;
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 8px;
            color: var(--text-light);
            font-size: 16px;
            min-height: 100px;
            resize: vertical;
            transition: all 0.3s;
        }

        .ability-description:focus {
            outline: none;
            border-color: var(--accent-blue);
            box-shadow: 0 2px 4px rgba(85, 85, 255, 0.2);
        }

        .add-ability-btn {
            width: 100%;
            padding: 12px;
            background: var(--abilities-bg);
            border: none;
            border-radius: 8px;
            color: var(--text-light);
            font-size: 16px;
            cursor: pointer;
            margin-top: 10px;
            transition: all 0.3s;
        }

        .add-ability-btn:hover {
            background: var(--accent-teal);
            color: var(--bg-dark);
            transform: translateY(-2px);
        }

        .delete-ability-btn {
            background: var(--accent-red);
            color: white;
            padding: 8px 15px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            transition: all 0.3s;
        }

        .delete-ability-btn:hover {
            background: #d32f2f;
            transform: translateY(-2px);
        }

        /* Стили для вкладки настроек */
        .settings-section {
            background: var(--card-dark);
            border-radius: 12px;
            padding: 25px;
            margin-bottom: 30px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
        }

        .setting-row {
            margin-bottom: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 15px;
            padding: 15px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 8px;
        }

        .setting-label {
            font-size: 16px;
            font-weight: bold;
            flex: 1;
            min-width: 200px;
        }

        .setting-control {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .switch {
            position: relative;
            display: inline-block;
            width: 60px;
            height: 34px;
        }

        .switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }

        .slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #ccc;
            transition: .4s;
            border-radius: 34px;
        }

        .slider:before {
            position: absolute;
            content: "";
            height: 26px;
            width: 26px;
            left: 4px;
            bottom: 4px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }

        input:checked + .slider {
            background-color: var(--accent-blue);
        }

        input:checked + .slider:before {
            transform: translateX(26px);
        }

        .reset-btn {
            padding: 10px 20px;
            background: var(--accent-red);
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s;
        }

        .reset-btn:hover {
            background: #d32f2f;
            transform: translateY(-2px);
        }

        .language-selector {
            padding: 8px 12px;
            background: var(--card-dark);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 6px;
            color: var(--text-light);
            font-size: 14px;
            cursor: pointer;
            transition: all 0.3s;
        }

        .language-selector:focus {
            outline: none;
            border-color: var(--accent-blue);
            box-shadow: 0 2px 4px rgba(85, 85, 255, 0.2);
        }

        /* Улучшенная анимация для значений */
        .value-animate {
            animation: pulse 0.3s ease-in-out;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.2); }
            100% { transform: scale(1); }
        }

        /* Стили для скроллбара */
        ::-webkit-scrollbar {
            width: 8px;
        }

        ::-webkit-scrollbar-track {
            background: var(--card-dark);
        }

        ::-webkit-scrollbar-thumb {
            background: var(--accent-blue);
            border-radius: 4px;
        }

        ::-webkit-scrollbar-thumb:hover {
            background: var(--accent-purple);
        }

        /* Улучшенная доступность */
        @media (prefers-reduced-motion: reduce) {
            * {
                animation-duration: 0.01ms !important;
                animation-iteration-count: 1 !important;
                transition-duration: 0.01ms !important;
            }
        }

        /* Подсветка фокуса для доступности */
        button:focus,
        input:focus,
        textarea:focus,
        select:focus {
            outline: 2px solid var(--accent-blue);
            outline-offset: 2px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="tab-buttons">
            <button class="tab-btn active" data-i18n="mainTab" onclick="switchTab('main')">Основные характеристики</button>
            <button class="tab-btn" data-i18n="abilitiesTab" onclick="switchTab('abilities')">Способности</button>
            <button class="tab-btn" data-i18n="traitsTab" onclick="switchTab('traits')">Персональные черты</button>
            <button class="tab-btn" data-i18n="settingsTab" onclick="switchTab('settings')">Настройки</button>
        </div>

        <!-- Основная вкладка -->
        <div class="tab-content active" id="main-tab">
            <div class="main-content">
                <div style="width: 100%;">
                    <div class="header-section">
                        <div class="character-name">
                            <h1 contenteditable="true" id="characterName" data-i18n="characterName">Имя персонажа</h1>
                        </div>
                        
                        <div class="info-group">
                            <div class="info-label" data-i18n="raceLabel">Раса</div>
                            <div class="info-value">
                                <input type="text" class="text-input" id="race" data-i18n-placeholder="racePlaceholder" placeholder="Человек">
                            </div>
                            
                            <div class="info-label" data-i18n="classLevelLabel">Класс и уровень</div>
                            <div class="info-value">
                                <input type="text" class="text-input" id="classLevel" data-i18n-placeholder="classLevelPlaceholder" placeholder="Воин 1">
                            </div>
                        </div>
                        
                        <div class="info-group">
                            <div class="info-label" data-i18n="backgroundLabel">Предыстория</div>
                            <div class="info-value">
                                <input type="text" class="text-input" id="background" data-i18n-placeholder="backgroundPlaceholder" placeholder="Народный герой">
                            </div>
                            
                            <div class="info-label" data-i18n="alignmentLabel">Мировоззрение</div>
                            <div class="info-value">
                                <input type="text" class="text-input" id="alignment" data-i18n-placeholder="alignmentPlaceholder" placeholder="Добрый">
                            </div>
                        </div>
                        
                        <div class="info-group">
                            <div class="info-label" data-i18n="experienceLabel">Опыт</div>
                            <div class="info-value">
                                <input type="text" class="text-input" id="experience" placeholder="0">
                            </div>
                            
                            <div class="info-label" data-i18n="playerNameLabel">Игрок</div>
                            <div class="info-value">
                                <input type="text" class="text-input" id="playerName" data-i18n-placeholder="playerNamePlaceholder" placeholder="Ваше имя">
                            </div>
                        </div>
                    </div>
                    
                    <div class="stats-container">
                        <!-- Сила -->
                        <div class="stat-box">
                            <div class="stat-name" data-i18n="strength">Сила</div>
                            <div class="stat-value strength" id="strengthValue">0</div>
                            <div class="stat-controls">
                                <button class="stat-btn strength-btn" onclick="changeStat('strength', -1)">-</button>
                                <button class="stat-btn strength-btn" onclick="changeStat('strength', 1)">+</button>
                            </div>
                            
                            <div class="save-box">
                                <div class="save-header">
                                    <label class="save-label">
                                        <input type="checkbox" class="save-checkbox" id="strengthSaveCheckbox">
                                        <span data-i18n="savingThrow">Спас-бросок:</span>
                                    </label>
                                    <span class="save-value" id="strengthSaveValue">0</span>
                                </div>
                                <div class="save-controls">
                                    <button class="save-btn strength-btn" onclick="changeSave('strength', -1)">-</button>
                                    <button class="save-btn strength-btn" onclick="changeSave('strength', 1)">+</button>
                                </div>
                            </div>
                        </div>
                        
                        <!-- Ловкость -->
                        <div class="stat-box">
                            <div class="stat-name" data-i18n="agility">Ловкость</div>
                            <div class="stat-value agility" id="agilityValue">0</div>
                            <div class="stat-controls">
                                <button class="stat-btn agility-btn" onclick="changeStat('agility', -1)">-</button>
                                <button class="stat-btn agility-btn" onclick="changeStat('agility', 1)">+</button>
                            </div>
                            
                            <div class="save-box">
                                <div class="save-header">
                                    <label class="save-label">
                                        <input type="checkbox" class="save-checkbox" id="agilitySaveCheckbox">
                                        <span data-i18n="savingThrow">Спас-бросок:</span>
                                    </label>
                                    <span class="save-value" id="agilitySaveValue">0</span>
                                </div>
                                <div class="save-controls">
                                    <button class="save-btn agility-btn" onclick="changeSave('agility', -1)">-</button>
                                    <button class="save-btn agility-btn" onclick="changeSave('agility', 1)">+</button>
                                </div>
                            </div>
                        </div>
                        
                        <!-- Телосложение -->
                        <div class="stat-box">
                            <div class="stat-name" data-i18n="constitution">Телосложение</div>
                            <div class="stat-value constitution" id="constitutionValue">0</div>
                            <div class="stat-controls">
                                <button class="stat-btn constitution-btn" onclick="changeStat('constitution', -1)">-</button>
                                <button class="stat-btn constitution-btn" onclick="changeStat('constitution', 1)">+</button>
                            </div>
                            
                            <div class="save-box">
                                <div class="save-header">
                                    <label class="save-label">
                                        <input type="checkbox" class="save-checkbox" id="constitutionSaveCheckbox">
                                        <span data-i18n="savingThrow">Спас-бросок:</span>
                                    </label>
                                    <span class="save-value" id="constitutionSaveValue">0</span>
                                </div>
                                <div class="save-controls">
                                    <button class="save-btn constitution-btn" onclick="changeSave('constitution', -1)">-</button>
                                    <button class="save-btn constitution-btn" onclick="changeSave('constitution', 1)">+</button>
                                </div>
                            </div>
                        </div>
                        
                        <!-- Мудрость -->
                        <div class="stat-box">
                            <div class="stat-name" data-i18n="wisdom">Мудрость</div>
                            <div class="stat-value wisdom" id="wisdomValue">0</div>
                            <div class="stat-controls">
                                <button class="stat-btn wisdom-btn" onclick="changeStat('wisdom', -1)">-</button>
                                <button class="stat-btn wisdom-btn" onclick="changeStat('wisdom', 1)">+</button>
                            </div>
                            
                            <div class="save-box">
                                <div class="save-header">
                                    <label class="save-label">
                                        <input type="checkbox" class="save-checkbox" id="wisdomSaveCheckbox">
                                        <span data-i18n="savingThrow">Спас-бросок:</span>
                                    </label>
                                    <span class="save-value" id="wisdomSaveValue">0</span>
                                </div>
                                <div class="save-controls">
                                    <button class="save-btn wisdom-btn" onclick="changeSave('wisdom', -1)">-</button>
                                    <button class="save-btn wisdom-btn" onclick="changeSave('wisdom', 1)">+</button>
                                </div>
                            </div>
                        </div>
                        
                        <!-- Интеллект -->
                        <div class="stat-box">
                            <div class="stat-name" data-i18n="intelligence">Интеллект</div>
                            <div class="stat-value intelligence" id="intelligenceValue">0</div>
                            <div class="stat-controls">
                                <button class="stat-btn intelligence-btn" onclick="changeStat('intelligence', -1)">-</button>
                                <button class="stat-btn intelligence-btn" onclick="changeStat('intelligence', 1)">+</button>
                            </div>
                            
                            <div class="save-box">
                                <div class="save-header">
                                    <label class="save-label">
                                        <input type="checkbox" class="save-checkbox" id="intelligenceSaveCheckbox">
                                        <span data-i18n="savingThrow">Спас-бросок:</span>
                                    </label>
                                    <span class="save-value" id="intelligenceSaveValue">0</span>
                                </div>
                                <div class="save-controls">
                                    <button class="save-btn intelligence-btn" onclick="changeSave('intelligence', -1)">-</button>
                                    <button class="save-btn intelligence-btn" onclick="changeSave('intelligence', 1)">+</button>
                                </div>
                            </div>
                        </div>
                        
                        <!-- Харизма -->
                        <div class="stat-box">
                            <div class="stat-name" data-i18n="charisma">Харизма</div>
                            <div class="stat-value charisma" id="charismaValue">0</div>
                            <div class="stat-controls">
                                <button class="stat-btn charisma-btn" onclick="changeStat('charisma', -1)">-</button>
                                <button class="stat-btn charisma-btn" onclick="changeStat('charisma', 1)">+</button>
                            </div>
                            
                            <div class="save-box">
                                <div class="save-header">
                                    <label class="save-label">
                                        <input type="checkbox" class="save-checkbox" id="charismaSaveCheckbox">
                                        <span data-i18n="savingThrow">Спас-бросок:</span>
                                    </label>
                                    <span class="save-value" id="charismaSaveValue">0</span>
                                </div>
                                <div class="save-controls">
                                    <button class="save-btn charisma-btn" onclick="changeSave('charisma', -1)">-</button>
                                    <button class="save-btn charisma-btn" onclick="changeSave('charisma', 1)">+</button>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Секция здоровья -->
                    <div class="health-section">
                        <div class="health-box" style="background: var(--health-bg);">
                            <div class="health-title current-health" data-i18n="currentHealth">Текущие очки здоровья</div>
                            <div class="health-value current-health" id="currentHealthValue">10</div>
                            <input type="number" class="health-input" id="maxHealthInput" data-i18n-placeholder="maxHealthPlaceholder" placeholder="Макс. здоровье" value="10">
                            <div class="health-controls">
                                <button class="health-btn" onclick="changeHealth('current', -1)">-1</button>
                                <button class="health-btn" onclick="changeHealth('current', -5)">-5</button>
                                <button class="health-btn" onclick="changeHealth('current', 1)">+1</button>
                                <button class="health-btn" onclick="changeHealth('current', 5)">+5</button>
                            </div>
                        </div>
                        
                        <div class="health-box" style="background: var(--temp-health-bg);">
                            <div class="health-title temp-health" data-i18n="tempHealth">Временные очки здоровья</div>
                            <div class="health-value temp-health" id="tempHealthValue">0</div>
                            <div class="health-controls">
                                <button class="health-btn" onclick="changeHealth('temp', -1)">-1</button>
                                <button class="health-btn" onclick="changeHealth('temp', -5)">-5</button>
                                <button class="health-btn" onclick="changeHealth('temp', 1)">+1</button>
                                <button class="health-btn" onclick="changeHealth('temp', 5)">+5</button>
                            </div>
                        </div>
                    </div>

                    <!-- Секция инвентаря -->
                    <div class="inventory-section">
                        <div class="inventory-title" data-i18n="inventoryTitle">Инвентарь</div>
                        
                        <div class="inventory-row">
                            <label class="inventory-label" data-i18n="inventory1Label">1. Оружие и броня</label>
                            <input type="text" class="inventory-input" id="inventory1" data-i18n-placeholder="inventory1Placeholder" placeholder="Меч, кольчуга, щит">
                        </div>
                        
                        <div class="inventory-row">
                            <label class="inventory-label" data-i18n="inventory2Label">2. Предметы и снаряжение</label>
                            <input type="text" class="inventory-input" id="inventory2" data-i18n-placeholder="inventory2Placeholder" placeholder="Верёвка, факелы, зелья">
                        </div>
                        
                        <div class="inventory-row">
                            <label class="inventory-label" data-i18n="inventory3Label">3. Сокровища и деньги</label>
                            <input type="text" class="inventory-input" id="inventory3" data-i18n-placeholder="inventory3Placeholder" placeholder="100 золотых, драгоценный камень">
                        </div>
                    </div>
                </div>

                <!-- Боковая панель -->
                <div class="sidebar">
                    <!-- Класс брони -->
                    <div class="side-box">
                        <div class="side-title" data-i18n="armorClass">Класс брони</div>
                        <div class="side-value armor-value" id="armorValue">10</div>
                        <div class="side-controls">
                            <button class="side-btn" onclick="changeValue('armor', -1)">-</button>
                            <button class="side-btn" onclick="changeValue('armor', 1)">+</button>
                        </div>
                    </div>
                    
                    <!-- Инициатива -->
                    <div class="side-box">
                        <div class="side-title" data-i18n="initiative">Инициатива</div>
                        <div class="side-value initiative-value" id="initiativeValue">0</div>
                        <div class="side-controls">
                            <button class="side-btn" onclick="changeValue('initiative', -1)">-</button>
                            <button class="side-btn" onclick="changeValue('initiative', 1)">+</button>
                        </div>
                    </div>
                    
                    <!-- Скорость -->
                    <div class="side-box">
                        <div class="side-title" data-i18n="speed">Скорость</div>
                        <div class="side-value speed-value" id="speedValue">30</div>
                        <div class="side-controls">
                            <button class="side-btn" onclick="changeValue('speed', -5)">-5</button>
                            <button class="side-btn" onclick="changeValue('speed', 5)">+5</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Вкладка способностей -->
        <div class="tab-content" id="abilities-tab">
            <div class="main-content">
                <div style="width: 100%;">
                    <h1 style="text-align: center; margin-bottom: 30px;" data-i18n="abilitiesTitle">Способности</h1>
                    
                    <div class="abilities-section" id="abilitiesContainer">
                        <!-- Способности будут добавляться здесь динамически -->
                    </div>
                    
                    <button class="add-ability-btn" onclick="addAbility()" data-i18n="addAbilityButton">Добавить способность</button>
                </div>
                
                <!-- Боковая панель -->
                <div class="sidebar">
                    <div class="side-box">
                        <div class="side-title" data-i18n="abilitiesTipsTitle">Советы</div>
                        <p style="padding: 0 10px;" data-i18n="abilitiesTipsText">
                            Записывайте здесь все способности, умения и особенности вашего персонажа: 
                            классовые способности, расовые черты, умения владения оружием и т.д.
                        </p>
                    </div>
                </div>
            </div>
        </div>

        <!-- Вкладка персональных черт -->
        <div class="tab-content" id="traits-tab">
            <div class="main-content">
                <div style="width: 100%;">
                    <h1 style="text-align: center; margin-bottom: 30px;" data-i18n="personalTraitsTitle">Персональные черты</h1>
                    
                    <div class="traits-section">
                        <div class="trait-row">
                            <label style="display: block; margin-bottom: 8px; font-weight: bold;" data-i18n="personalityTraitsLabel">1. Черты характера</label>
                            <input type="text" class="trait-input" id="personalityTraits" data-i18n-placeholder="personalityTraitsPlaceholder" placeholder="Описание характера персонажа">
                        </div>
                        
                        <div class="trait-row">
                            <label style="display: block; margin-bottom: 8px; font-weight: bold;" data-i18n="idealsLabel">2. Идеалы</label>
                            <input type="text" class="trait-input" id="ideals" data-i18n-placeholder="idealsPlaceholder" placeholder="Во что верит персонаж">
                        </div>
                        
                        <div class="trait-row">
                            <label style="display: block; margin-bottom: 8px; font-weight: bold;" data-i18n="bondsLabel">3. Привязанности</label>
                            <input type="text" class="trait-input" id="bonds" data-i18n-placeholder="bondsPlaceholder" placeholder="Что дорого персонажу">
                        </div>
                        
                        <div class="trait-row">
                            <label style="display: block; margin-bottom: 8px; font-weight: bold;" data-i18n="flawsLabel">4. Слабости</label>
                            <input type="text" class="trait-input" id="flaws" data-i18n-placeholder="flawsPlaceholder" placeholder="Слабые стороны персонажа">
                        </div>
                    </div>
                </div>
                
                <!-- Боковая панель -->
                <div class="sidebar">
                    <div class="side-box">
                        <div class="side-title" data-i18n="notesTitle">Заметки</div>
                        <textarea style="width: 100%; height: 300px; background: rgba(255, 255, 255, 0.1); border: 1px solid rgba(255, 255, 255, 0.2); border-radius: 8px; color: var(--text-light); padding: 10px;" id="notes" data-i18n-placeholder="notesPlaceholder" placeholder="Дополнительные заметки о персонаже..."></textarea>
                    </div>
                </div>
            </div>
        </div>

        <!-- Вкладка настроек -->
        <div class="tab-content" id="settings-tab">
            <div class="main-content">
                <div style="width: 100%;">
                    <h1 style="text-align: center; margin-bottom: 30px;" data-i18n="settingsTitle">Настройки</h1>
                    
                    <div class="settings-section">
                        <div class="setting-row">
                            <span class="setting-label" data-i18n="languageLabel">Язык</span>
                            <div class="setting-control">
                                <select class="language-selector" id="languageSelector" onchange="changeLanguage(this.value)">
                                    <option value="ru">Русский</option>
                                    <option value="en">English</option>
                                </select>
                            </div>
                        </div>
                        
                        <div class="setting-row">
                            <span class="setting-label" data-i18n="darkModeLabel">Тёмный режим</span>
                            <div class="setting-control">
                                <label class="switch">
                                    <input type="checkbox" id="darkModeToggle" checked>
                                    <span class="slider"></span>
                                </label>
                            </div>
                        </div>
                        
                        <div class="setting-row">
                            <span class="setting-label" data-i18n="autoSaveLabel">Автосохранение</span>
                            <div class="setting-control">
                                <label class="switch">
                                    <input type="checkbox" id="autoSaveToggle" checked>
                                    <span class="slider"></span>
                                </label>
                            </div>
                        </div>
                        
                        <div class="setting-row">
                            <span class="setting-label" data-i18n="animationsLabel">Анимации</span>
                            <div class="setting-control">
                                <label class="switch">
                                    <input type="checkbox" id="animationsToggle" checked>
                                    <span class="slider"></span>
                                </label>
                            </div>
                        </div>
                        
                        <div class="setting-row">
                            <span class="setting-label" data-i18n="resetAllLabel">Сбросить все данные</span>
                            <button class="reset-btn" onclick="resetAllData()" data-i18n="resetButton">Сбросить</button>
                        </div>
                    </div>
                </div>
                
                <!-- Боковая панель для настроек -->
                <div class="sidebar">
                    <div class="side-box">
                        <div class="side-title" data-i18n="aboutTitle">О приложении</div>
                        <p style="text-align: center; margin-top: 15px;">RPG Character Sheet v1.0</p>
                        <p style="text-align: center;" data-i18n="aboutDescription">Создано для настольных RPG</p>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Инициализация характеристик и спас-бросков
        const stats = {
            strength: 0,
            agility: 0,
            constitution: 0,
            wisdom: 0,
            intelligence: 0,
            charisma: 0
        };

        const saves = {
            strength: 0,
            agility: 0,
            constitution: 0,
            wisdom: 0,
            intelligence: 0,
            charisma: 0
        };

        const saveProficiencies = {
            strength: false,
            agility: false,
            constitution: false,
            wisdom: false,
            intelligence: false,
            charisma: false
        };

        // Новые значения для боковой панели
        const sideValues = {
            armor: 10,
            initiative: 0,
            speed: 30
        };

        // Значения здоровья
        const healthValues = {
            current: 10,
            max: 10,
            temp: 0
        };

        // Персональные черты
        const traits = {
            personalityTraits: '',
            ideals: '',
            bonds: '',
            flaws: '',
            notes: ''
        };

        // Способности
        let abilities = [];

        // Настройки приложения
        const appSettings = {
            darkMode: true,
            autoSave: true,
            animations: true,
            language: 'ru'
        };

        // Локализация
        const translations = {
            ru: {
                mainTab: "Основные характеристики",
                abilitiesTab: "Способности",
                traitsTab: "Персональные черты",
                settingsTab: "Настройки",
                characterName: "Имя персонажа",
                raceLabel: "Раса",
                racePlaceholder: "Человек",
                classLevelLabel: "Класс и уровень",
                classLevelPlaceholder: "Воин 1",
                backgroundLabel: "Предыстория",
                backgroundPlaceholder: "Народный герой",
                alignmentLabel: "Мировоззрение",
                alignmentPlaceholder: "Добрый",
                experienceLabel: "Опыт",
                playerNameLabel: "Игрок",
                playerNamePlaceholder: "Ваше имя",
                strength: "Сила",
                agility: "Ловкость",
                constitution: "Телосложение",
                wisdom: "Мудрость",
                intelligence: "Интеллект",
                charisma: "Харизма",
                savingThrow: "Спас-бросок:",
                currentHealth: "Текущие очки здоровья",
                maxHealthPlaceholder: "Макс. здоровье",
                tempHealth: "Временные очки здоровья",
                inventoryTitle: "Инвентарь",
                inventory1Label: "1. Оружие и броня",
                inventory1Placeholder: "Меч, кольчуга, щит",
                inventory2Label: "2. Предметы и снаряжение",
                inventory2Placeholder: "Верёвка, факелы, зелья",
                inventory3Label: "3. Сокровища и деньги",
                inventory3Placeholder: "100 золотых, драгоценный камень",
                armorClass: "Класс брони",
                initiative: "Инициатива",
                speed: "Скорость",
                personalTraitsTitle: "Персональные черты",
                personalityTraitsLabel: "1. Черты характера",
                personalityTraitsPlaceholder: "Описание характера персонажа",
                idealsLabel: "2. Идеалы",
                idealsPlaceholder: "Во что верит персонаж",
                bondsLabel: "3. Привязанности",
                bondsPlaceholder: "Что дорого персонажу",
                flawsLabel: "4. Слабости",
                flawsPlaceholder: "Слабые стороны персонажа",
                notesTitle: "Заметки",
                notesPlaceholder: "Дополнительные заметки о персонаже...",
                settingsTitle: "Настройки",
                languageLabel: "Язык",
                darkModeLabel: "Тёмный режим",
                autoSaveLabel: "Автосохранение",
                animationsLabel: "Анимации",
                resetAllLabel: "Сбросить все данные",
                resetButton: "Сбросить",
                aboutTitle: "О приложении",
                aboutDescription: "Создано для настольных RPG",
                abilitiesTitle: "Способности",
                addAbilityButton: "Добавить способность",
                abilitiesTipsTitle: "Советы",
                abilitiesTipsText: "Записывайте здесь все способности, умения и особенности вашего персонажа: классовые способности, расовые черты, умения владения оружием и т.д."
            },
            en: {
                mainTab: "Main Stats",
                abilitiesTab: "Abilities",
                traitsTab: "Personal Traits",
                settingsTab: "Settings",
                characterName: "Character Name",
                raceLabel: "Race",
                racePlaceholder: "Human",
                classLevelLabel: "Class & Level",
                classLevelPlaceholder: "Fighter 1",
                backgroundLabel: "Background",
                backgroundPlaceholder: "Folk Hero",
                alignmentLabel: "Alignment",
                alignmentPlaceholder: "Good",
                experienceLabel: "Experience",
                playerNameLabel: "Player",
                playerNamePlaceholder: "Your Name",
                strength: "Strength",
                agility: "Agility",
                constitution: "Constitution",
                wisdom: "Wisdom",
                intelligence: "Intelligence",
                charisma: "Charisma",
                savingThrow: "Saving Throw:",
                currentHealth: "Current HP",
                maxHealthPlaceholder: "Max HP",
                tempHealth: "Temporary HP",
                inventoryTitle: "Inventory",
                inventory1Label: "1. Weapons & Armor",
                inventory1Placeholder: "Sword, chainmail, shield",
                inventory2Label: "2. Items & Equipment",
                inventory2Placeholder: "Rope, torches, potions",
                inventory3Label: "3. Treasure & Money",
                inventory3Placeholder: "100 gold, gemstone",
                armorClass: "Armor Class",
                initiative: "Initiative",
                speed: "Speed",
                personalTraitsTitle: "Personal Traits",
                personalityTraitsLabel: "1. Personality Traits",
                personalityTraitsPlaceholder: "Character's personality description",
                idealsLabel: "2. Ideals",
                idealsPlaceholder: "What the character believes in",
                bondsLabel: "3. Bonds",
                bondsPlaceholder: "What's important to the character",
                flawsLabel: "4. Flaws",
                flawsPlaceholder: "Character's weaknesses",
                notesTitle: "Notes",
                notesPlaceholder: "Additional notes about the character...",
                settingsTitle: "Settings",
                languageLabel: "Language",
                darkModeLabel: "Dark Mode",
                autoSaveLabel: "Auto Save",
                animationsLabel: "Animations",
                resetAllLabel: "Reset All Data",
                resetButton: "Reset",
                aboutTitle: "About",
                aboutDescription: "Created for tabletop RPGs",
                abilitiesTitle: "Abilities",
                addAbilityButton: "Add Ability",
                abilitiesTipsTitle: "Tips",
                abilitiesTipsText: "Record all your character's abilities, skills and features here: class abilities, racial traits, weapon proficiencies, etc."
            }
        };

        // Функция для добавления новой способности
        function addAbility(name = '', description = '') {
            const abilityId = Date.now();
            abilities.push({
                id: abilityId,
                name: name,
                description: description
            });
            
            renderAbilities();
            saveCharacterData();
        }

        // Функция для удаления способности
        function deleteAbility(id) {
            abilities = abilities.filter(ability => ability.id !== id);
            renderAbilities();
            saveCharacterData();
        }

        // Функция для обновления способности
        function updateAbility(id, field, value) {
            const ability = abilities.find(a => a.id === id);
            if (ability) {
                ability[field] = value;
                saveCharacterData();
            }
        }

        // Функция для отображения всех способностей
        function renderAbilities() {
            const container = document.getElementById('abilitiesContainer');
            container.innerHTML = '';
            
            if (abilities.length === 0) {
                const emptyMsg = document.createElement('p');
                emptyMsg.textContent = appSettings.language === 'ru' ? 
                    'Нет добавленных способностей' : 'No abilities added';
                emptyMsg.style.textAlign = 'center';
                emptyMsg.style.opacity = '0.7';
                container.appendChild(emptyMsg);
                return;
            }
            
            abilities.forEach(ability => {
                const abilityRow = document.createElement('div');
                abilityRow.className = 'ability-row';
                abilityRow.innerHTML = `
                    <div class="ability-header">
                        <input type="text" class="ability-name" value="${ability.name}" 
                            onchange="updateAbility(${ability.id}, 'name', this.value)"
                            placeholder="${appSettings.language === 'ru' ? 'Название способности' : 'Ability name'}">
                        <div class="ability-controls">
                            <button class="delete-ability-btn" onclick="deleteAbility(${ability.id})">
                                ${appSettings.language === 'ru' ? 'Удалить' : 'Delete'}
                            </button>
                        </div>
                    </div>
                    <textarea class="ability-description" 
                        onchange="updateAbility(${ability.id}, 'description', this.value)"
                        placeholder="${appSettings.language === 'ru' ? 'Описание способности...' : 'Ability description...'}">${ability.description}</textarea>
                `;
                container.appendChild(abilityRow);
            });
        }

        // Функция переключения языка
        function changeLanguage(lang) {
            appSettings.language = lang;
            document.getElementById('languageSelector').value = lang;
            applyTranslations();
            saveSettings();
        }

        // Применение переводов
        function applyTranslations() {
            const lang = appSettings.language;
            const translation = translations[lang];
            
            // Обновляем текст элементов с атрибутом data-i18n
            document.querySelectorAll('[data-i18n]').forEach(el => {
                const key = el.getAttribute('data-i18n');
                if (translation[key]) {
                    el.textContent = translation[key];
                }
            });
            
            // Обновляем placeholder'ы
            document.querySelectorAll('[data-i18n-placeholder]').forEach(el => {
                const key = el.getAttribute('data-i18n-placeholder');
                if (translation[key]) {
                    el.setAttribute('placeholder', translation[key]);
                }
            });
            
            // Обновляем title страницы
            document.title = lang === 'ru' ? 'RPG Персонаж' : 'RPG Character Sheet';
            
            // Переводим кнопку добавления способности
            const addAbilityBtn = document.querySelector('.add-ability-btn');
            if (addAbilityBtn) {
                addAbilityBtn.textContent = translation['addAbilityButton'];
            }
            
            // Перерисовываем способности для обновления текстов
            renderAbilities();
        }

        // Функция переключения вкладок
        function switchTab(tabName) {
            // Скрыть все вкладки
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.remove('active');
            });
            
            // Деактивировать все кнопки
            document.querySelectorAll('.tab-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            // Показать выбранную вкладку
            document.getElementById(tabName + '-tab').classList.add('active');
            
            // Активировать выбранную кнопку
            event.currentTarget.classList.add('active');
        }

        // Изменение характеристики
        function changeStat(statName, amount) {
            stats[statName] += amount;
            updateStatDisplay(statName);
            animateValue(statName + 'Value');
            saveCharacterData();
        }

        // Изменение спас-броска
        function changeSave(statName, amount) {
            saves[statName] += amount;
            document.getElementById(statName + 'SaveValue').textContent = saves[statName];
            animateValue(statName + 'SaveValue');
            saveCharacterData();
        }

        // Переключение галочки спас-броска
        function toggleSaveProficiency(statName) {
            saveProficiencies[statName] = !saveProficiencies[statName];
            saveCharacterData();
        }

        // Изменение здоровья
        function changeHealth(type, amount) {
            if (type === 'max') {
                healthValues.max = Math.max(1, healthValues.max + amount);
                document.getElementById('maxHealthInput').value = healthValues.max;
            } else {
                healthValues[type] = Math.max(0, healthValues[type] + amount);
                if (type === 'current') {
                    healthValues[type] = Math.min(healthValues[type], healthValues.max);
                }
                document.getElementById(type + 'HealthValue').textContent = healthValues[type];
            }
            saveCharacterData();
        }

        // Обработка изменения максимального здоровья
        document.getElementById('maxHealthInput').addEventListener('change', function() {
            healthValues.max = Math.max(1, parseInt(this.value) || 1);
            healthValues.current = Math.min(healthValues.current, healthValues.max);
            document.getElementById('currentHealthValue').textContent = healthValues.current;
            this.value = healthValues.max;
            saveCharacterData();
        });

        // Изменение значений на боковой панели
        function changeValue(type, amount) {
            sideValues[type] += amount;
            document.getElementById(type + 'Value').textContent = sideValues[type];
            animateValue(type + 'Value');
            saveCharacterData();
        }

        // Обновление отображения
        function updateStatDisplay(statName) {
            document.getElementById(statName + 'Value').textContent = stats[statName];
        }

        // Анимация изменения значения
        function animateValue(elementId) {
            if (!appSettings.animations) return;
            
            const element = document.getElementById(elementId);
            element.classList.add('value-animate');
            setTimeout(() => {
                element.classList.remove('value-animate');
            }, 300);
        }

        // Сохранение данных персонажа
        function saveCharacterData() {
            if (!appSettings.autoSave) return;
            
            // Обновляем персональные черты
            traits.personalityTraits = document.getElementById('personalityTraits').value;
            traits.ideals = document.getElementById('ideals').value;
            traits.bonds = document.getElementById('bonds').value;
            traits.flaws = document.getElementById('flaws').value;
            traits.notes = document.getElementById('notes').value;

            const characterData = {
                name: document.getElementById('characterName').textContent,
                race: document.getElementById('race').value,
                classLevel: document.getElementById('classLevel').value,
                background: document.getElementById('background').value,
                alignment: document.getElementById('alignment').value,
                experience: document.getElementById('experience').value,
                playerName: document.getElementById('playerName').value,
                stats: {...stats},
                saves: {...saves},
                saveProficiencies: {...saveProficiencies},
                inventory1: document.getElementById('inventory1').value,
                inventory2: document.getElementById('inventory2').value,
                inventory3: document.getElementById('inventory3').value,
                sideValues: {...sideValues},
                healthValues: {...healthValues},
                traits: {...traits},
                abilities: [...abilities]
            };
            localStorage.setItem('rpgCharacter', JSON.stringify(characterData));
        }

        // Загрузка данных персонажа
        function loadCharacterData() {
            const savedData = localStorage.getItem('rpgCharacter');
            if (savedData) {
                const characterData = JSON.parse(savedData);
                
                // Заполняем текстовые поля
                document.getElementById('characterName').textContent = characterData.name || translations[appSettings.language]['characterName'];
                document.getElementById('race').value = characterData.race || "";
                document.getElementById('classLevel').value = characterData.classLevel || "";
                document.getElementById('background').value = characterData.background || "";
                document.getElementById('alignment').value = characterData.alignment || "";
                document.getElementById('experience').value = characterData.experience || "";
                document.getElementById('playerName').value = characterData.playerName || "";
                document.getElementById('inventory1').value = characterData.inventory1 || "";
                document.getElementById('inventory2').value = characterData.inventory2 || "";
                document.getElementById('inventory3').value = characterData.inventory3 || "";
                
                // Восстанавливаем характеристики
                for (const stat in characterData.stats) {
                    stats[stat] = characterData.stats[stat] || 0;
                    updateStatDisplay(stat);
                }
                
                // Восстанавливаем спас-броски
                for (const save in characterData.saves) {
                    saves[save] = characterData.saves[save] || 0;
                    document.getElementById(save + 'SaveValue').textContent = saves[save];
                }
                
                // Восстанавливаем галочки спас-бросков
                if (characterData.saveProficiencies) {
                    for (const save in characterData.saveProficiencies) {
                        saveProficiencies[save] = characterData.saveProficiencies[save] || false;
                        document.getElementById(save + 'SaveCheckbox').checked = saveProficiencies[save];
                    }
                }
                
                // Восстанавливаем значения боковой панели
                if (characterData.sideValues) {
                    for (const sideValue in characterData.sideValues) {
                        sideValues[sideValue] = characterData.sideValues[sideValue] || 
                            (sideValue === 'armor' ? 10 : 
                             sideValue === 'speed' ? 30 : 0);
                        document.getElementById(sideValue + 'Value').textContent = sideValues[sideValue];
                    }
                }
                
                // Восстанавливаем здоровье
                if (characterData.healthValues) {
                    healthValues.current = characterData.healthValues.current || 10;
                    healthValues.max = characterData.healthValues.max || 10;
                    healthValues.temp = characterData.healthValues.temp || 0;
                    document.getElementById('currentHealthValue').textContent = healthValues.current;
                    document.getElementById('maxHealthInput').value = healthValues.max;
                    document.getElementById('tempHealthValue').textContent = healthValues.temp;
                }
                
                // Восстанавливаем персональные черты
                if (characterData.traits) {
                    traits.personalityTraits = characterData.traits.personalityTraits || '';
                    traits.ideals = characterData.traits.ideals || '';
                    traits.bonds = characterData.traits.bonds || '';
                    traits.flaws = characterData.traits.flaws || '';
                    traits.notes = characterData.traits.notes || '';
                    
                    document.getElementById('personalityTraits').value = traits.personalityTraits;
                    document.getElementById('ideals').value = traits.ideals;
                    document.getElementById('bonds').value = traits.bonds;
                    document.getElementById('flaws').value = traits.flaws;
                    document.getElementById('notes').value = traits.notes;
                }
                
                // Восстанавливаем способности
                if (characterData.abilities) {
                    abilities = characterData.abilities;
                    renderAbilities();
                }
            }
        }

        // Загрузка настроек
        function loadSettings() {
            const savedSettings = localStorage.getItem('rpgCharacterSettings');
            if (savedSettings) {
                const settings = JSON.parse(savedSettings);
                appSettings.darkMode = settings.darkMode !== undefined ? settings.darkMode : true;
                appSettings.autoSave = settings.autoSave !== undefined ? settings.autoSave : true;
                appSettings.animations = settings.animations !== undefined ? settings.animations : true;
                appSettings.language = settings.language || 'ru';
                
                // Применяем настройки
                document.getElementById('darkModeToggle').checked = appSettings.darkMode;
                document.getElementById('autoSaveToggle').checked = appSettings.autoSave;
                document.getElementById('animationsToggle').checked = appSettings.animations;
                document.getElementById('languageSelector').value = appSettings.language;
                
                applySettings();
                applyTranslations();
            }
        }

        // Применение настроек
        function applySettings() {
            // Применение темы
            if (appSettings.darkMode) {
                document.documentElement.style.setProperty('--bg-dark', '#121212');
                document.documentElement.style.setProperty('--card-dark', '#1e1e1e');
                document.documentElement.style.setProperty('--text-light', '#e0e0e0');
            } else {
                document.documentElement.style.setProperty('--bg-dark', '#f0f0f0');
                document.documentElement.style.setProperty('--card-dark', '#ffffff');
                document.documentElement.style.setProperty('--text-light', '#333333');
            }
            
            // Сохраняем настройки
            saveSettings();
        }

        // Сохранение настроек
        function saveSettings() {
            localStorage.setItem('rpgCharacterSettings', JSON.stringify(appSettings));
        }

        // Сброс всех данных
        function resetAllData() {
            if (confirm(appSettings.language === 'ru' ? 
                'Вы уверены, что хотите сбросить ВСЕ данные персонажа? Это действие нельзя отменить.' : 
                'Are you sure you want to reset ALL character data? This action cannot be undone.')) {
                localStorage.removeItem('rpgCharacter');
                localStorage.removeItem('rpgCharacterSettings');
                location.reload();
            }
        }

        // Настройка автосохранения
        function setupAutoSave() {
            const inputs = document.querySelectorAll('input, textarea, [contenteditable="true"]');
            inputs.forEach(input => {
                if (input.id !== 'maxHealthInput') {
                    input.addEventListener('input', function() {
                        if (appSettings.autoSave) {
                            saveCharacterData();
                        }
                    });
                }
            });
        }

        // Загрузка при старте
        window.addEventListener('DOMContentLoaded', function() {
            loadSettings();
            loadCharacterData();
            setupAutoSave();
            
            // Навешиваем обработчики на галочки спас-бросков
            for (const stat in saveProficiencies) {
                document.getElementById(stat + 'SaveCheckbox').addEventListener('change', function() {
                    toggleSaveProficiency(stat);
                });
            }
            
            // Обработчики событий для переключателей
            document.getElementById('darkModeToggle').addEventListener('change', function() {
                appSettings.darkMode = this.checked;
                applySettings();
            });

            document.getElementById('autoSaveToggle').addEventListener('change', function() {
                appSettings.autoSave = this.checked;
                saveSettings();
            });

            document.getElementById('animationsToggle').addEventListener('change', function() {
                appSettings.animations = this.checked;
                saveSettings();
            });
        });
    </script>
</body>
</html>
