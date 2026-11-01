<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover">
<title>WARZONE - Survivant</title>
<style>
*{box-sizing:border-box;-webkit-tap-highlight-color:transparent;-webkit-user-select:none;user-select:none;-webkit-touch-callout:none}
html,body{margin:0;height:100%;background:#0a1118;color:#fff;font-family:system-ui,-apple-system,"Segoe UI",Roboto,Arial,sans-serif;overflow:hidden;touch-action:none}
canvas{display:block;width:100vw;height:100dvh}
#hud{position:fixed;inset:0;pointer-events:none}
.panel{position:absolute;background:linear-gradient(180deg,#0b1a26e6,#07111bcc);border:1px solid #3f6a85aa;border-radius:12px;padding:10px 12px;font-size:12px;letter-spacing:.4px;box-shadow:0 4px 18px #0008;backdrop-filter:blur(4px)}
#stats{top:max(10px,env(safe-area-inset-top));left:10px;min-width:150px}
#mission{top:max(10px,env(safe-area-inset-top));right:10px;text-align:right;min-width:120px}
.title{font-weight:800;font-size:11px;letter-spacing:2px;color:#7fd1ff;margin-bottom:6px}
.bar{height:9px;background:#2a3a46;border-radius:6px;overflow:hidden;margin:4px 0 7px;border:1px solid #0008}
#hpbar{height:100%;width:100%;background:linear-gradient(90deg,#1fd37a,#7dffb0);transition:width .15s,background .3s}
#ammobar{height:100%;width:100%;background:linear-gradient(90deg,#ffb02e,#ffe08a)}
.row{display:flex;justify-content:space-between;gap:10px;font-size:12px;margin-top:2px}
.row b{font-variant-numeric:tabular-nums}
#msg{position:absolute;top:24%;left:0;right:0;text-align:center;font-size:clamp(22px,6vw,40px);font-weight:900;letter-spacing:3px;text-shadow:0 3px 14px #000,0 0 30px #ff8a3d88;opacity:0;transition:opacity .35s}
#stick{position:fixed;left:max(18px,env(safe-area-inset-left));bottom:max(24px,env(safe-area-inset-bottom));width:132px;height:132px;border-radius:50%;background:radial-gradient(circle,#ffffff10,#ffffff06);border:2px solid #ffffff44;pointer-events:auto;touch-action:none}
#knob{position:absolute;left:50%;top:50%;width:56px;height:56px;margin:-28px 0 0 -28px;border-radius:50%;background:radial-gradient(circle at 35% 30%,#d8f1ff,#6aa9c9);box-shadow:0 4px 12px #0007;transition:transform .05s}
.btn{position:fixed;border-radius:50%;border:2px solid #ffffff66;color:#fff;font-weight:800;letter-spacing:1px;pointer-events:auto;touch-action:none;display:flex;align-items:center;justify-content:center;box-shadow:0 5px 16px #0008,inset 0 2px 6px #fff3;font-size:11px;text-shadow:0 1px 3px #000}
.btn:active,.btn.on{transform:scale(.92);filter:brightness(1.25)}
#bFire{right:max(20px,env(safe-area-inset-right));bottom:max(30px,env(safe-area-inset-bottom));width:92px;height:92px;background:radial-gradient(circle at 35% 30%,#ff7a6b,#b52a20);font-size:14px}
#bJump{right:max(122px,calc(env(safe-area-inset-right) + 102px));bottom:max(30px,env(safe-area-inset-bottom));width:64px;height:64px;background:radial-gradient(circle at 35% 30%,#7fc8ff,#2a6fa8)}
#bGren{right:max(30px,env(safe-area-inset-right));bottom:calc(max(30px,env(safe-area-inset-bottom)) + 104px);width:64px;height:64px;background:radial-gradient(circle at 35% 30%,#ffc766,#a8681a)}
#bReload{right:max(104px,calc(env(safe-area-inset-right) + 84px));bottom:calc(max(30px,env(safe-area-inset-bottom)) + 80px);width:52px;height:52px;background:radial-gradient(circle at 35% 30%,#8fe8c0,#1f8a63)}
#overlay{position:fixed;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:14px;text-align:center;padding:24px;background:radial-gradient(circle at 50% 40%,#0b1a26dd,#040a10f2);z-index:5}
#overlay h1{margin:0;font-size:clamp(30px,9vw,54px);letter-spacing:6px;font-weight:900;background:linear-gradient(180deg,#fff,#7fd1ff);-webkit-background-clip:text;background-clip:text;color:transparent;filter:drop-shadow(0 4px 14px #000)}
#overlay p{margin:0;color:#b8d2e2;font-size:14px;line-height:1.55;max-width:420px}
#go{padding:15px 38px;font-size:17px;font-weight:800;letter-spacing:2px;border:0;border-radius:14px;color:#04201c;background:linear-gradient(180deg,#34f0cf,#13a58d);box-shadow:0 8px 24px #13bca255,inset 0 2px 0 #fff6;touch-action:manipulation}
#go:active{transform:translateY(2px)}
.hide{display:none!important}
</style>
</head>
<body>
<canvas id="game"></canvas>

<div id="hud">
  <div class="panel" id="stats">
    <div class="title">WARZONE · SURVIVANT</div>
    <div class="row"><span>VIE</span><b id="hpTxt">100</b></div>
    <div class="bar"><div id="hpbar"></div></div>
    <div class="row"><span id="ammoLbl">MUNITIONS</span><b id="ammoTxt">30/30</b></div>
    <div class="bar"><div id="ammobar"></div></div>
    <div class="row"><span>GRENADES</span><b id="grenTxt">3</b></div>
  </div>
  <div class="panel" id="mission">
    <div class="title">MISSION</div>
    <div class="row"><span>Vague</span><b id="waveTxt">1</b></div>
    <div class="row"><span>Ennemis</span><b id="countTxt">0</b></div>
    <div class="row"><span>Éliminations</span><b id="killTxt">0</b></div>
    <div class="row"><span>Score</span><b id="scoreTxt">0</b></div>
  </div>
  <div id="msg"></div>
</div>

<div id="stick"><div id="knob"></div></div>
<button class="btn" id="bFire">TIRER</button>
<button class="btn" id="bJump">SAUT</button>
<button class="btn" id="bGren">BOMBE</button>
<button class="btn" id="bReload">RECH.</button>

<div id="overlay">
  <h1 id="ovTitle">WARZONE</h1>
  <p id="ovText">Survis aux vagues d'ennemis. Déplace-toi avec le joystick, tire (visée automatique), lance des grenades et esquive en sautant.<br><br><small>Clavier : WASD/Flèches · F tirer · G grenade · R recharger · Espace sauter</small></p>
  <button id="go">COMMENCER</button>
</div>

<script>
"use strict";
const $=id=>document.getElementById(id);
const cv=$("game"),ctx=cv.getContext("2d");
let W=0,H=0,HOR=0,F=0;
function resize(){
  const d=Math.min(devicePixelRatio||1,2);
  W=innerWidth;H=innerHeight;
  cv.width=W*d;cv.height=H*d;
  ctx.setTransform(d,0,0,d,0,0);
  HOR=H*.36;F=H*.55;
}
addEventListener("resize",resize);resize();

/* ---------- Constantes & utilitaires ---------- */
const CAM_D=200,CAM_H=90,PLAYER_SPEED=120;
const TYPES={
  grunt :{hp:60 ,sp:34,sc:1  ,dmg:8 ,pts:10,body:"#8a4a3a",vest:"#a5604a",helm:"#5a342b"},
  runner:{hp:35 ,sp:64,sc:.9 ,dmg:6 ,pts:15,body:"#a6803a",vest:"#c49a48",helm:"#6b5426"},
  heavy :{hp:170,sp:20,sc:1.3,dmg:14,pts:35,body:"#4a4f5c",vest:"#636a7a",helm:"#2b2f38"}
};
const rand=(a,b)=>a+Math.random()*(b-a);
const clamp=(v,a,b)=>Math.max(a,Math.min(b,v));
function hash(a,b){let h=Math.imul(a,374761393)^Math.imul(b,668265263);h=Math.imul(h^(h>>>13),1274126177);return((h^(h>>>16))>>>0)/4294967296}
function rr(x,y,w,h,r){ctx.beginPath();if(ctx.roundRect)ctx.roundRect(x,y,w,h,r);else ctx.rect(x,y,w,h)}

/* ---------- Audio ---------- */
let AC=null,noiseBuf=null;
function initAudio(){
  if(AC)return;
  try{
    AC=new(window.AudioContext||window.webkitAudioContext)();
    noiseBuf=AC.createBuffer(1,AC.sampleRate,AC.sampleRate);
    const d=noiseBuf.getChannelData(0);
    for(let i=0;i<d.length;i++)d[i]=Math.random()*2-1;
  }catch(e){AC=null}
}
function noise(dur,freq,type,vol){
  if(!AC)return;
  const s=AC.createBufferSource();s.buffer=noiseBuf;
  const f=AC.createBiquadFilter();f.type=type;f.frequency.value=freq;
  const g=AC.createGain();
  g.gain.setValueAtTime(vol,AC.currentTime);
  g.gain.exponentialRampToValueAtTime(.001,AC.currentTime+dur);
  s.connect(f);f.connect(g);g.connect(AC.destination);
  s.start();s.stop(AC.currentTime+dur);
}
function tone(freq,dur,vol,type){
  if(!AC)return;
  const o=AC.createOscillator(),g=AC.createGain();
  o.type=type||"sine";o.frequency.value=freq;
  g.gain.setValueAtTime(vol,AC.currentTime);
  g.gain.exponentialRampToValueAtTime(.001,AC.currentTime+dur);
  o.connect(g);g.connect(AC.destination);
  o.start();o.stop(AC.currentTime+dur);
}
function sfx(n){
  if(n==="shot")noise(.09,1800,"bandpass",.22);
  else if(n==="boom")noise(.7,420,"lowpass",.7);
  else if(n==="hit")tone(240,.06,.1,"square");
  else if(n==="hurt")tone(90,.16,.25,"sawtooth");
  else if(n==="reload")tone(520,.06,.1,"triangle");
  else if(n==="pick")tone(780,.12,.12,"sine");
}

/* ---------- État du jeu ---------- */
let P,enemies,fx,texts,tracers,nades,booms,packs;
let G={mode:"menu",wave:0,queue:0,spawnT:0,between:0,kills:0,score:0,best:0,shake:0,hurt:0,time:0,target:null};
const cam={x:0};
try{G.best=+localStorage.getItem("warzone_best")||0}catch(e){}
const inp={x:0,y:0,fire:false,jump:false};
const kb={};

function reset(){
  P={x:0,z:0,y:0,vy:0,hp:100,max:100,ammo:30,mag:30,cd:0,reload:0,gren:3,gt:0,muzzle:0,phase:0,moving:false};
  enemies=[];fx=[];texts=[];tracers=[];nades=[];booms=[];packs=[];
  G.wave=0;G.queue=0;G.spawnT=0;G.between=0;G.kills=0;G.score=0;G.shake=0;G.hurt=0;G.target=null;
  cam.x=0;
}
reset();

let msgTimer=null;
function showMsg(t,ms){
  const m=$("msg");m.textContent=t;m.style.opacity=1;
  clearTimeout(msgTimer);msgTimer=setTimeout(()=>m.style.opacity=0,ms||1800);
}
function startWave(){
  G.wave++;
  G.queue=5+G.wave*3;
  G.spawnT=1;
  showMsg("VAGUE "+G.wave,2000);
}
function spawnEnemy(){
  let r=Math.random(),type="grunt";
  if(G.wave>=3&&r<Math.min(.05*G.wave,.25))type="heavy";
  else if(G.wave>=2&&r<.45)type="runner";
  const T=TYPES[type];
  let x,z;
  if(Math.random()<.8){x=P.x+rand(-380,380);z=P.z+rand(380,620)}
  else{const s=Math.random()<.5?-1:1;x=P.x+s*rand(340,440);z=P.z+rand(0,200)}
  const hp=T.hp*(1+G.wave*.05);
  enemies.push({x,z,type,T,sc:T.sc,hp,max:hp,sp:T.sp*rand(.9,1.15),phase:Math.random()*6,flash:0,cd:0});
}
function damagePlayer(n){
  P.hp-=n;G.hurt=.4;G.shake=Math.max(G.shake,7);sfx("hurt");
  if(P.hp<=0){P.hp=0;endGame()}
}
function endGame(){
  G.mode="over";
  if(G.score>G.best){G.best=G.score;try{localStorage.setItem("warzone_best",G.best)}catch(e){}}
  $("ovTitle").textContent="MISSION ÉCHOUÉE";
  $("ovText").innerHTML="Vague atteinte : <b>"+G.wave+"</b><br>Éliminations : <b>"+G.kills+"</b><br>Score : <b>"+G.score+"</b><br>Meilleur score : <b>"+G.best+"</b>";
  $("go").textContent="REJOUER";
  $("overlay").classList.remove("hide");
}

/* ---------- Effets ---------- */
function burst(x,y,z,n,cols,spd,grav,size){
  for(let i=0;i<n;i++){
    const a=Math.random()*Math.PI*2,e=Math.random()*Math.PI-.3;
    const s=rand(.3,1)*spd;
    fx.push({x,y,z,vx:Math.cos(a)*Math.cos(e)*s,vy:Math.abs(Math.sin(e))*s+rand(10,50),vz:Math.sin(a)*Math.cos(e)*s,
      life:rand(.3,.9),max:.9,size:rand(.6,1)*size,col:cols[(Math.random()*cols.length)|0],g:grav});
  }
}
function explode(x,z,big){
  booms.push({x,z,t:0,max:big?.7:.3,r:big?95:30});
  burst(x,10,z,big?46:12,["#ffb02e","#ff6a2c","#ffe08a","#555"],big?200:90,300,big?5:2.5);
  G.shake=Math.max(G.shake,big?14:4);
  sfx(big?"boom":"hit");
}
function floatText(x,y,z,txt,col){texts.push({x,y,z,txt,col:col||"#fff",t:0})}

function killEnemy(e){
  enemies.splice(enemies.indexOf(e),1);
  G.kills++;G.score+=e.T.pts;
  burst(e.x,24,e.z,14,["#a31818","#6b0f0f","#d23b3b"],120,350,2.5);
  floatText(e.x,50*e.sc,e.z,"+"+e.T.pts,"#ffe08a");
  if(Math.random()<.14)packs.push({x:e.x,z:e.z,t:0});
}
function hitEnemy(e,dmg){
  e.hp-=dmg;e.flash=.08;e.z+=2;
  burst(e.x,28*e.sc,e.z,5,["#ffd27a","#ff9a3d"],80,300,1.8);
  sfx("hit");
  if(e.hp<=0)killEnemy(e);
}

/* ---------- Actions du joueur ---------- */
function startReload(){
  if(P.reload>0||P.ammo===P.mag)return;
  P.reload=1.1;sfx("reload");
}
function shoot(){
  if(P.cd>0||P.reload>0)return;
  if(P.ammo<=0){startReload();return}
  P.cd=.13;P.ammo--;P.muzzle=.06;
  G.shake=Math.max(G.shake,1.6);sfx("shot");
  const t=G.target;
  const x1=P.x+6,y1=P.y+38,z1=P.z+8;
  if(t){
    tracers.push({x1,y1,z1,x2:t.x,y2:26*t.sc,z2:t.z,t:0});
    hitEnemy(t,30);
  }else{
    tracers.push({x1,y1,z1,x2:P.x+rand(-25,25),y2:34,z2:P.z+520,t:0});
  }
  if(P.ammo===0)startReload();
}
function throwGrenade(){
  if(G.mode!=="play"||P.gren<=0)return;
  P.gren--;
  const t=G.target;
  nades.push({x:P.x,z:P.z,tx:t?t.x:P.x,tz:t?Math.min(t.z,P.z+260):P.z+190,t:0,dur:.65,sx:P.x,sz:P.z});
}
function grenadeBlast(x,z){
  explode(x,z,true);
  for(const e of enemies.slice()){
    const d=Math.hypot(e.x-x,e.z-z);
    if(d<125){
      const k=1-d/150;
      e.hp-=170*k;e.flash=.12;
      if(e.hp<=0)killEnemy(e);
    }
  }
}

/* ---------- Mise à jour ---------- */
function findTarget(){
  let best=null,bs=1e9;
  for(const e of enemies){
    const dz=e.z-P.z,dx=e.x-P.x;
    if(dz<-60||dz>650)continue;
    const s=Math.hypot(dx,dz)+Math.abs(dx)*1.3;
    if(s<bs){bs=s;best=e}
  }
  G.target=best;
}
function update(dt){
  G.time+=dt;
  cam.x+=(P.x-cam.x)*Math.min(1,dt*8);
  G.shake=Math.max(0,G.shake-dt*28);
  G.hurt=Math.max(0,G.hurt-dt*1.6);
  if(G.mode!=="play")return;

  // Déplacement
  let mx=inp.x,my=inp.y;
  if(kb.left)mx-=1;if(kb.right)mx+=1;if(kb.up)my+=1;if(kb.down)my-=1;
  const m=Math.hypot(mx,my);if(m>1){mx/=m;my/=m}
  P.moving=m>.1;
  P.x+=mx*PLAYER_SPEED*dt;P.z+=my*PLAYER_SPEED*dt;
  if(P.moving)P.phase+=dt*(P.y>0?0:1);
  if((inp.jump||kb.jump)&&P.y===0)P.vy=210;
  P.vy-=620*dt;P.y+=P.vy*dt;
  if(P.y<=0){P.y=0;P.vy=0}

  // Armes
  P.cd-=dt;P.muzzle-=dt;
  if(P.reload>0){P.reload-=dt;if(P.reload<=0){P.reload=0;P.ammo=P.mag}}
  P.gt+=dt;if(P.gren<3&&P.gt>12){P.gren++;P.gt=0}
  findTarget();
  if(inp.fire||kb.fire)shoot();

  // Vagues
  if(G.queue>0){
    G.spawnT-=dt;
    if(G.spawnT<=0&&enemies.length<12+Math.min(G.wave,8)){spawnEnemy();G.queue--;G.spawnT=Math.max(.25,.7-G.wave*.03)}
  }else if(enemies.length===0){
    if(G.between<=0){G.between=3;showMsg("VAGUE "+G.wave+" TERMINÉE",2200);P.hp=Math.min(P.max,P.hp+20);if(P.gren<3)P.gren++}
    G.between-=dt;
    if(G.between<=0)startWave();
  }

  // Ennemis
  for(const e of enemies){
    let dx=P.x-e.x,dz=P.z-e.z,d=Math.hypot(dx,dz)||1;
    let vx=dx/d*e.sp,vz=dz/d*e.sp;
    for(const o of enemies){
      if(o===e)continue;
      const ox=e.x-o.x,oz=e.z-o.z,od=Math.hypot(ox,oz);
      if(od>0&&od<24){vx+=ox/od*30;vz+=oz/od*30}
    }
    e.x+=vx*dt;e.z+=vz*dt;
    e.phase+=dt*e.sp/8;e.flash-=dt;e.cd-=dt;
    if(d<26*e.sc&&P.y<16&&e.cd<=0){e.cd=.9;damagePlayer(e.T.dmg)}
  }

  // Grenades
  for(const n of nades.slice()){
    n.t+=dt;
    const k=Math.min(1,n.t/n.dur);
    n.x=n.sx+(n.tx-n.sx)*k;n.z=n.sz+(n.tz-n.sz)*k;n.y=Math.sin(Math.PI*k)*70;
    if(k>=1){nades.splice(nades.indexOf(n),1);grenadeBlast(n.x,n.z)}
  }

  // Trousses de soin
  for(const p of packs.slice()){
    p.t+=dt;
    if(Math.hypot(p.x-P.x,p.z-P.z)<30){
      packs.splice(packs.indexOf(p),1);
      P.hp=Math.min(P.max,P.hp+25);sfx("pick");floatText(P.x,60,P.z,"+25 VIE","#7dffb0");
    }else if(p.t>20)packs.splice(packs.indexOf(p),1);
  }

  // Effets
  for(const f of fx){f.x+=f.vx*dt;f.y+=f.vy*dt;f.z+=f.vz*dt;f.vy-=f.g*dt;f.life-=dt;if(f.y<0){f.y=0;f.vy*=-.3;f.vx*=.6;f.vz*=.6}}
  fx=fx.filter(f=>f.life>0);
  for(const t of tracers)t.t+=dt;tracers=tracers.filter(t=>t.t<.08);
  for(const b of booms)b.t+=dt;booms=booms.filter(b=>b.t<b.max);
  for(const t of texts)t.t+=dt;texts=texts.filter(t=>t.t<.9);

  // HUD
  $("hpbar").style.width=(P.hp/P.max*100)+"%";
  $("hpbar").style.background=P.hp<30?"linear-gradient(90deg,#e23b3b,#ff8a7a)":"";
  $("hpTxt").textContent=Math.ceil(P.hp);
  $("ammoLbl").textContent=P.reload>0?"RECHARGEMENT…":"MUNITIONS";
  $("ammoTxt").textContent=P.ammo+"/"+P.mag;
  $("ammobar").style.width=(P.reload>0?(1-P.reload/1.1)*100:P.ammo/P.mag*100)+"%";
  $("grenTxt").textContent=P.gren;
  $("waveTxt").textContent=G.wave;
  $("countTxt").textContent=enemies.length+G.queue;
  $("killTxt").textContent=G.kills;
  $("scoreTxt").textContent=G.score;
}

/* ---------- Rendu ---------- */
function proj(x,y,z){
  const dz=z-P.z+CAM_D;
  if(dz<18)return null;
  const s=F/dz;
  return{x:W/2+(x-cam.x)*s,y:HOR+(CAM_H-y)*s,s,d:dz};
}
const gy=z=>HOR+CAM_H*F/(z-P.z+CAM_D);
const fog=d=>clamp(1-(d-650)/900,.15,1);

function drawSky(){
  const sky=ctx.createLinearGradient(0,0,0,HOR+10);
  sky.addColorStop(0,"#1c3556");sky.addColorStop(.55,"#b9714f");sky.addColorStop(1,"#f5cf93");
  ctx.fillStyle=sky;ctx.fillRect(0,0,W,HOR+10);
  const sx=W*.72,sy=HOR-H*.1;
  const sun=ctx.createRadialGradient(sx,sy,2,sx,sy,H*.28);
  sun.addColorStop(0,"#fffbe0");sun.addColorStop(.15,"#ffe9a8cc");sun.addColorStop(1,"#ffcf8000");
  ctx.fillStyle=sun;ctx.fillRect(0,0,W,HOR+10);
  // montagnes (parallaxe)
  const layers=[["#3d4f66",.045,.17,.0035,.12],["#2e3f4f",.09,.11,.006,.09]];
  for(const[c,par,amp,fr,off]of layers){
    ctx.fillStyle=c;ctx.beginPath();ctx.moveTo(0,HOR+2);
    for(let x=0;x<=W+20;x+=16){
      const wx=x+cam.x*par*10;
      const h=H*amp*(.55+.45*Math.sin(wx*fr*3+1)*Math.sin(wx*fr*1.3+off*40))+H*off*.15;
      ctx.lineTo(x,HOR-h);
    }
    ctx.lineTo(W,HOR+2);ctx.closePath();ctx.fill();
  }
}
function drawGround(){
  const g=ctx.createLinearGradient(0,HOR,0,H);
  g.addColorStop(0,"#8b8f5c");g.addColorStop(.25,"#5d6c42");g.addColorStop(1,"#1f3322");
  ctx.fillStyle=g;ctx.fillRect(0,HOR,W,H-HOR);
  // bandes alternées pour sentir la vitesse
  const step=70,z0=Math.floor((P.z-CAM_D+25)/step)*step;
  ctx.fillStyle="rgba(255,255,255,.04)";
  for(let z=z0,k=0;z<P.z+1600;z+=step,k++){
    if(((z/step)&1)===0)continue;
    const ya=gy(z),yb=gy(z+step);
    if(ya<HOR||yb>H)continue;
    ctx.fillRect(0,yb,W,Math.min(H,ya)-yb);
  }
  // lignes de fuite
  ctx.strokeStyle="rgba(220,235,190,.09)";ctx.lineWidth=1;
  const xs=Math.floor((cam.x-1400)/100)*100;
  for(let x=xs;x<cam.x+1400;x+=100){
    const a=proj(x,0,P.z-CAM_D+22),b=proj(x,0,P.z+1600);
    if(!a||!b)continue;
    ctx.beginPath();ctx.moveTo(a.x,a.y);ctx.lineTo(b.x,b.y);ctx.stroke();
  }
  // brume à l'horizon
  const hz=ctx.createLinearGradient(0,HOR-6,0,HOR+H*.12);
  hz.addColorStop(0,"#f5cf9300");hz.addColorStop(.25,"#f0c78b99");hz.addColorStop(1,"#f0c78b00");
  ctx.fillStyle=hz;ctx.fillRect(0,HOR-6,W,H*.12+6);
}

/* Décor */
function drawTree(x,z,seed){
  const p=proj(x,0,z);if(!p)return;
  ctx.save();ctx.globalAlpha=fog(p.d);ctx.translate(p.x,p.y);ctx.scale(p.s,p.s);
  ctx.fillStyle="#00000040";ctx.beginPath();ctx.ellipse(0,1,18,5,0,0,7);ctx.fill();
  ctx.fillStyle="#4a3425";ctx.fillRect(-3,-26,6,26);
  const h=44+seed*22;
  ctx.fillStyle="#2b5a35";ctx.beginPath();ctx.moveTo(-22,-20);ctx.lineTo(0,-20-h*.6);ctx.lineTo(22,-20);ctx.fill();
  ctx.fillStyle="#357043";ctx.beginPath();ctx.moveTo(-17,-38);ctx.lineTo(0,-38-h*.55);ctx.lineTo(17,-38);ctx.fill();
  ctx.fillStyle="#3f8450";ctx.beginPath();ctx.moveTo(-12,-54);ctx.lineTo(0,-54-h*.4);ctx.lineTo(12,-54);ctx.fill();
  ctx.restore();
}
function drawBuilding(x,z,seed){
  const p=proj(x,0,z);if(!p)return;
  const w=60+seed*50,h=45+seed*70;
  ctx.save();ctx.globalAlpha=fog(p.d);ctx.translate(p.x,p.y);ctx.scale(p.s,p.s);
  ctx.fillStyle="#00000040";ctx.beginPath();ctx.ellipse(8,1,w*.62,7,0,0,7);ctx.fill();
  const c1=seed>.5?"#7b7466":"#8f8672",c2=seed>.5?"#5f5a4f":"#6f6756";
  ctx.fillStyle=c1;ctx.fillRect(-w/2,-h,w,h);
  ctx.fillStyle=c2;ctx.fillRect(w/2,-h+6,12,h-6);
  ctx.fillStyle="#4a4a42";ctx.fillRect(-w/2-4,-h-6,w+8,7);
  const cols=Math.floor(w/20),rows=Math.floor(h/24);
  for(let i=0;i<cols;i++)for(let j=0;j<rows;j++){
    const lit=hash(i+((x*7)|0),j+((z*3)|0))>.7;
    ctx.fillStyle=lit?"#ffd98a":"#2a343b";
    ctx.fillRect(-w/2+7+i*20,-h+10+j*24,9,12);
  }
  ctx.fillStyle="#3a3027";ctx.fillRect(-7,-20,14,20);
  ctx.restore();
}
function drawCrate(x,z){
  const p=proj(x,0,z);if(!p)return;
  ctx.save();ctx.globalAlpha=fog(p.d);ctx.translate(p.x,p.y);ctx.scale(p.s,p.s);
  ctx.fillStyle="#00000040";ctx.beginPath();ctx.ellipse(0,1,20,5,0,0,7);ctx.fill();
  ctx.fillStyle="#7a5a34";ctx.fillRect(-14,-24,28,24);
  ctx.strokeStyle="#4d3820";ctx.lineWidth=2;ctx.strokeRect(-14,-24,28,24);
  ctx.beginPath();ctx.moveTo(-14,-24);ctx.lineTo(14,0);ctx.moveTo(14,-24);ctx.lineTo(-14,0);ctx.stroke();
  ctx.restore();
}

/* Personnages */
function drawHuman(o){
  const g=proj(o.x,0,o.z);if(!g)return;
  const p=proj(<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover">
<title>WARZONE - Survivant</title>
<style>
*{box-sizing:border-box;-webkit-tap-highlight-color:transparent;-webkit-user-select:none;user-select:none;-webkit-touch-callout:none}
html,body{margin:0;height:100%;background:#0a1118;color:#fff;font-family:system-ui,-apple-system,"Segoe UI",Roboto,Arial,sans-serif;overflow:hidden;touch-action:none}
canvas{display:block;width:100vw;height:100dvh}
#hud{position:fixed;inset:0;pointer-events:none}
.panel{position:absolute;background:linear-gradient(180deg,#0b1a26e6,#07111bcc);border:1px solid #3f6a85aa;border-radius:12px;padding:10px 12px;font-size:12px;letter-spacing:.4px;box-shadow:0 4px 18px #0008;backdrop-filter:blur(4px)}
#stats{top:max(10px,env(safe-area-inset-top));left:10px;min-width:150px}
#mission{top:max(10px,env(safe-area-inset-top));right:10px;text-align:right;min-width:120px}
.title{font-weight:800;font-size:11px;letter-spacing:2px;color:#7fd1ff;margin-bottom:6px}
.bar{height:9px;background:#2a3a46;border-radius:6px;overflow:hidden;margin:4px 0 7px;border:1px solid #0008}
#hpbar{height:100%;width:100%;background:linear-gradient(90deg,#1fd37a,#7dffb0);transition:width .15s,background .3s}
#ammobar{height:100%;width:100%;background:linear-gradient(90deg,#ffb02e,#ffe08a)}
.row{display:flex;justify-content:space-between;gap:10px;font-size:12px;margin-top:2px}
.row b{font-variant-numeric:tabular-nums}
#msg{position:absolute;top:24%;left:0;right:0;text-align:center;font-size:clamp(22px,6vw,40px);font-weight:900;letter-spacing:3px;text-shadow:0 3px 14px #000,0 0 30px #ff8a3d88;opacity:0;transition:opacity .35s}
#stick{position:fixed;left:max(18px,env(safe-area-inset-left));bottom:max(24px,env(safe-area-inset-bottom));width:132px;height:132px;border-radius:50%;background:radial-gradient(circle,#ffffff10,#ffffff06);border:2px solid #ffffff44;pointer-events:auto;touch-action:none}
#knob{position:absolute;left:50%;top:50%;width:56px;height:56px;margin:-28px 0 0 -28px;border-radius:50%;background:radial-gradient(circle at 35% 30%,#d8f1ff,#6aa9c9);box-shadow:0 4px 12px #0007;transition:transform .05s}
.btn{position:fixed;border-radius:50%;border:2px solid #ffffff66;color:#fff;font-weight:800;letter-spacing:1px;pointer-events:auto;touch-action:none;display:flex;align-items:center;justify-content:center;box-shadow:0 5px 16px #0008,inset 0 2px 6px #fff3;font-size:11px;text-shadow:0 1px 3px #000}
.btn:active,.btn.on{transform:scale(.92);filter:brightness(1.25)}
#bFire{right:max(20px,env(safe-area-inset-right));bottom:max(30px,env(safe-area-inset-bottom));width:92px;height:92px;background:radial-gradient(circle at 35% 30%,#ff7a6b,#b52a20);font-size:14px}
#bJump{right:max(122px,calc(env(safe-area-inset-right) + 102px));bottom:max(30px,env(safe-area-inset-bottom));width:64px;height:64px;background:radial-gradient(circle at 35% 30%,#7fc8ff,#2a6fa8)}
#bGren{right:max(30px,env(safe-area-inset-right));bottom:calc(max(30px,env(safe-area-inset-bottom)) + 104px);width:64px;height:64px;background:radial-gradient(circle at 35% 30%,#ffc766,#a8681a)}
#bReload{right:max(104px,calc(env(safe-area-inset-right) + 84px));bottom:calc(max(30px,env(safe-area-inset-bottom)) + 80px);width:52px;height:52px;background:radial-gradient(circle at 35% 30%,#8fe8c0,#1f8a63)}
#overlay{position:fixed;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:14px;text-align:center;padding:24px;background:radial-gradient(circle at 50% 40%,#0b1a26dd,#040a10f2);z-index:5}
#overlay h1{margin:0;font-size:clamp(30px,9vw,54px);letter-spacing:6px;font-weight:900;background:linear-gradient(180deg,#fff,#7fd1ff);-webkit-background-clip:text;background-clip:text;color:transparent;filter:drop-shadow(0 4px 14px #000)}
#overlay p{margin:0;color:#b8d2e2;font-size:14px;line-height:1.55;max-width:420px}
#go{padding:15px 38px;font-size:17px;font-weight:800;letter-spacing:2px;border:0;border-radius:14px;color:#04201c;background:linear-gradient(180deg,#34f0cf,#13a58d);box-shadow:0 8px 24px #13bca255,inset 0 2px 0 #fff6;touch-action:manipulation}
#go:active{transform:translateY(2px)}
.hide{display:none!important}
</style>
</head>
<body>
<canvas id="game"></canvas>

<div id="hud">
  <div class="panel" id="stats">
    <div class="title">WARZONE · SURVIVANT</div>
    <div class="row"><span>VIE</span><b id="hpTxt">100</b></div>
    <div class="bar"><div id="hpbar"></div></div>
    <div class="row"><span id="ammoLbl">MUNITIONS</span><b id="ammoTxt">30/30</b></div>
    <div class="bar"><div id="ammobar"></div></div>
    <div class="row"><span>GRENADES</span><b id="grenTxt">3</b></div>
  </div>
  <div class="panel" id="mission">
    <div class="title">MISSION</div>
    <div class="row"><span>Vague</span><b id="waveTxt">1</b></div>
    <div class="row"><span>Ennemis</span><b id="countTxt">0</b></div>
    <div class="row"><span>Éliminations</span><b id="killTxt">0</b></div>
    <div class="row"><span>Score</span><b id="scoreTxt">0</b></div>
  </div>
  <div id="msg"></div>
</div>

<div id="stick"><div id="knob"></div></div>
<button class="btn" id="bFire">TIRER</button>
<button class="btn" id="bJump">SAUT</button>
<button class="btn" id="bGren">BOMBE</button>
<button class="btn" id="bReload">RECH.</button>

<div id="overlay">
  <h1 id="ovTitle">WARZONE</h1>
  <p id="ovText">Survis aux vagues d'ennemis. Déplace-toi avec le joystick, tire (visée automatique), lance des grenades et esquive en sautant.<br><br><small>Clavier : WASD/Flèches · F tirer · G grenade · R recharger · Espace sauter</small></p>
  <button id="go">COMMENCER</button>
</div>

<script>
"use strict";
const $=id=>document.getElementById(id);
const cv=$("game"),ctx=cv.getContext("2d");
let W=0,H=0,HOR=0,F=0;
function resize(){
  const d=Math.min(devicePixelRatio||1,2);
  W=innerWidth;H=innerHeight;
  cv.width=W*d;cv.height=H*d;
  ctx.setTransform(d,0,0,d,0,0);
  HOR=H*.36;F=H*.55;
}
addEventListener("resize",resize);resize();

/* ---------- Constantes & utilitaires ---------- */
const CAM_D=200,CAM_H=90,PLAYER_SPEED=120;
const TYPES={
  grunt :{hp:60 ,sp:34,sc:1  ,dmg:8 ,pts:10,body:"#8a4a3a",vest:"#a5604a",helm:"#5a342b"},
  runner:{hp:35 ,sp:64,sc:.9 ,dmg:6 ,pts:15,body:"#a6803a",vest:"#c49a48",helm:"#6b5426"},
  heavy :{hp:170,sp:20,sc:1.3,dmg:14,pts:35,body:"#4a4f5c",vest:"#636a7a",helm:"#2b2f38"}
};
const rand=(a,b)=>a+Math.random()*(b-a);
const clamp=(v,a,b)=>Math.max(a,Math.min(b,v));
function hash(a,b){let h=Math.imul(a,374761393)^Math.imul(b,668265263);h=Math.imul(h^(h>>>13),1274126177);return((h^(h>>>16))>>>0)/4294967296}
function rr(x,y,w,h,r){ctx.beginPath();if(ctx.roundRect)ctx.roundRect(x,y,w,h,r);else ctx.rect(x,y,w,h)}

/* ---------- Audio ---------- */
let AC=null,noiseBuf=null;
function initAudio(){
  if(AC)return;
  try{
    AC=new(window.AudioContext||window.webkitAudioContext)();
    noiseBuf=AC.createBuffer(1,AC.sampleRate,AC.sampleRate);
    const d=noiseBuf.getChannelData(0);
    for(let i=0;i<d.length;i++)d[i]=Math.random()*2-1;
  }catch(e){AC=null}
}
function noise(dur,freq,type,vol){
  if(!AC)return;
  const s=AC.createBufferSource();s.buffer=noiseBuf;
  const f=AC.createBiquadFilter();f.type=type;f.frequency.value=freq;
  const g=AC.createGain();
  g.gain.setValueAtTime(vol,AC.currentTime);
  g.gain.exponentialRampToValueAtTime(.001,AC.currentTime+dur);
  s.connect(f);f.connect(g);g.connect(AC.destination);
  s.start();s.stop(AC.currentTime+dur);
}
function tone(freq,dur,vol,type){
  if(!AC)return;
  const o=AC.createOscillator(),g=AC.createGain();
  o.type=type||"sine";o.frequency.value=freq;
  g.gain.setValueAtTime(vol,AC.currentTime);
  g.gain.exponentialRampToValueAtTime(.001,AC.currentTime+dur);
  o.connect(g);g.connect(AC.destination);
  o.start();o.stop(AC.currentTime+dur);
}
function sfx(n){
  if(n==="shot")noise(.09,1800,"bandpass",.22);
  else if(n==="boom")noise(.7,420,"lowpass",.7);
  else if(n==="hit")tone(240,.06,.1,"square");
  else if(n==="hurt")tone(90,.16,.25,"sawtooth");
  else if(n==="reload")tone(520,.06,.1,"triangle");
  else if(n==="pick")tone(780,.12,.12,"sine");
}

/* ---------- État du jeu ---------- */
let P,enemies,fx,texts,tracers,nades,booms,packs;
let G={mode:"menu",wave:0,queue:0,spawnT:0,between:0,kills:0,score:0,best:0,shake:0,hurt:0,time:0,target:null};
const cam={x:0};
try{G.best=+localStorage.getItem("warzone_best")||0}catch(e){}
const inp={x:0,y:0,fire:false,jump:false};
const kb={};

function reset(){
  P={x:0,z:0,y:0,vy:0,hp:100,max:100,ammo:30,mag:30,cd:0,reload:0,gren:3,gt:0,muzzle:0,phase:0,moving:false};
  enemies=[];fx=[];texts=[];tracers=[];nades=[];booms=[];packs=[];
  G.wave=0;G.queue=0;G.spawnT=0;G.between=0;G.kills=0;G.score=0;G.shake=0;G.hurt=0;G.target=null;
  cam.x=0;
}
reset();

let msgTimer=null;
function showMsg(t,ms){
  const m=$("msg");m.textContent=t;m.style.opacity=1;
  clearTimeout(msgTimer);msgTimer=setTimeout(()=>m.style.opacity=0,ms||1800);
}
function startWave(){
  G.wave++;
  G.queue=5+G.wave*3;
  G.spawnT=1;
  showMsg("VAGUE "+G.wave,2000);
}
function spawnEnemy(){
  let r=Math.random(),type="grunt";
  if(G.wave>=3&&r<Math.min(.05*G.wave,.25))type="heavy";
  else if(G.wave>=2&&r<.45)type="runner";
  const T=TYPES[type];
  let x,z;
  if(Math.random()<.8){x=P.x+rand(-380,380);z=P.z+rand(380,620)}
  else{const s=Math.random()<.5?-1:1;x=P.x+s*rand(340,440);z=P.z+rand(0,200)}
  const hp=T.hp*(1+G.wave*.05);
  enemies.push({x,z,type,T,sc:T.sc,hp,max:hp,sp:T.sp*rand(.9,1.15),phase:Math.random()*6,flash:0,cd:0});
}
function damagePlayer(n){
  P.hp-=n;G.hurt=.4;G.shake=Math.max(G.shake,7);sfx("hurt");
  if(P.hp<=0){P.hp=0;endGame()}
}
function endGame(){
  G.mode="over";
  if(G.score>G.best){G.best=G.score;try{localStorage.setItem("warzone_best",G.best)}catch(e){}}
  $("ovTitle").textContent="MISSION ÉCHOUÉE";
  $("ovText").innerHTML="Vague atteinte : <b>"+G.wave+"</b><br>Éliminations : <b>"+G.kills+"</b><br>Score : <b>"+G.score+"</b><br>Meilleur score : <b>"+G.best+"</b>";
  $("go").textContent="REJOUER";
  $("overlay").classList.remove("hide");
}

/* ---------- Effets ---------- */
function burst(x,y,z,n,cols,spd,grav,size){
  for(let i=0;i<n;i++){
    const a=Math.random()*Math.PI*2,e=Math.random()*Math.PI-.3;
    const s=rand(.3,1)*spd;
    fx.push({x,y,z,vx:Math.cos(a)*Math.cos(e)*s,vy:Math.abs(Math.sin(e))*s+rand(10,50),vz:Math.sin(a)*Math.cos(e)*s,
      life:rand(.3,.9),max:.9,size:rand(.6,1)*size,col:cols[(Math.random()*cols.length)|0],g:grav});
  }
}
function explode(x,z,big){
  booms.push({x,z,t:0,max:big?.7:.3,r:big?95:30});
  burst(x,10,z,big?46:12,["#ffb02e","#ff6a2c","#ffe08a","#555"],big?200:90,300,big?5:2.5);
  G.shake=Math.max(G.shake,big?14:4);
  sfx(big?"boom":"hit");
}
function floatText(x,y,z,txt,col){texts.push({x,y,z,txt,col:col||"#fff",t:0})}

function killEnemy(e){
  enemies.splice(enemies.indexOf(e),1);
  G.kills++;G.score+=e.T.pts;
  burst(e.x,24,e.z,14,["#a31818","#6b0f0f","#d23b3b"],120,350,2.5);
  floatText(e.x,50*e.sc,e.z,"+"+e.T.pts,"#ffe08a");
  if(Math.random()<.14)packs.push({x:e.x,z:e.z,t:0});
}
function hitEnemy(e,dmg){
  e.hp-=dmg;e.flash=.08;e.z+=2;
  burst(e.x,28*e.sc,e.z,5,["#ffd27a","#ff9a3d"],80,300,1.8);
  sfx("hit");
  if(e.hp<=0)killEnemy(e);
}

/* ---------- Actions du joueur ---------- */
function startReload(){
  if(P.reload>0||P.ammo===P.mag)return;
  P.reload=1.1;sfx("reload");
}
function shoot(){
  if(P.cd>0||P.reload>0)return;
  if(P.ammo<=0){startReload();return}
  P.cd=.13;P.ammo--;P.muzzle=.06;
  G.shake=Math.max(G.shake,1.6);sfx("shot");
  const t=G.target;
  const x1=P.x+6,y1=P.y+38,z1=P.z+8;
  if(t){
    tracers.push({x1,y1,z1,x2:t.x,y2:26*t.sc,z2:t.z,t:0});
    hitEnemy(t,30);
  }else{
    tracers.push({x1,y1,z1,x2:P.x+rand(-25,25),y2:34,z2:P.z+520,t:0});
  }
  if(P.ammo===0)startReload();
}
function throwGrenade(){
  if(G.mode!=="play"||P.gren<=0)return;
  P.gren--;
  const t=G.target;
  nades.push({x:P.x,z:P.z,tx:t?t.x:P.x,tz:t?Math.min(t.z,P.z+260):P.z+190,t:0,dur:.65,sx:P.x,sz:P.z});
}
function grenadeBlast(x,z){
  explode(x,z,true);
  for(const e of enemies.slice()){
    const d=Math.hypot(e.x-x,e.z-z);
    if(d<125){
      const k=1-d/150;
      e.hp-=170*k;e.flash=.12;
      if(e.hp<=0)killEnemy(e);
    }
  }
}

/* ---------- Mise à jour ---------- */
function findTarget(){
  let best=null,bs=1e9;
  for(const e of enemies){
    const dz=e.z-P.z,dx=e.x-P.x;
    if(dz<-60||dz>650)continue;
    const s=Math.hypot(dx,dz)+Math.abs(dx)*1.3;
    if(s<bs){bs=s;best=e}
  }
  G.target=best;
}
function update(dt){
  G.time+=dt;
  cam.x+=(P.x-cam.x)*Math.min(1,dt*8);
  G.shake=Math.max(0,G.shake-dt*28);
  G.hurt=Math.max(0,G.hurt-dt*1.6);
  if(G.mode!=="play")return;

  // Déplacement
  let mx=inp.x,my=inp.y;
  if(kb.left)mx-=1;if(kb.right)mx+=1;if(kb.up)my+=1;if(kb.down)my-=1;
  const m=Math.hypot(mx,my);if(m>1){mx/=m;my/=m}
  P.moving=m>.1;
  P.x+=mx*PLAYER_SPEED*dt;P.z+=my*PLAYER_SPEED*dt;
  if(P.moving)P.phase+=dt*(P.y>0?0:1);
  if((inp.jump||kb.jump)&&P.y===0)P.vy=210;
  P.vy-=620*dt;P.y+=P.vy*dt;
  if(P.y<=0){P.y=0;P.vy=0}

  // Armes
  P.cd-=dt;P.muzzle-=dt;
  if(P.reload>0){P.reload-=dt;if(P.reload<=0){P.reload=0;P.ammo=P.mag}}
  P.gt+=dt;if(P.gren<3&&P.gt>12){P.gren++;P.gt=0}
  findTarget();
  if(inp.fire||kb.fire)shoot();

  // Vagues
  if(G.queue>0){
    G.spawnT-=dt;
    if(G.spawnT<=0&&enemies.length<12+Math.min(G.wave,8)){spawnEnemy();G.queue--;G.spawnT=Math.max(.25,.7-G.wave*.03)}
  }else if(enemies.length===0){
    if(G.between<=0){G.between=3;showMsg("VAGUE "+G.wave+" TERMINÉE",2200);P.hp=Math.min(P.max,P.hp+20);if(P.gren<3)P.gren++}
    G.between-=dt;
    if(G.between<=0)startWave();
  }

  // Ennemis
  for(const e of enemies){
    let dx=P.x-e.x,dz=P.z-e.z,d=Math.hypot(dx,dz)||1;
    let vx=dx/d*e.sp,vz=dz/d*e.sp;
    for(const o of enemies){
      if(o===e)continue;
      const ox=e.x-o.x,oz=e.z-o.z,od=Math.hypot(ox,oz);
      if(od>0&&od<24){vx+=ox/od*30;vz+=oz/od*30}
    }
    e.x+=vx*dt;e.z+=vz*dt;
    e.phase+=dt*e.sp/8;e.flash-=dt;e.cd-=dt;
    if(d<26*e.sc&&P.y<16&&e.cd<=0){e.cd=.9;damagePlayer(e.T.dmg)}
  }

  // Grenades
  for(const n of nades.slice()){
    n.t+=dt;
    const k=Math.min(1,n.t/n.dur);
    n.x=n.sx+(n.tx-n.sx)*k;n.z=n.sz+(n.tz-n.sz)*k;n.y=Math.sin(Math.PI*k)*70;
    if(k>=1){nades.splice(nades.indexOf(n),1);grenadeBlast(n.x,n.z)}
  }

  // Trousses de soin
  for(const p of packs.slice()){
    p.t+=dt;
    if(Math.hypot(p.x-P.x,p.z-P.z)<30){
      packs.splice(packs.indexOf(p),1);
      P.hp=Math.min(P.max,P.hp+25);sfx("pick");floatText(P.x,60,P.z,"+25 VIE","#7dffb0");
    }else if(p.t>20)packs.splice(packs.indexOf(p),1);
  }

  // Effets
  for(const f of fx){f.x+=f.vx*dt;f.y+=f.vy*dt;f.z+=f.vz*dt;f.vy-=f.g*dt;f.life-=dt;if(f.y<0){f.y=0;f.vy*=-.3;f.vx*=.6;f.vz*=.6}}
  fx=fx.filter(f=>f.life>0);
  for(const t of tracers)t.t+=dt;tracers=tracers.filter(t=>t.t<.08);
  for(const b of booms)b.t+=dt;booms=booms.filter(b=>b.t<b.max);
  for(const t of texts)t.t+=dt;texts=texts.filter(t=>t.t<.9);

  // HUD
  $("hpbar").style.width=(P.hp/P.max*100)+"%";
  $("hpbar").style.background=P.hp<30?"linear-gradient(90deg,#e23b3b,#ff8a7a)":"";
  $("hpTxt").textContent=Math.ceil(P.hp);
  $("ammoLbl").textContent=P.reload>0?"RECHARGEMENT…":"MUNITIONS";
  $("ammoTxt").textContent=P.ammo+"/"+P.mag;
  $("ammobar").style.width=(P.reload>0?(1-P.reload/1.1)*100:P.ammo/P.mag*100)+"%";
  $("grenTxt").textContent=P.gren;
  $("waveTxt").textContent=G.wave;
  $("countTxt").textContent=enemies.length+G.queue;
  $("killTxt").textContent=G.kills;
  $("scoreTxt").textContent=G.score;
}

/* ---------- Rendu ---------- */
function proj(x,y,z){
  const dz=z-P.z+CAM_D;
  if(dz<18)return null;
  const s=F/dz;
  return{x:W/2+(x-cam.x)*s,y:HOR+(CAM_H-y)*s,s,d:dz};
}
const gy=z=>HOR+CAM_H*F/(z-P.z+CAM_D);
const fog=d=>clamp(1-(d-650)/900,.15,1);

function drawSky(){
  const sky=ctx.createLinearGradient(0,0,0,HOR+10);
  sky.addColorStop(0,"#1c3556");sky.addColorStop(.55,"#b9714f");sky.addColorStop(1,"#f5cf93");
  ctx.fillStyle=sky;ctx.fillRect(0,0,W,HOR+10);
  const sx=W*.72,sy=HOR-H*.1;
  const sun=ctx.createRadialGradient(sx,sy,2,sx,sy,H*.28);
  sun.addColorStop(0,"#fffbe0");sun.addColorStop(.15,"#ffe9a8cc");sun.addColorStop(1,"#ffcf8000");
  ctx.fillStyle=sun;ctx.fillRect(0,0,W,HOR+10);
  // montagnes (parallaxe)
  const layers=[["#3d4f66",.045,.17,.0035,.12],["#2e3f4f",.09,.11,.006,.09]];
  for(const[c,par,amp,fr,off]of layers){
    ctx.fillStyle=c;ctx.beginPath();ctx.moveTo(0,HOR+2);
    for(let x=0;x<=W+20;x+=16){
      const wx=x+cam.x*par*10;
      const h=H*amp*(.55+.45*Math.sin(wx*fr*3+1)*Math.sin(wx*fr*1.3+off*40))+H*off*.15;
      ctx.lineTo(x,HOR-h);
    }
    ctx.lineTo(W,HOR+2);ctx.closePath();ctx.fill();
  }
}
function drawGround(){
  const g=ctx.createLinearGradient(0,HOR,0,H);
  g.addColorStop(0,"#8b8f5c");g.addColorStop(.25,"#5d6c42");g.addColorStop(1,"#1f3322");
  ctx.fillStyle=g;ctx.fillRect(0,HOR,W,H-HOR);
  // bandes alternées pour sentir la vitesse
  const step=70,z0=Math.floor((P.z-CAM_D+25)/step)*step;
  ctx.fillStyle="rgba(255,255,255,.04)";
  for(let z=z0,k=0;z<P.z+1600;z+=step,k++){
    if(((z/step)&1)===0)continue;
    const ya=gy(z),yb=gy(z+step);
    if(ya<HOR||yb>H)continue;
    ctx.fillRect(0,yb,W,Math.min(H,ya)-yb);
  }
  // lignes de fuite
  ctx.strokeStyle="rgba(220,235,190,.09)";ctx.lineWidth=1;
  const xs=Math.floor((cam.x-1400)/100)*100;
  for(let x=xs;x<cam.x+1400;x+=100){
    const a=proj(x,0,P.z-CAM_D+22),b=proj(x,0,P.z+1600);
    if(!a||!b)continue;
    ctx.beginPath();ctx.moveTo(a.x,a.y);ctx.lineTo(b.x,b.y);ctx.stroke();
  }
  // brume à l'horizon
  const hz=ctx.createLinearGradient(0,HOR-6,0,HOR+H*.12);
  hz.addColorStop(0,"#f5cf9300");hz.addColorStop(.25,"#f0c78b99");hz.addColorStop(1,"#f0c78b00");
  ctx.fillStyle=hz;ctx.fillRect(0,HOR-6,W,H*.12+6);
}

/* Décor */
function drawTree(x,z,seed){
  const p=proj(x,0,z);if(!p)return;
  ctx.save();ctx.globalAlpha=fog(p.d);ctx.translate(p.x,p.y);ctx.scale(p.s,p.s);
  ctx.fillStyle="#00000040";ctx.beginPath();ctx.ellipse(0,1,18,5,0,0,7);ctx.fill();
  ctx.fillStyle="#4a3425";ctx.fillRect(-3,-26,6,26);
  const h=44+seed*22;
  ctx.fillStyle="#2b5a35";ctx.beginPath();ctx.moveTo(-22,-20);ctx.lineTo(0,-20-h*.6);ctx.lineTo(22,-20);ctx.fill();
  ctx.fillStyle="#357043";ctx.beginPath();ctx.moveTo(-17,-38);ctx.lineTo(0,-38-h*.55);ctx.lineTo(17,-38);ctx.fill();
  ctx.fillStyle="#3f8450";ctx.beginPath();ctx.moveTo(-12,-54);ctx.lineTo(0,-54-h*.4);ctx.lineTo(12,-54);ctx.fill();
  ctx.restore();
}
function drawBuilding(x,z,seed){
  const p=proj(x,0,z);if(!p)return;
  const w=60+seed*50,h=45+seed*70;
  ctx.save();ctx.globalAlpha=fog(p.d);ctx.translate(p.x,p.y);ctx.scale(p.s,p.s);
  ctx.fillStyle="#00000040";ctx.beginPath();ctx.ellipse(8,1,w*.62,7,0,0,7);ctx.fill();
  const c1=seed>.5?"#7b7466":"#8f8672",c2=seed>.5?"#5f5a4f":"#6f6756";
  ctx.fillStyle=c1;ctx.fillRect(-w/2,-h,w,h);
  ctx.fillStyle=c2;ctx.fillRect(w/2,-h+6,12,h-6);
  ctx.fillStyle="#4a4a42";ctx.fillRect(-w/2-4,-h-6,w+8,7);
  const cols=Math.floor(w/20),rows=Math.floor(h/24);
  for(let i=0;i<cols;i++)for(let j=0;j<rows;j++){
    const lit=hash(i+((x*7)|0),j+((z*3)|0))>.7;
    ctx.fillStyle=lit?"#ffd98a":"#2a343b";
    ctx.fillRect(-w/2+7+i*20,-h+10+j*24,9,12);
  }
  ctx.fillStyle="#3a3027";ctx.fillRect(-7,-20,14,20);
  ctx.restore();
}
function drawCrate(x,z){
  const p=proj(x,0,z);if(!p)return;
  ctx.save();ctx.globalAlpha=fog(p.d);ctx.translate(p.x,p.y);ctx.scale(p.s,p.s);
  ctx.fillStyle="#00000040";ctx.beginPath();ctx.ellipse(0,1,20,5,0,0,7);ctx.fill();
  ctx.fillStyle="#7a5a34";ctx.fillRect(-14,-24,28,24);
  ctx.strokeStyle="#4d3820";ctx.lineWidth=2;ctx.strokeRect(-14,-24,28,24);
  ctx.beginPath();ctx.moveTo(-14,-24);ctx.lineTo(14,0);ctx.moveTo(14,-24);ctx.lineTo(-14,0);ctx.stroke();
  ctx.restore();
}

/* Personnages */
function drawHuman(o){
  const g=proj(o.x,0,o.z);if(!g)return;
  const p=proj(
