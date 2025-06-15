# Fly-Boom<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>FlyBoom Game</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: linear-gradient(#001, #003);
      color: #fff;
      display: flex;
      flex-direction: column;
      align-items: center;
      height: 100vh;
      overflow: hidden;
    }
    header {
      font-size: 2rem;
      margin: 20px;
      color: #00e5ff;
    }
    #game-container {
      width: 90%;
      height: 60vh;
      background: #111;
      border: 2px solid #00e5ff;
      border-radius: 15px;
      position: relative;
      overflow: hidden;
    }
    #plane {
      position: absolute;
      top: 50%;
      left: 0;
      width: 50px;
      transform: translateY(-50%);
      transition: left 0.05s linear;
    }
    #multiplier {
      font-size: 2rem;
      margin: 20px;
    }
    #controls {
      margin: 20px;
    }
    button {
      font-size: 1rem;
      padding: 10px 20px;
      margin: 0 10px;
      background: #00e5ff;
      color: #000;
      border: none;
      border-radius: 8px;
      cursor: pointer;
    }
    #status {
      margin-top: 10px;
      font-size: 1.2rem;
    }
  </style>
</head>
<body>
  <header>🚀 FlyBoom</header>
  <div id="game-container">
    <img id="plane" src="https://cdn-icons-png.flaticon.com/512/1630/1630773.png" alt="Rocket" />
  </div>
  <div id="multiplier">Multiplier: 1.00x</div>
  <div id="controls">
    <button onclick="placeBet()">Place Bet</button>
    <button onclick="cashOut()">Cash Out</button>
  </div>
  <div id="status"></div>  <script>
    let multiplier = 1.00;
    let running = false;
    let interval;
    let crashed = false;
    const plane = document.getElementById("plane");
    const MAX_TIME = 80000; // 1 minute 20 seconds in milliseconds
    let startTime;

    function placeBet() {
      if (running) return;
      multiplier = 1.00;
      crashed = false;
      running = true;
      startTime = Date.now();
      plane.style.left = '0';
      document.getElementById("status").textContent = "";
      interval = setInterval(() => {
        multiplier += 0.01;
        document.getElementById("multiplier").textContent = `Multiplier: ${multiplier.toFixed(2)}x`;
        let left = parseFloat(plane.style.left);
        plane.style.left = (left + 0.5) + "%";

        if (Date.now() - startTime > MAX_TIME || multiplier >= getRandomCrashMultiplier()) {
          clearInterval(interval);
          crashed = true;
          running = false;
          document.getElementById("status").textContent = "💥 Plane Crashed!";
        }
      }, 100);
    }

    function cashOut() {
      if (running && !crashed) {
        clearInterval(interval);
        running = false;
        document.getElementById("status").textContent = `✅ You cashed out at ${multiplier.toFixed(2)}x!`;
      }
    }

    function getRandomCrashMultiplier() {
      return parseFloat((Math.random() * 10 + 2).toFixed(2)); // Random crash between 2x to 12x
    }
  </script></body>
</html>
