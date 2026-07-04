
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CheefLofter // operator log</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;700;800&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#0a0e13;
    --panel:#111820;
    --panel-2:#0d131a;
    --line:#22303d;
    --text:#dbe4ec;
    --muted:#8695a5;
    --accent:#3ddc97;
    --accent-dim:#215940;
    --warn:#e8a33d;
    --mono:'JetBrains Mono', ui-monospace, monospace;
    --sans:'Inter', system-ui, sans-serif;
    --section-pad:64px;
    --page-pad:24px;
    --radius:10px;
  }
  *{box-sizing:border-box;}
  html{scroll-behavior:smooth;}
  body{
    margin:0;
    background:var(--bg);
    color:var(--text);
    font-family:var(--sans);
    line-height:1.65;
    -webkit-font-smoothing:antialiased;
  }
  img{ max-width:100%; }
  @media (prefers-reduced-motion: reduce){
    *{animation-duration:0.001ms !important; animation-iteration-count:1 !important; transition-duration:0.001ms !important;}
  }

  ::selection{ background:var(--accent); color:#04140d; }

  a{ color:var(--accent); text-decoration:none; }
  a:hover{ text-decoration:underline; }
  a:focus-visible, button:focus-visible{ outline:2px solid var(--accent); outline-offset:3px; }

  .wrap{ max-width:920px; margin:0 auto; padding:0 var(--page-pad); }

  /* ---------- top bar ---------- */
  .topbar{
    position:sticky; top:0; z-index:50;
    background:rgba(10,14,19,0.88);
    backdrop-filter:blur(8px);
    border-bottom:1px solid var(--line);
  }
  .topbar-inner{
    max-width:920px; margin:0 auto; padding:14px var(--page-pad);
    display:flex; align-items:center; justify-content:space-between;
    font-family:var(--mono); font-size:13px;
  }
  .topbar .tag{ color:var(--accent); }
  .topbar nav{ display:flex; gap:22px; }
  .topbar nav a{ color:var(--muted); transition:color .15s; }
  .topbar nav a:hover{ color:var(--text); text-decoration:none; }
  .nav-toggle{
    display:none;
    background:none; border:1px solid var(--line); border-radius:6px;
    width:34px; height:34px; padding:0; cursor:pointer;
    position:relative;
  }
  .nav-toggle span, .nav-toggle span::before, .nav-toggle span::after{
    content:""; position:absolute; left:9px; width:16px; height:1.5px;
    background:var(--text); transition:transform .2s, opacity .2s;
  }
  .nav-toggle span{ top:16px; }
  .nav-toggle span::before{ top:-5px; }
  .nav-toggle span::after{ top:5px; }
  .nav-toggle[aria-expanded="true"] span{ background:transparent; }
  .nav-toggle[aria-expanded="true"] span::before{ transform:translateY(5px) rotate(45deg); }
  .nav-toggle[aria-expanded="true"] span::after{ transform:translateY(-5px) rotate(-45deg); }
  .dot{
    display:inline-block; width:7px; height:7px; border-radius:50%;
    background:var(--accent); margin-right:8px;
    box-shadow:0 0 0 0 rgba(61,220,151,0.6);
    animation:pulse 2.4s infinite;
  }
  @keyframes pulse{
    0%{ box-shadow:0 0 0 0 rgba(61,220,151,0.45); }
    70%{ box-shadow:0 0 0 8px rgba(61,220,151,0); }
    100%{ box-shadow:0 0 0 0 rgba(61,220,151,0); }
  }

  /* ---------- hero ---------- */
  .hero{
    position:relative;
    border-bottom:1px solid var(--line);
    overflow:hidden;
  }
  .hero-photo{
    position:relative;
    height:min(58vw, 420px);
    min-height:260px;
    background:#000;
  }
  .hero-photo img{
    width:100%; height:100%; object-fit:cover;
    object-position:center 30%;
    filter:grayscale(35%) contrast(1.05) brightness(0.55);
  }
  .hero-photo::after{
    content:"";
    position:absolute; inset:0;
    background:
      linear-gradient(180deg, rgba(10,14,19,0.15) 0%, rgba(10,14,19,0.55) 60%, var(--bg) 100%),
      repeating-linear-gradient(180deg, rgba(255,255,255,0.025) 0px, rgba(255,255,255,0.025) 1px, transparent 1px, transparent 3px);
  }
  .scanline{
    position:absolute; left:0; right:0; height:2px;
    background:linear-gradient(90deg, transparent, var(--accent), transparent);
    opacity:0.5;
    animation:scan 5s linear infinite;
  }
  @keyframes scan{
    0%{ top:0%; }
    100%{ top:100%; }
  }
  .hero-content{
    position:relative;
    margin-top:-96px;
    padding:0 var(--page-pad) 40px;
  }
  .hero-inner{ max-width:920px; margin:0 auto; }
  .avatar{
    width:92px; height:92px; border-radius:10px;
    border:2px solid var(--accent);
    box-shadow:0 0 0 4px rgba(10,14,19,0.9), 0 12px 30px rgba(0,0,0,0.5);
    object-fit:cover;
    display:block;
  }
  .handle-row{
    margin-top:18px;
    display:flex; align-items:baseline; gap:14px; flex-wrap:wrap;
  }
  .handle{
    font-family:var(--mono); font-weight:800; font-size:clamp(26px,5vw,40px);
    letter-spacing:-0.02em;
    word-break:break-word;
  }
  .handle .prompt{ color:var(--accent); }
  .status-pill{
    font-family:var(--mono); font-size:11.5px; color:var(--accent);
    border:1px solid var(--accent-dim); background:rgba(61,220,151,0.07);
    padding:4px 10px; border-radius:20px; white-space:nowrap;
  }
  .tagline{
    font-family:var(--mono); color:var(--muted); font-size:14.5px; margin-top:10px;
  }
  .tagline .cursor{
    display:inline-block; width:8px; height:15px; background:var(--accent);
    margin-left:4px; vertical-align:-2px;
    animation:blink 1.1s steps(1) infinite;
  }
  @keyframes blink{ 50%{ opacity:0; } }
  .hero-desc{
    max-width:58ch; color:var(--text); margin-top:20px; font-size:15.5px;
  }
  .hero-links{ margin-top:24px; display:flex; gap:12px; flex-wrap:wrap; }
  .btn{
    font-family:var(--mono); font-size:13px;
    padding:10px 18px; border-radius:6px;
    border:1px solid var(--line);
    color:var(--text); background:var(--panel);
    transition:border-color .15s, transform .15s, background .15s;
    display:inline-block;
  }
  .btn:hover{ border-color:var(--accent); text-decoration:none; transform:translateY(-1px); }
  .btn.primary{ background:var(--accent); color:#04140d; border-color:var(--accent); font-weight:700; }
  .btn.primary:hover{ filter:brightness(1.08); }

  /* ---------- section scaffolding ---------- */
  section{ padding:var(--section-pad) 0; border-bottom:1px solid var(--line); }
  section:last-of-type{ border-bottom:none; }
  .eyebrow{
    font-family:var(--mono); font-size:12px; color:var(--accent);
    letter-spacing:.14em; text-transform:uppercase;
    display:flex; align-items:center; gap:10px; margin-bottom:10px;
  }
  .eyebrow::before{ content:"//"; color:var(--muted); }
  h2{
    font-family:var(--mono); font-size:clamp(20px,3vw,26px); margin:0 0 22px;
    letter-spacing:-0.01em;
  }
  .section-lede{ color:var(--muted); max-width:60ch; margin:-10px 0 28px; font-size:15px; }

  /* ---------- about / readout ---------- */
  .readout{
    background:var(--panel); border:1px solid var(--line); border-radius:var(--radius);
    padding:22px 24px; font-family:var(--mono); font-size:13.5px;
    box-shadow:0 8px 24px rgba(0,0,0,0.18);
  }
  .readout .row{ display:flex; gap:14px; padding:6px 0; border-bottom:1px dashed var(--line); }
  .readout .row:last-child{ border-bottom:none; }
  .readout .k{ color:var(--muted); min-width:120px; flex-shrink:0; text-transform:uppercase; letter-spacing:.04em; font-size:11.5px; }
  .readout .v{ color:var(--text); }
  .readout .v .chip{
    display:inline-block; background:var(--accent-dim); color:var(--accent);
    padding:2px 8px; border-radius:4px; font-size:12px; margin:2px 4px 2px 0;
  }

  /* ---------- stack grid ---------- */
  .stack-grid{
    display:grid; grid-template-columns:repeat(auto-fill,minmax(150px,1fr));
    gap:12px;
  }
  .stack-card{
    background:var(--panel); border:1px solid var(--line); border-radius:8px;
    padding:14px 16px; font-family:var(--mono); font-size:13px;
    display:flex; flex-direction:column; gap:6px;
    box-shadow:0 6px 18px rgba(0,0,0,0.16);
    transition:border-color .15s, transform .15s;
  }
  .stack-card:hover{ border-color:var(--accent-dim); transform:translateY(-2px); }
  .stack-card .name{ color:var(--text); font-weight:700; }
  .stack-card .role{ color:var(--muted); font-size:11.5px; }
  .stack-card .bar{ height:4px; border-radius:2px; background:var(--line); margin-top:4px; overflow:hidden; }
  .stack-card .bar span{ display:block; height:100%; background:var(--accent); }

  /* ---------- projects ---------- */
  .case{
    background:var(--panel); border:1px solid var(--line); border-radius:var(--radius);
    padding:22px 24px; margin-bottom:16px;
    display:grid; grid-template-columns:90px 1fr; gap:20px;
    box-shadow:0 8px 24px rgba(0,0,0,0.18);
    transition:border-color .15s;
  }
  .case:hover{ border-color:var(--accent-dim); }
  .case:last-child{ margin-bottom:0; }
  .case-id{
    font-family:var(--mono); color:var(--muted); font-size:12px;
  }
  .case-id .num{ display:block; color:var(--accent); font-size:20px; font-weight:800; margin-top:4px; }
  .case h3{ margin:0 0 6px; font-family:var(--mono); font-size:17px; }
  .case p{ margin:0 0 12px; color:var(--muted); font-size:14.5px; max-width:56ch; }
  .case .tags{ display:flex; gap:8px; flex-wrap:wrap; margin-bottom:12px; }
  .case .tags span{
    font-family:var(--mono); font-size:11px; color:var(--muted);
    border:1px solid var(--line); padding:2px 8px; border-radius:4px;
  }
  .case a.repo-link{ font-family:var(--mono); font-size:13px; }
  @media (max-width:560px){
    .case{ grid-template-columns:1fr; }
    .case-id .num{ margin-top:0; }
  }

  /* ---------- roadmap ---------- */
  .roadmap{ position:relative; padding-left:26px; }
  .roadmap::before{
    content:""; position:absolute; left:6px; top:6px; bottom:6px; width:1px;
    background:var(--line);
  }
  .step{ position:relative; padding-bottom:26px; }
  .step:last-child{ padding-bottom:0; }
  .step::before{
    content:""; position:absolute; left:-26px; top:3px; width:11px; height:11px;
    border-radius:50%; border:2px solid var(--line); background:var(--bg);
  }
  .step.done::before{ border-color:var(--accent); background:var(--accent); }
  .step.active::before{ border-color:var(--warn); background:var(--warn); animation:pulse 2.4s infinite; }
  .step-head{ display:flex; align-items:center; gap:10px; flex-wrap:wrap; }
  .step-head h4{ margin:0; font-family:var(--mono); font-size:15px; }
  .step-status{
    font-family:var(--mono); font-size:10.5px; letter-spacing:.06em; text-transform:uppercase;
    padding:2px 8px; border-radius:4px; border:1px solid var(--line); color:var(--muted);
  }
  .step.done .step-status{ color:var(--accent); border-color:var(--accent-dim); }
  .step.active .step-status{ color:var(--warn); border-color:#5a4522; }
  .step p{ margin:6px 0 0; color:var(--muted); font-size:14px; max-width:58ch; }

  /* ---------- contact ---------- */
  .contact-grid{
    display:grid; grid-template-columns:repeat(auto-fit,minmax(210px,1fr)); gap:14px;
  }
  .contact-card{
    background:var(--panel); border:1px solid var(--line); border-radius:var(--radius);
    padding:18px 20px; transition:border-color .15s, transform .15s;
    box-shadow:0 8px 24px rgba(0,0,0,0.18);
    display:block;
  }
  .contact-card:hover{ border-color:var(--accent); transform:translateY(-2px); }
  .contact-card .label{ font-family:var(--mono); font-size:11px; color:var(--muted); text-transform:uppercase; letter-spacing:.08em; }
  .contact-card .value{ font-family:var(--mono); font-size:15px; margin-top:6px; color:var(--text); }

  footer{
    padding:28px 0 40px; text-align:center;
    font-family:var(--mono); font-size:12px; color:var(--muted);
  }

  /* ---------- responsive ---------- */
  @media (max-width:820px){
    :root{ --section-pad:52px; }
  }

  @media (max-width:680px){
    :root{ --page-pad:18px; --section-pad:44px; }

    .nav-toggle{ display:block; }
    .topbar nav{
      position:absolute; top:100%; left:0; right:0;
      flex-direction:column; gap:0;
      background:var(--panel-2); border-bottom:1px solid var(--line);
      max-height:0; overflow:hidden;
      transition:max-height .2s ease;
    }
    .topbar nav.open{ max-height:260px; }
    .topbar nav a{
      padding:13px var(--page-pad); border-top:1px solid var(--line);
    }

    .hero-photo{ height:min(70vw, 320px); min-height:200px; }
    .hero-content{ margin-top:-72px; }
    .avatar{ width:76px; height:76px; }
    .handle{ font-size:26px; }
    .status-pill{ font-size:11px; }
    .hero-desc{ font-size:14.5px; }
    .hero-links{ gap:10px; }
    .btn{ flex:1 1 auto; text-align:center; padding:11px 14px; }

    .readout .row{ flex-direction:column; gap:2px; }
    .readout .k{ min-width:0; }

    .case{ grid-template-columns:1fr; padding:18px 20px; }
    .case-id{ display:flex; align-items:center; gap:8px; }
    .case-id .num{ display:inline; margin-top:0; font-size:16px; }

    .roadmap{ padding-left:22px; }
    .step::before{ left:-22px; }

    section{ padding:var(--section-pad) 0; }
  }

  @media (max-width:420px){
    .handle{ font-size:22px; }
    .handle-row{ gap:10px; }
    .stack-grid{ grid-template-columns:repeat(auto-fill,minmax(130px,1fr)); }
  }
</style>
</head>
<body>

<div class="topbar">
  <div class="topbar-inner">
    <div><span class="dot"></span><span class="tag">root@cheeflofter</span>:~$</div>
    <nav id="siteNav">
      <a href="#about">about</a>
      <a href="#stack">stack</a>
      <a href="#cases">cases</a>
      <a href="#roadmap">roadmap</a>
      <a href="#contact">contact</a>
    </nav>
    <button class="nav-toggle" id="navToggle" aria-expanded="false" aria-controls="siteNav" aria-label="Toggle navigation">
      <span></span>
    </button>
  </div>
</div>

<div class="hero">
  <div class="hero-photo">
    <img src="banner.jpeg" alt="banner">
    <div class="scanline"></div>
  </div>
  <div class="hero-content">
    <div class="hero-inner">
      <img class="avatar" src="https://avatars.githubusercontent.com/u/113684979?v=4" alt="CheefLofter avatar">
      <div class="handle-row">
        <div class="handle"><span class="prompt">~/</span>cheeflofter</div>
        <span class="status-pill">STATUS: ACTIVE // LEVEL 0</span>
      </div>
      <div class="tagline">student — cyber security — web dev<span class="cursor"></span></div>
      <p class="hero-desc">
        Cyber student who likes breaking things and then figuring out how to stop people like me. Spend my time between CTFs, home lab chaos, and learning both sides of the fence — offencive and defensive .
      </p>
      <div class="hero-links">
        <a class="btn primary" href="#roadmap">view roadmap</a>
        <a class="btn" href="https://github.com/CheefLofter" target="_blank" rel="noopener">github ↗</a>
        <a class="btn" href="#contact">get in touch</a>
      </div>
    </div>
  </div>
</div>

<div class="wrap">

  <section id="about">
    <div class="eyebrow">profile</div>
    <h2>about this operator</h2>
    <div class="readout">
      <div class="row"><span class="k">designation</span><span class="v">CheefLofter</span></div>
      <div class="row"><span class="k">track</span><span class="v">Student — transitioning from web development into cyber security</span></div>
      <div class="row"><span class="k">focus</span><span class="v"><span class="chip">app security</span><span class="chip">secure tooling</span><span class="chip">defensive fundamentals</span></span></div>
      <div class="row"><span class="k">approach</span><span class="v">Learning by building — shipping small real tools first, layering in security-specific practice as the fundamentals solidify.</span></div>
    </div>
  </section>

  <section id="stack">
    <div class="eyebrow">loaded modules</div>
    <h2>tech stack</h2>
    <p class="section-lede">The languages and frameworks currently in active use across projects.</p>
    <div class="stack-grid">
      <div class="stack-card"><span class="name">Python</span><span class="role">scripting / tooling</span><div class="bar"><span style="width:60%"></span></div></div>
      <div class="stack-card"><span class="name">Go</span><span class="role">backend services</span><div class="bar"><span style="width:60%"></span></div></div>
      <div class="stack-card"><span class="name">Flask</span><span class="role">Python web apps</span><div class="bar"><span style="width:70%"></span></div></div>
      <div class="stack-card"><span class="name">Next.js</span><span class="role">frontend framework</span><div class="bar"><span style="width:65%"></span></div></div>
      <div class="stack-card"><span class="name">HTML5</span><span class="role">markup</span><div class="bar"><span style="width:90%"></span></div></div>
      <div class="stack-card"><span class="name">CSS3</span><span class="role">styling</span><div class="bar"><span style="width:85%"></span></div></div>
      <div class="stack-card"><span class="name">NPM</span><span class="role">package tooling</span><div class="bar"><span style="width:75%"></span></div></div>
    </div>
  </section>

  <section id="cases">
    <div class="eyebrow">shipped work</div>
    <h2>case files</h2>
    <p class="section-lede">Projects built so far — the foundation the security track is being layered onto.</p>

    <div class="case">
      <div class="case-id">CASE<span class="num">01</span></div>
      <div>
        <h3>FileFlow</h3>
        <p>A zero-knowledge file sharing tool built to share files securely — the closest existing project to a security-first design goal: the server shouldn't be able to read what it's storing.</p>
        <div class="tags"><span>zero-knowledge</span><span>secure sharing</span><span>HTML</span></div>
        <a class="repo-link" href="https://github.com/CheefLofter/FileFlow" target="_blank" rel="noopener">github.com/CheefLofter/FileFlow ↗</a>
      </div>
    </div>

    <div class="case">
      <div class="case-id">CASE<span class="num">02</span></div>
      <div>
        <h3>Staky-ai</h3>
        <p>An audio-to-notes tool that converts spoken input into structured written notes.</p>
        <div class="tags"><span>JavaScript</span><span>audio processing</span></div>
        <a class="repo-link" href="https://github.com/CheefLofter/Staky-ai" target="_blank" rel="noopener">github.com/CheefLofter/Staky-ai ↗</a>
      </div>
    </div>

    <div class="case">
      <div class="case-id">CASE<span class="num">03</span></div>
      <div>
        <h3>better-posts</h3>
        <p>A JavaScript project exploring improvements to how posts are created and displayed.</p>
        <div class="tags"><span>JavaScript</span></div>
        <a class="repo-link" href="https://github.com/CheefLofter/better-posts" target="_blank" rel="noopener">github.com/CheefLofter/better-posts ↗</a>
      </div>
    </div>
  </section>

  

  <section id="contact">
    <div class="eyebrow">open channels</div>
    <h2>get in touch</h2>
    <div class="contact-grid">
      <a class="contact-card" href="https://github.com/CheefLofter" target="_blank" rel="noopener">
        <div class="label">GitHub</div>
        <div class="value">@CheefLofter</div>
      </a>
      <a class="contact-card" href="https://discord.gg/cheeflofter" target="_blank" rel="noopener">
        <div class="label">Discord</div>
        <div class="value">cheeflofter</div>
      </a>
    </div>
  </section>

</div>

<footer>
  built with intent // last sync: 2026
</footer>

<script>
  var toggle = document.getElementById('navToggle');
  var nav = document.getElementById('siteNav');
  toggle.addEventListener('click', function(){
    var open = nav.classList.toggle('open');
    toggle.setAttribute('aria-expanded', open ? 'true' : 'false');
  });
  nav.querySelectorAll('a').forEach(function(link){
    link.addEventListener('click', function(){
      nav.classList.remove('open');
      toggle.setAttribute('aria-expanded', 'false');
    });
  });
</script>

</body>
</html>
