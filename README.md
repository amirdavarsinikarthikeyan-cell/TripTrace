# TripTrace
TripTrace is a travel-related web application designed to help users plan, organize, and manage their trips in one place. The website captures trip-related information and provides useful travel tools to make journey planning easier and more efficient.
..............................................................................................................................................................
This is my website URL : 
              tracethetrip.netlify.app/
CODE : 
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<meta name="theme-color" content="#1a1a2e">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<title>TripTrace — Your Travel Journal</title>
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@500;700&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
:root {
  --sky:#0f766e;--sky-light:#e6f7f5;--sky-mid:#2dd4bf;
  --sand:#f7f3eb;--sand-dark:#e8dfcf;
  --terra:#d97706;--terra-light:#fff1dc;
  --forest:#256d4d;--forest-light:#eaf7ef;
  --accent:#ff7b54;--accent-soft:#fff2ec;--accent-2:#7c3aed;--accent-2-soft:#f4ebff;
  --ink:#111827;--ink-mid:#475569;--ink-light:#94a3b8;
  --white:#ffffff;--card-bg:#ffffff;--border:#e7e2d8;
  --danger:#ef4444;--success:#10b981;--warning:#f59e0b;
  --radius:16px;--shadow:0 12px 30px rgba(15,23,42,0.08);
  --shadow-hover:0 18px 40px rgba(15,23,42,0.14);
  --nav-h:60px; --bottom-h:64px;
}
body.dark{
  --sand:#14141f;--sand-dark:#1f1f30;
  --ink:#eceef5;--ink-mid:#b8bcd4;--ink-light:#7a7e9c;
  --white:#1c1c2c;--card-bg:#1c1c2c;--border:#2c2c42;
  --sky-light:#15303d;--terra-light:#3a2418;--forest-light:#16261a;
  --shadow:0 2px 16px rgba(0,0,0,.4);--shadow-hover:0 6px 24px rgba(0,0,0,.55);
}
body.dark .auth-card,body.dark .modal,body.dark .confirm-box{background:var(--card-bg);color:var(--ink);}
body.dark input,body.dark select,body.dark textarea{background:#22223a;color:var(--ink);border-color:var(--border);} 
body.dark select option{background:var(--white);color:var(--ink);} 
body.dark .hero-bg{background:linear-gradient(135deg,rgba(10,10,18,.9) 0%,rgba(15,55,70,.85) 100%);}
body.dark table{color:var(--ink);}
body.dark th{color:var(--ink-light);} 
body.dark .toast{background:#2c2c42;}
body.dark .btn-outline{background:rgba(255,255,255,.08);color:var(--ink);border-color:rgba(255,255,255,.24);} 
body.dark .btn-ghost{background:rgba(255,255,255,.06);color:var(--ink);border-color:var(--border);} 
body.dark .btn-sm{color:var(--ink);} 
body.dark .form-section button.active{background:var(--sky);color:#fff;border-color:var(--sky);}
body.dark .card{background:var(--card-bg);color:var(--ink);border-color:var(--border);}
body.dark nav{background:var(--card-bg);border-color:var(--border);}
body.dark .tabs{border-color:var(--border);}
body.dark .stat-card{background:var(--card-bg);color:var(--ink);border-color:var(--border);}
body.dark .waypoint{background:var(--card-bg);color:var(--ink);}
body.dark .place-card{background:var(--card-bg);color:var(--ink);}
body.dark .alert-info{background:var(--sky-light);}
body.dark .alert-success{background:var(--forest-light);}
body.dark .alert-danger{background:#3a1414;}
body.dark .section-title{color:var(--ink);}
body.dark .bottom-nav{background:var(--card-bg);border-color:var(--border);}
body.dark .map-layer-btn{background:var(--card-bg);color:var(--ink);border-color:var(--border);}
body.dark .map-layer-btn.active{background:var(--sky);color:#fff;border-color:var(--sky);}

*{box-sizing:border-box;margin:0;padding:0;}
body{font-family:'Inter',sans-serif;background:radial-gradient(circle at top left,rgba(45,212,191,.16),transparent 24%),radial-gradient(circle at top right,rgba(255,123,84,.16),transparent 24%),linear-gradient(135deg,#fcfaf7 0%,var(--sand) 45%,#efe7d8 100%);color:var(--ink);min-height:100vh;padding-bottom:var(--bottom-h);position:relative;overflow-x:hidden;}body::before{content:'';position:fixed;inset:0;pointer-events:none;opacity:.2;background-image:linear-gradient(rgba(255,255,255,.45) 1px,transparent 1px),linear-gradient(90deg,rgba(255,255,255,.45) 1px,transparent 1px);background-size:24px 24px;mask-image:linear-gradient(to bottom,rgba(0,0,0,.8),transparent);}
h1,h2,h3{font-family:'Playfair Display',serif;}

/* ── NAV ── */
nav{background:linear-gradient(135deg,#111827 0%,#0f766e 100%);color:#fff;display:flex;align-items:center;justify-content:space-between;padding:0 20px;height:var(--nav-h);position:sticky;top:0;z-index:200;box-shadow:0 12px 28px rgba(15,23,42,.18);backdrop-filter:blur(12px);}
.nav-brand{font-family:'Playfair Display',serif;font-size:22px;cursor:pointer;letter-spacing:-.5px;}
.nav-brand span{color:var(--sky-mid);}
.nav-right{display:flex;align-items:center;gap:8px;}
.desktop-nav{display:flex;gap:4px;align-items:center;}
.desktop-nav button{background:none;border:none;color:rgba(255,255,255,.7);padding:6px 12px;border-radius:6px;cursor:pointer;font-family:'Inter',sans-serif;font-size:13px;font-weight:500;transition:all .2s;}
.desktop-nav button:hover,.desktop-nav button.active{background:rgba(255,255,255,.12);color:#fff;}
.avatar{width:34px;height:34px;border-radius:50%;background:var(--sky-mid);display:flex;align-items:center;justify-content:center;font-weight:600;font-size:14px;cursor:pointer;flex-shrink:0;}
.sos-btn{background:var(--danger)!important;color:#fff!important;border:none;padding:6px 12px;border-radius:20px;font-size:12px;font-weight:700;cursor:pointer;animation:sosPulse 2s infinite;flex-shrink:0;}
@keyframes sosPulse{0%,100%{box-shadow:0 0 0 0 rgba(192,57,43,.4)}50%{box-shadow:0 0 0 8px rgba(192,57,43,0)}}

/* ── BOTTOM TAB BAR (mobile) ── */
.bottom-nav{display:none;position:fixed;bottom:0;left:0;right:0;height:var(--bottom-h);background:var(--ink);border-top:1px solid rgba(255,255,255,.1);z-index:200;flex-direction:row;align-items:stretch;}
.tab-btn{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:3px;background:none;border:none;color:rgba(255,255,255,.55);cursor:pointer;font-family:'Inter',sans-serif;font-size:10px;font-weight:500;padding:6px 2px;transition:color .2s;}
.tab-btn.active{color:var(--sky-mid);}
.tab-btn svg{width:20px;height:20px;stroke:currentColor;fill:none;stroke-width:2;stroke-linecap:round;stroke-linejoin:round;}

/* ── PAGES ── */
.page{display:none;}
.page.active{display:block;}

/* ── HERO ── */
.hero{position:relative;overflow:hidden;color:#fff;padding:86px 32px 72px;text-align:center;border-radius:0 0 28px 28px;margin-bottom:24px;} .hero::before,.hero::after{content:'';position:absolute;border-radius:50%;filter:blur(2px);pointer-events:none}.hero::before{width:280px;height:280px;background:rgba(255,255,255,.16);top:-120px;right:-100px}.hero::after{width:240px;height:240px;background:rgba(255,255,255,.12);bottom:-110px;left:-70px}
.hero-bg{position:absolute;inset:0;background:linear-gradient(120deg,rgba(17,24,39,.92) 0%,rgba(15,118,110,.84) 48%,rgba(217,119,6,.78) 100%);z-index:1;}
.hero-img{position:absolute;inset:0;background-size:cover;background-position:center;filter:brightness(.55);}
.hero-content{position:relative;z-index:2;}
.hero h1{font-size:48px;margin-bottom:14px;text-shadow:0 2px 8px rgba(0,0,0,.4);}
.hero p{font-size:17px;opacity:.9;margin-bottom:32px;}
.hero-btns{display:flex;gap:12px;justify-content:center;flex-wrap:wrap;}
.btn-primary{background:linear-gradient(135deg,var(--terra) 0%,#f59e0b 100%);color:#fff;border:none;padding:13px 26px;border-radius:999px;font-size:15px;font-weight:600;cursor:pointer;transition:all .2s;font-family:'Inter',sans-serif;box-shadow:0 12px 24px rgba(217,119,6,.26);} .btn-primary:hover{background:linear-gradient(135deg,#b45309 0%,var(--terra) 100%);transform:translateY(-2px);box-shadow:0 16px 28px rgba(217,119,6,.32);} .btn-outline{background:rgba(255,255,255,.12);color:#fff;border:1.5px solid rgba(255,255,255,.35);padding:13px 26px;border-radius:999px;font-size:15px;font-weight:600;cursor:pointer;transition:all .2s;font-family:'Inter',sans-serif;backdrop-filter:blur(8px);} .btn-outline:hover{border-color:#fff;background:rgba(255,255,255,.2);transform:translateY(-2px);}
.btn-primary:hover{background:#b0551f;transform:translateY(-1px);}
.btn-outline{background:transparent;color:#fff;border:2px solid rgba(255,255,255,.5);padding:13px 26px;border-radius:var(--radius);font-size:15px;font-weight:600;cursor:pointer;transition:all .2s;font-family:'Inter',sans-serif;}
.btn-outline:hover{border-color:#fff;background:rgba(255,255,255,.1);}

/* ── CONTAINERS & GRID ── */
.container{max-width:1100px;margin:0 auto;padding:32px 16px;}
.section-title{font-size:26px;margin-bottom:4px;color:var(--ink);}
.section-sub{color:var(--ink-light);margin-bottom:24px;font-size:14px;}
.grid-3{display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:18px;}
.grid-2{display:grid;grid-template-columns:repeat(auto-fill,minmax(340px,1fr));gap:18px;}

/* ── CARDS ── */
.card{background:rgba(255,255,255,.94);border:1px solid rgba(231,226,216,.95);border-radius:20px;box-shadow:0 14px 38px rgba(15,23,42,.08);overflow:hidden;transition:all .2s;backdrop-filter:blur(8px);} .card:hover{box-shadow:0 20px 44px rgba(15,23,42,.12);transform:translateY(-3px);} .signature-strip{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:12px;margin:24px 0 28px;}.signature-card{background:linear-gradient(135deg,var(--accent-soft),#fff);border:1px solid rgba(255,123,84,.16);border-radius:16px;padding:14px 16px;box-shadow:0 8px 20px rgba(15,23,42,.06);font-size:13px;color:var(--ink-mid);}.signature-card strong{display:block;font-size:14px;color:var(--ink);margin-bottom:4px;}.signature-card .chip{display:inline-flex;align-items:center;gap:6px;padding:5px 8px;border-radius:999px;background:var(--sky-light);color:var(--sky);font-size:11px;font-weight:700;margin-top:6px;}
.card:hover{box-shadow:var(--shadow-hover);transform:translateY(-2px);}
.card-body{padding:16px;}
.card-footer{padding:12px 16px;background:var(--sand);border-top:1px solid var(--border);display:flex;gap:8px;flex-wrap:wrap;}

/* ── TRIP CARD ── */
.trip-thumb{height:160px;position:relative;overflow:hidden;display:flex;align-items:center;justify-content:center;background:#e0e8ef;}
.trip-thumb img{width:100%;height:100%;object-fit:cover;display:block;}
.trip-thumb .emoji-fallback{font-size:52px;position:absolute;}
.trip-thumb .img-overlay{position:absolute;inset:0;background:linear-gradient(to bottom,transparent 40%,rgba(0,0,0,.5));z-index:1;}
.trip-badge{position:absolute;top:10px;right:10px;z-index:2;padding:3px 9px;border-radius:20px;font-size:11px;font-weight:600;}
.badge-planned{background:#e8f4f8;color:var(--sky);}
.badge-ongoing{background:#e8f5e8;color:var(--forest);}
.badge-completed{background:var(--terra-light);color:var(--terra);}
.trip-title{font-size:17px;margin-bottom:3px;}
.trip-meta{font-size:12px;color:var(--ink-light);margin-bottom:2px;}
.trip-stats{display:flex;gap:14px;margin-top:10px;}
.stat{text-align:center;}
.stat-val{font-size:16px;font-weight:700;color:var(--sky);}
.stat-label{font-size:10px;color:var(--ink-light);text-transform:uppercase;letter-spacing:.5px;}

/* ── STAT CARDS ── */
.stat-cards{display:grid;grid-template-columns:repeat(auto-fill,minmax(175px,1fr));gap:14px;margin-bottom:28px;}
.stat-card{background:linear-gradient(145deg,rgba(255,255,255,.98),rgba(247,243,235,.96));border:1px solid rgba(231,226,216,.95);border-radius:18px;padding:18px;box-shadow:0 10px 24px rgba(15,23,42,.06);text-align:center;}
.stat-card .big{font-size:32px;font-weight:700;margin-bottom:4px;}
.stat-card .lbl{font-size:12px;color:var(--ink-light);font-weight:500;}
.stat-card.sky .big{color:var(--sky);}
.stat-card.terra .big{color:var(--terra);}
.stat-card.forest .big{color:var(--forest);}
.stat-card.warn .big{color:var(--warning);}

/* ── FORMS ── */
.form-group{margin-bottom:16px;}
.form-group label{display:block;font-size:12px;font-weight:600;margin-bottom:5px;color:var(--ink-mid);}
.form-group input,.form-group select,.form-group textarea{width:100%;padding:10px 13px;border:1.5px solid var(--border);border-radius:10px;font-family:'Inter',sans-serif;font-size:14px;color:var(--ink);background:rgba(255,255,255,.96);transition:all .2s;box-shadow:inset 0 1px 2px rgba(15,23,42,.03);} .form-group input:focus,.form-group select:focus,.form-group textarea:focus{outline:none;border-color:var(--sky);box-shadow:0 0 0 4px rgba(15,118,110,.12);}
.form-group input:focus,.form-group select:focus,.form-group textarea:focus{outline:none;border-color:var(--sky);}
.form-row{display:grid;grid-template-columns:1fr 1fr;gap:14px;}
.form-section{background:var(--card-bg);border-radius:var(--radius);padding:24px;box-shadow:var(--shadow);margin-bottom:20px;}
.form-section-title{font-size:17px;margin-bottom:18px;padding-bottom:10px;border-bottom:1px solid var(--border);}

/* ── BUTTONS ── */
.btn-sm{padding:6px 13px;border-radius:6px;border:none;cursor:pointer;font-size:13px;font-weight:600;font-family:'Inter',sans-serif;transition:all .2s;}
.btn-sky{background:var(--sky);color:#fff;}
.btn-sky:hover{background:#145870;}
.btn-terra{background:var(--terra);color:#fff;}
.btn-terra:hover{background:#b0551f;}
.btn-ghost{background:var(--sand);color:var(--ink-mid);border:1px solid var(--border);}
.btn-ghost:hover{background:var(--sand-dark);}
.btn-danger{background:var(--danger);color:#fff;}
.btn-danger:hover{background:#a93226;}
.btn-success{background:var(--success);color:#fff;}
.btn-success:hover{background:#1e8449;}
.btn-lg{padding:11px 22px;font-size:14px;border-radius:var(--radius);}

/* ── TABS ── */
.tabs{display:flex;gap:4px;border-bottom:2px solid var(--border);margin-bottom:20px;overflow-x:auto;}
.tab{padding:9px 18px;border:none;background:none;cursor:pointer;font-family:'Inter',sans-serif;font-size:14px;font-weight:500;color:var(--ink-light);border-bottom:2px solid transparent;margin-bottom:-2px;transition:all .2s;white-space:nowrap;}
.tab.active{color:var(--sky);border-bottom-color:var(--sky);}
.tab:hover{color:var(--sky);}

/* ── EXPENSE TABLE ── */
table{width:100%;border-collapse:collapse;}
th{text-align:left;padding:9px 13px;font-size:11px;text-transform:uppercase;letter-spacing:.5px;color:var(--ink-light);border-bottom:2px solid var(--border);}
td{padding:11px 13px;border-bottom:1px solid var(--border);font-size:13px;}
tr:last-child td{border-bottom:none;}
tr:hover td{background:var(--sand);}
.cat-badge{padding:2px 7px;border-radius:12px;font-size:11px;font-weight:600;}
.cat-food{background:#fef3cd;color:#856404;}
.cat-hotel{background:#cce5ff;color:#004085;}
.cat-transport{background:#d4edda;color:#155724;}
.cat-activity{background:#f8d7da;color:#721c24;}
.cat-shopping{background:#e2d9f3;color:#4a235a;}
.cat-other{background:var(--sand);color:var(--ink-mid);}

/* ── PLACE CARDS ── */
.place-card{background:#fff;border-radius:var(--radius);padding:14px;box-shadow:var(--shadow);margin-bottom:10px;display:flex;gap:12px;transition:all .2s;}
.place-card:hover{box-shadow:var(--shadow-hover);}
.place-icon{font-size:28px;width:42px;text-align:center;flex-shrink:0;}
.place-info{flex:1;min-width:0;}
.place-name{font-weight:600;font-size:14px;margin-bottom:3px;}
.place-meta{font-size:12px;color:var(--ink-light);margin-bottom:4px;}
.stars{color:#f39c12;font-size:13px;}
.place-cost{font-weight:700;color:var(--forest);font-size:14px;}
.place-actions{display:flex;flex-direction:column;gap:6px;align-items:flex-end;flex-shrink:0;}

/* ── GALLERY ── */
.gallery-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(160px,1fr));gap:10px;}
.gallery-item{aspect-ratio:1;border-radius:8px;overflow:hidden;cursor:pointer;transition:all .2s;position:relative;background:var(--sky-light);}
.gallery-item img{width:100%;height:100%;object-fit:cover;display:block;}
.gallery-item .gal-emoji{position:absolute;inset:0;display:flex;align-items:center;justify-content:center;font-size:48px;}
.gallery-item .overlay{position:absolute;inset:0;background:rgba(0,0,0,.5);display:flex;align-items:flex-end;padding:10px;opacity:0;transition:opacity .2s;color:#fff;font-size:12px;font-weight:600;}
.gallery-item:hover{transform:scale(1.03);}
.gallery-item:hover .overlay{opacity:1;}

/* ── MODALS ── */
.modal-overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.55);z-index:1000;align-items:center;justify-content:center;padding:16px;}
.modal-overlay.open{display:flex;}
.modal{background:rgba(255,255,255,.97);border:1px solid rgba(231,226,216,.95);border-radius:22px;width:100%;max-width:540px;max-height:90vh;overflow-y:auto;box-shadow:0 24px 70px rgba(15,23,42,.16);backdrop-filter:blur(12px);}
.modal-header{padding:18px 22px;border-bottom:1px solid var(--border);display:flex;align-items:center;justify-content:space-between;position:sticky;top:0;background:#fff;z-index:1;}
.modal-header h3{font-size:19px;}
.modal-close{background:none;border:none;font-size:22px;cursor:pointer;color:var(--ink-light);line-height:1;}
.modal-body{padding:22px;}
.modal-footer{padding:14px 22px;border-top:1px solid var(--border);display:flex;gap:10px;justify-content:flex-end;}

/* ── ALERTS ── */
.alert{padding:11px 15px;border-radius:8px;font-size:13px;margin-bottom:14px;}
.alert-info{background:var(--sky-light);color:var(--sky);border-left:3px solid var(--sky);}
.alert-success{background:var(--forest-light);color:var(--forest);border-left:3px solid var(--forest);}
.alert-danger{background:#fdf0f0;color:var(--danger);border-left:3px solid var(--danger);}

/* ── EMPTY STATE ── */
.empty{text-align:center;padding:50px 20px;color:var(--ink-light);}
.empty .icon{font-size:52px;margin-bottom:14px;}
.empty h3{font-size:19px;color:var(--ink-mid);margin-bottom:6px;}
.empty p{font-size:13px;margin-bottom:20px;}

/* ── PROFILE ── */
.profile-hero{background:linear-gradient(135deg,var(--ink) 0%,var(--sky) 100%);color:#fff;padding:32px;display:flex;align-items:center;gap:20px;border-radius:var(--radius);margin-bottom:20px;}
.profile-avatar-big{width:72px;height:72px;border-radius:50%;background:var(--sky-mid);display:flex;align-items:center;justify-content:center;font-size:28px;font-weight:700;border:3px solid rgba(255,255,255,.4);flex-shrink:0;}

/* ── AUTH ── */
.auth-container{min-height:calc(100vh - var(--nav-h));display:flex;align-items:center;justify-content:center;padding:32px 16px;}
.auth-card{background:#fff;border-radius:16px;padding:36px;width:100%;max-width:420px;box-shadow:var(--shadow-hover);}
.auth-card h2{font-size:26px;margin-bottom:4px;}
.auth-card .sub{color:var(--ink-light);margin-bottom:24px;font-size:13px;}
.auth-tabs{display:flex;gap:8px;margin-bottom:24px;border:1px solid var(--border);border-radius:12px;overflow:hidden;background:var(--sand);}
.auth-tab{flex:1;text-align:center;padding:12px 0;font-weight:700;cursor:pointer;background:transparent;border:none;color:var(--ink-mid);transition:all .2s;font-family:'Inter',sans-serif;}
.auth-tab.active{background:var(--ink);color:#fff;}
.auth-panel{display:none;}
.auth-panel.active{display:block;}
.divider{display:flex;align-items:center;gap:10px;margin:16px 0;color:var(--ink-light);font-size:12px;}
.divider::before,.divider::after{content:'';flex:1;height:1px;background:var(--border);}

/* ── PROGRESS ── */
.progress-bar{background:var(--border);border-radius:4px;height:7px;overflow:hidden;}
.progress-fill{height:100%;border-radius:4px;background:var(--sky);transition:width .5s;}
.progress-fill.warn{background:var(--warning);}
.progress-fill.danger{background:var(--danger);}

/* ── WAYPOINTS ── */
.waypoint{display:flex;align-items:center;gap:10px;padding:10px 14px;background:#fff;border-radius:8px;margin-bottom:8px;box-shadow:var(--shadow);cursor:pointer;transition:all .2s;}
.waypoint:hover{transform:translateX(3px);}
.wp-info{flex:1;min-width:0;}
.wp-name{font-weight:600;font-size:13px;}
.wp-detail{font-size:11px;color:var(--ink-light);margin-top:1px;}

.timeline{display:flex;flex-direction:column;gap:12px;}
.timeline-item{display:grid;grid-template-columns:100px 1fr;gap:12px;align-items:flex-start;}
.timeline-date{font-size:12px;font-weight:700;color:var(--sky);}
.timeline-card{background:var(--sand);border-radius:12px;padding:14px;box-shadow:var(--shadow);}
.timeline-card .timeline-title{font-size:14px;font-weight:700;margin-bottom:6px;}
.timeline-card .timeline-detail{font-size:13px;color:var(--ink-mid);line-height:1.6;}

/* ── AI CHAT ── */
.ai-chat-box{background:#fff;border-radius:var(--radius);box-shadow:var(--shadow);overflow:hidden;}
.ai-messages{height:340px;overflow-y:auto;padding:16px;display:flex;flex-direction:column;gap:12px;background:#fafaf8;}
.ai-msg{max-width:88%;padding:11px 14px;border-radius:12px;font-size:13px;line-height:1.65;}
.ai-msg.user{align-self:flex-end;background:var(--sky);color:#fff;border-radius:12px 12px 2px 12px;}
.ai-msg.bot{align-self:flex-start;background:#fff;color:var(--ink);border:1px solid var(--border);border-radius:12px 12px 12px 2px;}
.ai-msg.loading{align-self:flex-start;background:#fff;border:1px solid var(--border);border-radius:12px 12px 12px 2px;color:var(--ink-light);font-style:italic;}
.ai-input-row{display:flex;gap:8px;padding:12px;border-top:1px solid var(--border);}
.ai-input-row input{flex:1;padding:9px 13px;border:1.5px solid var(--border);border-radius:8px;font-family:inherit;font-size:13px;}
.ai-input-row input:focus{outline:none;border-color:var(--sky);}
.ai-quick-btns{display:flex;gap:6px;flex-wrap:wrap;padding:0 12px 10px;}
.ai-quick-btn{padding:4px 10px;background:var(--sky-light);color:var(--sky);border:none;border-radius:20px;font-size:11px;font-weight:600;cursor:pointer;font-family:inherit;}
.ai-quick-btn:hover{background:var(--sky);color:#fff;}

/* ── SOS ── */
.sos-big-btn{width:100%;padding:20px;font-size:20px;font-weight:800;background:var(--danger);color:#fff;border:none;border-radius:var(--radius);cursor:pointer;font-family:'Inter',sans-serif;animation:sosPulse 2s infinite;letter-spacing:1px;}
.sos-info-card{background:#fff;border-radius:var(--radius);padding:18px;box-shadow:var(--shadow);margin-bottom:14px;}
.emergency-contact{display:flex;align-items:center;justify-content:space-between;padding:10px 0;border-bottom:1px solid var(--border);}
.emergency-contact:last-child{border-bottom:none;}

/* ── ANALYTICS ── */
.weather-forecast-day{text-align:center;padding:8px;background:rgba(255,255,255,.3);border-radius:8px;min-width:60px;}
.chart-bar-wrap{margin-bottom:12px;}
.chart-bar-label{display:flex;justify-content:space-between;font-size:12px;margin-bottom:4px;}

/* ── SEARCH BAR ── */
.search-bar{display:flex;align-items:center;gap:10px;margin-bottom:16px;}
.search-bar input{flex:1;padding:9px 14px;border:1.5px solid var(--border);border-radius:8px;font-family:inherit;font-size:14px;background:#fff;}
.search-bar input:focus{outline:none;border-color:var(--sky);}

/* ── TAG ── */
.tag{display:inline-block;padding:3px 9px;border-radius:12px;font-size:11px;font-weight:600;margin:2px;background:var(--sky-light);color:var(--sky);}

/* ── TOAST ── */
.toast{position:fixed;bottom:80px;right:16px;z-index:9999;background:var(--ink);color:#fff;padding:11px 18px;border-radius:8px;font-size:13px;font-weight:500;box-shadow:0 4px 20px rgba(0,0,0,.2);transform:translateY(100px);transition:transform .3s;pointer-events:none;max-width:280px;}
.toast.show{transform:translateY(0);}

/* ── ITINERARY ── */
.itinerary-day{background:#fff;border-radius:var(--radius);padding:18px;box-shadow:var(--shadow);margin-bottom:14px;}
.day-header{display:flex;align-items:center;gap:10px;margin-bottom:12px;}
.day-num{width:34px;height:34px;background:var(--sky);color:#fff;border-radius:50%;display:flex;align-items:center;justify-content:center;font-weight:700;font-size:13px;flex-shrink:0;}
.activity-item{display:flex;gap:10px;align-items:flex-start;padding:9px 0;border-bottom:1px solid var(--border);}
.activity-item:last-child{border-bottom:none;}
.activity-time{font-size:11px;color:var(--ink-light);font-weight:600;min-width:46px;padding-top:2px;}

/* ── PHOTO UPLOAD ── */
.photo-upload-area{border:2px dashed var(--border);border-radius:8px;padding:24px;text-align:center;cursor:pointer;transition:all .2s;background:var(--sand);}
.photo-upload-area:hover{border-color:var(--sky);background:var(--sky-light);}
.photo-upload-area input{display:none;}
.photo-preview{display:grid;grid-template-columns:repeat(auto-fill,minmax(80px,1fr));gap:8px;margin-top:10px;}
.photo-preview-item{aspect-ratio:1;border-radius:6px;overflow:hidden;position:relative;}
.photo-preview-item img{width:100%;height:100%;object-fit:cover;}
.receipt-upload{border:2px dashed var(--border);border-radius:8px;padding:18px;text-align:center;cursor:pointer;transition:all .2s;background:var(--sand);font-size:13px;color:var(--ink-mid);}
.receipt-upload:hover{border-color:var(--sky);background:var(--sky-light);}
.receipt-upload input{display:none;}
.receipt-preview{margin-top:10px;display:flex;gap:10px;flex-wrap:wrap;}
.receipt-preview img{max-height:120px;border-radius:8px;object-fit:cover;}

/* ── RESPONSIVE ── */
@media(max-width:700px){
  body{padding-bottom:calc(var(--bottom-h) + 8px);}
  .hero h1{font-size:30px;}
  .desktop-nav{display:none;}
  .bottom-nav{display:flex;}
  .form-row{grid-template-columns:1fr;}
  .grid-2,.grid-3{grid-template-columns:1fr;}
  .stat-cards{grid-template-columns:repeat(2,1fr);}
  .container{padding:20px 12px;}
  .toast{bottom:calc(var(--bottom-h) + 12px);}
}
@media(max-width:380px){
  .stat-cards{grid-template-columns:1fr 1fr;}
}

/* ── ITINERARY BUILDER ── */
.itin-day-card{background:var(--card-bg);border-radius:var(--radius);box-shadow:var(--shadow);margin-bottom:16px;overflow:hidden;}
.itin-day-header{background:linear-gradient(135deg,var(--sky),var(--sky-mid));color:#fff;padding:14px 18px;display:flex;align-items:center;justify-content:space-between;gap:10px;}
.itin-day-header .itin-day-title{font-size:15px;font-weight:700;}
.itin-day-header .itin-day-date{font-size:12px;opacity:.85;}
.itin-activity-row{display:flex;gap:12px;align-items:flex-start;padding:12px 18px;border-bottom:1px solid var(--border);}
.itin-activity-row:last-child{border-bottom:none;}
.itin-activity-time{min-width:54px;font-size:12px;font-weight:700;color:var(--sky);padding-top:2px;}
.itin-activity-emoji{font-size:20px;flex-shrink:0;}
.itin-activity-body{flex:1;min-width:0;}
.itin-activity-name{font-size:14px;font-weight:600;color:var(--ink);}
.itin-activity-notes{font-size:12px;color:var(--ink-light);margin-top:2px;}
.itin-add-row{padding:12px 18px;}

/* ── DIARY CARD ── */
.diary-card{background:var(--card-bg);border-radius:var(--radius);padding:18px;box-shadow:var(--shadow);margin-bottom:14px;border-left:4px solid var(--sky);}
.diary-card strong{font-size:14px;color:var(--ink);display:block;margin-bottom:4px;}
.diary-card small{font-size:11px;color:var(--ink-light);display:block;margin-bottom:10px;}

/* ── ACHIEVEMENT BADGE ── */
.badge-card{background:var(--card-bg);border-radius:var(--radius);padding:16px 12px;box-shadow:var(--shadow);text-align:center;transition:all .2s;border:2px solid transparent;}
.badge-card.unlocked{border-color:var(--warning);box-shadow:0 0 0 3px rgba(243,156,18,.15);}
.badge-card.locked{opacity:.45;filter:grayscale(1);}
.badge-card .badge-icon{font-size:36px;margin-bottom:8px;}
.badge-card .badge-name{font-size:12px;font-weight:700;color:var(--ink);margin-bottom:4px;}
.badge-card .badge-desc{font-size:10px;color:var(--ink-light);line-height:1.4;}
.badge-card.unlocked .badge-name{color:var(--terra);}

/* ── CUSTOM CONFIRM MODAL ── */
.confirm-overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.55);z-index:2000;align-items:center;justify-content:center;padding:16px;}
.confirm-overlay.open{display:flex;}
.confirm-box{background:#fff;border-radius:var(--radius);width:100%;max-width:380px;box-shadow:0 20px 60px rgba(0,0,0,.25);overflow:hidden;}
.confirm-box .confirm-header{padding:18px 22px;border-bottom:1px solid var(--border);display:flex;align-items:center;gap:12px;}
.confirm-box .confirm-icon{font-size:24px;}
.confirm-box .confirm-title{font-size:17px;font-weight:700;color:var(--ink);}
.confirm-box .confirm-body{padding:18px 22px;font-size:14px;color:var(--ink-mid);line-height:1.6;}
.confirm-box .confirm-footer{padding:14px 22px;border-top:1px solid var(--border);display:flex;gap:10px;justify-content:flex-end;}

/* ── SESSION LOADING OVERLAY ── */
#session-loading{position:fixed;inset:0;background:linear-gradient(135deg,#111827 0%,#0f766e 100%);z-index:5000;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:14px;color:#fff;}
#session-loading .spinner{width:38px;height:38px;border:3px solid rgba(255,255,255,.25);border-top-color:#fff;border-radius:50%;animation:spin .8s linear infinite;}
@keyframes spin{to{transform:rotate(360deg);}}
#session-loading p{font-family:'Inter',sans-serif;font-size:13px;opacity:.85;}

/* ── SYNC STATUS ── */
.sync-status{display:flex;align-items:center;gap:5px;font-size:11px;font-weight:600;color:rgba(255,255,255,.6);padding:4px 9px;border-radius:20px;background:rgba(255,255,255,.08);flex-shrink:0;transition:all .2s;}
.sync-status .dot{width:7px;height:7px;border-radius:50%;background:var(--ink-light);flex-shrink:0;}
.sync-status.saving .dot{background:var(--warning);animation:sosPulse 1s infinite;}
.sync-status.saved .dot{background:var(--success);}
.sync-status.offline .dot{background:var(--ink-light);}
.sync-status.error .dot{background:var(--danger);}
</style>
</head>
<body>

<!-- Shown until Firebase resolves whether you're already logged in -->
<div id="session-loading">
  <div class="spinner"></div>
  <p>Checking your session…</p>
</div>

<!-- NAV -->
<nav>
  <div class="nav-brand" onclick="showPage('home')">Trip<span>Trace</span></div>
  <div class="desktop-nav">
    <button onclick="showPage('home')" id="nav-home">Home</button>
    <button onclick="showPage('dashboard')" id="nav-dashboard">Dashboard</button>
    <button onclick="showPage('trips')" id="nav-trips">My Trips</button>
    <button onclick="showPage('map')" id="nav-map">Map</button>
    <button onclick="showPage('itinerary')" id="nav-itinerary">📅 Itinerary</button>
    <button onclick="showPage('packing')" id="nav-packing">🎒 Packing</button>
    <button onclick="showPage('expenses')" id="nav-expenses">Expenses</button>
    <button onclick="showPage('places')" id="nav-places">🏨 Places</button>
    <button onclick="showPage('gallery')" id="nav-gallery">Gallery</button>
    <button onclick="showPage('documents')" id="nav-documents">📄 Docs</button>
    <button onclick="showPage('analytics')" id="nav-analytics">📊 Analytics</button>
  </div>
  <div class="nav-right">
    <div class="sync-status" id="sync-status" title="Cloud sync status"><span class="dot"></span><span id="sync-status-text">Saved</span></div>
    <button class="btn-sm" onclick="toggleDarkMode()" id="dark-toggle-btn" style="background:transparent;border:none;font-size:18px;cursor:pointer;" title="Toggle dark mode">🌙</button>
    <button class="sos-btn" onclick="showPage('sos')">🆘 SOS</button>
    <div style="position:relative;">
      <button class="btn-sm" onclick="openNotifications()" style="position:relative;background:transparent;border:none;font-size:20px;">🔔
        <span id="notif-badge" style="position:absolute;top:-8px;right:-8px;background:#c02;color:#fff;border-radius:50%;width:20px;height:20px;display:none;align-items:center;justify-content:center;font-size:11px;font-weight:700;">0</span>
      </button>
    </div>
    <div class="avatar" onclick="showPage('profile')" id="user-avatar">?</div>
    <button class="btn-sm btn-ghost" onclick="logout()" style="margin-left:8px;">Logout</button>
  </div>
</nav>

<!-- BOTTOM NAV (mobile) -->
<div class="bottom-nav">
  <button class="tab-btn" onclick="showPage('home')" id="bnav-home">
    <svg viewBox="0 0 24 24"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/><polyline points="9,22 9,12 15,12 15,22"/></svg>Home
  </button>
  <button class="tab-btn" onclick="showPage('trips')" id="bnav-trips">
    <svg viewBox="0 0 24 24"><circle cx="12" cy="12" r="10"/><polyline points="12,6 12,12 16,14"/></svg>Trips
  </button>
  <button class="tab-btn" onclick="showPage('expenses')" id="bnav-expenses">
    <svg viewBox="0 0 24 24"><line x1="12" y1="1" x2="12" y2="23"/><path d="M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"/></svg>Expenses
  </button>
  <button class="tab-btn" onclick="showPage('places')" id="bnav-places">
    <svg viewBox="0 0 24 24"><path d="M3 21h18"/><path d="M5 21V7l8-4 8 4v14"/><path d="M9 9h1"/><path d="M14 9h1"/><path d="M9 13h1"/><path d="M14 13h1"/></svg>Places
  </button>
  <button class="tab-btn" onclick="showPage('itinerary')" id="bnav-itinerary">
    <svg viewBox="0 0 24 24"><rect x="3" y="4" width="18" height="18" rx="2"/><line x1="16" y1="2" x2="16" y2="6"/><line x1="8" y1="2" x2="8" y2="6"/><line x1="3" y1="10" x2="21" y2="10"/></svg>Plan
  </button>
  <button class="tab-btn" onclick="showPage('packing')" id="bnav-packing">
  <svg viewBox="0 0 24 24">
    <path d="M6 7h12v14H6z"/>
    <path d="M9 7V5a3 3 0 0 1 6 0v2"/>
    <path d="M6 12h12"/>
  </svg>
  Packing
</button>
  <button class="tab-btn" onclick="showPage('profile')" id="bnav-profile">
    <svg viewBox="0 0 24 24"><path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></svg>Profile
  </button>
</div>

<div class="toast" id="toast"></div>

<!-- ═══════ HOME ═══════ -->
<div class="page active" id="page-home">
  <div class="hero" id="hero-section">
    <div class="hero-img" id="hero-img" style="background-image:url('https://images.unsplash.com/photo-1488646953014-85cb44e25828?w=1400&q=80');"></div>
    <div class="hero-bg"></div>
    <div class="hero-content">
      <h1>✈️ Your Journey, Documented</h1>
      <p>Plan trips, track expenses, map your route — all in one place.</p>
      <div class="hero-btns">
        <button class="btn-primary" onclick="showPage('dashboard')">Go to Dashboard</button>
        <button class="btn-outline" onclick="openNewTripModal()">+ Start a New Trip</button>
      </div>
    </div>
  </div>
  <div class="container">
    <div class="signature-strip">
      <div class="signature-card"><strong>Trip DNA</strong>Capture your journey with destination, style, companions, budget, and a personal story in one place.</div>
      <div class="signature-card"><strong>Smart Planning</strong>Use route maps, packing checklists, and itinerary builder to travel with confidence.</div>
      <div class="signature-card"><strong>Beautiful Memories</strong>Save photos, tickets, and trip highlights as a polished digital scrapbook.</div>
    </div>
    <div style="text-align:center;margin-bottom:24px;">
      <h2 class="section-title">Everything for your travel memories</h2>
      <p class="section-sub">From planning to post-trip reports</p>
    </div>
    <div class="grid-3">
      <div class="card" style="padding:24px;text-align:center;">
        <div style="font-size:36px;margin-bottom:10px;">🗺️</div>
        <h3 style="font-size:17px;margin-bottom:6px;">Route Maps</h3>
        <p style="font-size:13px;color:var(--ink-light);">Log waypoints, scenic viewpoints, and your travel path.</p>
      </div>
      <div class="card" style="padding:24px;text-align:center;">
        <div style="font-size:36px;margin-bottom:10px;">💰</div>
        <h3 style="font-size:17px;margin-bottom:6px;">Expense Tracker</h3>
        <p style="font-size:13px;color:var(--ink-light);">Track every rupee — hotels, food, transport, activities.</p>
      </div>
      <div class="card" style="padding:24px;text-align:center;">
        <div style="font-size:36px;margin-bottom:10px;">🏨</div>
        <h3 style="font-size:17px;margin-bottom:6px;">Hotels & Dining</h3>
        <p style="font-size:13px;color:var(--ink-light);">Save hotels, restaurants with ratings and notes.</p>
      </div>
      <div class="card" style="padding:24px;text-align:center;">
        <div style="font-size:36px;margin-bottom:10px;">📅</div>
        <h3 style="font-size:17px;margin-bottom:6px;">Itinerary Planner</h3>
        <p style="font-size:13px;color:var(--ink-light);">Plan your day-by-day schedule and activities.</p>
      </div>
      <div class="card" style="padding:24px;text-align:center;">
        <div style="font-size:36px;margin-bottom:10px;">📸</div>
        <h3 style="font-size:17px;margin-bottom:6px;">Travel Gallery</h3>
        <p style="font-size:13px;color:var(--ink-light);">Store memories with photos and journal notes.</p>
      </div>
      <div class="card" style="padding:24px;text-align:center;">
        <div style="font-size:36px;margin-bottom:10px;">📊</div>
        <h3 style="font-size:17px;margin-bottom:6px;">Trip Reports</h3>
        <p style="font-size:13px;color:var(--ink-light);">Full summary — distance, spend, places, highlights.</p>
      </div>
    </div>
    <div style="text-align:center;margin-top:40px;">
      <h2 class="section-title">Recent Trips</h2>
      <p class="section-sub">Your latest adventures</p>
      <div id="home-recent-trips" class="grid-3" style="margin-top:18px;text-align:left;"></div>
      <button class="btn-primary btn-lg" style="margin-top:20px;" onclick="showPage('trips')">View All Trips →</button>
    </div>
  </div>
</div>

<!-- ═══════ DASHBOARD ═══════ -->
<div class="page" id="page-dashboard">
  <div class="container">
    <h2 class="section-title" style="margin-bottom:4px;">Dashboard</h2>
    <p class="section-sub">Your travel overview at a glance</p>
    <div class="stat-cards" id="dashboard-stats"></div>
    <div class="grid-2">
      <div>
        <h3 style="font-size:17px;margin-bottom:12px;">📋 Recent Trips</h3>
        <div id="dashboard-recent"></div>
        <button class="btn-sm btn-sky" onclick="showPage('trips')" style="margin-top:8px;">See All Trips</button>
      </div>
      <div>
        <h3 style="font-size:17px;margin-bottom:12px;">💸 Spending by Category</h3>
        <div id="dashboard-spend"></div>
      </div>
    </div>
    <div style="margin-top:28px;">
      <h3 style="font-size:17px;margin-bottom:12px;">🏔️ Viewpoints & Highlights</h3>
      <div id="dashboard-viewpoints" class="grid-3"></div>
    </div>
  </div>
</div>

<!-- ═══════ NOTIFICATIONS ═══════ -->
<div class="page" id="page-notifications">
  <div class="container">
    <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:20px;gap:12px;flex-wrap:wrap;">
      <div>
        <h2 class="section-title">Notifications</h2>
        <p class="section-sub">Alerts and reminders for your trips</p>
      </div>
      <button class="btn-sm btn-ghost" onclick="clearAllNotifications()">🗑️ Clear All</button>
    </div>
    <div id="notif-list"></div>
  </div>
</div>

<!-- ═══════ MY TRIPS ═══════ -->
<div class="page" id="page-trips">
  <div class="container">
    <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:20px;gap:12px;flex-wrap:wrap;">
      <div>
        <h2 class="section-title">My Trips</h2>
        <p class="section-sub">All your travel adventures</p>
      </div>
      <button class="btn-primary btn-lg" onclick="openNewTripModal()">+ New Trip</button>
    </div>
    <div class="search-bar">
      <input type="text" id="trip-search" placeholder="🔍 Search trips by name or destination..." oninput="loadTrips()">
    </div>
    <div style="display:flex;gap:8px;margin-bottom:18px;flex-wrap:wrap;">
      <button class="btn-sm btn-ghost" onclick="filterTrips('all')">All</button>
      <button class="btn-sm btn-ghost" onclick="filterTrips('planned')">Planned</button>
      <button class="btn-sm btn-ghost" onclick="filterTrips('ongoing')">Ongoing</button>
      <button class="btn-sm btn-ghost" onclick="filterTrips('completed')">Completed</button>
    </div>
    <div id="trips-grid" class="grid-3"></div>
  </div>
</div>

<!-- ═══════ TRIP DETAIL ═══════ -->
<div class="page" id="page-trip-detail">
  <div class="container">
    <button class="btn-sm btn-ghost" onclick="showPage('trips')" style="margin-bottom:14px;">← Back to Trips</button>
    <div id="trip-detail-content"></div>
  </div>
</div>

<!-- ═══════ MAP ═══════ -->
<div class="page" id="page-map">
  <div class="container">
    <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:20px;flex-wrap:wrap;gap:10px;">
      <div>
        <h2 class="section-title">Route Map</h2>
        <p class="section-sub">Live tracking, routes and trip waypoints</p>
      </div>
      <div style="display:flex;gap:8px;flex-wrap:wrap;align-items:center;">
        <button class="btn-sm btn-ghost" onclick="locateMe()">📡 My Location</button>
        <button class="btn-sm btn-terra" id="live-track-btn" onclick="toggleLiveTracking()">🚦 Live Track</button>
        <button class="btn-sm btn-primary" id="route-plan-btn" onclick="toggleRouteMode()">🧭 Route Mode</button>
        <button class="btn-sm btn-sky" onclick="showMapSearch()">🔎 Search Place</button>
        <button class="btn-primary" onclick="openWaypointModal()">+ Add Waypoint</button>
      </div>
    </div>
   <div id="live-track-status" style="margin-bottom:16px;color:var(--ink-mid);font-size:13px;">Live GPS tracking is off.</div>
   <!-- From / To Route Planner -->
<div class="form-section" style="padding:16px;margin-bottom:16px;">
  <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;gap:10px;flex-wrap:wrap;">
    <h3 class="form-section-title" style="margin:0;">🧭 Plan Your Route</h3>
    <button class="btn-sm btn-ghost" onclick="swapRoutePlaces()">⇅ Swap</button>
  </div>

  <div style="display:grid;grid-template-columns:1fr 1fr auto;gap:10px;align-items:end;">
    <div class="form-group" style="margin:0;">
      <label>From</label>
      <input id="route-from" type="text" placeholder="Example: Chennai" autocomplete="off">
    </div>

    <div class="form-group" style="margin:0;">
      <label>To</label>
      <input id="route-to" type="text" placeholder="Example: Ooty" autocomplete="off">
    </div>

    <button class="btn-primary" onclick="getRouteFromInputs()" style="height:42px;">
      🗺️ Get Route
    </button>
  </div>

  <div id="route-summary" style="display:none;margin-top:14px;padding:12px;border-radius:10px;background:var(--sky-light);font-size:13px;color:var(--ink);"></div>
</div>
    <div style="display:flex;gap:16px;flex-wrap:wrap;align-items:flex-start;">
      <div style="flex:1;min-width:320px;">
        <div style="position:relative;">
          <div id="leaflet-map" style="height:420px;border-radius:var(--radius);"></div>
          <div style="position:absolute;top:10px;right:10px;z-index:400;display:flex;gap:4px;background:rgba(255,255,255,.9);padding:4px;border-radius:8px;box-shadow:0 2px 8px rgba(0,0,0,.15);">
            <button id="layer-btn-streets" class="map-layer-btn active" onclick="setMapLayer('streets')" style="border:1px solid var(--border);background:var(--card-bg);color:var(--ink);border-radius:6px;padding:5px 8px;font-size:12px;cursor:pointer;">🗺️ Streets</button>
            <button id="layer-btn-satellite" class="map-layer-btn" onclick="setMapLayer('satellite')" style="border:1px solid var(--border);background:var(--card-bg);color:var(--ink);border-radius:6px;padding:5px 8px;font-size:12px;cursor:pointer;">🛰️ Satellite</button>
            <button id="layer-btn-terrain" class="map-layer-btn" onclick="setMapLayer('terrain')" style="border:1px solid var(--border);background:var(--card-bg);color:var(--ink);border-radius:6px;padding:5px 8px;font-size:12px;cursor:pointer;">⛰️ Terrain</button>
          </div>
        </div>
        <div style="margin-top:12px;display:grid;grid-template-columns:1fr auto;gap:10px;align-items:center;">
          <select id="map-trip-select" onchange="loadMapWaypoints()" style="width:100%;padding:8px 12px;border:1.5px solid var(--border);border-radius:8px;font-family:inherit;font-size:13px;">
          </select>
          <button class="btn-sm btn-ghost" onclick="clearMapSelection()">Clear</button>
        </div>
      </div>
      <div style="flex:0.9;min-width:320px;display:grid;grid-template-rows:auto 1fr;gap:16px;">
        <div class="form-section" style="padding:16px;">
          <h3 class="form-section-title">Map Details</h3>
          <div id="map-details" style="font-size:13px;color:var(--ink-mid);line-height:1.6;">
            Select a trip to show its route and waypoints. Use the map buttons to switch layers and search for places.
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- ═══════ EXPENSES ═══════ -->
<div class="page" id="page-expenses">
  <div class="container">
    <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:14px;flex-wrap:wrap;gap:10px;">
      <div>
        <h2 class="section-title">Expenses</h2>
        <p class="section-sub">Track and manage your travel spending</p>
      </div>
      <button class="btn-primary" onclick="openExpenseModal()">+ Add Expense</button>
    </div>
    <div style="display:flex;gap:10px;margin-bottom:14px;flex-wrap:wrap;">
      <select id="expense-trip-filter" onchange="loadExpenses()" style="padding:7px 12px;border:1.5px solid var(--border);border-radius:8px;font-family:inherit;font-size:13px;"></select>
      <select id="expense-cat-filter" onchange="loadExpenses()" style="padding:7px 12px;border:1.5px solid var(--border);border-radius:8px;font-family:inherit;font-size:13px;">
        <option value="">All categories</option>
        <option value="food">Food</option>
        <option value="hotel">Hotel</option>
        <option value="transport">Transport</option>
        <option value="activity">Activity</option>
        <option value="shopping">Shopping</option>
        <option value="other">Other</option>
      </select>
    </div>
    <div class="stat-cards" id="expense-summary" style="grid-template-columns:repeat(auto-fill,minmax(140px,1fr));"></div>

    <div class="form-section">
      <h3 style="font-size:15px;margin-bottom:14px;">💱 Currency Converter</h3>
      <div class="form-row" style="align-items:flex-end;">
        <div class="form-group" style="margin-bottom:0;"><label>Amount (₹ INR)</label><input type="number" id="conv-amount" placeholder="1000" oninput="updateCurrencyConversion()"></div>
        <div class="form-group" style="margin-bottom:0;"><label>Live rates</label><button class="btn-sm btn-ghost" style="width:100%;" onclick="loadExchangeRates(true)">🔄 Refresh Rates</button></div>
      </div>
      <div id="currency-rates-status" style="font-size:12px;color:var(--ink-light);margin:10px 0;">Loading exchange rates…</div>
      <div id="currency-conversion-grid" style="display:grid;grid-template-columns:repeat(auto-fill,minmax(110px,1fr));gap:10px;"></div>
    </div>

    <div class="form-section">
      <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:14px;">
        <h3 style="font-size:15px;">Expense Log</h3>
        <button class="btn-sm btn-ghost" onclick="exportExpensesCSV()">⬇ Export CSV</button>
      </div>
      <div style="overflow-x:auto;">
        <table><thead><tr><th>Date</th><th>Description</th><th>Category</th><th>Trip</th><th>Amount</th><th></th></tr></thead>
        <tbody id="expense-table-body"></tbody></table>
      </div>
    </div>
    <div class="form-section">
      <h3 style="font-size:15px;margin-bottom:14px;">Budget vs Actual</h3>
      <div id="budget-bars"></div>
    </div>
  </div>
</div>

<!-- ═══════ PLACES ═══════ -->
<div class="page" id="page-places">
  <div class="container">
    <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:14px;flex-wrap:wrap;gap:10px;">
      <div>
        <h2 class="section-title">Hotels & Restaurants</h2>
        <p class="section-sub">Places you stayed and dined</p>
      </div>
      <button class="btn-primary" onclick="openPlaceModal()">+ Add Place</button>
    </div>
    <div class="tabs">
      <button class="tab active" onclick="switchPlaceTab('hotels',this)">🏨 Hotels</button>
      <button class="tab" onclick="switchPlaceTab('restaurants',this)">🍽️ Restaurants</button>
      <button class="tab" onclick="switchPlaceTab('transport',this)">🚌 Transport</button>
      <button class="tab" onclick="switchPlaceTab('route',this)">🧭 Route</button>
    </div>
    <div id="places-content"></div>
  </div>
</div>

<!-- ═══════ ITINERARY BUILDER ═══════ -->
<div class="page" id="page-itinerary">
  <div class="container">
    <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:18px;flex-wrap:wrap;gap:10px;">
      <div><h2 class="section-title">📅 Itinerary Builder</h2><p class="section-sub">Plan your day-by-day schedule and activities</p></div>
      <select id="itin-trip-select" onchange="selectItineraryTrip(this.value)" style="padding:9px 14px;border:1.5px solid var(--border);border-radius:8px;font-family:inherit;font-size:14px;background:#fff;"></select>
    </div>
    <div id="itinerary-content"></div>
  </div>
</div>
<!-- ═══════ SMART PACKING ASSISTANT ═══════ -->
<div class="page" id="page-packing">
  <div class="container">

    <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:18px;flex-wrap:wrap;gap:10px;">
      <div>
        <h2 class="section-title">🎒 Smart Packing Assistant</h2>
        <p class="section-sub">Get a travel checklist based on your trip type and destination.</p>
      </div>
    </div>

    <div class="form-section">
      <div class="form-row">
        <div class="form-group">
          <label>Select Trip</label>
          <select id="packing-trip-select" onchange="loadPackingList()"></select>
        </div>

        <div class="form-group">
          <label>Trip Type</label>
          <select id="packing-trip-type">
            <option value="general">General Travel</option>
            <option value="hill">Hill Station</option>
            <option value="beach">Beach Trip</option>
            <option value="adventure">Adventure Trip</option>
            <option value="business">Business Trip</option>
          </select>
        </div>
      </div>

      <button class="btn-sm btn-sky" onclick="generatePackingList()">
        ✨ Generate Smart Checklist
      </button>
    </div>

    <div class="form-section">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:14px;flex-wrap:wrap;gap:10px;">
        <h3 style="font-size:16px;">My Packing Checklist</h3>
        <span id="packing-progress-text" style="font-size:13px;color:var(--ink-light);">0 / 0 packed</span>
      </div>

      <div class="progress-bar" style="margin-bottom:16px;">
        <div class="progress-fill" id="packing-progress-bar" style="width:0%;"></div>
      </div>

      <div id="packing-list-content"></div>

      <div style="display:flex;gap:8px;margin-top:18px;flex-wrap:wrap;">
        <input type="text" id="custom-packing-item" placeholder="Add custom item..." style="flex:1;min-width:200px;padding:9px 12px;border:1.5px solid var(--border);border-radius:8px;font-family:inherit;">
        <button class="btn-sm btn-primary" onclick="addCustomPackingItem()">+ Add Item</button>
      </div>
    </div>

  </div>
</div>

<!-- ═══════ GALLERY ═══════ -->
<div class="page" id="page-gallery">
  <div class="container">
    <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:20px;flex-wrap:wrap;gap:10px;">
      <div>
        <h2 class="section-title">Travel Gallery</h2>
        <p class="section-sub">Memories from your journeys</p>
      </div>
      <button class="btn-primary" onclick="openMemoryModal()">+ Add Memory</button>
    </div>
    <div id="gallery-grid" class="gallery-grid"></div>
  </div>
</div>

<!-- ═══════ PROFILE ═══════ -->
<div class="page" id="page-profile">
  <div class="container">
    <div class="profile-hero">
      <div class="profile-avatar-big" id="profile-avatar-big">?</div>
      <div>
        <h2 id="profile-name-display" style="font-size:26px;margin-bottom:3px;">Traveller</h2>
        <p id="profile-email-display" style="opacity:.75;font-size:13px;">Edit your profile below</p>
        <div id="profile-tags" style="margin-top:8px;"></div>
      </div>
    </div>
    <div class="grid-2">
      <div class="form-section">
        <h3 class="form-section-title">Personal Info</h3>
        <div class="form-group"><label>Full Name</label><input type="text" id="profile-name" placeholder="Your name"></div>
        <div class="form-group"><label>Email</label><input type="email" id="profile-email" placeholder="you@email.com"></div>
        <div class="form-group"><label>Home City</label><input type="text" id="profile-city" placeholder="Chennai, India"></div>
        <div class="form-group"><label>Travel Style</label>
          <select id="profile-style">
            <option value="">Select style</option>
            <option>Budget backpacker</option><option>Mid-range explorer</option>
            <option>Luxury traveller</option><option>Adventure seeker</option>
            <option>Cultural explorer</option><option>Family vacationer</option>
          </select>
        </div>
        <button class="btn-sky btn-lg btn-sm" onclick="saveProfile()" style="width:100%;padding:11px;">Save Profile</button>
      </div>
      <div class="form-section">
        <h3 class="form-section-title">Travel Stats</h3>
        <div id="profile-stats"></div>
        <div style="margin-top:18px;">
          <h4 style="font-size:14px;margin-bottom:10px;">Preferred Currency</h4>
          <div class="form-group">
            <select id="profile-currency">
              <option value="₹">₹ Indian Rupee</option>
              <option value="$">$ US Dollar</option>
              <option value="€">€ Euro</option>
              <option value="£">£ British Pound</option>
            </select>
          </div>
          <button class="btn-ghost btn-sm" onclick="saveProfile()" style="width:100%;padding:9px;">Save Preferences</button>
        </div>
        <div style="margin-top:18px;">
          <button class="btn-sm btn-danger" onclick="clearAllData()" style="width:100%;padding:9px;">🗑️ Clear All Data</button>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- ═══════ AUTH ═══════ -->
<div class="page" id="page-auth">
  <div class="auth-container">
    <div class="auth-card">
      <div class="auth-tabs">
        <button class="auth-tab active" id="tab-login" onclick="switchAuthTab('login')">Login</button>
        <button class="auth-tab" id="tab-signup" onclick="switchAuthTab('signup')">Sign Up</button>
      </div>
      <div class="auth-panel active" id="auth-login-panel">
        <h2>Welcome Back</h2>
        <p class="sub">Login to continue to TripTrace</p>
        <div class="form-group"><label>Email</label><input type="email" id="auth-email-login" placeholder="you@email.com"></div>
        <div class="form-group"><label>Password</label><input type="password" id="auth-password-login" placeholder="••••••••"></div>
        <button class="btn-primary" style="width:100%;padding:12px;border-radius:8px;font-size:14px;" onclick="doLogin()">Login</button>
        <p style="text-align:center;font-size:12px;color:var(--ink-light);margin-top:14px;">Your account and trips sync securely to the cloud.</p>
      </div>
      <div class="auth-panel" id="auth-signup-panel">
        <h2>Create Account</h2>
        <p class="sub">Sign up to start your travel journal</p>
        <div class="form-group"><label>Your Name</label><input type="text" id="auth-name-signup" placeholder="Your name"></div>
        <div class="form-group"><label>Email</label><input type="email" id="auth-email-signup" placeholder="you@email.com"></div>
        <div class="form-group"><label>Password</label><input type="password" id="auth-password-signup" placeholder="•••••••• (min. 6 characters)"></div>
        <button class="btn-primary" style="width:100%;padding:12px;border-radius:8px;font-size:14px;" onclick="doSignup()">Sign Up</button>
        <p style="text-align:center;font-size:12px;color:var(--ink-light);margin-top:14px;">Your journal syncs to your account, so it's there on every device.</p>
      </div>
    </div>
  </div>
</div>

<!-- ═══════ TRAVEL DOCUMENTS ═══════ -->
<div class="page" id="page-documents">
  <div class="container">
    <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:20px;flex-wrap:wrap;gap:10px;">
      <div>
        <h2 class="section-title">📄 Travel Documents</h2>
        <p class="section-sub">Secure storage for tickets, passport, insurance & bookings</p>
      </div>
      <button class="btn-primary" onclick="openModal('modal-upload-doc')">+ Upload Document</button>
    </div>
    <div class="stat-cards" id="doc-stats" style="margin-bottom:20px;"></div>
    <div style="display:grid;grid-template-columns:repeat(auto-fill,minmax(150px,1fr));gap:14px;" id="documents-grid"></div>
  </div>
</div>

<!-- ═══════ AI PLANNER ═══════ -->
<!-- ═══════ SOS ═══════ -->
<div class="page" id="page-sos">
  <div class="container">
    <h2 class="section-title" style="margin-bottom:4px;">SOS Safety Center</h2>
    <p class="section-sub">Emergency tools for solo and women travellers</p>
    <div class="grid-2" style="gap:20px;">
      <div>
        <button class="sos-big-btn" onclick="activateSOS()">🆘 SOS — SEND ALERT</button>
        <div style="text-align:center;margin-top:6px;font-size:12px;color:var(--ink-light);">Shares your live GPS location with emergency contacts</div>
        <div class="sos-info-card" style="margin-top:16px;">
          <h3 style="font-size:15px;margin-bottom:10px;">My Current Location</h3>
          <div id="sos-location-display" style="font-size:13px;color:var(--ink-mid);padding:10px;background:var(--sand);border-radius:8px;">Click below to fetch GPS.</div>
          <div style="display:flex;gap:8px;margin-top:10px;">
            <button class="btn-sm btn-sky" style="flex:1;" onclick="getSOSLocation()">📡 Get My Location</button>
            <button class="btn-sm btn-ghost" style="flex:1;" onclick="showSOSMapForManualPin()" title="If GPS is inaccurate on this device, open the map and tap your real position">📍 Set on map</button>
          </div>
          <div id="sos-map" style="height:180px;border-radius:8px;margin-top:10px;display:none;"></div>
        </div>
        <div class="sos-info-card">
          <h3 style="font-size:15px;margin-bottom:10px;">Emergency Contacts</h3>
          <div id="sos-contacts-list"></div>
          <div style="display:flex;gap:6px;margin-top:10px;flex-wrap:wrap;">
            <input type="text" id="sos-contact-name" placeholder="Name" style="flex:1;min-width:80px;padding:7px 11px;border:1.5px solid var(--border);border-radius:8px;font-family:inherit;font-size:13px;">
            <input type="tel" id="sos-contact-phone" placeholder="Phone" style="flex:1;min-width:110px;padding:7px 11px;border:1.5px solid var(--border);border-radius:8px;font-family:inherit;font-size:13px;">
            <button class="btn-sm btn-success" onclick="addSOSContact()">Add</button>
          </div>
        </div>
      </div>
      <div>
        <div class="sos-info-card">
          <h3 style="font-size:15px;margin-bottom:10px;">Emergency Numbers — India</h3>
          <div style="display:grid;gap:8px;">
            <div style="display:flex;align-items:center;justify-content:space-between;padding:10px;background:var(--sand);border-radius:8px;"><div style="font-weight:600;font-size:14px;">Police</div><a href="tel:100" style="background:var(--danger);color:#fff;padding:7px 14px;border-radius:6px;font-weight:700;text-decoration:none;font-size:15px;">📞 100</a></div>
            <div style="display:flex;align-items:center;justify-content:space-between;padding:10px;background:var(--sand);border-radius:8px;"><div style="font-weight:600;font-size:14px;">Ambulance</div><a href="tel:108" style="background:var(--danger);color:#fff;padding:7px 14px;border-radius:6px;font-weight:700;text-decoration:none;font-size:15px;">📞 108</a></div>
            <div style="display:flex;align-items:center;justify-content:space-between;padding:10px;background:var(--sand);border-radius:8px;"><div style="font-weight:600;font-size:14px;">Fire</div><a href="tel:101" style="background:var(--danger);color:#fff;padding:7px 14px;border-radius:6px;font-weight:700;text-decoration:none;font-size:15px;">📞 101</a></div>
            <div style="display:flex;align-items:center;justify-content:space-between;padding:10px;background:var(--sand);border-radius:8px;"><div style="font-weight:600;font-size:14px;">Women Helpline</div><a href="tel:1091" style="background:#9b59b6;color:#fff;padding:7px 14px;border-radius:6px;font-weight:700;text-decoration:none;font-size:15px;">📞 1091</a></div>
            <div style="display:flex;align-items:center;justify-content:space-between;padding:10px;background:var(--sand);border-radius:8px;"><div style="font-weight:600;font-size:14px;">National Emergency</div><a href="tel:112" style="background:var(--danger);color:#fff;padding:7px 14px;border-radius:6px;font-weight:700;text-decoration:none;font-size:15px;">📞 112</a></div>
          </div>
        </div>
        <div class="sos-info-card">
          <h3 style="font-size:15px;margin-bottom:10px;">Safety Tips</h3>
          <ul style="font-size:13px;color:var(--ink-mid);line-height:2.2;padding-left:16px;">
            <li>Share live location before travelling</li>
            <li>Save hotel address and local police offline</li>
            <li>Keep emergency contacts on speed dial</li>
            <li>Inform someone of your daily itinerary</li>
            <li>Carry ID and cash separately</li>
            <li>Use registered cabs only</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</div>

<!-- ═══════ ANALYTICS ═══════ -->
<div class="page" id="page-analytics">
  <div class="container">
    <h2 class="section-title" style="margin-bottom:4px;">Analytics</h2>
    <p class="section-sub">Travel stats, spending trends and trip insights</p>
    <div class="stat-cards" id="analytics-stats"></div>
    <div class="grid-2" style="gap:20px;margin-bottom:20px;">
      <div class="form-section"><h3 class="form-section-title">Spending by Category</h3><div id="analytics-cat-chart"></div></div>
      <div class="form-section"><h3 class="form-section-title">Budget Comparison</h3><div id="analytics-budget-chart"></div></div>
    </div>
    <div class="grid-2" style="gap:20px;margin-bottom:20px;">
      <div class="form-section">
        <h3 class="form-section-title">Smart Budget Prediction</h3>
        <div id="analytics-budget-prediction"></div>
        <h3 class="form-section-title" style="margin-top:18px;">Monthly Spending</h3>
        <div id="analytics-monthly"></div>
        <h3 style="font-size:15px;margin:18px 0 10px;">Top Destinations</h3>
        <div id="analytics-destinations"></div>
      </div>
      <div class="form-section">
        <h3 class="form-section-title">Travel Insights</h3>
        <div id="analytics-sustainability"></div>
      </div>
    </div>
  </div>
</div>

<!-- ═══════ MODALS ═══════ -->

<!-- SOS Active -->
<div class="modal-overlay" id="modal-sos-active">
  <div class="modal" style="max-width:400px;">
    <div class="modal-header" style="background:#fdf0f0;"><h3 style="color:var(--danger);">🆘 SOS Alert Activated</h3><button class="modal-close" onclick="closeModal('modal-sos-active')">×</button></div>
    <div class="modal-body" style="text-align:center;">
      <div style="font-size:60px;margin-bottom:14px;">🆘</div>
      <h3 style="margin-bottom:6px;font-size:20px;">Alert Ready</h3>
      <p style="color:var(--ink-mid);margin-bottom:16px;" id="sos-alert-text">Share your location with emergency contacts.</p>
      <div class="alert alert-danger" id="sos-location-text">Fetching location...</div>
      <div style="margin-top:14px;">
        <a href="tel:112" style="display:block;text-align:center;padding:13px;border-radius:8px;text-decoration:none;font-size:17px;font-weight:700;background:var(--danger);color:#fff;margin-bottom:8px;">📞 Call 112 — National Emergency</a>
        <a href="tel:100" class="btn-sm btn-danger" style="display:inline-block;margin:3px;text-decoration:none;">Police 100</a>
        <a href="tel:108" class="btn-sm btn-danger" style="display:inline-block;margin:3px;text-decoration:none;">Ambulance 108</a>
        <a href="tel:1091" style="display:inline-block;margin:3px;background:#9b59b6;color:#fff;border-radius:6px;padding:6px 13px;font-weight:600;text-decoration:none;font-size:13px;">Women 1091</a>
      </div>
    </div>
    <div class="modal-footer"><button class="btn-sm btn-ghost" onclick="closeModal('modal-sos-active')">I am Safe Now</button></div>
  </div>
</div>

<!-- New Trip -->
<div class="modal-overlay" id="modal-trip">
  <div class="modal">
    <div class="modal-header"><h3>✈️ New Trip</h3><button class="modal-close" onclick="closeModal('modal-trip')">×</button></div>
    <div class="modal-body">
      <div class="form-group"><label>Trip Name *</label><input type="text" id="trip-name-input" placeholder="e.g. Ooty Hill Station Adventure"></div>
      <div class="form-row">
        <div class="form-group"><label>Start Date</label><input type="date" id="trip-start"></div>
        <div class="form-group"><label>End Date</label><input type="date" id="trip-end"></div>
      </div>
      <div class="form-group"><label>Destination(s)</label><input type="text" id="trip-dest" placeholder="e.g. Ooty, Kodaikanal, Munnar"></div>
      <div class="form-row">
        <div class="form-group"><label>Total Budget (₹)</label><input type="number" id="trip-budget" placeholder="e.g. 15000"></div>
        <div class="form-group"><label>Travel Style</label><select id="trip-style"><option value="">Select style</option><option>Budget backpacker</option><option>Mid-range explorer</option><option>Adventure seeker</option><option>Luxury traveller</option><option>Cultural explorer</option><option>Family vacationer</option></select></div>
      </div>
      <div class="form-row">
        <div class="form-group"><label>Companions</label><input type="text" id="trip-companions" placeholder="Friends / Family / Solo"></div>
        <div class="form-group"><label>Status</label><select id="trip-status"><option value="planned">Planned</option><option value="ongoing">Ongoing</option><option value="completed">Completed</option></select></div>
      </div>
      <div class="form-group"><label>Trip Emoji / Theme</label>
        <select id="trip-emoji">
          <option value="🏔️">🏔️ Mountains</option><option value="🏖️">🏖️ Beach</option>
          <option value="🏙️">🏙️ City</option><option value="🌿">🌿 Nature</option>
          <option value="🛕">🛕 Heritage</option><option value="🎠">🎠 Family Fun</option>
          <option value="🚂">🚂 Road Trip</option><option value="✈️">✈️ International</option>
        </select>
      </div>
      <div class="form-group"><label>Notes</label><textarea id="trip-notes" rows="3" placeholder="What's this trip about?"></textarea></div>
    </div>
    <div class="modal-footer">
      <button class="btn-sm btn-ghost" onclick="closeModal('modal-trip')">Cancel</button>
      <button class="btn-sm btn-sky" onclick="saveTrip()">Save Trip</button>
    </div>
  </div>
</div>

<!-- Add Expense -->
<div class="modal-overlay" id="modal-expense">
  <div class="modal">
    <div class="modal-header"><h3>💰 Add Expense</h3><button class="modal-close" onclick="closeModal('modal-expense')">×</button></div>
    <div class="modal-body">
      <div class="form-group"><label>Trip</label><select id="exp-trip"></select></div>
      <div class="form-group"><label>Description *</label><input type="text" id="exp-desc" placeholder="e.g. Hotel stay at The Nilgiris"></div>
      <div class="form-row">
        <div class="form-group"><label>Amount (₹) *</label><input type="number" id="exp-amount" placeholder="0"></div>
        <div class="form-group"><label>Date</label><input type="date" id="exp-date"></div>
      </div>
      <div class="form-group"><label>Category</label>
        <select id="exp-cat">
          <option value="food">🍔 Food & Dining</option>
          <option value="hotel">🏨 Hotel / Stay</option>
          <option value="transport">🚌 Transport</option>
          <option value="activity">🎯 Activity / Entry</option>
          <option value="shopping">🛍️ Shopping</option>
          <option value="other">📌 Other</option>
        </select>
      </div>
      <div class="form-group"><label>Receipt</label>
        <div class="receipt-upload" onclick="document.getElementById('receipt-input').click()">📄 Upload receipt image</div>
        <input type="file" id="receipt-input" accept="image/*" onchange="handleReceiptUpload(event)">
        <div id="receipt-preview" class="receipt-preview"></div>
        <button class="btn-sm btn-outline" style="width:100%;margin-top:10px;" onclick="scanReceipt()" type="button">🔍 Scan Receipt</button>
        <div id="receipt-scan-status" style="font-size:12px;color:var(--ink-light);margin-top:8px;">Upload a receipt image to scan.</div>
      </div>
      <div class="form-group"><label>Notes</label><input type="text" id="exp-notes" placeholder="Any additional details"></div>
    </div>
    <div class="modal-footer">
      <button class="btn-sm btn-ghost" onclick="closeModal('modal-expense')">Cancel</button>
      <button class="btn-sm btn-sky" onclick="saveExpense()">Add Expense</button>
    </div>
  </div>
</div>
<!-- Automatic Travel Timeline Modal -->
<div class="modal-overlay" id="modal-trip-timeline">
  <div class="modal">
    <div class="modal-header">
      <h3 id="timeline-trip-title">Automatic Travel Timeline</h3>
      <button class="modal-close" onclick="closeModal('modal-trip-timeline')">×</button>
    </div>

    <div class="modal-body">
      <div class="alert alert-info">
        This timeline is automatically created from your trip dates, expenses, places, and waypoints.
      </div>

      <div id="timeline-trip-content"></div>
    </div>

    <div class="modal-footer">
      <button class="btn-sm btn-ghost" onclick="closeModal('modal-trip-timeline')">Close</button>
    </div>
  </div>
</div>

<!-- Add Waypoint (with lat/lng) -->
<div class="modal-overlay" id="modal-waypoint">
  <div class="modal">
    <div class="modal-header"><h3>📍 Add Waypoint / Viewpoint</h3><button class="modal-close" onclick="closeModal('modal-waypoint')">×</button></div>
    <div class="modal-body">
      <div class="form-group"><label>Trip</label><select id="wp-trip"></select></div>
      <div class="form-group"><label>Place Name *</label><input type="text" id="wp-name" placeholder="e.g. Doddabetta Peak"></div>
      <div class="form-group"><label>Type</label>
        <select id="wp-type">
          <option value="waypoint">📍 Waypoint</option>
          <option value="viewpoint">🏔️ Viewpoint</option>
          <option value="start">🟢 Starting Point</option>
          <option value="end">🔴 Destination</option>
        </select>
      </div>
      <div class="form-row">
        <div class="form-group"><label>Latitude</label><input type="number" id="wp-lat" placeholder="e.g. 11.4075" step="any"></div>
        <div class="form-group"><label>Longitude</label><input type="number" id="wp-lng" placeholder="e.g. 76.6931" step="any"></div>
      </div>
      <button class="btn-sm btn-ghost" onclick="fillMyLocation()" style="margin-bottom:14px;width:100%;">📡 Use My Current GPS Location</button>
      <div class="form-row">
        <div class="form-group"><label>Distance from start (km)</label><input type="number" id="wp-dist" placeholder="e.g. 120"></div>
        <div class="form-group"><label>Altitude (m)</label><input type="number" id="wp-alt" placeholder="e.g. 2637"></div>
      </div>
      <div class="form-group"><label>Notes</label><textarea id="wp-notes" rows="2" placeholder="What's special here?"></textarea></div>
    </div>
    <div class="modal-footer">
      <button class="btn-sm btn-ghost" onclick="closeModal('modal-waypoint')">Cancel</button>
      <button class="btn-sm btn-sky" onclick="saveWaypoint()">Save</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="modal-search">
  <div class="modal">
    <div class="modal-header"><h3>🔎 Search Map Place</h3><button class="modal-close" onclick="closeModal('modal-search')">×</button></div>
    <div class="modal-body">
      <div class="form-group"><label>Search query</label><input type="text" id="map-search-query" placeholder="e.g. Beach resort, museum, train station"></div>
      <button class="btn-sm btn-primary" onclick="searchMapPlace()">Search</button>
      <div id="map-search-results" style="margin-top:16px;font-size:13px;color:var(--ink-mid);">Search for places by name, address or landmark.</div>
    </div>
    <div class="modal-footer">
      <button class="btn-sm btn-ghost" onclick="closeModal('modal-search')">Close</button>
    </div>
  </div>
</div>

<!-- Add Place -->
<div class="modal-overlay" id="modal-place">
  <div class="modal">
    <div class="modal-header"><h3 id="place-modal-title">🏨 Add Place</h3><button class="modal-close" onclick="closeModal('modal-place')">×</button></div>
    <div class="modal-body">
      <div class="form-group"><label>Trip</label><select id="place-trip"></select></div>
      <div class="form-group"><label>Type</label>
        <select id="place-type" onchange="updatePlaceModal()">
          <option value="hotel">🏨 Hotel / Stay</option>
          <option value="restaurant">🍽️ Restaurant</option>
          <option value="transport">🚌 Transport</option>
        </select>
      </div>
      <div class="form-group"><label>Name *</label><input type="text" id="place-name" placeholder="Name of the place"></div>
      <div class="form-group"><label>Location / Address</label><input type="text" id="place-location" placeholder="City or address"></div>
      <div class="form-row">
        <div class="form-group"><label>Rating (1–5)</label>
          <select id="place-rating"><option>5 ⭐⭐⭐⭐⭐</option><option>4 ⭐⭐⭐⭐</option><option>3 ⭐⭐⭐</option><option>2 ⭐⭐</option><option>1 ⭐</option></select>
        </div>
        <div class="form-group"><label>Cost (₹)</label><input type="number" id="place-cost" placeholder="e.g. 2500"></div>
      </div>
      <div class="form-group">
        <label id="place-extra-label">Date</label>
        <input type="date" id="place-date">
      </div>
      <div class="form-group"><label>Notes / Review</label><textarea id="place-notes" rows="2" placeholder="Your thoughts..."></textarea></div>
    </div>
    <div class="modal-footer">
      <button class="btn-sm btn-ghost" onclick="closeModal('modal-place')">Cancel</button>
      <button class="btn-sm btn-sky" onclick="savePlace()">Save Place</button>
    </div>
  </div>
</div>

<!-- Add Memory (with real photo upload) -->
<div class="modal-overlay" id="modal-memory">
  <div class="modal">
    <div class="modal-header"><h3>📸 Add Memory</h3><button class="modal-close" onclick="closeModal('modal-memory')">×</button></div>
    <div class="modal-body">
      <div class="form-group"><label>Trip</label><select id="mem-trip"></select></div>
      <div class="form-group"><label>Title *</label><input type="text" id="mem-title" placeholder="e.g. Sunset at Doddabetta"></div>
      <div class="form-group">
        <label>Upload Photo</label>
        <div class="photo-upload-area" onclick="document.getElementById('mem-photo-input').click()">
          <div id="photo-upload-hint">📷 Tap to upload a photo</div>
          <div class="photo-preview" id="photo-preview-container"></div>
          <input type="file" id="mem-photo-input" accept="image/*" onchange="handlePhotoUpload(event)">
        </div>
      </div>
      <div class="form-group"><label>Emoji (if no photo)</label>
        <select id="mem-emoji">
          <option>🌄</option><option>🏔️</option><option>🌊</option><option>🌿</option>
          <option>🍛</option><option>🛕</option><option>🌉</option><option>🎠</option>
          <option>🌸</option><option>🐘</option><option>🦚</option><option>🌺</option>
          <option>🏖️</option><option>🌙</option><option>☀️</option>
        </select>
      </div>
      <div class="form-group"><label>Journal Note</label><textarea id="mem-note" rows="3" placeholder="Write about this moment..."></textarea></div>
      <div class="form-group"><label>Date</label><input type="date" id="mem-date"></div>
    </div>
    <div class="modal-footer">
      <button class="btn-sm btn-ghost" onclick="closeModal('modal-memory')">Cancel</button>
      <button class="btn-sm btn-sky" onclick="saveMemory()">Save Memory</button>
    </div>
  </div>
</div>

<!-- View Memory -->
<div class="modal-overlay" id="modal-view-memory">
  <div class="modal" style="max-width:480px;">
    <div class="modal-header"><h3 id="view-mem-title">Memory</h3><button class="modal-close" onclick="closeModal('modal-view-memory')">×</button></div>
    <div class="modal-body" id="view-mem-body"></div>
    <div class="modal-footer"><button class="btn-sm btn-ghost" onclick="closeModal('modal-view-memory')">Close</button></div>
  </div>
</div>

<!-- Upload Travel Document Modal -->
<div class="modal-overlay" id="modal-upload-doc">
  <div class="modal">
    <div class="modal-header"><h3>📄 Upload Document</h3><button class="modal-close" onclick="closeModal('modal-upload-doc')">×</button></div>
    <div class="modal-body">
      <div class="form-group"><label>Trip *</label><select id="doc-trip" required></select></div>
      <div class="form-group"><label>Document Type *</label>
        <select id="doc-type" required>
          <option value="passport">🛂 Passport</option>
          <option value="ticket">🎫 Flight/Train Ticket</option>
          <option value="hotel">🏨 Hotel Booking</option>
          <option value="insurance">🛡️ Travel Insurance</option>
          <option value="visa">📋 Visa/Permit</option>
          <option value="receipt">🧾 Receipt</option>
          <option value="itinerary">📍 Itinerary</option>
          <option value="other">📎 Other</option>
        </select>
      </div>
      <div class="form-group"><label>Document Name/Reference *</label><input type="text" id="doc-name" placeholder="e.g. Passport John Doe" required></div>
      <div class="form-group">
        <label>Upload Image/PDF (Photo of document)</label>
        <div class="photo-upload-area" onclick="document.getElementById('doc-image-input').click()" style="cursor:pointer;">
          <div id="doc-upload-hint">📷 Tap to upload document photo</div>
          <div id="doc-preview-container" style="margin-top:10px;"></div>
          <input type="file" id="doc-image-input" accept="image/*,.pdf" onchange="handleDocumentUpload(event)">
        </div>
      </div>
      <div class="form-group"><label>Notes</label><textarea id="doc-notes" rows="2" placeholder="Valid until date, issuing country, etc."></textarea></div>
      <div class="form-group"><label>Valid Until (optional)</label><input type="date" id="doc-expiry"></div>
    </div>
    <div class="modal-footer">
      <button class="btn-sm btn-ghost" onclick="closeModal('modal-upload-doc')">Cancel</button>
      <button class="btn-sm btn-primary" onclick="saveDocument()">Save Document</button>
    </div>
  </div>
</div>

<!-- View Document Modal -->
<div class="modal-overlay" id="modal-view-doc">
  <div class="modal" style="max-width:600px;">
    <div class="modal-header"><h3 id="doc-view-title">Document</h3><button class="modal-close" onclick="closeModal('modal-view-doc')">×</button></div>
    <div class="modal-body" id="doc-view-body" style="max-height:500px;overflow-y:auto;"></div>
    <div class="modal-footer">
      <button class="btn-sm btn-danger" onclick="deleteDocument(currentViewDocId)">Delete</button>
      <button class="btn-sm btn-ghost" onclick="closeModal('modal-view-doc')">Close</button>
    </div>
  </div>
</div>

<!-- Start Auto Trip Modal -->
<div class="modal-overlay" id="modal-start-trip">
  <div class="modal">
    <div class="modal-header"><h3>Start Trip Tracking</h3><button class="modal-close" onclick="closeModal('modal-start-trip')">×</button></div>
    <div class="modal-body">
      <div class="form-group"><label>Trip Name</label><input type="text" id="trip-track-name" placeholder="My Road Trip"></div>
      <div class="form-group"><label>Destination</label><input type="text" id="trip-track-dest" placeholder="e.g., Ooty"></div>
      <div class="form-group"><label>Estimated Budget (₹)</label><input type="number" id="trip-track-budget" placeholder="5000" value="5000"></div>
      <div class="form-group"><label>Travel Style</label><select id="trip-track-style"><option>Budget backpacker</option><option>Mid-range explorer</option><option>Adventure seeker</option><option>Luxury traveller</option></select></div>
    </div>
    <div class="modal-footer">
      <button class="btn-sm btn-ghost" onclick="closeModal('modal-start-trip')">Cancel</button>
      <button class="btn-sm btn-primary" onclick="startAutoTrip()">🚀 Start Tracking</button>
    </div>
  </div>
</div>

<!-- Trip Summary Modal -->
<div class="modal-overlay" id="modal-trip-summary">
  <div class="modal" style="max-width:520px;">
    <div class="modal-header"><h3>🏁 Trip Summary</h3><button class="modal-close" onclick="closeModal('modal-trip-summary')">×</button></div>
    <div class="modal-body" id="trip-summary-body" style="max-height:400px;overflow-y:auto;"></div>
    <div class="modal-footer">
      <button class="btn-sm btn-ghost" onclick="closeModal('modal-trip-summary')">Close</button>
      <button class="btn-sm btn-primary" onclick="saveSummaryAndContinue()">Save & Continue</button>
    </div>
  </div>
</div>

<!-- Packing Checklist Modal -->
<div class="modal-overlay" id="modal-packing">
  <div class="modal" style="max-width:600px;">
    <div class="modal-header">
      <h3>🎒 Packing Checklist</h3>
      <button class="modal-close" onclick="closeModal('modal-packing')">×</button>
    </div>
    <div class="modal-body" style="max-height:500px;overflow-y:auto;">
      <div id="packing-loading" style="text-align:center;padding:40px;color:var(--ink-light);">Generating smart checklist...</div>
      <div id="packing-checklist" style="display:none;">
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:20px;" id="packing-stats"></div>
        <div style="margin-bottom:16px;">
          <label style="display:flex;align-items:center;gap:8px;font-size:13px;margin-bottom:8px;">
            <input type="checkbox" id="packing-filter-essential" onchange="renderPackingChecklist()" checked>
            <span>Show only essential items</span>
          </label>
        </div>
        <div id="packing-items"></div>
      </div>
    </div>
    <div class="modal-footer">
      <button class="btn-sm btn-ghost" onclick="closeModal('modal-packing')">Close</button>
      <button class="btn-sm btn-primary" onclick="downloadPackingList()">⬇️ Download List</button>
    </div>
  </div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/tesseract.js@4.1.1/dist/tesseract.min.js"></script>
<script>
// ═══════════════════════════════════════════════════════════
// CONFIG — Replace OWM_API_KEY with your own free key from:
// https://openweathermap.org/api (free tier, no credit card)
// The key below is a demo key — get your own to avoid rate limits
// ═══════════════════════════════════════════════════════════

// ═══════ DATA STORE ═══════
// ═══════ DATA STORE ═══════
let currentUser = null;
let currentTripId = null;

const SHARED_KEYS = ['accounts', 'user'];

function uid(){
  if(window.crypto && crypto.randomUUID) return crypto.randomUUID();
  return 'id_' + Date.now().toString(36) + '_' + Math.random().toString(36).slice(2, 10);
}

function today(){
  return new Date().toISOString().slice(0, 10);
}

function fmt(dateStr){
  if(!dateStr) return '—';
  const d = new Date(dateStr + 'T00:00:00');
  if(isNaN(d.getTime())) return dateStr;
  return d.toLocaleDateString('en-IN', {day:'numeric', month:'short', year:'numeric'});
}

function fmtNum(amount){
  const symbol = (typeof currentUser !== 'undefined' && currentUser && currentUser.currency) || '₹';
  const num = Number(amount) || 0;
  return symbol + num.toLocaleString('en-IN');
}

function userStorageKey(key){
  if(SHARED_KEYS.includes(key)){
    return 'tt_' + key;
  }

  const userId = currentUser && currentUser.id ? currentUser.id : 'guest';
  return 'tt_' + userId + '_' + key;
}

// Cloud sync: once signed in, cloudUID holds the Firebase Auth uid and every
// DB.set() write is mirrored (debounced) up to that user's Firestore document,
// so the same account sees the same data on any browser/device.
let cloudUID = null;
let cloudSaveQueue = {};
let cloudSaveTimer = null;
let cloudProfileQueue = null;
let cloudProfileTimer = null;

function setSyncStatus(status){
  const el = document.getElementById('sync-status');
  const txt = document.getElementById('sync-status-text');
  if(!el||!txt) return;
  el.classList.remove('saving','saved','offline','error');
  el.classList.add(status);
  const labels = { saving:'Syncing…', saved:'Saved', offline:'Offline', error:'Sync error' };
  txt.textContent = labels[status] || 'Saved';
}

function queueCloudSave(key, value){
  if(!cloudUID) return;
  cloudSaveQueue[key] = value;
  clearTimeout(cloudSaveTimer);
  if(!navigator.onLine){ setSyncStatus('offline'); return; }
  setSyncStatus('saving');
  cloudSaveTimer = setTimeout(flushCloudSave, 800);
}

async function flushCloudSave(){
  if(!cloudUID || Object.keys(cloudSaveQueue).length===0) return;
  const batch = cloudSaveQueue;
  cloudSaveQueue = {};
  try{
    await window.FB.saveUserData(cloudUID, batch);
    setSyncStatus('saved');
  }catch(e){
    console.error('Cloud sync failed, changes stay saved locally on this device and will retry:', e);
    Object.assign(cloudSaveQueue, batch); // put it back so the next reconnect/save retries it
    setSyncStatus(navigator.onLine ? 'error' : 'offline');
  }
}

function queueCloudProfile(profile){
  if(!cloudUID) return;
  cloudProfileQueue = profile;
  clearTimeout(cloudProfileTimer);
  if(!navigator.onLine){ setSyncStatus('offline'); return; }
  setSyncStatus('saving');
  cloudProfileTimer = setTimeout(flushCloudProfile, 800);
}

async function flushCloudProfile(){
  if(!cloudUID || !cloudProfileQueue) return;
  const payload = cloudProfileQueue;
  cloudProfileQueue = null;
  try{
    await window.FB.saveProfile(cloudUID, payload);
    setSyncStatus('saved');
  }catch(e){
    console.error('Cloud sync failed, changes stay saved locally on this device and will retry:', e);
    cloudProfileQueue = payload;
    setSyncStatus(navigator.onLine ? 'error' : 'offline');
  }
}

// Retry anything queued as soon as the connection comes back.
window.addEventListener('online', ()=>{
  if(!cloudUID) return;
  flushCloudSave();
  flushCloudProfile();
});
window.addEventListener('offline', ()=> setSyncStatus('offline'));

const DB = {
  get: key => {
    try{
      return JSON.parse(localStorage.getItem(userStorageKey(key)) || 'null');
    }catch{
      return null;
    }
  },

  set: (key, value) => {
    localStorage.setItem(userStorageKey(key), JSON.stringify(value));
    if(key === 'user'){
      queueCloudProfile(value);
    }else if(!SHARED_KEYS.includes(key)){
      queueCloudSave(key, value);
    }
  },

  getList: key => DB.get(key) || [],

  pushItem: (key, item) => {
    const list = DB.getList(key);
    list.push(item);
    DB.set(key, list);
    return item;
  },

  deleteItem: (key, id) => {
    DB.set(key, DB.getList(key).filter(item => item.id !== id));
  }
};

// ═══════ AUTH (Firebase Authentication — works from any device) ═══════
let justAuthenticated = false;

function switchAuthTab(tab){
  document.querySelectorAll('.auth-tab').forEach(btn=>btn.classList.toggle('active',btn.id===`tab-${tab}`));
  document.querySelectorAll('.auth-panel').forEach(panel=>panel.classList.toggle('active',panel.id===`auth-${tab}-panel`));
}

function isValidEmail(email){
  // Reasonably strict RFC-5322-lite check: local@domain.tld, no spaces, no consecutive dots,
  // real-looking domain with a dot and a 2+ letter TLD.
  const re = /^[a-zA-Z0-9](?:[a-zA-Z0-9._%+-]*[a-zA-Z0-9])?@(?:[a-zA-Z0-9](?:[a-zA-Z0-9-]*[a-zA-Z0-9])?\.)+[a-zA-Z]{2,}$/;
  if(!re.test(email)) return false;
  if(email.includes('..')) return false;
  return true;
}

function friendlyAuthError(e){
  const code = (e && e.code) || '';
  if(code.includes('email-already-in-use')) return 'An account with that email already exists. Try logging in instead.';
  if(code.includes('invalid-credential') || code.includes('wrong-password') || code.includes('user-not-found')) return 'Login failed. Check your email/password.';
  if(code.includes('weak-password')) return 'Password should be at least 6 characters.';
  if(code.includes('invalid-email')) return "That email address doesn't look real. Please check and try again.";
  if(code.includes('too-many-requests')) return 'Too many attempts. Please wait a bit and try again.';
  if(code.includes('network-request-failed')) return 'Network error. Check your connection and try again.';
  return 'Something went wrong. Please try again.';
}

// Guards every auth action against the one case that produces a confusing
// crash instead of a helpful message: firebase.js hasn't finished loading
// yet (slow network) or failed to load at all (e.g. the page was opened as
// a local file:// double-click, which browsers block ES modules on — it
// needs to be served over http/https, even just via a simple local server).
// Waits up to `maxWaitMs` for window.FB to appear before giving up, instead
// of failing the instant someone taps Login/Sign up on a slow mobile
// connection where the Firebase SDK is still downloading. Resolves true as
// soon as FB is ready, or false if it never shows up in time.
function waitForFBReady(maxWaitMs = 12000){
  if(window.FB) return Promise.resolve(true);
  return new Promise((resolve)=>{
    const checkInterval = 300;
    let waited = 0;
    const timer = setInterval(()=>{
      if(window.FB){
        clearInterval(timer);
        resolve(true);
        return;
      }
      waited += checkInterval;
      if(waited >= maxWaitMs){
        clearInterval(timer);
        resolve(false);
      }
    }, checkInterval);
  });
}

async function doLogin(){
  const email=document.getElementById('auth-email-login').value.trim();
  const password=document.getElementById('auth-password-login').value;
  if(!email||!password){showToast('Enter email and password.');return;}
  if(!window.FB){
    showToast("Connecting to the login service — this can take a bit longer on mobile data, please hold on…");
    const ready = await waitForFBReady();
    if(!ready){
      showToast("Still couldn't reach the login service. Check your connection and try again. If you opened this page from inside WhatsApp/Instagram, try opening it directly in Chrome or Safari instead.");
      return;
    }
  }
  try{
    justAuthenticated = true;
    await window.FB.signIn(email,password);
    // window.FB.onAuthChange (wired up below) picks this up, pulls this
    // account's data from Firestore, and shows the home page.
  }catch(e){
    justAuthenticated = false;
    showToast(friendlyAuthError(e));
  }
}

async function doSignup(){
  const name=document.getElementById('auth-name-signup').value.trim()||'Traveller';
  const email=document.getElementById('auth-email-signup').value.trim();
  const password=document.getElementById('auth-password-signup').value;
  if(!email||!password){showToast('Enter email and password to sign up.');return;}
  if(!isValidEmail(email)){showToast('That email address doesn\'t look real. Please check and try again.');return;}
  if(password.length<6){showToast('Password should be at least 6 characters.');return;}
  if(!window.FB){
    showToast("Connecting to the login service — this can take a bit longer on mobile data, please hold on…");
    const ready = await waitForFBReady();
    if(!ready){
      showToast("Still couldn't reach the login service. Check your connection and try again. If you opened this page from inside WhatsApp/Instagram, try opening it directly in Chrome or Safari instead.");
      return;
    }
  }
  try{
    justAuthenticated = true;
    await window.FB.signUp(name,email,password);
  }catch(e){
    justAuthenticated = false;
    showToast(friendlyAuthError(e));
  }
}


// One-time migration: if this Firebase account has no cloud data yet, check
// whether this same browser has leftover data from the old localStorage-only
// version of the app (matched by email) and copy it over so early users
// don't lose their trips.
function migrateLegacyLocalData(email){
  try{
    const legacyAccounts = JSON.parse(localStorage.getItem('tt_accounts') || '[]');
    const legacyAccount = legacyAccounts.find(acc=>acc.email && acc.email.toLowerCase()===email.toLowerCase());
    if(!legacyAccount) return false;
    const legacyId = legacyAccount.id;
    const migrated = {};
    ['trips','expenses','waypoints','places','memories','sos_contacts','documents','notifications']
      .forEach(k=>{
        const raw = localStorage.getItem('tt_'+legacyId+'_'+k);
        if(raw){
          try{ migrated[k] = JSON.parse(raw); }catch{}
        }
      });
    // Packing lists are keyed per-trip: packing_list_<tripId>
    (migrated.trips||[]).forEach(trip=>{
      const raw = localStorage.getItem('tt_'+legacyId+'_packing_list_'+trip.id);
      if(raw){
        try{ migrated['packing_list_'+trip.id] = JSON.parse(raw); }catch{}
      }
    });
    if(Object.keys(migrated).length===0) return false;
    Object.entries(migrated).forEach(([key,value])=>{
      localStorage.setItem(userStorageKey(key), JSON.stringify(value));
    });
    Object.assign(cloudSaveQueue, migrated);
    flushCloudSave();
    return true;
  }catch(e){
    console.error('Legacy data migration skipped due to an error:', e);
    return false;
  }
}

// Pulls this account's profile + app data down from Firestore and hydrates
// localStorage with it, so all the existing DB.get()/DB.getList() calls
// throughout the app keep working unchanged.
async function hydrateFromCloud(user){
  cloudUID = user.uid;
  let cloudDoc = null;
  try{
    cloudDoc = await window.FB.loadUserData(user.uid);
  }catch(e){
    console.error('Could not reach cloud storage, using whatever is cached on this device.', e);
    setSyncStatus(navigator.onLine ? 'error' : 'offline');
  }

  const profile = (cloudDoc && cloudDoc.profile) || {
    name: user.displayName || 'Traveller',
    email: user.email,
    currency: '₹',
    joined: today()
  };
  currentUser = { id: user.uid, ...profile };
  localStorage.setItem(userStorageKey('user'), JSON.stringify(currentUser));

  const hasCloudData = cloudDoc && cloudDoc.data && Object.keys(cloudDoc.data).length>0;
  if(hasCloudData){
    Object.entries(cloudDoc.data).forEach(([key, value])=>{
      localStorage.setItem(userStorageKey(key), JSON.stringify(value));
    });
  }else if(user.email){
    // First time this account has been seen in the cloud — see if there's
    // leftover data from the old local-only version of the app to bring in.
    migrateLegacyLocalData(user.email);
  }
}
function updateNavUser(){
  if(!currentUser)return;
  const init=(currentUser.name||'T')[0].toUpperCase();
  document.getElementById('user-avatar').textContent=init;
  document.getElementById('profile-avatar-big').textContent=init;
  document.getElementById('profile-name-display').textContent=currentUser.name||'Traveller';
  document.getElementById('profile-email-display').textContent=currentUser.email||'No email set';
  const pn=document.getElementById('profile-name');
  if(pn)pn.value=currentUser.name||'';
  const pe=document.getElementById('profile-email');
  if(pe)pe.value=currentUser.email||'';
  const pc=document.getElementById('profile-city');
  if(pc)pc.value=currentUser.city||'';
  const ps=document.getElementById('profile-style');
  if(ps)ps.value=currentUser.style||'';
  const pcu=document.getElementById('profile-currency');
  if(pcu)pcu.value=currentUser.currency||'₹';
}

// ═══════ NAV ═══════
function showPage(page){
  if(!currentUser&&page!=='auth'&&page!=='home'){switchAuthTab('login');showPage('auth');return;}
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('.desktop-nav button,.tab-btn').forEach(b=>b.classList.remove('active'));
  const el=document.getElementById('page-'+page);
  if(el)el.classList.add('active');
  ['nav','bnav'].forEach(pfx=>{
    const b=document.getElementById(pfx+'-'+page);
    if(b)b.classList.add('active');
  });
 const loaders={
dashboard:loadDashboard,
trips:loadTrips,
map:loadMap,
expenses:()=>{loadExpenses();loadExchangeRates(false);},
places:()=>loadPlaces(currentPlaceTab),
itinerary:loadItinerary,
packing:loadPackingTrips,
gallery:loadGallery,
home:loadHomeRecent,
profile:loadProfileStats,
analytics:loadAnalytics,
sos:loadSOSContacts,
notifications:showNotifications,
documents:()=>{populateTripSelect('doc-trip');loadDocuments();}
};

  if(loaders[page])loaders[page]();
  window.scrollTo(0,0);
}

// ═══════ TOAST ═══════
function showToast(msg,dur=2500){
  const t=document.getElementById('toast');
  t.textContent=msg;t.classList.add('show');
  setTimeout(()=>t.classList.remove('show'),dur);
}

// ═══════ SMART NOTIFICATIONS ═══════
function addNotification(title,msg,icon='📢',type='info',action=null){
  let notifications=DB.get('notifications')||[];
  notifications.push({
    id:uid(),
    title,
    message:msg,
    icon,
    type,
    timestamp:Date.now(),
    read:false,
    action
  });
  DB.set('notifications',notifications.slice(-50));
  updateNotificationBadge();
  return uid();
}
function updateNotificationBadge(){
  const notifs=DB.get('notifications')||[];
  const unread=notifs.filter(n=>!n.read).length;
  const badge=document.getElementById('notif-badge');
  if(badge){
    badge.textContent=unread;
    badge.style.display=unread>0?'flex':'none';
  }
}
function checkSmartNotifications(){
  const trips=DB.getList('trips');
  const expenses=DB.getList();
  const places=DB.getList('places');
  const today_str=today();
  const now=new Date();

  // Trip starting soon (within 3 days) — planned trips
  trips.filter(t=>t.status==='planned'&&t.start).forEach(trip=>{
    const startDate=new Date(trip.start);
    const daysUntil=(startDate-now)/(1000*60*60*24);
    if(daysUntil>=0&&daysUntil<=3&&!trip.startSoonAlertSent){
      addNotification('🧳 Trip Coming Up',`${trip.name} starts in ${Math.ceil(daysUntil)} day${Math.ceil(daysUntil)!==1?'s':''}! Time to finish packing.`,'🧳','alert');
      trip.startSoonAlertSent=true;
      DB.set('trips',trips);
    }
  });

  // Hotel check-in reminders (same day, any time — not just a 30-min window)
  const hotelPlaces=places.filter(p=>p.type==='hotel'&&p.tripId&&p.date);
  hotelPlaces.forEach(place=>{
    if(!place.notificationSent){
      const trip=trips.find(t=>t.id===place.tripId);
      if(trip&&place.date===today_str){
        addNotification('🏨 Hotel Check-in Today',`${place.name} — check-in today for ${trip.name}.`,'🏨','alert');
        place.notificationSent=true;
        DB.set('places',places);
      }
    }
  });

  // Budget alerts — any trip with a budget, not just ongoing/completed
  trips.filter(t=>t.budget>0).forEach(trip=>{
    const tripExp=expenses.filter(e=>e.tripId===trip.id);
    const spent=tripExp.reduce((sum,e)=>sum+e.amount,0);
    if(spent>trip.budget*0.8&&spent<=trip.budget&&!trip.budgetAlert80){
      addNotification('💰 Budget Warning',`You've spent 80% of your ₹${trip.budget} budget for ${trip.name}`,'⚠️','warning');
      trip.budgetAlert80=true;
      DB.set('trips',trips);
    }
    if(spent>trip.budget&&!trip.budgetAlertOver){
      addNotification('💸 Over Budget!',`You've exceeded your ₹${trip.budget} budget for ${trip.name} by ₹${(spent-trip.budget).toFixed(0)}`,'🚨','danger');
      trip.budgetAlertOver=true;
      DB.set('trips',trips);
    }
  });

  // Weather alerts for ongoing AND planned trips starting within 3 days
  trips.filter(t=>{
    if(t.status==='ongoing')return true;
    if(t.status==='planned'&&t.start){
      const daysUntil=(new Date(t.start)-now)/(1000*60*60*24);
      return daysUntil>=0&&daysUntil<=3;
    }
    return false;
  }).forEach(trip=>{
    if(trip.destination&&!trip.weatherAlertSent){
      fetchWeatherAlerts(trip.destination).then(alerts=>{
        alerts.forEach(alert=>{
          addNotification(`🌤️ ${alert.title}`,alert.message,alert.icon,'alert');
        });
      });
      trip.weatherAlertSent=true;
      DB.set('trips',trips);
    }
  });
}
async function fetchWeatherAlerts(destination){
  const alerts=[];
  try{
    const destClean=destination.split(',')[0].trim();
    const geoResponse=await fetch(`https://nominatim.openstreetmap.org/search?format=json&q=${encodeURIComponent(destClean+' India')}`);
    if(!geoResponse.ok)return alerts;
    const geoData=await geoResponse.json();
    if(!geoData.length)return alerts;
    const geo=geoData[0];
    const wxResponse=await fetch(`https://api.openweathermap.org/data/2.5/forecast?lat=${geo.lat}&lon=${geo.lon}&appid=${OWM_API_KEY}&units=metric`);
    if(!wxResponse.ok)return alerts;
    const wxData=await wxResponse.json();
    const curr=wxData.list[0];
    const weather=curr.weather[0].main.toLowerCase();
    const temp=curr.main.temp;
    if(weather.includes('rain')){
      alerts.push({title:'Rain Expected',message:'Heavy rain detected at your destination.',icon:'🌧️'});
    }
    if(temp>35){
      alerts.push({title:'Heat Alert',message:'High temperature detected. Stay hydrated!',icon:'🔥'});
    }
    if(temp<5){
      alerts.push({title:'Cold Alert',message:'Very cold weather. Carry warm clothes!',icon:'❄️'});
    }
  }catch(e){}
  return alerts;
}
function showNotifications(){
  const notifs=(DB.get('notifications')||[]).reverse();
  const html=notifs.length?notifs.map(n=>`
    <div class="place-card" style="margin-bottom:10px;background:${n.type==='danger'?'#fee':'fff'};border-left:4px solid ${n.type==='danger'?'#c02':n.type==='warning'?'#f90':'#09c'};">
      <div style="flex:1;">
        <div style="font-weight:600;font-size:14px;">${n.icon} ${n.title}</div>
        <div style="font-size:13px;color:var(--ink-mid);margin-top:4px;">${n.message}</div>
        <div style="font-size:11px;color:var(--ink-light);margin-top:4px;">${new Date(n.timestamp).toLocaleString('en-US',{month:'short',day:'numeric',hour:'2-digit',minute:'2-digit'})}</div>
      </div>
      <button class="btn-sm btn-ghost" onclick="markNotificationRead('${n.id}')">✓</button>
    </div>`).join(''):
    '<p style="color:var(--ink-light);text-align:center;padding:20px;">No notifications yet. Your alerts will appear here!</p>';
  document.getElementById('notif-list').innerHTML=html;
  // Mark all as read once the user opens the notifications list
  let stored=DB.get('notifications')||[];
  if(stored.some(n=>!n.read)){
    stored=stored.map(n=>({...n,read:true}));
    DB.set('notifications',stored);
    updateNotificationBadge();
  }
}
function openNotifications(){
  showPage('notifications');
}
function markNotificationRead(id){
  let notifs=DB.get('notifications')||[];
  notifs=notifs.map(n=>n.id===id?{...n,read:true}:n);
  DB.set('notifications',notifs);
  updateNotificationBadge();
  showNotifications();
}
async function clearAllNotifications(){
  const ok=await showConfirm('This will clear all your notifications.','Clear notifications?','🗑️','Clear All','btn-danger');
  if(!ok)return;
  DB.set('notifications',[]);
  updateNotificationBadge();
  showNotifications();
  showToast('Notifications cleared.');
}

// ═══════ MODALS ═══════
function openModal(id){document.getElementById(id).classList.add('open');}
function closeModal(id){document.getElementById(id).classList.remove('open');}
document.querySelectorAll('.modal-overlay').forEach(o=>{
  o.addEventListener('click',e=>{if(e.target===o)o.classList.remove('open');});
});
function populateTripSelect(selId){
  const trips=DB.getList('trips');
  const sel=document.getElementById(selId);
  if(!sel)return;
  sel.innerHTML=trips.map(t=>`<option value="${t.id}">${t.emoji||'✈️'} ${t.name}</option>`).join('');
  if(trips.length)sel.value=trips[trips.length-1].id;
}

// ═══════ DESTINATION PHOTO (Unsplash) ═══════
function getDestPhoto(destination,emoji){
  if(!destination)return Promise.resolve(null);
  const q=encodeURIComponent(destination.split(',')[0].trim()+' travel landscape');
  return `https://source.unsplash.com/400x200/?${q}`;
}

// ═══════ TRIPS ═══════
let tripFilter='all';
let editingTripId=null;
function openNewTripModal(){
  editingTripId=null;
  document.getElementById('trip-name-input').value='';
  document.getElementById('trip-dest').value='';
  document.getElementById('trip-budget').value='';
  document.getElementById('trip-notes').value='';
  document.getElementById('trip-status').value='planned';
  document.getElementById('trip-style').value='';
  document.getElementById('trip-companions').value='';
  document.getElementById('trip-emoji').value='🏔️';
  document.getElementById('trip-start').value=today();
  document.getElementById('trip-end').value=today();
  document.querySelector('#modal-trip .modal-header h3').textContent='✈️ New Trip';
  document.querySelector('#modal-trip .modal-footer .btn-sky').textContent='Save Trip';
  openModal('modal-trip');
}
function openEditTripModal(id){
  const trip=DB.getList('trips').find(t=>t.id===id);
  if(!trip)return;
  editingTripId=id;
  document.getElementById('trip-name-input').value=trip.name||'';
  document.getElementById('trip-start').value=trip.start||today();
  document.getElementById('trip-end').value=trip.end||today();
  document.getElementById('trip-dest').value=trip.destination||'';
  document.getElementById('trip-budget').value=trip.budget||'';
  document.getElementById('trip-style').value=trip.style||'';
  document.getElementById('trip-companions').value=trip.companions||'';
  document.getElementById('trip-status').value=trip.status||'planned';
  document.getElementById('trip-emoji').value=trip.emoji||'🏔️';
  document.getElementById('trip-notes').value=trip.notes||'';
  document.querySelector('#modal-trip .modal-header h3').textContent='✏️ Edit Trip';
  document.querySelector('#modal-trip .modal-footer .btn-sky').textContent='Save Changes';
  openModal('modal-trip');
}
function saveTrip(){
  const name=document.getElementById('trip-name-input').value.trim();
  if(!name){showToast('Please enter a trip name!');return;}
  const tripData={
    name,
    start:document.getElementById('trip-start').value,
    end:document.getElementById('trip-end').value,
    destination:document.getElementById('trip-dest').value,
    budget:+document.getElementById('trip-budget').value||0,
    style:document.getElementById('trip-style').value,
    companions:document.getElementById('trip-companions').value,
    status:document.getElementById('trip-status').value,
    emoji:document.getElementById('trip-emoji').value,
    notes:document.getElementById('trip-notes').value
  };
  if(editingTripId){
    const trips=DB.getList('trips').map(t=>t.id===editingTripId?{...t,...tripData}:t);
    DB.set('trips',trips);
    showToast('Trip updated! ✓');
    editingTripId=null;
  } else {
    DB.pushItem('trips',{id:uid(),...tripData,created:today()});
    showToast('Trip saved! ✈️');
    addNotification('✈️ New Trip Created',`"${tripData.name}" has been added to your trips.`,'✈️','info');
  }
  closeModal('modal-trip');
  ['trip-name-input','trip-notes'].forEach(id=>{document.getElementById(id).value='';});
  loadTrips();
}
function filterTrips(s){tripFilter=s;loadTrips();}
function loadTrips(){
  const q=(document.getElementById('trip-search')||{}).value||'';
  let trips=DB.getList('trips');
  if(tripFilter!=='all')trips=trips.filter(t=>t.status===tripFilter);
  if(q)trips=trips.filter(t=>(t.name+' '+t.destination).toLowerCase().includes(q.toLowerCase()));
  const grid=document.getElementById('trips-grid');
  if(!trips.length){
    grid.innerHTML=`<div class="empty" style="grid-column:1/-1"><div class="icon">✈️</div><h3>No trips found</h3><p>Create your first adventure!</p><button class="btn-primary btn-lg" onclick="openNewTripModal()">+ Create Trip</button></div>`;
    return;
  }
  grid.innerHTML=trips.map(t=>tripCardHTML(t)).join('');
}
function tripCardHTML(t){
  const expenses=DB.getList('expenses').filter(e=>e.tripId===t.id);
  const spent=expenses.reduce((a,e)=>a+e.amount,0);
  const colors={planned:'#e8f4f8',ongoing:'#e8f5e8',completed:'#f5e6dc'};
  const pct=t.budget?Math.min(100,Math.round(spent/t.budget*100)):0;
  const imgUrl=t.destination?`https://source.unsplash.com/400x200/?${encodeURIComponent(t.destination.split(',')[0].trim()+' travel')}`:null;
  return `<div class="card">
    <div class="trip-thumb">
      ${imgUrl?`<img src="${imgUrl}" alt="${t.destination}" onerror="this.style.display='none'" loading="lazy">`:''}
      <span class="emoji-fallback">${t.emoji||'✈️'}</span>
      <div class="img-overlay"></div>
      <span class="trip-badge badge-${t.status}">${t.status}</span>
    </div>
    <div class="card-body">
      <div class="trip-title">${t.name}</div>
      <div class="trip-meta">📍 ${t.destination||'Destination TBD'}</div>
      <div class="trip-meta">📅 ${fmt(t.start)} → ${fmt(t.end)}</div>
      ${t.style?`<div class="trip-meta">✨ ${t.style}</div>`:''}
      ${t.companions?`<div class="trip-meta">👥 ${t.companions}</div>`:''}
      <div class="trip-stats">
        <div class="stat"><div class="stat-val">${fmtNum(spent)}</div><div class="stat-label">Spent</div></div>
        <div class="stat"><div class="stat-val">${fmtNum(t.budget)}</div><div class="stat-label">Budget</div></div>
        <div class="stat"><div class="stat-val">${expenses.length}</div><div class="stat-label">Expenses</div></div>
      </div>
      ${t.budget?`<div class="progress-bar" style="margin-top:10px;"><div class="progress-fill ${pct>90?'danger':pct>70?'warn':''}" style="width:${pct}%"></div></div>`:''}
    </div>
    <div class="card-footer">
      <button class="btn-sm btn-sky" onclick="viewTrip('${t.id}')">View</button>
      <button class="btn-sm btn-ghost" onclick="openEditTripModal('${t.id}')">✏️ Edit</button>
      <button class="btn-sm btn-ghost" onclick="downloadTripReport('${t.id}')">⬇️ Report</button>
      <button class="btn-sm btn-danger" onclick="deleteTrip('${t.id}')">Delete</button>
    </div>
  </div>`;
}
async function deleteTrip(id){
  const ok=await showConfirm('This will permanently delete the trip and all its expenses, waypoints, places and memories.','Delete trip?','🗑️','Delete','btn-danger');
  if(!ok)return;
  DB.deleteItem('trips',id);
  ['expenses','waypoints','places','memories'].forEach(k=>{DB.set(k,DB.getList(k).filter(x=>x.tripId!==id));});
  loadTrips();showToast('Trip deleted.');
}
function viewTrip(id){
  const trip=DB.getList('trips').find(t=>t.id===id);
  if(!trip)return;
  const expenses=DB.getList('expenses').filter(e=>e.tripId===id);
  const places=DB.getList('places').filter(p=>p.tripId===id);
  const waypoints=DB.getList('waypoints').filter(w=>w.tripId===id);
  const spent=expenses.reduce((a,e)=>a+e.amount,0);
  const hotels=places.filter(p=>p.type==='hotel');
  const restaurants=places.filter(p=>p.type==='restaurant');
  const pct=trip.budget?Math.min(100,Math.round(spent/trip.budget*100)):0;
  const barClass=pct>90?'danger':pct>70?'warn':'';
  const imgUrl=trip.destination?`https://source.unsplash.com/1200x300/?${encodeURIComponent(trip.destination.split(',')[0].trim()+' landscape')}`:null;
  const timelineHTML=buildTripTimeline(trip, places, expenses, waypoints);
  document.getElementById('trip-detail-content').innerHTML=`
    ${imgUrl?`<div style="height:200px;border-radius:var(--radius);overflow:hidden;margin-bottom:20px;"><img src="${imgUrl}" style="width:100%;height:100%;object-fit:cover;" onerror="this.parentElement.style.display='none'" loading="lazy"></div>`:''}
    <div style="display:flex;align-items:center;gap:14px;margin-bottom:20px;flex-wrap:wrap;">
      <span style="font-size:52px;">${trip.emoji||'✈️'}</span>
      <div>
        <h2 style="font-size:28px;margin-bottom:3px;">${trip.name}</h2>
        <p style="color:var(--ink-light);font-size:14px;">📍 ${trip.destination||'—'} &nbsp;|&nbsp; 📅 ${fmt(trip.start)} → ${fmt(trip.end)}</p>
        ${trip.style?`<p style="color:var(--ink-light);font-size:13px;margin-top:4px;">✨ ${trip.style}</p>`:''}
        ${trip.companions?`<p style="color:var(--ink-light);font-size:13px;margin-top:2px;">👥 ${trip.companions}</p>`:''}
        <span class="trip-badge badge-${trip.status}" style="position:static;display:inline-block;margin-top:6px;">${trip.status}</span>
      </div>
    </div>
    <div class="stat-cards">
      <div class="stat-card sky"><div class="big">${fmtNum(spent)}</div><div class="lbl">Total Spent</div></div>
      <div class="stat-card terra"><div class="big">${fmtNum(trip.budget)}</div><div class="lbl">Budget</div></div>
      <div class="stat-card forest"><div class="big">${expenses.length}</div><div class="lbl">Expenses</div></div>
      <div class="stat-card"><div class="big">${hotels.length}</div><div class="lbl">Hotels</div></div>
      <div class="stat-card"><div class="big">${restaurants.length}</div><div class="lbl">Restaurants</div></div>
      <div class="stat-card warn"><div class="big">${waypoints.length}</div><div class="lbl">Waypoints</div></div>
    </div>
    ${trip.budget?`<div class="form-section"><h3 style="font-size:15px;margin-bottom:10px;">Budget Progress</h3>
      <div style="display:flex;justify-content:space-between;font-size:12px;margin-bottom:5px;"><span>${fmtNum(spent)} spent</span><span>${pct}% of ${fmtNum(trip.budget)}</span></div>
      <div class="progress-bar"><div class="progress-fill ${barClass}" style="width:${pct}%"></div></div></div>`:''}
    ${trip.notes?`<div class="alert alert-info">${trip.notes}</div>`:''}
    <div id="weather-section" style="margin-top:18px;"></div>
    <div class="form-section" style="margin-top:18px;">
      <h3 style="font-size:15px;margin-bottom:10px;">Trip Timeline</h3>
      ${timelineHTML}
    </div>
    <div class="grid-2" style="margin-top:18px;">
      <div>
        <h3 style="font-size:15px;margin-bottom:10px;">Recent Expenses</h3>
        ${expenses.slice(-5).reverse().map(e=>`<div style="display:flex;justify-content:space-between;padding:9px 0;border-bottom:1px solid var(--border);font-size:13px;"><span>${e.desc}</span><span style="font-weight:600;">${fmtNum(e.amount)}</span></div>`).join('')||'<p style="color:var(--ink-light);font-size:13px;">No expenses logged.</p>'}
      </div>
      <div>
        <h3 style="font-size:15px;margin-bottom:10px;">Hotels & Stays</h3>
        ${hotels.map(h=>`<div class="waypoint"><span style="font-size:20px">🏨</span><div class="wp-info"><div class="wp-name">${h.name}</div><div class="wp-detail">${h.location||''} · ${fmtNum(h.cost)}</div></div></div>`).join('')||'<p style="color:var(--ink-light);font-size:13px;">No hotels logged.</p>'}
      </div>
    </div>
    <div style="margin-top:18px;display:flex;gap:10px;flex-wrap:wrap;">
      <button class="btn-sm btn-sky" onclick="addExpenseForTrip('${trip.id}')">+ Add Expense</button>
      <button class="btn-sm btn-sky" onclick="downloadTripReportPDF('${trip.id}')">📄 Download PDF</button>
      <button class="btn-sm btn-primary" onclick="generateTripSummary('${trip.id}')">✨ Generate AI Summary</button>
      <button class="btn-sm btn-ghost" onclick="showPage('map')">📍 View on Map</button>
      <button class="btn-sm btn-ghost" onclick="showPage('gallery')">📸 Gallery</button>
      <button class="btn-sm btn-primary" onclick="showTripTimeline('${trip.id}')">🕒 Travel Timeline</button>
      <button class="btn-sm btn-danger" onclick="deleteTrip('${trip.id}');showPage('trips')">Delete Trip</button>
    </div>
  `;
  currentTripId=id;
  if(trip.destination)fetchDestinationWeather(trip.destination,'weather-section');
  showPage('trip-detail');
}
function buildTripTimeline(trip, places, expenses, waypoints){
  const events=[];
  events.push({date:trip.start,title:'Trip begins',detail:`Depart for ${trip.destination||'destination'}`});
  const allEvents=[...places.map(p=>({date:p.date||trip.start,title:p.type==='hotel'?`Stay at ${p.name}`:`Visit ${p.name}`,detail:`${p.type==='hotel'?'Check in':'Explore'} ${p.location||'your destination'}`})),
                   ...expenses.map(e=>({date:e.date||trip.start,title:`Expense: ${e.desc}`,detail:`${fmtNum(e.amount)} — ${e.category}`})),
                   ...waypoints.map(w=>({date:w.lat||w.lng?trip.start:'',title:`Waypoint: ${w.name}`,detail:`${w.type}${w.altitude?` · ${w.altitude}m`:''}${w.dist?` · ${w.dist}km`:''}${w.notes?` · ${w.notes}`:''}`}))];
  const dated=allEvents.filter(ev=>ev.date).sort((a,b)=>a.date.localeCompare(b.date));
  const undated=allEvents.filter(ev=>!ev.date);
  const timeline=[...events, ...dated, ...undated, {date:trip.end,title:'Trip ends',detail:'Return home or complete your journey.'}];
  return `<div class="timeline">${timeline.map(item=>`<div class="timeline-item"><div class="timeline-date">${item.date||'—'}</div><div class="timeline-card"><div class="timeline-title">${item.title}</div><div class="timeline-detail">${item.detail||'—'}</div></div></div>`).join('')}</div>`;
}
function showTripTimeline(tripId){
  const trip = DB.getList('trips').find(t => t.id === tripId);

  if(!trip){
    showToast('Trip not found.');
    return;
  }

  const places = DB.getList('places').filter(p => p.tripId === tripId);
  const expenses = DB.getList('expenses').filter(e => e.tripId === tripId);
  const waypoints = DB.getList('waypoints').filter(w => w.tripId === tripId);

  document.getElementById('timeline-trip-title').textContent =
    `${trip.name || 'Trip'} — Automatic Timeline`;

  document.getElementById('timeline-trip-content').innerHTML =
    buildTripTimeline(trip, places, expenses, waypoints);

  openModal('modal-trip-timeline');
}
function downloadTripReport(tripId){
  const trip=DB.getList('trips').find(t=>t.id===tripId);
  if(!trip){showToast('Trip not found.');return;}
  const expenses=DB.getList('expenses').filter(e=>e.tripId===tripId);
  const places=DB.getList('places').filter(p=>p.tripId===tripId);
  const waypoints=DB.getList('waypoints').filter(w=>w.tripId===tripId);
  const safeName=(trip.name||'trip').replace(/[^a-zA-Z0-9-_ ]/g,'').trim()||'trip';
  const lines=[
    `Trip Report - ${trip.name}`,
    `Destination: ${trip.destination||'—'}`,
    `Dates: ${fmt(trip.start)} to ${fmt(trip.end)}`,
    `Status: ${trip.status}`,
    `Budget: ${fmtNum(trip.budget)}`,
    '',
    'Expenses:',
    ...expenses.length?expenses.map(e=>`- ${fmt(e.date)} | ${e.desc} | ${fmtNum(e.amount)} | ${e.category}`):['- No expenses logged.'],
    '',
    'Places & Stays:',
    ...places.length?places.map(p=>`- ${p.name} | ${p.type} | ${p.location||'—'} | ${fmt(p.date)} | ${fmtNum(p.cost)}${p.notes?` | ${p.notes}`:''}`):['- No places logged.'],
    '',
    'Waypoints:',
    ...waypoints.length?waypoints.map(w=>`- ${w.name} | ${w.type} | ${w.altitude?`${w.altitude}m`:''}${w.dist?` | ${w.dist}km`:''}${w.notes?` | ${w.notes}`:''}`):['- No waypoints logged.']
  ];
  const blob=new Blob([lines.join('\n')],{type:'text/plain;charset=utf-8'});
  const url=URL.createObjectURL(blob);
  const a=document.createElement('a');
  a.href=url;
  a.download=`${safeName}-report.txt`;
  document.body.appendChild(a);
  a.click();
  a.remove();
  URL.revokeObjectURL(url);
  showToast('Trip report downloaded.');
}
function downloadTripReportPDF(tripId){
  const trip=DB.getList('trips').find(t=>t.id===tripId);
  if(!trip){showToast('Trip not found.');return;}
  const expenses=DB.getList('expenses').filter(e=>e.tripId===tripId);
  const places=DB.getList('places').filter(p=>p.tripId===tripId);
  const waypoints=DB.getList('waypoints').filter(w=>w.tripId===tripId);
  if(!window.jspdf||!window.jspdf.jsPDF){
    showToast('Loading PDF engine…');
    const script=document.createElement('script');
    script.src='https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js';
    script.onload=()=>{ if(window.jspdf&&window.jspdf.jsPDF){ buildTripPDF(trip,expenses,places,waypoints); } else { showToast('PDF engine failed to load. Check your internet connection.'); } };
    script.onerror=()=>showToast('Could not load PDF engine. Check your internet connection.');
    document.head.appendChild(script);
    return;
  }
  buildTripPDF(trip,expenses,places,waypoints);
}
function buildTripPDF(trip,expenses,places,waypoints){
  const { jsPDF } = window.jspdf;
  const doc=new jsPDF({unit:'pt',format:'a4'});
  const margin=40;
  let y=margin;
  doc.setFontSize(22);
  doc.text('TripReport — '+trip.name,margin,y);
  doc.setFontSize(11);
  y+=28;
  doc.text(`Destination: ${trip.destination||'—'}`,margin,y);
  doc.text(`Dates: ${fmt(trip.start)} → ${fmt(trip.end)}`,margin, y+=16);
  doc.text(`Status: ${trip.status}`,margin, y+=16);
  doc.text(`Budget: ${fmtNum(trip.budget)}`,margin, y+=16);
  y+=8;
  if(trip.notes){
    doc.setFontSize(12);
    doc.text('Notes:',margin,y);
    y+=14;
    const notesLines=doc.splitTextToSize(trip.notes,520);
    doc.setFontSize(10);
    doc.text(notesLines,margin,y);
    y+=notesLines.length*14;
  }
  y+=10;
  doc.setFontSize(12);
  doc.text('Expenses',margin,y);
  y+=16;
  if(expenses.length){
    doc.setFontSize(10);
    expenses.forEach(exp=>{
      if(y>760){doc.addPage();y=margin;}
      doc.text(`• ${fmt(exp.date)} - ${exp.desc} (${fmtNum(exp.amount)})`,margin,y);
      y+=14;
    });
  } else {
    doc.setFontSize(10);
    doc.text('No expenses logged.',margin,y);
    y+=14;
  }
  y+=10;
  doc.setFontSize(12);
  doc.text('Places & Stays',margin,y);
  y+=16;
  if(places.length){
    doc.setFontSize(10);
    places.forEach(place=>{
      if(y>760){doc.addPage();y=margin;}
      doc.text(`• ${place.name} (${place.type}) - ${place.location||'—'} - ${fmt(place.date)} - ${fmtNum(place.cost)}`,margin,y);
      if(place.notes){y+=12;doc.text(`  Note: ${place.notes}`,margin+10,y);}      
      y+=16;
    });
  } else {
    doc.setFontSize(10);
    doc.text('No places logged.',margin,y);
    y+=14;
  }
  y+=10;
  doc.setFontSize(12);
  doc.text('Waypoints',margin,y);
  y+=16;
  if(waypoints.length){
    doc.setFontSize(10);
    waypoints.forEach(wp=>{
      if(y>760){doc.addPage();y=margin;}
      doc.text(`• ${wp.name} [${wp.type}] ${wp.altitude?`· ${wp.altitude}m`:''} ${wp.dist?`· ${wp.dist}km`:''}`,margin,y);
      if(wp.notes){y+=12;doc.text(`  Note: ${wp.notes}`,margin+10,y);}      
      y+=16;
    });
  } else {
    doc.setFontSize(10);
    doc.text('No waypoints logged.',margin,y);
    y+=14;
  }
  doc.save(`${trip.name.replace(/[^a-zA-Z0-9-_ ]/g,'')||'trip'}-report.pdf`);
}
function loadHomeRecent(){
  const trips=DB.getList('trips').slice(-3).reverse();
  const el=document.getElementById('home-recent-trips');
  if(!el)return;
  if(!trips.length){el.innerHTML='<p style="color:var(--ink-light);grid-column:1/-1;">No trips yet. Create your first!</p>';return;}
  el.innerHTML=trips.map(t=>tripCardHTML(t)).join('');
}
// ═══════ AI TRIP SUMMARY GENERATOR ═══════
function generateTripSummary(tripId) {
  const trip = DB.getList('trips').find(t => t.id === tripId);

  if (!trip) {
    showToast('Trip not found.');
    return;
  }

  const expenses = DB.getList('expenses').filter(e => e.tripId === tripId);
  const places = DB.getList('places').filter(p => p.tripId === tripId);
  const waypoints = DB.getList('waypoints').filter(w => w.tripId === tripId);
  const memories = DB.getList('memories').filter(m => m.tripId === tripId);

  const totalSpent = expenses.reduce((sum, item) => sum + (+item.amount || 0), 0);
  const totalDays = tripDays(trip.start, trip.end);

  const categoryTotals = {};
  expenses.forEach(item => {
    const category = item.category || 'other';
    categoryTotals[category] = (categoryTotals[category] || 0) + (+item.amount || 0);
  });

  const topCategory = Object.keys(categoryTotals).sort(
    (a, b) => categoryTotals[b] - categoryTotals[a]
  )[0];

  const hotelCount = places.filter(p => p.type === 'hotel').length;
  const restaurantCount = places.filter(p => p.type === 'restaurant').length;
  const viewpointCount = waypoints.filter(w => w.type === 'viewpoint').length;

  const budgetText = trip.budget
    ? totalSpent > trip.budget
      ? `The trip went over the planned budget by ${fmtNum(totalSpent - trip.budget)}.`
      : `The trip stayed within the planned budget, with ${fmtNum(trip.budget - totalSpent)} remaining.`
    : 'No budget was set for this trip.';

  const highlights = [];

  if (places.length) {
    highlights.push(`visited ${places.length} saved place${places.length > 1 ? 's' : ''}`);
  }

  if (hotelCount) {
    highlights.push(`stayed at ${hotelCount} hotel${hotelCount > 1 ? 's' : ''}`);
  }

  if (restaurantCount) {
    highlights.push(`explored ${restaurantCount} restaurant${restaurantCount > 1 ? 's' : ''}`);
  }

  if (viewpointCount) {
    highlights.push(`captured ${viewpointCount} scenic viewpoint${viewpointCount > 1 ? 's' : ''}`);
  }

  if (memories.length) {
    highlights.push(`saved ${memories.length} travel memor${memories.length > 1 ? 'ies' : 'y'}`);
  }

  const highlightText = highlights.length
    ? highlights.join(', ')
    : 'recorded the main trip details';

  const topExpenseText = topCategory
    ? `The highest spending category was ${topCategory}, totaling ${fmtNum(categoryTotals[topCategory])}.`
    : 'No expenses were recorded for this trip.';

  const placeNames = places.slice(0, 5).map(p => p.name).filter(Boolean);
  const placesText = placeNames.length
    ? `Key places included ${placeNames.join(', ')}.`
    : '';

  const summary = `
    <div class="form-section" style="margin-top:18px;border:2px solid var(--sky);">
      <div style="display:flex;justify-content:space-between;align-items:center;gap:10px;flex-wrap:wrap;">
        <h3 style="font-size:16px;margin:0;">✨ AI Trip Summary</h3>
        <button class="btn-sm btn-ghost" onclick="document.getElementById('ai-trip-summary').remove()">✕ Close</button>
      </div>

      <div style="margin-top:12px;line-height:1.8;font-size:14px;">
        <p>
          <strong>${trip.name}</strong> was a ${totalDays}-day trip to
          <strong>${trip.destination || 'the selected destination'}</strong>
          from ${fmt(trip.start)} to ${fmt(trip.end)}.
        </p>

        <p>
          During this journey, the traveller ${highlightText}.
          ${placesText}
        </p>

        <p>
          Total recorded spending was <strong>${fmtNum(totalSpent)}</strong>.
          ${topExpenseText}
          ${budgetText}
        </p>

        <p>
          ${trip.notes ? `Trip note: “${trip.notes}”` : 'This trip has been successfully captured in TripTrace with travel, expense, place, and memory information.'}
        </p>
      </div>

      <div class="grid-3" style="margin-top:14px;">
        <div class="stat-card sky">
          <div class="big">${totalDays}</div>
          <div class="lbl">Trip Days</div>
        </div>

        <div class="stat-card terra">
          <div class="big">${fmtNum(totalSpent)}</div>
          <div class="lbl">Recorded Spending</div>
        </div>

        <div class="stat-card forest">
          <div class="big">${places.length + waypoints.length}</div>
          <div class="lbl">Places Captured</div>
        </div>
      </div>
    </div>
  `;

  const oldSummary = document.getElementById('ai-trip-summary');

  if (oldSummary) {
    oldSummary.outerHTML = `<div id="ai-trip-summary">${summary}</div>`;
  } else {
    const buttonArea = document.querySelector('#trip-detail-content > div:last-child');

    if (buttonArea) {
      buttonArea.insertAdjacentHTML('beforebegin', `<div id="ai-trip-summary">${summary}</div>`);
    } else {
      document.getElementById('trip-detail-content').insertAdjacentHTML(
        'beforeend',
        `<div id="ai-trip-summary">${summary}</div>`
      );
    }
  }

  showToast('AI trip summary generated! ✨');
}

// ═══════ DASHBOARD ═══════
function loadDashboard(){
  const trips=DB.getList('trips');
  const expenses=DB.getList('expenses');
  const places=DB.getList('places');
  const waypoints=DB.getList('waypoints');
  const totalSpent=expenses.reduce((a,e)=>a+e.amount,0);
  document.getElementById('dashboard-stats').innerHTML=`
    <div class="stat-card sky"><div class="big">${trips.length}</div><div class="lbl">Total Trips</div></div>
    <div class="stat-card terra"><div class="big">${fmtNum(totalSpent)}</div><div class="lbl">Total Spent</div></div>
    <div class="stat-card forest"><div class="big">${places.filter(p=>p.type==='hotel').length}</div><div class="lbl">Hotels Stayed</div></div>
    <div class="stat-card warn"><div class="big">${places.filter(p=>p.type==='restaurant').length}</div><div class="lbl">Restaurants</div></div>
    <div class="stat-card"><div class="big">${waypoints.filter(w=>w.type==='viewpoint').length}</div><div class="lbl">Viewpoints</div></div>
    <div class="stat-card"><div class="big">${trips.filter(t=>t.status==='completed').length}</div><div class="lbl">Completed</div></div>`;
  const recent=document.getElementById('dashboard-recent');
  if(!trips.length){recent.innerHTML='<p style="color:var(--ink-light);font-size:13px;">No trips yet.</p>';}
  else recent.innerHTML=trips.slice(-4).reverse().map(t=>{
    const s=expenses.filter(e=>e.tripId===t.id).reduce((a,e)=>a+e.amount,0);
    return `<div class="waypoint" onclick="viewTrip('${t.id}')">
      <span style="font-size:20px">${t.emoji||'✈️'}</span>
      <div class="wp-info"><div class="wp-name">${t.name}</div><div class="wp-detail">${fmt(t.start)} · ${fmtNum(s)} spent</div></div>
      <span class="trip-badge badge-${t.status}" style="position:static">${t.status}</span>
    </div>`;
  }).join('');
  const catTotals={};
  const catLabels={food:'🍔 Food',hotel:'🏨 Hotel',transport:'🚌 Transport',activity:'🎯 Activity',shopping:'🛍️ Shopping',other:'📌 Other'};
  expenses.forEach(e=>{catTotals[e.category]=(catTotals[e.category]||0)+e.amount;});
  const maxCat=Math.max(1,...Object.values(catTotals));
  document.getElementById('dashboard-spend').innerHTML=Object.entries(catTotals).map(([cat,amt])=>`
    <div style="margin-bottom:12px;">
      <div style="display:flex;justify-content:space-between;font-size:12px;margin-bottom:4px;"><span>${catLabels[cat]||cat}</span><span style="font-weight:600">${fmtNum(amt)}</span></div>
      <div class="progress-bar"><div class="progress-fill" style="width:${Math.round(amt/maxCat*100)}%"></div></div>
    </div>`).join('')||'<p style="color:var(--ink-light);font-size:13px;">No expenses yet.</p>';
  const vps=waypoints.filter(w=>w.type==='viewpoint');
  document.getElementById('dashboard-viewpoints').innerHTML=vps.length?vps.map(v=>{
    const trip=trips.find(t=>t.id===v.tripId);
    return `<div class="card" style="padding:16px;">
      <div style="font-size:26px;margin-bottom:6px;">🏔️</div>
      <div style="font-weight:600;font-size:14px;margin-bottom:3px;">${v.name}</div>
      <div style="font-size:12px;color:var(--ink-light);">${trip?trip.name:''}</div>
      ${v.altitude?`<div style="font-size:11px;color:var(--sky);margin-top:3px;">⛰️ ${v.altitude}m</div>`:''}
    </div>`;
  }).join(''):'<p style="color:var(--ink-light);font-size:13px;grid-column:1/-1">No viewpoints yet.</p>';
}

// ═══════ EXPENSES ═══════
let receiptFile=null;
function handleReceiptUpload(e){
  const file=e.target.files[0];
  if(!file)return;
  if(file.size>5*1024*1024){showToast('Receipt image too large. Max 5MB.');return;}
  receiptFile=file;
  const reader=new FileReader();
  reader.onload=ev=>{
    const preview=document.getElementById('receipt-preview');
    preview.innerHTML=`<img src="${ev.target.result}" alt="receipt preview">`;
    document.getElementById('receipt-scan-status').textContent='Receipt ready. Tap Scan Receipt to extract data.';
  };
  reader.readAsDataURL(file);
}
function scanReceipt(){
  if(!receiptFile){showToast('Upload a receipt image first.');return;}
  if(typeof Tesseract==='undefined'){showToast('Receipt OCR library not loaded.');return;}
  const status=document.getElementById('receipt-scan-status');
  status.textContent='Scanning receipt — please wait...';
  Tesseract.recognize(receiptFile,'eng',{logger:m=>{if(m.status==='recognizing text'){status.textContent=`Scanning receipt — ${Math.round(m.progress*100)}%`;}}})
    .then(({data:{text}})=>{
      status.textContent='Receipt scanned. Parsing results...';
      parseReceiptText(text);
      status.textContent='Receipt scan complete. Review fields before saving.';
    }).catch(err=>{
      console.error(err);
      status.textContent='Receipt scan failed. Try a clearer photo.';
      showToast('Receipt scan failed.');
    });
}
function parseReceiptText(raw){
  const text=raw.replace(/\r/g,'\n');
  const lines=text.split('\n').map(l=>l.trim()).filter(Boolean);
  if(lines.length){document.getElementById('exp-desc').value=lines[0].slice(0,80);}  
  const amountMatches=[...text.matchAll(/(?:₹|Rs\.?|INR|\$)?\s*([0-9]{1,3}(?:[0-9,]*)(?:\.[0-9]{1,2})?)/gi)];
  let maxAmount=0;
  amountMatches.forEach(m=>{const v=Number(m[1].replace(/,/g,''));if(!isNaN(v)&&v>maxAmount)maxAmount=v;});
  if(maxAmount>0){document.getElementById('exp-amount').value=maxAmount;}
  const dateMatch=text.match(/(\d{4}-\d{2}-\d{2})|(\d{2}[\/\-]\d{2}[\/\-]\d{4})|(\d{1,2}\s+(?:Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec)[a-z]*\s+\d{4})/i);
  if(dateMatch){const parsed=normalizeDate(dateMatch[0]); if(parsed)document.getElementById('exp-date').value=parsed;}
  const noteLines=lines.slice(1,5);
  if(noteLines.length){document.getElementById('exp-notes').value=noteLines.join(' · ');}  
  const categoryKeywords={hotel:'hotel',restaurant:'food',cafe:'food',bar:'food',pub:'food',bakery:'food',taxi:'transport',uber:'transport',bus:'transport',train:'transport',flight:'transport',shopping:'shopping',souvenir:'shopping',ticket:'activity',entry:'activity',museum:'activity',admission:'activity'};
  const found=text.match(new RegExp(Object.keys(categoryKeywords).join('|'),'i'));
  if(found){document.getElementById('exp-cat').value=categoryKeywords[found[0].toLowerCase()]||'other';}
}
function normalizeDate(value){
  const v=value.trim();
  if(/^\d{4}-\d{2}-\d{2}$/.test(v))return v;
  const dm=v.match(/^(\d{2})[\/\-](\d{2})[\/\-](\d{4})$/);
  if(dm){return `${dm[3]}-${dm[2]}-${dm[1]}`;}
  const parsed=new Date(v);
  if(!isNaN(parsed))return parsed.toISOString().slice(0,10);
  return null;
}
let editingExpenseId=null;
function openExpenseModal(){
  editingExpenseId=null;
  populateTripSelect('exp-trip');
  document.getElementById('exp-date').value=today();
  document.getElementById('exp-desc').value='';
  document.getElementById('exp-amount').value='';
  document.getElementById('exp-notes').value='';
  document.getElementById('exp-cat').value='food';
  document.querySelector('#modal-expense .modal-header h3').textContent='💰 Add Expense';
  document.querySelector('#modal-expense .modal-footer .btn-sky').textContent='Add Expense';
  openModal('modal-expense');
}
function addExpenseForTrip(tripId){
  openExpenseModal();
  document.getElementById('exp-trip').value=tripId;
}
function openEditExpenseModal(id){
  const exp=DB.getList('expenses').find(e=>e.id===id);
  if(!exp)return;
  editingExpenseId=id;
  populateTripSelect('exp-trip');
  document.getElementById('exp-trip').value=exp.tripId;
  document.getElementById('exp-desc').value=exp.desc||'';
  document.getElementById('exp-amount').value=exp.amount||'';
  document.getElementById('exp-date').value=exp.date||today();
  document.getElementById('exp-cat').value=exp.category||'other';
  document.getElementById('exp-notes').value=exp.notes||'';
  document.querySelector('#modal-expense .modal-header h3').textContent='✏️ Edit Expense';
  document.querySelector('#modal-expense .modal-footer .btn-sky').textContent='Save Changes';
  openModal('modal-expense');
}
function saveExpense(){
  const desc=document.getElementById('exp-desc').value.trim();
  const amount=+document.getElementById('exp-amount').value;
  if(!desc||!amount){showToast('Fill in description and amount.');return;}
  const expData={tripId:document.getElementById('exp-trip').value,desc,amount,date:document.getElementById('exp-date').value,category:document.getElementById('exp-cat').value,notes:document.getElementById('exp-notes').value};
  if(editingExpenseId){
    const expenses=DB.getList('expenses').map(e=>e.id===editingExpenseId?{...e,...expData}:e);
    DB.set('expenses',expenses);
    showToast('Expense updated! ✓');
    editingExpenseId=null;
  } else {
    DB.pushItem('expenses',{id:uid(),...expData});
    showToast('Expense added! 💰');
  }
  closeModal('modal-expense');
  document.getElementById('exp-desc').value='';document.getElementById('exp-amount').value='';
  if(document.getElementById('page-expenses').classList.contains('active'))loadExpenses();
}
function loadExpenses(){
  const trips=DB.getList('trips');
  const tripSel=document.getElementById('expense-trip-filter');
  tripSel.innerHTML='<option value="">All Trips</option>'+trips.map(t=>`<option value="${t.id}">${t.emoji||'✈️'} ${t.name}</option>`).join('');
  const tripFilter=tripSel.value;
  const catFilter=document.getElementById('expense-cat-filter').value;
  let expenses=DB.getList('expenses');
  if(tripFilter)expenses=expenses.filter(e=>e.tripId===tripFilter);
  if(catFilter)expenses=expenses.filter(e=>e.category===catFilter);
  const total=expenses.reduce((a,e)=>a+e.amount,0);
  const catTotals={};
  expenses.forEach(e=>{catTotals[e.category]=(catTotals[e.category]||0)+e.amount;});
  document.getElementById('expense-summary').innerHTML=`
    <div class="stat-card sky"><div class="big">${fmtNum(total)}</div><div class="lbl">Total</div></div>
    <div class="stat-card terra"><div class="big">${fmtNum(catTotals.hotel||0)}</div><div class="lbl">Hotel</div></div>
    <div class="stat-card forest"><div class="big">${fmtNum(catTotals.food||0)}</div><div class="lbl">Food</div></div>
    <div class="stat-card warn"><div class="big">${fmtNum(catTotals.transport||0)}</div><div class="lbl">Transport</div></div>
    <div class="stat-card"><div class="big">${fmtNum(catTotals.activity||0)}</div><div class="lbl">Activity</div></div>`;
  const catLabels={food:'🍔 Food',hotel:'🏨 Hotel',transport:'🚌 Transport',activity:'🎯 Activity',shopping:'🛍️ Shopping',other:'📌 Other'};
  const tbody=document.getElementById('expense-table-body');
  if(!expenses.length){
    tbody.innerHTML=`<tr><td colspan="6" style="text-align:center;padding:28px;color:var(--ink-light);">No expenses. <button class="btn-sm btn-sky" onclick="openExpenseModal()">Add one!</button></td></tr>`;
  } else {
    tbody.innerHTML=expenses.slice().reverse().map(e=>{
      const trip=trips.find(t=>t.id===e.tripId);
      return `<tr>
        <td>${fmt(e.date)}</td>
        <td><strong>${e.desc}</strong>${e.notes?`<br><small style="color:var(--ink-light)">${e.notes}</small>`:''}</td>
        <td><span class="cat-badge cat-${e.category}">${catLabels[e.category]||e.category}</span></td>
        <td style="font-size:12px;">${trip?trip.name:'—'}</td>
        <td style="font-weight:700">${fmtNum(e.amount)}</td>
        <td style="white-space:nowrap;"><button class="btn-sm btn-ghost" onclick="openEditExpenseModal('${e.id}')" style="margin-right:4px;">✏️</button><button class="btn-sm btn-danger" onclick="deleteExpense('${e.id}')">✕</button></td>
      </tr>`;
    }).join('');
  }
  const budgetBars=document.getElementById('budget-bars');
  const filteredTrips=tripFilter?trips.filter(t=>t.id===tripFilter):trips.filter(t=>t.budget>0);
  budgetBars.innerHTML=filteredTrips.map(t=>{
    const spent=DB.getList('expenses').filter(e=>e.tripId===t.id).reduce((a,e)=>a+e.amount,0);
    const pct=Math.min(100,Math.round(spent/t.budget*100));
    const barClass=pct>90?'danger':pct>70?'warn':'';
    return `<div style="margin-bottom:14px;">
      <div style="display:flex;justify-content:space-between;font-size:12px;margin-bottom:4px;"><span>${t.emoji||'✈️'} ${t.name}</span><span style="font-weight:600">${fmtNum(spent)} / ${fmtNum(t.budget)} (${pct}%)</span></div>
      <div class="progress-bar"><div class="progress-fill ${barClass}" style="width:${pct}%"></div></div>
    </div>`;
  }).join('')||'<p style="color:var(--ink-light);font-size:13px;">Set budgets on trips to see them here.</p>';
}
function deleteExpense(id){DB.deleteItem('expenses',id);loadExpenses();showToast('Expense removed.');}
function exportExpensesCSV(){
  const expenses=DB.getList('expenses');
  const trips=DB.getList('trips');
  if(!expenses.length){showToast('No expenses to export.');return;}
  const rows=[['Date','Description','Category','Trip','Amount','Notes']];
  expenses.forEach(e=>{
    const trip=trips.find(t=>t.id===e.tripId);
    rows.push([e.date,e.desc,e.category,trip?trip.name:'',e.amount,e.notes||'']);
  });
  const csv=rows.map(r=>r.map(c=>'"'+String(c).replace(/"/g,'""')+'"').join(',')).join('\n');
  const blob=new Blob([csv],{type:'text/csv'});
  const a=document.createElement('a');
  a.href=URL.createObjectURL(blob);
  a.download='triptrace-expenses.csv';
  a.click();
  showToast('CSV exported! ⬇️');
}

// ═══════ MAP ═══════
let leafletMap=null,mapMarkers=[];
let plannedRouteLine=null;
let plannedRouteMarkers=[];
let routeMarkers=[],routeLine=null,mapRoutePoints=[],mapRouteMode=false,currentMapFilter='all';
let liveTrackWatchId=null;
const mapFilterLabels={all:'All places',hotels:'Hotels',restaurants:'Restaurants',viewpoints:'Viewpoints',transport:'Transport'};
function openWaypointModal(){populateTripSelect('wp-trip');openModal('modal-waypoint');}
function saveWaypoint(){
  const name=document.getElementById('wp-name').value.trim();
  if(!name){showToast('Enter a place name.');return;}
  DB.pushItem('waypoints',{
    id:uid(),tripId:document.getElementById('wp-trip').value,
    name,type:document.getElementById('wp-type').value,
    lat:+document.getElementById('wp-lat').value||null,
    lng:+document.getElementById('wp-lng').value||null,
    dist:+document.getElementById('wp-dist').value||0,
    altitude:+document.getElementById('wp-alt').value||0,
    notes:document.getElementById('wp-notes').value
  });
  closeModal('modal-waypoint');showToast('Waypoint saved! 📍');
  ['wp-name','wp-notes','wp-lat','wp-lng'].forEach(id=>{document.getElementById(id).value='';});
  loadMap();
}
function deleteWaypoint(id){DB.deleteItem('waypoints',id);loadMapWaypoints();updateMapDetails();showToast('Removed.');}
function clearMapSelection(){
  const sel=document.getElementById('map-trip-select');
  if(sel)sel.value='';
  currentMapFilter='all';
  mapRouteMode=false;
  mapRoutePoints=[];
  clearRouteMarkers();
  loadMapWaypoints();
  updateMapDetails();
  updateRouteButton();
  
  showToast('Map selection cleared.');
}
function updateRouteButton(){
  const btn=document.getElementById('route-plan-btn');
  if(!btn)return;
  if(mapRouteMode){
    btn.textContent='⛔ Stop Route Mode';
    btn.classList.remove('btn-primary');
    btn.classList.add('btn-danger');
  } else {
    btn.textContent='🧭 Plan Route';
    btn.classList.remove('btn-danger');
    btn.classList.add('btn-primary');
  }
}
function setMapFilter(type){
  currentMapFilter=type;
  document.querySelectorAll('#page-map .form-section button').forEach(btn=>btn.classList.remove('active'));
  const activeBtn=document.querySelector(`#page-map button[onclick="setMapFilter('${type}')"]`);
  if(activeBtn)activeBtn.classList.add('active');
  loadMapWaypoints();
  updateMapDetails();
  showToast(`${mapFilterLabels[type]||'Filter'} selected.`);
}
function showMapSearch(){
  const input=document.getElementById('map-search-query');
  if(input)input.value='';
  const results=document.getElementById('map-search-results');
  if(results)results.innerHTML='Search for places by name, address or landmark.';
  openModal('modal-search');
}
async function searchMapPlace(){
  const query=document.getElementById('map-search-query').value.trim();
  const results=document.getElementById('map-search-results');
  if(!query){showToast('Enter a search query.');return;}
  if(results)results.innerHTML='<div style="font-size:13px;color:var(--ink-mid);">Searching…</div>';
  try{
    const url='https://nominatim.openstreetmap.org/search?format=json&limit=8&q='+encodeURIComponent(query);
    const res=await fetch(url,{headers:{'Accept-Language':'en'}});
    if(!res.ok)throw new Error('Search failed');
    const data=await res.json();
    if(!Array.isArray(data)||!data.length){
      if(results)results.innerHTML='<div style="font-size:13px;color:var(--ink-mid);">No locations found.</div>';
      return;
    }
    window.__mapSearchResults=data;
    if(results){
      results.innerHTML='';
      data.forEach(item=>{
        const card=document.createElement('div');
        card.style.marginBottom='10px';
        card.style.padding='10px';
        card.style.border='1px solid var(--border)';
        card.style.borderRadius='12px';
        card.style.background='var(--surface)';
        card.style.display='grid';
        card.style.gridTemplateColumns='1fr auto';
        card.style.gap='10px';
        const info=document.createElement('div');
        const title=document.createElement('div');
        title.textContent=(item.display_name||'Place').split(',')[0];
        title.style.fontWeight='700';
        title.style.fontSize='14px';
        const meta=document.createElement('div');
        meta.textContent=(item.type||'place')+' · '+(item.display_name||'');
        meta.style.fontSize='12px';
        meta.style.color='var(--ink-mid)';
        info.appendChild(title);
        info.appendChild(meta);
        const actions=document.createElement('div');
        actions.style.display='flex';
        actions.style.flexDirection='column';
        actions.style.gap='6px';
        const viewBtn=document.createElement('button');
        viewBtn.className='btn-sm btn-ghost';
        viewBtn.textContent='View';
        viewBtn.onclick=()=>centerMapSearchResult(item);
        const saveBtn=document.createElement('button');
        saveBtn.className='btn-sm btn-sky';
        saveBtn.textContent='Save';
        saveBtn.onclick=()=>saveSearchResult(item);
        actions.appendChild(viewBtn);
        actions.appendChild(saveBtn);
        card.appendChild(info);
        card.appendChild(actions);
        results.appendChild(card);
      });
    }
  }catch(e){
    if(results)results.innerHTML=`<div style="font-size:13px;color:var(--danger);">Search failed: ${e.message}</div>`;
    showToast('Map search failed.');
  }
}
function centerMapSearchResult(item){
  if(!item||!item.lat||!item.lon){showToast('Invalid place result.');return;}
  initLeafletMap();
  const lat=+item.lat, lon=+item.lon;
  leafletMap.setView([lat,lon],16);
  const icon=L.divIcon({html:'<div style="font-size:22px;">📍</div>',className:'',iconSize:[28,28]});
  const marker=L.marker([lat,lon],{icon}).addTo(leafletMap).bindPopup(item.display_name||item.type||'Point').openPopup();
  mapMarkers.push(marker);
}
function saveSearchResult(item){
  if(!item||!item.lat||!item.lon){showToast('Invalid place result.');return;}
  const tripId=document.getElementById('map-trip-select').value||DB.getList('trips')[0]?.id;
  if(!tripId){showToast('Create or select a trip first.');return;}
  const type=item.type==='hotel'?'hotel':item.type==='restaurant'||item.type==='cafe'||item.type==='bar'||item.type==='pub'||item.type==='bakery'?'restaurant':'transport';
  DB.pushItem('places',{
    id:uid(),tripId,type,name:(item.display_name||'Saved place').split(',')[0],
    location:item.display_name,rating:4,cost:0,date:today(),notes:'Saved from map search',lat:+item.lat,lng:+item.lon
  });
  showToast('Place saved to your trip.');
  loadMapWaypoints();
  updateMapDetails();
}
function clearRouteMarkers(){
  routeMarkers.forEach(m=>{try{m.remove();}catch(e){}});
  routeMarkers=[];
  if(routeLine){try{routeLine.remove();}catch(e){};routeLine=null;}
}
function focusRoutePlanner(){
  const fromInput=document.getElementById('route-from');
  if(fromInput){
    fromInput.focus();
    fromInput.scrollIntoView({behavior:'smooth',block:'center'});
  }
}

function swapRoutePlaces(){
  const from=document.getElementById('route-from');
  const to=document.getElementById('route-to');

  const temp=from.value;
  from.value=to.value;
  to.value=temp;
}

async function getRouteFromInputs(){
  const fromName=document.getElementById('route-from').value.trim();
  const toName=document.getElementById('route-to').value.trim();
  const summary=document.getElementById('route-summary');

  if(!fromName || !toName){
    showToast('Enter both From and To locations.');
    return;
  }

  summary.style.display='block';
  summary.innerHTML='🔎 Finding locations and calculating the best route...';

  try{
    const [fromPlace,toPlace]=await Promise.all([
      geocodeRoutePlace(fromName),
      geocodeRoutePlace(toName)
    ]);

    if(!fromPlace || !toPlace){
      summary.innerHTML='❌ One or both locations were not found. Try a more specific name.';
      return;
    }

    await drawRoadRoute(fromPlace,toPlace);

  }catch(error){
    console.error(error);
    summary.innerHTML='❌ Route could not be calculated. Please try again.';
    showToast('Route calculation failed.');
  }
}

async function geocodeRoutePlace(placeName){
  const url='https://nominatim.openstreetmap.org/search?format=json&limit=1&q='+encodeURIComponent(placeName);

  const response=await fetch(url,{
    headers:{'Accept-Language':'en'}
  });

  if(!response.ok) throw new Error('Location search failed');

  const data=await response.json();

  if(!data || !data.length) return null;

  return {
    name:data[0].display_name,
    lat:parseFloat(data[0].lat),
    lng:parseFloat(data[0].lon)
  };
}

function clearPlannedRoute(){
  if(plannedRouteLine){
    try{ plannedRouteLine.remove(); }catch(e){}
    plannedRouteLine=null;
  }

  plannedRouteMarkers.forEach(marker=>{
    try{ marker.remove(); }catch(e){}
  });

  plannedRouteMarkers=[];
}

async function drawRoadRoute(fromPlace,toPlace){
  clearPlannedRoute();

  const summary=document.getElementById('route-summary');

  const routeURL=
    'https://router.project-osrm.org/route/v1/driving/' +
    fromPlace.lng + ',' + fromPlace.lat + ';' +
    toPlace.lng + ',' + toPlace.lat +
    '?overview=full&geometries=geojson';

  const response=await fetch(routeURL);

  if(!response.ok) throw new Error('Road route request failed');

  const data=await response.json();

  if(!data.routes || !data.routes.length){
    throw new Error('No route found');
  }

  const route=data.routes[0];

  const latLngs=route.geometry.coordinates.map(point=>[
    point[1],
    point[0]
  ]);

  plannedRouteLine=L.polyline(latLngs,{
    color:'#5B4BFF',
    weight:6,
    opacity:0.9
  }).addTo(leafletMap);

  const startMarker=L.marker([fromPlace.lat,fromPlace.lng])
    .addTo(leafletMap)
    .bindPopup('<b>🟢 From</b><br>'+fromPlace.name);

  const endMarker=L.marker([toPlace.lat,toPlace.lng])
    .addTo(leafletMap)
    .bindPopup('<b>🔴 To</b><br>'+toPlace.name);

  plannedRouteMarkers.push(startMarker,endMarker);

  leafletMap.fitBounds(plannedRouteLine.getBounds(),{
    padding:[40,40]
  });

  const distanceKm=(route.distance/1000).toFixed(1);
  const durationMinutes=Math.round(route.duration/60);
  const hours=Math.floor(durationMinutes/60);
  const minutes=durationMinutes%60;

  const durationText=hours
    ? `${hours} hr ${minutes} min`
    : `${minutes} min`;

  summary.innerHTML=`
    <strong>🛣️ Route Ready</strong><br>
    <span style="color:var(--ink-mid);">
      ${fromPlace.name.split(',')[0]} → ${toPlace.name.split(',')[0]}
    </span>
    <br>
    <strong>${distanceKm} km</strong> · Estimated driving time: <strong>${durationText}</strong>
  `;

  updateMapDetails();
  showToast('Route created successfully! 🗺️');
}
function toggleRouteMode(){
  if(!leafletMap){initLeafletMap();}
  mapRouteMode=!mapRouteMode;
  updateRouteButton();
  if(mapRouteMode){
    showToast('Route mode active. Click map to add route points.');
    leafletMap.on('click',mapRouteClickHandler);
  } else {
    showToast('Route mode stopped.');
    leafletMap.off('click',mapRouteClickHandler);
  }
  updateMapDetails();
}
function mapRouteClickHandler(e){
  if(!mapRouteMode||!e?.latlng)return;
  const latlng=e.latlng;
  mapRoutePoints.push([latlng.lat,latlng.lng]);
  const icon=L.divIcon({html:`<div style="font-size:16px;">${mapRoutePoints.length}</div>`,className:'',iconSize:[24,24],iconAnchor:[12,12]});
  const marker=L.marker([latlng.lat,latlng.lng],{icon}).addTo(leafletMap);
  routeMarkers.push(marker);
  if(routeLine){routeLine.remove();}
  if(mapRoutePoints.length>1){
    routeLine=L.polyline(mapRoutePoints,{color:'#ff6b00',weight:4,opacity:0.8}).addTo(leafletMap);
  }
  updateMapDetails();
  showToast(`Route point ${mapRoutePoints.length} added.`);
}
function updateMapDetails(){
  const details=document.getElementById('map-details');
  if(!details)return;
  const tripId=document.getElementById('map-trip-select').value;
  const trip=DB.getList('trips').find(t=>t.id===tripId);
  const tripLabel=trip?`${trip.emoji||'✈️'} ${trip.name}`:'All trips';
  const filterLabel=mapFilterLabels[currentMapFilter]||'All places';
  const places=DB.getList('places').filter(p=>p.lat&&p.lng&&(!tripId||p.tripId===tripId));
  const waypoints=DB.getList('waypoints').filter(w=>w.lat&&w.lng&&(!tripId||w.tripId===tripId));
  const filteredPlaces=currentMapFilter==='all'?places:places.filter(p=>p.type=== (currentMapFilter==='viewpoints'?'viewpoint':currentMapFilter));
  const filteredWaypoints=currentMapFilter==='viewpoints'?waypoints.filter(w=>w.type==='viewpoint'):waypoints;
  let summary=`<strong>${tripLabel}</strong> · ${filterLabel}. `;
  summary+=`Showing ${filteredWaypoints.length} waypoint${filteredWaypoints.length===1?'':'s'} and ${filteredPlaces.length} saved place${filteredPlaces.length===1?'':'s'}.`;
  if(mapRoutePoints.length){
    const dist=mapRoutePoints.reduce((sum,point,index)=>index?sum+distanceBetween(mapRoutePoints[index-1][0],mapRoutePoints[index-1][1],point[0],point[1]):0,0);
    summary+=`<br>Route points: ${mapRoutePoints.length} · route distance ${dist.toFixed(1)} km.`;
  }
  if(mapRouteMode)summary+=`<br><em>Route mode is active. Click map to add markers.</em>`;
  details.innerHTML=summary;
}
let mapTileLayers={},currentMapLayer='streets';
function initLeafletMap(){
  if(leafletMap)return;
  leafletMap=L.map('leaflet-map').setView([20.5937,78.9629],5);
  mapTileLayers.streets=L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{attribution:'© OpenStreetMap',maxZoom:18});
  mapTileLayers.satellite=L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}',{attribution:'© Esri',maxZoom:18});
  mapTileLayers.terrain=L.tileLayer('https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png',{attribution:'© OpenTopoMap',maxZoom:17});
  mapTileLayers.streets.addTo(leafletMap);
}
function setMapLayer(layer){
  if(!leafletMap||!mapTileLayers[layer])return;
  Object.values(mapTileLayers).forEach(l=>{if(leafletMap.hasLayer(l))leafletMap.removeLayer(l);});
  mapTileLayers[layer].addTo(leafletMap);
  currentMapLayer=layer;
  document.querySelectorAll('.map-layer-btn').forEach(b=>b.classList.remove('active'));
  const btn=document.getElementById('layer-btn-'+layer);
  if(btn)btn.classList.add('active');
}
function loadMap(){
  initLeafletMap();
  const trips=DB.getList('trips');
  const sel=document.getElementById('map-trip-select');
  sel.innerHTML='<option value="">All Trips</option>'+trips.map(t=>`<option value="${t.id}">${t.emoji||'✈️'} ${t.name}</option>`).join('');
  currentMapFilter='all';
  mapRouteMode=false;
  mapRoutePoints=[];
  updateRouteButton();
  const allBtn=document.querySelector(`#page-map button[onclick="setMapFilter('all')"]`);
  document.querySelectorAll('#page-map .form-section button').forEach(btn=>btn.classList.remove('active'));
  if(allBtn)allBtn.classList.add('active');
  loadMapWaypoints();
  updateMapDetails();
  setTimeout(()=>leafletMap.invalidateSize(),300);
}
function loadMapWaypoints(){
  if(!leafletMap)return;
  const tripId=document.getElementById('map-trip-select').value;
  const waypoints=DB.getList('waypoints').filter(w=>w.lat&&w.lng&&(!tripId||w.tripId===tripId));
  const allPlaces=DB.getList('places').filter(p=>p.lat&&p.lng&&(!tripId||p.tripId===tripId));
  let displayedWaypoints=waypoints;
  let displayedPlaces=allPlaces;
  if(currentMapFilter==='viewpoints'){
    displayedWaypoints=waypoints.filter(w=>w.type==='viewpoint');
  } else if(currentMapFilter==='hotels'){
    displayedPlaces=allPlaces.filter(p=>p.type==='hotel');
  } else if(currentMapFilter==='restaurants'){
    displayedPlaces=allPlaces.filter(p=>p.type==='restaurant');
  } else if(currentMapFilter==='transport'){
    displayedPlaces=allPlaces.filter(p=>p.type==='transport');
  }
  mapMarkers.forEach(m=>{try{m.remove();}catch(e){}});
  mapMarkers=[];
  const placeIcons={hotel:'🏨',restaurant:'🍽️',transport:'🚌'};
  const wpIcons={start:'🟢',waypoint:'📍',viewpoint:'🏔️',end:'🔴'};
  const latLngs=[];
  displayedPlaces.forEach(p=>{
    const icon=L.divIcon({html:`<div style="font-size:20px;line-height:1;">${placeIcons[p.type]||'📍'}</div>`,className:'',iconSize:[28,28],iconAnchor:[14,14]});
    const marker=L.marker([p.lat,p.lng],{icon}).addTo(leafletMap).bindPopup(`<b>${p.name}</b><br>${p.location||p.type}<br>${p.notes||''}`);
    mapMarkers.push(marker);
    latLngs.push([p.lat,p.lng]);
  });
  displayedWaypoints.forEach(w=>{
    const icon=L.divIcon({html:`<div style="font-size:22px;line-height:1;">${wpIcons[w.type]||'📍'}</div>`,className:'',iconSize:[28,28],iconAnchor:[14,14]});
    const marker=L.marker([w.lat,w.lng],{icon}).addTo(leafletMap).bindPopup(`<b>${w.name}</b>${w.notes?'<br>'+w.notes:''}${w.altitude?'<br>⛰️ '+w.altitude+'m':''}`);
    mapMarkers.push(marker);
    latLngs.push([w.lat,w.lng]);
  });
  if(latLngs.length>1){
    const bounds=L.latLngBounds(latLngs);
    leafletMap.fitBounds(bounds,{padding:[30,30]});
  } else if(latLngs.length===1){
    leafletMap.setView(latLngs[0],12);
  }
  updateMapDetails();
}
function locateMe(){
  initLeafletMap();
  if(!window.isSecureContext){
    showToast('Location is blocked because this page isn\'t loaded over https:// (or localhost).');
    return;
  }
  if(!navigator.geolocation){
    showToast('Location is not supported on this device.');
    return;
  }
  showToast('📡 Getting your location…');
  // enableHighAccuracy + maximumAge:0 asks the device/browser for the best fix it
  // can get (GPS chip, or a fresh WiFi/cell triangulation) instead of a quick, coarse
  // IP-based estimate — IP geolocation is the usual cause of the marker landing far
  // from your real position (sometimes an entire city or state away).
  navigator.geolocation.getCurrentPosition(async pos=>{
    const latlng=[pos.coords.latitude,pos.coords.longitude];
    const accuracy=pos.coords.accuracy||0;
    leafletMap.setView(latlng,accuracy>5000?11:14);
    if(window._meMarker){try{window._meMarker.remove();}catch(e){}}
    if(window._meAccuracyCircle){try{window._meAccuracyCircle.remove();}catch(e){}}
    let label='You are here';
    try{
      const revResponse=await fetch(`https://nominatim.openstreetmap.org/reverse?format=json&lat=${latlng[0]}&lon=${latlng[1]}&zoom=12`);
      if(revResponse.ok){
        const revData=await revResponse.json();
        const a=revData.address||{};
        const place=a.city||a.town||a.village||a.suburb||a.county||a.state_district;
        if(place)label='Near '+place;
      }
    }catch(e){}
    const icon=L.divIcon({html:'<div style="font-size:22px;">🔵</div>',className:'',iconSize:[28,28]});
    window._meMarker=L.marker(latlng,{icon}).addTo(leafletMap)
      .bindPopup(`📍 ${label}<br><span style="font-size:11px;color:#666;">Accuracy: ±${Math.round(accuracy)}m</span>`).openPopup();
    // Accuracy circle makes it visible on the map when the fix is only approximate,
    // instead of the single pin looking simply "wrong".
    window._meAccuracyCircle=L.circle(latlng,{radius:accuracy,color:'#3388ff',weight:1,fillOpacity:.08}).addTo(leafletMap);
    if(accuracy>5000){
      showToast(`Location found, but it's only accurate to ±${(accuracy/1000).toFixed(1)}km (common on desktops without GPS/WiFi positioning). Use "🔎 Search Place" for an exact pin.`,5000);
    }else{
      showToast('Location found! 📡');
    }
  },err=>{
    let reason='Could not get location.';
    if(err.code===err.PERMISSION_DENIED)reason='Location access was denied for this site. Check your browser\'s address-bar site settings.';
    else if(err.code===err.POSITION_UNAVAILABLE)reason='Your device could not determine a location.';
    else if(err.code===err.TIMEOUT)reason='Getting your location timed out. Try again or use "🔎 Search Place".';
    showToast(reason,4000);
  },{enableHighAccuracy:true,timeout:12000,maximumAge:0});
}
function distanceBetween(lat1,lon1,lat2,lon2){
  const toRad=x=>x*Math.PI/180;
  const R=6371;
  const dLat=toRad(lat2-lat1);
  const dLon=toRad(lon2-lon1);
  const a=Math.sin(dLat/2)**2+Math.cos(toRad(lat1))*Math.cos(toRad(lat2))*Math.sin(dLon/2)**2;
  return R*2*Math.atan2(Math.sqrt(a),Math.sqrt(1-a));
}
function toggleLiveTracking(){
  const statusEl=document.getElementById('live-track-status');
  const btn=document.getElementById('live-track-btn');
  if(liveTrackWatchId!==null){
    navigator.geolocation.clearWatch(liveTrackWatchId);
    liveTrackWatchId=null;
    statusEl.textContent='Live GPS tracking is off.';
    btn.textContent='🚦 Start Live Track';
    btn.classList.remove('btn-danger');
    btn.classList.add('btn-terra');
    showToast('Stopped live tracking.');
    return;
  }
  if(!navigator.geolocation){
    showToast('GPS is not supported by your browser.');
    return;
  }
  initLeafletMap();
  liveTrackWatchId=navigator.geolocation.watchPosition(pos=>{
    const latlng=[pos.coords.latitude,pos.coords.longitude];
    const icon=L.divIcon({html:'<div style="font-size:22px;">🔵</div>',className:'',iconSize:[28,28]});
    L.marker(latlng,{icon}).addTo(leafletMap).bindPopup('📍 Live location').openPopup();
    leafletMap.setView(latlng,14);
    statusEl.textContent=`Live GPS tracking active — ${latlng[0].toFixed(5)}, ${latlng[1].toFixed(5)}`;
    btn.textContent='⛔ Stop Live Track';
    btn.classList.remove('btn-terra');
    btn.classList.add('btn-danger');
  },err=>{
    showToast('Live tracking error: '+err.message);
  },{enableHighAccuracy:true,maximumAge:5000,timeout:10000});
}
function fillMyLocation(){
  navigator.geolocation.getCurrentPosition(pos=>{
    document.getElementById('wp-lat').value=pos.coords.latitude.toFixed(5);
    document.getElementById('wp-lng').value=pos.coords.longitude.toFixed(5);
    showToast('Location filled! 📡');
  },()=>showToast('GPS not available.'));
}

// ═══════ AUTOMATIC TRIP TRACKING ═══════
let autoTripActive=null, autoTripWatchId=null, autoTripWaypoints=[], autoTripStartTime=null, autoTripStartPos=null;
function startAutoTrip(){
  const name=document.getElementById('trip-track-name').value.trim();
  const dest=document.getElementById('trip-track-dest').value.trim();
  const budget=+document.getElementById('trip-track-budget').value||5000;
  const style=document.getElementById('trip-track-style').value;
  if(!name||!dest){showToast('Enter trip name and destination.');return;}
  const trip={
    id:uid(),
    name,
    start:new Date().toISOString().split('T')[0],
    end:'',
    destination:dest,
    budget,
    status:'ongoing',
    style,
    emoji:'✈️',
    notes:'Auto-tracked trip',
    created:new Date().toISOString(),
    routeWaypoints:[],
    totalDistance:0,
    duration:0,
    averageSpeed:0,
    stops:0,
    maxSpeed:0
  };
  DB.pushItem('trips',trip);
  autoTripActive=trip;
  autoTripWaypoints=[];
  autoTripStartTime=Date.now();
  closeModal('modal-start-trip');
  showToast('🟢 Trip tracking started!');
  ['trip-track-name','trip-track-dest','trip-track-budget'].forEach(id=>{document.getElementById(id).value='';});
  startGPSTracking();
  updateAutoTripUI();
}
function startGPSTracking(){
  if(!navigator.geolocation){showToast('GPS not available.');return;}
  let lastPos=null;
  autoTripWatchId=navigator.geolocation.watchPosition(pos=>{
    const lat=pos.coords.latitude, lon=pos.coords.longitude, speed=pos.coords.speed||0, timestamp=Date.now();
    const point={lat,lon,speed:speed||0,timestamp,accuracy:pos.coords.accuracy};
    autoTripWaypoints.push(point);
    if(lastPos){
      const dist=distanceBetween(lastPos.lat,lastPos.lon,lat,lon);
      if(autoTripActive){
        autoTripActive.totalDistance=(autoTripActive.totalDistance||0)+dist;
        autoTripActive.maxSpeed=Math.max(autoTripActive.maxSpeed||0,speed||0);
        if(speed<1&&autoTripActive.stops!==undefined)autoTripActive.stops++;
      }
    }
    lastPos={lat,lon};
    if(!autoTripStartPos)autoTripStartPos={lat,lon};
    updateAutoTripUI();
  },err=>{showToast('GPS Error: '+err.message);},{enableHighAccuracy:true,maximumAge:3000,timeout:10000});
}
function updateAutoTripUI(){
  if(!autoTripActive)return;
  const btn=document.getElementById('auto-trip-btn');
  const statusEl=document.getElementById('live-track-status');
  if(!btn)return;
  const elapsed=Math.round((Date.now()-autoTripStartTime)/1000);
  const hours=Math.floor(elapsed/3600), mins=Math.floor((elapsed%3600)/60), secs=elapsed%60;
  const timeStr=`${hours}h ${mins}m ${secs}s`;
  const speed=(autoTripActive.maxSpeed*3.6).toFixed(1);
  btn.textContent='⛔ Stop Trip';
  btn.classList.remove('btn-primary');
  btn.classList.add('btn-danger');
  statusEl.innerHTML=`<strong>🟢 Trip Active: ${autoTripActive.name}</strong><br>⏱️ ${timeStr} | 🛣️ ${autoTripActive.totalDistance.toFixed(1)}km | ⚡ ${speed}km/h | 📍 ${autoTripWaypoints.length} points`;
}
function endAutoTrip(){
  if(!autoTripActive){showToast('No active trip.');return;}
  if(autoTripWatchId!==null){
    navigator.geolocation.clearWatch(autoTripWatchId);
    autoTripWatchId=null;
  }
  const elapsed=Math.round((Date.now()-autoTripStartTime)/1000);
  autoTripActive.duration=elapsed;
  autoTripActive.end=new Date().toISOString().split('T')[0];
  autoTripActive.status='completed';
  autoTripActive.averageSpeed=autoTripWaypoints.length>0?(autoTripActive.totalDistance*1000)/(elapsed||1):0;
  autoTripActive.routeWaypoints=autoTripWaypoints;
  DB.set('trips',DB.getList('trips').map(t=>t.id===autoTripActive.id?autoTripActive:t));
  showTripSummary(autoTripActive);
  const btn=document.getElementById('auto-trip-btn');
  if(btn){
    btn.textContent='🟢 Start Trip';
    btn.classList.remove('btn-danger');
    btn.classList.add('btn-primary');
  }
  autoTripActive=null;
  autoTripWaypoints=[];
  autoTripStartTime=null;
}
function showTripSummary(trip){
  const hours=Math.floor(trip.duration/3600), mins=Math.floor((trip.duration%3600)/60);
  const summary=`
    <div class="form-section">
      <h4 style="text-align:center;font-size:16px;margin-bottom:16px;">✈️ ${trip.name}</h4>
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:12px;">
        <div class="stat-card" style="padding:12px;background:var(--sky-light);border-radius:8px;text-align:center;">
          <div style="font-size:11px;color:var(--ink-mid);margin-bottom:4px;">Duration</div>
          <div style="font-size:18px;font-weight:700;">${hours}h ${mins}m</div>
        </div>
        <div class="stat-card" style="padding:12px;background:var(--sky-light);border-radius:8px;text-align:center;">
          <div style="font-size:11px;color:var(--ink-mid);margin-bottom:4px;">Distance</div>
          <div style="font-size:18px;font-weight:700;">${trip.totalDistance.toFixed(1)} km</div>
        </div>
        <div class="stat-card" style="padding:12px;background:var(--sky-light);border-radius:8px;text-align:center;">
          <div style="font-size:11px;color:var(--ink-mid);margin-bottom:4px;">Max Speed</div>
          <div style="font-size:18px;font-weight:700;">${(trip.maxSpeed*3.6).toFixed(1)} km/h</div>
        </div>
        <div class="stat-card" style="padding:12px;background:var(--sky-light);border-radius:8px;text-align:center;">
          <div style="font-size:11px;color:var(--ink-mid);margin-bottom:4px;">GPS Points</div>
          <div style="font-size:18px;font-weight:700;">${trip.routeWaypoints.length}</div>
        </div>
      </div>
      <div style="background:var(--sand);border-radius:8px;padding:12px;margin-bottom:12px;font-size:13px;">
        <div><strong>📍 From:</strong> ${autoTripStartPos?autoTripStartPos.lat.toFixed(4)+', '+autoTripStartPos.lon.toFixed(4):'Unknown'}</div>
        <div><strong>📍 To:</strong> ${trip.destination}</div>
        <div><strong>💰 Budget:</strong> ₹${trip.budget}</div>
        <div><strong>🎯 Status:</strong> Completed</div>
      </div>
      <div style="border-top:1px solid var(--border);padding-top:12px;">
        <p style="font-size:12px;color:var(--ink-mid);margin-bottom:12px;">Auto-generated summary from GPS tracking. You can now add expenses, places, and photos to this trip.</p>
      </div>
    </div>`;
  document.getElementById('trip-summary-body').innerHTML=summary;
  openModal('modal-trip-summary');
}
function saveSummaryAndContinue(){
  closeModal('modal-trip-summary');
  showToast('✅ Trip saved! View it in Trips section.');
  loadTrips();
}
function toggleAutoTrip(){
  if(autoTripActive){endAutoTrip();}else{openModal('modal-start-trip');}
}

// ═══════ PACKING CHECKLIST ═══════
let currentPackingChecklist=null;
function savePackingToStorage(){
  if(currentPackingChecklist){
    try{
      localStorage.setItem(
        userStorageKey('packing'),
        JSON.stringify(currentPackingChecklist)
      );
    }catch(e){}
  }
}
function loadPackingFromStorage(){
  try{
    const saved=localStorage.getItem(userStorageKey('packing'));
    if(saved)currentPackingChecklist=JSON.parse(saved);
  }catch(e){}
}
async function generatePackingChecklist(){
  openModal('modal-packing');
  loadPackingFromStorage();
  if(currentPackingChecklist){
    document.getElementById('packing-loading').style.display='none';
    document.getElementById('packing-checklist').style.display='block';
    const s=document.getElementById('packing-stats');
    if(s)s.innerHTML=`<div style="padding:12px;background:var(--sky-light);border-radius:8px;grid-column:1/-1;font-size:13px;">✅ Restored your saved checklist for <strong>${currentPackingChecklist.trip?.name||'trip'}</strong>. <button class="btn-sm btn-ghost" style="margin-left:8px;" onclick="currentPackingChecklist=null;localStorage.removeItem(userStorageKey('packing'));generatePackingChecklist();">Start Fresh</button></div>`;
    renderPackingChecklist();
    return;
  }
  document.getElementById('packing-checklist').style.display='none';
  document.getElementById('packing-loading').style.display='block';
  const trips=DB.getList('trips').filter(t=>t.status==='ongoing'||t.status==='planned');
  const trip=trips[trips.length-1];
  if(!trip){
    document.getElementById('packing-loading').innerHTML='<p style="color:var(--ink-mid);">No active or planned trips. Create one first!</p>';
    return;
  }
  const days=tripDays(trip.start,trip.end);
  const weather=trip.destination?await getWeatherForPacking(trip.destination):{temp:20,condition:'mild'};
  const baseItems={
    essential:[
      {item:'Passport/ID',checked:false,why:'Travel document'},
      {item:'Travel tickets/bookings',checked:false,why:'Flight/train confirmations'},
      {item:'Hotel reservations',checked:false,why:'Accommodation proof'},
      {item:'Insurance documents',checked:false,why:'Travel insurance'},
      {item:'Cash & credit cards',checked:false,why:'Payment methods'},
      {item:'Phone & charger',checked:false,why:'Communication'},
      {item:'Medications',checked:false,why:'Personal health'},
      {item:'Eyeglasses (if needed)',checked:false,why:'Vision correction'},
    ],
    clothing:[],
    toiletries:[
      {item:'Toothbrush & toothpaste',checked:false,why:'Daily hygiene'},
      {item:'Soap/body wash',checked:false,why:'Bathing'},
      {item:'Shampoo & conditioner',checked:false,why:'Hair care'},
      {item:'Deodorant',checked:false,why:'Personal care'},
      {item:'Sunscreen',checked:false,why:'UV protection'},
      {item:'Face wash & moisturizer',checked:false,why:'Skincare'},
      {item:'Nail clippers',checked:false,why:'Personal grooming'},
      {item:'Tissues/handkerchief',checked:false,why:'Hygiene'},
    ],
    electronics:[
      {item:'Camera/phone for photos',checked:false,why:'Memory keeping'},
      {item:'Power bank',checked:false,why:'Device charging'},
      {item:'Headphones',checked:false,why:'Entertainment'},
      {item:'Adapter/converter',checked:false,why:'Electrical compatibility'},
      {item:'Portable charger',checked:false,why:'Emergency power'},
    ],
    weather:[]
  };
  
  // Add weather-specific items
  if(weather.temp>28){
    baseItems.weather=[
      {item:'Light cotton clothes',checked:false,why:'Hot weather'},
      {item:'Hat/cap',checked:false,why:'Sun protection'},
      {item:'Sunglasses',checked:false,why:'UV protection'},
      {item:'Flip flops/sandals',checked:false,why:'Breathable footwear'},
      {item:'Lightweight scarf',checked:false,why:'Sun cover'},
    ];
    baseItems.clothing=[
      {item:'T-shirts (5-7)',checked:false,why:'Daily wear'},
      {item:'Shorts (2-3)',checked:false,why:'Hot weather'},
      {item:'Lightweight pants (1-2)',checked:false,why:'Evening wear'},
      {item:'Summer dress/shirt',checked:false,why:'Casual outings'},
      {item:'Lightweight jacket',checked:false,why:'Occasional coolness'},
    ];
  } else if(weather.temp<10){
    baseItems.weather=[
      {item:'Warm jacket/coat',checked:false,why:'Cold protection'},
      {item:'Thermal wear',checked:false,why:'Body warmth'},
      {item:'Winter hat',checked:false,why:'Head warmth'},
      {item:'Gloves',checked:false,why:'Hand protection'},
      {item:'Scarf',checked:false,why:'Neck warmth'},
      {item:'Warm socks',checked:false,why:'Feet warmth'},
      {item:'Woolen sweater',checked:false,why:'Layering'},
    ];
    baseItems.clothing=[
      {item:'Long-sleeve shirts (5-7)',checked:false,why:'Warmth'},
      {item:'Jeans/trousers (2-3)',checked:false,why:'Daily wear'},
      {item:'Thermal leggings',checked:false,why:'Extra warmth'},
      {item:'Comfortable shoes',checked:false,why:'Walking'},
    ];
  } else {
    baseItems.clothing=[
      {item:'T-shirts (4-5)',checked:false,why:'Daily wear'},
      {item:'Shirts/blouses (2-3)',checked:false,why:'Casual outings'},
      {item:'Jeans/trousers (2-3)',checked:false,why:'Main wear'},
      {item:'Lightweight sweater',checked:false,why:'Layering'},
      {item:'Casual shoes (2)',checked:false,why:'Walking'},
      {item:'Formal shoes',checked:false,why:'Dining out'},
    ];
  }
  
  // Add trip-specific items based on style
  const tripStyle=trip.style||'midrange';
  if(tripStyle.includes('adventure')){
    baseItems.adventure=[
      {item:'Hiking boots',checked:false,why:'Trekking'},
      {item:'Backpack (40-50L)',checked:false,why:'Carrying gear'},
      {item:'Water bottle',checked:false,why:'Hydration'},
      {item:'Rain jacket',checked:false,why:'Weather protection'},
      {item:'First aid kit',checked:false,why:'Emergency care'},
      {item:'Torch/flashlight',checked:false,why:'Night visibility'},
      {item:'Waterproof bag',checked:false,why:'Gear protection'},
    ];
  }
  
  if(tripStyle.includes('beach')||trip.destination.toLowerCase().includes('goa')||trip.destination.toLowerCase().includes('beach')){
    baseItems.beach=[
      {item:'Swimsuit (2)',checked:false,why:'Swimming'},
      {item:'Beach towel',checked:false,why:'Drying'},
      {item:'Flip flops',checked:false,why:'Beach wear'},
      {item:'Waterproof phone case',checked:false,why:'Phone protection'},
      {item:'Aloe vera gel',checked:false,why:'Sunburn relief'},
      {item:'Light beach cover-up',checked:false,why:'Sun protection'},
    ];
  }
  
  currentPackingChecklist={
    trip,
    days,
    weather,
    items:baseItems
  };
  
  const stats=`
    <div style="padding:12px;background:var(--sky-light);border-radius:8px;">
      <div style="font-size:11px;color:var(--ink-mid);margin-bottom:3px;">📍 Destination</div>
      <div style="font-size:14px;font-weight:600;">${trip.destination}</div>
    </div>
    <div style="padding:12px;background:var(--sky-light);border-radius:8px;">
      <div style="font-size:11px;color:var(--ink-mid);margin-bottom:3px;">⏱️ Duration</div>
      <div style="font-size:14px;font-weight:600;">${days} days</div>
    </div>
    <div style="padding:12px;background:var(--sky-light);border-radius:8px;">
      <div style="font-size:11px;color:var(--ink-mid);margin-bottom:3px;">🌡️ Weather</div>
      <div style="font-size:14px;font-weight:600;">${weather.temp}°C · ${weather.condition}</div>
    </div>
    <div style="padding:12px;background:var(--sky-light);border-radius:8px;">
      <div style="font-size:11px;color:var(--ink-mid);margin-bottom:3px;">🎯 Travel Style</div>
      <div style="font-size:14px;font-weight:600;text-transform:capitalize;">${trip.style||'standard'}</div>
    </div>`;
  document.getElementById('packing-stats').innerHTML=stats;
  document.getElementById('packing-loading').style.display='none';
  document.getElementById('packing-checklist').style.display='block';
  renderPackingChecklist();
}
async function getWeatherForPacking(destination){
  try{
    const destClean=destination.split(',')[0].trim();
    const geoResponse=await fetch(`https://nominatim.openstreetmap.org/search?format=json&q=${encodeURIComponent(destClean+' India')}`);
    if(!geoResponse.ok)return {temp:20,condition:'mild'};
    const geoData=await geoResponse.json();
    if(!geoData.length)return {temp:20,condition:'mild'};
    const wxResponse=await fetch(`https://api.openweathermap.org/data/2.5/weather?lat=${geoData[0].lat}&lon=${geoData[0].lon}&appid=${OWM_API_KEY}&units=metric`);
    if(!wxResponse.ok)return {temp:20,condition:'mild'};
    const data=await wxResponse.json();
    return {
      temp:Math.round(data.main.temp),
      condition:data.weather[0].main
    };
  }catch{
    return {temp:20,condition:'mild'};
  }
}
function renderPackingChecklist(){
  if(!currentPackingChecklist)return;
  const showEssential=document.getElementById('packing-filter-essential').checked;
  const {items}=currentPackingChecklist;
  let html='';
  const categories=[
    {key:'essential',label:'🔑 Essential Documents & Items',color:'#f0f'},
    {key:'clothing',label:'👕 Clothing',color:'#0af'},
    {key:'toiletries',label:'🧴 Toiletries & Personal Care',color:'#8f0'},
    {key:'electronics',label:'⚡ Electronics',color:'#f80'},
    {key:'weather',label:'🌤️ Weather-Specific',color:'#fc0'},
    {key:'beach',label:'🏖️ Beach Essentials',color:'#0cf'},
    {key:'adventure',label:'⛰️ Adventure Gear',color:'#6f0'},
  ];
  
  categories.forEach(cat=>{
    const catItems=items[cat.key]||[];
    if(!catItems.length)return;
    if(showEssential&&cat.key!=='essential')return;
    html+=`<div style="margin-bottom:16px;">
      <div style="font-size:14px;font-weight:600;margin-bottom:10px;padding-bottom:8px;border-bottom:2px solid ${cat.color};">${cat.label}</div>
      <div>`;
    catItems.forEach((item,idx)=>{
      html+=`<label style="display:flex;align-items:center;gap:10px;padding:8px;background:var(--sand);border-radius:6px;margin-bottom:6px;cursor:pointer;">
        <input type="checkbox" onchange="updatePackingItem('${cat.key}',${idx})" style="cursor:pointer;">
        <div style="flex:1;">
          <div style="font-size:13px;font-weight:500;">${item.item}</div>
          <div style="font-size:11px;color:var(--ink-light);">${item.why}</div>
        </div>
      </label>`;
    });
    html+='</div></div>';
  });
  document.getElementById('packing-items').innerHTML=html;
}
function updatePackingItem(category,index){
  if(!currentPackingChecklist)return;
  if(!currentPackingChecklist.items[category]||!currentPackingChecklist.items[category][index])return;
  currentPackingChecklist.items[category][index].checked=!currentPackingChecklist.items[category][index].checked;
  savePackingToStorage();
}
function downloadPackingList(){
  if(!currentPackingChecklist)return;
  const {trip,items,weather,days}=currentPackingChecklist;
  let csv='Packing List for: '+trip.name+'\n';
  csv+='Destination: '+trip.destination+'\n';
  csv+='Duration: '+days+' days\n';
  csv+='Weather: '+weather.temp+'°C · '+weather.condition+'\n';
  csv+='Generated: '+new Date().toLocaleDateString()+'\n\n';
  csv+='Category,Item,Why,Packed\n';
  
  Object.entries(items).forEach(([category,itemList])=>{
    itemList.forEach(item=>{
      csv+=category+','+item.item+','+item.why+','+(item.checked?'✓':'')+'\n';
    });
  });
  
  const blob=new Blob([csv],{type:'text/csv'});
  const url=URL.createObjectURL(blob);
  const a=document.createElement('a');
  a.href=url;
  a.download='packing-list-'+trip.name+'.csv';
  a.click();
  URL.revokeObjectURL(url);
  showToast('Packing list downloaded! 🎒');
}

// ═══════ PLACES ═══════
let currentPlaceTab='hotels';
function openPlaceModal(){
  populateTripSelect('place-trip');
  document.getElementById('place-type').value=currentPlaceTab==='hotels'?'hotel':currentPlaceTab==='restaurants'?'restaurant':'transport';
  document.getElementById('place-date').value=today();
  openModal('modal-place');
}
function updatePlaceModal(){
  const type=document.getElementById('place-type').value;
  const labels={hotel:'Check-in Date',restaurant:'Visit Date',transport:'Departure Date'};
  document.getElementById('place-extra-label').textContent=labels[type]||'Date';
}
function savePlace(){
  const name=document.getElementById('place-name').value.trim();
  if(!name){showToast('Enter a place name.');return;}
  DB.pushItem('places',{id:uid(),tripId:document.getElementById('place-trip').value,type:document.getElementById('place-type').value,name,location:document.getElementById('place-location').value,rating:+document.getElementById('place-rating').value||5,cost:+document.getElementById('place-cost').value||0,date:document.getElementById('place-date').value,notes:document.getElementById('place-notes').value});
  closeModal('modal-place');showToast('Place saved!');
  ['place-name','place-notes'].forEach(id=>{document.getElementById(id).value='';});
  loadPlaces(currentPlaceTab);
}
function switchPlaceTab(tab,btn){
  currentPlaceTab=tab;
  document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
  btn.classList.add('active');
  loadPlaces(tab);
}
function loadPlaces(tab){
  currentPlaceTab=tab;
  const el=document.getElementById('places-content');
  if(tab==='route'){
    renderRouteList(el);
    return;
  }
  const typeMap={hotels:'hotel',restaurants:'restaurant',transport:'transport'};
  const type=typeMap[tab]||'hotel';
  const places=DB.getList('places').filter(p=>p.type===type);
  const trips=DB.getList('trips');
  const icons={hotel:'🏨',restaurant:'🍽️',transport:'🚌'};
  const stars=n=>'⭐'.repeat(n||5);
  if(!places.length){
    el.innerHTML=`<div class="empty"><div class="icon">${icons[type]}</div><h3>No ${tab} logged</h3><p>Add places you visited.</p><button class="btn-primary btn-lg" onclick="openPlaceModal()">+ Add</button></div>`;
    return;
  }
  el.innerHTML=places.slice().reverse().map(p=>{
    const trip=trips.find(t=>t.id===p.tripId);
    return `<div class="place-card">
      <div class="place-icon">${icons[p.type]||'📍'}</div>
      <div class="place-info">
        <div class="place-name">${p.name}</div>
        <div class="place-meta">📍 ${p.location||'—'} ${trip?'· ✈️ '+trip.name:''} · 📅 ${fmt(p.date)}</div>
        <div class="stars">${stars(p.rating)}</div>
        ${p.notes?`<div style="font-size:12px;color:var(--ink-mid);margin-top:3px;">${p.notes}</div>`:''}
      </div>
      <div class="place-actions">
        <div class="place-cost">${fmtNum(p.cost)}</div>
        <button class="btn-sm btn-danger" onclick="deletePlace('${p.id}')" style="margin-top:4px;">✕</button>
      </div>
    </div>`;
  }).join('');
}
function renderRouteList(el){
  const trips=DB.getList('trips');
  const wpIcons={start:'🟢',waypoint:'📍',viewpoint:'🏔️',end:'🔴'};
  const placeIcons={hotel:'🏨',restaurant:'🍽️',transport:'🚌'};
  const selectHtml=`<select id="route-trip-select" onchange="renderRouteList(document.getElementById('places-content'))" style="width:100%;padding:8px 12px;border:1.5px solid var(--border);border-radius:8px;font-family:inherit;font-size:13px;margin-bottom:14px;">
    <option value="">All Trips</option>${trips.map(t=>`<option value="${t.id}">${t.emoji||'✈️'} ${t.name}</option>`).join('')}
  </select>`;
  const tripId=document.getElementById('route-trip-select')?document.getElementById('route-trip-select').value:'';
  const waypoints=DB.getList('waypoints').filter(w=>w.lat&&w.lng&&(!tripId||w.tripId===tripId));
  const places=DB.getList('places').filter(p=>p.lat&&p.lng&&(!tripId||p.tripId===tripId));
  const items=[...places.map(p=>({name:p.name,detail:p.location||p.type,icon:placeIcons[p.type]||'📍',lat:p.lat,lng:p.lng})),
               ...waypoints.map(w=>({name:w.name,detail:w.notes||(w.altitude?'⛰️ '+w.altitude+'m':''),icon:wpIcons[w.type]||'📍',lat:w.lat,lng:w.lng}))];
  let distHtml='';
  if(items.length>1){
    let totalKm=0;
    for(let i=1;i<items.length;i++)totalKm+=distanceBetween(items[i-1].lat,items[i-1].lng,items[i].lat,items[i].lng);
    distHtml=`<div class="alert alert-info"><strong>📏 ${totalKm.toFixed(1)} km</strong> total · ${items.length} stop${items.length!==1?'s':''}</div>`;
  }
  if(!items.length){
    el.innerHTML=selectHtml+`<div class="empty"><div class="icon">🧭</div><h3>No waypoints or places yet</h3><p>Add stops on the Map page or log a place here.</p></div>`;
    return;
  }
  el.innerHTML=selectHtml+distHtml+items.map(i=>`<div class="waypoint">
      <span style="font-size:20px">${i.icon}</span>
      <div class="wp-info">
        <div class="wp-name">${i.name}</div>
        <div class="wp-detail">${i.detail||''}</div>
      </div>
    </div>`).join('');
}

function deletePlace(id){DB.deleteItem('places',id);loadPlaces(currentPlaceTab);showToast('Removed.');}

// ═══════ GALLERY (with real photo upload) ═══════
let pendingPhotoData=null;
function handlePhotoUpload(e){
  const file=e.target.files[0];
  if(!file)return;
  if(file.size>3*1024*1024){showToast('Photo too large. Max 3MB.');return;}
  const reader=new FileReader();
  reader.onload=ev=>{
    pendingPhotoData=ev.target.result;
    document.getElementById('photo-upload-hint').textContent='✅ Photo ready!';
    document.getElementById('photo-preview-container').innerHTML=`<div class="photo-preview-item"><img src="${pendingPhotoData}" alt="preview"></div>`;
  };
  reader.readAsDataURL(file);
} 
function openMemoryModal(){
  populateTripSelect('mem-trip');
  document.getElementById('mem-date').value=today();
  pendingPhotoData=null;
  document.getElementById('photo-upload-hint').textContent='📷 Tap to upload a photo';
  document.getElementById('photo-preview-container').innerHTML='';
  document.getElementById('mem-photo-input').value='';
  openModal('modal-memory');
}
function saveMemory(){
  const title=document.getElementById('mem-title').value.trim();
  if(!title){showToast('Enter a title.');return;}
  DB.pushItem('memories',{id:uid(),tripId:document.getElementById('mem-trip').value,title,emoji:document.getElementById('mem-emoji').value,photo:pendingPhotoData||null,note:document.getElementById('mem-note').value,date:document.getElementById('mem-date').value});
  closeModal('modal-memory');showToast('Memory saved! 📸');
  document.getElementById('mem-title').value='';document.getElementById('mem-note').value='';
  pendingPhotoData=null;
  loadGallery();
}
function loadGallery(){
  const memories=DB.getList('memories');
  const trips=DB.getList('trips');
  const grid=document.getElementById('gallery-grid');
  if(!memories.length){
    grid.innerHTML=`<div class="empty" style="grid-column:1/-1"><div class="icon">📸</div><h3>No memories yet</h3><p>Add photos and journal entries.</p><button class="btn-primary btn-lg" onclick="openMemoryModal()">+ Add Memory</button></div>`;
    return;
  }
  grid.innerHTML=memories.slice().reverse().map(m=>{
    const trip=trips.find(t=>t.id===m.tripId);
    return `<div class="gallery-item" onclick="viewMemory('${m.id}')">
      ${m.photo?`<img src="${m.photo}" alt="${m.title}" loading="lazy">`:`<div class="gal-emoji">${m.emoji||'📸'}</div>`}
      <div class="overlay">${m.title}</div>
    </div>`;
  }).join('');
}
function viewMemory(id){
  const m=DB.getList('memories').find(x=>x.id===id);
  if(!m)return;
  const trip=DB.getList('trips').find(t=>t.id===m.tripId);
  document.getElementById('view-mem-title').textContent=`${m.emoji||'📸'} ${m.title}`;
  document.getElementById('view-mem-body').innerHTML=`
    ${m.photo?`<img src="${m.photo}" style="width:100%;border-radius:8px;margin-bottom:14px;max-height:300px;object-fit:cover;">`:''}
    <p style="font-size:12px;color:var(--ink-light);margin-bottom:10px;">📅 ${fmt(m.date)} ${trip?'· ✈️ '+trip.name:''}</p>
    ${m.note?`<p style="font-size:14px;line-height:1.7;color:var(--ink-mid);">${m.note}</p>`:'<p style="color:var(--ink-light);font-size:13px;">No notes.</p>'}
    <button class="btn-sm btn-danger" onclick="deleteMemory('${m.id}')" style="margin-top:14px;">Delete Memory</button>
  `;
  openModal('modal-view-memory');
}
function deleteMemory(id){DB.deleteItem('memories',id);closeModal('modal-view-memory');loadGallery();showToast('Memory deleted.');}

// ═══════ TRAVEL DOCUMENTS ═══════
let pendingDocData=null, currentViewDocId=null;
function handleDocumentUpload(e){
  const file=e.target.files[0];
  if(!file)return;
  const reader=new FileReader();
  reader.onload=event=>{
    pendingDocData=event.target.result;
    const preview=document.getElementById('doc-preview-container');
    if(file.type.includes('pdf')){
      preview.innerHTML='<div style="padding:20px;background:var(--sand);border-radius:8px;text-align:center;"><div style="font-size:32px;">📄</div><div style="font-size:12px;color:var(--ink-mid);margin-top:8px;">PDF uploaded</div></div>';
    } else {
      preview.innerHTML=`<img src="${pendingDocData}" style="width:100%;max-height:200px;border-radius:8px;object-fit:cover;">`;
    }
    document.getElementById('doc-upload-hint').textContent='✓ Document ready';
  };
  reader.readAsDataURL(file);
}
function saveDocument(){
  const tripId=document.getElementById('doc-trip').value;
  const type=document.getElementById('doc-type').value;
  const name=document.getElementById('doc-name').value.trim();
  const notes=document.getElementById('doc-notes').value.trim();
  const expiry=document.getElementById('doc-expiry').value;
  if(!tripId||!type||!name){
    showToast('Fill in trip, type, and name.');
    return;
  }
  DB.pushItem('documents',{
    id:uid(),
    tripId,
    type,
    name,
    notes,
    expiry,
    image:pendingDocData||null,
    added:new Date().toISOString().split('T')[0],
    status:expiry&&new Date(expiry)<new Date()?'expired':'valid'
  });
  closeModal('modal-upload-doc');
  showToast('📄 Document saved securely!');
  document.getElementById('doc-name').value='';
  document.getElementById('doc-notes').value='';
  document.getElementById('doc-expiry').value='';
  document.getElementById('doc-preview-container').innerHTML='';
  document.getElementById('doc-upload-hint').textContent='📷 Tap to upload document photo';
  pendingDocData=null;
  loadDocuments();
}
function loadDocuments(){
  const docs=DB.getList('documents');
  const stats=document.getElementById('doc-stats');
  const grid=document.getElementById('documents-grid');
  if(!docs.length){
    stats.innerHTML='<p style="color:var(--ink-light);text-align:center;grid-column:1/-1;padding:20px;">No documents yet. Upload your travel documents above.</p>';
    grid.innerHTML='';
    return;
  }
  const typeCounts={};
  docs.forEach(d=>{typeCounts[d.type]=(typeCounts[d.type]||0)+1;});
  stats.innerHTML=Object.entries(typeCounts).map(([type,count])=>`
    <div class="stat-card" style="padding:14px;text-align:center;background:var(--sky-light);border-radius:8px;">
      <div style="font-size:20px;margin-bottom:4px;">${getDocTypeIcon(type)}</div>
      <div style="font-size:12px;color:var(--ink-mid);margin-bottom:2px;text-transform:capitalize;">${type}</div>
      <div style="font-size:16px;font-weight:700;color:var(--sky);">${count}</div>
    </div>`).join('');
  grid.innerHTML=docs.map(d=>`
    <div class="gallery-item" onclick="viewDocument('${d.id}')" style="cursor:pointer;position:relative;">
      ${d.image?`<img src="${d.image}" alt="${d.name}" style="width:100%;height:150px;object-fit:cover;">`:`<div class="gal-emoji" style="background:var(--sand);display:flex;align-items:center;justify-content:center;height:150px;border-radius:8px;font-size:32px;">${getDocTypeIcon(d.type)}</div>`}
      <div class="overlay">${d.name}</div>
      ${d.status==='expired'?`<div style="position:absolute;top:8px;right:8px;background:#c02;color:#fff;padding:4px 8px;border-radius:4px;font-size:10px;font-weight:600;">EXPIRED</div>`:''}
    </div>`).join('');
}
function getDocTypeIcon(type){
  const icons={passport:'🛂',ticket:'🎫',hotel:'🏨',insurance:'🛡️',visa:'📋',receipt:'🧾',itinerary:'📍',other:'📎'};
  return icons[type]||'📄';
}
function viewDocument(id){
  const doc=DB.getList('documents').find(d=>d.id===id);
  if(!doc)return;
  const trip=DB.getList('trips').find(t=>t.id===doc.tripId);
  currentViewDocId=id;
  document.getElementById('doc-view-title').textContent=`${getDocTypeIcon(doc.type)} ${doc.name}`;
  document.getElementById('doc-view-body').innerHTML=`
    <div style="margin-bottom:16px;">
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:14px;">
        <div style="background:var(--sand);padding:10px;border-radius:6px;">
          <div style="font-size:11px;color:var(--ink-mid);margin-bottom:4px;">Type</div>
          <div style="font-size:13px;font-weight:600;text-transform:capitalize;">${doc.type}</div>
        </div>
        <div style="background:var(--sand);padding:10px;border-radius:6px;">
          <div style="font-size:11px;color:var(--ink-mid);margin-bottom:4px;">Status</div>
          <div style="font-size:13px;font-weight:600;color:${doc.status==='expired'?'#c02':'#080'};">${doc.status==='expired'?'EXPIRED':'Valid'}</div>
        </div>
      </div>
      ${doc.expiry?`<div style="background:var(--sand);padding:10px;border-radius:6px;margin-bottom:14px;">
        <div style="font-size:11px;color:var(--ink-mid);margin-bottom:4px;">Valid Until</div>
        <div style="font-size:13px;font-weight:600;">${fmt(doc.expiry)}</div>
      </div>`:''}
      ${trip?`<div style="background:var(--sand);padding:10px;border-radius:6px;margin-bottom:14px;">
        <div style="font-size:11px;color:var(--ink-mid);margin-bottom:4px;">Trip</div>
        <div style="font-size:13px;font-weight:600;">✈️ ${trip.name}</div>
      </div>`:''}
    </div>
    ${doc.notes?`<div style="background:var(--sky-light);padding:12px;border-radius:6px;margin-bottom:14px;">
      <div style="font-size:11px;color:var(--ink-mid);margin-bottom:6px;">Notes</div>
      <div style="font-size:13px;color:var(--ink-mid);line-height:1.6;">${doc.notes}</div>
    </div>`:''}
    ${doc.image?`<div style="margin-bottom:14px;border-radius:8px;overflow:hidden;border:1px solid var(--border);">
      <img src="${doc.image}" style="width:100%;max-height:350px;object-fit:contain;background:var(--sand);">
    </div>`:'<div style="background:var(--sand);padding:40px;border-radius:8px;text-align:center;color:var(--ink-light);font-size:13px;">📄 No image uploaded</div>'}
    <div style="font-size:11px;color:var(--ink-light);text-align:center;padding-top:10px;">Added on ${fmt(doc.added)}</div>
  `;
  openModal('modal-view-doc');
}
async function deleteDocument(id){
  const ok=await showConfirm('Delete this document? This cannot be undone.','Delete document?','📄','Delete','btn-danger');
  if(!ok)return;
  DB.deleteItem('documents',id);
  closeModal('modal-view-doc');
  loadDocuments();
  showToast('Document deleted.');
}

// ═══════ PROFILE ═══════
function saveProfile(){
  currentUser={
    ...currentUser,
    name:document.getElementById('profile-name').value,
    email:document.getElementById('profile-email').value,
    city:document.getElementById('profile-city').value,
    style:document.getElementById('profile-style').value,
    currency:document.getElementById('profile-currency').value
  };
  DB.set('user',currentUser);
  updateNavUser();
  showToast('Profile saved! ✓');
}
async function logout(){
  try{
    await window.FB.signOutUser();
  }catch(e){
    console.error('Sign out error:', e);
  }
  currentUser=null;
  cloudUID=null;
  localStorage.removeItem('tt_user');
  showToast('Logged out. See you soon!');
  switchAuthTab('login');
  showPage('auth');
}
function loadProfileStats(){
  const trips=DB.getList('trips');
  const expenses=DB.getList('expenses');
  const places=DB.getList('places');
  const totalSpent=expenses.reduce((a,e)=>a+e.amount,0);
  document.getElementById('profile-stats').innerHTML=`
    <div class="stat-cards" style="grid-template-columns:1fr 1fr;gap:10px;">
      <div class="stat-card sky"><div class="big" style="font-size:22px;">${trips.length}</div><div class="lbl">Trips</div></div>
      <div class="stat-card terra"><div class="big" style="font-size:22px;">${trips.filter(t=>t.status==='completed').length}</div><div class="lbl">Completed</div></div>
      <div class="stat-card forest"><div class="big" style="font-size:18px;">${fmtNum(totalSpent)}</div><div class="lbl">Total Spent</div></div>
      <div class="stat-card warn"><div class="big" style="font-size:22px;">${places.length}</div><div class="lbl">Places</div></div>
    </div>
    ${currentUser&&currentUser.style?`<div class="alert alert-info" style="margin-top:10px;">🧳 ${currentUser.style}</div>`:''}`;
  if(currentUser&&currentUser.city){
    document.getElementById('profile-tags').innerHTML=`<span class="tag">📍 ${currentUser.city}</span>`;
  }
}
async function clearAllData(){
  const ok=await showConfirm('This will permanently delete ALL your trips, expenses, photos and data. This cannot be undone.','Clear all data?','☢️','Delete Everything','btn-danger');
  if(!ok)return;
  ['trips','expenses','waypoints','places','memories','sos_contacts','documents','notifications','packing']
  .forEach(k => localStorage.removeItem(userStorageKey(k)));
  showToast('All data cleared.');
  loadDashboard();
}

// ═══════ SOS ═══════
let sosLocation=null,sosMap=null,sosMarker=null;
function loadSOSContacts(){
  const contacts=DB.getList('sos_contacts');
  const el=document.getElementById('sos-contacts-list');
  if(!el)return;
  if(!contacts.length){el.innerHTML='<p style="color:var(--ink-light);font-size:12px;padding:6px 0;">No contacts added yet.</p>';return;}
  el.innerHTML=contacts.map(c=>`
    <div class="emergency-contact">
      <div><div style="font-weight:600;font-size:13px;">${c.name}</div><div style="font-size:12px;color:var(--ink-light);">${c.phone}</div></div>
      <div style="display:flex;gap:6px;">
        <a href="tel:${c.phone}" class="btn-sm btn-success" style="text-decoration:none;font-size:12px;">📞</a>
        <button class="btn-sm btn-danger" onclick="removeSOSContact('${c.id}')" style="font-size:12px;">✕</button>
      </div>
    </div>`).join('');
}
function addSOSContact(){
  const name=document.getElementById('sos-contact-name').value.trim();
  const phone=document.getElementById('sos-contact-phone').value.trim();
  if(!name||!phone){showToast('Enter name and phone.');return;}
  DB.pushItem('sos_contacts',{id:uid(),name,phone});
  document.getElementById('sos-contact-name').value='';
  document.getElementById('sos-contact-phone').value='';
  loadSOSContacts();showToast('Contact added! 👤');
}
function removeSOSContact(id){DB.deleteItem('sos_contacts',id);loadSOSContacts();}
function getSOSLocation(){
  const el=document.getElementById('sos-location-display');
  el.textContent='📡 Fetching your location...';
  navigator.geolocation.getCurrentPosition(pos=>{
    sosLocation={lat:pos.coords.latitude,lng:pos.coords.longitude};
    renderSOSLocation();
  },()=>{el.textContent='❌ Could not get location. Enable GPS, or click "Set on map" below.';showSOSMapForManualPin();},{enableHighAccuracy:true,timeout:10000,maximumAge:0});
}
function renderSOSLocation(){
  const el=document.getElementById('sos-location-display');
  el.innerHTML=`<strong>📍 Location found!</strong><br>Lat: ${sosLocation.lat.toFixed(5)}, Lng: ${sosLocation.lng.toFixed(5)}<br><a href="https://maps.google.com/?q=${sosLocation.lat},${sosLocation.lng}" target="_blank" style="color:var(--sky);">Open in Google Maps →</a><br><span style="font-size:11px;color:var(--ink-light);">If this pin is in the wrong place (common on desktop/laptop without GPS), tap the map to drop it in the right spot.</span>`;
  const mapEl=document.getElementById('sos-map');
  mapEl.style.display='block';
  if(!sosMap){
    sosMap=L.map('sos-map').setView([sosLocation.lat,sosLocation.lng],14);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{attribution:'© OSM'}).addTo(sosMap);
    // Manual correction: desktop/laptop geolocation has no GPS chip to rely
    // on and can be noticeably off. Letting the user tap the real spot is
    // more trustworthy for an emergency feature than a free-text address box.
    sosMap.on('click', e=>{
      sosLocation={lat:e.latlng.lat,lng:e.latlng.lng};
      renderSOSLocation();
      showToast('Location pin updated to where you tapped.');
    });
  } else {sosMap.setView([sosLocation.lat,sosLocation.lng],14);}
  if(sosMarker)sosMap.removeLayer(sosMarker);
  sosMarker=L.marker([sosLocation.lat,sosLocation.lng]).addTo(sosMap).bindPopup('📍 Your location').openPopup();
  setTimeout(()=>sosMap.invalidateSize(),150);
}
function showSOSMapForManualPin(){
  // No location yet at all — still show the map (centered on India) so the
  // person can tap their actual position instead of being stuck.
  const mapEl=document.getElementById('sos-map');
  mapEl.style.display='block';
  if(!sosMap){
    sosMap=L.map('sos-map').setView([20.5937,78.9629],5);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{attribution:'© OSM'}).addTo(sosMap);
    sosMap.on('click', e=>{
      sosLocation={lat:e.latlng.lat,lng:e.latlng.lng};
      renderSOSLocation();
      showToast('Location pin set to where you tapped.');
    });
  }
  setTimeout(()=>sosMap.invalidateSize(),150);
}
function activateSOS(){
  openModal('modal-sos-active');
  const locEl=document.getElementById('sos-location-text');
  const setLoc=loc=>{locEl.innerHTML=`📍 <strong>${loc.lat.toFixed(5)}, ${loc.lng.toFixed(5)}</strong><br><a href="https://maps.google.com/?q=${loc.lat},${loc.lng}" target="_blank" style="color:var(--sky);">Open in Google Maps →</a>`;};
  if(sosLocation){setLoc(sosLocation);}
  else{
    locEl.textContent='Getting your location...';
    navigator.geolocation.getCurrentPosition(pos=>{
      sosLocation={lat:pos.coords.latitude,lng:pos.coords.longitude};
      setLoc(sosLocation);
    },()=>{locEl.textContent='Location unavailable — call 112 directly!';});
  }
  const contacts=DB.getList('sos_contacts');
  const alertEl=document.getElementById('sos-alert-text');
  if(alertEl)alertEl.textContent=contacts.length?`Share your location with: ${contacts.map(c=>c.name).join(', ')}.`:'Add emergency contacts in SOS settings.';
  showToast('🆘 SOS Activated! Call 112 if in immediate danger.',4000);
}

// ═══════ WEATHER ═══════
// Replace with your own key from openweathermap.org (free tier)
const OWM_API_KEY = 'c2304c2a91884cb2d263a0c4cb493e36';
async function fetchWeatherByCoords(lat,lon,elementId,label,isGPS){
  try{
    // Use the actual "current weather" endpoint for the current
    // temperature — NOT the forecast endpoint's first slot, which is a
    // 3-hour-bucket forecast and can be noticeably off from what it
    // actually is right now. The forecast endpoint is still used below,
    // only for the upcoming-days strip.
    const [curResponse,wxResponse]=await Promise.all([
      fetch(`https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${OWM_API_KEY}&units=metric`),
      fetch(`https://api.openweathermap.org/data/2.5/forecast?lat=${lat}&lon=${lon}&appid=${OWM_API_KEY}&units=metric`)
    ]);
    if(!curResponse.ok||!wxResponse.ok)throw new Error('Weather API failed');
    const current=await curResponse.json();
    const wxData=await wxResponse.json();
    const forecasts=[wxData.list[0],wxData.list[8]||wxData.list[1],wxData.list[16]||wxData.list[2]];
    const wxIcon=getWeatherIcon(current.weather[0].icon);
    const forecastHTML=forecasts.map((f)=>{
      const date=new Date(f.dt*1000);
      const icon=getWeatherIcon(f.weather[0].icon);
      return `
      <div class="weather-forecast-day">
        <div style="font-size:11px;color:var(--ink-light);">${date.toLocaleDateString('en-US',{month:'short',day:'numeric'})}</div>
        <div style="font-size:24px;margin:4px 0;">${icon}</div>
        <div style="font-size:12px;font-weight:600;">${Math.round(f.main.temp_max)}°</div>
        <div style="font-size:11px;color:var(--ink-light);">${Math.round(f.main.temp_min)}°</div>
      </div>`;
    }).join('');
    document.getElementById(elementId).innerHTML=`
      <div class="form-section">
        <h3 style="font-size:15px;margin-bottom:12px;">📍 Weather Near You</h3>
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;">
          <div style="background:var(--sky-light);border-radius:12px;padding:16px;">
            <div style="display:flex;align-items:center;gap:10px;margin-bottom:10px;">
              <div style="font-size:48px;">${wxIcon}</div>
              <div>
                <div style="font-size:28px;font-weight:700;">${Math.round(current.main.temp)}°C</div>
                <div style="font-size:13px;color:var(--ink-mid);">${current.weather[0].main}</div>
              </div>
            </div>
            <div style="font-size:12px;color:var(--ink-mid);display:flex;gap:10px;flex-wrap:wrap;">
              <span>💧 ${current.main.humidity}%</span>
              <span>📍 ${label||'Your location'}</span>
            </div>
            ${isGPS?`<div style="font-size:11px;color:var(--ink-light);margin-top:8px;">Not your city? Your device's WiFi/network positioning can miss — type your city in the box above to fix it.</div>`:''}
          </div>
          <div style="background:var(--sand);border-radius:12px;padding:12px;display:flex;gap:8px;justify-content:space-around;overflow-x:auto;">
            ${forecastHTML}
          </div>
        </div>
      </div>`;
  }catch(e){
    document.getElementById(elementId).innerHTML=`<div class="alert alert-info">🌤️ Weather update not available</div>`;
  }
}
// Manual override for desktop/laptop users, where browser geolocation is
// WiFi/IP-based and can be noticeably inaccurate (no GPS chip to rely on).
// Geocodes the typed city name and shows weather for that exact place —
// bypassing device location entirely.
async function setWeatherCity(){
  const input=document.getElementById('home-weather-city');
  const city=input.value.trim();
  if(!city){showToast('Type a city name first.');return;}
  const el=document.getElementById('home-weather');
  if(el)el.innerHTML='<div class="alert alert-info">📡 Looking up weather for '+city+'…</div>';
  try{
    const geoResponse=await fetch(`https://nominatim.openstreetmap.org/search?format=json&q=${encodeURIComponent(city)}`);
    if(!geoResponse.ok)throw new Error('Geocoding failed');
    const geoData=await geoResponse.json();
    if(!geoData.length){
      if(el)el.innerHTML='<div class="alert alert-info">Couldn\'t find that place. Try a more specific name (e.g. add state/country).</div>';
      return;
    }
    const lat=parseFloat(geoData[0].lat);
    const lon=parseFloat(geoData[0].lon);
    localStorage.setItem('tt_manual_weather_city',city);
    fetchWeatherByCoords(lat,lon,'home-weather',city);
  }catch(e){
    if(el)el.innerHTML='<div class="alert alert-info">🌤️ Weather update not available</div>';
  }
}

async function loadCurrentLocationWeather(elementId){
  const el=document.getElementById(elementId);

  // Geolocation is blocked by browsers on insecure origins (file:// or plain http://).
  // Catching this explicitly avoids silently falling back to an unrelated city.
  if(!window.isSecureContext){
    if(el)el.innerHTML='<div class="alert alert-info">📍 Your browser is blocking location access because this page isn\'t loaded over <strong>https://</strong> (or localhost). Host/open TripTrace over HTTPS to get weather for where you actually are. Showing a fallback below in the meantime.</div>';
    showFallbackWeather(elementId);
    return;
  }
  if(!navigator.geolocation){
    if(el)el.innerHTML='<div class="alert alert-info">📍 Location not supported on this device. Add your home city in Profile to see weather.</div>';
    return;
  }
  if(el)el.innerHTML='<div class="alert alert-info">📡 Getting your current location for weather…</div>';
  navigator.geolocation.getCurrentPosition(async pos=>{
    const lat=pos.coords.latitude;
    const lon=pos.coords.longitude;
    const accuracy=pos.coords.accuracy||0;
    let label='Your current location';
    try{
      // zoom=10 biases Nominatim's reverse lookup to city/town level. The default
      // (zoom=18, building level) can fail to resolve a "city" field at all for
      // addresses outside city centers, which used to fall straight through to a
      // much broader "county" name and looked like the location was just wrong.
      const revResponse=await fetch(`https://nominatim.openstreetmap.org/reverse?format=json&lat=${lat}&lon=${lon}&zoom=10`);
      if(revResponse.ok){
        const revData=await revResponse.json();
        const a=revData.address||{};
        const place=a.city||a.town||a.village||a.suburb||a.county||a.state_district;
        // Include the state (e.g. "Chennai, Tamil Nadu" not just "Chennai") — several
        // towns share a name across different states/countries, so the bare place name
        // alone can look "wrong" even when the coordinates are correct, and can also
        // hide a genuine wrong-city mismatch when they aren't.
        if(place)label=a.state?`${place}, ${a.state}`:place;
      }
    }catch(e){}
    // A coarse fix (>15km) means the DEVICE reported the wrong coordinates before any
    // of our code ran — usually WiFi/IP-based positioning defaulting to the nearest big
    // city instead of a small town. No reverse-geocoding fix can correct bad input
    // coordinates, so we warn instead of silently presenting the guess as fact.
    if(accuracy>15000&&el){
      const existingWarn=document.getElementById(elementId+'-accuracy-warn');
      if(existingWarn)existingWarn.remove();
      const warn=document.createElement('div');
      warn.id=elementId+'-accuracy-warn';
      warn.className='alert alert-info';
      warn.style.marginBottom='6px';
      warn.innerHTML=`📍 Your device's location fix is only accurate to ±${(accuracy/1000).toFixed(0)}km, so "${label}" may not be your real town. For an exact reading, switch your device to <strong>High Accuracy / GPS</strong> location mode, or type your city below.`;
      el.parentElement.insertBefore(warn,el);
    }
    // Network-based (WiFi/cell) positioning can report a confidently WRONG city — a
    // small accuracy number, but the wrong place entirely — which the accuracy check
    // above can't catch. So always pass isGPS=true to show a low-key correction hint
    // alongside the result, not only when the accuracy field itself looks bad.
    fetchWeatherByCoords(lat,lon,elementId,label,true);
  },err=>{
    // Permission denied or unavailable — show the real reason instead of silently
    // switching to a trip destination's weather (which looked like the "wrong city" bug).
    let reason='Location could not be determined.';
    if(err.code===err.PERMISSION_DENIED)reason='Location access was denied for this site.';
    else if(err.code===err.POSITION_UNAVAILABLE)reason='Your device could not determine a location.';
    else if(err.code===err.TIMEOUT)reason='Getting your location timed out.';
    if(el)el.innerHTML=`<div class="alert alert-info">📍 ${reason} Check your browser's site settings (address bar → Location) and allow access, or add your home city in Profile. Showing a fallback below in the meantime.</div>`;
    showFallbackWeather(elementId);
  },{enableHighAccuracy:true,timeout:10000,maximumAge:0});
}

function showFallbackWeather(elementId){
  // Clearly-labelled fallback: never silently replace the "current location" heading
  // with weather for a different place without saying so.
  const trips=DB.getList('trips');
  const ongoingTrip=trips.find(t=>t.status==='ongoing')||trips[trips.length-1];
  const fallbackDest=ongoingTrip?.destination||(currentUser&&currentUser.city)||null;
  if(!fallbackDest)return;
  const existing=document.getElementById(elementId+'-fallback');
  if(existing)existing.remove();
  const holder=document.createElement('div');
  holder.id=elementId+'-fallback';
  holder.innerHTML=`<div class="alert alert-info" style="margin-bottom:6px;">Showing weather for <strong>${fallbackDest}</strong> (trip destination or profile city) instead of your live location — this is a fallback, not your GPS position.</div><div id="${elementId}-fallback-weather"></div>`;
  document.getElementById(elementId).insertAdjacentElement('afterend',holder);
  fetchDestinationWeather(fallbackDest,elementId+'-fallback-weather');
}
async function fetchDestinationWeather(destination,elementId){
  try{
    const destClean=destination.split(',')[0].trim();
    const geoResponse=await fetch(`https://nominatim.openstreetmap.org/search?format=json&q=${encodeURIComponent(destClean+' India')}`);
    if(!geoResponse.ok)throw new Error('Geocoding failed');
    const geoData=await geoResponse.json();
    if(!geoData.length)throw new Error('Destination not found');
    const geo=geoData[0];
    const lat=parseFloat(geo.lat);
    const lon=parseFloat(geo.lon);
    const wxResponse=await fetch(`https://api.openweathermap.org/data/2.5/forecast?lat=${lat}&lon=${lon}&appid=${OWM_API_KEY}&units=metric`);
    if(!wxResponse.ok)throw new Error('Weather API failed');
    const wxData=await wxResponse.json();
    const current=wxData.list[0];
    const forecasts=[wxData.list[0],wxData.list[8]||wxData.list[1],wxData.list[16]||wxData.list[2]];
    const wxIcon=getWeatherIcon(current.weather[0].icon);
    const forecastHTML=forecasts.map((f,i)=>{
      const date=new Date(f.dt*1000);
      const icon=getWeatherIcon(f.weather[0].icon);
      return `
      <div class="weather-forecast-day">
        <div style="font-size:11px;color:var(--ink-light);">${date.toLocaleDateString('en-US',{month:'short',day:'numeric'})}</div>
        <div style="font-size:24px;margin:4px 0;">${icon}</div>
        <div style="font-size:12px;font-weight:600;">${Math.round(f.main.temp_max)}°</div>
        <div style="font-size:11px;color:var(--ink-light);">${Math.round(f.main.temp_min)}°</div>
      </div>`;
    }).join('');
    document.getElementById(elementId).innerHTML=`
      <div class="form-section">
        <h3 style="font-size:15px;margin-bottom:12px;">🌤️ Weather Forecast</h3>
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;">
          <div style="background:var(--sky-light);border-radius:12px;padding:16px;">
            <div style="display:flex;align-items:center;gap:10px;margin-bottom:10px;">
              <div style="font-size:48px;">${wxIcon}</div>
              <div>
                <div style="font-size:28px;font-weight:700;">${Math.round(current.main.temp)}°C</div>
                <div style="font-size:13px;color:var(--ink-mid);">${current.weather[0].main}</div>
              </div>
            </div>
            <div style="font-size:12px;color:var(--ink-mid);display:flex;gap:10px;">
              <span>💧 ${current.main.humidity}%</span>
              <span>📍 ${destClean}</span>
            </div>
          </div>
          <div style="background:var(--sand);border-radius:12px;padding:12px;display:flex;gap:8px;justify-content:space-around;overflow-x:auto;">
            ${forecastHTML}
          </div>
        </div>
      </div>`;
  }catch(e){
    document.getElementById(elementId).innerHTML=`<div class="alert alert-info">🌤️ Weather update not available</div>`;
  }
}
function getWeatherIcon(code){
  // OpenWeatherMap icon codes: 01d, 02d, 03d, 04d, 09d, 10d, 11d, 13d, 50d (d=day, n=night)
  if(!code)return '🌤️';
  const mainCode=code.substring(0,2);
  if(mainCode==='01')return '☀️';
  if(mainCode==='02')return '⛅';
  if(mainCode==='03'||mainCode==='04')return '☁️';
  if(mainCode==='09'||mainCode==='10')return '🌧️';
  if(mainCode==='11')return '⛈️';
  if(mainCode==='13')return '❄️';
  if(mainCode==='50')return '🌫️';
  return '🌤️';
}

// ═══════ ANALYTICS ═══════
function tripDays(start,end){
  const s=new Date(start);
  const e=new Date(end);
  if(isNaN(s)||isNaN(e))return 1;
  return Math.max(1,Math.round((e-s)/(1000*60*60*24))+1);
}
function styleBudgetFactor(style){
  return {budget:0.8,midrange:1,adventure:1.1,family:1.2,luxury:1.4}[style]||1;
}
function loadAnalytics(){
  const trips=DB.getList('trips'),expenses=DB.getList('expenses'),places=DB.getList('places');
  const totalSpent=expenses.reduce((a,e)=>a+e.amount,0);
  const avgBudget=trips.length?Math.round(trips.reduce((a,t)=>a+(t.budget||0),0)/trips.length):0;
  document.getElementById('analytics-stats').innerHTML=`
    <div class="stat-card sky"><div class="big">${trips.length}</div><div class="lbl">Total Trips</div></div>
    <div class="stat-card terra"><div class="big">${fmtNum(totalSpent)}</div><div class="lbl">Total Spent</div></div>
    <div class="stat-card forest"><div class="big">${fmtNum(avgBudget)}</div><div class="lbl">Avg Budget</div></div>
    <div class="stat-card warn"><div class="big">${expenses.length}</div><div class="lbl">Expenses</div></div>
    <div class="stat-card"><div class="big">${places.length}</div><div class="lbl">Places</div></div>
    <div class="stat-card"><div class="big">${trips.filter(t=>t.status==='completed').length}</div><div class="lbl">Completed</div></div>`;
  const catTotals={},catLabels={food:'🍔 Food',hotel:'🏨 Hotel',transport:'🚌 Transport',activity:'🎯 Activity',shopping:'🛍️ Shopping',other:'📌 Other'};
  const catColors={food:'#f39c12',hotel:'#3498db',transport:'#2ecc71',activity:'#e74c3c',shopping:'#9b59b6',other:'#95a5a6'};
  expenses.forEach(e=>{catTotals[e.category]=(catTotals[e.category]||0)+e.amount;});
  const maxCat=Math.max(1,...Object.values(catTotals));
  document.getElementById('analytics-cat-chart').innerHTML=totalSpent?Object.entries(catTotals).sort((a,b)=>b[1]-a[1]).map(([cat,amt])=>`<div class="chart-bar-wrap"><div class="chart-bar-label"><span>${catLabels[cat]||cat}</span><span style="font-weight:600">${fmtNum(amt)} (${Math.round(amt/totalSpent*100)}%)</span></div><div class="progress-bar"><div class="progress-fill" style="width:${Math.round(amt/maxCat*100)}%;background:${catColors[cat]||'#1a6985'}"></div></div></div>`).join(''):'<p style="color:var(--ink-light);font-size:13px;">No expenses to chart yet.</p>';
  document.getElementById('analytics-budget-chart').innerHTML=trips.filter(t=>t.budget>0).map(t=>{const spent=expenses.filter(e=>e.tripId===t.id).reduce((a,e)=>a+e.amount,0);const pct=Math.min(100,Math.round(spent/t.budget*100));const barClass=pct>90?'danger':pct>70?'warn':'';return`<div class="chart-bar-wrap"><div class="chart-bar-label"><span>${t.emoji||'✈️'} ${t.name}</span><span>${pct}%</span></div><div class="progress-bar" style="height:10px;"><div class="progress-fill ${barClass}" style="width:${pct}%"></div></div><div style="font-size:11px;color:var(--ink-light);margin-top:2px;">${fmtNum(spent)} / ${fmtNum(t.budget)}</div></div>`;}).join('')||'<p style="color:var(--ink-light);font-size:13px;">Set trip budgets to compare.</p>';
  const completedTrips=trips.filter(t=>t.status==='completed');
  const historyData=completedTrips.map(t=>{const spent=expenses.filter(e=>e.tripId===t.id).reduce((a,e)=>a+e.amount,0);const days=tripDays(t.start,t.end);return {spent,days,avg:days?spent/days:0};});
  const avgDaily=historyData.length?historyData.reduce((a,h)=>a+h.avg,0)/historyData.length:2200;
  const predictionNodes=[];
  if(historyData.length){
    predictionNodes.push(`<div style="font-size:13px;color:var(--ink-mid);margin-bottom:12px;">Based on ${historyData.length} completed trip${historyData.length!==1?'s':''}, your average daily spend is <strong>₹${fmtNum(Math.round(avgDaily))}</strong>.</div>`);
    const plannedTrips=trips.filter(t=>t.status==='planned'||t.status==='ongoing');
    if(plannedTrips.length){
      predictionNodes.push('<div class="grid-2" style="gap:12px;">');
      plannedTrips.forEach(t=>{
        const days=tripDays(t.start,t.end);
        const predicted=Math.round(avgDaily*days*styleBudgetFactor(currentUser?.style||'midrange'));
        const diff=t.budget?predicted-t.budget:0;
        const note=t.budget?`Budget ${fmtNum(t.budget)} · ${diff===0?'On target':diff>0?`Over by ₹${fmtNum(diff)}`:`Under by ₹${fmtNum(Math.abs(diff))}`}`:'Suggested budget';
        predictionNodes.push(`<div class="stat-card"><div class="big">${fmtNum(predicted)}</div><div class="lbl">${t.name} · ${days} days</div><div style="font-size:12px;color:var(--ink-light);margin-top:8px;">${note}</div></div>`);
      });
      predictionNodes.push('</div>');
    } else {
      const style=styleBudgetFactor(currentUser?.style||'midrange');
      const forecast=Math.round(avgDaily*4*style);
      predictionNodes.push(`<div class="stat-card"><div class="big">${fmtNum(forecast)}</div><div class="lbl">4-day sample budget</div><div style="font-size:12px;color:var(--ink-light);margin-top:8px;">Style factor: ${style.toFixed(2)}</div></div>`);
    }
  } else {
    predictionNodes.push('<div class="alert alert-info">Log completed trips and expenses to enable smart budget forecasting. TripTrace uses your past spending history to suggest budgets for future trips.</div>');
  }
  document.getElementById('analytics-budget-prediction').innerHTML=predictionNodes.join('');
  const monthly={};
  expenses.forEach(e=>{if(e.date){const key=e.date.slice(0,7);monthly[key]=(monthly[key]||0)+e.amount;}});
  const months=Object.keys(monthly).sort().slice(-6);
  const maxMonth=Math.max(1,...Object.values(monthly));
  document.getElementById('analytics-monthly').innerHTML=months.length?months.map(m=>`<div class="chart-bar-wrap"><div class="chart-bar-label"><span>${m}</span><span style="font-weight:600">${fmtNum(monthly[m])}</span></div><div class="progress-bar"><div class="progress-fill" style="width:${Math.round(monthly[m]/maxMonth*100)}%"></div></div></div>`).join(''):'<p style="color:var(--ink-light);font-size:13px;">No data yet.</p>';
  const dests={};
  trips.forEach(t=>{if(t.destination){const d=t.destination.split(',')[0].trim();dests[d]=(dests[d]||0)+1;}});
  document.getElementById('analytics-destinations').innerHTML=Object.entries(dests).sort((a,b)=>b[1]-a[1]).slice(0,5).map(([dest,count])=>`<div style="display:flex;align-items:center;justify-content:space-between;padding:7px 0;border-bottom:1px solid var(--border);font-size:13px;"><span>📍 ${dest}</span><span style="font-weight:700;color:var(--sky);">${count} trip${count!==1?'s':''}</span></div>`).join('')||'<p style="color:var(--ink-light);font-size:13px;">Add destinations to trips.</p>';
  const transportExpenses=expenses.filter(e=>e.category==='transport');
  const trainTrips=transportExpenses.filter(e=>{const desc=(e.desc||'').toLowerCase();return desc.includes('train')||desc.includes('rail');}).length;
  const flightTrips=transportExpenses.filter(e=>{const desc=(e.desc||'').toLowerCase();return desc.includes('flight');}).length;
  document.getElementById('analytics-sustainability').innerHTML=`
    <div class="grid-3">
      <div style="text-align:center;padding:18px;"><div style="font-size:32px;margin-bottom:6px;">🚂</div><div style="font-size:22px;font-weight:700;color:var(--forest);">${trainTrips}</div><div style="font-size:12px;color:var(--ink-light);">Train journeys</div></div>
      <div style="text-align:center;padding:18px;"><div style="font-size:32px;margin-bottom:6px;">✈️</div><div style="font-size:22px;font-weight:700;color:var(--terra);">${flightTrips}</div><div style="font-size:12px;color:var(--ink-light);">Flights logged</div></div>
      <div style="text-align:center;padding:18px;"><div style="font-size:32px;margin-bottom:6px;">🧳</div><div style="font-size:22px;font-weight:700;color:var(--sky);">${trips.length}</div><div style="font-size:12px;color:var(--ink-light);">Trips tracked</div></div>
    </div>
    <div class="alert alert-info" style="margin-top:10px;">💡 <strong>Tip:</strong> Keep transport notes consistent to make your trip history easier to review.</div>`;
}



// ═══════ CUSTOM CONFIRM (Fix 6: replaces browser confirm()) ═══════
let _confirmResolve=null;
function showConfirm(message,title='Are you sure?',icon='⚠️',okLabel='Confirm',okClass='btn-danger'){
  return new Promise(resolve=>{
    _confirmResolve=resolve;
    document.getElementById('confirm-title').textContent=title;
    document.getElementById('confirm-body').textContent=message;
    document.getElementById('confirm-icon').textContent=icon;
    const okBtn=document.getElementById('confirm-ok-btn');
    okBtn.textContent=okLabel;
    okBtn.className='btn-sm '+okClass;
    document.getElementById('confirm-overlay').classList.add('open');
  });
}
function resolveConfirm(){
  document.getElementById('confirm-overlay').classList.remove('open');
  if(_confirmResolve)_confirmResolve(true);
  _confirmResolve=null;
}
function rejectConfirm(){
  document.getElementById('confirm-overlay').classList.remove('open');
  if(_confirmResolve)_confirmResolve(false);
  _confirmResolve=null;
}


// ═══════ CURRENCY CONVERTER (Fix 13) ═══════
let exchangeRates=null;
const CONV_CURRENCIES=[
  {code:'USD',symbol:'$',name:'US Dollar'},
  {code:'EUR',symbol:'€',name:'Euro'},
  {code:'GBP',symbol:'£',name:'British Pound'},
  {code:'AED',symbol:'AED',name:'UAE Dirham'},
  {code:'SGD',symbol:'S$',name:'Singapore $'},
  {code:'JPY',symbol:'¥',name:'Japanese Yen'},
];
async function loadExchangeRates(forceRefresh){
  const statusEl=document.getElementById('currency-rates-status');
  const cacheKey='tt_exchange_rates';
  if(!forceRefresh){
    const cached=DB.get('exchange_rates_cache');
    if(cached&&cached.timestamp&&(Date.now()-cached.timestamp)<3600000){
      exchangeRates=cached.rates;
      if(statusEl)statusEl.textContent='Rates cached '+new Date(cached.timestamp).toLocaleTimeString()+'. ';
      updateCurrencyConversion();
      return;
    }
  }
  if(statusEl)statusEl.textContent='Fetching live rates…';
  try{
    const res=await fetch('https://open.er-api.com/v6/latest/INR');
    if(!res.ok)throw new Error('Rate fetch failed');
    const data=await res.json();
    exchangeRates=data.rates;
    DB.set('exchange_rates_cache',{rates:exchangeRates,timestamp:Date.now()});
    if(statusEl)statusEl.textContent='✓ Live rates updated '+new Date().toLocaleTimeString();
    updateCurrencyConversion();
  }catch(e){
    const cached=DB.get('exchange_rates_cache');
    if(cached){
      exchangeRates=cached.rates;
      if(statusEl)statusEl.textContent='⚠️ Using cached rates (offline). Last updated '+new Date(cached.timestamp).toLocaleTimeString();
      updateCurrencyConversion();
    } else {
      if(statusEl)statusEl.textContent='⚠️ Could not fetch exchange rates. Check your connection.';
    }
  }
}
function updateCurrencyConversion(){
  const grid=document.getElementById('currency-conversion-grid');
  if(!grid)return;
  const amount=+document.getElementById('conv-amount').value||1000;
  if(!exchangeRates){
    grid.innerHTML='<p style="color:var(--ink-light);font-size:12px;grid-column:1/-1;">Rates not loaded yet.</p>';
    return;
  }
  grid.innerHTML=CONV_CURRENCIES.map(c=>{
    const rate=exchangeRates[c.code];
    if(!rate)return '';
    const converted=(amount*rate).toLocaleString('en-IN',{maximumFractionDigits:2});
    return `<div class="stat-card" style="padding:12px;">
      <div style="font-size:11px;color:var(--ink-light);margin-bottom:4px;">${c.name}</div>
      <div style="font-size:16px;font-weight:700;color:var(--sky);">${c.symbol}${converted}</div>
    </div>`;
  }).join('');
}

// ═══════ ITINERARY BUILDER ═══════
let currentItinTripId=null;
function loadItinerary(){
  const trips=DB.getList('trips');
  const sel=document.getElementById('itin-trip-select');
  if(!sel)return;
  const previousTripId=currentItinTripId || sel.value;
  sel.innerHTML=trips.length?trips.map(t=>`<option value="${t.id}">${t.emoji||'✈️'} ${t.name}</option>`).join(''):'<option value="">No trips yet</option>';
  if(!trips.length){
    currentItinTripId=null;
    sel.value='';
  } else {
    const chosenTrip=trips.find(t=>t.id===previousTripId) || trips[0];
    currentItinTripId=chosenTrip.id;
    sel.value=chosenTrip.id;
  }
  renderItinerary();
}
function selectItineraryTrip(tripId){
  currentItinTripId=tripId;
  const sel=document.getElementById('itin-trip-select');
  if(sel)sel.value=tripId;
  renderItinerary();
}
function renderItinerary(){
  const container=document.getElementById('itinerary-content');
  if(!container)return;
  const trips=DB.getList('trips');
  const trip=trips.find(t=>t.id===currentItinTripId);
  if(!trip){
    container.innerHTML=`<div class="empty"><div class="icon">📅</div><h3>No trip selected</h3><p>Create a trip first to start planning your itinerary.</p><button class="btn-primary btn-lg" onclick="showPage('trips')">+ Create a Trip</button></div>`;
    return;
  }
  const days=tripDays(trip.start,trip.end);
  const itinData=DB.getList('itineraries').find(i=>i.tripId===trip.id)||{tripId:trip.id,days:[]};
  // Ensure days array has an entry for every day of the trip
  while(itinData.days.length<days){
    itinData.days.push({activities:[]});
  }
  saveItinerary(itinData);
  let html=`<div class="alert alert-info">✈️ <strong>${trip.name}</strong> — ${fmt(trip.start)} to ${fmt(trip.end)} (${days} day${days!==1?'s':''})</div>`;
  for(let i=0;i<days;i++){
    const dayDate=addDays(trip.start,i);
    const dayActivities=(itinData.days[i]?.activities||[]).slice().sort((a,b)=>(a.time||'').localeCompare(b.time||''));
    html+=`<div class="itin-day-card">
      <div class="itin-day-header">
        <div><div class="itin-day-title">Day ${i+1}</div><div class="itin-day-date">${fmt(dayDate)}</div></div>
        <button class="btn-sm" style="background:rgba(255,255,255,.2);color:#fff;" onclick="openActivityModal(${i})">+ Add Activity</button>
      </div>
      ${dayActivities.length?dayActivities.map(act=>`
        <div class="itin-activity-row">
          <div class="itin-activity-time">${act.time||'--:--'}</div>
          <div class="itin-activity-emoji">${act.emoji||'📌'}</div>
          <div class="itin-activity-body">
            <div class="itin-activity-name">${act.place}</div>
            ${act.notes?`<div class="itin-activity-notes">${act.notes}</div>`:''}
          </div>
          <button class="btn-sm btn-danger" style="padding:4px 9px;" onclick="deleteActivity(${i},'${act.id}')">✕</button>
        </div>`).join(''):'<div class="itin-activity-row" style="color:var(--ink-light);font-size:13px;">No activities planned yet for this day.</div>'}
    </div>`;
  }
  container.innerHTML=html;
}
function addDays(dateStr,n){
  const d=new Date(dateStr);
  if(isNaN(d))return dateStr;
  d.setDate(d.getDate()+n);
  return d.toISOString().split('T')[0];
}
function saveItinerary(itinData){
  const all=DB.getList('itineraries').filter(i=>i.tripId!==itinData.tripId);
  all.push(itinData);
  DB.set('itineraries',all);
}
function openActivityModal(dayIndex){
  document.getElementById('act-day-index').value=dayIndex;
  document.getElementById('act-time').value='09:00';
  document.getElementById('act-place').value='';
  document.getElementById('act-notes').value='';
  document.getElementById('act-emoji').value='🏛️';
  openModal('modal-activity');
}
function saveActivity(){
  if(!currentItinTripId){showToast('Select a trip before adding an activity.');return;}
  const dayIndex=+document.getElementById('act-day-index').value;
  const place=document.getElementById('act-place').value.trim();
  if(!place){showToast('Enter a place or activity name.');return;}
  const activity={
    id:uid(),
    time:document.getElementById('act-time').value,
    emoji:document.getElementById('act-emoji').value,
    place,
    notes:document.getElementById('act-notes').value
  };
  const all=DB.getList('itineraries');
  let itinData=all.find(i=>i.tripId===currentItinTripId);
  if(!itinData){itinData={tripId:currentItinTripId,days:[]};}
  while(itinData.days.length<=dayIndex)itinData.days.push({activities:[]});
  itinData.days[dayIndex].activities.push(activity);
  saveItinerary(itinData);
  closeModal('modal-activity');
  showToast('Activity added! 📌');
  renderItinerary();
}
async function deleteActivity(dayIndex,activityId){
  const ok=await showConfirm('Remove this activity from your itinerary?','Delete activity?','📌','Delete','btn-danger');
  if(!ok)return;
  const all=DB.getList('itineraries');
  const itinData=all.find(i=>i.tripId===currentItinTripId);
  if(!itinData)return;
  if(itinData.days[dayIndex]){
    itinData.days[dayIndex].activities=itinData.days[dayIndex].activities.filter(a=>a.id!==activityId);
  }
  saveItinerary(itinData);
  showToast('Activity removed.');
  renderItinerary();
}

// ═══════ ACHIEVEMENTS (Fix 1: was completely missing) ═══════
const ACHIEVEMENTS=[
  {id:'first_trip',icon:'✈️',name:'First Trip',desc:'Create your first trip',check:({trips})=>trips.length>=1},
  {id:'five_trips',icon:'🗺️',name:'Trip Planner',desc:'Plan 5 trips or more',check:({trips})=>trips.length>=5},
  {id:'ten_trips',icon:'🌍',name:'Globe Trotter',desc:'Plan 10 trips or more',check:({trips})=>trips.length>=10},
  {id:'first_expense',icon:'💰',name:'Money Tracker',desc:'Add your first expense',check:({expenses})=>expenses.length>=1},
  {id:'ten_expenses',icon:'📊',name:'Smart Spender',desc:'Add 10 expenses or more',check:({expenses})=>expenses.length>=10},
  {id:'first_photo',icon:'📸',name:'Memory Keeper',desc:'Save your first travel photo',check:({memories})=>memories.some(m=>m.photo)},
  {id:'five_memories',icon:'🌟',name:'Story Builder',desc:'Save 5 memories or more',check:({memories})=>memories.length>=5},
  {id:'first_waypoint',icon:'📍',name:'Map Marker',desc:'Add your first place on the map',check:({waypoints})=>waypoints.length>=1},
  {id:'viewpoint_hunter',icon:'🏔️',name:'Viewpoint Finder',desc:'Save 3 scenic stops',check:({waypoints})=>waypoints.filter(w=>w.type==='viewpoint').length>=3},
  {id:'budget_hero',icon:'🏆',name:'Budget Saver',desc:'Finish a trip within budget',check:({trips,expenses})=>{
    return trips.filter(t=>t.status==='completed'&&t.budget>0).some(t=>{
      const spent=expenses.filter(e=>e.tripId===t.id).reduce((a,e)=>a+e.amount,0);
      return spent>0&&spent<=t.budget;
    });
  }},
  {id:'hotel_collector',icon:'🏨',name:'Stay Explorer',desc:'Save 5 hotel stays or more',check:({places})=>places.filter(p=>p.type==='hotel').length>=5},
  {id:'foodie',icon:'🍽️',name:'Food Explorer',desc:'Save 5 restaurants or more',check:({places})=>places.filter(p=>p.type==='restaurant').length>=5},
  {id:'sos_ready',icon:'🆘',name:'Safety Ready',desc:'Add one emergency contact',check:()=>DB.getList('sos_contacts').length>=1},
  {id:'diary_writer',icon:'📝',name:'Journal Keeper',desc:'Create a travel diary entry',check:()=>DB.getList('diary_entries').length>=1},
  {id:'completer',icon:'🏁',name:'Trip Finisher',desc:'Complete 3 trips or more',check:({trips})=>trips.filter(t=>t.status==='completed').length>=3},
];

function loadAchievements(){
  const trips=DB.getList('trips');
  const expenses=DB.getList('expenses');
  const places=DB.getList('places');
  const waypoints=DB.getList('waypoints');
  const memories=DB.getList('memories');
  const ctx={trips,expenses,places,waypoints,memories};

  const unlocked=ACHIEVEMENTS.filter(a=>{try{return a.check(ctx);}catch{return false;}});
  const locked=ACHIEVEMENTS.filter(a=>{try{return !a.check(ctx);}catch{return true;}});

  const statsEl=document.getElementById('achievement-stats');
  if(statsEl){
    statsEl.innerHTML=`
      <div class="stat-card sky"><div class="big">${unlocked.length}</div><div class="lbl">Earned</div></div>
      <div class="stat-card terra"><div class="big">${locked.length}</div><div class="lbl">Still to Earn</div></div>
      <div class="stat-card forest"><div class="big">${ACHIEVEMENTS.length}</div><div class="lbl">Total Badges</div></div>
      <div class="stat-card warn"><div class="big">${Math.round(unlocked.length/ACHIEVEMENTS.length*100)}%</div><div class="lbl">Progress</div></div>
    `;
  }

  const grid=document.getElementById('achievements-grid');
  if(!grid)return;

  const renderBadge=(a,isUnlocked)=>`
    <div class="badge-card ${isUnlocked?'unlocked':'locked'}">
      <div class="badge-icon">${a.icon}</div>
      <div class="badge-name">${a.name}</div>
      <div class="badge-desc">${a.desc}</div>
      ${isUnlocked?'<div style="font-size:10px;color:var(--success);margin-top:6px;font-weight:700;">✓ Earned</div>':'<div style="font-size:10px;color:var(--ink-light);margin-top:6px;">🔒 Not yet</div>'}
    </div>`;

  grid.innerHTML=
    unlocked.map(a=>renderBadge(a,true)).join('')+
    locked.map(a=>renderBadge(a,false)).join('');

  // Check for newly unlocked badges and toast them
  const prevUnlocked=DB.get('prev_unlocked')||[];
  unlocked.forEach(a=>{
    if(!prevUnlocked.includes(a.id)){
      showToast('🏆 Badge unlocked: '+a.name+'!',3500);
      addNotification('🏆 Badge Unlocked!','You earned the "'+a.name+'" badge. '+a.desc,a.icon,'success');
    }
  });
  DB.set('prev_unlocked',unlocked.map(a=>a.id));
}

// ═══════ INIT ═══════
// ═══════ PWA: INSTALL + OFFLINE + TRIP REMINDERS (Fix 15) ═══════
function setupPWA(){
  // Inject manifest as a blob URL (no separate file needed)
  try{
    const manifest={
      name:'TripTrace — Travel Journal',
      short_name:'TripTrace',
      start_url:'.',
      display:'standalone',
      background_color:'#1a1a2e',
      theme_color:'#1a1a2e',
      icons:[
        {src:'data:image/svg+xml,%3Csvg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"%3E%3Crect width="100" height="100" rx="20" fill="%231a6985"/%3E%3Ctext x="50" y="65" font-size="50" text-anchor="middle" fill="white"%3E%E2%9C%88%EF%B8%8F%3C/text%3E%3C/svg%3E',sizes:'192x192',type:'image/svg+xml'}
      ]
    };
    const manifestBlob=new Blob([JSON.stringify(manifest)],{type:'application/json'});
    const manifestURL=URL.createObjectURL(manifestBlob);
    const link=document.createElement('link');
    link.rel='manifest';
    link.href=manifestURL;
    document.head.appendChild(link);
  }catch(e){console.warn('Manifest injection failed',e);}

  // Register a service worker with real runtime caching (offline shell) via blob URL
  if('serviceWorker' in navigator){
    try{
      const appShellURL=JSON.stringify(location.href);
      const swCode=`
        const CACHE_NAME='triptrace-v2';
        const APP_SHELL=${appShellURL};
        self.addEventListener('install',e=>{
          self.skipWaiting();
          e.waitUntil(caches.open(CACHE_NAME).then(cache=>cache.add(APP_SHELL)).catch(()=>{}));
        });
        self.addEventListener('activate',e=>{
          self.clients.claim();
          e.waitUntil(caches.keys().then(keys=>Promise.all(keys.filter(k=>k!==CACHE_NAME).map(k=>caches.delete(k)))));
        });
        self.addEventListener('fetch',e=>{
          if(e.request.method!=='GET')return;
          e.respondWith(
            fetch(e.request).then(res=>{
              const resClone=res.clone();
              caches.open(CACHE_NAME).then(cache=>cache.put(e.request,resClone)).catch(()=>{});
              return res;
            }).catch(()=>caches.match(e.request).then(cached=>cached||caches.match(APP_SHELL)))
          );
        });
      `;
      const swBlob=new Blob([swCode],{type:'application/javascript'});
      const swURL=URL.createObjectURL(swBlob);
      navigator.serviceWorker.register(swURL).catch(()=>{/* offline support unavailable, app still works online */});
    }catch(e){console.warn('Service worker registration failed',e);}
  }
}
function requestNotificationPermission(){
  if('Notification' in window && Notification.permission==='default'){
    Notification.requestPermission();
  }
}
function checkTripReminders(){
  if(!('Notification' in window)||Notification.permission!=='granted')return;
  const trips=DB.getList('trips').filter(t=>t.status==='planned'&&t.start);
  const remindersSent=DB.get('trip_reminders_sent')||[];
  const now=new Date();
  trips.forEach(trip=>{
    const startDate=new Date(trip.start);
    const hoursUntil=(startDate-now)/(1000*60*60);
    if(hoursUntil>0&&hoursUntil<=24&&!remindersSent.includes(trip.id)){
      try{
        new Notification('✈️ Trip Reminder — TripTrace',{
          body:`Your "${trip.name}" trip to ${trip.destination||'your destination'} starts tomorrow! Check your packing list.`,
          icon:'data:image/svg+xml,%3Csvg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"%3E%3Crect width="100" height="100" rx="20" fill="%231a6985"/%3E%3Ctext x="50" y="65" font-size="50" text-anchor="middle" fill="white"%3E%E2%9C%88%EF%B8%8F%3C/text%3E%3C/svg%3E'
        });
      }catch(e){}
      addNotification('✈️ Trip Reminder',`Your "${trip.name}" trip starts tomorrow!`,'✈️','alert');
      remindersSent.push(trip.id);
      DB.set('trip_reminders_sent',remindersSent);
    }
  });
}

// ═══════ DARK MODE (Fix 12) ═══════
function toggleDarkMode(){
  const isDark=document.body.classList.toggle('dark');
  localStorage.setItem('tt_darkmode',isDark?'1':'0');
  document.getElementById('dark-toggle-btn').textContent=isDark?'☀️':'🌙';
}
function applyDarkModePreference(){
  const isDark=localStorage.getItem('tt_darkmode')==='1';
  if(isDark){
    document.body.classList.add('dark');
    const btn=document.getElementById('dark-toggle-btn');
    if(btn)btn.textContent='☀️';
  }
}
const packingTemplates = {
  general: [
    'Clothes',
    'Innerwear',
    'Toiletries',
    'Phone charger',
    'Power bank',
    'Wallet',
    'ID proof',
    'Medicines',
    'Water bottle',
    'Sunglasses'
  ],

  hill: [
    'Warm jacket',
    'Sweater',
    'Thermal wear',
    'Raincoat',
    'Comfortable shoes',
    'Woollen socks',
    'Gloves',
    'Cap / beanie',
    'Medicines',
    'Phone charger',
    'Power bank',
    'ID proof',
    'Water bottle'
  ],

  beach: [
    'Swimwear',
    'Sunscreen',
    'Sunglasses',
    'Hat',
    'Flip-flops',
    'Beach towel',
    'Water bottle',
    'Light clothes',
    'Phone charger',
    'Power bank',
    'ID proof'
  ],

  adventure: [
    'Trekking shoes',
    'Backpack',
    'Water bottle',
    'First aid kit',
    'Torch',
    'Power bank',
    'Raincoat',
    'Energy bars',
    'Extra socks',
    'ID proof',
    'Phone charger'
  ],

  business: [
    'Formal clothes',
    'Laptop',
    'Laptop charger',
    'Phone charger',
    'ID proof',
    'Business cards',
    'Notebook',
    'Pen',
    'Toiletries',
    'Wallet'
  ]
};

function loadPackingTrips(){
  const trips = DB.getList('trips');
  const select = document.getElementById('packing-trip-select');

  if(!select) return;

  select.innerHTML = trips.length
    ? trips.map(trip =>
        `<option value="${trip.id}">${trip.name || 'Unnamed Trip'} — ${trip.destination || 'No destination'}</option>`
      ).join('')
    : '<option value="">No trips available</option>';

  loadPackingList();
}

function getPackingStorageKey(tripId){
  return `packing_list_${tripId}`;
}

function generatePackingList(){
  const tripId = document.getElementById('packing-trip-select').value;
  const tripType = document.getElementById('packing-trip-type').value;

  if(!tripId){
    showToast('Please create or select a trip first.');
    return;
  }

  const items = packingTemplates[tripType].map(item => ({
    id: Date.now() + Math.random(),
    name: item,
    packed: false
  }));

  DB.set(getPackingStorageKey(tripId), items);

  loadPackingList();
  showToast('Smart packing checklist created!');
}

function loadPackingList(){
  const tripId = document.getElementById('packing-trip-select')?.value;
  const container = document.getElementById('packing-list-content');

  if(!container || !tripId){
    return;
  }

  const items = DB.get(getPackingStorageKey(tripId)) || [];

  if(items.length === 0){
    container.innerHTML = `
      <div class="empty">
        <div class="icon">🎒</div>
        <h3>No packing list yet</h3>
        <p>Select a trip type and generate your smart checklist.</p>
      </div>
    `;

    updatePackingProgress([]);
    return;
  }

  container.innerHTML = items.map(item => `
    <div class="waypoint" style="cursor:default;">
      <input
        type="checkbox"
        ${item.packed ? 'checked' : ''}
        onchange="togglePackingItem('${tripId}', '${item.id}')"
        style="width:18px;height:18px;accent-color:var(--sky);"
      >

      <div class="wp-info">
        <div class="wp-name" style="${item.packed ? 'text-decoration:line-through;color:var(--ink-light);' : ''}">
          ${item.name}
        </div>
      </div>

      <button class="btn-sm btn-danger" onclick="deletePackingItem('${tripId}', '${item.id}')">
        ×
      </button>
    </div>
  `).join('');

  updatePackingProgress(items);
}

function togglePackingItem(tripId, itemId){
  const items = DB.get(getPackingStorageKey(tripId)) || [];

  const updatedItems = items.map(item => {
    if(String(item.id) === String(itemId)){
      item.packed = !item.packed;
    }
    return item;
  });

  DB.set(getPackingStorageKey(tripId), updatedItems);
  loadPackingList();
}

function addCustomPackingItem(){
  const tripId = document.getElementById('packing-trip-select').value;
  const input = document.getElementById('custom-packing-item');
  const itemName = input.value.trim();

  if(!tripId){
    showToast('Select a trip first.');
    return;
  }

  if(!itemName){
    showToast('Enter an item name.');
    return;
  }

  const items = DB.get(getPackingStorageKey(tripId)) || [];

  items.push({
    id: Date.now() + Math.random(),
    name: itemName,
    packed: false
  });

  DB.set(getPackingStorageKey(tripId), items);

  input.value = '';
  loadPackingList();
  showToast('Item added to packing list.');
}

function deletePackingItem(tripId, itemId){
  const items = DB.get(getPackingStorageKey(tripId)) || [];

  const updatedItems = items.filter(item => String(item.id) !== String(itemId));

  DB.set(getPackingStorageKey(tripId), updatedItems);

  loadPackingList();
  showToast('Packing item removed.');
}

function updatePackingProgress(items){
  const total = items.length;
  const packed = items.filter(item => item.packed).length;
  const percent = total ? Math.round((packed / total) * 100) : 0;

  const progressText = document.getElementById('packing-progress-text');
  const progressBar = document.getElementById('packing-progress-bar');

  if(progressText){
    progressText.textContent = `${packed} / ${total} packed`;
  }

  if(progressBar){
    progressBar.style.width = `${percent}%`;
  }
}

function init(){
  applyDarkModePreference();
  setupPWA();
  // Real login state now comes from Firebase Auth (below), not a cached
  // localStorage flag, so the same account works from any device.
  showPage('auth');
  // Safety net: if Firebase never finishes loading (ad-blocker, offline on
  // first visit, slow mobile connection, etc.), don't leave the user staring
  // at a spinner forever. Mobile networks (especially cellular data, or
  // links opened from inside apps like WhatsApp/Instagram) can take much
  // longer than a laptop on wifi to download the Firebase SDK files, so we
  // give it a much longer grace period (25s) before giving up, and we keep
  // checking every second in case it finishes late instead of only checking
  // once at a fixed cutoff.
  const FB_READY_TIMEOUT_MS = 25000;
  const FB_READY_CHECK_INTERVAL_MS = 1000;
  let fbWaitElapsed = 0;
  const fbReadyChecker = setInterval(()=>{
    if(window.FB){
      clearInterval(fbReadyChecker);
      return;
    }
    fbWaitElapsed += FB_READY_CHECK_INTERVAL_MS;
    if(fbWaitElapsed >= FB_READY_TIMEOUT_MS){
      clearInterval(fbReadyChecker);
      const overlay = document.getElementById('session-loading');
      if(overlay && overlay.style.display!=='none' && !window.FB){
        overlay.style.display='none';
        showToast("Couldn't reach the login service. This can happen on a slow connection — please check your internet and reload the page. If you opened this link from inside an app like WhatsApp or Instagram, try opening it directly in Chrome or Safari instead.");
      }
    }
  }, FB_READY_CHECK_INTERVAL_MS);
}
init();

// Runs once Firebase (loaded as a module at the bottom of the page) is ready.
window.addEventListener('fb-ready', ()=>{
  window.FB.onAuthChange(async (user)=>{
    const overlay = document.getElementById('session-loading');
    if(user){
      await hydrateFromCloud(user);
      setSyncStatus(navigator.onLine ? 'saved' : 'offline');
      updateNavUser();
      updateNotificationBadge();
      checkSmartNotifications();
      requestNotificationPermission();
      checkTripReminders();
      setInterval(checkSmartNotifications,60000);
      setInterval(checkTripReminders,3600000);
      setTimeout(()=>loadAchievements(),1500);
      showPage('home');
      if(justAuthenticated){
        justAuthenticated=false;
        showToast('Welcome, '+(currentUser.name||'Traveller')+'! 🌍');
      }
    }else{
      currentUser=null;
      cloudUID=null;
      showPage('auth');
    }
    if(overlay) overlay.style.display='none';
  });
});
</script>

<!-- Add Activity (Itinerary Builder) -->
<div class="modal-overlay" id="modal-activity">
  <div class="modal">
    <div class="modal-header"><h3>📌 Add Activity</h3><button class="modal-close" onclick="closeModal('modal-activity')">×</button></div>
    <div class="modal-body">
      <input type="hidden" id="act-day-index">
      <div class="form-group"><label>Time</label><input type="time" id="act-time" value="09:00"></div>
      <div class="form-group"><label>Activity Type</label>
        <select id="act-emoji">
          <option value="🍳">🍳 Breakfast</option><option value="🍽️">🍽️ Lunch/Dinner</option>
          <option value="🏛️">🏛️ Sightseeing</option><option value="🏔️">🏔️ Adventure</option>
          <option value="🛍️">🛍️ Shopping</option><option value="🚗">🚗 Travel/Transit</option>
          <option value="🏨">🏨 Check-in/out</option><option value="🎭">🎭 Entertainment</option>
          <option value="🛌">🛌 Rest</option><option value="📸">📸 Photo stop</option>
          <option value="⛪">⛪ Religious/Heritage</option><option value="🏖️">🏖️ Beach/Relax</option>
        </select>
      </div>
      <div class="form-group"><label>Place / Activity Name *</label><input type="text" id="act-place" placeholder="e.g. Doddabetta Peak"></div>
      <div class="form-group"><label>Notes</label><textarea id="act-notes" rows="2" placeholder="Booking ref, tips, what to bring..."></textarea></div>
    </div>
    <div class="modal-footer">
      <button class="btn-sm btn-ghost" onclick="closeModal('modal-activity')">Cancel</button>
      <button class="btn-sm btn-sky" onclick="saveActivity()">Save Activity</button>
    </div>
  </div>
</div>

<!-- Custom Confirm Dialog (Fix 6: replaces browser confirm()) -->
<div class="confirm-overlay" id="confirm-overlay">
  <div class="confirm-box">
    <div class="confirm-header">
      <span class="confirm-icon" id="confirm-icon">⚠️</span>
      <span class="confirm-title" id="confirm-title">Are you sure?</span>
    </div>
    <div class="confirm-body" id="confirm-body">This action cannot be undone.</div>
    <div class="confirm-footer">
      <button class="btn-sm btn-ghost" onclick="rejectConfirm()">Cancel</button>
      <button class="btn-sm btn-danger" id="confirm-ok-btn" onclick="resolveConfirm()">Confirm</button>
    </div>
  </div>
</div>
<script type="module"
src="firebase.js"></script>
</body>
</html>

FIREBASE : 
// ═══════════════════════════════════════════════════════════
//  Firebase init — Auth (email/password) + Firestore (cloud
//  data, with offline caching so the app still works with a
//  flaky connection)
// ═══════════════════════════════════════════════════════════
import { initializeApp } from "https://www.gstatic.com/firebasejs/12.0.0/firebase-app.js";
import {
  getAuth,
  createUserWithEmailAndPassword,
  signInWithEmailAndPassword,
  onAuthStateChanged,
  signOut,
  updateProfile
} from "https://www.gstatic.com/firebasejs/12.0.0/firebase-auth.js";
import {
  initializeFirestore,
  persistentLocalCache,
  persistentSingleTabManager,
  doc,
  getDoc,
  setDoc
} from "https://www.gstatic.com/firebasejs/12.0.0/firebase-firestore.js";

const firebaseConfig = {
  apiKey: "AIzaSyD94vGC_ZX_peULEOt0aNyRT0RrtiYcKSE",
  authDomain: "triptrace-906d7.firebaseapp.com",
  projectId: "triptrace-906d7",
  storageBucket: "triptrace-906d7.firebasestorage.app",
  messagingSenderId: "97920741808",
  appId: "1:97920741808:web:554337ab92baabba63a675",
  measurementId: "G-EWEGFT8305"
};

const app = initializeApp(firebaseConfig);
const auth = getAuth(app);

// Offline persistence: cached reads/writes survive refreshes and a dropped
// connection, then sync automatically once back online.
let db;
try{
  db = initializeFirestore(app, {
    localCache: persistentLocalCache({ tabManager: persistentSingleTabManager() })
  });
}catch(e){
  console.warn('Offline persistence unavailable (private browsing / unsupported browser); falling back to online-only Firestore.', e);
  db = initializeFirestore(app, {});
}

window.FB = {
  auth,

  // Create a brand-new account in Firebase Authentication + an empty
  // Firestore profile document for that user.
  async signUp(name, email, password){
    const cred = await createUserWithEmailAndPassword(auth, email, password);
    await updateProfile(cred.user, { displayName: name });
    await setDoc(doc(db, "users", cred.user.uid), {
      profile: { name, email, currency: "₹", joined: new Date().toISOString().slice(0, 10) },
      data: {}
    });
    return cred.user;
  },

  // Sign in — works from any browser/device with the same email + password.
  async signIn(email, password){
    const cred = await signInWithEmailAndPassword(auth, email, password);
    return cred.user;
  },

  async signOutUser(){
    return signOut(auth);
  },

  // Fires immediately with the current session (or null), then again on
  // every login/logout.
  onAuthChange(callback){
    return onAuthStateChanged(auth, callback);
  },

  // Pulls the user's saved profile + app data down from the cloud.
  async loadUserData(uid){
    const snap = await getDoc(doc(db, "users", uid));
    return snap.exists() ? snap.data() : null;
  },

  // Merge-writes one or more app-data keys (trips, expenses, etc.) up to
  // the cloud without touching the rest of the document.
  async saveUserData(uid, keyValuePairs){
    await setDoc(doc(db, "users", uid), { data: keyValuePairs }, { merge: true });
  },

  // Merge-writes the user's profile (name/email/currency/etc.) up to the cloud.
  async saveProfile(uid, profile){
    await setDoc(doc(db, "users", uid), { profile }, { merge: true });
  }
};

window.dispatchEvent(new Event("fb-ready"));
console.log("Firebase Connected Successfully! (Auth + Firestore, offline-capable)");
