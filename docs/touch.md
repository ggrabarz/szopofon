<div class="tc-root">

<section class="tc-card">
  <h3 class="tc-h">Drawing surface</h3>
  <canvas class="tc-draw" id="tc-draw"></canvas>
  <div class="tc-row">
    <button type="button" class="tc-btn" id="tc-clear">Clear canvas</button>
    <button type="button" class="tc-btn" id="tc-resetstats">Reset stats</button>
  </div>
  <div class="tc-readout" id="tc-readout"></div>
  <p class="tc-note" id="tc-pressure">Pressure: not reported by this browser.</p>
</section>

<section class="tc-card">
  <h3 class="tc-h">Tap accuracy</h3>
  <div class="tc-tapbox" id="tc-tapbox">
    <div class="tc-target" id="tc-target"></div>
  </div>
  <div class="tc-row">
    <button type="button" class="tc-btn" id="tc-tapreset">Reset</button>
  </div>
  <div class="tc-readout" id="tc-tapstats"></div>
  <p class="tc-note">Tap the circle. Targets can land near the corners, so dead zones show up as misses.</p>
</section>

<section class="tc-card">
  <h3 class="tc-h">Touch point map</h3>
  <div class="tc-mapbox" id="tc-mapbox"></div>
  <div class="tc-row">
    <button type="button" class="tc-btn" id="tc-mapreset">Reset</button>
  </div>
  <div class="tc-readout" id="tc-mapstats"></div>
  <p class="tc-note">Drag a finger to all four corners and edges. A strip that never reports means an unreachable edge.</p>
</section>

</div>

