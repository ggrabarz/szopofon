# Music

<style>
.mu-wrap{max-width:720px;margin:0 auto;padding:.5rem 0;font-size:16px;line-height:1.4}
.mu-section{margin:1.25rem 0}
.mu-section h2{font-size:1.15rem;margin:0 0 .5rem}
.mu-row{display:flex;align-items:center;gap:.75rem;padding:.75rem;border:1px solid var(--md-default-fg-color--lightest,#ccc);border-radius:.5rem;margin:.5rem 0;min-height:44px;cursor:pointer}
.mu-row.mu-playing{background:rgba(0,120,215,.12)}
.mu-playbtn{min-width:44px;min-height:44px;padding:.5rem .75rem;font-size:16px;border:1px solid currentColor;border-radius:.4rem;background:transparent;cursor:pointer}
.mu-station{flex:1;min-width:0}
.mu-station-name{font-weight:600}
.mu-station-genre{font-size:.85rem;opacity:.75}
.mu-state{font-size:.8rem;opacity:.7;min-width:4rem;text-align:right}
.mu-controls{display:flex;flex-wrap:wrap;align-items:center;gap:.75rem;margin:.75rem 0}
.mu-controls label{display:flex;align-items:center;gap:.5rem;flex:1;min-width:180px}
.mu-controls input[type=range]{flex:1;min-width:120px}
.mu-btn{min-height:44px;padding:.5rem 1rem;font-size:16px;border:1px solid currentColor;border-radius:.4rem;background:transparent;cursor:pointer}
.mu-nowplaying{font-size:.9rem;opacity:.85;margin:.5rem 0;min-height:1.2em}
.mu-err{color:#c00;background:rgba(200,0,0,.08);padding:.5rem .75rem;border-radius:.4rem;margin:.5rem 0;font-size:.9rem}
.mu-yt-wrap{position:relative;padding-bottom:56.25%;height:0;overflow:hidden;border-radius:.5rem;margin:.5rem 0}
.mu-yt-wrap iframe{position:absolute;top:0;left:0;width:100%;height:100%;border:0}
.mu-sp-iframe{width:100%;height:152px;border:0;border-radius:.5rem;margin:.5rem 0}
.mu-sp-link{display:inline-block;min-height:44px;line-height:44px;padding:0 .5rem}
.mu-sweep{display:grid;grid-template-columns:repeat(2,1fr);gap:.5rem;margin:.5rem 0}
.mu-sweep button{min-height:44px;font-size:16px;border:1px solid currentColor;border-radius:.4rem;background:transparent;cursor:pointer}
@media (min-width:480px){.mu-sweep{grid-template-columns:repeat(3,1fr)}}
.mu-note{font-size:.85rem;opacity:.75;margin:.5rem 0}
.mu-muted{opacity:.7;font-size:.9rem}
</style>

## Radio

<div class="mu-radio">
  <div class="mu-row" data-url="https://ice1.somafm.com/groovesalad-256-mp3" data-name="Groove Salad">
    <button class="mu-playbtn" type="button">Play</button>
    <div class="mu-station">
      <div class="mu-station-name">Groove Salad</div>
      <div class="mu-station-genre">chill downtempo</div>
    </div>
    <span class="mu-state">stopped</span>
  </div>
  <div class="mu-row" data-url="https://ice1.somafm.com/u80s-256-mp3" data-name="Underground 80s">
    <button class="mu-playbtn" type="button">Play</button>
    <div class="mu-station">
      <div class="mu-station-name">Underground 80s</div>
      <div class="mu-station-genre">synthpop and new wave</div>
    </div>
    <span class="mu-state">stopped</span>
  </div>
  <div class="mu-row" data-url="https://ice1.somafm.com/defcon-256-mp3" data-name="DEF CON Radio">
    <button class="mu-playbtn" type="button">Play</button>
    <div class="mu-station">
      <div class="mu-station-name">DEF CON Radio</div>
      <div class="mu-station-genre">electronic, bass heavy</div>
    </div>
    <span class="mu-state">stopped</span>
  </div>
  <div class="mu-row" data-url="https://ice1.somafm.com/dronezone-256-mp3" data-name="Drone Zone">
    <button class="mu-playbtn" type="button">Play</button>
    <div class="mu-station">
      <div class="mu-station-name">Drone Zone</div>
      <div class="mu-station-genre">ambient, quiet detail</div>
    </div>
    <span class="mu-state">stopped</span>
  </div>
  <div class="mu-row" data-url="https://stream.radioparadise.com/aac-320" data-name="Radio Paradise">
    <button class="mu-playbtn" type="button">Play</button>
    <div class="mu-station">
      <div class="mu-station-name">Radio Paradise</div>
      <div class="mu-station-genre">eclectic rock mix, 320 kbps AAC</div>
    </div>
    <span class="mu-state">stopped</span>
  </div>
  <div class="mu-controls">
    <label>Volume <input id="mu-vol" type="range" min="0" max="1" step="0.01" value="0.8"></label>
    <button id="mu-stopall" class="mu-btn" type="button">Stop all</button>
  </div>
  <div id="mu-nowplaying" class="mu-nowplaying">Nothing playing</div>
  <div id="mu-radioerr" class="mu-err" hidden></div>
</div>

## YouTube

<div class="mu-yt">
  <div class="mu-yt-wrap"><iframe src="https://www.youtube-nocookie.com/embed/lTRiuFIWV54" title="lofi hip hop study session" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen loading="lazy"></iframe></div>
  <p class="mu-note">1 A.M Study Session, lofi hip hop</p>
  <div class="mu-yt-wrap"><iframe src="https://www.youtube-nocookie.com/embed/1ZYbU82GVz4" title="relaxing sleep music" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen loading="lazy"></iframe></div>
  <p class="mu-note">Peder B. Helland, relaxing sleep music</p>
  <div class="mu-yt-wrap"><iframe src="https://www.youtube-nocookie.com/embed/LXb3EKWsInQ" title="Costa Rica in 4K" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen loading="lazy"></iframe></div>
  <p class="mu-note">Costa Rica in 4K, speaker/treble check</p>
</div>

<p class="mu-note">YouTube ads may play. That is normal, not a phone fault.</p>

## Spotify

<div class="mu-sp">
  <iframe class="mu-sp-iframe" src="https://open.spotify.com/embed/track/0gJzcPxdJDa8XCFIkFzHGk?theme=0" title="Woods of Mystery" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture" loading="lazy"></iframe>
  <a class="mu-sp-link" href="https://open.spotify.com/track/0gJzcPxdJDa8XCFIkFzHGk" target="_blank" rel="noopener">Open Woods of Mystery in app</a>
  <iframe class="mu-sp-iframe" src="https://open.spotify.com/embed/track/0EDeh0yFVKQZZQoJ1AVw3Z?theme=0" title="Sabbath Bloody Sabbath" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture" loading="lazy"></iframe>
  <a class="mu-sp-link" href="https://open.spotify.com/track/0EDeh0yFVKQZZQoJ1AVw3Z" target="_blank" rel="noopener">Open Sabbath Bloody Sabbath in app</a>
</div>

<p class="mu-note">Embed plays a short preview without login. Full playback needs an account.</p>

## Speaker check

<div class="mu-spkr">
  <div id="mu-audioMsg" class="mu-note" hidden></div>
  <div class="mu-sweep">
    <button type="button" data-freq="60">60 Hz</button>
    <button type="button" data-freq="120">120 Hz</button>
    <button type="button" data-freq="250">250 Hz</button>
    <button type="button" data-freq="1000">1 kHz</button>
    <button type="button" data-freq="4000">4 kHz</button>
    <button type="button" data-freq="8000">8 kHz</button>
    <button type="button" data-freq="12000">12 kHz</button>
    <button type="button" data-stereo="left">Left only</button>
    <button type="button" data-stereo="right">Right only</button>
  </div>
  <p class="mu-note">On iOS, tap a button first; audio starts only after a tap. Each tone runs 5 s with fade.</p>
</div>

<p class="mu-muted">On a phone the browser may force volume limits, so test with the phone's own volume keys at maximum.</p>

<script>
(function(){
  var rows = document.querySelectorAll('.mu-row');
  var volEl = document.getElementById('mu-vol');
  var stopAllBtn = document.getElementById('mu-stopall');
  var nowEl = document.getElementById('mu-nowplaying');
  var errEl = document.getElementById('mu-radioerr');
  var audios = new Map();
  var current = null;
  var timer = null;
  var startedAt = 0;

  function setErr(msg){ errEl.textContent = msg; errEl.hidden = !msg; }

  function updateElapsed(){
    if(!current){ nowEl.textContent = 'Nothing playing'; return; }
    var sec = Math.floor((Date.now() - startedAt) / 1000);
    nowEl.textContent = 'Now playing: ' + current.name + ' — ' + sec + 's';
  }

  function stopAll(){
    audios.forEach(function(a){ try{ a.pause(); a.src = ''; }catch(e){} });
    audios.clear();
    current = null;
    rows.forEach(function(r){
      r.classList.remove('mu-playing');
      var b = r.querySelector('.mu-playbtn');
      if(b) b.textContent = 'Play';
      var s = r.querySelector('.mu-state');
      if(s) s.textContent = 'stopped';
    });
    if(timer){ clearInterval(timer); timer = null; }
    updateElapsed();
  }

  function playStation(row){
    var url = row.getAttribute('data-url');
    var name = row.getAttribute('data-name');
    if(current && current.row !== row){
      var prev = audios.get(current.row);
      if(prev){ try{ prev.pause(); prev.src = ''; }catch(e){} }
      audios.delete(current.row);
      current.row.classList.remove('mu-playing');
      var pb = current.row.querySelector('.mu-playbtn');
      if(pb) pb.textContent = 'Play';
      var st = current.row.querySelector('.mu-state');
      if(st) st.textContent = 'stopped';
      current = null;
      if(timer){ clearInterval(timer); timer = null; }
    }
    if(current && current.row === row){
      var a = audios.get(row);
      if(a){ try{ a.pause(); a.src = ''; }catch(e){} }
      audios.delete(row);
      current = null;
      row.classList.remove('mu-playing');
      var b = row.querySelector('.mu-playbtn');
      if(b) b.textContent = 'Play';
      var s = row.querySelector('.mu-state');
      if(s) s.textContent = 'stopped';
      if(timer){ clearInterval(timer); timer = null; }
      updateElapsed();
      return;
    }
    setErr('');
    var audio = new Audio(url);
    audio.preload = 'none';
    audio.volume = parseFloat(volEl.value);
    audios.set(row, audio);
    audio.addEventListener('error', function(){
      setErr(name + ' failed to load. Store wifi may block streams — try another station.');
      try{ audio.pause(); audio.src = ''; }catch(e){}
      audios.delete(row);
      if(current && current.row === row){ current = null; }
      row.classList.remove('mu-playing');
      var b = row.querySelector('.mu-playbtn');
      if(b) b.textContent = 'Play';
      var s = row.querySelector('.mu-state');
      if(s) s.textContent = 'error';
      if(timer){ clearInterval(timer); timer = null; }
      updateElapsed();
    });
    audio.play().then(function(){
      current = { row: row, name: name };
      row.classList.add('mu-playing');
      var b = row.querySelector('.mu-playbtn');
      if(b) b.textContent = 'Stop';
      var s = row.querySelector('.mu-state');
      if(s) s.textContent = 'playing';
      startedAt = Date.now();
      if(timer) clearInterval(timer);
      timer = setInterval(updateElapsed, 1000);
      updateElapsed();
    }).catch(function(){
      setErr(name + ' could not start. Tap play again, or try another station.');
      try{ audio.pause(); audio.src = ''; }catch(e){}
      audios.delete(row);
    });
  }

  rows.forEach(function(row){
    row.setAttribute('role','button');
    row.setAttribute('tabindex','0');
    row.addEventListener('click', function(){ playStation(row); });
    row.addEventListener('keydown', function(e){ if(e.key==='Enter' || e.key===' '){ e.preventDefault(); playStation(row); } });
    var btn = row.querySelector('.mu-playbtn');
    if(btn) btn.addEventListener('click', function(e){ e.stopPropagation(); playStation(row); });
  });
  if(volEl) volEl.addEventListener('input', function(){
    var v = parseFloat(volEl.value);
    audios.forEach(function(a){ a.volume = v; });
  });
  if(stopAllBtn) stopAllBtn.addEventListener('click', stopAll);
  updateElapsed();
})();

(function(){
  var msg = document.getElementById('mu-audioMsg');
  var Ctx = window.AudioContext || window.webkitAudioContext;
  if(!Ctx){
    if(msg){ msg.textContent = 'Web Audio is not supported in this browser. Try another browser.'; msg.hidden = false; }
    return;
  }
  var ctx = null;
  var active = [];
  function getCtx(){
    if(!ctx){ ctx = new Ctx(); }
    if(ctx.state === 'suspended'){ ctx.resume(); }
    return ctx;
  }
  function stopAll(){
    active.forEach(function(o){ try{ o.stop(); }catch(e){} });
    active = [];
  }
  function tone(freq, pan){
    stopAll();
    var c = getCtx();
    var now = c.currentTime;
    var osc = c.createOscillator();
    osc.type = 'sine';
    osc.frequency.value = freq;
    var gain = c.createGain();
    gain.gain.setValueAtTime(0, now);
    gain.gain.linearRampToValueAtTime(0.25, now + 0.05);
    gain.gain.setValueAtTime(0.25, now + 4.9);
    gain.gain.linearRampToValueAtTime(0, now + 5);
    osc.connect(gain);
    if(pan !== 0 && typeof c.createStereoPanner === 'function'){
      var p = c.createStereoPanner();
      p.pan.value = pan;
      gain.connect(p);
      p.connect(c.destination);
    } else {
      gain.connect(c.destination);
    }
    osc.start(now);
    osc.stop(now + 5);
    active.push(osc);
  }
  document.querySelectorAll('.mu-sweep button').forEach(function(b){
    b.addEventListener('click', function(){
      var f = b.getAttribute('data-freq');
      var st = b.getAttribute('data-stereo');
      if(f){ tone(parseFloat(f), 0); }
      else if(st === 'left'){ tone(1000, -1); }
      else if(st === 'right'){ tone(1000, 1); }
    });
  });
})();
</script>