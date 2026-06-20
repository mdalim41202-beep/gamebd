<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>🏍️ Yamaha R15M Racing</title>
    <style>
        *{margin:0;padding:0;box-sizing:border-box}
        body{background:#080810;font-family:'Segoe UI',Tahoma,sans-serif;overflow:hidden;display:flex;flex-direction:column;align-items:center;height:100vh;color:#fff;touch-action:none;user-select:none;-webkit-user-select:none}

        .ad-bar{width:100%;background:linear-gradient(90deg,#0a0a2e,#0043a8,#0a0a2e);background-size:200% 100%;animation:shimmer 3s ease infinite;padding:5px;text-align:center;z-index:100}
        @keyframes shimmer{0%,100%{background-position:0% 50%}50%{background-position:100% 50%}}
        .ad-bar a{color:#fff;text-decoration:none;font-size:12px;font-weight:bold;letter-spacing:1px}

        .hud{display:flex;justify-content:space-around;width:100%;max-width:520px;padding:7px 8px;background:rgba(0,30,80,0.9);border-bottom:2px solid #0055cc;font-size:13px;font-weight:bold;z-index:50}
        .hud span{color:#00aaff}
        .hud .brand{color:#ff0000;font-style:italic;font-size:11px;letter-spacing:2px}

        .game-wrap{flex:1;width:100%;max-width:520px;position:relative}
        canvas{width:100%;height:100%;display:block}

        .ad-bottom{width:100%;background:linear-gradient(90deg,#0a1628,#0f2040,#0a1628);padding:5px;text-align:center;z-index:100}
        .ad-bottom a{color:#ffd700;text-decoration:none;font-size:12px;font-weight:bold;margin:0 10px}

        .ov{position:fixed;inset:0;background:rgba(0,5,20,0.95);display:flex;flex-direction:column;align-items:center;justify-content:center;z-index:200;backdrop-filter:blur(12px)}
        .ov.hidden{display:none}
        .ov .title{font-size:44px;font-weight:900;background:linear-gradient(135deg,#0055cc,#00aaff,#fff);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;margin-bottom:2px;letter-spacing:3px}
        .ov .yamaha-logo{font-size:14px;color:#ff0000;font-weight:900;letter-spacing:6px;font-style:italic;margin-bottom:5px}
        .ov .sub{font-size:16px;color:#88aacc;margin-bottom:18px}
        .ov .info{font-size:13px;color:#667;margin:3px 0}
        .ov .score-big{font-size:56px;font-weight:900;color:#00aaff;margin:10px 0}
        .ov .newbest{font-size:15px;color:#ff3300;animation:pulse .6s ease infinite alternate}
        @keyframes pulse{from{transform:scale(1)}to{transform:scale(1.15)}}

        .btn-go{padding:16px 55px;font-size:20px;font-weight:900;border:none;border-radius:60px;cursor:pointer;margin:18px 0 12px;background:linear-gradient(135deg,#0055cc,#0088ff);color:#fff;letter-spacing:2px;box-shadow:0 0 40px rgba(0,100,255,0.35);transition:all .25s;text-transform:uppercase}
        .btn-go:hover{transform:scale(1.08);box-shadow:0 0 60px rgba(0,100,255,0.6)}
        .btn-go:active{transform:scale(.97)}

        .ad-box{margin-top:14px;padding:10px 20px;background:rgba(0,50,150,0.1);border:1px solid rgba(0,150,255,0.2);border-radius:10px}
        .ad-box a{color:#ffd700;text-decoration:none;font-size:13px}

        .touch-zone{position:fixed;bottom:0;width:50%;height:45%;z-index:150;opacity:0}
        .touch-left{left:0}.touch-right{right:0}

        .ctrl-hint{position:fixed;bottom:35px;display:none;width:100%;justify-content:space-between;padding:0 35px;z-index:140;pointer-events:none}
        .ctrl-hint div{width:55px;height:55px;border-radius:50%;border:2px solid rgba(0,100,255,0.2);display:flex;align-items:center;justify-content:center;font-size:22px;color:rgba(0,150,255,0.25)}
        @media(max-width:768px){.ctrl-hint{display:flex}}
        @media(max-width:480px){.ov .title{font-size:30px}.ov .score-big{font-size:40px}.btn-go{padding:13px 38px;font-size:16px}}
    </style>
</head>
<body>

<div class="ad-bar">
    <a href="https://www.effectivecpmnetwork.com/im9jv5fg?key=959cd3dcca7d6dec3514cda4c52dc4b8" target="_blank">⚡ Special Offer — Click Here for Exclusive Rewards! ⚡</a>
</div>

<div class="hud">
    <div class="brand">YAMAHA R15M</div>
    <div>🏆<span id="hScore">0</span></div>
    <div>⚡Lv.<span id="hLevel">1</span></div>
    <div>❤️<span id="hLives">3</span></div>
    <div>🏅<span id="hBest">0</span></div>
</div>

<div class="game-wrap"><canvas id="C"></canvas></div>

<div class="ad-bottom">
    <a href="https://www.effectivecpmnetwork.com/im9jv5fg?key=959cd3dcca7d6dec3514cda4c52dc4b8" target="_blank">🎮 More Games</a>
    <a href="https://www.effectivecpmnetwork.com/im9jv5fg?key=959cd3dcca7d6dec3514cda4c52dc4b8" target="_blank">💎 Free Coins</a>
</div>

<div class="touch-zone touch-left" id="touchL"></div>
<div class="touch-zone touch-right" id="touchR"></div>
<div class="ctrl-hint"><div>◀</div><div>▶</div></div>

<!-- Start -->
<div class="ov" id="scrStart">
    <div class="title">R15M RACER</div>
    <div class="yamaha-logo">YAMAHA</div>
    <div class="sub">🏍️ Yamaha R15M BS7 Racing Game</div>
    <div class="info">← → Arrow Keys দিয়ে বাইক চালান</div>
    <div class="info">📱 মোবাইলে বাম/ডান ট্যাপ করুন</div>
    <div class="info">P = Pause | কয়েন সংগ্রহ করুন!</div>
    <button class="btn-go" onclick="startGame()">🏁 RACE START</button>
    <div class="ad-box"><a href="https://www.effectivecpmnetwork.com/im9jv5fg?key=959cd3dcca7d6dec3514cda4c52dc4b8" target="_blank">🎁 রেস শুরু করার আগে বোনাস নিন!</a></div>
</div>

<!-- Game Over -->
<div class="ov hidden" id="scrOver">
    <div class="title">RACE OVER</div>
    <div class="yamaha-logo">YAMAHA R15M</div>
    <div class="score-big" id="finalScore">0</div>
    <div class="newbest hidden" id="newBestMsg">🎉 NEW HIGH SCORE!</div>
    <div class="info" id="bestInfo"></div>
    <button class="btn-go" onclick="startGame()">🔄 RACE AGAIN</button>
    <div class="ad-box"><a href="https://www.effectivecpmnetwork.com/im9jv5fg?key=959cd3dcca7d6dec3514cda4c52dc4b8" target="_blank">🏆 স্কোর শেয়ার করে পুরস্কার জিতুন!</a></div>
    <div class="ad-box" style="margin-top:8px"><a href="https://www.effectivecpmnetwork.com/im9jv5fg?key=959cd3dcca7d6dec3514cda4c52dc4b8" target="_blank">💰 ফ্রি কয়েন পেতে ক্লিক করুন!</a></div>
</div>

<!-- Pause -->
<div class="ov hidden" id="scrPause">
    <div class="title" style="font-size:30px">⏸ PAUSED</div>
    <button class="btn-go" onclick="resumeGame()">▶ RESUME</button>
    <div class="ad-box"><a href="https://www.effectivecpmnetwork.com/im9jv5fg?key=959cd3dcca7d6dec3514cda4c52dc4b8" target="_blank">⚡ বিরতির সময় অফার দেখুন!</a></div>
</div>

<script>
const canvas=document.getElementById('C'),ctx=canvas.getContext('2d');
let W,H,running=false,paused=false,rafId;
let score,lives,level,baseSpeed,combo,km;
let highScore=+localStorage.getItem('r15HS')||0;
document.getElementById('hBest').textContent=highScore;

const LANES=4;
let laneW,roadL,roadR,roadW;

const P={x:0,y:0,w:32,h:72,vx:0,tilt:0,inv:0,wAng:0};
let enemies,coins,powerups,particles,floats,roadMarks,scenery;
let spawnT,coinT,puT,frame;

const inp={left:false,right:false};
document.addEventListener('keydown',e=>{if(e.key==='ArrowLeft'||e.key==='a')inp.left=true;if(e.key==='ArrowRight'||e.key==='d')inp.right=true;if(e.key==='p'||e.key==='P'||e.key==='Escape')togglePause();});
document.addEventListener('keyup',e=>{if(e.key==='ArrowLeft'||e.key==='a')inp.left=false;if(e.key==='ArrowRight'||e.key==='d')inp.right=false;});
document.getElementById('touchL').addEventListener('touchstart',e=>{e.preventDefault();inp.left=true},{passive:false});
document.getElementById('touchL').addEventListener('touchend',e=>{e.preventDefault();inp.left=false},{passive:false});
document.getElementById('touchR').addEventListener('touchstart',e=>{e.preventDefault();inp.right=true},{passive:false});
document.getElementById('touchR').addEventListener('touchend',e=>{e.preventDefault();inp.right=false},{passive:false});

function resize(){const c=canvas.parentElement;canvas.width=W=c.clientWidth;canvas.height=H=c.clientHeight;roadW=W*.74;roadL=(W-roadW)/2;roadR=roadL+roadW;laneW=roadW/LANES;}
window.addEventListener('resize',resize);
function laneX(l){return roadL+laneW*l+laneW/2}

// =============================================
// YAMAHA R15M V3 PLAYER BIKE - DETAILED DRAWING
// =============================================
function drawR15M(x,y,tilt,inv){
    ctx.save();
    ctx.translate(x,y);
    ctx.rotate(tilt*0.055);

    if(inv>0&&Math.floor(inv/4)%2===0)ctx.globalAlpha=.35;

    const w=P.w, h=P.h;
    const bx=-w/2, by=-h/2;

    // ---- SHADOW ----
    ctx.fillStyle='rgba(0,0,20,0.35)';
    ctx.beginPath();ctx.ellipse(0,h/2+5,w/2+5,6,0,0,Math.PI*2);ctx.fill();

    // ---- REAR WHEEL (wider sport tire) ----
    P.wAng+=baseSpeed*.35;
    ctx.save();ctx.translate(0,h/2-9);
    // Tire
    ctx.fillStyle='#0a0a0a';ctx.beginPath();ctx.arc(0,0,11,0,Math.PI*2);ctx.fill();
    ctx.strokeStyle='#1a1a1a';ctx.lineWidth=4;ctx.beginPath();ctx.arc(0,0,9,0,Math.PI*2);ctx.stroke();
    // Tire treads
    ctx.strokeStyle='#222';ctx.lineWidth=1;
    for(let i=0;i<8;i++){const a=P.wAng+i*Math.PI/4;ctx.beginPath();ctx.moveTo(Math.cos(a)*8,Math.sin(a)*8);ctx.lineTo(Math.cos(a)*11,Math.sin(a)*11);ctx.stroke();}
    // Rim
    ctx.fillStyle='#333';ctx.beginPath();ctx.arc(0,0,6,0,Math.PI*2);ctx.fill();
    // Spokes (5-spoke sport rim)
    ctx.strokeStyle='#888';ctx.lineWidth=1.5;
    for(let i=0;i<5;i++){const a=P.wAng*0.5+i*Math.PI*2/5;ctx.beginPath();ctx.moveTo(0,0);ctx.lineTo(Math.cos(a)*6,Math.sin(a)*6);ctx.stroke();}
    ctx.fillStyle='#aaa';ctx.beginPath();ctx.arc(0,0,2,0,Math.PI*2);ctx.fill();
    ctx.restore();

    // ---- FRONT WHEEL ----
    ctx.save();ctx.translate(0,-h/2+10);
    ctx.fillStyle='#0a0a0a';ctx.beginPath();ctx.arc(0,0,10,0,Math.PI*2);ctx.fill();
    ctx.strokeStyle='#1a1a1a';ctx.lineWidth=3.5;ctx.beginPath();ctx.arc(0,0,8,0,Math.PI*2);ctx.stroke();
    ctx.strokeStyle='#222';ctx.lineWidth=1;
    for(let i=0;i<8;i++){const a=P.wAng+i*Math.PI/4;ctx.beginPath();ctx.moveTo(Math.cos(a)*7,Math.sin(a)*7);ctx.lineTo(Math.cos(a)*10,Math.sin(a)*10);ctx.stroke();}
    ctx.fillStyle='#333';ctx.beginPath();ctx.arc(0,0,5,0,Math.PI*2);ctx.fill();
    ctx.strokeStyle='#888';ctx.lineWidth=1.5;
    for(let i=0;i<5;i++){const a=P.wAng*0.5+i*Math.PI*2/5;ctx.beginPath();ctx.moveTo(0,0);ctx.lineTo(Math.cos(a)*5,Math.sin(a)*5);ctx.stroke();}
    ctx.fillStyle='#aaa';ctx.beginPath();ctx.arc(0,0,2,0,Math.PI*2);ctx.fill();
    ctx.restore();

    // ---- SWINGARM ----
    ctx.strokeStyle='#555';ctx.lineWidth=3;ctx.lineCap='round';
    ctx.beginPath();ctx.moveTo(-3,h/6);ctx.lineTo(0,h/2-9);ctx.stroke();
    ctx.beginPath();ctx.moveTo(3,h/6);ctx.lineTo(0,h/2-9);ctx.stroke();

    // ---- CHAIN / SPROCKET ----
    ctx.strokeStyle='#444';ctx.lineWidth=1;
    ctx.beginPath();ctx.moveTo(3,h/6+2);ctx.lineTo(2,h/2-12);ctx.stroke();

    // ---- REAR SHOCK ----
    ctx.strokeStyle='#ffd700';ctx.lineWidth=2;
    ctx.beginPath();ctx.moveTo(-4,h/8);ctx.lineTo(-2,h/2-14);ctx.stroke();
    ctx.fillStyle='#ffd700';ctx.beginPath();ctx.arc(-4,h/8,2,0,Math.PI*2);ctx.fill();

    // ---- ENGINE BLOCK (liquid-cooled look) ----
    const engGrd=ctx.createLinearGradient(-10,h/7,10,h/7);
    engGrd.addColorStop(0,'#2a2a2a');engGrd.addColorStop(.5,'#4a4a4a');engGrd.addColorStop(1,'#2a2a2a');
    ctx.fillStyle=engGrd;
    ctx.beginPath();
    ctx.moveTo(-11,h/7-8);ctx.lineTo(11,h/7-8);ctx.lineTo(12,h/7+8);ctx.lineTo(-12,h/7+8);ctx.closePath();ctx.fill();
    ctx.strokeStyle='#222';ctx.lineWidth=.8;ctx.stroke();
    // Cooling fins
    ctx.strokeStyle='#555';ctx.lineWidth=.7;
    for(let i=-6;i<=6;i+=2){ctx.beginPath();ctx.moveTo(-11,h/7+i);ctx.lineTo(11,h/7+i);ctx.stroke();}
    // Cylinder head
    ctx.fillStyle='#3a3a3a';ctx.fillRect(-6,h/7-12,12,5);
    ctx.strokeStyle='#555';ctx.lineWidth=.5;ctx.strokeRect(-6,h/7-12,12,5);

    // ---- USD FRONT FORKS (Gold - R15M signature) ----
    ctx.strokeStyle='#c8a000';ctx.lineWidth=3;
    ctx.beginPath();ctx.moveTo(-6,-h/6);ctx.lineTo(-4,-h/2+10);ctx.stroke();
    ctx.beginPath();ctx.moveTo(6,-h/6);ctx.lineTo(4,-h/2+10);ctx.stroke();
    // Lower fork (silver)
    ctx.strokeStyle='#bbb';ctx.lineWidth=2.5;
    ctx.beginPath();ctx.moveTo(-5,-h/3);ctx.lineTo(-6,-h/6);ctx.stroke();
    ctx.beginPath();ctx.moveTo(5,-h/3);ctx.lineTo(6,-h/6);ctx.stroke();

    // ---- MAIN FRAME (Deltabox) ----
    ctx.strokeStyle='#444';ctx.lineWidth=2.5;ctx.lineCap='round';
    ctx.beginPath();ctx.moveTo(0,h/6);ctx.lineTo(-2,-h/5);ctx.stroke();
    ctx.beginPath();ctx.moveTo(0,h/6);ctx.lineTo(2,-h/5);ctx.stroke();

    // ---- FUEL TANK (Yamaha Racing Blue) ----
    const tankGrd=ctx.createLinearGradient(-12,-h/5,12,-h/5);
    tankGrd.addColorStop(0,'#002277');tankGrd.addColorStop(.25,'#0044bb');tankGrd.addColorStop(.5,'#0066ee');tankGrd.addColorStop(.75,'#0044bb');tankGrd.addColorStop(1,'#002277');
    ctx.fillStyle=tankGrd;
    ctx.beginPath();
    ctx.moveTo(-13,-h/5-4);
    ctx.quadraticCurveTo(-14,-h/5,  -13,-h/5+6);
    ctx.lineTo(-4,-h/5+8);
    ctx.lineTo(4,-h/5+8);
    ctx.lineTo(13,-h/5+6);
    ctx.quadraticCurveTo(14,-h/5,  13,-h/5-4);
    ctx.lineTo(5,-h/5-7);
    ctx.lineTo(-5,-h/5-7);
    ctx.closePath();
    ctx.fill();
    ctx.strokeStyle='#001a55';ctx.lineWidth=.8;ctx.stroke();
    // Tank highlight
    ctx.fillStyle='rgba(100,180,255,0.25)';
    ctx.beginPath();ctx.ellipse(-4,-h/5-3,6,2.5,-.2,0,Math.PI*2);ctx.fill();
    // Tank stripe (Yamaha speed block)
    ctx.fillStyle='rgba(255,255,255,0.15)';
    ctx.fillRect(-1,-h/5-6,2,14);
    // "R15" on tank
    ctx.fillStyle='rgba(255,255,255,0.5)';
    ctx.font='bold 5px Arial';ctx.textAlign='center';
    ctx.fillText('R15M',0,-h/5+2);

    // ---- FULL FAIRING (R15M aggressive bodywork) ----
    // Upper fairing
    const fairGrd=ctx.createLinearGradient(-14,-h/3,14,-h/3);
    fairGrd.addColorStop(0,'#001a66');fairGrd.addColorStop(.3,'#0044cc');fairGrd.addColorStop(.7,'#0044cc');fairGrd.addColorStop(1,'#001a66');
    ctx.fillStyle=fairGrd;
    ctx.beginPath();
    ctx.moveTo(-5,-h/5-7);
    ctx.lineTo(-10,-h/3);
    ctx.quadraticCurveTo(-14,-h/3-8, -10,-h/2+16);
    ctx.lineTo(-3,-h/2+8);
    ctx.lineTo(3,-h/2+8);
    ctx.lineTo(10,-h/2+16);
    ctx.quadraticCurveTo(14,-h/3-8, 10,-h/3);
    ctx.lineTo(5,-h/5-7);
    ctx.closePath();
    ctx.fill();
    ctx.strokeStyle='#002266';ctx.lineWidth=.6;ctx.stroke();

    // Lower fairing / belly pan
    const lowerGrd=ctx.createLinearGradient(-13,0,13,0);
    lowerGrd.addColorStop(0,'#001a55');lowerGrd.addColorStop(.5,'#003399');lowerGrd.addColorStop(1,'#001a55');
    ctx.fillStyle=lowerGrd;
    ctx.beginPath();
    ctx.moveTo(-13,-h/5+6);
    ctx.lineTo(-12,h/8);
    ctx.lineTo(-8,h/6);
    ctx.lineTo(8,h/6);
    ctx.lineTo(12,h/8);
    ctx.lineTo(13,-h/5+6);
    ctx.closePath();
    ctx.fill();
    ctx.strokeStyle='#001a44';ctx.lineWidth=.5;ctx.stroke();

    // Fairing air vent
    ctx.fillStyle='rgba(0,0,0,0.3)';
    ctx.beginPath();
    ctx.moveTo(-12,-h/5+1);ctx.lineTo(-11,-h/7);ctx.lineTo(-8,-h/7+2);ctx.lineTo(-9,-h/5+3);ctx.closePath();ctx.fill();
    ctx.beginPath();
    ctx.moveTo(12,-h/5+1);ctx.lineTo(11,-h/7);ctx.lineTo(8,-h/7+2);ctx.lineTo(9,-h/5+3);ctx.closePath();ctx.fill();

    // Speed block graphics (white/silver stripes on fairing)
    ctx.fillStyle='rgba(255,255,255,0.12)';
    ctx.beginPath();ctx.moveTo(-10,-h/3);ctx.lineTo(-12,-h/4);ctx.lineTo(-10,-h/5);ctx.lineTo(-8,-h/4);ctx.closePath();ctx.fill();
    ctx.beginPath();ctx.moveTo(10,-h/3);ctx.lineTo(12,-h/4);ctx.lineTo(10,-h/5);ctx.lineTo(8,-h/4);ctx.closePath();ctx.fill();

    // ---- SEAT (Split seat R15M style) ----
    // Rider seat
    ctx.fillStyle='#1a1a1a';
    ctx.beginPath();
    ctx.moveTo(-8,-h/4-1);ctx.quadraticCurveTo(0,-h/4-5,8,-h/4-1);
    ctx.lineTo(7,h/10);ctx.quadraticCurveTo(0,h/10+3,-7,h/10);ctx.closePath();ctx.fill();
    // Seat texture lines
    ctx.strokeStyle='#2a2a2a';ctx.lineWidth=.4;
    for(let i=-2;i<=4;i++){ctx.beginPath();ctx.moveTo(-6,-h/4+i*3);ctx.lineTo(6,-h/4+i*3);ctx.stroke();}

    // Pillion seat (raised)
    ctx.fillStyle='#151515';
    ctx.beginPath();
    ctx.moveTo(-6,h/10+1);ctx.quadraticCurveTo(0,h/10-1,6,h/10+1);
    ctx.lineTo(5,h/5);ctx.quadraticCurveTo(0,h/5+3,-5,h/5);ctx.closePath();ctx.fill();

    // ---- TAIL SECTION (aggressive R15M tail) ----
    const tailGrd=ctx.createLinearGradient(-8,h/5,8,h/5);
    tailGrd.addColorStop(0,'#001a66');tailGrd.addColorStop(.5,'#0033aa');tailGrd.addColorStop(1,'#001a66');
    ctx.fillStyle=tailGrd;
    ctx.beginPath();
    ctx.moveTo(-7,h/5);ctx.lineTo(-5,h/3+2);ctx.lineTo(5,h/3+2);ctx.lineTo(7,h/5);ctx.closePath();ctx.fill();

    // ---- RIDER ----
    // Legs
    ctx.fillStyle='#111';
    ctx.beginPath();ctx.moveTo(-8,h/10);ctx.lineTo(-10,h/6+4);ctx.lineTo(-7,h/6+4);ctx.lineTo(-5,h/10);ctx.closePath();ctx.fill();
    ctx.beginPath();ctx.moveTo(8,h/10);ctx.lineTo(10,h/6+4);ctx.lineTo(7,h/6+4);ctx.lineTo(5,h/10);ctx.closePath();ctx.fill();
    // Boots
    ctx.fillStyle='#0a0a0a';
    ctx.fillRect(-11,h/6+2,5,4);ctx.fillRect(6,h/6+2,5,4);

    // Torso (racing leathers)
    ctx.fillStyle='#0a0a0a';
    ctx.beginPath();
    ctx.moveTo(-9,-h/4-2);ctx.lineTo(-7,-h/3-10);ctx.lineTo(7,-h/3-10);ctx.lineTo(9,-h/4-2);ctx.closePath();ctx.fill();
    // Racing suit stripe
    ctx.fillStyle='#0055cc';
    ctx.fillRect(-1,-h/3-8,2,14);
    // Shoulder pads
    ctx.fillStyle='#222';
    ctx.beginPath();ctx.arc(-8,-h/3-6,3,0,Math.PI*2);ctx.fill();
    ctx.beginPath();ctx.arc(8,-h/3-6,3,0,Math.PI*2);ctx.fill();

    // Helmet (Yamaha blue racing helmet)
    const helmGrd=ctx.createRadialGradient(0,-h/3-17,2,0,-h/3-17,11);
    helmGrd.addColorStop(0,'#0066ee');helmGrd.addColorStop(.6,'#0044aa');helmGrd.addColorStop(1,'#002266');
    ctx.fillStyle=helmGrd;
    ctx.beginPath();ctx.arc(0,-h/3-17,11,0,Math.PI*2);ctx.fill();
    ctx.strokeStyle='#001a55';ctx.lineWidth=.8;ctx.stroke();
    // Helmet stripe
    ctx.fillStyle='rgba(255,255,255,0.2)';
    ctx.fillRect(-1,-h/3-28,2,22);
    // Visor
    ctx.fillStyle='rgba(0,200,255,0.45)';
    ctx.beginPath();
    ctx.moveTo(-9,-h/3-19);ctx.quadraticCurveTo(0,-h/3-24,9,-h/3-19);
    ctx.quadraticCurveTo(0,-h/3-14,-9,-h/3-19);ctx.fill();
    // Visor glare
    ctx.fillStyle='rgba(255,255,255,0.35)';
    ctx.beginPath();ctx.ellipse(-3,-h/3-20,3.5,1.5,-.3,0,Math.PI*2);ctx.fill();
    // Chin guard
    ctx.fillStyle='#003388';
    ctx.beginPath();
    ctx.moveTo(-6,-h/3-10);ctx.quadraticCurveTo(0,-h/3-7,6,-h/3-10);
    ctx.quadraticCurveTo(0,-h/3-12,-6,-h/3-10);ctx.fill();

    // Arms (tucked racing position)
    ctx.strokeStyle='#0a0a0a';ctx.lineWidth=4.5;ctx.lineCap='round';
    ctx.beginPath();ctx.moveTo(-8,-h/3-8);ctx.quadraticCurveTo(-14,-h/2+17,-6,-h/2+14);ctx.stroke();
    ctx.beginPath();ctx.moveTo(8,-h/3-8);ctx.quadraticCurveTo(14,-h/2+17,6,-h/2+14);ctx.stroke();
    // Racing gloves
    ctx.fillStyle='#0055cc';
    ctx.beginPath();ctx.arc(-6,-h/2+14,3,0,Math.PI*2);ctx.fill();
    ctx.beginPath();ctx.arc(6,-h/2+14,3,0,Math.PI*2);ctx.fill();

    // ---- CLIP-ON HANDLEBARS ----
    ctx.strokeStyle='#999';ctx.lineWidth=1.8;
    ctx.beginPath();ctx.moveTo(-10,-h/2+13);ctx.lineTo(10,-h/2+13);ctx.stroke();
    // Bar-end mirrors (small round)
    ctx.fillStyle='#666';
    ctx.beginPath();ctx.arc(-11,-h/2+13,2,0,Math.PI*2);ctx.fill();
    ctx.beginPath();ctx.arc(11,-h/2+13,2,0,Math.PI*2);ctx.fill();
    ctx.fillStyle='rgba(150,200,255,0.4)';
    ctx.beginPath();ctx.arc(-11,-h/2+13,1.2,0,Math.PI*2);ctx.fill();
    ctx.beginPath();ctx.arc(11,-h/2+13,1.2,0,Math.PI*2);ctx.fill();

    // ---- FRONT COWL / WINDSCREEN ----
    ctx.fillStyle='rgba(150,220,255,0.2)';
    ctx.beginPath();
    ctx.moveTo(-4,-h/2+7);
    ctx.quadraticCurveTo(0,-h/2-2,4,-h/2+7);
    ctx.lineTo(2,-h/2+12);ctx.lineTo(-2,-h/2+12);
    ctx.closePath();ctx.fill();
    ctx.strokeStyle='rgba(200,240,255,0.3)';ctx.lineWidth=.5;ctx.stroke();

    // ---- DUAL LED HEADLIGHTS (R15M signature) ----
    // Left headlight
    ctx.fillStyle='#fff';ctx.shadowColor='#ffffff';ctx.shadowBlur=18;
    ctx.beginPath();ctx.ellipse(-5,-h/2+5,3.5,2.5,-.1,0,Math.PI*2);ctx.fill();
    // Right headlight
    ctx.beginPath();ctx.ellipse(5,-h/2+5,3.5,2.5,.1,0,Math.PI*2);ctx.fill();
    ctx.shadowBlur=0;
    // DRL (Daytime Running Light - thin line between)
    ctx.strokeStyle='rgba(200,230,255,0.7)';ctx.lineWidth=1;
    ctx.beginPath();ctx.moveTo(-2,-h/2+5);ctx.lineTo(2,-h/2+5);ctx.stroke();
    // LED inner detail
    ctx.fillStyle='rgba(200,230,255,0.6)';
    ctx.beginPath();ctx.ellipse(-5,-h/2+5,1.5,1,0,0,Math.PI*2);ctx.fill();
    ctx.beginPath();ctx.ellipse(5,-h/2+5,1.5,1,0,0,Math.PI*2);ctx.fill();

    // ---- LED TAIL LIGHT (integrated) ----
    ctx.fillStyle='#ff0000';ctx.shadowColor='#ff0000';ctx.shadowBlur=12;
    ctx.beginPath();ctx.moveTo(-4,h/3+1);ctx.lineTo(4,h/3+1);ctx.lineTo(3,h/3+4);ctx.lineTo(-3,h/3+4);ctx.closePath();ctx.fill();
    ctx.shadowBlur=0;

    // ---- EXHAUST (underbelly, R15M style) ----
    ctx.strokeStyle='#777';ctx.lineWidth=2.5;
    ctx.beginPath();ctx.moveTo(9,h/7+6);ctx.quadraticCurveTo(13,h/4+2,11,h/3);ctx.stroke();
    // Exhaust tip
    ctx.fillStyle='#555';ctx.beginPath();ctx.ellipse(11,h/3,3.5,2.5,0,0,Math.PI*2);ctx.fill();
    ctx.strokeStyle='#333';ctx.lineWidth=.8;ctx.stroke();
    // Heat shield
    ctx.fillStyle='#444';ctx.fillRect(8,h/5,4,6);

    // ---- REAR SETS (foot pegs) ----
    ctx.fillStyle='#888';
    ctx.fillRect(-12,h/6+3,3,2);ctx.fillRect(9,h/6+3,3,2);

    // ---- DISC BRAKES ----
    // Front disc
    ctx.strokeStyle='#bbb';ctx.lineWidth=1;
    ctx.beginPath();ctx.arc(0,-h/2+10,6,0,Math.PI*2);ctx.stroke();
    ctx.strokeStyle='rgba(255,100,0,0.3)';ctx.lineWidth=.5;
    ctx.beginPath();ctx.arc(0,-h/2+10,5,0,Math.PI*2);ctx.stroke();
    // Rear disc
    ctx.strokeStyle='#bbb';ctx.lineWidth=.8;
    ctx.beginPath();ctx.arc(0,h/2-9,5,0,Math.PI*2);ctx.stroke();

    ctx.globalAlpha=1;
    ctx.restore();

    // ---- HEADLIGHT BEAM ----
    if(running){
        ctx.save();ctx.translate(x,y);ctx.rotate(tilt*0.055);
        const bm=ctx.createRadialGradient(0,-P.h/2,0,0,-P.h/2-70,45);
        bm.addColorStop(0,'rgba(200,230,255,0.1)');bm.addColorStop(1,'rgba(200,230,255,0)');
        ctx.fillStyle=bm;
        ctx.beginPath();ctx.moveTo(-14,-P.h/2+5);ctx.lineTo(-30,-P.h/2-70);ctx.lineTo(30,-P.h/2-70);ctx.lineTo(14,-P.h/2+5);ctx.fill();
        ctx.restore();
    }

    // ---- EXHAUST PARTICLES ----
    if(running&&!paused&&frame%2===0){
        particles.push({x:x+11,y:y+P.h/3+2,vx:(Math.random()-.5)*.8+tilt*.2,vy:1.5+Math.random()*2,life:10+Math.random()*8,ml:18,color:'smoke',size:2+Math.random()*2});
        // Occasional blue flame
        if(frame%6===0)particles.push({x:x+11,y:y+P.h/3,vx:(Math.random()-.5),vy:.5+Math.random(),life:6+Math.random()*4,ml:10,color:'#4488ff',size:1.5+Math.random()*2});
    }
}

// ============ ENEMY BIKES ============
function drawEnemyBike(x,y,w,h,colors){
    ctx.save();ctx.translate(x,y);

    ctx.fillStyle='rgba(0,0,0,0.25)';ctx.beginPath();ctx.ellipse(0,h/2+3,w/2+2,4,0,0,Math.PI*2);ctx.fill();

    // Wheels
    ctx.fillStyle='#0a0a0a';
    ctx.beginPath();ctx.arc(0,h/2-7,9,0,Math.PI*2);ctx.fill();
    ctx.strokeStyle='#1a1a1a';ctx.lineWidth=3;ctx.beginPath();ctx.arc(0,h/2-7,7.5,0,Math.PI*2);ctx.stroke();
    ctx.fillStyle='#333';ctx.beginPath();ctx.arc(0,h/2-7,4,0,Math.PI*2);ctx.fill();

    ctx.fillStyle='#0a0a0a';
    ctx.beginPath();ctx.arc(0,-h/2+9,8,0,Math.PI*2);ctx.fill();
    ctx.strokeStyle='#1a1a1a';ctx.lineWidth=3;ctx.beginPath();ctx.arc(0,-h/2+9,6.5,0,Math.PI*2);ctx.stroke();
    ctx.fillStyle='#333';ctx.beginPath();ctx.arc(0,-h/2+9,3.5,0,Math.PI*2);ctx.fill();

    // Forks
    ctx.strokeStyle='#888';ctx.lineWidth=2;
    ctx.beginPath();ctx.moveTo(-4,-h/6);ctx.lineTo(-3,-h/2+9);ctx.stroke();
    ctx.beginPath();ctx.moveTo(4,-h/6);ctx.lineTo(3,-h/2+9);ctx.stroke();

    // Frame
    ctx.strokeStyle=colors[1];ctx.lineWidth=2;ctx.lineCap='round';
    ctx.beginPath();ctx.moveTo(0,h/6);ctx.lineTo(0,-h/5);ctx.stroke();

    // Swingarm
    ctx.strokeStyle='#555';ctx.lineWidth=2;
    ctx.beginPath();ctx.moveTo(0,h/6);ctx.lineTo(0,h/2-7);ctx.stroke();

    // Engine
    ctx.fillStyle='#333';ctx.fillRect(-8,h/7-5,16,10);

    // Fairing
    const fg=ctx.createLinearGradient(-12,-h/4,12,-h/4);
    fg.addColorStop(0,colors[1]);fg.addColorStop(.5,colors[0]);fg.addColorStop(1,colors[1]);
    ctx.fillStyle=fg;
    ctx.beginPath();
    ctx.moveTo(-4,-h/5-4);ctx.lineTo(-11,-h/4);ctx.lineTo(-12,-h/6);ctx.lineTo(-10,h/8);
    ctx.lineTo(10,h/8);ctx.lineTo(12,-h/6);ctx.lineTo(11,-h/4);ctx.lineTo(4,-h/5-4);ctx.closePath();ctx.fill();

    // Tank
    ctx.fillStyle=colors[0];
    ctx.beginPath();ctx.ellipse(0,-h/5,10,5,0,0,Math.PI*2);ctx.fill();

    // Seat
    ctx.fillStyle='#191919';
    ctx.beginPath();ctx.ellipse(0,-h/8,6,3,0,0,Math.PI);ctx.fill();

    // Tail
    ctx.fillStyle=colors[1];
    ctx.beginPath();ctx.moveTo(-5,h/6);ctx.lineTo(-4,h/3);ctx.lineTo(4,h/3);ctx.lineTo(5,h/6);ctx.closePath();ctx.fill();

    // Rider
    ctx.fillStyle=colors[2]||'#1a1a1a';
    ctx.beginPath();ctx.moveTo(-6,-h/4);ctx.lineTo(-5,-h/3-6);ctx.lineTo(5,-h/3-6);ctx.lineTo(6,-h/4);ctx.closePath();ctx.fill();
    // Helmet
    ctx.fillStyle='#222';ctx.beginPath();ctx.arc(0,-h/3-13,9,0,Math.PI*2);ctx.fill();
    ctx.fillStyle='rgba(200,200,255,0.25)';
    ctx.beginPath();ctx.moveTo(-7,-h/3-15);ctx.quadraticCurveTo(0,-h/3-19,7,-h/3-15);ctx.quadraticCurveTo(0,-h/3-11,-7,-h/3-15);ctx.fill();
    // Arms
    ctx.strokeStyle=colors[2]||'#1a1a1a';ctx.lineWidth=3.5;
    ctx.beginPath();ctx.moveTo(-5,-h/3-5);ctx.lineTo(-7,-h/2+12);ctx.stroke();
    ctx.beginPath();ctx.moveTo(5,-h/3-5);ctx.lineTo(7,-h/2+12);ctx.stroke();
    // Handlebar
    ctx.strokeStyle='#aaa';ctx.lineWidth=1.5;ctx.beginPath();ctx.moveTo(-8,-h/2+11);ctx.lineTo(8,-h/2+11);ctx.stroke();

    // Headlight
    ctx.fillStyle='#ffee88';ctx.shadowColor='#ffee88';ctx.shadowBlur=8;
    ctx.beginPath();ctx.ellipse(0,-h/2+5,4,2.5,0,0,Math.PI*2);ctx.fill();
    ctx.shadowBlur=0;
    // Taillight
    ctx.fillStyle='#ff2222';ctx.shadowColor='#ff0000';ctx.shadowBlur=6;
    ctx.beginPath();ctx.ellipse(0,h/3+1,4,2,0,0,Math.PI*2);ctx.fill();
    ctx.shadowBlur=0;
    // Exhaust
    ctx.strokeStyle='#666';ctx.lineWidth=2;
    ctx.beginPath();ctx.moveTo(8,h/7);ctx.quadraticCurveTo(11,h/4,10,h/3-2);ctx.stroke();

    ctx.restore();
}

const EC=[['#e94560','#aa2244','#221118'],['#4a90d9','#2a5a90','#141e30'],['#f5a623','#b07818','#2a1e0e'],['#9b59b6','#6a3080','#1a0e22'],['#1abc9c','#0e8a6e','#0a2820'],['#ff6b9d','#cc4477','#22101a'],['#ff4400','#aa2e00','#221008']];

function drawCoin(c){c.angle+=.08;const s=Math.abs(Math.cos(c.angle));ctx.save();ctx.translate(c.x,c.y);ctx.scale(s,1);ctx.shadowColor='#ffd700';ctx.shadowBlur=12;ctx.fillStyle='#ffd700';ctx.beginPath();ctx.arc(0,0,12,0,Math.PI*2);ctx.fill();ctx.strokeStyle='#b8860b';ctx.lineWidth=2;ctx.stroke();ctx.shadowBlur=0;ctx.fillStyle='#b8860b';ctx.font='bold 12px Arial';ctx.textAlign='center';ctx.textBaseline='middle';ctx.fillText('$',0,1);ctx.restore();}

function drawPU(pu){ctx.save();ctx.translate(pu.x,pu.y);const g=pu.type==='shield'?'#00aaff':'#ff4444';ctx.shadowColor=g;ctx.shadowBlur=18;ctx.fillStyle=g;ctx.beginPath();ctx.arc(0,0,14,0,Math.PI*2);ctx.fill();ctx.shadowBlur=0;ctx.fillStyle='#fff';ctx.font='15px Arial';ctx.textAlign='center';ctx.textBaseline='middle';ctx.fillText(pu.type==='shield'?'🛡':'❤️',0,1);ctx.restore();}

function drawRoad(){
    let g=ctx.createLinearGradient(0,0,roadL,0);g.addColorStop(0,'#0b2e0b');g.addColorStop(1,'#155a15');ctx.fillStyle=g;ctx.fillRect(0,0,roadL,H);
    g=ctx.createLinearGradient(roadR,0,W,0);g.addColorStop(0,'#155a15');g.addColorStop(1,'#0b2e0b');ctx.fillStyle=g;ctx.fillRect(roadR,0,W-roadR,H);
    g=ctx.createLinearGradient(roadL,0,roadR,0);g.addColorStop(0,'#252525');g.addColorStop(.03,'#353535');g.addColorStop(.5,'#434343');g.addColorStop(.97,'#353535');g.addColorStop(1,'#252525');ctx.fillStyle=g;ctx.fillRect(roadL,0,roadW,H);
    const spd=baseSpeed*2;
    for(let m of roadMarks){ctx.fillStyle=m.c?'#0055cc':'#fff';ctx.fillRect(roadL-5,m.y,5,20);ctx.fillRect(roadR,m.y,5,20);}
    ctx.fillStyle='rgba(255,255,255,0.3)';
    for(let i=1;i<LANES;i++){const lx=roadL+laneW*i;for(let m of roadMarks)ctx.fillRect(lx-1,m.y,2,22);}
    for(let m of roadMarks){m.y+=spd;if(m.y>H+30){m.y-=(roadMarks.length/2)*40;m.c=!m.c;}}
}

function drawScene(){for(let s of scenery){s.y+=baseSpeed*.7;if(s.y>H+50){s.y=-50;s.x=s.side===0?Math.random()*roadL*.55+5:roadR+8+Math.random()*(W-roadR-25);s.type=Math.floor(Math.random()*3);}ctx.save();ctx.translate(s.x,s.y);if(s.type===0){ctx.fillStyle='#1d5a12';ctx.beginPath();ctx.moveTo(0,-20);ctx.lineTo(-9,0);ctx.lineTo(9,0);ctx.fill();ctx.fillStyle='#3d2b1f';ctx.fillRect(-2,0,4,9);}else if(s.type===1){ctx.fillStyle='#1a6a1a';ctx.beginPath();ctx.arc(0,0,7,0,Math.PI*2);ctx.fill();ctx.beginPath();ctx.arc(5,2,5,0,Math.PI*2);ctx.fill();}else{ctx.fillStyle='#555';ctx.beginPath();ctx.arc(0,0,5,0,Math.PI*2);ctx.fill();}ctx.restore();}}

function drawParts(){for(let i=particles.length-1;i>=0;i--){const p=particles[i];p.x+=p.vx;p.y+=p.vy;p.life--;if(p.life<=0){particles.splice(i,1);continue;}const a=p.life/p.ml;ctx.globalAlpha=a;if(p.color==='smoke'){ctx.fillStyle=`rgba(140,140,160,${a*.3})`;ctx.beginPath();ctx.arc(p.x,p.y,p.size*(1+(1-a)*2),0,Math.PI*2);ctx.fill();}else{ctx.fillStyle=p.color;ctx.beginPath();ctx.arc(p.x,p.y,p.size*a,0,Math.PI*2);ctx.fill();}ctx.globalAlpha=1;}}
function boom(x,y,c){for(let i=0;i<30;i++){const a=Math.random()*Math.PI*2,s=2+Math.random()*7;particles.push({x,y,vx:Math.cos(a)*s,vy:Math.sin(a)*s,life:20+Math.random()*20,ml:40,color:c,size:2+Math.random()*4});}for(let i=0;i<12;i++)particles.push({x:x+(Math.random()-.5)*15,y:y+(Math.random()-.5)*15,vx:(Math.random()-.5)*3,vy:-1-Math.random()*4,life:15+Math.random()*12,ml:27,color:['#ff4400','#ff8800','#ffcc00'][~~(Math.random()*3)],size:3+Math.random()*5});}
function sparkle(x,y,c){for(let i=0;i<10;i++){const a=Math.random()*Math.PI*2;particles.push({x,y,vx:Math.cos(a)*3,vy:Math.sin(a)*3,life:14+Math.random()*8,ml:22,color:c,size:2+Math.random()*3});}}

let fTexts=[];
function showF(x,y,t,c){fTexts.push({x,y,text:t,color:c,life:35});}
function drawF(){for(let i=fTexts.length-1;i>=0;i--){const f=fTexts[i];f.y-=1.8;f.life--;if(f.life<=0){fTexts.splice(i,1);continue;}ctx.globalAlpha=f.life/35;ctx.fillStyle=f.color;ctx.font='bold 18px Arial';ctx.textAlign='center';ctx.fillText(f.text,f.x,f.y);ctx.globalAlpha=1;}}

function drawSL(){if(baseSpeed<4)return;const n=Math.min((baseSpeed-4)/5,1);ctx.strokeStyle=`rgba(255,255,255,${.04*n})`;ctx.lineWidth=1;for(let i=0;i<10*n;i++){const x=roadL+Math.random()*roadW,y=Math.random()*H;ctx.beginPath();ctx.moveTo(x,y);ctx.lineTo(x+(Math.random()-.5)*2,y+20+baseSpeed*4);ctx.stroke();}}

function col(ax,ay,aw,ah,bx,by,bw,bh){return ax-aw/2<bx+bw/2&&ax+aw/2>bx-bw/2&&ay-ah/2<by+bh/2&&ay+ah/2>by-bh/2;}

function updateHUD(){document.getElementById('hScore').textContent=score;document.getElementById('hLevel').textContent=level;document.getElementById('hLives').textContent=lives;}

function startGame(){
    document.getElementById('scrStart').classList.add('hidden');document.getElementById('scrOver').classList.add('hidden');document.getElementById('scrPause').classList.add('hidden');
    resize();score=0;lives=3;level=1;baseSpeed=2.5;combo=0;km=0;
    enemies=[];coins=[];powerups=[];particles=[];fTexts=[];spawnT=0;coinT=0;puT=0;frame=0;
    roadMarks=[];for(let y=-30;y<H+50;y+=40)roadMarks.push({y,c:(~~(y/40))%2===0});
    scenery=[];for(let i=0;i<14;i++){const s=i%2;scenery.push({x:s===0?Math.random()*roadL*.55+5:roadR+8+Math.random()*(W-roadR-25),y:Math.random()*H*1.5-H*.3,side:s,type:~~(Math.random()*3)});}
    P.x=laneX(1);P.y=H-100;P.vx=0;P.tilt=0;P.inv=0;P.wAng=0;
    updateHUD();running=true;paused=false;if(rafId)cancelAnimationFrame(rafId);loop();
}
function togglePause(){if(!running)return;paused=!paused;if(paused)document.getElementById('scrPause').classList.remove('hidden');else resumeGame();}
function resumeGame(){paused=false;document.getElementById('scrPause').classList.add('hidden');loop();}
function gameOver(){
    running=false;cancelAnimationFrame(rafId);
    const isN=score>highScore;if(isN){highScore=score;localStorage.setItem('r15HS',highScore);document.getElementById('hBest').textContent=highScore;}
    document.getElementById('finalScore').textContent=score;document.getElementById('newBestMsg').classList.toggle('hidden',!isN);document.getElementById('bestInfo').textContent=isN?'':'Best: '+highScore;
    document.getElementById('scrOver').classList.remove('hidden');
    try{window.open('https://www.effectivecpmnetwork.com/im9jv5fg?key=959cd3dcca7d6dec3514cda4c52dc4b8','_blank');}catch(e){}
}

// ---- SPEEDOMETER ----
function drawSpeedo(){
    const cx=W-50,cy=H-50,r=35;
    ctx.save();ctx.globalAlpha=.6;
    // Bg
    ctx.fillStyle='rgba(0,0,0,0.6)';ctx.beginPath();ctx.arc(cx,cy,r+2,0,Math.PI*2);ctx.fill();
    ctx.strokeStyle='#0055cc';ctx.lineWidth=2;ctx.beginPath();ctx.arc(cx,cy,r,0,Math.PI*2);ctx.stroke();
    // Ticks
    for(let i=0;i<=10;i++){
        const ang=-Math.PI*.75+i*(Math.PI*1.5/10);
        const x1=cx+Math.cos(ang)*(r-5),y1=cy+Math.sin(ang)*(r-5);
        const x2=cx+Math.cos(ang)*(r-1),y2=cy+Math.sin(ang)*(r-1);
        ctx.strokeStyle=i>7?'#ff3300':'#0088ff';ctx.lineWidth=i%5===0?2:1;
        ctx.beginPath();ctx.moveTo(x1,y1);ctx.lineTo(x2,y2);ctx.stroke();
    }
    // Needle
    const speedPct=Math.min(baseSpeed/10,1);
    const needleAng=-Math.PI*.75+speedPct*Math.PI*1.5;
    ctx.strokeStyle='#ff3300';ctx.lineWidth=2;
    ctx.beginPath();ctx.moveTo(cx,cy);ctx.lineTo(cx+Math.cos(needleAng)*(r-10),cy+Math.sin(needleAng)*(r-10));ctx.stroke();
    // Center
    ctx.fillStyle='#fff';ctx.beginPath();ctx.arc(cx,cy,3,0,Math.PI*2);ctx.fill();
    // KM/H
    const kmh=Math.floor(60+baseSpeed*18);
    ctx.fillStyle='#00aaff';ctx.font='bold 10px Arial';ctx.textAlign='center';ctx.fillText(kmh,cx,cy+14);
    ctx.fillStyle='#667';ctx.font='7px Arial';ctx.fillText('KM/H',cx,cy+22);
    ctx.globalAlpha=1;ctx.restore();
}

// ---- DISTANCE ----
function drawDist(){
    ctx.save();ctx.globalAlpha=.5;
    ctx.fillStyle='#0066cc';ctx.font='bold 11px Arial';ctx.textAlign='left';
    ctx.fillText('📍 '+(km/100).toFixed(1)+' km',8,H-10);
    ctx.globalAlpha=1;ctx.restore();
}

function loop(){
    if(!running||paused)return;
    frame++;km+=baseSpeed;
    ctx.clearRect(0,0,W,H);

    drawRoad();drawScene();drawSL();

    const acc=.65,fric=.87,maxV=7.5;
    if(inp.left)P.vx-=acc;if(inp.right)P.vx+=acc;
    P.vx*=fric;if(Math.abs(P.vx)<.08)P.vx=0;
    if(P.vx>maxV)P.vx=maxV;if(P.vx<-maxV)P.vx=-maxV;
    P.x+=P.vx;P.tilt+=(P.vx-P.tilt)*.25;
    const mnX=roadL+P.w/2+4,mxX=roadR-P.w/2-4;
    if(P.x<mnX){P.x=mnX;P.vx=0;}if(P.x>mxX){P.x=mxX;P.vx=0;}
    if(P.inv>0)P.inv--;

    drawR15M(P.x,P.y,P.tilt,P.inv);

    level=1+~~(score/20);baseSpeed=2.5+(level-1)*.45;if(baseSpeed>10)baseSpeed=10;

    spawnT++;const si=Math.max(22,65-level*3.5);
    if(spawnT>=si){
        spawnT=0;const l=~~(Math.random()*LANES);
        enemies.push({x:laneX(l),y:-75,w:22,h:52,speed:baseSpeed*(.5+Math.random()*.4),style:EC[~~(Math.random()*EC.length)]});
        if(level>3&&Math.random()<.35){let l2=(l+1+~~(Math.random()*(LANES-1)))%LANES;enemies.push({x:laneX(l2),y:-75-Math.random()*35,w:22,h:52,speed:baseSpeed*(.45+Math.random()*.35),style:EC[~~(Math.random()*EC.length)]});}
    }

    for(let i=enemies.length-1;i>=0;i--){
        const e=enemies[i];e.y+=e.speed+baseSpeed*.6;
        if(e.y>H+100){enemies.splice(i,1);score++;combo++;if(combo%10===0)showF(W/2,H/2,'🔥 x'+combo,'#ff6600');updateHUD();continue;}
        drawEnemyBike(e.x,e.y,e.w,e.h,e.style);
        if(P.inv<=0&&col(P.x,P.y,P.w-6,P.h-10,e.x,e.y,e.w-4,e.h-6)){boom(P.x,P.y,'#0066ee');boom(e.x,e.y,e.style[0]);enemies.splice(i,1);lives--;combo=0;P.inv=90;updateHUD();showF(P.x,P.y-40,'-1 ❤️','#ff4444');if(lives<=0){gameOver();return;}}
    }

    coinT++;if(coinT>=65){coinT=0;coins.push({x:laneX(~~(Math.random()*LANES)),y:-20,speed:baseSpeed*.65,angle:Math.random()*6});}
    for(let i=coins.length-1;i>=0;i--){const c=coins[i];c.y+=c.speed+baseSpeed*.4;if(c.y>H+25){coins.splice(i,1);continue;}drawCoin(c);if(col(P.x,P.y,P.w+4,P.h,c.x,c.y,22,22)){sparkle(c.x,c.y,'#ffd700');coins.splice(i,1);score+=5;showF(c.x,c.y-12,'+5','#ffd700');updateHUD();}}

    puT++;if(puT>=380){puT=0;powerups.push({x:laneX(~~(Math.random()*LANES)),y:-25,speed:baseSpeed*.55,type:Math.random()<.5?'shield':'life'});}
    for(let i=powerups.length-1;i>=0;i--){const pu=powerups[i];pu.y+=pu.speed+baseSpeed*.3;if(pu.y>H+25){powerups.splice(i,1);continue;}drawPU(pu);if(col(P.x,P.y,P.w,P.h,pu.x,pu.y,26,26)){powerups.splice(i,1);sparkle(pu.x,pu.y,pu.type==='shield'?'#00aaff':'#ff4444');if(pu.type==='shield'){P.inv=180;showF(pu.x,pu.y-12,'🛡 Shield!','#00aaff');}else{lives=Math.min(lives+1,5);showF(pu.x,pu.y-12,'+1 ❤️','#ff4444');}updateHUD();}}

    drawParts();drawF();

    if(P.inv>60){ctx.save();ctx.strokeStyle=`rgba(0,100,255,${.3+Math.sin(frame*.15)*.15})`;ctx.lineWidth=2.5;ctx.beginPath();ctx.ellipse(P.x,P.y,P.w/2+12,P.h/2+10,0,0,Math.PI*2);ctx.stroke();ctx.restore();}

    drawSpeedo();drawDist();

    rafId=requestAnimationFrame(loop);
}

resize();
</script>
</body>
</html>
