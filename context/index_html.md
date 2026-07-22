# Фронтенд: index.html (Главная страница)

**Проект:** Game-Vision Analyzer
**Суть файла:** Исходный код главной страницы (лендинга) приложения. Содержит навигационную панель, секции описания, интерактивное демо и модальные окна авторизации (вход/регистрация).
**Исходный файл:** `index.html`

---

## 💻 HTML Код

```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Game-Vision Analyzer</title>
    <script src="[https://cdn.tailwindcss.com](https://cdn.tailwindcss.com)"></script>
    <link href="[https://fonts.googleapis.com/css2?family=Orbitron:wght@500;700;900&family=Rajdhani:wght@400;500;600;700&display=swap](https://fonts.googleapis.com/css2?family=Orbitron:wght@500;700;900&family=Rajdhani:wght@400;500;600;700&display=swap)" rel="stylesheet">
    <script src="[https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2](https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2)"></script>
    <link rel="stylesheet" href="css/styles.css">
</head>
<body class="grid-bg">

    <div class="orb" style="width:400px;height:400px;background:#00f0ff;top:-100px;left:-100px;"></div>
    <div class="orb" style="width:500px;height:500px;background:#b026ff;top:30%;right:-150px;animation-delay:2s;"></div>

    <nav class="navbar fixed top-0 left-0 right-0 z-50 border-b border-white/5">
        <div class="max-w-7xl mx-auto px-6 py-4 flex items-center justify-between gap-4">
            <a href="index.html" class="flex items-center gap-3 group">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-br from-cyan-400 to-purple-600 flex items-center justify-center font-display font-black text-black text-xl shadow-[0_0_20px_rgba(0,240,255,0.4)] group-hover:scale-105 transition">GV</div>
                <span class="font-display font-bold text-lg tracking-wider bg-gradient-to-r from-white via-cyan-200 to-cyan-400 bg-clip-text text-transparent">GAME-VISION</span>
            </a>

            <div class="hidden md:flex items-center gap-8">
                <a href="#features" class="text-sm text-gray-300 hover:text-cyan-400 transition" data-i18n="nav.features">Возможности</a>
                <a href="#demo" class="text-sm text-gray-300 hover:text-cyan-400 transition" data-i18n="nav.demo">Демо</a>
                <a href="#pricing" class="text-sm text-gray-300 hover:text-cyan-400 transition" data-i18n="nav.pricing">Цены</a>
            </div>

            <div class="flex items-center gap-3">
                <select id="langSwitch" class="bg-black/50 border border-white/10 rounded-lg px-2.5 py-1.5 text-xs text-gray-300 focus:outline-none focus:border-cyan-400 cursor-pointer">
                    <option value="ru" class="bg-gray-900">RU</option>
                    <option value="en" class="bg-gray-900">EN</option>
                </select>

                <div id="authNavArea">
                    <button onclick="gvAuth.openLogin()" class="btn-cta text-black font-bold px-5 py-2 rounded-xl text-sm" data-i18n="nav.login">Войти</button>
                </div>
            </div>
        </div>
    </nav>

    <header class="relative pt-36 pb-20 px-6 overflow-hidden">
        <div class="max-w-5xl mx-auto text-center relative z-10">
            <div class="inline-flex items-center gap-2 px-4 py-1.5 rounded-full bg-cyan-950/50 border border-cyan-500/30 text-cyan-400 text-xs font-semibold mb-6 animate-pulse">
                <span class="w-2 h-2 rounded-full bg-cyan-400"></span>
                <span data-i18n="hero.badge">ИИ-ассистент нового поколения</span>
            </div>
            <h1 class="font-display text-4xl md:text-6xl font-black tracking-tight mb-6 leading-tight">
                <span data-i18n="hero.title1">ДОМИНИРУЙ В ИГРАХ</span><br>
                <span class="bg-gradient-to-r from-cyan-400 via-purple-400 to-pink-500 bg-clip-text text-transparent" data-i18n="hero.title2">С ИИ-АНАЛИТИКОЙ</span>
            </h1>
            <p class="text-lg md:text-xl text-gray-400 max-w-2xl mx-auto mb-10" data-i18n="hero.desc">
                Мгновенный анализ экрана, тактические подсказки в реальном времени и персональный ИИ-тренер для киберспорта.
            </p>
            <div class="flex flex-col sm:flex-row items-center justify-center gap-4">
                <a href="#demo" class="btn-cta text-black font-bold px-8 py-4 rounded-xl text-base w-full sm:w-auto text-center" data-i18n="hero.ctaPrimary">Попробовать демо</a>
                <a href="#pricing" class="border border-white/20 hover:border-cyan-400/50 bg-white/5 hover:bg-white/10 font-bold px-8 py-4 rounded-xl text-base w-full sm:w-auto text-center transition" data-i18n="hero.ctaSecondary">Узнать тарифы</a>
            </div>
        </div>
    </header>

    <section id="demo" class="py-20 px-6 relative">
        <div class="max-w-5xl mx-auto">
            <div class="text-center mb-12">
                <h2 class="font-display text-3xl font-bold mb-3" data-i18n="demo.heading">ИНТЕРАКТИВНОЕ ДЕМО</h2>
                <p class="text-gray-400 text-sm" data-i18n="demo.subheading">Выбери ситуацию и посмотри, как ИИ помогает принимать решения</p>
            </div>
            
            <div class="card p-6 md:p-8 rounded-2xl border border-white/10 relative overflow-hidden">
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <div class="lg:col-span-1 space-y-3">
                        <label class="text-xs uppercase font-semibold text-gray-400 tracking-wider" data-i18n="demo.selectScenario">Выбери сценарий:</label>
                        <div id="scenarioList" class="space-y-2"></div>
                    </div>
                    <div class="lg:col-span-2 flex flex-col justify-between">
                        <div class="bg-black/40 rounded-xl p-4 border border-white/5 min-h-[220px] flex items-center justify-center relative">
                            <div id="demoScreen" class="text-center text-gray-500 text-sm">
                                <span data-i18n="demo.placeholder">Нажми на сценарий слева</span>
                            </div>
                        </div>
                        <div class="mt-4 pt-4 border-t border-white/10 flex items-center justify-between">
                            <span id="demoHint" class="text-xs text-gray-400"></span>
                            <button id="runAnalysisBtn" onclick="gvDemo.run()" class="btn-cta text-black font-semibold px-6 py-2 rounded-lg text-xs" data-i18n="demo.analyzeBtn">Анализировать</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <div id="authModal" class="fixed inset-0 z-50 bg-black/80 backdrop-blur-sm hidden flex items-center justify-center p-4">
        <div class="card w-full max-w-md p-8 rounded-2xl border border-white/10 relative">
            <button onclick="gvAuth.closeModal()" class="absolute top-4 right-4 text-gray-400 hover:text-white">&times;</button>
            
            <div id="loginView">
                <h3 class="font-display text-2xl font-bold mb-6 text-center" data-i18n="auth.loginTitle">Вход в систему</h3>
                <form id="loginForm" onsubmit="gvAuth.handleLogin(event)" class="space-y-4">
                    <div>
                        <label class="block text-xs font-semibold text-gray-400 uppercase mb-1" data-i18n="auth.email">Email</label>
                        <input type="email" id="loginEmail" required class="w-full bg-black/50 border border-white/10 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-cyan-400">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-400 uppercase mb-1" data-i18n="auth.password">Пароль</label>
                        <input type="password" id="loginPassword" required class="w-full bg-black/50 border border-white/10 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-cyan-400">
                    </div>
                    <div id="loginError" class="auth-error hidden"></div>
                    <button type="submit" class="btn-cta text-black font-bold w-full py-3 rounded-xl" data-i18n="auth.loginBtn">Войти</button>
                </form>
                <div class="text-center mt-4">
                    <button onclick="gvAuth.switchToRegister()" class="text-sm text-cyan-400 hover:underline" data-i18n="auth.toRegister">Нет аккаунта? Зарегистрироваться</button>
                </div>
            </div>

            <div id="registerView" class="hidden">
                <h3 class="font-display text-2xl font-bold mb-6 text-center" data-i18n="auth.registerTitle">Регистрация</h3>
                <form id="registerForm" onsubmit="gvAuth.handleRegister(event)" class="space-y-4">
                    <div>
                        <label class="block text-xs font-semibold text-gray-400 uppercase mb-1" data-i18n="auth.email">Email</label>
                        <input type="email" id="regEmail" required class="w-full bg-black/50 border border-white/10 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-cyan-400">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-400 uppercase mb-1" data-i18n="auth.password">Пароль</label>
                        <input type="password" id="regPassword" required class="w-full bg-black/50 border border-white/10 rounded-xl px-4 py-3 text-sm text-white focus:outline-none focus:border-cyan-400">
                        <p class="text-xs text-gray-600 mt-1" data-i18n="auth.passwordHint">Минимум 6 символов</p>
                    </div>
                    <div id="registerError" class="auth-error hidden"></div>
                    <button type="submit" class="btn-cta text-black font-bold w-full py-3 rounded-xl" data-i18n="auth.registerBtn">Зарегистрироваться</button>
                </form>
                <div class="text-center mt-4">
                    <button onclick="gvAuth.switchToLogin()" class="text-sm text-cyan-400 hover:underline" data-i18n="auth.toLogin">Уже есть аккаунт? Войти</button>
                </div>
            </div>
        </div>
    </div>

    <script src="js/config.js"></script>
    <script src="js/i18n.js"></script>
    <script src="js/auth.js"></script>
    <script src="js/subscription.js"></script>
    <script src="js/demo.js"></script>
</body>
</html>
