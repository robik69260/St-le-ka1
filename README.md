<!DOCTYPE html>  <html lang="en">  
<head>  
  <meta charset="UTF-8" />  
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />  
  <title>Mobile Shooter Game</title>  
  <style>  
    html, body {  
      margin: 0;  
      padding: 0;  
      overflow: hidden;  
      background: black;  
      color: white;  
      touch-action: none;  
    }  
    canvas {  
      display: block;  
    }  
    #controls {  
      position: absolute;  
      bottom: 10px;  
      left: 50%;  
      transform: translateX(-50%);  
      display: flex;  
      gap: 10px;  
      z-index: 10;  
    }  
    button {  
      font-size: 24px;  
      padding: 10px;  
      border-radius: 10px;  
      border: none;  
      background: #444;  
      color: white;  
    }  
  </style>  
</head>  
<body>  
  <canvas id="gameCanvas"></canvas>  
  <div id="controls">  
    <button id="left">⬅️</button>  
    <button id="shoot">🔫</button>  
    <button id="right">➡️</button>  
  </div>  
  <script>  
    const canvas = document.getElementById('gameCanvas');  
    const ctx = canvas.getContext('2d');  
    canvas.width = window.innerWidth;  
    canvas.height = window.innerHeight;  const player = {  
  x: canvas.width / 2 - 25,  
  y: canvas.height - 60,  
  width: 50,  
  height: 50,  
  speed: 5,  
  bullets: [],  
  lives: 3,  
  powerLevel: 1  
};  

let enemies = [], powerUps = [], boss = null, score = 0, keys = {};  

function drawPlayer() {  
  ctx.fillStyle = 'lime';  
  ctx.fillRect(player.x, player.y, player.width, player.height);  
}  

function drawBullets() {  
  ctx.fillStyle = 'yellow';  
  player.bullets.forEach(bullet => {  
    bullet.y -= 10;  
    ctx.fillRect(bullet.x, bullet.y, 5, 10);  
  });  
  player.bullets = player.bullets.filter(b => b.y > 0);  
}  

function spawnEnemy() {  
  enemies.push({ x: Math.random() * canvas.width, y: -30, width: 30, height: 30 });  
}  

function drawEnemies() {  
  ctx.fillStyle = 'red';  
  enemies.forEach(enemy => {  
    enemy.y += 2;  
    ctx.fillRect(enemy.x, enemy.y, enemy.width, enemy.height);  
  });  
  enemies = enemies.filter(e => e.y < canvas.height);  
}  

function drawPowerUps() {  
  powerUps.forEach(p => {  
    ctx.fillStyle = p.type === 'life' ? 'aqua' : 'gold';  
    p.y += 1.5;  
    ctx.fillRect(p.x, p.y, 20, 20);  
  });  
}  

function spawnPowerUp() {  
  const type = Math.random() > 0.5 ? 'life' : 'power';  
  powerUps.push({ x: Math.random() * canvas.width, y: 0, type });  
}  

function handleCollisions() {  
  enemies.forEach((enemy, ei) => {  
    player.bullets.forEach((bullet, bi) => {  
      if (bullet.x < enemy.x + enemy.width && bullet.x + 5 > enemy.x && bullet.y < enemy.y + enemy.height && bullet.y + 10 > enemy.y) {  
        enemies.splice(ei, 1);  
        player.bullets.splice(bi, 1);  
        score++;  
      }  
    });  
    if (enemy.x < player.x + player.width && enemy.x + enemy.width > player.x && enemy.y < player.y + player.height && enemy.y + enemy.height > player.y) {  
      enemies.splice(ei, 1);  
      player.lives--;  
    }  
  });  

  powerUps = powerUps.filter(p => {  
    if (p.x < player.x + player.width && p.x + 20 > player.x && p.y < player.y + player.height && p.y + 20 > player.y) {  
      if (p.type === 'life') player.lives++;  
      else if (p.type === 'power' && player.powerLevel < 3) player.powerLevel++;  
      return false;  
    }  
    return p.y < canvas.height;  
  });  
}  

