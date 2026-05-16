
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Kanata Chess Academy — Ottawa's Premier Youth Chess Association</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,600;0,700;1,300;1,600&family=Instrument+Sans:wght@400;500;600&family=Playfair+Display:wght@700;800;900&display=swap" rel="stylesheet" />

  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            obsidian: '#0f1a14',
            inkblue: '#162218',
            royalblue: '#1b4332',
            sapphire: '#2d6a4f',
            gold: '#d4a853',
            champagne: '#f2c572',
            ivory: '#f8f4ed',
            slate: '#8aad96',
          },
          fontFamily: {
            serif: ['Cormorant Garamond', 'Georgia', 'serif'],
            display: ['Playfair Display', 'Georgia', 'serif'],
            sans: ['Instrument Sans', 'system-ui', 'sans-serif'],
          }
        }
      }
    }
  </script>

  <style>
    *, *::before, *::after { box-sizing: border-box; }

    :root {
      --gold: #d4a853;
      --champagne: #f2c572;
      --obsidian: #0f1a14;
      --inkblue: #162218;
      --sapphire: #2d6a4f;
      --royalblue: #1b4332;
      --ivory: #f8f4ed;
      --slate: #8aad96;
    }

    html { scroll-behavior: smooth; }

    body {
      background: var(--obsidian);
      color: var(--ivory);
      font-family: 'Instrument Sans', system-ui, sans-serif;
      overflow-x: hidden;
    }

    /* ── Scrollbar ── */
    ::-webkit-scrollbar { width: 4px; }
    ::-webkit-scrollbar-track { background: var(--obsidian); }
    ::-webkit-scrollbar-thumb { background: var(--gold); border-radius: 2px; }

    /* ── Glassmorphism Nav ── */
    .glass-nav {
      background: rgba(10, 10, 15, 0.72);
      backdrop-filter: blur(20px) saturate(180%);
      -webkit-backdrop-filter: blur(20px) saturate(180%);
      border-bottom: 1px solid rgba(201, 168, 76, 0.18);
    }

    /* ── Hero grain overlay ── */
    .hero-grain::after {
      content: '';
      position: absolute;
      inset: 0;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");
      pointer-events: none;
      z-index: 1;
    }

    /* ── Chess board pattern ── */
    .board-pattern {
      background-image:
        linear-gradient(rgba(45,106,79,0.07) 1px, transparent 1px),
        linear-gradient(90deg, rgba(45,106,79,0.07) 1px, transparent 1px);
      background-size: 60px 60px;
    }

    /* ── Gold gradient text ── */
    .gold-text {
      background: linear-gradient(135deg, #d4a853 0%, #f2c572 45%, #d4a853 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .gold-text-2 {
      background: linear-gradient(90deg, #f2c572, #d4a853, #e8b85a);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    /* ── Animated underline ── */
    .underline-gold {
      position: relative;
      display: inline-block;
    }
    .underline-gold::after {
      content: '';
      position: absolute;
      left: 0; bottom: -4px;
      width: 100%; height: 2px;
      background: linear-gradient(90deg, var(--gold), var(--champagne));
      transform: scaleX(0);
      transform-origin: left;
      transition: transform 0.4s cubic-bezier(0.22, 1, 0.36, 1);
    }

    .underline-gold:hover::after { transform: scaleX(1); }

    /* ── CTA Button styles ── */
    .btn-primary {
      background: linear-gradient(135deg, #d4a853 0%, #e8b85a 50%, #d4a853 100%);
      background-size: 200% 200%;
      color: #0f1a14;
      font-weight: 600;
      letter-spacing: 0.03em;
      transition: all 0.35s cubic-bezier(0.22, 1, 0.36, 1);
      position: relative;
      overflow: hidden;
    }
    .btn-primary::before {
      content: '';
      position: absolute;
      inset: 0;
      background: linear-gradient(135deg, #e8b85a, #f2c572);
      opacity: 0;
      transition: opacity 0.3s;
    }
    .btn-primary:hover { transform: translateY(-2px); box-shadow: 0 12px 40px rgba(212,168,83,0.35); }
    .btn-primary:hover::before { opacity: 1; }
    .btn-primary span { position: relative; z-index: 1; }

    .btn-outline {
      border: 1.5px solid rgba(212,168,83,0.6);
      color: var(--champagne);
      background: transparent;
      letter-spacing: 0.03em;
      transition: all 0.35s cubic-bezier(0.22, 1, 0.36, 1);
    }
    .btn-outline:hover {
      border-color: var(--gold);
      background: rgba(212,168,83,0.1);
      transform: translateY(-2px);
      box-shadow: 0 8px 30px rgba(212,168,83,0.18);
    }

    /* ── Canvas container ── */
    #chess-canvas {
      display: block;
      width: 100%;
      height: 100%;
    }

    .canvas-wrapper {
      position: relative;
      width: 100%;
      height: 520px;
    }

    /* ── Canvas glow ── */
    .canvas-glow {
      position: absolute;
      bottom: -40px;
      left: 50%;
      transform: translateX(-50%);
      width: 280px;
      height: 80px;
      background: radial-gradient(ellipse, rgba(212,168,83,0.25) 0%, transparent 70%);
      filter: blur(20px);
      pointer-events: none;
      z-index: 0;
    }

    /* ── Media banner ── */
    .media-item {
      border: 1px solid rgba(45,106,79,0.35);
      background: rgba(27,67,50,0.25);
      transition: all 0.3s ease;
    }
    .media-item:hover {
      border-color: rgba(212,168,83,0.5);
      background: rgba(27,67,50,0.4);
      transform: translateY(-2px);
    }

    /* ── Curriculum cards ── */
    .level-card {
      border: 1px solid rgba(45,106,79,0.3);
      background: rgba(27,67,50,0.18);
      transition: all 0.4s cubic-bezier(0.22, 1, 0.36, 1);
      cursor: pointer;
      position: relative;
      overflow: hidden;
    }
    .level-card::before {
      content: '';
      position: absolute;
      inset: 0;
      background: linear-gradient(135deg, rgba(212,168,83,0.07) 0%, transparent 60%);
      opacity: 0;
      transition: opacity 0.4s;
    }
    .level-card:hover { border-color: rgba(212,168,83,0.5); transform: translateY(-4px); box-shadow: 0 20px 60px rgba(0,0,0,0.4), 0 0 0 1px rgba(212,168,83,0.12); }
    .level-card:hover::before { opacity: 1; }
    .level-card.active { border-color: var(--gold); background: rgba(212,168,83,0.07); box-shadow: 0 0 0 1px rgba(212,168,83,0.3), 0 20px 60px rgba(27,67,50,0.2); }

    /* ── Level detail panel ── */
    .level-detail {
      opacity: 0;
      transform: translateY(12px);
      transition: opacity 0.45s cubic-bezier(0.22, 1, 0.36, 1), transform 0.45s cubic-bezier(0.22, 1, 0.36, 1);
    }
    .level-detail.visible { opacity: 1; transform: translateY(0); }

    /* ── Tabs ── */
    .tab-btn { transition: all 0.3s ease; border-bottom: 2px solid transparent; }
    .tab-btn.active { border-color: var(--gold); color: var(--champagne); }
    .tab-panel { display: none; }
    .tab-panel.active { display: block; animation: fadeUp 0.4s cubic-bezier(0.22, 1, 0.36, 1); }

    /* ── Form inputs ── */
    .form-input {
      background: rgba(27,67,50,0.3);
      border: 1px solid rgba(45,106,79,0.4);
      color: var(--ivory);
      transition: all 0.3s ease;
      outline: none;
    }
    .form-input:focus {
      border-color: rgba(212,168,83,0.7);
      background: rgba(27,67,50,0.45);
      box-shadow: 0 0 0 3px rgba(212,168,83,0.1);
    }
    .form-input::placeholder { color: rgba(138,173,150,0.6); }
    .form-input option { background: #162218; color: var(--ivory); }

    /* ── Success message ── */
    .success-msg {
      display: none;
      animation: fadeUp 0.5s cubic-bezier(0.22, 1, 0.36, 1);
    }
    .success-msg.show { display: flex; }

    /* ── Decorative chess squares ── */
    .chess-deco {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 2px;
      opacity: 0.12;
    }
    .chess-sq { aspect-ratio: 1; background: var(--gold); }
    .chess-sq:nth-child(even) { background: transparent; border: 1px solid rgba(212,168,83,0.3); }

    /* ── Divider ── */
    .gold-divider {
      height: 1px;
      background: linear-gradient(90deg, transparent, rgba(45,106,79,0.5), rgba(212,168,83,0.3), rgba(45,106,79,0.5), transparent);
    }

    /* ── Section label ── */
    .section-label {
      font-family: 'Instrument Sans', sans-serif;
      font-size: 0.7rem;
      letter-spacing: 0.2em;
      text-transform: uppercase;
      color: var(--gold);
      opacity: 0.85;
    }

    /* ── Number badge ── */
    .piece-badge {
      width: 36px; height: 36px;
      border: 1.5px solid rgba(212,168,83,0.5);
      font-family: 'Cormorant Garamond', serif;
      font-weight: 600;
      font-size: 1rem;
      color: var(--gold);
      display: flex; align-items: center; justify-content: center;
      border-radius: 4px;
      background: rgba(212,168,83,0.08);
      flex-shrink: 0;
    }

    /* ── Stat counter ── */
    .stat-num {
      font-family: 'Playfair Display', serif;
      font-size: 3rem;
      font-weight: 700;
      line-height: 1;
    }

    /* ── Animations ── */
    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(20px); }
      to   { opacity: 1; transform: translateY(0); }
    }
    @keyframes fadeIn {
      from { opacity: 0; }
      to   { opacity: 1; }
    }
    @keyframes shimmer {
      0%   { background-position: -400% center; }
      100% { background-position: 400% center; }
    }
    @keyframes pulse-ring {
      0%   { transform: scale(1); opacity: 0.6; }
      100% { transform: scale(1.5); opacity: 0; }
    }
    @keyframes float-badge {
      0%, 100% { transform: translateY(0); }
      50%       { transform: translateY(-6px); }
    }
    @keyframes slide-in-left {
      from { opacity: 0; transform: translateX(-40px); }
      to   { opacity: 1; transform: translateX(0); }
    }

    .anim-fade-up   { animation: fadeUp 0.8s cubic-bezier(0.22, 1, 0.36, 1) both; }
    .anim-slide-left{ animation: slide-in-left 0.9s cubic-bezier(0.22, 1, 0.36, 1) both; }
    .delay-100 { animation-delay: 0.1s; }
    .delay-200 { animation-delay: 0.2s; }
    .delay-300 { animation-delay: 0.3s; }
    .delay-400 { animation-delay: 0.4s; }
    .delay-500 { animation-delay: 0.5s; }
    .delay-600 { animation-delay: 0.6s; }

    .floating-badge {
      animation: float-badge 3s ease-in-out infinite;
    }

    /* ── Footer ── */
    .footer-grid {
      border-top: 1px solid rgba(45,106,79,0.25);
    }

    /* ── Responsive ── */
    @media (max-width: 768px) {
      .hero-grid { grid-template-columns: 1fr; }
      .canvas-wrapper { height: 300px; }
      .stat-num { font-size: 2rem; }
      nav .hidden { display: none !important; }
      .floating-badge { top: 4px; right: 4px; }
    }
    @media (max-width: 480px) {
      .stat-num { font-size: 1.75rem; }
      section { padding-left: 1rem; padding-right: 1rem; }
    }

    /* ── Mobile hamburger menu ── */
    #mobile-menu {
      display: none;
      flex-direction: column;
      gap: 12px;
      padding: 20px 24px;
      background: rgba(15,26,20,0.97);
      border-bottom: 1px solid rgba(45,106,79,0.3);
    }
    #mobile-menu.open { display: flex; }
  </style>
</head>
<body class="board-pattern">

  <!-- ══════════════════════════════════════════
       NAVIGATION
  ══════════════════════════════════════════ -->
  <nav class="glass-nav fixed top-0 left-0 right-0 z-50 px-6 py-4">
    <div class="max-w-7xl mx-auto flex items-center justify-between">

      <!-- Logo -->
      <div class="flex items-center gap-3">
        <div class="w-9 h-9 relative flex items-center justify-center flex-shrink-0">
          <div class="chess-deco w-9 h-9 absolute">
            <div class="chess-sq"></div><div class="chess-sq"></div>
            <div class="chess-sq"></div><div class="chess-sq"></div>
            <div class="chess-sq"></div><div class="chess-sq"></div>
            <div class="chess-sq"></div><div class="chess-sq"></div>
            <div class="chess-sq"></div><div class="chess-sq"></div>
            <div class="chess-sq"></div><div class="chess-sq"></div>
            <div class="chess-sq"></div><div class="chess-sq"></div>
            <div class="chess-sq"></div><div class="chess-sq"></div>
          </div>
          <svg class="relative z-10" width="20" height="20" viewBox="0 0 24 24" fill="none">
            <path d="M12 2L13.5 7H18L14.25 10L15.75 15L12 12L8.25 15L9.75 10L6 7H10.5L12 2Z" fill="#d4a853"/>
            <rect x="7" y="20" width="10" height="2" rx="1" fill="#d4a853"/>
            <rect x="9" y="16" width="6" height="4" rx="0.5" fill="#d4a853" opacity="0.7"/>
          </svg>
        </div>
        <div>
          <div class="font-display font-bold text-ivory text-sm leading-tight tracking-wide">Kanata Chess</div>
          <div class="section-label" style="font-size:0.58rem; letter-spacing:0.15em;">ACADEMY</div>
        </div>
      </div>

      <!-- Nav links -->
      <div class="hidden md:flex items-center gap-8">
        <a href="#curriculum" class="underline-gold text-slate text-sm hover:text-ivory transition-colors duration-200">Programs</a>
        <a href="#register" class="underline-gold text-slate text-sm hover:text-ivory transition-colors duration-200">Register</a>
        <a href="#impact" class="underline-gold text-slate text-sm hover:text-ivory transition-colors duration-200">Impact</a>
        <a href="#footer" class="underline-gold text-slate text-sm hover:text-ivory transition-colors duration-200">Contact</a>
      </div>

      <!-- CTA -->
      <a href="#register" class="hidden md:inline-flex btn-primary px-5 py-2.5 rounded-md text-sm font-semibold" style="font-family:'Instrument Sans',sans-serif;">
        <span>Book Assessment</span>
      </a>

      <!-- Hamburger (mobile) -->
      <button class="md:hidden flex flex-col gap-1.5 p-2" onclick="document.getElementById('mobile-menu').classList.toggle('open')" aria-label="Menu">
        <span style="display:block;width:22px;height:2px;background:var(--ivory);border-radius:2px;"></span>
        <span style="display:block;width:22px;height:2px;background:var(--ivory);border-radius:2px;"></span>
        <span style="display:block;width:22px;height:2px;background:var(--ivory);border-radius:2px;"></span>
      </button>
    </div>
  </nav>

  <!-- Mobile menu -->
  <div id="mobile-menu" class="fixed top-[65px] left-0 right-0 z-40">
    <a href="#curriculum" onclick="document.getElementById('mobile-menu').classList.remove('open')">Programs</a>
    <a href="#register" onclick="document.getElementById('mobile-menu').classList.remove('open')">Register</a>
    <a href="#impact" onclick="document.getElementById('mobile-menu').classList.remove('open')">Impact</a>
    <a href="#footer" onclick="document.getElementById('mobile-menu').classList.remove('open')">Contact</a>
    <a href="#register" class="btn-primary px-5 py-2.5 rounded-md text-sm font-semibold text-center mt-2" onclick="document.getElementById('mobile-menu').classList.remove('open')">
      <span>Book Assessment</span>
    </a>
  </div>


  <!-- ══════════════════════════════════════════
       HERO SECTION
  ══════════════════════════════════════════ -->
  <section class="hero-grain relative min-h-screen flex items-center pt-20 overflow-hidden" style="background: radial-gradient(ellipse at 70% 50%, rgba(45,106,79,0.12) 0%, transparent 60%), radial-gradient(ellipse at 20% 80%, rgba(212,168,83,0.06) 0%, transparent 50%), var(--obsidian);">

    <!-- Decorative diagonal line -->
    <div class="absolute inset-0 pointer-events-none z-0" aria-hidden="true">
      <svg class="absolute right-0 top-0 h-full opacity-5" viewBox="0 0 400 900" fill="none" preserveAspectRatio="none" style="width:420px;">
        <line x1="0" y1="0" x2="400" y2="900" stroke="#2d6a4f" stroke-width="1"/>
        <line x1="60" y1="0" x2="460" y2="900" stroke="#2d6a4f" stroke-width="0.5"/>
      </svg>
    </div>

    <div class="max-w-7xl mx-auto px-6 w-full z-10 relative">
      <div class="hero-grid grid md:grid-cols-2 gap-0 items-center min-h-[calc(100vh-80px)]">

        <!-- Left: Typography -->
        <div class="py-16 md:py-0 md:pr-12">
          <div class="anim-slide-left section-label mb-6">
            ♟ Ottawa's Premier Youth Chess Association
          </div>

          <h1 class="anim-fade-up delay-100 font-display leading-[1.08] mb-6" style="font-size: clamp(2.4rem, 5vw, 4rem); font-weight: 800; color: var(--ivory);">
            Building Ottawa's<br/>
            <em class="gold-text not-italic">Next Generation</em><br/>
            of Chess Leaders
          </h1>

          <p class="anim-fade-up delay-200 text-slate leading-relaxed mb-10" style="font-size:1.05rem; max-width:480px;">
            Ottawa's largest youth chess association — mentoring <strong class="text-ivory font-medium">200+ students</strong> across the city, hosting competitive tournaments, and forging lifelong critical thinkers.
          </p>

          <div class="anim-fade-up delay-300 flex flex-wrap gap-3 mb-14">
            <a href="#curriculum" class="btn-primary px-7 py-3.5 rounded-md text-sm font-semibold inline-block">
              <span>Explore Programs</span>
            </a>
            <a href="#register" class="btn-outline px-7 py-3.5 rounded-md text-sm font-semibold inline-block">
              Schedule Free Assessment
            </a>
          </div>

          <!-- Stats row -->
          <div class="anim-fade-up delay-400 grid grid-cols-3 gap-6 pt-8" style="border-top: 1px solid rgba(45,106,79,0.3);">
            <div>
              <div class="stat-num gold-text">200+</div>
              <div class="text-slate text-xs mt-1 tracking-wide" style="font-size:0.72rem;">Students Mentored</div>
            </div>
            <div>
              <div class="stat-num gold-text">5+</div>
              <div class="text-slate text-xs mt-1 tracking-wide" style="font-size:0.72rem;">City Tournaments Hosted</div>
            </div>
            <div>
              <div class="stat-num gold-text">3</div>
              <div class="text-slate text-xs mt-1 tracking-wide" style="font-size:0.72rem;">National Recognitions</div>
            </div>
          </div>
        </div>

        <!-- Right: Three.js Canvas -->
        <div class="relative flex flex-col items-center justify-center py-8">
          <div class="canvas-wrapper relative anim-fade-up delay-200">
            <canvas id="chess-canvas"></canvas>
            <div class="canvas-glow"></div>
          </div>

          <!-- Floating badge -->
          <div class="floating-badge absolute top-8 right-4 md:right-0 glass-nav px-4 py-3 rounded-xl" style="border:1px solid rgba(212,168,83,0.35);">
            <div class="section-label mb-0.5" style="font-size:0.6rem;">FOUNDED BY</div>
            <div class="font-serif text-ivory font-semibold" style="font-size:0.9rem;">Ankita Jain</div>
          </div>

          <!-- Floating label -->
          <div class="absolute bottom-12 left-4 md:left-0 text-xs text-slate" style="font-family:'Instrument Sans',sans-serif; letter-spacing:0.1em;">
            <span class="text-gold opacity-60">↑</span> Drag to explore
          </div>
        </div>

      </div>
    </div>
  </section>


  <!-- ══════════════════════════════════════════
       MEDIA & IMPACT BANNER
  ══════════════════════════════════════════ -->
  <section id="impact" class="py-16 relative" style="background: linear-gradient(180deg, rgba(27,67,50,0.15) 0%, transparent 100%);">
    <div class="gold-divider mb-12"></div>
    <div class="max-w-6xl mx-auto px-6">
      <div class="section-label text-center mb-10">Recognition & Partnerships</div>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-4">

        <!-- CTV News -->
        <div class="media-item rounded-xl p-6 text-center">
          <div class="text-2xl mb-3">📺</div>
          <div class="font-serif font-semibold text-ivory mb-1" style="font-size:1.05rem;">As Seen on CTV News</div>
          <div class="text-slate text-sm leading-relaxed">Featured for transforming youth chess education across the Ottawa region</div>
        </div>

        <!-- Ottawa Food Bank -->
        <div class="media-item rounded-xl p-6 text-center" style="border-color:rgba(212,168,83,0.35);">
          <div class="text-2xl mb-3">🤝</div>
          <div class="font-serif font-semibold text-ivory mb-1" style="font-size:1.05rem;">Partnered with Ottawa Food Bank</div>
          <div class="text-slate text-sm leading-relaxed">Chess for good — community outreach blending strategy with social impact</div>
        </div>

        <!-- RBC Award -->
        <div class="media-item rounded-xl p-6 text-center">
          <div class="text-2xl mb-3">🏆</div>
          <div class="font-serif font-semibold text-ivory mb-1" style="font-size:1.05rem;">RBC 21 Under 21 Award</div>
          <div class="text-slate text-sm leading-relaxed">Awarded Leadership Excellence — recognizing Ankita Jain's visionary impact</div>
        </div>

      </div>
    </div>
    <div class="gold-divider mt-12"></div>
  </section>


  <!-- ══════════════════════════════════════════
       CURRICULUM — 5 LEVELS
  ══════════════════════════════════════════ -->
  <section id="curriculum" class="py-24 relative">
    <div class="max-w-7xl mx-auto px-6">

      <div class="text-center mb-16">
        <div class="section-label mb-4">Progressive Learning System</div>
        <h2 class="font-display text-ivory" style="font-size: clamp(2rem, 4vw, 3.2rem); font-weight:800; line-height:1.1;">
          The <span class="gold-text">Five-Level</span> Mastery Path
        </h2>
        <p class="text-slate mt-4 max-w-lg mx-auto" style="font-size:0.95rem;">
          A structured curriculum designed to take students from first moves to championship-level play.
        </p>
      </div>

      <!-- Level Cards Grid -->
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-5 gap-4 mb-10" id="level-cards">

        <!-- Level 1: Pawn -->
        <div class="level-card rounded-xl p-5 active" data-level="1">
          <div class="flex items-center gap-3 mb-4">
            <div class="piece-badge">♙</div>
            <div class="section-label" style="font-size:0.6rem;">Level 01</div>
          </div>
          <div class="font-serif text-ivory font-semibold mb-1" style="font-size:1.1rem;">Pawn</div>
          <div class="text-gold text-xs mb-3" style="letter-spacing:0.05em;">Beginner Foundations</div>
          <div class="text-slate text-xs leading-relaxed">Board orientation, piece movements, basic objectives.</div>
        </div>

        <!-- Level 2: Knight -->
        <div class="level-card rounded-xl p-5" data-level="2">
          <div class="flex items-center gap-3 mb-4">
            <div class="piece-badge">♘</div>
            <div class="section-label" style="font-size:0.6rem;">Level 02</div>
          </div>
          <div class="font-serif text-ivory font-semibold mb-1" style="font-size:1.1rem;">Knight</div>
          <div class="text-gold text-xs mb-3" style="letter-spacing:0.05em;">Tactics & Coordination</div>
          <div class="text-slate text-xs leading-relaxed">Forks, pins, skewers, and piece coordination.</div>
        </div>

        <!-- Level 3: Bishop -->
        <div class="level-card rounded-xl p-5" data-level="3">
          <div class="flex items-center gap-3 mb-4">
            <div class="piece-badge">♗</div>
            <div class="section-label" style="font-size:0.6rem;">Level 03</div>
          </div>
          <div class="font-serif text-ivory font-semibold mb-1" style="font-size:1.1rem;">Bishop</div>
          <div class="text-gold text-xs mb-3" style="letter-spacing:0.05em;">Middle-Game Strategy</div>
          <div class="text-slate text-xs leading-relaxed">Pawn structures, positional play, long-term plans.</div>
        </div>

        <!-- Level 4: Rook -->
        <div class="level-card rounded-xl p-5" data-level="4">
          <div class="flex items-center gap-3 mb-4">
            <div class="piece-badge">♖</div>
            <div class="section-label" style="font-size:0.6rem;">Level 04</div>
          </div>
          <div class="font-serif text-ivory font-semibold mb-1" style="font-size:1.1rem;">Rook</div>
          <div class="text-gold text-xs mb-3" style="letter-spacing:0.05em;">Endgame Mastery</div>
          <div class="text-slate text-xs leading-relaxed">King activity, rook endgames, converting advantages.</div>
        </div>

        <!-- Level 5: King -->
        <div class="level-card rounded-xl p-5" data-level="5">
          <div class="flex items-center gap-3 mb-4">
            <div class="piece-badge">♔</div>
            <div class="section-label" style="font-size:0.6rem;">Level 05</div>
          </div>
          <div class="font-serif text-ivory font-semibold mb-1" style="font-size:1.1rem;">King</div>
          <div class="text-gold text-xs mb-3" style="letter-spacing:0.05em;">Advanced Masterclasses</div>
          <div class="text-slate text-xs leading-relaxed">Tournament prep, deep analysis, grandmaster games.</div>
        </div>

      </div>

      <!-- Level Detail Panel -->
      <div id="level-detail-panel" class="level-detail visible rounded-2xl p-8 md:p-10" style="border:1px solid rgba(212,168,83,0.25); background: linear-gradient(135deg, rgba(27,67,50,0.35) 0%, rgba(212,168,83,0.04) 100%);">
        <div class="flex flex-col md:flex-row gap-8 items-start">
          <div class="text-6xl flex-shrink-0" id="detail-icon">♙</div>
          <div class="flex-1">
            <div class="section-label mb-2" id="detail-label">Level 01 — Pawn</div>
            <h3 class="font-display text-ivory font-bold mb-3" id="detail-title" style="font-size:1.7rem;">Beginner Foundations</h3>
            <p class="text-slate leading-relaxed mb-6" id="detail-desc">
              The journey of every chess master begins with understanding the board. In Level 1, students learn how each piece moves, the objective of the game, and the fundamentals of safe play. We use engaging games, puzzles, and group exercises to build confidence and a love for the game from the very first session.
            </p>
            <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
              <div id="detail-topics"></div>
            </div>
            <div class="flex flex-wrap gap-3 mt-6" id="detail-tags"></div>
          </div>
        </div>
      </div>

    </div>
  </section>


  <!-- ══════════════════════════════════════════
       REGISTRATION & TOURNAMENT HUB
  ══════════════════════════════════════════ -->
  <section id="register" class="py-24 relative" style="background: radial-gradient(ellipse at 50% 0%, rgba(27,67,50,0.18) 0%, transparent 60%);">
    <div class="gold-divider mb-20"></div>
    <div class="max-w-4xl mx-auto px-6">

      <div class="text-center mb-12">
        <div class="section-label mb-4">Enrollment & Competition</div>
        <h2 class="font-display text-ivory" style="font-size: clamp(1.8rem, 3.5vw, 2.8rem); font-weight:800; line-height:1.15;">
          Join the <span class="gold-text">Academy</span>
        </h2>
        <p class="text-slate mt-3" style="font-size:0.95rem;">Weekly classes and city-wide tournaments — take your first move.</p>
      </div>

      <!-- Tab Container -->
      <div class="rounded-2xl overflow-hidden" style="border:1px solid rgba(45,106,79,0.35); background: rgba(15,26,20,0.5);">

        <!-- Tab Buttons -->
        <div class="flex border-b" style="border-color:rgba(45,106,79,0.25); background:rgba(0,0,0,0.2);">
          <button class="tab-btn active flex-1 py-4 px-6 text-sm font-medium text-ivory" onclick="switchTab('classes')" id="tab-classes" style="font-family:'Instrument Sans',sans-serif;">
            ♟ Weekly Class Enrollment
          </button>
          <button class="tab-btn flex-1 py-4 px-6 text-sm font-medium text-slate" onclick="switchTab('tournament')" id="tab-tournament" style="font-family:'Instrument Sans',sans-serif;">
            🏆 Tournament Registration
          </button>
        </div>

        <!-- Tab Panels -->
        <div class="p-8 md:p-10">

          <!-- Classes Tab -->
          <div class="tab-panel active" id="panel-classes">
            <div id="success-classes" class="success-msg flex-col items-center justify-center py-12 text-center gap-4">
              <div style="width:64px;height:64px;background:linear-gradient(135deg,#d4a853,#f2c572);border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:1.8rem;">✓</div>
              <h3 class="font-display text-ivory text-xl font-bold">Enrollment Submitted!</h3>
              <p class="text-slate text-sm max-w-sm">We'll reach out within 24 hours to confirm your slot and share pre-class resources.</p>
              <button onclick="resetForm('classes')" class="btn-outline px-6 py-2.5 rounded-md text-sm mt-2">Enroll Another Student</button>
            </div>
            <form id="form-classes" onsubmit="submitForm(event,'classes')">
              <div class="grid md:grid-cols-2 gap-6 mb-6">
                <div>
                  <label class="block text-xs text-slate mb-2 tracking-wide" style="letter-spacing:0.08em;">PARENT / GUARDIAN NAME *</label>
                  <input type="text" required placeholder="e.g. Priya Sharma" class="form-input w-full px-4 py-3 rounded-lg text-sm" />
                </div>
                <div>
                  <label class="block text-xs text-slate mb-2 tracking-wide" style="letter-spacing:0.08em;">STUDENT AGE *</label>
                  <input type="number" required min="5" max="18" placeholder="e.g. 10" class="form-input w-full px-4 py-3 rounded-lg text-sm" />
                </div>
                <div>
                  <label class="block text-xs text-slate mb-2 tracking-wide" style="letter-spacing:0.08em;">CURRENT SKILL LEVEL *</label>
                  <select required class="form-input w-full px-4 py-3 rounded-lg text-sm">
                    <option value="" disabled selected>Select a level…</option>
                    <option>Level 1 — Pawn (Complete Beginner)</option>
                    <option>Level 2 — Knight (Knows the moves)</option>
                    <option>Level 3 — Bishop (Club player)</option>
                    <option>Level 4 — Rook (Competitive)</option>
                    <option>Level 5 — King (Advanced / Rated)</option>
                  </select>
                </div>
                <div>
                  <label class="block text-xs text-slate mb-2 tracking-wide" style="letter-spacing:0.08em;">PREFERRED WEEKEND SLOT *</label>
                  <select required class="form-input w-full px-4 py-3 rounded-lg text-sm">
                    <option value="" disabled selected>Choose a slot…</option>
                    <option>Saturday 9:00 AM – 10:30 AM</option>
                    <option>Saturday 11:00 AM – 12:30 PM</option>
                    <option>Saturday 2:00 PM – 3:30 PM</option>
                    <option>Sunday 10:00 AM – 11:30 AM</option>
                    <option>Sunday 1:00 PM – 2:30 PM</option>
                  </select>
                </div>
              </div>
              <div class="mb-6">
                <label class="block text-xs text-slate mb-2 tracking-wide" style="letter-spacing:0.08em;">EMAIL ADDRESS *</label>
                <input type="email" required placeholder="parent@email.com" class="form-input w-full px-4 py-3 rounded-lg text-sm" />
              </div>
              <button type="submit" class="btn-primary w-full py-4 rounded-lg text-sm font-semibold">
                <span>Submit Enrollment →</span>
              </button>
            </form>
          </div>

          <!-- Tournament Tab -->
          <div class="tab-panel" id="panel-tournament">
            <div id="success-tournament" class="success-msg flex-col items-center justify-center py-12 text-center gap-4">
              <div style="width:64px;height:64px;background:linear-gradient(135deg,#d4a853,#f2c572);border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:1.8rem;">✓</div>
              <h3 class="font-display text-ivory text-xl font-bold">Registration Confirmed!</h3>
              <p class="text-slate text-sm max-w-sm">Tournament details, pairings schedule, and venue information will be sent to your email shortly.</p>
              <button onclick="resetForm('tournament')" class="btn-outline px-6 py-2.5 rounded-md text-sm mt-2">Register Another Team</button>
            </div>
            <form id="form-tournament" onsubmit="submitForm(event,'tournament')">
              <div class="grid md:grid-cols-2 gap-6 mb-6">
                <div>
                  <label class="block text-xs text-slate mb-2 tracking-wide" style="letter-spacing:0.08em;">REGISTRATION TYPE *</label>
                  <select required class="form-input w-full px-4 py-3 rounded-lg text-sm" id="reg-type" onchange="toggleSchoolField()">
                    <option value="" disabled selected>Individual or Team?</option>
                    <option value="individual">Individual Player</option>
                    <option value="school">School Team</option>
                  </select>
                </div>
                <div>
                  <label class="block text-xs text-slate mb-2 tracking-wide" style="letter-spacing:0.08em;">CONTACT EMAIL *</label>
                  <input type="email" required placeholder="contact@school.ca" class="form-input w-full px-4 py-3 rounded-lg text-sm" />
                </div>
                <div id="school-name-field">
                  <label class="block text-xs text-slate mb-2 tracking-wide" style="letter-spacing:0.08em;">SCHOOL NAME</label>
                  <input type="text" placeholder="e.g. Earl of March Secondary" class="form-input w-full px-4 py-3 rounded-lg text-sm" id="school-name-input" />
                </div>
                <div id="player-count-field">
                  <label class="block text-xs text-slate mb-2 tracking-wide" style="letter-spacing:0.08em;">NUMBER OF PLAYERS *</label>
                  <input type="number" required min="1" max="30" placeholder="e.g. 4" class="form-input w-full px-4 py-3 rounded-lg text-sm" />
                </div>
              </div>
              <div class="mb-6">
                <label class="block text-xs text-slate mb-2 tracking-wide" style="letter-spacing:0.08em;">UPCOMING TOURNAMENT</label>
                <select class="form-input w-full px-4 py-3 rounded-lg text-sm">
                  <option>Kanata Open — Summer 2025</option>
                  <option>Ottawa Youth Championship — Fall 2025</option>
                  <option>Inter-School Blitz Cup — Winter 2025</option>
                </select>
              </div>
              <button type="submit" class="btn-primary w-full py-4 rounded-lg text-sm font-semibold">
                <span>Complete Tournament Registration →</span>
              </button>
            </form>
          </div>

        </div>
      </div>

    </div>
    <div class="gold-divider mt-20"></div>
  </section>


  <!-- ══════════════════════════════════════════
       FOOTER
  ══════════════════════════════════════════ -->
  <footer id="footer" class="footer-grid pt-14 pb-10">
    <div class="max-w-7xl mx-auto px-6">
      <div class="grid md:grid-cols-3 gap-12 mb-12">

        <!-- Brand column -->
        <div>
          <div class="font-display text-ivory font-bold text-xl mb-2">Kanata Chess Academy</div>
          <div class="section-label mb-4" style="font-size:0.62rem;">Founded by Ankita Jain</div>
          <p class="text-slate text-sm leading-relaxed">Empowering Ottawa's youth through the art of chess — developing critical thinking, patience, and strategic excellence.</p>
        </div>

        <!-- Quick links -->
        <div>
          <div class="section-label mb-5">Navigate</div>
          <ul class="space-y-3">
            <li><a href="#curriculum" class="text-slate text-sm hover:text-champagne transition-colors underline-gold">Programs & Curriculum</a></li>
            <li><a href="#register" class="text-slate text-sm hover:text-champagne transition-colors underline-gold">Enroll / Register</a></li>
            <li><a href="#impact" class="text-slate text-sm hover:text-champagne transition-colors underline-gold">Recognition & Media</a></li>
            <li><a href="#footer" class="text-slate text-sm hover:text-champagne transition-colors underline-gold">Contact Us</a></li>
          </ul>
        </div>

        <!-- Contact -->
        <div>
          <div class="section-label mb-5">Contact</div>
          <ul class="space-y-4">
            <li class="flex items-start gap-3">
              <span class="text-gold mt-0.5">📍</span>
              <div>
                <div class="text-ivory text-sm font-medium">Location</div>
                <div class="text-slate text-sm">Kanata, Ottawa, ON, Canada</div>
              </div>
            </li>
            <li class="flex items-start gap-3">
              <span class="text-gold mt-0.5">✉️</span>
              <div>
                <div class="text-ivory text-sm font-medium">Email</div>
                <a href="mailto:kanatachess@gmail.com" class="text-slate text-sm hover:text-champagne transition-colors">kanatachess@gmail.com</a>
              </div>
            </li>
            <li class="flex items-start gap-3">
              <span class="text-gold mt-0.5">🕐</span>
              <div>
                <div class="text-ivory text-sm font-medium">Classes</div>
                <div class="text-slate text-sm">Weekends — Morning & Afternoon</div>
              </div>
            </li>
          </ul>
        </div>

      </div>

      <div class="gold-divider mb-6"></div>
      <div class="flex flex-col md:flex-row justify-between items-center gap-3 text-xs text-slate">
        <div>© 2025 Kanata Chess Academy · Founded by Ankita Jain · All rights reserved</div>
        <div class="gold-text-2 font-serif italic" style="font-size:0.85rem;">"Every master was once a beginner."</div>
      </div>

    </div>
  </footer>


<!-- ══════════════════════════════════════════
     JAVASCRIPT: Three.js + Interactions
══════════════════════════════════════════ -->
<script>
/* ─────────────────────────────────────────
   THREE.JS 3D CHESS PIECE
───────────────────────────────────────── */
(function initThreeJS() {
  const canvas = document.getElementById('chess-canvas');
  if (!canvas) return;

  const container = canvas.parentElement;

  // Scene
  const scene = new THREE.Scene();

  // Camera
  const camera = new THREE.PerspectiveCamera(42, container.clientWidth / container.clientHeight, 0.1, 100);
  camera.position.set(0, 1.2, 6.5);
  camera.lookAt(0, 0.4, 0);

  // Renderer
  const renderer = new THREE.WebGLRenderer({
    canvas,
    antialias: true,
    alpha: true,
  });
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  renderer.setSize(container.clientWidth, container.clientHeight);
  renderer.shadowMap.enabled = true;
  renderer.shadowMap.type = THREE.PCFSoftShadowMap;
  renderer.outputEncoding = THREE.sRGBEncoding;
  renderer.toneMapping = THREE.ACESFilmicToneMapping;
  renderer.toneMappingExposure = 1.2;

  /* ── Materials ── */
  const goldMat = new THREE.MeshStandardMaterial({
    color: 0xd4a853,
    metalness: 0.88,
    roughness: 0.12,
    envMapIntensity: 1.0,
  });
  const marbMat = new THREE.MeshStandardMaterial({
    color: 0xd8f0e4,
    metalness: 0.05,
    roughness: 0.65,
  });
  const darkMat = new THREE.MeshStandardMaterial({
    color: 0x0f1a14,
    metalness: 0.3,
    roughness: 0.5,
  });
  const accentMat = new THREE.MeshStandardMaterial({
    color: 0xf2c572,
    metalness: 0.95,
    roughness: 0.05,
  });

  /* ── Build King Piece ── */
  const pieceGroup = new THREE.Group();

  // Base disc
  const base = new THREE.Mesh(
    new THREE.CylinderGeometry(0.85, 1.0, 0.18, 48),
    goldMat
  );
  base.position.y = 0.09;
  base.castShadow = true;
  pieceGroup.add(base);

  // Base ring inset
  const baseRing = new THREE.Mesh(
    new THREE.TorusGeometry(0.78, 0.06, 16, 48),
    accentMat
  );
  baseRing.rotation.x = Math.PI / 2;
  baseRing.position.y = 0.22;
  baseRing.castShadow = true;
  pieceGroup.add(baseRing);

  // Lower column taper
  const col1 = new THREE.Mesh(
    new THREE.CylinderGeometry(0.5, 0.82, 0.35, 40),
    marbMat
  );
  col1.position.y = 0.35;
  col1.castShadow = true;
  pieceGroup.add(col1);

  // Middle shaft
  const shaft = new THREE.Mesh(
    new THREE.CylinderGeometry(0.28, 0.48, 1.1, 36),
    marbMat
  );
  shaft.position.y = 1.05;
  shaft.castShadow = true;
  pieceGroup.add(shaft);

  // Gold band around shaft
  const band1 = new THREE.Mesh(
    new THREE.TorusGeometry(0.34, 0.045, 12, 48),
    goldMat
  );
  band1.rotation.x = Math.PI / 2;
  band1.position.y = 0.72;
  pieceGroup.add(band1);

  const band2 = new THREE.Mesh(
    new THREE.TorusGeometry(0.32, 0.04, 12, 48),
    goldMat
  );
  band2.rotation.x = Math.PI / 2;
  band2.position.y = 1.35;
  pieceGroup.add(band2);

  // Upper shoulder
  const shoulder = new THREE.Mesh(
    new THREE.CylinderGeometry(0.52, 0.30, 0.22, 40),
    marbMat
  );
  shoulder.position.y = 1.71;
  shoulder.castShadow = true;
  pieceGroup.add(shoulder);

  // Neck
  const neck = new THREE.Mesh(
    new THREE.CylinderGeometry(0.20, 0.24, 0.32, 32),
    marbMat
  );
  neck.position.y = 1.98;
  neck.castShadow = true;
  pieceGroup.add(neck);

  // Gold neck ring
  const neckRing = new THREE.Mesh(
    new THREE.TorusGeometry(0.24, 0.05, 12, 40),
    accentMat
  );
  neckRing.rotation.x = Math.PI / 2;
  neckRing.position.y = 2.08;
  pieceGroup.add(neckRing);

  // Crown base sphere
  const crownBase = new THREE.Mesh(
    new THREE.SphereGeometry(0.38, 36, 36),
    marbMat
  );
  crownBase.position.y = 2.52;
  crownBase.castShadow = true;
  pieceGroup.add(crownBase);

  // Crown cross — vertical
  const crossV = new THREE.Mesh(
    new THREE.BoxGeometry(0.095, 0.64, 0.095),
    goldMat
  );
  crossV.position.y = 2.95;
  crossV.castShadow = true;
  pieceGroup.add(crossV);

  // Crown cross — horizontal
  const crossH = new THREE.Mesh(
    new THREE.BoxGeometry(0.5, 0.095, 0.095),
    goldMat
  );
  crossH.position.y = 3.08;
  crossH.castShadow = true;
  pieceGroup.add(crossH);

  // Crown orb at top
  const orb = new THREE.Mesh(
    new THREE.SphereGeometry(0.11, 24, 24),
    accentMat
  );
  orb.position.y = 3.32;
  pieceGroup.add(orb);

  // Base shadow disc (decorative)
  const shadowDisc = new THREE.Mesh(
    new THREE.CylinderGeometry(1.05, 1.05, 0.03, 48),
    new THREE.MeshStandardMaterial({ color: 0x000000, metalness: 0, roughness: 1, opacity: 0.3, transparent: true })
  );
  shadowDisc.position.y = -0.01;
  pieceGroup.add(shadowDisc);

  // Center entire piece
  pieceGroup.position.y = -1.2;
  scene.add(pieceGroup);

  /* ── Lights ── */
  // Ambient
  const ambient = new THREE.AmbientLight(0xffffff, 0.25);
  scene.add(ambient);

  // Key light (warm amber)
  const keyLight = new THREE.DirectionalLight(0xf0d090, 2.4);
  keyLight.position.set(3, 5, 4);
  keyLight.castShadow = true;
  keyLight.shadow.mapSize.set(1024, 1024);
  scene.add(keyLight);

  // Fill light (forest green)
  const fillLight = new THREE.PointLight(0x2d6a4f, 2.2, 20);
  fillLight.position.set(-4, 2, 2);
  scene.add(fillLight);

  // Rim light (champagne)
  const rimLight = new THREE.PointLight(0xf2c572, 1.2, 15);
  rimLight.position.set(0, 4, -4);
  scene.add(rimLight);

  // Moving accent light
  const movingLight = new THREE.PointLight(0x52b788, 1.0, 12);
  scene.add(movingLight);

  /* ── Mouse interaction ── */
  let mouseX = 0, mouseY = 0;
  const canvasRect = canvas.getBoundingClientRect();

  canvas.addEventListener('mousemove', (e) => {
    const rect = canvas.getBoundingClientRect();
    mouseX = ((e.clientX - rect.left) / rect.width - 0.5) * 2;
    mouseY = -((e.clientY - rect.top) / rect.height - 0.5) * 2;
  });

  canvas.addEventListener('mouseleave', () => {
    mouseX = 0;
    mouseY = 0;
  });

  /* ── Resize handler ── */
  function onResize() {
    const w = container.clientWidth;
    const h = container.clientHeight;
    camera.aspect = w / h;
    camera.updateProjectionMatrix();
    renderer.setSize(w, h);
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  }
  window.addEventListener('resize', onResize);

  /* ── Animation loop ── */
  let targetRotY = 0;
  let currentRotY = 0;

  function animate(t) {
    requestAnimationFrame(animate);

    const time = t * 0.001;

    // Auto-rotate + mouse influence
    targetRotY = time * 0.35 + mouseX * 0.4;
    currentRotY += (targetRotY - currentRotY) * 0.04;
    pieceGroup.rotation.y = currentRotY;

    // Subtle tilt toward mouse
    pieceGroup.rotation.x += (mouseY * 0.08 - pieceGroup.rotation.x) * 0.05;

    // Float up/down
    pieceGroup.position.y = -1.2 + Math.sin(time * 0.9) * 0.12;

    // Moving accent light orbit
    movingLight.position.x = Math.sin(time * 0.7) * 3.5;
    movingLight.position.z = Math.cos(time * 0.7) * 3.5;
    movingLight.position.y = 2 + Math.sin(time * 0.5) * 1;
    movingLight.intensity = 0.8 + Math.sin(time * 1.2) * 0.3;

    // Key light gentle sweep
    keyLight.position.x = 3 + Math.sin(time * 0.3) * 1;

    renderer.render(scene, camera);
  }

  animate(0);
})();


/* ─────────────────────────────────────────
   CURRICULUM LEVEL INTERACTIONS
───────────────────────────────────────── */
const levelData = {
  1: {
    icon: '♙', label: 'Level 01 — Pawn', title: 'Beginner Foundations',
    desc: 'The journey of every chess master begins here. Students learn how each piece moves, the objective of the game, and the fundamentals of safe play. We use engaging games, puzzles, and group exercises to build confidence and a love for the game from the very first session.',
    topics: ['Board Setup', 'Piece Movements', 'Check & Checkmate', 'Basic Opening Rules'],
    tags: ['Ages 5–8', 'Beginner', '12-Week Program']
  },
  2: {
    icon: '♘', label: 'Level 02 — Knight', title: 'Tactics & Coordination',
    desc: 'Students who know how the pieces move now learn how to make them work together. Level 2 introduces the building blocks of tactical play: forks, pins, skewers, discovered attacks, and the art of combining pieces to deliver decisive blows.',
    topics: ['Forks & Pins', 'Skewers', 'Piece Coordination', 'Tactical Patterns'],
    tags: ['Ages 8–12', 'Intermediate', '16-Week Program']
  },
  3: {
    icon: '♗', label: 'Level 03 — Bishop', title: 'Middle-Game Strategy',
    desc: 'With tactics mastered, students now learn how to think long-term. Level 3 dives into pawn structures, positional concepts, how to form plans, exploit weaknesses, and control key squares — the hallmarks of a true strategist.',
    topics: ['Pawn Structure', 'Open Files', 'Weak Squares', 'Strategic Planning'],
    tags: ['Ages 10–14', 'Club Level', '20-Week Program']
  },
  4: {
    icon: '♖', label: 'Level 04 — Rook', title: 'Endgame Mastery',
    desc: 'Many games are won or lost in the endgame. Level 4 teaches students to activate the king, dominate open files with rooks, navigate pawn races, and convert material advantages — turning promising positions into victories.',
    topics: ['King Activation', 'Rook Endgames', 'Pawn Promotion', 'Converting Edges'],
    tags: ['Ages 12–16', 'Competitive', '20-Week Program']
  },
  5: {
    icon: '♔', label: 'Level 05 — King', title: 'Advanced Masterclasses',
    desc: 'The pinnacle of the Kanata curriculum. Level 5 is reserved for serious competitors ready to study grandmaster games, analyze complex positions at depth, develop opening repertoires, and compete in national and provincial tournaments.',
    topics: ['Opening Repertoire', 'GM Game Analysis', 'Tournament Prep', 'Deep Calculation'],
    tags: ['Ages 14+', 'Advanced / Rated', 'Ongoing']
  }
};

function setActiveLevel(level) {
  // Update cards
  document.querySelectorAll('.level-card').forEach(c => {
    c.classList.toggle('active', parseInt(c.dataset.level) === level);
  });

  const data = levelData[level];
  const panel = document.getElementById('level-detail-panel');

  // Fade out
  panel.classList.remove('visible');

  setTimeout(() => {
    document.getElementById('detail-icon').textContent = data.icon;
    document.getElementById('detail-label').textContent = data.label;
    document.getElementById('detail-title').textContent = data.title;
    document.getElementById('detail-desc').textContent = data.desc;

    // Topics
    const topicsEl = document.getElementById('detail-topics');
    topicsEl.parentElement.innerHTML = data.topics.map(t =>
      `<div class="rounded-lg px-4 py-3" style="border:1px solid rgba(45,106,79,0.35); background:rgba(27,67,50,0.25);">
        <div class="text-gold text-xs mb-1">✦</div>
        <div class="text-ivory text-xs font-medium">${t}</div>
       </div>`
    ).join('');
    topicsEl.parentElement.className = 'grid grid-cols-2 md:grid-cols-4 gap-4';

    // Tags
    const tagsEl = document.getElementById('detail-tags');
    tagsEl.innerHTML = data.tags.map(tag =>
      `<span class="px-3 py-1 rounded-full text-xs" style="border:1px solid rgba(212,168,83,0.3); color:var(--champagne); background:rgba(27,67,50,0.3);">${tag}</span>`
    ).join('');

    // Fade in
    panel.classList.add('visible');
  }, 180);
}

// Attach click events
document.querySelectorAll('.level-card').forEach(card => {
  card.addEventListener('click', () => {
    setActiveLevel(parseInt(card.dataset.level));
  });
});


/* ─────────────────────────────────────────
   TABS
───────────────────────────────────────── */
function switchTab(tab) {
  ['classes', 'tournament'].forEach(t => {
    document.getElementById(`panel-${t}`).classList.toggle('active', t === tab);
    const btn = document.getElementById(`tab-${t}`);
    btn.classList.toggle('active', t === tab);
    btn.classList.toggle('text-ivory', t === tab);
    btn.classList.toggle('text-slate', t !== tab);
  });
}


/* ─────────────────────────────────────────
   FORM SUBMIT
───────────────────────────────────────── */
function submitForm(e, type) {
  e.preventDefault();
  const form = document.getElementById(`form-${type}`);
  const success = document.getElementById(`success-${type}`);
  form.style.display = 'none';
  success.classList.add('show');
}

function resetForm(type) {
  const form = document.getElementById(`form-${type}`);
  const success = document.getElementById(`success-${type}`);
  form.reset();
  form.style.display = '';
  success.classList.remove('show');
}

/* ─────────────────────────────────────────
   SCHOOL FIELD TOGGLE
───────────────────────────────────────── */
function toggleSchoolField() {
  const type = document.getElementById('reg-type').value;
  const schoolField = document.getElementById('school-name-field');
  const schoolInput = document.getElementById('school-name-input');
  const isSchool = type === 'school';
  schoolField.style.display = isSchool ? '' : 'none';
  schoolInput.required = isSchool;
}
// Initialize hidden
document.getElementById('school-name-field').style.display = 'none';


/* ─────────────────────────────────────────
   SCROLL REVEAL (lightweight)
───────────────────────────────────────── */
const observer = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      e.target.style.animationPlayState = 'running';
    }
  });
}, { threshold: 0.12 });

document.querySelectorAll('.anim-fade-up, .anim-slide-left').forEach(el => {
  el.style.animationPlayState = 'paused';
  observer.observe(el);
});
</script>

</body>
</html>
