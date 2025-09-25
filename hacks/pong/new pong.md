---
layout: default
title: 🏓 Complete Pong Game Code Implementation
description: Complete HTML, CSS, and JavaScript code for building a fully functional 2-player Pong game
categories: ['Game Development', 'JavaScript', 'Canvas API', 'Code Implementation']
permalink: /newcustompong
menu: nav/tools_setup.html
toc: True
comments: True
---

## 🎮 Pong Game Demo

<div class="game-canvas-container" style="text-align:center;">
  <canvas id="pongCanvas" width="800" height="500"></canvas>
  <br>
  <button id="restartBtn">Restart Game</button>

  <!-- SpeedBoostMode controls -->
  <div id="controls" style="margin-top:12px;">
    <label style="color:#000; user-select:none;">
      <input type="checkbox" id="boostToggle"> Speed Boost Mode
    </label>
    <span id="boostStatus" style="color:#000; margin-left:12px;">x1.00</span>

    <!-- Gravity Mode toggle -->
    <label style="color:#000; user-select:none; margin-left:20px;">
      <input type="checkbox" id="gravityToggle"> Gravity Mode
    </label>
  </div>
</div>

<style>
  .game-canvas-container { margin-top: 20px; }
  #pongCanvas { border: 2px solid #fff; background: #000; }
  #restartBtn {
    display: none;
    margin-top: 15px;
    padding: 10px 20px;
    font-size: 18px;
    border: none;
    border-radius: 6px;
    background: #4caf50;
    color: black;
    cursor: pointer;
  }
  #restartBtn:hover { background: #45a049; }
  #controls { display: inline-flex; align-items: center; gap: 12px; }
  #controls input { transform: scale(1.2); cursor: pointer; }
  #controls label { cursor: pointer; }
</style>

<script>

const canvas = document.getElementById('pongCanvas');
const ctx = canvas.getContext('2d');

const paddleWidth = 8, paddleHeight = 80;
let player1Y = (canvas.height - paddleHeight) / 2;
let player2Y = (canvas.height - paddleHeight) / 2;
const paddleSpeed = 7;
  


let ballX, ballY, ballSpeedX, ballSpeedY, ballRadius = 10;
let ballColor = "#fff";

let player1Score = 0, player2Score = 0;
const winningScore = 10;
let gameOver = false;

const restartBtn = document.getElementById('restartBtn');

// === Speed Boost Mode (with localStorage) ===
const boostToggle = document.getElementById('boostToggle');
const boostStatus = document.getElementById('boostStatus');

const HITS_PER_BOOST = 3;
const BOOST_FACTOR = 1.10;
const MAX_SPEED = 18;

let hitCount = 0;
let speedMultiplier = 1;

// === Gravity Mode ===
const gravityToggle = document.getElementById('gravityToggle');
const GRAVITY_FORCE = 0.25; // pixels/frame²

// === Bug fix ===
const MIN_X_SPEED = 2.0; 
function enforceMinXSpeed() {
  if (Math.abs(ballSpeedX) < MIN_X_SPEED) {
    const dir = (ballSpeedX === 0 ? (Math.random() < 0.5 ? -1 : 1) : Math.sign(ballSpeedX));
    ballSpeedX = dir * MIN_X_SPEED;
  }
}

// ======= UI helper =======
function updateBoostStatus() { boostStatus.textContent = "x" + speedMultiplier.toFixed(2); }

function clampSpeed() {
  const s = Math.hypot(ballSpeedX, ballSpeedY);
  if (s > MAX_SPEED) {
    const k = MAX_SPEED / s;
    ballSpeedX *= k;
    ballSpeedY *= k;
  }
}

let boostFlashFrames = 0, boostFlashX = 0, boostFlashY = 0;

function applySpeedBoostIfNeeded() {
  if (boostToggle.checked && hitCount > 0 && hitCount % HITS_PER_BOOST === 0) {
    ballSpeedX *= BOOST_FACTOR;
    ballSpeedY *= BOOST_FACTOR;
    speedMultiplier *= BOOST_FACTOR;
    clampSpeed();
    enforceMinXSpeed();
    updateBoostStatus();
    boostFlashFrames = 30; 
    boostFlashX = ballX;
    boostFlashY = ballY;
  }
}

function resetSpeedTracking() {
  hitCount = 0;
  speedMultiplier = 1;
  updateBoostStatus();
}

// === localStorage ===
function loadSettings() {
  const raw = localStorage.getItem('pong.settings');
  if (!raw) return;
  try {
    const cfg = JSON.parse(raw);
    boostToggle.checked = !!cfg.boostEnabled;
    gravityToggle.checked = !!cfg.gravityEnabled;
  } catch (e) { /* ignore */ }
}

function saveSettings() {
  const cfg = { 
    boostEnabled: boostToggle.checked, 
    gravityEnabled: gravityToggle.checked 
  };
  localStorage.setItem('pong.settings', JSON.stringify(cfg));
}

boostToggle.addEventListener('change', () => { saveSettings(); updateBoostStatus(); });
gravityToggle.addEventListener('change', saveSettings);

