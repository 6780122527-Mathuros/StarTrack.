
<html lang="th">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>StarTrack DEMO</title>
<style>
/* --- Styles จากต้นฉบับ (ปรับเล็กน้อย) --- */
body {
  font-family: 'Sarabun', Arial, sans-serif;
  background: linear-gradient(135deg,#f4eaff,#d3ecfd);
  color: #444;
  margin: 0;
}
header {
  text-align: center;
  background: #fcecfb;
  border-bottom:2px solid #e5d9f7;
  padding-top:1.7em; padding-bottom:.3em;
}
h1 { color: #a645ae; margin:1.5em 0 .1em 0;}
nav { text-align:center; padding:1.1em; background:#f2f7fd; position:relative;}
.rolebtn {
  background:#e9dfff;
  color: #86398e;
  font-size:1.03em;
  border:none;
  border-radius:11px;
  padding:.6em 1.4em;
  margin:.4em;
  cursor:pointer;
}
.rolebtn:hover {background: #e4e5ff;}
section {
  max-width: 930px;
  margin: 2em auto;
  background: #fffefe;
  border-radius: 23px;
  padding:2em 2.2em;
  box-shadow: 0 4px 25px #e4eaf4cc;
}
h2 {
  color:#a645ae;
  margin-top:0;
  letter-spacing:.03em;
}
.box {
  background: #f7f9fd;
  border-radius: 15px;
  padding:1.35em 2em;
  margin-bottom:2em;
  box-shadow:0 1px 18px #e7e1fa60;
}
.box h3 {color:#7193a6;}
.star { color: #ffe780; font-size:1.2em;}
.good {color:#399d2f;}
.bad {color:#d04a6a;}
.stats-table, .stats-table th, .stats-table td {
  border: 1px solid #ccc;
  border-collapse: collapse;
  padding: .34em .65em;
}
.stats-table {
  margin-top: .7em;
  width: 100%;
  background: #f2f7fd;
}
.stats-table th { background: #f9e3ff;}
.emotion-btns button {
  margin:.4em .13em;
  font-size:1.22em;
  padding:.19em .44em;
  border-radius:50%;
  border: 1.3px solid #b6c5ee;
  background:#e6f6ff;
  cursor:pointer;
}
.emotion-btns button.selected,
.emotion-btns button:hover {
  background:#ffd7ef;
  border-color:#bb5ecf;
}
textarea, select, input[type="text"], input[type="number"], input[type="datetime-local"], input[type="date"] {
  width: 100%;
  padding:.7em;
  margin:.13em 0;
  border-radius: 7px;
  border: 1.25px solid #d3ecfd;
  background: #fff6f8;
}
.btn-main, .test-btn, .appoint-btn {
  background: #a651b1;
  color: #fff;
  border: none;
  border-radius: 9px;
  font-weight:bold;
  font-size:1.07em;
  padding:.7em 2em;
  margin:.8em 0;
  box-shadow:0 1px 7px #ede0fb40;
  cursor:pointer;
}
.btn-main:hover, .test-btn:hover, .appoint-btn:hover {background:#e6c6f5; color:#640a73;}
.test-btn{background:#b1e5e0;color:#446e67; margin:0 .7em 0 0;}
.appoint-btn{background:#ffc285;color:#764108;}
.diary-list {margin:1em 0;}
.diary-entry {
  background: #f3f1fc;
  border-radius:10px;
  margin-bottom:.5em;
  padding:.7em 1em;
  position:relative;
}
.diary-date {font-size:.93em; color:#ae8cc1;display:block;}
.diary-del-btn {
  position:absolute;
  right:.7em;
  top:.6em;
  background: #ffd7ef;
  color: #d13d84;
  border-radius: 6px;
  padding: .08em .7em;
  border: none;
  cursor:pointer;
}
.reward-box {
  background: #e6f6ff;
  border-radius:11px;
  padding:1em 1em;
  margin:1em 0;
  color:#608498;
}
.test-res {
  margin:1em 0;
  background: #d9fff5;
  color:#575;
  border-radius:.7em;
  padding:1em;
}
.piebox {text-align:center; background:#f2f7fd; border-radius:12px;margin:1em 0; padding:1em;}
.historylist {
  background:#e6f6ff;
  border-radius:10px;
  padding:.8em;
}
.msgbox {
  background:#f1ffea;
  padding:1em 1.5em;
  border-radius:10px;
  margin-top:.9em;
}
.msg-entry {margin-bottom:.68em;}
.logout-btn {
  background: #e67c96;
  color: #fff;
  border: none;
  border-radius: 8px;
  padding:.7em 1.3em;
  font-weight:bold;
}
@media (max-width:600px){
  section,.box{padding:.6em .2em;}
  .piebox,canvas{width:100%!important;}
  nav{padding:0;}
}
.small {font-size:.92em;color:#6b6b6b;}
.header-actions {position:absolute; right:1rem; top:1rem;}
</style>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
<header>
  <h1>StarTrack DEMO</h1>
  <div style="color: #a14f88;">ระบบติดตามอารมณ์และดาวเด็กดี</div>
</header>
<nav>
  <button class="rolebtn" onclick="switchRole('student')">👦 นักเรียน</button>
  <button class="rolebtn" onclick="switchRole('teacher')">👩‍🏫 ครู</button>
  <button class="rolebtn" onclick="switchRole('admin')">🏫 ผู้บริหาร</button>
  <div class="header-actions">
    <button class="btn-main" onclick="location.reload()">ออกจากระบบ</button>
  </div>
</nav>

<!-- STUDENT SECTION -->
<section id="student-section" style="display:none">
  <h2>นักเรียน — บันทึกอารมณ์</h2>
  <div class="box">
    <label class="small">ชื่อเด็ก (ใช้เพื่อเก็บแยกไดอารี่):</label>
    <input id="student-name" type="text" placeholder="เช่น น้องมินท์">
    <div style="margin-top:.6em;">
      <label class="small">เลือกอารมณ์:</label>
      <div class="emotion-btns" id="emotion-buttons" style="margin-top:.4em;">
        <button data-emotion="ยิ้ม">😊</button>
        <button data-emotion="สุข">😄</button>
        <button data-emotion="ปกติ">😐</button>
        <button data-emotion="เศร้า">😢</button>
        <button data-emotion="โกรธ">😠</button>
        <button data-emotion="ตื่นเต้น">🤩</button>
      </div>
    </div>

    <label style="margin-top:.6em;" class="small">บันทึก (ไม่บังคับ):</label>
    <textarea id="student-note" rows="3" placeholder="อยากเล่าอะไรให้ครูฟังไหม?"></textarea>
    <label class="small">วันที่:</label>
    <input id="student-date" type="date">
    <div>
      <button class="btn-main" onclick="saveStudentEntry()">บันทึก</button>
    </div>

    <div class="box" style="margin-top:1em;">
      <h3>ไดอารี่ของฉัน</h3>
      <div id="diary-list" class="diary-list"></div>
    </div>
  </div>
</section>

<!-- TEACHER SECTION -->
<section id="teacher-section" style="display:none">
  <h2>ครู — จัดการนักเรียน</h2>
  <div class="box">
    <h3>รายชื่อนักเรียน</h3>
    <div id="student-list" class="historylist"></div>
    <div style="margin-top:1em;">
      <label class="small">เลือกนักเรียนเพื่อดูไดอารี่:</label>
      <select id="teacher-select-student" onchange="renderTeacherEntries()">
        <option value="">-- เลือก --</option>
      </select>
    </div>

    <div id="teacher-entries" style="margin-top:1em;"></div>

    <div style="margin-top:1em;">
      <h3>ส่งข้อความถึงนักเรียน</h3>
      <textarea id="teacher-message" rows="2" placeholder="ข้อความสำหรับนักเรียน"></textarea>
      <button class="test-btn" onclick="teacherSendMessage()">ส่งข้อความ</button>
    </div>
  </div>
</section>

<!-- ADMIN SECTION -->
<section id="admin-section" style="display:none">
  <h2>ผู้บริหาร — สรุปสถิติ</h2>
  <div class="box">
    <h3>ภาพรวมอารมณ์</h3>
    <div class="piebox">
      <canvas id="emotionChart" width="300" height="200"></canvas>
    </div>
    <div>
      <h3>สรุปตัวเลข</h3>
      <table class="stats-table">
        <thead><tr><th>อารมณ์</th><th>จำนวน</th></tr></thead>
        <tbody id="admin-stats-body"></tbody>
      </table>
    </div>

    <div class="reward-box">
      <strong>นโยบายให้ดาว:</strong> กำหนดเกณฑ์การให้ดาวหรือรางวัลแล้วติดตามจากมุมมองครู (ทดลองโดยการกดดาวจากมุมมองครู)
    </div>
  </div>
</section>

<script>
/* Global data key และสีสำหรับ Pie Chart */
const STORAGE_KEY = 'startrack_entries_v1';
const CHART_COLORS = ["#b1e5e0", "#a651b1", "#ffd7ef", "#ffe780", "#bfffa5", "#8dd6ee"];
const EMOTION_LABELS = ["ยิ้ม","สุข","ปกติ","เศร้า","โกรธ","ตื่นเต้น"];

/* --- การสลับ role --- */
function switchRole(role){
  document.getElementById('student-section').style.display = role === 'student' ? 'block' : 'none';
  document.getElementById('teacher-section').style.display = role === 'teacher' ? 'block' : 'none';
  document.getElementById('admin-section').style.display = role === 'admin' ? 'block' : 'none';
  if(role === 'student') {
    renderDiaryList();
  } else if(role === 'teacher') {
    renderStudentListForTeacher();
  } else if(role === 'admin') {
    renderAdminStats();
  }
}

/* --- Storage helpers --- */
function loadEntries(){
  try {
    const raw = localStorage.getItem(STORAGE_KEY);
    return raw ? JSON.parse(raw) : [];
  } catch(e) {
    console.error('loadEntries error', e);
    return [];
  }
}
function saveEntries(entries){
  localStorage.setItem(STORAGE_KEY, JSON.stringify(entries));
}

/* --- Student: select emotion --- */
document.getElementById('emotion-buttons').addEventListener('click', function(e){
  if(e.target && e.target.matches('button[data-emotion]')){
    const btns = this.querySelectorAll('button');
    btns.forEach(b => b.classList.remove('selected'));
    e.target.classList.add('selected');
  }
});

/* --- Student: save entry --- */
function saveStudentEntry(){
  const name = document.getElementById('student-name').value.trim();
  if(!name){
    alert('กรุณากรอกชื่อเด็กก่อนบันทึก');
    return;
  }
  const note = document.getElementById('student-note').value.trim();
  const dateVal = document.getElementById('student-date').value || new Date().toISOString().slice(0,10);
  const selected = document.querySelector('#emotion-buttons button.selected');
  if(!selected){
    alert('กรุณาเลือกอารมณ์ก่อนบันทึก');
    return;
  }
  const emotion = selected.getAttribute('data-emotion');
  const entries = loadEntries();
  const entry = {
    id: 'e' + Date.now(),
    name,
    emotion,
    note,
    date: dateVal,
    createdAt: new Date().toISOString(),
    starred: false,
    messages: []
  };
  entries.push(entry);
  saveEntries(entries);
  // clear inputs (keep name)
  document.getElementById('student-note').value = '';
  selected.classList.remove('selected');
  document.getElementById('student-date').value = '';
  renderDiaryList();
  alert('บันทึกเรียบร้อยแล้ว');
}

/* --- Student: render diary filtered by name input --- */
function renderDiaryList(){
  const name = document.getElementById('student-name').value.trim();
  const container = document.getElementById('diary-list');
  container.innerHTML = '';
  if(!name) {
    container.innerHTML = '<div class="small">พิมพ์ชื่อเด็กด้านบนเพื่อดูไดอารี่ของเขา</div>';
    return;
  }
  const entries = loadEntries().filter(e => e.name === name).sort((a,b)=> b.createdAt.localeCompare(a.createdAt));
  if(entries.length === 0) {
    container.innerHTML = '<div class="small">ยังไม่มีบันทึกสำหรับชื่อนี้</div>';
    return;
  }
  entries.forEach(e=>{
    const div = document.createElement('div');
    div.className = 'diary-entry';
    div.innerHTML = `
      <strong>${e.emotion} ${e.starred ? '⭐' : ''}</strong>
      <span class="diary-date">${e.date} • ${new Date(e.createdAt).toLocaleString()}</span>
      <div style="margin-top:.4em;">${e.note ? escapeHtml(e.note) : '<span class="small">ไม่มีข้อความ</span>'}</div>
      <button class="diary-del-btn" onclick="deleteEntry('${e.id}')">ลบ</button>
    `;
    container.appendChild(div);
  });
}

/* --- Delete entry --- */
function deleteEntry(id){
  if(!confirm('คุณแน่ใจว่าต้องการลบบันทึกนี้?')) return;
  const entries = loadEntries().filter(e=> e.id !== id);
  saveEntries(entries);
  renderDiaryList();
  renderStudentListForTeacher();
  renderAdminStats();
}

/* --- Teacher: render student list (unique names) --- */
function renderStudentListForTeacher(){
  const entries = loadEntries();
  const names = Array.from(new Set(entries.map(e=>e.name))).sort();
  const listDiv = document.getElementById('student-list');
  const sel = document.getElementById('teacher-select-student');
  listDiv.innerHTML = '';
  sel.innerHTML = '<option value="">-- เลือก --</option>';
  if(names.length === 0){
    listDiv.innerHTML = '<div class="small">ยังไม่มีข้อมูลนักเรียน</div>';
    return;
  }
  names.forEach(n=>{
    const count = entries.filter(e=>e.name===n).length;
    const row = document.createElement('div');
    row.style.padding = '.4em 0';
    row.innerHTML = `<strong>${escapeHtml(n)}</strong> <span class="small">(${count} บันทึก)</span>
      <button style="margin-left:.6em;" onclick="quickStar('${escapeJs(n)}')">ให้ดาว</button>`;
    listDiv.appendChild(row);

    const opt = document.createElement('option');
    opt.value = n;
    opt.textContent = n;
    sel.appendChild(opt);
  });
}

/* --- Teacher: render entries for selected student --- */
function renderTeacherEntries(){
  const name = document.getElementById('teacher-select-student').value;
  const container = document.getElementById('teacher-entries');
  container.innerHTML = '';
  if(!name) return;
  const entries = loadEntries().filter(e => e.name === name).sort((a,b)=> b.createdAt.localeCompare(a.createdAt));
  if(entries.length === 0) {
    container.innerHTML = '<div class="small">ไม่มีบันทึก</div>';
    return;
  }
  entries.forEach(e=>{
    const div = document.createElement('div');
    div.className = 'diary-entry';
    let messagesHtml = '';
    if(e.messages && e.messages.length){
      messagesHtml = '<div class="msgbox"><strong>ข้อความ:</strong>';
      e.messages.forEach(m => messagesHtml += `<div class="msg-entry">• ${escapeHtml(m)}</div>`);
      messagesHtml += '</div>';
    }
    div.innerHTML = `
      <div style="display:flex;justify-content:space-between;align-items:center;">
        <div><strong>${e.emotion} ${e.starred ? '⭐' : ''}</strong><br/><span class="diary-date">${e.date}</span></div>
        <div>
          <button onclick="toggleStar('${e.id}')">⭐ ให้/ถอนดาว</button>
          <button onclick="replyToEntry('${e.id}')">ส่งข้อความ</button>
        </div>
      </div>
      <div style="margin-top:.5em;">${e.note ? escapeHtml(e.note) : '<span class="small">ไม่มีข้อความ</span>'}</div>
      ${messagesHtml}
    `;
    container.appendChild(div);
  });
}

/* --- Teacher: give star to all entries of a student quickly --- */
function quickStar(name){
  if(!confirm(`ให้ดาวทั้งหมดของ ${name} หรือไม่?`)) return;
  const entries = loadEntries().map(e=>{
    if(e.name === name) e.starred = true;
    return e;
  });
  saveEntries(entries);
  renderStudentListForTeacher();
  renderTeacherEntries();
  renderAdminStats();
}

/* --- Toggle star for one entry --- */
function toggleStar(id){
  const entries = loadEntries().map(e=>{
    if(e.id === id) e.starred = !e.starred;
    return e;
  });
  saveEntries(entries);
  renderTeacherEntries();
  renderDiaryList();
  renderAdminStats();
}

/* --- Teacher: reply to a single entry --- */
function replyToEntry(id){
  const msg = prompt('พิมพ์ข้อความถึงนักเรียน (ข้อความจะถูกแนบไปกับบันทึกนี้):');
  if(!msg) return;
  const entries = loadEntries().map(e=>{
    if(e.id === id){
      e.messages = e.messages || [];
      e.messages.push(msg);
    }
    return e;
  });
  saveEntries(entries);
  renderTeacherEntries();
}

/* --- Teacher: send message (general) --- */
function teacherSendMessage(){
  const sel = document.getElementById('teacher-select-student').value;
  const msg = document.getElementById('teacher-message').value.trim();
  if(!sel){ alert('เลือกนักเรียนก่อนส่งข้อความ'); return; }
  if(!msg){ alert('พิมพ์ข้อความก่อนส่ง'); return; }
  // attach to most recent entry of student, else create a system-message entry
  const entries = loadEntries();
  const studentEntries = entries.filter(e=> e.name === sel).sort((a,b)=> b.createdAt.localeCompare(a.createdAt));
  if(studentEntries.length > 0){
    const targetId = studentEntries[0].id;
    entries.forEach(e=>{ if(e.id === targetId){ e.messages = e.messages || []; e.messages.push(msg); }});
  } else {
    entries.push({
      id: 'sys' + Date.now(),
      name: sel,
      emotion: 'ข้อความจากครู',
      note: '',
      date: new Date().toISOString().slice(0,10),
      createdAt: new Date().toISOString(),
      starred: false,
      messages: [msg]
    });
  }
  saveEntries(entries);
  document.getElementById('teacher-message').value = '';
  alert('ส่งข้อความเรียบร้อย');
  renderTeacherEntries();
}

/* --- Admin: compute stats and render pie chart --- */
let emotionChart = null;
function renderAdminStats(){
  const entries = loadEntries();
  const counts = {};
  EMOTION_LABELS.forEach(l => counts[l] = 0);
  entries.forEach(e => {
    if(counts.hasOwnProperty(e.emotion)) counts[e.emotion] += 1;
    else counts[e.emotion] = (counts[e.emotion] || 0) + 1;
  });
  // build table
  const tbody = document.getElementById('admin-stats-body');
  tbody.innerHTML = '';
  const labels = [];
  const data = [];
  const colors = [];
  let i = 0;
  for(const label of Object.keys(counts)){
    labels.push(label);
    data.push(counts[label]);
    colors.push(CHART_COLORS[i % CHART_COLORS.length]);
    const tr = document.createElement('tr');
    tr.innerHTML = `<td>${escapeHtml(label)}</td><td>${counts[label]}</td>`;
    tbody.appendChild(tr);
    i++;
  }

  // render chart
  const ctx = document.getElementById('emotionChart').getContext('2d');
  if(emotionChart) emotionChart.destroy();
  emotionChart = new Chart(ctx, {
    type: 'pie',
    data: {
      labels,
      datasets: [{
        data,
        backgroundColor: colors,
        borderColor: '#fff'
      }]
    },
    options: {
      plugins: { legend: { position: 'bottom' } }
    }
  });
}

/* --- Utilities --- */
function escapeHtml(str){
  if(!str) return '';
  return String(str).replace(/[&<>"']/g, function(m){ return ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'})[m]; });
}
function escapeJs(str){
  return String(str).replace(/'/g,"\\'");
}

/* --- On load: setup sample data (optional) and initial render --- */
(function initDemo(){
  // If no data at all, seed a few demo entries for immediate visualization
  if(!localStorage.getItem(STORAGE_KEY)){
    const demo = [
      { id:'d1', name:'น้องมินท์', emotion:'ยิ้ม', note:'วันนี้ได้ดาวจากครู', date:'2025-11-20', createdAt:new Date().toISOString(), starred:true, messages:[] },
      { id:'d2', name:'น้องมินท์', emotion:'สุข', note:'สนุกที่ได้ทำกิจกรรม', date:'2025-11-21', createdAt:new Date().toISOString(), starred:false, messages:[] },
      { id:'d3', name:'น้องต้น', emotion:'เศร้า', note:'ไม่สบาย', date:'2025-11-22', createdAt:new Date().toISOString(), starred:false, messages:[] },
      { id:'d4', name:'น้องต้น', emotion:'ปกติ', note:'กลับมาเรียน', date:'2025-11-24', createdAt:new Date().toISOString(), starred:false, messages:[] },
      { id:'d5', name:'น้องจอย', emotion:'ตื่นเต้น', note:'แข่งกีฬา', date:'2025-11-25', createdAt:new Date().toISOString(), starred:true, messages:['เก่งมาก!'] }
    ];
    saveEntries(demo);
  }
  // default to student view
  switchRole('student');
  // populate teacher student list
  renderStudentListForTeacher();
  // admin chart ready
  renderAdminStats();
})();
</script>
</body>
</html>
