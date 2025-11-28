
<html lang="th">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>StarTrack DEMO</title>

<!-- Use Tailwind Play CDN (supported) -->
<script src="https://cdn.tailwindcss.com"></script>

<!-- Google Font -->
<link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;500;600;700&display=swap" rel="stylesheet">

<!-- Chart.js stable build -->
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>

<style>
  body {
    font-family: 'Sarabun', Arial, sans-serif;
    background: linear-gradient(135deg,#f4eaff,#d3ecfd);
    color: #444;
  }
  /* Simple selected state for emotion buttons */
  .emotion {
    transition: transform .12s, box-shadow .12s;
  }
  .emotion.selected {
    transform: translateY(-4px) scale(1.08);
    box-shadow: 0 6px 18px rgba(99,102,241,0.18);
    outline: 3px solid rgba(167,139,250,0.18);
  }
</style>
</head>

<body class="pb-20">

<header class="text-center bg-pink-100 border-b-2 border-purple-200 py-6">
  <h1 class="text-4xl font-bold text-purple-600">StarTrack DEMO</h1>
  <p class="text-pink-700 text-lg">ระบบติดตามอารมณ์และดาวเด็กดี</p>
</header>

<nav class="text-center bg-blue-50 py-4 sticky top-0 shadow z-50">
  <button id="role-student" class="px-6 py-2 mx-1 rounded-lg bg-purple-100 hover:bg-purple-200">👦 นักเรียน</button>
  <button id="role-teacher" class="px-6 py-2 mx-1 rounded-lg bg-purple-100 hover:bg-purple-200">👩‍🏫 ครู</button>
  <button id="role-admin" class="px-6 py-2 mx-1 rounded-lg bg-purple-100 hover:bg-purple-200">🏫 ผู้บริหาร</button>
  <button id="logout" class="px-5 py-2 bg-red-400 text-white rounded-lg float-right">ออกจากระบบ</button>
</nav>

<!-- STUDENT -->
<section id="student-section" class="max-w-3xl mx-auto mt-8 hidden" aria-labelledby="student-heading">

  <div class="bg-white shadow-xl rounded-2xl p-6 mb-6">
    <h2 id="student-heading" class="text-2xl font-bold text-purple-700 mb-3">นักเรียน (เลือกบัญชี)</h2>

    <div class="mb-4">
      <label for="student-select" class="block text-sm font-medium text-gray-700 mb-1">เลือกนักเรียน</label>
      <select id="student-select" class="border rounded-lg p-2 w-48 bg-white"></select>
    </div>

    <h3 class="text-xl font-semibold text-purple-700 mb-2">เลือกอารมณ์วันนี้</h3>
    <div class="flex space-x-3 text-4xl" role="radiogroup" aria-label="เลือกอารมณ์">
      <button type="button" data-emotion="happy" class="emotion" aria-pressed="false">😄</button>
      <button type="button" data-emotion="normal" class="emotion" aria-pressed="false">😐</button>
      <button type="button" data-emotion="sad" class="emotion" aria-pressed="false">😢</button>
      <button type="button" data-emotion="angry" class="emotion" aria-pressed="false">😡</button>
      <button type="button" data-emotion="surprise" class="emotion" aria-pressed="false">😲</button>
    </div>

    <div class="mt-4 flex items-center space-x-3">
      <button id="save-emotion-btn" class="px-5 py-2 bg-purple-600 text-white rounded-lg">บันทึกอารมณ์</button>
      <p id="emotion-msg" class="mt-0 text-green-600" role="status" aria-live="polite"></p>
    </div>
  </div>

  <div class="bg-white shadow-xl rounded-2xl p-6 mb-6">
    <h2 class="text-2xl font-bold text-purple-700 mb-3">ไดอารี่ประจำวัน</h2>
    <textarea id="diary-input" class="w-full h-28 p-3 border rounded-lg border-purple-200" placeholder="เขียนความรู้สึกของวันนี้…" aria-label="ไดอารี่"></textarea>

    <div class="mt-3 flex items-center space-x-3">
      <button id="save-diary-btn" class="px-5 py-2 bg-purple-600 text-white rounded-lg">บันทึก</button>
      <p id="diary-msg" class="text-green-600" role="status" aria-live="polite"></p>
    </div>

    <div id="diary-list" class="mt-4"></div>
  </div>

  <div class="bg-white shadow-xl rounded-2xl p-6 mb-6">
    <h2 class="text-2xl font-bold text-purple-700 mb-3">⭐ ดาวเด็กดี</h2>
    <div class="flex items-center space-x-3">
      <button id="add-star-btn" class="px-5 py-2 bg-yellow-400 text-black rounded-lg">เพิ่มดาว</button>
      <p id="star-count" class="text-xl mt-2 text-purple-700"></p>
    </div>
  </div>

</section>

<!-- TEACHER -->
<section id="teacher-section" class="max-w-3xl mx-auto mt-8 hidden" aria-labelledby="teacher-heading">
  <div class="bg-white shadow-xl rounded-2xl p-6 mb-6">
    <h2 id="teacher-heading" class="text-2xl font-bold text-purple-700 mb-4">รายชื่อนักเรียน</h2>

    <table class="w-full text-center border" role="table">
      <thead>
        <tr class="bg-purple-100">
          <th class="border py-2">ชื่อ</th>
          <th class="border py-2">อารมณ์ล่าสุด</th>
          <th class="border py-2">ดาว</th>
          <th class="border py-2">ดูไดอารี่</th>
        </tr>
      </thead>
      <tbody id="teacher-student-table"></tbody>
    </table>
  </div>

  <div class="bg-white shadow-xl rounded-2xl p-6">
    <h2 class="text-2xl font-bold text-purple-700 mb-3">ไดอารี่ของนักเรียน</h2>
    <div id="teacher-diary" aria-live="polite"></div>
  </div>
</section>

<!-- ADMIN -->
<section id="admin-section" class="max-w-3xl mx-auto mt-8 hidden" aria-labelledby="admin-heading">

  <div class="bg-white shadow-xl rounded-2xl p-6 mb-6">
    <h2 id="admin-heading" class="text-2xl font-bold text-purple-700">สถิติอารมณ์รวม</h2>
    <canvas id="chart-emotion" class="mt-4" role="img" aria-label="แผนภูมิอารมณ์รวม"></canvas>
  </div>

  <div class="bg-white shadow-xl rounded-2xl p-6">
    <h2 class="text-2xl font-bold text-purple-700">ตารางดาวเด็กดี</h2>

    <table class="w-full text-center border mt-3">
      <thead>
        <tr class="bg-purple-100">
          <th class="border py-2">ชื่อ</th>
          <th class="border py-2">ดาว</th>
        </tr>
      </thead>
      <tbody id="admin-star-table"></tbody>
    </table>
  </div>
</section>

<!-- JAVASCRIPT -->
<script>
/*
  Improvements:
  - Use a currentStudent variable instead of hard-coded "เด็ก A"
  - Provide visual selected state for emotion buttons
  - Prevent saving empty emotion and show message
  - Use event listeners instead of inline onclick
  - Guard Chart drawing when no context or data
*/

const STORAGE_KEY = "startrackDB_v1";
let db = JSON.parse(localStorage.getItem(STORAGE_KEY)) || {
  students: {
    "เด็ก A": { emotion:"", diary:[], stars:0 },
    "เด็ก B": { emotion:"", diary:[], stars:0 },
    "เด็ก C": { emotion:"", diary:[], stars:0 }
  }
};

let currentStudent = "เด็ก A";
let selectedEmotion = "";

/* --- DOM references --- */
const sections = {
  student: document.getElementById("student-section"),
  teacher: document.getElementById("teacher-section"),
  admin: document.getElementById("admin-section")
};
const roleStudentBtn = document.getElementById("role-student");
const roleTeacherBtn = document.getElementById("role-teacher");
const roleAdminBtn = document.getElementById("role-admin");
const logoutBtn = document.getElementById("logout");

const studentSelect = document.getElementById("student-select");
const emotionMsg = document.getElementById("emotion-msg");
const diaryMsg = document.getElementById("diary-msg");

/* --- Event wiring --- */
roleStudentBtn.addEventListener("click", ()=> switchRole("student"));
roleTeacherBtn.addEventListener("click", ()=> switchRole("teacher"));
roleAdminBtn.addEventListener("click", ()=> switchRole("admin"));
logoutBtn.addEventListener("click", ()=> location.reload());

document.getElementById("save-emotion-btn").addEventListener("click", saveEmotion);
document.getElementById("save-diary-btn").addEventListener("click", saveDiary);
document.getElementById("add-star-btn").addEventListener("click", addStar);
studentSelect.addEventListener("change", (e)=> {
  currentStudent = e.target.value;
  loadStudent();
});

/* emotion buttons: delegate */
document.querySelectorAll(".emotion").forEach(btn=>{
  btn.addEventListener("click", (ev)=>{
    const e = btn.dataset.emotion;
    selectEmotion(e, btn);
  });
});

/* initial render */
populateStudentSelect();
switchRole("student");

/* --- Functions --- */

function saveToStorage(){
  try {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(db));
  } catch (err) {
    console.warn("Failed to save DB:", err);
  }
}

