<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Cinematic Laptop AI Assistant 2026</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #f5f5f5;
      background: #020617;
      overflow-x: hidden;
    }
    .bg-layer { position: fixed; inset: 0; z-index: -3; transition: background 1s ease; }
    .bg-step-1  { background: radial-gradient(circle at top, #0ea5e9 0, #020617 70%); }
    .bg-step-2  { background: radial-gradient(circle at top, #8b5cf6 0, #020617 70%); }
    .bg-step-3  { background: radial-gradient(circle at top, #ec4899 0, #020617 70%); }
    .bg-step-4  { background: radial-gradient(circle at top, #f59e0b 0, #020617 70%); }
    .bg-step-5  { background: radial-gradient(circle at top, #10b981 0, #020617 70%); }
    .bg-step-6  { background: radial-gradient(circle at top, #6366f1 0, #020617 70%); }
    .bg-step-7  { background: radial-gradient(circle at top, #f43f5e 0, #020617 70%); }
    .bg-step-8  { background: radial-gradient(circle at top, #06b6d4 0, #020617 70%); }
    
    .overlay-gradient { position: fixed; inset: 0; background: rgba(15,23,42,0.85); z-index: -2; }
    .container { width: 100%; max-width: 820px; padding: 20px; z-index: 1; }
    
    .assistant-card { 
        border-radius: 24px; padding: 35px; 
        background: rgba(30, 41, 59, 0.4); border: 1px solid rgba(255, 255, 255, 0.1);
        backdrop-filter: blur(25px); box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
    }
    .header-row { display: flex; justify-content: space-between; align-items: center; margin-bottom: 30px; border-bottom: 1px solid rgba(255,255,255,0.1); padding-bottom: 15px; }
    .step-indicator { background: rgba(56, 189, 248, 0.2); color: #38bdf8; padding: 6px 16px; border-radius: 20px; font-size: 0.85rem; font-weight: bold; border: 1px solid rgba(56, 189, 248, 0.3); }
    
    .question-wrapper { min-height: 280px; }
    .question { display: none; animation: fadeIn 0.5s ease; }
    .question.active { display: block; }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

    h3 { margin-bottom: 20px; font-size: 1.4rem; color: #f8fafc; }
    .option-label { 
        display: flex; align-items: center; background: rgba(255,255,255,0.03); 
        padding: 16px; margin-bottom: 12px; border-radius: 14px; cursor: pointer; 
        border: 1px solid rgba(255,255,255,0.05); transition: all 0.2s;
    }
    .option-label:hover { background: rgba(56, 189, 248, 0.1); border-color: #38bdf8; }
    input[type="radio"] { margin-right: 15px; accent-color: #38bdf8; transform: scale(1.2); }

    .nav-row { display: flex; justify-content: space-between; margin-top: 30px; }
    button { padding: 12px 28px; border-radius: 12px; border: none; cursor: pointer; font-weight: 600; transition: 0.3s; }
    .btn-primary { background: #38bdf8; color: #0f172a; box-shadow: 0 4px 14px 0 rgba(56, 189, 248, 0.39); }
    .btn-primary:hover { background: #0ea5e9; transform: translateY(-2px); }
    .btn-secondary { background: transparent; color: #94a3b8; border: 1px solid #475569; }
    .btn-secondary:hover { color: white; border-color: white; }

    .hidden { display: none; }
    #result { animation: slideUp 0.6s ease; }
    @keyframes slideUp { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }

    .product-card { 
        background: linear-gradient(145deg, rgba(30, 41, 59, 0.6), rgba(15, 23, 42, 0.8));
        padding: 25px; border-radius: 20px; border: 1px solid #38bdf8; margin-top: 20px; 
    }
    .alt-card { border-color: #94a3b8; opacity: 0.9; margin-top: 15px; padding: 15px; font-size: 0.9rem; }
    
    .buy-link {
        display: inline-block; background: #22c55e; color: white; 
        padding: 14px 28px; border-radius: 12px; text-decoration: none;
        margin-top: 20px; font-weight: bold; text-align: center; width: 100%;
        transition: 0.3s;
    }
    .buy-link:hover { background: #16a34a; box-shadow: 0 10px 15px -3px rgba(34, 197, 94, 0.4); }
  </style>
</head>
<body>

<div class="bg-layer bg-step-1" id="bgLayer"></div>
<div class="overlay-gradient"></div>

<div class="container">
  <div class="assistant-card">
    <div class="header-row">
      <div class="title-block">
        <h1 style="font-size: 1.6rem; color: #38bdf8;">Laptop AI Finder</h1>
        <p style="font-size: 0.9rem; color: #94a3b8;">Precision Article Match (2026 Database)</p>
      </div>
      <div class="step-indicator" id="stepIndicator">Step 1 of 8</div>
    </div>

    <form id="quizForm">
      <div class="question-wrapper">
        <div class="question active" data-step="1">
          <h3>What will be your primary task?</h3>
          <label class="option-label"><input type="radio" name="use" value="basic" required> Student / Basic Browsing</label>
          <label class="option-label"><input type="radio" name="use" value="office"> Professional / Office Work</label>
          <label class="option-label"><input type="radio" name="use" value="creator"> Creative (Video/Photo Editing)</label>
          <label class="option-label"><input type="radio" name="use" value="gaming"> High-End Gaming</label>
        </div>
        
        <div class="question" data-step="2">
          <h3>Will you carry it daily?</h3>
          <label class="option-label"><input type="radio" name="carry" value="daily"> Yes, lightweight is a priority</label>
          <label class="option-label"><input type="radio" name="carry" value="no"> No, it will mostly stay on a desk</label>
        </div>

        <div class="question" data-step="3">
          <h3>Expected Battery Life?</h3>
          <label class="option-label"><input type="radio" name="battery" value="long"> Long (9+ hours / All Day)</label>
          <label class="option-label"><input type="radio" name="battery" value="normal"> Normal (4-6 hours)</label>
        </div>

        <div class="question" data-step="4">
          <h3>Set your budget range:</h3>
          <label class="option-label"><input type="radio" name="budget" value="low"> Budget (Under ₹40,000)</label>
          <label class="option-label"><input type="radio" name="budget" value="medium"> Mid-Range (₹40,000 - ₹75,000)</label>
          <label class="option-label"><input type="radio" name="budget" value="high"> Premium (Above ₹75,000)</label>
        </div>

        <div class="question" data-step="5">
            <h3>Preferred Brand?</h3>
            <label class="option-label"><input type="radio" name="brand" value="hp"> HP (Reliable Service)</label>
            <label class="option-label"><input type="radio" name="brand" value="asus"> ASUS (Innovation / OLED)</label>
            <label class="option-label"><input type="radio" name="brand" value="apple"> Apple (macOS / M3)</label>
            <label class="option-label"><input type="radio" name="brand" value="any"> No Preference</label>
        </div>

        <div class="question" data-step="6">
            <h3>Do you need a Touchscreen or Convertible?</h3>
            <label class="option-label"><input type="radio" name="touch" value="yes"> Yes (360° Tablet Mode / Touch)</label>
            <label class="option-label"><input type="radio" name="touch" value="no"> No (Standard Laptop)</label>
        </div>

        <div class="question" data-step="7">
            <h3>Is a Backlit Keyboard necessary?</h3>
            <label class="option-label"><input type="radio" name="backlight" value="yes"> Yes, I work in low light</label>
            <label class="option-label"><input type="radio" name="backlight" value="no"> No, not a priority</label>
        </div>

        <div class="question" data-step="8">
            <h3>Screen Type Preference?</h3>
            <label class="option-label"><input type="radio" name="screen" value="oled"> OLED (Vibrant Colors / Deep Blacks)</label>
            <label class="option-label"><input type="radio" name="screen" value="ips"> IPS / Anti-Glare (Standard)</label>
        </div>
      </div>

      <div class="nav-row">
        <button type="button" class="btn-secondary" id="prevBtn" style="visibility: hidden;">Back</button>
        <button type="button" class="btn-primary" id="nextBtn">Next Question</button>
      </div>
    </form>

    <div id="result" class="hidden">
      <h2 style="color: #22c55e;">Analysis Complete!</h2>
      <p id="summaryText" style="margin: 10px 0; color: #cbd5e1;"></p>
      
      <div class="product-card">
          <span style="color: #38bdf8; font-weight: bold; font-size: 0.8rem; text-transform: uppercase;">Top Recommendation</span>
          <h3 id="modelName" style="margin: 10px 0 5px 0;"></h3>
          <p id="modelSpecs" style="color: #94a3b8; font-size: 0.95rem; line-height: 1.4;"></p>
          <p id="articleCode" style="font-family: monospace; color: #38bdf8; margin-top: 10px;"></p>
          <div id="linkContainer"></div>
      </div>

      <div id="altMatch" class="product-card alt-card hidden">
          <span style="color: #94a3b8; font-weight: bold; font-size: 0.75rem;">Great Alternative</span>
          <h4 id="altModelName" style="margin: 5px 0;"></h4>
          <p id="altArticle" style="font-size: 0.8rem; color: #64748b;"></p>
      </div>
    </div>
  </div>
</div>

<script>
// UPDATED DATABASE WITH NEW TAGS
const laptopDatabase = [
  { article: "494352115", name: "HP Victus 15-fb Series", specs: "Ryzen 5, 16GB, RTX 3050. Non-touch, Backlit.", tags: { use: "gaming", budget: "medium", carry: "no", brand: "hp", touch: "no", backlight: "yes", screen: "ips" } },
  { article: "493179294", name: "ASUS Vivobook Go 15", specs: "Ryzen 3, 8GB. Basic use, IPS Screen.", tags: { use: "basic", budget: "low", carry: "daily", brand: "asus", touch: "no", backlight: "no", screen: "ips" } },
  { article: "493838383", name: "Lenovo IdeaPad Slim 3", specs: "i5 12th Gen, 16GB. Solid office workhorse.", tags: { use: "office", budget: "medium", carry: "daily", brand: "any", touch: "no", backlight: "yes", screen: "ips" } },
  { article: "494421456", name: "Apple MacBook Air M3", specs: "M3 Chip, 16GB. Liquid Retina, No Touch.", tags: { use: "creator", budget: "high", carry: "daily", brand: "apple", touch: "no", backlight: "yes", screen: "ips" } },
  { article: "493712555", name: "HP Pavilion x360", specs: "i5 13th Gen, Touchscreen, Convertible 2-in-1.", tags: { use: "office", budget: "high", carry: "daily", brand: "hp", touch: "yes", backlight: "yes", screen: "ips" } },
  { article: "494399102", name: "ASUS Zenbook 14 OLED", specs: "Ultra 5, 16GB, Stunning OLED Display.", tags: { use: "creator", budget: "high", carry: "daily", brand: "asus", touch: "no", backlight: "yes", screen: "oled" } },
  { article: "492166443", name: "ASUS Vivobook 16X", specs: "Ryzen 5, OLED screen, Basic Backlight.", tags: { use: "basic", budget: "medium", carry: "no", brand: "asus", touch: "no", backlight: "yes", screen: "oled" } }
];

let currentStep = 1;
const totalSteps = 8;

document.getElementById('nextBtn').addEventListener('click', () => {
    const activeQ = document.querySelector('.question.active');
    const checked = activeQ.querySelector('input:checked');
    
    if (!checked) {
        alert("Please select an option to continue.");
        return;
    }

    if (currentStep < totalSteps) {
        currentStep++;
        updateUI();
    } else {
        processResults();
    }
});

document.getElementById('prevBtn').addEventListener('click', () => {
    if (currentStep > 1) {
        currentStep--;
        updateUI();
    }
});

function updateUI() {
    document.querySelectorAll('.question').forEach(q => q.classList.remove('active'));
    document.querySelector(`.question[data-step="${currentStep}"]`).classList.add('active');
    
    document.getElementById('stepIndicator').innerText = `Step ${currentStep} of ${totalSteps}`;
    document.getElementById('bgLayer').className = `bg-layer bg-step-${currentStep}`;
    document.getElementById('prevBtn').style.visibility = currentStep === 1 ? 'hidden' : 'visible';
    document.getElementById('nextBtn').innerText = currentStep === totalSteps ? 'See Results' : 'Next Question';
}

function processResults() {
    const formData = new FormData(document.getElementById('quizForm'));
    const user = {
        use: formData.get('use'),
        budget: formData.get('budget'),
        carry: formData.get('carry'),
        brand: formData.get('brand'),
        touch: formData.get('touch'),
        backlight: formData.get('backlight'),
        screen: formData.get('screen')
    };

    let scoredList = laptopDatabase.map(laptop => {
        let score = 0;
        if (laptop.tags.use === user.use) score += 10;
        if (laptop.tags.budget === user.budget) score += 8;
        if (laptop.tags.carry === user.carry) score += 5;
        if (user.brand !== "any" && laptop.tags.brand === user.brand) score += 12;
        if (laptop.tags.touch === user.touch) score += 15; // High weight for touch
        if (laptop.tags.backlight === user.backlight) score += 5;
        if (laptop.tags.screen === user.screen) score += 7;
        
        return { ...laptop, totalScore: score };
    }).sort((a, b) => b.totalScore - a.totalScore);

    displayFinalResult(scoredList[0], scoredList[1]);
}

function displayFinalResult(best, alt) {
    document.getElementById('quizForm').classList.add('hidden');
    document.getElementById('result').classList.remove('hidden');
    document.getElementById('stepIndicator').classList.add('hidden');
    
    document.getElementById('summaryText').innerText = `We found a ${best.totalScore > 35 ? 'Perfect' : 'Strong'} match for your requirements.`;
    document.getElementById('modelName').innerText = best.name;
    document.getElementById('modelSpecs').innerText = best.specs;
    document.getElementById('articleCode').innerText = "Reliance Article: " + best.article;

    const searchUrl = `https://www.reliancedigital.in/search?q=${best.article}:relevance`;
    
    document.getElementById('linkContainer').innerHTML = `
        <a href="${searchUrl}" target="_blank" class="buy-link">
            Buy on Reliance Digital →
        </a>
    `;

    if (alt && alt.totalScore > 20) {
        document.getElementById('altMatch').classList.remove('hidden');
        document.getElementById('altModelName').innerText = alt.name;
        document.getElementById('altArticle').innerText = "Alternative Article: " + alt.article;
    }
}
</script>
</body>
</html>
