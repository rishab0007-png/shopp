<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Simple Laptop AI Assistant</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 600px;
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
  const requiredNames = ["use", "carry", "battery", "budget", "screen", "brand"];
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

  const summaryText = summaryParts.join(" ");

  // Now decide specs + example link
  let specText = "";
  let linkHtml = "";

  // Default values
  let ram = "8 GB RAM";
  let storage = "256 GB SSD";
  let gpu = "integrated graphics";
  let size = (screen === "small") ? "14 inch" : (screen === "big" ? "15.6 inch" : "14–15.6 inch");

  // Adjust by use
  if (use === "office") {
    ram = "16 GB RAM";
    storage = "512 GB SSD";
  } else if (use === "creator" || use === "gaming") {
    ram = "16 GB or 32 GB RAM";
    storage = "512 GB or 1 TB SSD";
    gpu = "dedicated graphics card";
  }

  // Adjust by budget (simple logic)
  if (budget === "low") {
    if (use === "basic") {
      ram = "8 GB RAM";
      storage = "256 GB SSD or more";
    }
  } else if (budget === "high") {
    if (use === "basic" || use === "office") {
      ram = "16 GB RAM";
      storage = "512 GB SSD";
    }
  }

  // Portability influence
  let weightNote = "";
  if (carry === "daily") {
    weightNote = "Try to keep the weight under about 1.5–1.6 kg for easy daily carrying.";
  } else if (carry === "sometimes") {
    weightNote = "Weight can be medium; up to around 1.8–2.0 kg is still okay.";
  } else {
    weightNote = "Weight is less important, you can also consider slightly heavier performance laptops.";
  }

  specText =
    "A good match for you would be a " +
    size + " laptop with " + ram + ", " + storage +
    ", and " + gpu + ". " + weightNote +
    " Also look for a battery that can give at least " +
    (battery === "long" ? "7–8+ hours" : battery === "medium" ? "4–6 hours" : "3–4 hours") +
    " of real use.";

  // Build brand and spec keywords for Reliance Digital search
  let baseUrl = "https://www.reliancedigital.in/search?q=";
  let keywords = "";

  // Add brand
  if (brand !== "any") {
    keywords += encodeURIComponent(brand + " laptop ");
  } else {
    keywords += encodeURIComponent("laptop ");
  }

  // Add spec hints
  if (use === "gaming") {
    keywords += encodeURIComponent("gaming ");
  } else if (use === "creator") {
    keywords += encodeURIComponent("creator ");
  }

  // RAM keywords
  if (ram.includes("32")) {
    keywords += encodeURIComponent("32gb ram ");
  } else if (ram.includes("16")) {
    keywords += encodeURIComponent("16gb ram ");
  } else {
    keywords += encodeURIComponent("8gb ram ");
  }

  // Storage keywords
  if (storage.includes("1 TB")) {
    keywords += encodeURIComponent("1tb ssd ");
  } else if (storage.includes("512")) {
    keywords += encodeURIComponent("512gb ssd ");
  } else {
    keywords += encodeURIComponent("256gb ssd ");
  }

  // Screen size hint
  if (screen === "small") {
    keywords += encodeURIComponent("14 inch ");
  } else if (screen === "big") {
    keywords += encodeURIComponent("15.6 inch ");
  }

  let finalUrl = baseUrl + keywords;

  linkHtml =
    'You can see matching laptops on Reliance Digital here: ' +
    '<a href="' + finalUrl + '" target="_blank">View laptops on Reliance Digital</a>.';

  document.getElementById('summaryText').innerText = summaryText;
  document.getElementById('specText').innerText = specText;
  document.getElementById('linkText').innerHTML = linkHtml;

  document.getElementById('result').classList.remove('hidden');
}
</script>

</body>
</html>
