<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Willian Ushiña — Portfolio README</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Space+Mono:ital,wght@0,400;0,700;1,400&family=Rajdhani:wght@300;400;600;700&display=swap" rel="stylesheet" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<style>
  :root {
    --neon-cyan: #00f5ff;
    --neon-blue: #0080ff;
    --neon-violet: #7c3aed;
    --neon-green: #00ff88;
    --neon-orange: #ff6b35;
    --dark-bg: #020408;
    --dark-surface: #060d1a;
    --dark-card: #0a1628;
    --dark-border: #0d2040;
    --text-primary: #e8f4fd;
    --text-secondary: #7aa8cc;
    --text-muted: #3a6080;
    --glow-cyan: 0 0 20px #00f5ff60, 0 0 60px #00f5ff30;
    --glow-blue: 0 0 20px #0080ff60, 0 0 60px #0080ff30;
    --glow-green: 0 0 20px #00ff8860, 0 0 60px #00ff8830;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  html { scroll-behavior: smooth; }

  body {
    background: var(--dark-bg);
    color: var(--text-primary);
    font-family: 'Rajdhani', sans-serif;
    font-size: 16px;
    overflow-x: hidden;
    line-height: 1.6;
  }

  /* ── CANVAS 3D BACKGROUND ── */
  #canvas3d {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    z-index: 0;
    pointer-events: none;
  }

  .wrapper {
    position: relative;
    z-index: 1;
    max-width: 960px;
    margin: 0 auto;
    padding: 0 24px 100px;
  }

  /* ── SCANLINES OVERLAY ── */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background: repeating-linear-gradient(
      0deg,
      transparent,
      transparent 2px,
      rgba(0,245,255,0.015) 2px,
      rgba(0,245,255,0.015) 4px
    );
    pointer-events: none;
    z-index: 2;
    animation: scanMove 8s linear infinite;
  }

  @keyframes scanMove {
    0% { background-position: 0 0; }
    100% { background-position: 0 400px; }
  }

  /* ── GLITCH NOISE OVERLAY ── */
  body::after {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.03'/%3E%3C/svg%3E");
    opacity: 0.4;
    pointer-events: none;
    z-index: 2;
  }

  /* ── HERO SECTION ── */
  .hero {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    padding: 60px 0 40px;
    position: relative;
  }

  .hero-badge {
    display: inline-block;
    padding: 6px 18px;
    border: 1px solid var(--neon-cyan);
    border-radius: 2px;
    font-family: 'Space Mono', monospace;
    font-size: 11px;
    color: var(--neon-cyan);
    letter-spacing: 3px;
    text-transform: uppercase;
    margin-bottom: 30px;
    animation: fadeSlideDown 1s ease 0.2s both;
    box-shadow: var(--glow-cyan);
    position: relative;
    overflow: hidden;
  }

  .hero-badge::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(90deg, transparent, rgba(0,245,255,0.1), transparent);
    animation: shimmer 2.5s ease-in-out infinite;
  }

  @keyframes shimmer {
    0% { transform: translateX(-100%); }
    100% { transform: translateX(100%); }
  }

  .hero-name {
    font-family: 'Orbitron', monospace;
    font-size: clamp(2.5rem, 8vw, 5.5rem);
    font-weight: 900;
    letter-spacing: -2px;
    line-height: 1;
    animation: fadeSlideDown 1s ease 0.4s both;
    margin-bottom: 8px;
    position: relative;
  }

  .hero-name span {
    display: block;
    background: linear-gradient(135deg, var(--neon-cyan) 0%, var(--neon-blue) 50%, var(--neon-violet) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    filter: drop-shadow(0 0 30px rgba(0,245,255,0.5));
  }

  .hero-name .surname {
    background: linear-gradient(135deg, #ffffff 0%, var(--neon-cyan) 60%, var(--neon-green) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  /* ── TYPING TERMINAL ── */
  .terminal-line {
    font-family: 'Space Mono', monospace;
    font-size: clamp(0.75rem, 2vw, 1rem);
    color: var(--neon-green);
    margin-top: 24px;
    animation: fadeSlideDown 1s ease 0.6s both;
    height: 1.5em;
  }

  .terminal-cursor {
    display: inline-block;
    width: 2px;
    height: 1em;
    background: var(--neon-green);
    margin-left: 2px;
    animation: blink 1s step-end infinite;
    vertical-align: middle;
  }

  @keyframes blink { 0%, 100% { opacity: 1; } 50% { opacity: 0; } }

  /* ── LOCATION PILL ── */
  .location-pill {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    margin-top: 20px;
    padding: 8px 20px;
    background: rgba(0,245,255,0.05);
    border: 1px solid rgba(0,245,255,0.2);
    border-radius: 100px;
    font-size: 13px;
    color: var(--text-secondary);
    animation: fadeSlideDown 1s ease 0.8s both;
  }

  .location-pill .dot {
    width: 8px; height: 8px;
    background: var(--neon-green);
    border-radius: 50%;
    animation: pulse 2s ease infinite;
    box-shadow: 0 0 8px var(--neon-green);
  }

  @keyframes pulse { 0%, 100% { transform: scale(1); opacity: 1; } 50% { transform: scale(1.4); opacity: 0.6; } }

  /* ── SECTION STYLES ── */
  section { margin: 80px 0; animation: fadeSlideUp 0.8s ease both; }

  .section-header {
    display: flex;
    align-items: center;
    gap: 16px;
    margin-bottom: 36px;
  }

  .section-header::before {
    content: '';
    display: block;
    width: 3px;
    height: 32px;
    background: linear-gradient(180deg, var(--neon-cyan), var(--neon-violet));
    border-radius: 2px;
    box-shadow: var(--glow-cyan);
  }

  .section-label {
    font-family: 'Space Mono', monospace;
    font-size: 10px;
    color: var(--neon-cyan);
    letter-spacing: 4px;
    text-transform: uppercase;
    opacity: 0.7;
    display: block;
    margin-bottom: 2px;
  }

  .section-title {
    font-family: 'Orbitron', monospace;
    font-size: clamp(1.3rem, 3vw, 1.8rem);
    font-weight: 700;
    color: var(--text-primary);
  }

  /* ── ABOUT GRID ── */
  .about-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
  }

  .about-paragraph {
    grid-column: 1 / -1;
    background: linear-gradient(135deg, rgba(0,245,255,0.03), rgba(124,58,237,0.03));
    border: 1px solid var(--dark-border);
    border-left: 3px solid var(--neon-cyan);
    padding: 24px 28px;
    border-radius: 4px;
    font-size: 15px;
    color: var(--text-secondary);
    line-height: 1.9;
    position: relative;
    overflow: hidden;
    transition: border-color 0.3s, box-shadow 0.3s;
  }

  .about-paragraph:hover {
    border-color: var(--neon-cyan);
    box-shadow: var(--glow-cyan);
  }

  .about-paragraph strong { color: var(--neon-cyan); font-weight: 600; }

  .info-card {
    background: var(--dark-card);
    border: 1px solid var(--dark-border);
    border-radius: 6px;
    padding: 20px 22px;
    position: relative;
    overflow: hidden;
    transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
    cursor: default;
  }

  .info-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, var(--neon-cyan), var(--neon-violet));
    transform: scaleX(0);
    transform-origin: left;
    transition: transform 0.4s ease;
  }

  .info-card:hover::before { transform: scaleX(1); }

  .info-card:hover {
    transform: translateY(-4px);
    border-color: rgba(0,245,255,0.3);
    box-shadow: 0 20px 60px rgba(0,0,0,0.4), var(--glow-blue);
  }

  .info-card-icon {
    font-size: 22px;
    margin-bottom: 10px;
    display: block;
  }

  .info-card-title {
    font-family: 'Space Mono', monospace;
    font-size: 10px;
    color: var(--neon-cyan);
    letter-spacing: 2px;
    text-transform: uppercase;
    margin-bottom: 6px;
  }

  .info-card-text {
    font-size: 13px;
    color: var(--text-secondary);
    line-height: 1.6;
  }

  .info-card-text strong { color: var(--text-primary); }

  /* ── TECH GRID ── */
  .tech-category { margin-bottom: 32px; }

  .tech-cat-title {
    font-family: 'Space Mono', monospace;
    font-size: 10px;
    color: var(--neon-cyan);
    letter-spacing: 3px;
    text-transform: uppercase;
    margin-bottom: 14px;
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .tech-cat-title::after {
    content: '';
    flex: 1;
    height: 1px;
    background: linear-gradient(90deg, var(--dark-border), transparent);
  }

  .badge-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
  }

  .tech-badge {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 8px 16px;
    background: var(--dark-card);
    border: 1px solid var(--dark-border);
    border-radius: 4px;
    font-family: 'Space Mono', monospace;
    font-size: 12px;
    color: var(--text-secondary);
    text-decoration: none;
    position: relative;
    overflow: hidden;
    transition: all 0.3s cubic-bezier(0.23, 1, 0.32, 1);
    cursor: default;
  }

  .tech-badge::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, transparent, rgba(0,245,255,0.08), transparent);
    opacity: 0;
    transition: opacity 0.3s;
  }

  .tech-badge:hover::before { opacity: 1; }

  .tech-badge:hover {
    transform: translateY(-3px) scale(1.02);
    border-color: var(--neon-cyan);
    color: var(--neon-cyan);
    box-shadow: var(--glow-cyan);
  }

  .badge-dot {
    width: 6px; height: 6px;
    border-radius: 50%;
    flex-shrink: 0;
  }

  /* ── STATS SECTION ── */
  .stats-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 16px;
    margin-bottom: 28px;
  }

  .stat-card {
    background: var(--dark-card);
    border: 1px solid var(--dark-border);
    border-radius: 6px;
    padding: 24px;
    text-align: center;
    position: relative;
    overflow: hidden;
    transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
  }

  .stat-card::after {
    content: '';
    position: absolute;
    bottom: 0; left: 0; right: 0;
    height: 2px;
    background: linear-gradient(90deg, var(--neon-cyan), var(--neon-green));
    transform: scaleX(0);
    transition: transform 0.4s ease;
  }

  .stat-card:hover::after { transform: scaleX(1); }

  .stat-card:hover {
    transform: translateY(-6px);
    box-shadow: 0 30px 80px rgba(0,0,0,0.5), var(--glow-cyan);
    border-color: rgba(0,245,255,0.25);
  }

  .stat-number {
    font-family: 'Orbitron', monospace;
    font-size: 2.4rem;
    font-weight: 900;
    background: linear-gradient(135deg, var(--neon-cyan), var(--neon-blue));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    display: block;
    animation: countUp 2s ease;
  }

  .stat-label {
    font-family: 'Space Mono', monospace;
    font-size: 9px;
    color: var(--text-muted);
    letter-spacing: 2px;
    text-transform: uppercase;
    margin-top: 6px;
  }

  /* ── GITHUB CARDS ── */
  .gh-cards {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
    margin-bottom: 16px;
  }

  .gh-card {
    border-radius: 6px;
    overflow: hidden;
    position: relative;
    background: var(--dark-card);
    border: 1px solid var(--dark-border);
    transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
  }

  .gh-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 20px 60px rgba(0,0,0,0.4);
    border-color: rgba(0,245,255,0.2);
  }

  .gh-card img {
    width: 100%;
    height: auto;
    display: block;
    filter: brightness(0.95) saturate(1.1);
  }

  .gh-card-full {
    border-radius: 6px;
    overflow: hidden;
    border: 1px solid var(--dark-border);
    background: var(--dark-card);
    transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
  }

  .gh-card-full:hover {
    transform: translateY(-4px);
    box-shadow: 0 20px 60px rgba(0,0,0,0.4);
    border-color: rgba(0,245,255,0.2);
  }

  .gh-card-full img { width: 100%; height: auto; display: block; }

  /* ── CONNECT SECTION ── */
  .connect-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
    gap: 14px;
  }

  .connect-card {
    display: flex;
    align-items: center;
    gap: 16px;
    padding: 18px 22px;
    background: var(--dark-card);
    border: 1px solid var(--dark-border);
    border-radius: 6px;
    text-decoration: none;
    color: var(--text-secondary);
    transition: all 0.4s cubic-bezier(0.23, 1, 0.32, 1);
    position: relative;
    overflow: hidden;
  }

  .connect-card::before {
    content: '';
    position: absolute;
    left: 0; top: 0; bottom: 0;
    width: 3px;
    background: linear-gradient(180deg, var(--neon-cyan), var(--neon-violet));
    transform: scaleY(0);
    transition: transform 0.3s ease;
  }

  .connect-card:hover::before { transform: scaleY(1); }

  .connect-card:hover {
    transform: translateX(6px);
    border-color: rgba(0,245,255,0.3);
    color: var(--text-primary);
    box-shadow: var(--glow-blue);
  }

  .connect-icon {
    font-size: 24px;
    flex-shrink: 0;
    filter: grayscale(0.3);
    transition: filter 0.3s;
  }

  .connect-card:hover .connect-icon { filter: none; }

  .connect-label {
    font-family: 'Space Mono', monospace;
    font-size: 9px;
    color: var(--neon-cyan);
    letter-spacing: 2px;
    text-transform: uppercase;
    display: block;
    margin-bottom: 2px;
  }

  .connect-value {
    font-size: 13px;
    font-weight: 600;
  }

  /* ── FOOTER ── */
  footer {
    text-align: center;
    padding: 60px 0 0;
    position: relative;
  }

  .footer-line {
    height: 1px;
    background: linear-gradient(90deg, transparent, var(--neon-cyan), transparent);
    margin-bottom: 30px;
    box-shadow: 0 0 10px rgba(0,245,255,0.3);
  }

  .footer-text {
    font-family: 'Space Mono', monospace;
    font-size: 11px;
    color: var(--text-muted);
    letter-spacing: 2px;
  }

  .footer-text span { color: var(--neon-cyan); }

  /* ── SCROLL INDICATOR ── */
  .scroll-indicator {
    position: absolute;
    bottom: 30px;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 8px;
    animation: fadeSlideDown 1s ease 1.5s both;
  }

  .scroll-mouse {
    width: 22px; height: 36px;
    border: 2px solid rgba(0,245,255,0.4);
    border-radius: 11px;
    position: relative;
    display: flex;
    justify-content: center;
    padding-top: 6px;
  }

  .scroll-wheel {
    width: 4px; height: 8px;
    background: var(--neon-cyan);
    border-radius: 2px;
    animation: scrollWheel 1.8s ease infinite;
    box-shadow: 0 0 6px var(--neon-cyan);
  }

  @keyframes scrollWheel {
    0% { transform: translateY(0); opacity: 1; }
    100% { transform: translateY(10px); opacity: 0; }
  }

  .scroll-text {
    font-family: 'Space Mono', monospace;
    font-size: 9px;
    color: var(--text-muted);
    letter-spacing: 2px;
    text-transform: uppercase;
  }

  /* ── 3D CUBE SPINNER ── */
  .cube-container {
    width: 120px; height: 120px;
    perspective: 600px;
    margin: 0 auto 30px;
    animation: fadeSlideDown 1s ease 0.1s both;
  }

  .cube {
    width: 100%; height: 100%;
    position: relative;
    transform-style: preserve-3d;
    animation: rotateCube 8s linear infinite;
  }

  @keyframes rotateCube {
    0% { transform: rotateX(0deg) rotateY(0deg) rotateZ(0deg); }
    33% { transform: rotateX(120deg) rotateY(120deg) rotateZ(40deg); }
    66% { transform: rotateX(240deg) rotateY(240deg) rotateZ(80deg); }
    100% { transform: rotateX(360deg) rotateY(360deg) rotateZ(120deg); }
  }

  .cube-face {
    position: absolute;
    width: 120px; height: 120px;
    border: 1px solid rgba(0,245,255,0.4);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 2rem;
    background: rgba(0,245,255,0.04);
    backdrop-filter: blur(2px);
  }

  .cube-face.front  { transform: translateZ(60px); }
  .cube-face.back   { transform: translateZ(-60px) rotateY(180deg); }
  .cube-face.left   { transform: translateX(-60px) rotateY(-90deg); }
  .cube-face.right  { transform: translateX(60px) rotateY(90deg); }
  .cube-face.top    { transform: translateY(-60px) rotateX(90deg); }
  .cube-face.bottom { transform: translateY(60px) rotateX(-90deg); }

  /* ── ANIMATIONS ── */
  @keyframes fadeSlideDown {
    from { opacity: 0; transform: translateY(-20px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  @keyframes fadeSlideUp {
    from { opacity: 0; transform: translateY(30px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  /* ── INTERSECTION OBSERVER targets ── */
  .reveal { opacity: 0; transform: translateY(40px); transition: all 0.7s cubic-bezier(0.23, 1, 0.32, 1); }
  .reveal.visible { opacity: 1; transform: translateY(0); }
  .reveal.visible:nth-child(2) { transition-delay: 0.1s; }
  .reveal.visible:nth-child(3) { transition-delay: 0.2s; }
  .reveal.visible:nth-child(4) { transition-delay: 0.3s; }

  /* ── RESPONSIVE ── */
  @media (max-width: 640px) {
    .about-grid, .stats-grid, .gh-cards { grid-template-columns: 1fr; }
    .about-paragraph { grid-column: 1; }
    .hero-name { letter-spacing: -1px; }
  }

  /* ── GLITCH EFFECT on hover ── */
  .hero-name:hover span {
    animation: glitch 0.4s steps(2, jump-none) 1;
  }

  @keyframes glitch {
    0%   { text-shadow: 2px 0 red, -2px 0 cyan; }
    25%  { text-shadow: -2px 0 red,  2px 0 cyan; }
    50%  { text-shadow: 2px 0 red, -2px 0 cyan; }
    75%  { text-shadow: -2px 0 red,  2px 0 cyan; }
    100% { text-shadow: none; }
  }

  /* ── PROGRESS BARS ── */
  .skill-bars { display: flex; flex-direction: column; gap: 12px; }

  .skill-bar-item { display: grid; grid-template-columns: 140px 1fr 40px; gap: 12px; align-items: center; }

  .skill-bar-label {
    font-family: 'Space Mono', monospace;
    font-size: 11px;
    color: var(--text-secondary);
    text-align: right;
  }

  .skill-bar-track {
    height: 4px;
    background: var(--dark-border);
    border-radius: 2px;
    overflow: hidden;
    position: relative;
  }

  .skill-bar-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--neon-cyan), var(--neon-blue));
    border-radius: 2px;
    width: 0;
    box-shadow: var(--glow-cyan);
    transition: width 1.5s cubic-bezier(0.23, 1, 0.32, 1);
  }

  .skill-bar-pct {
    font-family: 'Orbitron', monospace;
    font-size: 10px;
    color: var(--neon-cyan);
    text-align: right;
  }
</style>
</head>
<body>

<!-- THREE.JS CANVAS -->
<canvas id="canvas3d"></canvas>

<div class="wrapper">

  <!-- ════════════════════════════════════════
       HERO
  ═════════════════════════════════════════ -->
  <section class="hero">

    <div class="cube-container">
      <div class="cube">
        <div class="cube-face front">💻</div>
        <div class="cube-face back">🧠</div>
        <div class="cube-face left">⚡</div>
        <div class="cube-face right">🔬</div>
        <div class="cube-face top">🚀</div>
        <div class="cube-face bottom">🛢️</div>
      </div>
    </div>

    <div class="hero-badge">// Ingeniero en Computación · UPEC</div>

    <h1 class="hero-name">
      <span>WILLIAN</span>
      <span class="surname">USHIÑA</span>
    </h1>

    <div class="terminal-line">
      <span id="typed"></span><span class="terminal-cursor"></span>
    </div>

    <div class="location-pill">
      <span class="dot"></span>
      Tulcán, Ecuador — UTC-5
    </div>

    <div class="scroll-indicator">
      <div class="scroll-mouse"><div class="scroll-wheel"></div></div>
      <span class="scroll-text">scroll</span>
    </div>
  </section>

  <!-- ════════════════════════════════════════
       SOBRE MÍ
  ═════════════════════════════════════════ -->
  <section class="reveal">
    <div class="section-header">
      <div>
        <span class="section-label">01 // about</span>
        <h2 class="section-title">Sobre Mí</h2>
      </div>
    </div>

    <div class="about-grid">
      <div class="about-paragraph">
        Soy un apasionado de la <strong>Ingeniería en Computación</strong> enfocado en desarrollar software bajo estándares globales de calidad. Mi enfoque técnico se centra en construir <strong>arquitecturas escalables</strong>, aplicar <strong>Clean Code</strong>, garantizar la seguridad de los sistemas y optimizar <strong>bases de datos empresariales</strong>.<br /><br />
        Participo activamente en investigación académica sobre el impacto de la <strong>Inteligencia Artificial</strong> en la educación técnica y colidero iniciativas de formación tecnológica para las nuevas generaciones.
      </div>

      <div class="info-card reveal">
        <span class="info-card-icon">🔭</span>
        <div class="info-card-title">Proyectos Actuales</div>
        <div class="info-card-text"><strong>Iluminatura</strong> — Catálogo Interactivo<br /><strong>UshiDev</strong> — Plataforma de Educación Digital</div>
      </div>

      <div class="info-card reveal">
        <span class="info-card-icon">📝</span>
        <div class="info-card-title">Investigación</div>
        <div class="info-card-text">Revisiones sistemáticas bajo protocolo <strong>PRISMA 2020</strong> — LLMs & pensamiento algorítmico</div>
      </div>

      <div class="info-card reveal">
        <span class="info-card-icon">🛠️</span>
        <div class="info-card-title">Docs-as-Code</div>
        <div class="info-card-text">Diagramas de arquitectura técnica con <strong>Mermaid.js</strong> y <strong>PlantUML</strong></div>
      </div>

      <div class="info-card reveal">
        <span class="info-card-icon">⚡</span>
        <div class="info-card-title">Dato Curioso</div>
        <div class="info-card-text">Configuro entornos virtualizados complejos en <strong>Ubuntu/VirtualBox</strong> solo por diversión los fines de semana</div>
      </div>
    </div>
  </section>

  <!-- ════════════════════════════════════════
       TECNOLOGÍAS
  ═════════════════════════════════════════ -->
  <section class="reveal">
    <div class="section-header">
      <div>
        <span class="section-label">02 // stack</span>
        <h2 class="section-title">Tecnologías & Herramientas</h2>
      </div>
    </div>

    <div class="tech-category">
      <div class="tech-cat-title">Frontend & Mobile</div>
      <div class="badge-grid">
        <div class="tech-badge"><span class="badge-dot" style="background:#F7DF1E"></span>JavaScript</div>
        <div class="tech-badge"><span class="badge-dot" style="background:#61DAFB"></span>React</div>
        <div class="tech-badge"><span class="badge-dot" style="background:#E34F26"></span>HTML5</div>
        <div class="tech-badge"><span class="badge-dot" style="background:#1572B6"></span>CSS3</div>
      </div>
    </div>

    <div class="tech-category">
      <div class="tech-cat-title">Bases de Datos</div>
      <div class="badge-grid">
        <div class="tech-badge"><span class="badge-dot" style="background:#F80000"></span>Oracle PL/SQL</div>
        <div class="tech-badge"><span class="badge-dot" style="background:#00758F"></span>SQL / MySQL</div>
        <div class="tech-badge"><span class="badge-dot" style="background:#005C84"></span>Toad for Oracle</div>
      </div>
    </div>

    <div class="tech-category">
      <div class="tech-cat-title">Infraestructura & DevOps</div>
      <div class="badge-grid">
        <div class="tech-badge"><span class="badge-dot" style="background:#FC6D26"></span>GitLab</div>
        <div class="tech-badge"><span class="badge-dot" style="background:#F05032"></span>Git</div>
        <div class="tech-badge"><span class="badge-dot" style="background:#E95420"></span>Ubuntu</div>
        <div class="tech-badge"><span class="badge-dot" style="background:#183A61"></span>VirtualBox</div>
        <div class="tech-badge"><span class="badge-dot" style="background:#F24E1E"></span>Figma</div>
        <div class="tech-badge"><span class="badge-dot" style="background:#00f5ff"></span>Mermaid.js</div>
        <div class="tech-badge"><span class="badge-dot" style="background:#7c3aed"></span>PlantUML</div>
      </div>
    </div>

    <!-- SKILL PROFICIENCY BARS -->
    <div style="margin-top:36px">
      <div class="tech-cat-title">Nivel de Dominio</div>
      <div class="skill-bars" id="skillBars">
        <div class="skill-bar-item">
          <span class="skill-bar-label">Oracle PL/SQL</span>
          <div class="skill-bar-track"><div class="skill-bar-fill" data-pct="92"></div></div>
          <span class="skill-bar-pct">92%</span>
        </div>
        <div class="skill-bar-item">
          <span class="skill-bar-label">JavaScript / React</span>
          <div class="skill-bar-track"><div class="skill-bar-fill" data-pct="85"></div></div>
          <span class="skill-bar-pct">85%</span>
        </div>
        <div class="skill-bar-item">
          <span class="skill-bar-label">Git / GitLab</span>
          <div class="skill-bar-track"><div class="skill-bar-fill" data-pct="90"></div></div>
          <span class="skill-bar-pct">90%</span>
        </div>
        <div class="skill-bar-item">
          <span class="skill-bar-label">Ubuntu / Linux</span>
          <div class="skill-bar-track"><div class="skill-bar-fill" data-pct="88"></div></div>
          <span class="skill-bar-pct">88%</span>
        </div>
        <div class="skill-bar-item">
          <span class="skill-bar-label">Figma / UI Design</span>
          <div class="skill-bar-track"><div class="skill-bar-fill" data-pct="78"></div></div>
          <span class="skill-bar-pct">78%</span>
        </div>
      </div>
    </div>
  </section>

  <!-- ════════════════════════════════════════
       ESTADÍSTICAS
  ═════════════════════════════════════════ -->
  <section class="reveal">
    <div class="section-header">
      <div>
        <span class="section-label">03 // metrics</span>
        <h2 class="section-title">Estadísticas GitHub</h2>
      </div>
    </div>

    <div class="stats-grid">
      <div class="stat-card reveal">
        <span class="stat-number" data-target="3">0</span>
        <div class="stat-label">Proyectos Activos</div>
      </div>
      <div class="stat-card reveal">
        <span class="stat-number" data-target="5">0</span>
        <div class="stat-label">Años de Experiencia</div>
      </div>
      <div class="stat-card reveal">
        <span class="stat-number" data-target="12">0</span>
        <div class="stat-label">Repos Públicos</div>
      </div>
    </div>

    <div class="gh-cards">
      <div class="gh-card">
        <img src="https://github-readme-stats.vercel.app/api?username=willian-ushina&show_icons=true&theme=tokyonight&count_private=true&bg_color=060d1a&border_color=0d2040&title_color=00f5ff&icon_color=0080ff&text_color=7aa8cc" alt="GitHub Stats" loading="lazy" />
      </div>
      <div class="gh-card">
        <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=willian-ushina&layout=compact&theme=tokyonight&bg_color=060d1a&border_color=0d2040&title_color=00f5ff&text_color=7aa8cc" alt="Top Languages" loading="lazy" />
      </div>
    </div>

    <div class="gh-card-full">
      <img src="https://github-readme-streak-stats.herokuapp.com/?user=willian-ushina&theme=tokyonight&background=060d1a&border=0d2040&ring=00f5ff&fire=0080ff&currStreakLabel=00f5ff" alt="GitHub Streak" loading="lazy" />
    </div>
  </section>

  <!-- ════════════════════════════════════════
       CONECTAR
  ═════════════════════════════════════════ -->
  <section class="reveal">
    <div class="section-header">
      <div>
        <span class="section-label">04 // contact</span>
        <h2 class="section-title">Conéctate Conmigo</h2>
      </div>
    </div>

    <div class="connect-grid">
      <a href="mailto:willian.ushinia@upec.edu.ec" class="connect-card">
        <span class="connect-icon">📧</span>
        <div>
          <span class="connect-label">Email Institucional</span>
          <div class="connect-value">willian.ushinia@upec.edu.ec</div>
        </div>
      </a>

      <div class="connect-card">
        <span class="connect-icon">🏫</span>
        <div>
          <span class="connect-label">Comunidad</span>
          <div class="connect-value">DevClub UPEC</div>
        </div>
      </div>

      <div class="connect-card">
        <span class="connect-icon">📍</span>
        <div>
          <span class="connect-label">Ubicación</span>
          <div class="connect-value">Tulcán, Ecuador</div>
        </div>
      </div>

      <div class="connect-card">
        <span class="connect-icon">🎓</span>
        <div>
          <span class="connect-label">Mentoría</span>
          <div class="connect-value">Mentor Académico · UPEC</div>
        </div>
      </div>
    </div>
  </section>

  <!-- FOOTER -->
  <footer>
    <div class="footer-line"></div>
    <p class="footer-text">
      CRAFTED WITH <span>❤</span> BY WILLIAN USHIÑA · TULCÁN, ECUADOR · <span>2025</span>
    </p>
    <p class="footer-text" style="margin-top:8px; opacity:0.5">
      // Full-Stack · Database · Research · Education
    </p>
  </footer>

</div><!-- /wrapper -->

<!-- ════════════════════════════════════════
     SCRIPTS
═════════════════════════════════════════ -->
<script>
// ── THREE.JS PARTICLE FIELD ──
(function() {
  const canvas = document.getElementById('canvas3d');
  const renderer = new THREE.WebGLRenderer({ canvas, antialias: true, alpha: true });
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  renderer.setSize(window.innerWidth, window.innerHeight);

  const scene = new THREE.Scene();
  const camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.1, 1000);
  camera.position.z = 60;

  // ── PARTICLE GEOMETRY ──
  const particleCount = 800;
  const positions = new Float32Array(particleCount * 3);
  const colors = new Float32Array(particleCount * 3);

  const colorA = new THREE.Color('#00f5ff');
  const colorB = new THREE.Color('#0080ff');
  const colorC = new THREE.Color('#7c3aed');

  for (let i = 0; i < particleCount; i++) {
    const theta = Math.random() * Math.PI * 2;
    const phi   = Math.acos(2 * Math.random() - 1);
    const r     = 20 + Math.random() * 60;

    positions[i * 3]     = r * Math.sin(phi) * Math.cos(theta);
    positions[i * 3 + 1] = r * Math.sin(phi) * Math.sin(theta);
    positions[i * 3 + 2] = r * Math.cos(phi);

    const t = Math.random();
    const c = t < 0.5
      ? colorA.clone().lerp(colorB, t * 2)
      : colorB.clone().lerp(colorC, (t - 0.5) * 2);

    colors[i * 3]     = c.r;
    colors[i * 3 + 1] = c.g;
    colors[i * 3 + 2] = c.b;
  }

  const geo = new THREE.BufferGeometry();
  geo.setAttribute('position', new THREE.BufferAttribute(positions, 3));
  geo.setAttribute('color', new THREE.BufferAttribute(colors, 3));

  const mat = new THREE.PointsMaterial({
    size: 0.3,
    vertexColors: true,
    transparent: true,
    opacity: 0.7,
    sizeAttenuation: true,
  });

  const particles = new THREE.Points(geo, mat);
  scene.add(particles);

  // ── WIRE SPHERE ──
  const wireSphere = new THREE.Mesh(
    new THREE.IcosahedronGeometry(18, 2),
    new THREE.MeshBasicMaterial({
      color: 0x00f5ff,
      wireframe: true,
      transparent: true,
      opacity: 0.06,
    })
  );
  scene.add(wireSphere);

  // ── FLOATING RINGS ──
  for (let i = 0; i < 3; i++) {
    const ring = new THREE.Mesh(
      new THREE.TorusGeometry(14 + i * 8, 0.05, 8, 64),
      new THREE.MeshBasicMaterial({
        color: i === 0 ? 0x00f5ff : i === 1 ? 0x0080ff : 0x7c3aed,
        transparent: true,
        opacity: 0.15 - i * 0.04,
      })
    );
    ring.rotation.x = Math.PI / (3 + i);
    ring.rotation.y = i * 0.5;
    ring.userData = { rotX: 0.0003 + i * 0.0001, rotY: 0.0006 + i * 0.0002 };
    scene.add(ring);
  }

  // ── MOUSE INTERACTION ──
  let mouseX = 0, mouseY = 0;
  window.addEventListener('mousemove', e => {
    mouseX = (e.clientX / window.innerWidth  - 0.5) * 2;
    mouseY = (e.clientY / window.innerHeight - 0.5) * 2;
  });

  // ── RESIZE ──
  window.addEventListener('resize', () => {
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
  });

  // ── ANIMATE ──
  let t = 0;
  function animate() {
    requestAnimationFrame(animate);
    t += 0.005;

    particles.rotation.y += 0.0008;
    particles.rotation.x += 0.0003;

    wireSphere.rotation.x += 0.001;
    wireSphere.rotation.y += 0.0015;

    scene.children.forEach(obj => {
      if (obj.userData.rotX) {
        obj.rotation.x += obj.userData.rotX;
        obj.rotation.y += obj.userData.rotY;
      }
    });

    // Smooth camera follow mouse
    camera.position.x += (mouseX * 8 - camera.position.x) * 0.03;
    camera.position.y += (-mouseY * 5 - camera.position.y) * 0.03;
    camera.lookAt(scene.position);

    renderer.render(scene, camera);
  }
  animate();
})();

// ── TYPING EFFECT ──
(function() {
  const lines = [
    'Full-Stack Developer & Database Admin',
    'Consultor Técnico Senior',
    'Mentor Académico | UPEC',
    'Investigador PRISMA 2020 | LLMs',
    'Ingeniero en Computación',
  ];
  let lineIdx = 0, charIdx = 0, deleting = false;
  const el = document.getElementById('typed');

  function tick() {
    const current = lines[lineIdx];
    if (!deleting) {
      el.textContent = current.slice(0, ++charIdx);
      if (charIdx === current.length) {
        deleting = true;
        setTimeout(tick, 2000);
        return;
      }
    } else {
      el.textContent = current.slice(0, --charIdx);
      if (charIdx === 0) {
        deleting = false;
        lineIdx = (lineIdx + 1) % lines.length;
      }
    }
    setTimeout(tick, deleting ? 40 : 65);
  }
  tick();
})();

// ── INTERSECTION OBSERVER for .reveal ──
const revealEls = document.querySelectorAll('.reveal');
const revealObs = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      e.target.classList.add('visible');
    }
  });
}, { threshold: 0.1 });
revealEls.forEach(el => revealObs.observe(el));

// ── SKILL BARS animation ──
const barObs = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      e.target.querySelectorAll('.skill-bar-fill').forEach(bar => {
        const pct = bar.getAttribute('data-pct');
        setTimeout(() => { bar.style.width = pct + '%'; }, 200);
      });
      barObs.unobserve(e.target);
    }
  });
}, { threshold: 0.2 });
const barsSection = document.getElementById('skillBars');
if (barsSection) barObs.observe(barsSection);

// ── COUNT-UP ANIMATION ──
const counterObs = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      e.target.querySelectorAll('[data-target]').forEach(el => {
        const target = +el.getAttribute('data-target');
        const step = target / 40;
        let cur = 0;
        const timer = setInterval(() => {
          cur = Math.min(cur + step, target);
          el.textContent = Math.ceil(cur);
          if (cur >= target) clearInterval(timer);
        }, 30);
      });
      counterObs.unobserve(e.target);
    }
  });
}, { threshold: 0.3 });
document.querySelectorAll('.stats-grid').forEach(el => counterObs.observe(el));
</script>

</body>
</html>
