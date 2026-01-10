<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<title>Cinematic Laptop AI Assistant</title>
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
  align-items: center;
  justify-content: center;
  color: #f5f5f5;
  background: radial-gradient(circle at top, #1f2937 0, #020617 55%);
  overflow: hidden;
}
/* Background layer that we animate per step */
.bg-layer {
  position: fixed;
  inset: 0;
  z-index: -3;
  transition: background 0.8s ease-in-out;
}
/* Different color moods per step, plus a slight pattern feeling */
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
/* Large abstract laptop-like glow in background */
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
  max-width: 820px;
  padding: 24px;
}
/* LIQUID GLASS CARD */
.assistant-card {
  position: relative;
  border-radius: 24px;
  padding: 28px 26px 24px;
  /* glass base */
  background: linear-gradient(
    135deg,
    rgba(15, 23, 42, 0.35),
    rgba(30, 64, 175, 0.18)
  );
  border: 1px solid rgba(148, 163, 184, 0.45);
  box-shadow:
    0 18px 55px rgba(15, 23, 42, 0.9),
    0 0 80px rgba(56, 189, 248, 0.45),
    inset 0 0 0 1px rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(22px) saturate(160%);
  -webkit-backdrop-filter: blur(22px) saturate(160%);
  overflow: hidden;
}
/* inner liquid-glass shine */
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
/* Small cinematic icon/text in corner related to question */
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

/* UPDATED: wrapper with smooth scroll */
.question-wrapper {
  position: relative;
  min-height: 220px;
  max-height: 60vh;
  margin-top: 10px;
  overflow-y: auto;
  z-index: 1;
  scroll-behavior: smooth;
}

