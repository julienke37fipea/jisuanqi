<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        .calculator {
            background: #fff;
            border-radius: 20px;
            box-shadow: 0 10px 50px rgba(0, 0, 0, 0.3);
            overflow: hidden;
            width: 320px;
        }

        .display {
            background: #2d3436;
            color: #00ff88;
            font-size: 2.5em;
            padding: 20px;
            text-align: right;
            min-height: 100px;
            display: flex;
            align-items: flex-end;
            justify-content: flex-end;
            word-wrap: break-word;
            word-break: break-all;
            font-weight: bold;
            font-family: 'Courier New', monospace;
        }

        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            padding: 20px;
            gap: 10px;
        }

        button {
            padding: 20px;
            font-size: 1.5em;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.2s ease;
        }

        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }

        button:active {
            transform: translateY(0);
        }

        .number {
            background: #f0f0f0;
            color: #2d3436;
        }

        .number:hover {
            background: #e0e0e0;
        }

        .operator {
            background: #667eea;
            color: #fff;
        }

        .operator:hover {
            background: #5568d3;
        }

        .equals {
            grid-column: span 2;
            background: #00ff88;
            color: #2d3436;
        }

        .equals:hover {
            background: #00dd6f;
        }

        .clear {
            grid-column: span 2;
            background: #ff6b6b;
            color: #fff;
        }

        .clear:hover {
            background: #ff5252;
        }

        .delete {
            background: #ffa500;
            color: #fff;
        }

        .delete:hover {
            background: #ff9500;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="display" id="display">0</div>
        <div class="buttons">
            <button class="clear" onclick="clearDisplay()">AC</button>
            <button class="delete" onclick="deleteLast()">DEL</button>
            <button class="operator" onclick="appendOperator('/')">/</button>
            <button class="operator" onclick="appendOperator('*')">×</button>

            <button class="number" onclick="appendNumber('7')">7</button>
            <button class="number" onclick="appendNumber('8')">8</button>
            <button class="number" onclick="appendNumber('9')">9</button>
            <button class="operator" onclick="appendOperator('-')">−</button>

            <button class="number" onclick="appendNumber('4')">4</button>
            <button class="number" onclick="appendNumber('5')">5</button>
            <button class="number" onclick="appendNumber('6')">6</button>
            <button class="operator" onclick="appendOperator('+')">+</button>

            <button class="number" onclick="appendNumber('1')">1</button>
            <button class="number" onclick="appendNumber('2')">2</button>
            <button class="number" onclick="appendNumber('3')">3</button>
            <button class="operator" onclick="appendOperator('.')">,</button>

            <button class="number" onclick="appendNumber('0')">0</button>
            <button class="equals" onclick="calculate()">=</button>
        </div>
    </div>

    <script>
        let display = document.getElementById('display');
        let currentValue = '0';
        let previousValue = '';
        let operation = null;
        let shouldResetDisplay = false;

        function updateDisplay() {
            display.textContent = currentValue;
        }

        function appendNumber(num) {
            if (shouldResetDisplay) {
                currentValue = num;
                shouldResetDisplay = false;
            } else {
                currentValue = currentValue === '0' ? num : currentValue + num;
            }
            updateDisplay();
        }

        function appendOperator(op) {
            if (operation !== null && !shouldResetDisplay) {
                calculate();
            }
            previousValue = currentValue;
            operation = op;
            shouldResetDisplay = true;
            updateDisplay();
        }

        function calculate() {
            if (operation === null || shouldResetDisplay) {
                return;
            }

            let result;
            const prev = parseFloat(previousValue);
            const current = parseFloat(currentValue);

            switch (operation) {
                case '+':
                    result = prev + current;
                    break;
                case '-':
                    result = prev - current;
                    break;
                case '*':
                    result = prev * current;
                    break;
                case '/':
                    result = current === 0 ? 'Error' : prev / current;
                    break;
                case '.':
                    result = current;
                    break;
                default:
                    return;
            }

            currentValue = result.toString();
            operation = null;
            shouldResetDisplay = true;
            updateDisplay();
        }

        function clearDisplay() {
            currentValue = '0';
            previousValue = '';
            operation = null;
            shouldResetDisplay = false;
            updateDisplay();
        }

        function deleteLast() {
            if (currentValue.length > 1) {
                currentValue = currentValue.slice(0, -1);
            } else {
                currentValue = '0';
            }
            updateDisplay();
        }
    </script>
</body>
</html>
