<!DOCTYPE html>
<html lang="tg">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Касса бо Хотира</title>
    <style>
        * { box-sizing: border-box; transition: all 0.3s ease; }
        
        body { 
            margin: 0; 
            padding: 20px; 
            font-family: 'Segoe UI', Roboto, sans-serif; 
            background: linear-gradient(135deg, #0f0c29 0%, #302b63 50%, #24243e 100%); 
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .glass-card {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 35px;
            padding: 30px;
            width: 100%;
            max-width: 400px;
            box-shadow: 0 25px 50px rgba(0,0,0,0.5);
            color: white;
            text-align: center;
        }

        h2 { font-weight: 300; letter-spacing: 2px; margin-bottom: 30px; color: #00d2ff; text-transform: uppercase; }

        .stat-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-bottom: 30px;
        }

        .stat-item {
            background: rgba(255, 255, 255, 0.05);
            padding: 15px;
            border-radius: 20px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .stat-label { font-size: 0.7rem; text-transform: uppercase; opacity: 0.6; letter-spacing: 1px; }
        .stat-value { font-size: 1.8rem; font-weight: bold; display: block; margin: 5px 0; color: #fff; }
        .stat-money { font-size: 1rem; color: #00ff88; font-weight: bold; }

        .control-row {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
        }

        .main-btn {
            flex: 4;
            padding: 22px;
            border: none;
            border-radius: 20px;
            font-size: 1.1rem;
            font-weight: bold;
            cursor: pointer;
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .undo-btn {
            flex: 1;
            padding: 22px;
            border: none;
            border-radius: 20px;
            background: rgba(255, 255, 255, 0.1);
            color: #ff4b2b;
            cursor: pointer;
            font-size: 1.4rem;
            border: 1px solid rgba(255, 75, 43, 0.3);
        }

        .btn-p { background: linear-gradient(45deg, #1e90ff, #1c7ed6); box-shadow: 0 5px 15px rgba(30, 144, 255, 0.3); }
        .btn-s { background: linear-gradient(45deg, #f39c12, #e67e22); box-shadow: 0 5px 15px rgba(243, 156, 18, 0.3); }

        .main-btn:active, .undo-btn:active { transform: scale(0.9); }

        .total-display {
            background: rgba(0, 255, 136, 0.05);
            margin-top: 20px;
            padding: 25px;
            border-radius: 25px;
            border: 1px solid rgba(0, 255, 136, 0.2);
        }

        .total-label { font-size: 0.8rem; opacity: 0.7; letter-spacing: 2px; }
        .total-amount { font-size: 3.2rem; font-weight: 900; display: block; color: #00ff88; text-shadow: 0 0 20px rgba(0,255,136,0.5); }

        .footer-reset {
            margin-top: 30px;
            font-size: 0.8rem;
            opacity: 0.4;
            cursor: pointer;
            text-decoration: underline;
        }
    </style>
</head>
<body>

<div class="glass-card">
    <h2>Ҳисобот</h2>

    <div class="stat-container">
        <div class="stat-item">
            <span class="stat-label">ОДАМОН</span>
            <span class="stat-value" id="p-count">0</span>
            <span class="stat-money" id="p-sum">0 см</span>
        </div>
        <div class="stat-item">
            <span class="stat-label">СИГАРЕТ</span>
            <span class="stat-value" id="s-count">0</span>
            <span class="stat-money" id="s-sum">0 см</span>
        </div>
    </div>

    <div class="control-row">
        <button class="main-btn btn-p" onclick="change('p', 1)">🚶+ Одам</button>
        <button class="undo-btn" onclick="change('p', -1)">↩️</button>
    </div>

    <div class="control-row">
        <button class="main-btn btn-s" onclick="change('s', 1)">🚬+ Сигарет</button>
        <button class="undo-btn" onclick="change('s', -1)">↩️</button>
    </div>

    <div class="total-display">
        <span class="total-label">ҶАМЪИ УМУМӢ</span>
        <span class="total-amount" id="grand-total">0 см</span>
    </div>

    <div class="footer-reset" onclick="resetAll()">Тоза кардани таърих (Рӯзи нав)</div>
</div>

<script>
    // Боркунии маълумот аз хотираи браузер (агар бошад)
    let state = JSON.parse(localStorage.getItem('kassaData')) || { p: 0, s: 0 };
    const price = 2;

    // Навсозии сайт ҳангоми кушода шудан
    update();

    function change(type, val) {
        if (state[type] + val >= 0) {
            state[type] += val;
            saveAndUpdate();
        }
    }

    function saveAndUpdate() {
        // Захира кардан дар хотира
        localStorage.setItem('kassaData', JSON.stringify(state));
        update();
    }

    function update() {
        document.getElementById('p-count').innerText = state.p;
        document.getElementById('s-count').innerText = state.s;
        
        const pMoney = state.p * price;
        const sMoney = state.s * price;
        
        document.getElementById('p-sum').innerText = pMoney + " см";
        document.getElementById('s-sum').innerText = sMoney + " см";
        document.getElementById('grand-total').innerText = (pMoney + sMoney) + " см";
    }

    function resetAll() {
        if(confirm("Оё боварӣ доред, ки рӯзи навро оғоз мекунед ва ҳамаро 0 мекунед?")) {
            state = { p: 0, s: 0 };
            saveAndUpdate();
        }
    }
</script>

</body>
</html>
