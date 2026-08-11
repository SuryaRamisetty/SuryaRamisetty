<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Ramisetty T M Surya — AI/ML Engineer &amp; Security Researcher</title>
<meta name="description" content="Portfolio of Ramisetty T M Surya — AI/ML Engineer, Security Researcher, Final-Year CSE Student at SRM Chennai." />
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600;700;800&family=Manrope:wght@400;500;600;700;800&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<style>
  :root{
    --bg: #0a1626;
    --bg-2: #0e1f36;
    --grid-line: rgba(94,234,212,0.055);
    --grid-line-strong: rgba(94,234,212,0.11);
    --ink: #e9eef6;
    --ink-dim: #8ea3c2;
    --ink-faint: #5b6d8a;
    --cyan: #5eead4;
    --cyan-dim: rgba(94,234,212,0.35);
    --amber: #f5a623;
    --panel: rgba(255,255,255,0.025);
    --panel-strong: rgba(255,255,255,0.05);
    --line: rgba(94,234,212,0.22);
    --line-soft: rgba(255,255,255,0.08);
    --radius: 3px;
    --mono: 'JetBrains Mono', monospace;
    --sans: 'Manrope', sans-serif;
  }

  *{ box-sizing:border-box; margin:0; padding:0; }
  html{ scroll-behavior:smooth; }
  html,body{ background:var(--bg); }

  @media (prefers-reduced-motion: reduce){
    html{ scroll-behavior:auto; }
    *{ animation-duration:0.01ms !important; animation-iteration-count:1 !important; transition-duration:0.01ms !important; }
  }

  body{
    font-family:var(--sans);
    color:var(--ink);
    line-height:1.6;
    overflow-x:hidden;
    position:relative;
  }

  /* Blueprint grid backdrop */
  .grid-backdrop{
    position:fixed; inset:0; z-index:0; pointer-events:none;
    background-image:
      linear-gradient(var(--grid-line) 1px, transparent 1px),
      linear-gradient(90deg, var(--grid-line) 1px, transparent 1px),
      linear-gradient(var(--grid-line-strong) 1px, transparent 1px),
      linear-gradient(90deg, var(--grid-line-strong) 1px, transparent 1px);
    background-size: 24px 24px, 24px 24px, 120px 120px, 120px 120px;
    background-position: center;
  }
  .grid-backdrop::after{
    content:'';
    position:absolute; inset:0;
    background: radial-gradient(ellipse 80% 60% at 50% 0%, rgba(94,234,212,0.06), transparent 60%),
                radial-gradient(ellipse 60% 50% at 100% 100%, rgba(245,166,35,0.05), transparent 60%),
                linear-gradient(180deg, var(--bg) 0%, var(--bg-2) 100%);
  }

  /* Live network canvas — sits above grid, behind content */
  #netCanvas{ position:fixed; inset:0; z-index:1; pointer-events:none; opacity:0.8; }

  a{ color:inherit; text-decoration:none; }
  ::selection{ background:var(--cyan); color:#04141c; }

  .wrap{ max-width:1180px; margin:0 auto; padding:0 32px; position:relative; z-index:2; }

  /* ===== Side rail nav (schematic status panel) ===== */
  .rail{
    position:fixed; left:28px; top:50%; transform:translateY(-50%);
    z-index:50; display:flex; flex-direction:column; gap:22px;
    font-family:var(--mono); font-size:10px;
  }
  .rail-item{ display:flex; align-items:center; gap:10px; cursor:pointer; opacity:0.5; transition:opacity .3s ease; }
  .rail-item.active{ opacity:1; }
  .rail-dot{ width:8px; height:8px; border-radius:50%; border:1px solid var(--cyan-dim); position:relative; flex-shrink:0; transition:all .3s ease; }
  .rail-item.active .rail-dot{ background:var(--cyan); box-shadow:0 0 8px var(--cyan); border-color:var(--cyan); }
  .rail-label{ color:var(--ink-dim); letter-spacing:.08em; text-transform:uppercase; white-space:nowrap; transition:color .3s; }
  .rail-item.active .rail-label{ color:var(--cyan); }
  .rail-line{ position:absolute; left:3px; top:12px; bottom:-22px; width:1px; background:linear-gradient(var(--line), transparent); }
  .rail-item:last-child .rail-line{ display:none; }
  @media (max-width: 900px){ .rail{ display:none; } }

  /* ===== Corner frame marks (blueprint sheet feel) ===== */
  .frame-mark{ position:fixed; z-index:40; font-family:var(--mono); font-size:9px; color:var(--ink-faint); letter-spacing:.1em; pointer-events:none; }
  .frame-mark.tl{ top:20px; left:20px; }
  .frame-mark.tr{ top:20px; right:20px; text-align:right; }
  .frame-mark.br{ bottom:20px; right:20px; text-align:right; }
  @media (max-width: 700px){ .frame-mark{ display:none; } }

  /* ===== Header / Hero ===== */
  header.hero{ min-height:100svh; display:flex; flex-direction:column; justify-content:center; position:relative; padding-top:80px; padding-bottom:60px; }

  .eyebrow{ font-family:var(--mono); font-size:12px; color:var(--cyan); letter-spacing:.18em; text-transform:uppercase; display:flex; align-items:center; gap:10px; margin-bottom:28px; opacity:0; animation:fadeUp .7s ease .1s forwards; }
  .eyebrow::before{ content:''; width:26px; height:1px; background:var(--cyan); }

  .hero-name{
    font-family:var(--sans); font-weight:800;
    font-size:clamp(1.1rem, 2.2vw, 1.5rem);
    letter-spacing:.02em; color:var(--ink);
    margin-bottom:14px; opacity:0; animation:fadeUp .7s ease 0s forwards;
    display:flex; align-items:center; gap:14px;
  }
  .hero-name .rule{ width:44px; height:2px; background:var(--amber); }
  .hero-name .handle{ font-family:var(--mono); font-size:12px; color:var(--ink-faint); font-weight:400; letter-spacing:.04em; }

  h1.title{
    font-family:var(--mono); font-weight:800;
    font-size:clamp(2.4rem, 6.4vw, 5.4rem);
    line-height:1.03; letter-spacing:-.02em;
    color:var(--ink); margin-bottom:26px;
    min-height:1.1em;
  }
  h1.title .cursor-blink{ display:inline-block; width:0.5ch; background:var(--cyan); animation:blink 1s steps(1) infinite; }
  @keyframes blink{ 50%{ opacity:0; } }

  .hero-sub{ max-width:640px; font-size:clamp(1rem,1.6vw,1.15rem); color:var(--ink-dim); opacity:0; animation:fadeUp .7s ease .9s forwards; margin-bottom:38px; }
  .hero-sub strong{ color:var(--ink); font-weight:600; }

  .hero-meta{ display:flex; flex-wrap:wrap; gap:14px 34px; font-family:var(--mono); font-size:12px; color:var(--ink-faint); opacity:0; animation:fadeUp .7s ease 1.05s forwards; margin-bottom:42px; }
  .hero-meta span{ display:flex; align-items:center; gap:8px; }
  .hero-meta b{ color:var(--cyan); font-weight:600; }

  .hero-cta{ display:flex; flex-wrap:wrap; gap:16px; opacity:0; animation:fadeUp .7s ease 1.2s forwards; margin-bottom:46px; }

  .icon-strip{ opacity:0; animation:fadeUp .7s ease 1.4s forwards; }
  .icon-strip img{ max-width:520px; width:100%; filter:saturate(0.85) brightness(0.95); opacity:.9; }
  .btn{
    font-family:var(--mono); font-size:12.5px; letter-spacing:.06em; text-transform:uppercase;
    padding:14px 26px; border-radius:var(--radius); cursor:pointer; transition:all .25s ease;
    display:inline-flex; align-items:center; gap:10px; border:1px solid var(--line);
  }
  .btn-primary{ background:var(--cyan); color:#04141c; font-weight:700; border-color:var(--cyan); }
  .btn-primary:hover{ background:#7ff2df; box-shadow:0 0 26px rgba(94,234,212,0.35); transform:translateY(-2px); }
  .btn-ghost{ color:var(--ink); background:transparent; }
  .btn-ghost:hover{ border-color:var(--cyan); color:var(--cyan); background:rgba(94,234,212,0.06); }

  @keyframes fadeUp{ from{ opacity:0; transform:translateY(14px); } to{ opacity:1; transform:translateY(0); } }

  .hero-scan{ position:absolute; right:0; top:0; bottom:0; width:38%; opacity:.5; pointer-events:none; display:flex; align-items:center; justify-content:center; }
  @media (max-width: 900px){ .hero-scan{ display:none; } }

  .scroll-cue{ position:absolute; bottom:34px; left:32px; font-family:var(--mono); font-size:10px; color:var(--ink-faint); letter-spacing:.15em; display:flex; align-items:center; gap:10px; }
  .scroll-cue .bar{ width:1px; height:34px; background:linear-gradient(var(--cyan), transparent); animation:scanDown 2s ease-in-out infinite; }
  @keyframes scanDown{ 0%{ transform:scaleY(0); transform-origin:top; } 50%{ transform:scaleY(1); transform-origin:top; } 50.01%{ transform-origin:bottom; } 100%{ transform:scaleY(0); transform-origin:bottom; } }

  /* ===== Section framework ===== */
  section{ padding:150px 0; position:relative; border-top:1px solid var(--line-soft); }
  section:first-of-type{ border-top:none; }

  .sec-head{ display:flex; align-items:baseline; gap:18px; margin-bottom:64px; }
  .sec-code{ font-family:var(--mono); font-size:12px; color:var(--amber); letter-spacing:.1em; }
  .sec-title{ font-family:var(--mono); font-size:clamp(1.6rem,3vw,2.3rem); font-weight:700; color:var(--ink); letter-spacing:-.01em; }
  .sec-rule{ flex:1; height:1px; background:linear-gradient(90deg, var(--line), transparent); }

  .reveal{ opacity:0; transform:translateY(28px); transition:opacity .8s cubic-bezier(.2,.7,.2,1), transform .8s cubic-bezier(.2,.7,.2,1); }
  .reveal.in{ opacity:1; transform:translateY(0); }

  /* ===== About ===== */
  .about-grid{ display:grid; grid-template-columns: 1.3fr 1fr; gap:60px; }
  @media (max-width:850px){ .about-grid{ grid-template-columns:1fr; } }
  .about-text p{ color:var(--ink-dim); font-size:1.02rem; margin-bottom:18px; max-width:60ch; }
  .about-text b{ color:var(--ink); font-weight:700; }

  .spec-sheet{ border:1px solid var(--line-soft); border-radius:var(--radius); padding:26px 28px; background:var(--panel); font-family:var(--mono); font-size:12.5px; }
  .spec-row{ display:flex; justify-content:space-between; gap:20px; padding:11px 0; border-bottom:1px dashed var(--line-soft); color:var(--ink-dim); }
  .spec-row:last-child{ border-bottom:none; }
  .spec-row b{ color:var(--cyan); font-weight:600; text-align:right; }

  /* ===== Timeline (experience) ===== */
  .tl{ position:relative; padding-left:34px; }
  .tl::before{ content:''; position:absolute; left:5px; top:6px; bottom:6px; width:1px; background:linear-gradient(var(--line), var(--line-soft)); }
  .tl-item{ position:relative; padding-bottom:52px; }
  .tl-item:last-child{ padding-bottom:0; }
  .tl-node{ position:absolute; left:-34px; top:4px; width:11px; height:11px; border-radius:50%; background:var(--bg); border:2px solid var(--cyan); box-shadow:0 0 0 4px rgba(94,234,212,0.08); }
  .tl-date{ font-family:var(--mono); font-size:11.5px; color:var(--amber); letter-spacing:.06em; margin-bottom:6px; display:block; }
  .tl-role{ font-size:1.15rem; font-weight:700; color:var(--ink); margin-bottom:4px; }
  .tl-org{ font-family:var(--mono); font-size:12.5px; color:var(--ink-faint); margin-bottom:12px; }
  .tl-item ul{ list-style:none; color:var(--ink-dim); font-size:.95rem; max-width:62ch; }
  .tl-item li{ position:relative; padding-left:18px; margin-bottom:6px; }
  .tl-item li::before{ content:'—'; position:absolute; left:0; color:var(--cyan-dim); }

  /* ===== Projects ===== */
  .proj-featured{ display:grid; gap:22px; margin-bottom:22px; }
  .proj-card{
    border:1px solid var(--line-soft); border-radius:var(--radius); background:var(--panel);
    padding:34px; position:relative; overflow:hidden; transition:border-color .3s ease, background .3s ease, transform .12s ease-out;
    transform-style:preserve-3d; will-change:transform;
  }
  .proj-card::before{ content:''; position:absolute; top:0; left:0; width:0; height:2px; background:var(--cyan); transition:width .4s ease; }
  .proj-card:hover{ border-color:var(--cyan-dim); background:var(--panel-strong); }
  .proj-card:hover::before{ width:100%; }
  .proj-top{ display:flex; justify-content:space-between; align-items:flex-start; gap:20px; margin-bottom:14px; flex-wrap:wrap; }
  .proj-name{ font-size:1.3rem; font-weight:800; color:var(--ink); }
  .proj-tag{ font-family:var(--mono); font-size:10.5px; color:var(--amber); border:1px solid rgba(245,166,35,0.35); padding:4px 10px; border-radius:20px; letter-spacing:.06em; white-space:nowrap; }
  .proj-desc{ color:var(--ink-dim); font-size:.97rem; max-width:70ch; margin-bottom:16px; }
  .proj-stack{ display:flex; flex-wrap:wrap; gap:8px; }
  .chip{ font-family:var(--mono); font-size:10.5px; color:var(--ink-dim); border:1px solid var(--line-soft); padding:5px 10px; border-radius:20px; }
  .proj-meta{ font-family:var(--mono); font-size:11px; color:var(--ink-faint); margin-top:14px; }

  .proj-grid{ display:grid; grid-template-columns:repeat(auto-fill, minmax(250px,1fr)); gap:16px; }
  .mini-card{ border:1px solid var(--line-soft); border-radius:var(--radius); padding:20px; background:var(--panel); transition:all .3s ease, transform .12s ease-out; transform-style:preserve-3d; will-change:transform; }
  .mini-card:hover{ border-color:var(--cyan-dim); }
  .mini-name{ font-weight:700; color:var(--ink); margin-bottom:6px; font-size:.98rem; }
  .mini-desc{ color:var(--ink-faint); font-size:.85rem; }

  /* ===== Publications / Patent ===== */
  .doc-list{ display:grid; gap:16px; }
  .doc-item{ display:grid; grid-template-columns: 90px 1fr; gap:24px; border:1px solid var(--line-soft); border-radius:var(--radius); padding:24px 28px; background:var(--panel); align-items:start; }
  @media (max-width:600px){ .doc-item{ grid-template-columns:1fr; gap:10px; } }
  .doc-stamp{ font-family:var(--mono); font-size:10px; color:var(--cyan); border:1px solid var(--cyan-dim); border-radius:var(--radius); padding:6px 0; text-align:center; letter-spacing:.06em; height:fit-content; }
  .doc-title{ font-weight:700; color:var(--ink); margin-bottom:6px; }
  .doc-meta{ font-family:var(--mono); font-size:11.5px; color:var(--ink-faint); }

  /* ===== Skills ===== */
  .skill-groups{ display:grid; grid-template-columns:repeat(auto-fit, minmax(220px,1fr)); gap:34px; }
  .skill-group-title{ font-family:var(--mono); font-size:11.5px; color:var(--amber); letter-spacing:.1em; text-transform:uppercase; margin-bottom:16px; }
  .skill-tags{ display:flex; flex-wrap:wrap; gap:9px; }
  .skill-tags .chip{ color:var(--ink-dim); }
  .skill-tags .chip:hover{ border-color:var(--cyan-dim); color:var(--cyan); }

  /* ===== Certifications ===== */
  .cert-list{ columns:2; column-gap:40px; }
  @media (max-width:700px){ .cert-list{ columns:1; } }
  .cert-item{ break-inside:avoid; display:flex; gap:12px; padding:12px 0; border-bottom:1px dashed var(--line-soft); font-size:.93rem; color:var(--ink-dim); }
  .cert-item .tick{ color:var(--cyan); font-family:var(--mono); flex-shrink:0; }

  /* ===== Contact / Footer ===== */
  .contact-inner{ display:grid; grid-template-columns:1.1fr 1fr; gap:60px; align-items:center; }
  @media (max-width:850px){ .contact-inner{ grid-template-columns:1fr; } }
  .contact-title{ font-family:var(--mono); font-size:clamp(1.8rem,4vw,3rem); font-weight:800; color:var(--ink); line-height:1.15; margin-bottom:22px; }
  .contact-title span{ color:var(--cyan); }
  .contact-links{ display:grid; gap:12px; }
  .contact-link{ display:flex; align-items:center; justify-content:space-between; border:1px solid var(--line-soft); border-radius:var(--radius); padding:16px 20px; font-family:var(--mono); font-size:13px; color:var(--ink-dim); transition:all .25s ease; }
  .contact-link:hover{ border-color:var(--cyan-dim); color:var(--cyan); background:var(--panel); transform:translateX(4px); }
  .contact-link .arrow{ opacity:0; transition:opacity .25s; }
  .contact-link:hover .arrow{ opacity:1; }

  footer{ padding:40px 0 60px; text-align:center; font-family:var(--mono); font-size:11px; color:var(--ink-faint); letter-spacing:.05em; }
</style>
</head>
<body>

<div class="grid-backdrop"></div>
<canvas id="netCanvas"></canvas>

<div class="frame-mark tl">SHEET&nbsp;01/01</div>
<div class="frame-mark tr">SUBJ: T.M.&nbsp;SURYA<br/>REV: 2026.08</div>

<nav class="rail" id="rail">
  <div class="rail-item" data-target="hero"><div class="rail-line"></div><span class="rail-dot"></span><span class="rail-label">Init</span></div>
  <div class="rail-item" data-target="about"><div class="rail-line"></div><span class="rail-dot"></span><span class="rail-label">Profile</span></div>
  <div class="rail-item" data-target="experience"><div class="rail-line"></div><span class="rail-dot"></span><span class="rail-label">Training</span></div>
  <div class="rail-item" data-target="projects"><div class="rail-line"></div><span class="rail-dot"></span><span class="rail-label">Builds</span></div>
  <div class="rail-item" data-target="research"><div class="rail-line"></div><span class="rail-dot"></span><span class="rail-label">Research</span></div>
  <div class="rail-item" data-target="skills"><div class="rail-line"></div><span class="rail-dot"></span><span class="rail-label">Stack</span></div>
  <div class="rail-item" data-target="contact"><div class="rail-line"></div><span class="rail-dot"></span><span class="rail-label">Connect</span></div>
</nav>

<header class="hero" id="hero">
  <div class="wrap">
    <div class="eyebrow">System Online — Chennai, India</div>
    <div class="hero-name"><span class="rule"></span>RAMISETTY&nbsp;T&nbsp;M&nbsp;SURYA<span class="handle">@SuryaRamisetty</span></div>
    <h1 class="title" id="decryptTitle"><span class="cursor-blink">&nbsp;</span></h1>
    <p class="hero-sub">Final-year <strong>B.Tech CSE (AI/ML)</strong> undergraduate building threat-aware security models, applied ML systems, and full-stack platforms — backed by a filed patent and a peer-reviewed publication.</p>
    <div class="icon-strip" aria-hidden="true">
      <img src="https://skillicons.dev/icons?i=python,java,cpp,c,tensorflow,pytorch,flask,nodejs,react,mysql,postgres,mongodb,docker,git,linux" alt="" loading="lazy" />
    </div>
    <div class="hero-meta">
      <span>CGPA <b>9.0</b></span>
      <span>Patent <b>202641048590</b></span>
      <span>Publication <b>IJIREEICE '25</b></span>
      <span>Internships <b>03</b></span>
    </div>
    <div class="hero-cta">
      <a href="#projects" class="btn btn-primary">View builds →</a>
      <a href="https://drive.google.com/file/d/1DbZ0M1ykd1j2LOFJerMHyvnJVUj471CC/view?usp=sharing" target="_blank" rel="noopener" class="btn btn-ghost">Download resume</a>
      <a href="mailto:tmsuryaramisetty@gmail.com" class="btn btn-ghost">Email me</a>
    </div>
  </div>
  <div class="scroll-cue"><div class="bar"></div>SCROLL</div>
</header>

<main>

<section id="about">
  <div class="wrap">
    <div class="sec-head reveal"><span class="sec-code">FIG.&nbsp;01</span><span class="sec-title">Profile</span><span class="sec-rule"></span></div>
    <div class="about-grid">
      <div class="about-text reveal">
        <p>I'm <b>Ramisetty T M Surya</b>, a final-year Computer Science &amp; Engineering student specializing in AI/ML at SRM Institute of Science and Technology, Chennai. Over four months of hands-on training across three structured internships took me through the full ML lifecycle — <b>data preprocessing, model evaluation, and end-to-end deployment.</b></p>
        <p>My core interest sits at the intersection of <b>applied machine learning and data security</b> — work that led to a co-authored peer-reviewed publication and a filed Indian patent on an intelligent, threat-adaptive encryption framework.</p>
        <p>Outside coursework, I've shipped full-stack platforms, contributed to community outreach through NGO volunteering, and I'm currently deepening my footing in <b>system design and cloud deployment</b> ahead of graduating in 2027.</p>
      </div>
      <div class="spec-sheet reveal">
        <div class="spec-row"><span>Institution</span><b>SRM IST, Chennai</b></div>
        <div class="spec-row"><span>Programme</span><b>B.Tech CSE (AI/ML)</b></div>
        <div class="spec-row"><span>CGPA</span><b>9.0</b></div>
        <div class="spec-row"><span>Graduation</span><b>2027</b></div>
        <div class="spec-row"><span>Location</span><b>Chennai, India</b></div>
        <div class="spec-row"><span>Focus</span><b>ML · Security · Full-Stack</b></div>
        <div class="spec-row"><span>Status</span><b>Open to AI/ML roles</b></div>
      </div>
    </div>
  </div>
</section>

<section id="experience">
  <div class="wrap">
    <div class="sec-head reveal"><span class="sec-code">FIG.&nbsp;02</span><span class="sec-title">Training &amp; Internships</span><span class="sec-rule"></span></div>
    <div class="tl reveal">
      <div class="tl-item">
        <div class="tl-node"></div>
        <span class="tl-date">DEC 2025 — JAN 2026</span>
        <div class="tl-role">Machine Learning Intern</div>
        <div class="tl-org">SkillFied Mentor</div>
        <ul>
          <li>Trained in supervised and unsupervised learning through hands-on exercises</li>
          <li>Built data preprocessing, training, and evaluation pipelines under mentorship</li>
        </ul>
      </div>
      <div class="tl-item">
        <div class="tl-node"></div>
        <span class="tl-date">NOV 2025 — DEC 2025</span>
        <div class="tl-role">AI/ML Intern</div>
        <div class="tl-org">InternPe</div>
        <ul>
          <li>Completed applied AI/ML training covering core workflows and tools</li>
          <li>Strengthened analytical skills via case studies and project-based tasks</li>
        </ul>
      </div>
      <div class="tl-item">
        <div class="tl-node"></div>
        <span class="tl-date">DEC 2024 — FEB 2025</span>
        <div class="tl-role">Machine Learning Intern</div>
        <div class="tl-org">Mindenious</div>
        <ul>
          <li>Trained in Python-based supervised and unsupervised ML techniques</li>
          <li>Applied preprocessing, training, and evaluation on real-world datasets</li>
        </ul>
      </div>
    </div>
  </div>
</section>

<section id="projects">
  <div class="wrap">
    <div class="sec-head reveal"><span class="sec-code">FIG.&nbsp;03</span><span class="sec-title">Selected Builds</span><span class="sec-rule"></span></div>

    <div class="proj-featured">
      <div class="proj-card reveal">
        <div class="proj-top">
          <div class="proj-name">Intelligent Threat-Aware Mixed Encryption Model</div>
          <div class="proj-tag">Patent Filed</div>
        </div>
        <p class="proj-desc">A hybrid security framework combining AES, ECC, and BLAKE3 for secure data transfer, with IDS-inspired threat monitoring layered in to detect abnormal activity in real time. Guided by Prof. Neelam Sanjeev Kumar.</p>
        <div class="proj-stack"><span class="chip">AES</span><span class="chip">ECC</span><span class="chip">BLAKE3</span><span class="chip">IDS</span><span class="chip">Python</span></div>
        <div class="proj-meta">Jan – May 2026 · Indian Patent App. No. 202641048590</div>
      </div>

      <div class="proj-card reveal">
        <div class="proj-top">
          <div class="proj-name">AI-Enabled Software Bug Prediction System</div>
          <div class="proj-tag">ML + Flask</div>
        </div>
        <p class="proj-desc">An ML model that predicts software defects directly from code-quality metrics, wrapped in a Flask web app for automated bug-risk analysis. Guided by Prof. M. Rajvel.</p>
        <div class="proj-stack"><span class="chip">Python</span><span class="chip">scikit-learn</span><span class="chip">Flask</span></div>
        <div class="proj-meta">Jan – Apr 2026</div>
      </div>

      <div class="proj-card reveal">
        <div class="proj-top">
          <div class="proj-name">NetPrime — OTT Streaming Web Platform</div>
          <div class="proj-tag">Full-Stack UI</div>
        </div>
        <p class="proj-desc">A Netflix-inspired, multi-page streaming platform — login, home, movies, TV shows, categories, profile — with a glassmorphism design, category browsing, live keyword search, and dropdown/video-background interactions.</p>
        <div class="proj-stack"><span class="chip">HTML/CSS</span><span class="chip">JavaScript</span></div>
        <div class="proj-meta">2026</div>
      </div>
    </div>

    <div class="proj-grid reveal">
      <div class="mini-card"><div class="mini-name">Low-Light Image Enhancement</div><div class="mini-desc">Deep learning model for enhancing visibility and detail in low-light images.</div></div>
      <div class="mini-card"><div class="mini-name">AI Resume Analyzer</div><div class="mini-desc">ML-based resume ranking and recommendation system.</div></div>
      <div class="mini-card"><div class="mini-name">Crop Health &amp; Yield Prediction</div><div class="mini-desc">Satellite-data-driven crop yield prediction using machine learning.</div></div>
      <div class="mini-card"><div class="mini-name">Diabetes Prediction</div><div class="mini-desc">ML model for diabetes risk prediction on healthcare datasets.</div></div>
      <div class="mini-card"><div class="mini-name">Car Price Prediction</div><div class="mini-desc">Regression-based price estimation model.</div></div>
      <div class="mini-card"><div class="mini-name">IPL Winning Team Prediction</div><div class="mini-desc">Sports analytics and outcome prediction using machine learning.</div></div>
    </div>
  </div>
</section>

<section id="research">
  <div class="wrap">
    <div class="sec-head reveal"><span class="sec-code">FIG.&nbsp;04</span><span class="sec-title">Publications &amp; Patent</span><span class="sec-rule"></span></div>
    <div class="doc-list reveal">
      <div class="doc-item">
        <div class="doc-stamp">PATENT</div>
        <div>
          <div class="doc-title">Intelligent Threat-Aware Mixed Encryption Model for Privacy Preservation in Large-Scale Data Communication</div>
          <div class="doc-meta">Indian Patent Application No. 202641048590 · Filed April 2026</div>
        </div>
      </div>
      <div class="doc-item">
        <div class="doc-stamp">PUBLISHED</div>
        <div>
          <div class="doc-title">Advanced Machine Learning for Real-Time Driver Distraction Analysis with Visual Inputs</div>
          <div class="doc-meta">IJIREEICE, Vol. 13, Issue 10 · October 2025</div>
        </div>
      </div>
    </div>
  </div>
</section>

<section id="skills">
  <div class="wrap">
    <div class="sec-head reveal"><span class="sec-code">FIG.&nbsp;05</span><span class="sec-title">Stack &amp; Certifications</span><span class="sec-rule"></span></div>
    <div class="skill-groups reveal" style="margin-bottom:70px;">
      <div>
        <div class="skill-group-title">Languages</div>
        <div class="skill-tags"><span class="chip">Python</span><span class="chip">Java</span><span class="chip">C</span><span class="chip">C++</span><span class="chip">SQL</span></div>
      </div>
      <div>
        <div class="skill-group-title">Core Skills</div>
        <div class="skill-tags"><span class="chip">Machine Learning</span><span class="chip">Deep Learning</span><span class="chip">Data Analysis</span><span class="chip">Data Visualization</span></div>
      </div>
      <div>
        <div class="skill-group-title">Tools &amp; Platforms</div>
        <div class="skill-tags"><span class="chip">MySQL</span><span class="chip">PostgreSQL</span><span class="chip">MongoDB</span><span class="chip">Docker</span><span class="chip">Git / GitHub</span><span class="chip">Jupyter</span><span class="chip">Postman</span><span class="chip">LaTeX</span></div>
      </div>
    </div>

    <div class="cert-list reveal">
      <div class="cert-item"><span class="tick">✓</span>NPTEL Elite — Programming in Java (95%), IIT Kharagpur, 2026</div>
      <div class="cert-item"><span class="tick">✓</span>NPTEL Elite — Introduction to Machine Learning, IIT Kharagpur, 2025</div>
      <div class="cert-item"><span class="tick">✓</span>DeepLearning.AI — Neural Networks and Deep Learning</div>
      <div class="cert-item"><span class="tick">✓</span>DeepLearning.AI — Probability &amp; Statistics for ML</div>
      <div class="cert-item"><span class="tick">✓</span>MongoDB — Building AI-Powered Search with Vector Search</div>
      <div class="cert-item"><span class="tick">✓</span>NASSCOM FutureSkills Prime — Data Processing &amp; Visualization</div>
      <div class="cert-item"><span class="tick">✓</span>Cisco Networking Academy — Intro to Cybersecurity</div>
      <div class="cert-item"><span class="tick">✓</span>Docker and Kubernetes: The Complete Guide (Udemy)</div>
    </div>
  </div>
</section>

<section id="contact">
  <div class="wrap">
    <div class="contact-inner">
      <div class="reveal">
        <div class="sec-code" style="display:block; margin-bottom:14px;">FIG.&nbsp;06 — CONNECT</div>
        <div class="contact-title">Let's build something <span>intelligent &amp; secure.</span></div>
        <p style="color:var(--ink-dim); max-width:46ch;">Open to AI/ML engineering roles and collaborations. Reach out directly — I usually reply within a day.</p>
      </div>
      <div class="contact-links reveal">
        <a class="contact-link" href="mailto:tmsuryaramisetty@gmail.com"><span>tmsuryaramisetty@gmail.com</span><span class="arrow">→</span></a>
        <a class="contact-link" href="tel:+916301656616"><span>+91 63016 56616</span><span class="arrow">→</span></a>
        <a class="contact-link" href="https://www.linkedin.com/in/surya-ramisetty-baab5a29b/" target="_blank" rel="noopener"><span>LinkedIn</span><span class="arrow">→</span></a>
        <a class="contact-link" href="https://github.com/SuryaRamisetty" target="_blank" rel="noopener"><span>GitHub</span><span class="arrow">→</span></a>
        <a class="contact-link" href="https://leetcode.com/u/RamisettySurya/" target="_blank" rel="noopener"><span>LeetCode</span><span class="arrow">→</span></a>
        <a class="contact-link" href="https://x.com/suryaramisetty_" target="_blank" rel="noopener"><span>X / Twitter</span><span class="arrow">→</span></a>
      </div>
    </div>
  </div>
</section>

</main>

<footer>© 2026 RAMISETTY T M SURYA — BUILT FROM SCRATCH, NO TEMPLATE</footer>
<div class="frame-mark br">END OF DOCUMENT</div>

<script>
// ---------- Live 3D network scene (Three.js) ----------
(function(){
  const canvas = document.getElementById('netCanvas');
  const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
  if(typeof THREE === 'undefined'){ canvas.style.display='none'; return; }

  const scene = new THREE.Scene();
  const camera = new THREE.PerspectiveCamera(55, window.innerWidth/window.innerHeight, 0.1, 2000);
  camera.position.set(0, 0, 480);

  const renderer = new THREE.WebGLRenderer({ canvas, alpha:true, antialias:true });
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  renderer.setSize(window.innerWidth, window.innerHeight);

  const rootGroup = new THREE.Group();
  scene.add(rootGroup);

  // --- Particle network scattered in a 3D volume ---
  const NODE_COUNT = 130;
  const SPREAD = { x: 600, y: 380, z: 420 };
  const nodePositions = [];
  const velocities = [];
  for(let i=0;i<NODE_COUNT;i++){
    const p = new THREE.Vector3(
      (Math.random()-0.5) * SPREAD.x,
      (Math.random()-0.5) * SPREAD.y,
      (Math.random()-0.5) * SPREAD.z
    );
    nodePositions.push(p);
    velocities.push(new THREE.Vector3((Math.random()-0.5)*0.25,(Math.random()-0.5)*0.25,(Math.random()-0.5)*0.25));
  }

  const pointsGeo = new THREE.BufferGeometry();
  const pointsArr = new Float32Array(NODE_COUNT*3);
  pointsGeo.setAttribute('position', new THREE.BufferAttribute(pointsArr, 3));
  const pointsMat = new THREE.PointsMaterial({ color:0x5eead4, size:3.2, transparent:true, opacity:0.85, sizeAttenuation:true });
  const pointCloud = new THREE.Points(pointsGeo, pointsMat);
  rootGroup.add(pointCloud);

  const MAX_LINE_SEGMENTS = NODE_COUNT * 10;
  const lineGeo = new THREE.BufferGeometry();
  const lineArr = new Float32Array(MAX_LINE_SEGMENTS * 2 * 3);
  lineGeo.setAttribute('position', new THREE.BufferAttribute(lineArr, 3));
  const lineMat = new THREE.LineBasicMaterial({ color:0x5eead4, transparent:true, opacity:0.16 });
  const lineSegments = new THREE.LineSegments(lineGeo, lineMat);
  rootGroup.add(lineSegments);

  // --- Central wireframe "core" — icosahedron, the neural/security signature element ---
  const coreGeo = new THREE.IcosahedronGeometry(150, 1);
  const coreMat = new THREE.MeshBasicMaterial({ color:0x5eead4, wireframe:true, transparent:true, opacity:0.22 });
  const core = new THREE.Mesh(coreGeo, coreMat);
  core.position.set(200, 40, -80);
  rootGroup.add(core);

  const coreGeo2 = new THREE.IcosahedronGeometry(95, 0);
  const coreMat2 = new THREE.MeshBasicMaterial({ color:0xf5a623, wireframe:true, transparent:true, opacity:0.16 });
  const core2 = new THREE.Mesh(coreGeo2, coreMat2);
  core2.position.set(200, 40, -80);
  rootGroup.add(core2);

  function resize(){
    camera.aspect = window.innerWidth/window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
  }
  window.addEventListener('resize', resize);

  let targetRotX = 0, targetRotY = 0, curRotX = 0, curRotY = 0;
  window.addEventListener('mousemove', (e)=>{
    targetRotY = ((e.clientX / window.innerWidth) - 0.5) * 0.5;
    targetRotX = ((e.clientY / window.innerHeight) - 0.5) * 0.3;
  });

  const LINK_DIST = 130;
  function updateNetwork(){
    for(let i=0;i<NODE_COUNT;i++){
      const p = nodePositions[i], v = velocities[i];
      p.add(v);
      if(p.x < -SPREAD.x/2 || p.x > SPREAD.x/2) v.x *= -1;
      if(p.y < -SPREAD.y/2 || p.y > SPREAD.y/2) v.y *= -1;
      if(p.z < -SPREAD.z/2 || p.z > SPREAD.z/2) v.z *= -1;
      pointsArr[i*3] = p.x; pointsArr[i*3+1] = p.y; pointsArr[i*3+2] = p.z;
    }
    pointsGeo.attributes.position.needsUpdate = true;

    let segCount = 0;
    for(let i=0;i<NODE_COUNT && segCount < MAX_LINE_SEGMENTS;i++){
      for(let j=i+1;j<NODE_COUNT && segCount < MAX_LINE_SEGMENTS;j++){
        const dx = nodePositions[i].x-nodePositions[j].x;
        const dy = nodePositions[i].y-nodePositions[j].y;
        const dz = nodePositions[i].z-nodePositions[j].z;
        const dist = Math.sqrt(dx*dx+dy*dy+dz*dz);
        if(dist < LINK_DIST){
          const o = segCount*6;
          lineArr[o] = nodePositions[i].x; lineArr[o+1] = nodePositions[i].y; lineArr[o+2] = nodePositions[i].z;
          lineArr[o+3] = nodePositions[j].x; lineArr[o+4] = nodePositions[j].y; lineArr[o+5] = nodePositions[j].z;
          segCount++;
        }
      }
    }
    lineGeo.setDrawRange(0, segCount*2);
    lineGeo.attributes.position.needsUpdate = true;
  }

  function animate(){
    updateNetwork();
    rootGroup.rotation.y += 0.0011;
    core.rotation.x += 0.0025; core.rotation.y += 0.0018;
    core2.rotation.x -= 0.0018; core2.rotation.y -= 0.0022;

    curRotX += (targetRotX - curRotX) * 0.03;
    curRotY += (targetRotY - curRotY) * 0.03;
    camera.position.x = curRotY * 120;
    camera.position.y = -curRotX * 80;
    camera.lookAt(0,0,0);

    renderer.render(scene, camera);
    if(!reduceMotion) requestAnimationFrame(animate);
  }
  animate();
})();

// ---------- 3D tilt on hover for project / mini cards ----------
(function(){
  const cards = document.querySelectorAll('.proj-card, .mini-card');
  cards.forEach(card=>{
    card.addEventListener('mousemove', (e)=>{
      const r = card.getBoundingClientRect();
      const px = (e.clientX - r.left)/r.width - 0.5;
      const py = (e.clientY - r.top)/r.height - 0.5;
      card.style.transform = `perspective(700px) rotateX(${(-py*7).toFixed(2)}deg) rotateY(${(px*9).toFixed(2)}deg) translateZ(6px)`;
    });
    card.addEventListener('mouseleave', ()=>{
      card.style.transform = 'perspective(700px) rotateX(0deg) rotateY(0deg) translateZ(0px)';
    });
  });
})();

// ---------- Decrypt / scramble hero title ----------
(function(){
  const el = document.getElementById('decryptTitle');
  const finalText = "AI/ML Engineer &\nSecurity Researcher";
  const chars = "!<>-_\\/[]{}—=+*^?#________";
  const cursor = '<span class="cursor-blink">&nbsp;</span>';
  let frame = 0;
  const totalFrames = 34;

  function render(revealCount){
    let out = "";
    for(let i=0;i<finalText.length;i++){
      const c = finalText[i];
      if(c === "\n"){ out += "<br/>"; continue; }
      if(i < revealCount){ out += c; }
      else if(c === " "){ out += " "; }
      else { out += chars[Math.floor(Math.random()*chars.length)]; }
    }
    el.innerHTML = out + cursor;
  }

  function tick(){
    frame++;
    const revealCount = Math.floor((frame/totalFrames) * finalText.length);
    render(revealCount);
    if(frame < totalFrames){
      requestAnimationFrame(()=> setTimeout(tick, 28));
    } else {
      el.innerHTML = finalText.replace("\n","<br/>") + cursor;
    }
  }
  requestAnimationFrame(()=> setTimeout(tick, 250));
})();

// ---------- Scroll reveal ----------
(function(){
  const items = document.querySelectorAll('.reveal');
  const io = new IntersectionObserver((entries)=>{
    entries.forEach(e=>{
      if(e.isIntersecting){ e.target.classList.add('in'); io.unobserve(e.target); }
    });
  }, { threshold:0.12, rootMargin:"0px 0px -60px 0px" });
  items.forEach(i=> io.observe(i));
})();

// ---------- Rail active-section tracking ----------
(function(){
  const railItems = document.querySelectorAll('.rail-item');
  const sections = Array.from(railItems).map(r => document.getElementById(r.dataset.target)).filter(Boolean);

  railItems.forEach(r=>{
    r.addEventListener('click', ()=>{
      const target = document.getElementById(r.dataset.target);
      if(target) target.scrollIntoView({ behavior:'smooth' });
    });
  });

  const io = new IntersectionObserver((entries)=>{
    entries.forEach(entry=>{
      const id = entry.target.id;
      const railItem = document.querySelector('.rail-item[data-target="'+id+'"]');
      if(!railItem) return;
      if(entry.isIntersecting){
        railItems.forEach(r=>r.classList.remove('active'));
        railItem.classList.add('active');
      }
    });
  }, { threshold:0.4, rootMargin:"-10% 0px -10% 0px" });
  sections.forEach(s=> io.observe(s));
})();
</script>

</body>
</html>
