<!DOCTYPE html>
<html lang="pt-BR" class="dark">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Calculadora Tech | ADS Project</title>
  <!-- Tailwind CSS para estilização moderna -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Math.js para cálculos matemáticos precisos -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/11.8.0/math.js"></script>
  <!-- Lucide Icons para ícones vetorizados -->
  <script src="https://unpkg.com/lucide@latest"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          colors: {
            brand: {
              500: '#6366f1',
              600: '#4f46e5',
            }
          }
        }
      }
    }
  </script>
  <style>
    .glass {
      background: rgba(17, 24, 39, 0.75);
      backdrop-filter: blur(16px);
      border: 1px solid rgba(255, 255, 255, 0.1);
    }
  </style>
</head>
<body class="bg-gradient-to-br from-slate-950 via-slate-900 to-indigo-950 text-slate-100 min-h-screen flex items-center justify-center p-4 font-sans">

  <div class="glass w-full max-full max-w-md rounded-3xl p-6 shadow-2xl relative overflow-hidden">
    <!-- Efeito de brilho de fundo -->
    <div class="absolute -top-10 -left-10 w-32 h-32 bg-indigo-500/20 rounded-full blur-3xl pointer-events-none"></div>

    <!-- Cabeçalho -->
    <div class="flex justify-between items-center mb-4">
      <div class="flex items-center gap-2">
        <i data-lucide="cpu" class="text-indigo-400 w-5 h-5"></i>
        <span class="text-xs font-semibold tracking-wider text-indigo-300 uppercase">TechCalc v2.0</span>
      </div>
      <button id="toggle-history" class="p-2 hover:bg-slate-800/60 rounded-xl transition-colors text-slate-400 hover:text-white" title="Histórico">
        <i data-lucide="history" class="w-5 h-5"></i>
      </button>
    </div>

    <!-- Visor de Cálculo -->
    <div class="bg-slate-900/80 rounded-2xl p-4 mb-5 border border-slate-800 text-right shadow-inner">
      <div id="expression" class="text-slate-400 text-sm h-6 overflow-hidden font-mono tracking-wide"></div>
      <div id="result" class="text-3xl font-bold font-mono tracking-tight text-white truncate mt-1">0</div>
    </div>

    <!-- Painel do Histórico (Oculto por padrão) -->
    <div id="history-panel" class="hidden bg-slate-900/95 rounded-2xl p-4 mb-5 border border-slate-800 max-h-40 overflow-y-auto">
      <div class="flex justify-between items-center mb-2">
        <span class="text-xs font-semibold text-slate-400">HISTÓRICO</span>
        <button id="clear-history" class="text-xs text-rose-400 hover:underline">Limpar</button>
      </div>
      <ul id="history-list" class="space-y-1 text-xs font-mono text-slate-300"></ul>
    </div>

    <!-- Teclado da Calculadora -->
    <div class="grid grid-cols-4 gap-2.5">
      <!-- Linha 1 -->
      <button onclick="clearAll()" class="col-span-2 bg-rose-500/20 text-rose-300 hover:bg-rose-500/30 p-3.5 rounded-xl font-bold transition-all active:scale-95 border border-rose-500/30">AC</button>
      <button onclick="deleteLast()" class="bg-slate-800/80 hover:bg-slate-700 text-slate-300 p-3.5 rounded-xl font-bold transition-all active:scale-95 border border-slate-700/50">DEL</button>
      <button onclick="appendOperator('/')" class="bg-indigo-600/30 hover:bg-indigo-600/40 text-indigo-300 p-3.5 rounded-xl font-bold transition-all active:scale-95 border border-indigo-500/30">÷</button>

      <!-- Linha 2 -->
      <button onclick="appendNumber('7')" class="bg-slate-800/40 hover:bg-slate-700/60 p-3.5 rounded-xl font-semibold transition-all active:scale-95 border border-slate-800">7</button>
      <button onclick="appendNumber('8')" class="bg-slate-800/40 hover:bg-slate-700/60 p-3.5 rounded-xl font-semibold transition-all active:scale-95 border border-slate-800">8</button>
      <button onclick="appendNumber('9')" class="bg-slate-800/40 hover:bg-slate-700/60 p-3.5 rounded-xl font-semibold transition-all active:scale-95 border border-slate-800">9</button>
      <button onclick="appendOperator('*')" class="bg-indigo-600/30 hover:bg-indigo-600/40 text-indigo-300 p-3.5 rounded-xl font-bold transition-all active:scale-95 border border-indigo-500/30">×</button>

      <!-- Linha 3 -->
      <button onclick="appendNumber('4')" class="bg-slate-800/40 hover:bg-slate-700/60 p-3.5 rounded-xl font-semibold transition-all active:scale-95 border border-slate-800">4</button>
      <button onclick="appendNumber('5')" class="bg-slate-800/40 hover:bg-slate-700/60 p-3.5 rounded-xl font-semibold transition-all active:scale-95 border border-slate-800">5</button>
      <button onclick="appendNumber('6')" class="bg-slate-800/40 hover:bg-slate-700/60 p-3.5 rounded-xl font-semibold transition-all active:scale-95 border border-slate-800">6</button>
      <button onclick="appendOperator('-')" class="bg-indigo-600/30 hover:bg-indigo-600/40 text-indigo-300 p-3.5 rounded-xl font-bold transition-all active:scale-95 border border-indigo-500/30">-</button>

      <!-- Linha 4 -->
      <button onclick="appendNumber('1')" class="bg-slate-800/40 hover:bg-slate-700/60 p-3.5 rounded-xl font-semibold transition-all active:scale-95 border border-slate-800">1</button>
      <button onclick="appendNumber('2')" class="bg-slate-800/40 hover:bg-slate-700/60 p-3.5 rounded-xl font-semibold transition-all active:scale-95 border border-slate-800">2</button>
      <button onclick="appendNumber('3')" class="bg-slate-800/40 hover:bg-slate-700/60 p-3.5 rounded-xl font-semibold transition-all active:scale-95 border border-slate-800">3</button>
      <button onclick="appendOperator('+')" class="bg-indigo-600/30 hover:bg-indigo-600/40 text-indigo-300 p-3.5 rounded-xl font-bold transition-all active:scale-95 border border-indigo-500/30">+</button>

      <!-- Linha 5 -->
      <button onclick="appendNumber('0')" class="col-span-2 bg-slate-800/40 hover:bg-slate-700/60 p-3.5 rounded-xl font-semibold transition-all active:scale-95 border border-slate-800">0</button>
      <button onclick="appendDot()" class="bg-slate-800/40 hover:bg-slate-700/60 p-3.5 rounded-xl font-semibold transition-all active:scale-95 border border-slate-800">.</button>
      <button onclick="calculate()" class="bg-gradient-to-r from-indigo-500 to-indigo-600 hover:from-indigo-600 hover:to-indigo-700 text-white p-3.5 rounded-xl font-bold shadow-lg shadow-indigo-500/25 transition-all active:scale-95">=</button>
    </div>
  </div>

  <script>
    lucide.createIcons();

    let currentExpression = '';
    const expressionEl = document.getElementById('expression');
    const resultEl = document.getElementById('result');
    const historyPanel = document.getElementById('history-panel');
    const historyList = document.getElementById('history-list');

    function updateDisplay() {
      expressionEl.textContent = currentExpression;
    }

    function appendNumber(num) {
      currentExpression += num;
      updateDisplay();
    }

    function appendOperator(op) {
      if (currentExpression === '' && op !== '-') return;
      const lastChar = currentExpression.slice(-1);
      if (['+', '-', '*', '/'].includes(lastChar)) {
        currentExpression = currentExpression.slice(0, -1) + op;
      } else {
        currentExpression += op;
      }
      updateDisplay();
    }

    function appendDot() {
      const parts = currentExpression.split(/[\+\-\*\/]/);
      if (!parts[parts.length - 1].includes('.')) {
        currentExpression += '.';
        updateDisplay();
      }
    }

    function clearAll() {
      currentExpression = '';
      resultEl.textContent = '0';
      updateDisplay();
    }

    function deleteLast() {
      currentExpression = currentExpression.slice(0, -1);
      updateDisplay();
    }

    function calculate() {
      try {
        if (!currentExpression) return;
        const res = math.evaluate(currentExpression);
        const formattedRes = Number.isInteger(res) ? res : res.toFixed(4).replace(/\.?0+$/, '');
        
        // Add to history
        addHistory(currentExpression, formattedRes);
        
        resultEl.textContent = formattedRes;
        currentExpression = formattedRes.toString();
      } catch (error) {
        resultEl.textContent = 'Erro';
      }
    }

    function addHistory(expr, res) {
      const li = document.createElement('li');
      li.className = 'flex justify-between py-1 border-b border-slate-800/50';
      li.innerHTML = `<span class="text-slate-400">${expr} =</span> <span class="text-indigo-400 font-bold">${res}</span>`;
      historyList.prepend(li);
    }

    document.getElementById('toggle-history').addEventListener('click', () => {
      historyPanel.classList.toggle('hidden');
    });

    document.getElementById('clear-history').addEventListener('click', () => {
      historyList.innerHTML = '';
    });

    // Atalhos de Teclado
    window.addEventListener('keydown', (e) => {
      if (e.key >= '0' && e.key <= '9') appendNumber(e.key);
      if (['+', '-', '*', '/'].includes(e.key)) appendOperator(e.key);
      if (e.key === '.' || e.key === ',') appendDot();
      if (e.key === 'Enter' || e.key === '=') calculate();
      if (e.key === 'Backspace') deleteLast();
      if (e.key === 'Escape') clearAll();
    });
  </script>
</body>
</html>
