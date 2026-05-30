<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI 智慧衣櫃與穿搭數據分析系統</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Anton&family=Noto+Sans+TC:wght@400;500;700;900&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#0a0a0b;
  --surface:#141416;
  --surface-2:#1b1b1e;
  --line:rgba(255,255,255,.12);
  --line-soft:rgba(255,255,255,.06);
  --text:#f5f5f6;
  --muted:#8c8c93;
  --dim:#5a5a60;
  --white:#ffffff;
  --black:#0a0a0b;
  --accent:#e9ff3f;
  --radius:0px;
  --maxw:1320px;
}
*{margin:0;padding:0;box-sizing:border-box}
html{scroll-behavior:smooth}
body{
  background:var(--bg);
  color:var(--text);
  font-family:"Noto Sans TC",sans-serif;
  font-weight:400;
  line-height:1.6;
  -webkit-font-smoothing:antialiased;
  overflow-x:hidden;
}
/* grain + grid backdrop */
body::before{
  content:"";position:fixed;inset:0;z-index:0;pointer-events:none;
  background-image:
    linear-gradient(var(--line-soft) 1px,transparent 1px),
    linear-gradient(90deg,var(--line-soft) 1px,transparent 1px);
  background-size:64px 64px;
  mask-image:radial-gradient(ellipse 90% 70% at 50% 0%,#000 30%,transparent 80%);
}
body::after{
  content:"";position:fixed;inset:0;z-index:0;pointer-events:none;opacity:.035;
  background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='120' height='120'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.9' numOctaves='3'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
}
::selection{background:var(--accent);color:var(--black)}
.mono{font-family:"Space Mono",monospace}

/* ---------- HEADER ---------- */
header{
  position:sticky;top:0;z-index:100;
  background:rgba(10,10,11,.72);
  backdrop-filter:blur(18px);
  border-bottom:1px solid var(--line);
}
.topbar{
  max-width:var(--maxw);margin:0 auto;
  display:flex;align-items:center;justify-content:space-between;
  padding:14px 28px;gap:20px;
}
.brand{display:flex;align-items:center;gap:14px;cursor:pointer;user-select:none}
.brand .logo{
  width:38px;height:38px;border:1.5px solid var(--text);
  display:grid;place-items:center;font-family:"Anton";font-size:20px;
  transition:.35s;
}
.brand:hover .logo{background:var(--accent);color:var(--black);border-color:var(--accent);transform:rotate(-8deg)}
.brand .bt{display:flex;flex-direction:column;line-height:1.1}
.brand .bt b{font-family:"Anton";letter-spacing:.5px;font-size:15px;font-weight:400}
.brand .bt span{font-size:9px;letter-spacing:3px;color:var(--muted)}
nav{display:flex;align-items:center;gap:2px;flex-wrap:wrap}
.navlink{
  background:none;border:none;color:var(--muted);cursor:pointer;
  font-family:"Noto Sans TC";font-size:13px;font-weight:500;
  padding:9px 13px;letter-spacing:.5px;position:relative;transition:.25s;white-space:nowrap;
}
.navlink:hover{color:var(--text)}
.navlink.active{color:var(--text)}
.navlink.active::after{
  content:"";position:absolute;left:13px;right:13px;bottom:2px;height:2px;background:var(--accent);
}
.navlink .en{font-family:"Space Mono";font-size:9px;display:block;color:var(--dim);letter-spacing:1px}
.menu-toggle{display:none;background:none;border:1px solid var(--line);color:var(--text);width:42px;height:42px;cursor:pointer;font-size:18px}

/* ---------- LAYOUT ---------- */
main{position:relative;z-index:1;max-width:var(--maxw);margin:0 auto;padding:0 28px 120px}
.view{display:none;animation:fade .5s ease both}
.view.active{display:block}
@keyframes fade{from{opacity:0;transform:translateY(14px)}to{opacity:1;transform:none}}
.eyebrow{font-family:"Space Mono";font-size:11px;letter-spacing:3px;color:var(--accent);text-transform:uppercase}
.section-head{display:flex;align-items:flex-end;justify-content:space-between;gap:24px;flex-wrap:wrap;margin:64px 0 32px;border-bottom:1px solid var(--line);padding-bottom:22px}
.section-head h2{font-family:"Anton";font-weight:400;font-size:clamp(34px,5vw,58px);line-height:.95;letter-spacing:.5px}
.section-head .zh{font-size:14px;color:var(--muted);margin-top:8px}

/* ---------- HERO / HOME ---------- */
.hero{position:relative;padding:70px 0 40px}
.hero .kicker{display:flex;gap:14px;align-items:center;font-family:"Space Mono";font-size:11px;letter-spacing:3px;color:var(--muted);text-transform:uppercase;margin-bottom:26px}
.hero .kicker .dot{width:8px;height:8px;background:var(--accent);border-radius:50%;animation:pulse 2s infinite}
@keyframes pulse{0%,100%{opacity:1;transform:scale(1)}50%{opacity:.3;transform:scale(.7)}}
.hero h1{
  font-family:"Anton";font-weight:400;
  font-size:clamp(48px,11vw,150px);line-height:.86;letter-spacing:-1px;
  text-transform:uppercase;
}
.hero h1 .stroke{-webkit-text-stroke:1.5px var(--text);color:transparent}
.hero h1 .hl{color:var(--accent)}
.hero .zh-title{font-size:clamp(20px,3vw,32px);font-weight:900;margin-top:18px;letter-spacing:2px}
.hero p.lede{max-width:560px;color:var(--muted);margin-top:22px;font-size:16px}
.hero-cta{display:flex;gap:14px;margin-top:36px;flex-wrap:wrap}
.btn{
  font-family:"Noto Sans TC";font-weight:700;font-size:14px;letter-spacing:1px;
  padding:15px 30px;border:1.5px solid var(--text);background:none;color:var(--text);
  cursor:pointer;transition:.3s;display:inline-flex;align-items:center;gap:10px;
}
.btn:hover{background:var(--text);color:var(--black)}
.btn.fill{background:var(--accent);border-color:var(--accent);color:var(--black)}
.btn.fill:hover{background:var(--text);border-color:var(--text);color:var(--black)}
.btn .ar{font-family:"Space Mono"}

.stat-row{display:grid;grid-template-columns:repeat(4,1fr);gap:1px;background:var(--line);border:1px solid var(--line);margin-top:70px}
.stat{background:var(--bg);padding:30px 26px}
.stat .num{font-family:"Anton";font-size:clamp(34px,5vw,56px);font-weight:400;line-height:1}
.stat .lbl{font-family:"Space Mono";font-size:11px;color:var(--muted);letter-spacing:1px;margin-top:8px}
.stat .lbl .en{display:block;color:var(--dim);font-size:9px;letter-spacing:1px}

.feature-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:1px;background:var(--line);border:1px solid var(--line);margin-top:1px}
.fcard{background:var(--bg);padding:34px 28px;transition:.35s;cursor:pointer;position:relative;overflow:hidden}
.fcard:hover{background:var(--surface)}
.fcard .ic{font-size:26px;margin-bottom:18px}
.fcard h3{font-size:18px;font-weight:700;margin-bottom:10px}
.fcard p{font-size:13.5px;color:var(--muted)}
.fcard .idx{position:absolute;top:20px;right:24px;font-family:"Space Mono";font-size:11px;color:var(--dim)}
.fcard .go{font-family:"Space Mono";font-size:11px;color:var(--accent);margin-top:18px;display:block;opacity:0;transform:translateX(-6px);transition:.3s}
.fcard:hover .go{opacity:1;transform:none}