.question {
  position: absolute;
  inset: 0;
  opacity: 0;
  transform: translateY(18px) scale(0.98);
  transition: opacity 0.45s ease, transform 0.45s ease;
  pointer-events: none;
}
.question.active {
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

/* UPDATED: iOS‑style liquid glass option tiles */
.option-label {
  position: relative;
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 8px;
  padding: 10px 12px;
  border-radius: 18px;
  background: radial-gradient(circle at top left, rgba(15,23,42,0.85), rgba(15,23,42,0.96));
  border: 1px solid rgba(148,163,184,0.55);
  cursor: pointer;
  transition:
    border 0.18s ease,
    background 0.18s ease,
    transform 0.12s ease,
    box-shadow 0.20s ease,
    backdrop-filter 0.18s ease;
  box-shadow:
    0 0 0 rgba(56,189,248,0),
    0 10px 20px rgba(15,23,42,0.75);
  backdrop-filter: blur(12px) saturate(160%);
  -webkit-backdrop-filter: blur(12px) saturate(160%);
}
.option-label:hover {
  border-color: rgba(56,189,248,0.9);
  background: radial-gradient(circle at top left, rgba(15,23,42,0.95), rgba(15,23,42,1));
  transform: translateY(-1px);
  box-shadow:
    0 0 16px rgba(56,189,248,0.35),
    0 14px 26px rgba(15,23,42,0.95);
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
    padding: 20px 18px 16px;
    border-radius: 18px;
  }
  .question-wrapper {
    min-height: 260px;
    max-height: 65vh;
  }
  h1 {
    font-size: 1.25rem;
  }
  .question-visual {
    display: none;
  }
  .option-label {
    padding: 9px 11px;
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
<!-- Cinematic corner visual reflecting question topic -->
<div class="question-visual" id="questionVisual">
<span class="icon">💻</span>
<span class="text">Main laptop usage</span>
</div>
<form id="quizForm">
<div class="question-wrapper">
<!-- Q1 -->
<div class="question active" data-step="1">
<h3>1. What will you mostly do on the laptop?</h3>
<p class="question-sub">This helps to decide power and graphics.</p>
<label class="option-label">
<input type="radio" name="use" value="basic" required>
Simple work (browsing, YouTube, MS Office, online classes)
</label>
<label class="option-label">
<input type="radio" name="use" value="office">
Office work / coding / study projects
</label>
<label class="option-label">
<input type="radio" name="use" value="creator">
Photo / video editing, graphic design
</label>
<label class="option-label">
<input type="radio" name="use" value="gaming">
Gaming and heavy work
</label>
</div>
<!-- Q2 -->
<div class="question" data-step="2">
<h3>2. Will you carry the laptop outside your home?</h3>
<p class="question-sub">College, office, travel, daily commute, etc.</p>
<label class="option-label">
<input type="radio" name="carry" value="daily" required>
Yes, almost every day
</label>
<label class="option-label">
<input type="radio" name="carry" value="sometimes">
Sometimes
</label>
<label class="option-label">
<input type="radio" name="carry" value="rarely">
Rarely or almost never
</label>
</div>
<!-- Q3 -->
<div class="question" data-step="3">
<h3>3. Without charging, how many hours do you want it to run?</h3>
<p class="question-sub">Real daily use, not company claim.</p>
<label class="option-label">
<input type="radio" name="battery" value="short" required>
1–3 hours is enough
</label>
<label class="option-label">
<input type="radio" name="battery" value="medium">
Around 4–6 hours
</label>
<label class="option-label">
<input type="radio" name="battery" value="long">
7+ hours, I want long battery
</label>
</div>
<!-- Q4 -->
<div class="question" data-step="4">
<h3>4. What is your budget range?</h3>
<p class="question-sub">Just an approximate idea.</p>
<label class="option-label">
<input type="radio" name="budget" value="low" required>
Low budget
</label>
<label class="option-label">
<input type="radio" name="budget" value="medium">
Medium budget
</label>
<label class="option-label">
<input type="radio" name="budget" value="high">
High budget, I can spend more for performance
</label>
</div>
<!-- Q5 -->
<div class="question" data-step="5">
<h3>5. What do you prefer more?</h3>
<p class="question-sub">Size changes weight and comfort.</p>
<label class="option-label">
<input type="radio" name="screen" value="small" required>
Easy to carry (13–14 inch)
</label>
<label class="option-label">
<input type="radio" name="screen" value="big">
Bigger screen (15–16 inch)
</label>
<label class="option-label">
<input type="radio" name="screen" value="any">
Any size is fine for me
</label>
</div>
<!-- Q6 -->
<div class="question" data-step="6">
<h3>6. Do you want a specific laptop brand?</h3>
<p class="question-sub">We will still focus on specs first.</p>
<label class="option-label">
<input type="radio" name="brand" value="any" required>
Any good brand is okay
</label>
<label class="option-label">
<input type="radio" name="brand" value="hp">
I prefer HP
</label>
<label class="option-label">
<input type="radio" name="brand" value="dell">
I prefer Dell
</label>
<label class="option-label">
<input type="radio" name="brand" value="lenovo">
I prefer Lenovo
</label>
<label class="option-label">
<input type="radio" name="brand" value="asus">
I prefer ASUS
</label>
<label class="option-label">
<input type="radio" name="brand" value="acer">
I prefer Acer
</label>
</div>
<!-- Q7 -->
<div class="question" data-step="7">
<h3>7. Do you store big files like games, videos or many photos?</h3>
<p class="question-sub">This controls SSD size.</p>
<label class="option-label">
<input type="radio" name="storageUse" value="heavy" required>
Yes, many big files
</label>
<label class="option-label">
<input type="radio" name="storageUse" value="medium">
Some big files
</label>
<label class="option-label">
<input type="radio" name="storageUse" value="light">
No, only small documents and light files
</label>
</div>
<!-- Q8 -->
<div class="question" data-step="8">
<h3>8. For how many years do you want this laptop to feel good?</h3>
<p class="question-sub">Longer years need stronger hardware.</p>
<label class="option-label">
<input type="radio" name="years" value="short" required>
Around 1–2 years is okay
</label>
<label class="option-label">
<input type="radio" name="years" value="medium">
Around 3–4 years
</label>
<label class="option-label">
<input type="radio" name="years" value="long">
5+ years, I want to keep it long
</label>
</div>
<!-- Q9 -->
<div class="question" data-step="9">
<h3>9. Do you type a lot (coding, writing, office work)?</h3>
<p class="question-sub">Helps to decide keyboard importance.</p>
<label class="option-label">
<input type="radio" name="typing" value="heavy" required>
Yes, I type a lot
</label>
<label class="option-label">
<input type="radio" name="typing" value="normal">
Normal typing only
</label>
</div>
<!-- Q10 -->
<div class="question" data-step="10">
<h3>10. Do you connect many devices (monitor, projector, LAN, USB)?</h3>
<p class="question-sub">Ports matter for office and setup.</p>
<label class="option-label">
<input type="radio" name="ports" value="many" required>
Yes, I need many ports
</label>
<label class="option-label">
<input type="radio" name="ports" value="normal">
Normal ports are enough
</label>
</div>
<!-- Q11 -->
<div class="question" data-step="11">
<h3>11. Do you attend many online classes or video meetings?</h3>
<p class="question-sub">This decides webcam and mic importance.</p>
<label class="option-label">
<input type="radio" name="webcam" value="often" required>
Yes, very often
</label>
<label class="option-label">
<input type="radio" name="webcam" value="sometimes">
Sometimes
</label>
<label class="option-label">
<input type="radio" name="webcam" value="rarely">
Rarely
</label>
</div>
<!-- Q12: OS preference -->
<div class="question" data-step="12">
<h3>12. Which operating system do you prefer?</h3>
<p class="question-sub">This decides Windows / other OS type.</p>
<label class="option-label">
<input type="radio" name="os" value="windows" required>
Windows only
</label>
<label class="option-label">
<input type="radio" name="os" value="any">
Any OS is fine
</label>
</div>
<!-- Q13: Touch screen -->
<div class="question" data-step="13">
<h3>13. Do you need a touch screen?</h3>
<p class="question-sub">Useful for drawing, notes, and tablet style.</p>
<label class="option-label">
<input type="radio" name="touch" value="must" required>
Yes, I want touch screen
</label>
<label class="option-label">
<input type="radio" name="touch" value="nice">
Nice to have, not compulsory
</label>
<label class="option-label">
<input type="radio" name="touch" value="no">
No, I don’t need touch screen
</label>
</div>
<!-- Q14: Build quality -->
<div class="question" data-step="14">
<h3>14. How important is strong build quality?</h3>
<p class="question-sub">Metal body and strong hinges cost more.</p>
<label class="option-label">
<input type="radio" name="build" value="high" required>
Very important, I want solid build
</label>
<label class="option-label">
<input type="radio" name="build" value="medium">
Medium important, normal is okay
</label>
<label class="option-label">
<input type="radio" name="build" value="low">
Not very important, I focus on specs
</label>
</div>
<!-- Q15: Special features -->
<div class="question" data-step="15">
<h3>15. Any special feature you like?</h3>
<p class="question-sub">This helps to fine-tune model type.</p>
<label class="option-label">
<input type="radio" name="special" value="backlit" required>
Backlit keyboard is important
</label>
<label class="option-label">
<input type="radio" name="special" value="fingerprint">
Fingerprint / fast login is important
</label>
<label class="option-label">
<input type="radio" name="special" value="none">
Nothing special, normal features are fine
</label>
</div>
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
const totalSteps = 15;
let currentStep = 1;
const bgLayer = document.getElementById('bgLayer');
const stepIndicator = document.getElementById('stepIndicator');
const questions = document.querySelectorAll('.question');
const prevBtn = document.getElementById('prevBtn');
const nextBtn = document.getElementById('nextBtn');
const progressDotsContainer = document.getElementById('progressDots');
const resultBlock = document.getElementById('result');
const questionVisual = document.getElementById('questionVisual');
const questionWrapper = document.querySelector('.question-wrapper');

// Small label + icon text for each step
const visualTexts = {
1: { icon: '💻', text: 'Main laptop usage' },
2: { icon: '🎒', text: 'Carrying & weight' },
3: { icon: '🔋', text: 'Battery backup' },
4: { icon: '💰', text: 'Budget range' },
5: { icon: '🖥️', text: 'Screen size & comfort' },
6: { icon: '🏷️', text: 'Brand preference' },
7: { icon: '💾', text: 'Storage & files' },
8: { icon: '📆', text: 'How many years' },
9: { icon: '⌨️', text: 'Typing & keyboard' },
10: { icon: '🔌', text: 'Ports & connections' },
11: { icon: '📹', text: 'Online meetings' },
12: { icon: '🧩', text: 'OS preference' },
13: { icon: '🖐️', text: 'Touch screen need' },
14: { icon: '🛡️', text: 'Build quality' },
15: { icon: '⭐', text: 'Special features' }
};
// Create progress dots
for (let i = 1; i <= totalSteps; i++) {
const dot = document.createElement('div');
dot.className = 'dot' + (i === 1 ? ' active' : '');
dot.dataset.step = i;
progressDotsContainer.appendChild(dot);
}
function updateVisual() {
const data = visualTexts[currentStep];
if (!data) return;
questionVisual.querySelector('.icon').textContent = data.icon;
questionVisual.querySelector('.text').textContent = data.text;
}
function updateUI() {
questions.forEach(q => {
const step = Number(q.dataset.step);
q.classList.toggle('active', step === currentStep);
});
stepIndicator.textContent = `Step ${currentStep} of ${totalSteps}`;
prevBtn.style.visibility = currentStep === 1 ? 'hidden' : 'visible';
nextBtn.textContent = currentStep === totalSteps ? 'Get suggestion' : 'Next';
bgLayer.className = 'bg-layer bg-step-' + currentStep;
document.querySelectorAll('.dot').forEach(dot => {
const step = Number(dot.dataset.step);
dot.classList.toggle('active', step === currentStep);
});
updateVisual();
// reset scroll on each step
if (questionWrapper) {
  questionWrapper.scrollTop = 0;
}
}
function validateCurrentStep() {
const activeQuestion = document.querySelector('.question.active');
const firstInput = activeQuestion.querySelector('input[type="radio"]');
const name = firstInput ? firstInput.name : null;
if (!name) return true;
const checked = document.querySelector(`input[name="${name}"]:checked`);
if (!checked) {
alert('Please choose one option to continue.');
return false;
}
return true;
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
// Initial
updateVisual();
updateUI();
// ===== Recommendation logic =====
function getRecommendation() {
const form = document.getElementById('quizForm');
const requiredNames = [
"use", "carry", "battery", "budget", "screen", "brand",
"storageUse", "years", "typing", "ports", "webcam",
"os", "touch", "build", "special"
];
for (let name of requiredNames) {
const checked = form.querySelector('input[name="' + name + '"]:checked');
if (!checked) {
alert("Please answer all questions.");
return;
}
}
const use = form.querySelector('input[name="use"]:checked').value;
const carry = form.querySelector('input[name="carry"]:checked').value;
const battery = form.querySelector('input[name="battery"]:checked').value;
const budget = form.querySelector('input[name="budget"]:checked').value;
const screen = form.querySelector('input[name="screen"]:checked').value;
const brand = form.querySelector('input[name="brand"]:checked').value;
const storageUse = form.querySelector('input[name="storageUse"]:checked').value;
const years = form.querySelector('input[name="years"]:checked').value;
const typing = form.querySelector('input[name="typing"]:checked').value;
const ports = form.querySelector('input[name="ports"]:checked').value;
const webcam = form.querySelector('input[name="webcam"]:checked').value;
const os = form.querySelector('input[name="os"]:checked').value;
const touch = form.querySelector('input[name="touch"]:checked').value;
const build = form.querySelector('input[name="build"]:checked').value;
const special = form.querySelector('input[name="special"]:checked').value;
let summaryParts = [];
// Use
if (use === "basic") {
summaryParts.push("You mainly want a laptop for simple daily work like browsing, videos and basic office tasks.");
} else if (use === "office") {
summaryParts.push("You will use the laptop for office work, coding or study projects.");
} else if (use === "creator") {
summaryParts.push("You plan to do photo or video editing and other creative work.");
} else if (use === "gaming") {
summaryParts.push("You want to play games or do other heavy work on the laptop.");
}
// Carry
if (carry === "daily") {
summaryParts.push("You will carry the laptop almost every day, so it should be light and easy to carry.");
} else if (carry === "sometimes") {
summaryParts.push("You will carry the laptop sometimes, so medium weight is okay.");
} else {
summaryParts.push("You will rarely carry the laptop outside, so weight is not a big problem.");
}
// Battery
if (battery === "long") {
summaryParts.push("You want long battery life for many hours away from charging.");
} else if (battery === "medium") {
summaryParts.push("You need decent battery life for normal daily use.");
} else {
summaryParts.push("Battery life is not your main concern.");
}
// Budget
if (budget === "low") {
summaryParts.push("You have a low budget and want good value for money.");
} else if (budget === "medium") {
summaryParts.push("You have a medium budget and want a balance of price and performance.");
} else {
summaryParts.push("You are ready to pay more for better performance and features.");
}
// Screen
if (screen === "small") {
summaryParts.push("You prefer a smaller and lighter laptop size, around 13–14 inches.");
} else if (screen === "big") {
summaryParts.push("You prefer a bigger screen, around 15–16 inches, for comfortable viewing.");
} else {
summaryParts.push("Any screen size is fine for you.");
}
// Brand
if (brand === "any") {
summaryParts.push("You are open to any good and reliable brand.");
} else {
summaryParts.push("You prefer the brand: " + brand.toUpperCase() + ".");
}
// Storage use
if (storageUse === "heavy") {
summaryParts.push("You store many big files like games, videos or a lot of photos.");
} else if (storageUse === "medium") {
summaryParts.push("You store some big files, but not too many.");
} else {
summaryParts.push("You mostly store small documents and light files.");
}
// Years
if (years === "short") {
summaryParts.push("You are okay if the laptop is good for around 1–2 years.");
} else if (years === "medium") {
summaryParts.push("You want the laptop to feel good for around 3–4 years.");
} else {
summaryParts.push("You want to keep this laptop for 5 or more years.");
}
// Typing
if (typing === "heavy") {
summaryParts.push("You type a lot, so a comfortable keyboard is important for you.");
} else {
summaryParts.push("You do normal typing, nothing very heavy.");
}
// Ports
if (ports === "many") {
summaryParts.push("You connect many devices, so you need enough ports (HDMI, USB, maybe LAN).");
} else {
summaryParts.push("Normal ports are enough for your use.");
}
// Webcam
if (webcam === "often") {
summaryParts.push("You attend many online classes or video meetings, so webcam and mic quality matters.");
} else if (webcam === "sometimes") {
summaryParts.push("You sometimes attend online meetings or classes.");
} else {
summaryParts.push("You rarely use the laptop for online video calls.");
}
// OS
if (os === "windows") {
summaryParts.push("You clearly prefer Windows as your operating system.");
} else {
summaryParts.push("You are flexible with operating system choice.");
}
// Touch
if (touch === "must") {
summaryParts.push("You want a touch screen for better interaction.");
} else if (touch === "nice") {
summaryParts.push("Touch screen is nice for you, but not compulsory.");
} else {
summaryParts.push("You do not need a touch screen.");
}
// Build
if (build === "high") {
summaryParts.push("Strong build quality and a solid body are very important for you.");
} else if (build === "medium") {
summaryParts.push("Normal build quality is enough, you want a balance of strength and price.");
} else {
summaryParts.push("You focus more on internal specs than on build strength.");
}
// Special
if (special === "backlit") {
summaryParts.push("You want a backlit keyboard for comfortable typing in low light.");
} else if (special === "fingerprint") {
summaryParts.push("You like quick and secure login with fingerprint or similar features.");
} else {
summaryParts.push("You do not require any very special extra features.");
}
const summaryText = summaryParts.join(" ");
// Basic spec logic
let ram = "8 GB RAM";
let storage = "256 GB SSD";
let gpu = "integrated graphics";
let sizeText = (screen === "small") ? "14 inch" : (screen === "big" ? "15.6 inch" : "14–15.6 inch");
if (use === "office") {
ram = "16 GB RAM";
storage = "512 GB SSD";
} else if (use === "creator" || use === "gaming") {
ram = "16 GB or 32 GB RAM";
storage = "512 GB or 1 TB SSD";
gpu = "dedicated graphics (at least entry‑level)";
}
if (budget === "low") {
if (use === "gaming" || use === "creator") {
ram = "8 GB RAM (minimum, future upgrade recommended)";
gpu = "basic or older dedicated graphics if available in budget";
}
} else if (budget === "medium") {
storage = (use === "basic") ? "512 GB SSD" : "512 GB or 1 TB SSD";
} else if (budget === "high") {
if (use === "gaming" || use === "creator") {
ram = "16 GB or 32 GB RAM";
storage = "1 TB SSD";
gpu = "strong dedicated graphics (gaming/creator series)";
} else {
ram = "16 GB RAM";
storage = "512 GB or 1 TB SSD";
}
}
let brandText = "";
if (brand === "any") {
brandText = "Brand is flexible, so we can focus on the best value model from trusted brands.";
} else {
brandText = "You prefer brand " + brand.toUpperCase() + ", so we will try to stay inside that brand family if the specs match.";
}
let batteryText = "";
if (battery === "long") {
batteryText = "Battery should be a model known for good backup, especially if you travel or attend long classes.";
} else if (battery === "medium") {
batteryText = "Normal 4–6 hour real life battery will be enough.";
} else {
batteryText = "Battery backup is less priority, so we can focus more on performance/price.";
}
let osText = (os === "windows")
? "Windows will be the main OS, which is perfect for normal use, office, and most games."
: "OS is flexible, so Windows will be default, but you are open if any other option gives better value.";
let touchText = "";
if (touch === "must") {
touchText = "Touch screen is required, so we will look at 2‑in‑1 or touch‑enabled models.";
} else if (touch === "nice") {
touchText = "Touch screen is nice but not compulsory, so if value is better without touch, we can skip it.";
} else {
touchText = "Touch screen is not required, normal non‑touch display is fine.";
}
let buildText = "";
if (build === "high") {
buildText = "Strong build quality and solid hinge are important, so metal body or well‑reviewed sturdy design will be preferred.";
} else if (build === "medium") {
buildText = "Medium build quality is okay as long as the laptop feels decent and not very flimsy.";
} else {
buildText = "Build is not your highest priority, so internal configuration can take priority within your budget.";
}
let specialText = "";
if (special === "backlit") {
specialText = "Backlit keyboard is important, so we will filter for models with keyboard backlight.";
} else if (special === "fingerprint") {
specialText = "Fingerprint / fast login is important, so models with fingerprint reader or similar security feature are preferred.";
} else {
specialText = "No special extra features are required beyond normal good specs.";
}
const specLines = [];
specLines.push(`• Use case: ${use === "basic" ? "basic everyday work" : use === "office" ? "office / coding / study work" : use === "creator" ? "creator & editing work" : "gaming / heavy tasks"}.`);
specLines.push(`• Recommended RAM: ${ram}.`);
specLines.push(`• Recommended storage: ${storage}.`);
specLines.push(`• Graphics: ${gpu}.`);
specLines.push(`• Screen size: around ${sizeText}.`);
let linkTextHtml = `
<a href="https://www.reliancedigital.in/laptops/c/S101210" target="_blank">
  Open Reliance Digital laptop section
</a>
<br>
You can use filters there for RAM, SSD, brand (${brand.toUpperCase()} or others), screen size and price to match this summary.
`;
document.getElementById('summaryText').textContent = summaryText;
document.getElementById('specText').innerHTML = specLines.join('<br>') +
"<br><br>" + brandText + " " + batteryText + " " + osText + " " + touchText + " " + buildText + " " + specialText;
document.getElementById('linkText').innerHTML = linkTextHtml;
resultBlock.classList.remove('hidden');
resultBlock.scrollIntoView({ behavior: 'smooth', block: 'start' });
}
</script>
</body>
</html>
