<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>StarTrack DEMO</title>

  <!-- TailwindCSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>

  <!-- Google Font -->
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

  <!-- Header -->
  <h1 class="text-3xl font-bold text-center mb-6">StarTrack DEMO</h1>

  <!-- App Container -->
  <div id="root" class="max-w-xl mx-auto bg-white shadow-lg rounded-xl p-6">
    <h2 class="text-xl font-semibold mb-4">บันทึกอารมณ์ประจำวัน</h2>

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
    <textarea id="note" class="w-full p-3 border rounded-lg mb-4" rows="4" placeholder="เขียนความรู้สึกของวันนี้..."></textarea>

    <button onclick="saveMood()" class="w-full py-3 bg-purple-600 text-white font-semibold rounded-lg hover:bg-purple-700">
      บันทึกข้อมูล
    </button>

    <hr class="my-6">

    <h2 class="text-xl font-semibold mb-4">ประวัติอารมณ์</h2>
    <div id="history" class="space-y-3"></div>
  </div>

  <!-- JavaScript -->
  <script>
    function saveMood() {
      const emotion = document.getElementById("emotion").value;
      const note = document.getElementById("note").value;

      if (!emotion) {
        alert("กรุณาเลือกอารมณ์");
        return;
      }

      const record = {
        emotion,
        note,
        time: new Date().toLocaleString("th-TH")
      };

      const data = JSON.parse(localStorage.getItem("moodData") || "[]");
      data.push(record);
      localStorage.setItem("moodData", JSON.stringify(data));

      document.getElementById("note").value = "";
      document.getElementById("emotion").value = "";

      loadHistory();
    }

    function loadHistory() {
      const container = document.getElementById("history");
      const data = JSON.parse(localStorage.getItem("moodData") || "[]");

      container.innerHTML = data
        .map(item => `
          <div class="p-4 border rounded-lg bg-gray-50">
            <p class="font-semibold">${item.emotion}</p>
            <p class="text-sm text-gray-600 mt-1">${item.note || "-"}</p>
            <p class="text-xs text-gray-400 mt-2">${item.time}</p>
          </div>
        `)
        .join("");
    }

    loadHistory();
  </script>

</body>
</html>
