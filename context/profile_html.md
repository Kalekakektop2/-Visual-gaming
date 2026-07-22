# Фронтенд: profile.html (Личный кабинет)

**Проект:** Game-Vision Analyzer
**Суть файла:** Страница личного кабинета пользователя. Отвечает за отображение email, статуса подписки (активна/неактивна), оставшихся дней и кнопки управления подпиской/выхода из системы.
**Исходный файл:** `profile.html`

---

## 💻 HTML Код

```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Личный кабинет — Game-Vision</title>
    <script src="[https://cdn.tailwindcss.com](https://cdn.tailwindcss.com)"></script>
    <link href="[https://fonts.googleapis.com/css2?family=Orbitron:wght@500;700;900&family=Rajdhani:wght@400;500;600;700&display=swap](https://fonts.googleapis.com/css2?family=Orbitron:wght@500;700;900&family=Rajdhani:wght@400;500;600;700&display=swap)" rel="stylesheet">
    <script src="[https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2](https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2)"></script>
    <link rel="stylesheet" href="css/styles.css">
</head>
<body class="grid-bg">

    <div class="orb" style="width:400px;height:400px;background:#00f0ff;top:-100px;left:-100px;"></div>
    <div class="orb" style="width:500px;height:500px;background:#b026ff;top:40%;right:-150px;animation-delay:2s;"></div>

    <nav class="navbar fixed top-0 left-0 right-0 z-50 border-b border-white/5">
        <div class="max-w-7xl mx-auto px-6 py-4 flex items-center justify-between gap-4">
            <a href="index.html" class="flex items-center gap-3 group">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-br from-cyan-400 to-purple-600 flex items-center justify-center font-display font-black text-black text-xl shadow-[0_0_20px_rgba(0,240,255,0.4)] group-hover:scale-105 transition">GV</div>
                <span class="font-display font-bold text-lg tracking-wider bg-gradient-to-r from-white via-cyan-200 to-cyan-400 bg-clip-text text-transparent">GAME-VISION</span>
            </a>

            <div class="flex items-center gap-3">
                <select id="langSwitch" class="bg-black/50 border border-white/10 rounded-lg px-2.5 py-1.5 text-xs text-gray-300 focus:outline-none focus:border-cyan-400 cursor-pointer">
                    <option value="ru" class="bg-gray-900">RU</option>
                    <option value="en" class="bg-gray-900">EN</option>
                </select>
                <button onclick="gvAuth.logout()" class="border border-red-500/30 hover:border-red-500 bg-red-500/10 hover:bg-red-500/20 text-red-400 px-4 py-2 rounded-xl text-xs font-semibold transition" data-i18n="nav.logout">Выйти</button>
            </div>
        </div>
    </nav>

    <main class="relative pt-32 pb-20 px-6 max-w-4xl mx-auto">
        <div class="mb-8">
            <h1 class="font-display text-3xl font-bold mb-2" data-i18n="profile.title">Личный кабинет</h1>
            <p id="userEmailDisplay" class="text-gray-400 text-sm">Загрузка данных...</p>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
            <div class="card p-6 rounded-2xl border border-white/10">
                <span class="text-xs text-gray-400 uppercase tracking-wider block mb-2" data-i18n="profile.statusLabel">Статус подписки</span>
                <div id="subStatusBadge" class="inline-flex items-center gap-2 px-3 py-1 rounded-full text-xs font-bold bg-gray-800 text-gray-400">
                    <span class="w-2 h-2 rounded-full bg-gray-500"></span>
                    <span id="subStatusText">Проверка...</span>
                </div>
            </div>

            <div class="card p-6 rounded-2xl border border-white/10">
                <span class="text-xs text-gray-400 uppercase tracking-wider block mb-2" data-i18n="profile.expiresLabel">Действует до</span>
                <p id="subExpiresText" class="font-display font-bold text-lg text-gray-300">—</p>
            </div>

            <div class="card p-6 rounded-2xl border border-white/10">
                <span class="text-xs text-gray-400 uppercase tracking-wider block mb-2" data-i18n="profile.daysLabel">Осталось дней</span>
                <p id="subDaysText" class="font-display font-bold text-2xl text-cyan-400">—</p>
            </div>
        </div>

        <div class="card p-8 rounded-2xl border border-white/10 flex flex-col sm:flex-row items-center justify-between gap-4">
            <div>
                <h3 class="font-display font-bold text-lg mb-1" data-i18n="profile.manageTitle">Продление доступа</h3>
                <p class="text-gray-400 text-sm" data-i18n="profile.manageDesc">Активируйте или продлите подписку для полного доступа ко всем функциям ИИ.</p>
            </div>
            <button id="renewBtn" class="btn-cta text-black font-bold px-6 py-3 rounded-xl text-sm whitespace-nowrap" data-i18n="profile.renewBtn">Активировать / Продлить</button>
        </div>
    </main>

    <script src="js/config.js"></script>
    <script src="js/i18n.js"></script>
    <script src="js/auth.js"></script>
    <script src="js/subscription.js"></script>
    <script>
        async function renderProfile() {
            await waitForAuth();
            const user = window.gvAuth.getUser();
            if (!user) {
                window.location.href = 'index.html';
                return;
            }
            document.getElementById('userEmailDisplay').textContent = user.email;
            renderSubscription();
        }

        function renderSubscription() {
            const sub = window.gvSub.getData();
            const badge = document.getElementById('subStatusBadge');
            const statusText = document.getElementById('subStatusText');
            const expiresEl = document.getElementById('subExpiresText');
            const daysEl = document.getElementById('subDaysText');

            if (sub && sub.status === 'active') {
                badge.className = 'inline-flex items-center gap-2 px-3 py-1 rounded-full text-xs font-bold bg-cyan-950/80 text-cyan-400 border border-cyan-500/30';
                badge.querySelector('span').className = 'w-2 h-2 rounded-full bg-cyan-400 animate-pulse';
                statusText.textContent = window.t('profile.active');
            } else {
                badge.className = 'inline-flex items-center gap-2 px-3 py-1 rounded-full text-xs font-bold bg-gray-800 text-gray-400 border border-white/10';
                badge.querySelector('span').className = 'w-2 h-2 rounded-full bg-gray-500';
                statusText.textContent = window.t('profile.inactive');
            }

            if (sub && sub.expires_at) {
                const date = new Date(sub.expires_at).toLocaleDateString('ru-RU');
                expiresEl.textContent = date;
                expiresEl.classList.remove('text-gray-500');
                expiresEl.classList.add('text-gray-300');
            } else {
                expiresEl.textContent = window.t('profile.notSet');
                expiresEl.classList.add('text-gray-500');
            }

            const days = window.gvSub.getDaysLeft();
            daysEl.textContent = days !== null ? days : '0';
        }

        function waitForAuth() {
            return new Promise(resolve => {
                if (window.gvAuth && window.gvAuth.getUser()) return resolve();
                let tries = 0;
                const interval = setInterval(() => {
                    tries++;
                    if ((window.gvAuth && window.gvAuth.getUser()) || tries > 40) {
                        clearInterval(interval);
                        resolve();
                    }
                }, 150);
            });
        }

        window.refreshProfile = function () {
            if (window.gvAuth && window.gvAuth.getUser()) renderSubscription();
        };

        document.getElementById('renewBtn').addEventListener('click', () => {
            window.gvSub.buy();
        });

        document.addEventListener('DOMContentLoaded', renderProfile);
    </script>
</body>
</html>
