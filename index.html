<!doctype html>
<html lang="pt-BR">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover"/>
<title>3-Corpos — Mobile (Verlet + Pinch Zoom)</title>
<style>
  :root { --bg:#000; --panel:#0f1720; --accent:#0a84ff; --ui:#ddd; }
  html,body{height:100%;margin:0;background:var(--bg);color:var(--ui);-webkit-tap-highlight-color:transparent; touch-action: none;}
  #canvas{display:block;width:100vw;height:100vh;}
  #uiBtn{
    position:fixed; left:12px; top:12px; z-index:20;
    background:rgba(255,255,255,0.06); border:0; color:var(--ui); padding:8px 10px; border-radius:8px; font-weight:600;
  }
  #panel{
    position:fixed; right:-320px; top:0; height:100vh; width:300px; background:var(--panel); padding:16px; box-sizing:border-box;
    transition:right .28s ease; z-index:21; color:var(--ui); font-family:system-ui,Arial;
  }
  #panel.open{ right:0; }
  label{display:block;margin-top:10px;font-size:13px;color:#cbd5e1;}
  input[type="number"], input[type="range"]{ width:100%; margin-top:6px; padding:8px; border-radius:6px; border:0; background:#0b1220; color:var(--ui); }
  button{ margin-top:12px; width:100%; padding:10px; border-radius:8px; border:0; background:var(--accent); color:white; font-weight:700; }
  .small{ font-size:12px; color:#94a3b8; margin-top:6px; }
  .footer { position:fixed; right:12px; bottom:12px; z-index:20; color:#9ca3af; background:rgba(255,255,255,0.03); padding:6px 8px; border-radius:6px; font-size:12px; }
  #fps { position:fixed; left:12px; bottom:50px; color:#9ca3af; font-size:12px; z-index:20; background:rgba(0,0,0,0.3); padding:4px 6px; border-radius:4px; }
</style>
</head>
<body>
<canvas id="canvas"></canvas>

<button id="uiBtn" aria-label="Abrir painel">⚙ Configurar</button>

<aside id="panel" aria-hidden="true">
  <h3 style="margin:0 0 6px 0;">Criar corpo</h3>

  <label for="massa">Massa</label>
  <input id="massa" type="number" value="20" step="1" min="0.1">

  <label for="posX">Pos X (mundo)</label>
  <input id="posX" type="number" value="-150" step="1">

  <label for="posY">Pos Y (mundo)</label>
  <input id="posY" type="number" value="0" step="1">

  <label for="velX">Vel X</label>
  <input id="velX" type="number" value="0" step="0.1">

  <label for="velY">Vel Y</label>
  <input id="velY" type="number" value="1.2" step="0.1">

  <button id="addBtn">Adicionar corpo</button>

  <div class="small">Toque e arraste na tela para criar um corpo: início = posição, arraste = velocidade.</div>

  <hr style="margin:12px 0; border:none; border-top:1px solid rgba(255,255,255,0.04)">

  <label for="gRange">Constante G</label>
  <input id="gRange" type="range" min="0.01" max="10" step="0.01" value="0.2">
  <div id="gVal" class="small">G = 0.20</div>

  <label for="dtRange">Time-step (dt)</label>
  <input id="dtRange" type="range" min="0.1" max="15" step="0.1" value="0.9">
  <div id="dtVal" class="small">dt = 0.90</div>

  <button id="resetBtn" style="background:#ef4444;">Resetar cena</button>
</aside>

<div class="footer">Rastro ≤ 450 pontos • Pinch-to-zoom & pan (mobile)</div>
<div id="fps">FPS: 0</div>

<script>
/* === Config / Canvas / Camera (mobile-focused) === */
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d', { alpha: false });
let DPR = Math.max(1, window.devicePixelRatio || 1);

function fitCanvas(){
  DPR = Math.max(1, window.devicePixelRatio || 1);
  canvas.style.width = innerWidth + 'px';
  canvas.style.height = innerHeight + 'px';
  canvas.width = Math.floor(innerWidth * DPR);
  canvas.height = Math.floor(innerHeight * DPR);
  ctx.setTransform(DPR,0,0,DPR,0,0);
}
fitCanvas();
addEventListener('resize', fitCanvas);

const cam = { x: 0, y: 0, scale: 1 };
function worldToScreen(p){ const cx = canvas.clientWidth/2, cy = canvas.clientHeight/2; return { x: (p.x - cam.x)*cam.scale + cx, y: (p.y - cam.y)*cam.scale + cy }; }
function screenToWorld(sx, sy){ const cx = canvas.clientWidth/2, cy = canvas.clientHeight/2; return { x: cam.x + (sx - cx)/cam.scale, y: cam.y + (sy - cy)/cam.scale }; }

/* === Simulation state === */
let G = parseFloat(document.getElementById('gRange').value);
let dt = parseFloat(document.getElementById('dtRange').value);
const MAX_TRAIL = 450;

function makeBody(m, x, y, vx=0, vy=0){ return { m, x, y, vx, vy, rgb:[200,200,255], trail:[] }; }

let bodies = [
  makeBody(20, -150, 0, 0, 1.2),
  makeBody(20, 150, 0, 0, -1.2),
  makeBody(35, 0, 0, 0, 0)
];

function lerp(a,b,t){ return a + (b-a)*t; }

function recomputeColors(){
  if(bodies.length===0) return;
  let minM = Infinity, maxM=-Infinity;
  for(const b of bodies){ if(b.m<minM) minM=b.m; if(b.m>maxM) maxM=b.m; }
  if(minM===maxM){ minM*=0.5; maxM*=1.5; if(minM===maxM){ minM=0; maxM=maxM||1;} }
  for(const b of bodies){
    let t = (b.m - minM)/(maxM - minM);
    t=Math.max(0,Math.min(1,t));
    let r,g,bb;
    if(t<0.25){ let u=t/0.25; r=lerp(40,0,u); g=lerp(90,180,u); bb=lerp(255,200,u);}
    else if(t<0.5){ let u=(t-0.25)/0.25; r=lerp(0,180,u); g=lerp(180,220,u); bb=lerp(200,80,u);}
    else if(t<0.75){ let u=(t-0.5)/0.25; r=lerp(180,240,u); g=lerp(220,160,u); bb=lerp(80,40,u);}
    else{ let u=(t-0.75)/0.25; r=lerp(240,255,u); g=lerp(160,100,u); bb=lerp(40,20,u);}
    b.rgb=[Math.round(r),Math.round(g),Math.round(bb)];
  }
}

/* === Physics: Velocity Verlet === */
function computeAccs(arr){
  const n=arr.length, ax=new Array(n).fill(0), ay=new Array(n).fill(0), eps=1.0;
  for(let i=0;i<n;i++){
    for(let j=i+1;j<n;j++){
      const A=arr[i], B=arr[j];
      const dx=B.x-A.x, dy=B.y-A.y, dist2=dx*dx+dy*dy+eps, dist=Math.sqrt(dist2);
      const f=G*A.m*B.m/dist2;
      const fx=f*dx/dist, fy=f*dy/dist;
      ax[i]+=fx/A.m; ay[i]+=fy/A.m;
      ax[j]-=fx/B.m; ay[j]-=fy/B.m;
    }
  }
  return {ax, ay};
}

function stepVerlet(dtStep){
  if(bodies.length===0) return;
  const {ax:ax0, ay:ay0} = computeAccs(bodies);
  for(let i=0;i<bodies.length;i++){
    const b=bodies[i];
    b.x+=b.vx*dtStep + 0.5*ax0[i]*dtStep*dtStep;
    b.y+=b.vy*dtStep + 0.5*ay0[i]*dtStep*dtStep;
  }
  const {ax:ax1, ay:ay1}=computeAccs(bodies);
  for(let i=0;i<bodies.length;i++){
    const b=bodies[i];
    b.vx+=0.5*(ax0[i]+ax1[i])*dtStep;
    b.vy+=0.5*(ay0[i]+ay1[i])*dtStep;
    b.trail.push({x:b.x,y:b.y});
    if(b.trail.length>MAX_TRAIL) b.trail.shift();
  }
}

/* === Rendering === */
function draw(){
  ctx.fillStyle='#000';
  ctx.fillRect(0,0,canvas.clientWidth,canvas.clientHeight);
  recomputeColors();

  ctx.lineWidth=1;
  ctx.setLineDash([6,6]);
  for(const b of bodies){
    if(b.trail.length<2) continue;
    ctx.beginPath();
    ctx.strokeStyle=`rgba(${b.rgb[0]},${b.rgb[1]},${b.rgb[2]},0.28)`;
    for(let i=0;i<b.trail.length-1;i++){
      const p1=worldToScreen(b.trail[i]);
      const p2=worldToScreen(b.trail[i+1]);
      ctx.moveTo(p1.x,p1.y); ctx.lineTo(p2.x,p2.y);
    }
    ctx.stroke();
  }
  ctx.setLineDash([]);

  for(const b of bodies){
    const screen=worldToScreen(b);
    const radius=Math.max(4,Math.log(b.m+1)*4)*cam.scale;
    ctx.beginPath();
    ctx.arc(screen.x,screen.y,Math.max(3,radius),0,Math.PI*2);
    ctx.fillStyle=`rgb(${b.rgb[0]},${b.rgb[1]},${b.rgb[2]})`;
    ctx.fill();
    ctx.lineWidth=1;
    ctx.strokeStyle=`rgba(${b.rgb[0]},${b.rgb[1]},${b.rgb[2]},0.9)`;
    ctx.stroke();
  }
}

/* === Main loop + FPS === */
let lastTime=performance.now(), fpsCounter=0, fpsTime=0, fpsDisplay=document.getElementById('fps');
function loop(now){
  const delta=(now-lastTime)/1000;
  lastTime=now;
  stepVerlet(dt);
  draw();
  fpsTime+=delta; fpsCounter++;
  if(fpsTime>=0.5){ fpsDisplay.textContent=`FPS: ${Math.round(fpsCounter/fpsTime)}`; fpsTime=0; fpsCounter=0; }
  requestAnimationFrame(loop);
}
requestAnimationFrame(loop);

/* === UI actions === */
const uiBtn=document.getElementById('uiBtn'), panel=document.getElementById('panel');
uiBtn.addEventListener('click', ()=>{ panel.classList.toggle('open'); panel.setAttribute('aria-hidden', panel.classList.contains('open')?'false':'true'); });

document.getElementById('addBtn').addEventListener('click', ()=>{
  const m=parseFloat(document.getElementById('massa').value)||10;
  const x=parseFloat(document.getElementById('posX').value)||0;
  const y=parseFloat(document.getElementById('posY').value)||0;
  const vx=parseFloat(document.getElementById('velX').value)||0;
  const vy=parseFloat(document.getElementById('velY').value)||0;
  bodies.push(makeBody(m,x,y,vx,vy));
  recomputeColors();
});

document.getElementById('resetBtn').addEventListener('click', ()=>{
  bodies=[
    makeBody(20,-150,0,0,1.2),
    makeBody(20,150,0,0,-1.2),
    makeBody(35,0,0,0,0)
  ];
  cam.x=0; cam.y=0; cam.scale=1;
  recomputeColors();
});

document.getElementById('gRange').addEventListener('input',(e)=>{ G=parseFloat(e.target.value); document.getElementById('gVal').textContent=`G = ${G.toFixed(2)}`; });
document.getElementById('dtRange').addEventListener('input',(e)=>{ dt=parseFloat(e.target.value); document.getElementById('dtVal').textContent=`dt = ${dt.toFixed(2)}`; });

/* === Touch handling === */
let touchState={mode:'idle', startWorld:null, startScreen:null, lastPan:null, lastDist:0, lastCenter:null};
let holdTimer=null,HOLD_MS=220;

function distTouches(a,b){ return Math.hypot(a.clientX-b.clientX, a.clientY-b.clientY); }
function centerTouches(a,b){ return {x:(a.clientX+b.clientX)/2, y:(a.clientY+b.clientY)/2}; }

canvas.addEventListener('touchstart',(ev)=>{
  ev.preventDefault();
  const touches=ev.touches;
  if(touches.length===1){
    const t=touches[0];
    touchState.startScreen={x:t.clientX,y:t.clientY};
    touchState.startWorld=screenToWorld(t.clientX,t.clientY);
    touchState.lastPan={x:t.clientX,y:t.clientY};
    touchState.mode='creating';
    clearTimeout(holdTimer);
    holdTimer=setTimeout(()=>{
      touchState.mode='pan';
    },HOLD_MS);
  } else if(touches.length===2){
    touchState.mode='pinch';
    const a=touches[0], b=touches[1];
    touchState.lastDist=distTouches(a,b);
    touchState.lastCenter=centerTouches(a,b);
    touchState.pinchRefWorld=screenToWorld(touchState.lastCenter.x,touchState.lastCenter.y);
    touchState.startScale=cam.scale;
  }
},{passive:false});

canvas.addEventListener('touchmove',(ev)=>{
  ev.preventDefault();
  const touches=ev.touches;
  if(touches.length===1){
    const t=touches[0];
    if(touchState.mode==='pan'){
      const dx=t.clientX-touchState.lastPan.x, dy=t.clientY-touchState.lastPan.y;
      touchState.lastPan={x:t.clientX,y:t.clientY};
      cam.x-=dx/cam.scale; cam.y-=dy/cam.scale;
    } else if(touchState.mode==='creating'){
      const dx=t.clientX-touchState.startScreen.x, dy=t.clientY-touchState.startScreen.y;
      if(Math.hypot(dx,dy)>8) clearTimeout(holdTimer);
    }
  } else if(touches.length===2 && touchState.mode==='pinch'){
    const a=touches[0], b=touches[1];
    const newDist=distTouches(a,b), center=centerTouches(a,b);
    const factor=newDist/(touchState.lastDist||newDist);
    const newScale=Math.max(0.08, Math.min(8, touchState.startScale*factor));
    const screenCenter={x:center.x,y:center.y};
    const worldAtScreenBefore=touchState.pinchRefWorld;
    cam.scale=newScale;
    cam.x=worldAtScreenBefore.x-(screenCenter.x-canvas.clientWidth/2)/cam.scale;
    cam.y=worldAtScreenBefore.y-(screenCenter.y-canvas.clientHeight/2)/cam.scale;
  }
},{passive:false});

canvas.addEventListener('touchend',(ev)=>{
  clearTimeout(holdTimer);
  if(touchState.mode==='creating'){
    const changed=ev.changedTouches[0];
    if(changed){
      const endScreen={x:changed.clientX,y:changed.clientY};
      const dx=endScreen.x-touchState.startScreen.x;
      const dy=endScreen.y-touchState.startScreen.y;
      const vx=dx*0.02, vy=dy*0.02;
      const m=parseFloat(document.getElementById('massa').value)||10;
      const pos=touchState.startWorld||screenToWorld(endScreen.x,endScreen.y);
      bodies.push(makeBody(m,pos.x,pos.y,vx,vy));
      recomputeColors();
    }
  }
  touchState.mode='idle';
  touchState.lastDist=0;
  touchState.pinchRefWorld=null;
},{passive:false});

canvas.addEventListener('touchcancel',(ev)=>{ touchState.mode='idle'; clearTimeout(holdTimer); });
canvas.addEventListener('dblclick',(ev)=>{ 
  cam.x=0;
  cam.y=0; 
  cam.scale=1;
});

/* === Optional: orientation change === */
window.addEventListener('orientationchange', ()=>{ fitCanvas(); });

/* === initialization color recompute === */
recomputeColors();

</script>
</body>
</html>
