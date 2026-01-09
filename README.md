<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Cinematic Laptop AI Assistant</title>
  <style>
    /* ... [Your existing CSS remains the same] ... */
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: Arial, sans-serif;
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #f5f5f5;
      background: radial-gradient(circle at top, #1f2937 0, #020617 55%);
    }
    .bg-layer { position: fixed; inset: 0; z-index: -3; transition: background 0.8s ease; }
    .bg-step-1  { background: radial-gradient(circle at top, #0ea5e9 0, #020617 55%); }
    .bg-step-15 { background: radial-gradient(circle at top, #f59e0b 0, #020617 55%); }
    .overlay-gradient { position: fixed; inset: 0; background: rgba(15,23,42,0.9); z-index: -2; }
    .container { width: 100%; max-width: 820px; padding: 24px; }
    .assistant-card { 
        position: relative; border-radius: 24px; padding: 28px; 
        background: rgba(15, 23, 42, 0.5); border: 1px solid rgba(148, 163, 184, 0.45);
        backdrop-filter: blur(20px);
    }
    .header-row { display: flex; justify-content: space-between; margin-bottom: 20px; }
    .step-indicator { border: 1px solid #38bdf8; padding: 4px 12px; border-radius: 20px; font-size: 0.8rem; }
    .question-wrapper { min-height: 250px; position: relative; }
    .question { display: none; }
    .question.active { display: block; }
    .option-label { 
        display: block; background: rgba(255,255,255,0.05); 
        padding: 12px; margin-bottom: 8px; border-radius: 12px; cursor: pointer; 
        border: 1px solid transparent; transition: 0.3s;
    }
    .option-label:hover { border-color: #38bdf8; background: rgba(56,189,248,0.1); }
    .nav-row { display: flex; justify-content: space-between; margin-top: 20px; }
    button { padding: 10px 20px; border-radius: 20px; border: none; cursor: pointer; font-weight: bold; }
    .btn-primary { background: #38bdf8; color: #020617; }
    .btn-secondary { background: #1f2937; color: white; }
    .hidden { display: none; }
    #result { 
        background: rgba(56, 189, 248, 0.1); padding: 20px; border-radius: 15px; 
        border: 1px solid #38bdf8; margin-top: 20px; 
    }
    .buy-link {
        display: inline-block; background: #22c55e; color: white; 
        padding: 12px 24px; border-radius: 30px; text-decoration: none;
        margin-top: 15px; font-weight: bold;
    }
  </style>
</head>
<body>

<div class="bg-layer bg-step-1" id="bgLayer"></div>
<div class="overlay-gradient"></div>

<div class="container">
  <div class="assistant-card">
    <div class="header-row">
      <div class="title-block">
        <h1>Smart Laptop Helper</h1>
        <p>Finding the exact article from Reliance Digital...</p>
      </div>
      <div class="step-indicator" id="stepIndicator">Step 1 of 15</div>
    </div>

    <form id="quizForm">
      <div class="question-wrapper">
        <div class="question active" data-step="1">
          <h3>1. What will you mostly do on the laptop?</h3>
          <label class="option-label"><input type="radio" name="use" value="basic" required> Simple work</label>
          <label class="option-label"><input type="radio" name="use" value="office"> Office / Coding</label>
          <label class="option-label"><input type="radio" name="use" value="creator"> Editing / Design</label>
          <label class="option-label"><input type="radio" name="use" value="gaming"> Gaming</label>
        </div>
        
        <div class="question" data-step="2"><h3>2. Carry outside?</h3><label class="option-label"><input type="radio" name="carry" value="daily"> Yes</label><label class="option-label"><input type="radio" name="carry" value="no"> No</label></div>
        <div class="question" data-step="3"><h3>3. Battery?</h3><label class="option-label"><input type="radio" name="battery" value="long"> 7+ hrs</label><label class="option-label"><input type="radio" name="battery" value="normal"> Normal</label></div>
        <div class="question" data-step="4"><h3>4. Budget?</h3><label class="option-label"><input type="radio" name="budget" value="low"> Low</label><label class="option-label"><input type="radio" name="budget" value="medium"> Medium</label><label class="option-label"><input type="radio" name="budget" value="high"> High</label></div>
        </div>

      <div class="nav-row">
        <button type="button" class="btn-secondary" id="prevBtn">Back</button>
        <button type="button" class="btn-primary" id="nextBtn">Next</button>
      </div>
    </form>

    <div id="result" class="hidden">
      <h2>Success! We found your match.</h2>
      <p id="summaryText" style="margin: 10px 0; color: #cbd5e1;"></p>
      <div id="productBox" style="background: rgba(0,0,0,0.3); padding: 15px; border-radius: 10px;">
          <h3 id="modelName" style="color: #38bdf8;"></h3>
          <p id="modelSpecs" style="font-size: 0.9rem;"></p>
          <p id="articleCode" style="font-size: 0.8rem; color: #94a3b8; margin-top: 5px;"></p>
      </div>
      <div id="linkContainer"></div>
    </div>
  </div>
</div>

<script>
// MASTER DATA: Map user needs to Reliance Digital Articles
const laptopDatabase = [
  {
    article: "494352115",
    name: "HP Victus Gaming 15",
    specs: "AMD Ryzen 5, 16GB RAM, 512GB SSD, RTX 3050 Graphics",
    tags: { use: "gaming", budget: "medium", carry: "no" }
  },
  {
    article: "494352292",
    name: "Apple MacBook Air M3",
    specs: "Apple M3 Chip, 8GB RAM, 256GB SSD, Liquid Retina Display",
    tags: { use: "creator", budget: "high", carry: "daily" }
  },
  {
    article: "493179294",
    name: "ASUS Vivobook Go 15",
    specs: "Ryzen 3 7320U, 8GB RAM, 512GB SSD, Windows 11",
    tags: { use: "basic", budget: "low", carry: "daily" }
  },
  {
    article: "493838383",
    name: "Lenovo IdeaPad Slim 3",
    specs: "Intel i5 12th Gen, 16GB RAM, 512GB SSD",
    tags: { use: "office", budget: "medium", carry: "daily" }
  }
  // You can expand this to 300+ entries
];

let currentStep = 1;
const totalSteps = 4; // Change to 15 when you add all your questions back

// UI Navigation Logic
document.getElementById('nextBtn').addEventListener('click', () => {
    const activeQ = document.querySelector('.question.active');
    if (!activeQ.querySelector('input:checked')) return alert("Select an option");

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
    document.getElementById('prevBtn').style.display = currentStep === 1 ? 'none' : 'block';
}

function processResults() {
    const formData = new FormData(document.getElementById('quizForm'));
    const userUse = formData.get('use');
    const userBudget = formData.get('budget');
    const userCarry = formData.get('carry');

    let bestMatch = null;
    let topScore = -1;

    // SCORING ENGINE
    laptopDatabase.forEach(laptop => {
        let score = 0;
        if (laptop.tags.use === userUse) score += 10;
        if (laptop.tags.budget === userBudget) score += 7;
        if (laptop.tags.carry === userCarry) score += 3;

        if (score > topScore) {
            topScore = score;
            bestMatch = laptop;
        }
    });

    displayFinalResult(bestMatch);
}

function displayFinalResult(item) {
    document.getElementById('quizForm').classList.add('hidden');
    document.getElementById('result').classList.remove('hidden');
    
    document.getElementById('summaryText').innerText = "Based on your 15-step profile, this laptop offers the best balance of performance and value.";
    document.getElementById('modelName').innerText = item.name;
    document.getElementById('modelSpecs').innerText = item.specs;
    document.getElementById('articleCode').innerText = "Article Code: " + item.article;

    // RELIANCE DIGITAL SEARCH INTEGRATION
    const relianceSearchUrl = `https://www.reliancedigital.in/search?q=${item.article}:relevance`;
    
    document.getElementById('linkContainer').innerHTML = `
        <a href="${relianceSearchUrl}" target="_blank" class="buy-link">
            Check Availability on Reliance Digital
        </a>
    `;
}
</script>
</body>
</html>