// ======= Core Game =======
function initBall() {
  ballX = canvas.width/2;
  ballY = canvas.height/2;
  ballSpeedX = Math.random() > 0.5 ? 5 : -5;
  ballSpeedY = (Math.random() * 4) - 2;
  ballColor = getRandomColor();
  enforceMinXSpeed();
  if (boostToggle.checked) resetSpeedTracking();
  boostFlashFrames = 0;
}

function drawRect(x, y, w, h, color) {
  ctx.fillStyle = color; ctx.fillRect(x, y, w, h);
}

function drawCircle(x, y, r, color) {
  ctx.fillStyle = color; ctx.beginPath();
  ctx.arc(x, y, r, 0, Math.PI*2, false);
  ctx.closePath(); ctx.fill();
}

function drawText(text, x, y, color="white") {
  ctx.fillStyle = color; ctx.font = "30px Arial"; ctx.fillText(text, x, y);
}

function draw() {
  drawRect(0, 0, canvas.width, canvas.height, "#000");
  drawRect(0, player1Y, paddleWidth, paddleHeight, "#fff");
  drawRect(canvas.width - paddleWidth, player2Y, paddleWidth, paddleHeight, "#fff");
  drawCircle(ballX, ballY, ballRadius, ballColor);
  drawText(player1Score, canvas.width/4, 50);
  drawText(player2Score, 3*canvas.width/4, 50);
  if(gameOver) {
    drawText("Game Over", canvas.width/2 - 80, canvas.height/2 - 20, "red");
    drawText(player1Score >= winningScore ? "Player 1 Wins!" : "Player 2 Wins!", canvas.width/2 - 120, canvas.height/2 + 20, "yellow");
  }
  if (boostFlashFrames > 0) {
    const alpha = boostFlashFrames / 30;
    ctx.save(); ctx.globalAlpha = alpha;
    ctx.fillStyle = "cyan"; ctx.font = "20px Arial";
    ctx.fillText("+10%", boostFlashX - 16, boostFlashY - 16 - (30 - boostFlashFrames) * 0.5);
    ctx.restore();
    boostFlashFrames--;
  }
}

function update() {
  if (gameOver) return;

  ballX += ballSpeedX;
  ballY += ballSpeedY;

  // Gravity mode effect
  if (gravityToggle.checked) { ballSpeedY += GRAVITY_FORCE; }

  if(ballY + ballRadius > canvas.height || ballY - ballRadius < 0) ballSpeedY = -ballSpeedY;

  if(ballX - ballRadius < paddleWidth && ballY > player1Y && ballY < player1Y + paddleHeight) {
    ballSpeedX = -ballSpeedX;
    const deltaY = ballY - (player1Y + paddleHeight/2);
    ballSpeedY = deltaY * 0.2;
    ballColor = getRandomColor();
    ballSpeedX *= 1.02; hitCount++; applySpeedBoostIfNeeded(); enforceMinXSpeed();
  }

  if(ballX + ballRadius > canvas.width - paddleWidth && ballY > player2Y && ballY < player2Y + paddleHeight) {
    ballSpeedX = -ballSpeedX;
    const deltaY = ballY - (player2Y + paddleHeight/2);
    ballSpeedY = deltaY * 0.3;
    ballColor = getRandomColor();
    ballSpeedX *= 1.02; hitCount++; applySpeedBoostIfNeeded(); enforceMinXSpeed();
  }

  if(ballX - ballRadius < 0) {
    player2Score++; if(player2Score >= winningScore) { gameOver = true; restartBtn.style.display = "inline-block"; }
    initBall();
  } else if(ballX + ballRadius > canvas.width) {
    player1Score++; if(player1Score >= winningScore) { gameOver = true; restartBtn.style.display = "inline-block"; }
    initBall();
  }
}

const keys = {};
document.addEventListener("keydown", e => keys[e.key] = true);
document.addEventListener("keyup", e => keys[e.key] = false);

function handleInput() {
  if(keys["w"] && player1Y > 0) player1Y -= paddleSpeed;
  if(keys["s"] && player1Y + paddleHeight < canvas.height) player1Y += paddleSpeed;
  if(keys["i"] && player2Y > 0) player2Y -= paddleSpeed;
  if(keys["k"] && player2Y + paddleHeight < canvas.height) player2Y += paddleSpeed;
}

function gameLoop() {
  update(); handleInput(); draw(); requestAnimationFrame(gameLoop);
}

restartBtn.addEventListener("click", () => {
  player1Score = 0; player2Score = 0;
  player1Y = (canvas.height - paddleHeight) / 2;
  player2Y = (canvas.height - paddleHeight) / 2;
  gameOver = false; restartBtn.style.display = "none"; initBall();
});

function getRandomColor() {
  const letters = "0123456789ABCDEF";
  let color = "#";
  for (let i = 0; i < 6; i++) color += letters[Math.floor(Math.random() * 16)];
  return color;
}

loadSettings();
updateBoostStatus();
initBall();
gameLoop();
</script>

Short Reflection:

What was the most challenging part of adding your feature?
  Figuring out how to describe what we wanted to do in code.

How did using commits help you track progress?
  Using commits helped us keep track of which change was made when and what it included. They kept our work organized.

What did you learn about debugging or OOP in this project?
  We learned about how to organize our game characters, such as the ball, the paddle, and the score using classes. 