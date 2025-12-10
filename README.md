<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Simple Laptop AI Assistant</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 700px;
      margin: 20px auto;
      padding: 10px;
    }
    h1 {
      text-align: center;
    }
    .question {
      margin-bottom: 20px;
    }
    .question h3 {
      margin-bottom: 8px;
    }
    button {
      padding: 8px 16px;
      margin-top: 10px;
      cursor: pointer;
    }
    #result {
      margin-top: 25px;
      padding: 15px;
      border-radius: 6px;
      border: 1px solid #ccc;
      background: #f9f9f9;
    }
    .hidden {
      display: none;
    }
    .option-label {
      display: block;
      margin-bottom: 5px;
    }
  </style>
</head>
<body>

<h1>Laptop Helper Assistant</h1>
<p>Answer a few easy questions and get a laptop suggestion.</p>

<form id="quizForm">
  <!-- Q1: main use -->
  <div class="question">
    <h3>1. What will you mostly do on the laptop?</h3>
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

  <!-- Q2: portability -->
  <div class="question">
    <h3>2. Will you carry the laptop outside your home (college, office, travel)?</h3>
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

  <!-- Q3: battery -->
  <div class="question">
    <h3>3. Without charging, how many hours do you want it to run in a day?</h3>
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

  <!-- Q4: budget -->
  <div class="question">
    <h3>4. What is your budget range?</h3>
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
      High budget, I can spend more for good performance
    </label>
  </div>

  <!-- Q5: screen preference -->
  <div class="question">
    <h3>5. What do you prefer more?</h3>
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

  <!-- Q6: brand preference -->
  <div class="question">
    <h3>6. Do you want a specific laptop brand?</h3>
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

  <!-- Q7: storage/file size -->
  <div class="question">
    <h3>7. Do you store big files like games, videos or many photos?</h3>
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

  <!-- Q8: years of use -->
  <div class="question">
    <h3>8. For how many years do you want this laptop to feel good and usable?</h3>
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

  <!-- Q9: typing -->
  <div class="question">
    <h3>9. Do you type a lot (coding, writing, office work)?</h3>
    <label class="option-label">
      <input type="radio" name="typing" value="heavy" required>
      Yes, I type a lot
    </label>
    <label class="option-label">
      <input type="radio" name="typing" value="normal">
      Normal typing only
    </label>
  </div>

  <!-- Q10: ports -->
  <div class="question">
    <h3>10. Do you connect many devices (monitor, projector, LAN cable, USB devices)?</h3>
    <label class="option-label">
      <input type="radio" name="ports" value="many" required>
      Yes, I need many ports
    </label>
    <label class="option-label">
      <input type="radio" name="ports" value="normal">
      Normal ports are enough
    </label>
  </div>

  <!-- Q11: webcam / online meetings -->
  <div class="question">
    <h3>11. Do you attend many online classes or video meetings?</h3>
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

  <button type="button" onclick="getRecommendation()">Get my laptop suggestion</button>
</form>

<div id="result" class="hidden">
  <h2>Your laptop summary</h2>
  <p id="summaryText"></p>
  <h3>Suggested laptop type</h3>
  <p id="specText"></p>
  <h3>Example laptop link on Reliance Digital</h3>
  <p id="linkText"></p>
</div>

