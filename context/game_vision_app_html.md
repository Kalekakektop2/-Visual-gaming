# Фронтенд: game-vision.html (Главный интерфейс приложения)

**Проект:** Game-Vision Analyzer
**Суть файла:** Основной рабочий интерфейс приложения, где геймер взаимодействует с ИИ-ассистентом, выбирает игры/сценарии и получает тактические подсказки в реальном времени.
**Исходный файл:** `game-vision.html`

---

## 💻 HTML Код

```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Game-Vision Analyzer — AI-ассистент для геймеров</title>
    <script src="[https://cdn.tailwindcss.com](https://cdn.tailwindcss.com)"></script>
    <link rel="preconnect" href="[https://fonts.googleapis.com](https://fonts.googleapis.com)">
    <link rel="preconnect" href="[https://fonts.gstatic.com](https://fonts.gstatic.com)" crossorigin>
    <link href="[https://fonts.googleapis.com/css2?family=Orbitron:wght@500;700;900&family=Rajdhani:wght@400;500;600;700&family=Inter:wght@400;500;600&display=swap](https://fonts.googleapis.com/css2?family=Orbitron:wght@500;700;900&family=Rajdhani:wght@400;500;600;700&family=Inter:wght@400;500;600&display=swap)" rel="stylesheet">
    <style>
        :root {
            --neon-cyan: #00f0ff;
            --neon-purple: #b026ff;
            --neon-pink: #ff2e9a;
            --bg-deep: #07060d;
            --bg-card: #0f0e1a;
        }

        * { font-family: 'Rajdhani', 'Inter', sans-serif; }
        h1, h2, h3, .font-display { font-family: 'Orbitron', sans-serif; }

        body {
            background: var(--bg-deep);
            color: #e8e8f0;
            overflow-x: hidden;
        }

        .grid-bg {
            background-image: 
                linear-gradient(to right, rgba(0, 240, 255, 0.03) 1px, transparent 1px),
                linear-gradient(to bottom, rgba(0, 240, 255, 0.03) 1px, transparent 1px);
            background-size: 32px 32px;
        }

        .neon-border {
            border: 1px solid rgba(0, 240, 255, 0.2);
            box-shadow: 0 0 15px rgba(0, 240, 255, 0.05);
            transition: all 0.3s ease;
        }

        .neon-border:hover {
            border-color: rgba(0, 240, 255, 0.5);
            box-shadow: 0 0 25px rgba(0, 240, 255, 0.15);
        }

        .btn-glow {
            background: linear-gradient(135deg, #00f0ff 0%, #b026ff 100%);
            color: #07060d;
            font-weight: 700;
            transition: all 0.3s ease;
            box-shadow: 0 0 20px rgba(0, 240, 255, 0.4);
        }

        .btn-glow:hover {
            transform: translateY(-2px);
            box-shadow: 0 0 30px rgba(176, 38, 255, 0.6);
        }
    </style>
</head>
<body class="grid-bg min-h-screen flex flex-col">

    <nav class="border-b border-white/10 bg-black/40 backdrop-blur-md sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-6 h-16 flex items-center justify-between">
            <a href="index.html" class="flex items-center gap-3">
                <div class="w-9 h-9 rounded-lg bg-gradient-to-br from-cyan-400 to-purple-600 flex items-center justify-center font-display font-black text-black">GV</div>
                <span class="font-display font-bold tracking-wider text-white">GAME-VISION <span class="text-cyan-400 text-xs px-2 py-0.5 rounded bg-cyan-950/60 border border-cyan-500/30">APP</span></span>
            </a>
            <div class="flex items-center gap-4">
                <a href="profile.html" class="text-sm text-gray-300 hover:text-cyan-400 transition">Личный кабинет</a>
                <a href="index.html" class="text-sm text-gray-400 hover:text-white transition">Выход</a>
            </div>
        </div>
    </nav>

    <main class="flex-1 max-w-7xl w-full mx-auto p-6 grid grid-cols-1 lg:grid-cols-4 gap-6">
        <div class="lg:col-span-1 space-y-6">
            <div class="bg-[#0f0e1a] border border-white/10 rounded-2xl p-5">
                <h2 class="font-display text-sm font-bold text-cyan-400 mb-4 uppercase tracking-wider">Параметры сессии</h2>
                <div class="space-y-4">
                    <div>
                        <label class="block text-xs text-gray-400 mb-1">Выбрать игру</label>
                        <select id="gameSelect" class="w-full bg-black/60 border border-white/10 rounded-xl px-3 py-2 text-sm text-white focus:outline-none focus:border-cyan-400">
                            <option value="cs2">Counter-Strike 2</option>
                            <option value="dota2">Dota 2</option>
                            <option value="valorant">Valorant</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs text-gray-400 mb-1">Режим ИИ</label>
                        <select id="modeSelect" class="w-full bg-black/60 border border-white/10 rounded-xl px-3 py-2 text-sm text-white focus:outline-none focus:border-cyan-400">
                            <option value="tactical">Тактический анализ</option>
                            <option value="coach">Персональный тренер</option>
                        </select>
                    </div>
                </div>
            </div>
        </div>

        <div class="lg:col-span-3 space-y-6 flex flex-col">
            <div class="bg-[#0f0e1a] border border-white/10 rounded-2xl p-6 flex-1 flex flex-col justify-between min-h-[400px]">
                <div id="displayArea" class="text-center text-gray-500 py-20">
                    <p>Инициализация видеопотока и ИИ-ассистента...</p>
                </div>
                <div class="border-t border-white/10 pt-4 flex gap-3">
                    <input type="text" id="userQuestion" placeholder="Задай вопрос ИИ по текущей ситуации..." class="flex-1 bg-black/50 border border-white/10 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-cyan-400">
                    <button onclick="askQuestion()" class="btn-glow px-6 py-3 rounded-xl text-sm">Спросить</button>
                </div>
            </div>
        </div>
    </main>

    <script>
        function askQuestion() {
            const input = document.getElementById('userQuestion');
            const display = document.getElementById('displayArea');
            if (!input.value.trim()) return;
            display.innerHTML = `<div class="text-cyan-400 animate-pulse">ИИ анализирует: "${input.value}"...</div>`;
            input.value = '';
        }
    </script>
</body>
</html>
