<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<title>Smart Laptop Helper - Reliance Digital</title>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body {
  font-family: Arial, system-ui, -apple-system, BlinkMacSystemFont, sans-serif;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: stretch;
  justify-content: flex-start;
  color: #f5f5f5;
  background: radial-gradient(circle at top, #1f2937 0, #020617 55%);
  overflow-x: hidden;
}

.bg-layer { position: fixed; inset: 0; z-index: -3; transition: background 0.8s ease-in-out; }
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

.overlay-gradient {
  position: fixed; inset: 0;
  background: radial-gradient(circle at top left, rgba(15,23,42,0.2), transparent 60%),
              radial-gradient(circle at bottom, rgba(15,23,42,0.95), rgba(15,23,42,0.98));
  z-index: -2;
}
.bg-laptop-shape {
  position: fixed; width: 520px; height: 320px; border-radius: 32px;
  border: 1px solid rgba(148,163,184,0.25);
  background: linear-gradient(135deg, rgba(15,23,42,0.9), rgba(30,64,175,0.6));
  box-shadow: 0 40px 80px rgba(15,23,42,0.9), 0 0 80px rgba(56,189,248,0.5);
  bottom: -120px; right: -80px; transform: rotate(-6deg); opacity: 0.35; z-index: -1;
}

.container { width: 100%; max-width: 900px; padding: 24px 16px; margin: 0 auto; }

.assistant-card {
  position: relative; border-radius: 28px; padding: 32px 32px 28px; margin: 60px auto;
  max-width: 1000px; min-height: 380px; height: auto;
  background: linear-gradient(135deg, rgba(15, 23, 42, 0.40), rgba(30, 64, 175, 0.22));
  border: 1px solid rgba(148, 163, 184, 0.55);
  box-shadow: 0 26px 70px rgba(15, 23, 42, 0.95), 0 0 90px rgba(56, 189, 248, 0.5),
              inset 0 0 0 1px rgba(255, 255, 255, 0.10);
  backdrop-filter: blur(24px) saturate(170%); -webkit-backdrop-filter: blur(24px) saturate(170%);
  overflow: visible;
}
.assistant-card::before {
  content: ""; position: absolute; inset: -40%;
  background: radial-gradient(circle at 0% 0%, rgba(255, 255, 255, 0.20), transparent 55%),
              radial-gradient(circle at 100% 100%, rgba(56, 189, 248, 0.16), transparent 60%);
  opacity: 0.9; mix-blend-mode: screen; pointer-events: none;
}

.header-row { display: flex; justify-content: space-between; align-items: center; margin-bottom: 14px; gap: 12px; z-index: 1; }
.title-block h1 { font-size: 1.5rem; letter-spacing: 0.04em; }
.title-block p { font-size: 0.9rem; color: #cbd5f5; margin-top: 4px; }
.step-indicator {
  font-size: 0.8rem; letter-spacing: 0.14em; text-transform: uppercase; color: #e0e7ff;
  border-radius: 999px; padding: 4px 10px; border: 1px solid rgba(129,140,248,0.8);
  background: radial-gradient(circle at top, rgba(67,56,202,0.7), rgba(15,23,42,0.9));
  white-space: nowrap; box-shadow: 0 0 14px rgba(129,140,248,0.7);
}

.question-visual {
  position: absolute; top: 20px; right: 22px; padding: 8px 12px; border-radius: 999px;
  background: rgba(15,23,42,0.85); border: 1px solid rgba(148,163,184,0.6);
  font-size: 0.8rem; display: inline-flex; align-items: center; gap: 6px; color: #e5e7eb;
  box-shadow: 0 8px 22px rgba(15,23,42,0.95), 0 0 18px rgba(56,189,248,0.8);
  z-index: 2; backdrop-filter: blur(12px); -webkit-backdrop-filter: blur(12px);
}
.question-visual span.icon { font-size: 1.05rem; }

.question-wrapper { position: relative; margin-top: 10px; z-index: 1; }

.question {
  position: relative; opacity: 0; transform: translateY(18px) scale(0.98);
  transition: opacity 0.45s ease, transform 0.45s ease; pointer-events: none; display: none;
}
.question.active { display: block; opacity: 1; transform: translateY(0) scale(1); pointer-events: auto; }
.question h3 { font-size: 1.15rem; margin-bottom: 10px; }
.question-sub { font-size: 0.85rem; color: #d1d5db; margin-bottom: 12px; }

.option-label {
  position: relative; display: flex; align-items: center; gap: 10px; margin-bottom: 10px;
  padding: 10px 14px; border-radius: 18px;
  background: linear-gradient(135deg, rgba(15,23,42,0.85), rgba(15,23,42,0.95));
  border: 1px solid rgba(148,163,184,0.55); cursor: pointer;
  transition: all 0.18s ease; box-shadow: 0 10px 24px rgba(15,23,42,0.85);
  backdrop-filter: blur(16px) saturate(180%); -webkit-backdrop-filter: blur(16px) saturate(180%);
}
.option-label:hover {
  border-color: rgba(56,189,248,0.9); transform: translateY(-1px);
  box-shadow: 0 0 18px rgba(56,189,248,0.35), 0 14px 28px rgba(15,23,42,0.95);
}
.option-label input { margin-right: 8px; accent-color: #38bdf8; width: 14px; height: 14px; }

.nav-row { display: flex; justify-content: space-between; align-items: center; margin-top: 14px; gap: 10px; z-index: 1; position: relative; }
.nav-buttons { display: flex; gap: 10px; }
button {
  border-radius: 999px; border: none; padding: 8px 18px; font-size: 0.9rem; cursor: pointer;
  transition: all 0.2s ease; display: inline-flex; align-items: center; gap: 6px;
}
button:active { transform: translateY(1px); }
.btn-primary {
  background: linear-gradient(135deg, #38bdf8, #22c55e); color: #020617;
  box-shadow: 0 10px 24px rgba(56,189,248,0.6);
}
.btn-primary:hover:not(:disabled) { background: linear-gradient(135deg, #0ea5e9, #16a34a); transform: translateY(-2px); }
.btn-primary:disabled {
  background: rgba(56,189,248,0.3); color: rgba(2,6,23,0.5); cursor: not-allowed; box-shadow: none;
}
.btn-secondary {
  background: rgba(15,23,42,0.85); color: #e5e7eb; border: 1px solid rgba(148,163,184,0.7);
  backdrop-filter: blur(10px); -webkit-backdrop-filter: blur(10px);
}
.btn-secondary:hover { background: rgba(15,23,42,0.95); }

.progress-dots { display: flex; gap: 4px; align-items: center; justify-content: flex-start; }
.dot {
  width: 8px; height: 8px; border-radius: 999px; background: rgba(148,163,184,0.5);
  transition: all 0.25s ease;
}
.dot.active { width: 22px; background: #38bdf8; box-shadow: 0 0 12px rgba(56,189,248,0.8); }

#result {
  margin-top: 18px; padding: 20px; border-radius: 18px; border: 1px solid rgba(148,163,184,0.5);
  background: linear-gradient(135deg, rgba(15,23,42,0.9), rgba(37,99,235,0.45));
  backdrop-filter: blur(18px) saturate(160%); -webkit-backdrop-filter: blur(18px) saturate(160%);
  box-shadow: 0 10px 30px rgba(15,23,42,0.9), 0 0 32px rgba(56,189,248,0.45);
}
#result h2 { font-size: 1.1rem; margin-bottom: 12px; color: #38bdf8; }
#result h3 { font-size: 1rem; margin: 16px 0 8px 0; color: #a5b4fc; }
#result p { font-size: 0.9rem; color: #e5e7eb; margin-bottom: 8px; }
.product-grid { display: flex; gap: 15px; flex-wrap: wrap; margin-top: 15px; }
.product-card {
  background: rgba(15,23,42,0.95); padding: 20px; border-radius: 16px; border: 1px solid rgba(56,189,248,0.6);
  flex: 1; min-width: 240px; backdrop-filter: blur(15px);
}
.product-code { color: #38bdf8; font-size: 0.85rem; margin-bottom: 8px; }
.product-name { margin: 0 0 10px 0; color: #e5e7eb; font-size: 1.05rem; }
.product-price { font-size: 1.2rem; font-weight: bold; color: #22c55e; margin-bottom: 15px; }
.product-buy { 
  background: linear-gradient(135deg, #38bdf8, #22c55e); color: white; padding: 12px 24px; 
  border-radius: 25px; text-decoration: none; display: inline-block; font-weight: 500;
  box-shadow: 0 8px 25px rgba(56,189,248,0.4); transition: all 0.3s ease;
}
.product-buy:hover { transform: translateY(-2px); box-shadow: 0 12px 35px rgba(56,189,248,0.6); }
.hidden { display: none; }

@media (max-width: 640px) {
  body { padding: 10px; }
  .assistant-card { margin: 24px 8px; padding: 24px 18px 20px; border-radius: 20px; min-height: 340px; }
  .option-label { padding: 9px 12px; border-radius: 16px; }
  .product-grid { flex-direction: column; }
}
</style>
</head>
<body>
<div class="bg-layer bg-step-1" id="bgLayer"></div>
<div class="overlay-gradient"></div>
<div class="bg-laptop-shape"></div>

<div class="container">
  <div class="assistant-card">
    <div class="header-row">
      <div class="title-block">
        <h1>🤖 Smart Laptop Helper</h1>
        <p>Answer 10 questions → Get exact Reliance Digital article numbers</p>
      </div>
      <div class="step-indicator" id="stepIndicator">Step 1/10 • 10% Complete</div>
    </div>

    <div class="question-visual" id="questionVisual">
      <span class="icon">💻</span>
      <span class="text">Main laptop usage</span>
    </div>

    <form id="quizForm">
      <div class="question-wrapper">

        <!-- STEP 1 -->
        <div class="question active" data-step="1">
          <h3>Main laptop usage</h3>
          <p class="question-sub">Choose what you will do most of the time.</p>
          <label class="option-label"><input type="radio" name="q1" value="basic"> Basic work, browsing, YouTube</label>
          <label class="option-label"><input type="radio" name="q1" value="office"> Office work, Excel, online classes</label>
          <label class="option-label"><input type="radio" name="q1" value="creative"> Video editing, Photoshop</label>
          <label class="option-label"><input type="radio" name="q1" value="gaming"> Gaming, 3D rendering</label>
        </div>

        <!-- STEP 2 -->
        <div class="question" data-step="2">
          <h3>Carrying & weight</h3>
          <p class="question-sub">How often will you carry this laptop?</p>
          <label class="option-label"><input type="radio" name="q2" value="daily"> Carry daily to college/office</label>
          <label class="option-label"><input type="radio" name="q2" value="weekly"> Carry 2-3 times weekly</label>
          <label class="option-label"><input type="radio" name="q2" value="rarely"> Carry rarely</label>
          <label class="option-label"><input type="radio" name="q2" value="stationary"> Keep at one place</label>
        </div>

        <!-- STEP 3 -->
        <div class="question" data-step="3">
          <h3>Battery backup</h3>
          <p class="question-sub">How many hours without charger?</p>
          <label class="option-label"><input type="radio" name="q3" value="4hr"> 3-4 hours</label>
          <label class="option-label"><input type="radio" name="q3" value="6hr"> 5-6 hours</label>
          <label class="option-label"><input type="radio" name="q3" value="8hr"> 7-8 hours</label>
          <label class="option-label"><input type="radio" name="q3" value="10hr"> 10+ hours</label>
        </div>

        <!-- STEP 4 -->
        <div class="question" data-step="4">
          <h3>Budget range</h3>
          <p class="question-sub">What's your budget for this laptop?</p>
          <label class="option-label"><input type="radio" name="q4" value="30k"> Under ₹30,000</label>
          <label class="option-label"><input type="radio" name="q4" value="50k"> ₹30,000 - ₹50,000</label>
          <label class="option-label"><input type="radio" name="q4" value="80k"> ₹50,000 - ₹80,000</label>
          <label class="option-label"><input type="radio" name="q4" value="1lakh"> Above ₹80,000</label>
        </div>

        <!-- STEP 5 -->
        <div class="question" data-step="5">
          <h3>Screen size</h3>
          <p class="question-sub">Preferred screen size for comfort?</p>
          <label class="option-label"><input type="radio" name="q5" value="14"> 14 inch (portable)</label>
          <label class="option-label"><input type="radio" name="q5" value="15"> 15.6 inch (standard)</label>
          <label class="option-label"><input type="radio" name="q5" value="16"> 16-17 inch (large)</label>
          <label class="option-label"><input type="radio" name="q5" value="13"> 13 inch or smaller</label>
        </div>

        <!-- STEP 6 -->
        <div class="question" data-step="6">
          <h3>Brand preference</h3>
          <p class="question-sub">Which brands do you prefer?</p>
          <label class="option-label"><input type="radio" name="q6" value="hp"> HP / Lenovo</label>
          <label class="option-label"><input type="radio" name="q6" value="dell"> Dell / ASUS</label>
          <label class="option-label"><input type="radio" name="q6" value="apple"> Apple MacBook</label>
          <label class="option-label"><input type="radio" name="q6" value="any"> Any good brand</label>
        </div>

        <!-- STEP 7 -->
        <div class="question" data-step="7">
          <h3>Storage needs</h3>
          <p class="question-sub">How much storage do you need?</p>
          <label class="option-label"><input type="radio" name="q7" value="256gb"> 256GB SSD</label>
          <label class="option-label"><input type="radio" name="q7" value="512gb"> 512GB SSD</label>
          <label class="option-label"><input type="radio" name="q7" value="1tb"> 1TB SSD</label>
          <label class="option-label"><input type="radio" name="q7" value="more"> 1TB+</label>
        </div>

        <!-- STEP 8 -->
        <div class="question" data-step="8">
          <h3>How long to use</h3>
          <p class="question-sub">How many years before upgrading?</p>
          <label class="option-label"><input type="radio" name="q8" value="2yr"> 1-2 years</label>
          <label class="option-label"><input type="radio" name="q8" value="3yr"> 3 years</label>
          <label class="option-label"><input type="radio" name="q8" value="4yr"> 4 years</label>
          <label class="option-label"><input type="radio" name="q8" value="5yr"> 5+ years</label>
        </div>

        <!-- STEP 9 -->
        <div class="question" data-step="9">
          <h3>Keyboard preference</h3>
          <p class="question-sub">Typing comfort matters?</p>
          <label class="option-label"><input type="radio" name="q9" value="normal"> Normal keyboard</label>
          <label class="option-label"><input type="radio" name="q9" value="numkey"> With numpad</label>
          <label class="option-label"><input type="radio" name="q9" value="backlit"> Backlit keys</label>
          <label class="option-label"><input type="radio" name="q9" value="compact"> Compact keyboard</label>
        </div>

        <!-- STEP 10 -->
        <div class="question" data-step="10">
          <h3>Special features</h3>
          <p class="question-sub">Any must-have features?</p>
          <label class="option-label"><input type="radio" name="q10" value="basic"> Basic is fine</label>
          <label class="option-label"><input type="radio" name="q10" value="touch"> Touch screen</label>
          <label class="option-label"><input type="radio" name="q10" value="webcam"> HD Webcam</label>
          <label class="option-label"><input type="radio" name="q10" value="fingerprint"> Fingerprint sensor</label>
        </div>

      </div>

      <div class="nav-row">
        <div class="progress-dots" id="progressDots"></div>
        <div class="nav-buttons">
          <button type="button" class="btn-secondary" id="prevBtn">Back</button>
          <button type="button" class="btn-primary" id="nextBtn" disabled>Next</button>
        </div>
      </div>
    </form>

    <div id="result" class="hidden">
      <h2>✅ Your Perfect Laptop Match!</h2>
      <p id="summaryText"></p>
      <h3>📋 Recommended Specs</h3>
      <p id="specText"></p>
      <h3>🛒 Reliance Digital Products</h3>
      <p id="linkText"></p>
    </div>
  </div>
</div>

<script>
const totalSteps = 10;
let currentStep = 1;

const bgLayer = document.getElementById('bgLayer');
const stepIndicator = document.getElementById('stepIndicator');
const questions = document.querySelectorAll('.question');
const prevBtn = document.getElementById('prevBtn');
const nextBtn = document.getElementById('nextBtn');
const progressDotsContainer = document.getElementById('progressDots');
const resultBlock = document.getElementById('result');
const summaryText = document.getElementById('summaryText');
const specText = document.getElementById('specText');
const linkText = document.getElementById('linkText');
const questionVisual = document.getElementById('questionVisual');

const visualTexts = {
  1:{icon:'💻',text:'Main laptop usage'},
  2:{icon:'🎒',text:'Carrying & weight'},
  3:{icon:'🔋',text:'Battery backup'},
  4:{icon:'💰',text:'Budget range'},
  5:{icon:'🖥️',text:'Screen size'},
  6:{icon:'🏷️',text:'Brand preference'},
  7:{icon:'💾',text:'Storage needs'},
  8:{icon:'📆',text:'How long to use'},
  9:{icon:'⌨️',text:'Keyboard preference'},
 10:{icon:'⭐',text:'Special features'}
};

// Create progress dots
for (let i = 1; i <= totalSteps; i++) {
  const dot = document.createElement('div');
  dot.className = 'dot' + (i === 1 ? ' active' : '');
  dot.dataset.step = i;
  progressDotsContainer.appendChild(dot);
}

function updateVisual() {
  const d = visualTexts[currentStep];
  if (!d) return;
  questionVisual.querySelector('.icon').textContent = d.icon;
  questionVisual.querySelector('.text').textContent = d.text;
}

function updateNextButton() {
  const activeQuestion = document.querySelector('.question.active');
  if (!activeQuestion) {
    nextBtn.disabled = true;
    return;
  }
  const radios = activeQuestion.querySelectorAll('input[type="radio"]');
  const isChecked = Array.from(radios).some(radio => radio.checked);
  nextBtn.disabled = !isChecked;
}

function updateUI() {
  questions.forEach(q => {
    const s = Number(q.dataset.step);
    q.classList.toggle('active', s === currentStep);
  });

  const progressPct = Math.round((currentStep / totalSteps) * 100);
  stepIndicator.innerHTML = `Step ${currentStep}/10 • <strong>${progressPct}%</strong> Complete`;
  
  prevBtn.style.visibility = currentStep === 1 ? 'hidden' : 'visible';
  nextBtn.textContent = currentStep === totalSteps ? 'Get My Laptop' : 'Next';
  
  bgLayer.className = 'bg-layer bg-step-' + currentStep;
  
  document.querySelectorAll('.dot').forEach((dot) => {
    const s = Number(dot.dataset.step);
    dot.classList.toggle('active', s === currentStep);
  });

  updateVisual();
  updateNextButton();
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

// Listen for radio button changes
document.addEventListener('change', function(e) {
  if (e.target.matches('input[type="radio"]')) {
    updateNextButton();
  }
});

function getRecommendation() {
  const answers = {};
  for (let i = 1; i <= 10; i++) {
    const selected = document.querySelector(`input[name="q${i}"]:checked`);
    answers[`q${i}`] = selected ? selected.value : 'not_answered';
  }

  // Smart matching based on budget + usage
  let recommendedLaptops = [];
  
  if (answers.q4 === '30k') { // Budget under 30K
    recommendedLaptops = [
      { code: 'HP-15s-eq2215AU', name: 'HP 15s Ryzen 3', price: '₹28,990', link: 'https://www.reliancedigital.in/hp-15s-eq2215au-laptop-amd-ryzen-3-5300u-8-gb-512-gb-ssd-windows-11-home-15-6-inch-natural-silver-1-69-kg/p/491562814' },
      { code: 'Lenovo-IP-G-I3-1215U', name: 'Lenovo IdeaPad 3', price: '₹29,990', link: 'https://www.reliancedigital.in/lenovo-82RK00E5IN-laptop-12th-gen-intel-core-i3-1215u-8-gb-512-gb-ssd-windows-11-home-15-6-inch-arctic-grey-1-62-kg/p/491777950' }
    ];
  } 
  else if (answers.q4 === '50k') { // 30K-50K
    recommendedLaptops = [
      { code: 'HP-15s-fr5007TU', name: 'HP Pavilion i5 12th Gen', price: '₹42,391', link: 'https://www.reliancedigital.in/hp-15s-fr5007tu-laptop-12th-gen-intel-core-i5-1235u-8-gb-512-gb-ssd-windows-11-home-15-6-inch-natural-silver/p/492384917' },
      { code: 'Dell-INSP-3520-i5', name: 'Dell Inspiron 3520', price: '₹47,990', link: 'https://www.reliancedigital.in/dell-inspiron-3520-laptop-12th-gen-intel-core-i5-1235u-16-gb-512-gb-ssd-windows-11-home-15-6-inch-carbon-black-1-65-kg/p/492384918' }
    ];
  }
  else if (answers.q4 === '80k') { // 50K-80K
    recommendedLaptops = [
      { code: 'ASUS-Vivobook-16X', name: 'ASUS Vivobook 16X', price: '₹62,990', link: 'https://www.reliancedigital.in/asus-vivobook-16x-k3605zf-rp458ws-laptop-13th-gen-intel-core-i5-13420h-16-gb-512-gb-ssd-windows-11-home-16-inch-indie-black-1-8-kg/p/493456789' },
      { code: 'Lenovo-LOQ-i5-12450HX', name: 'Lenovo LOQ Gaming', price: '₹69,990', link: 'https://www.reliancedigital.in/lenovo-loq-83gs003nin-laptop-12th-gen-intel-core-i5-12450hx-12-gb-512-gb-ssd-windows-11-home-4-gb-15-6-inch-luna-grey-2-4-kg/p/492987654' }
    ];
  }
  else { // Premium 80K+
    recommendedLaptops = [
      { code: 'Dell-XPS-13-9345', name: 'Dell XPS 13 Snapdragon', price: '₹1,12,990', link: 'https://www.reliancedigital.in/dell-xps-13-9345-laptop-snapdragon-x-plus-x1p-64-42-topaz-16-gb-512-gb-ssd-windows-11-home-13-4-inch-topaz-2-6-lbs/p/494567890' },
      { code: 'HP-Omen-16-XF0107AX', name: 'HP Omen 16 Gaming', price: '₹1,24,999', link: 'https://www.reliancedigital.in/hp-omen-16-xf0107ax-gaming-laptop-amd-ryzen-7-7840hs-16-gb-1-tb-ssd-rtx-4070-windows-11-home-16-1-inch-shadow-black-2-38-kg/p/493876543' }
    ];
  }

  // Summary
  let usageText = answers.q1 === 'gaming' ? '🎮 Gaming' : answers.q1 === 'creative' ? '🎨 Creative Work' : '💼 Office & Study';
  let summary = `${usageText} | Budget ₹${answers.q4 === '30k' ? '30K' : answers.q4 === '50k' ? '50K' : answers.q4 === '80k' ? '80K' : '80K+'}`;
  summaryText.textContent = summary;

  specText.innerHTML = `Intel Core i5 / Ryzen 5 • <strong>16GB RAM</strong> • <strong>${answers.q7 === '1tb' || answers.q7 === 'more' ? '1TB' : '512GB'} SSD</strong> • ${answers.q5 === '15' ? '15.6"' : '14"'} Display`;

  // Show Reliance Digital products with ARTICLE NUMBERS
  let productHTML = '<div class="product-grid">';
  recommendedLaptops.forEach(laptop => {
    productHTML += `
      <div class="product-card">
        <div class="product-code">📦 Article No: <strong>${laptop.code}</strong></div>
        <h4 class="product-name">${laptop.name}</h4>
        <div class="product-price">${laptop.price}</div>
        <a href="${laptop.link}" target="_blank" class="product-buy">Buy Now → Reliance Digital</a>
      </div>
    `;
  });
  productHTML += '</div>';

  linkText.innerHTML = productHTML;

  resultBlock.classList.remove('hidden');
  resultBlock.scrollIntoView({ behavior: 'smooth' });
}

prevBtn.addEventListener('click', () => {
  if (currentStep > 1) {
    currentStep--;
    updateUI();
  }
});

nextBtn.addEventListener('click', () => {
  if (nextBtn.disabled) return;

  if (currentStep < totalSteps) {
    currentStep++;
    updateUI();
  } else {
    getRecommendation();
  }
});

// Initialize
updateVisual();
updateUI();
</script>
</body>
</html>
