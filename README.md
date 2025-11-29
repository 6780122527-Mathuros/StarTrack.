<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>StarTrack DEMO - Login System</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;500;600;700&display=swap" rel="stylesheet">

  <style>
    body {
      font-family: 'Sarabun', sans-serif;
      background: linear-gradient(135deg, #f4eaff 0%, #d3ecfd 100%);
      color: #444;
    }
  </style>
</head>
<body class="p-6">
  
  <div id="app" class="max-w-xl mx-auto"></div>

  <script>
    // -----------------------
    // 📌 ตัวแปรกลาง
    // -----------------------
    let currentUser = null;

    // -----------------------
    // 📌 หน้าเลือกบทบาท
    // -----------------------
    function showRoleSelect() {
      document.getElementById("app").innerHTML = `
        <h1 class="text-3xl font-bold text-center mb-6">StarTrack DEMO</h1>
        <div class="bg-white p-6 rounded-xl shadow-lg">
          <h2 class="text-xl font-semibold mb-4 text-center">เลือกบทบาทเข้าใช้งาน</h2>

          <div class="space-y-4">
            <button onclick="showLogin('student')" class="w-full py-3 bg-blue-600 text-white rounded-lg">นักเรียน</button>
            <button onclick="showLogin('teacher')" class="w-full py-3 bg-green-600 text-white rounded-lg">ครู</button>
            <button onclick="showLogin('admin')" class="w-full py-3 bg-purple-700 text-white rounded-lg">ผู้บริหาร</button>
          </div>
        </div>
      `;
    }

    // -----------------------
    // 📌 หน้าเข้าสู่ระบบ
    // -----------------------
    function showLogin(role) {
      document.getElementById("app").innerHTML = `
        <div class="bg-white p-6 rounded-xl shadow-lg">
          <h2 class="text-xl font-bold mb-4 text-center">เข้าสู่ระบบ (${role})</h2>

          <input id="username" class="w-full p-3 border rounded-lg mb-4" placeholder="ชื่อผู้ใช้">
          <input id="password" type="password" class="w-full p-3 border rounded-lg mb-4" placeholder="รหัสผ่าน">

          <button onclick="login('${role}')" class="w-full py-3 bg-purple-600 text-white rounded-lg">เข้าสู่ระบบ</button>
          <button onclick="showRoleSelect()" class="w-full py-3 mt-3 bg-gray-300 rounded-lg">ย้อนกลับ</button>
        </div>
      `;
    }

    // -----------------------
    // 📌 ระบบ Login แบบง่าย
    // -----------------------
    function login(role) {
      const user = document.getElementById("username").value;
      const pass = document.getElementById("password").value;

      if (!user || !pass) {
        alert("กรุณากรอกข้อมูลให้ครบ");
        return;
      }

      // จำลองระบบ users (ไม่ต้องมี server)
      currentUser = { username: user, role };

      if (role === "student") showStudentPage();
      if (role === "teacher") showTeacherPage();
      if (role === "admin") showAdminPage();
    }

    // -----------------------
    // 📌 นักเรียน — บันทึกอารมณ์
    // -----------------------
    function showStudentPage() {
      document.getElementById("app").innerHTML = `
        <h1 class="text-2xl font-bold mb-4">นักเรียน: ${currentUser.username}</h1>

        <div class="bg-white p-6 rounded-xl shadow-lg">
          <label class="block mb-2 font-medium">เลือกอารมณ์</label>
          <select id="emotion" class="w-full p-3 border rounded-lg mb-4">
            <option value="">-- เลือกอารมณ์ --</option>
            <option value="ดีมาก 😊">ดีมาก 😊</option>
            <option value="ดี 🙂">ดี 🙂</option>
            <option value="เฉย ๆ 😐">เฉย ๆ 😐</option>
            <option value="ไม่ค่อยดี 🙁">ไม่ค่อยดี 🙁</option>
            <option value="แย่มาก 😢">แย่มาก 😢</option>
          </select>

          <label class="block mb-2 font-medium">บันทึกข้อความ</label>
          <textarea id="note" class="w-full p-3 border rounded-lg mb-4" rows="4"></textarea>

          <button onclick="saveMood()" class="w-full py-3 bg-blue-600 text-white rounded-lg">บันทึก</button>

          <hr class="my-6">
          <h2 class="text-xl font-semibold mb-4">ประวัติอารมณ์ของฉัน</h2>
          <div id="history"></div>
        </div>

        <button onclick="logout()" class="mt-4 w-full py-2 bg-gray-300 rounded-lg">ออกจากระบบ</button>
      `;

      loadHistory();
    }

    function saveMood() {
      const emotion = document.getElementById("emotion").value;
      const note = document.getElementById("note").value;

      if (!emotion) {
        alert("กรุณาเลือกอารมณ์");
        return;
      }

      const record = {
        user: currentUser.username,
        emotion,
        note,
        time: new Date().toLocaleString("th-TH")
      };

      const data = JSON.parse(localStorage.getItem("moodData") || "[]");
      data.push(record);
      localStorage.setItem("moodData", JSON.stringify(data));

      loadHistory();
    }

    function loadHistory() {
      const data = JSON.parse(localStorage.getItem("moodData") || "[]")
        .filter(r => r.user === currentUser.username);

      document.getElementById("history").innerHTML = data.map(item => `
        <div class="p-4 border rounded-lg bg-gray-50 mb-2">
          <p class="font-semibold">${item.emotion}</p>
          <p class="text-sm">${item.note || "-"}</p>
          <p class="text-xs text-gray-400 mt-1">${item.time}</p>
        </div>
      `).join("");
    }

    // -----------------------
    // 📌 ครู — ดูประวัตินักเรียน
    // -----------------------
    function showTeacherPage() {
      const data = JSON.parse(localStorage.getItem("moodData") || "[]");

      document.getElementById("app").innerHTML = `
        <h1 class="text-2xl font-bold mb-4">ครู: ${currentUser.username}</h1>

        <div class="bg-white p-6 rounded-xl shadow-lg">
          <h2 class="text-xl font-semibold mb-4">ประวัติอารมณ์นักเรียนทั้งหมด</h2>

          <div class="space-y-3">
            ${
              data.length
                ? data.map(r => `
                    <div class="p-4 bg-gray-50 border rounded-lg">
                      <p><strong>นักเรียน:</strong> ${r.user}</p>
                      <p><strong>อารมณ์:</strong> ${r.emotion}</p>
                      <p class="text-sm text-gray-600">${r.note || "-"}</p>
                      <p class="text-xs text-gray-400">${r.time}</p>
                    </div>
                  `).join("")
                : `<p class="text-center text-gray-500">ยังไม่มีข้อมูล</p>`
            }
          </div>
        </div>

        <button onclick="logout()" class="mt-4 w-full py-2 bg-gray-300 rounded-lg">ออกจากระบบ</button>
      `;
    }

    // -----------------------
    // 📌 ผู้บริหาร — สถิติรวม
    // -----------------------
    function showAdminPage() {
      const data = JSON.parse(localStorage.getItem("moodData") || "[]");

      // นับสถิติ
      const stats = {
        good: data.filter(r => r.emotion.includes("ดีมาก") || r.emotion.includes("ดี 🙂")).length,
        neutral: data.filter(r => r.emotion.includes("เฉย")).length,
        bad: data.filter(r => r.emotion.includes("ไม่ค่อยดี") || r.emotion.includes("แย่มาก")).length
      };

      document.getElementById("app").innerHTML = `
        <h1 class="text-2xl font-bold mb-4">ผู้บริหาร: ${currentUser.username}</h1>

        <div class="bg-white p-6 rounded-xl shadow-lg">
          <h2 class="text-xl font-semibold mb-4">สถิติอารมณ์นักเรียน</h2>

          <div class="space-y-4">
            <p>อารมณ์ดี: <strong>${stats.good}</strong></p>
            <p>เฉย ๆ: <strong>${stats.neutral}</strong></p>
            <p>อารมณ์แย่: <strong>${stats.bad}</strong></p>
          </div>
        </div>

        <button onclick="logout()" class="mt-4 w-full py-2 bg-gray-300 rounded-lg">ออกจากระบบ</button>
      `;
    }

    // -----------------------
    // 📌 ออกจากระบบ
    // -----------------------
    function logout() {
      currentUser = null;
      showRoleSelect();
    }

    // เปิดหน้าแรก
    showRoleSelect();
  </script>

</body>
</html>
