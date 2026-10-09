# CALCULADORA-DE-TEMPO<!DOCTYPE html>
<html lang="pt-BR" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora de Tempo (Time-Entry)</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#f0f9ff',
                            500: '#0284c7',
                            600: '#0369a1',
                            700: '#075985',
                        }
                    }
                }
            }
        }
    </script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600;700&family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap');
        
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
        }
        .font-mono {
            font-family: 'JetBrains Mono', monospace;
        }
        .key-active {
            transform: scale(0.95);
            filter: brightness(1.2);
            box-shadow: inset 0 2px 4px rgba(0,0,0,0.2) !important;
        }
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: transparent;
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(156, 163, 175, 0.3);
            border-radius: 9999px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: rgba(156, 163, 175, 0.5);
        }
    </style>
</head>
<body class="bg-slate-100 dark:bg-slate-950 text-slate-800 dark:text-slate-100 min-h-screen flex items-center justify-center p-3 md:p-6 transition-colors duration-300">

    <div class="max-w-4xl w-full grid grid-cols-1 lg:grid-cols-12 gap-6 bg-white dark:bg-slate-900 rounded-3xl shadow-2xl p-4 sm:p-6 border border-slate-200 dark:border-slate-800">
        
        <!-- Calculadora (Lado Esquerdo/Principal) -->
        <div class="lg:col-span-7 flex flex-col justify-between space-y-4">
            
            <!-- Header -->
            <div class="flex items-center justify-between">
                <div class="flex items-center gap-2">
                    <div class="p-2.5 bg-brand-500/10 text-brand-600 dark:text-brand-500 rounded-2xl">
                        <i class="fa-solid fa-clock text-xl"></i>
                    </div>
                    <div>
                        <h1 class="text-xl font-bold tracking-tight">TimeCalc Pro</h1>
                        <p class="text-xs text-slate-500 dark:text-slate-400">Entrada Contínua (SS → MM:SS → HH:MM:SS)</p>
                    </div>
                </div>
                <div class="flex items-center gap-2">
                    <button id="themeToggleBtn" class="p-2.5 rounded-xl text-slate-500 hover:bg-slate-100 dark:hover:bg-slate-800 dark:text-slate-400 transition" title="Alternar Tema">
                        <i class="fa-solid fa-moon dark:hidden text-lg"></i>
                        <i class="fa-solid fa-sun hidden dark:block text-lg"></i>
                    </button>
                    <button id="helpBtn" class="p-2.5 rounded-xl text-slate-500 hover:bg-slate-100 dark:hover:bg-slate-800 dark:text-slate-400 transition" title="Atalhos do Teclado">
                        <i class="fa-solid fa-keyboard text-lg"></i>
                    </button>
                </div>
            </div>

            <!-- Display da Calculadora -->
            <div class="bg-slate-50 dark:bg-slate-950/80 p-4 rounded-2xl border border-slate-200 dark:border-slate-800 shadow-inner flex flex-col justify-between min-h-[120px] transition-all">
                <div class="flex justify-between items-start text-xs font-mono text-slate-400 dark:text-slate-500 min-h-[20px] overflow-x-auto">
                    <span id="expressionDisplay" class="truncate pr-2"></span>
                    <span id="unitIndicator" class="bg-slate-200 dark:bg-slate-800 px-2 py-0.5 rounded text-[10px] font-sans font-medium uppercase tracking-wider text-slate-600 dark:text-slate-400">HH:MM:SS</span>
                </div>
                
                <div class="text-right overflow-x-auto scrollbar-none py-1">
                    <span id="mainDisplay" class="font-mono text-3xl sm:text-4xl font-bold tracking-tight text-slate-900 dark:text-white transition-all">00:00:00</span>
                </div>

                <div id="errorMessage" class="text-xs text-red-500 dark:text-red-400 font-medium hidden flex items-center justify-end gap-1">
                    <i class="fa-solid fa-triangle-exclamation"></i>
                    <span id="errorText">Expressão inválida</span>
                </div>
            </div>

            <!-- Botões do Teclado Numérico -->
            <div class="grid grid-cols-4 gap-2 text-sm sm:text-base select-none">
                <!-- Linha 1 -->
                <button data-key="Escape" class="calc-btn p-3 sm:p-4 rounded-xl font-semibold bg-red-100 dark:bg-red-950/40 text-red-600 dark:text-red-400 hover:bg-red-200 dark:hover:bg-red-900/60 transition active:scale-95">AC</button>
                <button data-key="Backspace" class="calc-btn p-3 sm:p-4 rounded-xl font-semibold bg-slate-200 dark:bg-slate-800 text-slate-700 dark:text-slate-300 hover:bg-slate-300 dark:hover:bg-slate-700 transition active:scale-95">
                    <i class="fa-solid fa-backspace"></i>
                </button>
                <button data-key="/" class="calc-btn p-3 sm:p-4 rounded-xl font-semibold bg-brand-50 dark:bg-brand-950/50 text-brand-600 dark:text-brand-400 hover:bg-brand-100 dark:hover:bg-brand-900/50 transition active:scale-95">÷</button>
                <button data-key="*" class="calc-btn p-3 sm:p-4 rounded-xl font-semibold bg-brand-50 dark:bg-brand-950/50 text-brand-600 dark:text-brand-400 hover:bg-brand-100 dark:hover:bg-brand-900/50 transition active:scale-95">×</button>

                <!-- Linha 2 -->
                <button data-key="7" class="calc-btn p-3 sm:p-4 rounded-xl font-semibold bg-slate-100 dark:bg-slate-800/60 text-slate-800 dark:text-slate-100 hover:bg-slate-200 dark:hover:bg-slate-700 transition active:scale-95">7</button>
                <button data-key="8" class="calc-btn p-3 sm:p-4 rounded-xl font-semibold bg-slate-100 dark:bg-slate-800/60 text-slate-800 dark:text-slate-100 hover:bg-slate-200 dark:hover:bg-slate-700 transition active:scale-95">8</button>
                <button data-key="9" class="calc-btn p-3 sm:p-4 rounded-xl font-semibold bg-slate-100 dark:bg-slate-800/60 text-slate-800 dark:text-slate-100 hover:bg-slate-200 dark:hover:bg-slate-700 transition active:scale-95">9</button>
                <button data-key="-" class="calc-btn p-3 sm:p-4 rounded-xl font-semibold bg-brand-50 dark:bg-brand-950/50 text-brand-600 dark:text-brand-400 hover:bg-brand-100 dark:hover:bg-brand-900/50 transition active:scale-95">−</button>

                <!-- Linha 3 -->
                <button data-key="4" class="calc-btn p-3 sm:p-4 rounded-xl font-semibold bg-slate-100 dark:bg-slate-800/60 text-slate-800 dark:text-slate-100 hover:bg-slate-200 dark:hover:bg-slate-700 transition active:scale-95">4</button>
                <button data-key="5" class="calc-btn p-3 sm:p-4 rounded-xl font-semibold bg-slate-100 dark:bg-slate-800/60 text-slate-800 dark:text-slate-100 hover:bg-slate-200 dark:hover:bg-slate-700 transition active:scale-95">5</button>
                <button data-key="6" class="calc-btn p-3 sm:p-4 rounded-xl font-semibold bg-slate-100 dark:bg-slate-800/60 text-slate-800 dark:text-slate-100 hover:bg-slate-200 dark:hover:bg-slate-700 transition active:scale-95">6</button>
                <button data-key="+" class="calc-btn p-3 sm:p-4 rounded-xl font-semibold bg-brand-50 dark:bg-brand-950/50 text-brand-600 dark:text-brand-400 hover:bg-brand-100 dark:hover:bg-brand-900/50 transition active:scale-95">+</button>

                <!-- Linha 4 -->
                <button data-key="1" class="calc-btn p-3 sm:p-4 rounded-xl font-semibold bg-slate-100 dark:bg-slate-800/60 text-slate-800 dark:text-slate-100 hover:bg-slate-200 dark:hover:bg-slate-700 transition active:scale-95">1</button>
                <button data-key="2" class="calc-btn p-3 sm:p-4 rounded-xl font-semibold bg-slate-100 dark:bg-slate-800/60 text-slate-800 dark:text-slate-100 hover:bg-slate-200 dark:hover:bg-slate-700 transition active:scale-95">2</button>
                <button data-key="3" class="calc-btn p-3 sm:p-4 rounded-xl font-semibold bg-slate-100 dark:bg-slate-800/60 text-slate-800 dark:text-slate-100 hover:bg-slate-200 dark:hover:bg-slate-700 transition active:scale-95">3</button>
                <button data-key="Enter" class="calc-btn row-span-2 p-3 sm:p-4 rounded-xl font-semibold bg-brand-500 text-white hover:bg-brand-600 shadow-lg shadow-brand-500/30 transition active:scale-95 flex items-center justify-center text-xl">=</button>

                <!-- Linha 5 -->
                <button data-key="0" class="calc-btn col-span-2 p-3 sm:p-4 rounded-xl font-semibold bg-slate-100 dark:bg-slate-800/60 text-slate-800 dark:text-slate-100 hover:bg-slate-200 dark:hover:bg-slate-700 transition active:scale-95">0</button>
                <button data-key="00" class="calc-btn p-3 sm:p-4 rounded-xl font-semibold bg-slate-100 dark:bg-slate-800/60 text-slate-800 dark:text-slate-100 hover:bg-slate-200 dark:hover:bg-slate-700 transition active:scale-95">00</button>
            </div>
        </div>

        <!-- Painel de Histórico (Lado Direito) -->
        <div class="lg:col-span-5 border-t lg:border-t-0 lg:border-l border-slate-200 dark:border-slate-800 pt-6 lg:pt-0 lg:pl-6 flex flex-col justify-between">
            <div>
                <div class="flex items-center justify-between mb-4">
                    <h2 class="text-sm font-bold uppercase tracking-wider text-slate-500 dark:text-slate-400 flex items-center gap-2">
                        <i class="fa-solid fa-history"></i> Histórico
                    </h2>
                    <button id="clearHistoryBtn" class="text-xs text-red-500 hover:text-red-600 hover:underline transition hidden">
                        Limpar
                    </button>
                </div>

                <div id="historyList" class="space-y-2 max-h-[300px] lg:max-h-[380px] overflow-y-auto pr-1">
                    <div id="emptyHistory" class="text-center py-10 text-slate-400 dark:text-slate-600 text-sm">
                        <i class="fa-regular fa-clock text-3xl mb-2 block"></i>
                        Nenhum cálculo recente
                    </div>
                </div>
            </div>

            <!-- Rodapé informativo -->
            <div class="mt-4 pt-4 border-t border-slate-100 dark:border-slate-800/60 text-xs text-slate-400 dark:text-slate-500 flex justify-between items-center">
                <span>Entrada direta: ex. '130' → 00:01:30</span>
                <span class="font-mono bg-slate-100 dark:bg-slate-800 px-2 py-1 rounded">0-9</span>
            </div>
        </div>
    </div>

    <!-- Modal de Ajuda -->
    <div id="helpModal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white dark:bg-slate-900 rounded-2xl max-w-md w-full p-6 shadow-2xl border border-slate-200 dark:border-slate-800">
            <div class="flex items-center justify-between mb-4">
                <h3 class="font-bold text-lg text-slate-900 dark:text-white flex items-center gap-2">
                    <i class="fa-solid fa-keyboard text-brand-500"></i> Como Funciona a Entrada
                </h3>
                <button id="closeHelpBtn" class="text-slate-400 hover:text-slate-600 dark:hover:text-slate-200">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>
            <div class="space-y-3 text-sm">
                <div class="p-3 bg-slate-50 dark:bg-slate-800/60 rounded-xl space-y-1">
                    <p class="font-semibold text-brand-600 dark:text-brand-400">Entrada estilo caixa de somar:</p>
                    <p class="text-xs text-slate-600 dark:text-slate-300">• <strong>5</strong> → 00:00:05 (5 seg)</p>
                    <p class="text-xs text-slate-600 dark:text-slate-300">• <strong>45</strong> → 00:00:45 (45 seg)</p>
                    <p class="text-xs text-slate-600 dark:text-slate-300">• <strong>130</strong> → 00:01:30 (1 min 30 seg)</p>
                    <p class="text-xs text-slate-600 dark:text-slate-300">• <strong>1245</strong> → 00:12:45 (12 min 45 seg)</p>
                    <p class="text-xs text-slate-600 dark:text-slate-300">• <strong>11245</strong> → 01:12:45 (1h 12m 45s)</p>
                    <p class="text-xs text-slate-600 dark:text-slate-300">• <strong>101530</strong> → 10:15:30 (10h 15m 30s)</p>
                </div>
                <div class="flex justify-between items-center py-1 border-b border-slate-100 dark:border-slate-800">
                    <span class="text-slate-600 dark:text-slate-400">Teclado Numérico</span>
                    <kbd class="px-2 py-1 bg-slate-100 dark:bg-slate-800 rounded text-xs font-mono">0 - 9</kbd>
                </div>
                <div class="flex justify-between items-center py-1 border-b border-slate-100 dark:border-slate-800">
                    <span class="text-slate-600 dark:text-slate-400">Operações</span>
                    <kbd class="px-2 py-1 bg-slate-100 dark:bg-slate-800 rounded text-xs font-mono">+  -  *  /</kbd>
                </div>
                <div class="flex justify-between items-center py-1 border-b border-slate-100 dark:border-slate-800">
                    <span class="text-slate-600 dark:text-slate-400">Calcular Resultado</span>
                    <kbd class="px-2 py-1 bg-slate-100 dark:bg-slate-800 rounded text-xs font-mono">Enter / =</kbd>
                </div>
            </div>
            <button id="gotItBtn" class="mt-6 w-full py-2.5 bg-brand-500 hover:bg-brand-600 text-white font-medium rounded-xl transition">
                Entendi
            </button>
        </div>
    </div>

    <script>
        // --- ESTADO DA CALCULADORA ---
        let rawDigitBuffer = ""; // Armazena apenas os dígitos brutos inseridos (ex: "130")
        let pendingExpression = "";
        let history = [];
        let isResultState = false; // Indica se o valor no visor é um resultado recém-calculado

        // --- ELEMENTOS DO DOM ---
        const mainDisplay = document.getElementById('mainDisplay');
        const expressionDisplay = document.getElementById('expressionDisplay');
        const errorMessage = document.getElementById('errorMessage');
        const errorText = document.getElementById('errorText');
        const historyList = document.getElementById('historyList');
        const emptyHistory = document.getElementById('emptyHistory');
        const clearHistoryBtn = document.getElementById('clearHistoryBtn');
        const themeToggleBtn = document.getElementById('themeToggleBtn');
        const helpBtn = document.getElementById('helpBtn');
        const helpModal = document.getElementById('helpModal');
        const closeHelpBtn = document.getElementById('closeHelpBtn');
        const gotItBtn = document.getElementById('gotItBtn');

        /**
         * Formata uma string de dígitos numéricos da direita para a esquerda no padrão HH:MM:SS
         * Ex: "5" -> "00:00:05"
         * Ex: "130" -> "00:01:30"
         * Ex: "11245" -> "01:12:45"
         */
        function formatDigitsToTimeDisplay(digits) {
            if (!digits || digits.length === 0) return "00:00:00";
            
            // Limita a no máximo 6 dígitos (HHMMSS)
            const padded = digits.padStart(6, '0').slice(-6);
            
            const hh = padded.slice(0, 2);
            const mm = padded.slice(2, 4);
            const ss = padded.slice(4, 6);
            
            return `${hh}:${mm}:${ss}`;
        }

        /**
         * Converte uma string formatada HH:MM:SS ou MM:SS para segundos totais acumulados.
         * Suporta normalização de minutos/segundos acima de 60 (ex: 90s = 1m 30s)
         */
        function parseFormattedTimeToSeconds(timeStr) {
            if (typeof timeStr === 'number') return timeStr;
            if (!timeStr || timeStr.trim() === "") return 0;

            const parts = timeStr.split(':').map(p => parseFloat(p) || 0);

            if (parts.length === 1) return parts[0];
            if (parts.length === 2) return (parts[0] * 60) + parts[1];
            if (parts.length === 3) return (parts[0] * 3600) + (parts[1] * 60) + parts[2];

            return 0;
        }

        /**
         * Converte segundos totais em representação HH:MM:SS normalizada
         */
        function formatSecondsToTime(totalSeconds) {
            const isNegative = totalSeconds < 0;
            let absSeconds = Math.abs(Math.round(totalSeconds));

            const hours = Math.floor(absSeconds / 3600);
            absSeconds %= 3600;
            const minutes = Math.floor(absSeconds / 60);
            const seconds = absSeconds % 60;

            const pad = num => String(num).padStart(2, '0');
            const formatted = `${pad(hours)}:${pad(minutes)}:${pad(seconds)}`;
            
            return isNegative ? `-${formatted}` : formatted;
        }

        /**
         * Converte uma string no formato HH:MM:SS para dígitos brutos desconsiderando zeros à esquerda
         */
        function timeToRawDigits(timeStr) {
            const clean = timeStr.replace(/[^0-9]/g, '');
            const trimmed = clean.replace(/^0+/, '');
            return trimmed;
        }

        function updateDisplay() {
            if (isResultState && rawDigitBuffer !== "") {
                mainDisplay.textContent = rawDigitBuffer; // Exibe o tempo resultado retornado
            } else {
                mainDisplay.textContent = formatDigitsToTimeDisplay(rawDigitBuffer);
            }
            expressionDisplay.textContent = pendingExpression;
            hideError();
        }

        function showError(msg) {
            errorText.textContent = msg;
            errorMessage.classList.remove('hidden');
        }

        function hideError() {
            errorMessage.classList.add('hidden');
        }

        function handleDigit(digits) {
            if (isResultState) {
                // Se o usuário começa a digitar após um resultado, limpa a entrada atual
                rawDigitBuffer = "";
                isResultState = false;
            }

            if (rawDigitBuffer.length >= 6 && digits !== '00') return; // Limite de 6 dígitos (HHMMSS)

            if (digits === '00') {
                if (rawDigitBuffer.length === 0) return;
                rawDigitBuffer = (rawDigitBuffer + '00').slice(0, 6);
            } else {
                rawDigitBuffer += digits;
                if (rawDigitBuffer.length > 6) {
                    rawDigitBuffer = rawDigitBuffer.slice(-6);
                }
            }

            // Remove zeros à esquerda adicionais no buffer bruto para manutenção limpa
            rawDigitBuffer = rawDigitBuffer.replace(/^0+/, '');
            updateDisplay();
        }

        function handleBackspace() {
            if (isResultState) {
                rawDigitBuffer = "";
                isResultState = false;
            } else {
                rawDigitBuffer = rawDigitBuffer.slice(0, -1);
            }
            updateDisplay();
        }

        function handleClear() {
            rawDigitBuffer = "";
            pendingExpression = "";
            isResultState = false;
            updateDisplay();
        }

        function getCurrentFormattedValue() {
            if (isResultState) {
                return rawDigitBuffer || "00:00:00";
            }
            return formatDigitsToTimeDisplay(rawDigitBuffer);
        }

        function handleOperator(op) {
            const currentValue = getCurrentFormattedValue();

            if (rawDigitBuffer === "" && pendingExpression !== "" && !isResultState) {
                // Troca operador atual se digitado em sequência
                pendingExpression = pendingExpression.trim().slice(0, -1) + " " + op + " ";
                updateDisplay();
                return;
            }

            pendingExpression += `${currentValue} ${op} `;
            rawDigitBuffer = "";
            isResultState = false;
            updateDisplay();
        }

        function calculateResult() {
            if (!pendingExpression && rawDigitBuffer === "") return;

            const currentValue = getCurrentFormattedValue();
            const fullExpr = pendingExpression + currentValue;
            
            if (!fullExpr) return;

            try {
                const tokens = fullExpr.trim().split(/\s+/);
                if (tokens.length < 3) return;

                let isTimeResult = true;
                let firstValRaw = tokens[0];
                let currentVal = parseFormattedTimeToSeconds(firstValRaw);

                for (let i = 1; i < tokens.length; i += 2) {
                    const operator = tokens[i];
                    const nextValRaw = tokens[i + 1];

                    if (!nextValRaw) throw new Error("Expressão incompleta");

                    const isNextTime = nextValRaw.includes(':');
                    const nextVal = isNextTime ? parseFormattedTimeToSeconds(nextValRaw) : parseFloat(nextValRaw);

                    if (isNaN(nextVal)) throw new Error("Valor numérico inválido");

                    if (operator === '+') {
                        currentVal += nextVal;
                    } else if (operator === '-') {
                        currentVal -= nextVal;
                    } else if (operator === '*') {
                        if (isNextTime) throw new Error("Não é possível multiplicar tempo por tempo");
                        currentVal *= nextVal;
                    } else if (operator === '/') {
                        if (nextVal === 0) throw new Error("Divisão por zero");
                        if (isNextTime) {
                            currentVal /= nextVal;
                            isTimeResult = false;
                        } else {
                            currentVal /= nextVal;
                        }
                    }
                }

                let finalFormattedResult = isTimeResult ? formatSecondsToTime(currentVal) : currentVal.toFixed(2);

                addToHistory(fullExpr, finalFormattedResult);

                // Armazena resultado e configura o estado da calculadora
                rawDigitBuffer = finalFormattedResult;
                isResultState = true;
                pendingExpression = "";
                updateDisplay();

            } catch (err) {
                showError(err.message || "Erro no cálculo");
            }
        }

        function addToHistory(expr, result) {
            history.unshift({ expr, result });
            if (history.length > 20) history.pop();
            renderHistory();
        }

        function renderHistory() {
            if (history.length === 0) {
                emptyHistory.classList.remove('hidden');
                clearHistoryBtn.classList.add('hidden');
                historyList.innerHTML = '';
                historyList.appendChild(emptyHistory);
                return;
            }

            emptyHistory.classList.add('hidden');
            clearHistoryBtn.classList.remove('hidden');
            historyList.innerHTML = '';

            history.forEach((item) => {
                const el = document.createElement('div');
                el.className = 'p-2.5 rounded-xl bg-slate-50 dark:bg-slate-800/40 border border-slate-100 dark:border-slate-800 flex justify-between items-center group hover:border-brand-500/50 transition cursor-pointer';
                el.innerHTML = `
                    <div class="overflow-hidden pr-2">
                        <div class="text-xs text-slate-400 font-mono truncate">${item.expr}</div>
                        <div class="text-sm font-bold font-mono text-slate-800 dark:text-slate-200">${item.result}</div>
                    </div>
                    <button class="opacity-0 group-hover:opacity-100 p-1.5 text-xs text-brand-600 dark:text-brand-400 hover:bg-brand-50 dark:hover:bg-slate-700 rounded-lg transition" title="Usar resultado">
                        <i class="fa-solid fa-arrow-left"></i>
                    </button>
                `;
                el.addEventListener('click', () => {
                    rawDigitBuffer = item.result;
                    isResultState = true;
                    updateDisplay();
                });
                historyList.appendChild(el);
            });
        }

        clearHistoryBtn.addEventListener('click', () => {
            history = [];
            renderHistory();
        });

        // --- FEEDBACK VISUAL DAS TECLAS ---
        function triggerKeyFeedback(key) {
            const btn = document.querySelector(`button[data-key="${key}"]`);
            if (btn) {
                btn.classList.add('key-active');
                setTimeout(() => btn.classList.remove('key-active'), 120);
            }
        }

        // --- EVENTOS DO TECLADO DO COMPUTADOR ---
        window.addEventListener('keydown', (e) => {
            if (e.target.tagName === 'INPUT' || e.target.tagName === 'TEXTAREA') return;

            const key = e.key;

            if (/[0-9]/.test(key)) {
                triggerKeyFeedback(key);
                handleDigit(key);
            } else if (['+', '-', '*', '/'].includes(key)) {
                triggerKeyFeedback(key);
                handleOperator(key);
            } else if (key === 'Enter' || key === '=') {
                e.preventDefault();
                triggerKeyFeedback('Enter');
                calculateResult();
            } else if (key === 'Backspace') {
                triggerKeyFeedback('Backspace');
                handleBackspace();
            } else if (key === 'Escape' || key === 'Delete') {
                triggerKeyFeedback('Escape');
                handleClear();
            }
        });

        // --- EVENTOS DE CLIQUE NOS BOTÕES DA CALCULADORA ---
        document.querySelectorAll('.calc-btn').forEach(btn => {
            btn.addEventListener('click', () => {
                const key = btn.getAttribute('data-key');
                if (/[0-9]/.test(key) || key === '00') {
                    handleDigit(key);
                } else if (['+', '-', '*', '/'].includes(key)) {
                    handleOperator(key);
                } else if (key === 'Enter') {
                    calculateResult();
                } else if (key === 'Backspace') {
                    handleBackspace();
                } else if (key === 'Escape') {
                    handleClear();
                }
            });
        });

        // --- CONTROLE DE MODAL E TEMA ---
        themeToggleBtn.addEventListener('click', () => {
            document.documentElement.classList.toggle('dark');
        });

        helpBtn.addEventListener('click', () => helpModal.classList.remove('hidden'));
        closeHelpBtn.addEventListener('click', () => helpModal.classList.add('hidden'));
        gotItBtn.addEventListener('click', () => helpModal.classList.add('hidden'));
        helpModal.addEventListener('click', (e) => {
            if (e.target === helpModal) helpModal.classList.add('hidden');
        });

        // Inicialização
        updateDisplay();
    </script>
</body>
</html>