function switchRole(role){
  Object.values(sections).forEach(sec=>sec.classList.add("hidden"));
  const sec = sections[role];
  if(sec) sec.classList.remove("hidden");

  if(role==="student") loadStudent();
  if(role==="teacher") loadTeacher();
  if(role==="admin") loadAdmin();
}

function populateStudentSelect(){
  studentSelect.innerHTML = "";
  for(const name in db.students){
    const opt = document.createElement("option");
    opt.value = name;
    opt.text = name;
    studentSelect.appendChild(opt);
  }
  studentSelect.value = currentStudent;
}

function selectEmotion(e, buttonEl){
  selectedEmotion = e || "";
  // toggle selected class for buttons
  document.querySelectorAll(".emotion").forEach(b=>{
    b.classList.toggle("selected", b === buttonEl);
    b.setAttribute("aria-pressed", b === buttonEl ? "true" : "false");
  });
}

function saveEmotion(){
  if(!selectedEmotion){
    emotionMsg.innerText = "กรุณาเลือกอารมณ์ก่อนบันทึก";
    setTimeout(()=> emotionMsg.innerText = "", 2000);
    return;
  }
  db.students[currentStudent].emotion = selectedEmotion;
  saveToStorage();
  emotionMsg.innerText = "✔ บันทึกแล้ว";
  setTimeout(()=> emotionMsg.innerText = "", 2000);
  // refresh views that show emotion
  loadTeacher();
  loadAdmin();
}

