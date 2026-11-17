<div class="se-root">

<section class="se-card">
  <h3 class="se-h">Motion sensors</h3>
  <button type="button" class="se-btn" id="se-start">Start sensors</button>
  <div class="se-readout" id="se-status">Tap Start. iOS will ask for permission.</div>
  <div class="se-grid">
    <div class="se-cell"><span class="se-label">alpha</span><span class="se-val" id="se-alpha">n/a</span></div>
    <div class="se-cell"><span class="se-label">beta</span><span class="se-val" id="se-beta">n/a</span></div>
    <div class="se-cell"><span class="se-label">gamma</span><span class="se-val" id="se-gamma">n/a</span></div>
    <div class="se-cell"><span class="se-label">acc X</span><span class="se-val" id="se-ax">n/a</span></div>
    <div class="se-cell"><span class="se-label">acc Y</span><span class="se-val" id="se-ay">n/a</span></div>
    <div class="se-cell"><span class="se-label">acc Z</span><span class="se-val" id="se-az">n/a</span></div>
  </div>
</section>

<section class="se-card">
  <h3 class="se-h">Vibration</h3>
  <div class="se-row">
    <button type="button" class="se-btn" id="se-vshort">Short</button>
    <button type="button" class="se-btn" id="se-vlong">Long</button>
    <button type="button" class="se-btn" id="se-vpat">Pattern</button>
  </div>
  <div class="se-readout" id="se-vstatus"></div>
</section>

<section class="se-card">
  <h3 class="se-h">Microphone level</h3>
  <div class="se-row">
    <button type="button" class="se-btn" id="se-micstart">Start mic</button>
    <button type="button" class="se-btn" id="se-micstop">Stop mic</button>
  </div>
  <div class="se-meter"><div class="se-meterbar" id="se-meterbar"></div></div>
  <div class="se-readout" id="se-micstatus">Mic is off.</div>
</section>

<section class="se-card">
  <h3 class="se-h">Brightness hint</h3>
  <p class="se-note">Ambient light is not exposed to the web on iOS, and only behind a flag on Chrome. No value is faked here. To test auto-brightness, open the <a href="screen/">screen test page</a>, set a white fill, and cover the top bezel with a finger; the panel should dim.</p>
</section>

</div>

