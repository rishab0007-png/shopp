<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<title>Smart Laptop Helper</title>
<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
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

/* Background layer */
.bg-layer {
  position: fixed;
  inset: 0;
  z-index: -3;
  transition: background 0.8s ease-in-out;
}
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
.bg-step-11 { background: radial-gradient(circle at top, #f97316 0, #020617 55%); }
.bg-step-12 { background: radial-gradient(circle at top, #ef4444 0, #020617 55%); }
.bg-step-13 { background: radial-gradient(circle at top, #8b5cf6 0, #020617 55%); }
.bg-step-14 { background: radial-gradient(circle at top, #10b981 0, #020617 55%); }
.bg-step-15 { background: radial-gradient(circle at top, #f59e0b 0, #020617 55%); }

.overlay-gradient {
  position: fixed;
  inset: 0;
  background:
    radial-gradient(circle at top left, rgba(15,23,42,0.2), transparent 60%),
    radial-gradient(circle at bottom, rgba(15,23,42,0.95), rgba(15,23,42,0.98));
  z-index: -2;
}
.bg-laptop-shape {
  position: fixed;
  width: 520px;
  height: 320px;
  border-radius: 32px;
  border: 1px solid rgba(148,163,184,0.25);
  background: linear-gradient(135deg, rgba(15,23,42,0.9), rgba(30,64,175,0.6));
  box-shadow:
    0 40px 80px rgba(15,23,42,0.9),
    0 0 80px rgba(56,189,248,0.5);
  bottom: -120px;
  right: -80px;
  transform: rotate(-6deg);
  opacity: 0.35;
  z-index: -1;
}

.container {
  width: 100%;
  max-width: 900px;
  padding: 24px 16px;
}

/* BIGGER GLASS CARD */
.assistant-card {
  position: relative;
  border-radius: 28px;
  padding: 32px 32px 28px;
  margin: 60px auto;
  max-width: 1000px;
  min-height: 380px;
  height: auto;
  background: linear-gradient(
    135deg,
    rgba(15, 23, 42, 0.40),
    rgba(30, 64, 175, 0.22)
  );
  border: 1px solid rgba(148, 163, 184, 0.55);
  box-shadow:
    0 26px 70px rgba(15, 23, 42, 0.95),
    0 0 90px rgba(56, 189, 248, 0.5),
    inset 0 0 0 1px rgba(255, 255, 255, 0.10);
  backdrop-filter: blur(24px) saturate(170%);
  -webkit-backdrop-filter: blur(24px) saturate(170%);
  overflow: visible;
}
.assistant-card::before {
  content: "";
  position: absolute;
  inset: -40%;
  background:
    radial-gradient(circle at 0% 0%, rgba(255, 255, 255, 0.20), transparent 55%),
    radial-gradient(circle at 100% 100%, rgba(56, 189, 248, 0.16), transparent 60%);
  opacity: 0.9;
  mix-blend-mode: screen;
  pointer-events: none;
}

.header-row {
  position: relative;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 14px;
  gap: 12px;
  z-index: 1;
}
.title-block h1 {
  font-size: 1.5rem;
  letter-spacing: 0.04em;
}
.title-block p {
  font-size: 0.9rem;
  color: #cbd5f5;
  margin-top: 4px;
}
.step-indicator {
  font-size: 0.8rem;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: #e0e7ff;
  border-radius: 999px;
  padding: 4px 10px;
  border: 1px solid rgba(129,140,248,0.8);
  background: radial-gradient(circle at top, rgba(67,56,202,0.7), rgba(15,23,42,0.9));
  white-space: nowrap;
  box-shadow: 0 0 14px rgba(129,140,248,0.7);
}

/* small badge top-right */
.question-visual {
  position: absolute;
  top: 20px;
  right: 22px;
  padding: 8px 12px;
  border-radius: 999px;
  background: rgba(15,23,42,0.85);
  border: 1px solid rgba(148,163,184,0.6);
  font-size: 0.8rem;
  display: inline-flex;
  align-items: center;
  gap: 6px;
  color: #e5e7eb;
  box-shadow:
    0 8px 22px rgba(15,23,42,0.95),
    0 0 18px rgba(56,189,248,0.8);
  z-index: 2;
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
}
.question-visual span.icon {
  font-size: 1.05rem;
}

.question-wrapper {
  position: relative;
  margin-top: 10px;
  z-index: 1;
}

/* Question blocks */
.question {
  position: relative;
  opacity: 0;
  transform: translateY(18px) scale(0.98);
  transition: opacity 0.45s ease, transform 0.45s ease;
  pointer-events: none;
  display: none;
}
.question.active {
  display: block;
  opacity: 1;
  transform: translateY(0) scale(1);
  pointer-events: auto;
}
.question h3 {
  font-size: 1.15rem;
  margin-bottom: 10px;
}
.question-sub {
  font-size: 0.85rem;
  color: #d1d5db;
  margin-bottom: 12px;
}

/* iPhone notification–like glass tiles */
.option-label {
  position: relative;
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 10px;
  padding: 10px 14px;
  border-radius: 18px;
  background: linear-gradient(
    135deg,
    rgba(15,23,42,0.85),
    rgba(15,23,42,0.95)
  );
  border: 1px solid rgba(148,163,184,0.55);
  cursor: pointer;
  transition:
    border 0.18s ease,
    background 0.18s ease,
    transform 0.10s ease,
    box-shadow 0.20s ease,
    backdrop-filter 0.18s ease;
  box-shadow:
    0 10px 24px rgba(15,23,42,0.85);
  backdrop-filter: blur(16px) saturate(180%);
  -webkit-backdrop-filter: blur(16px) saturate(180%);
}
.option-label:hover {
  border-color: rgba(56,189,248,0.9);
  background: linear-gradient(
    135deg,
    rgba(15,23,42,0.95),
    rgba(15,23,42,1)
  );
  transform: translateY(-1px);
  box-shadow:
    0 0 18px rgba(56,189,248,0.35),
    0 14px 28px rgba(15,23,42,0.95);
}
.option-label input {
  margin-right: 8px;
  accent-color: #38bdf8;
  width: 14px;
  height: 14px;
}

.nav-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 14px;
  gap: 10px;
  z-index: 1;
  position: relative;
}
.nav-buttons {
  display: flex;
  gap: 10px;
}
button {
  border-radius: 999px;
  border: none;
  padding: 8px 18px;
  font-size: 0.9rem;
  cursor: pointer;
  transition: transform 0.1s ease, box-shadow 0.18s ease, background 0.18s ease;
  display: inline-flex;
  align-items: center;
  gap: 6px;
}
button:active {
  transform: translateY(1px);
  box-shadow: none;
}
.btn-primary {
  background: linear-gradient(135deg, #38bdf8, #22c55e);
  color: #020617;
  box-shadow: 0 10px 24px rgba(56,189,248,0.6);
}
.btn-primary:hover {
  background: linear-gradient(135deg, #0ea5e9, #16a34a);
}
.btn-secondary {
  background: rgba(15,23,42,0.85);
  color: #e5e7eb;
  border: 1px solid rgba(148,163,184,0.7);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
}
.btn-secondary:hover {
  background: rgba(15,23,42,0.95);
}
.progress-dots {
  display: flex;
  gap: 4px;
  align-items: center;
  justify-content: flex-start;
}
.dot {
  width: 8px;
  height: 8px;
  border-radius: 999px;
  background: rgba(148,163,184,0.5);
  transition: width 0.25s ease, background 0.25s ease, box-shadow 0.25s ease;
}
.dot.active {
  width: 22px;
  background: #38bdf8;
  box-shadow: 0 0 12px rgba(56,189,248,0.8);
}

#result {
  margin-top: 18px;
  padding: 14px 16px 12px;
  border-radius: 18px;
  border: 1px solid rgba(148,163,184,0.5);
  background: linear-gradient(
    135deg,
    rgba(15,23,42,0.9),
    rgba(37,99,235,0.45)
  );
  max-height: 280px;
  overflow-y: auto;
  backdrop-filter: blur(18px) saturate(160%);
  -webkit-backdrop-filter: blur(18px) saturate(160%);
  box-shadow:
    0 10px 30px rgba(15,23,42,0.9),
    0 0 32px rgba(56,189,248,0.45);
  z-index: 1;
  position: relative;
}
#result h2 {
  font-size: 1.05rem;
  margin-bottom: 6px;
}
#result h3 {
  font-size: 0.95rem;
  margin-top: 8px;
  margin-bottom: 4px;
  color: #a5b4fc;
}
#result p {
  font-size: 0.9rem;
  color: #e5e7eb;
}
a {
  color: #38bdf8;
  text-decoration: none;
}
a:hover {
  text-decoration: underline;
}
.hidden {
  display: none;
}

@media (max-width: 640px) {
  body {
    padding: 10px;
    align-items: flex-start;
  }
  .assistant-card {
    margin: 24px 8px;
    padding: 24px 18px 20px;
    border-radius: 20px;
    min-height: 340px;
  }
  .option-label {
    padding: 9px 12px;
    border-radius: 16px;
  }
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
        <p>Answer step by step, get a smart summary and laptops from Reliance Digital.</p>
      </div>
      <div class="step-indicator" id="stepIndicator">Step 1 of 15</div>
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

          <label class="option-label">
            <input type="radio" name="q1" value="basic">
            Basic work, browsing, YouTube
          </label>
          <label class="option-label">
            <input type="radio" name="q1" value="office">
            Office work, online classes
          </label>
          <label class="option-label">
            <input type="radio" name="q1" value="gaming">
            Gaming / video editing / design
          </label>
        </div>

        <!-- STEP 2 -->
        <div class="question" data-step="2">
          <h3>Carrying & weight</h3>
          <p class="question-sub">How important is a light laptop for you?</p>

          <label class="option-label">
            <input type="radio" name="q2" value="very_light">
            Very important, carry daily
          </label>
          <label class="option-label">
            <input type="radio" name="q2" value="normal">
            Normal weight is okay
          </label>
          <label class="option-label">
            <input type="radio" name="q2" value="not_important">
            Weight is not important
          </label>
        </div>

        <!-- STEP 3 -->
        <div class="question" data-step="3">
          <h3>Battery backup</h3>
          <p class="question-sub">How many hours do you want without charger?</p>

          <label class="option-label">
            <input type="radio" name="q3" value="4">
            Around 4 hours
          </label>
          <label class="option-label">
            <input type="radio" name="q3" value="6">
            6–7 hours
          </label>
          <label class="option-label">
            <input type="radio" name="q3" value="8plus">
            8+ hours if possible
          </label>
        </div>

        <!-- TODO: add steps 4 .. 15 in same style -->

      </div>

      <div class="nav-row">
        <div class="progress-dots" id="progressDots"></div>
        <div class="nav-buttons">
          <button type="button" class="btn-secondary" id="prevBtn">Back</button>
          <button type="button" class="btn-primary" id="nextBtn">Next</button>
        </div>
      </div>
    </form>

    <div id="result" class="hidden">
      <h2>Your laptop summary</h2>
      <p id="summaryText"></p>
      <h3>Suggested laptop type</h3>
      <p id="specText"></p>
      <h3>Example laptop link on Reliance Digital</h3>
      <p id="linkText"></p>
    </div>
  </div>
</div>

<script>
const totalSteps = 15; // keep 15, UI and backgrounds depend on this
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
  5:{icon:'🖥️',text:'Screen size & comfort'},
  6:{icon:'🏷️',text:'Brand preference'},
  7:{icon:'💾',text:'Storage & files'},
  8:{icon:'📆',text:'How many years'},
  9:{icon:'⌨️',text:'Typing & keyboard'},
 10:{icon:'🔌',text:'Ports & connections'},
 11:{icon:'📹',text:'Online meetings'},
 12:{icon:'🧩',text:'OS preference'},
 13:{icon:'🖐️',text:'Touch screen need'},
 14:{icon:'🛡️',text:'Build quality'},
 15:{icon:'⭐',text:'Special features'}
};

// create progress dots
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

function updateUI() {
  questions.forEach(q => {
    const s = Number(q.dataset.step);
    q.classList.toggle('active', s === currentStep);
  });

  stepIndicator.textContent = `Step ${currentStep} of ${totalSteps}`;
  prevBtn.style.visibility = currentStep === 1 ? 'hidden' : 'visible';
  nextBtn.textContent = currentStep === totalSteps ? 'Get suggestion' : 'Next';

  bgLayer.className = 'bg-layer bg-step-' + currentStep;

  document.querySelectorAll('.dot').forEach(dot => {
    const s = Number(dot.dataset.step);
    dot.classList.toggle('active', s === currentStep);
  });

  updateVisual();
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

function validateCurrentStep() {
  const active = document.querySelector('.question.active');
  if (!active) return true; // nothing to validate

  const first = active.querySelector('input[type="radio"]');
  const name = first ? first.name : null;
  if (!name) return true;

  const checked = active.querySelector(`input[name="${name}"]:checked`);
  if (!checked) {
    alert('Please choose one option to continue.');
    return false;
  }
  return true;
}

// simple recommendation using first 3 questions
function getRecommendation() {
  const q1 = document.querySelector('input[name="q1"]:checked')?.value;
  const q2 = document.querySelector('input[name="q2"]:checked')?.value;
  const q3 = document.querySelector('input[name="q3"]:checked')?.value;

  let usageText = 'balanced everyday use';
  if (q1 === 'basic') usageText = 'basic browsing and office work';
  else if (q1 === 'gaming') usageText = 'gaming and heavy tasks';

  let weightText = (q2 === 'very_light') ? 'lightweight, easy to carry' : 'normal weight';

  let batteryText = 'around 5–6 hours';
  if (q3 === '8plus') batteryText = 'long battery life (8+ hours)';

  summaryText.textContent =
    `You need a laptop for ${usageText}, with a ${weightText} body and ${batteryText} of backup.`;

  specText.textContent =
    'Look for 16 GB RAM, SSD storage, and at least Intel Core i5 / Ryzen 5 or above for smooth performance.';

  linkText.innerHTML =
    'Example: <a href="https://www.reliancedigital.in/laptops/c/S101210" target="_blank">Browse laptops on Reliance Digital</a>.';

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
  if (!validateCurrentStep()) return;

  if (currentStep < totalSteps) {
    currentStep++;
    updateUI();
  } else {
    getRecommendation();
  }
});

// initial UI
updateVisual();
updateUI();
</script>
</body>
</html>
