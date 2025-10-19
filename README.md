<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Alejandro Andreu — Terminal</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Orbitron:wght@600;800&display=swap" rel="stylesheet">
  <style>
    /* CSS embebido para evitar problemas de carga */
    :root{
      /* Terminal palette con más colores */
      --ink:#000000;
      --ink-2:#0a0a0a;
      --text:#00ff00;
      --text-glow: 0 0 5px currentColor;
      --muted:#006400;
      --green:#00ff00;
      --green-glow: 0 0 10px #00ff00;
      --cyan:#00ffff;
      --cyan-glow: 0 0 10px #00ffff;
      --yellow:#ffff00;
      --yellow-glow: 0 0 10px #ffff00;
      --blue:#0099ff;
      --blue-glow: 0 0 10px #0099ff;
      --red:#ff0000;
      --red-glow: 0 0 10px #ff0000;
      --maxw:1320px;
      font-family: 'Share Tech Mono', monospace;
    }

    *{box-sizing:border-box}
    html,body{height:100%;margin:0;background:var(--ink);
      color:var(--text); overflow-x: hidden;}

    /* Efecto de pantalla CRT */
    .crt-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: 
        linear-gradient(rgba(0, 255, 0, 0.03) 50%, transparent 50%),
        linear-gradient(90deg, rgba(255, 0, 0, 0.03), rgba(0, 255, 0, 0.02), rgba(0, 0, 255, 0.03));
      background-size: 100% 4px, 3px 100%;
      z-index: 100;
      pointer-events: none;
      animation: scan 8s linear infinite;
    }

    @keyframes scan {
      0% { background-position: 0 0, 0 0; }
      100% { background-position: 0 100%, 0 0; }
    }

    /* Boot cyberpunk mejorado */
    .boot {
      position: fixed;
      inset: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      background: #000;
      z-index: 1000;
      font-family: 'Share Tech Mono', monospace;
      overflow: hidden;
    }

    .boot-terminal {
      position: relative;
      color: #00ff00;
      text-shadow: 0 0 10px #00ff00;
      font-size: 18px;
      width: 100%;
      max-width: 800px;
      padding: 20px;
      background: rgba(0, 0, 0, 0.9);
      border: 1px solid #00ff00;
      box-shadow: 0 0 40px rgba(0, 255, 0, 0.7);
    }

    /* Grid de fondo cyberpunk */
    .cyber-grid {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: 
        linear-gradient(90deg, transparent 99%, rgba(0, 255, 0, 0.1) 99%),
        linear-gradient(transparent 99%, rgba(0, 255, 0, 0.1) 99%);
      background-size: 20px 20px;
      animation: gridMove 15s linear infinite;
      opacity: 0.4;
    }

    @keyframes gridMove {
      0% { transform: translate(0, 0); }
      100% { transform: translate(20px, 20px); }
    }

    /* Línea de escaneo cyberpunk */
    .cyber-scan {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 3px;
      background: linear-gradient(90deg, transparent, #00ff00, #00ffff, #00ff00, transparent);
      box-shadow: 0 0 15px #00ff00;
      animation: scan 3s linear infinite;
      z-index: 2;
    }

    .boot-content {
      position: relative;
      z-index: 2;
    }

    .terminal-line {
      opacity: 0;
      margin-bottom: 10px;
      overflow: hidden;
      white-space: nowrap;
      border-right: 2px solid #00ff00;
      animation: typeLine 1s forwards, blinkCursor 0.8s infinite;
      font-size: 18px;
      font-weight: normal;
    }

    /* TIEMPOS DE BOOT REDUCIDOS */
    .system-init { animation-delay: 0.2s; color: #00ffff; }
    .system-loading { animation-delay: 0.8s; color: #00ffff; }
    .system-connect { animation-delay: 1.4s; color: #00ffff; }
    .welcome-msg { 
      animation-delay: 2.0s; 
      color: #ffff00; 
      text-shadow: 0 0 15px #ffff00;
      font-size: 20px;
      font-weight: bold;
    }
    .system-ready { 
      animation-delay: 2.8s; 
      color: #00ff00; 
      font-weight: bold;
    }
    .access-granted { 
      animation-delay: 3.5s; 
      color: #00ff00; 
      text-shadow: 0 0 20px #00ff00;
      font-size: 22px;
      font-weight: bold;
      animation: typeLine 1s forwards, blinkCursor 0.5s infinite, pulse 2s infinite 4.5s;
    }

    @keyframes typeLine {
      0% { 
        opacity: 0;
        width: 0;
      }
      100% { 
        opacity: 1;
        width: 100%;
      }
    }

    @keyframes blinkCursor {
      0%, 100% { border-color: #00ff00; }
      50% { border-color: transparent; }
    }

    @keyframes pulse {
      0%, 100% { opacity: 1; text-shadow: 0 0 20px #00ff00; }
      50% { opacity: 0.7; text-shadow: 0 0 10px #00ff00; }
    }

    /* Efectos de glitch para el boot */
    .boot-terminal::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: linear-gradient(45deg, transparent 49%, rgba(0, 255, 0, 0.1) 50%, transparent 51%);
      background-size: 10px 10px;
      animation: glitchMove 5s infinite;
      opacity: 0;
      pointer-events: none;
      z-index: 1;
    }

    @keyframes glitchMove {
      0%, 90%, 100% { transform: translate(0, 0); opacity: 0; }
      5% { transform: translate(-2px, 2px); opacity: 0.1; }
      10% { transform: translate(2px, -2px); opacity: 0.2; }
      15% { transform: translate(-1px, 1px); opacity: 0.1; }
      20% { transform: translate(1px, -1px); opacity: 0.2; }
    }

    /* TRANSICIÓN MÁS RÁPIDA - REDUCIDA DE 3s A 1.5s */
    .boot.fade-out {
      animation: cinematicFadeOut 1.5s ease-in-out forwards;
    }

    @keyframes cinematicFadeOut {
      0% { 
        opacity: 1;
        filter: brightness(1) blur(0);
      }
      30% {
        opacity: 0.9;
        filter: brightness(1.2) blur(1px);
      }
      70% {
        opacity: 0.5;
        filter: brightness(1.5) blur(3px);
      }
      100% { 
        opacity: 0;
        filter: brightness(2) blur(8px);
        display: none;
      }
    }

    /* CONTENIDO APARECE MÁS RÁPIDO - REDUCIDO DE 2.5s A 1s */
    .content-reveal {
      opacity: 0;
      animation: contentReveal 1.2s ease-in-out 0.3s forwards;
    }

    @keyframes contentReveal {
      0% { 
        opacity: 0;
        transform: translateY(20px);
        filter: blur(5px);
      }
      100% { 
        opacity: 1;
        transform: translateY(0);
        filter: blur(0);
      }
    }

    .hidden{display:none}

    /* Cabecera de terminal */
    .terminal-header {
      padding: 10px 3vw;
      border-bottom: 1px solid var(--green);
      margin-bottom: 20px;
      box-shadow: 0 0 10px rgba(0, 255, 0, 0.3);
      background: rgba(0, 0, 0, 0.8);
      position: sticky;
      top: 0;
      z-index: 100;
    }

    .system-status {
      display: flex;
      justify-content: space-between;
      align-items: center;
      max-width: var(--maxw);
      margin: 0 auto;
    }

    .status-indicator {
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background: var(--green);
      box-shadow: var(--green-glow);
      animation: pulse 2s infinite;
    }

    .status-text {
      font-weight: bold;
      letter-spacing: 1px;
    }

    .datetime {
      color: var(--muted);
      font-size: 14px;
    }

    /* Layout */
    .top{
      padding: 20px 3vw;
      display: flex;
      justify-content: center;
      min-height: calc(100vh - 60px);
    }

    .container{
      max-width: var(--maxw);
      margin: 0 auto;
      padding: 0 3vw 40px;
      position: relative; 
      z-index: 10;
      width: 100%;
    }

    .identity{
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      gap: 18px;
      flex-wrap: wrap;
      margin-bottom: 30px;
    }

    .name{
      font-family: 'Share Tech Mono', monospace;
      font-weight: 800;
      font-size: 34px;
      letter-spacing: 1px;
      margin: 0;
      color: var(--green);
      text-shadow: var(--green-glow);
      line-height: 1.2;
      min-height: 40px;
    }

    .role{
      margin: 10px 0 0;
      color: var(--cyan);
      font-weight: 700;
      letter-spacing: 1px;
      font-size: 18px;
      min-height: 24px;
      text-shadow: var(--cyan-glow);
    }

    .loc{
      margin: 5px 0 0;
      color: var(--muted);
      font-size: 14px;
      letter-spacing: 1px;
      min-height: 18px;
    }

    .actions{
      display: flex;
      gap: 10px;
      align-items: center;
      flex-wrap: wrap;
    }

    /* Botones de terminal */
    .terminal-btn {
      background: transparent;
      border: 1px solid var(--green);
      padding: 10px 15px;
      border-radius: 0;
      color: var(--green);
      font-weight: 800;
      text-decoration: none;
      cursor: pointer;
      transition: all 0.3s ease;
      position: relative;
      overflow: hidden;
      font-family: 'Share Tech Mono', monospace;
      text-transform: uppercase;
      letter-spacing: 1px;
      font-size: 14px;
    }

    .terminal-btn::before {
      content: '';
      position: absolute;
      top: 0;
      left: -100%;
      width: 100%;
      height: 100%;
      background: linear-gradient(90deg, transparent, rgba(0, 255, 0, 0.2), transparent);
      transition: left 0.5s;
    }

    .terminal-btn:hover::before {
      left: 100%;
    }

    .terminal-btn:hover {
      box-shadow: 0 0 15px rgba(0, 255, 0, 0.5);
      text-shadow: var(--green-glow);
      transform: translateY(-2px);
    }

    .terminal-btn.ghost {
      border-style: dashed;
      color: var(--green);
      border-color: var(--green);
    }

    /* Feature Profile card */
    .terminal-card {
      background: rgba(0, 0, 0, 0.7);
      border-radius: 0;
      padding: 20px;
      border: 1px solid var(--green);
      position: relative;
      overflow: hidden;
      box-shadow: 0 0 10px rgba(0, 255, 0, 0.2);
      transition: all 0.3s ease;
      margin-bottom: 20px;
      animation: subtleGlow 15s infinite alternate;
    }

    @keyframes subtleGlow {
      0%, 100% { 
        box-shadow: 0 0 10px rgba(0, 255, 0, 0.2);
        border-color: var(--green);
      }
      25% { 
        box-shadow: 0 0 15px rgba(0, 255, 255, 0.3);
        border-color: var(--cyan);
      }
      50% { 
        box-shadow: 0 0 12px rgba(255, 255, 0, 0.25);
        border-color: var(--yellow);
      }
      75% { 
        box-shadow: 0 0 18px rgba(0, 153, 255, 0.35);
        border-color: var(--blue);
      }
    }

    .terminal-card:hover {
      animation: none;
      box-shadow: 0 0 20px rgba(0, 255, 0, 0.6);
      transform: translateY(-2px);
    }

    .profile-feature {
      margin-top: 16px;
    }

    .card-title {
      font-family: 'Share Tech Mono', monospace;
      color: var(--green);
      margin: 0 0 15px;
      font-size: 18px;
      letter-spacing: 1px;
      text-transform: uppercase;
      border-bottom: 1px solid rgba(0, 255, 0, 0.3);
      padding-bottom: 8px;
      min-height: 30px;
    }

    .card-title.alt {
      color: var(--blue);
    }

    .sub {
      margin-top: 15px;
      margin-bottom: 10px;
      font-weight: 800;
      text-transform: uppercase;
      font-size: 14px;
    }

    .sub.cyan { color: var(--cyan); text-shadow: var(--cyan-glow); }
    .sub.yellow { color: var(--yellow); text-shadow: var(--yellow-glow); }
    .sub.green { color: var(--green); text-shadow: var(--green-glow); }
    .sub.blue { color: var(--blue); text-shadow: var(--blue-glow); }

    /* Grid mejorado */
    .grid {
      display: grid;
      grid-template-columns: 1fr 1fr 1fr;
      gap: 20px;
      align-items: start;
      margin-top: 18px;
    }

    .col-1, .col-2, .col-3 {
      display: flex;
      flex-direction: column;
    }

    /* Chips con colores */
    .chips {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      list-style: none;
      padding: 0;
      margin: 0 0 15px 0;
    }

    .chips li {
      background: rgba(0, 0, 0, 0.8);
      border: 1px solid var(--green);
      padding: 6px 10px;
      border-radius: 0;
      color: var(--text);
      font-weight: 700;
      transition: .15s ease;
      font-family: 'Share Tech Mono', monospace;
      font-size: 13px;
    }

    .chips li:hover {
      transform: translateY(-1px);
      box-shadow: 0 0 10px rgba(0, 255, 0, 0.5);
      text-shadow: var(--text-glow);
    }

    .chips.alt li {
      border-color: var(--cyan);
    }

    .cert-chip {
      border-color: var(--yellow) !important;
      color: var(--yellow) !important;
    }

    .cert-chip:hover {
      box-shadow: 0 0 10px rgba(255, 255, 0, 0.5) !important;
      text-shadow: var(--yellow-glow) !important;
    }

    /* Idiomas */
    .languages {
      margin-top: 10px;
    }

    .language-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 10px;
      padding: 5px 0;
      border-bottom: 1px dashed rgba(0, 255, 0, 0.2);
    }

    .lang-name {
      color: var(--cyan);
      font-size: 14px;
    }

    .lang-level {
      display: flex;
      gap: 4px;
    }

    .level-dot {
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background: rgba(0, 255, 0, 0.2);
      border: 1px solid var(--green);
    }

    .level-dot.active {
      background: var(--green);
      box-shadow: var(--green-glow);
    }

    /* Timeline para formación */
    .timeline {
      margin: 15px 0;
    }

    .timeline-item {
      display: flex;
      margin-bottom: 15px;
      padding-bottom: 15px;
      border-bottom: 1px dashed rgba(0, 255, 0, 0.2);
    }

    .timeline-date {
      min-width: 110px;
      color: var(--cyan);
      font-weight: 800;
      font-size: 14px;
      text-shadow: var(--cyan-glow);
    }

    .timeline-content {
      flex: 1;
    }

    .timeline-title {
      color: var(--yellow);
      font-weight: 800;
      margin-bottom: 5px;
      text-shadow: var(--yellow-glow);
    }

    .timeline-desc {
      color: var(--muted);
      font-size: 13px;
    }

    /* Animación hacker */
    .hacker-animation {
      background: rgba(0, 0, 0, 0.9) !important;
      border: 1px solid var(--cyan) !important;
      padding: 15px !important;
      margin-bottom: 20px !important;
      min-height: 150px;
      display: flex;
      flex-direction: column;
      justify-content: center;
    }

    .code-line {
      margin-bottom: 8px;
      font-size: 14px;
      opacity: 0;
      animation: fadeInLine 0.5s forwards;
    }

    .code-line:nth-child(1) { animation-delay: 0.2s; }
    .code-line:nth-child(2) { animation-delay: 0.8s; }
    .code-line:nth-child(3) { animation-delay: 1.4s; }
    .code-line:nth-child(4) { animation-delay: 2s; }
    .code-line:nth-child(5) { animation-delay: 2.6s; }

    @keyframes fadeInLine {
      to { opacity: 1; }
    }

    .blinking {
      animation: blink 2s infinite;
    }

    @keyframes blink {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.3; }
    }

    /* Experiencia */
    .exp-item {
      margin-top: 10px;
    }

    .exp-head {
      margin-bottom: 10px;
    }

    .role-title {
      font-weight: 900;
      text-transform: uppercase;
      font-size: 16px;
      margin-bottom: 5px;
    }

    .role-title.yellow { color: var(--yellow); text-shadow: var(--yellow-glow); }

    .muted {
      color: var(--muted);
      font-size: 13px;
      margin-bottom: 5px;
    }

    .tools {
      font-size: 13px;
      color: var(--muted);
      opacity: .95;
      font-style: italic;
    }

    .tools.cyan { color: var(--cyan); }

    .bullet {
      margin-top: 8px;
      padding-left: 18px;
    }

    .bullet li {
      margin-bottom: 5px;
      font-size: 14px;
    }

    /* Proyectos */
    .proj-grid {
      display: grid;
      grid-template-columns: 1fr;
      gap: 15px;
      margin-top: 12px;
    }

    .proj {
      background: rgba(0, 0, 0, 0.5);
      padding: 15px;
      border-radius: 0;
      border: 1px solid rgba(0, 255, 0, 0.3);
      font-size: 14px;
      transition: all 0.3s ease;
      min-height: 120px;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
    }

    .proj:hover {
      border-color: var(--green);
      box-shadow: 0 0 10px rgba(0, 255, 0, 0.3);
      transform: translateY(-3px);
    }

    .proj p {
      margin: 8px 0 0;
      color: var(--muted);
    }

    /* Estilos para enlaces de proyectos */
    .proj-link {
      text-decoration: none;
      color: inherit;
      display: block;
    }

    .proj-link:hover .proj {
      border-color: #00ff00;
      box-shadow: 0 0 20px rgba(0, 255, 0, 0.5);
      transform: translateY(-5px);
    }

    .project-link-indicator {
      margin-top: 8px;
      font-size: 12px;
      color: #00ffff;
      text-shadow: 0 0 5px #00ffff;
      opacity: 0;
      transition: opacity 0.3s ease;
    }

    .proj-link:hover .project-link-indicator {
      opacity: 1;
      animation: blink 1s infinite;
    }

    /* Footer */
    .foot {
      text-align: center;
      padding: 28px 0;
      color: var(--muted);
      opacity: .85;
      font-size: 13px;
      border-top: 1px solid var(--green);
      margin-top: 40px;
    }

    .terminal-footer {
      position: relative;
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 10px;
    }

    .cursor {
      animation: blink 1s infinite;
      color: var(--green);
    }

    /* Efecto glitch ocasional */
    @keyframes glitch {
      0% { transform: translate(0); }
      20% { transform: translate(-2px, 2px); }
      40% { transform: translate(-2px, -2px); }
      60% { transform: translate(2px, 2px); }
      80% { transform: translate(2px, -2px); }
      100% { transform: translate(0); }
    }

    .glitch {
      animation: glitch 0.3s linear;
    }

    /* Typing cursor */
    .typing-cursor {
      display: inline-block;
      width: 8px;
      height: 1.2em;
      background-color: var(--green);
      margin-left: 2px;
      animation: blink 1s infinite;
      vertical-align: middle;
    }

    /* Responsive */
    @media (max-width: 1180px) {
      .grid {
        grid-template-columns: 1fr 1fr;
      }
      .col-3 {
        grid-column: 1 / span 2;
      }
    }

    @media (max-width: 980px) {
      .grid {
        grid-template-columns: 1fr;
      }
      .col-3 {
        grid-column: 1;
      }
      
      .actions {
        flex-wrap: wrap;
      }
      
      .name {
        font-size: 28px;
      }
      
      .identity {
        flex-direction: column;
      }
    }

    @media (max-width: 768px) {
      .system-status {
        flex-direction: column;
        gap: 10px;
      }
      
      .terminal-btn {
        padding: 8px 12px;
        font-size: 12px;
      }
      
      .name {
        font-size: 24px;
      }
      
      .role {
        font-size: 16px;
      }
      
      .grid {
        gap: 15px;
      }
    }

    /* Asegurar que el fondo cubre toda la página */
    body {
      min-height: 100vh;
      background: var(--ink);
    }

    /* Canvas layer behind */
    .fx-layer {
      position: fixed;
      inset: 0;
      width: 100%;
      height: 100%;
      z-index: 0;
      pointer-events: none;
    }

    /* Elevate content above FX */
    .container, .top, main {
      position: relative;
      z-index: 1;
    }
  </style>
</head>
<body class="terminal">
  <!-- Efecto de pantalla CRT -->
  <div class="crt-overlay"></div>
  
  <!-- Boot mejorado con animación cyberpunk -->
  <div id="boot" class="boot">
    <div class="boot-terminal">
      <div class="cyber-grid"></div>
      <div class="boot-content">
        <div class="terminal-line system-init">> INITIALIZING TERMINAL...</div>
        <div class="terminal-line system-loading">> LOADING SECURITY PROTOCOLS...</div>
        <div class="terminal-line system-connect">> ESTABLISHING CONNECTION...</div>
        <div class="terminal-line welcome-msg">> WELCOME TO MY PROFILE INTERFACE</div>
        <div class="terminal-line system-ready">> SYSTEM READY.</div>
      </div>
      <div class="cyber-scan"></div>
    </div>
  </div>

  <main id="app" class="hidden content-reveal">
    <!-- Cabecera tipo terminal -->
    <header class="terminal-header">
      <div class="system-status">
        <span class="status-indicator online"></span>
        <span class="status-text">SYSTEM ONLINE</span>
        <span class="datetime" id="datetime"></span>
      </div>
    </header>

    <section class="top">
      <div class="container">
        <!-- Identidad con efecto de escritura mejorado -->
        <div class="identity">
          <div class="titles">
            <h1 class="name" id="typed-name"></h1>
            <p class="role" id="typed-role"></p>
            <p class="loc" id="typed-loc"></p>
          </div>
          <nav class="actions">
            <a class="btn terminal-btn" href="https://www.linkedin.com/in/alex-andreu/" target="_blank" rel="noopener">
              <span class="btn-text">[LINKEDIN]</span>
            </a>
            <a class="btn terminal-btn" href="https://github.com/4lex-42" target="_blank" rel="noopener">
              <span class="btn-text">[GITHUB]</span>
            </a>
            <button id="copyEmail" class="btn ghost terminal-btn">
              <span class="btn-text">[COPY_EMAIL]</span>
            </button>
          </nav>
        </div>

        <!-- PERFIL con efecto de escritura -->
        <article class="card profile-feature terminal-card">
          <h3 class="card-title alt" id="profile-title"></h3>
          <p id="profile-text"></p>
        </article>

        <!-- GRID principal reorganizado -->
        <section class="grid">
          <!-- Columna 1: Competencias e Idiomas -->
          <div class="col-1">
            <article class="card terminal-card">
              <h3 class="card-title">> COMPETENCIAS DESTACADAS</h3>
              <div class="sub cyan">SISTEMAS & CLOUD</div>
              <ul class="chips">
                <li>Win Server / AD</li><li>GNU/Linux</li><li>Azure</li>
                <li>VMware / Hyper‑V</li><li>Firewalls</li><li>Microsoft 365</li><li>Network Infrastructure</li>
              </ul>
              <div class="sub yellow">DESARROLLO</div>
              <ul class="chips">
                <li>C / C++</li><li>Bash</li><li>PowerShell</li><li>HTML / CSS / JS</li><li>SQL</li>
              </ul>
              <div class="sub green">HABILIDADES</div>
              <ul class="chips">
                <li>IT Security</li><li>Cloud Migration</li><li>Automation</li>
                <li>IT Support</li><li>Docker / K8s</li>
              </ul>
              
              <!-- Nueva sección de idiomas -->
              <div class="sub blue">IDIOMAS</div>
              <div class="languages">
                <div class="language-item">
                  <span class="lang-name">Español</span>
                  <div class="lang-level">
                    <span class="level-dot active"></span>
                    <span class="level-dot active"></span>
                    <span class="level-dot active"></span>
                    <span class="level-dot active"></span>
                    <span class="level-dot active"></span>
                  </div>
                </div>
                <div class="language-item">
                  <span class="lang-name">Inglés</span>
                  <div class="lang-level">
                    <span class="level-dot active"></span>
                    <span class="level-dot active"></span>
                    <span class="level-dot active"></span>
                    <span class="level-dot"></span>
                    <span class="level-dot"></span>
                  </div>
                </div>
              </div>
            </article>
          </div>

          <!-- Columna 2: Experiencia y Formación -->
          <div class="col-2">
            <article class="card terminal-card">
              <h3 class="card-title">> EXPERIENCIA</h3>
              <div class="exp-item">
                <div class="exp-head">
                  <div>
                    <div class="role-title yellow">IT SUPPORT — ANDERSEN IN SPAIN</div>
                    <div class="muted">JUN. 2025 — ACTUALIDAD · PRÁCTICAS · PRESENCIAL</div>
                    <div class="tools cyan">Windows Server · Azure · Intune-MDM · PaperCut · Backup</div>
                  </div>
                </div>
                <ul class="bullet">
                  <li>> Soporte a usuarios y resolución de incidencias</li>
                  <li>> Mantenimiento y configuración de equipos</li>
                  <li>> Gestión de redes y configuración de servidores</li>
                  <li>> Migraciones locales a Azure, IAM - MDM - despliegue automatizado con Autopilot</li>
                </ul>
              </div>
            </article>

            <!-- Animación hacker debajo de experiencia -->
            <div class="hacker-animation terminal-card">
              <div class="code-line">> ANALYZING SYSTEM INTEGRITY...</div>
              <div class="code-line">> SECURITY: <span class="green">OPTIMAL</span></div>
              <div class="code-line">> NETWORK: <span class="cyan">STABLE</span></div>
              <div class="code-line">> CLOUD SERVICES: <span class="yellow">OPERATIONAL</span></div>
              <div class="code-line blinking">> READY FOR NEW CHALLENGES</div>
            </div>

            <article class="card terminal-card">
              <h3 class="card-title">> FORMACIÓN & CERTIFICACIONES</h3>
              <div class="timeline">
                <div class="timeline-item">
                  <div class="timeline-date cyan">2025–2027</div>
                  <div class="timeline-content">
                    <div class="timeline-title yellow">ASIR · FP CEU MADRID</div>
                    <div class="timeline-desc">Administración de Sistemas Informáticos en Red</div>
                  </div>
                </div>
                <div class="timeline-item">
                  <div class="timeline-date cyan">2023–2025</div>
                  <div class="timeline-content">
                    <div class="timeline-title yellow">SMR · FP CEU MADRID</div>
                    <div class="timeline-desc">Sistemas Microinformáticos y Redes</div>
                  </div>
                </div>
              </div>
              <div class="sub green">CERTIFICACIONES</div>
              <ul class="chips alt">
                <li class="cert-chip">Cisco CCNA 1</li>
                <li class="cert-chip">IT Essentials</li>
                <li class="cert-chip">Microsoft Azure Fundamentals</li>
              </ul>
            </article>
          </div>

          <!-- Columna 3: Proyectos -->
          <div class="col-3">
            <article class="card terminal-card">
              <h3 class="card-title">> PROYECTOS</h3>
              <div class="proj-grid">
                <div class="proj">
                  <div class="sub cyan">> IMPLEMENTACIÓN DE ACTIVE DIRECTORY</div>
                  <p>Creación de usuarios, grupos y políticas GPO. LDAP/Kerberos.</p>
                </div>
                <div class="proj">
                  <div class="sub yellow">> MIGRACIÓN A WINDOWS SERVER 2022</div>
                  <p>Hyper‑V, DFS, DHCP y DNS empresarial.</p>
                </div>
                <a href="https://github.com/4lex-42/Discovery-Web-42" target="_blank" class="proj-link">
                  <div class="proj">
                    <div class="sub green">> PISCINE DISCOVERY WEB — 42 MADRID</div>
                    <p>ENE 2025 · Mini‑proyectos full‑stack (front/back).</p>
                    <div class="project-link-indicator">↗ ENLACE A GITHUB</div>
                  </div>
                </a>
                <a href="https://github.com/4lex-42/42-Madrid-Piscine-C-Shell" target="_blank" class="proj-link">
                  <div class="proj">
                    <div class="sub blue">> PISCINE C & SHELL — 42 MADRID</div>
                    <p>FEB–MAR 2024 · Programación en C y scripting en Shell.</p>
                    <div class="project-link-indicator">↗ ENLACE A GITHUB</div>
                  </div>
                </a>
              </div>
            </article>
          </div>
        </section>

        <footer class="foot terminal-footer">
          <span class="cursor">_</span> ALEJANDRO ANDREU © 2025 <span class="cursor">_</span>
        </footer>
      </div>
    </section>
  </main>

  <script>
    // JavaScript completo embebido
    const app = document.getElementById('app');
    const boot = document.getElementById('boot');

    // Inicializar efectos del boot
    function initBootEffects() {
      // Efecto de parpadeo inicial
      setTimeout(() => {
        document.querySelector('.boot-terminal').style.animation = 'terminalFlicker 0.3s';
      }, 500);
      
      // Efectos de glitch aleatorios durante el boot
      const glitchInterval = setInterval(() => {
        if (Math.random() > 0.7) {
          const bootTerminal = document.querySelector('.boot-terminal');
          bootTerminal.style.animation = 'glitchEffect 0.2s';
          setTimeout(() => {
            bootTerminal.style.animation = '';
          }, 200);
        }
      }, 800);
      
      // Limpiar intervalo cuando termine el boot
      setTimeout(() => {
        clearInterval(glitchInterval);
      }, 4500); // Reducido de 6500 a 4500
    }

    // Añadir estilos de animación para efectos del boot
    const style = document.createElement('style');
    style.textContent = `
      @keyframes terminalFlicker {
        0%, 100% { opacity: 1; }
        50% { opacity: 0.8; }
      }
      
      @keyframes glitchEffect {
        0% { transform: translate(0); filter: hue-rotate(0deg); }
        20% { transform: translate(-2px, 2px); filter: hue-rotate(90deg); }
        40% { transform: translate(-2px, -2px); filter: hue-rotate(180deg); }
        60% { transform: translate(2px, 2px); filter: hue-rotate(270deg); }
        80% { transform: translate(2px, -2px); filter: hue-rotate(360deg); }
        100% { transform: translate(0); filter: hue-rotate(0deg); }
      }
    `;
    document.head.appendChild(style);

    // TIEMPOS DE TRANSICIÓN REDUCIDOS
    // Iniciar boot sequence con transición cinematográfica
    setTimeout(() => { 
      // Iniciar transición cinematográfica
      boot.classList.add('fade-out');
      
      // Mostrar el contenido principal después de que comience la transición
      setTimeout(() => {
        boot.style.display = 'none';
        app.classList.remove('hidden');
        startTerminalEffects();
      }, 1500); // Reducido de 3000 a 1500ms
    }, 4500); // Reducido de 6500 a 4500ms

    // Inicializar efectos del boot
    initBootEffects();

    // Efectos de terminal después del boot
    function startTerminalEffects() {
      // Actualizar fecha y hora en tiempo real
      updateDateTime();
      setInterval(updateDateTime, 1000);
      
      // TIEMPOS DE ESCRITURA MÁS RÁPIDOS
      typeText('typed-name', 'ALEJANDRO ANDREU', 80, true);
      setTimeout(() => typeText('typed-role', 'IT INFRASTRUCTURES & CLOUDOPS', 60, true), 1000); // Reducido
      setTimeout(() => typeText('typed-loc', 'MADRID / SEGOVIA, ESPAÑA', 60, true), 2000); // Reducido
      setTimeout(() => typeText('profile-title', '> PERFIL PROFESIONAL', 60, false), 3000); // Reducido
      setTimeout(() => typeText('profile-text', 
        'Técnico en Sistemas Microinformáticos y Redes con experiencia en administración de infraestructuras, gestión de conectividad, servicios Cloud e IT Support. Me enfoco en investigar, aprender y proponer mejoras que optimicen procesos y faciliten el trabajo diario.', 
        40, true), 4000); // Reducido
      
      // Efecto glitch aleatorio
      setInterval(randomGlitch, 15000);
    }

    // Efecto de escritura mejorado
    function typeText(elementId, text, speed, showCursor) {
      const element = document.getElementById(elementId);
      if (!element) return;
      
      element.textContent = '';
      if (showCursor) {
        const cursor = document.createElement('span');
        cursor.className = 'typing-cursor';
        element.appendChild(cursor);
      }
      
      let i = 0;
      const timer = setInterval(() => {
        if (i < text.length) {
          element.textContent = text.substring(0, i + 1);
          if (showCursor) {
            const cursor = document.createElement('span');
            cursor.className = 'typing-cursor';
            element.appendChild(cursor);
          }
          i++;
        } else {
          clearInterval(timer);
          if (showCursor) {
            element.textContent = text;
          }
        }
      }, speed);
    }

    // Actualizar fecha y hora
    function updateDateTime() {
      const now = new Date();
      const datetimeElement = document.getElementById('datetime');
      if (datetimeElement) {
        const date = now.toLocaleDateString('es-ES', { 
          day: '2-digit', 
          month: '2-digit', 
          year: 'numeric' 
        });
        const time = now.toLocaleTimeString('es-ES', { 
          hour: '2-digit', 
          minute: '2-digit',
          second: '2-digit'
        });
        datetimeElement.textContent = `${date} | ${time}`;
      }
    }

    // Efecto glitch aleatorio
    function randomGlitch() {
      const elements = document.querySelectorAll('.terminal-card, .name, .card-title');
      if (elements.length > 0) {
        const randomElement = elements[Math.floor(Math.random() * elements.length)];
        randomElement.classList.add('glitch');
        setTimeout(() => {
          randomElement.classList.remove('glitch');
        }, 300);
      }
    }

    // Copiar email
    const copyBtn = document.getElementById('copyEmail');
    if(copyBtn){
      copyBtn.addEventListener('click', async() => {
        const email = 'alexandreu.adc@proton.me';
        try { 
          await navigator.clipboard.writeText(email); 
          copyBtn.querySelector('.btn-text').textContent = '[EMAIL_COPIED]';
          copyBtn.style.background = 'rgba(0, 255, 0, 0.1)';
          
          setTimeout(() => {
            copyBtn.querySelector('.btn-text').textContent = '[COPY_EMAIL]';
            copyBtn.style.background = 'transparent';
          }, 2000);
        } catch { 
          const tempInput = document.createElement('input');
          tempInput.value = email;
          document.body.appendChild(tempInput);
          tempInput.select();
          document.execCommand('copy');
          document.body.removeChild(tempInput);
          
          copyBtn.querySelector('.btn-text').textContent = '[EMAIL_COPIED]';
          copyBtn.style.background = 'rgba(0, 255, 0, 0.1)';
          
          setTimeout(() => {
            copyBtn.querySelector('.btn-text').textContent = '[COPY_EMAIL]';
            copyBtn.style.background = 'transparent';
          }, 2000);
        }
      });
    }

    // Efecto de partículas simplificado para terminal
    (function(){
      const canvas = document.createElement('canvas');
      canvas.className = 'fx-layer';
      document.body.appendChild(canvas);
      
      const ctx = canvas.getContext('2d');
      let W, H, particles = [];
      const COUNT = 40;

      function resize() { 
        W = canvas.width = innerWidth; 
        H = canvas.height = innerHeight; 
      }
      
      window.addEventListener('resize', resize); 
      resize();

      // Crear partículas
      function spawn() {
        particles = Array.from({length: COUNT}, () => ({
          x: Math.random() * W,
          y: Math.random() * H,
          vx: (Math.random() - 0.5) * 0.5,
          vy: (Math.random() - 0.5) * 0.5,
          life: Math.random() * 100,
          maxLife: 100 + Math.random() * 200,
          size: Math.random() * 1.5 + 0.5,
          color: Math.random() < 0.6 ? '#00ff00' : 
                 Math.random() < 0.8 ? '#00ffff' : 
                 Math.random() < 0.9 ? '#ffff00' : '#0099ff'
        }));
      }
      
      spawn();

      function tick() {
        ctx.fillStyle = 'rgba(0, 0, 0, 0.05)';
        ctx.fillRect(0, 0, W, H);
        
        particles.forEach(p => {
          p.x += p.vx;
          p.y += p.vy;
          p.life++;
          
          if (p.x < -10 || p.x > W + 10 || p.y < -10 || p.y > H + 10 || p.life > p.maxLife) {
            p.x = Math.random() * W;
            p.y = Math.random() * H;
            p.life = 0;
            p.vx = (Math.random() - 0.5) * 0.5;
            p.vy = (Math.random() - 0.5) * 0.5;
          }
          
          const alpha = 0.1 + 0.1 * Math.sin(p.life * 0.05);
          ctx.fillStyle = p.color.replace(')', `, ${alpha})`).replace('rgb', 'rgba');
          ctx.fillRect(p.x, p.y, p.size, p.size);
          
          ctx.fillStyle = p.color.replace(')', `, ${alpha * 0.3})`).replace('rgb', 'rgba');
          ctx.fillRect(p.x - p.vx * 2, p.y - p.vy * 2, p.size * 0.7, p.size * 0.7);
        });
        
        requestAnimationFrame(tick);
      }
      
      requestAnimationFrame(tick);
    })();
  </script>
</body>
</html>
