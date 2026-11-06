<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>VZ · Trading journal, analytics and discipline</title>
<meta name="description" content="VZ is a trading journal with session analytics, mindset check-ins, and a discipline tracker. Log every trade and review what really works.">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=IBM+Plex+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
/* ---------- Tokens: near-black base with pale light accents ---------- */
:root {
  --bg: #05070c; --bg-2: #0a0e16; --panel: #0f141e; --line: rgba(233, 239, 255, .1);
  --ink: #e9efff; --muted: rgba(233, 239, 255, .6); --dim: rgba(233, 239, 255, .4);
  --glow: #9fd8ff; --gain: #4ade80; --loss: #f87171; --white: #ffffff;
  --radius: 18px;
}
*, *::before, *::after { box-sizing: border-box; }
html { scroll-behavior: smooth; scroll-padding-top: 84px; }
body { margin: 0; background: var(--bg); color: var(--ink); font: 16px/1.6 "Inter", system-ui, sans-serif; overflow-x: hidden; }
a { color: inherit; text-decoration: none; }
h1, h2, h3 { line-height: 1.1; letter-spacing: -.025em; margin: 0; }
.wrap { width: min(1180px, 92%); margin-inline: auto; }
.sec { padding: clamp(72px, 10vw, 120px) 0; position: relative; }
.kicker { font-size: .72rem; letter-spacing: .38em; text-transform: uppercase; color: var(--glow); font-weight: 600; }
.mono { font-family: "IBM Plex Mono", ui-monospace, monospace; font-variant-numeric: tabular-nums; }
.pos { color: var(--gain); } .neg { color: var(--loss); }
:focus-visible { outline: 2px solid var(--glow); outline-offset: 3px; border-radius: 6px; }