.marquee{margin-top:70px;border-top:1px solid var(--line);border-bottom:1px solid var(--line);overflow:hidden;padding:18px 0}
.marquee .track{display:flex;gap:46px;white-space:nowrap;animation:scroll 26s linear infinite;font-family:"Anton";font-size:26px;color:var(--dim);text-transform:uppercase;letter-spacing:1px}
.marquee .track span{display:flex;align-items:center;gap:46px}
.marquee .track .s{color:var(--accent)}
@keyframes scroll{to{transform:translateX(-50%)}}

/* ---------- FORMS ---------- */
.form-wrap{display:grid;grid-template-columns:1.1fr .9fr;gap:1px;background:var(--line);border:1px solid var(--line)}
.form-pane{background:var(--bg);padding:38px}
.field{margin-bottom:22px}
.field label{display:block;font-family:"Space Mono";font-size:11px;letter-spacing:1px;color:var(--muted);margin-bottom:9px;text-transform:uppercase}
.field input,.field select,.field textarea{
  width:100%;background:var(--surface);border:1px solid var(--line);color:var(--text);
  padding:13px 15px;font-family:"Noto Sans TC";font-size:14px;transition:.25s;
}
.field input:focus,.field select:focus,.field textarea:focus{outline:none;border-color:var(--accent)}
.field textarea{resize:vertical;min-height:80px}
.field .row{display:grid;grid-template-columns:1fr 1fr;gap:14px}
.swatch-row{display:flex;gap:10px;flex-wrap:wrap}
.swatch{width:34px;height:34px;border:2px solid var(--line);cursor:pointer;transition:.2s;position:relative}
.swatch.sel{border-color:var(--accent);transform:scale(1.12)}
.swatch.sel::after{content:"✓";position:absolute;inset:0;display:grid;place-items:center;font-size:13px;color:#fff;mix-blend-mode:difference}
.preview-pane{background:var(--surface);padding:38px;display:flex;flex-direction:column}
.preview-pane .ttl{font-family:"Space Mono";font-size:11px;letter-spacing:2px;color:var(--muted);margin-bottom:20px}
.preview-card{flex:1;border:1px solid var(--line);background:var(--bg);display:flex;flex-direction:column;min-height:340px}
.preview-card .img{flex:1;display:grid;place-items:center;font-size:74px;position:relative;transition:.4s}
.preview-card .meta{padding:20px;border-top:1px solid var(--line)}
.preview-card .meta .pn{font-size:17px;font-weight:700}
.preview-card .meta .pc{font-family:"Space Mono";font-size:11px;color:var(--muted);margin-top:6px;letter-spacing:1px}
.preview-card .tags{display:flex;gap:6px;margin-top:12px;flex-wrap:wrap}
.tag{font-family:"Space Mono";font-size:10px;padding:4px 9px;border:1px solid var(--line);color:var(--muted);letter-spacing:.5px}

/* ---------- WARDROBE GRID ---------- */
.filters{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:30px}
.chip{font-family:"Space Mono";font-size:12px;padding:9px 16px;border:1px solid var(--line);background:none;color:var(--muted);cursor:pointer;transition:.25s;letter-spacing:.5px}
.chip:hover{color:var(--text);border-color:var(--text)}
.chip.active{background:var(--text);color:var(--black);border-color:var(--text)}
.grid{display:grid;grid-template-columns:repeat(4,1fr);gap:1px;background:var(--line);border:1px solid var(--line)}
.product{background:var(--bg);cursor:pointer;transition:.35s;position:relative;overflow:hidden}
.product:hover{background:var(--surface)}
.product .ph{aspect-ratio:3/4;display:grid;place-items:center;font-size:64px;position:relative;transition:.45s}
.product:hover .ph{transform:scale(1.06)}
.product .ph .num{position:absolute;top:14px;left:14px;font-family:"Space Mono";font-size:10px;color:var(--dim)}
.product .ph .wear{position:absolute;top:14px;right:14px;font-family:"Space Mono";font-size:10px;color:var(--muted);border:1px solid var(--line);padding:2px 7px}
.product .info{padding:16px 16px 20px;border-top:1px solid var(--line)}
.product .info .nm{font-size:14.5px;font-weight:700;display:flex;justify-content:space-between;gap:8px}
.product .info .cat{font-family:"Space Mono";font-size:10.5px;color:var(--muted);margin-top:6px;letter-spacing:.5px}
.product .quick{position:absolute;left:0;right:0;bottom:0;background:var(--accent);color:var(--black);text-align:center;font-family:"Space Mono";font-size:11px;font-weight:700;padding:11px;letter-spacing:1px;transform:translateY(100%);transition:.3s}
.product:hover .quick{transform:none}
.empty{grid-column:1/-1;background:var(--bg);padding:80px 20px;text-align:center;color:var(--muted)}
.empty .big{font-size:40px;margin-bottom:14px}

/* ---------- MODAL ---------- */
.modal-bg{position:fixed;inset:0;z-index:300;background:rgba(0,0,0,.8);backdrop-filter:blur(6px);display:none;align-items:center;justify-content:center;padding:24px}
.modal-bg.show{display:flex;animation:fade .3s both}
.modal{background:var(--bg);border:1px solid var(--line);max-width:860px;width:100%;max-height:88vh;overflow:auto;display:grid;grid-template-columns:1fr 1fr}
.modal .mimg{aspect-ratio:1;display:grid;place-items:center;font-size:120px;border-right:1px solid var(--line);position:relative}
.modal .mbody{padding:38px;position:relative}
.modal .close{position:absolute;top:20px;right:20px;width:36px;height:36px;border:1px solid var(--line);background:none;color:var(--text);cursor:pointer;font-size:16px;transition:.25s}
.modal .close:hover{background:var(--text);color:var(--black)}
.modal .mcat{font-family:"Space Mono";font-size:11px;letter-spacing:2px;color:var(--accent);text-transform:uppercase}
.modal h3{font-family:"Anton";font-weight:400;font-size:34px;margin:10px 0 20px;line-height:1}
.dl-list{border-top:1px solid var(--line)}
.dl-list .r{display:flex;justify-content:space-between;padding:13px 0;border-bottom:1px solid var(--line-soft);font-size:14px}
.dl-list .r .k{color:var(--muted);font-family:"Space Mono";font-size:11px;letter-spacing:1px}
.modal .mnote{color:var(--muted);font-size:13.5px;margin-top:18px}
.modal-actions{display:flex;gap:10px;margin-top:26px;flex-wrap:wrap}

/* ---------- RECOMMEND ---------- */
.rec-controls{display:flex;gap:24px;flex-wrap:wrap;align-items:flex-end;margin-bottom:34px}
.rec-controls .field{margin:0;min-width:170px}
.outfit{display:grid;grid-template-columns:repeat(4,1fr);gap:1px;background:var(--line);border:1px solid var(--line)}
.slot{background:var(--bg);min-height:300px;display:flex;flex-direction:column;position:relative}
.slot .slot-lbl{position:absolute;top:14px;left:14px;font-family:"Space Mono";font-size:10px;color:var(--accent);letter-spacing:1px;z-index:2}
.slot .si{flex:1;display:grid;place-items:center;font-size:62px}
.slot .sm{padding:16px;border-top:1px solid var(--line)}
.slot .sm .n{font-size:14px;font-weight:700}
.slot .sm .c{font-family:"Space Mono";font-size:10px;color:var(--muted);margin-top:5px}
.slot.miss{display:grid;place-items:center;color:var(--dim);font-size:13px;text-align:center;padding:20px}
.rec-meta{display:flex;gap:30px;flex-wrap:wrap;margin-top:28px;padding:24px;border:1px solid var(--line);background:var(--surface)}
.rec-meta .m{flex:1;min-width:150px}
.rec-meta .m .k{font-family:"Space Mono";font-size:10px;color:var(--muted);letter-spacing:1px}
.rec-meta .m .v{font-size:16px;font-weight:700;margin-top:6px}
.score-bar{height:6px;background:var(--surface-2);margin-top:10px;overflow:hidden}
.score-bar i{display:block;height:100%;background:var(--accent);width:0;transition:1s}

/* ---------- RECORDS / HISTORY ---------- */
.timeline{border-left:1px solid var(--line);margin-left:8px;padding-left:30px}
.tl-item{position:relative;padding-bottom:30px}
.tl-item::before{content:"";position:absolute;left:-37px;top:4px;width:13px;height:13px;background:var(--bg);border:2px solid var(--accent)}
.tl-item .t{font-family:"Space Mono";font-size:11px;color:var(--muted);letter-spacing:1px}
.tl-item .h{font-size:16px;font-weight:700;margin:6px 0}
.tl-item .d{font-size:13.5px;color:var(--muted)}
.tl-item .pills{display:flex;gap:6px;margin-top:10px;flex-wrap:wrap}
.record-card{border:1px solid var(--line);background:var(--bg);padding:24px;margin-bottom:1px;display:flex;gap:24px;flex-wrap:wrap;align-items:center;justify-content:space-between}
.record-card .rl .occ{font-size:18px;font-weight:700}
.record-card .rl .dt{font-family:"Space Mono";font-size:11px;color:var(--muted);margin-top:5px;letter-spacing:1px}
.record-card .items{display:flex;gap:8px;flex-wrap:wrap}
.mini{display:flex;align-items:center;gap:8px;border:1px solid var(--line);padding:7px 12px;font-size:12px}
.mini .e{font-size:18px}

/* ---------- ANALYTICS ---------- */
.an-grid{display:grid;grid-template-columns:1fr 1fr;gap:1px;background:var(--line);border:1px solid var(--line)}
.panel{background:var(--bg);padding:30px}
.panel.full{grid-column:1/-1}
.panel h3{font-size:15px;font-weight:700;display:flex;align-items:center;gap:10px;margin-bottom:6px}
.panel h3 .en{font-family:"Space Mono";font-size:10px;color:var(--dim);font-weight:400;letter-spacing:1px}
.panel .sub{font-size:12px;color:var(--muted);margin-bottom:24px}
.bar-row{display:flex;align-items:center;gap:14px;margin-bottom:14px}
.bar-row .bl{width:90px;font-size:13px;flex-shrink:0}
.bar-row .bt{flex:1;height:26px;background:var(--surface-2);position:relative;overflow:hidden}
.bar-row .bt i{position:absolute;left:0;top:0;bottom:0;background:var(--text);width:0;transition:1.1s cubic-bezier(.2,.8,.2,1)}
.bar-row .bt i.acc{background:var(--accent)}
.bar-row .bv{font-family:"Space Mono";font-size:12px;color:var(--muted);width:42px;text-align:right;flex-shrink:0}
.donut-wrap{display:flex;gap:30px;align-items:center;flex-wrap:wrap}
.legend{flex:1;min-width:160px}
.legend .lg{display:flex;align-items:center;gap:10px;padding:7px 0;font-size:13px;border-bottom:1px solid var(--line-soft)}
.legend .lg .sw{width:12px;height:12px;flex-shrink:0}
.legend .lg .pc{margin-left:auto;font-family:"Space Mono";font-size:12px;color:var(--muted)}
.color-bars{display:flex;flex-direction:column;gap:12px}
.cbar{display:flex;align-items:center;gap:12px}
.cbar .dot{width:20px;height:20px;border:1px solid var(--line);flex-shrink:0}
.cbar .nm{width:54px;font-size:13px}
.cbar .track{flex:1;height:20px;background:var(--surface-2);position:relative;overflow:hidden}
.cbar .track i{position:absolute;inset:0 auto 0 0;width:0;transition:1.1s}
.cbar .vv{font-family:"Space Mono";font-size:12px;color:var(--muted);width:30px;text-align:right}
.kpis{display:grid;grid-template-columns:repeat(4,1fr);gap:1px;background:var(--line);border:1px solid var(--line);margin-bottom:1px}
.kpi{background:var(--bg);padding:26px}
.kpi .v{font-family:"Anton";font-size:42px;line-height:1}
.kpi .k{font-family:"Space Mono";font-size:10.5px;color:var(--muted);letter-spacing:1px;margin-top:8px}
.kpi .k .en{display:block;color:var(--dim);font-size:9px}

/* ---------- ABOUT ---------- */
.about-grid{display:grid;grid-template-columns:1fr 1fr;gap:1px;background:var(--line);border:1px solid var(--line)}
.about-pane{background:var(--bg);padding:38px}
.about-pane h3{font-family:"Anton";font-weight:400;font-size:26px;margin-bottom:16px}
.about-pane p{color:var(--muted);font-size:14.5px;margin-bottom:14px}
.tech-list{display:flex;flex-wrap:wrap;gap:8px;margin-top:8px}
.flow{display:flex;flex-direction:column;gap:0}
.flow .step{display:flex;gap:18px;padding:18px 0;border-bottom:1px solid var(--line-soft)}
.flow .step:last-child{border:none}
.flow .step .no{font-family:"Anton";font-size:22px;color:var(--accent);width:40px;flex-shrink:0}
.flow .step .sb h4{font-size:15px;font-weight:700}
.flow .step .sb p{font-size:13px;color:var(--muted);margin:4px 0 0}

/* ---------- TOAST ---------- */
.toast{position:fixed;bottom:30px;left:50%;transform:translateX(-50%) translateY(120px);z-index:400;background:var(--accent);color:var(--black);padding:14px 26px;font-weight:700;font-size:14px;display:flex;gap:10px;align-items:center;transition:.4s cubic-bezier(.2,.9,.3,1);box-shadow:0 20px 50px rgba(0,0,0,.5)}
.toast.show{transform:translateX(-50%) translateY(0)}

footer{position:relative;z-index:1;border-top:1px solid var(--line);max-width:var(--maxw);margin:0 auto;padding:34px 28px;display:flex;justify-content:space-between;flex-wrap:wrap;gap:16px;color:var(--muted);font-family:"Space Mono";font-size:11px;letter-spacing:1px}

/* ---------- RESPONSIVE ---------- */
@media(max-width:1080px){
  .grid,.outfit,.feature-grid{grid-template-columns:repeat(2,1fr)}
  .stat-row,.kpis{grid-template-columns:repeat(2,1fr)}
  .form-wrap,.an-grid,.about-grid{grid-template-columns:1fr}
  .modal{grid-template-columns:1fr}
  .modal .mimg{border-right:none;border-bottom:1px solid var(--line);aspect-ratio:16/9}
}
@media(max-width:680px){
  main,.topbar,footer{padding-left:18px;padding-right:18px}
  nav{position:fixed;top:67px;left:0;right:0;background:rgba(10,10,11,.97);backdrop-filter:blur(20px);flex-direction:column;align-items:stretch;border-bottom:1px solid var(--line);padding:10px;transform:translateY(-130%);transition:.35s;z-index:99}
  nav.open{transform:none}
  .navlink{padding:12px 14px;border-bottom:1px solid var(--line-soft)}
  .navlink.active::after{display:none}
  .menu-toggle{display:grid;place-items:center}
  .grid,.outfit{grid-template-columns:1fr 1fr}
  .stat-row{grid-template-columns:1fr 1fr}
  .kpis{grid-template-columns:1fr 1fr}
}
</style>
</head>
<body>

<header>
  <div class="topbar">
    <div class="brand" onclick="go('home')">
      <div class="logo">W</div>
      <div class="bt"><b>WARDROBE.AI</b><span>SMART STYLING SYSTEM</span></div>
    </div>
    <button class="menu-toggle" onclick="document.getElementById('nav').classList.toggle('open')">≡</button>
    <nav id="nav">
      <button class="navlink active" data-v="home" onclick="go('home')">首頁<span class="en">HOME</span></button>
      <button class="navlink" data-v="add" onclick="go('add')">新增衣物<span class="en">ADD</span></button>
      <button class="navlink" data-v="wardrobe" onclick="go('wardrobe')">我的衣櫃<span class="en">WARDROBE</span></button>
      <button class="navlink" data-v="recommend" onclick="go('recommend')">AI穿搭推薦<span class="en">AI MATCH</span></button>
      <button class="navlink" data-v="records" onclick="go('records')">穿搭紀錄<span class="en">LOOKS</span></button>
      <button class="navlink" data-v="history" onclick="go('history')">歷史紀錄<span class="en">HISTORY</span></button>
      <button class="navlink" data-v="analytics" onclick="go('analytics')">數據分析<span class="en">ANALYTICS</span></button>
      <button class="navlink" data-v="about" onclick="go('about')">系統介紹<span class="en">ABOUT</span></button>
    </nav>
  </div>
</header>

<main>

  <!-- ============ HOME ============ -->
  <section class="view active" id="view-home">
    <div class="hero">
      <div class="kicker"><span class="dot"></span> AI-POWERED FASHION ENGINE · 2026</div>
      <h1>SMART<br><span class="stroke">WARD</span><span class="hl">ROBE</span></h1>
      <div class="zh-title">AI 智慧衣櫃與穿搭數據分析系統</div>
      <p class="lede">把你的整個衣櫃數位化。記錄每一件衣物、讓 AI 演算法為你搭配每日穿搭,並用數據看清你的穿衣習慣與風格輪廓。</p>
      <div class="hero-cta">
        <button class="btn fill" onclick="go('add')">新增第一件衣物 <span class="ar">→</span></button>
        <button class="btn" onclick="go('recommend')">AI 幫我穿 <span class="ar">↗</span></button>
      </div>
    </div>

    <div class="stat-row">
      <div class="stat"><div class="num" data-count="0" id="h-total">0</div><div class="lbl">衣物總數<span class="en">TOTAL ITEMS</span></div></div>
      <div class="stat"><div class="num" data-count="0" id="h-looks">0</div><div class="lbl">穿搭紀錄<span class="en">LOGGED LOOKS</span></div></div>
      <div class="stat"><div class="num" data-count="0" id="h-cats">0</div><div class="lbl">衣物分類<span class="en">CATEGORIES</span></div></div>
      <div class="stat"><div class="num" data-count="0" id="h-wears">0</div><div class="lbl">總穿著次數<span class="en">TOTAL WEARS</span></div></div>
    </div>

    <div class="feature-grid">
      <div class="fcard" onclick="go('add')"><span class="idx">01</span><div class="ic">＋</div><h3>新增衣物</h3><p>輸入名稱、分類、顏色與季節,建立你的數位衣物資料庫。</p><span class="go">前往 ADD CLOTHES →</span></div>
      <div class="fcard" onclick="go('wardrobe')"><span class="idx">02</span><div class="ic">▦</div><h3>我的衣櫃</h3><p>以電商商品牆方式瀏覽全部衣物,點擊查看詳細資訊。</p><span class="go">前往 WARDROBE →</span></div>
      <div class="fcard" onclick="go('recommend')"><span class="idx">03</span><div class="ic">✦</div><h3>AI 穿搭推薦</h3><p>依場合與季節,演算法自動組合一套協調的 Look。</p><span class="go">前往 AI MATCH →</span></div>
      <div class="fcard" onclick="go('records')"><span class="idx">04</span><div class="ic">◷</div><h3>穿搭紀錄</h3><p>記錄每天穿了什麼,累積屬於你的穿搭日誌。</p><span class="go">前往 LOOKS →</span></div>
      <div class="fcard" onclick="go('analytics')"><span class="idx">05</span><div class="ic">◔</div><h3>數據分析</h3><p>分類比例、色彩分布、穿著頻率,一目了然。</p><span class="go">前往 ANALYTICS →</span></div>
      <div class="fcard" onclick="go('about')"><span class="idx">06</span><div class="ic">ⓘ</div><h3>系統介紹</h3><p>了解專題架構、使用技術與系統運作流程。</p><span class="go">前往 ABOUT →</span></div>
    </div>

    <div class="marquee"><div class="track">
      <span>WARDROBE.AI <span class="s">✦</span> DIGITAL CLOSET <span class="s">✦</span> OUTFIT MATCHING <span class="s">✦</span> STYLE ANALYTICS <span class="s">✦</span> SMART FASHION <span class="s">✦</span></span>
      <span>WARDROBE.AI <span class="s">✦</span> DIGITAL CLOSET <span class="s">✦</span> OUTFIT MATCHING <span class="s">✦</span> STYLE ANALYTICS <span class="s">✦</span> SMART FASHION <span class="s">✦</span></span>
    </div></div>
  </section>

  <!-- ============ ADD ============ -->
  <section class="view" id="view-add">
    <div class="section-head"><div><div class="eyebrow">STEP 01 / INPUT</div><h2>ADD CLOTHES</h2><div class="zh">新增衣物 — 建立你的數位衣物資料</div></div></div>
    <div class="form-wrap">
      <div class="form-pane">
        <div class="field"><label>衣物名稱 · ITEM NAME</label><input id="f-name" placeholder="例：黑色寬版西裝外套"></div>
        <div class="field"><div class="row">
          <div><label>分類 · CATEGORY</label><select id="f-cat"></select></div>
          <div><label>季節 · SEASON</label><select id="f-season"></select></div>
        </div></div>
        <div class="field"><label>品牌 · BRAND</label><input id="f-brand" placeholder="例：UNIQLO / 無 / 自有"></div>
        <div class="field"><label>主要顏色 · COLOR</label>
          <div class="swatch-row" id="f-swatches"></div>
        </div>
        <div class="field"><label>備註 · NOTES</label><textarea id="f-note" placeholder="材質、購入時間、搭配建議…"></textarea></div>
        <button class="btn fill" onclick="addClothes()">加入衣櫃 <span class="ar">＋</span></button>
        <button class="btn" style="margin-left:8px" onclick="resetForm()">清除</button>
      </div>
      <div class="preview-pane">
        <div class="ttl">// LIVE PREVIEW</div>
        <div class="preview-card">
          <div class="img" id="p-img" style="background:#1b1b1e">👕</div>
          <div class="meta">
            <div class="pn" id="p-name">衣物名稱</div>
            <div class="pc" id="p-cat">CATEGORY · SEASON</div>
            <div class="tags" id="p-tags"></div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- ============ WARDROBE ============ -->
  <section class="view" id="view-wardrobe">
    <div class="section-head"><div><div class="eyebrow">YOUR DIGITAL CLOSET</div><h2>WARDROBE</h2><div class="zh">我的衣櫃 — 點擊任一件查看詳細資訊</div></div>
      <button class="btn" onclick="go('add')">＋ 新增</button></div>
    <div class="filters" id="filters"></div>
    <div class="grid" id="wardrobe-grid"></div>
  </section>

  <!-- ============ RECOMMEND ============ -->
  <section class="view" id="view-recommend">
    <div class="section-head"><div><div class="eyebrow">AI STYLING ENGINE</div><h2>AI MATCH</h2><div class="zh">AI 穿搭推薦 — 依場合與季節自動組合</div></div></div>
    <div class="rec-controls">
      <div class="field"><label>場合 · OCCASION</label><select id="r-occ"><option>日常休閒</option><option>上班通勤</option><option>正式場合</option><option>運動</option><option>約會</option></select></div>
      <div class="field"><label>季節 · SEASON</label><select id="r-season"><option>全季</option><option>春</option><option>夏</option><option>秋</option><option>冬</option></select></div>
      <button class="btn fill" onclick="recommend()">生成穿搭 <span class="ar">✦</span></button>
      <button class="btn" onclick="recommend()">換一套 ↻</button>
    </div>
    <div class="outfit" id="outfit"></div>
    <div class="rec-meta" id="rec-meta" style="display:none">
      <div class="m"><div class="k">// 整體協調度 HARMONY</div><div class="v" id="rec-score">—</div><div class="score-bar"><i id="rec-scorebar"></i></div></div>
      <div class="m"><div class="k">// 推薦場合 OCCASION</div><div class="v" id="rec-occ">—</div></div>
      <div class="m"><div class="k">// 主色調 PALETTE</div><div class="v" id="rec-palette">—</div></div>
      <div class="m" style="display:flex;align-items:flex-end"><button class="btn fill" onclick="wearThis()">就穿這套 · 記錄 ✓</button></div>
    </div>
  </section>

  <!-- ============ RECORDS ============ -->
  <section class="view" id="view-records">
    <div class="section-head"><div><div class="eyebrow">OUTFIT JOURNAL</div><h2>LOOKS</h2><div class="zh">穿搭紀錄 — 你穿過的每一套</div></div></div>
    <div id="records-list"></div>
  </section>

  <!-- ============ HISTORY ============ -->
  <section class="view" id="view-history">
    <div class="section-head"><div><div class="eyebrow">ACTIVITY LOG</div><h2>HISTORY</h2><div class="zh">歷史紀錄 — 系統操作時間軸</div></div>
      <button class="btn" onclick="clearHistory()">清空紀錄</button></div>
    <div class="timeline" id="history-list"></div>
  </section>

  <!-- ============ ANALYTICS ============ -->
  <section class="view" id="view-analytics">
    <div class="section-head"><div><div class="eyebrow">DATA INSIGHTS</div><h2>ANALYTICS</h2><div class="zh">數據分析 — 看清你的穿衣習慣</div></div></div>
    <div class="kpis" id="kpis"></div>
    <div class="an-grid">
      <div class="panel"><h3>衣物分類分布 <span class="en">BY CATEGORY</span></h3><div class="sub">各類別衣物的數量占比</div><div class="donut-wrap"><div id="donut"></div><div class="legend" id="donut-legend"></div></div></div>
      <div class="panel"><h3>季節分布 <span class="en">BY SEASON</span></h3><div class="sub">四季衣物配置狀況</div><div id="season-bars"></div></div>
      <div class="panel"><h3>色彩分布 <span class="en">COLOR PALETTE</span></h3><div class="sub">衣櫃整體色彩傾向</div><div class="color-bars" id="color-bars"></div></div>
      <div class="panel"><h3>最常穿著 TOP 5 <span class="en">MOST WORN</span></h3><div class="sub">穿著次數最高的單品</div><div id="worn-bars"></div></div>
    </div>
  </section>

  <!-- ============ ABOUT ============ -->
  <section class="view" id="view-about">
    <div class="section-head"><div><div class="eyebrow">PROJECT OVERVIEW</div><h2>ABOUT</h2><div class="zh">系統介紹 — 資訊科學系專題</div></div></div>
    <div class="about-grid">
      <div class="about-pane">
        <h3>專題簡介</h3>
        <p>「AI 智慧衣櫃與穿搭數據分析系統」是一套將個人衣櫃完整數位化的應用。使用者可以登錄每一件衣物的屬性,系統透過搭配演算法產生每日穿搭建議,並將穿著行為轉化為可視化的數據洞察。</p>
        <p>目標是解決「衣櫃裡永遠少一件衣服」的問題 — 透過數據讓人重新認識自己擁有的衣物,減少重複購買、提升既有衣物的使用率。</p>
        <h3 style="margin-top:24px">核心功能</h3>
        <p>衣物資料管理、AI 穿搭推薦引擎、穿搭紀錄日誌、歷史操作軌跡,以及涵蓋分類 / 季節 / 色彩 / 穿著頻率的數據分析儀表板。</p>
        <h3 style="margin-top:24px">使用技術</h3>
        <div class="tech-list">
          <span class="tag">HTML5</span><span class="tag">CSS3 GRID</span><span class="tag">VANILLA JS</span><span class="tag">SVG CHARTS</span><span class="tag">LOCALSTORAGE</span><span class="tag">RESPONSIVE</span><span class="tag">RULE-BASED AI</span>
        </div>
      </div>
      <div class="about-pane">
        <h3>系統運作流程</h3>
        <div class="flow">
          <div class="step"><div class="no">01</div><div class="sb"><h4>建檔 INPUT</h4><p>使用者新增衣物,輸入分類、顏色、季節等結構化屬性。</p></div></div>
          <div class="step"><div class="no">02</div><div class="sb"><h4>儲存 STORE</h4><p>資料寫入本機儲存,形成個人衣物資料庫。</p></div></div>
          <div class="step"><div class="no">03</div><div class="sb"><h4>運算 MATCH</h4><p>推薦引擎依場合 / 季節 / 色彩協調規則,組合候選穿搭並評分。</p></div></div>
          <div class="step"><div class="no">04</div><div class="sb"><h4>紀錄 LOG</h4><p>採用的穿搭寫入紀錄,單品穿著次數 +1。</p></div></div>
          <div class="step"><div class="no">05</div><div class="sb"><h4>分析 ANALYZE</h4><p>彙整所有資料,輸出視覺化的衣櫃數據洞察。</p></div></div>
        </div>
      </div>
    </div>
  </section>

</main>

<footer>
  <div>© 2026 WARDROBE.AI — 資訊科學系專題</div>
  <div>AI 智慧衣櫃與穿搭數據分析系統</div>
  <div>DESIGN × CODE × DATA</div>
</footer>

<!-- MODAL -->
<div class="modal-bg" id="modal" onclick="if(event.target===this)closeModal()">
  <div class="modal">
    <div class="mimg" id="m-img">👕</div>
    <div class="mbody">
      <button class="close" onclick="closeModal()">✕</button>
      <div class="mcat" id="m-cat">CATEGORY</div>
      <h3 id="m-name">Item</h3>
      <div class="dl-list" id="m-details"></div>
      <div class="mnote" id="m-note"></div>
      <div class="modal-actions">
        <button class="btn fill" id="m-wear">穿這件 ✓</button>
        <button class="btn" id="m-del">刪除</button>
      </div>
    </div>
  </div>
</div>

<div class="toast" id="toast"><span id="toast-ic">✓</span><span id="toast-msg"></span></div>

<script>
/* ===================== DATA LAYER ===================== */
const CATEGORIES = [
  {key:"top",zh:"上衣",emoji:"👕"},
  {key:"bottom",zh:"下身",emoji:"👖"},
  {key:"outer",zh:"外套",emoji:"🧥"},
  {key:"shoes",zh:"鞋子",emoji:"👟"},
  {key:"acc",zh:"配件",emoji:"🧢"}
];
const SEASONS = ["全季","春","夏","秋","冬"];
const COLORS = [
  {name:"黑",hex:"#111111"},{name:"白",hex:"#f4f4f4"},{name:"灰",hex:"#9aa0a6"},
  {name:"米",hex:"#d8c9a8"},{name:"藍",hex:"#3b5b8c"},{name:"綠",hex:"#4a7c59"},
  {name:"紅",hex:"#b23b3b"},{name:"棕",hex:"#7a5236"}
];
const catMap = Object.fromEntries(CATEGORIES.map(c=>[c.key,c]));
function colorOf(n){return COLORS.find(c=>c.name===n)||{name:n,hex:"#666"}}

const SEED = [
  {name:"基本款白T",cat:"top",season:"夏",brand:"UNIQLO",color:"白",note:"百搭內搭,純棉。",wear:14},
  {name:"黑色寬版西裝外套",cat:"outer",season:"全季",brand:"自有",color:"黑",note:"正式 / 休閒皆可。",wear:9},
  {name:"水洗直筒牛仔褲",cat:"bottom",season:"全季",brand:"Levi's",color:"藍",note:"耐穿主力。",wear:18},
  {name:"灰色連帽帽T",cat:"top",season:"秋",brand:"GU",color:"灰",note:"舒適休閒。",wear:11},
  {name:"白色帆布鞋",cat:"shoes",season:"全季",brand:"Converse",color:"白",note:"萬用百搭。",wear:22},
  {name:"米色針織毛衣",cat:"top",season:"冬",brand:"無",color:"米",note:"保暖柔軟。",wear:7},
  {name:"卡其工裝褲",cat:"bottom",season:"秋",brand:"Dickies",color:"棕",note:"街頭風格。",wear:6},
  {name:"黑色皮革短靴",cat:"shoes",season:"冬",brand:"自有",color:"黑",note:"造型重點。",wear:4},
  {name:"深藍羽絨外套",cat:"outer",season:"冬",brand:"The North Face",color:"藍",note:"極保暖。",wear:8},
  {name:"棒球帽",cat:"acc",season:"全季",brand:"NY",color:"黑",note:"造型配件。",wear:5},
  {name:"條紋長袖上衣",cat:"top",season:"春",brand:"無",color:"白",note:"法式休閒。",wear:9},
  {name:"黑色運動短褲",cat:"bottom",season:"夏",brand:"Nike",color:"黑",note:"運動專用。",wear:12}
];

let state = {clothes:[],records:[],history:[]};
let currentRec = null;
let filter = "all";

function uid(){return "c"+Date.now().toString(36)+Math.random().toString(36).slice(2,6)}
function save(){try{localStorage.setItem("wardrobe_ai",JSON.stringify(state))}catch(e){}}
function load(){
  try{
    const raw=localStorage.getItem("wardrobe_ai");
    if(raw){state=JSON.parse(raw);return}
  }catch(e){}
  // seed first time
  state.clothes = SEED.map(s=>({id:uid(),name:s.name,cat:s.cat,season:s.season,brand:s.brand,color:s.color,note:s.note,wear:s.wear,added:Date.now()}));
  state.history = [{id:uid(),time:Date.now(),type:"sys",text:"系統初始化 — 已載入 "+SEED.length+" 件範例衣物"}];
  state.records = [];
  save();
}
function logHistory(type,text){
  state.history.unshift({id:uid(),time:Date.now(),type,text});
  if(state.history.length>60)state.history.pop();
  save();
}

/* ===================== ROUTER ===================== */
function go(v){
  document.querySelectorAll(".view").forEach(s=>s.classList.remove("active"));
  document.getElementById("view-"+v).classList.add("active");
  document.querySelectorAll(".navlink").forEach(n=>n.classList.toggle("active",n.dataset.v===v));
  document.getElementById("nav").classList.remove("open");
  window.scrollTo({top:0,behavior:"smooth"});
  if(v==="home")renderHome();
  if(v==="wardrobe")renderWardrobe();
  if(v==="records")renderRecords();
  if(v==="history")renderHistory();
  if(v==="analytics")renderAnalytics();
  if(v==="recommend")renderOutfitEmpty();
}

/* ===================== TOAST ===================== */
let toastTimer;
function toast(msg,ic="✓"){
  document.getElementById("toast-msg").textContent=msg;
  document.getElementById("toast-ic").textContent=ic;
  const t=document.getElementById("toast");t.classList.add("show");
  clearTimeout(toastTimer);toastTimer=setTimeout(()=>t.classList.remove("show"),2600);
}

/* ===================== HOME ===================== */
function animateCount(el,target){
  const dur=900,start=performance.now(),from=0;
  function step(now){
    const p=Math.min((now-start)/dur,1);
    el.textContent=Math.round(from+(target-from)*(1-Math.pow(1-p,3)));
    if(p<1)requestAnimationFrame(step);
  }
  requestAnimationFrame(step);
}
function renderHome(){
  const total=state.clothes.length;
  const looks=state.records.length;
  const cats=new Set(state.clothes.map(c=>c.cat)).size;
  const wears=state.clothes.reduce((a,c)=>a+(c.wear||0),0);
  animateCount(document.getElementById("h-total"),total);
  animateCount(document.getElementById("h-looks"),looks);
  animateCount(document.getElementById("h-cats"),cats);
  animateCount(document.getElementById("h-wears"),wears);
}

/* ===================== ADD ===================== */
let selColor = "黑";
function initForm(){
  const cat=document.getElementById("f-cat");
  cat.innerHTML=CATEGORIES.map(c=>`<option value="${c.key}">${c.zh} · ${c.key.toUpperCase()}</option>`).join("");
  document.getElementById("f-season").innerHTML=SEASONS.map(s=>`<option>${s}</option>`).join("");
  const sw=document.getElementById("f-swatches");
  sw.innerHTML=COLORS.map(c=>`<div class="swatch ${c.name===selColor?'sel':''}" style="background:${c.hex}" data-c="${c.name}" title="${c.name}"></div>`).join("");
  sw.querySelectorAll(".swatch").forEach(s=>s.onclick=()=>{
    selColor=s.dataset.c;
    sw.querySelectorAll(".swatch").forEach(x=>x.classList.toggle("sel",x.dataset.c===selColor));
    updatePreview();
  });
  ["f-name","f-cat","f-season","f-brand"].forEach(id=>document.getElementById(id).addEventListener("input",updatePreview));
  updatePreview();
}
function updatePreview(){
  const name=document.getElementById("f-name").value||"衣物名稱";
  const ck=document.getElementById("f-cat").value;
  const c=catMap[ck];
  const season=document.getElementById("f-season").value;
  const brand=document.getElementById("f-brand").value;
  const col=colorOf(selColor);
  document.getElementById("p-name").textContent=name;
  document.getElementById("p-cat").textContent=`${c.zh.toUpperCase()} · ${season}`;
  const img=document.getElementById("p-img");
  img.textContent=c.emoji;img.style.background=col.hex;
  document.getElementById("p-tags").innerHTML=
    `<span class="tag">${selColor}</span><span class="tag">${season}</span>`+(brand?`<span class="tag">${brand}</span>`:"");
}
function addClothes(){
  const name=document.getElementById("f-name").value.trim();
  if(!name){toast("請輸入衣物名稱","!");return}
  const item={id:uid(),name,cat:document.getElementById("f-cat").value,
    season:document.getElementById("f-season").value,
    brand:document.getElementById("f-brand").value.trim()||"無",
    color:selColor,note:document.getElementById("f-note").value.trim(),wear:0,added:Date.now()};
  state.clothes.unshift(item);
  logHistory("add","新增衣物：「"+name+"」("+catMap[item.cat].zh+")");
  save();
  toast("已加入衣櫃：「"+name+"」");
  resetForm();
  go("wardrobe");
}
function resetForm(){
  ["f-name","f-brand","f-note"].forEach(id=>document.getElementById(id).value="");
  document.getElementById("f-cat").selectedIndex=0;
  document.getElementById("f-season").selectedIndex=0;
  selColor="黑";initForm();
}

/* ===================== WARDROBE ===================== */
function renderWardrobe(){
  const f=document.getElementById("filters");
  const all=[{key:"all",zh:"全部"}].concat(CATEGORIES);
  f.innerHTML=all.map(c=>`<button class="chip ${filter===c.key?'active':''}" onclick="setFilter('${c.key}')">${c.zh}${c.key!=="all"?" · "+c.key.toUpperCase():" ALL"} <span style="opacity:.5">${countCat(c.key)}</span></button>`).join("");
  const grid=document.getElementById("wardrobe-grid");
  const items=state.clothes.filter(c=>filter==="all"||c.cat===filter);
  if(items.length===0){grid.innerHTML=`<div class="empty"><div class="big">∅</div>這個分類還沒有衣物。<br><button class="btn fill" style="margin-top:18px" onclick="go('add')">＋ 新增衣物</button></div>`;return}
  grid.innerHTML=items.map((c,i)=>{
    const cat=catMap[c.cat],col=colorOf(c.color);
    return `<div class="product" onclick="openModal('${c.id}')">
      <div class="ph" style="background:${col.hex}">
        <span class="num">#${String(i+1).padStart(2,'0')}</span>
        <span class="wear">穿過 ${c.wear||0}×</span>
        ${cat.emoji}
      </div>
      <div class="info">
        <div class="nm">${c.name}</div>
        <div class="cat">${cat.zh.toUpperCase()} · ${c.season} · ${c.color}</div>
      </div>
      <div class="quick">查看詳情 →</div>
    </div>`;
  }).join("");
}
function countCat(k){return k==="all"?state.clothes.length:state.clothes.filter(c=>c.cat===k).length}
function setFilter(k){filter=k;renderWardrobe()}

/* ===================== MODAL ===================== */
let modalId=null;
function openModal(id){
  const c=state.clothes.find(x=>x.id===id);if(!c)return;
  modalId=id;const cat=catMap[c.cat],col=colorOf(c.color);
  const img=document.getElementById("m-img");img.textContent=cat.emoji;img.style.background=col.hex;
  document.getElementById("m-cat").textContent=cat.zh+" · "+c.cat.toUpperCase();
  document.getElementById("m-name").textContent=c.name;
  document.getElementById("m-details").innerHTML=`
    <div class="r"><span class="k">分類 CATEGORY</span><span>${cat.zh}</span></div>
    <div class="r"><span class="k">顏色 COLOR</span><span><span style="display:inline-block;width:11px;height:11px;background:${col.hex};border:1px solid #555;margin-right:7px;vertical-align:middle"></span>${c.color}</span></div>
    <div class="r"><span class="k">適用季節 SEASON</span><span>${c.season}</span></div>
    <div class="r"><span class="k">品牌 BRAND</span><span>${c.brand}</span></div>
    <div class="r"><span class="k">穿著次數 WEARS</span><span>${c.wear||0} 次</span></div>
    <div class="r"><span class="k">建檔 ADDED</span><span>${new Date(c.added).toLocaleDateString('zh-TW')}</span></div>`;
  document.getElementById("m-note").textContent=c.note?("備註："+c.note):"（無備註）";
  document.getElementById("m-wear").onclick=()=>{wearItem(id)};
  document.getElementById("m-del").onclick=()=>{delItem(id)};
  document.getElementById("modal").classList.add("show");
}
function closeModal(){document.getElementById("modal").classList.remove("show");modalId=null}
function wearItem(id){
  const c=state.clothes.find(x=>x.id===id);if(!c)return;
  c.wear=(c.wear||0)+1;
  logHistory("wear","穿著單品：「"+c.name+"」(累計 "+c.wear+" 次)");
  save();toast("已記錄穿著：「"+c.name+"」");
  renderWardrobe();openModal(id);
}
function delItem(id){
  const c=state.clothes.find(x=>x.id===id);if(!c)return;
  state.clothes=state.clothes.filter(x=>x.id!==id);
  logHistory("del","刪除衣物：「"+c.name+"」");
  save();toast("已刪除：「"+c.name+"」","🗑");closeModal();renderWardrobe();
}

/* ===================== RECOMMEND ===================== */
function renderOutfitEmpty(){
  document.getElementById("outfit").innerHTML=`<div class="slot miss" style="grid-column:1/-1;min-height:200px">點擊「生成穿搭」,讓 AI 為你組合一套 Look ✦</div>`;
  document.getElementById("rec-meta").style.display="none";
}
function pick(cat,season){
  let pool=state.clothes.filter(c=>c.cat===cat);
  if(season!=="全季")pool=pool.filter(c=>c.season===season||c.season==="全季");
  if(pool.length===0)pool=state.clothes.filter(c=>c.cat===cat);
  if(pool.length===0)return null;
  // prefer less-worn for variety, with randomness
  pool=pool.slice().sort((a,b)=>(a.wear||0)-(b.wear||0)+(Math.random()-0.5)*6);
  return pool[Math.floor(Math.random()*Math.min(3,pool.length))];
}
function recommend(){
  const occ=document.getElementById("r-occ").value;
  const season=document.getElementById("r-season").value;
  const plan=[["outer","外套"],["top","上衣"],["bottom","下身"],["shoes","鞋子"]];
  const picks={};
  plan.forEach(([k])=>picks[k]=pick(k,season));
  if(!picks.top&&!picks.bottom){toast("衣櫃單品不足,先去新增衣物吧","!");go("add");return}
  currentRec={occ,season,picks};
  const out=document.getElementById("outfit");
  out.innerHTML=plan.map(([k,zh])=>{
    const c=picks[k],cat=catMap[k];
    if(!c)return `<div class="slot miss"><div><span class="slot-lbl">${zh.toUpperCase()}</span><br>缺少${zh}<br><small style="color:var(--dim)">可至「新增衣物」補齊</small></div></div>`;
    const col=colorOf(c.color);
    return `<div class="slot"><span class="slot-lbl">${zh}</span>
      <div class="si" style="background:${col.hex}">${cat.emoji}</div>
      <div class="sm"><div class="n">${c.name}</div><div class="c">${c.color} · ${c.season} · ${c.brand}</div></div></div>`;
  }).join("");
  // harmony score: color cohesion + season match
  const chosen=Object.values(picks).filter(Boolean);
  const colors=new Set(chosen.map(c=>c.color));
  let score=100-(colors.size-1)*9 - (4-chosen.length)*12;
  score=Math.max(58,Math.min(98,score+Math.floor(Math.random()*8)));
  const palette=[...colors].join(" / ");
  document.getElementById("rec-meta").style.display="flex";
  document.getElementById("rec-score").textContent=score+" / 100";
  document.getElementById("rec-occ").textContent=occ+" · "+season;
  document.getElementById("rec-palette").textContent=palette;
  setTimeout(()=>document.getElementById("rec-scorebar").style.width=score+"%",60);
  logHistory("rec","AI 生成穿搭推薦 ("+occ+" / "+season+")");
}
function wearThis(){
  if(!currentRec)return;
  const chosen=Object.values(currentRec.picks).filter(Boolean);
  chosen.forEach(c=>c.wear=(c.wear||0)+1);
  state.records.unshift({id:uid(),date:Date.now(),occ:currentRec.occ,season:currentRec.season,
    itemIds:chosen.map(c=>c.id),items:chosen.map(c=>({name:c.name,cat:c.cat,color:c.color}))});
  logHistory("look","採用穿搭：「"+currentRec.occ+"」共 "+chosen.length+" 件單品");
  save();toast("已記錄這套穿搭 ✓");go("records");
}

/* ===================== RECORDS ===================== */
function renderRecords(){
  const el=document.getElementById("records-list");
  if(state.records.length===0){el.innerHTML=`<div class="empty" style="border:1px solid var(--line)"><div class="big">◷</div>還沒有穿搭紀錄。<br>到「AI 穿搭推薦」生成並採用一套吧!<br><button class="btn fill" style="margin-top:18px" onclick="go('recommend')">前往 AI 推薦 →</button></div>`;return}
  el.innerHTML=state.records.map(r=>{
    const items=r.items.map(it=>`<span class="mini"><span class="e">${catMap[it.cat]?catMap[it.cat].emoji:'👕'}</span>${it.name}</span>`).join("");
    return `<div class="record-card">
      <div class="rl"><div class="occ">${r.occ}</div><div class="dt">${new Date(r.date).toLocaleString('zh-TW')} · ${r.season}</div></div>
      <div class="items">${items}</div>
    </div>`;
  }).join("");
}

/* ===================== HISTORY ===================== */
const HICON={add:"＋",del:"🗑",wear:"✓",rec:"✦",look:"◷",sys:"⚙",clear:"↻"};
function renderHistory(){
  const el=document.getElementById("history-list");
  if(state.history.length===0){el.innerHTML=`<div style="color:var(--muted)">尚無歷史紀錄。</div>`;return}
  el.innerHTML=state.history.map(h=>`
    <div class="tl-item">
      <div class="t">${new Date(h.time).toLocaleString('zh-TW')}</div>
      <div class="h">${HICON[h.type]||"•"} ${h.text}</div>
    </div>`).join("");
}
function clearHistory(){
  state.history=[{id:uid(),time:Date.now(),type:"clear",text:"已清空歷史紀錄"}];
  save();renderHistory();toast("歷史紀錄已清空","↻");
}

/* ===================== ANALYTICS ===================== */
function renderAnalytics(){
  const cl=state.clothes;
  const total=cl.length||1;
  const wears=cl.reduce((a,c)=>a+(c.wear||0),0);
  const avgWear=(wears/(cl.length||1)).toFixed(1);
  // KPI
  document.getElementById("kpis").innerHTML=[
    [cl.length,"衣物總數","TOTAL ITEMS"],
    [state.records.length,"穿搭組合","LOGGED LOOKS"],
    [wears,"總穿著次數","TOTAL WEARS"],
    [avgWear,"平均穿著","AVG WEAR / ITEM"]
  ].map(k=>`<div class="kpi"><div class="v">${k[0]}</div><div class="k">${k[1]}<span class="en">${k[2]}</span></div></div>`).join("");

  // category donut
  const catCount=CATEGORIES.map(c=>({...c,n:cl.filter(x=>x.cat===c.key).length})).filter(c=>c.n>0);
  const grays=["#f4f4f4","#bdbdbd","#8a8a8a","#5c5c5c","#333333"];
  let acc=0,segs=[];
  catCount.forEach((c,i)=>{const frac=c.n/total;segs.push({...c,start:acc,frac,col:grays[i%grays.length]});acc+=frac});
  const C=2*Math.PI*54;
  const donutSvg=`<svg width="150" height="150" viewBox="0 0 150 150" style="transform:rotate(-90deg)">
    <circle cx="75" cy="75" r="54" fill="none" stroke="#1b1b1e" stroke-width="20"/>
    ${segs.map(s=>`<circle cx="75" cy="75" r="54" fill="none" stroke="${s.col}" stroke-width="20" stroke-dasharray="${s.frac*C} ${C}" stroke-dashoffset="${-s.start*C}"><title>${s.zh} ${Math.round(s.frac*100)}%</title></circle>`).join("")}
  </svg>`;
  document.getElementById("donut").innerHTML=donutSvg;
  document.getElementById("donut-legend").innerHTML=segs.map(s=>`<div class="lg"><span class="sw" style="background:${s.col}"></span>${s.zh} <span style="color:var(--dim);font-family:'Space Mono';font-size:11px;margin-left:4px">(${s.n})</span><span class="pc">${Math.round(s.frac*100)}%</span></div>`).join("");

  // season bars
  const maxS=Math.max(1,...SEASONS.map(s=>cl.filter(c=>c.season===s).length));
  document.getElementById("season-bars").innerHTML=SEASONS.map(s=>{
    const n=cl.filter(c=>c.season===s).length;
    return `<div class="bar-row"><div class="bl">${s}</div><div class="bt"><i data-w="${n/maxS*100}"></i></div><div class="bv">${n}</div></div>`;
  }).join("");

  // color bars
  const maxC=Math.max(1,...COLORS.map(c=>cl.filter(x=>x.color===c.name).length));
  const cb=COLORS.map(c=>({...c,n:cl.filter(x=>x.color===c.name).length})).filter(c=>c.n>0).sort((a,b)=>b.n-a.n);
  document.getElementById("color-bars").innerHTML=cb.map(c=>
    `<div class="cbar"><div class="dot" style="background:${c.hex}"></div><div class="nm">${c.name}</div><div class="track"><i data-w="${c.n/maxC*100}" style="background:${c.hex}"></i></div><div class="vv">${c.n}</div></div>`).join("");

  // most worn top5
  const top=cl.slice().sort((a,b)=>(b.wear||0)-(a.wear||0)).slice(0,5);
  const maxW=Math.max(1,...top.map(c=>c.wear||0));
  document.getElementById("worn-bars").innerHTML=top.map((c,i)=>
    `<div class="bar-row"><div class="bl" style="font-size:12px">${c.name.length>6?c.name.slice(0,6)+"…":c.name}</div><div class="bt"><i class="${i===0?'acc':''}" data-w="${(c.wear||0)/maxW*100}"></i></div><div class="bv">${c.wear||0}×</div></div>`).join("");

  // animate bars
  requestAnimationFrame(()=>setTimeout(()=>{
    document.querySelectorAll("#view-analytics .bt i, #view-analytics .cbar .track i").forEach(i=>{i.style.width=(i.dataset.w||0)+"%"});
  },80));
}

/* ===================== INIT ===================== */
load();
initForm();
renderHome();
// keyboard close modal
document.addEventListener("keydown",e=>{if(e.key==="Escape")closeModal()});
</script>
</body>
</html>
