<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>活力早餐店：晨間戰場</title>
    <link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@400;600&family=Noto+Sans+TC:wght@400;700&display=swap" rel="stylesheet">
    <style>
        :root {
            /* 早餐店配色 */
            --toast-color: #f4a261;
            --egg-yellow: #ffb703;
            --lettuce-green: #90be6d;
            --ketchup-red: #e76f51;
            --soy-milk: #fdfcdc;
            --bg-color: #fff8e1;
            --text-brown: #5d4037;
            --panel-white: #ffffff;
        }

        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
        
        body {
            font-family: 'Fredoka', 'Noto Sans TC', sans-serif;
            background-color: var(--bg-color);
            color: var(--text-brown);
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            /* 背景圖案：方格桌布 */
            background-image: 
                linear-gradient(90deg, rgba(231, 111, 81, 0.1) 1px, transparent 1px),
                linear-gradient(rgba(231, 111, 81, 0.1) 1px, transparent 1px);
            background-size: 40px 40px;
            overscroll-behavior: none; 
        }

        .game-card {
            background-color: var(--panel-white);
            border: 4px solid var(--toast-color);
            border-radius: 25px;
            width: 100%;
            max-width: 900px;
            box-shadow: 0 10px 0 rgba(244, 162, 97, 0.4);
            padding: 30px;
            position: relative;
            display: flex;
            flex-direction: column;
            max-height: 95vh;
            overflow-y: auto;
        }

        /* 裝飾：店招牌 */
        .signboard {
            background: var(--ketchup-red);
            color: white;
            padding: 10px 30px;
            border-radius: 50px;
            position: absolute;
            top: -25px;
            left: 50%;
            transform: translateX(-50%) rotate(-2deg);
            font-size: 1.5rem;
            font-weight: bold;
            box-shadow: 0 5px 0 rgba(0,0,0,0.2);
            z-index: 10;
            white-space: nowrap;
        }

        .header {
            margin-top: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 3px dashed var(--egg-yellow);
            padding-bottom: 15px;
            margin-bottom: 20px;
        }

        .stats-box {
            background: var(--soy-milk);
            padding: 8px 15px;
            border-radius: 15px;
            border: 2px solid var(--egg-yellow);
            font-weight: bold;
            color: var(--text-brown);
        }

        /* 故事區塊 */
        .story-bubble {
            background: white;
            border: 3px solid var(--lettuce-green);
            border-radius: 20px;
            padding: 20px;
            margin-bottom: 25px;
            font-size: 1.3rem;
            line-height: 1.6;
            position: relative;
        }
        .story-bubble::after {
            content: '🥪'; position: absolute; bottom: -15px; right: 20px; font-size: 2rem;
        }

        /* 視覺展示區 */
        .visual-plate {
            background: #fff;
            border-radius: 20px;
            border: 2px solid #eee;
            box-shadow: inset 0 0 20px rgba(0,0,0,0.05);
            padding: 25px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 250px;
            margin-bottom: 25px;
            position: relative;
            overflow: hidden;
        }

        /* Menu Table */
        .menu-table {
            width: 100%; border-collapse: separate; border-spacing: 0 8px;
        }
        .menu-table td {
            background: var(--soy-milk); padding: 15px;
            border-top: 2px solid var(--egg-yellow);
            border-bottom: 2px solid var(--egg-yellow);
            font-size: 1.2rem;
        }
        .menu-table td:first-child { 
            border-left: 2px solid var(--egg-yellow); border-radius: 15px 0 0 15px; font-weight:bold; 
        }
        .menu-table td:last-child { 
            border-right: 2px solid var(--egg-yellow); border-radius: 0 15px 15px 0; text-align: right; 
        }

        /* Griddle Grid */
        .griddle {
            display: grid;
            grid-template-columns: repeat(10, 30px); /* 10列 */
            gap: 2px;
            background: #333; /* 鐵板黑 */
            padding: 10px;
            border-radius: 10px;
            border: 4px solid #aaa;
        }
        .grid-cell {
            width: 30px; height: 30px;
            background: #444;
            border-radius: 2px;
        }
        .grid-cell.food { background: var(--egg-yellow); border: 2px solid #f57f17; }
        .grid-cell.hashbrown { background: var(--toast-color); border: 2px solid #d84315; }
        .grid-cell.blocked { 
            background:repeating-linear-gradient(45deg, #555, #555 5px, #666 5px, #666 10px); 
        }

        /* Sandwich Angle Visual */
        .sandwich-visual {
            width: 160px; height: 160px; background: white;
            border: 4px solid var(--toast-color);
            position: relative;
            display: flex; justify-content: center; align-items: center;
        }
        .knife-line {
            position: absolute; width: 140%; height: 4px; background: #ccc;
            transform: rotate(45deg); top: 50%; left: -20%;
            border-top: 2px dashed #666;
            animation: cutAnim 2s infinite;
        }
        @keyframes cutAnim { 0% { opacity: 0.5; } 50% { opacity: 1; } 100% { opacity: 0.5; } }
        
        /* Triangle Sandwich Shape */
        .tri-sandwich {
            width: 0; height: 0;
            border-left: 80px solid transparent;
            border-right: 80px solid transparent;
            border-bottom: 80px solid var(--soy-milk);
            position: relative;
            margin: 0 20px;
        }
        .tri-sandwich::after {
            content: ''; position: absolute; top: 20px; left: -20px;
            width: 40px; height: 40px; background: var(--ketchup-red); border-radius: 50%;
            box-shadow: 10px -10px 0 var(--lettuce-green);
        }

        /* Inputs */
        .input-area { display: flex; gap: 15px; margin-top: 20px; }
        
        input[type="number"] {
            flex: 1; padding: 15px; border-radius: 15px;
            border: 3px solid var(--toast-color);
            background: #fff; font-size: 1.5rem; text-align: center;
            outline: none; color: var(--text-brown);
        }

        button {
            padding: 12px 30px; border: none; border-radius: 15px;
            font-weight: bold; font-size: 1.3rem; cursor: pointer;
            font-family: inherit; transition: transform 0.1s;
            box-shadow: 0 4px 0 rgba(0,0,0,0.1);
        }
        button:active { transform: translateY(4px); box-shadow: none; }

        .btn-primary { background: var(--egg-yellow); color: var(--text-brown); }
        .btn-opt { flex: 1; background: white; border: 3px solid var(--toast-color); color: var(--text-brown); }
        .btn-opt.selected { background: var(--toast-color); color: white; }

        /* Feedback */
        .feedback {
            margin-top: 20px; padding: 15px; border-radius: 15px;
            text-align: center; font-size: 1.3rem; font-weight: bold;
            display: none; animation: popIn 0.3s;
        }
        .feedback.correct { background: var(--lettuce-green); color: white; }
        .feedback.wrong { background: var(--ketchup-red); color: white; }

        @keyframes popIn { from { transform: scale(0.8); opacity: 0; } to { transform: scale(1); opacity: 1; } }
        .hidden { display: none !important; }

        /* Progress Bar */
        .progress-container { width: 100%; height: 10px; background: #eee; border-radius: 5px; overflow: hidden; }
        .progress-fill { height: 100%; background: var(--ketchup-red); width: 0%; transition: width 0.5s; }

        @media (max-width: 600px) {
            .griddle { grid-template-columns: repeat(10, 25px); }
            .grid-cell { width: 25px; height: 25px; }
            .input-area { flex-direction: column; }
        }
    </style>
</head>
<body>

<div class="game-card">
    <div class="signboard">🌞 活力早餐店</div>
    
    <div class="header">
        <div>店長好！<br><small style="color:#888">今日目標：營收破千</small></div>
        <div style="width: 150px;">
            <div style="text-align:right; margin-bottom:5px;">進度</div>
            <div class="progress-container"><div class="progress-fill" id="progress-bar"></div></div>
        </div>
    </div>

    <div id="intro-screen">
        <div class="story-bubble">
            <strong>早安，店長！( ^_^ )/</strong><br><br>
            現在是早上 7:00，學生和上班族都湧進來了！<br>
            煎台滋滋作響，收銀機叮咚響個不停。<br><br>
            為了應付尖峰時刻，你需要：<br>
            1. 💰 <strong>快速結帳</strong> (乘法與加法)<br>
            2. 🥪 <strong>切三明治</strong> (角度辨識)<br>
            3. 🍳 <strong>管理煎台空間</strong> (面積計算)<br><br>
            穿上圍裙，我們開始接單吧！
        </div>
        <div style="text-align: center; margin-top: 40px;">
            <button class="btn-primary" onclick="startGame()" style="font-size: 1.5rem; padding: 20px 60px;">開店營業！🛎️</button>
        </div>
    </div>

    <div id="game-screen" class="hidden">
        <div class="story-bubble" id="q-text">Loading...</div>
        
        <div class="visual-plate" id="q-visual">
            </div>

        <div id="input-number" class="input-area hidden">
            <input type="number" id="user-input" placeholder="輸入數字..." inputmode="decimal">
            <button class="btn-primary" onclick="submitAnswer()">結帳</button>
        </div>

        <div id="input-options" class="input-area hidden">
            </div>

        <div class="feedback" id="feedback"></div>

        <div style="margin-top: 30px; display: flex; justify-content: space-between;">
            <button style="background:transparent; color:#999; border:none; box-shadow:none;" onclick="showHint()">💡 呼叫阿姨幫忙 (提示)</button>
            <button id="btn-next" class="hidden btn-primary" onclick="nextLevel()">下一單 ➡️</button>
        </div>
    </div>

    <div id="end-screen" class="hidden" style="text-align: center;">
        <h1 style="color: var(--ketchup-red); font-size: 2.5rem;">打烊囉！收工！</h1>
        <div class="story-bubble" style="text-align: center;">
            辛苦了，店長！<br>
            今天的帳目清清楚楚，餐點也都完美出餐。<br>
            客人們都說明天的早餐還要來這裡吃！
        </div>
        <div class="stats-box" style="display:inline-block; font-size: 2rem; margin: 20px 0;">
            今日營業額：<span id="final-score" style="color:var(--ketchup-red);">0</span> 元
        </div>
        <br>
        <button class="btn-primary" onclick="location.reload()">明天繼續加油</button>
    </div>
</div>

<script>
    const questions = [
        // Level 1: Multiplication (Order Total)
        {
            type: "number",
            text: "【第一單：大胃王訂單】<br>這位同學剛打完球，肚子很餓！<br>他點了：<br>🥓 <strong>3 份 培根蛋餅 (每份 45 元)</strong><br>🥤 <strong>2 杯 大冰奶 (每杯 25 元)</strong><br><br>請問：這筆訂單總共多少錢？",
            visual: `
                <table class="menu-table">
                    <tr><td>🥓 培根蛋餅</td><td>$45 x 3</td></tr>
                    <tr><td>🥤 大杯奶茶</td><td>$25 x 2</td></tr>
                    <tr><td colspan="2" style="text-align:center; color:#aaa;">(請計算總金額)</td></tr>
                </table>
            `,
            answer: 185,
            hint: "分開算再加起來：\n1. 蛋餅：45 x 3 = ?\n2. 奶茶：25 x 2 = ?\n3. 總和：? + ?",
            explanation: "蛋餅錢：45 x 3 = 135 元。<br>奶茶錢：25 x 2 = 50 元。<br>總金額：135 + 50 = 185 元。<br>收錢囉！"
        },
        // Level 2: Angles (Sandwich Cut)
        {
            type: "option",
            options: ["銳角 (尖尖的)", "直角 (方方的)", "鈍角 (胖胖的)"],
            text: "【第二單：切三明治】<br>客人說要吃「三角形」的三明治。<br>我們把正方形吐司沿著對角線切開 (如下圖)。<br>請觀察切開後三角形的<strong>最尖的那個角</strong>。<br>請問：那是什麼角？",
            visual: `
                <div style="display:flex; gap:20px; align-items:center;">
                    <div style="text-align:center;">
                        <div class="sandwich-visual"><div class="knife-line"></div></div>
                        <small>切開前</small>
                    </div>
                    <div style="font-size:2rem;">➡</div>
                    <div style="text-align:center;">
                        <div style="width:100px; height:100px; display:flex; justify-content:center; align-items:center;">
                            <div class="tri-sandwich"></div>
                        </div>
                        <small>切開後(看尖角)</small>
                    </div>
                </div>
            `,
            answer: "銳角 (尖尖的)",
            hint: "正方形的角是直角(90度)。<br>切一半之後，角變小了，比直角還小的是什麼角？",
            explanation: "正方形切對角線後，尖尖的角是 45 度。<br>比 90 度(直角)還要小，所以是「銳角」喔！"
        },
        // Level 3: Area (Griddle Management)
        {
            type: "number",
            text: "【第三單：煎台空間】<br>我們的鐵板總面積是 <strong>60 格</strong> (10x6)。<br>現在上面正在煎：<br>🥞 <strong>1 份大蔥抓餅 (佔 16 格)</strong><br>🥔 <strong>2 份薯餅 (每份佔 6 格)</strong><br><br>請問：鐵板上還剩下多少空格可以煎別的東西？",
            visual: `
                <div class="griddle">
                    <div class="grid-cell food" style="grid-column: span 4; grid-row: span 4;">蔥抓餅</div>
                    <div class="grid-cell hashbrown" style="grid-column: span 2; grid-row: span 3;">薯</div>
                    <div class="grid-cell hashbrown" style="grid-column: span 2; grid-row: span 3;">薯</div>
                    <div class="grid-cell" style="background:#333; border:none;"></div>
                </div>
                <div style="font-size:0.9rem; color:#888; margin-top:5px;">(總共 60 格，扣掉食物佔用的)</div>
            `,
            answer: 32,
            hint: "1. 算出食物總共佔了幾格：\n   蔥抓餅(16) + 薯餅(6) + 薯餅(6)\n2. 用總面積(60)減去食物面積。",
            explanation: "食物佔用：16 + 6 + 6 = 28 格。<br>剩餘空間：60 - 28 = 32 格。<br>還夠位子煎荷包蛋！"
        }
    ];

    let currentIdx = 0;
    let score = 0;

    document.getElementById('user-input').addEventListener('keypress', function (e) {
        if (e.key === 'Enter') submitAnswer();
    });

    function startGame() {
        document.getElementById('intro-screen').classList.add('hidden');
        document.getElementById('game-screen').classList.remove('hidden');
        loadQuestion();
    }

    function loadQuestion() {
        const q = questions[currentIdx];
        
        document.getElementById('feedback').style.display = 'none';
        document.getElementById('feedback').className = 'feedback';
        document.getElementById('btn-next').classList.add('hidden');
        document.getElementById('user-input').value = '';
        document.getElementById('user-input').disabled = false;
        
        document.getElementById('progress-bar').style.width = `${(currentIdx / questions.length) * 100}%`;
        
        document.getElementById('q-text').innerHTML = q.text;
        document.getElementById('q-visual').innerHTML = q.visual;

        const numInput = document.getElementById('input-number');
        const optInput = document.getElementById('input-options');

        if (q.type === 'number') {
            numInput.classList.remove('hidden');
            optInput.classList.add('hidden');
        } else {
            numInput.classList.add('hidden');
            optInput.classList.remove('hidden');
            optInput.innerHTML = '';
            q.options.forEach(opt => {
                const btn = document.createElement('button');
                btn.className = 'btn-opt';
                btn.innerText = opt;
                btn.onclick = () => checkOption(opt, btn);
                optInput.appendChild(btn);
            });
        }
    }

    function showHint() {
        alert("🍳 阿姨提示：\n" + questions[currentIdx].hint);
    }

    function submitAnswer() {
        const val = document.getElementById('user-input').value;
        if (!val) return;
        checkResult(parseFloat(val) === questions[currentIdx].answer);
    }

    function checkOption(val, btn) {
        document.querySelectorAll('.btn-opt').forEach(b => b.classList.remove('selected'));
        btn.classList.add('selected');
        checkResult(val === questions[currentIdx].answer);
    }

    function checkResult(isCorrect) {
        const fb = document.getElementById('feedback');
        fb.style.display = 'block';

        if (isCorrect) {
            score += 500;
            fb.innerHTML = `✅ <strong>沒錯！</strong><br>${questions[currentIdx].explanation}`;
            fb.className = 'feedback correct';
            
            document.getElementById('user-input').disabled = true;
            document.querySelectorAll('.btn-opt').forEach(b => b.disabled = true);
            document.getElementById('btn-next').classList.remove('hidden');
        } else {
            fb.innerHTML = `❌ <strong>算錯囉！</strong><br>再算一次看看，客人再等了！`;
            fb.className = 'feedback wrong';
        }
    }

    function nextLevel() {
        currentIdx++;
        if (currentIdx < questions.length) {
            loadQuestion();
        } else {
            endGame();
        }
    }

    function endGame() {
        document.getElementById('game-screen').classList.add('hidden');
        document.getElementById('end-screen').classList.remove('hidden');
        document.getElementById('progress-bar').style.width = '100%';
        document.getElementById('final-score').innerText = score;
    }
</script>

</body>
</html>
