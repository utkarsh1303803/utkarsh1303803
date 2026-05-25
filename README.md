<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Syne:wght@400;600;700;800&family=Inter:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: #080B10;
    color: #E8EAF0;
    font-family: 'Inter', sans-serif;
    min-height: 100vh;
    overflow-x: hidden;
  }

  .noise {
    position: fixed; inset: 0; pointer-events: none; z-index: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");
    opacity: 0.6;
  }

  .grid-bg {
    position: fixed; inset: 0; pointer-events: none; z-index: 0;
    background-image: linear-gradient(rgba(0,245,255,0.03) 1px, transparent 1px), linear-gradient(90deg, rgba(0,245,255,0.03) 1px, transparent 1px);
    background-size: 48px 48px;
  }

  .glow-orb {
    position: fixed; border-radius: 50%; pointer-events: none; z-index: 0;
    filter: blur(80px);
  }
  .orb1 { width: 400px; height: 400px; background: rgba(0,245,255,0.06); top: -100px; right: -100px; }
  .orb2 { width: 300px; height: 300px; background: rgba(100,0,255,0.05); bottom: 200px; left: -80px; }

  .container {
    position: relative; z-index: 1;
    max-width: 780px; margin: 0 auto;
    padding: 48px 24px 80px;
  }

  /* HERO */
  .hero {
    display: grid; grid-template-columns: 1fr auto;
    align-items: center; gap: 32px; margin-bottom: 64px;
  }

  .hero-label {
    font-family: 'Space Mono', monospace;
    font-size: 11px; letter-spacing: 3px;
    color: #00F5FF; text-transform: uppercase;
    margin-bottom: 12px;
    opacity: 0; animation: fadeUp 0.6s 0.1s forwards;
  }

  .hero-name {
    font-family: 'Syne', sans-serif;
    font-size: clamp(42px, 7vw, 68px);
    font-weight: 800; line-height: 1;
    color: #F0F4FF;
    letter-spacing: -2px;
    opacity: 0; animation: fadeUp 0.7s 0.2s forwards;
  }

  .hero-name span { color: #00F5FF; }

  .hero-role {
    font-size: 15px; font-weight: 300;
    color: #8A9BB5; line-height: 1.6;
    margin-top: 16px;
    opacity: 0; animation: fadeUp 0.7s 0.35s forwards;
  }

  .hero-links {
    display: flex; gap: 12px; margin-top: 28px;
    opacity: 0; animation: fadeUp 0.7s 0.5s forwards;
  }

  .btn {
    display: inline-flex; align-items: center; gap: 8px;
    padding: 10px 20px; border-radius: 6px;
    font-size: 13px; font-weight: 500; text-decoration: none;
    transition: all 0.2s; cursor: pointer; border: none;
    font-family: 'Inter', sans-serif;
  }

  .btn-primary {
    background: #00F5FF; color: #080B10;
  }
  .btn-primary:hover { background: #00D4E0; transform: translateY(-1px); }

  .btn-outline {
    background: transparent; color: #00F5FF;
    border: 1px solid rgba(0,245,255,0.35);
  }
  .btn-outline:hover { background: rgba(0,245,255,0.08); transform: translateY(-1px); }

  .avatar-wrap {
    position: relative;
    opacity: 0; animation: fadeIn 0.8s 0.3s forwards;
  }

  .avatar {
    width: 120px; height: 120px; border-radius: 16px;
    background: linear-gradient(135deg, #0D1B2A 0%, #1A2F45 100%);
    border: 1px solid rgba(0,245,255,0.2);
    display: flex; align-items: center; justify-content: center;
    font-family: 'Syne', sans-serif;
    font-size: 36px; font-weight: 800; color: #00F5FF;
    position: relative; overflow: hidden;
  }

  .avatar::before {
    content: ''; position: absolute; inset: 0;
    background: linear-gradient(135deg, rgba(0,245,255,0.06), rgba(100,0,255,0.06));
  }

  .avatar-ring {
    position: absolute; inset: -6px; border-radius: 20px;
    border: 1px solid rgba(0,245,255,0.15);
  }

  /* STATUS */
  .status-bar {
    display: flex; align-items: center; gap: 8px;
    padding: 10px 16px; border-radius: 8px;
    background: rgba(0,245,255,0.05);
    border: 1px solid rgba(0,245,255,0.12);
    margin-bottom: 48px; width: fit-content;
    opacity: 0; animation: fadeUp 0.7s 0.6s forwards;
  }

  .status-dot {
    width: 7px; height: 7px; border-radius: 50%;
    background: #00F5FF;
    box-shadow: 0 0 0 0 rgba(0,245,255,0.4);
    animation: pulse 2s infinite;
  }

  .status-text {
    font-family: 'Space Mono', monospace;
    font-size: 12px; color: #8A9BB5;
    letter-spacing: 0.5px;
  }

  .status-text span { color: #00F5FF; }

  /* SECTION */
  .section { margin-bottom: 56px; }

  .section-header {
    display: flex; align-items: center; gap: 16px;
    margin-bottom: 24px;
  }

  .section-num {
    font-family: 'Space Mono', monospace;
    font-size: 11px; color: #00F5FF; letter-spacing: 2px;
  }

  .section-title {
    font-family: 'Syne', sans-serif;
    font-size: 20px; font-weight: 700;
    color: #F0F4FF; letter-spacing: -0.5px;
  }

  .section-line {
    flex: 1; height: 1px;
    background: linear-gradient(90deg, rgba(0,245,255,0.2), transparent);
  }

  /* SKILLS GRID */
  .skills-grid {
    display: grid; grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
    gap: 10px;
  }

  .skill-tag {
    padding: 10px 14px; border-radius: 8px;
    background: rgba(255,255,255,0.03);
    border: 1px solid rgba(255,255,255,0.07);
    font-size: 13px; color: #C5D0E0;
    font-family: 'Space Mono', monospace;
    transition: all 0.2s;
    display: flex; align-items: center; gap: 8px;
  }

  .skill-tag:hover {
    background: rgba(0,245,255,0.07);
    border-color: rgba(0,245,255,0.25);
    color: #00F5FF; transform: translateY(-2px);
  }

  .skill-dot { width: 5px; height: 5px; border-radius: 50%; background: #00F5FF; flex-shrink: 0; }
  .skill-dot.purple { background: #8B5CF6; }
  .skill-dot.blue { background: #3B82F6; }
  .skill-dot.green { background: #22C55E; }
  .skill-dot.orange { background: #F59E0B; }

  /* PROJECTS */
  .projects-grid {
    display: grid; grid-template-columns: 1fr 1fr;
    gap: 12px;
  }

  .project-card {
    padding: 20px; border-radius: 12px;
    background: rgba(255,255,255,0.02);
    border: 1px solid rgba(255,255,255,0.07);
    transition: all 0.25s; cursor: default;
    position: relative; overflow: hidden;
  }

  .project-card::before {
    content: ''; position: absolute;
    top: 0; left: 0; right: 0; height: 2px;
    background: linear-gradient(90deg, #00F5FF, transparent);
    opacity: 0; transition: opacity 0.25s;
  }

  .project-card:hover {
    background: rgba(0,245,255,0.04);
    border-color: rgba(0,245,255,0.2);
    transform: translateY(-3px);
  }

  .project-card:hover::before { opacity: 1; }

  .project-icon {
    font-size: 22px; margin-bottom: 12px;
    display: block;
  }

  .project-name {
    font-family: 'Syne', sans-serif;
    font-size: 15px; font-weight: 700;
    color: #F0F4FF; margin-bottom: 8px;
  }

  .project-desc {
    font-size: 12px; color: #6B7C95;
    line-height: 1.6; margin-bottom: 14px;
  }

  .project-tags {
    display: flex; flex-wrap: wrap; gap: 6px;
  }

  .tag {
    font-family: 'Space Mono', monospace;
    font-size: 10px; padding: 3px 8px; border-radius: 4px;
    background: rgba(0,245,255,0.08);
    color: #00C5CC; border: 1px solid rgba(0,245,255,0.15);
  }

  .tag.purple-tag {
    background: rgba(139,92,246,0.1);
    color: #A78BFA; border-color: rgba(139,92,246,0.2);
  }

  /* ABOUT */
  .code-block {
    padding: 24px; border-radius: 12px;
    background: rgba(255,255,255,0.02);
    border: 1px solid rgba(255,255,255,0.07);
    font-family: 'Space Mono', monospace;
    font-size: 13px; line-height: 1.9;
  }

  .code-line { display: flex; gap: 20px; }
  .code-num { color: rgba(255,255,255,0.18); user-select: none; min-width: 20px; }
  .code-key { color: #8B5CF6; }
  .code-str { color: #00F5FF; }
  .code-val { color: #E8EAF0; }
  .code-comment { color: rgba(255,255,255,0.25); }
  .code-link { color: #00F5FF; text-decoration: none; }
  .code-link:hover { text-decoration: underline; }

  /* CONNECT */
  .connect-grid {
    display: grid; grid-template-columns: 1fr 1fr;
    gap: 12px;
  }

  .connect-card {
    display: flex; align-items: center; gap: 14px;
    padding: 18px 20px; border-radius: 10px;
    background: rgba(255,255,255,0.02);
    border: 1px solid rgba(255,255,255,0.07);
    text-decoration: none; transition: all 0.2s;
  }

  .connect-card:hover {
    background: rgba(0,245,255,0.05);
    border-color: rgba(0,245,255,0.2);
    transform: translateY(-2px);
  }

  .connect-icon {
    width: 40px; height: 40px; border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    font-size: 18px; flex-shrink: 0;
  }

  .icon-gh { background: rgba(255,255,255,0.06); }
  .icon-li { background: rgba(0,119,181,0.12); }
  .icon-mail { background: rgba(0,245,255,0.08); }

  .connect-label {
    font-family: 'Syne', sans-serif;
    font-size: 14px; font-weight: 600; color: #F0F4FF;
  }

  .connect-val {
    font-size: 12px; color: #6B7C95; margin-top: 2px;
    word-break: break-all;
  }

  /* CURRENTLY BUILDING */
  .building-bar {
    display: flex; align-items: center; justify-content: space-between;
    padding: 16px 20px; border-radius: 10px;
    background: rgba(0,245,255,0.04);
    border: 1px solid rgba(0,245,255,0.12);
    margin-bottom: 10px;
  }

  .building-left { display: flex; align-items: center; gap: 12px; }

  .building-label {
    font-family: 'Space Mono', monospace;
    font-size: 12px; color: #8A9BB5;
  }

  .building-name {
    font-family: 'Syne', sans-serif;
    font-size: 14px; font-weight: 700; color: #F0F4FF;
  }

  .progress-wrap { width: 120px; }
  .progress-label {
    font-family: 'Space Mono', monospace;
    font-size: 10px; color: #8A9BB5; text-align: right; margin-bottom: 4px;
  }
  .progress-bar {
    height: 3px; border-radius: 2px;
    background: rgba(255,255,255,0.08);
    overflow: hidden;
  }
  .progress-fill {
    height: 100%; border-radius: 2px;
    background: #00F5FF;
    animation: grow 1.2s ease-out forwards;
    transform-origin: left;
  }

  /* FOOTER */
  .footer {
    border-top: 1px solid rgba(255,255,255,0.06);
    padding-top: 32px; margin-top: 16px;
    display: flex; align-items: center; justify-content: space-between;
  }

  .footer-left {
    font-family: 'Space Mono', monospace;
    font-size: 11px; color: rgba(255,255,255,0.2);
    letter-spacing: 1px;
  }

  .footer-badge {
    font-family: 'Space Mono', monospace;
    font-size: 10px; padding: 5px 12px; border-radius: 20px;
    background: rgba(0,245,255,0.07);
    border: 1px solid rgba(0,245,255,0.15);
    color: #00F5FF; letter-spacing: 1px;
  }

  /* ANIMATIONS */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(16px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  @keyframes fadeIn {
    from { opacity: 0; } to { opacity: 1; }
  }
  @keyframes pulse {
    0%   { box-shadow: 0 0 0 0 rgba(0,245,255,0.4); }
    70%  { box-shadow: 0 0 0 6px rgba(0,245,255,0); }
    100% { box-shadow: 0 0 0 0 rgba(0,245,255,0); }
  }
  @keyframes grow {
    from { width: 0; } to { width: var(--w); }
  }

  .section { opacity: 0; animation: fadeUp 0.7s forwards; }
  .s1 { animation-delay: 0.7s; }
  .s2 { animation-delay: 0.85s; }
  .s3 { animation-delay: 1s; }
  .s4 { animation-delay: 1.15s; }
  .s5 { animation-delay: 1.3s; }

  @media (max-width: 600px) {
    .hero { grid-template-columns: 1fr; }
    .avatar-wrap { display: none; }
    .projects-grid { grid-template-columns: 1fr; }
    .connect-grid { grid-template-columns: 1fr; }
  }
</style>
</head>
<body>
<div class="noise"></div>
<div class="grid-bg"></div>
<div class="glow-orb orb1"></div>
<div class="glow-orb orb2"></div>

<div class="container">

  <!-- HERO -->
  <div class="hero">
    <div>
      <p class="hero-label">// portfolio.init()</p>
      <h1 class="hero-name">Utkarsh<br><span>Singh</span></h1>
      <p class="hero-role">Java Backend Developer &nbsp;·&nbsp; DSA Enthusiast<br>Building scalable systems, one commit at a time.</p>
      <div class="hero-links">
        <a href="https://github.com/utkarsh1303803" class="btn btn-primary">GitHub ↗</a>
        <a href="https://www.linkedin.com/in/utkarsh-singh-code/" class="btn btn-outline">LinkedIn ↗</a>
      </div>
    </div>
    <div class="avatar-wrap">
      <div class="avatar-ring"></div>
      <div class="avatar">US</div>
    </div>
  </div>

  <!-- STATUS -->
  <div class="status-bar">
    <div class="status-dot"></div>
    <p class="status-text">currently learning <span>Spring Boot + Docker</span> &nbsp;·&nbsp; open to opportunities</p>
  </div>

  <!-- ABOUT -->
  <div class="section s1">
    <div class="section-header">
      <span class="section-num">01</span>
      <h2 class="section-title">about.yaml</h2>
      <div class="section-line"></div>
    </div>
    <div class="code-block">
      <div class="code-line"><span class="code-num">1</span><span class="code-key">name:</span>&nbsp;<span class="code-str">"Utkarsh Singh"</span></div>
      <div class="code-line"><span class="code-num">2</span><span class="code-key">role:</span>&nbsp;<span class="code-str">"Java Backend Developer"</span></div>
      <div class="code-line"><span class="code-num">3</span><span class="code-key">location:</span>&nbsp;<span class="code-str">"India 🇮🇳"</span></div>
      <div class="code-line"><span class="code-num">4</span><span class="code-key">email:</span>&nbsp;<a href="mailto:singhutkarsh3637@gmail.com" class="code-link code-str">"singhutkarsh3637@gmail.com"</a></div>
      <div class="code-line"><span class="code-num">5</span><span class="code-key">linkedin:</span>&nbsp;<a href="https://www.linkedin.com/in/utkarsh-singh-code/" class="code-link code-str">"utkarsh-singh-code"</a></div>
      <div class="code-line"><span class="code-num">6</span><span class="code-key">mindset:</span>&nbsp;<span class="code-val">"consistency &gt; motivation"</span></div>
      <div class="code-line"><span class="code-num">7</span><span class="code-key">focus:</span>&nbsp;<span class="code-val">["Backend Dev", "DSA", "System Design"]</span></div>
      <div class="code-line"><span class="code-num">8</span><span class="code-comment">// life_cycle: code → debug → coffee → repeat</span></div>
    </div>
  </div>

  <!-- SKILLS -->
  <div class="section s2">
    <div class="section-header">
      <span class="section-num">02</span>
      <h2 class="section-title">tech stack</h2>
      <div class="section-line"></div>
    </div>
    <div class="skills-grid">
      <div class="skill-tag"><div class="skill-dot"></div>Core Java</div>
      <div class="skill-tag"><div class="skill-dot"></div>Advanced Java</div>
      <div class="skill-tag"><div class="skill-dot purple"></div>Spring Boot</div>
      <div class="skill-tag"><div class="skill-dot purple"></div>Spring MVC</div>
      <div class="skill-tag"><div class="skill-dot purple"></div>Hibernate</div>
      <div class="skill-tag"><div class="skill-dot blue"></div>JDBC</div>
      <div class="skill-tag"><div class="skill-dot blue"></div>MySQL</div>
      <div class="skill-tag"><div class="skill-dot green"></div>Docker</div>
      <div class="skill-tag"><div class="skill-dot"></div>Multithreading</div>
      <div class="skill-tag"><div class="skill-dot orange"></div>DSA</div>
      <div class="skill-tag"><div class="skill-dot blue"></div>Git / GitHub</div>
      <div class="skill-tag"><div class="skill-dot purple"></div>REST APIs</div>
      <div class="skill-tag"><div class="skill-dot"></div>Collections</div>
      <div class="skill-tag"><div class="skill-dot green"></div>C / C++</div>
    </div>
  </div>

  <!-- PROJECTS -->
  <div class="section s3">
    <div class="section-header">
      <span class="section-num">03</span>
      <h2 class="section-title">featured projects</h2>
      <div class="section-line"></div>
    </div>
    <div class="projects-grid">

      <div class="project-card">
        <span class="project-icon">🔐</span>
        <div class="project-name">SecureSphere</div>
        <p class="project-desc">Advanced auth system with secure access control, OOP architecture and full DB integration.</p>
        <div class="project-tags">
          <span class="tag">Java</span>
          <span class="tag">MySQL</span>
          <span class="tag">JDBC</span>
        </div>
      </div>

      <div class="project-card">
        <span class="project-icon">🚆</span>
        <div class="project-name">Railway Management</div>
        <p class="project-desc">Full reservation system with ticket booking, admin dashboard and database integration.</p>
        <div class="project-tags">
          <span class="tag">Java</span>
          <span class="tag">MySQL</span>
          <span class="tag">JDBC</span>
        </div>
      </div>

      <div class="project-card">
        <span class="project-icon">🌱</span>
        <div class="project-name">Spring Boot APIs</div>
        <p class="project-desc">Production-grade REST APIs with layered backend, Hibernate ORM and DB connectivity.</p>
        <div class="project-tags">
          <span class="tag purple-tag">Spring Boot</span>
          <span class="tag purple-tag">Hibernate</span>
          <span class="tag">MySQL</span>
        </div>
      </div>

      <div class="project-card">
        <span class="project-icon">🧠</span>
        <div class="project-name">Multithreading Lab</div>
        <p class="project-desc">Java concurrency deep-dive: executor service, thread lifecycle, producer-consumer, sync.</p>
        <div class="project-tags">
          <span class="tag">Java</span>
          <span class="tag purple-tag">Concurrency</span>
        </div>
      </div>

      <div class="project-card">
        <span class="project-icon">📚</span>
        <div class="project-name">DSA Solutions</div>
        <p class="project-desc">Optimized solutions: arrays, sliding window, heaps, graphs, linked lists.</p>
        <div class="project-tags">
          <span class="tag">Java</span>
          <span class="tag purple-tag">Algorithms</span>
        </div>
      </div>

      <div class="project-card">
        <span class="project-icon">⚙️</span>
        <div class="project-name">JDBC CRUD System</div>
        <p class="project-desc">Clean DB management: CRUD ops, prepared statements, and exception handling.</p>
        <div class="project-tags">
          <span class="tag">Java</span>
          <span class="tag">JDBC</span>
          <span class="tag">MySQL</span>
        </div>
      </div>

    </div>
  </div>

  <!-- CURRENTLY -->
  <div class="section s4">
    <div class="section-header">
      <span class="section-num">04</span>
      <h2 class="section-title">currently building</h2>
      <div class="section-line"></div>
    </div>

    <div class="building-bar">
      <div class="building-left">
        <span style="font-size:18px;">🚢</span>
        <div>
          <p class="building-label">learning</p>
          <p class="building-name">Spring Boot + Microservices</p>
        </div>
      </div>
      <div class="progress-wrap">
        <p class="progress-label">70%</p>
        <div class="progress-bar"><div class="progress-fill" style="--w:70%"></div></div>
      </div>
    </div>

    <div class="building-bar">
      <div class="building-left">
        <span style="font-size:18px;">🐳</span>
        <div>
          <p class="building-label">exploring</p>
          <p class="building-name">Docker + Containerization</p>
        </div>
      </div>
      <div class="progress-wrap">
        <p class="progress-label">50%</p>
        <div class="progress-bar"><div class="progress-fill" style="--w:50%"></div></div>
      </div>
    </div>

    <div class="building-bar">
      <div class="building-left">
        <span style="font-size:18px;">🧩</span>
        <div>
          <p class="building-label">practicing</p>
          <p class="building-name">Advanced DSA Daily</p>
        </div>
      </div>
      <div class="progress-wrap">
        <p class="progress-label">ongoing</p>
        <div class="progress-bar"><div class="progress-fill" style="--w:90%"></div></div>
      </div>
    </div>

  </div>

  <!-- CONNECT -->
  <div class="section s5">
    <div class="section-header">
      <span class="section-num">05</span>
      <h2 class="section-title">connect</h2>
      <div class="section-line"></div>
    </div>
    <div class="connect-grid">
      <a href="https://github.com/utkarsh1303803" class="connect-card">
        <div class="connect-icon icon-gh">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor" style="color:#E8EAF0"><path d="M12 0C5.37 0 0 5.37 0 12c0 5.3 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61-.546-1.385-1.335-1.755-1.335-1.755-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.605-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 21.795 24 17.295 24 12c0-6.63-5.37-12-12-12"/></svg>
        </div>
        <div>
          <p class="connect-label">GitHub</p>
          <p class="connect-val">utkarsh1303803</p>
        </div>
      </a>

      <a href="https://www.linkedin.com/in/utkarsh-singh-code/" class="connect-card">
        <div class="connect-icon icon-li">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="#0A66C2"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 01-2.063-2.065 2.064 2.064 0 112.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>
        </div>
        <div>
          <p class="connect-label">LinkedIn</p>
          <p class="connect-val">utkarsh-singh-code</p>
        </div>
      </a>

      <a href="mailto:singhutkarsh3637@gmail.com" class="connect-card">
        <div class="connect-icon icon-mail">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#00F5FF" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/></svg>
        </div>
        <div>
          <p class="connect-label">Email</p>
          <p class="connect-val">singhutkarsh3637@gmail.com</p>
        </div>
      </a>

      <a href="https://github.com/utkarsh1303803" class="connect-card">
        <div class="connect-icon icon-gh">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#F59E0B" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="16 18 22 12 16 6"/><polyline points="8 6 2 12 8 18"/></svg>
        </div>
        <div>
          <p class="connect-label">LeetCode</p>
          <p class="connect-val">Daily problem solver</p>
        </div>
      </a>
    </div>
  </div>

  <!-- FOOTER -->
  <div class="footer">
    <p class="footer-left">UTKARSH.SINGH © 2025</p>
    <span class="footer-badge">OPEN TO WORK</span>
  </div>

</div>
</body>
</html>
