

<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sol Hotel 抽獎活動</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 50px;
      background: linear-gradient(135deg, #f9f9f9, #e0f7fa);
      overflow-x: hidden;
      overflow-y: hidden;
    }
    h1 {
      color: #ff9800;
    }
    .game-area {
      margin-top: 30px;
    }
    .box {
      display: inline-block;
      width: 100px;
      height: 100px;
      margin: 15px;
      background-color: #ffc107;
      border-radius: 15px;
      cursor: pointer;
      transition: transform 0.6s;
      transform-style: preserve-3d;
      position: relative;
      font-size: 2em;
    }
    .box.opened {
      transform: rotateY(180deg);
    }
    .box-face {
      position: absolute;
      width: 100%;
      height: 100%;
      backface-visibility: hidden;
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 15px;
    }
    .box-front {
      background-color: #ffc107;
    }
    .box-back {
      background-color: #ff5722;
      color: white;
      transform: rotateY(180deg);
    }
    #result {
      margin-top: 20px;
      font-size: 1.2em;
      font-weight: bold;
    }
    .lang-toggle {
      position: absolute;
      top: 10px;
      right: 10px;
      cursor: pointer;
      background: #2196f3;
      color: white;
      border: none;
      padding: 5px 10px;
      border-radius: 5px;
    }
    canvas {
      position: fixed;
      top: 0;
      left: 0;
      pointer-events: none;
    }
  </style>
</head>
<body>
  <button class="lang-toggle" onclick="toggleLang()">EN</button>
  <h1 id="title">🎉 SOL Hotel 抽獎活動 🎉</h1>
  <p id="desc">點擊任一禮盒看看您是否中獎！</p>

  <div class="game-area" id="gameArea">
    <div class="box" onclick="play(this)">
      <div class="box-face box-front">🎁</div>
      <div class="box-face box-back"></div>
    </div>
    <div class="box" onclick="play(this)">
      <div class="box-face box-front">🎁</div>
      <div class="box-face box-back"></div>
    </div>
    <div class="box" onclick="play(this)">
      <div class="box-face box-front">🎁</div>
      <div class="box-face box-back"></div>
    </div>
  </div>

  <div id="result"></div>
  <canvas id="fireworks"></canvas>
  <audio id="winSound" src="https://www.soundjay.com/buttons/sounds/button-4.mp3" preload="auto"></audio>

  <script>
    let currentLang = 'zh';
    const canvas = document.getElementById('fireworks');
    const ctx = canvas.getContext('2d');
    let particles = [];
    const winSound = document.getElementById('winSound');

    function resizeCanvas() {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    }
    window.addEventListener('resize', resizeCanvas);
    resizeCanvas();

    function play(clickedBox) {
      const boxes = Array.from(document.querySelectorAll('.box'));
      // 生成一個中獎分配，保證至少一個中獎
      let winIndexes = [];
      while(winIndexes.length === 0) {
        boxes.forEach((box, i) => {
          if(Math.random() < 0.3) winIndexes.push(i);
        });
      }
      // 分配結果到盒子
      boxes.forEach((box, i) => {
        if(!box.classList.contains('opened')) {
          box.classList.add('opened');
          const isWin = winIndexes.includes(i);
          box.querySelector('.box-back').textContent = isWin ? '🎉' : '😊';
          if(isWin && box === clickedBox){
            winSound.currentTime = 0;
            winSound.play();
            launchFireworks();
            document.getElementById('result').innerText = currentLang === 'zh' ? '🎊 恭喜中獎！請至櫃檯領取獎品 🎊' : '🎊 Congratulations! You won! Please claim your prize at the front desk 🎊';
          } else if(box === clickedBox && !isWin){
            document.getElementById('result').innerText = currentLang === 'zh' ? '💡 謝謝參加，祝您下次好運！' : '💡 Thank you for playing, better luck next time!';
          }
        }
      });
    }

    function toggleLang() {
      currentLang = currentLang === 'zh' ? 'en' : 'zh';
      document.querySelector('.lang-toggle').innerText = currentLang === 'zh' ? 'EN' : '中';
      document.getElementById('title').innerText = currentLang === 'zh' ? '🎉 SOL Hotel 抽獎活動 🎉' : '🎉 SOL Hotel Lucky Draw 🎉';
      document.getElementById('desc').innerText = currentLang === 'zh' ? '點擊任一禮盒看看您是否中獎！' : 'Click any gift box to see if you win!';
    }

    function launchFireworks() {
      for (let i = 0; i < 100; i++) {
        particles.push({
          x: canvas.width / 2,
          y: canvas.height / 2,
          angle: Math.random() * 2 * Math.PI,
          speed: Math.random() * 5 + 2,
          radius: Math.random() * 3 + 2,
          alpha: 1
        });
      }
      animateFireworks();
    }

    function animateFireworks() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      particles.forEach((p) => {
        p.x += Math.cos(p.angle) * p.speed;
        p.y += Math.sin(p.angle) * p.speed;
        p.alpha -= 0.01;
        ctx.beginPath();
        ctx.arc(p.x, p.y, p.radius, 0, 2 * Math.PI);
        ctx.fillStyle = `rgba(255, ${Math.floor(Math.random()*255)}, 0, ${p.alpha})`;
        ctx.fill();
      });
      particles = particles.filter(p => p.alpha > 0);
      if (particles.length > 0) requestAnimationFrame(animateFireworks);
    }
  </script>
</body>
</html>
