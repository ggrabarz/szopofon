<div class="sc-root">

<div class="sc-bar">
  <button type="button" class="sc-btn sc-btn-red" data-fill="0">Red</button>
  <button type="button" class="sc-btn sc-btn-green" data-fill="1">Green</button>
  <button type="button" class="sc-btn sc-btn-blue" data-fill="2">Blue</button>
  <button type="button" class="sc-btn sc-btn-white" data-fill="3">White</button>
  <button type="button" class="sc-btn sc-btn-black" data-fill="4">Black</button>
  <button type="button" class="sc-btn sc-btn-grey" data-fill="5">50% grey</button>
  <button type="button" class="sc-btn sc-btn-fs" id="sc-fs">Fullscreen</button>
</div>

<div class="sc-stage" id="sc-stage">
  <div class="sc-fill" id="sc-fill"></div>
  <span class="sc-close" id="sc-close" role="button" tabindex="0" aria-label="Exit fullscreen">×</span>
  <div class="sc-fill-label" id="sc-fill-label">Black</div>
</div>

<section class="sc-card">
  <h3 class="sc-h">Gradient banding</h3>
  <canvas class="sc-canvas" id="sc-grad"></canvas>
  <p class="sc-note">Top: black to white. Bottom: hue sweep. Bands or steps mean the panel is not smooth.</p>
</section>

<section class="sc-card">
  <h3 class="sc-h">Uniformity (grey steps)</h3>
  <canvas class="sc-canvas" id="sc-uni"></canvas>
  <p class="sc-note">16 steps from 0 to 255. Crushed or identical neighbours mean poor low-end separation.</p>
</section>

<section class="sc-card">
  <h3 class="sc-h">Screen info</h3>
  <div class="sc-info" id="sc-info"></div>
</section>

</div>

