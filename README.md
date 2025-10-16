<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Calculadora Completa - AnthZz</title>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&family=Pacifico&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg: linear-gradient(135deg, #0f0c29, #302b63, #24243e);
      --screen-bg: rgba(0, 0, 0, 0.8);
      --screen-text: #00f3ff;
      --btn-basic: #2c2c3e;
      --btn-sci: #4a3a7d;
      --btn-special: #ff2a6d;
      --btn-equals: #00f3ff;
      --text: #ffffff;
      --error: #ff5555;
      --history-bg: rgba(30, 30, 50, 0.85);
      --credits-bg: rgba(20, 20, 40, 0.9);
    }

    .light-mode {
      --bg: linear-gradient(135deg, #f8f9ff, #e0e7ff);
      --screen-bg: rgba(250, 250, 255, 0.95);
      --screen-text: #0066cc;
      --btn-basic: #d5d9f0;
      --btn-sci: #b8a9e0;
      --btn-special: #ff6b9d;
      --btn-equals: #00ccff;
      --text: #1a1a2e;
      --error: #ff3333;
      --history-bg: rgba(245, 245, 255, 0.9);
      --credits-bg: rgba(240, 240, 255, 0.9);
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: 'Orbitron', monospace;
      background: var(--bg);
      color: var(--text);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 20px;
      transition: background 0.4s ease;
    }

    .calculator {
      width: 100%;
      max-width: 480px;
      background: rgba(20, 20, 40, 0.85);
      border-radius: 24px;
      padding: 24px;
      box-shadow: 0 12px 30px rgba(0, 0, 0, 0.6);
      backdrop-filter: blur(10px);
      margin-bottom: 30px;
      border: 1px solid rgba(255, 255, 255, 0.1);
    }

    .controls {
      display: flex;
      justify-content: flex-end;
      margin-bottom: 18px;
    }

    .theme-toggle {
      background: var(--btn-sci);
      color: white;
      border: none;
      padding: 8px 16px;
      border-radius: 20px;
      font-family: 'Orbitron', monospace;
      cursor: pointer;
      font-size: 0.9rem;
      transition: transform 0.2s;
    }

    .theme-toggle:hover {
      transform: translateY(-2px);
    }

    .display {
      background: var(--screen-bg);
      color: var(--screen-text);
      padding: 22px;
      font-size: 1.8rem;
      text-align: right;
      border-radius: 16px;
      margin-bottom: 20px;
      min-height: 80px;
      word-break: break-all;
      box-shadow: inset 0 0 12px rgba(0, 243, 255, 0.5);
      text-shadow: 0 0 8px rgba(0, 243, 255, 0.7);
      font-weight: 700;
    }

    .buttons {
      display: grid;
      grid-template-columns: repeat(5, 1fr);
      gap: 12px;
    }

    .btn {
      padding: 14px 0;
      font-size: 1rem;
      background: var(--btn-basic);
      color: white;
      border: none;
      border-radius: 14px;
      cursor: pointer;
      font-family: 'Orbitron', monospace;
      font-weight: 600;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
      transition: all 0.15s ease;
    }

    .btn.sci {
      background: var(--btn-sci);
    }

    .btn.special {
      background: var(--btn-special);
    }

    .btn.equals {
      background: var(--btn-equals);
      color: #000;
      font-weight: bold;
    }

    .btn:hover {
      transform: scale(1.03);
      box-shadow: 0 6px 12px rgba(0, 0, 0, 0.4);
    }

    .btn:active {
      transform: scale(0.98);
      box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
    }

    .history-toggle {
      text-align: center;
      margin-top: 16px;
      color: #aaa;
      font-size: 0.95rem;
      cursor: pointer;
      text-decoration: underline;
    }

    #historyList {
      max-height: 120px;
      overflow-y: auto;
      background: var(--history-bg);
      padding: 12px;
      border-radius: 12px;
      margin-top: 10px;
      font-size: 0.9rem;
      display: none;
      font-family: 'Orbitron', monospace;
    }

    .credits-box {
      width: 100%;
      max-width: 480px;
      background: var(--credits-bg);
      padding: 20px;
      border-radius: 20px;
      text-align: center;
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.5);
      border: 1px solid rgba(255, 204, 0, 0.3);
    }

    .credits {
      font-family: 'Pacifico', cursive;
      font-size: 1.7rem;
      color: #ffcc00;
      text-shadow: 0 0 10px rgba(255, 204, 0, 0.8);
      animation: glow 2s infinite alternate;
    }

    @keyframes glow {
      from { text-shadow: 0 0 8px #fff, 0 0 15px #ffcc00; }
      to { text-shadow: 0 0 15px #fff, 0 0 25px #ffcc00, 0 0 35px #ffcc00; }
    }

    @media (max-width: 500px) {
      .btn { font-size: 0.9rem; padding: 12px 0; }
      .display { font-size: 1.5rem; }
    }
  </style>
</head>
<body>
  <!-- Calculadora -->
  <div class="calculator">
    <div class="controls">
      <button class="theme-toggle" id="themeBtn">🌙 Modo Oscuro</button>
    </div>
    <div class="display" id="display">0</div>
    <div class="buttons" id="buttons"></div>
    <div class="history-toggle" id="toggleHistory">📜 Ver historial</div>
    <div id="historyList"></div>
  </div>

  <!-- Créditos en cuadro -->
  <div class="credits-box">
    <div class="credits">By AnthZz Berrocal BerMatMods</div>
  </div>

  <script>
    let currentInput = '0';
    let previousInput = '';
    let operation = null;
    let resetScreen = false;
    let history = [];
    let darkMode = true;

    const display = document.getElementById('display');
    const buttonsContainer = document.getElementById('buttons');
    const themeBtn = document.getElementById('themeBtn');
    const toggleHistory = document.getElementById('toggleHistory');
    const historyList = document.getElementById('historyList');

    // Todos los botones en una sola vista
    const allButtons = [
      'C', '⌫', '±', '%', '÷',
      'sin', 'cos', 'tan', 'log', 'ln',
      '7', '8', '9', '×', 'x²',
      '4', '5', '6', '−', 'x³',
      '1', '2', '3', '+', '√',
      '0', '.', '(', ')', '=',
      'π', 'e', '!', '10ˣ', 'eˣ',
      '1/x', 'xʸ', 'abs', 'rad', 'deg'
    ];

    const sciFunc = {
      'sin': x => Math.sin(x * Math.PI / 180),
      'cos': x => Math.cos(x * Math.PI / 180),
      'tan': x => Math.tan(x * Math.PI / 180),
      'log': x => x > 0 ? Math.log10(x) : new Error('log(x) indefinido'),
      'ln': x => x > 0 ? Math.log(x) : new Error('ln(x) indefinido'),
      '√': x => x >= 0 ? Math.sqrt(x) : new Error('√ negativa'),
      'x²': x => x * x,
      'x³': x => x * x * x,
      '!': x => {
        x = Math.floor(x);
        if (x < 0) return new Error('Factorial inválido');
        let r = 1;
        for (let i = 2; i <= x; i++) r *= i;
        return r;
      },
      '10ˣ': x => Math.pow(10, x),
      'eˣ': x => Math.exp(x),
      '1/x': x => x !== 0 ? 1 / x : new Error('1/0 indefinido'),
      'abs': x => Math.abs(x),
      'π': () => Math.PI,
      'e': () => Math.E
    };

    function toRadians(deg) { return deg * Math.PI / 180; }
    function toDegrees(rad) { return rad * 180 / Math.PI; }

    // Renderizar todos los botones
    function renderButtons() {
      buttonsContainer.innerHTML = '';
      allButtons.forEach(label => {
        const btn = document.createElement('button');
        btn.textContent = label;
        btn.className = 'btn';
        if (['sin','cos','tan','log','ln','x²','x³','√','!','10ˣ','eˣ','1/x','xʸ','abs','π','e','rad','deg'].includes(label)) {
          btn.classList.add('sci');
        }
        if (['C', '⌫'].includes(label)) btn.classList.add('special');
        if (label === '=') btn.classList.add('equals');

        btn.addEventListener('click', () => handleInput(label));
        buttonsContainer.appendChild(btn);
      });
    }

    function handleInput(value) {
      if (value === 'C') {
        resetAll();
      } else if (value === '⌫') {
        currentInput = currentInput.length > 1 ? currentInput.slice(0, -1) : '0';
      } else if (value === '±') {
        currentInput = currentInput.startsWith('-') ? currentInput.slice(1) : '-' + currentInput;
      } else if (value === '%') {
        currentInput = String(parseFloat(currentInput) / 100);
        resetScreen = true;
      } else if (['+', '−', '×', '÷', 'xʸ'].includes(value)) {
        if (operation !== null) compute();
        previousInput = currentInput;
        operation = value;
        resetScreen = true;
      } else if (value === '=') {
        compute();
      } else if (value === 'π' || value === 'e') {
        currentInput = String(sciFunc[value]());
        resetScreen = true;
      } else if (value === 'rad') {
        currentInput = String(toRadians(parseFloat(currentInput)));
        resetScreen = true;
      } else if (value === 'deg') {
        currentInput = String(toDegrees(parseFloat(currentInput)));
        resetScreen = true;
      } else if (sciFunc[value]) {
        const num = parseFloat(currentInput);
        if (isNaN(num)) {
          showError('Entrada inválida');
          return;
        }
        const result = sciFunc[value](num);
        if (result instanceof Error) {
          showError(result.message);
        } else {
          addToHistory(`${value}(${num}) = ${formatResult(result)}`);
          currentInput = String(result);
          resetScreen = true;
        }
      } else {
        if (resetScreen) {
          currentInput = value;
          resetScreen = false;
        } else {
          currentInput = currentInput === '0' && value !== '.' ? value : currentInput + value;
        }
      }
      updateDisplay();
    }

    function compute() {
      if (operation === null) return;
      const prev = parseFloat(previousInput);
      const curr = parseFloat(currentInput);
      if (isNaN(prev) || isNaN(curr)) {
        showError('Entrada inválida');
        return;
      }

      let result;
      try {
        switch (operation) {
          case '+': result = prev + curr; break;
          case '−': result = prev - curr; break;
          case '×': result = prev * curr; break;
          case '÷':
            if (curr === 0) throw new Error('División por cero');
            result = prev / curr;
            break;
          case 'xʸ': result = Math.pow(prev, curr); break;
          default: result = NaN;
        }
      } catch (e) {
        showError(e.message);
        return;
      }

      if (isNaN(result)) {
        showError('Operación inválida');
        return;
      }

      addToHistory(`${previousInput} ${operation} ${currentInput} = ${formatResult(result)}`);
      currentInput = String(result);
      operation = null;
      previousInput = '';
      resetScreen = true;
      updateDisplay();
    }

    function formatResult(num) {
      if (Math.abs(num) > 1e10 || (Math.abs(num) < 1e-6 && num !== 0)) {
        return num.toExponential(6);
      }
      return parseFloat(num.toFixed(10)).toString();
    }

    function showError(msg) {
      display.textContent = 'Error';
      display.style.color = 'var(--error)';
      setTimeout(() => {
        resetAll();
        updateDisplay();
      }, 1800);
    }

    function resetAll() {
      currentInput = '0';
      previousInput = '';
      operation = null;
      resetScreen = false;
    }

    function updateDisplay() {
      let text = currentInput;
      if (operation !== null && !resetScreen) {
        text = `${previousInput} ${operation} ${currentInput}`;
      }
      display.textContent = text;
      display.style.color = 'var(--screen-text)';
    }

    function addToHistory(entry) {
      history.unshift(entry);
      if (history.length > 12) history.pop();
      if (historyList.style.display === 'block') renderHistory();
    }

    function renderHistory() {
      historyList.innerHTML = '';
      history.forEach(entry => {
        const div = document.createElement('div');
        div.textContent = entry;
        div.style.padding = '4px 0';
        historyList.appendChild(div);
      });
    }

    // Eventos
    themeBtn.addEventListener('click', () => {
      darkMode = !darkMode;
      document.body.classList.toggle('light-mode', !darkMode);
      themeBtn.textContent = darkMode ? '🌙 Modo Oscuro' : '☀️ Modo Claro';
    });

    toggleHistory.addEventListener('click', () => {
      const show = historyList.style.display !== 'block';
      historyList.style.display = show ? 'block' : 'none';
      toggleHistory.textContent = show ? '⬆️ Ocultar historial' : '📜 Ver historial';
      if (show) renderHistory();
    });

    document.addEventListener('keydown', e => {
      const keyMap = {
        'Enter': '=', 'Escape': 'C', 'Backspace': '⌫',
        '+': '+', '-': '−', '*': '×', '/': '÷', '%': '%',
        '(': '(', ')': ')', '.': '.'
      };
      if (keyMap[e.key]) handleInput(keyMap[e.key]);
      else if (/^\d$/.test(e.key)) handleInput(e.key);
    });

    // Iniciar
    renderButtons();
    updateDisplay();
  </script>
</body>
</html>
