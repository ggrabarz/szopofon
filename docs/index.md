This page tests a display, speakers, camera, touch, and sensors. Nothing is uploaded. It works on any phone with a modern browser.

<div class="ix-root">
<style>
.ix-root{font-family:inherit;color:inherit}
.ix-card{border:1px solid rgba(127,127,127,.35);border-radius:14px;padding:14px;background:rgba(127,127,127,.06)}
.ix-readout{display:grid;gap:6px;font-size:16px}
.ix-row{display:flex;justify-content:space-between;gap:12px;align-items:baseline;flex-wrap:wrap}
.ix-key{opacity:.7;font-size:13px}
.ix-val{font-family:ui-monospace,monospace;font-size:13px;word-break:break-all;text-align:right}
.ix-grid{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-top:16px}
@media(min-width:480px){.ix-grid{grid-template-columns:1fr 1fr 1fr}}
.ix-linkcard{display:flex;align-items:center;min-height:88px;padding:14px;border-radius:14px;border:1px solid rgba(127,127,127,.35);background:rgba(127,127,127,.08);text-decoration:none;color:inherit;font-size:18px;font-weight:600;line-height:1.2}
.ix-linkcard:active{transform:scale(.98)}
.ix-linkcard small{display:block;font-size:13px;font-weight:400;opacity:.75;margin-top:4px}
.ix-title{font-size:15px}
</style>

<div class="ix-grid">
  <a class="ix-linkcard" href="music/"><span>Music<small>Radio, YouTube, Spotify, speaker tones</small></span></a>
  <a class="ix-linkcard" href="video/"><span>4K Video<small>Real 4K clips and detail checks</small></span></a>
  <a class="ix-linkcard" href="photo/"><span>Photo<small>Shoot or load a photo, inspect at 1:1</small></span></a>
  <a class="ix-linkcard" href="screen/"><span>Screen<small>Dead pixels, fills, gradients</small></span></a>
  <a class="ix-linkcard" href="touch/"><span>Touch<small>Multi-touch, accuracy, dead zones</small></span></a>
  <a class="ix-linkcard" href="sensors/"><span>Sensors<small>Motion, vibration, microphone</small></span></a>
  <a class="ix-linkcard" href="benchmarks/"><span>Benchmarks<small>Deeper tools in new tabs</small></span></a>
</div>

<div class="ix-card" style="margin-top:16px">
  <div class="ix-readout" id="ix-readout">
    <div class="ix-row"><span class="ix-key">User agent</span><span class="ix-val" id="ix-ua">…</span></div>
    <div class="ix-row"><span class="ix-key">Screen CSS size</span><span class="ix-val" id="ix-screen">…</span></div>
    <div class="ix-row"><span class="ix-key">devicePixelRatio</span><span class="ix-val" id="ix-dpr">…</span></div>
    <div class="ix-row"><span class="ix-key">Hardware pixels</span><span class="ix-val" id="ix-hw">…</span></div>
    <div class="ix-row"><span class="ix-key">Viewport</span><span class="ix-val" id="ix-vp">…</span></div>
    <div class="ix-row"><span class="ix-key">colorDepth</span><span class="ix-val" id="ix-cd">…</span></div>
    <div class="ix-row"><span class="ix-key">Secure context (HTTPS)</span><span class="ix-val" id="ix-sec">…</span></div>
    <div class="ix-row"><span class="ix-key">hardwareConcurrency</span><span class="ix-val" id="ix-hc">…</span></div>
    <div class="ix-row"><span class="ix-key">deviceMemory</span><span class="ix-val" id="ix-dm">…</span></div>
    <div class="ix-row"><span class="ix-key">Platform</span><span class="ix-val" id="ix-pl">…</span></div>
    <div class="ix-row"><span class="ix-key">Refresh rate</span><span class="ix-val" id="ix-rr">measuring…</span></div>
  </div>
</div>

<script>
(function(){
  var d = document;
  function set(id, v){ var e = d.getElementById(id); if(e) e.textContent = v; }
  set('ix-ua', navigator.userAgent);
  set('ix-screen', screen.width + ' x ' + screen.height);
  var dpr = window.devicePixelRatio || 1;
  set('ix-dpr', dpr);
  set('ix-hw', Math.round(screen.width*dpr) + ' x ' + Math.round(screen.height*dpr));
  set('ix-vp', window.innerWidth + ' x ' + window.innerHeight);
  set('ix-cd', screen.colorDepth);
  set('ix-sec', window.isSecureContext ? 'yes' : 'no');
  set('ix-hc', navigator.hardwareConcurrency || 'unknown');
  set('ix-dm', (navigator.deviceMemory != null) ? navigator.deviceMemory + ' GB' : 'not reported');
  set('ix-pl', (navigator.userAgentData && navigator.userAgentData.platform) || navigator.platform || 'unknown');

  var samples = [];
  var last = null;
  function frame(ts){
    if(last !== null) samples.push(ts - last);
    last = ts;
    if(samples.length < 60) requestAnimationFrame(frame);
    else {
      var s = samples.slice().sort(function(a,b){return a-b;});
      var mid = Math.floor(s.length/2);
      var med = s.length % 2 ? s[mid] : (s[mid-1]+s[mid])/2;
      var hz = 1000/med;
      set('ix-rr', hz.toFixed(1) + ' Hz (' + hz.toFixed(1) + ' fps) - measured, approximate');
    }
  }
  requestAnimationFrame(frame);
})();
</script>
</div>
