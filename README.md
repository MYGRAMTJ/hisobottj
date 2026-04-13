<!DOCTYPE html>
<html lang="tg">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Кассаи Шишагӣ</title>
    <style>
        * { box-sizing: border-box; transition: all 0.3s ease; }
        
        body { 
            margin: 0; 
            padding: 20px; 
            font-family: 'Segoe UI', Roboto, sans-serif; 
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); 
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .glass-card {
            background: rgba(255, 255, 255, 0.15);
            backdrop-filter: blur(15px);
            -webkit-backdrop-filter: blur(15px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 30px;
            padding: 30px;
            width: 100%;
            max-width: 420px;
            box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
            color: white;
        }

        h2 { text-align: center; font-weight: 300; letter-spacing: 1px; margin-bottom: 30px; }

        .stat-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-bottom: 30px;
        }

        .stat-item {
            background: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 20px;
            text-align: center;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .stat-label { font-size: 0.75rem; text-transform: uppercase; opacity: 0.8; }
        .stat-value { font-size: 1.5rem; font-weight: bold; display: block; margin: 5px 0; }
        .stat-money { font-size: 1rem; color: #00ff88; font-weight: 500; }

        .control-group {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }

        .main-btn {
            flex: 4;
            padding: 20px;
            border: none;
            border-radius: 20px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
        }

        .undo-btn {
            flex: 1;
            padding: 20px;
            border: none;
            border-radius: 20px;
            background: rgba(255, 255, 255, 0.2);
            color: white;
            cursor: pointer;
            font-size: 1.2rem;
        }

        .btn-p { background: linear-gradient(45deg, #4facfe 0%, #00f2fe 100%); }
        .btn-s { background: linear-gradient(45deg, #f093fb 0%, #f5576c 100%); }

        .main-btn:active, .undo-btn:active { transform: scale(0.92); }

        .total-display {
            background: rgba(255, 255, 255, 0.1);
            margin-top: 10px;
            padding: 25px;
            border-radius: 25px;
            text-align: center;
            border: 1px solid rgba(0, 255, 136, 0.3);
        }

        .total-label { font-size: 0.9rem; opacity: 0.9; }
        .total-amount { font-size: 3rem; font-weight: 800; display: block; color: #00ff88; text-shadow: 0 0 15px rgba(0,255,136,0.4); }

        .footer-reset {
            text-align: center;
            margin-top: 30px;
            font-size: 0.8rem;
            opacity: 0.5;
            text-decoration: underline;
            cursor: pointer;
        }
    </style>
</head>
<body>

<div class="glass-card">
    <h2>ҲИСОБОТ</h2>

    <div class="stat-container">
        <div class="stat-item">
            <span class="stat-label">Одамон</span>
            <span class="stat-value" id="p-count">0</span>
            <span class="stat-money" id="p-sum">0 см</span>
        </div>
        <div class="stat-item">
            <span class="stat-label">Сигарет</span>
            <span class="stat-value" id="s-count">0</span>
            <span class="stat-money" id="s-sum">0 см</span>
        </div>
    </div>

    <div class="control-group">
        <button class="main-btn btn-p" onclick="change('p', 1)">🚶+ Одам</button>
        <button class="undo-btn" onclick="change('p', -1)">↩️</button>
    </div>

    <div class="control-group">
        <button class="main-btn btn-s" onclick="change('s', 1)">🚬+ Сигарет</button>
        <button class="undo-btn" onclick="change('s', -1)">↩️</button>
    </div>

    <div class="total-display">
        <span class="total-label">ҶАМЪИ УМУМӢ</span>
        <span class="total-amount" id="grand-total">0 см</span>
    </div>

    <div class="footer-reset" onclick="resetAll()">Тоза кардани рӯз</div>
</div>

<script>
    let state = { p: 0, s: 0 };
    const price = 2;

    function change(type, val) {
        if (state[type] + val >= 0) {
            state[type] += val;
            update();
        }
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
        if(confirm("Ҳамаи маълумот тоза карда шавад?")) {
            state = { p: 0, s: 0 };
            update();
        }
    }
</script>

</body>
</html>
