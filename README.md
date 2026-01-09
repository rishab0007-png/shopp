<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Reliance Digital | AI Laptop Expert</title>
    <style>
        :root { --primary: #38bdf8; --bg: #020617; --card: rgba(30, 41, 59, 0.7); --success: #22c55e; }
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', sans-serif; }
        
        body {
            background: var(--bg);
            color: white;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background-image: radial-gradient(circle at 50% 10%, #0ea5e922, transparent);
        }

        .container {
            width: 95%; max-width: 600px;
            background: var(--card);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(255,255,255,0.1);
            border-radius: 28px;
            padding: 40px;
            box-shadow: 0 25px 50px -12px rgba(0,0,0,0.5);
        }

        .progress-bar { height: 4px; background: rgba(255,255,255,0.1); margin-bottom: 30px; border-radius: 2px; overflow: hidden; }
        .progress-fill { height: 100%; background: var(--primary); transition: 0.4s; width: 12.5%; }

        h2 { font-size: 1.5rem; margin-bottom: 25px; font-weight: 600; }

        .options-list { display: grid; gap: 12px; }
        .opt-btn {
            background: rgba(255,255,255,0.05);
            border: 1px solid rgba(255,255,255,0.1);
            padding: 18px; border-radius: 16px;
            cursor: pointer; text-align: left; transition: 0.2s;
            font-size: 1rem; color: #cbd5e1;
        }
        .opt-btn:hover { background: rgba(56, 189, 248, 0.1); border-color: var(--primary); }
        .opt-btn.active { background: var(--primary); color: #020617; font-weight: 700; border-color: var(--primary); }

        .controls { display: flex; justify-content: space-between; margin-top: 35px; }
        .nav-btn { padding: 12px 28px; border-radius: 12px; border: none; font-weight: 700; cursor: pointer; transition: 0.2s; }
        .next { background: var(--primary); color: #020617; }
        .back { background: transparent; color: #94a3b8; border: 1px solid #334155; }
        .next:disabled { opacity: 0.5; cursor: not-allowed; }

        .hidden { display: none !important; }
        .result-screen { text-align: center; animation: fadeIn 0.6s ease; }
        .article-tag { display: inline-block; background: #1e293b; padding: 8px 16px; border-radius: 8px; border: 1px solid var(--primary); color: var(--primary); font-family: monospace; margin: 15px 0; }
        .buy-link { display: block; background: var(--success); color: white; padding: 18px; border-radius: 15px; text-decoration: none; font-weight: 800; margin-top: 20px; }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } }
    </style>
</head>
<body>

<div class="container">
    <div id="quiz-part">
        <div class="progress-bar"><div class="progress-fill" id="pFill"></div></div>
        <h2 id="qText">Loading...</h2>
        <div class="options-list" id="optList"></div>
        <div class="controls">
            <button class="nav-btn back" id="back" onclick="changeStep(-1)">Back</button>
            <button class="nav-btn next" id="next" onclick="changeStep(1)" disabled>Next</button>
        </div>
    </div>

    <div id="res-part" class="hidden">
        <div class="result-screen">
            <div style="font-size: 3rem; margin-bottom: 10px;">🌟</div>
            <h2 id="resName"></h2>
            <p id="resSpecs" style="color: #94a3b8; line-height: 1.6;"></p>
            <div class="article-tag" id="resArt"></div>
            <a href="#" id="resUrl" target="_blank" class="buy-link">Check Availability at Reliance Digital</a>
            <button onclick="location.reload()" style="background:none; border:none; color:#64748b; margin-top:20px; cursor:pointer;">Restart Recommendation</button>
        </div>
    </div>
</div>

<script>
    /* --- THE DATABASE (ADD ALL 300+ ARTICLES HERE) --- */
    const db = [
        { art: "494352115", name: "HP Victus 15", brand: "hp", budget: "mid", use: "gaming", specs: "Ryzen 5 | 16GB RAM | RTX 3050 | 512GB SSD" },
        { art: "493712555", name: "Lenovo Yoga 7i", brand: "lenovo", budget: "high", use: "office", specs: "Intel i7 | 16GB RAM | Touch Screen | 2-in-1" },
        { art: "493179294", name: "Dell Inspiron 15", brand: "dell", budget: "low", use: "basic", specs: "Intel i3 | 8GB RAM | 512GB SSD | Win 11" },
        { art: "494421456", name: "Apple MacBook Air M3", brand: "mac", budget: "high", use: "creator", specs: "Apple M3 Chip | 8GB RAM | Liquid Retina" },
        { art: "494399102", name: "ASUS Vivobook 16X", brand: "asus", budget: "mid", use: "gaming", specs: "Ryzen 7 | 16GB RAM | RTX 2050 | OLED" },
        // ... paste more items here
    ];

    const questions = [
        { id: "use", q: "What is your main requirement?", opts: [{l:"Student / Home Use", v:"basic"}, {l:"Office / Professional", v:"office"}, {l:"Gaming / High Performance", v:"gaming"}, {l:"Graphic / Video Editing", v:"creator"}]},
        { id: "brand", q: "Which brand do you prefer?", opts: [{l:"HP", v:"hp"}, {l:"Lenovo", v:"lenovo"}, {l:"Dell", v:"dell"}, {l:"ASUS", v:"asus"}, {l:"Apple (Mac)", v:"mac"}, {l:"I like all brands", v:"any"}]},
        { id: "budget", q: "Select your budget range:", opts: [{l:"Entry (Under 40k)", v:"low"}, {l:"Value (40k - 75k)", v:"mid"}, {l:"Premium (Above 75k)", v:"high"}]},
        // You can add 5 more questions here
    ];

    let step = 0;
    let picks = {};

    function loadQ() {
        const q = questions[step];
        document.getElementById('qText').innerText = q.q;
        document.getElementById('pFill').style.width = ((step + 1) / questions.length * 100) + "%";
        
        const list = document.getElementById('optList');
        list.innerHTML = "";
        
        q.opts.forEach(o => {
            const b = document.createElement('button');
            b.className = `opt-btn ${picks[q.id] === o.v ? 'active' : ''}`;
            b.innerText = o.l;
            b.onclick = () => { picks[q.id] = o.v; loadQ(); document.getElementById('next').disabled = false; };
            list.appendChild(b);
        });

        document.getElementById('back').style.visibility = step === 0 ? "hidden" : "visible";
        document.getElementById('next').innerText = step === questions.length - 1 ? "Find My Laptop" : "Next";
        document.getElementById('next').disabled = !picks[q.id];
    }

    window.changeStep = (n) => {
        if (n === 1 && step < questions.length - 1) { step++; loadQ(); }
        else if (n === -1 && step > 0) { step--; loadQ(); }
        else if (n === 1) { showFinal(); }
    };

    function showFinal() {
        /* --- ERROR-PROOF LOGIC --- */
        // 1. First, strictly filter by brand IF the user picked one
        let filtered = db;
        if (picks.brand !== "any") {
            filtered = db.filter(item => item.brand === picks.brand);
        }

        // 2. If the user's chosen brand has no match for their budget, 
        // fallback to the whole database so they aren't left with "No Results"
        if (filtered.length === 0) { filtered = db; }

        // 3. Score the remaining items
        let winner = filtered.map(item => {
            let s = 0;
            if (item.budget === picks.budget) s += 50;
            if (item.use === picks.use) s += 30;
            return { ...item, s };
        }).sort((a,b) => b.s - a.s)[0];

        // 4. Update UI
        document.getElementById('quiz-part').classList.add('hidden');
        document.getElementById('res-part').classList.remove('hidden');
        document.getElementById('resName').innerText = winner.name;
        document.getElementById('resSpecs').innerText = winner.specs;
        document.getElementById('resArt').innerText = "Reliance Article: " + winner.art;
        document.getElementById('resUrl').href = `https://www.reliancedigital.in/search?q=${winner.art}:relevance`;
    }

    document.addEventListener('DOMContentLoaded', loadQ);
</script>
</body>
</html>
