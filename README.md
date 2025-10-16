
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Calculadora BerMatMods</title>
  <!-- Fuente Poppins (gruesa y moderna) -->
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg: #0f172a;
      --card: #1e293b;
      --display: #06d6a0;
      --btn: #334155;
      --btn-hover: #475569;
      --op: #7c3aed;
      --eq: #06b6d4;
      --clear: #ef4444;
      --sci-trig: #8b5cf6;
      --sci-hyp: #a78bfa;
      --sci-log: #3b82f6;
      --sci-exp: #10b981;
      --sci-const: #ec4899;
      --sci-other: #f59e0b;
      --body-bg: linear-gradient(135deg, #7c3aed, #06b6d4);
    }

    * {
      box-sizing: border-box;
    }

    body {
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      margin: 0;
      background: var(--body-bg);
      font-family: 'Poppins', 'Segoe UI', sans-serif;
      transition: background 0.4s ease;
      padding: 10px;
    }

    .calc {
      background: var(--bg);
      padding: 24px;
      border-radius: 24px;
      box-shadow: 0 12px 40px rgba(0, 0, 0, 0.5);
      width: 100%;
      max-width: 400px;
      position: relative;
      transition: all 0.3s ease;
    }

    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 18px;
    }

    .menu-btn {
      background: var(--btn);
      border: none;
      width: 40px;
      height: 40px;
      border-radius: 10px;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      cursor: pointer;
      padding: 0;
    }

    .menu-btn span {
      width: 22px;
      height: 3px;
      background: white;
      margin: 2px 0;
      border-radius: 2px;
    }

    .display {
      background: var(--card);
      color: var(--display);
      font-size: clamp(1.5rem, 5vw, 2rem);
      padding: 18px;
      border-radius: 16px;
      text-align: right;
      overflow-x: auto;
      white-space: nowrap;
      font-weight: 700;
      letter-spacing: -0.5px;
      box-shadow: inset 0 0 12px rgba(6, 214, 160, 0.2);
    }

    .keys {
      display: grid;
      grid-template-columns: repeat(5, 1fr);
      gap: 9px;
      margin-top: 12px;
    }

    button {
      padding: 12px 8px;
      font-size: 0.95rem;
      border: none;
      border-radius: 12px;
      cursor: pointer;
      background: var(--btn);
      color: white;
      transition: all 0.2s ease;
      font-weight: 700;
      text-shadow: 0 1px 2px rgba(0,0,0,0.4);
    }

    button:hover {
      transform: translateY(-2px);
      box-shadow: 0 5px 10px rgba(0,0,0,0.3);
    }

    .op { background: var(--op); }
    .eq { background: var(--eq); }
    .clear { background: var(--clear); }
    .sci-trig { background: var(--sci-trig); }
    .sci-hyp { background: var(--sci-hyp); }
    .sci-log { background: var(--sci-log); }
    .sci-exp { background: var(--sci-exp); }
    .sci-const { background: var(--sci-const); }
    .sci-other { background: var(--sci-other); }

    /* === Panel de Configuración === */
    .settings-panel {
      position: fixed;
      top: 0;
      left: -100%;
      width: 320px;
      height: 100vh;
      background: var(--bg);
      padding: 30px 20px;
      box-shadow: 4px 0 25px rgba(0,0,0,0.6);
      transition: left 0.4s cubic-bezier(0.4, 0, 0.2, 1);
      z-index: 1000;
      overflow-y: auto;
      font-family: 'Poppins', sans-serif;
    }

    .settings-panel.active {
      left: 0;
    }

    .close-settings {
      position: absolute;
      top: 25px;
      right: 25px;
      background: #334155;
      border: none;
      color: white;
      width: 36px;
      height: 36px;
      border-radius: 50%;
      cursor: pointer;
      font-weight: bold;
      font-size: 1.2rem;
    }

    .settings-title {
      color: white;
      font-size: 1.6rem;
      margin-bottom: 25px;
      text-align: center;
      font-weight: 700;
    }

    .setting-group {
      margin-bottom: 25px;
    }

    .setting-group h3 {
      color: #a5b4fc;
      margin-bottom: 14px;
      font-size: 1.2rem;
      font-weight: 700;
    }

    .color-option, .bg-option {
      display: inline-block;
      width: 36px;
      height: 36px;
      border-radius: 8px;
      margin: 5px;
      cursor: pointer;
      border: 2px solid transparent;
      transition: transform 0.2s;
    }

    .color-option:hover, .bg-option:hover {
      transform: scale(1.1);
    }

    .color-option.active, .bg-option.active {
      border-color: white;
      transform: scale(1.15);
    }

    .font-size-slider {
      width: 100%;
      margin-top: 10px;
      height: 8px;
      -webkit-appearance: none;
      background: #334155;
      border-radius: 4px;
      outline: none;
    }

    .font-size-slider::-webkit-slider-thumb {
      -webkit-appearance: none;
      width: 20px;
      height: 20px;
      border-radius: 50%;
      background: var(--eq);
      cursor: pointer;
    }

    /* === Perfil === */
    .profile-info {
      background: var(--card);
      padding: 22px;
      border-radius: 18px;
      margin-top: 20px;
      text-align: center;
      border: 1px solid rgba(139, 92, 246, 0.3);
    }

    .profile-info h3 {
      color: var(--display);
      margin: 0 0 12px 0;
      font-size: 1.4rem;
      font-weight: 700;
    }

    .profile-info p {
      color: #cbd5e1;
      font-size: 1rem;
      margin-bottom: 14px;
      line-height: 1.5;
    }

    .whatsapp-link {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      color: #34d399;
      text-decoration: none;
      font-weight: 700;
      font-size: 1.05rem;
      margin-top: 8px;
    }

    .whatsapp-link:hover {
      color: #10b981;
      text-decoration: underline;
    }

    .credits {
      text-align: center;
      color: rgba(255, 255, 255, 0.65);
      font-size: 0.85rem;
      margin-top: 16px;
      font-family: 'Poppins', sans-serif;
      font-weight: 600;
      letter-spacing: 0.4px;
    }
  </style>