<style>
.se-root{box-sizing:border-box;font-size:16px}
.se-root *{box-sizing:border-box}
.se-card{margin:0 0 14px;padding:12px;border-radius:12px;background:rgba(127,127,127,.08);border:1px solid rgba(127,127,127,.18)}
.se-h{margin:0 0 8px;font-size:17px}
.se-row{display:flex;gap:8px;flex-wrap:wrap;margin:8px 0}
.se-btn{min-height:44px;min-width:44px;padding:8px 12px;border:1px solid rgba(127,127,127,.3);border-radius:10px;background:#2563eb;color:#fff;font-size:15px;cursor:pointer}
.se-btn:active{transform:scale(.97)}
.se-readout{font-family:ui-monospace,monospace;font-size:14px;line-height:1.6;white-space:pre-wrap;word-break:break-word;margin-top:8px}
.se-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-top:8px}
.se-cell{padding:8px;border-radius:8px;background:rgba(127,127,127,.06);border:1px solid rgba(127,127,127,.15);text-align:center}
.se-label{display:block;font-size:12px;opacity:.7;margin-bottom:4px}
.se-val{font-family:ui-monospace,monospace;font-size:15px}
.se-meter{width:100%;height:18px;border-radius:9px;overflow:hidden;background:rgba(127,127,127,.15);margin-top:8px}
.se-meterbar{height:100%;width:0%;background:linear-gradient(90deg,#22c55e,#eab308,#ef4444);transition:width 60ms linear}
.se-note{margin:8px 0 0;font-size:13px;opacity:.75;line-height:1.5}
</style>

<script>
(function(){
  const status = document.getElementById('se-status');
  const fmt = function(v){ return (v === null || v === undefined) ? 'n/a' : Number(v).toFixed(2); };

  async function requestPerms(){
    try {
      if (typeof DeviceOrientationEvent !== 'undefined' && typeof DeviceOrientationEvent.requestPermission === 'function') {
        const r = await DeviceOrientationEvent.requestPermission();
        if (r !== 'granted') { status.textContent = 'Permission refused for device orientation.'; return false; }
      }
      if (typeof DeviceMotionEvent !== 'undefined' && typeof DeviceMotionEvent.requestPermission === 'function') {
        const r = await DeviceMotionEvent.requestPermission();
        if (r !== 'granted') { status.textContent = 'Permission refused for device motion.'; return false; }
      }
      return true;
    } catch(e){
      status.textContent = 'Permission request failed: ' + (e && e.message ? e.message : e);
      return false;
    }
  }

  function onOrient(e){
    document.getElementById('se-alpha').textContent = fmt(e.alpha);
    document.getElementById('se-beta').textContent = fmt(e.beta);
    document.getElementById('se-gamma').textContent = fmt(e.gamma);
  }
  function onMotion(e){
    const a = e.acceleration || e.accelerationIncludingGravity;
    if (a){
      document.getElementById('se-ax').textContent = fmt(a.x);
      document.getElementById('se-ay').textContent = fmt(a.y);
      document.getElementById('se-az').textContent = fmt(a.z);
    }
  }

  document.getElementById('se-start').addEventListener('click', async function(){
    const ok = await requestPerms();
    if (!ok) return;
    if (typeof DeviceOrientationEvent === 'undefined') {
      status.textContent = 'DeviceOrientation not supported on this browser.';
      return;
    }
    window.addEventListener('deviceorientation', onOrient, true);
    window.addEventListener('devicemotion', onMotion, true);
    status.textContent = 'Sensors running. Move the phone.';
  });

  // Vibration
  const vstatus = document.getElementById('se-vstatus');
  function vibrate(ms){
    if (!navigator.vibrate) { vstatus.textContent = 'Vibration not supported on iOS.'; return; }
    try { navigator.vibrate(ms); vstatus.textContent = 'Vibrated.'; }
    catch(e){ vstatus.textContent = 'Vibration failed: ' + (e && e.message ? e.message : e); }
  }
  document.getElementById('se-vshort').addEventListener('click', function(){ vibrate(50); });
  document.getElementById('se-vlong').addEventListener('click', function(){ vibrate(500); });
  document.getElementById('se-vpat').addEventListener('click', function(){
    if (!navigator.vibrate) { vstatus.textContent = 'Vibration not supported on iOS.'; return; }
    try { navigator.vibrate([100,50,100,50,300]); vstatus.textContent = 'Pattern played.'; }
    catch(e){ vstatus.textContent = 'Vibration failed: ' + (e && e.message ? e.message : e); }
  });
  if (!navigator.vibrate) vstatus.textContent = 'Vibration not supported on iOS.';

  // Microphone
  const micstatus = document.getElementById('se-micstatus');
  const meterbar = document.getElementById('se-meterbar');
  let stream = null, audioCtx = null, analyser = null, rafId = 0, dataArr = null;
  async function startMic(){
    if (stream) stopMic();
    if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) {
      micstatus.textContent = 'getUserMedia not available. Requires HTTPS.';
      return;
    }
    try {
      stream = await navigator.mediaDevices.getUserMedia({ audio: true });
      audioCtx = new (window.AudioContext || window.webkitAudioContext)();
      const src = audioCtx.createMediaStreamSource(stream);
      analyser = audioCtx.createAnalyser();
      analyser.fftSize = 1024;
      src.connect(analyser);
      dataArr = new Float32Array(analyser.fftSize);
      micstatus.textContent = 'Listening. Speak or tap the phone.';
      runMeter();
    } catch(e){
      micstatus.textContent = 'Microphone denied or unavailable: ' + (e && e.message ? e.message : e);
      stopMic();
    }
  }
  function runMeter(){
    if (!analyser) return;
    analyser.getFloatTimeDomainData(dataArr);
    let sum = 0;
    for (let i = 0; i < dataArr.length; i++) sum += dataArr[i]*dataArr[i];
    const rms = Math.sqrt(sum / dataArr.length);
    const pct = Math.min(100, rms * 300);
    meterbar.style.width = pct + '%';
    rafId = requestAnimationFrame(runMeter);
  }
  function stopMic(){
    if (rafId){ cancelAnimationFrame(rafId); rafId = 0; }
    if (stream){ stream.getTracks().forEach(function(t){ t.stop(); }); stream = null; }
    if (audioCtx){ try { audioCtx.close(); } catch(e){} audioCtx = null; }
    analyser = null; dataArr = null;
    meterbar.style.width = '0%';
    micstatus.textContent = 'Mic stopped. All tracks released.';
  }
  document.getElementById('se-micstart').addEventListener('click', startMic);
  document.getElementById('se-micstop').addEventListener('click', stopMic);
  window.addEventListener('pagehide', stopMic);
})();
</script>
