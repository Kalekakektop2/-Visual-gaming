# Фронтенд: js/config.js (Конфигурация Supabase)

**Проект:** Game-Vision Analyzer
**Суть файла:** Инициализация клиента Supabase для связи фронтенда с базой данных и сервисом авторизации.
**Исходный файл:** `js/config.js`

---

## 💻 JavaScript Код

```javascript
// Конфигурация Supabase
// Вставь свои ключи из панели Supabase (Project Settings -> API)
const SUPABASE_URL = '[https://твой-проект.supabase.co](https://твой-проект.supabase.co)';
const SUPABASE_ANON_KEY = 'твой-длинный-anon-ключ';

// Инициализация глобального клиента Supabase
window.supabaseClient = supabase.createClient(SUPABASE_URL, SUPABASE_ANON_KEY);
