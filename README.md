<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<title>Smart Laptop Helper</title>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body {
  font-family: Arial, system-ui, -apple-system, BlinkMacSystemFont, sans-serif;
  min-height: 100vh;
  display: flex;
  align-items: stretch;
  justify-content: center;
  color: #f5f5f5;
  background: radial-gradient(circle at top, #1f2937 0, #020617 55%);
  overflow-y: auto;
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

.container { width: 100%; max-width: 900px; padding: 24px 16px; }

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
  transition: border 0.18s ease, background 0.18s ease, transform 0.10s ease, box-shadow 0.20s ease, backdrop-filter 0.18s ease;
  box-shadow: 0 10px 24px rgba(15,23,42,0.85);
  backdrop-filter: blur(16px) saturate(180%); -webkit-backdrop-filter: blur(16px) saturate(180%);
}
.option-label:hover {
  border-color: rgba(56,189,248,0.9);
  background: linear-gradient(135deg, rgba(15,23,42,0.95), rgba(15,23,42,1));
  transform: translateY(-1px);
  box-shadow: 0 0 18px rgba(56,189,248,0.35), 0 14px 28px rgba(15,23,42,0.95);
}
.option-label input { margin-right: 8px; accent-color: #38bdf8; width: 14px; height: 14px; }

.nav-row { display: flex; justify-content: space-between; align-items: center; margin-top: 14px; gap: 10px; z-index: 1; position: relative; }
.nav-buttons { display: flex; gap: 10px; }
button {
  border-radius: 999px; border: none; padding: 8px 18px; font-size: 0.9rem; cursor: pointer;
  transition: transform 0.1s ease, box-shadow 0.18s ease, background 0.18s ease;
  display: inline-flex; align-items: center; gap: 6px;
}
button:active { transform: translateY(1px); box-shadow: none; }
.btn-primary {
  background: linear-gradient(135deg, #38bdf8, #22c55e); color: #020617;
  box-shadow: 0 10px 24px rgba(56,189,248,0.6);
}
.btn-primary:hover:not(:disabled) { background: linear-gradient(135deg, #0ea5e9, #16a34a); }
.btn-primary:disabled {
  background: rgba(56,189,248,0.3); color: rgba(2,6,23,0.5); cursor: not-allowed;
  box-shadow: none;
}
.btn-secondary {
  background: rgba(15,23,42,0.85); color: #e5e7eb; border: 1px solid rgba(148,163,184,0.7);
  backdrop-filter: blur(10px); -webkit-backdrop-filter: blur(10px);
}
.btn-secondary:hover { background: rgba(15,23,42,0.95); }

.progress-dots { display: flex; gap: 4px; align-items: center; justify-content: flex-start; }
.dot {
  width: 8px; height: 8px; border-radius: 999px; background: rgba(148,163,184,0.5);
  transition: width 0.25s ease, background 0.25s ease, box-shadow 0.25s ease;
}
.dot.active { width: 22px; background: #38bdf8; box-shadow: 0 0 12px rgba(56,189,248,0.8); }

#result {
  margin-top: 18px; padding: 14px 16px 12px; border-radius: 18px; border: 1px solid rgba(148,163,184,0.5);
  background: linear-gradient(135deg, rgba(15,23,42,0.9), rgba(37,99,235,0.45));
  max-height: 280px; overflow-y: auto; backdrop-filter: blur(18px) saturate(160%);
  -webkit-backdrop-filter: blur(18px) saturate(160%);
  box-shadow: 0 10px 30px rgba(15,23,42,0.9), 0 0 32px rgba(56,189,248,0.45);
  z-index: 1; position: relative;
}
#result h2 { font-size: 1.05rem; margin-bottom: 6px; }
#result h3 { font-size: 0.95rem; margin-top: 8px; margin-bottom: 4px; color: #a5b4fc; }
#result p { font-size: 0.9rem; color: #e5e7eb; }
a { color: #38bdf8; text-decoration: none; }
a:hover { text-decoration: underline; }
.hidden { display: none; }

@media (max-width: 640px) {
  body { padding: 10px; align-items: flex-start; }
  .assistant-card { margin: 24px 8px; padding: 24px 18px 20px; border-radius: 20px; min-height: 340px; }
  .option-label { padding: 9px 12px; border-radius: 16px; }
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
        <h1>Smart Laptop Helper</h1>
        <p>Answer step by step, get smart summary & laptops from Reliance Digital</p>
      </div>
      <div class="step-indicator" id="stepIndicator">Step 1 of 10</div>
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
      <h2>Your Perfect Laptop</h2>
      <p id="summaryText"></p>
      <h3>Recommended Specs</h3>
      <p id="specText"></p>
      <h3>Shop on Reliance Digital</h3>
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

  stepIndicator.textContent = `Step ${currentStep} of ${totalSteps}`;
  prevBtn.style.visibility = currentStep === 1 ? 'hidden' : 'visible';
  nextBtn.textContent = currentStep === totalSteps ? 'Get My Laptop' : 'Next';
  
  bgLayer.className = 'bg-layer bg-step-' + currentStep;
  
  document.querySelectorAll('.dot').forEach((dot, index) => {
    const s = Number(dot.dataset.step);
    dot.classList.toggle('active', s === currentStep);
  });

  updateVisual();
  updateNextButton(); // Check if current step has selection
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

// Listen for radio button changes across ALL questions
document.addEventListener('change', function(e) {
  if (e.target.matches('input[type="radio"]')) {
    updateNextButton(); // Update button only for current active question
  }
});

function getRecommendation() {
  const answers = {};
  for (let i = 1; i <= 10; i++) {
    const selected = document.querySelector(`input[name="q${i}"]:checked`);
    answers[`q${i}`] = selected ? selected.value : 'not_answered';
  }

  let summary = `You need a laptop for ${answers.q1 === 'gaming' ? 'gaming & heavy work' : 
                   answers.q1 === 'creative' ? 'creative work' : 'office & study'}. `;
  
  summary += `Budget ₹${answers.q4 === '30k' ? '30K' : answers.q4 === '50k' ? '50K' : 
              answers.q4 === '80k' ? '80K' : '80K+'}. `;
  
  summary += `${answers.q2 === 'daily' ? 'Lightweight' : 'Standard'} design with `;
  summary += `${answers.q3 === '10hr' ? '10+hr' : answers.q3 === '8hr' ? '8hr' : '6hr'} battery.`;

  let specs = `Core i5/Ryzen 5, 16GB RAM, ${answers.q7 === '1tb' || answers.q7 === 'more' ? '1TB' : '512GB'} SSD, `;
  specs += `${answers.q5 === '15' ? '15.6"' : '14"'} display`;

  summaryText.textContent = summary;
  specText.textContent = specs;
  linkText.innerHTML = '<a href="https://www.reliancedigital.in/laptops/c/S101210" target="_blank">Browse matching laptops →</a>';

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
  if (nextBtn.disabled) return; // Already validated by button state

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
