<!DOCTYPE html>
<html lang="ckb" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>نامەیەک بۆ تۆ</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Amiri:ital,wght@0,400;0,700;1,400&family=Vazirmatn:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --paper:#FBF1E4;
    --paper-deep:#F3DCC9;
    --ink:#4A2E2A;
    --ink-soft:#7A5A4F;
    --rose:#C8536B;
    --rose-deep:#A23B52;
    --gold:#C98B3B;
    --gold-light:#E9C27D;
    --teal:#2F5D58;
  }

  *{box-sizing:border-box;}

  html,body{
    margin:0;
    padding:0;
    min-height:100%;
    background:
      radial-gradient(circle at 15% 10%, #F7E3CE 0%, transparent 45%),
      radial-gradient(circle at 85% 85%, #F1D6CE 0%, transparent 50%),
      linear-gradient(160deg, #FBF1E4 0%, #F7E6D6 55%, #F3DCC9 100%);
    font-family:'Vazirmatn', sans-serif;
    color:var(--ink);
    overflow-x:hidden;
  }

  .sr-only{
    position:absolute;
    width:1px;height:1px;
    overflow:hidden;
    clip:rect(0,0,0,0);
  }

  /* decorative kilim-style border pattern */
  .kilim-border{
    width:100%;
    height:18px;
    background-repeat:repeat-x;
    background-size:36px 18px;
  }
  .kilim-top{
    background-image:
      linear-gradient(45deg, transparent 47%, var(--gold) 47%, var(--gold) 53%, transparent 53%),
      linear-gradient(135deg, transparent 47%, var(--rose-deep) 47%, var(--rose-deep) 53%, transparent 53%);
    opacity:0.55;
  }

  .stage{
    position:relative;
    max-width:720px;
    margin:0 auto;
    padding:48px 24px 80px;
  }

  /* floating petals */
  .petals{
    position:fixed;
    inset:0;
    pointer-events:none;
    overflow:hidden;
    z-index:0;
  }
  .petal{
    position:absolute;
    top:-40px;
    width:10px;
    height:10px;
    border-radius:60% 0 60% 0;
    background:var(--rose);
    opacity:0.35;
    animation:fall linear infinite;
  }
  @keyframes fall{
    to{ transform:translateY(110vh) rotate(360deg); }
  }

  /* ============ ENVELOPE ============ */
  .envelope-wrap{
    position:relative;
    z-index:1;
    display:flex;
    flex-direction:column;
    align-items:center;
    justify-content:center;
    min-height:70vh;
    text-align:center;
  }

  .eyebrow{
    font-size:13px;
    letter-spacing:0.04em;
    color:var(--ink-soft);
    margin:0 0 10px;
  }

  .envelope{
    position:relative;
    width:280px;
    height:190px;
    cursor:pointer;
    margin:18px 0 28px;
    perspective:1000px;
  }

  .env-body{
    position:absolute;
    inset:0;
    background:linear-gradient(180deg, #F4E3CE, #ECD3B4);
    border:0.5px solid #D9B98A;
    border-radius:6px;
    box-shadow:0 18px 40px -18px rgba(74,46,42,0.35);
  }

  .env-body::before, .env-body::after{
    content:"";
    position:absolute;
    top:0;
    width:0;
    height:0;
    border-style:solid;
  }
  .env-body::before{
    right:0;
    border-width:95px 140px 0 0;
    border-color:#E7CBA5 transparent transparent transparent;
  }
  .env-body::after{
    left:0;
    border-width:95px 0 0 140px;
    border-color:transparent transparent transparent #E7CBA5;
  }

  .env-flap{
    position:absolute;
    top:0;
    left:0;
    width:280px;
    height:100px;
    background:linear-gradient(160deg, #EFD9B9, #E2C09A);
    clip-path:polygon(0 0, 100% 0, 50% 88%);
    transform-origin:top center;
    transform-style:preserve-3d;
    transition:transform 0.9s cubic-bezier(.5,0,.2,1);
    border-radius:4px 4px 0 0;
    z-index:3;
  }

  .seal{
    position:absolute;
    top:60px;
    left:50%;
    transform:translate(-50%,0);
    width:54px;
    height:54px;
    border-radius:50%;
    background:radial-gradient(circle at 35% 30%, var(--rose) 0%, var(--rose-deep) 70%);
    box-shadow:0 6px 14px -4px rgba(162,59,82,0.55);
    display:flex;
    align-items:center;
    justify-content:center;
    z-index:4;
    transition:transform 0.4s ease, opacity 0.4s ease;
  }
  .seal svg{ width:24px; height:24px; }
  .envelope:hover .seal{ transform:translate(-50%,0) scale(1.06); }

  .env-hint{
    margin-top:6px;
    font-size:13px;
    color:var(--ink-soft);
  }

  .envelope.opened .env-flap{
    transform:rotateX(160deg);
    z-index:1;
  }
  .envelope.opened .seal{
    opacity:0;
    transform:translate(-50%,-10px) scale(0.7);
  }

  .letter-peek{
    position:absolute;
    left:18px;
    right:18px;
    bottom:14px;
    height:120px;
    background:var(--paper);
    border:0.5px solid #E3CDAE;
    border-radius:4px;
    z-index:2;
    transition:transform 0.7s cubic-bezier(.4,0,.2,1) 0.15s, opacity 0.6s ease 0.15s;
  }
  .envelope.opened .letter-peek{
    transform:translateY(-230px) scale(1.02);
    opacity:0;
  }

  /* ============ LETTER ============ */
  .letter{
    position:relative;
    z-index:1;
    max-width:620px;
    margin:0 auto;
    background:var(--paper);
    border-radius:18px;
    padding:46px 38px 40px;
    box-shadow:0 30px 60px -30px rgba(74,46,42,0.25), 0 0 0 0.5px #E7D2B4;
    opacity:0;
    transform:translateY(24px);
    transition:opacity 0.7s ease, transform 0.7s ease;
  }
  .letter.show{
    opacity:1;
    transform:translateY(0);
  }

  .letter-heading{
    font-family:'Amiri', serif;
    font-size:28px;
    font-weight:700;
    color:var(--rose-deep);
    text-align:center;
    margin:0 0 4px;
  }
  .letter-sub{
    text-align:center;
    font-size:13px;
    color:var(--ink-soft);
    margin:0 0 28px;
    letter-spacing:0.02em;
  }

  .letter p{
    font-size:17px;
    line-height:2.05;
    margin:0 0 20px;
    color:var(--ink);
  }
  .letter p:first-of-type::first-letter{
    font-family:'Amiri', serif;
    font-size:30px;
    color:var(--gold);
  }

  .signature{
    margin-top:30px;
    text-align:left;
    font-family:'Amiri', serif;
    font-style:italic;
    font-size:19px;
    color:var(--rose-deep);
  }
  .signature .sig-name{
    display:block;
    margin-top:4px;
    font-size:21px;
  }
  .editable-hint{
    display:inline-block;
    margin-top:14px;
    font-family:'Vazirmatn', sans-serif;
    font-style:normal;
    font-size:12px;
    color:var(--ink-soft);
    background:var(--paper-deep);
    padding:4px 10px;
    border-radius:20px;
  }

  /* ============ REASONS CARDS ============ */
  .section-title{
    text-align:center;
    font-family:'Amiri', serif;
    font-size:22px;
    color:var(--ink);
    margin:64px 0 6px;
  }
  .section-note{
    text-align:center;
    font-size:13px;
    color:var(--ink-soft);
    margin:0 0 28px;
  }

  .cards-grid{
    display:grid;
    grid-template-columns:repeat(2, 1fr);
    gap:16px;
  }
  @media (max-width:520px){
    .cards-grid{ grid-template-columns:1fr; }
  }

  .flip-card{
    perspective:1200px;
    height:150px;
    cursor:pointer;
  }
  .flip-inner{
    position:relative;
    width:100%;
    height:100%;
    transition:transform 0.6s cubic-bezier(.4,0,.2,1);
    transform-style:preserve-3d;
  }
  .flip-card.flipped .flip-inner{ transform:rotateY(180deg); }

  .flip-face{
    position:absolute;
    inset:0;
    border-radius:14px;
    display:flex;
    flex-direction:column;
    align-items:center;
    justify-content:center;
    padding:14px;
    text-align:center;
    backface-visibility:hidden;
  }
  .flip-front{
    background:linear-gradient(160deg, #fff 0%, var(--paper-deep) 100%);
    border:0.5px solid #E7D2B4;
  }
  .flip-front .num{
    font-family:'Amiri', serif;
    font-size:26px;
    color:var(--gold);
    margin-bottom:6px;
  }
  .flip-front .lbl{
    font-size:13px;
    color:var(--ink-soft);
  }
  .flip-back{
    background:linear-gradient(160deg, var(--rose) 0%, var(--rose-deep) 100%);
    color:#FCEAEE;
    transform:rotateY(180deg);
    font-size:14px;
    line-height:1.75;
  }

  /* ============ INBOX PREVIEW ============ */
  .inbox-card{
    width:100%;
    max-width:340px;
    background:#fff;
    border-radius:18px;
    box-shadow:0 18px 36px -18px rgba(74,46,42,0.3), 0 0 0 0.5px #E7D2B4;
    overflow:hidden;
    margin-bottom:22px;
  }
  .inbox-bar{
    background:linear-gradient(160deg, var(--rose), var(--rose-deep));
    color:#FCEAEE;
    font-size:13px;
    font-weight:500;
    padding:9px 16px;
    text-align:center;
  }
  .inbox-row{
    display:flex;
    align-items:flex-start;
    gap:10px;
    padding:14px 14px 16px;
    position:relative;
  }
  .avatar{
    flex:0 0 46px;
    width:46px;
    height:46px;
    border-radius:50%;
    background:var(--gold-light);
    display:flex;
    align-items:center;
    justify-content:center;
    overflow:hidden;
  }
  .avatar svg{ width:30px; height:30px; }
  .inbox-text{ flex:1; text-align:right; min-width:0; }
  .inbox-top{
    display:flex;
    justify-content:space-between;
    align-items:baseline;
    gap:8px;
  }
  .inbox-name{
    font-size:14px;
    font-weight:600;
    color:var(--ink);
  }
  .inbox-time{
    font-size:11px;
    color:var(--ink-soft);
    flex:0 0 auto;
  }
  .inbox-preview{
    font-size:13px;
    color:var(--ink-soft);
    margin-top:3px;
    line-height:1.5;
    overflow:hidden;
    text-overflow:ellipsis;
    display:-webkit-box;
    -webkit-line-clamp:2;
    -webkit-box-orient:vertical;
  }
  .unread-dot{
    position:absolute;
    top:14px;
    left:14px;
    width:9px;
    height:9px;
    border-radius:50%;
    background:var(--rose);
    animation:pulse-dot 1.6s ease-in-out infinite;
  }
  @keyframes pulse-dot{
    0%, 100%{ box-shadow:0 0 0 0 rgba(200,83,107,0.5); }
    50%{ box-shadow:0 0 0 5px rgba(200,83,107,0); }
  }

  /* ============ LOVE METER ============ */
  .meter-box{
    margin-top:56px;
    text-align:center;
  }
  .meter-readout{
    font-family:'Amiri', serif;
    font-size:30px;
    color:var(--rose-deep);
    margin:6px 0 4px;
  }
  .meter-msg{
    font-size:13px;
    color:var(--ink-soft);
    min-height:18px;
    margin-bottom:14px;
  }
  .meter-box input[type="range"]{
    width:100%;
    max-width:380px;
    -webkit-appearance:none;
    height:10px;
    border-radius:6px;
    background:linear-gradient(90deg, var(--gold-light), var(--rose));
    outline:none;
  }
  .meter-box input[type="range"]::-webkit-slider-thumb{
    -webkit-appearance:none;
    width:26px;
    height:26px;
    border-radius:50%;
    background:#fff;
    border:3px solid var(--rose-deep);
    cursor:pointer;
  }
  .meter-box input[type="range"]::-moz-range-thumb{
    width:26px;
    height:26px;
    border-radius:50%;
    background:#fff;
    border:3px solid var(--rose-deep);
    cursor:pointer;
  }

  /* ============ CONFETTI ============ */
  .confetti-piece{
    position:fixed;
    top:-20px;
    width:8px;
    height:14px;
    z-index:50;
    pointer-events:none;
    animation:confetti-fall ease-in forwards;
  }
  @keyframes confetti-fall{
    to{ transform:translateY(100vh) rotate(540deg); opacity:0.4; }
  }

  /* ============ JOKE BOX ============ */
  .joke-box{
    margin-top:60px;
    text-align:center;
    background:var(--paper-deep);
    border-radius:18px;
    padding:30px 26px 34px;
  }
  .joke-text{
    font-size:16px;
    line-height:1.9;
    min-height:54px;
    margin:14px 0 18px;
    color:var(--ink);
  }
  .joke-btn{
    font-family:'Vazirmatn', sans-serif;
    font-size:14px;
    font-weight:500;
    color:#FCEAEE;
    background:linear-gradient(160deg, var(--teal), #1F4541);
    border:none;
    border-radius:30px;
    padding:11px 26px;
    cursor:pointer;
    transition:transform 0.15s ease;
  }
  .joke-btn:active{ transform:scale(0.96); }

  footer{
    text-align:center;
    margin-top:50px;
    font-size:12px;
    color:var(--ink-soft);
  }

  @media (prefers-reduced-motion: reduce){
    *{ animation:none !important; transition:none !important; }
  }
</style>
</head>
<body>

<h1 class="sr-only">نامەیەکی گەرم و گاڵتەجاڕانە بۆ پووری خۆشەویست، بە شێوەی سایتێکی بۆنی نامە کە دەکرێت بکرێتەوە</h1>

<div class="kilim-border kilim-top"></div>

<div class="petals" id="petals"></div>

<div class="stage">

  <div class="envelope-wrap" id="envelopeWrap">
    <div class="inbox-card">
      <div class="inbox-bar">پەیامەکان</div>
      <div class="inbox-row">
        <div class="unread-dot"></div>
        <div class="avatar">
          <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <circle cx="12" cy="12" r="11" fill="#C98B3B" opacity="0.18"/>
            <circle cx="9" cy="10.5" r="1.3" fill="#A23B52"/>
            <circle cx="15" cy="10.5" r="1.3" fill="#A23B52"/>
            <path d="M8 15c1.2 1.4 2.6 2 4 2s2.8-0.6 4-2" stroke="#A23B52" stroke-width="1.6" stroke-linecap="round" fill="none"/>
          </svg>
        </div>
        <div class="inbox-text">
          <div class="inbox-top">
            <span class="inbox-time">ئێستا</span>
            <span class="inbox-name">کەسێکی خۆشت</span>
          </div>
          <p class="inbox-preview">سڵاو... هەستم کرد دەبێ ئەمەم بنووسم بۆت. کلیک بکە و بخوێنەرەوە، چونکە شایستەیت 💌</p>
        </div>
      </div>
    </div>
    <p class="eyebrow">یەک نامەی تایبەت بۆت هاتووە...</p>
    <div class="envelope" id="envelope" role="button" tabindex="0" aria-label="نامەکە بکەرەوە">
      <div class="env-body"></div>
      <div class="letter-peek" id="letterPeek"></div>
      <div class="env-flap" id="envFlap"></div>
      <div class="seal" id="seal">
        <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M12 21s-7.5-4.6-10-9.3C0.3 8 2 4 6 4c2.3 0 4 1.4 6 4 2-2.6 3.7-4 6-4 4 0 5.7 4 4 7.7C19.5 16.4 12 21 12 21z" fill="#FCEAEE"/>
        </svg>
      </div>
    </div>
    <p class="env-hint" id="envHint">کلیکی لەسەر مۆرەکە بکە بۆ کردنەوەی نامەکە</p>
  </div>

  <article class="letter" id="letter">
    <h2 class="letter-heading">بۆ پووری خۆشەویستم</h2>
    <p class="letter-sub">نامەیەک، تەنها بۆ ئەوەی بزانیت چەند بەنرخیت</p>

    <p>سڵاو لێت، گیان! تۆ کەسێکیت کە لەگەڵی هەرگیز پێویست بە وشەی ڕێکوپێک نییە، بۆیە ڕاستەوخۆ دەڵێم: لەگەڵتدا هەمیشە پێکەنین و ئەو کاتە خۆشانە بووە کە قەت بیرناچێتەوە .</p>

    <p>هەندێک جار زیادە لە حەد گاڵتەم پێکردووی، هەندێک جاریش تۆ گاڵتەت پێکردووم، بەڵام هەرگیز ئەوە نەبووە هۆی ئەوەی لێک دووربکەوینەوە، بەڵکوو هەر جارە زیاتر یەکترمان ناسیوە. ئەم نامەیە تەنها بۆیە نووسیومە، بێ هیچ بۆنە، بۆ ئەوەی بزانیت جێگەیەکی تایبەتیت لە دڵم هەیە.</p>

    <p>بمێنیتەوە ئەوەی هەمیشە بوویت، کەسێکی گەرم، خۆش و بێ وێنە.</p>

    <p class="signature">
      بە خۆشەویستییەکی زۆری خۆشکەزاکەت
      <span class="sig-name">ئارا</span>
    </p>
  </article>

  <div class="meter-box" id="meterBox" style="display:none;">
    <p class="section-note" style="margin-bottom:0;">پێوەری دیاری کردنی خۆشەویستی؛دیاری بکە</p>
    <p class="meter-readout" id="meterReadout">٥٠٪</p>
    <p class="meter-msg" id="meterMsg">سلایدەرەکە بەرەو ڕاست بگوازەرەوە...</p>
    <input type="range" min="0" max="100" value="50" id="meterRange">
  </div>

  <h3 class="section-title" id="reasonsTitle" style="display:none;">چەند هۆکار بۆ خۆشەویستیت</h3>
  <p class="section-note" id="reasonsNote" style="display:none;">کلیک لەسەر هەر کارتێک بکە بۆ بینینی پاشەوەی</p>

  <div class="cards-grid" id="cardsGrid" style="display:none;"></div>

  <div class="joke-box" id="jokeBox" style="display:none;">
    <p class="section-note" style="margin:0;">کەمێک گاڵتە، چونکە شایستەی پێکەنینیت</p>
    <p class="joke-text" id="jokeText">دوگمەکە دابگرە بۆ یەکەم گاڵتە...</p>
    <button class="joke-btn" id="jokeBtn">گاڵتەیەکی تر بدەرە</button>
  </div>

  <footer id="pageFooter" style="display:none;">بە دڵی گەرم دروستکراوە &middot; تەنها بۆ تۆ</footer>

</div>

<div class="kilim-border kilim-top"></div>

<script>
  // floating petals
  var petalHost = document.getElementById('petals');
  var colors = ['#C8536B', '#C98B3B', '#2F5D58'];
  for (var i = 0; i < 14; i++) {
    var p = document.createElement('div');
    p.className = 'petal';
    var size = 6 + Math.random() * 8;
    p.style.width = size + 'px';
    p.style.height = size + 'px';
    p.style.left = (Math.random() * 100) + 'vw';
    p.style.background = colors[i % colors.length];
    p.style.animationDuration = (10 + Math.random() * 12) + 's';
    p.style.animationDelay = (Math.random() * 10) + 's';
    petalHost.appendChild(p);
  }

  // envelope open
  var envelope = document.getElementById('envelope');
  var letter = document.getElementById('letter');
  var envHint = document.getElementById('envHint');
  var revealEls = ['reasonsTitle','reasonsNote','cardsGrid','jokeBox','pageFooter','meterBox'].map(function(id){return document.getElementById(id);});
  var opened = false;

  function burstConfetti(){
    var colors = ['#C8536B', '#C98B3B', '#2F5D58', '#A23B52'];
    for (var i = 0; i < 36; i++) {
      var c = document.createElement('div');
      c.className = 'confetti-piece';
      c.style.left = (Math.random() * 100) + 'vw';
      c.style.background = colors[i % colors.length];
      c.style.animationDuration = (1.4 + Math.random() * 1.2) + 's';
      c.style.animationDelay = (Math.random() * 0.3) + 's';
      document.body.appendChild(c);
      (function(el){ setTimeout(function(){ el.remove(); }, 3200); })(c);
    }
  }

  function openEnvelope(){
    if (opened) return;
    opened = true;
    envelope.classList.add('opened');
    envHint.style.opacity = '0';
    burstConfetti();
    setTimeout(function(){
      letter.classList.add('show');
      revealEls.forEach(function(el){ el.style.display = (el.id === 'cardsGrid') ? 'grid' : 'block'; });
      letter.scrollIntoView({ behavior: 'smooth', block: 'start' });
    }, 550);
  }

  envelope.addEventListener('click', openEnvelope);
  envelope.addEventListener('keypress', function(e){
    if (e.key === 'Enter' || e.key === ' ') openEnvelope();
  });

  // flip cards
  var reasons = [
    { back: "چونکە کاتێک لەگەڵتدام، پێکەنین وەک هەوای ئاسایی دێتە سەرم، بێ هیچ هۆکارێکی تەواو." },
    { back: "چونکە هەرکاتێک پێویستم بە کەسێک هەبووە بۆ گوێگرتن، تۆ بێ پرسیارکردن لەوێ بوویت." },
    { back: "چونکە تۆ پووریت، بەڵام لە ڕاستیدا زیاتر لە برا یان خوشکیت." },
    { back: "چونکە یادەوەرییەکانی منداڵیم، زۆربەیان تۆیان تێدا بوویت." },
    { back: "چونکە تۆ یەکێکیت کە هەرگیز پێویست نییە خۆت بگۆڕیت تاکوو لەگەڵم بێیتە دەنگ." },
    { back: "چونکە لای تۆ، هەر شتێکی سادە دەتوانێت ببێتە یادەوەرییەکی هەتاهەتایی." },
    { back: "چونکە تۆ یەکێکیت کە بێ پلانیشت، هەمیشە ڕێگات بۆ کاتی خۆش دۆزیوەتەوە." },
    { back: "چونکە دڵم پێ ئاسوودەیە کاتێک دەزانم تۆ لەوێی، تەنانەت بێ ئەوەی پێم بڵێیت." }
  ];
  var grid = document.getElementById('cardsGrid');
  reasons.forEach(function(r, idx){
    var card = document.createElement('div');
    card.className = 'flip-card';
    card.innerHTML =
      '<div class="flip-inner">' +
        '<div class="flip-face flip-front"><div class="num">' + (idx+1) + '</div><div class="lbl">کلیک بکە</div></div>' +
        '<div class="flip-face flip-back">' + r.back + '</div>' +
      '</div>';
    card.addEventListener('click', function(){ this.classList.toggle('flipped'); });
    grid.appendChild(card);
  });

  // jokes
  var jokes = [
    "دڵنیام ئەگەر خەڵاتی گەورەترین رووخۆشی خێزان هەبووایە، تۆ دەیبردەوە.",
    "ئاگاداربە، خۆشەویستیی زیادە لەو ڕادەیە دەبێتە هۆی پێکەنینی بەردەوام.",
    "تۆ وەک قاوەی بەیانییانیت، بەبێت ڕۆژ ناڕێک دەڕوات.",
    "بیرت دەخەمەوە: مۆڵەتی پێکەنین و گاڵتەت لەلایەن هەموومان پەسەند کراوە.",
    "ئەگەر پێکەنین وزە بووایە، تۆ بەتەنها دەتوانیت شارۆچکەیەک ڕووناک بکەیتەوە."
  ];
  var jokeIndex = -1;
  var jokeText = document.getElementById('jokeText');
  document.getElementById('jokeBtn').addEventListener('click', function(){
    jokeIndex = (jokeIndex + 1) % jokes.length;
    jokeText.textContent = jokes[jokeIndex];
  });

  // love meter
  var toArabicDigits = function(n){
    var map = ['٠','١','٢','٣','٤','٥','٦','٧','٨','٩'];
    return String(n).split('').map(function(ch){ return /[0-9]/.test(ch) ? map[ch] : ch; }).join('');
  };
  var meterMessages = [
    { max: 20, msg: "ها؟ کەمێک سلایدەرەکە بگوازەرەوە، خۆشەویستی لێرە نازانم چۆن کۆتایی پێ بهات!" },
    { max: 50, msg: "باشە، بەڵام دڵنیام ڕاستی زیاترە لەمە..." },
    { max: 80, msg: "ئاهان، نزیک بووینەتەوە لە ڕاستی!" },
    { max: 99, msg: "زۆر نزیکیت، بەڵام هێشتا تەواو نییت!" },
    { max: 100, msg: "ئەوەی! ١٠٠٪ خۆشەویستی، وەک ڕاستی!" }
  ];
  var meterRange = document.getElementById('meterRange');
  var meterReadout = document.getElementById('meterReadout');
  var meterMsg = document.getElementById('meterMsg');
  function updateMeter(){
    var v = parseInt(meterRange.value, 10);
    meterReadout.textContent = toArabicDigits(v) + '٪';
    for (var i = 0; i < meterMessages.length; i++){
      if (v <= meterMessages[i].max){ meterMsg.textContent = meterMessages[i].msg; break; }
    }
  }
  meterRange.addEventListener('input', updateMeter);
  updateMeter();
</script>

</body>
</html>
