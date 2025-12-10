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
      background: #020617;
      overflow: hidden;
    }

    /* MAIN BLUE ANIMATED BACKGROUND */
    .bg-layer {
      position: fixed;
      inset: 0;
      z-index: -4;
      background: radial-gradient(circle at top, #0f172a 0, #020617 55%);
      overflow: hidden;
    }

    /* Base gradient shifts per step */
    .bg-step-1  { background: radial-gradient(circle at top, #0ea5e9 0, #020617 55%); }
    .bg-step-2  { background: radial-gradient(circle at top, #22c55e 0, #020617 55%); }
    .bg-step-3  { background: radial-gradient(circle at top, #eab308 0, #020617 55%); }
    .bg-step-4  { background: radial-gradient(circle at top, #6366f1 0, #020617 55%); }
    .bg-step-5  { background: radial-gradient(circle at top, #f97316 0, #020617 55%); }
    .bg-step-6  { background: radial-gradient(circle at top, #06b6d4 0, #020617 55%); }
    .bg-step-7  { background: radial-gradient(circle at top, #ec4899 0, #020617 55%); }
    .bg-step-8  { background: radial-gradient(circle at top, #facc15 0, #020617 55%); }
    .bg-step-9  { background: radial-gradient(circle at top, #22c55e 0, #020617 55%); }
    .bg-step-10 { background: radial-gradient(circle at top, #3b82f6 0, #020617 55%); }
    .bg-step-11 { background: radial-gradient(circle at top, #f97316 0, #020617 55%); }

    /* Floating particles – look like small laptop chips / dots */
    .particles {
      position: absolute;
      inset: 0;
      overflow: hidden;
      pointer-events: none;
    }
    .particle {
      position: absolute;
      border-radius: 999px;
      background: rgba(148, 163, 184, 0.15);
      box-shadow: 0 0 12px rgba(148, 163, 184, 0.4);
      animation: floatUp 18s linear infinite;
    }
    .particle-chip {
      border-radius: 8px;
      width: 70px;
      height: 40px;
      background: linear-gradient(135deg, rgba(15,23,42,0.9), rgba(56,189,248,0.6));
      box-shadow: 0 0 20px rgba(56,189,248,0.7);
    }

    .particle:nth-child(1) { width: 10px; height: 10px; left: 5%;  animation-duration: 25s; animation-delay: 0s; }
    .particle:nth-child(2) { width: 14px; height: 14px; left: 20%; animation-duration: 20s; animation-delay: 3s; }
    .particle:nth-child(3) { width: 8px;  height: 8px;  left: 40%; animation-duration: 18s; animation-delay: 6s; }
    .particle:nth-child(4) { width: 16px; height: 16px; left: 60%; animation-duration: 22s; animation-delay: 2s; }
    .particle:nth-child(5) { width: 12px; height: 12px; left: 80%; animation-duration: 19s; animation-delay: 4s; }

    /* One bigger "laptop chip" */
    .particle-chip {
      top: 110%;
      left: 70%;
      animation: floatChip 26s linear infinite;
    }

    @keyframes floatUp {
      0% {
        transform: translateY(40vh) translateX(0);
        opacity: 0;
      }
      10% {
        opacity: 0.6;
      }
      50% {
        transform: translateY(-40vh) translateX(20px);
        opacity: 0.8;
      }
      100% {
        transform: translateY(-80vh) translateX(-10px);
        opacity: 0;
      }
    }

    @keyframes floatChip {
      0% {
        transform: translateY(60vh) translateX(0) rotate(-8deg);
        opacity: 0;
      }
      15% {
        opacity: 0.7;
      }
      55% {
        transform: translateY(-10vh) translateX(-30px) rotate(4deg);
        opacity: 0.85;
      }
      100% {
        transform: translateY(-60vh) translateX(10px) rotate(0deg);
        opacity: 0;
      }
    }

    .overlay-gradient {
      position: fixed;
      inset: 0;
      background:
        radial-gradient(circle at top left, rgba(15,23,42,0.1), transparent 60%),
        radial-gradient(circle at bottom, rgba(15,23,42,0.96), rgba(15,23,42,0.98));
      z-index: -2;
    }

    .bg-laptop-shape {
      position: fixed;
      width: 520px;
      height: 320px;
      border-radius: 32px;
      border: 1px solid rgba(148,163,184,0.25);
      background: linear-gradient(135deg, rgba(15,23,42,0.95), rgba(30,64,175,0.7));
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

    .assistant-card {
      position: relative;
      background: linear-gradient(135deg, rgba(15,23,42,0.97), rgba(15,23,42,0.92));
      border-radius: 24px;
      padding: 28px 26px 24px;
      border: 1px solid rgba(148,163,184,0.35);
      box-shadow:
        0 18px 45px rgba(15,23,42,0.9),
        0 0 60px rgba(56,189,248,0.35);
      overflow: hidden;
    }

    .assistant-card::before {
      content: "";
      position: absolute;
      width: 320px;
      height: 320px;
      background: radial-gradient(circle, rgba(56,189,248,0.2), transparent 60%);
      top: -120px;
      right: -60px;
      opacity: 0.7;
      pointer-events: none;
    }

    .header-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 14px;
      gap: 12px;
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
      color: #a5b4fc;
      border-radius: 999px;
      padding: 4px 10px;
      border: 1px solid rgba(129,140,248,0.6);
      background: radial-gradient(circle at top, rgba(67,56,202,0.5), rgba(15,23,42,0.9));
      white-space: nowrap;
    }

    .question-visual {
      position: absolute;
      top: 20px;
      right: 22px;
      padding: 8px 12px;
      border-radius: 999px;
      background: rgba(15,23,42,0.85);
      border: 1px solid rgba(148,163,184,0.4);
      font-size: 0.8rem;
      display: inline-flex;
      align-items: center;
      gap: 6px;
      color: #e5e7eb;
      box-shadow: 0 8px 18px rgba(15,23,42,0.9);
    }
    .question-visual span.icon {
      font-size: 1.05rem;
    }

    .question-wrapper {
      position: relative;
      min-height: 220px;
      margin-top: 10px;
      overflow: hidden;
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
      color: #9ca3af;
      margin-bottom: 12px;
    }

    .option-label {
      display: block;
      margin-bottom: 6px;
      font-size: 0.9rem;
      padding: 6px 10px;
      border-radius: 999px;
      background: rgba(15,23,42,0.9);
      border: 1px solid rgba(148,163,184,0.35);
      cursor: pointer;
      transition: border 0.2s ease, background 0.2s ease, transform 0.1s ease;
    }
    .option-label:hover {
      border-color: rgba(56,189,248,0.9);
      transform: translateY(-1px);
    }
    .option-label input {
      margin-right: 8px;
      accent-color: #38bdf8;
    }

    .nav-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-top: 14px;
      gap: 10px;
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
      background: transparent;
      color: #e5e7eb;
      border: 1px solid rgba(148,163,184,0.7);
    }
    .btn-secondary:hover {
      background: rgba(15,23,42,0.9);
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
      transition: width 0.25s ease, background 0.25s ease;
    }
    .dot.active {
      width: 22px;
      background: #38bdf8;
    }

    #result {
      margin-top: 18px;
      padding: 14px 16px 12px;
      border-radius: 18px;
      border: 1px solid rgba(148,163,184,0.5);
      background: radial-gradient(circle at top left, rgba(56,189,248,0.12), rgba(15,23,42,0.96));
      max-height: 280px;
      overflow-y: auto;
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

    @media (max-width: 640px) {
      .assistant-card {
        padding: 20px 18px 16px;
        border-radius: 18px;
      }
      .question-wrapper {
        min-height: 260px;
      }
      h1 {
        font-size: 1.25rem;
      }
      .question-visual {
        display: none;
      }
    }
  </style>
</head>
<body>

<div class="bg-layer bg-step-1" id="bgLayer">
  <div class="particles">
    <div class="particle"></div>
    <div class="particle"></div>
    <div class="particle"></div>
    <div class="particle"></div>
    <div class="particle"></div>
    <div class="particle particle-chip"></div>
  </div>
</div>
<div class="overlay-gradient"></div>
<div class="bg-laptop-shape"></div>

<div class="container">
  <div class="assistant-card">
    <div class="header-row">
      <div class="title-block">
        <h1>Smart Laptop Helper</h1>
        <p>Answer step by step, get a smart summary and laptops from Reliance Digital.</p>
      </div>
      <div class="step-indicator" id="stepIndicator">Step 1 of 11</div>
    </div>

    <div class="question-visual" id="questionVisual">
      <span class="icon">💻</span>
      <span class="text">Main laptop usage</span>
    </div>

    <form id="quizForm">
      <div class="question-wrapper">
        <!-- All 11 questions are exactly as in previous version (omitted here for brevity) -->
        <!-- REUSE THE SAME QUESTION HTML FROM THE LAST ANSWER -->
        <!-- For space: copy from your previous working version's <div class="question-wrapper"> block -->
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
const totalSteps = 11;
let currentStep = 1;

const bgLayer = document.getElementById('bgLayer');
const stepIndicator = document.getElementById('stepIndicator');
const questions = document.querySelectorAll('.question');
const prevBtn = document.getElementById('prevBtn');
const nextBtn = document.getElementById('nextBtn');
const progressDotsContainer = document.getElementById('progressDots');
const resultBlock = document.getElementById('result');
const questionVisual = document.getElementById('questionVisual');

const visualTexts = {
  1: { icon: '💻', text: 'Main laptop usage' },
  2: { icon: '🎒', text: 'Light vs heavy to carry' },
  3: { icon: '🔋', text: 'Battery backup needs' },
  4: { icon: '💰', text: 'Budget comfort zone' },
  5: { icon: '🖥️', text: 'Screen size preference' },
  6: { icon: '🏷️', text: 'Brand choice' },
  7: { icon: '💾', text: 'Storage & big files' },
  8: { icon: '📆', text: 'How many years to use' },
  9: { icon: '⌨️', text: 'Typing & keyboard feel' },
 10: { icon: '🔌', text: 'Ports & connections' },
 11: { icon: '📹', text: 'Online classes & meetings' }
};

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
}

function validateCurrentStep() {
  const activeQuestion = document.querySelector('.question.active');
  const name = activeQuestion.querySelector('input[type="radio"]').name;
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

updateVisual();

/* REUSE YOUR EXISTING getRecommendation() FUNCTION FROM LAST VERSION HERE
   (the logic with summary, spec, and Reliance Digital link). */
</script>

</body>
</html>
