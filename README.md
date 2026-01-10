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

    .bg-orbit {
      position: fixed;
      inset: -40%;
      background:
        radial-gradient(circle at 10% 20%, rgba(56,189,248,0.28), transparent 55%),
        radial-gradient(circle at 80% 0%, rgba(37,99,235,0.32), transparent 55%),
        radial-gradient(circle at 50% 90%, rgba(129,140,248,0.22), transparent 60%);
      opacity: 0.92;
      pointer-events: none;
      z-index: -2;
    }

    .bg-stripes {
      position: fixed;
      inset: 0;
      background-image:
        linear-gradient(120deg, rgba(15,23,42,0.96) 0, rgba(15,23,42,0.92) 34%, transparent 65%),
        linear-gradient(150deg, rgba(30,64,175,0.25) 0, transparent 55%);
      mix-blend-mode: screen;
      opacity: 0.9;
      pointer-events: none;
      z-index: -1;
    }

    .flare-left {
      position: fixed;
      width: 360px;
      height: 360px;
      background: radial-gradient(circle, rgba(59,130,246,0.35), transparent 60%);
      left: -120px;
      top: 18%;
      filter: blur(1px);
      opacity: 0.8;
      pointer-events: none;
      z-index: -1;
    }

    .flare-right {
      position: fixed;
      width: 400px;
      height: 400px;
      background: radial-gradient(circle, rgba(6,182,212,0.32), transparent 60%);
      right: -120px;
      bottom: 8%;
      filter: blur(1px);
      opacity: 0.8;
      pointer-events: none;
      z-index: -1;
    }

    .floor-glow {
      position: fixed;
      width: 160%;
      height: 260px;
      background: radial-gradient(ellipse at center, rgba(15,23,42,0.0), rgba(15,23,42,1) 70%);
      bottom: -140px;
      left: -30%;
      z-index: -1;
    }

    .container {
      width: 100%;
      max-width: 860px;
      padding: 24px;
    }

    .assistant-card {
      position: relative;
      border-radius: 26px;
      padding: 22px 26px 20px;
      background: radial-gradient(circle at top left, rgba(96,165,250,0.16), transparent 60%),
                  radial-gradient(circle at bottom right, rgba(59,130,246,0.14), transparent 55%),
                  linear-gradient(135deg, rgba(15,23,42,0.97), rgba(15,23,42,0.94));
      backdrop-filter: blur(28px);
      border: 1px solid rgba(148,163,184,0.28);
      box-shadow:
        0 30px 80px rgba(15,23,42,0.9),
        0 0 80px rgba(37,99,235,0.55);
      overflow: hidden;
    }

    .accent-orbit {
      position: absolute;
      width: 260px;
      height: 260px;
      border-radius: 999px;
      border: 1px solid rgba(59,130,246,0.55);
      opacity: 0.7;
      top: -140px;
      right: -40px;
      filter: blur(1px);
    }

    .accent-orbit::before {
      content: "";
      position: absolute;
      inset: 18%;
      border-radius: inherit;
      border: 1px dashed rgba(96,165,250,0.5);
      opacity: 0.55;
      transform: rotate(14deg);
    }

    .accent-orbit::after {
      content: "";
      position: absolute;
      width: 19px;
      height: 19px;
      border-radius: 999px;
      background: radial-gradient(circle, #60a5fa, transparent 70%);
      top: 16%;
      right: 14%;
      box-shadow: 0 0 22px rgba(96,165,250,1);
    }

    .assistant-card::before {
      content: "";
      position: absolute;
      width: 220px;
      height: 220px;
      border-radius: 50%;
      background:
        radial-gradient(circle at 30% 20%, rgba(248,250,252,0.14), transparent 50%),
        radial-gradient(circle at 70% 80%, rgba(59,130,246,0.2), transparent 60%);
      opacity: 0.16;
      top: -60px;
      left: -40px;
      filter: blur(1px);
    }

    .assistant-card::after {
      content: "";
      position: absolute;
      width: 280px;
      height: 280px;
      border-radius: 50%;
      background:
        radial-gradient(circle at 40% 40%, rgba(56,189,248,0.32), transparent 58%),
        radial-gradient(circle at 60% 70%, rgba(29,78,216,0.28), transparent 60%);
      opacity: 0.18;
      bottom: -90px;
      right: -60px;
      filter: blur(1px);
    }

    .glow-bottom {
      position: absolute;
      width: 120%;
      height: 150px;
      background: radial-gradient(ellipse at center, rgba(59,130,246,0.35), transparent 70%);
      bottom: -90px;
      left: -10%;
      opacity: 0.5;
      pointer-events: none;
    }

    .header-row {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 14px;
      margin-bottom: 14px;
      position: relative;
      z-index: 2;
    }

    .brand-block {
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .brand-pill {
      width: 38px;
      height: 38px;
      border-radius: 14px;
      background: radial-gradient(circle at 30% 25%, #eff6ff, #60a5fa);
      box-shadow:
        0 0 14px rgba(191,219,254,0.7),
        0 10px 25px rgba(30,64,175,0.85);
      display: flex;
      align-items: center;
      justify-content: center;
      color: #0b1120;
      font-weight: 700;
      font-size: 15px;
      letter-spacing: 0.03em;
    }

    .brand-text {
      display: flex;
      flex-direction: column;
      gap: 1px;
    }

    .brand-title {
      font-size: 13px;
      text-transform: uppercase;
      letter-spacing: 0.18em;
      color: #e5e7eb;
      opacity: 0.92;
    }

    .brand-sub {
      font-size: 11px;
      color: #9ca3af;
      letter-spacing: 0.06em;
      text-transform: uppercase;
    }

    .info-block {
      display: flex;
      flex-direction: column;
      align-items: flex-end;
      gap: 4px;
    }

    .info-label {
      font-size: 10px;
      letter-spacing: 0.18em;
      text-transform: uppercase;
      color: #9ca3af;
      opacity: 0.85;
    }

    .tag-row {
      display: flex;
      gap: 6px;
      flex-wrap: wrap;
      justify-content: flex-end;
    }

    .tag-pill {
      font-size: 10px;
      text-transform: uppercase;
      letter-spacing: 0.14em;
      padding: 4px 9px;
      border-radius: 999px;
      background: linear-gradient(120deg, rgba(15,23,42,0.92), rgba(15,23,42,0.85));
      border: 1px solid rgba(148,163,184,0.4);
      color: #e5e7eb;
      display: inline-flex;
      align-items: center;
      gap: 5px;
    }

    .tag-dot {
      width: 7px;
      height: 7px;
      border-radius: 999px;
      background: radial-gradient(circle, #60a5fa, #1d4ed8);
      box-shadow: 0 0 10px rgba(59,130,246,0.9);
    }

    .main-layout {
      display: grid;
      grid-template-columns: minmax(0, 1.1fr) minmax(0, 0.95fr);
      gap: 18px;
      margin-top: 6px;
      position: relative;
      z-index: 2;
    }

    .question-panel {
      position: relative;
      background: linear-gradient(150deg, rgba(15,23,42,0.98), rgba(15,23,42,0.93));
      border-radius: 20px;
      padding: 16px 15px 15px;
      border: 1px solid rgba(51,65,85,0.85);
      box-shadow:
        inset 0 0 22px rgba(15,23,42,0.9),
        0 15px 40px rgba(15,23,42,0.85);
      overflow: hidden;
      min-height: 320px;
    }

    .panel-shine {
      position: absolute;
      inset: 0;
      background:
        linear-gradient(145deg, rgba(226,232,240,0.06), transparent 55%),
        radial-gradient(circle at top left, rgba(96,165,250,0.08), transparent 60%);
      mix-blend-mode: screen;
      opacity: 0.8;
      pointer-events: none;
    }

    .panel-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 4px;
    }

    .panel-title {
      font-size: 13px;
      letter-spacing: 0.16em;
      text-transform: uppercase;
      color: #9ca3af;
    }

    .step-indicator {
      font-size: 11px;
      color: #e5e7eb;
      letter-spacing: 0.12em;
      text-transform: uppercase;
    }

    .step-indicator span {
      color: #60a5fa;
    }

    .question-text {
      font-size: 17px;
      font-weight: 600;
      margin-top: 6px;
      margin-bottom: 12px;
      color: #e5e7eb;
      text-shadow: 0 0 12px rgba(15,23,42,0.9);
    }

    .question-sub {
      font-size: 11px;
      color: #9ca3af;
      margin-bottom: 12px;
      max-width: 90%;
    }

    .options-wrapper {
      position: relative;
      border-radius: 14px;
      background: radial-gradient(circle at top, rgba(15,23,42,0.95), rgba(15,23,42,0.98));
      border: 1px solid rgba(55,65,81,0.9);
      box-shadow: inset 0 0 16px rgba(15,23,42,1);
      padding: 8px 8px 6px;
      max-height: 200px;
      overflow-y: auto;
      scroll-behavior: smooth;
      scrollbar-width: thin;
      scrollbar-color: rgba(75,85,99,0.9) transparent;
    }

    .options-wrapper::-webkit-scrollbar {
      width: 5px;
    }

    .options-wrapper::-webkit-scrollbar-track {
      background: transparent;
    }

    .options-wrapper::-webkit-scrollbar-thumb {
      background: linear-gradient(to bottom, #4b5563, #1f2937);
      border-radius: 999px;
    }

    .option {
      display: flex;
      align-items: flex-start;
      gap: 8px;
      padding: 8px 9px;
      border-radius: 12px;
      margin-bottom: 5px;
      background: linear-gradient(120deg, rgba(17,24,39,0.9), rgba(15,23,42,0.96));
      border: 1px solid rgba(55,65,81,0.95);
      cursor: pointer;
      transition: all 0.18s ease-out;
    }

    .option:hover {
      border-color: rgba(96,165,250,0.9);
      box-shadow:
        0 0 0 1px rgba(59,130,246,0.5),
        0 10px 20px rgba(15,23,42,0.9);
      transform: translateY(-1px);
    }

    .option.selected {
      border-color: #60a5fa;
      box-shadow:
        0 0 0 1px rgba(59,130,246,0.8),
        0 0 25px rgba(37,99,235,0.9);
      background: radial-gradient(circle at top left, rgba(37,99,235,0.35), rgba(15,23,42,0.98));
    }

    .option-icon {
      width: 22px;
      height: 22px;
      border-radius: 999px;
      border: 1px solid rgba(148,163,184,0.9);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 11px;
      color: #e5e7eb;
      flex-shrink: 0;
      margin-top: 1px;
      background: radial-gradient(circle, rgba(15,23,42,1), rgba(30,64,175,0.7));
    }

    .option-text-main {
      font-size: 13px;
      color: #e5e7eb;
      margin-bottom: 1px;
    }

    .option-text-sub {
      font-size: 11px;
      color: #9ca3af;
    }

    .hint-row {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-top: 8px;
      font-size: 10px;
      color: #9ca3af;
    }

    .hint-pill {
      display: inline-flex;
      align-items: center;
      gap: 5px;
      padding: 4px 8px;
      border-radius: 999px;
      background: rgba(15,23,42,0.95);
      border: 1px solid rgba(55,65,81,1);
      box-shadow: inset 0 0 12px rgba(15,23,42,0.9);
    }

    .hint-dot {
      width: 7px;
      height: 7px;
      border-radius: 999px;
      background: radial-gradient(circle, #22c55e, #15803d);
      box-shadow: 0 0 10px rgba(34,197,94,0.9);
    }

    .controls-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-top: 10px;
      gap: 8px;
    }

    .nav-buttons {
      display: flex;
      gap: 8px;
    }

    .btn {
      border: none;
      outline: none;
      cursor: pointer;
      border-radius: 999px;
      padding: 7px 13px;
      font-size: 12px;
      letter-spacing: 0.14em;
      text-transform: uppercase;
      display: inline-flex;
      align-items: center;
      gap: 6px;
      transition: all 0.18s ease-out;
      white-space: nowrap;
    }

    .btn-prev {
      background: rgba(15,23,42,0.98);
      border: 1px solid rgba(55,65,81,1);
      color: #e5e7eb;
      box-shadow: 0 8px 18px rgba(15,23,42,0.9);
    }

    .btn-prev:hover {
      border-color: rgba(148,163,184,1);
      transform: translateY(-1px);
    }

    .btn-next {
      background: radial-gradient(circle at 30% 20%, #eff6ff, #60a5fa);
      color: #0b1120;
      border: 1px solid rgba(191,219,254,1);
      box-shadow:
        0 0 18px rgba(191,219,254,0.9),
        0 10px 25px rgba(30,64,175,0.9);
    }

    .btn-next:hover {
      box-shadow:
        0 0 22px rgba(191,219,254,1),
        0 12px 28px rgba(30,64,175,1);
      transform: translateY(-1px);
    }

    .btn:disabled {
      opacity: 0.4;
      cursor: default;
      box-shadow: none;
      transform: none;
    }

    .step-dots {
      display: flex;
      gap: 6px;
      align-items: center;
    }

    .step-dot {
      width: 6px;
      height: 6px;
      border-radius: 999px;
      background: rgba(55,65,81,1);
      opacity: 0.65;
      transition: all 0.18s ease-out;
    }

    .step-dot.active {
      width: 14px;
      border-radius: 999px;
      background: linear-gradient(90deg, #60a5fa, #22c55e);
      box-shadow: 0 0 14px rgba(56,189,248,0.9);
      opacity: 1;
    }

    .visual-panel {
      position: relative;
      border-radius: 20px;
      padding: 15px 14px 14px;
      background:
        radial-gradient(circle at top right, rgba(56,189,248,0.26), transparent 55%),
        linear-gradient(145deg, rgba(15,23,42,0.98), rgba(17,24,39,0.97));
      border: 1px solid rgba(51,65,85,0.95);
      box-shadow:
        inset 0 0 16px rgba(15,23,42,1),
        0 16px 40px rgba(15,23,42,0.95);
      overflow: hidden;
    }

    .visual-top-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 10px;
      gap: 8px;
    }

    .visual-title-block {
      display: flex;
      flex-direction: column;
      gap: 3px;
    }

    .visual-eyebrow {
      font-size: 10px;
      letter-spacing: 0.18em;
      text-transform: uppercase;
      color: #9ca3af;
    }

    .visual-main-title {
      font-size: 15px;
      font-weight: 600;
      color: #e5e7eb;
    }

    .visual-subtitle {
      font-size: 11px;
      color: #9ca3af;
    }

    .visual-chip-row {
      display: flex;
      flex-direction: column;
      align-items: flex-end;
      gap: 4px;
    }

    .visual-chip {
      font-size: 10px;
      letter-spacing: 0.16em;
      text-transform: uppercase;
      padding: 4px 9px;
      border-radius: 999px;
      background: rgba(15,23,42,0.94);
      border: 1px solid rgba(148,163,184,0.6);
      color: #e5e7eb;
      display: inline-flex;
      align-items: center;
      gap: 6px;
    }

    .visual-chip-badge {
      width: 16px;
      height: 16px;
      border-radius: 999px;
      background: radial-gradient(circle, #22c55e, #166534);
      box-shadow: 0 0 14px rgba(34,197,94,0.9);
    }

    .visual-main-box {
      position: relative;
      margin-top: 7px;
      border-radius: 18px;
      padding: 10px 11px 10px;
      background:
        radial-gradient(circle at top left, rgba(59,130,246,0.38), transparent 62%),
        radial-gradient(circle at bottom right, rgba(6,182,212,0.26), transparent 60%),
        linear-gradient(135deg, rgba(15,23,42,0.98), rgba(15,23,42,0.96));
      border: 1px solid rgba(30,64,175,0.85);
      box-shadow:
        inset 0 0 22px rgba(15,23,42,1),
        0 12px 30px rgba(15,23,42,1);
      overflow: hidden;
    }

    .visual-laptop {
      position: relative;
      width: 100%;
      aspect-ratio: 16/9;
      border-radius: 15px;
      overflow: hidden;
      border: 1px solid rgba(148,163,184,0.85);
      background:
        radial-gradient(circle at 10% 0%, rgba(239,246,255,0.16), transparent 55%),
        radial-gradient(circle at 90% 80%, rgba(59,130,246,0.38), transparent 60%),
        linear-gradient(135deg, rgba(15,23,42,0.98), rgba(15,23,42,0.96));
      box-shadow:
        0 18px 35px rgba(15,23,42,1),
        0 0 24px rgba(37,99,235,0.75);
    }

    .visual-laptop-inner {
      position: absolute;
      inset: 0;
      padding: 12px 14px;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
    }

    .visual-laptop-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 8px;
    }

    .visual-label-block {
      display: flex;
      flex-direction: column;
      gap: 3px;
    }

    .visual-label {
      font-size: 11px;
      letter-spacing: 0.18em;
      text-transform: uppercase;
      color: #e5e7eb;
    }

    .visual-label-sub {
      font-size: 10px;
      color: #cbd5f5;
    }

    .visual-mini-badges {
      font-size: 9px;
      color: #e5e7eb;
      display: flex;
      flex-wrap: wrap;
      gap: 4px;
    }

    .visual-mini-badge {
      padding: 2px 7px;
      border-radius: 999px;
      border: 1px solid rgba(148,163,184,0.8);
      background: rgba(15,23,42,0.94);
    }

    .visual-laptop-footer {
      display: flex;
      justify-content: space-between;
      align-items: flex-end;
      gap: 8px;
    }

    .visual-question-context {
      max-width: 60%;
      font-size: 11px;
      color: #e5e7eb;
    }

    .visual-progress-bar {
      flex: 1;
      height: 6px;
      border-radius: 999px;
      background: rgba(15,23,42,0.9);
      border: 1px solid rgba(51,65,85,0.95);
      overflow: hidden;
      box-shadow: inset 0 0 8px rgba(15,23,42,1);
    }

    .visual-progress-fill {
      height: 100%;
      width: 18%;
      border-radius: inherit;
      background: linear-gradient(90deg, #22c55e, #60a5fa);
      box-shadow: 0 0 12px rgba(56,189,248,0.9);
      transition: width 0.25s ease-out;
    }

    .visual-footer-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-top: 9px;
      font-size: 10px;
      color: #9ca3af;
    }

    .visual-footer-metrics {
      display: flex;
      gap: 10px;
      align-items: center;
    }

    .metric-pill {
      display: inline-flex;
      align-items: center;
      gap: 3px;
      padding: 3px 7px;
      border-radius: 999px;
      border: 1px solid rgba(75,85,99,1);
      background: rgba(15,23,42,0.95);
    }

    .metric-dot {
      width: 6px;
      height: 6px;
      border-radius: 999px;
      background: radial-gradient(circle, #38bdf8, #0ea5e9);
      box-shadow: 0 0 10px rgba(56,189,248,0.9);
    }

    .metric-dot-green {
      background: radial-gradient(circle, #22c55e, #16a34a);
      box-shadow: 0 0 10px rgba(34,197,94,0.9);
    }

    .visual-footer-hint {
      font-size: 10px;
      color: #9ca3af;
    }

    .result-block {
      margin-top: 14px;
      border-radius: 16px;
      padding: 11px 12px 11px;
      background: radial-gradient(circle at top left, rgba(59,130,246,0.32), transparent 60%),
                  linear-gradient(145deg, rgba(15,23,42,0.98), rgba(15,23,42,0.96));
      border: 1px solid rgba(37,99,235,0.9);
      box-shadow:
        0 0 18px rgba(37,99,235,0.85),
        0 18px 40px rgba(15,23,42,1);
      font-size: 12px;
      color: #e5e7eb;
      position: relative;
      overflow: hidden;
    }

    .result-block.hidden {
      display: none;
    }

    .result-title-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 6px;
      gap: 8px;
    }

    .result-title {
      font-size: 13px;
      letter-spacing: 0.16em;
      text-transform: uppercase;
      color: #bfdbfe;
    }

    .result-tag {
      font-size: 10px;
      padding: 3px 8px;
      border-radius: 999px;
      border: 1px solid rgba(191,219,254,1);
      background: rgba(15,23,42,0.94);
      color: #e5e7eb;
    }

    .result-body {
      display: grid;
      grid-template-columns: minmax(0, 1.2fr) minmax(0, 0.9fr);
      gap: 8px;
      margin-top: 6px;
    }

    .result-summary {
      font-size: 12px;
      color: #e5e7eb;
      line-height: 1.45;
    }

    .result-specs {
      font-size: 11px;
      color: #bfdbfe;
      line-height: 1.4;
    }

    .result-specs strong {
      color: #e5e7eb;
    }

    .result-link-row {
      margin-top: 8px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 8px;
      font-size: 11px;
      color: #9ca3af;
    }

    .result-link-row a {
      color: #bfdbfe;
      text-decoration: none;
      font-weight: 600;
    }

    .result-link-row a:hover {
      text-decoration: underline;
      color: #e5e7eb;
    }

    .result-glow {
      position: absolute;
      width: 220px;
      height: 220px;
      border-radius: 50%;
      background: radial-gradient(circle, rgba(59,130,246,0.4), transparent 60%);
      opacity: 0.35;
      bottom: -120px;
      right: -80px;
      pointer-events: none;
    }

    @media (max-width: 900px) {
      body {
        align-items: flex-start;
        padding-top: 18px;
      }
      .assistant-card {
        padding: 18px 16px 16px;
      }
      .main-layout {
        grid-template-columns: minmax(0, 1fr);
      }
      .visual-panel {
        margin-top: 10px;
      }
    }

    @media (max-width: 600px) {
      body {
        padding: 12px;
      }
      .assistant-card {
        border-radius: 20px;
      }
      .question-text {
        font-size: 16px;
      }
      .question-sub {
        font-size: 10px;
      }
      .options-wrapper {
        max-height: 220px;
      }
      .result-body {
        grid-template-columns: minmax(0, 1fr);
      }
    }
  </style>
</head>
<body>
<div class="bg-orbit"></div>
<div class="bg-stripes"></div>
<div class="flare-left"></div>
<div class="flare-right"></div>
<div class="floor-glow"></div>

<div class="container">
  <div class="assistant-card">
    <div class="accent-orbit"></div>
    <div class="glow-bottom"></div>

    <div class="header-row">
      <div class="brand-block">
        <div class="brand-pill">AI</div>
        <div class="brand-text">
          <div class="brand-title">Cinematic Laptop Guide</div>
          <div class="brand-sub">New Delhi • Reliance style</div>
        </div>
      </div>
      <div class="info-block">
        <div class="info-label">Flow status</div>
        <div class="tag-row">
          <div class="tag-pill">
            <span class="tag-dot"></span>
            <span id="tagStepLabel">Step 1 • Intro</span>
          </div>
          <div class="tag-pill">
            <span class="tag-dot"></span>
            <span id="tagMoodLabel">Exploring options</span>
          </div>
        </div>
      </div>
    </div>

    <div class="main-layout">
      <div class="question-panel">
        <div class="panel-shine"></div>

        <div class="panel-header">
          <div class="panel-title">Laptop matchmaking</div>
          <div class="step-indicator">
            Question <span id="stepNumber">1</span> / <span id="totalSteps">15</span>
          </div>
        </div>

        <div class="question-text" id="questionText"></div>
        <div class="question-sub" id="questionSub"></div>

        <div class="options-wrapper" id="optionsWrapper"></div>

        <div class="hint-row">
          <div class="hint-pill">
            <div class="hint-dot"></div>
            <span id="hintText">Tap only one option per step</span>
          </div>
          <span>Each choice refines your perfect match</span>
        </div>

        <div class="controls-row">
          <div class="nav-buttons">
            <button class="btn btn-prev" id="prevBtn">
              ← Back
            </button>
            <button class="btn btn-next" id="nextBtn">
              Next →
            </button>
          </div>
          <div class="step-dots" id="stepDots"></div>
        </div>
      </div>

      <div class="visual-panel">
        <div class="visual-top-row">
          <div class="visual-title-block">
            <div class="visual-eyebrow">Question focus</div>
            <div class="visual-main-title" id="visualTitle">Usage pattern</div>
            <div class="visual-subtitle" id="visualSubtitle">
              Tell how you will actually use your laptop.
            </div>
          </div>
          <div class="visual-chip-row">
            <div class="visual-chip">
              <div class="visual-chip-badge"></div>
              <span id="visualChipText">Cinematic guidance</span>
            </div>
          </div>
        </div>

        <div class="visual-main-box">
          <div class="visual-laptop">
            <div class="visual-laptop-inner">
              <div class="visual-laptop-header">
                <div class="visual-label-block">
                  <div class="visual-label" id="visualLabelText">Screen • Comfort • Power</div>
                  <div class="visual-label-sub" id="visualLabelSub">
                    Balancing performance vs portability for daily life.
                  </div>
                </div>
                <div class="visual-mini-badges">
                  <div class="visual-mini-badge" id="miniBadge1">Daily use</div>
                  <div class="visual-mini-badge" id="miniBadge2">Movies & work</div>
                </div>
              </div>

              <div class="visual-laptop-footer">
                <div class="visual-question-context" id="visualContext">
                  Pick the option that feels closest to your real-life usage. No need to overthink.
                </div>
                <div class="visual-progress-bar">
                  <div class="visual-progress-fill" id="progressFill"></div>
                </div>
              </div>
            </div>
          </div>

          <div class="visual-footer-row">
            <div class="visual-footer-metrics">
              <div class="metric-pill">
                <span class="metric-dot"></span>
                <span id="metricLeft">Profile clarity: 18%</span>
              </div>
              <div class="metric-pill">
                <span class="metric-dot metric-dot-green"></span>
                <span id="metricRight">Match quality: warming up</span>
              </div>
            </div>
            <div class="visual-footer-hint">
              Each answer locks one more preference into your ideal Reliance Digital laptop.
            </div>
          </div>
        </div>

        <div class="result-block hidden" id="resultBlock">
          <div class="result-title-row">
            <div class="result-title">Your cinematic laptop match</div>
            <div class="result-tag">Generated from your 15 answers</div>
          </div>
          <div class="result-body">
            <div class="result-summary" id="resultSummary"></div>
            <div class="result-specs" id="resultSpecs"></div>
          </div>
          <div class="result-link-row">
            <span>Open on Reliance Digital for live price & offers:</span>
            <span id="linkText"></span>
          </div>
          <div class="result-glow"></div>
        </div>
      </div>
    </div>
  </div>
</div>

<script>
const questions = [
  {
    id: "usage",
    title: "What will you mostly do on this laptop?",
    sub: "Your main use decides if we prioritise power, battery, or comfort.",
    options: [
      { id: "office_basic", label: "Office work, browsing & YouTube", detail: "Docs, Excel, emails, Chrome, OTT, and light usage." },
      { id: "student_mix", label: "Study + a bit of everything", detail: "Notes, lectures, little editing, light coding, some entertainment." },
      { id: "creator", label: "Photo / video editing & design", detail: "Premiere, Photoshop, Canva, thumbnails, reels, or client videos." },
      { id: "gaming", label: "Gaming + entertainment", detail: "AAA titles, esports, and GPU-heavy modern games." },
      { id: "programming", label: "Coding, software dev & tools", detail: "VS Code, Android Studio, Docker, local servers, heavy multi-tasking." },
      { id: "travel_casual", label: "Travel, office + casual use", detail: "Light, slim laptop for meetings, travel, and media." }
    ],
    visual: {
      title: "Usage pattern",
      subtitle: "Tell how you will actually use your laptop.",
      chip: "Daily workload focus",
      label: "Usage • Workload • Mood",
      labelSub: "Mix of entertainment, office work, and potential editing.",
      badge1: "Usage core",
      badge2: "Daily reality",
      context: "Pick what feels 60–70% accurate. This step sets the laptop’s performance level."
    }
  },
  {
    id: "budget",
    title: "What is your comfortable budget range?",
    sub: "Be honest about a range where you feel okay, not stressed.",
    options: [
      { id: "under_30", label: "Under ₹30,000", detail: "Just basic tasks, no heavy expectations." },
      { id: "30_40", label: "₹30,000 – ₹40,000", detail: "Good for students & office with some comfort." },
      { id: "40_55", label: "₹40,000 – ₹55,000", detail: "Balanced performance, some editing/gaming possible." },
      { id: "55_75", label: "₹55,000 – ₹75,000", detail: "Serious performance for creator or gamer type." },
      { id: "75_100", label: "₹75,000 – ₹1,00,000", detail: "Premium feel, strong specs & future proof." },
      { id: "100_plus", label: "Above ₹1,00,000", detail: "High-end, top-tier experience and brand feel." }
    ],
    visual: {
      title: "Budget comfort",
      subtitle: "Price range where you feel comfortable and safe.",
      chip: "Smart money zone",
      label: "Budget • Comfort • Value",
      labelSub: "We target best value inside your comfort range, not overspend.",
      badge1: "Value focus",
      badge2: "Offer aware",
      context: "We stay near this range but may show close options if value is very high."
    }
  },
  {
    id: "size",
    title: "Which screen size do you prefer?",
    sub: "Screen size affects weight, bag fit, and eye comfort.",
    options: [
      { id: "compact_14", label: "Compact 14 inch", detail: "Easy to carry, good balance of size and portability." },
      { id: "standard_15", label: "Standard 15.6 inch", detail: "Big enough for movies and work, still easy to manage." },
      { id: "large_16", label: "16 inch or bigger", detail: "For editing, movies, or people who love big screens." },
      { id: "no_fixed", label: "No fixed preference", detail: "Okay with any size if overall laptop is good." }
    ],
    visual: {
      title: "Screen & size",
      subtitle: "Portability vs comfort for your eyes.",
      chip: "Display comfort",
      label: "Size • Weight • Travel",
      labelSub: "Your bag, desk and lifestyle decide the sweet spot.",
      badge1: "Portability",
      badge2: "Visual comfort",
      context: "If you travel a lot, smaller is better. If you edit or watch movies, bigger feels nicer."
    }
  },
  {
    id: "weight",
    title: "How important is light weight?",
    sub: "Think about walking, metro, office bag, and daily carrying.",
    options: [
      { id: "ultra_light", label: "Very important, want light & slim", detail: "Carry daily, want something that feels almost like a file." },
      { id: "balanced_weight", label: "Medium important, balance is fine", detail: "Okay with some weight if performance is strong." },
      { id: "not_important", label: "Not very important", detail: "Mostly stays on table; rarely carried outside." }
    ],
    visual: {
      title: "Portability factor",
      subtitle: "Your back, shoulders & travel routine matter.",
      chip: "Carry comfort",
      label: "Weight • Travel • Daily use",
      labelSub: "Lighter laptops cost more but feel better daily.",
      badge1: "Slim factor",
      badge2: "Desk usage",
      context: "If you’ll carry it more than 3–4 days a week, weight starts mattering a lot."
    }
  },
  {
    id: "battery",
    title: "Battery backup expectation?",
    sub: "Imagine using it without charger in cafe, class, train, or office.",
    options: [
      { id: "basic_4", label: "4–5 hours is okay", detail: "Mostly near charger, not a big issue." },
      { id: "medium_6_7", label: "6–7 hours needed", detail: "College + some travel + power cuts." },
      { id: "long_8_plus", label: "8+ hours ideally", detail: "Serious travel, long classes, or full day office." }
    ],
    visual: {
      title: "Battery life",
      subtitle: "How long you want it to stay awake without charger.",
      chip: "Power freedom",
      label: "Battery • Mobility • Backup",
      labelSub: "Higher battery often comes with efficient processors.",
      badge1: "On-the-go",
      badge2: "Desk plugged",
      context: "If you mostly sit near a socket, we can push more performance instead of only battery."
    }
  },
  {
    id: "brand_feel",
    title: "Brand preference / image in your mind?",
    sub: "Honest feeling matters: service trust + design + image.",
    options: [
      { id: "no_strong", label: "No strong brand preference", detail: "Open to any trusted brand with good value." },
      { id: "hp_dell_lenovo", label: "HP / Dell / Lenovo type", detail: "Office + professional feel, classic brand image." },
      { id: "asus_gaming_creator", label: "ASUS / MSI type", detail: "Gaming/creator vibe, performance image." },
      { id: "thin_premium", label: "Thin & premium look", detail: "Like MacBook-type feel in Windows brands too." }
    ],
    visual: {
      title: "Brand & design",
      subtitle: "The name and look you feel proud to carry.",
      chip: "Brand comfort",
      label: "Trust • Image • Design",
      labelSub: "We keep service, resale, and reliability in mind.",
      badge1: "Classic brands",
      badge2: "Modern vibe",
      context: "If you’re flexible, we chase best value. If you want a particular feel, we respect that."
    }
  },
  {
    id: "os_choice",
    title: "Which operating system do you prefer?",
    sub: "This decides the ecosystem and software comfort.",
    options: [
      { id: "windows_only", label: "Windows only", detail: "Familiar, works with most software and games." },
      { id: "open_to_mac", label: "Open to macOS also", detail: "Okay to explore MacBook if budget and needs match." },
      { id: "no_strict", label: "No strict choice, just best fit", detail: "Comfortable to learn if laptop is strong value." }
    ],
    visual: {
      title: "OS comfort",
      subtitle: "The environment you’ll live in daily.",
      chip: "System choice",
      label: "Windows • Mac • Flexible",
      labelSub: "Most budget and gaming options are Windows.",
      badge1: "Familiar",
      badge2: "Exploring",
      context: "If you edit in Final Cut or Xcode, macOS matters. For games, Windows dominates."
    }
  },
  {
    id: "ram",
    title: "Minimum RAM you are comfortable with?",
    sub: "RAM controls how many things you can do smoothly at same time.",
    options: [
      { id: "8_ok", label: "8 GB is enough for me", detail: "Normal usage, light editing, basic gaming." },
      { id: "16_better", label: "16 GB preferred", detail: "Heavy Chrome tabs, editing, coding, or gaming." },
      { id: "32_pro", label: "32 GB or more", detail: "Serious creator / developer / multi-tasking." }
    ],
    visual: {
      title: "Memory & smoothness",
      subtitle: "How heavy your multitasking will be.",
      chip: "RAM comfort",
      label: "RAM • Multitasking • Future",
      labelSub: "Higher RAM keeps laptop usable for more years.",
      badge1: "Basic smooth",
      badge2: "Pro smooth",
      context: "If you love many Chrome tabs, 16 GB becomes a sweet spot in many cases."
    }
  },
  {
    id: "storage",
    title: "Storage style you prefer?",
    sub: "Think about speed vs how much data you keep locally.",
    options: [
      { id: "512_ssd", label: "512 GB SSD", detail: "Fast, enough for most users with some cleaning." },
      { id: "1tb_ssd", label: "1 TB SSD", detail: "Big local library: games, videos, photos." },
      { id: "256_plus_hdd", label: "256 GB + external / cloud", detail: "Light user, okay with external hard disk." }
    ],
    visual: {
      title: "Fast vs capacity",
      subtitle: "Balance between speed and storage size.",
      chip: "Storage style",
      label: "SSD • Capacity • Cloud",
      labelSub: "SSD is non‑negotiable for speed today.",
      badge1: "Speed",
      badge2: "Space",
      context: "If you store many videos/games locally, we push higher SSD or easy expansion."
    }
  },
  {
    id: "graphics",
    title: "Do you need dedicated graphics (GPU)?",
    sub: "This matters for gaming and heavier editing work.",
    options: [
      { id: "no_gpu", label: "No, integrated graphics is enough", detail: "Office, movies, browsing, light editing." },
      { id: "entry_gpu", label: "Entry GPU is okay", detail: "Casual gaming, basic video editing & design." },
      { id: "strong_gpu", label: "Strong GPU required", detail: "Serious gaming or 4K video editing, 3D work." }
    ],
    visual: {
      title: "Graphics need",
      subtitle: "How hard you push visuals and frames.",
      chip: "GPU level",
      label: "Graphics • FPS • Editing",
      labelSub: "Dedicated GPU costs more but unlocks heavy tasks.",
      badge1: "Casual",
      badge2: "Hardcore",
      context: "If you just watch content and do office work, integrated GPU saves money and battery."
    }
  },
  {
    id: "build_keyboard",
    title: "Build quality & keyboard feel?",
    sub: "This decides typing comfort and how solid the laptop feels.",
    options: [
      { id: "solid_keyboard", label: "Want solid build + good keyboard", detail: "Typing feel and body strength are important." },
      { id: "normal_ok", label: "Normal is okay", detail: "Not very sensitive as long as it works." },
      { id: "premium_finish", label: "Premium metal / sleek finish", detail: "Look and touch feel matter a lot." }
    ],
    visual: {
      title: "Typing & feel",
      subtitle: "Everyday comfort when hands are on keyboard.",
      chip: "Build & keys",
      label: "Chassis • Keyboard • Finish",
      labelSub: "Strong build helps for long term and travel.",
      badge1: "Solid feel",
      badge2: "Premium look",
      context: "If you type long hours or carry daily, a better build and keyboard feels worth it."
    }
  },
  {
    id: "camera_mic",
    title: "How important are camera & mic?",
    sub: "Think about online meetings, classes, and video calls.",
    options: [
      { id: "basic_ok", label: "Basic is okay", detail: "Rare video calls, not very serious about quality." },
      { id: "good_quality", label: "Need good, clear quality", detail: "Regular meetings, classes, or client calls." },
      { id: "external_ok", label: "Okay to use external webcam/mic", detail: "Flexible, can add accessories later." }
    ],
    visual: {
      title: "Online presence",
      subtitle: "Video calls and audio clarity expectations.",
      chip: "Cam & mic",
      label: "Calls • Classes • Meetings",
      labelSub: "Premium cameras are still rare in non‑Mac laptops.",
      badge1: "Call ready",
      badge2: "Accessory friendly",
      context: "If you attend many professional calls, better mic & camera become a quality‑of‑life upgrade."
    }
  },
  {
    id: "ports",
    title: "Ports & connectivity needs?",
    sub: "Think about pendrives, HDMI, SD card, and future accessories.",
    options: [
      { id: "basic_ports", label: "Just normal USB & HDMI", detail: "Basic mouse, keyboard, pendrive use." },
      { id: "creator_ports", label: "Need SD card / more USB", detail: "Camera users, creators, multiple devices." },
      { id: "typec_focus", label: "More Type‑C, maybe charging also", detail: "Modern accessories, docking, and fast transfer." }
    ],
    visual: {
      title: "Ports & future",
      subtitle: "How you plug into the world around.",
      chip: "Connectivity",
      label: "USB • HDMI • Type‑C",
      labelSub: "Type‑C helps with docks and displays later.",
      badge1: "Basic I/O",
      badge2: "Creator setup",
      context: "If you shoot photos/videos or present on displays, extra ports remove daily friction."
    }
  },
  {
    id: "future",
    title: "How many years should this laptop comfortably last?",
    sub: "Imagine when you’ll want to upgrade again.",
    options: [
      { id: "2_3_years", label: "Around 2–3 years", detail: "Okay to upgrade sooner, not very long term." },
      { id: "4_5_years", label: "Around 4–5 years", detail: "Want solid mid‑term value and smoothness." },
      { id: "5_plus_years", label: "5+ years ideally", detail: "Need something strong and future‑proof." }
    ],
    visual: {
      title: "Longevity plan",
      subtitle: "How long you want this laptop to stay relevant.",
      chip: "Future proofing",
      label: "Years • Upgrades • Value",
      labelSub: "More RAM/SSD + better CPU help for long term.",
      badge1: "Short cycle",
      badge2: "Long cycle",
      context: "If you want 5+ years, we choose components that age slowly and stay smooth."
    }
  },
  {
    id: "vibe",
    title: "What overall vibe do you want when you open this laptop?",
    sub: "This is about emotional feel: calm, powerful, or premium.",
    options: [
      { id: "calm_clean", label: "Calm, clean & simple", detail: "Minimalist, no extra RGB, just neat design." },
      { id: "powerful_machine", label: "Looks like a powerful machine", detail: "Aggressive, gamer or creator energy." },
      { id: "premium_classy", label: "Premium & classy", detail: "Sleek, modern, almost showroom feel." }
    ],
    visual: {
      title: "Emotional feel",
      subtitle: "How it should feel when you use it daily.",
      chip: "Laptop personality",
      label: "Calm • Powerful • Premium",
      labelSub: "Looks matter because you will see it every day.",
      badge1: "Clean look",
      badge2: "Bold look",
      context: "If the vibe matches your personality, you enjoy using it more and keep it longer."
    }
  }
];

const totalSteps = questions.length;
const answers = {};

let currentStep = 0;

const questionText = document.getElementById("questionText");
const questionSub = document.getElementById("questionSub");
const optionsWrapper = document.getElementById("optionsWrapper");
const prevBtn = document.getElementById("prevBtn");
const nextBtn = document.getElementById("nextBtn");
const stepNumber = document.getElementById("stepNumber");
const totalStepsSpan = document.getElementById("totalSteps");
const stepDotsContainer = document.getElementById("stepDots");
const tagStepLabel = document.getElementById("tagStepLabel");
const tagMoodLabel = document.getElementById("tagMoodLabel");
const hintText = document.getElementById("hintText");
const visualTitle = document.getElementById("visualTitle");
const visualSubtitle = document.getElementById("visualSubtitle");
const visualChipText = document.getElementById("visualChipText");
const visualLabelText = document.getElementById("visualLabelText");
const visualLabelSub = document.getElementById("visualLabelSub");
const miniBadge1 = document.getElementById("miniBadge1");
const miniBadge2 = document.getElementById("miniBadge2");
const visualContext = document.getElementById("visualContext");
const progressFill = document.getElementById("progressFill");
const metricLeft = document.getElementById("metricLeft");
const metricRight = document.getElementById("metricRight");
const resultBlock = document.getElementById("resultBlock");
const resultSummary = document.getElementById("resultSummary");
const resultSpecs = document.getElementById("resultSpecs");
const linkText = document.getElementById("linkText");

totalStepsSpan.textContent = totalSteps.toString();

function buildStepDots() {
  stepDotsContainer.innerHTML = "";
  for (let i = 0; i < totalSteps; i++) {
    const dot = document.createElement("div");
    dot.className = "step-dot" + (i === 0 ? " active" : "");
    stepDotsContainer.appendChild(dot);
  }
}
buildStepDots();

function updateUI() {
  const step = questions[currentStep];
  questionText.textContent = step.title;
  questionSub.textContent = step.sub;

  stepNumber.textContent = (currentStep + 1).toString();

  const dots = stepDotsContainer.querySelectorAll(".step-dot");
  dots.forEach((dot, index) => {
    dot.classList.toggle("active", index === currentStep);
  });

  const progressPercent = Math.round(((currentStep + 1) / totalSteps) * 100);
  progressFill.style.width = progressPercent + "%";
  metricLeft.textContent = "Profile clarity: " + progressPercent + "%";

  if (progressPercent < 30) {
    metricRight.textContent = "Match quality: warming up";
  } else if (progressPercent < 70) {
    metricRight.textContent = "Match quality: taking shape";
  } else if (progressPercent < 100) {
    metricRight.textContent = "Match quality: almost locked";
  } else {
    metricRight.textContent = "Match quality: locked";
  }

  tagStepLabel.textContent = "Step " + (currentStep + 1) + " • " + step.id.replace("_", " ").toUpperCase();

  if (currentStep === 0) {
    tagMoodLabel.textContent = "Exploring options";
  } else if (currentStep < 4) {
    tagMoodLabel.textContent = "Defining basics";
  } else if (currentStep < 9) {
    tagMoodLabel.textContent = "Tuning experience";
  } else {
    tagMoodLabel.textContent = "Finalising profile";
  }

  visualTitle.textContent = step.visual.title;
  visualSubtitle.textContent = step.visual.subtitle;
  visualChipText.textContent = step.visual.chip;
  visualLabelText.textContent = step.visual.label;
  visualLabelSub.textContent = step.visual.labelSub;
  miniBadge1.textContent = step.visual.badge1;
  miniBadge2.textContent = step.visual.badge2;
  visualContext.textContent = step.visual.context;

  hintText.textContent = currentStep === 0
    ? "Tap only one option per step"
    : currentStep === totalSteps - 1
      ? "Last question. Your cinematic match is ready next."
      : "You can go back to adjust previous answers any time.";

  prevBtn.disabled = currentStep === 0;
  nextBtn.textContent = currentStep === totalSteps - 1 ? "Finish" : "Next →";

  optionsWrapper.innerHTML = "";

  step.options.forEach((opt, index) => {
    const optionDiv = document.createElement("div");
    optionDiv.className = "option";
    if (answers[step.id] === opt.id) {
      optionDiv.classList.add("selected");
    }

    const iconSpan = document.createElement("div");
    iconSpan.className = "option-icon";
    iconSpan.textContent = index + 1;

    const textContainer = document.createElement("div");
    const mainLabel = document.createElement("div");
    mainLabel.className = "option-text-main";
    mainLabel.textContent = opt.label;

    const subLabel = document.createElement("div");
    subLabel.className = "option-text-sub";
    subLabel.textContent = opt.detail;

    textContainer.appendChild(mainLabel);
    textContainer.appendChild(subLabel);

    optionDiv.appendChild(iconSpan);
    optionDiv.appendChild(textContainer);

    optionDiv.addEventListener("click", () => {
      const optionElements = optionsWrapper.querySelectorAll(".option");
      optionElements.forEach(el => el.classList.remove("selected"));
      optionDiv.classList.add("selected");
      answers[step.id] = opt.id;
    });

    optionsWrapper.appendChild(optionDiv);
  });

  optionsWrapper.scrollTop = 0;
  resultBlock.classList.add("hidden");
}

function getRecommendation() {
  const usage = answers["usage"];
  const budget = answers["budget"];
  const gpu = answers["graphics"];
  const ram = answers["ram"];
  const future = answers["future"];
  const size = answers["size"];
  const vibe = answers["vibe"];

  let profileLine = "";
  let specLine = "";
  let linkHtml = "";
  let summaryText = "";

  if (usage === "gaming" || gpu === "strong_gpu") {
    profileLine = "High‑performance gaming & creator laptop with strong graphics and cooling.";
    specLine = "<strong>Specs idea:</strong> Ryzen 7 / Intel i7, 16 GB RAM, RTX series GPU, 512 GB–1 TB SSD, 15.6\" 144 Hz display.";
    linkHtml = `<a href="https://www.reliancedigital.in/laptops/c/S101210" target="_blank">Open Reliance Digital gaming laptops</a>`;
    summaryText =
      "Your answers show you lean towards a powerful gaming/creator machine that can handle heavy workloads and modern titles without feeling slow.";
  } else if (usage === "creator") {
    profileLine = "Creator‑focused laptop tuned for editing, thumbnails, and content work with colour‑friendly display.";
    specLine = "<strong>Specs idea:</strong> Intel i5 / Ryzen 5 or above, 16 GB RAM, fast SSD (512 GB or 1 TB), good IPS display, optional entry GPU.";
    linkHtml = `<a href="https://www.reliancedigital.in/laptops/c/S101210" target="_blank">Open Reliance Digital creator‑friendly laptops</a>`;
    summaryText =
      "Your profile suggests a content‑creator style usage where smooth editing, multiple tabs, and reliable display quality matter more than only raw FPS.";
  } else if (usage === "programming") {
    profileLine = "Developer‑friendly laptop good for coding, Android Studio, and multiple tools together.";
    specLine = "<strong>Specs idea:</strong> Intel i5 / Ryzen 5 or above, 16 GB RAM, 512 GB SSD, comfortable keyboard, 14–15.6\" full HD display.";
    linkHtml = `<a href="https://www.reliancedigital.in/laptops/c/S101210" target="_blank">Open Reliance Digital productivity laptops</a>`;
    summaryText =
      "Your choices match a developer or power‑user style workflow where RAM and SSD speed are more important than fancy RGB.";
  } else if (usage === "student_mix") {
    profileLine = "Balanced student + everyday use laptop that can study, browse, and handle light editing or coding.";
    specLine = "<strong>Specs idea:</strong> Intel i3/i5 or Ryzen 3/5, 8–16 GB RAM depending on budget, 512 GB SSD, 14–15.6\" display.";
    linkHtml = `<a href="https://www.reliancedigital.in/laptops/c/S101210" target="_blank">Open Reliance Digital student laptops</a>`;
    summaryText =
      "Your profile fits a student‑style everyday machine that feels responsive for notes, browsing, classes, and light creative experiments.";
  } else if (usage === "travel_casual") {
    profileLine = "Slim, travel‑friendly laptop with decent battery and clean design.";
    specLine = "<strong>Specs idea:</strong> 14\" thin‑and‑light, Intel i5 / Ryzen 5, 8–16 GB RAM, 512 GB SSD, strong battery focus.";
    linkHtml = `<a href="https://www.reliancedigital.in/laptops/c/S101210" target="_blank">Open Reliance Digital thin & light laptops</a>`;
    summaryText =
      "Your answers point to a light, easy‑to‑carry machine that looks neat, lasts through the day, and handles everyday work comfortably.";
  } else {
    profileLine = "Comfort‑oriented everyday laptop for office, browsing, OTT, and light tasks.";
    specLine = "<strong>Specs idea:</strong> Intel i3/i5 or Ryzen 3/5, 8 GB RAM, 512 GB SSD, 14–15.6\" display, focus on comfort and reliability.";
    linkHtml = `<a href="https://www.reliancedigital.in/laptops/c/S101210" target="_blank">Open Reliance Digital everyday laptops</a>`;
    summaryText =
      "Your selections show a normal everyday user pattern where reliability, comfort, and good value per rupee matter more than extreme specs.";
  }

  let budgetNote = "";
  if (budget === "under_30") {
    budgetNote = "We’ll stay in entry‑level territory, focusing on basic smoothness and reliability over fancy features.";
  } else if (budget === "30_40") {
    budgetNote = "Within this range, we aim for a laptop that still feels modern, with SSD and decent RAM, without stretching too much.";
  } else if (budget === "40_55") {
    budgetNote = "This range allows a nice balance of performance and features, ideal for students and mixed users.";
  } else if (budget === "55_75") {
    budgetNote = "Here we can touch stronger processors, maybe dedicated graphics, and better build quality.";
  } else if (budget === "75_100") {
    budgetNote = "You’re in near‑premium territory where design, display quality, and long‑term comfort improve a lot.";
  } else if (budget === "100_plus") {
    budgetNote = "This budget opens doors to high‑end or near‑flagship options with strong build, displays, and performance.";
  }

  let futureNote = "";
  if (future === "2_3_years") {
    futureNote = "Your plan suggests this laptop can be a solid mid‑term companion without extreme future‑proofing.";
  } else if (future === "4_5_years") {
    futureNote = "So we tilt towards a configuration that will feel smooth for at least 4–5 years if handled properly.";
  } else if (future === "5_plus_years") {
    futureNote = "We’ll aim for a configuration that ages slowly: stronger CPU, more RAM, and SSD for long‑term comfort.";
  }

  let vibeNote = "";
  if (vibe === "calm_clean") {
    vibeNote = "Visually, a clean, non‑RGB, office‑friendly design will suit your personality the best.";
  } else if (vibe === "powerful_machine") {
    vibeNote = "Design‑wise, a gamer/creator vibe with bold lines or RGB will match your energy.";
  } else if (vibe === "premium_classy") {
    vibeNote = "A premium, showroom‑style, sleek finish will make you feel good every time you open the lid.";
  }

  resultSummary.innerHTML =
    "<strong>Reading your answers as a shop assistant:</strong><br>" +
    summaryText + "<br><br>" +
    budgetNote + " " + futureNote + " " + vibeNote;

  let ramLine = "";
  if (ram === "8_ok") {
    ramLine = "• RAM: 8 GB is workable if budget is tight and usage is lighter, but 16 GB feels safer for future.";
  } else if (ram === "16_better") {
    ramLine = "• RAM: 16 GB strongly recommended for your pattern; it keeps multitasking smooth for years.";
  } else if (ram === "32_pro") {
    ramLine = "• RAM: 32 GB is pro‑level; ideal if you handle very heavy projects, VMs, or big editing timelines.";
  }

  let sizeLine = "";
  if (size === "compact_14") {
    sizeLine = "• Size: 14\" compact body for easy travel and tight spaces.";
  } else if (size === "standard_15") {
    sizeLine = "• Size: 15.6\" standard body, good mix for movies and multitasking.";
  } else if (size === "large_16") {
    sizeLine = "• Size: 16\" or above for serious screen space and editing comfort.";
  } else if (size === "no_fixed") {
    sizeLine = "• Size: Flexible – we can choose between 14\" or 15.6\" based on best offer available.";
  }

  resultSpecs.innerHTML =
    profileLine + "<br><br>" +
    specLine + "<br>" +
    ramLine + "<br>" +
    sizeLine;

  linkText.innerHTML = linkHtml;

  resultBlock.classList.remove("hidden");
  resultBlock.scrollIntoView({ behavior: "smooth", block: "start" });
}

prevBtn.addEventListener("click", () => {
  if (currentStep > 0) {
    currentStep--;
    updateUI();
  }
});

nextBtn.addEventListener("click", () => {
  const currentQuestion = questions[currentStep];
  const chosen = answers[currentQuestion.id];

  if (!chosen) {
    hintText.textContent = "Please select one option before going ahead.";
    return;
  }

  if (currentStep < totalSteps - 1) {
    currentStep++;
    updateUI();
  } else {
    getRecommendation();
  }
});

updateUI();
</script>
</body>
</html>