<script>
function getRecommendation() {
  const form = document.getElementById('quizForm');

  // Make sure all questions answered
  const requiredNames = [
    "use", "carry", "battery", "budget", "screen", "brand",
    "storageUse", "years", "typing", "ports", "webcam"
  ];
  for (let name of requiredNames) {
    const checked = form.querySelector('input[name="' + name + '"]:checked');
    if (!checked) {
      alert("Please answer all questions.");
      return;
    }
  }

  const use        = form.querySelector('input[name="use"]:checked').value;
  const carry      = form.querySelector('input[name="carry"]:checked').value;
  const battery    = form.querySelector('input[name="battery"]:checked').value;
  const budget     = form.querySelector('input[name="budget"]:checked').value;
  const screen     = form.querySelector('input[name="screen"]:checked').value;
  const brand      = form.querySelector('input[name="brand"]:checked').value;
  const storageUse = form.querySelector('input[name="storageUse"]:checked').value;
  const years      = form.querySelector('input[name="years"]:checked').value;
  const typing     = form.querySelector('input[name="typing"]:checked').value;
  const ports      = form.querySelector('input[name="ports"]:checked').value;
  const webcam     = form.querySelector('input[name="webcam"]:checked').value;

  // Build a simple natural summary
  let summaryParts = [];

  // Use summary
  if (use === "basic") {
    summaryParts.push("You mainly want a laptop for simple daily work like browsing, videos and basic office tasks.");
  } else if (use === "office") {
    summaryParts.push("You will use the laptop for office work, coding or study projects.");
  } else if (use === "creator") {
    summaryParts.push("You plan to do photo or video editing and other creative work.");
  } else if (use === "gaming") {
    summaryParts.push("You want to play games or do other heavy work on the laptop.");
  }

  // Portability summary
  if (carry === "daily") {
    summaryParts.push("You will carry the laptop almost every day, so it should be light and easy to carry.");
  } else if (carry === "sometimes") {
    summaryParts.push("You will carry the laptop sometimes, so medium weight is okay.");
  } else {
    summaryParts.push("You will rarely carry the laptop outside, so weight is not a big problem.");
  }

  // Battery summary
  if (battery === "long") {
    summaryParts.push("You want long battery life for many hours away from charging.");
  } else if (battery === "medium") {
    summaryParts.push("You need decent battery life for normal daily use.");
  } else {
    summaryParts.push("Battery life is not your main concern.");
  }

  // Budget summary
  if (budget === "low") {
    summaryParts.push("You have a low budget and want good value for money.");
  } else if (budget === "medium") {
    summaryParts.push("You have a medium budget and want a balance of price and performance.");
  } else {
    summaryParts.push("You are ready to pay more for better performance and features.");
  }

  // Screen summary
  if (screen === "small") {
    summaryParts.push("You prefer a smaller and lighter laptop size, around 13–14 inches.");
  } else if (screen === "big") {
    summaryParts.push("You prefer a bigger screen, around 15–16 inches, for comfortable viewing.");
  } else {
    summaryParts.push("Any screen size is fine for you.");
  }

  // Brand summary
  if (brand === "any") {
    summaryParts.push("You are open to any good and reliable brand.");
  } else {
    summaryParts.push("You prefer the brand: " + brand.toUpperCase() + ".");
  }

  // Storage usage summary
  if (storageUse === "heavy") {
    summaryParts.push("You store many big files like games, videos or a lot of photos.");
  } else if (storageUse === "medium") {
    summaryParts.push("You store some big files, but not too many.");
  } else {
    summaryParts.push("You mostly store small documents and light files.");
  }

  // Years of use summary
  if (years === "short") {
    summaryParts.push("You are okay if the laptop is good for around 1–2 years.");
  } else if (years === "medium") {
    summaryParts.push("You want the laptop to feel good for around 3–4 years.");
  } else {
    summaryParts.push("You want to keep this laptop for 5 or more years.");
  }

  // Typing summary
  if (typing === "heavy") {
    summaryParts.push("You type a lot, so a comfortable keyboard is important for you.");
  } else {
    summaryParts.push("You do normal typing, nothing very heavy.");
  }

  // Ports summary
  if (ports === "many") {
    summaryParts.push("You connect many devices, so you need enough ports (HDMI, USB, maybe LAN).");
  } else {
    summaryParts.push("Normal ports are enough for your use.");
  }

  // Webcam summary
  if (webcam === "often") {
    summaryParts.push("You attend many online classes or video meetings, so webcam and mic quality matters.");
  } else if (webcam === "sometimes") {
    summaryParts.push("You sometimes attend online meetings or classes.");
  } else {
    summaryParts.push("You rarely use the laptop for online video calls.");
  }

  const summaryText = summaryParts.join(" ");

  // Decide specs
  let ram = "8 GB RAM";
  let storage = "256 GB SSD";
  let gpu = "integrated graphics";
  let sizeText = (screen === "small") ? "14 inch" : (screen === "big" ? "15.6 inch" : "14–15.6 inch");

  // Base on use
  if (use === "office") {
    ram = "16 GB RAM";
    storage = "512 GB SSD";
  } else if (use === "creator" || use === "gaming") {
    ram = "16 GB or 32 GB RAM";
    storage = "512 GB or 1 TB SSD";
    gpu = "dedicated graphics card";
  }

  // Adjust by storage usage
  if (storageUse === "heavy") {
    storage = "512 GB or 1 TB SSD";
  } else if (storageUse === "medium" && storage === "256 GB SSD") {
    storage = "512 GB SSD";
  }

  // Adjust by years expectation
  if (years === "medium" || years === "long") {
    if (ram === "8 GB RAM") {
      ram = "16 GB RAM";
    }
  }

  // Budget influence (kept simple)
  if (budget === "low" && use === "basic" && storageUse !== "heavy") {
    ram = "8 GB RAM";
    storage = "256 GB SSD or more";
  }

  let weightNote = "";
  if (carry === "daily") {
    weightNote = "Try to keep the weight under about 1.5–1.6 kg for easy daily carrying.";
  } else if (carry === "sometimes") {
    weightNote = "Weight can be medium; up to around 1.8–2.0 kg is still okay.";
  } else {
    weightNote = "Weight is less important, you can also consider slightly heavier performance laptops.";
  }

  // Extra notes
  let extraNotes = [];
  if (typing === "heavy") {
    extraNotes.push("Look for a good quality, comfortable keyboard (preferably backlit).");
  }
  if (ports === "many") {
    extraNotes.push("Make sure the laptop has enough ports (HDMI, multiple USB ports, maybe LAN).");
  }
  if (webcam === "often") {
    extraNotes.push("Give importance to a good webcam and microphone for clear online meetings.");
  }

  const specText =
    "A good match for you would be a " +
    sizeText + " laptop with " + ram + ", " + storage +
    ", and " + gpu + ". " + weightNote +
    " Also look for a battery that can give at least " +
    (battery === "long" ? "7–8+ hours" : battery === "medium" ? "4–6 hours" : "3–4 hours") +
    " of real use." +
    (extraNotes.length ? " " + extraNotes.join(" ") : "");

  // ---- Reliance Digital URL (simplified search) ----
  let baseUrl = "https://www.reliancedigital.in/search?q=";
  let qParts = [];

  // brand
  if (brand !== "any") {
    qParts.push(brand);
  }
  qParts.push("laptop");

  // type hints
  if (use === "gaming") qParts.push("gaming");
  if (use === "creator") qParts.push("creator");

  // RAM hint
  if (ram.includes("32")) {
    qParts.push("32gb");
  } else if (ram.includes("16")) {
    qParts.push("16gb");
  } else {
    qParts.push("8gb");
  }

  // SSD hint
  if (storage.includes("1 TB")) {
    qParts.push("1tb");
  } else if (storage.includes("512")) {
    qParts.push("512gb");
  } else {
    qParts.push("256gb");
  }

  const query = encodeURIComponent(qParts.join(" "));
  const finalUrl = baseUrl + query;

  const linkHtml =
    'You can see matching laptops on Reliance Digital here: ' +
    '<a href="' + finalUrl + '" target="_blank" rel="noopener noreferrer">View laptops on Reliance Digital</a>.';

  document.getElementById('summaryText').innerText = summaryText;
  document.getElementById('specText').innerText = specText;
  document.getElementById('linkText').innerHTML = linkHtml;

  document.getElementById('result').classList.remove('hidden');
}
</script>

</body>
</html>
