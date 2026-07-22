# Фронтенд: js/i18n.js (Система мультиязычности)

**Проект:** Game-Vision Analyzer
**Суть файла:** Модуль интернационализации (i18n). Содержит словари текстов для русского (RU) и английского (EN) языков, управляет переключением языка и автоматическим обновлением интерфейса.
**Исходный файл:** `js/i18n.js`

---

## 💻 JavaScript Код

```javascript
const translations = {
    ru: {
        "nav.logout": "Выйти",
        "profile.title": "Личный кабинет",
        "profile.statusLabel": "Статус подписки",
        "profile.expiresLabel": "Действует до",
        "profile.daysLabel": "Осталось дней",
        "profile.manageTitle": "Продление доступа",
        "profile.manageDesc": "Активируйте или продлите подписку для полного доступа ко всем функциям ИИ.",
        "profile.renewBtn": "Активировать / Продлить",
        "profile.active": "Активна",
        "profile.inactive": "Неактивна",
        "profile.notSet": "Не задано",
        "auth.passwordHint": "Минимум 6 символов",
        "auth.registerBtn": "Зарегистрироваться",
        "auth.toLogin": "Уже есть аккаунт? Войти"
    },
    en: {
        "nav.logout": "Log out",
        "profile.title": "Personal Account",
        "profile.statusLabel": "Subscription Status",
        "profile.expiresLabel": "Expires At",
        "profile.daysLabel": "Days Left",
        "profile.manageTitle": "Extend Access",
        "profile.manageDesc": "Activate or extend your subscription for full access to all AI features.",
        "profile.renewBtn": "Activate / Extend",
        "profile.active": "Active",
        "profile.inactive": "Inactive",
        "profile.notSet": "Not set",
        "auth.passwordHint": "At least 6 characters",
        "auth.registerBtn": "Register",
        "auth.toLogin": "Already have an account? Sign in"
    }
};

let currentLang = localStorage.getItem('gv_lang') || 'ru';

function t(key) {
    return translations[currentLang]?.[key] || key;
}

function setLanguage(lang) {
    if (!translations[lang]) return;
    currentLang = lang;
    localStorage.setItem('gv_lang', lang);
    
    document.querySelectorAll('[data-i18n]').forEach(el => {
        const key = el.getAttribute('data-i18n');
        if (translations[lang][key]) {
            el.textContent = translations[lang][key];
        }
    });

    const langSelect = document.getElementById('langSwitch');
    if (langSelect) langSelect.value = lang;

    if (typeof window.refreshProfile === 'function') {
        window.refreshProfile();
    }
}

document.addEventListener('DOMContentLoaded', () => {
    const langSelect = document.getElementById('langSwitch');
    if (langSelect) {
        langSelect.value = currentLang;
        langSelect.addEventListener('change', (e) => {
            setLanguage(e.target.value);
        });
    }
    setLanguage(currentLang);
});

window.t = t;
window.setLanguage = setLanguage;
