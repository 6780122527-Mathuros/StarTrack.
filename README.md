
<html lang="th">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Preview — AI Studio Link</title>
  <style>
    :root{
      --bg:#f4eaff;
      --card:#fffefe;
      --muted:#6b6b6b;
      --accent:#a645ae;
      --btn:#a651b1;
    }
    body{
      font-family: system-ui, "Sarabun", Arial, sans-serif;
      margin:0;
      min-height:100vh;
      background: linear-gradient(135deg,var(--bg),#d3ecfd);
      color:#222;
      display:flex;
      flex-direction:column;
      gap:1rem;
      padding:1rem;
      box-sizing:border-box;
    }
    header{ text-align:center; }
    h1{ margin:.2rem 0; color:var(--accent); font-size:1.25rem; }
    .container{
      max-width:1100px; margin:0 auto; width:100%;
      background:var(--card); border-radius:14px; padding:1rem; box-shadow:0 6px 24px rgba(0,0,0,0.06);
    }
    .controls{ display:flex; gap:.5rem; flex-wrap:wrap; align-items:center; }
    input[type="text"]{
      flex:1; min-width:260px;
      padding:.6rem .75rem; border-radius:8px; border:1px solid #dfe7f3; background:#fff;
    }
    button{ background:var(--btn); color:#fff; border:none; padding:.55rem .8rem; border-radius:8px; cursor:pointer; font-weight:600; }
    button.ghost{ background:transparent; color:var(--accent); border:1px solid #ecd8f6; }
    .small{ font-size:.88rem; color:var(--muted); }
    .preview-wrap{ margin-top:.9rem; display:flex; gap:1rem; flex-direction:column; }
    .frame-toolbar{ display:flex; gap:.5rem; align-items:center; flex-wrap:wrap; }
    .device-btn{ padding:.4rem .6rem; border-radius:8px; cursor:pointer; border:1px solid #eee; background:#fafafa; }
    .device-btn.active{ box-shadow:inset 0 -3px 0 rgba(0,0,0,0.04); border-color:#e0d0f1; background:#fff3ff; }
    .frame-box{ border-radius:12px; overflow:hidden; border:1px solid #e9e7f7; background:#fff; position:relative; }
    .frame{
      width:100%;
      height:640px;
      border:0;
      display:block;
      background:#fff;
    }
    .frame-notice{
      position:absolute; inset:0; display:flex; flex-direction:column; justify-content:center; align-items:center; gap:.6rem;
      padding:1rem; text-align:center; background:linear-gradient(180deg,rgba(255,255,255,.95),rgba(255,255,255,.97));
    }
    .hint{ font-size:.92rem; color:var(--muted); }
    .danger{ color:#b02a3b; font-weight:700; }
    .row{ display:flex; gap:.6rem; align-items:center; flex-wrap:wrap; }
    @media (min-width:900px){
      .preview-wrap{ flex-direction:row; }
      .frame-box{ flex:1; }
      .side{ width:320px; min-width:260px; }
    }
  </style>
</head>
<body>
  <header>
    <h1>Preview — ลิงก์ AI Studio</h1>
    <div class="small">ฝัง (embed) ลิงก์และดูตัวอย่างอย่างรวดเร็ว — ถ้าเว็บไซต์บล็อกการฝังจะแสดง fallback</div>
  </header>

  <div class="container" role="main">
    <div class="controls" aria-label="controls">
      <input id="urlInput" type="text" value="https://ai.studio/apps/drive/1_BMm4iQEzYc0XCqR5fCFsCFEZYN8L_rW" aria-label="URL to preview" />
      <button id="loadBtn">🔁 โหลดตัวอย่าง</button>
      <button id="openBtn" class="ghost">↗ เปิดในแท็บใหม่</button>
      <button id="copyBtn" class="ghost">📋 คัดลอกลิงก์</button>
      <div style="margin-left:auto" class="small">ถ้าไม่แสดง: หน้าอาจไม่อนุญาตให้ฝัง (X-Frame-Options)</div>
    </div>

    <div class="preview-wrap" style="margin-top:12px;">
      <div class="frame-box" id="frameBox" aria-live="polite">
        <div style="padding:.7rem .9rem; border-bottom:1px solid #f0ecfb; display:flex; justify-content:space-between; align-items:center;">
          <div class="row">
            <div class="small">Preview</div>
            <div id="currentSize" class="small" style="margin-left:.6rem; color:#444">Desktop • 100% width</div>
          </div>
          <div class="row">
            <button id="desktopBtn" class="device-btn active">💻 Desktop</button>
            <button id="tabletBtn" class="device-btn">📱 Tablet</button>
            <button id="mobileBtn" class="device-btn">📱 Mobile</button>
            <button id="reloadFrame" class="device-btn">🔄 Reload</button>
          </div>
        </div>

        <iframe id="previewFrame" class="frame" sandbox="allow-scripts allow-same-origin allow-forms allow-popups"></iframe>

        <!-- Notice overlay if cannot embed -->
        <div id="notice" class="frame-notice" style="display:none;">
          <div class="danger">ไม่สามารถฝังหน้าเว็บนี้ได้</div>
          <div class="hint">เว็บไซต์อาจตั้งค่าไม่ให้ฝังใน iframe (X-Frame-Options หรือ Content-Security-Policy)</div>
          <div class="row">
            <button id="openFallback" class="btn">เปิดในแท็บใหม่</button>
            <button id="tryProxy" class="ghost">ลองผ่าน CORS-proxy (ทดสอบ)</button>
          </div>
        </div>
      </div>

      <aside class="side" style="display:flex; flex-direction:column; gap:.8rem;">
        <div class="box" style="padding:.8rem; border-radius:10px; background:#fbf7ff;">
          <div style="font-weight:700; color:var(--accent); margin-bottom:.2rem;">ข้อมูลตัวอย่าง</div>
          <div class="small">URL:</div>
          <div id="metaUrl" class="small" style="word-break:break-all; margin-bottom:.5rem;"></div>
          <div class="small">สถานะ iframe:</div>
          <div id="metaStatus" class="small" style="font-weight:600; margin-bottom:.25rem;">-</div>
          <div class="small">คำแนะนำ:</div>
          <ul class="small" style="padding-left:1rem; margin-top:.3rem;">
            <li>ถ้าไม่แสดง ให้กด "เปิดในแท็บใหม่"</li>
            <li>ถ้าต้องการฝังจริงจัง อาจต้องใช้ reverse-proxy หรือให้เจ้าของไซต์อนุญาตการฝัง</li>
            <li>การจับภาพ (screenshot) ของ iframe ข้ามโดเมนจะไม่สามารถทำได้ด้วย JavaScript</li>
          </ul>
        </div>

        <div class="box" style="padding:.8rem; border-radius:10px; background:#f7fff6;">
          <div style="font-weight:700; color:#2b7a4b; margin-bottom:.4rem;">เครื่องมือเพิ่มเติม</div>
          <div style="display:flex; gap:.5rem; flex-wrap:wrap;">
            <button id="openInMobile" class="device-btn">เปิดขนาดมือถือ</button>
            <button id="openInTablet" class="device-btn">เปิดขนาดแท็บเล็ต</button>
            <button id="downloadHtml" class="ghost">บันทึกเป็น HTML (ดาวน์โหลด)</button>
          </div>
        </div>

      </aside>
    </div>
  </div>

  <script>
    const urlInput = document.getElementById('urlInput');
    const loadBtn = document.getElementById('loadBtn');
    const openBtn = document.getElementById('openBtn');
    const copyBtn = document.getElementById('copyBtn');
    const previewFrame = document.getElementById('previewFrame');
    const notice = document.getElementById('notice');
    const openFallback = document.getElementById('openFallback');
    const tryProxy = document.getElementById('tryProxy');
    const metaUrl = document.getElementById('metaUrl');
    const metaStatus = document.getElementById('metaStatus');
    const currentSize = document.getElementById('currentSize');
    const desktopBtn = document.getElementById('desktopBtn');
    const tabletBtn = document.getElementById('tabletBtn');
    const mobileBtn = document.getElementById('mobileBtn');
    const reloadFrame = document.getElementById('reloadFrame');
    const openInMobile = document.getElementById('openInMobile');
    const openInTablet = document.getElementById('openInTablet');
    const downloadHtml = document.getElementById('downloadHtml');

    // default URL (pre-filled)
    const defaultUrl = urlInput.value.trim();
    metaUrl.textContent = defaultUrl;

    let loadTimeout = null;
    const LOAD_TIMEOUT_MS = 3500; // ถ้าไม่ได้ onload ภายในเวลานี้ ให้ถือว่าไม่สามารถฝังได้

    function setFrameUrl(url){
      metaUrl.textContent = url;
      metaStatus.textContent = 'กำลังโหลด...';
      notice.style.display = 'none';
      // set src then wait for load event
      previewFrame.src = url;
      // clear previous timeout
      if(loadTimeout) clearTimeout(loadTimeout);
      loadTimeout = setTimeout(()=>{
        // หากยังไม่มี onload => แจ้งปัญหา (ฝังไม่ได้)
        // Some browsers may still fire load even if blocked; this heuristic is best-effort
        if(!previewFrame.dataset.loaded){
          metaStatus.textContent = 'อาจฝังไม่ได้';
          notice.style.display = 'flex';
        }
      }, LOAD_TIMEOUT_MS);
      // clear loaded flag
      previewFrame.dataset.loaded = "";
    }

    previewFrame.addEventListener('load', ()=>{
      // If the frame loads successfully, mark it.
      previewFrame.dataset.loaded = "1";
      metaStatus.textContent = 'โหลดสำเร็จ (ถ้าเนื้อหาข้ามโดเมนบางฟังก์ชันจะถูกจำกัด)';
      notice.style.display = 'none';
      if(loadTimeout) clearTimeout(loadTimeout);
    });

    previewFrame.addEventListener('error', ()=>{
      metaStatus.textContent = 'เกิดข้อผิดพลาดในการโหลด';
      notice.style.display = 'flex';
    });

    loadBtn.addEventListener('click', ()=> {
      const url = urlInput.value.trim();
      if(!url) return alert('กรุณาใส่ URL');
      // normalize URL (add https if missing)
      const normalized = normalizeUrl(url);
      urlInput.value = normalized;
      setFrameUrl(normalized);
    });

    openBtn.addEventListener('click', ()=> {
      const url = normalizeUrl(urlInput.value.trim());
      window.open(url, '_blank', 'noopener');
    });

    copyBtn.addEventListener('click', async ()=>{
      try {
        await navigator.clipboard.writeText(urlInput.value.trim());
        copyBtn.textContent = '✔️ คัดลอกแล้ว';
        setTimeout(()=> copyBtn.textContent = '📋 คัดลอกลิงก์', 1400);
      } catch(e){
        alert('ไม่สามารถคัดลอกอัตโนมัติ: ' + e.message);
      }
    });

    openFallback.addEventListener('click', ()=> {
      window.open(normalizeUrl(urlInput.value.trim()), '_blank', 'noopener');
    });

    tryProxy.addEventListener('click', ()=> {
      // Warning: public CORS proxies are unreliable. This is only for quick testing.
      const url = normalizeUrl(urlInput.value.trim());
      const proxy = prompt('กรุณาระบุ CORS proxy base URL หรือกดตกลงเพื่อใช้ demo proxy (ไม่รับประกันความปลอดภัย):', 'https://cors.bridged.cc/');
      if(!proxy) return;
      const proxied = proxy + url;
      // set frame to proxied URL
      setFrameUrl(proxied);
    });

    // device buttons
    function setDevice(kind){
      desktopBtn.classList.remove('active');
      tabletBtn.classList.remove('active');
      mobileBtn.classList.remove('active');
      if(kind === 'desktop'){
        desktopBtn.classList.add('active');
        previewFrame.style.width = '100%';
        previewFrame.style.height = '700px';
        currentSize.textContent = 'Desktop • 100% width';
      } else if(kind === 'tablet'){
        tabletBtn.classList.add('active');
        previewFrame.style.width = '900px';
        previewFrame.style.maxWidth = '100%';
        previewFrame.style.height = '800px';
        currentSize.textContent = 'Tablet • 900×800';
      } else {
        mobileBtn.classList.add('active');
        previewFrame.style.width = '420px';
        previewFrame.style.maxWidth = '100%';
        previewFrame.style.height = '800px';
        currentSize.textContent = 'Mobile • 420×800';
      }
    }
    desktopBtn.addEventListener('click', ()=> setDevice('desktop'));
    tabletBtn.addEventListener('click', ()=> setDevice('tablet'));
    mobileBtn.addEventListener('click', ()=> setDevice('mobile'));
    reloadFrame.addEventListener('click', ()=> {
      // reload iframe
      // if same-origin we could call frame.contentWindow.location.reload(), but use src trick:
      const src = previewFrame.src;
      previewFrame.src = 'about:blank';
      setTimeout(()=> previewFrame.src = src, 60);
    });

    openInMobile.addEventListener('click', ()=> {
      setDevice('mobile');
      const url = normalizeUrl(urlInput.value.trim());
      window.open(url, '_blank', 'noopener');
    });
    openInTablet.addEventListener('click', ()=> {
      setDevice('tablet');
      const url = normalizeUrl(urlInput.value.trim());
      window.open(url, '_blank', 'noopener');
    });

    downloadHtml.addEventListener('click', ()=> {
      const url = normalizeUrl(urlInput.value.trim());
      const blob = new Blob([`<!doctype html>
<html><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1"><title>Embedded preview</title></head>
<body style="margin:0">
<iframe src="${escapeHtmlAttr(url)}" style="width:100%;height:100vh;border:0"></iframe>
</body></html>`], {type:'text/html'});
      const a = document.createElement('a');
      a.href = URL.createObjectURL(blob);
      a.download = 'embed-preview.html';
      document.body.appendChild(a);
      a.click();
      a.remove();
    });

    // helper: add protocol if missing
    function normalizeUrl(u){
      if(!u) return u;
      try{
        const parsed = new URL(u, window.location.href);
        if(!parsed.protocol) parsed.protocol = 'https:';
        return parsed.toString();
      }catch(e){
        // if user provided without protocol, prefix https://
        if(!/^https?:\/\//i.test(u)) return 'https://' + u;
        return u;
      }
    }

    // escape for attribute
    function escapeHtmlAttr(s){
      if(!s) return '';
      return s.replace(/"/g, '&quot;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
    }

    // small html escape
    function escapeHtml(s){
      if(!s) return '';
      return s.replace(/[&<>"']/g, m => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'})[m]);
    }

    // On load: set default frame and device
    (function init(){
      const initial = urlInput.value.trim();
      setDevice('desktop');
      if(initial) setFrameUrl(initial);
    })();
  </script>
</body>
</html>
