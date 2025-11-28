<!doctype html>
<html lang="th">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>แอพบันทึกอารมณ์ & ดาว (Demo)</title>
  <style>
    :root{--bg:#f6f8fb;--card:#fff;--muted:#666;--accent:#2563eb}
    body{font-family:system-ui,-apple-system,Segoe UI,Roboto,"Noto Sans",Arial;padding:16px;background:var(--bg);color:#111}
    .container{max-width:1100px;margin:0 auto}
    header{display:flex;gap:12px;align-items:center;margin-bottom:16px}
    header h1{margin:0;font-size:18px}
    select,button,input,textarea{font-size:14px;padding:8px;border:1px solid #ddd;border-radius:6px}
    nav{display:flex;gap:8px;margin-bottom:12px}
    .grid{display:grid;grid-template-columns:1fr 340px;gap:12px}
    .card{background:var(--card);padding:12px;border-radius:8px;box-shadow:0 1px 3px rgba(16,24,40,0.05)}
    h2{margin:0 0 8px 0;font-size:16px}
    label{display:block;margin:8px 0 4px;color:var(--muted);font-size:13px}
    .mood-pill{display:inline-block;padding:6px 8px;border-radius:999px;margin:3px;background:#eef2ff;color:#1e3a8a;font-weight:600}
    .small{font-size:13px;color:var(--muted)}
    ul{padding-left:18px;margin:6px 0}
    .list-item{padding:6px;border-radius:6px;background:#fbfbfd;margin-bottom:6px;border:1px solid #f0f0f3}
    .flex{display:flex;gap:8px;align-items:center}
    .right{margin-left:auto}
    .badge{background:#f3f4f6;padding:4px 8px;border-radius:999px;font-weight:600}
    table{width:100%;border-collapse:collapse}
    td,th{padding:8px;border-bottom:1px solid #f1f1f4;font-size:13px;text-align:left}
    .muted{color:var(--muted)}
    .danger{color:#b91c1c}
    .success{color:#0f766e}
    footer{margin-top:14px;color:var(--muted);font-size:13px}
  </style>
</head>
<body>
  <div class="container">
    <header>
      <h1>แอพบันทึกอารมณ์ & ดาว (ตัวอย่าง)</h1>
      <div style="margin-left:auto" class="flex">
        <label class="small" style="margin-right:6px">เลือก Role</label>
        <select id="roleSelect" aria-label="role select">
          <option value="STUDENT">Student</option>
          <option value="TEACHER">Teacher</option>
          <option value="ADMIN">Admin</option>
        </select>
      </div>
    </header>

    <div class="grid">
      <div>
        <!-- Student section -->
        <div id="studentSection" class="card" style="display:none">
          <h2>Student Dashboard</h2>
          <div class="flex" style="align-items:center">
            <div>
              <div class="small">อารมณ์วันนี้</div>
              <select id="studentMood">
                <option value="VERY_HAPPY">😊 ดีมาก (VERY_HAPPY)</option>
                <option value="HAPPY">🙂 ดี (HAPPY)</option>
                <option value="NEUTRAL">😐 ปกติ (NEUTRAL)</option>
                <option value="SAD">☹️ เศร้า (SAD)</option>
                <option value="VERY_SAD">😭 เสียใจมาก (VERY_SAD)</option>
              </select>
            </div>
            <div style="margin-left:12px">
              <div class="small">บันทึกเพิ่มเติม</div>
              <textarea id="studentMoodNote" rows="2" style="width:260px"></textarea>
            </div>
          </div>
          <div style="margin-top:8px">
            <button id="saveMoodBtn">บันทึกอารมณ์</button>
            <span id="moodSavedMsg" class="muted" style="margin-left:8px"></span>
          </div>

          <hr style="margin:12px 0" />

          <div class="flex">
            <div>
              <div class="small">ดาวสะสม</div>
              <div style="font-size:22px;font-weight:700" id="studentStars">0 ⭐</div>
            </div>
            <div class="right">
              <div class="small">แลกของรางวัล</div>
              <div style="margin-top:6px">
                <button class="redeemBtn" data-reward="break" data-cost="10">ขอเวลาพัก 5 นาที — 10 ดาว</button>
                <button class="redeemBtn" data-reward="stationery" data-cost="12">คูปองเครื่องเขียน — 12 ดาว</button>
                <button class="redeemBtn" data-reward="food" data-cost="15">คูปองอาหาร — 15 ดาว</button>
              </div>
              <div class="small" style="margin-top:6px">สถานะการแลก: <span id="redemptionsList" class="muted"></span></div>
            </div>
          </div>

          <hr style="margin:12px 0" />

          <div>
            <div class="small">Diary</div>
            <textarea id="diaryContent" rows="3" style="width:100%"></textarea>
            <div style="margin-top:6px">
              <button id="saveDiary">บันทึกไดอารี่</button>
            </div>
            <div id="diaryList" style="margin-top:10px"></div>
          </div>

          <hr style="margin:12px 0" />

          <div>
            <div class="small">ขอนัดเข้าปรึกษา</div>
            <input id="apptPreferred" type="datetime-local" />
            <input id="apptNote" placeholder="บอกเหตุผล (optional)" style="width:60%;margin-left:8px" />
            <div style="margin-top:8px">
              <button id="requestAppt">ส่งคำขอ</button>
            </div>
            <div id="apptStatus" style="margin-top:8px" class="muted"></div>
          </div>

          <hr style="margin:12px 0" />

          <div>
            <div class="small">แบบทดสอบจิตวิทยาตัวอย่าง (self-rating)</div>
            <label class="small">ความเครียดวันนี้: <span id="stressVal">5</span></label>
            <input id="stressRange" type="range" min="0" max="10" value="5" />
            <button id="saveTest" style="margin-left:8px">บันทึกแบบทดสอบ</button>
            <div id="testResult" style="margin-top:8px" class="muted"></div>
          </div>
        </div>

        <!-- Teacher section -->
        <div id="teacherSection" class="card" style="display:none">
          <h2>Teacher Dashboard</h2>
          <div>
            <div class="small">บันทึกอารมณ์ (ครู)</div>
            <select id="teacherMood">
              <option value="VERY_HAPPY">VERY_HAPPY</option>
              <option value="HAPPY">HAPPY</option>
              <option value="NEUTRAL">NEUTRAL</option>
              <option value="SAD">SAD</option>
              <option value="VERY_SAD">VERY_SAD</option>
            </select>
            <textarea id="teacherMoodNote" rows="2" style="width:40%;margin-left:8px"></textarea>
            <div style="margin-top:8px"><button id="saveTeacherMood">บันทึกอารมณ์</button></div>
          </div>

          <hr style="margin:12px 0" />

          <div>
            <h3 style="margin:6px 0">ปรับดาวให้นักเรียน</h3>
            <div class="flex">
              <select id="selectStudentForAdjust"></select>
              <input id="starChange" placeholder="+1 หรือ -1" style="width:110px" />
              <input id="starReason" placeholder="เหตุผล (optional)" style="width:40%" />
              <button id="applyStarChange">ส่ง</button>
            </div>
            <div id="adjustMsg" class="muted" style="margin-top:6px"></div>
          </div>

          <hr style="margin:12px 0" />

          <div>
            <h3 style="margin:6px 0">คำขอนัดจากนักเรียน (รออนุมัติ)</h3>
            <div id="teacherAppts"></div>
          </div>

          <hr style="margin:12px 0" />

          <div>
            <h3 style="margin:6px 0">รายงานพฤติกรรม</h3>
            <div class="flex">
              <select id="reportStudent"></select>
              <select id="reportKind">
                <option value="POSITIVE">เชิงบวก</option>
                <option value="NEGATIVE">เชิงลบ</option>
              </select>
              <input id="reportNote" placeholder="รายละเอียด" style="width:40%" />
              <button id="submitReport">รายงาน</button>
            </div>
            <div id="reportsList" style="margin-top:8px"></div>
          </div>
        </div>

        <!-- Admin section -->
        <div id="adminSection" class="card" style="display:none">
          <h2>Admin Dashboard</h2>
          <div>
            <div class="small">บันทึกอารมณ์ (ผู้บริหาร)</div>
            <select id="adminMood">
              <option value="VERY_HAPPY">VERY_HAPPY</option>
              <option value="HAPPY">HAPPY</option>
              <option value="NEUTRAL">NEUTRAL</option>
              <option value="SAD">SAD</option>
              <option value="VERY_SAD">VERY_SAD</option>
            </select>
            <textarea id="adminMoodNote" rows="2" style="width:40%;margin-left:8px"></textarea>
            <div style="margin-top:8px"><button id="saveAdminMood">บันทึกอารมณ์</button></div>
          </div>

          <hr style="margin:12px 0" />

          <div>
            <h3 style="margin:6px 0">ปรับดาว (Admin)</h3>
            <div class="flex">
              <select id="selectStudentForAdjustAdmin"></select>
              <input id="starChangeAdmin" placeholder="+1 หรือ -1" style="width:110px" />
              <input id="starReasonAdmin" placeholder="เหตุผล (optional)" style="width:40%" />
              <button id="applyStarChangeAdmin">ส่ง</button>
            </div>
          </div>

          <hr style="margin:12px 0" />

          <div>
            <h3 style="margin:6px 0">สถิติภาพรวม</h3>
            <div id="statsArea"></div>
          </div>

          <hr style="margin:12px 0" />

          <div>
            <h3 style="margin:6px 0">การอนุมัติการแลกของรางวัล</h3>
            <div id="redemptionsAdmin"></div>
          </div>
        </div>
      </div>

      <aside>
        <div class="card">
          <h2>ผู้ใช้งานตัวอย่าง</h2>
          <div class="small">นักเรียน</div>
          <ul id="studentList"></ul>
          <div class="small" style="margin-top:8px">ครู / ผู้บริหาร</div>
          <ul id="teacherList"></ul>
        </div>

        <div class="card" style="margin-top:12px">
          <h2>Quick Debug</h2>
          <div class="small">ล้างข้อมูลทดสอบ</div>
          <button id="resetData">รีเซ็ตข้อมูล</button>
          <div style="margin-top:8px" class="muted">ข้อมูลจะถูกเก็บไว้ใน localStorage ของเบราว์เซอร์</div>
        </div>
      </aside>
    </div>

    <footer>
      ตัวอย่างโดย GitHub Copilot-style assistant — โค้ดนี้เป็น demo ไม่มีระบบ authentication จริง ห้ามใช้ใน production โดยตรง
    </footer>
  </div>

  <script>
    // Simple demo data and storage helpers
    const STORAGE_KEYS = {
      users: 'demo_users_v1',
      moods: 'demo_moods_v1',
      stars: 'demo_stars_v1',
      redemptions: 'demo_redemptions_v1',
      diaries: 'demo_diaries_v1',
      appointments: 'demo_appts_v1',
      reports: 'demo_reports_v1',
      tests: 'demo_tests_v1'
    };

    const sampleUsers = [
      { id: 'student-1', name: 'นร. สมชาย', role: 'STUDENT' },
      { id: 'student-2', name: 'นร. สมหญิง', role: 'STUDENT' },
      { id: 'teacher-1', name: 'ครู ปกรณ์', role: 'TEACHER' },
      { id: 'admin-1', name: 'ผอ. สมศรี', role: 'ADMIN' }
    ];

    function save(key, v){ localStorage.setItem(key, JSON.stringify(v)); }
    function load(key){ try{ return JSON.parse(localStorage.getItem(key) || 'null') }catch(e){ return null } }

    // initialize
    function ensureInit(){
      if(!load(STORAGE_KEYS.users)){ save(STORAGE_KEYS.users, sampleUsers) }
      if(!load(STORAGE_KEYS.moods)){ save(STORAGE_KEYS.moods, []) }
      if(!load(STORAGE_KEYS.stars)){ save(STORAGE_KEYS.stars, []) }
      if(!load(STORAGE_KEYS.redemptions)){ save(STORAGE_KEYS.redemptions, []) }
      if(!load(STORAGE_KEYS.diaries)){ save(STORAGE_KEYS.diaries, []) }
      if(!load(STORAGE_KEYS.appointments)){ save(STORAGE_KEYS.appointments, []) }
      if(!load(STORAGE_KEYS.reports)){ save(STORAGE_KEYS.reports, []) }
      if(!load(STORAGE_KEYS.tests)){ save(STORAGE_KEYS.tests, []) }
    }

    ensureInit();

    // utility
    function getUsers(){ return load(STORAGE_KEYS.users) || [] }
    function getStars(){ return load(STORAGE_KEYS.stars) || [] }
    function getMoods(){ return load(STORAGE_KEYS.moods) || [] }
    function getRedemptions(){ return load(STORAGE_KEYS.redemptions) || [] }
    function getDiaries(){ return load(STORAGE_KEYS.diaries) || [] }
    function getAppts(){ return load(STORAGE_KEYS.appointments) || [] }
    function getReports(){ return load(STORAGE_KEYS.reports) || [] }
    function getTests(){ return load(STORAGE_KEYS.tests) || [] }

    function addStarTransaction(giverId, receiverId, change, reason){
      const stars = getStars();
      stars.push({ id: cryptoRandomId(), giverId, receiverId, change: Number(change), reason: reason || '', createdAt: new Date().toISOString() });
      save(STORAGE_KEYS.stars, stars);
    }

    function sumStarsFor(userId){
      return (getStars().filter(s => s.receiverId === userId).reduce((a,b)=>a+(Number(b.change)||0),0) || 0);
    }

    function cryptoRandomId(){ return 'id-'+Math.random().toString(36).slice(2,9) }

    // UI wiring
    const roleSelect = document.getElementById('roleSelect');
    const studentSection = document.getElementById('studentSection');
    const teacherSection = document.getElementById('teacherSection');
    const adminSection = document.getElementById('adminSection');

    function showSectionFor(role){
      studentSection.style.display = role === 'STUDENT' ? 'block' : 'none';
      teacherSection.style.display = role === 'TEACHER' ? 'block' : 'none';
      adminSection.style.display = role === 'ADMIN' ? 'block' : 'none';
      populateLists();
      renderAll();
    }

    roleSelect.addEventListener('change', ()=> showSectionFor(roleSelect.value));
    showSectionFor(roleSelect.value);

    // populate user lists
    function populateLists(){
      const users = getUsers();
      const studentList = document.getElementById('studentList');
      const teacherList = document.getElementById('teacherList');
      studentList.innerHTML = ''; teacherList.innerHTML = '';
      users.filter(u => u.role === 'STUDENT').forEach(s => {
        const li = document.createElement('li'); li.textContent = s.name + ' ('+s.id+')';
        studentList.appendChild(li);
      });
      users.filter(u => u.role !== 'STUDENT').forEach(t => {
        const li = document.createElement('li'); li.textContent = t.name + ' — '+t.role;
        teacherList.appendChild(li);
      });

      // dropdowns for teacher/admin actions
      const sel = document.getElementById('selectStudentForAdjust');
      const sel2 = document.getElementById('reportStudent');
      const sel3 = document.getElementById('selectStudentForAdjustAdmin');
      sel.innerHTML = ''; sel2.innerHTML = ''; sel3.innerHTML = '';
      users.filter(u=>u.role==='STUDENT').forEach(s=>{
        const opt = document.createElement('option'); opt.value = s.id; opt.textContent = s.name+' ('+s.id+')';
        sel.appendChild(opt);
        const opt2 = opt.cloneNode(true); sel2.appendChild(opt2);
        const opt3 = opt.cloneNode(true); sel3.appendChild(opt3);
      });
    }

    // Student functions
    document.getElementById('saveMoodBtn').addEventListener('click', ()=>{
      const mood = document.getElementById('studentMood').value;
      const note = document.getElementById('studentMoodNote').value;
      const userId = getUsers().find(u=>u.role==='STUDENT')?.id || 'student-1';
      const moods = getMoods();
      moods.push({ id: cryptoRandomId(), userId, mood, note, date: new Date().toISOString() });
      save(STORAGE_KEYS.moods, moods);
      document.getElementById('moodSavedMsg').textContent = 'บันทึกแล้ว';
      renderAll();
      setTimeout(()=> document.getElementById('moodSavedMsg').textContent = '', 2000);
    });

    function renderStudentStars(){
      const userId = getUsers().find(u=>u.role==='STUDENT')?.id || 'student-1';
      const total = sumStarsFor(userId);
      document.getElementById('studentStars').textContent = total + ' ⭐';
    }

    // redemption
    document.querySelectorAll('.redeemBtn').forEach(b=>{
      b.addEventListener('click', ()=>{
        const reward = b.getAttribute('data-reward');
        const cost = Number(b.getAttribute('data-cost'));
        const userId = getUsers().find(u=>u.role==='STUDENT')?.id || 'student-1';
        const total = sumStarsFor(userId);
        if(total < cost){
          alert('ดาวไม่เพียงพอสำหรับแลกรางวัลนี้');
          return;
        }
        // create pending redemption (do not deduct yet; admin/teacher approves and then deduction occurs)
        const redemptions = getRedemptions();
        redemptions.push({ id: cryptoRandomId(), studentId: userId, reward, cost, status: 'PENDING', createdAt: new Date().toISOString() });
        save(STORAGE_KEYS.redemptions, redemptions);
        renderAll();
        alert('ส่งคำขอแลกรางวัลเรียบร้อย (รออนุมัติ)');
      });
    });

    function renderRedemptionsList(){
      const userId = getUsers().find(u=>u.role==='STUDENT')?.id || 'student-1';
      const rs = getRedemptions().filter(r => r.studentId === userId);
      const container = document.getElementById('redemptionsList');
      if(rs.length===0) { container.textContent = 'ยังไม่มี'; return; }
      container.innerHTML = rs.map(r => `${r.reward} (${r.cost} ⭐) — ${r.status}`).join('; ');
    }

    // diary
    document.getElementById('saveDiary').addEventListener('click', ()=>{
      const content = document.getElementById('diaryContent').value.trim();
      if(!content){ alert('กรุณากรอกเนื้อหา'); return; }
      const userId = getUsers().find(u=>u.role==='STUDENT')?.id || 'student-1';
      const diaries = getDiaries();
      diaries.push({ id: cryptoRandomId(), studentId: userId, content, date: new Date().toISOString() });
      save(STORAGE_KEYS.diaries, diaries);
      document.getElementById('diaryContent').value = '';
      renderAll();
    });

    function renderDiaryList(){
      const userId = getUsers().find(u=>u.role==='STUDENT')?.id || 'student-1';
      const ds = getDiaries().filter(d => d.studentId === userId);
      const el = document.getElementById('diaryList');
      el.innerHTML = ds.length === 0 ? '<div class="muted">ไม่มีบันทึก</div>' : ds.map(d => `<div class="list-item"><div class="small">${new Date(d.date).toLocaleString()}</div><div>${escapeHtml(d.content)}</div></div>`).join('');
    }

    // appointments
    document.getElementById('requestAppt').addEventListener('click', ()=>{
      const pref = document.getElementById('apptPreferred').value;
      const note = document.getElementById('apptNote').value;
      const userId = getUsers().find(u=>u.role==='STUDENT')?.id || 'student-1';
      const appts = getAppts();
      appts.push({ id: cryptoRandomId(), studentId: userId, preferredAt: pref || null, note: note || '', status: 'PENDING', createdAt: new Date().toISOString() });
      save(STORAGE_KEYS.appointments, appts);
      document.getElementById('apptPreferred').value = '';
      document.getElementById('apptNote').value = '';
      renderAll();
      document.getElementById('apptStatus').textContent = 'ส่งคำขอแล้ว (รอครู/จิตแพทย์อนุมัติ)';
      setTimeout(()=>document.getElementById('apptStatus').textContent = '', 3000);
    });

    // psych test
    const stressRange = document.getElementById('stressRange');
    const stressVal = document.getElementById('stressVal');
    stressRange.addEventListener('input', ()=> stressVal.textContent = stressRange.value);
    document.getElementById('saveTest').addEventListener('click', ()=>{
      const test = { id: cryptoRandomId(), studentId: getUsers().find(u=>u.role==='STUDENT')?.id, testType:'stress-self', score: Number(stressRange.value), createdAt: new Date().toISOString() };
      const arr = getTests(); arr.push(test); save(STORAGE_KEYS.tests, arr);
      document.getElementById('testResult').textContent = 'บันทึกแบบทดสอบแล้ว (score: '+test.score+')';
      setTimeout(()=> document.getElementById('testResult').textContent = '', 2500);
    });

    // Teacher functions
    document.getElementById('saveTeacherMood').addEventListener('click', ()=>{
      const mood = document.getElementById('teacherMood').value;
      const note = document.getElementById('teacherMoodNote').value;
      const teacherId = getUsers().find(u=>u.role==='TEACHER')?.id || 'teacher-1';
      const moods = getMoods(); moods.push({ id: cryptoRandomId(), userId: teacherId, mood, note, date: new Date().toISOString() }); save(STORAGE_KEYS.moods, moods);
      alert('บันทึกอารมณ์ครูแล้ว'); renderAll();
    });

    document.getElementById('applyStarChange').addEventListener('click', ()=>{
      const giverId = getUsers().find(u=>u.role==='TEACHER')?.id || 'teacher-1';
      const receiverId = document.getElementById('selectStudentForAdjust').value;
      const changeVal = document.getElementById('starChange').value;
      const reason = document.getElementById('starReason').value;
      if(!receiverId || !changeVal){ alert('กรุณากรอกข้อมูลให้ครบ'); return; }
      addStarTransaction(giverId, receiverId, Number(changeVal), reason);
      document.getElementById('adjustMsg').textContent = 'ปรับดาวเรียบร้อย';
      renderAll();
      setTimeout(()=> document.getElementById('adjustMsg').textContent = '', 2000);
    });

    // teacher appointments view & approve
    function renderTeacherAppts(){
      const el = document.getElementById('teacherAppts');
      const appts = getAppts().filter(a => a.status === 'PENDING');
      if(appts.length===0){ el.innerHTML = '<div class="muted">ไม่มีคำขอ</div>'; return; }
      el.innerHTML = appts.map(a => {
        const student = getUsers().find(u=>u.id===a.studentId);
        return `<div class="list-item"><div class="small">${student?.name || a.studentId} — ${new Date(a.createdAt).toLocaleString()}</div>
                 <div>preferred: ${a.preferredAt ? a.preferredAt : '-'}</div>
                 <div>note: ${escapeHtml(a.note)}</div>
                 <div style="margin-top:6px"><button data-appt="${a.id}" class="approveAppt">ยืนยัน</button> <button data-appt="${a.id}" class="cancelAppt">ยกเลิก</button></div></div>`;
      }).join('');
      document.querySelectorAll('.approveAppt').forEach(b=> b.addEventListener('click', (ev)=>{
        const id = ev.target.getAttribute('data-appt');
        const appts = getAppts(); const a = appts.find(x=>x.id===id); if(a){ a.status='CONFIRMED'; save(STORAGE_KEYS.appointments, appts); renderAll(); alert('ยืนยันเรียบร้อย'); }
      }));
      document.querySelectorAll('.cancelAppt').forEach(b=> b.addEventListener('click', (ev)=>{
        const id = ev.target.getAttribute('data-appt');
        const appts = getAppts(); const a = appts.find(x=>x.id===id); if(a){ a.status='CANCELLED'; save(STORAGE_KEYS.appointments, appts); renderAll(); alert('ยกเลิกแล้ว'); }
      }));
    }

    // behavior report
    document.getElementById('submitReport').addEventListener('click', ()=>{
      const teacherId = getUsers().find(u=>u.role==='TEACHER')?.id || 'teacher-1';
      const studentId = document.getElementById('reportStudent').value;
      const kind = document.getElementById('reportKind').value;
      const note = document.getElementById('reportNote').value;
      if(!studentId){ alert('เลือกนักเรียน'); return; }
      const reports = getReports();
      reports.push({ id: cryptoRandomId(), teacherId, studentId, kind, note, createdAt: new Date().toISOString() });
      save(STORAGE_KEYS.reports, reports);
      document.getElementById('reportNote').value = '';
      renderAll();
      alert('รายงานเรียบร้อย');
    });

    function renderReportsList(){
      const rs = getReports();
      const el = document.getElementById('reportsList');
      if(rs.length===0){ el.innerHTML = '<div class="muted">ยังไม่มีรายงาน</div>'; return; }
      el.innerHTML = rs.slice().reverse().map(r => {
        const student = getUsers().find(u=>u.id===r.studentId);
        const teacher = getUsers().find(u=>u.id===r.teacherId);
        return `<div class="list-item"><div class="small">${new Date(r.createdAt).toLocaleString()} — ${teacher?.name||r.teacherId}</div><div>${r.kind} — ${student?.name||r.studentId}</div><div>${escapeHtml(r.note)}</div></div>`;
      }).join('');
    }

    // Admin functions
    document.getElementById('saveAdminMood').addEventListener('click', ()=>{
      const mood = document.getElementById('adminMood').value;
      const note = document.getElementById('adminMoodNote').value;
      const adminId = getUsers().find(u=>u.role==='ADMIN')?.id || 'admin-1';
      const moods = getMoods(); moods.push({ id: cryptoRandomId(), userId: adminId, mood, note, date: new Date().toISOString() }); save(STORAGE_KEYS.moods, moods);
      alert('บันทึกอารมณ์แอดมินแล้ว'); renderAll();
    });

    document.getElementById('applyStarChangeAdmin').addEventListener('click', ()=>{
      const giverId = getUsers().find(u=>u.role==='ADMIN')?.id || 'admin-1';
      const receiverId = document.getElementById('selectStudentForAdjustAdmin').value;
      const changeVal = document.getElementById('starChangeAdmin').value;
      const reason = document.getElementById('starReasonAdmin').value;
      if(!receiverId || !changeVal){ alert('กรุณากรอกข้อมูลให้ครบ'); return; }
      addStarTransaction(giverId, receiverId, Number(changeVal), reason);
      renderAll();
      alert('ปรับดาวโดย Admin เรียบร้อย');
    });

    // Admin redemption approval
    function renderRedemptionsAdmin(){
      const el = document.getElementById('redemptionsAdmin');
      const reds = getRedemptions().filter(r => r.status === 'PENDING');
      if(reds.length===0){ el.innerHTML = '<div class="muted">ไม่มีคำขอแลกของรางวัล</div>'; return; }
      el.innerHTML = reds.map(r => {
        const student = getUsers().find(u=>u.id===r.studentId);
        return `<div class="list-item"><div class="small">${new Date(r.createdAt).toLocaleString()} — ${student?.name || r.studentId}</div><div>${r.reward} — ${r.cost} ⭐</div>
                <div style="margin-top:8px"><button data-red="${r.id}" class="approveRed">อนุมัติ</button> <button data-red="${r.id}" class="rejectRed">ปฏิเสธ</button></div></div>`;
      }).join('');
      document.querySelectorAll('.approveRed').forEach(b=> b.addEventListener('click',(ev)=>{
        const id = ev.target.getAttribute('data-red');
        const reds = getRedemptions(); const r = reds.find(x=>x.id===id);
        if(!r) return;
        // deduct stars as transaction by admin
        const adminId = getUsers().find(u=>u.role==='ADMIN')?.id || 'admin-1';
        addStarTransaction(adminId, r.studentId, -Math.abs(r.cost), 'Redeem: '+r.reward);
        r.status = 'APPROVED';
        save(STORAGE_KEYS.redemptions, reds);
        renderAll();
        alert('อนุมัติ และหักดาวเรียบร้อย');
      }));
      document.querySelectorAll('.rejectRed').forEach(b=> b.addEventListener('click',(ev)=>{
        const id = ev.target.getAttribute('data-red');
        const reds = getRedemptions(); const r = reds.find(x=>x.id===id);
        if(!r) return;
        r.status = 'REJECTED';
        save(STORAGE_KEYS.redemptions, reds);
        renderAll();
        alert('ปฏิเสธคำขอ');
      }));
    }

    // Stats
    function renderStats(){
      const moods = getMoods();
      const users = getUsers();
      const students = users.filter(u=>u.role==='STUDENT').map(u=>u.id);
      const teachers = users.filter(u=>u.role==='TEACHER'||u.role==='ADMIN').map(u=>u.id);
      const group = (ids) => {
        const arr = moods.filter(m=>ids.includes(m.userId));
        const counts = arr.reduce((acc,cur)=>{ acc[cur.mood] = (acc[cur.mood]||0)+1; return acc },{});
        return counts;
      }
      const studentMoodCounts = group(students);
      const teacherMoodCounts = group(teachers);
      const area = document.getElementById('statsArea');
      area.innerHTML = `<div><strong>นักเรียน</strong><div class="small">${JSON.stringify(studentMoodCounts)}</div></div>
                        <div style="margin-top:8px"><strong>ครู/ผู้บริหาร</strong><div class="small">${JSON.stringify(teacherMoodCounts)}</div></div>`;
    }

    // global renderer
    function renderAll(){
      renderStudentStars();
      renderRedemptionsList();
      renderDiaryList();
      renderTeacherAppts();
      renderReportsList();
      renderRedemptionsAdmin();
      renderStats();
      // update student list in side (stars)
      const ul = document.getElementById('studentList');
      const users = getUsers();
      ul.innerHTML = users.filter(u=>u.role==='STUDENT').map(s => `<li>${s.name} — ${sumStarsFor(s.id)} ⭐</li>`).join('');
    }

    renderAll();

    // reset
    document.getElementById('resetData').addEventListener('click', ()=>{
      if(!confirm('ลบข้อมูลทดสอบทั้งหมดใน localStorage หรือไม่?')) return;
      localStorage.removeItem(STORAGE_KEYS.moods);
      localStorage.removeItem(STORAGE_KEYS.stars);
      localStorage.removeItem(STORAGE_KEYS.redemptions);
      localStorage.removeItem(STORAGE_KEYS.diaries);
      localStorage.removeItem(STORAGE_KEYS.appointments);
      localStorage.removeItem(STORAGE_KEYS.reports);
      localStorage.removeItem(STORAGE_KEYS.tests);
      save(STORAGE_KEYS.users, sampleUsers);
      ensureInit();
      populateLists();
      renderAll();
      alert('รีเซ็ตเสร็จ');
    });

    // small helper
    function escapeHtml(s){ if(!s) return ''; return s.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;') }

  </script>
</body>
</html>