</head>
<body>
  <!-- Panel de Configuración -->
  <div class="settings-panel" id="settingsPanel">
    <button class="close-settings" id="closeSettings">✕</button>
    <h2 class="settings-title">⚙️ Configuración</h2>

    <div class="setting-group">
      <h3>🎨 Tema de colores</h3>
      <div>
        <div class="color-option" style="background: linear-gradient(135deg, #7c3aed, #06b6d4);" data-theme="default"></div>
        <div class="color-option" style="background: linear-gradient(135deg, #0ea5e9, #8b5cf6);" data-theme="blue-purple"></div>
        <div class="color-option" style="background: linear-gradient(135deg, #10b981, #f59e0b);" data-theme="green-amber"></div>
        <div class="color-option" style="background: linear-gradient(135deg, #ef4444, #f97316);" data-theme="red-orange"></div>
        <div class="color-option" style="background: linear-gradient(135deg, #000, #333);" data-theme="dark"></div>
      </div>
    </div>

    <div class="setting-group">
      <h3>🖼️ Fondo de pantalla</h3>
      <div>
        <div class="bg-option" style="background: linear-gradient(135deg, #7c3aed, #06b6d4);" data-bg="1"></div>
        <div class="bg-option" style="background: linear-gradient(135deg, #ec4899, #8b5cf6);" data-bg="2"></div>
        <div class="bg-option" style="background: linear-gradient(135deg, #06b6d4, #0ea5e9);" data-bg="3"></div>
        <div class="bg-option" style="background: linear-gradient(135deg, #10b981, #34d399);" data-bg="4"></div>
      </div>
    </div>

    <div class="setting-group">
      <h3>🔤 Tamaño del display</h3>
      <input type="range" min="1.4" max="2.4" step="0.1" value="1.8" class="font-size-slider" id="fontSizeSlider">
    </div>

    <div class="profile-info">
      <h3>👨‍💻 AnthZz Berrocal BerMatMods</h3>
      <p>Programador creativo especializado en crear detalles virtuales únicos y experiencias digitales memorables.</p>
      <a href="https://wa.me/51930569195" target="_blank" class="whatsapp-link">
        💬 Contactar Por WhatsApp
      </a>
    </div>
  </div>

  <!-- Calculadora -->
  <div class="calc" id="calculator">
    <div class="header">
      <button class="menu-btn" id="menuBtn">
        <span></span>
        <span></span>
        <span></span>
      </button>
    </div>
    <div class="display" id="display">0</div>
    <div class="keys">
      <button class="clear">C</button>
      <button class="sci-const">π</button>
      <button class="sci-const">e</button>
      <button class="op">(</button>
      <button class="op">)</button>

      <button class="sci-trig">sin</button>
      <button class="sci-trig">cos</button>
      <button class="sci-trig">tan</button>
      <button class="sci-trig">asin</button>
      <button class="sci-trig">acos</button>

      <button class="sci-hyp">sinh</button>
      <button class="sci-hyp">cosh</button>
      <button class="sci-hyp">tanh</button>
      <button class="sci-trig">atan</button>
      <button class="op">/</button>

      <button class="sci-log">log</button>
      <button class="sci-log">ln</button>
      <button class="sci-exp">eˣ</button>
      <button class="sci-exp">10ˣ</button>
      <button class="op">*</button>

      <button class="sci-other">x²</button>
      <button class="sci-other">xʸ</button>
      <button class="sci-other">√</button>
      <button class="sci-other">!</button>
      <button class="op">-</button>

      <button class="sci-other">1/x</button>
      <button class="sci-other">|x|</button>
      <button class="sci-other">⌊x⌋</button>
      <button class="sci-other">⌈x⌉</button>
      <button class="op">+</button>

      <button class="sci-other">deg</button>
      <button class="sci-other">rad</button>
      <button class="sci-other">rand</button>
      <button>7</button>
      <button>8</button>

      <button>9</button>
      <button>4</button>
      <button>5</button>
      <button>6</button>
      <button class="eq">=</button>

      <button>1</button>
      <button>2</button>
      <button>3</button>
      <button>0</button>
      <button>.</button>
    </div>
    <div class="credits">By AnthZz Berrocal BerMatMods</div>
  </div>

  <!-- Sonidos -->
  <audio id="clickSound" src="https://files.catbox.moe/deprpa.mp3" preload="auto"></audio>
  <audio id="resultSound" src="https://files.catbox.moe/z9agnn.mp3" preload="auto"></audio>

  <script>
    // === SONIDOS ===
    const clickSound = document.getElementById('clickSound');
    const resultSound = document.getElementById('resultSound');

    function playClick() {
      clickSound.currentTime = 0;
      clickSound.play().catch(() => {});
    }
    function playResult() {
      resultSound.currentTime = 0;
      resultSound.play().catch(() => {});
    }

    // === CONFIGURACIÓN ===
    const settingsPanel = document.getElementById('settingsPanel');
    const menuBtn = document.getElementById('menuBtn');
    const closeSettings = document.getElementById('closeSettings');
    const colorOptions = document.querySelectorAll('.color-option');
    const bgOptions = document.querySelectorAll('.bg-option');
    const fontSizeSlider = document.getElementById('fontSizeSlider');
    const root = document.documentElement;
    const body = document.body;
    const display = document.getElementById('display');

    const THEMES = {
      default: { bg: '#0f172a', card: '#1e293b', display: '#06d6a0', bodyBg: 'linear-gradient(135deg, #7c3aed, #06b6d4)' },
      'blue-purple': { bg: '#0f172a', card: '#1e293b', display: '#60a5fa', bodyBg: 'linear-gradient(135deg, #0ea5e9, #8b5cf6)' },
      'green-amber': { bg: '#0f172a', card: '#1e293b', display: '#34d399', bodyBg: 'linear-gradient(135deg, #10b981, #f59e0b)' },
      'red-orange': { bg: '#0f172a', card: '#1e293b', display: '#f87171', bodyBg: 'linear-gradient(135deg, #ef4444, #f97316)' },
      dark: { bg: '#000000', card: '#111827', display: '#818cf8', bodyBg: 'linear-gradient(135deg, #000, #333)' }
    };

    const BACKGROUNDS = {
      '1': 'linear-gradient(135deg, #7c3aed, #06b6d4)',
      '2': 'linear-gradient(135deg, #ec4899, #8b5cf6)',
      '3': 'linear-gradient(135deg, #06b6d4, #0ea5e9)',
      '4': 'linear-gradient(135deg, #10b981, #34d399)'
    };

    function applyTheme(themeKey) {
      const t = THEMES[themeKey] || THEMES.default;
      root.style.setProperty('--bg', t.bg);
      root.style.setProperty('--card', t.card);
      root.style.setProperty('--display', t.display);
      root.style.setProperty('--body-bg', t.bodyBg);
    }

    function applyBackground(bgKey) {
      body.style.background = BACKGROUNDS[bgKey] || BACKGROUNDS['1'];
    }

    function saveSettings(settings) {
      localStorage.setItem('calcSettings_v2', JSON.stringify(settings));
    }

    function loadSettings() {
      const saved = JSON.parse(localStorage.getItem('calcSettings_v2')) || {
        theme: 'default',
        bg: '1',
        fontSize: 1.8
      };

      // Aplicar tema
      applyTheme(saved.theme);
      document.querySelectorAll('.color-option').forEach(el => el.classList.remove('active'));
      const themeEl = document.querySelector(`.color-option[data-theme="${saved.theme}"]`);
      if (themeEl) themeEl.classList.add('active');

      // Aplicar fondo
      applyBackground(saved.bg);
      document.querySelectorAll('.bg-option').forEach(el => el.classList.remove('active'));
      const bgEl = document.querySelector(`.bg-option[data-bg="${saved.bg}"]`);
      if (bgEl) bgEl.classList.add('active');

      // Aplicar tamaño
      fontSizeSlider.value = saved.fontSize;
      display.style.fontSize = `${saved.fontSize}rem`;
    }

    // Eventos
    menuBtn.addEventListener('click', () => {
      settingsPanel.classList.add('active');
      playClick();
    });

    closeSettings.addEventListener('click', () => {
      settingsPanel.classList.remove('active');
      playClick();
    });

    colorOptions.forEach(opt => {
      opt.addEventListener('click', () => {
        colorOptions.forEach(o => o.classList.remove('active'));
        opt.classList.add('active');
        const theme = opt.dataset.theme;
        applyTheme(theme);
        const current = JSON.parse(localStorage.getItem('calcSettings_v2') || '{}');
        saveSettings({ ...current, theme });
        playClick();
      });
    });

    bgOptions.forEach(opt => {
      opt.addEventListener('click', () => {
        bgOptions.forEach(o => o.classList.remove('active'));
        opt.classList.add('active');
        const bg = opt.dataset.bg;
        applyBackground(bg);
        const current = JSON.parse(localStorage.getItem('calcSettings_v2') || '{}');
        saveSettings({ ...current, bg });
        playClick();
      });
    });

    fontSizeSlider.addEventListener('input', () => {
      const size = parseFloat(fontSizeSlider.value);
      display.style.fontSize = `${size}rem`;
      const current = JSON.parse(localStorage.getItem('calcSettings_v2') || '{}');
      saveSettings({ ...current, fontSize: size });
    });

    // === MOTOR DE CÁLCULO ===
    let current = '';

    function factorial(n) {
      if (n < 0 || !Number.isInteger(n)) return NaN;
      if (n === 0 || n === 1) return 1;
      let res = 1;
      for (let i = 2; i <= n; i++) res *= i;
      return res;
    }
    function deg(x) { return x * 180 / Math.PI; }
    function rad(x) { return x * Math.PI / 180; }

    function evaluate(expr) {
      let processed = expr
        .replace(/π/g, 'Math.PI')
        .replace(/e(?![\w])/g, 'Math.E')
        .replace(/√\(/g, 'Math.sqrt(')
        .replace(/√([0-9.]+)/g, 'Math.sqrt($1)')
        .replace(/x²/g, '**2')
        .replace(/xʸ/g, '**')
        .replace(/\^/g, '**')
        .replace(/sin\(/g, 'Math.sin(')
        .replace(/cos\(/g, 'Math.cos(')
        .replace(/tan\(/g, 'Math.tan(')
        .replace(/asin\(/g, 'Math.asin(')
        .replace(/acos\(/g, 'Math.acos(')
        .replace(/atan\(/g, 'Math.atan(')
        .replace(/sinh\(/g, 'Math.sinh(')
        .replace(/cosh\(/g, 'Math.cosh(')
        .replace(/tanh\(/g, 'Math.tanh(')
        .replace(/log\(/g, 'Math.log10(')
        .replace(/ln\(/g, 'Math.log(')
        .replace(/eˣ\(/g, 'Math.exp(')
        .replace(/10ˣ\(/g, 'tenPow(')
        .replace(/1\/x/g, 'inv(')
        .replace(/\|x\|/g, 'Math.abs(')
        .replace(/⌊x⌋/g, 'Math.floor(')
        .replace(/⌈x⌉/g, 'Math.ceil(')
        .replace(/deg\(/g, 'deg(')
        .replace(/rad\(/g, 'rad(')
        .replace(/rand/g, 'Math.random()');
      processed = processed.replace(/([0-9.]+)!/g, (_, num) => `factorial(${num})`);

      const tenPow = x => Math.pow(10, x);
      const inv = x => 1 / x;

      try {
        return Function('tenPow', 'inv', 'deg', 'rad', 'factorial',
          `'use strict'; return (${processed})`
        )(tenPow, inv, deg, rad, factorial);
      } catch (e) {
        throw new Error('Calc error');
      }
    }

    document.querySelectorAll('button').forEach(btn => {
      if ([menuBtn, closeSettings].includes(btn)) return;
      btn.addEventListener('click', () => {
        playClick();
        const val = btn.textContent;

        if (btn.classList.contains('clear')) {
          current = '';
          display.textContent = '0';
        } else if (btn.classList.contains('eq')) {
          playResult();
          try {
            if (!current.trim()) {
              display.textContent = '0';
              return;
            }
            let result = evaluate(current);
            if (typeof result !== 'number' || !isFinite(result)) throw new Error();
            result = parseFloat(result.toPrecision(12));
            current = result.toString();
            display.textContent = current;
          } catch (e) {
            display.textContent = 'Error';
            current = '';
          }
        } else {
          let append = val;
          const funcNames = ['sin','cos','tan','asin','acos','atan','sinh','cosh','tanh','log','ln','eˣ','10ˣ','deg','rad','√'];
          if (funcNames.includes(val)) {
            append = val + '(';
          } else if (val === 'x²') append = 'x²';
          else if (val === 'xʸ') append = 'xʸ';
          else if (val === '!') append = '!';
          else if (val === '1/x') append = '1/x';
          else if (val === '|x|') append = '|x|';
          else if (val === '⌊x⌋') append = '⌊x⌋';
          else if (val === '⌈x⌉') append = '⌈x⌉';

          current += append;
          display.textContent = current;
        }
      });
    });

    // Iniciar
    loadSettings();
  </script>
</body>
</html>