<style>
.tc-root{box-sizing:border-box;font-size:16px}
.tc-root *{box-sizing:border-box}
.tc-card{margin:0 0 14px;padding:12px;border-radius:12px;background:rgba(127,127,127,.08);border:1px solid rgba(127,127,127,.18)}
.tc-h{margin:0 0 8px;font-size:17px}
.tc-row{display:flex;gap:8px;flex-wrap:wrap;margin:8px 0}
.tc-btn{min-height:44px;min-width:44px;padding:8px 12px;border:1px solid rgba(127,127,127,.3);border-radius:10px;background:#2563eb;color:#fff;font-size:15px;cursor:pointer}
.tc-btn:active{transform:scale(.97)}
.tc-draw{width:100%;height:300px;display:block;border-radius:8px;background:#0b0d10;touch-action:none;cursor:crosshair}
.tc-readout{font-family:ui-monospace,monospace;font-size:14px;line-height:1.6;white-space:pre-wrap;word-break:break-word;margin-top:8px}
.tc-note{margin:8px 0 0;font-size:13px;opacity:.75}
.tc-tapbox{position:relative;width:100%;height:260px;border-radius:8px;background:rgba(127,127,127,.06);border:1px solid rgba(127,127,127,.2);overflow:hidden;touch-action:none}
.tc-target{position:absolute;border-radius:999px;background:#ef4444;box-shadow:0 0 0 2px rgba(255,255,255,.6);pointer-events:none}
.tc-mapbox{position:relative;width:100%;height:220px;border-radius:8px;background:rgba(127,127,127,.06);border:1px solid rgba(127,127,127,.2);overflow:hidden;touch-action:none}
</style>

<script>
(function(){
  // Drawing surface
  const canvas = document.getElementById('tc-draw');
  const ctx = canvas.getContext('2d');
  const active = new Map();
  let peak = 0;
  let totalMoves = 0;
  const moveTimes = [];
  let colorSeed = 0;

  function hueFor(id){ return 'hsl(' + ((id * 47 + 13) % 360) + ',85%,55%)'; }

  function resizeCanvas(){
    const dpr = window.devicePixelRatio || 1;
    const r = canvas.getBoundingClientRect();
    const w = Math.max(1, Math.round(r.width * dpr));
    const h = Math.max(1, Math.round(r.height * dpr));
    if (canvas.width !== w || canvas.height !== h){ canvas.width = w; canvas.height = h; }
    ctx.setTransform(dpr,0,0,dpr,0,0);
  }
  resizeCanvas();
  const roCanvas = new ResizeObserver(resizeCanvas);
  roCanvas.observe(canvas);

  function pos(e){
    const r = canvas.getBoundingClientRect();
    return { x: e.clientX - r.left, y: e.clientY - r.top };
  }
  function updateReadout(){
    const now = performance.now();
    while (moveTimes.length && now - moveTimes[0] > 1000) moveTimes.shift();
    const rate = moveTimes.length;
    const el = document.getElementById('tc-readout');
    el.textContent =
      'Pointers down now: ' + active.size + '\n' +
      'Peak this session: ' + peak + '\n' +
      'Total moves: ' + totalMoves + '\n' +
      'Move rate: ' + rate + ' Hz';
  }
  let rafId = 0;
  function loop(){ updateReadout(); rafId = requestAnimationFrame(loop); }
  rafId = requestAnimationFrame(loop);

  canvas.addEventListener('pointerdown', function(e){
    e.preventDefault();
    try { canvas.setPointerCapture(e.pointerId); } catch(_){}
    const p = pos(e);
    active.set(e.pointerId, { x: p.x, y: p.y, color: hueFor(e.pointerId) });
    if (active.size > peak) peak = active.size;
    updateReadout();
  });
  canvas.addEventListener('pointermove', function(e){
    if (!active.has(e.pointerId)) return;
    e.preventDefault();
    const p = pos(e);
    const st = active.get(e.pointerId);
    ctx.strokeStyle = st.color;
    ctx.lineWidth = 8;
    ctx.lineCap = 'round';
    ctx.lineJoin = 'round';
    ctx.beginPath();
    ctx.moveTo(st.x, st.y);
    ctx.lineTo(p.x, p.y);
    ctx.stroke();
    st.x = p.x; st.y = p.y;
    totalMoves++;
    moveTimes.push(performance.now());
  });
  function endPointer(e){
    active.delete(e.pointerId);
    try { canvas.releasePointerCapture(e.pointerId); } catch(_){}
    updateReadout();
  }
  canvas.addEventListener('pointerup', endPointer);
  canvas.addEventListener('pointercancel', endPointer);

  // pressure note
  const pressureEl = document.getElementById('tc-pressure');
  const pressureAvailable = (typeof PointerEvent !== 'undefined') && ('pressure' in PointerEvent.prototype);
  if (!pressureAvailable) {
    pressureEl.style.display = 'none';
  } else {
    canvas.addEventListener('pointermove', function(e){
      if (e.pressure !== undefined) pressureEl.textContent = 'Pressure: ' + (e.pressure || 0).toFixed(2);
    });
  }

  document.getElementById('tc-clear').addEventListener('click', function(){
    ctx.clearRect(0,0,canvas.width,canvas.height);
  });
  document.getElementById('tc-resetstats').addEventListener('click', function(){
    peak = 0; totalMoves = 0; moveTimes.length = 0;
    active.clear();
    ctx.clearRect(0,0,canvas.width,canvas.height);
    updateReadout();
  });

  // Tap accuracy
  const tapbox = document.getElementById('tc-tapbox');
  const target = document.getElementById('tc-target');
  const TAP_R = 18;
  let tX = 0, tY = 0;
  let hits = 0, misses = 0, sumDist = 0, taps = 0;
  function newTarget(){
    const r = tapbox.getBoundingClientRect();
    const pad = TAP_R + 4;
    tX = pad + Math.random() * Math.max(1, r.width - 2*pad);
    tY = pad + Math.random() * Math.max(1, r.height - 2*pad);
    target.style.left = (tX - TAP_R) + 'px';
    target.style.top = (tY - TAP_R) + 'px';
    target.style.width = (2*TAP_R) + 'px';
    target.style.height = (2*TAP_R) + 'px';
  }
  function updateTapStats(){
    const avg = taps ? (sumDist / taps) : 0;
    document.getElementById('tc-tapstats').textContent =
      'Hits: ' + hits + '   Misses: ' + misses + '   Taps: ' + taps + '\n' +
      'Average distance from center: ' + avg.toFixed(1) + ' px';
  }
  newTarget();
  const roTap = new ResizeObserver(newTarget);
  roTap.observe(tapbox);
  tapbox.addEventListener('pointerdown', function(e){
    const r = tapbox.getBoundingClientRect();
    const x = e.clientX - r.left;
    const y = e.clientY - r.top;
    const d = Math.hypot(x - tX, y - tY);
    taps++;
    sumDist += d;
    if (d <= TAP_R) hits++; else misses++;
    updateTapStats();
    newTarget();
  });

  document.getElementById('tc-tapreset').addEventListener('click', function(){
    hits = 0; misses = 0; sumDist = 0; taps = 0;
    updateTapStats();
    newTarget();
  });
  updateTapStats();

  // Touch point map
  const mapbox = document.getElementById('tc-mapbox');
  let minX = null, maxX = null, minY = null, maxY = null;
  const mapActive = new Set();
  let mapTaps = 0;
  function updateMapStats(){
    const fmt = function(v){ return (v === null) ? 'n/a' : v.toFixed(1) + '%'; };
    document.getElementById('tc-mapstats').textContent =
      'Min X: ' + fmt(minX) + '   Max X: ' + fmt(maxX) + '\n' +
      'Min Y: ' + fmt(minY) + '   Max Y: ' + fmt(maxY) + '\n' +
      'Samples: ' + mapTaps;
  }
  function recordMap(e){
    const r = mapbox.getBoundingClientRect();
    const rawX = (e.clientX - r.left) / r.width * 100;
    const rawY = (e.clientY - r.top) / r.height * 100;
    if (rawX < 0 || rawX > 100 || rawY < 0 || rawY > 100) return;
    if (minX === null || rawX < minX) minX = rawX;
    if (maxX === null || rawX > maxX) maxX = rawX;
    if (minY === null || rawY < minY) minY = rawY;
    if (maxY === null || rawY > maxY) maxY = rawY;
    mapTaps++;
    updateMapStats();
  }
  mapbox.addEventListener('pointerdown', function(e){
    try { mapbox.setPointerCapture(e.pointerId); } catch(_){}
    mapActive.add(e.pointerId);
    recordMap(e);
  });
  mapbox.addEventListener('pointermove', function(e){
    if (!mapActive.has(e.pointerId)) return;
    recordMap(e);
  });
  function endMap(e){
    mapActive.delete(e.pointerId);
    try { mapbox.releasePointerCapture(e.pointerId); } catch(_){}
  }
  mapbox.addEventListener('pointerup', endMap);
  mapbox.addEventListener('pointercancel', endMap);
  document.getElementById('tc-mapreset').addEventListener('click', function(){
    minX = null; maxX = null; minY = null; maxY = null; mapTaps = 0;
    updateMapStats();
  });
  updateMapStats();
})();
</script>
