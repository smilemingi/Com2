<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>컴프야 2025</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Black+Han+Sans&family=Noto+Sans+KR:wght@400;700;900&display=swap');

  * { margin:0; padding:0; box-sizing:border-box; -webkit-tap-highlight-color:transparent; user-select:none; }
  body { background:#0a0a14; font-family:'Noto Sans KR',sans-serif; overflow:hidden; width:100vw; height:100vh; }

  /* ── SCREENS ── */
  .screen { position:absolute; inset:0; display:none; }
  .screen.active { display:flex; flex-direction:column; }

  /* ══════════════ TITLE SCREEN ══════════════ */
  #titleScreen {
    background: radial-gradient(ellipse at 50% 30%, #1a2a4a 0%, #0a0a14 70%);
    align-items:center; justify-content:center;
  }
  .title-field {
    position:absolute; bottom:0; left:0; right:0; height:45%;
    background: linear-gradient(to top, #2d5a1b 0%, #3d7a25 40%, #4a9030 60%, transparent 100%);
    clip-path: ellipse(120% 60% at 50% 100%);
  }
  .title-field::before {
    content:''; position:absolute; inset:0;
    background: repeating-linear-gradient(90deg, rgba(255,255,255,0.03) 0px, rgba(255,255,255,0.03) 1px, transparent 1px, transparent 30px);
  }
  .title-logo { position:relative; z-index:10; text-align:center; margin-bottom:20px; }
  .title-logo h1 { font-family:'Black Han Sans',sans-serif; font-size:52px; color:#fff;
    text-shadow: 0 0 20px rgba(255,180,0,0.8), 0 4px 8px rgba(0,0,0,0.8);
    letter-spacing:2px; }
  .title-logo .subtitle { color:#ffd700; font-size:14px; font-weight:700; letter-spacing:4px; margin-top:4px; }
  .title-logo .year { font-family:'Black Han Sans',sans-serif; font-size:28px; color:#ff6b35;
    text-shadow:0 0 15px rgba(255,107,53,0.7); }
  .title-btn {
    position:relative; z-index:10; margin-top:30px;
    background: linear-gradient(135deg, #ff6b35, #ff3d00);
    border:none; color:#fff; font-family:'Black Han Sans',sans-serif; font-size:22px;
    padding:16px 60px; border-radius:50px;
    box-shadow: 0 6px 20px rgba(255,107,53,0.5);
    cursor:pointer; animation: pulse 1.5s infinite;
  }
  @keyframes pulse { 0%,100%{transform:scale(1)} 50%{transform:scale(1.05)} }
  .stars { position:absolute; inset:0; overflow:hidden; }
  .star { position:absolute; background:#fff; border-radius:50%; animation:twinkle 3s infinite; }
  @keyframes twinkle { 0%,100%{opacity:0.2} 50%{opacity:1} }

  /* ══════════════ TEAM SELECT ══════════════ */
  #teamScreen {
    background: linear-gradient(160deg, #0f1923 0%, #1a0a2e 100%);
    padding:20px; align-items:center;
  }
  .screen-title { font-family:'Black Han Sans',sans-serif; font-size:24px; color:#ffd700;
    text-align:center; margin-bottom:20px; text-shadow:0 0 10px rgba(255,215,0,0.5); }
  .team-grid { display:grid; grid-template-columns:repeat(2,1fr); gap:12px; width:100%; max-width:420px; }
  .team-card {
    background: rgba(255,255,255,0.05); border:2px solid rgba(255,255,255,0.1);
    border-radius:16px; padding:16px; text-align:center; cursor:pointer;
    transition:all 0.2s; position:relative; overflow:hidden;
  }
  .team-card:active { transform:scale(0.95); }
  .team-card.selected { border-color:#ffd700; background:rgba(255,215,0,0.1); }
  .team-card .team-emoji { font-size:36px; display:block; margin-bottom:6px; }
  .team-card .team-name { font-family:'Black Han Sans',sans-serif; font-size:16px; color:#fff; }
  .team-card .team-city { font-size:11px; color:rgba(255,255,255,0.5); margin-top:2px; }
  .team-card .team-power { font-size:10px; color:#ffd700; margin-top:4px; }
  .vs-label { color:#ff6b35; font-family:'Black Han Sans',sans-serif; font-size:18px; margin:10px 0; }
  .start-game-btn {
    margin-top:20px; background:linear-gradient(135deg,#ffd700,#ff6b35);
    border:none; color:#000; font-family:'Black Han Sans',sans-serif; font-size:20px;
    padding:14px 50px; border-radius:50px; cursor:pointer;
    box-shadow:0 6px 20px rgba(255,215,0,0.4);
    opacity:0.5; pointer-events:none; transition:all 0.3s;
  }
  .start-game-btn.ready { opacity:1; pointer-events:all; }

  /* ══════════════ GAME SCREEN ══════════════ */
  #gameScreen { background:#1a1a2e; }

  /* SCOREBOARD */
  .scoreboard {
    background:linear-gradient(180deg,#0d1117,#1a2332);
    padding:8px 12px; display:flex; align-items:center; gap:8px;
    border-bottom:2px solid rgba(255,215,0,0.3); z-index:100; flex-shrink:0;
  }
  .score-team { display:flex; align-items:center; gap:6px; flex:1; }
  .score-team .t-logo { width:28px; height:28px; border-radius:50%; display:flex; align-items:center; justify-content:center; font-size:14px; }
  .score-team .t-name { font-family:'Black Han Sans',sans-serif; font-size:13px; color:#fff; }
  .score-team .t-score { font-family:'Black Han Sans',sans-serif; font-size:22px; color:#fff; min-width:24px; text-align:center; }
  .score-sep { color:rgba(255,255,255,0.3); font-size:18px; }
  .inning-info { text-align:center; }
  .inning-info .inning-num { font-family:'Black Han Sans',sans-serif; font-size:16px; color:#ffd700; }
  .inning-info .inning-arrow { font-size:10px; color:#ff6b35; }
  .bases-display { display:grid; grid-template-columns:1fr 1fr; gap:2px; width:30px; height:30px; }
  .base-dot { width:12px; height:12px; background:rgba(255,255,255,0.1); border:1px solid rgba(255,255,255,0.3); transform:rotate(45deg); }
  .base-dot.occupied { background:#ffd700; border-color:#ff6b35; }
  .count-display { display:flex; flex-direction:column; gap:2px; font-size:9px; color:rgba(255,255,255,0.6); }
  .count-row { display:flex; gap:3px; align-items:center; }
  .count-dot { width:7px; height:7px; border-radius:50%; background:rgba(255,255,255,0.15); border:1px solid rgba(255,255,255,0.3); }
  .count-dot.s { background:#ff4444; }
  .count-dot.b { background:#44bb44; }
  .count-dot.o { background:#ff8c00; }

  /* FIELD */
  .field-container { flex:1; position:relative; overflow:hidden; }
  
  /* ── PITCHER VIEW ── */
  #pitcherView { position:absolute; inset:0; display:flex; flex-direction:column; }
  .field-bg {
    flex:1; position:relative; overflow:hidden;
    background: radial-gradient(ellipse at 50% 100%, #2d5a1b 0%, #1e3d12 60%, #0f2008 100%);
  }
  .grass-lines {
    position:absolute; inset:0;
    background: repeating-linear-gradient(0deg, rgba(255,255,255,0.02) 0px, rgba(255,255,255,0.02) 1px, transparent 1px, transparent 24px);
  }
  .infield-dirt {
    position:absolute; bottom:0; left:50%; transform:translateX(-50%);
    width:200%; height:55%; 
    background: radial-gradient(ellipse at 50% 100%, #8b5e3c 0%, #6b4423 40%, transparent 70%);
  }
  .pitcher-mound {
    position:absolute; bottom:25%; left:50%; transform:translateX(-50%);
    width:60px; height:30px;
    background: radial-gradient(ellipse, #9b6e4c, #7a5235);
    border-radius:50%;
  }
  .home-plate {
    position:absolute; bottom:8%; left:50%; transform:translateX(-50%);
    width:30px; height:20px; background:#fff; clip-path:polygon(0 0,100% 0,100% 70%,50% 100%,0 70%);
  }
  .batter-box-left {
    position:absolute; bottom:10%; left:calc(50% - 50px);
    width:35px; height:55px; border:2px solid rgba(255,255,255,0.4);
    border-radius:4px;
  }
  .batter-box-right {
    position:absolute; bottom:10%; left:calc(50% + 15px);
    width:35px; height:55px; border:2px solid rgba(255,255,255,0.4);
    border-radius:4px;
  }

  /* Stadium walls */
  .outfield-wall {
    position:absolute; top:5%; left:5%; right:5%;
    height:60px; background:linear-gradient(180deg,#1a3a5c,#0d1f33);
    border-radius:50% 50% 0 0 / 30px 30px 0 0;
    border:3px solid #2a5a8c;
  }
  .wall-ads {
    position:absolute; top:10%; left:8%; right:8%; height:35px;
    display:flex; gap:4px; overflow:hidden;
  }
  .wall-ad { flex:1; background:rgba(255,255,255,0.05); border-radius:4px;
    display:flex; align-items:center; justify-content:center;
    font-size:9px; color:rgba(255,255,255,0.4); font-weight:700; letter-spacing:1px; }
  .scoreboard-big {
    position:absolute; top:2%; left:50%; transform:translateX(-50%);
    background:#0d1117; border:2px solid #2a5a8c; border-radius:8px;
    padding:4px 12px; font-family:'Black Han Sans',sans-serif; font-size:11px;
    color:#ffd700; white-space:nowrap;
  }
  .foul-lines {
    position:absolute; bottom:8%; left:50%; transform-origin:bottom center;
    width:2px; height:45%; background:rgba(255,255,255,0.3);
    transform:translateX(-50%) rotate(-30deg);
  }
  .foul-line-right {
    position:absolute; bottom:8%; left:50%; transform-origin:bottom center;
    width:2px; height:45%; background:rgba(255,255,255,0.3);
    transform:translateX(-50%) rotate(30deg);
  }

  /* PITCHER SPRITE */
  .pitcher-sprite {
    position:absolute; bottom:22%; left:50%; transform:translateX(-50%);
    width:60px; height:90px; cursor:pointer;
  }
  .pitcher-body { position:relative; width:100%; height:100%; }
  /* Head */
  .p-head { position:absolute; top:0; left:50%; transform:translateX(-50%); width:28px; height:28px; border-radius:50%; }
  .p-cap { position:absolute; top:0; left:0; right:0; height:14px; border-radius:14px 14px 0 0; }
  .p-face { position:absolute; top:10px; left:0; right:0; bottom:0; border-radius:0 0 50% 50%; }
  /* Eyes */
  .p-eyes { position:absolute; top:14px; left:0; right:0; display:flex; justify-content:center; gap:5px; }
  .p-eye { width:4px; height:4px; border-radius:50%; background:#333; }
  /* Body */
  .p-torso { position:absolute; top:25px; left:50%; transform:translateX(-50%); width:32px; height:30px; border-radius:4px; }
  .p-number { position:absolute; top:30px; left:50%; transform:translateX(-50%); font-size:9px; font-weight:900; color:rgba(255,255,255,0.8); }
  /* Arms */
  .p-arm-left { position:absolute; top:28px; left:2px; width:12px; height:22px; border-radius:6px; transform-origin:top center; }
  .p-arm-right { position:absolute; top:28px; right:2px; width:12px; height:22px; border-radius:6px; transform-origin:top center; }
  /* Legs */
  .p-leg-left { position:absolute; bottom:0; left:50%; margin-left:-14px; width:12px; height:28px; border-radius:6px; }
  .p-leg-right { position:absolute; bottom:0; left:50%; margin-left:2px; width:12px; height:28px; border-radius:6px; }
  /* Glove */
  .p-glove { position:absolute; top:32px; left:-2px; width:14px; height:14px; border-radius:50%; background:#8b4513; }
  /* Ball */
  .p-ball { position:absolute; top:28px; right:-4px; width:10px; height:10px; border-radius:50%;
    background:radial-gradient(circle at 35% 35%, #fff 0%, #e0e0e0 60%);
    border:1px solid #ccc; }
  /* Wind-up animation */
  .pitcher-sprite.windup .p-arm-right { animation:windup-arm 0.6s ease-in-out; }
  @keyframes windup-arm { 0%{transform:rotate(0)} 40%{transform:rotate(-90deg)} 70%{transform:rotate(20deg)} 100%{transform:rotate(0)} }

  /* BATTER SPRITE (pitcher view) */
  .batter-sprite-far {
    position:absolute; bottom:9%; right:calc(50% - 65px);
    width:40px; height:65px;
  }

  /* CATCHER SPRITE */
  .catcher-sprite {
    position:absolute; bottom:6%; left:50%; transform:translateX(-50%);
    width:38px; height:50px;
  }

  /* ── BATTER VIEW ── */
  #batterView { position:absolute; inset:0; display:none; flex-direction:column; }
  .batter-field {
    flex:1; position:relative; overflow:hidden;
    background: radial-gradient(ellipse at 50% 110%, #2d5a1b 0%, #1e3d12 50%, #0f2008 100%);
  }
  .batter-outfield-wall {
    position:absolute; top:15%; left:-5%; right:-5%; height:80px;
    background:linear-gradient(180deg,#1a3a5c,#0d1f33);
    border:3px solid #2a5a8c;
  }
  .batter-wall-ads {
    position:absolute; top:18%; left:2%; right:2%; height:40px;
    display:flex; gap:6px;
  }
  .batter-ground {
    position:absolute; bottom:0; left:0; right:0; height:45%;
    background: linear-gradient(0deg, #8b5e3c 0%, #7a5235 30%, #5a3a20 60%, transparent 100%);
  }
  .batter-grass {
    position:absolute; bottom:25%; left:0; right:0; height:30%;
    background: linear-gradient(0deg, #2d5a1b 0%, #1e3d12 50%, transparent 100%);
  }

  /* pitcher (far, small) */
  .pitcher-far {
    position:absolute; bottom:28%; left:50%; transform:translateX(-50%);
    width:40px; height:65px;
  }

  /* BATTER SPRITE (large, right side) */
  .batter-sprite-main {
    position:absolute; bottom:18%; right:5%;
    width:80px; height:130px;
  }
  .b-head { position:absolute; top:0; left:50%; transform:translateX(-50%); width:36px; height:36px; border-radius:50%; }
  .b-helmet { position:absolute; top:0; left:50%; transform:translateX(-50%); width:36px; height:22px; border-radius:18px 18px 5px 5px; }
  .b-face { position:absolute; top:10px; left:50%; transform:translateX(-50%); width:30px; height:22px; border-radius:0 0 15px 15px; }
  .b-eyes { position:absolute; top:16px; left:50%; transform:translateX(-50%); width:22px; display:flex; justify-content:space-between; }
  .b-eye { width:5px; height:5px; border-radius:50%; background:#333; }
  .b-torso { position:absolute; top:32px; left:50%; transform:translateX(-50%); width:44px; height:40px; border-radius:6px; }
  .b-number { position:absolute; top:40px; left:50%; transform:translateX(-50%); font-size:12px; font-weight:900; color:rgba(255,255,255,0.85); }
  .b-arm-left { position:absolute; top:34px; left:4px; width:16px; height:28px; border-radius:8px; }
  .b-arm-right { position:absolute; top:34px; right:4px; width:16px; height:28px; border-radius:8px; }
  .b-leg-left { position:absolute; bottom:0; left:50%; margin-left:-22px; width:18px; height:40px; border-radius:9px; }
  .b-leg-right { position:absolute; bottom:0; left:50%; margin-left:4px; width:18px; height:40px; border-radius:9px; }
  .b-bat {
    position:absolute; top:20px; right:-10px; width:10px; height:80px;
    background:linear-gradient(180deg,#8b6914,#6b4a0e,#4a3008);
    border-radius:5px 5px 2px 2px; transform:rotate(-20deg); transform-origin:bottom center;
    box-shadow:inset -2px 0 4px rgba(0,0,0,0.4);
  }
  .b-bat::after { content:''; position:absolute; top:0; left:0; right:0; height:20px;
    background:linear-gradient(180deg,#a07820,transparent);
    border-radius:5px 5px 0 0; }
  /* Batting animation */
  .batter-sprite-main.swinging .b-bat { animation:swing 0.3s ease-out; }
  .batter-sprite-main.swinging .b-arm-right { animation:swing-arm 0.3s ease-out; }
  @keyframes swing { 0%{transform:rotate(-20deg)} 40%{transform:rotate(60deg)} 100%{transform:rotate(10deg)} }
  @keyframes swing-arm { 0%{transform:none} 50%{transform:translateX(10px) rotate(20deg)} 100%{transform:none} }

  /* PITCH ZONE */
  .pitch-zone {
    position:absolute; bottom:22%; left:50%; transform:translateX(-50%);
    width:90px; height:110px; border:2px solid rgba(255,255,255,0.2);
    border-radius:4px;
  }
  .pz-inner { position:absolute; inset:20% 15%; border:1px solid rgba(255,255,255,0.15); }
  .zone-grid {
    position:absolute; inset:0; display:grid; grid-template-columns:repeat(3,1fr); grid-template-rows:repeat(3,1fr);
  }
  .zone-cell { border:1px solid rgba(255,255,255,0.08); }
  .zone-label { position:absolute; bottom:-20px; left:0; right:0; text-align:center; font-size:9px; color:rgba(255,255,255,0.4); }

  /* PITCH BALL */
  .pitch-ball {
    position:absolute; width:12px; height:12px; border-radius:50%;
    background:radial-gradient(circle at 35% 35%, #fff 0%, #e0e0e0 60%);
    border:1px solid #ccc; display:none; pointer-events:none; z-index:50;
    box-shadow:0 0 6px rgba(255,255,255,0.5);
  }
  .pitch-ball.flying { display:block; animation:pitch-fly var(--duration,0.4s) ease-in forwards; }
  @keyframes pitch-fly {
    0% { transform:translate(var(--sx),var(--sy)) scale(0.4); opacity:1; }
    100% { transform:translate(0,0) scale(1); opacity:1; }
  }

  /* SWING ZONE (batter view) */
  .swing-area {
    position:absolute; bottom:10%; left:50%; transform:translateX(-50%);
    width:120px; height:140px; cursor:pointer; z-index:40;
    border:2px solid rgba(255,215,0,0.2); border-radius:8px;
  }
  .swing-area:active { background:rgba(255,215,0,0.05); }
  .swing-hint { position:absolute; top:50%; left:50%; transform:translate(-50%,-50%);
    font-size:11px; color:rgba(255,215,0,0.4); font-weight:700; text-align:center; pointer-events:none; }

  /* HIT EFFECT */
  .hit-effect {
    position:absolute; top:40%; left:50%; transform:translate(-50%,-50%);
    font-family:'Black Han Sans',sans-serif; font-size:48px; color:#ffd700;
    text-shadow:0 0 20px rgba(255,215,0,0.8), 0 0 40px rgba(255,100,0,0.5);
    pointer-events:none; opacity:0; z-index:100;
  }
  .hit-effect.show { animation:hitshow 1s ease-out forwards; }
  @keyframes hitshow { 0%{opacity:1;transform:translate(-50%,-50%) scale(0.5)} 50%{opacity:1;transform:translate(-50%,-80%) scale(1.2)} 100%{opacity:0;transform:translate(-50%,-120%) scale(1)} }

  /* BOTTOM UI */
  .pitch-ui {
    background:linear-gradient(180deg,rgba(10,10,20,0.95),#0a0a14);
    padding:10px 12px; display:flex; flex-direction:column; gap:8px;
    border-top:1px solid rgba(255,215,0,0.2); flex-shrink:0;
  }
  .pitch-buttons { display:grid; grid-template-columns:1fr 1fr; gap:8px; }
  .pitch-btn {
    display:flex; align-items:center; gap:8px; padding:10px 12px;
    background:rgba(255,255,255,0.06); border:1px solid rgba(255,255,255,0.12);
    border-radius:10px; cursor:pointer; transition:all 0.15s; position:relative;
  }
  .pitch-btn:active { transform:scale(0.96); background:rgba(255,255,255,0.12); }
  .pitch-btn .pb-grade {
    width:26px; height:26px; border-radius:6px; display:flex; align-items:center; justify-content:center;
    font-family:'Black Han Sans',sans-serif; font-size:14px; font-weight:900;
  }
  .pb-grade.A { background:linear-gradient(135deg,#4CAF50,#2e7d32); color:#fff; }
  .pb-grade.B { background:linear-gradient(135deg,#2196F3,#1565C0); color:#fff; }
  .pb-grade.C { background:linear-gradient(135deg,#FF9800,#e65100); color:#fff; }
  .pitch-btn .pb-name { font-family:'Black Han Sans',sans-serif; font-size:14px; color:#fff; }
  .pitch-btn .pb-bar { position:absolute; bottom:4px; right:8px; width:40px; height:3px; background:rgba(255,255,255,0.1); border-radius:2px; }
  .pitch-btn .pb-bar-fill { height:100%; border-radius:2px; background:linear-gradient(90deg,#4CAF50,#ffd700); }
  .bottom-actions { display:flex; gap:8px; }
  .action-btn {
    flex:1; padding:8px; background:rgba(255,255,255,0.05); border:1px solid rgba(255,255,255,0.1);
    border-radius:8px; color:rgba(255,255,255,0.7); font-size:11px; font-weight:700;
    cursor:pointer; text-align:center; display:flex; align-items:center; justify-content:center; gap:4px;
  }
  .action-btn:active { background:rgba(255,255,255,0.1); }
  .stamina-bar { display:flex; align-items:center; gap:8px; padding:0 4px; }
  .stamina-label { font-size:10px; color:rgba(255,255,255,0.5); min-width:28px; }
  .stamina-track { flex:1; height:6px; background:rgba(255,255,255,0.1); border-radius:3px; overflow:hidden; }
  .stamina-fill { height:100%; border-radius:3px; background:linear-gradient(90deg,#4CAF50,#8BC34A); transition:width 0.5s; }
  .pitch-count-label { font-size:10px; color:rgba(255,255,255,0.4); margin-left:auto; }

  /* BATTER UI */
  .batter-ui {
    background:linear-gradient(180deg,rgba(10,10,20,0.95),#0a0a14);
    padding:10px 12px; flex-shrink:0;
    border-top:1px solid rgba(255,215,0,0.2);
  }
  .batter-actions { display:flex; gap:8px; margin-bottom:8px; }
  .swing-btn {
    flex:2; padding:12px; background:linear-gradient(135deg,#ff6b35,#ff3d00);
    border:none; border-radius:12px; color:#fff; font-family:'Black Han Sans',sans-serif;
    font-size:18px; cursor:pointer; box-shadow:0 4px 15px rgba(255,107,53,0.4);
    transition:all 0.15s;
  }
  .swing-btn:active { transform:scale(0.96); }
  .take-btn {
    flex:1; padding:12px; background:rgba(255,255,255,0.07); border:1px solid rgba(255,255,255,0.15);
    border-radius:12px; color:rgba(255,255,255,0.7); font-family:'Black Han Sans',sans-serif; font-size:14px;
    cursor:pointer; transition:all 0.15s;
  }
  .take-btn:active { background:rgba(255,255,255,0.12); }
  .batter-info { display:flex; align-items:center; gap:10px; }
  .batter-name-label { font-family:'Black Han Sans',sans-serif; font-size:13px; color:#ffd700; }
  .batter-stats { display:flex; gap:6px; }
  .batter-stat { font-size:10px; color:rgba(255,255,255,0.5); }
  .batter-stat span { color:#4CAF50; font-weight:700; }

  /* COMMENTARY */
  .commentary {
    position:absolute; bottom:0; left:0; right:0; z-index:50;
    background:rgba(0,0,0,0.7); padding:6px 10px;
    border-top:1px solid rgba(255,215,0,0.2);
    display:flex; align-items:center; gap:8px;
  }
  .commentary-icon { width:24px; height:24px; border-radius:50%; background:#333; flex-shrink:0;
    display:flex; align-items:center; justify-content:center; font-size:12px; }
  .commentary-text { font-size:11px; color:#fff; flex:1; }
  .commentary-text .speaker { color:#ffd700; font-weight:700; }

  /* RESULT MODAL */
  .result-modal {
    position:fixed; inset:0; z-index:200;
    background:rgba(0,0,0,0.8); display:none;
    align-items:center; justify-content:center; flex-direction:column;
  }
  .result-modal.show { display:flex; }
  .result-box {
    background:linear-gradient(160deg,#1a2a4a,#0d1117);
    border:2px solid rgba(255,215,0,0.4); border-radius:20px;
    padding:30px 40px; text-align:center; animation:modalIn 0.4s ease-out;
    max-width:320px; width:90%;
  }
  @keyframes modalIn { 0%{transform:scale(0.5);opacity:0} 100%{transform:scale(1);opacity:1} }
  .result-title { font-family:'Black Han Sans',sans-serif; font-size:36px; margin-bottom:8px; }
  .result-title.win { color:#ffd700; text-shadow:0 0 20px rgba(255,215,0,0.7); }
  .result-title.lose { color:#ff4444; text-shadow:0 0 20px rgba(255,68,68,0.7); }
  .result-score { font-family:'Black Han Sans',sans-serif; font-size:24px; color:#fff; margin-bottom:16px; }
  .result-stats { display:flex; gap:20px; justify-content:center; margin-bottom:20px; }
  .result-stat { text-align:center; }
  .result-stat .rs-num { font-family:'Black Han Sans',sans-serif; font-size:20px; color:#4CAF50; }
  .result-stat .rs-label { font-size:10px; color:rgba(255,255,255,0.5); }
  .result-btns { display:flex; gap:10px; }
  .result-btn {
    flex:1; padding:12px; border-radius:10px; border:none; cursor:pointer;
    font-family:'Black Han Sans',sans-serif; font-size:14px;
  }
  .result-btn.again { background:linear-gradient(135deg,#ffd700,#ff6b35); color:#000; }
  .result-btn.menu { background:rgba(255,255,255,0.1); color:#fff; border:1px solid rgba(255,255,255,0.2); }

  /* PITCH RESULT POPUP */
  .pitch-result {
    position:absolute; top:30%; left:50%; transform:translateX(-50%);
    font-family:'Black Han Sans',sans-serif; font-size:28px;
    pointer-events:none; opacity:0; z-index:80; white-space:nowrap;
  }
  .pitch-result.strike { color:#ff4444; text-shadow:0 0 15px rgba(255,68,68,0.7); }
  .pitch-result.ball { color:#44bb44; text-shadow:0 0 15px rgba(68,187,68,0.7); }
  .pitch-result.out { color:#ff8c00; text-shadow:0 0 15px rgba(255,140,0,0.7); }
  .pitch-result.hit { color:#ffd700; text-shadow:0 0 15px rgba(255,215,0,0.7); }
  .pitch-result.show { animation:popShow 1.2s ease-out forwards; }
  @keyframes popShow { 0%{opacity:1;transform:translateX(-50%) scale(0.5)} 30%{opacity:1;transform:translateX(-50%) scale(1.1)} 60%{opacity:1;transform:translateX(-50%) scale(1)} 100%{opacity:0;transform:translateX(-50%) scale(1)} }

  /* SWITCH VIEW BTN */
  .switch-view-btn {
    position:absolute; top:8px; right:8px; z-index:60;
    background:rgba(0,0,0,0.5); border:1px solid rgba(255,215,0,0.4);
    color:#ffd700; font-size:10px; font-weight:700; padding:4px 8px; border-radius:6px;
    cursor:pointer;
  }

  /* LINEUP PANEL */
  .lineup-panel {
    position:fixed; inset:0; z-index:150; background:rgba(0,0,0,0.85);
    display:none; flex-direction:column; padding:20px;
  }
  .lineup-panel.show { display:flex; }
  .lineup-title { font-family:'Black Han Sans',sans-serif; font-size:22px; color:#ffd700; margin-bottom:15px; text-align:center; }
  .lineup-list { flex:1; overflow-y:auto; display:flex; flex-direction:column; gap:6px; }
  .lineup-row {
    display:flex; align-items:center; gap:10px; padding:10px 12px;
    background:rgba(255,255,255,0.05); border-radius:10px;
    border:1px solid rgba(255,255,255,0.08);
  }
  .lineup-row.current { border-color:#ffd700; background:rgba(255,215,0,0.1); }
  .lr-num { font-family:'Black Han Sans',sans-serif; font-size:16px; color:#ffd700; min-width:20px; }
  .lr-pos { font-size:10px; color:#4CAF50; min-width:24px; font-weight:700; }
  .lr-name { font-family:'Black Han Sans',sans-serif; font-size:14px; color:#fff; flex:1; }
  .lr-stats { font-size:10px; color:rgba(255,255,255,0.5); }
  .close-lineup { margin-top:12px; padding:12px; background:rgba(255,255,255,0.08);
    border:1px solid rgba(255,255,255,0.15); border-radius:10px; color:#fff;
    font-family:'Black Han Sans',sans-serif; font-size:16px; cursor:pointer; text-align:center; }

  /* PARTICLES */
  .particle {
    position:absolute; pointer-events:none; z-index:90;
    border-radius:50%; animation:particle-fly var(--d,1s) ease-out forwards;
  }
  @keyframes particle-fly {
    0%{transform:translate(0,0) scale(1);opacity:1}
    100%{transform:translate(var(--tx,0),var(--ty,-60px)) scale(0);opacity:0}
  }

  /* INNING CHANGE */
  .inning-change {
    position:fixed; inset:0; z-index:180; background:rgba(0,0,0,0.75);
    display:none; align-items:center; justify-content:center;
  }
  .inning-change.show { display:flex; }
  .ic-box {
    text-align:center; animation:icIn 0.5s ease-out;
  }
  @keyframes icIn { 0%{opacity:0;transform:scaleY(0)} 100%{opacity:1;transform:scaleY(1)} }
  .ic-box h2 { font-family:'Black Han Sans',sans-serif; font-size:48px; color:#ffd700; text-shadow:0 0 30px rgba(255,215,0,0.8); }
  .ic-box p { font-size:16px; color:#fff; margin-top:8px; }
</style>
</head>
<body>

<!-- ══════════════ TITLE SCREEN ══════════════ -->
<div id="titleScreen" class="screen active">
  <div class="stars" id="stars"></div>
  <div class="title-field"></div>
  <div class="title-logo">
    <div class="year">2025</div>
    <h1>컴프야</h1>
    <div class="subtitle">COM2US PRO BASEBALL</div>
  </div>
  <button class="title-btn" onclick="showTeamSelect()">게임 시작</button>
</div>

<!-- ══════════════ TEAM SELECT ══════════════ -->
<div id="teamScreen" class="screen">
  <div style="padding:20px; padding-bottom:0;">
    <div class="screen-title">⚾ 팀 선택</div>
    <div style="text-align:center;font-size:12px;color:rgba(255,255,255,0.4);margin-bottom:15px;">내 팀을 선택하세요</div>
  </div>
  <div style="flex:1;overflow-y:auto;padding:0 20px;">
    <div class="team-grid" id="teamGrid"></div>
  </div>
  <div style="padding:20px;text-align:center;">
    <button class="start-game-btn" id="startBtn" onclick="startGame()">시즌 시작!</button>
  </div>
</div>

<!-- ══════════════ GAME SCREEN ══════════════ -->
<div id="gameScreen" class="screen">
  <!-- Scoreboard -->
  <div class="scoreboard">
    <div class="score-team">
      <div class="t-logo" id="homeLogoEl"></div>
      <div class="t-name" id="homeNameEl"></div>
      <div class="t-score" id="homeScoreEl">0</div>
    </div>
    <div class="score-sep">-</div>
    <div class="score-team" style="flex-direction:row-reverse;">
      <div class="t-logo" id="awayLogoEl"></div>
      <div class="t-name" id="awayNameEl"></div>
      <div class="t-score" id="awayScoreEl">0</div>
    </div>
    <div class="inning-info">
      <div class="inning-arrow" id="inningArrow">▲</div>
      <div class="inning-num" id="inningNum">1</div>
    </div>
    <div id="basesDisplay" class="bases-display">
      <div class="base-dot" id="base2"></div>
      <div class="base-dot" id="base3" style="visibility:hidden"></div>
      <div class="base-dot" id="base1"></div>
      <div class="base-dot" style="visibility:hidden"></div>
    </div>
    <div class="count-display">
      <div class="count-row">
        <span style="font-size:8px;color:rgba(255,255,255,0.4);width:8px">S</span>
        <div class="count-dot s" id="s1"></div><div class="count-dot s" id="s2"></div>
      </div>
      <div class="count-row">
        <span style="font-size:8px;color:rgba(255,255,255,0.4);width:8px">B</span>
        <div class="count-dot b" id="b1"></div><div class="count-dot b" id="b2"></div><div class="count-dot b" id="b3"></div>
      </div>
      <div class="count-row">
        <span style="font-size:8px;color:rgba(255,255,255,0.4);width:8px">O</span>
        <div class="count-dot o" id="o1"></div><div class="count-dot o" id="o2"></div>
      </div>
    </div>
  </div>

  <!-- Field -->
  <div class="field-container" id="fieldContainer">

    <!-- ── PITCHER VIEW ── -->
    <div id="pitcherView">
      <div class="field-bg" id="pitcherFieldBg">
        <div class="grass-lines"></div>
        <div class="outfield-wall"></div>
        <div class="wall-ads" id="wallAds"></div>
        <div class="scoreboard-big" id="sbBig"></div>
        <div class="foul-lines"></div>
        <div class="foul-line-right"></div>
        <div class="infield-dirt"></div>
        <div class="pitcher-mound"></div>
        <div class="batter-box-left"></div>
        <div class="batter-box-right"></div>
        <div class="home-plate"></div>

        <!-- Catcher -->
        <div class="catcher-sprite" id="catcherSprite"></div>
        <!-- Batter (far) -->
        <div class="batter-sprite-far" id="batterFar"></div>
        <!-- Pitcher -->
        <div class="pitcher-sprite" id="pitcherSprite" onclick="triggerPitch()"></div>

        <!-- Pitch ball (pitcher view) -->
        <div class="pitch-ball" id="pitchBallP"></div>

        <!-- Pitch result -->
        <div class="pitch-result" id="pitchResult"></div>
        <!-- Hit effect -->
        <div class="hit-effect" id="hitEffect"></div>

        <!-- Commentary -->
        <div class="commentary">
          <div class="commentary-icon">🎙</div>
          <div class="commentary-text" id="commentaryText"><span class="speaker">[권성욱]</span> 경기가 시작됩니다!</div>
        </div>
      </div>

      <!-- Pitch UI -->
      <div class="pitch-ui">
        <div class="stamina-bar">
          <div class="stamina-label">체력</div>
          <div class="stamina-track"><div class="stamina-fill" id="staminaFill" style="width:100%"></div></div>
          <div class="pitch-count-label">투구수 <span id="pitchCountLabel">0</span></div>
        </div>
        <div class="pitch-buttons" id="pitchButtons"></div>
        <div class="bottom-actions">
          <div class="action-btn" onclick="showLineup(true)">📋 수비 위치</div>
          <div class="action-btn" onclick="">⚡ 고의사구</div>
          <div class="action-btn" onclick="switchToAuto()">🤖 AUTO</div>
        </div>
      </div>
    </div>

    <!-- ── BATTER VIEW ── -->
    <div id="batterView">
      <div class="batter-field" id="batterFieldBg">
        <div class="batter-outfield-wall"></div>
        <div class="batter-wall-ads" id="bwAds"></div>
        <div class="batter-grass"></div>
        <div class="batter-ground"></div>

        <!-- Pitcher (far) -->
        <div class="pitcher-far" id="pitcherFar"></div>

        <!-- Pitch zone -->
        <div class="pitch-zone" id="pitchZone">
          <div class="pz-inner"></div>
          <div class="zone-grid">
            <div class="zone-cell"></div><div class="zone-cell"></div><div class="zone-cell"></div>
            <div class="zone-cell"></div><div class="zone-cell"></div><div class="zone-cell"></div>
            <div class="zone-cell"></div><div class="zone-cell"></div><div class="zone-cell"></div>
          </div>
          <div class="zone-label">스트라이크 존</div>
        </div>

        <!-- Pitch ball (batter view) -->
        <div class="pitch-ball" id="pitchBallB" style="top:10%;left:50%;"></div>

        <!-- Batter -->
        <div class="batter-sprite-main" id="batterMain"></div>

        <!-- Hit effect -->
        <div class="hit-effect" id="hitEffectB"></div>
        <div class="pitch-result" id="pitchResultB"></div>

        <!-- Swing area -->
        <div class="swing-area" id="swingArea" onclick="doSwing()">
          <div class="swing-hint">⚾<br>터치하여<br>스윙</div>
        </div>

        <!-- Commentary -->
        <div class="commentary">
          <div class="commentary-icon">🎙</div>
          <div class="commentary-text" id="commentaryTextB"><span class="speaker">[권성욱]</span> 타석에 들어섭니다!</div>
        </div>
      </div>

      <!-- Batter UI -->
      <div class="batter-ui">
        <div class="batter-actions">
          <button class="swing-btn" id="swingBtn" onclick="doSwing()">스윙!</button>
          <button class="take-btn" onclick="doTake()">보내기</button>
        </div>
        <div class="batter-info">
          <div class="batter-name-label" id="batterNameLabel">타자</div>
          <div class="batter-stats">
            <div class="batter-stat">타율 <span id="bAvg">.280</span></div>
            <div class="batter-stat">파워 <span id="bPow">75</span></div>
            <div class="batter-stat">컨택 <span id="bCon">80</span></div>
          </div>
        </div>
      </div>
    </div>

    <!-- Switch view -->
    <div class="switch-view-btn" id="switchViewBtn" onclick="toggleView()">👁 시점 전환</div>
  </div>

  <!-- Lineup Panel -->
  <div class="lineup-panel" id="lineupPanel">
    <div class="lineup-title">📋 타순 / 라인업</div>
    <div class="lineup-list" id="lineupList"></div>
    <div class="close-lineup" onclick="closeLineup()">닫기</div>
  </div>
</div>

<!-- Result Modal -->
<div class="result-modal" id="resultModal">
  <div class="result-box">
    <div class="result-title" id="resultTitle">승리!</div>
    <div class="result-score" id="resultScore">5 - 3</div>
    <div class="result-stats">
      <div class="result-stat"><div class="rs-num" id="rHits">8</div><div class="rs-label">안타</div></div>
      <div class="result-stat"><div class="rs-num" id="rSO">7</div><div class="rs-label">삼진</div></div>
      <div class="result-stat"><div class="rs-num" id="rHR">2</div><div class="rs-label">홈런</div></div>
    </div>
    <div class="result-btns">
      <button class="result-btn again" onclick="restartGame()">다시 하기</button>
      <button class="result-btn menu" onclick="goMenu()">메뉴</button>
    </div>
  </div>
</div>

<!-- Inning change -->
<div class="inning-change" id="inningChange">
  <div class="ic-box">
    <h2 id="icText">2회</h2>
    <p id="icSub">공격 시작!</p>
  </div>
</div>

<script>
// ══════════════ DATA ══════════════
const TEAMS = [
  { id:'lotte', name:'롯데 자이언츠', city:'부산', emoji:'🔵', color:'#003087', accent:'#CC0000', power:'A', stadium:'사직야구장' },
  { id:'kiwoom', name:'키움 히어로즈', city:'서울', emoji:'🔴', color:'#820024', accent:'#FFFFFF', power:'B+', stadium:'고척스카이돔' },
  { id:'samsung', name:'삼성 라이온즈', city:'대구', emoji:'⚡', color:'#074CA1', accent:'#C0C0C0', power:'A-', stadium:'삼성 라이온즈 파크' },
  { id:'kt', name:'KT 위즈', city:'수원', emoji:'🟣', color:'#000000', accent:'#DB0328', power:'B+', stadium:'수원KT위즈파크' },
  { id:'nc', name:'NC 다이노스', city:'창원', emoji:'🦕', color:'#071D49', accent:'#BFA46F', power:'A', stadium:'창원NC파크' },
  { id:'lg', name:'LG 트윈스', city:'서울', emoji:'⚪', color:'#C30452', accent:'#000000', power:'A', stadium:'잠실야구장' },
  { id:'doosan', name:'두산 베어스', city:'서울', emoji:'🐻', color:'#131230', accent:'#C0392B', power:'B+', stadium:'잠실야구장' },
  { id:'ssg', name:'SSG 랜더스', city:'인천', emoji:'🟠', color:'#CE0E2D', accent:'#BFA46F', power:'A-', stadium:'인천SSG랜더스필드' },
  { id:'hanwha', name:'한화 이글스', city:'대전', emoji:'🦅', color:'#FF6600', accent:'#000000', power:'B', stadium:'한화생명이글스파크' },
  { id:'kia', name:'KIA 타이거즈', city:'광주', emoji:'🐯', color:'#EA0029', accent:'#05141F', power:'A+', stadium:'광주-기아 챔피언스필드' },
];

const ROSTER_TEMPLATES = [
  { name:'강민호', pos:'C', order:2, pow:72, con:78, spd:55, eye:76, face:'round' },
  { name:'이대호', pos:'1B', order:4, pow:88, con:82, spd:42, eye:80, face:'wide' },
  { name:'박민우', pos:'2B', order:1, pow:60, con:88, spd:92, eye:85, face:'slim' },
  { name:'허경민', pos:'3B', order:6, pow:65, con:80, spd:70, eye:78, face:'square' },
  { name:'김하성', pos:'SS', order:3, pow:75, con:86, spd:88, eye:82, face:'round' },
  { name:'최형우', pos:'LF', order:5, pow:84, con:85, spd:60, eye:83, face:'wide' },
  { name:'나성범', pos:'CF', order:7, pow:80, con:84, spd:82, eye:79, face:'slim' },
  { name:'손아섭', pos:'RF', order:8, pow:78, con:87, spd:75, eye:81, face:'square' },
  { name:'양의지', pos:'DH', order:9, pow:76, con:82, spd:52, eye:77, face:'round' },
];

const PITCHER_TEMPLATES = [
  { name:'류현진', throws:'L', arm:88, stamina:92, pitches:['포심','슬라이더','체인지업','커브'] },
  { name:'고영표', throws:'R', arm:82, stamina:88, pitches:['커터','포심','슬라이더','포크'] },
  { name:'안우진', throws:'R', arm:85, stamina:85, pitches:['포심','슬라이더','커터','스플리터'] },
  { name:'원태인', throws:'R', arm:83, stamina:87, pitches:['포심','커브','체인지업','슬라이더'] },
];

const PITCH_TYPES = {
  '포심': { grade:'A', color:'#4CAF50', speed:148, break_x:1, break_z:3, strikeRate:0.55 },
  '슬라이더': { grade:'B', color:'#2196F3', speed:136, break_x:7, break_z:2, strikeRate:0.50 },
  '커터': { grade:'B', color:'#2196F3', speed:142, break_x:4, break_z:2, strikeRate:0.52 },
  '체인지업': { grade:'B', color:'#FF9800', speed:132, break_x:5, break_z:4, strikeRate:0.48 },
  '커브': { grade:'C', color:'#FF9800', speed:124, break_x:6, break_z:8, strikeRate:0.45 },
  '포크': { grade:'B', color:'#2196F3', speed:130, break_x:2, break_z:7, strikeRate:0.52 },
  '스플리터': { grade:'B', color:'#2196F3', speed:134, break_x:3, break_z:6, strikeRate:0.51 },
};

const COMMENTARIES_STRIKE = ['스트라이크! 멋진 투구입니다!','완벽한 코스! 타자가 꼼짝 못 합니다!','스트라이크! 투수가 기세를 올립니다!','아, 스트라이크! 배트가 나오지 않습니다!'];
const COMMENTARIES_BALL = ['볼! 조금 빗나갔습니다.','볼이 선언됩니다. 투수, 안정을 찾아야 합니다.','볼! 제구가 흔들리고 있습니다.','볼 판정입니다.'];
const COMMENTARIES_HIT = ['안타! 힘차게 뻗어나갑니다!','잘 맞았습니다! 타구가 날아갑니다!','통쾌한 안타! 관중이 환호합니다!','클린 히트! 완벽하게 잡아냈습니다!'];
const COMMENTARIES_HR = ['홈런!!! 담장을 완전히 넘어갑니다!','장외 홈런! 멀리멀리 날아갑니다!','홈런입니다! 관중이 열광합니다!'];
const COMMENTARIES_OUT = ['아웃! 잘 잡아냈습니다.','삼진 아웃! 투수의 승리입니다!','타구를 잡아냈습니다, 아웃!'];

// ══════════════ STATE ══════════════
let state = {
  myTeam: null, oppTeam: null,
  myScore: 0, oppScore: 0,
  inning: 1, isTop: true, // true=내가 수비(투수), false=내가 공격(타자)
  outs: 0, strikes: 0, balls: 0,
  bases: [false, false, false], // 1루,2루,3루
  pitchCount: 0, stamina: 100,
  currentBatterIdx: 0,
  currentPitcher: null,
  myRoster: [], oppRoster: [],
  view: 'pitcher', // 'pitcher' | 'batter'
  pitching: false, batting: false,
  totalHits: 0, totalSO: 0, totalHR: 0,
  gameOver: false,
  autoMode: false,
};

// ══════════════ TITLE ══════════════
(function initStars(){
  const s=document.getElementById('stars');
  for(let i=0;i<80;i++){
    const d=document.createElement('div');
    d.className='star';
    d.style.cssText=`left:${Math.random()*100}%;top:${Math.random()*100}%;width:${Math.random()*3+1}px;height:${Math.random()*3+1}px;animation-delay:${Math.random()*3}s;animation-duration:${2+Math.random()*3}s`;
    s.appendChild(d);
  }
})();

// ══════════════ TEAM SELECT ══════════════
function showTeamSelect(){
  document.getElementById('titleScreen').classList.remove('active');
  document.getElementById('teamScreen').classList.add('active');
  buildTeamGrid();
}

function buildTeamGrid(){
  const g=document.getElementById('teamGrid');
  g.innerHTML='';
  TEAMS.forEach(t=>{
    const c=document.createElement('div');
    c.className='team-card';
    c.innerHTML=`<span class="team-emoji">${t.emoji}</span><div class="team-name">${t.name}</div><div class="team-city">${t.city}</div><div class="team-power">전력 ${t.power}</div>`;
    c.onclick=()=>selectTeam(t, c);
    g.appendChild(c);
  });
}

function selectTeam(team, el){
  document.querySelectorAll('.team-card').forEach(c=>c.classList.remove('selected'));
  el.classList.add('selected');
  state.myTeam = team;
  // Pick opponent randomly
  const others = TEAMS.filter(t=>t.id!==team.id);
  state.oppTeam = others[Math.floor(Math.random()*others.length)];
  document.getElementById('startBtn').classList.add('ready');
}

// ══════════════ BUILD ROSTER ══════════════
function buildRoster(team, isHome){
  const names = shuffleNames(team);
  return ROSTER_TEMPLATES.map((tmpl,i)=>({
    ...tmpl,
    name: names[i],
    team: team,
    jersey: (i+1)*10 + Math.floor(Math.random()*9),
    skinTone: ['#F5CBA7','#F0B27A','#D98B4A','#C68642','#A0522D'][Math.floor(Math.random()*5)],
    hairColor: ['#1a1a1a','#3d2b1f','#000','#222','#4a3728'][Math.floor(Math.random()*5)],
    helmetColor: team.color,
    uniformColor: team.color,
    accentColor: team.accent,
    pow: tmpl.pow + Math.floor(Math.random()*10-5),
    con: tmpl.con + Math.floor(Math.random()*10-5),
    spd: tmpl.spd + Math.floor(Math.random()*8-4),
    avg: (0.230 + Math.random()*0.100).toFixed(3),
  }));
}

function buildPitcher(team){
  const t = PITCHER_TEMPLATES[Math.floor(Math.random()*PITCHER_TEMPLATES.length)];
  return {
    ...t,
    name: shufflePitcherName(team),
    skinTone: ['#F5CBA7','#F0B27A','#D98B4A'][Math.floor(Math.random()*3)],
    hairColor: '#1a1a1a',
    capColor: team.color,
    uniformColor: team.color,
    accentColor: team.accent,
    number: Math.floor(Math.random()*40+10),
  };
}

const KOREAN_SURNAMES=['김','이','박','최','정','강','조','윤','장','임','한','오','서','신','권','황','안','송','류','전','홍','고','문','양','손','배','백','허','유','남'];
const KOREAN_NAMES=['민준','서준','도윤','예준','시우','하준','주원','지후','지호','준서','준우','현우','민서','도현','지훈','민재','현준','선우','정우','동현','지원','재원','정호','민호','성준','준혁','성민','동윤','유준','건우'];

function shuffleNames(team){
  return ROSTER_TEMPLATES.map(()=>{
    const s=KOREAN_SURNAMES[Math.floor(Math.random()*KOREAN_SURNAMES.length)];
    const n=KOREAN_NAMES[Math.floor(Math.random()*KOREAN_NAMES.length)];
    return s+n;
  });
}
function shufflePitcherName(team){
  const s=KOREAN_SURNAMES[Math.floor(Math.random()*KOREAN_SURNAMES.length)];
  const n=KOREAN_NAMES[Math.floor(Math.random()*KOREAN_NAMES.length)];
  return s+n;
}

// ══════════════ START GAME ══════════════
function startGame(){
  if(!state.myTeam) return;
  state.myRoster = buildRoster(state.myTeam, true);
  state.oppRoster = buildRoster(state.oppTeam, false);
  state.currentPitcher = buildPitcher(state.myTeam);
  state.oppPitcher = buildPitcher(state.oppTeam);

  document.getElementById('teamScreen').classList.remove('active');
  document.getElementById('gameScreen').classList.add('active');

  // Scoreboard
  document.getElementById('homeLogoEl').textContent=state.myTeam.emoji;
  document.getElementById('homeLogoEl').style.background=state.myTeam.color;
  document.getElementById('homeNameEl').textContent=state.myTeam.name.split(' ')[0];
  document.getElementById('awayLogoEl').textContent=state.oppTeam.emoji;
  document.getElementById('awayLogoEl').style.background=state.oppTeam.color;
  document.getElementById('awayNameEl').textContent=state.oppTeam.name.split(' ')[0];

  buildWallAds();
  document.getElementById('sbBig').textContent=`${state.myTeam.name} VS ${state.oppTeam.name}`;

  renderPitcher();
  renderBatterFar();
  renderCatcher();
  renderPitcherFar();
  renderBatterMain();
  buildPitchButtons();
  updateScoreboard();
  updateLineup();
  setView('pitcher');
  setCommentary(`${state.myTeam.name} VS ${state.oppTeam.name}! 1회가 시작됩니다. 투수 ${state.currentPitcher.name} 선발 등판!`);
}

// ══════════════ SVG SPRITES ══════════════
function createPlayerSVG(opts){
  // opts: type('pitcher'|'batter'|'batter-far'|'catcher'), team colors, skin, hair, number, face
  const {type,cap,uniform,accent,skin,hair,num,face} = opts;
  let w,h;
  if(type==='pitcher'){w=60;h=90;}
  else if(type==='batter'){w=80;h=130;}
  else if(type==='batter-far'){w=40;h=65;}
  else{w=38;h=50;}

  const eyeStyle = face==='wide'?'gap:7px':'face==="slim"?gap:3px':'gap:5px';
  const headW = type==='batter'?36:type==='pitcher'?28:18;

  // Build inline SVG
  if(type==='pitcher'){
    return `<svg width="60" height="90" viewBox="0 0 60 90" xmlns="http://www.w3.org/2000/svg">
      <!-- Shadow -->
      <ellipse cx="30" cy="88" rx="20" ry="4" fill="rgba(0,0,0,0.3)"/>
      <!-- Legs -->
      <rect x="16" y="62" width="12" height="26" rx="6" fill="${uniform}"/>
      <rect x="32" y="62" width="12" height="26" rx="6" fill="${uniform}"/>
      <!-- Shoes -->
      <ellipse cx="22" cy="87" rx="8" ry="4" fill="#111"/>
      <ellipse cx="38" cy="87" rx="8" ry="4" fill="#111"/>
      <!-- Body -->
      <rect x="14" y="30" width="32" height="34" rx="5" fill="${uniform}"/>
      <!-- Number -->
      <text x="30" y="52" text-anchor="middle" font-size="10" font-weight="900" fill="${accent}" font-family="sans-serif">${num}</text>
      <!-- Belt -->
      <rect x="14" y="57" width="32" height="5" rx="2" fill="#333"/>
      <!-- Arm left (glove) -->
      <rect x="4" y="32" width="12" height="22" rx="6" fill="${uniform}"/>
      <!-- Glove -->
      <circle cx="7" cy="56" r="8" fill="#8b4513"/>
      <circle cx="7" cy="56" r="5" fill="#6b3010"/>
      <!-- Arm right (ball) -->
      <rect x="44" y="32" width="12" height="22" rx="6" fill="${uniform}"/>
      <!-- Ball -->
      <circle cx="54" cy="30" r="6" fill="#f0f0f0"/>
      <path d="M50,27 Q54,25 58,27" stroke="#cc0000" stroke-width="1.5" fill="none"/>
      <path d="M50,33 Q54,35 58,33" stroke="#cc0000" stroke-width="1.5" fill="none"/>
      <!-- Head -->
      <circle cx="30" cy="16" r="14" fill="${skin}"/>
      <!-- Cap -->
      <path d="M16,12 Q30,4 44,12 L44,18 Q30,16 16,18 Z" fill="${cap}"/>
      <rect x="10" y="16" width="6" height="4" rx="1" fill="${cap}"/>
      <!-- Cap logo -->
      <circle cx="30" cy="11" r="4" fill="${accent}" opacity="0.8"/>
      <!-- Eyes -->
      <circle cx="${face==='wide'?25:27}" cy="18" r="2.5" fill="#333"/>
      <circle cx="${face==='wide'?35:33}" cy="18" r="2.5" fill="#333"/>
      <circle cx="${face==='wide'?25.8:27.8}" cy="17.2" r="0.8" fill="#fff"/>
      <circle cx="${face==='wide'?35.8:33.8}" cy="17.2" r="0.8" fill="#fff"/>
      <!-- Nose -->
      <circle cx="30" cy="21" r="1.5" fill="${skin}" stroke="rgba(0,0,0,0.2)" stroke-width="0.5"/>
      <!-- Mouth -->
      <path d="${face==='slim'?'M27,24 Q30,26 33,24':'M27,24 Q30,27 33,24'}" stroke="#c0704a" stroke-width="1.5" fill="none" stroke-linecap="round"/>
      <!-- Ear -->
      <circle cx="16" cy="17" r="4" fill="${skin}"/>
      <circle cx="44" cy="17" r="4" fill="${skin}"/>
    </svg>`;
  }

  if(type==='batter'){
    return `<svg width="80" height="130" viewBox="0 0 80 130" xmlns="http://www.w3.org/2000/svg">
      <!-- Shadow -->
      <ellipse cx="40" cy="128" rx="28" ry="5" fill="rgba(0,0,0,0.3)"/>
      <!-- Legs -->
      <rect x="18" y="90" width="16" height="38" rx="8" fill="${uniform}"/>
      <rect x="46" y="90" width="16" height="38" rx="8" fill="${uniform}"/>
      <!-- Shoes -->
      <ellipse cx="26" cy="127" rx="11" ry="5" fill="#111"/>
      <ellipse cx="54" cy="127" rx="11" ry="5" fill="#111"/>
      <!-- Stirrups -->
      <rect x="21" y="110" width="10" height="3" rx="1.5" fill="${accent}" opacity="0.6"/>
      <rect x="49" y="110" width="10" height="3" rx="1.5" fill="${accent}" opacity="0.6"/>
      <!-- Body -->
      <rect x="16" y="40" width="48" height="52" rx="7" fill="${uniform}"/>
      <!-- Number -->
      <text x="40" y="72" text-anchor="middle" font-size="14" font-weight="900" fill="${accent}" font-family="sans-serif">${num}</text>
      <!-- Belt -->
      <rect x="16" y="84" width="48" height="6" rx="3" fill="#222"/>
      <rect x="34" y="84" width="12" height="6" rx="3" fill="#888"/>
      <!-- Arms -->
      <rect x="2" y="44" width="16" height="30" rx="8" fill="${uniform}"/>
      <rect x="62" y="44" width="16" height="30" rx="8" fill="${uniform}"/>
      <!-- Gloves -->
      <circle cx="10" cy="76" r="8" fill="#1a1a1a"/>
      <circle cx="70" cy="76" r="8" fill="#1a1a1a"/>
      <!-- Bat -->
      <rect x="68" y="10" width="10" height="75" rx="5" fill="#8b6914" transform="rotate(-15,73,48)"/>
      <rect x="69" y="10" width="8" height="20" rx="4" fill="#a07820" transform="rotate(-15,73,48)"/>
      <!-- Head -->
      <circle cx="40" cy="22" r="18" fill="${skin}"/>
      <!-- Helmet -->
      <path d="M22,18 Q40,6 58,18 L56,28 Q40,24 24,28 Z" fill="${cap}"/>
      <path d="M22,18 L20,28 Q22,32 26,28 L24,22 Z" fill="${cap}"/>
      <!-- Helmet logo -->
      <circle cx="40" cy="13" r="5" fill="${accent}" opacity="0.9"/>
      <!-- Eyes -->
      <circle cx="33" cy="24" r="3" fill="#333"/>
      <circle cx="47" cy="24" r="3" fill="#333"/>
      <circle cx="34" cy="23" r="1" fill="#fff"/>
      <circle cx="48" cy="23" r="1" fill="#fff"/>
      <!-- Ear guard -->
      <path d="M22,20 Q18,25 20,32 Q22,36 26,32 L24,20 Z" fill="${cap}"/>
      <!-- Nose -->
      <circle cx="40" cy="28" r="2" fill="${skin}" stroke="rgba(0,0,0,0.15)" stroke-width="0.5"/>
      <!-- Mouth -->
      <path d="M35,33 Q40,37 45,33" stroke="#c0704a" stroke-width="2" fill="none" stroke-linecap="round"/>
    </svg>`;
  }

  if(type==='batter-far'){
    return `<svg width="40" height="65" viewBox="0 0 40 65" xmlns="http://www.w3.org/2000/svg">
      <ellipse cx="20" cy="63" rx="12" ry="3" fill="rgba(0,0,0,0.3)"/>
      <rect x="10" y="52" width="8" height="13" rx="4" fill="${uniform}"/>
      <rect x="22" y="52" width="8" height="13" rx="4" fill="${uniform}"/>
      <rect x="8" y="26" width="24" height="28" rx="4" fill="${uniform}"/>
      <text x="20" y="42" text-anchor="middle" font-size="8" font-weight="900" fill="${accent}" font-family="sans-serif">${num}</text>
      <rect x="2" y="28" width="8" height="16" rx="4" fill="${uniform}"/>
      <rect x="30" y="28" width="8" height="16" rx="4" fill="${uniform}"/>
      <circle cx="20" cy="14" r="10" fill="${skin}"/>
      <path d="M10,10 Q20,4 30,10 L30,16 Q20,14 10,16 Z" fill="${cap}"/>
    </svg>`;
  }

  if(type==='catcher'){
    return `<svg width="38" height="50" viewBox="0 0 38 50" xmlns="http://www.w3.org/2000/svg">
      <ellipse cx="19" cy="48" rx="14" ry="3" fill="rgba(0,0,0,0.3)"/>
      <rect x="8" y="28" width="10" height="20" rx="5" fill="${uniform}"/>
      <rect x="20" y="28" width="10" height="20" rx="5" fill="${uniform}"/>
      <rect x="6" y="14" width="26" height="16" rx="4" fill="${uniform}"/>
      <rect x="2" y="16" width="8" height="14" rx="4" fill="${uniform}"/>
      <!-- Catcher mitt -->
      <circle cx="3" cy="32" r="7" fill="#8b4513"/>
      <circle cx="3" cy="32" r="4" fill="#6b3010"/>
      <rect x="28" y="16" width="8" height="14" rx="4" fill="${uniform}"/>
      <!-- Chest protector -->
      <rect x="8" y="14" width="22" height="14" rx="3" fill="#555" opacity="0.6"/>
      <!-- Mask -->
      <circle cx="19" cy="8" r="9" fill="${skin}"/>
      <rect x="10" y="2" width="18" height="14" rx="4" fill="none" stroke="#333" stroke-width="2.5"/>
      <line x1="13" y1="4" x2="13" y2="14" stroke="#333" stroke-width="1.5"/>
      <line x1="16" y1="4" x2="16" y2="14" stroke="#333" stroke-width="1.5"/>
      <line x1="19" y1="4" x2="19" y2="14" stroke="#333" stroke-width="1.5"/>
      <line x1="22" y1="4" x2="22" y2="14" stroke="#333" stroke-width="1.5"/>
      <line x1="25" y1="4" x2="25" y2="14" stroke="#333" stroke-width="1.5"/>
      <path d="M10,8 Q19,4 28,8" stroke="#333" stroke-width="1.5" fill="none"/>
    </svg>`;
  }
}

function renderPitcher(){
  const p=state.currentPitcher;
  const el=document.getElementById('pitcherSprite');
  el.innerHTML=createPlayerSVG({type:'pitcher',cap:p.capColor,uniform:p.uniformColor,accent:p.accentColor,skin:p.skinTone,hair:p.hairColor,num:p.number,face:'round'});
}
function renderBatterFar(){
  const b=state.oppRoster[state.currentBatterIdx];
  const el=document.getElementById('batterFar');
  el.innerHTML=createPlayerSVG({type:'batter-far',cap:state.oppTeam.color,uniform:state.oppTeam.color,accent:state.oppTeam.accent,skin:b.skinTone,hair:b.hairColor,num:b.jersey,face:b.face});
}
function renderCatcher(){
  const el=document.getElementById('catcherSprite');
  const c=state.myRoster[0]; // catcher = first in roster
  el.innerHTML=createPlayerSVG({type:'catcher',cap:state.myTeam.color,uniform:state.myTeam.color,accent:state.myTeam.accent,skin:c.skinTone,hair:c.hairColor,num:c.jersey,face:c.face});
}
function renderPitcherFar(){
  const p=state.oppPitcher;
  const el=document.getElementById('pitcherFar');
  el.innerHTML=createPlayerSVG({type:'pitcher',cap:p.capColor,uniform:p.uniformColor,accent:p.accentColor,skin:p.skinTone,hair:p.hairColor,num:p.number,face:'round'});
}
function renderBatterMain(){
  const b=state.myRoster[state.currentBatterIdx % state.myRoster.length];
  const el=document.getElementById('batterMain');
  el.innerHTML=createPlayerSVG({type:'batter',cap:state.myTeam.color,uniform:state.myTeam.color,accent:state.myTeam.accent,skin:b.skinTone,hair:b.hairColor,num:b.jersey,face:b.face});
  document.getElementById('batterNameLabel').textContent=b.name;
  document.getElementById('bAvg').textContent=b.avg;
  document.getElementById('bPow').textContent=b.pow;
  document.getElementById('bCon').textContent=b.con;
}

function buildWallAds(){
  const ads=['파워에이드','INZONE','IDEAL FOR MEN','com2us','Culture Land','흥행봉','emart','TV조선'];
  const el=document.getElementById('wallAds');
  el.innerHTML=ads.map(a=>`<div class="wall-ad">${a}</div>`).join('');
  const bwel=document.getElementById('bwAds');
  bwel.innerHTML=ads.map(a=>`<div class="wall-ad">${a}</div>`).join('');
}

// ══════════════ PITCH BUTTONS ══════════════
function buildPitchButtons(){
  const p=state.currentPitcher;
  const container=document.getElementById('pitchButtons');
  container.innerHTML='';
  p.pitches.slice(0,4).forEach(name=>{
    const pt=PITCH_TYPES[name];
    const btn=document.createElement('div');
    btn.className='pitch-btn';
    btn.innerHTML=`<div class="pb-grade ${pt.grade}">${pt.grade}</div><div class="pb-name">${name}</div><div style="margin-left:auto;font-size:10px;color:rgba(255,255,255,0.4)">${pt.speed}km</div><div class="pb-bar"><div class="pb-bar-fill" style="width:${pt.strikeRate*100}%"></div></div>`;
    btn.onclick=()=>selectPitch(name);
    container.appendChild(btn);
  });
}

let selectedPitch = null;
function selectPitch(name){
  if(state.pitching||state.gameOver) return;
  selectedPitch=name;
  document.querySelectorAll('.pitch-btn').forEach(b=>{
    b.style.borderColor='rgba(255,255,255,0.12)';
    b.style.background='rgba(255,255,255,0.06)';
  });
  event.currentTarget.style.borderColor='#ffd700';
  event.currentTarget.style.background='rgba(255,215,0,0.1)';
  setTimeout(()=>executePitch(), 400);
}

// ══════════════ PITCH EXECUTION ══════════════
function triggerPitch(){
  if(!selectedPitch||state.pitching||state.gameOver) return;
  executePitch();
}

function executePitch(){
  if(state.pitching||state.gameOver) return;
  state.pitching=true;
  state.pitchCount++;
  document.getElementById('pitchCountLabel').textContent=state.pitchCount;

  // Stamina decrease
  state.stamina = Math.max(0, state.stamina - (2 + Math.random()*3));
  document.getElementById('staminaFill').style.width=state.stamina+'%';
  if(state.stamina<30) document.getElementById('staminaFill').style.background='linear-gradient(90deg,#f44336,#ff7043)';
  else if(state.stamina<60) document.getElementById('staminaFill').style.background='linear-gradient(90deg,#ff9800,#ffc107)';

  const pt = PITCH_TYPES[selectedPitch||'포심'];
  const batter = state.oppRoster[state.currentBatterIdx];

  // Determine outcome
  const isStrike = Math.random() < (pt.strikeRate + (state.stamina/200));
  const batterSwings = Math.random() < 0.65;
  const hitChance = isStrike && batterSwings ? (batter.con/100)*0.35 : 0;
  const isHit = Math.random() < hitChance;
  const isHR = isHit && Math.random() < (batter.pow/100)*0.15;

  // Animate pitch
  animatePitchFromPitcher(isStrike, ()=>{
    // Batter reaction
    if(batterSwings && isStrike){
      animateBatterFarSwing();
    }
    setTimeout(()=>{
      if(isHit){
        handleHit(isHR);
      } else if(isStrike && batterSwings){
        // Foul or miss
        if(Math.random()<0.3 && state.strikes<2){
          handleFoul();
        } else {
          handleStrike();
        }
      } else if(isStrike && !batterSwings){
        handleStrike();
      } else {
        handleBall();
      }
      state.pitching=false;
      selectedPitch=null;
      document.querySelectorAll('.pitch-btn').forEach(b=>{
        b.style.borderColor='rgba(255,255,255,0.12)';
        b.style.background='rgba(255,255,255,0.06)';
      });
    }, 300);
  });
}

function animatePitchFromPitcher(isStrike, cb){
  // Pitcher wind-up
  const sprite=document.getElementById('pitcherSprite');
  sprite.style.transition='transform 0.15s';
  sprite.style.transform='translateX(-50%) scale(1.05)';
  setTimeout(()=>{sprite.style.transform='translateX(-50%) scale(1)';},150);

  // Ball animation (pitcher view - ball flies toward camera)
  const ball=document.getElementById('pitchBallP');
  const fieldBg=document.getElementById('pitcherFieldBg');
  const rect=fieldBg.getBoundingClientRect();
  // Start near pitcher mound
  const startX = rect.width*0.5;
  const startY = rect.height*0.25;
  ball.style.display='block';
  ball.style.left=(startX-6)+'px';
  ball.style.top=(startY-6)+'px';
  ball.style.transition='none';
  ball.style.transform='scale(0.3)';
  setTimeout(()=>{
    ball.style.transition='all 0.4s ease-in';
    ball.style.transform='scale(1)';
    ball.style.left=(rect.width*0.5-6 + (isStrike?0:(Math.random()*30-15)))+'px';
    ball.style.top=(rect.height*0.65 + (isStrike?0:(Math.random()*20-10)))+'px';
    setTimeout(()=>{
      ball.style.display='none';
      if(cb) cb();
    },420);
  },50);
}

function animateBatterFarSwing(){
  const el=document.getElementById('batterFar');
  el.style.transition='transform 0.1s';
  el.style.transform='scaleX(-1.05) rotate(-5deg)';
  setTimeout(()=>{el.style.transform='scaleX(-1)';},200);
}

// ══════════════ PITCH OUTCOMES ══════════════
function handleStrike(){
  state.strikes++;
  if(state.strikes>=3){
    state.totalSO++;
    showPitchResult('삼진!','strike');
    setCommentary(COMMENTARIES_OUT[Math.floor(Math.random()*COMMENTARIES_OUT.length)]+` ${getCurrentBatterName()} 삼진 아웃!`);
    addOut();
  } else {
    showPitchResult(`스트라이크 ${state.strikes}`,'strike');
    setCommentary(COMMENTARIES_STRIKE[Math.floor(Math.random()*COMMENTARIES_STRIKE.length)]);
  }
  updateScoreboard();
}

function handleBall(){
  state.balls++;
  if(state.balls>=4){
    showPitchResult('볼넷','ball');
    setCommentary('볼 넷! 볼넷으로 진루합니다.');
    advanceRunners(true);
    nextBatter();
  } else {
    showPitchResult(`볼 ${state.balls}`,'ball');
    setCommentary(COMMENTARIES_BALL[Math.floor(Math.random()*COMMENTARIES_BALL.length)]);
  }
  updateScoreboard();
}

function handleFoul(){
  if(state.strikes<2) state.strikes++;
  showPitchResult('파울','strike');
  setCommentary('파울 타구! 카운트가 늘어납니다.');
  updateScoreboard();
}

function handleHit(isHR){
  state.totalHits++;
  if(isHR){
    state.totalHR++;
    // Count runners on base
    const runners = state.bases.filter(Boolean).length + 1;
    // Determine if it's MY team batting (isTop = I'm pitching = opp scores)
    if(state.isTop){
      state.oppScore += runners;
      document.getElementById('awayScoreEl').textContent=state.oppScore;
    } else {
      state.myScore += runners;
      document.getElementById('homeScoreEl').textContent=state.myScore;
    }
    state.bases=[false,false,false];
    showPitchResult('홈런!!!','hit');
    setCommentary(COMMENTARIES_HR[Math.floor(Math.random()*COMMENTARIES_HR.length)]);
    showHitEffect('💥 홈런!!!');
    spawnParticles(document.getElementById('pitcherFieldBg'));
  } else {
    advanceRunners(false);
    showPitchResult('안타!','hit');
    setCommentary(COMMENTARIES_HIT[Math.floor(Math.random()*COMMENTARIES_HIT.length)]+` ${getCurrentBatterName()} 안타!`);
    showHitEffect('안타!');
  }
  nextBatter();
  updateScoreboard();
  checkGameOver();
}

function addOut(){
  state.outs++;
  if(state.outs>=3){
    setTimeout(()=>endHalf(), 800);
  } else {
    nextBatter();
  }
}

function advanceRunners(isBB){
  // Simple: push runners forward
  if(state.bases[1] && state.bases[0]){ // 1,2 on
    if(isBB || state.bases[2]){
      // Run scores
      if(state.isTop){ state.oppScore++; document.getElementById('awayScoreEl').textContent=state.oppScore; }
      else { state.myScore++; document.getElementById('homeScoreEl').textContent=state.myScore; }
    }
  }
  state.bases[2]=state.bases[1]&&state.bases[0];
  state.bases[1]=state.bases[0];
  state.bases[0]=true;
  updateBases();
}

function nextBatter(){
  state.currentBatterIdx=(state.currentBatterIdx+1)%9;
  state.strikes=0;
  state.balls=0;
  renderBatterFar();
  renderBatterMain();
  setCommentary(`타석에 ${state.currentBatterIdx+1}번 타자 ${getCurrentBatterName()} 선수입니다.`);
  updateLineup();
}

function getCurrentBatterName(){
  return state.oppRoster[state.currentBatterIdx].name;
}

function endHalf(){
  state.outs=0; state.strikes=0; state.balls=0;
  state.bases=[false,false,false];
  updateBases();
  updateScoreboard();

  if(state.isTop){
    state.isTop=false;
    showInningChange(`${state.inning}회`, '우리 팀 공격!');
  } else {
    state.inning++;
    state.isTop=true;
    if(state.inning>9){
      endGame(); return;
    }
    showInningChange(`${state.inning}회`, '수비 시작!');
  }
}

function showInningChange(text, sub){
  const el=document.getElementById('inningChange');
  document.getElementById('icText').textContent=text;
  document.getElementById('icSub').textContent=sub;
  el.classList.add('show');
  setTimeout(()=>{el.classList.remove('show');},2000);
}

// ══════════════ BATTER MODE ══════════════
let pitchIncoming = false;
let pitchResult_batter = null;

function doSwing(){
  if(!pitchIncoming||state.gameOver) return;
  pitchIncoming=false;
  const batter=state.myRoster[state.currentBatterIdx % state.myRoster.length];
  // Animate bat swing
  const batterEl=document.getElementById('batterMain');
  batterEl.classList.add('swinging');
  setTimeout(()=>batterEl.classList.remove('swinging'),400);

  const isStrike=pitchResult_batter&&pitchResult_batter.inZone;
  const hitChance = isStrike ? (batter.con/100)*0.4 : (batter.con/100)*0.1;
  const isHit = Math.random()<hitChance;
  const isHR = isHit && Math.random()<(batter.pow/100)*0.12;

  if(isHit){
    if(isHR){
      state.myScore+=(state.bases.filter(Boolean).length+1);
      document.getElementById('homeScoreEl').textContent=state.myScore;
      state.bases=[false,false,false];
      showPitchResultB('홈런!!!','hit');
      showHitEffectB('💥 홈런!!!');
      spawnParticles(document.getElementById('batterFieldBg'));
      setCommentaryB(COMMENTARIES_HR[Math.floor(Math.random()*COMMENTARIES_HR.length)]);
    } else {
      advanceRunnersB();
      showPitchResultB('안타!','hit');
      showHitEffectB('안타!');
      setCommentaryB(COMMENTARIES_HIT[Math.floor(Math.random()*COMMENTARIES_HIT.length)]);
    }
    state.totalHits++;
    nextBatterB();
  } else if(isStrike){
    state.strikes++;
    if(state.strikes>=3){
      state.totalSO++;
      showPitchResultB('삼진!','strike');
      setCommentaryB('삼진 아웃!');
      addOutB();
    } else {
      showPitchResultB(`스트라이크 ${state.strikes}`,'strike');
    }
  } else {
    // Swing and miss outside zone - strike
    state.strikes++;
    if(state.strikes>=3){
      showPitchResultB('삼진!','strike');
      addOutB();
    } else {
      showPitchResultB(`스트라이크 ${state.strikes}`,'strike');
    }
  }
  updateScoreboard();
  checkGameOver();
}

function doTake(){
  if(!pitchIncoming||state.gameOver) return;
  pitchIncoming=false;
  const inZone=pitchResult_batter&&pitchResult_batter.inZone;
  if(inZone){
    state.strikes++;
    if(state.strikes>=3){
      showPitchResultB('삼진!','strike');
      addOutB();
    } else {
      showPitchResultB(`스트라이크 ${state.strikes}`,'strike');
      setCommentaryB('보내기... 스트라이크입니다!');
    }
  } else {
    state.balls++;
    if(state.balls>=4){
      advanceRunnersB(true);
      nextBatterB();
      showPitchResultB('볼넷','ball');
      setCommentaryB('볼 넷! 출루합니다.');
    } else {
      showPitchResultB(`볼 ${state.balls}`,'ball');
      setCommentaryB('볼!');
    }
  }
  updateScoreboard();
}

function advanceRunnersB(isBB){
  if(state.bases[1]&&state.bases[0]){
    if(isBB||state.bases[2]){ state.myScore++; document.getElementById('homeScoreEl').textContent=state.myScore; }
  }
  state.bases[2]=state.bases[1]&&state.bases[0];
  state.bases[1]=state.bases[0];
  state.bases[0]=true;
  updateBases();
}

function addOutB(){
  state.outs++;
  if(state.outs>=3){ setTimeout(()=>endHalfB(),800); }
  else { nextBatterB(); }
}

function nextBatterB(){
  state.currentBatterIdx=(state.currentBatterIdx+1)%9;
  state.strikes=0; state.balls=0;
  renderBatterMain();
  updateLineup();
  // Trigger next pitch from AI pitcher
  setTimeout(()=>triggerAIPitch(), 1200);
}

function endHalfB(){
  state.outs=0; state.strikes=0; state.balls=0;
  state.bases=[false,false,false];
  updateBases(); updateScoreboard();
  if(!state.isTop){
    state.isTop=true;
    showInningChange(`${state.inning}회`, '수비 시작!');
    setTimeout(()=>setView('pitcher'),2000);
  } else {
    state.inning++;
    if(state.inning>9){ endGame(); return; }
    state.isTop=false;
    showInningChange(`${state.inning}회`, '우리 팀 공격!');
    setTimeout(()=>triggerAIPitch(),2500);
  }
}

// AI pitcher throws to batter view
function triggerAIPitch(){
  if(state.gameOver) return;
  pitchIncoming=true;
  const pitchNames=Object.keys(PITCH_TYPES);
  const pName=pitchNames[Math.floor(Math.random()*pitchNames.length)];
  const pt=PITCH_TYPES[pName];
  const inZone=Math.random()<pt.strikeRate;
  pitchResult_batter={inZone, pt, name:pName};

  // Animate ball in batter view
  animatePitchToBatter(inZone, pName);
  setCommentaryB(`${pName} ${pt.speed}km/h!`);
}

function animatePitchToBatter(inZone, pName){
  const ball=document.getElementById('pitchBallB');
  ball.style.display='block';
  ball.style.left='calc(50% - 6px)';
  ball.style.top='25%';
  ball.style.transform='scale(0.3)';
  ball.style.transition='none';

  const zone=document.getElementById('pitchZone');
  const zRect=zone.getBoundingClientRect();
  const fieldRect=document.getElementById('batterFieldBg').getBoundingClientRect();

  let targetLeft,targetTop;
  if(inZone){
    targetLeft=(zRect.left-fieldRect.left+Math.random()*zRect.width)+'px';
    targetTop=(zRect.top-fieldRect.top+Math.random()*zRect.height)+'px';
  } else {
    const side=Math.random();
    if(side<0.25){ targetLeft=(zRect.left-fieldRect.left-20+Math.random()*15)+'px'; targetTop=(zRect.top-fieldRect.top+Math.random()*zRect.height)+'px'; }
    else if(side<0.5){ targetLeft=(zRect.right-fieldRect.left+5+Math.random()*15)+'px'; targetTop=(zRect.top-fieldRect.top+Math.random()*zRect.height)+'px'; }
    else if(side<0.75){ targetLeft=(zRect.left-fieldRect.left+Math.random()*zRect.width)+'px'; targetTop=(zRect.top-fieldRect.top-20)+'px'; }
    else{ targetLeft=(zRect.left-fieldRect.left+Math.random()*zRect.width)+'px'; targetTop=(zRect.bottom-fieldRect.top+10)+'px'; }
  }

  setTimeout(()=>{
    ball.style.transition='all 0.45s ease-in';
    ball.style.transform='scale(1.2)';
    ball.style.left=targetLeft;
    ball.style.top=targetTop;
    setTimeout(()=>{ ball.style.display='none'; },460);
  },50);
}

// ══════════════ UI HELPERS ══════════════
function showPitchResult(text, type){
  const el=document.getElementById('pitchResult');
  el.textContent=text; el.className='pitch-result '+type;
  void el.offsetWidth;
  el.classList.add('show');
  setTimeout(()=>el.classList.remove('show'),1300);
}
function showPitchResultB(text, type){
  const el=document.getElementById('pitchResultB');
  el.textContent=text; el.className='pitch-result '+type;
  void el.offsetWidth;
  el.classList.add('show');
  setTimeout(()=>el.classList.remove('show'),1300);
}
function showHitEffect(text){
  const el=document.getElementById('hitEffect');
  el.textContent=text; el.className='hit-effect';
  void el.offsetWidth; el.classList.add('show');
}
function showHitEffectB(text){
  const el=document.getElementById('hitEffectB');
  el.textContent=text; el.className='hit-effect';
  void el.offsetWidth; el.classList.add('show');
}
function setCommentary(txt){
  document.getElementById('commentaryText').innerHTML=`<span class="speaker">[권성욱]</span> ${txt}`;
}
function setCommentaryB(txt){
  document.getElementById('commentaryTextB').innerHTML=`<span class="speaker">[권성욱]</span> ${txt}`;
}

function updateScoreboard(){
  document.getElementById('homeScoreEl').textContent=state.myScore;
  document.getElementById('awayScoreEl').textContent=state.oppScore;
  document.getElementById('inningNum').textContent=state.inning;
  document.getElementById('inningArrow').textContent=state.isTop?'▲':'▽';
  // Counts
  ['s1','s2'].forEach((id,i)=>document.getElementById(id).classList.toggle('s',i<state.strikes));
  ['b1','b2','b3'].forEach((id,i)=>document.getElementById(id).classList.toggle('b',i<state.balls));
  ['o1','o2'].forEach((id,i)=>document.getElementById(id).classList.toggle('o',i<state.outs));
}
function updateBases(){
  document.getElementById('base1').classList.toggle('occupied',state.bases[0]);
  document.getElementById('base2').classList.toggle('occupied',state.bases[1]);
  document.getElementById('base3').classList.toggle('occupied',state.bases[2]);
}

function spawnParticles(container){
  const colors=['#ffd700','#ff6b35','#ff4444','#4CAF50','#2196F3','#fff'];
  for(let i=0;i<20;i++){
    const p=document.createElement('div');
    p.className='particle';
    const size=4+Math.random()*8;
    const tx=(Math.random()-0.5)*200;
    const ty=-(50+Math.random()*100);
    p.style.cssText=`width:${size}px;height:${size}px;background:${colors[Math.floor(Math.random()*colors.length)]};left:${30+Math.random()*40}%;top:${30+Math.random()*30}%;--tx:${tx}px;--ty:${ty}px;--d:${0.5+Math.random()*1}s;`;
    container.appendChild(p);
    setTimeout(()=>p.remove(),1500);
  }
}

// ══════════════ VIEW TOGGLE ══════════════
function setView(v){
  state.view=v;
  if(v==='pitcher'){
    document.getElementById('pitcherView').style.display='flex';
    document.getElementById('batterView').style.display='none';
    document.getElementById('switchViewBtn').textContent='⚾ 타자 시점';
  } else {
    document.getElementById('pitcherView').style.display='none';
    document.getElementById('batterView').style.display='flex';
    document.getElementById('batterView').style.flexDirection='column';
    document.getElementById('switchViewBtn').textContent='⚾ 투수 시점';
    if(!state.isTop && !pitchIncoming){
      setTimeout(()=>triggerAIPitch(),800);
    }
  }
}

function toggleView(){
  if(state.view==='pitcher'){ setView('batter'); }
  else { setView('pitcher'); }
}

// ══════════════ AUTO MODE ══════════════
function switchToAuto(){
  state.autoMode=!state.autoMode;
  if(state.autoMode){
    setCommentary('AUTO 모드! 자동으로 투구합니다.');
    autoPlay();
  }
}
function autoPlay(){
  if(!state.autoMode||state.gameOver) return;
  if(!state.pitching){
    const pitchNames=state.currentPitcher.pitches;
    selectedPitch=pitchNames[Math.floor(Math.random()*pitchNames.length)];
    executePitch();
  }
  setTimeout(()=>autoPlay(), 1500);
}

// ══════════════ LINEUP ══════════════
function showLineup(show){
  document.getElementById('lineupPanel').classList.toggle('show',show);
}
function closeLineup(){
  document.getElementById('lineupPanel').classList.remove('show');
}
function updateLineup(){
  const list=document.getElementById('lineupList');
  list.innerHTML=state.oppRoster.map((p,i)=>`
    <div class="lineup-row ${i===state.currentBatterIdx?'current':''}">
      <div class="lr-num">${i+1}</div>
      <div class="lr-pos">${p.pos}</div>
      <div class="lr-name">${p.name}</div>
      <div class="lr-stats">타율 ${p.avg} 파워 ${p.pow}</div>
    </div>`).join('');
}

// ══════════════ GAME OVER ══════════════
function checkGameOver(){
  if(state.inning>=9 && !state.isTop && state.outs>=3){ endGame(); }
}

function endGame(){
  state.gameOver=true;
  const won = state.myScore > state.oppScore;
  const modal=document.getElementById('resultModal');
  document.getElementById('resultTitle').textContent=won?'승리! 🎉':'패배...';
  document.getElementById('resultTitle').className='result-title '+(won?'win':'lose');
  document.getElementById('resultScore').textContent=`${state.myScore} : ${state.oppScore}`;
  document.getElementById('rHits').textContent=state.totalHits;
  document.getElementById('rSO').textContent=state.totalSO;
  document.getElementById('rHR').textContent=state.totalHR;
  modal.classList.add('show');
}

function restartGame(){
  document.getElementById('resultModal').classList.remove('show');
  // Reset state
  Object.assign(state,{myScore:0,oppScore:0,inning:1,isTop:true,outs:0,strikes:0,balls:0,
    bases:[false,false,false],pitchCount:0,stamina:100,currentBatterIdx:0,
    pitching:false,batting:false,totalHits:0,totalSO:0,totalHR:0,gameOver:false,autoMode:false});
  startGame();
}

function goMenu(){
  document.getElementById('resultModal').classList.remove('show');
  document.getElementById('gameScreen').classList.remove('active');
  document.getElementById('titleScreen').classList.add('active');
}

// Initial update
updateScoreboard();
</script>
</body>
</html>
