<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart Laptop Helper - Reliance Digital</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body {
            font-family: -apple-system, BlinkMacSystemFont, "SF Pro Display", sans-serif;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            color: #f5f5f5;
            background: #000;
            overflow-x: hidden;
        }

        /* 2026 Dynamic Background Layers */
        .bg-layer { position: fixed; inset: 0; z-index: -3; transition: all 1s cubic-bezier(0.32, 0.72, 0, 1); }
        .bg-step-1  { background: radial-gradient(circle at top, #0ea5e9 0, #020617 55%); }
        .bg-step-2  { background: radial-gradient(circle at top, #22c55e 0, #020617 55%); }
        .bg-step-3  { background: radial-gradient(circle at top, #eab308 0, #020617 55%); }
        .bg-step-4  { background: radial-gradient(circle at top, #a855f7 0, #020617 55%); }
        .bg-step-5  { background: radial-gradient(circle at top, #f97316 0, #020617 55%); }
        .bg-step-6  { background: radial-gradient(circle at top, #06b6d4 0, #020617 55%); }
        .bg-step-7  { background: radial-gradient(circle at top, #ec4899 0, #020617 55%); }
        .bg-step-8  { background: radial-gradient(circle at top, #facc15 0, #020617 55%); }
        .bg-step-9  { background: radial-gradient(circle at top, #22c55e 0, #020617 55%); }
        .bg-step-10 { background: radial-gradient(circle at top, #3b82f6 0, #020617 55%); }

        .container { width: 100%; max-width: 600px; padding: 24px 16px; margin: 0 auto; z-index: 1; }

        /* Liquid Glass Card */
        .assistant-card {
            position: relative; border-radius: 40px; padding: 32px; margin-top: 40px;
            background: rgba(255, 255, 255, 0.08);
            border: 1px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 40px 100px rgba(0,0,0,0.5);
            backdrop-filter: blur(40px) saturate(180%);
            -webkit-backdrop-filter: blur(40px) saturate(180%);
        }

        .island {
            width: 140px; height: 32px; background: #000; color: #fff;
            margin: -15px auto 30px; border-radius: 20px;
            display: flex; align-items: center; justify-content: center;
            font-size: 11px; font-weight: 700;
        }

        .header-row h1 { font-size: 1.4rem; letter-spacing: -0.5px; margin-bottom: 5px;}
        .header-row p { font-size: 0.9rem; opacity: 0.6; margin-bottom: 25px;}

        .question { display: none; animation: iosSlide 0.5s cubic-bezier(0.32, 0.72, 0, 1); }
        .question.active { display: block; }

        @keyframes iosSlide {
            from { opacity: 0; transform: translateY(15px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .option-label {
            display: block; padding: 18px; background: rgba(255,255,255,0.06);
            border: 1px solid transparent; border-radius: 20px; margin-bottom: 12px;
            cursor: pointer; transition: 0.2s;
        }
        .option-label:hover { background: rgba(255,255,255,0.1); }
        .option-label.selected { border-color: #38bdf8; background: rgba(56, 189, 248, 0.15); }
        .option-label input { display: none; }

        .dock { display: flex; gap: 12px; margin-top: 30px; }
        button {
            flex: 1; height: 55px; border-radius: 18px; border: none; font-weight: 600; cursor: pointer;
        }
        .btn-primary { background: #fff; color: #000; }
        .btn-secondary { background: rgba(255,255,255,0.1); color: #fff; }

        #result { display: none; text-align: center; }
        .price-tag { font-size: 32px; color: #38bdf8; font-weight: 800; margin: 15px 0; }
        .article-box { background: #000; padding: 15px; border-radius: 15px; font-family: monospace; font-size: 14px; }
    </style>
</head>
<body>

<div class="bg-layer bg-step-1" id="bgLayer"></div>

<div class="container">
    <div class="assistant-card">
        <div class="island" id="island">STEP 1 OF 10</div>
        
        <div class="header-row">
            <h1>Smart Laptop Helper</h1>
            <p>Reliance Digital 2026 Inventory</p>
        </div>

        <form id="quizForm">
            <div class="question active" data-step="1">
                <h3>Main Usage</h3>
                <label class="option-label"><input type="radio" name="q1" value="basic"> Basic Browsing & YouTube</label>
                <label class="option-label"><input type="radio" name="q1" value="office"> Office, Excel & Zoom</label>
                <label class="option-label"><input type="radio" name="q1" value="gaming"> Gaming & 3D Render</label>
            </div>

            <div class="question" data-step="2">
                <h3>Portability</h3>
                <label class="option-label"><input type="radio" name="q2" value="daily"> Carry Every Day</label>
                <label class="option-label"><input type="radio" name="q2" value="rarely"> Mostly Desk Use</label>
            </div>

            <div class="question" data-step="3">
                <h3>Battery Need</h3>
                <label class="option-label"><input type="radio" name="q3" value="4hr"> 4-5 Hours</label>
                <label class="option-label"><input type="radio" name="q3" value="10hr"> 10+ Hours (Long Battery)</label>
            </div>

            <div class="question" data-step="4">
                <h3>Budget</h3>
                <label class="option-label"><input type="radio" name="q4" value="30k"> Under ₹40,000</label>
                <label class="option-label"><input type="radio" name="q4" value="70k"> ₹40,000 - ₹80,000</label>
                <label class="option-label"><input type="radio" name="q4" value="high"> Above ₹80,000</label>
            </div>

            <div class="question" data-step="5">
                <h3>Screen Size</h3>
                <label class="option-label"><input type="radio" name="q5" value="13"> 13-14" (Compact)</label>
                <label class="option-label"><input type="radio" name="q5" value="15"> 15.6" (Standard)</label>
            </div>

            <div class="question" data-step="6">
                <h3>OS Preference</h3>
                <label class="option-label"><input type="radio" name="q6" value="windows"> Windows 11</label>
                <label class="option-label"><input type="radio" name="q6" value="macos"> Apple macOS</label>
            </div>

            <div class="question" data-step="7">
                <h3>RAM Amount</h3>
                <label class="option-label"><input type="radio" name="q7" value="8"> 8GB (Basic)</label>
                <label class="option-label"><input type="radio" name="q7" value="16"> 16GB+ (Smooth)</label>
            </div>

            <div class="question" data-step="8">
                <h3>Storage</h3>
                <label class="option-label"><input type="radio" name="q8" value="512"> 512GB SSD</label>
                <label class="option-label"><input type="radio" name="q8" value="1tb"> 1TB SSD</label>
            </div>

            <div class="question" data-step="9">
                <h3>Screen Type</h3>
                <label class="option-label"><input type="radio" name="q9" value="ips"> Standard LCD</label>
                <label class="option-label"><input type="radio" name="q9" value="oled"> OLED (Colors)</label>
            </div>

            <div class="question" data-step="10">
                <h3>Upgrade Cycle</h3>
                <label class="option-label"><input type="radio" name="q10" value="2y"> Use for 2 Years</label>
                <label class="option-label"><input type="radio" name="q10" value="5y"> Long Term (4-5 Years)</label>
            </div>

            <div class="dock">
                <button type="button" class="btn-secondary" id="backBtn" onclick="move(-1)" style="visibility:hidden">Back</button>
                <button type="button" class="btn-primary" id="nextBtn" onclick="move(1)">Continue</button>
            </div>
        </form>

        <div id="result">
            <h4 style="color:#38bdf8">TOP MATCH</h4>
            <h1 id="resName"></h1>
            <p id="resSpecs" style="margin:10px 0; opacity:0.7;"></p>
            <div class="price-tag" id="resPrice"></div>
            <div class="article-box">Reliance Article: <span id="resCode" style="color:#38bdf8"></span></div>
            <button class="btn-primary" style="margin-top:30px; width:100%;" onclick="location.reload()">Reset Assistant</button>
        </div>
    </div>
</div>

<script>
    let currentStep = 1;
    const totalSteps = 10;
    const bgLayer = document.getElementById('bgLayer');
    const island = document.getElementById('island');

    // Handle Visual Selection
    document.querySelectorAll('.option-label').forEach(label => {
        label.addEventListener('click', () => {
            label.parentElement.querySelectorAll('.option-label').forEach(l => l.classList.remove('selected'));
            label.classList.add('selected');
        });
    });

    function move(dir) {
        // Validation: Must select an option
        const currentQ = document.querySelector(`.question[data-step="${currentStep}"]`);
        if (dir === 1 && !currentQ.querySelector('input:checked')) {
            alert("Please select an option to continue.");
            return;
        }

        if (dir === 1 && currentStep === totalSteps) {
            showResult();
            return;
        }

        // Update UI
        currentQ.classList.remove('active');
        currentStep += dir;
        document.querySelector(`.question[data-step="${currentStep}"]`).classList.add('active');
        
        // Update Island and Background
        island.innerText = `STEP ${currentStep} OF ${totalSteps}`;
        bgLayer.className = `bg-layer bg-step-${currentStep}`;
        
        // Update Buttons
        document.getElementById('backBtn').style.visibility = currentStep === 1 ? 'hidden' : 'visible';
        document.getElementById('nextBtn').innerText = currentStep === totalSteps ? 'Get Result' : 'Continue';
    }

    function showResult() {
        document.getElementById('quizForm').style.display = 'none';
        document.getElementById('result').style.display = 'block';
        island.innerText = "MATCH FOUND";
        
        const form = new FormData(document.getElementById('quizForm'));
        const use = form.get('q1');
        const budget = form.get('q4');
        const os = form.get('q6');

        let r = { n: "HP Laptop 15s", p: "₹52,990", s: "Core i5 13th Gen, 16GB, 512GB SSD", c: "15-fd0011TU" };

        if (os === 'macos') {
            r = { n: "Apple MacBook Air M4", p: "₹1,14,900", s: "M4 Chip, 16GB RAM, 512GB SSD", c: "MW1M3HN/A" };
        } else if (use === 'gaming') {
            r = { n: "HP OMEN Gaming", p: "₹94,990", s: "Ryzen 7, RTX 4060, 16GB RAM", c: "16-xf0059AX" };
        } else if (budget === '30k') {
            r = { n: "Lenovo IdeaPad Slim 1", p: "₹34,490", s: "Ryzen 3, 8GB RAM, 512GB SSD", c: "15AMN7" };
        }

        document.getElementById('resName').innerText = r.n;
        document.getElementById('resPrice').innerText = r.p;
        document.getElementById('resSpecs').innerText = r.s;
        document.getElementById('resCode').innerText = r.c;
    }
</script>
</body>
</html>
