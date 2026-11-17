<div class="vi-root">
<style>
.vi-root{box-sizing:border-box;font-size:16px;line-height:1.45}
.vi-root *{box-sizing:inherit}
.vi-root .vi-card{background:var(--md-default-bg-color,#1a1a1a);border:1px solid rgba(127,127,127,.25);border-radius:14px;padding:14px;margin:0 0 16px}
.vi-root .vi-h2{font-size:1.25rem;margin:0 0 10px;font-weight:700}
.vi-root .vi-note{font-size:14px;opacity:.85;margin:8px 0 0}
.vi-root .vi-chips{display:flex;flex-wrap:wrap;gap:8px;margin:10px 0}
.vi-root .vi-chip{min-height:44px;padding:10px 14px;border-radius:999px;border:1px solid currentColor;background:transparent;color:inherit;font-size:16px;cursor:pointer}
.vi-root .vi-chip.is-active{background:var(--md-primary-fg-color,#2196f3);color:#fff;border-color:transparent}
.vi-root .vi-player-wrap{position:relative;width:100%;aspect-ratio:16/9;background:#000;border-radius:10px;overflow:hidden}
.vi-root .vi-video{width:100%;height:100%;display:block;object-fit:contain;background:#000}
.vi-root .vi-fs{position:absolute;right:8px;bottom:8px;min-width:44px;min-height:44px;border-radius:8px;border:none;background:rgba(0,0,0,.65);color:#fff;font-size:16px;cursor:pointer}
.vi-root .vi-badge{display:inline-block;margin-top:10px;padding:8px 12px;border-radius:8px;background:rgba(127,127,127,.18);font-size:16px;font-weight:600}
.vi-root .vi-yt-wrap{position:relative;width:100%;aspect-ratio:16/9;border-radius:10px;overflow:hidden;background:#000}
.vi-root .vi-yt-wrap iframe{position:absolute;inset:0;width:100%;height:100%;border:0}
.vi-root .vi-detail-wrap{width:100%;max-height:60vh;overflow:auto;border-radius:10px;background:#111;border:1px solid rgba(127,127,127,.25);cursor:pointer}
.vi-root .vi-detail-canvas{display:block;max-width:100%;height:auto}
.vi-root .vi-detail-canvas.vi-one{max-width:none}
.vi-root .vi-rowlabel{font-size:13px;opacity:.8;margin:10px 0 4px;font-weight:600}
.vi-root .vi-strip{display:flex;gap:6px;overflow-x:auto;padding:4px 0}
.vi-root .vi-patchcol{flex:0 0 48px;display:flex;flex-direction:column;align-items:center;gap:2px}
.vi-root .vi-patch{width:48px;height:48px;border-radius:6px;border:1px solid rgba(127,127,127,.3)}
.vi-root .vi-patchlabel{font-size:11px}
</style>
<section class="vi-card">
<h2 class="vi-h2">Play in 4K</h2>
<div class="vi-chips">
<button type="button" class="vi-chip is-active" data-src="https://videos.pexels.com/video-files/2098989/2098989-uhd_3840_2160_30fps.mp4" data-size="108">Clip 1 · 108 MB</button>
<button type="button" class="vi-chip" data-src="https://videos.pexels.com/video-files/3571264/3571264-uhd_3840_2160_30fps.mp4" data-size="53">Clip 2 · 53 MB</button>
<button type="button" class="vi-chip" data-src="https://videos.pexels.com/video-files/3129957/3129957-uhd_3840_2160_25fps.mp4" data-size="73">Clip 3 · 73 MB</button>
</div>
<div class="vi-player-wrap" id="vi-player-wrap">
<video id="vi-video" class="vi-video" preload="none" playsinline controls loop poster='data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="320" height="180"><rect width="100%" height="100%" fill="%23111"/><text x="50%" y="50%" dominant-baseline="middle" text-anchor="middle" fill="%23fff" font-size="48" font-family="sans-serif">4K</text></svg>' src="https://videos.pexels.com/video-files/2098989/2098989-uhd_3840_2160_30fps.mp4"></video>
<button type="button" class="vi-fs" id="vi-fs" aria-label="Enter fullscreen">Full</button>
</div>
<p class="vi-note">Tap a chip to swap clips. Data cost per clip: 108 MB, 53 MB, 73 MB.</p>
<div class="vi-badge" id="vi-badge">No metadata yet — tap play.</div>
</section>
<section class="vi-card">
<h2 class="vi-h2">Costa Rica in 4K 60fps HDR</h2>
<div class="vi-yt-wrap">
<iframe src="https://www.youtube-nocookie.com/embed/LXb3EKWsInQ?rel=0" title="Costa Rica in 4K 60fps HDR" loading="lazy" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>
<p class="vi-note">Mobile web players often cap at 1080p, so use the direct MP4 above when you need real 4K, and open the YouTube app for HDR.</p>
</section>
<section class="vi-card">
<h2 class="vi-h2">Detail check at 4K</h2>
<div class="vi-detail-wrap" id="vi-detail-wrap">
<canvas class="vi-detail-canvas" id="vi-detail-canvas" width="3840" height="2160"></canvas>
</div>
<button type="button" class="vi-chip" id="vi-detail-toggle">Fit to screen (scaled)</button>
<p class="vi-note">Tap the image or button to toggle 1:1 pixels. Fit = scaled to your screen. 1:1 = one image pixel per screen pixel.</p>
</section>
<section class="vi-card">
<h2 class="vi-h2">Brightness and contrast</h2>
<p class="vi-note">Each patch is a single value. If the first dark patches or last light patches look identical, the panel crushes shadows or clips highlights.</p>
<div class="vi-rowlabel">Dark end (%)</div>
<div class="vi-strip" id="vi-dark"></div>
<div class="vi-rowlabel">Light end (%)</div>
<div class="vi-strip" id="vi-light"></div>
</section>
<script>
(function(){
var video=document.getElementById('vi-video');
var wrap=document.getElementById('vi-player-wrap');
var badge=document.getElementById('vi-badge');
var chips=Array.prototype.slice.call(document.querySelectorAll('.vi-chip[data-src]'));
function badgeText(){
var res=(video.videoWidth && video.videoHeight)?(video.videoWidth+' x '+video.videoHeight):'no metadata yet';
var fs=(document.fullscreenElement===wrap)||(document.fullscreenElement && wrap.contains(document.fullscreenElement));
badge.textContent=res+' · '+(fs?'fullscreen':'not fullscreen');
}
video.addEventListener('loadedmetadata',badgeText);
document.addEventListener('fullscreenchange',badgeText);
document.addEventListener('webkitfullscreenchange',badgeText);
chips.forEach(function(c){
c.addEventListener('click',function(){
if(c.classList.contains('is-active'))return;
chips.forEach(function(x){x.classList.remove('is-active');});
c.classList.add('is-active');
video.pause();
video.src=c.getAttribute('data-src');
video.load();
badgeText();
});
});
var fsBtn=document.getElementById('vi-fs');
var canFull=!!(wrap.requestFullscreen||wrap.webkitRequestFullscreen||video.webkitEnterFullscreen||video.requestFullscreen);
if(!canFull){fsBtn.disabled=true;fsBtn.title='Fullscreen not supported by this browser';}
fsBtn.addEventListener('click',function(){
if(wrap.requestFullscreen){wrap.requestFullscreen().catch(function(){});}
else if(wrap.webkitRequestFullscreen){wrap.webkitRequestFullscreen();}
else if(video.webkitEnterFullscreen){video.webkitEnterFullscreen();}
else if(video.requestFullscreen){video.requestFullscreen().catch(function(){});}
});
var canvas=document.getElementById('vi-detail-canvas');
var detailWrap=document.getElementById('vi-detail-wrap');
var toggle=document.getElementById('vi-detail-toggle');
var fit=true;
function drawPattern(){
var w=canvas.width,h=canvas.height;
var ctx=canvas.getContext('2d');
ctx.fillStyle='#ffffff';
ctx.fillRect(0,0,w,h);
ctx.fillStyle='#000000';
for(var x=0;x<w;x+=2){ctx.fillRect(x,0,1,h);}
var cx=w/2,cy=h/2,r=Math.min(w,h)*0.35;
if(typeof ctx.createConicGradient==='function'){
var grad=ctx.createConicGradient(0,cx,cy);
var spokes=144;
for(var i=0;i<spokes;i++){
var col=(i%2===0)?'#000000':'#ffffff';
grad.addColorStop(i/spokes,col);
grad.addColorStop((i+1)/spokes,col);
}
ctx.save();
ctx.beginPath();
ctx.arc(cx,cy,r,0,Math.PI*2);
ctx.clip();
ctx.fillStyle=grad;
ctx.fillRect(cx-r,cy-r,r*2,r*2);
ctx.restore();
}else{
ctx.fillStyle='#888888';
ctx.font='bold 96px sans-serif';
ctx.textAlign='center';
ctx.textBaseline='middle';
ctx.fillText('Conic gradients unsupported',cx,cy);
}
ctx.fillStyle='#000000';
for(var i=0;i<60;i++){
var barH=Math.max(1,12-Math.floor(i/5));
ctx.fillStyle=(i%2===0)?'#000000':'#ffffff';
ctx.fillRect(0,cy+r+40+i*14,w,barH);
}
}
setTimeout(drawPattern,0);
function applyDetail(){
if(fit){
canvas.classList.remove('vi-one');
canvas.style.width='100%';
canvas.style.height='auto';
toggle.textContent='Fit to screen (scaled)';
}else{
canvas.classList.add('vi-one');
canvas.style.width=canvas.width+'px';
canvas.style.height=canvas.height+'px';
toggle.textContent='1:1 pixels (scroll to inspect)';
}
}
function flip(){fit=!fit;applyDetail();}
toggle.addEventListener('click',flip);
detailWrap.addEventListener('click',flip);
applyDetail();
function makeStrip(el,values){
values.forEach(function(v){
var col=document.createElement('div');
col.className='vi-patchcol';
var patch=document.createElement('div');
patch.className='vi-patch';
var c=Math.round(255*v/100);
patch.style.background='rgb('+c+','+c+','+c+')';
var label=document.createElement('span');
label.className='vi-patchlabel';
label.textContent=v+'%';
col.appendChild(patch);
col.appendChild(label);
el.appendChild(col);
});
}
makeStrip(document.getElementById('vi-dark'),[0,1,2,3,4,5,6]);
makeStrip(document.getElementById('vi-light'),[94,95,96,97,98,99,100]);
})();
</script>
</div>