function saveDiary(){
  let txt = document.getElementById("diary-input").value.trim();
  if(!txt){
    diaryMsg.innerText = "เนื้อหาไดอารี่ว่างเปล่า";
    setTimeout(()=> diaryMsg.innerText = "", 2000);
    return;
  }

  db.students[currentStudent].diary.push({
    text: txt,
    date: new Date().toLocaleString()
  });

  saveToStorage();
  document.getElementById("diary-input").value="";
  diaryMsg.innerText = "✔ บันทึกแล้ว";
  setTimeout(()=> diaryMsg.innerText = "", 2000);
  loadStudent();
  loadTeacher();
}

function loadStudent(){
  let list = document.getElementById("diary-list");
  list.innerHTML = "";

  const student = db.students[currentStudent];
  if(!student) return;

  student.diary.slice().reverse().forEach((d,i)=>{
    list.innerHTML += `
      <div class="bg-purple-50 p-3 rounded-xl mb-2 border">
        <div class="font-bold">${d.date}</div>
        <div class="whitespace-pre-wrap">${escapeHtml(d.text)}</div>
      </div>`;
  });

  document.getElementById("star-count").innerText =
    "จำนวนดาว: " + (student.stars || 0);
  // ensure selectedEmotion matches stored emotion visually
  const curE = student.emotion || "";
  selectedEmotion = curE;
  document.querySelectorAll(".emotion").forEach(b=>{
    b.classList.toggle("selected", b.dataset.emotion === curE);
    b.setAttribute("aria-pressed", b.dataset.emotion === curE ? "true" : "false");
  });
}

