<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Reliance Digital | Smart Laptop Finder</title>
    <style>
        :root {
            --primary: #38bdf8;
            --bg: #020617;
            --card-bg: rgba(30, 41, 59, 0.7);
            --accent: #22c55e;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Inter', system-ui, sans-serif; }

        body {
            background-color: var(--bg);
            color: white;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow-x: hidden;
        }

        /* Dynamic Background Glow */
        .glow {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: radial-gradient(circle at 50% -20%, #0ea5e955, transparent 70%);
            z-index: -1;
            transition: 0.8s ease;
        }

        .container {
            width: 90%;
            max-width: 650px;
            background: var(--card-bg);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(255,255,255,0.1);
            border-radius: 24px;
            padding: 40px;
            box-shadow: 0 25px 50px -12px rgba(0,0,0,0.5);
        }

        .step-info { color: var(--primary); font-weight: 700; margin-bottom: 10px; font-size: 0.9rem; }
        h2 { font-size: 1.8rem; margin-bottom: 25px; line-height: 1.3; }

        .options-grid { display: grid; gap: 12px; }

        .option-card {
            background: rgba(255,255,255,0.05);
            border: 1px solid rgba(255,255,255,0.1);
            padding: 18px;
            border-radius: 15px;
            cursor: pointer;
            transition: 0.3s;
            display: flex;
            align-items: center;
        }

        .option-card:hover { background: rgba(56, 189, 248, 0.15); border-color: var(--primary); }
        .option-card.selected { background: var(--primary); color: #000; font-weight: 600; }

        .btn-row { display: flex; justify-content: space-between; margin-top: 30px; }
        button {
            padding: 12px 30px;
            border-radius: 12px;
            border: none;
            font-weight: 600;
            cursor: pointer;
            transition: 0.2s;
        }

        .btn-next { background: var(--primary); color: #000; }
        .btn-back { background: transparent; color: #94a3b8; border: 1px solid #334155; }

        .hidden { display: none; }

        /* Result Card */
        .result-card { text-align: center; animation: fadeIn 0.6s ease; }
        .price-tag { color: var(--accent); font-size: 1.5rem; font-weight: bold; margin: 15px 0; }
        .article-id { font-family: monospace; background: #1e293b; padding: 5px 10px; border-radius: 5px; }
        .buy-now {
            display: block; background: var(--accent); color: white;
            padding: 18px; border-radius: 12px; text-decoration: none;
            margin-top: 25px; font-weight: 700; font-size: 1.1rem;
        }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body>

<div class="glow" id="glow"></div>

<div class="container">
    <div id="quiz-ui">
        <div class="step-info" id="stepText">Step 1 of 8</div>
        <h2 id="questionText">What will you use the laptop for?</h2>
        
        <div class="options-grid" id="options">
            </div>

        <div class="btn-row">
            <button class="btn-back" id="backBtn" onclick="prevStep()" style="visibility:hidden">Back</button>
            <button class="btn-next" id="nextBtn" onclick="nextStep()">Next Question</button>
        </div>
    </div>

    <div id="result-ui" class="hidden">
        <div class="result-card">
            <div style="font-size: 3rem;">💻</div>
            <h2 id="resTitle">Best Match Found!</h2>
            <h3 id="resModel" style="color: var(--primary);">Laptop Model Name</h3>
            <p id="resSpecs" style="color: #94a3b8; margin: 10px 0;"></p>
            <p class="article-id" id="resArticle">Article: 000000000</p>
            <a href="#" id="resLink" target="_blank" class="buy-now">View on Reliance Digital</a>
            <button onclick="location.reload()" style="margin-top:20px; background:none; color:#64748b; border:none; cursor:pointer;">Restart Quiz</button>
        </div>
    </div>
</div>

<script>
    // --- DATABASE: ADD YOUR 300+ ARTICLES HERE ---
    // Tags: use, brand, budget (low/mid/high), carry (yes/no), screen (ips/oled)
    const laptopDB = [
        { article: "494352115", name: "HP Victus 15", specs: "Ryzen 5, 16GB, 512GB SSD, RTX 3050", tags: { use: "gaming", brand: "hp", budget: "mid", carry: "no", screen: "ips" } },
        { article: "493712555", name: "Lenovo Yoga 7i", specs: "Intel i7 13th Gen, 16GB, Touch Flip", tags: { use: "office", brand: "lenovo", budget: "high", carry: "yes", screen: "oled" } },
        { article: "494421456", name: "Apple MacBook Air M3", specs: "M3 Chip, 8GB RAM, 256GB SSD", tags: { use: "creator", brand: "mac", budget: "high", carry: "yes", screen: "ips" } },
        { article: "493179294", name: "Dell Inspiron 3520", specs: "Intel i3 12th Gen, 8GB RAM, 512GB SSD", tags: { use: "basic", brand: "dell", budget: "low", carry: "no", screen: "ips" } },
        { article: "494399102", name: "ASUS Vivobook 16X", specs: "Ryzen 7, 16GB, RTX 2050, OLED", tags: { use: "gaming", brand: "asus", budget: "mid", carry: "yes", screen: "oled" } },
        // ... simply add more lines like these to reach 300+
    ];

    const questions = [
        { id: "use", q: "What's the main purpose?", options: [{l:"Browsing & Students", v:"basic"}, {l:"Office & Coding", v:"office"}, {l:"Gaming", v:"gaming"}, {l:"Video Editing", v:"creator"}]},
        { id: "budget", q: "Your Budget Range?", options: [{l:"Under ₹40k", v:"low"}, {l:"₹40k - ₹75k", v:"mid"}, {l:"Above ₹75k", v:"high"}]},
        { id: "brand", q: "Preferred Brand?", options: [{l:"HP", v:"hp"}, {l:"Lenovo", v:"lenovo"}, {l:"Dell", v:"dell"}, {l:"ASUS", v:"asus"}, {l:"Apple (Mac)", v:"mac"}, {l:"Any Brand", v:"any"}]},
        { id: "carry", q: "Need to carry it daily?", options: [{l:"Yes, Light & Thin", v:"yes"}, {l:"No, Performance First", v:"no"}]},
        { id: "screen", q: "Screen Preference?", options: [{l:"OLED (Vivid Colors)", v:"oled"}, {l:"IPS (Standard)", v:"ips"}]},
        { id: "touch", q: "Do you need Touchscreen?", options: [{l:"Yes", v:"yes"}, {l:"No", v:"no"}]},
        { id: "battery", q: "Battery Expectation?", options: [{l:"All Day (8h+)", v:"long"}, {l:"Standard (4h+)", v:"normal"}]},
        { id: "visual", q: "Preferred Screen Size?", options: [{l:"Compact (13-14\")", v:"small"}, {l:"Standard (15.6\")", v:"large"}]}
    ];

    let currentStep = 0;
    let userAnswers = {};

    function renderQuestion() {
        const step = questions[currentStep];
        document.getElementById('stepText').innerText = `Step ${currentStep + 1} of ${questions.length}`;
        document.getElementById('questionText').innerText = step.q;
        
        const optionsDiv = document.getElementById('options');
        optionsDiv.innerHTML = '';
        
        step.options.forEach(opt => {
            const div = document.createElement('div');
            div.className = `option-card ${userAnswers[step.id] === opt.v ? 'selected' : ''}`;
            div.innerText = opt.l;
            div.onclick = () => selectOption(step.id, opt.v);
            optionsDiv.appendChild(div);
        });

        document.getElementById('backBtn').style.visibility = currentStep === 0 ? 'hidden' : 'visible';
        document.getElementById('nextBtn').innerText = currentStep === questions.length - 1 ? "Get Recommendation" : "Next Question";
    }

    function selectOption(key, val) {
        userAnswers[key] = val;
        renderQuestion();
    }

    function nextStep() {
        if (!userAnswers[questions[currentStep].id]) {
            alert("Please pick an option!");
            return;
        }
        if (currentStep < questions.length - 1) {
            currentStep++;
            renderQuestion();
        } else {
            calculateResult();
        }
    }

    function prevStep() {
        if (currentStep > 0) {
            currentStep--;
            renderQuestion();
        }
    }

    function calculateResult() {
        let scoredList = laptopDB.map(laptop => {
            let score = 0;
            // 1. Budget is Critical (Penalty System)
            if (laptop.tags.budget === userAnswers.budget) score += 50;
            else score -= 100;

            // 2. Brand preference
            if (userAnswers.brand !== 'any' && laptop.tags.brand === userAnswers.brand) score += 30;

            // 3. Use Case Match
            if (laptop.tags.use === userAnswers.use) score += 25;

            // 4. Portability / Screen
            if (laptop.tags.carry === userAnswers.carry) score += 10;
            if (laptop.tags.screen === userAnswers.screen) score += 10;

            return { ...laptop, totalScore: score };
        });

        // Sort by highest score
        const winner = scoredList.sort((a, b) => b.totalScore - a.totalScore)[0];
        showResult(winner);
    }

    function showResult(item) {
        document.getElementById('quiz-ui').classList.add('hidden');
        document.getElementById('result-ui').classList.remove('hidden');
        document.getElementById('resModel').innerText = item.name;
        document.getElementById('resSpecs').innerText = item.specs;
        document.getElementById('resArticle').innerText = "Article Code: " + item.article;
        document.getElementById('resLink').href = `https://www.reliancedigital.in/search?q=${item.article}:relevance`;
    }

    renderQuestion();
</script>
</body>
</html>