function shoot() {  
  const center = player.x + player.width / 2;  
  if (player.powerLevel === 1) {  
    player.bullets.push({ x: center - 2, y: player.y });  
  } else if (player.powerLevel === 2) {  
    player.bullets.push({ x: center - 10, y: player.y }, { x: center + 5, y: player.y });  
  } else {  
    player.bullets.push({ x: center - 15, y: player.y }, { x: center - 2, y: player.y }, { x: center + 10, y: player.y });  
  }  
}  

function drawHUD() {  
  ctx.fillStyle = 'white';  
  ctx.fillText('Lives: ' + player.lives, 10, 20);  
  ctx.fillText('Score: ' + score, 10, 40);  
}  

function gameLoop() {  
  ctx.clearRect(0, 0, canvas.width, canvas.height);  
  drawPlayer();  
  drawBullets();  
  drawEnemies();  
  drawPowerUps();  
  handleCollisions();  
  drawHUD();  

  if (keys['ArrowLeft'] && player.x > 0) player.x -= player.speed;  
  if (keys['ArrowRight'] && player.x + player.width < canvas.width) player.x += player.speed;  

  requestAnimationFrame(gameLoop);  
}  

setInterval(spawnEnemy, 1000);  
setInterval(spawnPowerUp, 8000);  

document.getElementById('left').addEventListener('touchstart', () => keys['ArrowLeft'] = true);  
document.getElementById('left').addEventListener('touchend', () => keys['ArrowLeft'] = false);  
document.getElementById('right').addEventListener('touchstart', () => keys['ArrowRight'] = true);  
document.getElementById('right').addEventListener('touchend', () => keys['ArrowRight'] = false);  
document.getElementById('shoot').addEventListener('touchstart', shoot);  

gameLoop();

  </script>  
</body>  
</html>  Vytvoř z toho odkaz abych si to mohl zahrát prosím

<!DOCTYPE html>  <html lang="en">  
<head>  
  <meta charset="UTF-8" />  
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />  
  <title>Mobile Shooter Game</title>  
  <style>  
    html, body {  
      margin: 0;  
      padding: 0;  
      overflow: hidden;  
      background: black;  
      color: white;  
      touch-action: none;  
    }  
    canvas {  
      display: block;  
    }  
    #controls {  
      position: absolute;  
      bottom: 10px;  
      left: 50%;  
      transform: translateX(-50%);  
      display: flex;  
      gap: 10px;  
      z-index: 10;  
    }  
    button {  
      font-size: 24px;  
      padding: 10px;  
      border-radius: 10px;  
      border: none;  
      background: #444;  
      color: white;  
    }  
  </style>  
</head>  
<body>  
  <canvas id="gameCanvas"></canvas>  
  <div id="controls">  
    <button id="left">⬅️</button>  
    <button id="shoot">🔫</button>  
    <button id="right">➡️</button>  
  </div>  
  <script>  
    const canvas = document.getElementById('gameCanvas');  
    const ctx = canvas.getContext('2d');  
    canvas.width = window.innerWidth;  
    canvas.height = window.innerHeight;  const player = {  
  x: canvas.width / 2 - 25,  
  y: canvas.height - 60,  
  width: 50,  
  height: 50,  
  speed: 5,  
  bullets: [],  
  lives: 3,  
  powerLevel: 1  
};  

let enemies = [], powerUps = [], boss = null, score = 0, keys = {};  

function drawPlayer() {  
  ctx.fillStyle = 'lime';  
  ctx.fillRect(player.x, player.y, player.width, player.height);  
}  

function drawBullets() {  
  ctx.fillStyle = 'yellow';  
  player.bullets.forEach(bullet => {  
    bullet.y -= 10;  
    ctx.fillRect(bullet.x, bullet.y, 5, 10);  
  });  
  player.bullets = player.bullets.filter(b => b.y > 0);  
}  

