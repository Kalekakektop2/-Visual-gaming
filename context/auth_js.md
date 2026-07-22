# Фронтенд: js/auth.js (Управление авторизацией)

**Проект:** Game-Vision Analyzer
**Суть файла:** Логика работы с Supabase Auth. Обрабатывает форму входа, регистрации, выход из аккаунта, переключение между модалками и отслеживание текущей сессии пользователя.
**Исходный файл:** `js/auth.js`

---

## 💻 JavaScript Код

```javascript
// Модуль авторизации через Supabase Auth
const gvAuth = {
    currentUser: null,

    async init() {
        // Проверяем текущую сессию при загрузке страницы
        const { data: { session } } = await window.supabaseClient.auth.getSession();
        this.currentUser = session?.user || null;
        this.updateUI();

        // Слушаем изменения состояния авторизации (вход/выход)
        window.supabaseClient.auth.onAuthStateChange((event, session) => {
            this.currentUser = session?.user || null;
            this.updateUI();
        });
    },

    getUser() {
        return this.currentUser;
    },

    async register(email, password) {
        const { data, error } = await window.supabaseClient.auth.signUp({
            email,
            password
        });
        if (error) throw error;
        return data;
    },

    async login(email, password) {
        const { data, error } = await window.supabaseClient.auth.signInWithPassword({
            email,
            password
        });
        if (error) throw error;
        return data;
    },

    async logout() {
        const { error } = await window.supabaseClient.auth.signOut();
        if (error) throw error;
        window.location.href = 'index.html';
    },

    updateUI() {
        const userEmailSpan = document.getElementById('userEmail');
        const authButtons = document.getElementById('authButtons');
        const userMenu = document.getElementById('userMenu');

        if (this.currentUser) {
            if (userEmailSpan) userEmailSpan.textContent = this.currentUser.email;
            if (authButtons) authButtons.classList.add('hidden');
            if (userMenu) userMenu.classList.remove('hidden');
        } else {
            if (authButtons) authButtons.classList.remove('hidden');
            if (userMenu) userMenu.classList.add('hidden');
        }
    },

    openLoginModal() {
        const modal = document.getElementById('authModal');
        if (modal) modal.classList.remove('hidden');
    },

    closeLoginModal() {
        const modal = document.getElementById('authModal');
        if (modal) modal.classList.add('hidden');
    }
};

document.addEventListener('DOMContentLoaded', () => {
    gvAuth.init();
});

window.gvAuth = gvAuth;