<style>
.sc-root{box-sizing:border-box;font-size:16px}
.sc-root *{box-sizing:border-box}
.sc-bar{display:flex;flex-wrap:wrap;gap:8px;margin:0 0 12px}
.sc-btn{min-height:44px;min-width:44px;padding:8px 12px;border:1px solid rgba(127,127,127,.3);border-radius:10px;background:#2563eb;color:#fff;font-size:15px;cursor:pointer}
.sc-btn:active{transform:scale(.97)}
.sc-stage{position:relative;width:100%;min-height:62vh;border-radius:12px;overflow:hidden;background:#000;border:1px solid rgba(127,127,127,.25)}
.sc-fill{position:absolute;inset:0;cursor:pointer;touch-action:none}
.sc-close{position:absolute;top:8px;right:8px;z-index:5;width:36px;height:36px;display:grid;place-items:center;border-radius:999px;background:rgba(0,0,0,.55);color:#fff;font-size:20px;line-height:1;cursor:pointer;user-select:none;border:1px solid rgba(255,255,255,.25)}
.sc-fill-label{position:absolute;bottom:8px;left:8px;z-index:4;padding:4px 8px;border-radius:8px;background:rgba(0,0,0,.55);color:#fff;font-size:13px;pointer-events:none}
.sc-card{margin:14px 0;padding:12px;border-radius:12px;background:rgba(127,127,127,.08);border:1px solid rgba(127,127,127,.18)}
.sc-h{margin:0 0 8px;font-size:17px}
.sc-canvas{width:100%;height:90px;display:block;border-radius:8px;background:#000}
.sc-note{margin:8px 0 0;font-size:13px;opacity:.75}
.sc-info{font-family:ui-monospace,monospace;font-size:14px;line-height:1.6;white-space:pre-wrap;word-break:break-word}
.sc-stage:fullscreen,.sc-stage:-webkit-full-screen{border-radius:0;border:none}
.sc-stage:fullscreen .sc-fill,.sc-stage:-webkit-full-screen .sc-fill{inset:0}
</style>

<script>
(function(){
  const root = document.currentScript && document.currentScript.previousElementSibling && document.currentScript.previousElementSibling.previousElementSibling;
  const fills = [
    {name:'Red', color:'#ff0000'},
    {name:'Green', color:'#00ff00'},
    {name:'Blue', color:'#0000ff'},
    {name:'White', color:'#ffffff'},
    {name:'Black', color:'#000000'},
    {name:'50% grey', color:'#808080'}
  ];
  let idx = 4;
  const stage = document.getElementById('sc-stage');
  const fill = document.getElementById('sc-fill');
  const label = document.getElementById('sc-fill-label');
  const close = document.getElementById('sc-close');

  function applyFill(i){
    idx = ((i % fills.length) + fills.length) % fills.length;
    fill.style.background = fills[idx].color;
    label.textContent = fills[idx].name;
    label.style.color = (fills[idx].color === '#ffffff' || fills[idx].color === '#808080' || fills[idx].color === '#00ff00') ? '#000' : '#fff';
  }
  function advance(){ applyFill(idx + 1); }
  function exitFs(){
    try { if (document.exitFullscreen) document.exitFullscreen(); else if (document.webkitExitFullscreen) document.webkitExitFullscreen(); } catch(e){}
  }
  async function enterFs(){
    try {
      if (stage.requestFullscreen) await stage.requestFullscreen();
      else if (stage.webkitRequestFullscreen) stage.webkitRequestFullscreen();
      else alert('Fullscreen is not supported by this browser.');
    } catch(e){ alert('Fullscreen refused: ' + (e && e.message ? e.message : e)); }
  }

  applyFill(idx);

  // single tap advances (delayed to allow double-tap exit)
  let clicks = 0, timer = null;
  fill.addEventListener('click', function(e){
    clicks++;
    if (clicks === 1) {
      timer = setTimeout(function(){ advance(); clicks = 0; }, 260);
    } else {
      clearTimeout(timer); clicks = 0;
      exitFs();
    }
  });

  close.addEventListener('click', function(e){ e.stopPropagation(); exitFs(); });
  close.addEventListener('keydown', function(e){ if (e.key === 'Enter' || e.key === ' ') { e.preventDefault(); exitFs(); } });

  document.getElementById('sc-fs').addEventListener('click', enterFs);
  document.querySelectorAll('.sc-bar [data-fill]').forEach(function(b){
    b.addEventListener('click', function(){ applyFill(parseInt(b.getAttribute('data-fill'),10)); });
  });

  // screen info
  const info = document.getElementById('sc-info');
  function updateInfo(){
    const dpr = window.devicePixelRatio || 1;
    const lines = [];
    lines.push('CSS px: ' + window.innerWidth + ' x ' + window.innerHeight);
    lines.push('devicePixelRatio: ' + dpr);
    lines.push('device px: ' + Math.round(window.innerWidth * dpr) + ' x ' + Math.round(window.innerHeight * dpr));
    lines.push('screen: ' + screen.width + ' x ' + screen.height);
    lines.push('orientation: ' + (screen.orientation ? screen.orientation.type : 'unknown'));
    lines.push('colorDepth: ' + screen.colorDepth);
    info.textContent = lines.join('\n');
  }
  updateInfo();
  window.addEventListener('resize', updateInfo);
  window.addEventListener('orientationchange', updateInfo);
  if (window.visualViewport) window.visualViewport.addEventListener('resize', updateInfo);

  // canvas helpers
  function fit(canvas){
    const dpr = window.devicePixelRatio || 1;
    const r = canvas.getBoundingClientRect();
    const w = Math.max(1, Math.round(r.width * dpr));
    const h = Math.max(1, Math.round(r.height * dpr));
    if (canvas.width !== w || canvas.height !== h) { canvas.width = w; canvas.height = h; }
    const ctx = canvas.getContext('2d');
    ctx.setTransform(dpr,0,0,dpr,0,0);
    return {ctx:ctx, w:r.width, h:r.height};
  }

  function drawGrad(){
    const c = document.getElementById('sc-grad');
    const o = fit(c);
    const ctx = o.ctx;
    const w = o.w, h = o.h;
    ctx.clearRect(0,0,w,h);
    // grayscale top half
    const g1 = ctx.createLinearGradient(0,0,w,0);
    g1.addColorStop(0,'#000'); g1.addColorStop(1,'#fff');
    ctx.fillStyle = g1;
    ctx.fillRect(0,0,w,h/2);
    // hue bottom half
    const g2 = ctx.createLinearGradient(0,0,w,0);
    for (let i=0;i<=6;i++){ g2.addColorStop(i/6, 'hsl(' + (i*60) + ',100%,50%)'); }
    ctx.fillStyle = g2;
    ctx.fillRect(0,h/2,w,h/2);
    // divider
    ctx.strokeStyle = 'rgba(127,127,127,.4)';
    ctx.beginPath(); ctx.moveTo(0,h/2); ctx.lineTo(w,h/2); ctx.stroke();
  }

  function drawUni(){
    const c = document.getElementById('sc-uni');
    const o = fit(c);
    const ctx = o.ctx;
    const w = o.w, h = o.h;
    ctx.clearRect(0,0,w,h);
    const steps = 16;
    const cell = w / steps;
    ctx.font = '10px ui-monospace,monospace';
    ctx.textAlign = 'center';
    ctx.textBaseline = 'middle';
    for (let i=0;i<steps;i++){
      const v = Math.round(i * 255 / (steps-1));
      const x = i * cell;
      ctx.fillStyle = 'rgb(' + v + ',' + v + ',' + v + ')';
      ctx.fillRect(x, 0, cell, h - 16);
      ctx.fillStyle = (v > 127) ? '#000' : '#fff';
      ctx.fillText(String(v), x + cell/2, h - 8);
    }
  }

  function redraw(){ drawGrad(); drawUni(); }
  redraw();
  const ro = new ResizeObserver(redraw);
  ro.observe(document.getElementById('sc-grad'));
  ro.observe(document.getElementById('sc-uni'));
  window.addEventListener('orientationchange', function(){ setTimeout(redraw, 120); });
})();
</script>