function addStar(){
  db.students[currentStudent].stars = (db.students[currentStudent].stars || 0) + 1;
  saveToStorage();
  loadStudent();
  loadTeacher();
  loadAdmin();
}

function loadTeacher(){
  let tb = document.getElementById("teacher-student-table");
  tb.innerHTML = "";

  for(let name in db.students){
    let st = db.students[name];
    tb.innerHTML += `
      <tr>
        <td class="border py-2">${escapeHtml(name)}</td>
        <td class="border py-2">${escapeHtml(st.emotion || "-")}</td>
        <td class="border py-2">${st.stars || 0}</td>
        <td class="border py-2">
          <button data-stname="${escapeHtml(name)}" class="px-3 py-1 bg-purple-300 rounded-lg teacher-view-diary">ดู</button>
        </td>
      </tr>`;
  }
  // wire diary buttons
  document.querySelectorAll(".teacher-view-diary").forEach(btn=>{
    btn.addEventListener("click", ()=> showDiary(btn.dataset.stname));
  });
}

function showDiary(name){
  let box = document.getElementById("teacher-diary");
  box.innerHTML = `<h3 class="text-xl font-bold mb-2">${escapeHtml(name)}</h3>`;

  const di = db.students[name].diary || [];
  if(di.length === 0){
    box.innerHTML += `<div class="text-sm text-gray-600">ยังไม่มีไดอารี่</div>`;
    return;
  }

  di.slice().reverse().forEach(d=>{
    box.innerHTML += `
      <div class="bg-purple-50 p-3 rounded-xl mb-2 border">
        <div class="font-bold">${d.date}</div>
        <div class="whitespace-pre-wrap">${escapeHtml(d.text)}</div>
      </div>`;
  });
}

function loadAdmin(){
  loadAdminStarTable();
  drawChart();
}

function loadAdminStarTable(){
  let tb = document.getElementById("admin-star-table");
  tb.innerHTML = "";

  for(let s in db.students){
    tb.innerHTML += `
      <tr>
        <td class="border py-2">${escapeHtml(s)}</td>
        <td class="border py-2">${db.students[s].stars || 0}</td>
      </tr>`;
  }
}

let emotionChart = null;
function drawChart(){
  const ctx = document.getElementById("chart-emotion");
  if(!ctx) return;

  let counts = {happy:0, normal:0, sad:0, angry:0, surprise:0};

  for(let s in db.students){
    let e = db.students[s].emotion;
    if(e && counts.hasOwnProperty(e)) counts[e]++;
  }

  const data = [counts.happy, counts.normal, counts.sad, counts.angry, counts.surprise];

  try {
    if(emotionChart) {
      emotionChart.data.datasets[0].data = data;
      emotionChart.update();
      return;
    }
    emotionChart = new Chart(ctx, {
      type:"pie",
      data:{
        labels:["ดีใจ", "เฉยๆ", "เศร้า", "โกรธ", "ประหลาดใจ"],
        datasets:[{
          data,
          backgroundColor:["#ffc2df","#b1e5e0","#ffd480","#ff9999","#cdb6ff"]
        }]
      },
      options: { responsive: true }
    });
  } catch (err) {
    console.warn("Chart draw error:", err);
  }
}

/* small helper */
function escapeHtml(unsafe) {
  if(typeof unsafe !== "string") return "";
  return unsafe.replace(/[&<"'>]/g, function(m) {
    return ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":"&#039;"})[m];
  });
}
</script>

</body>
</html>
