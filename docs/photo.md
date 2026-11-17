Open a photo to judge this phone's display against it. Nothing is uploaded; the photo never leaves this device.

<div class="ph-root">
<style>
.ph-root{font-family:inherit;color:inherit}
.ph-lede{font-size:16px;margin:0 0 12px;opacity:.9}
.ph-drop{position:relative;border:2px dashed rgba(127,127,127,.5);border-radius:14px;padding:24px 16px;text-align:center;background:rgba(127,127,127,.06);min-height:120px;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:10px}
.ph-drop.ph-dragover{border-color:var(--md-primary-fg-color,#1976d2);background:rgba(127,127,127,.12)}
.ph-drop input[type=file]{position:absolute;inset:0;opacity:0;width:100%;height:100%;cursor:pointer;pointer-events:none}
.ph-choose{position:relative;z-index:2;display:inline-flex;align-items:center;justify-content:center;min-height:44px;min-width:120px;padding:10px 18px;border-radius:10px;border:1px solid rgba(127,127,127,.4);background:rgba(127,127,127,.12);font-size:16px;font-weight:600;cursor:pointer}
.ph-drop-hint{font-size:16px;opacity:.8;margin:0}
.ph-error{color:#c0392b;font-size:16px;margin:8px 0}
.ph-stage{position:relative;width:100%;height:60vh;min-height:260px;background:#000;border-radius:12px;overflow:hidden;margin-top:12px;touch-action:none}
.ph-layer{position:absolute;inset:0;width:100%;height:100%;object-fit:contain;display:none}
.ph-layer.ph-active{display:block}
.ph-layer.ph-one{width:auto;height:auto;max-width:none;max-height:none;object-fit:none}
.ph-empty{position:absolute;inset:0;display:flex;align-items:center;justify-content:center;color:#888;font-size:16px;pointer-events:none}
.ph-toolbar{display:flex;gap:8px;flex-wrap:wrap;margin-top:10px}
.ph-btn{min-height:44px;padding:8px 12px;border-radius:10px;border:1px solid rgba(127,127,127,.4);background:rgba(127,127,127,.08);font-size:16px;cursor:pointer}
.ph-btn:active{transform:scale(.98)}
.ph-compare{margin-top:10px;padding:10px;border-radius:10px;background:rgba(127,127,127,.06);border:1px solid rgba(127,127,127,.25)}
.ph-slider-label{display:block;font-size:16px;margin-bottom:6px}
.ph-compare input[type=range]{width:100%;min-height:44px}
.ph-ab{display:flex;justify-content:space-between;font-size:16px;opacity:.8;margin-top:6px}
.ph-thumbs{display:flex;gap:8px;overflow-x:auto;padding:10px 0;margin-top:8px}
.ph-thumb{position:relative;flex:0 0 auto;width:88px;height:88px;border-radius:8px;overflow:hidden;border:2px solid transparent;cursor:pointer;background:#000}
.ph-thumb.ph-sel{border-color:var(--md-primary-fg-color,#1976d2)}
.ph-thumb img{width:100%;height:100%;object-fit:cover;display:block}
.ph-thumb .ph-rm{position:absolute;top:2px;right:2px;min-width:44px;min-height:44px;width:44px;height:44px;border-radius:50%;border:none;background:rgba(0,0,0,.7);color:#fff;font-size:20px;line-height:1;cursor:pointer;display:flex;align-items:center;justify-content:center}
.ph-info,.ph-ppi{margin-top:12px;padding:12px;border-radius:12px;background:rgba(127,127,127,.06);border:1px solid rgba(127,127,127,.25)}
.ph-row{display:flex;justify-content:space-between;gap:12px;align-items:baseline;flex-wrap:wrap;font-size:16px;margin:4px 0}
.ph-key{opacity:.7}
.ph-val{font-family:ui-monospace,monospace;text-align:right;word-break:break-all}
.ph-ppi label{display:flex;align-items:center;gap:8px;font-size:16px;flex-wrap:wrap}
.ph-ppi input[type=number]{min-height:44px;padding:6px 10px;border-radius:8px;border:1px solid rgba(127,127,127,.4);background:transparent;color:inherit;font-size:16px;width:90px}
.ph-hint{font-size:16px;margin-top:10px;opacity:.85}
</style>

<div class="ph-drop" id="phDrop">
  <input type="file" id="phInput" accept="image/*" multiple>
  <input type="file" id="phCam" accept="image/*" capture="environment">
  <label for="phCam" class="ph-choose">Take photo</label>
  <label for="phInput" class="ph-choose">Choose existing</label>
  <p class="ph-drop-hint">Shoot with this phone's camera, or drop a photo here.</p>
</div>

<p class="ph-error" id="phError" role="alert" hidden></p>

<div class="ph-stage" id="phStage">
  <img class="ph-layer ph-base" id="phBase" alt="">
  <img class="ph-layer ph-top" id="phTop" alt="" style="clip-path:inset(0 50% 0 0);">
  <div class="ph-empty" id="phEmpty">No photo yet</div>
</div>

<div class="ph-toolbar">
  <button type="button" class="ph-btn" id="phModeBtn">View: fit</button>
  <button type="button" class="ph-btn" id="phCompareBtn">Compare: off</button>
  <button type="button" class="ph-btn" id="phClearBtn">Clear all</button>
</div>

<div class="ph-compare" id="phComparePanel" hidden>
  <label class="ph-slider-label" for="phSlider">Wipe <span id="phSliderVal">50</span>%</label>
  <input type="range" id="phSlider" min="0" max="100" value="50">
  <div class="ph-ab">
    <span>A = older photo</span>
    <span>B = newer photo</span>
  </div>
</div>

<div class="ph-thumbs" id="phThumbs"></div>

<div class="ph-info">
  <div class="ph-row"><span class="ph-key">Dimensions</span><span class="ph-val" id="phDims">-</span></div>
  <div class="ph-row"><span class="ph-key">Megapixels</span><span class="ph-val" id="phMp">-</span></div>
  <div class="ph-row"><span class="ph-key">Aspect ratio</span><span class="ph-val" id="phAspect">-</span></div>
  <div class="ph-row"><span class="ph-key">File size</span><span class="ph-val" id="phSize">-</span></div>
</div>

<div class="ph-ppi">
  <label>Screen diagonal (inches) <input type="number" id="phDiag" value="6.1" min="3" max="10" step="0.1"></label>
  <div class="ph-row"><span class="ph-key">PPI (if this photo fills the panel)</span><span class="ph-val" id="phPpi">-</span></div>
</div>

<p class="ph-hint">Orientation: <span id="phOrient">-</span>. The browser applies your phone's rotation automatically; EXIF metadata is not read here.</p>

<script>
(function(){
  var $ = function(id){ return document.getElementById(id); };
  var drop = $('phDrop');
  var input = $('phInput');
  var cam = $('phCam');
  var errEl = $('phError');
  var stage = $('phStage');
  var base = $('phBase');
  var top = $('phTop');
  var empty = $('phEmpty');
  var modeBtn = $('phModeBtn');
  var compareBtn = $('phCompareBtn');
  var clearBtn = $('phClearBtn');
  var comparePanel = $('phComparePanel');
  var slider = $('phSlider');
  var sliderVal = $('phSliderVal');
  var thumbs = $('phThumbs');
  var dims = $('phDims');
  var mp = $('phMp');
  var aspect = $('phAspect');
  var size = $('phSize');
  var ppi = $('phPpi');
  var diag = $('phDiag');
  var orient = $('phOrient');

  var photos = [];
  var selectedId = null;
  var mode = 'fit';
  var compare = false;
  var panX = 0, panY = 0;
  var dragging = false, sx = 0, sy = 0, spx = 0, spy = 0;

  function uid(){ return 'p' + Date.now().toString(36) + Math.random().toString(36).slice(2); }
  function gcd(a, b){ a = Math.abs(a); b = Math.abs(b); while(b){ var t = a % b; a = b; b = t; } return a || 1; }

  function showError(msg){ errEl.textContent = msg; errEl.hidden = false; }
  function clearError(){ errEl.hidden = true; errEl.textContent = ''; }

  function loadImg(url){ return new Promise(function(resolve, reject){ var im = new Image(); im.onload = function(){ resolve(im); }; im.onerror = function(){ reject(); }; im.src = url; }); }

  async function addFiles(fileList){
    clearError();
    var errs = [];
    var files = Array.from(fileList);
    for(var i = 0; i < files.length; i++){
      var f = files[i];
      var url = URL.createObjectURL(f);
      try {
        var im = await loadImg(url);
        if(!im.naturalWidth || !im.naturalHeight) throw new Error('bad');
        photos.push({ id: uid(), url: url, file: f, name: f.name, size: f.size, w: im.naturalWidth, h: im.naturalHeight });
      } catch(e){
        URL.revokeObjectURL(url);
        errs.push(f.name);
      }
    }
    if(errs.length) showError('Could not decode as an image: ' + errs.join(', '));
    if(photos.length){ selectedId = photos[photos.length - 1].id; }
    render();
  }

  function removePhoto(id){
    var idx = photos.findIndex(function(p){ return p.id === id; });
    if(idx < 0) return;
    URL.revokeObjectURL(photos[idx].url);
    photos.splice(idx, 1);
    if(selectedId === id){ selectedId = photos.length ? photos[photos.length - 1].id : null; }
    if(compare && photos.length < 2) compare = false;
    render();
  }

  function clearAll(){
    photos.forEach(function(p){ URL.revokeObjectURL(p.url); });
    photos = [];
    selectedId = null;
    compare = false;
    render();
  }

  function selectPhoto(id){ selectedId = id; if(mode === 'one'){ panX = 0; panY = 0; } render(); }

  function resetPan(){ panX = 0; panY = 0; }

  function applyPanClamp(){
    var sel = photos.find(function(p){ return p.id === selectedId; });
    if(!sel) return;
    var r = stage.getBoundingClientRect();
    panX = Math.max(-(sel.w - 50), Math.min(r.width - 50, panX));
    panY = Math.max(-(sel.h - 50), Math.min(r.height - 50, panY));
  }

  function applyMode(){
    if(mode === 'fit'){
      base.classList.remove('ph-one');
      base.style.width = '';
      base.style.height = '';
      base.style.transform = '';
      stage.style.cursor = 'default';
      modeBtn.textContent = 'View: fit';
    } else {
      var sel = photos.find(function(p){ return p.id === selectedId; });
      if(!sel) return;
      base.classList.add('ph-one');
      base.style.width = sel.w + 'px';
      base.style.height = sel.h + 'px';
      base.style.transform = 'translate(' + panX + 'px,' + panY + 'px)';
      stage.style.cursor = 'grab';
      modeBtn.textContent = 'View: 1:1';
    }
  }

  function updateClip(){ top.style.clipPath = 'inset(0 ' + (100 - Number(slider.value)) + '% 0 0)'; }

  function render(){
    if(compare && photos.length >= 2){
      mode = 'fit';
      modeBtn.textContent = 'View: fit';
      var a = photos[photos.length - 2];
      var b = photos[photos.length - 1];
      base.src = a.url;
      top.src = b.url;
      base.classList.remove('ph-one');
      top.classList.remove('ph-one');
      base.style.transform = '';
      top.style.transform = '';
      base.style.width = '';
      base.style.height = '';
      top.style.width = '';
      top.style.height = '';
      base.classList.add('ph-active');
      top.classList.add('ph-active');
      empty.style.display = 'none';
      stage.style.cursor = 'default';
      comparePanel.hidden = false;
      updateClip();
    } else {
      comparePanel.hidden = true;
      top.classList.remove('ph-active');
      top.removeAttribute('src');
      var sel = photos.find(function(p){ return p.id === selectedId; });
      if(sel){
        base.src = sel.url;
        base.classList.add('ph-active');
        empty.style.display = 'none';
        applyMode();
      } else {
        base.classList.remove('ph-active');
        base.removeAttribute('src');
        empty.style.display = 'flex';
      }
    }
    compareBtn.textContent = 'Compare: ' + (compare ? 'on' : 'off');

    thumbs.innerHTML = '';
    photos.forEach(function(p){
      var wrap = document.createElement('div');
      wrap.className = 'ph-thumb' + (p.id === selectedId ? ' ph-sel' : '');
      var img = document.createElement('img');
      img.src = p.url; img.alt = '';
      wrap.appendChild(img);
      var rm = document.createElement('button');
      rm.type = 'button';
      rm.className = 'ph-rm';
      rm.textContent = '\u00D7';
      rm.setAttribute('aria-label', 'Remove');
      rm.addEventListener('click', function(e){ e.stopPropagation(); removePhoto(p.id); });
      wrap.appendChild(rm);
      wrap.addEventListener('click', function(){ selectPhoto(p.id); });
      thumbs.appendChild(wrap);
    });

    updateInfo();
  }

  function updateInfo(){
    var sel = photos.find(function(p){ return p.id === selectedId; });
    if(!sel){
      dims.textContent = '-';
      mp.textContent = '-';
      aspect.textContent = '-';
      size.textContent = '-';
      orient.textContent = '-';
      ppi.textContent = '-';
      return;
    }
    dims.textContent = sel.w + ' x ' + sel.h;
    mp.textContent = (sel.w * sel.h / 1e6).toFixed(1) + ' MP';
    var g = gcd(sel.w, sel.h);
    aspect.textContent = (sel.w / g) + ':' + (sel.h / g);
    size.textContent = (sel.size / 1048576).toFixed(2) + ' MB';
    if(sel.w > sel.h) orient.textContent = 'landscape';
    else if(sel.h > sel.w) orient.textContent = 'portrait';
    else orient.textContent = 'square';
    var d = parseFloat(diag.value);
    if(d > 0){
      var pp = Math.sqrt(sel.w * sel.w + sel.h * sel.h) / d;
      ppi.textContent = Math.round(pp) + ' PPI';
    } else {
      ppi.textContent = '-';
    }
  }

  modeBtn.addEventListener('click', function(){
    if(compare) compare = false;
    mode = (mode === 'fit') ? 'one' : 'fit';
    if(mode === 'one'){ panX = 0; panY = 0; }
    render();
  });

  compareBtn.addEventListener('click', function(){
    if(photos.length < 2){ showError('Need at least two photos to compare.'); return; }
    compare = !compare;
    if(compare) mode = 'fit';
    render();
  });

  clearBtn.addEventListener('click', clearAll);

  slider.addEventListener('input', function(){ sliderVal.textContent = slider.value; updateClip(); });

  diag.addEventListener('input', updateInfo);

  drop.addEventListener('dragover', function(e){ e.preventDefault(); drop.classList.add('ph-dragover'); });
  drop.addEventListener('dragleave', function(){ drop.classList.remove('ph-dragover'); });
  drop.addEventListener('drop', function(e){ e.preventDefault(); drop.classList.remove('ph-dragover'); if(e.dataTransfer && e.dataTransfer.files) addFiles(e.dataTransfer.files); });
  input.addEventListener('change', function(){ if(input.files && input.files.length) addFiles(input.files); input.value = ''; });
  cam.addEventListener('change', function(){ if(cam.files && cam.files.length) addFiles(cam.files); cam.value = ''; });

  stage.addEventListener('pointerdown', function(e){
    if(mode !== 'one' || compare) return;
    dragging = true;
    try { stage.setPointerCapture(e.pointerId); } catch(err){}
    sx = e.clientX; sy = e.clientY; spx = panX; spy = panY;
    stage.style.cursor = 'grabbing';
  });
  stage.addEventListener('pointermove', function(e){
    if(!dragging) return;
    panX = spx + (e.clientX - sx);
    panY = spy + (e.clientY - sy);
    applyPanClamp();
    base.style.transform = 'translate(' + panX + 'px,' + panY + 'px)';
  });
  stage.addEventListener('pointerup', function(e){
    if(!dragging) return;
    dragging = false;
    try { stage.releasePointerCapture(e.pointerId); } catch(err){}
    stage.style.cursor = 'grab';
  });
  stage.addEventListener('pointercancel', function(){ dragging = false; stage.style.cursor = 'grab'; });

  render();
})();
</script>
</div>