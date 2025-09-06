<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>SOL Hotel｜Lucky Draw 抽獎活動</title>
<style>
  :root{
    --bg:#111215;              /* 背景色 */
    --card:#1b1f24;            /* 卡片色 */
    --accent:#FFC107;          /* 強調色 */
    --text:#f2f2f2;            /* 文字色 */
    --muted:#a9b1ba;           /* 次要文字 */
    --success:#22c55e;         /* 中獎綠 */
    --danger:#ef4444;          /* 未中紅 */
    --shadow: 0 8px 30px rgba(0,0,0,.25);
  }
  html,body{height:100%;}
  body{
    margin:0;
    font-family: "Segoe UI", system-ui, -apple-system, PingFangTC, "Noto Sans TC", Arial, sans-serif;
    color:var(--text);
    background: radial-gradient(80vw 80vh at 20% -10%, #1e2026 0%, #111215 60%, #0d0e10 100%);
    display:flex;justify-content:center;align-items:center;
    padding:24px;
  }
  .app{ width:min(980px, 100%); }
  header{
    display:flex; justify-content:space-between; align-items:center; gap:16px; flex-wrap:wrap;
    background:linear-gradient(135deg, #181b20, #16181d);
    border:1px solid rgba(255,255,255,.06);
    box-shadow: var(--shadow);
    border-radius:20px; padding:16px 20px; margin-bottom:18px;
  }
  .brand{
    display:flex; align-items:center; gap:12px; font-weight:700; letter-spacing:.3px;
  }
  .logo{ width:36px; height:36px; border-radius:50%; background:conic-gradient(from 0deg, #FFD54F, #FFC107, #FF9800, #FFA000, #FFC107); box-shadow:0 0 0 3px rgba(255,193,7,.15), inset 0 0 12px rgba(0,0,0,.35);}
  .brand h1{ font-size:1.05rem; margin:0; }
  .lang{
    display:flex; gap:8px; align-items:center;
  }
  .lang button{
    background:transparent; border:1px solid rgba(255,255,255,.15); color:var(--text);
    padding:6px 10px; border-radius:999px; cursor:pointer; font-weight:600;
  }
  .lang button.active{ border-color:var(--accent); box-shadow:0 0 0 3px rgba(255,193,7,.15); }

  .card{
    background:var(--card);
    border:1px solid rgba(255,255,255,.06);
    box-shadow: var(--shadow);
    border-radius:22px; padding:18px; margin-bottom:16px;
  }

  .hero{ display:grid; grid-template-columns: 1.2fr .8fr; gap:16px; }
  @media (max-width: 820px){ .hero{ grid-template-columns: 1fr; } }

  .left .title{ font-size:1.35rem; font-weight:800; margin:4px 0 10px; }
  .left .subtitle{ color:var(--muted); font-size:.95rem; line-height:1.6; }

  .right{
    display:flex; flex-direction:column; gap:10px; align-items:stretch; justify-content:center;
  }
  .btn{
    appearance:none; border:none; cursor:pointer;
    padding:14px 18px; border-radius:16px; font-weight:800; letter-spacing:.3px; font-size:1.05rem;
    transition: transform .06s ease, box-shadow .2s ease;
  }
  .btn:active{ transform: translateY(1px); }
  .btn-primary{ background:var(--accent); color:#111; box-shadow:0 10px 20px rgba(255,193,7,.28); }
  .btn-ghost{ background:transparent; color:var(--text); border:1px dashed rgba(255,255,255,.25); }
  .btn[disabled]{ opacity:.5; cursor:not-allowed; }

  .stage{ display:grid; grid-template-columns: repeat(3, 1fr); gap:14px; margin-top:8px; }

  .box{
    background:linear-gradient(180deg, #20242b, #161a20);
    border:1px solid rgba(255,255,255,.06);
    border-radius:20px; position:relative; isolation:isolate;
    display:flex; align-items:center; justify-content:center; padding:18px; height:150px; overflow:hidden;
    transition: transform .15s ease, border-color .2s ease, box-shadow .2s ease;
  }
  @media (max-width:540px){ .box{ height:120px; } }
  .box:hover{ transform: translateY(-2px); border-color: rgba(255,193,7,.35); box-shadow:0 10px 20px rgba(255,193,7,.15); }
  .box .emoji{ font-size:54px; filter: drop-shadow(0 6px 12px rgba(0,0,0,.35)); }
  .ribbon{ position:absolute; top:10px; left:-38px; rotate:-22deg; background:#2b3340; color:#fff; font-weight:800; padding:6px 44px; border-radius:999px; font-size:.8rem; opacity:.85; letter-spacing:.4px; }

  .box.wiggle{ animation: wiggle .7s ease-in-out infinite; }
  @keyframes wiggle{ 0%{ transform:rotate(0deg)} 25%{ transform:rotate(1.6deg)} 50%{ transform:rotate(0deg)} 75%{ transform:rotate(-1.6deg)} 100%{ transform:rotate(0deg)} }

  .notice{ color:var(--muted); font-size:.86rem; }
  .tag{ display:inline-flex; align-items:center; gap:8px; padding:8px 12px; background:#21262d; border-radius:999px; border:1px solid rgba(255,255,255,.08); }

  .result{ text-align:center; margin-top:14px; font-size:1.05rem; font-weight:700; min-height:1.6em; }
  .result.win{ color:var(--success); }
  .result.lose{ color:var(--danger); }

  /* Modal */
  .modal{ position:fixed; inset:0; display:none; align-items:center; justify-content:center; background:rgba(0,0,0,.6); z-index:50; }
  .modal.show{ display:flex; }
  .sheet{ width:min(520px, 92%); background:var(--card); border:1px solid rgba(255,255,255,.08); border-radius:20px; padding:20px; box-shadow: var(--shadow); text-align:center; }
  .sheet h3{ margin:6px 0 6px; }
  .sheet p{ color:var(--muted); }

  /* Confetti canvas sits on top */
  #confetti{ position:fixed; inset:0; pointer-events:none; z-index:60; }

  footer{ text-align:center; color:var(--muted); font-size:.8rem; margin-top:10px; }
  a{ color:var(--accent); text-decoration:none; }
</style>
</head>
<body>
  <canvas id="confetti"></canvas>
  <div class="app">
    <header class="card">
      <div class="brand">
        <div class="logo" aria-hidden="true"></div>
        <h1>SOL Hotel｜Lucky Draw 抽獎活動</h1>
      </div>
      <div class="lang" aria-label="Language">
        <button class="active" id="lang-zh">中文</button>
        <button id="lang-en">EN</button>
      </div>
    </header>

    <section class="hero">
      <div class="left card">
        <div class="title" data-i18n="title">轉轉好運，抽驚喜！</div>
        <div class="subtitle" data-i18n="subtitle">
          點擊下方 <b>開始</b> 後，挑選一個禮盒。中獎機率固定為 <b>30%</b>，祝您好運！
        </div>

        <div class="stage" role="list" aria-label="gift boxes">
          <button class="box wiggle" data-index="0" role="listitem" aria-label="gift box 1" disabled>
            <span class="ribbon" data-i18n="pick">挑我！</span>
            <span class="emoji" aria-hidden="true">🎁</span>
          </button>
          <button class="box wiggle" data-index="1" role="listitem" aria-label="gift box 2" disabled>
            <span class="ribbon" data-i18n="pick">挑我！</span>
            <span class="emoji" aria-hidden="true">🎁</span>
          </button>
          <button class="box wiggle" data-index="2" role="listitem" aria-label="gift box 3" disabled>
            <span class="ribbon" data-i18n="pick">挑我！</span>
            <span class="emoji" aria-hidden="true">🎁</span>
          </button>
        </div>

        <div class="result" id="result"></div>
      </div>

      <div class="right">
        <div class="card">
          <button class="btn btn-primary" id="btnStart" data-i18n="start">開始 / START</button>
          <button class="btn btn-ghost" id="btnRules" data-i18n="rules">活動說明 / Rules</button>
          <div class="notice" style="margin-top:8px;">
            <span class="tag">🎯 <span data-i18n="rate">中獎機率 30%</span></span>
            <span class="tag">🛡️ <span data-i18n="once">每裝置每日限玩一次</span></span>
          </div>
        </div>

        <div class="card">
          <b data-i18n="prizeTitle">🎉 中獎禮品</b>
          <div class="notice" data-i18n="prizeInfo">小禮物（以現場公告為準）。請於入住期間出示此畫面給櫃檯人員兌換。</div>
        </div>
      </div>
    </section>

    <footer>
      <div data-i18n="footer">SOL Hotel 新竹迎曦大飯店｜本活動依現場公告為準</div>
    </footer>
  </div>

  <!-- Modal -->
  <div class="modal" id="modal">
    <div class="sheet">
      <div style="font-size:56px;" id="modalIcon">🏆</div>
      <h3 id="modalTitle">恭喜中獎！</h3>
      <p id="modalMsg">請截圖此畫面並出示給櫃檯人員完成兌換。</p>
      <button class="btn btn-primary" id="modalClose">OK</button>
    </div>
  </div>

<script>
  /* =====================
     CONFIG (可調整設定)
     ===================== */
  const WIN_RATE = 0.30;               // 中獎機率 30%
  const DAILY_LIMIT = 1;               // 每日可玩次數（每台裝置）
  const STORAGE_KEY = 'solhotel_luckydraw_playcount_v1';
  const PRIZE_TEXT_ZH = '恭喜中獎！請截圖此畫面並於入住期間出示給櫃檯人員兌換。';
  const PRIZE_TEXT_EN = 'Congratulations! Please screenshot this page and present it at the front desk to redeem your gift during your stay.';
  const MISS_TEXT_ZH  = '謝謝您的參與～祝您下次好運！';
  const MISS_TEXT_EN  = 'Thanks for participating — better luck next time!';

  /* =====================
     簡易 i18n 語系
     ===================== */
  const I18N = {
    zh: {
      title: '轉轉好運，抽驚喜！',
      subtitle: '點擊下方 <b>開始</b> 後，挑選一個禮盒。中獎機率固定為 <b>30%</b>，祝您好運！',
      pick: '挑我！',
      start: '開始 / START',
      rules: '活動說明 / Rules',
      rate: '中獎機率 30%',
      once: '每裝置每日限玩一次',
      prizeTitle: '🎉 中獎禮品',
      prizeInfo: '小禮物（以現場公告為準）。請於入住期間出示此畫面給櫃檯人員兌換。',
      footer: 'SOL Hotel 新竹迎曦大飯店｜本活動依現場公告為準',
    },
    en: {
      title: 'Try Your Luck! Win a Surprise!',
      subtitle: 'Tap <b>START</b>, then pick a gift box. The win rate is fixed at <b>30%</b>. Good luck!',
      pick: 'Pick me!',
      start: 'START',
      rules: 'Rules',
      rate: 'Win Rate 30%',
      once: 'Play limit: once per device per day',
      prizeTitle: '🎉 Prize',
      prizeInfo: 'A small gift (subject to on-site announcement). Please redeem at the front desk during your stay.',
      footer: 'SOL Hotel Hsinchu｜Subject to on-site announcement',
    }
  };

  let lang = 'zh';
  const $ = (sel, root=document)=>root.querySelector(sel);
  const $$ = (sel, root=document)=>Array.from(root.querySelectorAll(sel));

  const langZHBtn = $('#lang-zh');
  const langENBtn = $('#lang-en');
  langZHBtn.addEventListener('click',()=>switchLang('zh'));
  langENBtn.addEventListener('click',()=>switchLang('en'));

  function switchLang(l){
    lang = l; localStorage.setItem('solhotel_lang', l);
    langZHBtn.classList.toggle('active', l==='zh');
    langENBtn.classList.toggle('active', l==='en');
    const dict = I18N[l];
    $$('[data-i18n]').forEach(node=>{
      const key = node.getAttribute('data-i18n');
      node.innerHTML = dict[key] || node.innerHTML;
    });
  }
  switchLang(localStorage.getItem('solhotel_lang') || 'zh');

  /* =====================
     元素參照
     ===================== */
  const btnStart = $('#btnStart');
  const btnRules = $('#btnRules');
  const boxes = $$('.box');
  const result = $('#result');

  const modal = $('#modal');
  const modalClose = $('#modalClose');
  const modalTitle = $('#modalTitle');
  const modalMsg = $('#modalMsg');
  const modalIcon = $('#modalIcon');

  btnRules.addEventListener('click', ()=>{
    openModal('活動說明 / Rules',
      lang==='zh' ? '點擊「開始」後，3 個禮盒會洗牌，請點選其中一個。中獎機率固定 30%，與選擇哪個禮盒無關；小遊戲僅為增添趣味。每台裝置每日限玩一次。' :
      'After tapping START, the three boxes will shuffle. Pick any one. The win rate is fixed at 30% regardless of your choice; the mini-game is for fun only. Limit: once per device per day.'
    , 'ℹ️');
  });

  modalClose.addEventListener('click', ()=> closeModal());
  modal.addEventListener('click', (e)=>{ if(e.target===modal) closeModal(); });

  function openModal(title, msg, icon='🏆'){
    modalTitle.textContent = title;
    modalMsg.textContent = msg;
    modalIcon.textContent = icon;
    modal.classList.add('show');
  }
  function closeModal(){ modal.classList.remove('show'); }

  /* =====================
     每日次數限制 (localStorage)
     ===================== */
  function getTodayKey(){
    const d = new Date();
    return d.getFullYear()+ '-' + (d.getMonth()+1).toString().padStart(2,'0') + '-' + d.getDate().toString().padStart(2,'0');
  }
  function canPlay(){
    const data = JSON.parse(localStorage.getItem(STORAGE_KEY) || '{}');
    const today = getTodayKey();
    return (data[today]||0) < DAILY_LIMIT;
  }
  function recordPlay(){
    const data = JSON.parse(localStorage.getItem(STORAGE_KEY) || '{}');
    const today = getTodayKey();
    data[today] = (data[today]||0) + 1;
    localStorage.setItem(STORAGE_KEY, JSON.stringify(data));
  }

  /* =====================
     Shuffle 動畫 + 點擊流程
     ===================== */
  let gameActive = false;
  let outcome = null; // true=win, false=lose

  btnStart.addEventListener('click', startGame);
  boxes.forEach((b)=> b.addEventListener('click', onPick));

  function startGame(){
    if(!canPlay()){
      openModal('已達次數 / Limit Reached', lang==='zh' ? '您今日的遊戲次數已用完，明日再來試試手氣吧！' : 'You have reached today\'s play limit. Please try again tomorrow!','⏳');
      return;
    }
    gameActive = true; outcome = null; result.textContent=''; result.className='result';
    btnStart.disabled = true; boxes.forEach(b=>{ b.disabled=false; b.classList.add('wiggle'); });

    // 洗牌動畫：隨機交換 order
    const order = [0,1,2];
    let t=0, swaps=0;
    const shuffle = setInterval(()=>{
      // 隨機兩個位置交換
      const i = Math.floor(Math.random()*3); const j = (i + 1 + Math.floor(Math.random()*2))%3;
      [order[i], order[j]] = [order[j], order[i]];
      boxes.forEach((b,idx)=> b.style.order = order[idx]);
      t+=120; swaps++;
      if(swaps>12){ clearInterval(shuffle); boxes.forEach(b=> b.classList.remove('wiggle')); }
    }, 120);
    // 在動畫一開始就決定結果（與選擇無關）
    outcome = Math.random() < WIN_RATE;
  }

  function onPick(e){
    if(!gameActive) return;
    const picked = e.currentTarget;
    boxes.forEach(b=> b.disabled = true);
    recordPlay();

    // 揭曉：顯示結果（與選擇哪個盒子無關）
    setTimeout(()=>{
      if(outcome){
        result.textContent = (lang==='zh'? '恭喜中獎！' : 'Congratulations!');
        result.classList.add('win');
        burstConfetti();
        openModal(lang==='zh'? '恭喜中獎！' : 'Congratulations!', lang==='zh'? PRIZE_TEXT_ZH : PRIZE_TEXT_EN, '🏆');
        picked.innerHTML = '<span class="emoji" aria-hidden="true">🏆</span>';
      } else {
        result.textContent = (lang==='zh'? '未中獎，謝謝參與～' : 'No win this time.');
        result.classList.add('lose');
        openModal(lang==='zh'? '未中獎' : 'No Win', lang==='zh'? MISS_TEXT_ZH : MISS_TEXT_EN, '🎈');
        picked.innerHTML = '<span class="emoji" aria-hidden="true">🎈</span>';
      }
      btnStart.disabled = false; gameActive=false;
    }, 400);
  }

  /* =====================
     輕量彩帶特效（無外掛）
     ===================== */
  const canvas = document.getElementById('confetti');
  const ctx = canvas.getContext('2d');
  let confettiPieces = []; let confettiTimer=null; let resizeTimer=null;

  function resize(){ canvas.width = window.innerWidth; canvas.height = window.innerHeight; }
  window.addEventListener('resize', ()=>{ clearTimeout(resizeTimer); resizeTimer=setTimeout(resize, 100); });
  resize();

  function burstConfetti(){
    const count = 220;
    confettiPieces = Array.from({length: count}).map(()=>({
      x: Math.random()*canvas.width,
      y: -10 - Math.random()*50,
      r: 2 + Math.random()*4,
      vx: -2 + Math.random()*4,
      vy: 2 + Math.random()*3,
      a: Math.random()*Math.PI*2,
      s: .02 + Math.random()*.04,
    }));
    if(!confettiTimer){
      confettiTimer = requestAnimationFrame(drawConfetti);
      setTimeout(()=>{ cancelAnimationFrame(confettiTimer); confettiTimer=null; }, 2000);
    }
  }

  function drawConfetti(){
    ctx.clearRect(0,0,canvas.width, canvas.height);
    confettiPieces.forEach(p=>{
      p.x += p.vx; p.y += p.vy; p.a += p.s;
      if(p.y > canvas.height+20) p.y = -10;
      ctx.save();
      ctx.translate(p.x, p.y);
      ctx.rotate(p.a);
      // 不指定顏色，使用 HSL 隨機 (瀏覽器自動配色)
      ctx.fillStyle = `hsl(${Math.floor(Math.random()*360)}, 90%, 60%)`;
      ctx.fillRect(-p.r, -p.r, p.r*2, p.r*2);
      ctx.restore();
    });
    confettiTimer = requestAnimationFrame(drawConfetti);
  }

  // 初始狀態：盒子不可點，待按「開始」
  boxes.forEach(b=> b.disabled = true);
</script>
</body>
</html>