/* ---------- Buttons ---------- */
.btn { display: inline-flex; align-items: center; gap: 8px; padding: 13px 22px; border-radius: 999px; font-weight: 700; font-size: .95rem; border: 1px solid transparent; cursor: pointer; transition: transform .25s, box-shadow .3s, background .25s, border-color .25s; }
.btn-white { background: var(--white); color: #05070c; }
.btn-white:hover { transform: translateY(-2px); box-shadow: 0 0 36px rgba(159, 216, 255, .35); }
.btn-line { border-color: var(--line); color: var(--ink); background: rgba(255, 255, 255, .03); }
.btn-line:hover { border-color: var(--glow); background: rgba(159, 216, 255, .08); }
.btn-sm { padding: 9px 16px; font-size: .85rem; }

/* ---------- Ticker tape (sample data) ---------- */
.ticker { border-bottom: 1px solid var(--line); background: var(--bg-2); overflow: hidden; white-space: nowrap; font-size: .8rem; }
.ticker-track { display: inline-flex; gap: 40px; padding: 10px 0; animation: scroll 38s linear infinite; }
.ticker span { color: var(--muted); }
.ticker b { color: var(--ink); font-weight: 600; margin-right: 6px; }
@keyframes scroll { to { transform: translateX(-50%); } }

/* ---------- Nav ---------- */
.nav { position: sticky; top: 0; z-index: 50; background: rgba(5, 7, 12, .75); backdrop-filter: blur(14px); -webkit-backdrop-filter: blur(14px); border-bottom: 1px solid var(--line); }
.nav .wrap { display: flex; align-items: center; justify-content: space-between; height: 68px; gap: 20px; }
.logo { display: inline-flex; align-items: center; gap: 12px; font-weight: 800; }
.mark { width: 36px; height: 36px; border-radius: 10px; display: grid; place-items: center; font-size: 12px; letter-spacing: .24em; text-indent: .24em; border: 1px solid rgba(233, 239, 255, .28); background: linear-gradient(160deg, rgba(255,255,255,.12), rgba(255,255,255,.02)); box-shadow: 0 0 22px rgba(159, 216, 255, .18); }
.logo .word { letter-spacing: .32em; font-size: .85rem; }
.nav-links { display: flex; gap: 26px; font-size: .9rem; color: var(--muted); }
.nav-links a { position: relative; transition: color .2s; }
.nav-links a:hover, .nav-links a.active { color: var(--ink); }
.nav-links a.active::after { content: ""; position: absolute; left: 0; right: 0; bottom: -8px; height: 2px; background: var(--glow); border-radius: 2px; }
.nav-actions { display: flex; align-items: center; gap: 10px; }
.nav-login { font-size: .9rem; color: var(--muted); padding: 8px 12px; }
.nav-login:hover { color: var(--ink); }
.burger { display: none; width: 40px; height: 40px; border-radius: 10px; border: 1px solid var(--line); background: transparent; color: var(--ink); cursor: pointer; place-items: center; }
.progress { position: fixed; top: 0; left: 0; right: 0; height: 2px; z-index: 60; }
.progress i { display: block; height: 100%; width: 0; background: linear-gradient(90deg, var(--glow), #ffffff); }

/* ---------- Hero ---------- */
.hero { padding: clamp(56px, 8vw, 96px) 0 clamp(64px, 8vw, 90px); position: relative; overflow: hidden; }
.hero::before { content: ""; position: absolute; left: 50%; top: -220px; width: 1100px; height: 700px; transform: translateX(-50%); background: radial-gradient(ellipse at center, rgba(159, 216, 255, .18), transparent 60%); pointer-events: none; }
.hero-grid { display: grid; grid-template-columns: 1fr 1.05fr; gap: 56px; align-items: center; position: relative; }
.hero h1 { font-size: clamp(2.6rem, 6vw, 4.8rem); font-weight: 800; margin: 16px 0 20px; }
.hero h1 .soft { color: rgba(233, 239, 255, .5); font-weight: 300; }
.lede { font-size: clamp(1rem, 1.5vw, 1.15rem); color: var(--muted); max-width: 34rem; }
.hero-cta { display: flex; gap: 12px; flex-wrap: wrap; margin-top: 30px; }
.hero-proof { display: flex; gap: 22px; flex-wrap: wrap; margin-top: 28px; font-size: .85rem; color: var(--dim); }
.hero-proof span::before { content: "✓ "; color: var(--gain); }

/* Dashboard mockup in the hero */
.mock { background: linear-gradient(180deg, #111826, #0b0f18); border: 1px solid var(--line); border-radius: 22px; padding: 22px; box-shadow: 0 40px 90px rgba(0, 0, 0, .55), 0 0 0 1px rgba(159, 216, 255, .05); }
.mock-top { display: flex; justify-content: space-between; align-items: center; margin-bottom: 14px; font-size: .8rem; color: var(--muted); }
.mock-dots { display: flex; gap: 6px; } .mock-dots i { width: 9px; height: 9px; border-radius: 50%; background: rgba(233, 239, 255, .18); display: block; }
.mock-stats { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; margin-bottom: 14px; }
.mock-stat { background: rgba(255, 255, 255, .03); border: 1px solid var(--line); border-radius: 12px; padding: 12px; }
.mock-stat small { display: block; color: var(--dim); font-size: .7rem; letter-spacing: .08em; text-transform: uppercase; }
.mock-stat b { font: 500 1.15rem "IBM Plex Mono", monospace; }
.mock canvas { width: 100%; height: 190px; display: block; border-radius: 12px; background: rgba(255, 255, 255, .02); }
.mock-foot { display: flex; justify-content: space-between; font-size: .75rem; color: var(--dim); margin-top: 10px; }

/* ---------- Scroll sequence: a trading story pinned in the page ---------- */
.seq { height: 700vh; position: relative; }
.seq-stage { position: sticky; top: 0; height: 100vh; overflow: hidden; background-color: var(--bg); background-image: linear-gradient(rgba(159, 216, 255, .045) 1px, transparent 1px); background-size: 100% 56px; }
.seq-stage::after { content: ""; position: absolute; inset: 0; background: radial-gradient(ellipse at 60% 50%, rgba(159, 216, 255, .08), transparent 60%); pointer-events: none; }
#seqFx { position: absolute; inset: 0; width: 100%; height: 100%; }
.seq-counter { position: absolute; left: 50%; top: 42%; transform: translate(-50%, -50%); text-align: center; z-index: 2; pointer-events: none; }
.seq-num { font-size: clamp(5rem, 16vw, 12rem); font-weight: 800; letter-spacing: -.04em; line-height: 1; font-variant-numeric: tabular-nums; text-shadow: 0 0 40px rgba(159, 216, 255, .35); }
.seq-unit { font-size: .75rem; letter-spacing: .4em; color: var(--muted); margin-top: 14px; text-transform: uppercase; }
.seq-block { position: absolute; z-index: 3; left: 50%; top: 80%; max-width: min(640px, 88vw); text-align: center; opacity: 0; transform: translate(-50%, var(--y, 0px)); will-change: opacity, transform; }
.seq-block p { color: var(--muted); font-size: clamp(1rem, 1.5vw, 1.15rem); margin: 0; }
#seqTagline p { color: var(--ink); font-size: clamp(1.1rem, 2.4vw, 1.6rem); font-weight: 300; }
.seq-block h2 { font-size: clamp(2rem, 4.6vw, 3.8rem); font-weight: 800; letter-spacing: -.03em; line-height: 1.05; margin: 14px 0 18px; }
.seq-left { left: 6vw; top: 50%; max-width: min(420px, 40vw); text-align: left; transform: translateY(calc(-50% + var(--y, 0px))); }
.seq-btns { display: flex; gap: 12px; margin-top: 26px; flex-wrap: wrap; }
.seq-cue { position: absolute; left: 50%; bottom: 22px; transform: translateX(-50%); font-size: .6rem; letter-spacing: .5em; color: var(--muted); z-index: 3; }
@media (max-width: 760px) { .seq-left { left: 50%; top: 60%; max-width: 88vw; text-align: center; transform: translate(-50%, var(--y, 0px)); } .seq-block { top: 84%; } }
@media (prefers-reduced-motion: reduce) { .seq { height: auto; } .seq-stage { position: relative; height: auto; min-height: 100vh; } .seq-block, .seq-counter { position: relative; left: auto; top: auto; transform: none; opacity: 1 !important; margin: 40px auto; } #seqFx { display: none; } }

/* ---------- Count-up stats ---------- */
.stats { border-block: 1px solid var(--line); background: var(--bg-2); }
.stats-grid { display: grid; grid-template-columns: repeat(4, 1fr); }
.stat { padding: 34px 20px; text-align: center; border-right: 1px solid var(--line); }
.stat:last-child { border-right: 0; }
.stat .n { font: 500 clamp(1.8rem, 3.4vw, 2.6rem) "IBM Plex Mono", monospace; color: var(--white); }
.stat .l { font-size: .85rem; color: var(--muted); margin-top: 4px; }

/* ---------- Section heads ---------- */
.head { max-width: 760px; margin-bottom: 52px; }
.head h2 { font-size: clamp(2.1rem, 4.6vw, 3.6rem); font-weight: 800; margin: 14px 0; }
.head p { color: var(--muted); font-size: 1.08rem; margin: 0; }
.split { display: grid; grid-template-columns: 1fr 1.1fr; gap: 56px; align-items: center; }
.split h2 { font-size: clamp(2rem, 4.4vw, 3.4rem); font-weight: 800; margin: 14px 0; }

/* ---------- Tabbed feature explorer ---------- */
.tabs { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 28px; }
.tab { padding: 10px 18px; border-radius: 999px; border: 1px solid var(--line); background: transparent; color: var(--muted); font: inherit; font-weight: 600; cursor: pointer; transition: all .25s; }
.tab:hover { color: var(--ink); }
.tab[aria-selected="true"] { background: var(--white); color: #05070c; border-color: var(--white); }
.tab-panel { display: none; grid-template-columns: 1fr 1.1fr; gap: 40px; align-items: center; }
.tab-panel.on { display: grid; animation: fadeUp .6s ease both; }
@keyframes fadeUp { from { opacity: 0; transform: translateY(18px); } to { opacity: 1; transform: none; } }
.tab-panel h3 { font-size: clamp(1.6rem, 3vw, 2.3rem); margin-bottom: 14px; font-weight: 800; }
.tab-panel p { color: var(--muted); margin: 0 0 18px; }
.ticks { list-style: none; padding: 0; margin: 0; display: grid; gap: 12px; }
.ticks li { display: flex; gap: 12px; color: var(--ink); font-size: .95rem; }
.ticks li::before { content: ""; flex: none; width: 18px; height: 18px; margin-top: 3px; border-radius: 50%; background: radial-gradient(circle, var(--glow) 0 3px, transparent 4px), rgba(159, 216, 255, .12); }
.panel-box { background: var(--panel); border: 1px solid var(--line); border-radius: var(--radius); padding: 22px; min-height: 260px; }
.hint { font-size: .8rem; color: var(--dim); margin-top: 16px; }

/* ---------- Bars (analytics and entry models) ---------- */
.bars { display: grid; gap: 14px; }
.bar-row { display: grid; grid-template-columns: 110px 1fr 84px; gap: 12px; align-items: center; font-size: .88rem; cursor: pointer; }
.bar-row span:first-child { color: var(--muted); }
.bar-track { height: 12px; border-radius: 99px; background: rgba(255, 255, 255, .04); overflow: hidden; }
.bar-fill { height: 100%; border-radius: 99px; width: 0; transition: width 1s cubic-bezier(.2, .7, .1, 1), filter .2s, opacity .2s; }
.bar-row:hover .bar-fill { filter: brightness(1.25); }
.bar-row.dim .bar-fill { opacity: .35; }
.bar-val { text-align: right; font-family: "IBM Plex Mono", monospace; }

/* ---------- Discipline checklist ---------- */
.score-row { display: flex; justify-content: space-between; align-items: baseline; margin-bottom: 8px; }
.score-row b { font: 500 2rem "IBM Plex Mono", monospace; }
.meter { height: 10px; border-radius: 99px; background: rgba(255, 255, 255, .05); overflow: hidden; margin-bottom: 18px; }
.meter i { display: block; height: 100%; width: 0; background: linear-gradient(90deg, var(--glow), var(--white)); border-radius: 99px; transition: width .5s ease; }
.check { display: flex; align-items: center; gap: 12px; padding: 11px 4px; border-bottom: 1px solid var(--line); cursor: pointer; user-select: none; font-size: .95rem; }
.check .box { width: 22px; height: 22px; border-radius: 6px; border: 1.5px solid rgba(233, 239, 255, .3); display: grid; place-items: center; font-size: 13px; font-weight: 700; color: #05070c; transition: all .2s; flex: none; }
.check.on .box { background: var(--gain); border-color: var(--gain); }
.check.on span:last-child { color: var(--muted); text-decoration: line-through; text-decoration-color: rgba(233, 239, 255, .3); }

/* ---------- Mindset selector ---------- */
.moods { display: grid; grid-template-columns: repeat(5, 1fr); gap: 8px; margin-bottom: 18px; }
.mood { padding: 14px 6px; border-radius: 12px; border: 1px solid var(--line); background: transparent; color: var(--muted); font: inherit; font-weight: 600; cursor: pointer; transition: all .2s; font-size: .85rem; }
.mood:hover { color: var(--ink); }
.mood[aria-pressed="true"] { background: var(--white); color: #05070c; border-color: var(--white); }
.tags { display: flex; flex-wrap: wrap; gap: 8px; margin-bottom: 18px; }
.tagchip { padding: 7px 13px; border-radius: 999px; border: 1px solid var(--line); background: rgba(255, 255, 255, .03); color: var(--muted); font: inherit; font-size: .83rem; cursor: pointer; transition: all .2s; }
.tagchip[aria-pressed="true"] { border-color: var(--glow); color: var(--ink); background: rgba(159, 216, 255, .12); }
.reminder { background: rgba(159, 216, 255, .06); border: 1px solid rgba(159, 216, 255, .2); border-radius: 14px; padding: 16px 18px; font-size: .95rem; min-height: 80px; transition: opacity .3s; }

/* ---------- Benefits, partners, FAQ ---------- */
.ben-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; }
.ben { padding: 26px; border-radius: var(--radius); border: 1px solid var(--line); background: linear-gradient(180deg, rgba(255,255,255,.05), rgba(255,255,255,.01)); transition: transform .3s, border-color .3s; }
.ben:hover { transform: translateY(-4px); border-color: rgba(159,216,255,.45); }
.ben .no { font: 500 .72rem "IBM Plex Mono", monospace; color: var(--glow); margin-bottom: 14px; letter-spacing: .15em; }
.ben h3 { font-size: 1.1rem; margin-bottom: 8px; }
.ben p { margin: 0; color: var(--muted); font-size: .92rem; }
.partners { display: grid; grid-template-columns: repeat(6, 1fr); gap: 12px; }
.partner { border: 1px dashed var(--line); border-radius: 12px; padding: 26px 8px; text-align: center; color: var(--dim); font-size: .75rem; letter-spacing: .1em; text-transform: uppercase; }
.faq { max-width: 860px; margin: 0 auto; }
.faq details { border-bottom: 1px solid var(--line); padding: 20px 4px; }
.faq summary { cursor: pointer; font-weight: 600; font-size: 1.05rem; list-style: none; display: flex; justify-content: space-between; gap: 16px; align-items: center; }
.faq summary::-webkit-details-marker { display: none; }
.faq summary::after { content: "+"; font-size: 1.4rem; color: var(--glow); transition: transform .3s; }
.faq details[open] summary::after { transform: rotate(45deg); }
.faq details p { color: var(--muted); margin: 14px 0 0; font-size: .95rem; }

/* ---------- CTA band ---------- */
.cta { text-align: center; padding: clamp(80px, 11vw, 130px) 0; background: radial-gradient(ellipse at 50% 0%, rgba(159,216,255,.14), transparent 60%); border-top: 1px solid var(--line); }
.cta h2 { font-size: clamp(2.2rem, 5vw, 3.8rem); font-weight: 800; margin: 14px 0 18px; }
.cta p { color: var(--muted); margin: 0 auto 30px; max-width: 34rem; }
.hero-cta.center { justify-content: center; }

/* ---------- Footer ---------- */
.footer { border-top: 1px solid var(--line); padding: 56px 0 36px; background: var(--bg-2); }
.footer-grid { display: grid; grid-template-columns: 1.4fr 1fr 1fr 1fr; gap: 32px; }
.footer h4 { margin: 0 0 14px; font-size: .72rem; letter-spacing: .3em; text-transform: uppercase; color: var(--glow); font-weight: 600; }
.footer a { display: block; color: var(--muted); font-size: .92rem; margin-bottom: 9px; transition: color .2s; }
.footer a:hover { color: var(--ink); }
.legal { margin-top: 40px; padding-top: 22px; border-top: 1px solid var(--line); display: flex; justify-content: space-between; flex-wrap: wrap; gap: 12px; font-size: .8rem; color: var(--dim); }

/* ---------- Reveal on scroll ---------- */
.rv { opacity: 0; transform: translateY(26px); transition: opacity .8s ease, transform .8s cubic-bezier(.2,.7,.1,1); }
.rv.in { opacity: 1; transform: none; }

/* ---------- Responsive ---------- */
@media (max-width: 980px) {
  .hero-grid, .split, .tab-panel { grid-template-columns: 1fr; }
  .nav-links { display: none; }
  .burger { display: grid; }
  .nav-links.open { display: flex; flex-direction: column; position: absolute; top: 68px; left: 0; right: 0; padding: 18px 4%; background: var(--bg-2); border-bottom: 1px solid var(--line); gap: 16px; }
  .stats-grid { grid-template-columns: repeat(2, 1fr); }
  .stat:nth-child(2) { border-right: 0; }
  .stat:nth-child(-n+2) { border-bottom: 1px solid var(--line); }
  .ben-grid { grid-template-columns: repeat(2, 1fr); }
  .partners { grid-template-columns: repeat(3, 1fr); }
  .footer-grid { grid-template-columns: 1fr 1fr; }
}
@media (max-width: 620px) {
  .nav-login { padding: 8px 10px; }
  .ben-grid { grid-template-columns: 1fr; }
  .bar-row { grid-template-columns: 84px 1fr 72px; }
  .footer-grid { grid-template-columns: 1fr; }
  .mock-stats { grid-template-columns: 1fr 1fr; }
}
@media (prefers-reduced-motion: reduce) {
  html { scroll-behavior: auto; }
  .ticker-track, .tab-panel.on { animation: none; }
  .rv { opacity: 1; transform: none; transition: none; }
}

/* VZ logo (matches the app sidebar) */
.vz-mark { width: 36px; height: 36px; flex: none; overflow: visible; }
.vz-words { display: flex; flex-direction: column; }
.vz-name { font-family: Inter, system-ui, sans-serif; font-size: 16px; font-weight: 700; letter-spacing: .3em; color: #e9efff; line-height: 1; }
.vz-sub { font-family: Inter, system-ui, sans-serif; font-size: 7.5px; font-weight: 500; letter-spacing: .32em; color: #9fd8ff; opacity: .75; margin-top: 5px; }
</style>
</head>
<body>

<!-- Sticky navigation with active-section highlight -->
<header class="nav" id="top">
  <div class="wrap">
    <a href="#top" class="logo" aria-label="VZ home"><svg class="vz-mark" viewBox="0 0 48 48" aria-hidden="true"><rect x="8" y="28" width="6" height="12" fill="#9fd8ff"/><rect x="18" y="20" width="6" height="20" fill="#ffffff"/><rect x="28" y="12" width="6" height="28" fill="#9fd8ff"/><circle cx="40" cy="8" r="2.4" fill="#ffffff"/></svg><span class="vz-words"><span class="vz-name">VZ</span><span class="vz-sub">TRADING JOURNAL</span></span></a>
    <nav class="nav-links" id="links" aria-label="Primary">
      <a href="#features">Features</a>
      <a href="#analytics">Analytics</a>
      <a href="#discipline">Discipline</a>
      <a href="#mindset">Mindset</a>
      <a href="#faq">FAQ</a>
    </nav>
    <div class="nav-actions">
      <a class="nav-login" href="vz-offline.html">Log in</a>
      <a class="btn btn-white btn-sm" href="vz-offline.html">Open the app</a>
      <button class="burger" id="burger" aria-label="Open menu" aria-expanded="false" aria-controls="links">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M4 7h16M4 12h16M4 17h16"/></svg>
      </button>
    </div>
  </div>
</header>
<div class="progress" aria-hidden="true"><i id="prog"></i></div>

<!-- Scrolling ticker. Prices are sample values, not live quotes. -->
<div class="ticker" aria-label="Sample market ticker">
  <div class="ticker-track" id="tickerTrack"></div>
</div>

<main>

  <!-- HERO -->
  <section class="hero">
    <div class="wrap hero-grid">
      <div>
        <div class="kicker">Trading journal · analytics · discipline</div>
        <h1>Trade with structure.<br><span class="soft">Review with precision.</span></h1>
        <p class="lede">VZ turns every trade into data. Log sessions, rate your mindset, follow your rules, and see exactly which setups make money.</p>
        <div class="hero-cta">
          <a class="btn btn-line" href="#features">Explore features</a>
        </div>
        <div class="hero-proof"><span>Session analytics</span><span>Mindset check-ins</span><span>Multiple accounts</span><span>CSV import and export</span></div>
      </div>

      <!-- Dashboard mockup with an equity curve that draws itself (sample data) -->
      <div class="mock rv" aria-hidden="true">
        <div class="mock-top"><span>Dashboard · All accounts</span><span class="mock-dots"><i></i><i></i><i></i></span></div>
        <div class="mock-stats">
          <div class="mock-stat"><small>Net P&amp;L</small><b class="pos" id="mNet">$0</b></div>
          <div class="mock-stat"><small>Win rate</small><b id="mWin">0%</b></div>
          <div class="mock-stat"><small>Profit factor</small><b id="mPf">0.00</b></div>
        </div>
        <canvas id="equity" width="640" height="380"></canvas>
        <div class="mock-foot"><span>Sample data</span><span>1,042 sample trades</span></div>
      </div>
    </div>
  </section>

  <!-- SCROLL SEQUENCE: a trading story. Trade tickets become a candlestick chart,
       the chart turns in 3D, then it resolves into P&L by session. Pinned while you scroll. -->
  <section class="seq" id="seq" aria-label="Scroll sequence: from trades to a clear review">
    <div class="seq-stage">
      <canvas id="seqFx" aria-hidden="true"></canvas>
      <div class="seq-counter"><div class="seq-num" id="seqNum">3000</div><div class="seq-unit">Trades logged → one plan</div></div>
      <div class="seq-block" id="seqTagline"><p>Every session, every rule, every trade, held in one journal.</p></div>
      <div class="seq-block seq-left" id="seqRisk">
        <div class="kicker">Risk and structure</div>
        <h2>Less noise. Clear rules.</h2>
        <p>Measured from your first trade to your full review, with nothing left to memory.</p>
        <div class="seq-btns"><a class="btn btn-white btn-sm" href="#features">Explore features</a><a class="btn btn-line btn-sm" href="#discipline">See discipline</a></div>
      </div>
      <div class="seq-block seq-left" id="seqAnalytics">
        <div class="kicker">Analytics</div>
        <h2>Built for serial improvement.</h2>
        <p>See which sessions earn their place in your plan, and which ones to stop trading.</p>
        <div class="seq-btns"><a class="btn btn-white btn-sm" href="#analytics">See analytics</a></div>
      </div>
      <div class="seq-cue" id="seqCue">SCROLL</div>
    </div>
  </section>

  <!-- STATS: count-up on scroll -->
  <section class="stats">
    <div class="wrap stats-grid">
      <div class="stat"><div class="n" data-count="4">0</div><div class="l">Default sessions to start</div></div>
      <div class="stat"><div class="n" data-count="11">0</div><div class="l">Discipline rules to begin with</div></div>
      <div class="stat"><div class="n" data-count="7">0</div><div class="l">Mindset tags to choose from</div></div>
      <div class="stat"><div class="n" data-count="12">0</div><div class="l">Weeks in your consistency heat map</div></div>
    </div>
  </section>

  <!-- FEATURE EXPLORER -->
  <section class="sec" id="features">
    <div class="wrap">
      <div class="head rv">
        <div class="kicker">Features</div>
        <h2>Everything a serious trader tracks, in one place.</h2>
        <p>Pick a module to see how it works.</p>
      </div>
      <div class="tabs" role="tablist" aria-label="Feature modules">
        <button class="tab" role="tab" aria-selected="true" aria-controls="p-journal" id="t-journal">Journal</button>
        <button class="tab" role="tab" aria-selected="false" aria-controls="p-accounts" id="t-accounts">Accounts</button>
        <button class="tab" role="tab" aria-selected="false" aria-controls="p-models" id="t-models">Entry models</button>
        <button class="tab" role="tab" aria-selected="false" aria-controls="p-data" id="t-data">Data</button>
      </div>

      <div class="tab-panel on" role="tabpanel" id="p-journal" aria-labelledby="t-journal">
        <div>
          <h3>A journal built for every trade</h3>
          <p>Record the symbol, date, session, P&amp;L, entry model, a written reflection, and screenshots. Every trade stays connected to its context.</p>
          <ul class="ticks">
            <li>Reflections and files on each trade</li>
            <li>Calendar view with daily totals and one-click day drill-down</li>
            <li>Daily journal notes for every trading day</li>
          </ul>
        </div>
        <div class="panel-box">
          <div class="mono" style="font-size:.8rem;color:var(--dim);margin-bottom:10px">Recent trades · sample</div>
          <div class="mono" style="font-size:.9rem;line-height:2.1">
            <div>Mon 11/03 &nbsp; NY &nbsp; NVDA &nbsp; <span class="pos">+$412</span></div>
            <div>Mon 11/03 &nbsp; NY &nbsp; MES &nbsp; <span class="neg">-$95</span></div>
            <div>Tue 11/04 &nbsp; London &nbsp; TSLA &nbsp; <span class="pos">+$138</span></div>
            <div>Tue 11/04 &nbsp; Asia &nbsp; AAPL &nbsp; <span class="neg">-$60</span></div>
          </div>
        </div>
      </div>

      <div class="tab-panel" role="tabpanel" id="p-accounts" aria-labelledby="t-accounts">
        <div>
          <h3>Every account, one dashboard</h3>
          <p>Track prop firm and personal accounts separately, or switch to all accounts at once. Charts and stats follow your selection.</p>
          <ul class="ticks">
            <li>Unlimited accounts, renamed and selected from the side panel</li>
            <li>Stats and calendars update to the account you pick</li>
            <li>Accounts with trades are protected from accidental deletion</li>
          </ul>
        </div>
        <div class="panel-box">
          <div class="mono" style="font-size:.9rem;line-height:2.4">
            <div>Prop challenge A &nbsp; <span class="pos">+$18,420</span></div>
            <div>Personal swing &nbsp; <span class="pos">+$6,820</span></div>
            <div>Prop challenge B &nbsp; <span class="neg">-$3,310</span></div>
            <div style="color:var(--glow);margin-top:10px">All accounts &nbsp; <span class="pos">+$21,930</span></div>
          </div>
          <p class="hint">Sample figures.</p>
        </div>
      </div>

      <div class="tab-panel" role="tabpanel" id="p-models" aria-labelledby="t-models">
        <div>
          <h3>Know which setups pay</h3>
          <p>Create an entry model for each setup you trade. Link every trade to it, then review the results of each model side by side.</p>
          <ul class="ticks">
            <li>Model pages list every trade that used the setup</li>
            <li>Win rate and profit factor per model, updated as you log</li>
            <li>Add a trade straight from a model page</li>
          </ul>
        </div>
        <div class="panel-box">
          <div class="bars" id="modelBars"></div>
          <p class="hint">Sample data.</p>
        </div>
      </div>

      <div class="tab-panel" role="tabpanel" id="p-data" aria-labelledby="t-data">
        <div>
          <h3>Your data, yours to keep</h3>
          <p>Import trades from a CSV, export everything whenever you want, and keep full control of your records.</p>
          <ul class="ticks">
            <li>Import a CSV with date, symbol, and net P&amp;L</li>
            <li>Export a full CSV with session, model, and reflection</li>
            <li>Journal data stays in your browser on the device you use</li>
          </ul>
        </div>
        <div class="panel-box">
          <div class="mono" style="font-size:.85rem;line-height:1.9;color:var(--muted)">
            date,symbol,net_pnl<br>2026-11-03,NVDA,412<br>2026-11-03,MES,-95<br>2026-11-04,TSLA,138
          </div>
          <p class="hint">Example import file.</p>
        </div>
      </div>
    </div>
  </section>

  <!-- ANALYTICS -->
  <section class="sec" id="analytics" style="background:var(--bg-2);border-block:1px solid var(--line)">
    <div class="wrap split">
      <div class="rv">
        <div class="kicker">Analytics</div>
        <h2>See where your money is made.</h2>
        <p class="lede">Break your results down by session, symbol, weekday, and cumulative equity. Hover a row to focus on it.</p>
        <ul class="ticks" style="margin-top:22px">
          <li>Date range presets: last 30 days, this month, this year, or any custom range</li>
          <li>Heat map of consistency across 12 weeks</li>
          <li>Win rate, profit factor, and average win to loss</li>
        </ul>
      </div>
      <div class="panel-box rv">
        <div class="kicker" style="margin-bottom:18px">Performance by session · sample</div>
        <div class="bars" id="sessionBars"></div>
        <p class="hint">Hover a row to highlight it.</p>
      </div>
    </div>
  </section>

  <!-- DISCIPLINE -->
  <section class="sec" id="discipline">
    <div class="wrap split" style="align-items:start">
      <div class="rv">
        <div class="kicker">Discipline</div>
        <h2>Turn rules into a daily score.</h2>
        <p class="lede">Prepare, trade, and reflect against a checklist you design. Tick items off and watch the score and streak respond.</p>
        <ul class="ticks" style="margin-top:22px">
          <li>Custom rules in four stages, including habit goals</li>
          <li>Current streak and best streak, counted from a clear 80 percent threshold</li>
          <li>Adherence per rule across the last 30 days</li>
        </ul>
      </div>
      <div class="panel-box rv">
        <div class="score-row"><span class="kicker">Today's score</span><b id="scoreVal">0%</b></div>
        <div class="meter"><i id="scoreBar"></i></div>
        <div id="checklist"></div>
        <p class="hint">Try it. Your choices here are not saved.</p>
      </div>
    </div>
  </section>

  <!-- MINDSET -->
  <section class="sec" id="mindset" style="background:var(--bg-2);border-block:1px solid var(--line)">
    <div class="wrap split">
      <div class="panel-box rv">
        <div class="kicker" style="margin-bottom:14px">Rate your state before you trade</div>
        <div class="moods" id="moods" role="group" aria-label="Focus rating"></div>
        <div class="tags" id="tags" role="group" aria-label="Emotions"></div>
        <div class="reminder" id="reminder" aria-live="polite"></div>
      </div>
      <div class="rv">
        <div class="kicker">Mindset</div>
        <h2>Trade the plan, not the feeling.</h2>
        <p class="lede">Log your focus and the emotions behind each trade. When a feeling like revenge or FOMO shows up, VZ gives you a short reminder to pause.</p>
        <ul class="ticks" style="margin-top:22px">
          <li>Focus rating from rough to sharp</li>
          <li>Tags for emotions, including after a loss and feeling FOMO</li>
          <li>Rotating motivational quotes and reset reminders</li>
        </ul>
      </div>
    </div>
  </section>

  <!-- BENEFITS -->
  <section class="sec" id="benefits">
    <div class="wrap">
      <div class="head rv"><div class="kicker">Benefits</div><h2>Why traders use VZ.</h2></div>
      <div class="ben-grid">
        <article class="ben rv"><div class="no">01</div><h3>Fewer emotional trades</h3><p>Check-ins and reminders help you pause before the next click.</p></article>
        <article class="ben rv"><div class="no">02</div><h3>A clearer edge</h3><p>See which sessions, symbols, and models make money, and which ones don't.</p></article>
        <article class="ben rv"><div class="no">03</div><h3>Habits you can measure</h3><p>Daily rules, a heat map, and streaks turn discipline into a score.</p></article>
        <article class="ben rv"><div class="no">04</div><h3>Every record in one place</h3><p>Trades, reflections, files, and daily notes stay together.</p></article>
        <article class="ben rv"><div class="no">05</div><h3>Honest reviews</h3><p>Filter any period, from a single day to a full year, and compare accounts.</p></article>
        <article class="ben rv"><div class="no">06</div><h3>Your data, exportable</h3><p>Download a CSV of your trades whenever you want.</p></article>
      </div>
    </div>
  </section>

  <!-- PARTNERS: placeholders until real partners are added -->
  <section class="sec" id="partners" style="padding-top:0">
    <div class="wrap">
      <div class="head rv"><div class="kicker">Trusted partners</div><h2 style="font-size:clamp(1.8rem,3.6vw,2.6rem)">Built with traders and the teams who support them.</h2></div>
      <div class="partners">
        <div class="partner">Partner logo</div><div class="partner">Partner logo</div><div class="partner">Partner logo</div>
        <div class="partner">Partner logo</div><div class="partner">Partner logo</div><div class="partner">Partner logo</div>
      </div>
    </div>
  </section>

  <!-- FAQ -->
  <section class="sec" id="faq">
    <div class="wrap">
      <div class="head rv" style="text-align:center;margin-inline:auto"><div class="kicker">FAQ</div><h2 style="font-size:clamp(2rem,4.4vw,3.2rem)">Common questions</h2></div>
      <div class="faq">
        <details><summary>Does VZ connect to my broker?</summary><p>Not yet. Today you import trades from a CSV file with date, symbol, and net P&amp;L, and export your data at any time.</p></details>
        <details><summary>Where is my data stored?</summary><p>Trades, notes, and check-ins are saved in your browser on the device you use. Nothing is sent to a server by this page.</p></details>
        <details><summary>Can I track more than one account?</summary><p>Yes. Add as many accounts as you need, then view one at a time or all together on the dashboard.</p></details>
        <details><summary>Is VZ financial advice?</summary><p>No. VZ is a journal and a discipline tool. It doesn't recommend trades, and trading involves risk, including the loss of capital.</p></details>
        <details><summary>Does it work on a phone?</summary><p>The site and the app adapt to smaller screens. For the most detail, use a laptop or a larger screen.</p></details>
      </div>
    </div>
  </section>

  <!-- CTA -->
  <section class="cta">
    <div class="wrap">
      <div class="kicker">Ready when you are</div>
      <h2>Start with your next session.</h2>
      <p>Open the app, log your first trade, and see your numbers in one place.</p>
      <div class="hero-cta center">
        <a class="btn btn-line" href="#features">See features</a>
      </div>
    </div>
  </section>

</main>

<footer class="footer">
  <div class="wrap">
    <div class="footer-grid">
      <div>
        <a href="#top" class="logo" style="display:inline-flex;margin-bottom:14px"><svg class="vz-mark" viewBox="0 0 48 48" aria-hidden="true"><rect x="8" y="28" width="6" height="12" fill="#9fd8ff"/><rect x="18" y="20" width="6" height="20" fill="#ffffff"/><rect x="28" y="12" width="6" height="28" fill="#9fd8ff"/><circle cx="40" cy="8" r="2.4" fill="#ffffff"/></svg><span class="vz-words"><span class="vz-name">VZ</span><span class="vz-sub">TRADING JOURNAL</span></span></a>
        <p style="color:var(--muted);font-size:.92rem;max-width:20rem;margin:0">A trading journal, analytics, and discipline tracker for traders who want to grow.</p>
      </div>
      <div><h4>Product</h4><a href="#features">Features</a><a href="#analytics">Analytics</a><a href="#discipline">Discipline</a><a href="#mindset">Mindset</a></div>
      <div><h4>Account</h4><a href="#faq">FAQ</a></div>
      <div><h4>Contact</h4><a href="mailto:hello@example.com">hello@example.com</a><a href="#">X</a><a href="#">Instagram</a><a href="#">YouTube</a><a href="#">Discord</a></div>
    </div>
    <div class="legal"><span>© <span id="yr"></span> VZ. All rights reserved.</span><span>Trading involves risk. VZ is a journal, not financial advice.</span></div>
  </div>
</footer>

<script>
/* ============ 1. Ticker tape (sample values) ============ */
(function ticker() {
  const items = [["NVDA", "412.00", "+1.2%"], ["MES", "5,812.25", "-0.3%"], ["TSLA", "238.40", "+0.8%"], ["AAPL", "221.10", "-0.4%"], ["NFLX", "612.90", "+0.6%"], ["MNQ", "20,340.50", "+0.2%"], ["AMD", "152.30", "-1.1%"]];
  const row = items.map(([s, p, c]) => `<span><b>${s}</b>${p} <span class="${c[0] === "+" ? "pos" : "neg"}">${c}</span></span>`).join("");
  document.getElementById("tickerTrack").innerHTML = row + row;
})();

/* ============ 2. Sticky nav: active section, mobile menu, progress bar ============ */
(function nav() {
  const links = [...document.querySelectorAll(".nav-links a")];
  const burger = document.getElementById("burger"), menu = document.getElementById("links");
  burger.addEventListener("click", () => {
    const open = !menu.classList.contains("open");
    menu.classList.toggle("open", open);
    burger.setAttribute("aria-expanded", String(open));
  });
  links.forEach(a => a.addEventListener("click", () => { menu.classList.remove("open"); burger.setAttribute("aria-expanded", "false"); }));
  const io = new IntersectionObserver(entries => entries.forEach(e => {
    if (e.isIntersecting) links.forEach(a => a.classList.toggle("active", a.getAttribute("href") === "#" + e.target.id));
  }), { rootMargin: "-45% 0px -50% 0px" });
  ["features", "analytics", "discipline", "mindset", "faq"].forEach(id => { const el = document.getElementById(id); if (el) io.observe(el); });
})();

/* ============ 3. Hero equity curve (sample series, drawn on canvas) ============ */
(function equity() {
  const cv = document.getElementById("equity"), ctx = cv.getContext("2d");
  let seed = 11; const rnd = () => (seed = (seed * 16807) % 2147483647) / 2147483647;
  const series = []; let v = 0;
  for (let i = 0; i < 1042; i++) { v += (rnd() - 0.42) * 60; series.push(v); }
  const net = series[series.length - 1];
  const wins = series.filter((x, i) => i > 0 && x > series[i - 1]).length;
  const mNet = document.getElementById("mNet");
  mNet.textContent = (net < 0 ? "-" : "+") + "$" + Math.abs(Math.round(net)).toLocaleString("en-US");
  mNet.className = net >= 0 ? "pos" : "neg";
  document.getElementById("mWin").textContent = Math.round(wins / (series.length - 1) * 100) + "%";
  document.getElementById("mPf").textContent = (1.4 + rnd() * 0.8).toFixed(2);
  const min = Math.min(0, ...series), max = Math.max(...series), W = cv.width, H = cv.height, pad = 18;
  const x = i => pad + (i / (series.length - 1)) * (W - pad * 2);
  const y = val => H - pad - ((val - min) / (max - min || 1)) * (H - pad * 2);
  let t = 0;
  const draw = () => {
    t = Math.min(1, t + 0.02);
    const n = Math.max(2, Math.floor(t * series.length));
    ctx.clearRect(0, 0, W, H);
    ctx.strokeStyle = "rgba(233,239,255,.12)"; ctx.setLineDash([4, 6]);
    ctx.beginPath(); ctx.moveTo(pad, y(0)); ctx.lineTo(W - pad, y(0)); ctx.stroke(); ctx.setLineDash([]);
    const g = ctx.createLinearGradient(0, 0, 0, H);
    g.addColorStop(0, "rgba(159,216,255,.28)"); g.addColorStop(1, "rgba(159,216,255,0)");
    ctx.beginPath(); ctx.moveTo(x(0), y(0));
    for (let i = 0; i < n; i++) ctx.lineTo(x(i), y(series[i]));
    ctx.lineTo(x(n - 1), H - pad); ctx.lineTo(x(0), H - pad); ctx.closePath();
    ctx.fillStyle = g; ctx.fill();
    ctx.beginPath();
    for (let i = 0; i < n; i++) i ? ctx.lineTo(x(i), y(series[i])) : ctx.moveTo(x(i), y(series[i]));
    ctx.strokeStyle = "#e9efff"; ctx.lineWidth = 2.2; ctx.lineJoin = "round"; ctx.stroke();
    ctx.beginPath(); ctx.arc(x(n - 1), y(series[n - 1]), 5, 0, Math.PI * 2); ctx.fillStyle = "#9fd8ff"; ctx.fill();
    if (t < 1) requestAnimationFrame(draw);
  };
  if (window.matchMedia("(prefers-reduced-motion: reduce)").matches) t = 1;
  const io = new IntersectionObserver(es => { if (es[0].isIntersecting) { draw(); io.disconnect(); } }, { threshold: 0.3 });
  io.observe(cv);
})();

/* ============ 4. Count-up numbers on scroll ============ */
(function countUp() {
  const io = new IntersectionObserver(entries => entries.forEach(e => {
    if (!e.isIntersecting) return;
    const el = e.target, target = +el.dataset.count, start = performance.now(), dur = 1200;
    const step = now => {
      const k = Math.min(1, (now - start) / dur), eased = 1 - Math.pow(1 - k, 3);
      el.textContent = Math.round(target * eased);
      if (k < 1) requestAnimationFrame(step);
    };
    requestAnimationFrame(step);
    io.unobserve(el);
  }), { threshold: 0.5 });
  document.querySelectorAll("[data-count]").forEach(n => io.observe(n));
})();

/* ============ 5. Feature tabs (keyboard friendly) ============ */
(function tabs() {
  const tabEls = [...document.querySelectorAll(".tab")];
  const select = tab => tabEls.forEach(t => {
    const on = t === tab;
    t.setAttribute("aria-selected", String(on));
    document.getElementById(t.getAttribute("aria-controls")).classList.toggle("on", on);
  });
  tabEls.forEach((t, i) => {
    t.addEventListener("click", () => select(t));
    t.addEventListener("keydown", e => {
      if (e.key === "ArrowRight" || e.key === "ArrowLeft") {
        const next = tabEls[(i + (e.key === "ArrowRight" ? 1 : -1) + tabEls.length) % tabEls.length];
        next.focus(); select(next);
      }
    });
  });
})();

/* ============ 6. Entry-model bars (sample) ============ */
(function modelBars() {
  const data = [["Breakout retest", 1240], ["Opening range", 820], ["Trend pullback", -210], ["Reversal fade", 460]];
  const max = Math.max(...data.map(d => Math.abs(d[1])));
  document.getElementById("modelBars").innerHTML = data.map(([name, v]) => `
    <div class="bar-row"><span>${name}</span>
      <div class="bar-track"><div class="bar-fill" data-w="${Math.abs(v) / max * 100}" style="background:${v >= 0 ? "linear-gradient(90deg,#9fd8ff,#ffffff)" : "linear-gradient(90deg,#f87171,#fca5a5)"}"></div></div>
      <span class="bar-val ${v >= 0 ? "pos" : "neg"}">${v >= 0 ? "+" : "-"}$${Math.abs(v).toLocaleString("en-US")}</span></div>`).join("");
})();

/* ============ 7. Session analytics: grow on scroll, hover to focus ============ */
(function sessionBars() {
  const data = [["New York", 18420], ["London", 9860], ["Asia", -3210], ["Sydney", 1540]];
  const max = Math.max(...data.map(d => Math.abs(d[1])));
  const box = document.getElementById("sessionBars");
  box.innerHTML = data.map(([name, v]) => `
    <div class="bar-row" tabindex="0"><span>${name}</span>
      <div class="bar-track"><div class="bar-fill" data-w="${Math.abs(v) / max * 100}" style="background:${v >= 0 ? "linear-gradient(90deg,#9fd8ff,#ffffff)" : "linear-gradient(90deg,#f87171,#fca5a5)"}"></div></div>
      <span class="bar-val ${v >= 0 ? "pos" : "neg"}">${v >= 0 ? "+" : "-"}$${Math.abs(v).toLocaleString("en-US")}</span></div>`).join("");
  box.querySelectorAll(".bar-row").forEach(row => {
    row.addEventListener("mouseenter", () => box.querySelectorAll(".bar-row").forEach(r => r.classList.toggle("dim", r !== row)));
    row.addEventListener("mouseleave", () => box.querySelectorAll(".bar-row").forEach(r => r.classList.remove("dim")));
  });
  const io = new IntersectionObserver(es => {
    if (!es[0].isIntersecting) return;
    box.querySelectorAll(".bar-fill").forEach(f => f.style.width = f.dataset.w + "%");
    io.disconnect();
  }, { threshold: 0.3 });
  io.observe(box);
})();

/* ============ 8. Discipline checklist: live score ============ */
(function checklist() {
  const rules = ["Session started on time", "Reviewed the higher timeframe", "Wrote the plan before the open", "Only traded my entry model", "Risk stayed within my limit", "Stopped at my maximum trade count", "Journaled the session"];
  const box = document.getElementById("checklist");
  const on = new Array(rules.length).fill(false);
  const render = () => {
    const pct = Math.round(on.filter(Boolean).length / rules.length * 100);
    document.getElementById("scoreVal").textContent = pct + "%";
    document.getElementById("scoreBar").style.width = pct + "%";
    box.innerHTML = rules.map((r, i) => `
      <div class="check ${on[i] ? "on" : ""}" role="checkbox" aria-checked="${on[i]}" tabindex="0" data-i="${i}">
        <span class="box">${on[i] ? "✓" : ""}</span><span>${r}</span></div>`).join("");
  };
  const toggle = i => { on[i] = !on[i]; render(); };
  box.addEventListener("click", e => { const row = e.target.closest(".check"); if (row) toggle(+row.dataset.i); });
  box.addEventListener("keydown", e => { if ((e.key === " " || e.key === "Enter") && e.target.classList.contains("check")) { e.preventDefault(); toggle(+e.target.dataset.i); } });
  render();
})();

/* ============ 9. Mindset selector: focus rating, emotion tags, reminders ============ */
(function mindset() {
  const moods = ["Rough", "Shaky", "Neutral", "Focused", "Sharp"];
  const tags = { "After a loss": "A loss is data, not a verdict. Step away for ten minutes, then come back with a clear head.", "Feeling FOMO": "If you are chasing, the move already happened without you. Wait for your entry model.", "Revenge urge": "Trading to win back money is how small losses grow. Stop for the session and return with a plan.", "Another": "Wanting another trade right now is a signal to pause. Check whether it is in your plan.", "Calm": "Calm is a skill. Keep the pace you planned.", "Patient": "Patience is a position too. Hold it.", "Confident": "Confidence is quiet. Trust the rules that got you here." };
  let mood = 3, active = null;
  const moodBox = document.getElementById("moods"), tagBox = document.getElementById("tags"), rem = document.getElementById("reminder");
  const renderReminder = () => {
    rem.style.opacity = 0;
    setTimeout(() => {
      rem.textContent = active ? tags[active] : `Right now you feel ${moods[mood - 1].toLowerCase()}. Pick an emotion above for a reminder.`;
      rem.style.opacity = 1;
    }, 180);
  };
  moodBox.innerHTML = moods.map((m, i) => `<button class="mood" aria-pressed="${i + 1 === mood}" data-v="${i + 1}">${m}</button>`).join("");
  tagBox.innerHTML = Object.keys(tags).map(t => `<button class="tagchip" aria-pressed="false" data-t="${t}">${t}</button>`).join("");
  moodBox.addEventListener("click", e => {
    const b = e.target.closest(".mood"); if (!b) return;
    mood = +b.dataset.v;
    moodBox.querySelectorAll(".mood").forEach(x => x.setAttribute("aria-pressed", String(x === b)));
    renderReminder();
  });
  tagBox.addEventListener("click", e => {
    const b = e.target.closest(".tagchip"); if (!b) return;
    active = active === b.dataset.t ? null : b.dataset.t;
    tagBox.querySelectorAll(".tagchip").forEach(x => x.setAttribute("aria-pressed", String(x.dataset.t === active)));
    renderReminder();
  });
  renderReminder();
})();

/* ============ 10. Scroll sequence: a trading story ============
   Phase 0 (0 to 0.2): counter runs from 3000 trades down to one plan, while trade tickets drift.
   Phase 1 (0.06 to 0.4): tickets tilt and fly into a row, becoming candles (green wins, red losses).
   Phase 2 (0.42 to 0.7): the candlestick chart turns in 3D.
   Phase 3 (0.74 to 1.0): the chart resolves into P&L pillars by trading session. */
(function sequence() {
  const sec = document.getElementById("seq"), cv = document.getElementById("seqFx"), ctx = cv.getContext("2d");
  const prog = document.getElementById("prog");
  const dpr = Math.min(window.devicePixelRatio || 1, 2);
  const clamp = (v, a = 0, b = 1) => Math.min(b, Math.max(a, v));
  const lerp = (a, b, t) => a + (b - a) * t;
  const ease = t => t < 0.5 ? 4 * t * t * t : 1 - Math.pow(-2 * t + 2, 3) / 2;
  const fade = (p, a, b, c, d) => clamp((p - a) / (b - a)) * (1 - clamp((p - c) / (d - c)));
  let W = 0, H = 0, spin = 0, lastP = -1, queued = false;

  // Sample trades, seeded so the story is the same on every visit
  let seed = 42; const rnd = () => (seed = (seed * 16807) % 2147483647) / 2147483647;
  const N = 40, trades = [];
  for (let i = 0; i < N; i++) {
    const pnl = Math.round((rnd() - 0.42) * 260 / 5) * 5;
    trades.push({ pnl, sym: ["NVDA", "MES", "TSLA", "AAPL", "NFLX", "MNQ", "AMD"][Math.floor(rnd() * 7)], start: { x: (rnd() - 0.5) * 1.6, y: (rnd() - 0.5) * 1.1 } });
  }
  // Running price path: each trade's body goes from the previous close to the new close
  const bodies = []; let level = 0;
  trades.forEach(t => { const o = level, c = level + t.pnl * 0.12; bodies.push({ o, c, up: t.pnl > 0 }); level = c; });
  const mean = (bodies.reduce((s, b) => s + b.c, 0) / N);
  const candleX = i => (i - (N - 1) / 2) * 15;
  const candleY = v => -(v - mean) * 1.4;

  // Session pillars for phase 3 (same sample values as the analytics bars)
  const sessions = [["New York", 18420], ["London", 9860], ["Asia", -3210], ["Sydney", 1540]];

  const project = (x, y, z, f, cx, cy) => { const s = f / (f + z); return [cx + x * s, cy + y * s, s]; };
  function box(cx, cy, x, y, z, w, h, d, ry, rgb, alpha) {
    if (alpha <= 0.01) return;
    const hw = w / 2, hh = h / 2, hd = d / 2, c = Math.cos(ry), s = Math.sin(ry);
    const pts = [[-hw,-hh,-hd],[hw,-hh,-hd],[hw,hh,-hd],[-hw,hh,-hd],[-hw,-hh,hd],[hw,-hh,hd],[hw,hh,hd],[-hw,hh,hd]].map(([px, py, pz]) => [px * c + pz * s + x, py + y, -px * s + pz * c + z]);
    const top = [3, 2, 6, 7].map(i => project(...pts[i], 900, cx, cy));
    ctx.fillStyle = `rgba(${rgb},${0.12 * alpha})`;
    ctx.beginPath(); top.forEach(([px, py], i) => i ? ctx.lineTo(px, py) : ctx.moveTo(px, py)); ctx.closePath(); ctx.fill();
    [[0,1],[1,2],[2,3],[3,0],[4,5],[5,6],[6,7],[7,4],[0,4],[1,5],[2,6],[3,7]].forEach(([a, b]) => {
      const [x1, y1, s1] = project(...pts[a], 900, cx, cy), [x2, y2, s2] = project(...pts[b], 900, cx, cy);
      ctx.strokeStyle = `rgba(${rgb},${alpha * (0.35 + 0.5 * Math.min(s1, s2))})`;
      ctx.beginPath(); ctx.moveTo(x1, y1); ctx.lineTo(x2, y2); ctx.stroke();
    });
  }
  const UP = "74,222,128", DOWN = "248,113,113", INK = "233,239,255";

  function render() {
    queued = false;
    const top = sec.getBoundingClientRect().top + window.scrollY;
    const span = Math.max(1, sec.offsetHeight - window.innerHeight);
    const p = clamp((window.scrollY - top) / span);
    const docMax = Math.max(1, document.documentElement.scrollHeight - window.innerHeight);
    prog.style.width = (clamp(window.scrollY / docMax) * 100).toFixed(2) + "%";
    if (Math.abs(p - lastP) < 0.0005) return;
    lastP = p;
    spin += 0.0015;
    ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
    ctx.clearRect(0, 0, W, H);

    const pTix = clamp((p - 0.06) / 0.34);
    const pChart = fade(p, 0.3, 0.42, 0.74, 0.8);
    const pTurn = clamp((p - 0.42) / 0.28);
    const pPillar = clamp((p - 0.74) / 0.12);
    const ry = ease(pTurn) * 0.7 + spin * 0.5;

    // Counter
    const val = Math.round(lerp(3000, 1, ease(clamp(p / 0.2))));
    const num = document.getElementById("seqNum");
    num.textContent = val;
    num.style.filter = `blur(${(Math.abs(ease(clamp(p / 0.2)) - ease(clamp((p - 0.01) / 0.2))) * 300).toFixed(1)}px)`;
    document.querySelector(".seq-counter").style.opacity = (1 - clamp((p - 0.3) / 0.1)).toFixed(3);

    const cx = W * 0.62, cy = H * 0.5;

    // Trade tickets: scattered, then flying into a row that becomes the candles
    trades.forEach((t, i) => {
      const e = ease(pTix);
      const sx = t.start.x * W * 0.5 + W / 2, sy = t.start.y * H * 0.4 + H / 2;
      const tx = cx + candleX(i), ty = cy + candleY(bodies[i].c);
      const px = lerp(sx, tx, e), py = lerp(sy, ty, e);
      const alpha = clamp(pTix * 2.5) * (1 - pChart);
      if (alpha < 0.02) return;
      const w = lerp(120, 9, e), h = lerp(26, 9, e);
      ctx.fillStyle = `rgba(${t.pnl >= 0 ? UP : DOWN},${0.22 * alpha})`;
      ctx.strokeStyle = `rgba(${t.pnl >= 0 ? UP : DOWN},${0.7 * alpha})`;
      ctx.beginPath(); ctx.roundRect ? ctx.roundRect(px - w / 2, py - h / 2, w, h, 5) : ctx.rect(px - w / 2, py - h / 2, w, h);
      ctx.fill(); ctx.stroke();
      if (e < 0.6) {
        ctx.fillStyle = `rgba(${INK},${alpha * (1 - e / 0.6)})`;
        ctx.font = "11px 'IBM Plex Mono', monospace";
        ctx.fillText(`${t.sym} ${t.pnl >= 0 ? "+" : "-"}$${Math.abs(t.pnl)}`, px - w / 2 + 8, py + 4);
      }
    });

    // Candlestick chart, turning into 3D (candles are real boxes: body from open to close)
    if (pChart > 0.01) {
      const keep = 1 - pPillar;
      bodies.forEach((b, i) => {
        const x = candleX(i), bodyTop = candleY(Math.max(b.o, b.c)), bodyBot = candleY(Math.min(b.o, b.c));
        const h = Math.max(3, bodyBot - bodyTop);
        const alpha = pChart * keep;
        box(cx, cy, x, (bodyTop + bodyBot) / 2, 0, 9, h, 9, ry, b.up ? UP : DOWN, alpha);
      });
    }

    // Session pillars: the chart resolves into P&L by session
    if (pPillar > 0.01) {
      sessions.forEach(([name, v], j) => {
        const x = (j - (sessions.length - 1) / 2) * 130, hgt = Math.abs(v) * 0.09;
        const yPos = v >= 0 ? -hgt / 2 : hgt / 2;
        box(cx, cy, x, yPos, 0, 70, hgt, 70, ry, v >= 0 ? UP : DOWN, pPillar);
        const [lx, ly] = project(x, 120 + (v >= 0 ? hgt / 2 : -hgt / 2) * 0.0, 0, 900, cx, cy);
        ctx.fillStyle = `rgba(${INK},${0.7 * pPillar})`;
        ctx.font = "12px 'Inter', sans-serif"; ctx.textAlign = "center";
        ctx.fillText(name, lx, cy + 150);
        ctx.fillStyle = `rgba(${v >= 0 ? UP : DOWN},${pPillar})`;
        ctx.font = "600 12px 'IBM Plex Mono', monospace";
        ctx.fillText((v >= 0 ? "+$" : "-$") + Math.abs(v).toLocaleString("en-US"), lx, cy + 168);
        ctx.textAlign = "start";
      });
    }

    // Text blocks
    const setBlock = (id, v, y) => {
      const el = document.getElementById(id);
      el.style.opacity = clamp(v).toFixed(3);
      el.style.setProperty("--y", y.toFixed(1) + "px");
    };
    setBlock("seqTagline", fade(p, 0.3, 0.44, 0.52, 0.6), (1 - clamp((p - 0.3) / 0.14)) * 24);
    setBlock("seqRisk", fade(p, 0.56, 0.66, 0.72, 0.76), (1 - clamp((p - 0.56) / 0.1)) * 30);
    setBlock("seqAnalytics", fade(p, 0.78, 0.86, 0.94, 0.98), (1 - clamp((p - 0.78) / 0.08)) * 30);
    document.getElementById("seqCue").style.opacity = clamp(1 - p * 8).toFixed(3);
  }

  function schedule() { if (!queued) { queued = true; requestAnimationFrame(render); } }
  function resize() {
    W = cv.clientWidth; H = cv.clientHeight;
    cv.width = W * dpr; cv.height = H * dpr;
    lastP = -1; schedule();
  }
  window.addEventListener("scroll", schedule, { passive: true });
  window.addEventListener("resize", resize);
  resize();
})();

/* ============ 11. Reveal on scroll ============ */
(function reveal() {
  const els = document.querySelectorAll(".rv");
  if (!("IntersectionObserver" in window)) { els.forEach(e => e.classList.add("in")); return; }
  const io = new IntersectionObserver(es => es.forEach(e => { if (e.isIntersecting) { e.target.classList.add("in"); io.unobserve(e.target); } }), { threshold: 0.14 });
  els.forEach(e => io.observe(e));
})();

/* ============ 12. Footer year ============ */
document.getElementById("yr").textContent = new Date().getFullYear();
</script>
</body>
</html>
