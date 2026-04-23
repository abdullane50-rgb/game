<!DOCTYPE html>
<html>
<head>
  <title>Casino Pro</title>
  <style>
    body {
      margin: 0;
      font-family: Arial;
      background: #0b0f14;
      color: #fff;
    }

    header {
      display: flex;
      justify-content: space-between;
      padding: 15px 30px;
      background: #11161c;
      align-items: center;
    }

    header h1 {
      color: #00ffcc;
      font-size: 22px;
    }

    .balance {
      background: #1a222b;
      padding: 8px 15px;
      border-radius: 8px;
    }

    .container {
      display: flex;
    }

    /* sidebar */
    .sidebar {
      width: 200px;
      background: #0f141a;
      height: 100vh;
      padding-top: 20px;
    }

    .sidebar button {
      display: block;
      width: 90%;
      margin: 10px auto;
      padding: 12px;
      background: #1a222b;
      border: none;
      color: white;
      border-radius: 8px;
      cursor: pointer;
    }

    .sidebar button:hover {
      background: #00ffcc;
      color: black;
    }

    /* main */
    .main {
      flex: 1;
      padding: 20px;
      text-align: center;
    }

    canvas {
      background: #0f141a;
      border-radius: 10px;
      margin-top: 15px;
    }

    button.play {
      background: #00ffcc;
      color: black;
      padding: 10px 20px;
      border: none;
      border-radius: 6px;
      margin-top: 10px;
      cursor: pointer;
    }

  </style>
</head>

<body>

<header>
  <h1>🎰 CASINO PRO</h1>
  <div class="balance" id="balance">رصيدك: 100</div>
</header>

<div class="container">

  <!-- sidebar -->
  <div class="sidebar">
    <button onclick="showGame('plinko')">🎯 Plinko</button>
    <button onclick="showGame('dice')">🎲 Dice</button>
  </div>

  <!-- main -->
  <div class="main">

    <!-- Plinko -->
    <div id="plinko">
      <h2>Plinko</h2>
      <button class="play" onclick="dropBall()">Play</button>
      <canvas id="canvas" width="400" height="500"></canvas>
    </div>

    <!-- Dice -->
    <div id="dice" style="display:none;">
      <h2>Dice</h2>
      <button class="play" onclick="playDice()">Roll</button>
      <p id="diceResult"></p>
    </div>

  </div>

</div>

<script>
let balance = 100;

function updateBalance() {
  document.getElementById("balance").innerText = "رصيدك: " + balance;
}

function showGame(game) {
  document.getElementById("plinko").style.display = "none";
  document.getElementById("dice").style.display = "none";
  document.getElementById(game).style.display = "block";
}

/* Dice */
function playDice() {
  if (balance <= 0) return;

  let win = Math.random() < 0.5;

  if (win) {
    balance += 10;
    document.getElementById("diceResult").innerText = "🔥 فزت!";
  } else {
    balance -= 10;
    document.getElementById("diceResult").innerText = "💀 خسرت!";
  }

  updateBalance();
}

/* Plinko */
let canvas = document.getElementById("canvas");
let ctx = canvas.getContext("2d");

let pegs = [];
let ball = null;

for (let y = 50; y < 400; y += 50) {
  for (let x = 50; x < 350; x += 50) {
    pegs.push({x: x + (y % 100 === 0 ? 25 : 0), y: y});
  }
}

function drawPegs() {
  ctx.fillStyle = "#00ffcc";
  pegs.forEach(p => {
    ctx.beginPath();
    ctx.arc(p.x, p.y, 5, 0, Math.PI * 2);
    ctx.fill();
  });
}

function dropBall() {
  if (balance <= 0) return;

  balance -= 10;
  updateBalance();

  ball = {x: 200, y: 0, radius: 8, speedY: 2};
}

function update() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  drawPegs();

  if (ball) {
    ball.y += ball.speedY;

    pegs.forEach(p => {
      let dx = ball.x - p.x;
      let dy = ball.y - p.y;
      let dist = Math.sqrt(dx*dx + dy*dy);

      if (dist < 10) {
        ball.x += (Math.random() - 0.5) * 20;
      }
    });

    if (ball.y > 480) {
      let win = Math.random() < 0.3;
      if (win) balance += 30;

      updateBalance();
      ball = null;
    }

    ctx.fillStyle = "#00ffcc";
    ctx.beginPath();
    ctx.arc(ball.x, ball.y, ball.radius, 0, Math.PI * 2);
    ctx.fill();
  }

  requestAnimationFrame(update);
}

update();
</script>

</body>
</html>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
@media (max-width: 768px) {
  .container {
    flex-direction: column;
  }

  .sidebar {
    width: 100%;
    height: auto;
  }

  canvas {
    width: 100%;
  }
  
