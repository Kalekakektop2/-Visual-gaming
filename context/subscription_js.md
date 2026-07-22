# Фронтенд: js/subscription.js (Логика подписок)

**Проект:** Game-Vision Analyzer
**Суть файла:** Управление подписками пользователя через таблицу `public.subscriptions` в Supabase. Отвечает за проверку статуса, расчет оставшихся дней и симуляцию продления доступа.
**Исходный файл:** `js/subscription.js`

---

## 💻 JavaScript Код

```javascript
// Модуль управления подписками
const gvSub = {
    currentSub: null,

    async fetchSubscription(userId) {
        if (!userId) return null;
        const { data, error } = await window.supabaseClient
            .from('subscriptions')
            .select('*')
            .eq('user_id', userId)
            .single();

        if (error) {
            console.error('Ошибка получения подписки:', error);
            return null;
        }

        this.currentSub = data;
        return data;
    },

    async buy() {
        const user = window.gvAuth?.getUser();
        if (!user) {
            alert('Сначала войдите в систему!');
            return;
        }

        const now = new Date();
        const expires = new Date();
        expires.setDate(now.getDate() + 30); // Подписка на 30 дней

        const { data, error } = await window.supabaseClient
            .from('subscriptions')
            .update({
                status: 'active',
                purchased_at: now.toISOString(),
                expires_at: expires.toISOString()
            })
            .eq('user_id', user.id)
            .select()
            .single();

        if (error) {
            alert('Ошибка при оформлении подписки: ' + error.message);
            return;
        }

        this.currentSub = data;
        alert('Подписка успешно активирована на 30 дней!');
        window.location.reload();
    },

    getDaysLeft() {
        if (!this.currentSub || !this.currentSub.expires_at) return null;
        if (this.currentSub.status !== 'active') return 0;

        const expDate = new Date(this.currentSub.expires_at);
        const now = new Date();
        const diffTime = expDate - now;
        const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));

        return diffDays > 0 ? diffDays : 0;
    }
};

window.gvSub = gvSub;
