Thiep moi ky yeu · HTML
Copy

<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Thiệp Mời Kỷ Yếu – A5 K62</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=Crimson+Text:ital,wght@0,400;0,600;1,400&family=Dancing+Script:wght@600;700&display=swap" rel="stylesheet">
<style>
  :root {
    --red: #C0392B;
    --dark-red: #922B21;
    --gold: #D4AF37;
    --light-gold: #F0D080;
    --cream: #FDF8F0;
    --dark: #2C1810;
    --shadow: rgba(0,0,0,0.4);
  }
 
  * { margin: 0; padding: 0; box-sizing: border-box; }
 
  body {
    background: radial-gradient(ellipse at center, #1a0a00 0%, #0d0500 60%, #000 100%);
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: 'Crimson Text', serif;
    overflow: hidden;
    cursor: none;
  }
 
  /* Custom cursor – con chuột */
  #cursor {
    position: fixed;
    pointer-events: none;
    z-index: 9999;
    transform: translate(-50%, -50%);
    transition: transform 0.1s;
  }
  #cursor svg {
    width: 36px; height: 36px;
    filter: drop-shadow(0 0 6px var(--gold));
    animation: cursorFloat 2s ease-in-out infinite;
  }
  @keyframes cursorFloat {
    0%,100% { transform: rotate(-15deg) translateY(0); }
    50%      { transform: rotate(-15deg) translateY(-4px); }
  }
 
  /* Particle canvas */
  #particles { position: fixed; inset: 0; pointer-events: none; z-index: 0; }
 
  /* Scene container */
  .scene {
    perspective: 1200px;
    width: 380px;
    height: 260px;
    position: relative;
    z-index: 10;
    cursor: none;
  }
 
  /* ===== ENVELOPE ===== */
  .envelope-wrap {
    width: 100%; height: 100%;
    transform-style: preserve-3d;
    position: relative;
    transition: transform 0.6s cubic-bezier(.25,.8,.25,1);
    animation: floatEnvelope 4s ease-in-out infinite;
  }
  .envelope-wrap.tilt {
    /* handled by JS */
  }
  .envelope-wrap.spin360 {
    animation: spin360 2s ease-in-out forwards;
  }
  
  @keyframes floatEnvelope {
    0%, 100% { transform: translateY(0px) rotateZ(0deg); }
    25%      { transform: translateY(-8px) rotateZ(1deg); }
    50%      { transform: translateY(-4px) rotateZ(0deg); }
    75%      { transform: translateY(-12px) rotateZ(-1deg); }
  }
  
  @keyframes spin360 {
    0%   { transform: rotateY(0deg) rotateX(0deg) scale(1); }
    25%  { transform: rotateY(90deg) rotateX(15deg) scale(0.9); }
    50%  { transform: rotateY(180deg) rotateX(0deg) scale(0.95); }
    75%  { transform: rotateY(270deg) rotateX(-15deg) scale(0.9); }
    100% { transform: rotateY(360deg) rotateX(0deg) scale(1); }
  }
 
  /* Envelope body */
  .envelope-body {
    width: 380px; height: 260px;
    position: absolute;
    background: linear-gradient(145deg, #8B0000 0%, #C0392B 40%, #922B21 70%, #6B1A10 100%);
    border-radius: 8px;
    box-shadow:
      0 0 60px rgba(192,57,43,0.5),
      0 20px 80px rgba(0,0,0,0.8),
      inset 0 1px 0 rgba(255,255,255,0.1);
    overflow: hidden;
    backface-visibility: hidden;
  }
 
  /* Gold border */
  .envelope-body::before {
    content: '';
    position: absolute;
    inset: 8px;
    border: 1.5px solid rgba(212,175,55,0.5);
    border-radius: 4px;
    pointer-events: none;
  }
  /* Diagonal lines texture */
  .envelope-body::after {
    content: '';
    position: absolute;
    inset: 0;
    background:
      repeating-linear-gradient(
        45deg,
        transparent,
        transparent 20px,
        rgba(0,0,0,0.08) 20px,
        rgba(0,0,0,0.08) 21px
      );
    border-radius: 8px;
  }
 
  /* Back flap (bottom fold) */
  .flap-back {
    position: absolute;
    bottom: 0; left: 0; right: 0;
    height: 130px;
    background: linear-gradient(to top, #6B1A10, #922B21);
    clip-path: polygon(0 100%, 50% 0%, 100% 100%);
    border-radius: 0 0 8px 8px;
    z-index: 1;
  }
 
  /* Side flaps */
  .flap-left, .flap-right {
    position: absolute;
    top: 0; bottom: 0;
    width: 190px;
    z-index: 2;
  }
  .flap-left {
    left: 0;
    background: linear-gradient(to right, #8B0000, #A93226);
    clip-path: polygon(0 0, 100% 50%, 0 100%);
  }
  .flap-right {
    right: 0;
    background: linear-gradient(to left, #8B0000, #A93226);
    clip-path: polygon(100% 0, 0 50%, 100% 100%);
  }
 
  /* Top flap */
  .flap-top {
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 130px;
    background: linear-gradient(to bottom, #C0392B, #A93226);
    clip-path: polygon(0 0, 50% 100%, 100% 0);
    z-index: 5;
    transform-origin: top center;
    transition: transform 0.8s cubic-bezier(.4,0,.2,1);
    border-radius: 8px 8px 0 0;
  }
  .flap-top.open {
    transform: rotateX(-180deg);
  }
 
  /* Seal */
  .seal {
    position: absolute;
    top: 50%; left: 50%;
    transform: translate(-50%, -50%);
    z-index: 10;
    width: 72px; height: 72px;
    cursor: pointer;
    transition: transform 0.3s, box-shadow 0.3s;
  }
  .seal:hover { transform: translate(-50%, -50%) scale(1.08); }
  .seal.broken { animation: sealBreak 0.4s forwards; }
  @keyframes sealBreak {
    0%   { transform: translate(-50%,-50%) scale(1) rotate(0); opacity: 1; }
    40%  { transform: translate(-50%,-50%) scale(1.2) rotate(10deg); }
    100% { transform: translate(-50%,-50%) scale(0) rotate(25deg); opacity: 0; }
  }
 
  .seal-circle {
    width: 72px; height: 72px;
    border-radius: 50%;
    background: radial-gradient(circle at 35% 35%, #F5D060, #D4AF37 50%, #A0780A);
    border: 2px solid #F0D080;
    box-shadow: 0 0 20px rgba(212,175,55,0.8), 0 4px 12px rgba(0,0,0,0.5);
    display: flex; align-items: center; justify-content: center;
    overflow: hidden;
  }
  .seal-logo {
    width: 60px; height: 60px;
    border-radius: 50%;
    object-fit: cover;
    opacity: 0.95;
  }
 
  /* Hover hint */
  .hint {
    position: absolute;
    bottom: -36px; left: 50%;
    transform: translateX(-50%);
    color: var(--gold);
    font-family: 'Crimson Text', serif;
    font-style: italic;
    font-size: 13px;
    opacity: 0.8;
    white-space: nowrap;
    animation: pulse 2s ease-in-out infinite;
  }
  @keyframes pulse { 0%,100%{opacity:.4} 50%{opacity:1} }
 
  /* ===== LETTER PANEL ===== */
  .letter-container {
    position: fixed;
    inset: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 100;
    pointer-events: none;
    opacity: 0;
    transition: opacity 0.5s;
  }
  .letter-container.visible {
    opacity: 1;
    pointer-events: all;
  }
 
  .letter-bg {
    position: absolute;
    inset: 0;
    background: rgba(0,0,0,0.75);
    backdrop-filter: blur(4px);
  }
 
  /* Letter 3D scene */
  .letter-scene {
    perspective: 900px;
    position: relative;
    z-index: 2;
    width: min(600px, 94vw);
  }
 
  .letter-card {
    transform-style: preserve-3d;
    animation: letterRise 0.8s cubic-bezier(.2,.8,.3,1) forwards;
  }
  @keyframes letterRise {
    from { transform: translateY(80px) rotateX(20deg); opacity: 0; }
    to   { transform: translateY(0) rotateX(0deg); opacity: 1; }
  }
 
  .letter-paper {
    background:
      linear-gradient(160deg, #FFFDF5 0%, #FDF8E8 50%, #FAF3D5 100%);
    border-radius: 4px;
    padding: 44px 48px 36px;
    box-shadow:
      0 0 0 1px rgba(212,175,55,0.25),
      0 2px 0 rgba(212,175,55,0.4),
      0 30px 80px rgba(0,0,0,0.7),
      0 0 120px rgba(192,57,43,0.15),
      inset 0 0 40px rgba(212,175,55,0.06);
    position: relative;
    overflow: hidden;
    /* 3D rotation via JS */
    transform-style: preserve-3d;
    transition: transform 0.15s ease-out;
  }
 
  /* Paper texture lines */
  .letter-paper::before {
    content: '';
    position: absolute;
    inset: 0;
    background: repeating-linear-gradient(
      transparent, transparent 27px,
      rgba(180,140,60,0.12) 27px, rgba(180,140,60,0.12) 28px
    );
    top: 76px;
  }
 
  /* Decorative corner */
  .corner {
    position: absolute;
    width: 50px; height: 50px;
    border-color: var(--gold);
    border-style: solid;
    opacity: 0.5;
  }
  .corner.tl { top: 14px; left: 14px; border-width: 2px 0 0 2px; }
  .corner.tr { top: 14px; right: 14px; border-width: 2px 2px 0 0; }
  .corner.bl { bottom: 14px; left: 14px; border-width: 0 0 2px 2px; }
  .corner.br { bottom: 14px; right: 14px; border-width: 0 2px 2px 0; }
 
  /* Gold divider */
  .divider {
    width: 80%; height: 1px;
    background: linear-gradient(to right, transparent, var(--gold), transparent);
    margin: 14px auto;
    opacity: 0.6;
  }
 
  /* Letter content */
  .letter-title {
    font-family: 'Playfair Display', serif;
    font-size: 22px;
    font-weight: 700;
    color: var(--dark-red);
    text-align: center;
    letter-spacing: 2px;
    text-shadow: 0 1px 2px rgba(0,0,0,0.1);
    margin-bottom: 4px;
  }
 
  .letter-sub {
    font-family: 'Dancing Script', cursive;
    font-size: 15px;
    color: var(--gold);
    text-align: center;
    margin-bottom: 4px;
  }
 
  .letter-body {
    font-family: 'Crimson Text', serif;
    font-size: 15.5px;
    line-height: 1.8;
    color: #3A2010;
    margin: 10px 0;
  }
 
  .letter-body p { margin-bottom: 10px; }
 
  .info-row {
    display: flex;
    align-items: baseline;
    gap: 8px;
    margin: 6px 0;
    font-size: 15.5px;
    color: #3A2010;
  }
  .info-icon { font-size: 16px; flex-shrink: 0; }
  .info-label { font-weight: 600; color: var(--dark-red); }
  .info-val { font-style: italic; }
 
  .letter-footer {
    font-family: 'Dancing Script', cursive;
    font-size: 18px;
    color: var(--dark-red);
    text-align: right;
    margin-top: 10px;
  }
  .letter-sign {
    font-family: 'Playfair Display', serif;
    font-size: 15px;
    font-style: italic;
    color: #5A3020;
    text-align: right;
    margin-top: 2px;
  }
 
  /* Close button */
  .close-btn {
    position: absolute;
    top: 12px; right: 16px;
    background: none;
    border: 1.5px solid rgba(192,57,43,0.4);
    color: var(--red);
    width: 32px; height: 32px;
    border-radius: 50%;
    font-size: 16px;
    cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    transition: background 0.2s, color 0.2s;
    z-index: 20;
    font-family: serif;
  }
  .close-btn:hover { background: var(--red); color: #fff; }
 
  /* Gold wax seal on letter */
  .letter-seal {
    position: absolute;
    bottom: -18px;
    left: 50%;
    transform: translateX(-50%);
    width: 52px; height: 52px;
    border-radius: 50%;
    background: radial-gradient(circle at 35% 35%, #F5D060, #D4AF37 50%, #A0780A);
    border: 2px solid #F0D080;
    box-shadow: 0 0 16px rgba(212,175,55,0.7), 0 4px 10px rgba(0,0,0,0.4);
    display: flex; align-items: center; justify-content: center;
    overflow: hidden;
  }
  .letter-seal img {
    width: 44px; height: 44px;
    border-radius: 50%;
    object-fit: cover;
    opacity: 0.9;
  }
 
  /* Floating particles */
  .sparkle {
    position: fixed;
    pointer-events: none;
    z-index: 50;
    font-size: 20px;
    animation: sparkleFly 1.5s ease-out forwards;
  }
  @keyframes sparkleFly {
    0%   { transform: translate(0,0) scale(1) rotate(0deg); opacity: 1; }
    100% { transform: translate(var(--tx), var(--ty)) scale(0) rotate(360deg); opacity: 0; }
  }
 
  /* ===== DOVE DELIVERY ===== */
  .dove-container {
    position: fixed;
    top: 0; left: 0;
    width: 100vw; height: 100vh;
    pointer-events: none;
    z-index: 200;
    opacity: 0;
  }
  .dove-container.flying {
    opacity: 1;
  }
 
  .dove {
    position: absolute;
    width: 80px; height: 60px;
    top: -100px; left: -100px;
    transform-origin: center;
    transition: none;
  }
 
  .dove svg {
    width: 100%; height: 100%;
    filter: drop-shadow(0 4px 8px rgba(0,0,0,0.3));
  }
 
  /* Dove flight path */
  @keyframes doveDelivery {
    0%   { 
      transform: translate(-100px, -100px) rotate(0deg) scale(0.8); 
      opacity: 0; 
    }
    10%  { 
      transform: translate(100px, 100px) rotate(25deg) scale(1); 
      opacity: 1; 
    }
    30%  { 
      transform: translate(40vw, 20vh) rotate(15deg) scale(1.1); 
    }
    50%  { 
      transform: translate(50vw, 40vh) rotate(-5deg) scale(1); 
    }
    70%  { 
      transform: translate(45vw, 35vh) rotate(10deg) scale(1.05); 
    }
    85%  { 
      transform: translate(50vw, 50vh) rotate(0deg) scale(1); 
    }
    100% { 
      transform: translate(60vw, 30vh) rotate(-15deg) scale(0.9); 
      opacity: 0; 
    }
  }
 
  .dove.delivering {
    animation: doveDelivery 3.5s cubic-bezier(.25,.8,.25,1) forwards;
  }
 
  /* Wing flap animation */
  .dove-wings {
    animation: wingFlap 0.3s ease-in-out infinite;
    transform-origin: center;
  }
  
  @keyframes wingFlap {
    0%, 100% { transform: scaleY(1) rotateZ(0deg); }
    50%      { transform: scaleY(0.7) rotateZ(2deg); }
  }
 
  /* Letter carried by dove */
  .dove-letter {
    position: absolute;
    bottom: -20px; right: -15px;
    width: 25px; height: 18px;
    background: linear-gradient(145deg, #FDF8E8, #FAF3D5);
    border: 1px solid #D4AF37;
    border-radius: 2px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.2);
    opacity: 0;
    animation: letterSway 0.6s ease-in-out infinite;
  }
  .dove.delivering .dove-letter { opacity: 1; }
 
  @keyframes letterSway {
    0%, 100% { transform: rotate(-8deg); }
    50%      { transform: rotate(-12deg); }
  }
 
  /* Enhanced sparkle trail */
  .dove-trail {
    position: absolute;
    pointer-events: none;
    font-size: 16px;
    animation: trailSparkle 1.2s ease-out forwards;
  }
  
  @keyframes trailSparkle {
    0%   { opacity: 1; transform: scale(1) rotate(0deg); }
    100% { opacity: 0; transform: scale(0.3) rotate(180deg) translateY(30px); }
  }
 
  /* Responsive */
  @media (max-width: 480px) {
    .scene { width: 320px; height: 220px; }
    .envelope-body { width: 320px; height: 220px; }
    .seal { width: 60px; height: 60px; }
    .seal-circle { width: 60px; height: 60px; }
    .seal-logo { width: 50px; height: 50px; }
    .letter-paper { padding: 32px 24px 28px; }
    .letter-title { font-size: 17px; }
    .letter-body { font-size: 14px; }
    .flap-top { height: 110px; }
    .flap-back { height: 110px; }
  }
</style>
</head>
<body>
 
<canvas id="particles"></canvas>
 
<!-- DOVE DELIVERY ANIMATION -->
<div class="dove-container" id="doveContainer">
  <div class="dove" id="dove">
    <svg viewBox="0 0 80 60" fill="none" xmlns="http://www.w3.org/2000/svg">
      <!-- Dove body -->
      <ellipse cx="40" cy="35" rx="18" ry="12" fill="#F8F8F8" stroke="#E0E0E0" stroke-width="1"/>
      <!-- Dove head -->
      <circle cx="25" cy="25" r="8" fill="#FFFFFF" stroke="#D0D0D0" stroke-width="1"/>
      <!-- Beak -->
      <polygon points="17,25 12,23 12,27" fill="#FFA500"/>
      <!-- Eye -->
      <circle cx="23" cy="23" r="2" fill="#333"/>
      <circle cx="24" cy="22" r="0.8" fill="#FFF"/>
      <!-- Wings (animated) -->
      <g class="dove-wings">
        <ellipse cx="45" cy="30" rx="15" ry="8" fill="#F0F0F0" stroke="#D0D0D0" stroke-width="1"/>
        <ellipse cx="50" cy="28" rx="12" ry="6" fill="#F8F8F8" stroke="#E0E0E0" stroke-width="1"/>
      </g>
      <!-- Tail -->
      <ellipse cx="58" cy="40" rx="8" ry="4" fill="#F0F0F0" stroke="#D0D0D0" stroke-width="1"/>
      <!-- Letter carried -->
      <rect class="dove-letter" x="0" y="0" width="25" height="18" rx="2"/>
    </svg>
  </div>
</div>
 
<div id="cursor">
  <svg viewBox="0 0 36 36" fill="none" xmlns="http://www.w3.org/2000/svg">
    <!-- Simple mouse silhouette -->
    <rect x="9" y="4" width="18" height="28" rx="9" fill="#D4AF37" stroke="#F0D080" stroke-width="1.5"/>
    <line x1="18" y1="4" x2="18" y2="18" stroke="#8B0000" stroke-width="2" stroke-linecap="round"/>
    <circle cx="18" cy="18" r="2.5" fill="#8B0000"/>
  </svg>
</div>
 
<!-- ENVELOPE SCENE -->
<div class="scene" id="envelopeScene">
  <div class="envelope-wrap" id="envelopeWrap">
    <div class="envelope-body" id="envelopeBody">
      <div class="flap-back"></div>
      <div class="flap-left"></div>
      <div class="flap-right"></div>
      <div class="flap-top" id="flapTop"></div>
 
      <!-- WAX SEAL -->
      <div class="seal" id="seal" onclick="openEnvelope()">
        <div class="seal-circle">
          <img class="seal-logo" src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'%3E%3Ccircle cx='50' cy='50' r='48' fill='%23D4AF37'/%3E%3Ctext x='50' y='38' text-anchor='middle' font-size='22' font-family='serif' font-weight='bold' fill='%238B0000'%3EA5%3C/text%3E%3Ctext x='50' y='62' text-anchor='middle' font-size='18' font-family='serif' font-weight='bold' fill='%238B0000'%3EK62%3C/text%3E%3Ccircle cx='50' cy='50' r='44' fill='none' stroke='%23922B21' stroke-width='2'/%3E%3C/svg%3E" alt="Logo A5 K62" id="sealImg">
        </div>
      </div>
    </div>
  </div>
  <div class="hint">✦ Nhấp vào con dấu để mở thư • Nhấp đúp để xoay 360° ✦</div>
</div>
 
<!-- LETTER PANEL -->
<div class="letter-container" id="letterContainer">
  <div class="letter-bg" onclick="closeLetter()"></div>
  <div class="letter-scene">
    <div class="letter-card" id="letterCard">
      <div class="letter-paper" id="letterPaper">
        <button class="close-btn" onclick="closeLetter()" title="Đóng thư">✕</button>
 
        <div class="corner tl"></div>
        <div class="corner tr"></div>
        <div class="corner bl"></div>
        <div class="corner br"></div>
 
        <div class="letter-title">🎓 THIỆP MỜI CHỤP KỶ YẾU 🎓</div>
        <div class="letter-sub">— Lớp A5 · K62 · THPT Chợ Đồn —</div>
        <div class="divider"></div>
 
        <div class="letter-body">
          <p>Thanh xuân rồi sẽ trôi qua, nhưng những khoảnh khắc bên nhau sẽ còn mãi.<br>
          Để lưu giữ những ngày tháng đẹp nhất của tuổi học trò, trân trọng mời bạn tham gia buổi chụp kỷ yếu cùng tôi lớp.</p>
        </div>
 
        <div class="info-row">
          <span class="info-icon">📍</span>
          <span class="info-label">Địa điểm:</span>
          <span class="info-val">THPT Chợ Đồn</span>
        </div>
        <div class="info-row">
          <span class="info-icon">⏰</span>
          <span class="info-label">Thời gian:</span>
          <span class="info-val">01 / 05 / 2026</span>
        </div>
 
        <div class="divider"></div>
 
        <div class="letter-body">
          <p>Sự có mặt của bạn chính là mảnh ghép không thể thiếu để tạo nên một thanh xuân trọn vẹn và đáng nhớ.</p>
          <p>Hãy cùng nhau ghi lại những nụ cười, những kỷ niệm, và cả những điều chưa kịp nói.</p>
        </div>
 
        <div class="letter-footer">💫 Rất mong được gặp bạn!</div>
        <div class="letter-sign">Người gửi: Tống Minh Đạt</div>
 
        <div class="letter-seal">
          <img src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'%3E%3Ccircle cx='50' cy='50' r='48' fill='%23D4AF37'/%3E%3Ctext x='50' y='38' text-anchor='middle' font-size='22' font-family='serif' font-weight='bold' fill='%238B0000'%3EA5%3C/text%3E%3Ctext x='50' y='62' text-anchor='middle' font-size='18' font-family='serif' font-weight='bold' fill='%238B0000'%3EK62%3C/text%3E%3Ccircle cx='50' cy='50' r='44' fill='none' stroke='%23922B21' stroke-width='2'/%3E%3C/svg%3E" alt="Seal">
        </div>
      </div>
    </div>
  </div>
</div>
 
<script>
// ---- Load logo image as base64 ----
(function loadLogo() {
  const img = new Image();
  img.crossOrigin = 'anonymous';
  // We embed the uploaded logo as a canvas-drawn version
  // Since we can't reference the upload directly in HTML, we use the SVG fallback above
  // If running locally with the image, replace the src below:
  // img.src = 'IMG_9716.png';
})();
 
// ---- Custom cursor ----
const cursor = document.getElementById('cursor');
document.addEventListener('mousemove', e => {
  cursor.style.left = e.clientX + 'px';
  cursor.style.top  = e.clientY + 'px';
});
 
// ---- Particle canvas ----
const canvas = document.getElementById('particles');
const ctx = canvas.getContext('2d');
let W, H, particles = [];
 
function resize() {
  W = canvas.width  = window.innerWidth;
  H = canvas.height = window.innerHeight;
}
resize();
window.addEventListener('resize', resize);
 
function randomParticle() {
  return {
    x: Math.random() * W,
    y: Math.random() * H,
    r: Math.random() * 1.8 + 0.3,
    vx: (Math.random() - 0.5) * 0.3,
    vy: -Math.random() * 0.5 - 0.1,
    alpha: Math.random(),
    color: Math.random() > 0.5 ? '#D4AF37' : '#C0392B'
  };
}
for (let i = 0; i < 120; i++) particles.push(randomParticle());
 
function animateParticles() {
  ctx.clearRect(0, 0, W, H);
  for (let p of particles) {
    ctx.beginPath();
    ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
    ctx.fillStyle = p.color;
    ctx.globalAlpha = p.alpha * 0.6;
    ctx.fill();
    p.x += p.vx; p.y += p.vy;
    p.alpha += (Math.random() - 0.5) * 0.02;
    p.alpha = Math.max(0.1, Math.min(0.9, p.alpha));
    if (p.y < -5) { Object.assign(p, randomParticle(), { y: H + 5 }); }
    if (p.x < -5 || p.x > W + 5) { Object.assign(p, randomParticle()); }
  }
  ctx.globalAlpha = 1;
  requestAnimationFrame(animateParticles);
}
animateParticles();
 
// ---- 3D tilt on envelope with 360° spin ----
const scene = document.getElementById('envelopeScene');
const wrap  = document.getElementById('envelopeWrap');
let isSpinning = false;
 
scene.addEventListener('mousemove', e => {
  if (isSpinning || opened) return;
  const rect = scene.getBoundingClientRect();
  const cx = rect.left + rect.width / 2;
  const cy = rect.top  + rect.height / 2;
  const rx =  (e.clientY - cy) / (rect.height / 2) * 18;
  const ry = -(e.clientX - cx) / (rect.width  / 2) * 18;
  wrap.style.transform = `rotateX(${rx}deg) rotateY(${ry}deg)`;
});
 
scene.addEventListener('mouseleave', () => {
  if (isSpinning || opened) return;
  wrap.style.transition = 'transform 0.6s ease';
  wrap.style.transform = 'rotateX(0) rotateY(0)';
  setTimeout(() => wrap.style.transition = '', 600);
});
 
// Double-click for 360° spin
scene.addEventListener('dblclick', () => {
  if (opened || isSpinning) return;
  isSpinning = true;
  wrap.classList.add('spin360');
  
  // Spawn sparkle trail during spin
  const rect = scene.getBoundingClientRect();
  spawnSpinTrail(rect);
  
  setTimeout(() => {
    wrap.classList.remove('spin360');
    isSpinning = false;
  }, 2000);
});
 
// ---- Dove delivery animation ----
function startDoveDelivery() {
  const doveContainer = document.getElementById('doveContainer');
  const dove = document.getElementById('dove');
  
  doveContainer.classList.add('flying');
  dove.classList.add('delivering');
  
  // Create sparkle trail behind dove
  const trailInterval = setInterval(() => {
    if (!dove.classList.contains('delivering')) {
      clearInterval(trailInterval);
      return;
    }
    createDoveTrail();
  }, 150);
  
  // Remove dove after animation
  setTimeout(() => {
    doveContainer.classList.remove('flying');
    dove.classList.remove('delivering');
  }, 3500);
}
 
function createDoveTrail() {
  const dove = document.getElementById('dove');
  const rect = dove.getBoundingClientRect();
  const sparkles = ['✨','⭐','💛','🕊️','💌'];
  
  for (let i = 0; i < 3; i++) {
    const trail = document.createElement('div');
    trail.className = 'dove-trail';
    trail.textContent = sparkles[Math.floor(Math.random() * sparkles.length)];
    trail.style.left = (rect.left + rect.width/2 + (Math.random()-0.5)*30) + 'px';
    trail.style.top = (rect.top + rect.height/2 + (Math.random()-0.5)*20) + 'px';
    trail.style.animationDelay = (i * 0.1) + 's';
    document.body.appendChild(trail);
    setTimeout(() => trail.remove(), 1200);
  }
}
 
function spawnSpinTrail(rect) {
  const emojis = ['✨','⭐','💫','🌟','✦','💛'];
  for (let i = 0; i < 20; i++) {
    setTimeout(() => {
      const el = document.createElement('div');
      el.className = 'sparkle';
      el.textContent = emojis[Math.floor(Math.random() * emojis.length)];
      const cx = rect.left + rect.width / 2;
      const cy = rect.top  + rect.height / 2;
      const angle = (i / 20) * Math.PI * 2;
      const radius = 120 + Math.random() * 60;
      el.style.left = (cx + Math.cos(angle) * (radius * 0.3)) + 'px';
      el.style.top  = (cy + Math.sin(angle) * (radius * 0.3)) + 'px';
      el.style.setProperty('--tx', Math.cos(angle) * radius + 'px');
      el.style.setProperty('--ty', Math.sin(angle) * radius + 'px');
      document.body.appendChild(el);
      setTimeout(() => el.remove(), 1500);
    }, i * 80);
  }
}
 
// ---- Open envelope with dove delivery ----
let opened = false;
function openEnvelope() {
  if (opened) return;
  opened = true;
 
  const seal = document.getElementById('seal');
  seal.classList.add('broken');
  spawnSparkles(seal.getBoundingClientRect());
 
  // Start dove delivery animation
  setTimeout(() => {
    startDoveDelivery();
  }, 200);
 
  setTimeout(() => {
    document.getElementById('flapTop').classList.add('open');
  }, 300);
 
  setTimeout(() => {
    document.getElementById('letterContainer').classList.add('visible');
    wrap.style.transform = 'rotateX(0) rotateY(0)';
  }, 900);
}
 
// ---- Close letter ----
function closeLetter() {
  document.getElementById('letterContainer').classList.remove('visible');
}
 
// ---- Letter 3D tilt ----
const paper = document.getElementById('letterPaper');
document.addEventListener('mousemove', e => {
  if (!document.getElementById('letterContainer').classList.contains('visible')) return;
  const rect = paper.getBoundingClientRect();
  if (!rect.width) return;
  const cx = rect.left + rect.width / 2;
  const cy = rect.top  + rect.height / 2;
  const rx =  (e.clientY - cy) / (rect.height / 2) * 6;
  const ry = -(e.clientX - cx) / (rect.width  / 2) * 6;
  paper.style.transform = `rotateX(${rx}deg) rotateY(${ry}deg)`;
});
 
// ---- Sparkle burst ----
function spawnSparkles(rect) {
  const emojis = ['✨','⭐','💛','❤️','🌟','✦'];
  for (let i = 0; i < 14; i++) {
    const el = document.createElement('div');
    el.className = 'sparkle';
    el.textContent = emojis[Math.floor(Math.random() * emojis.length)];
    const cx = rect.left + rect.width / 2;
    const cy = rect.top  + rect.height / 2;
    el.style.left = cx + 'px';
    el.style.top  = cy + 'px';
    const angle = (i / 14) * Math.PI * 2;
    const dist  = 60 + Math.random() * 80;
    el.style.setProperty('--tx', Math.cos(angle) * dist + 'px');
    el.style.setProperty('--ty', Math.sin(angle) * dist + 'px');
    el.style.animationDelay = Math.random() * 0.2 + 's';
    document.body.appendChild(el);
    setTimeout(() => el.remove(), 1800);
  }
}
 
// ---- Touch support ----
scene.addEventListener('touchstart', openEnvelope, { once: true });
</script>
</body>
</html>