function spawnEnemy() {  
  enemies.push({ x: Math.random() * canvas.width, y: -30, width: 30, height: 30 });  
}  

function drawEnemies() {  
  ctx.fillStyle = 'red';  
  enemies.forEach(enemy => {  
    enemy.y += 2;  
    ctx.fillRect(enemy.x, enemy.y, enemy.width, enemy.height);  
  });  
  enemies = enemies.filter(e => e.y < canvas.height);  
}  

function drawPowerUps() {  
  powerUps.forEach(p => {  
    ctx.fillStyle = p.type === 'life' ? 'aqua' : 'gold';  
    p.y += 1.5;  
    ctx.fillRect(p.x, p.y, 20, 20);  
  });  
}  

function spawnPowerUp() {  
  const type = Math.random() > 0.5 ? 'life' : 'power';  
  powerUps.push({ x: Math.random() * canvas.width, y: 0, type });  
}  

function handleCollisions() {  
  enemies.forEach((enemy, ei) => {  
    player.bullets.forEach((bullet, bi) => {  
      if (bullet.x < enemy.x + enemy.width && bullet.x + 5 > enemy.x && bullet.y < enemy.y + enemy.height && bullet.y + 10 > enemy.y) {  
        enemies.splice(ei, 1);  
        player.bullets.splice(bi, 1);  
        score++;  
      }  
    });  
    if (enemy.x < player.x + player.width && enemy.x + enemy.width > player.x && enemy.y < player.y + player.height && enemy.y + enemy.height > player.y) {  
      enemies.splice(ei, 1);  
      player.lives--;  
    }  
  });  

  powerUps = powerUps.filter(p => {  
    if (p.x < player.x + player.width && p.x + 20 > player.x && p.y < player.y + player.height && p.y + 20 > player.y) {  
      if (p.type === 'life') player.lives++;  
      else if (p.type === 'power' && player.powerLevel < 3) player.powerLevel++;  
      return false;  
    }  
    return p.y < canvas.height;  
  });  
}  

function shoot() {  
  const center = player.x + player.width / 2;  
  if (player.powerLevel === 1) {  
    player.bullets.push({ x: center - 2, y: player.y });  
  } else if (player.powerLevel === 2) {  
    player.bullets.push({ x: center - 10, y: player.y }, { x: center + 5, y: player.y });  
  } else {  
    player.bullets.push({ x: center - 15, y: player.y }, { x: center - 2, y: player.y }, { x: center + 10, y: player.y });  
  }  
}  

function drawHUD() {  
  ctx.fillStyle = 'white';  
  ctx.fillText('Lives: ' + player.lives, 10, 20);  
  ctx.fillText('Score: ' + score, 10, 40);  
}  

function gameLoop() {  
  ctx.clearRect(0, 0, canvas.width, canvas.height);  
  drawPlayer();  
  drawBullets();  
  drawEnemies();  
  drawPowerUps();  
  handleCollisions();  
  drawHUD();  

  if (keys['ArrowLeft'] && player.x > 0) player.x -= player.speed;  
  if (keys['ArrowRight'] && player.x + player.width < canvas.width) player.x += player.speed;  

  requestAnimationFrame(gameLoop);  
}  

setInterval(spawnEnemy, 1000);  
setInterval(spawnPowerUp, 8000);  

document.getElementById('left').addEventListener('touchstart', () => keys['ArrowLeft'] = true);  
document.getElementById('left').addEventListener('touchend', () => keys['ArrowLeft'] = false);  
document.getElementById('right').addEventListener('touchstart', () => keys['ArrowRight'] = true);  
document.getElementById('right').addEventListener('touchend', () => keys['ArrowRight'] = false);  
document.getElementById('shoot').addEventListener('touchstart', shoot);  

gameLoop();

  </script>  
</body>  
</html>  Vytvoř z toho odkaz abych si to mohl zahrát prosím
