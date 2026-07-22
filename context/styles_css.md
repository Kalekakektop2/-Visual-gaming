# Фронтенд: css/styles.css (Стили и оформление)

**Проект:** Game-Vision Analyzer
**Суть файла:** Глобальные стили интерфейса в киберпанк-стилистике. Содержит анимации пульсации неоновых сфер, стили карточек, сетку фона (grid-bg) и кастомные цвета.
**Исходный файл:** `css/styles.css`

---

## 💻 CSS Код

```css
:root {
    --neon-cyan: #00f0ff;
    --neon-purple: #b026ff;
    --neon-pink: #ff2e9a;
    --bg-deep: #07060d;
    --bg-card: #0f0e1a;
}

body {
    background-color: var(--bg-deep);
    color: #e8e8f0;
    font-family: 'Rajdhani', sans-serif;
}

.grid-bg {
    background-image: 
        linear-gradient(to right, rgba(255,255,255,0.03) 1px, transparent 1px),
        linear-gradient(to bottom, rgba(255,255,255,0.03) 1px, transparent 1px);
    background-size: 32px 32px;
}

.orb {
    position: absolute;
    border-radius: 50%;
    filter: blur(100px);
    opacity: 0.15;
    pointer-events: none;
    animation: float 10s ease-in-out infinite alternate;
}

@keyframes float {
    0% { transform: translateY(0px) scale(1); }
    100% { transform: translateY(-30px) scale(1.1); }
}

.card-cyber {
    background: var(--bg-card);
    border: 1px solid rgba(0, 240, 255, 0.15);
    box-shadow: 0 0 20px rgba(0, 240, 255, 0.05);
    transition: all 0.3s ease;
}

.card-cyber:hover {
    border-color: rgba(0, 240, 255, 0.4);
    box-shadow: 0 0 25px rgba(0, 240, 255, 0.15);
}

.btn-cta {
    background: linear-gradient(135deg, var(--neon-cyan), var(--neon-purple));
    transition: opacity 0.2s ease, transform 0.2s ease;
}

.btn-cta:hover {
    opacity: 0.9;
    transform: translateY(-1px);
}
