# school-results-gateway
🎓 بوابة نتائج الطلاب الإلكترونية مع لوحة تحكم المدير
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>بوابة نتائج الطلاب</title>
  <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;900&family=Tajawal:wght@400;500;700;900&display=swap" rel="stylesheet" />
  <style>
    :root {
      --navy: #0a1628;
      --blue: #1a3a6b;
      --gold: #c9a227;
      --gold-light: #f0c94d;
      --cream: #fdf6e3;
      --white: #ffffff;
      --success: #2ecc71;
      --danger: #e74c3c;
      --text-dark: #0a1628;
      --shadow: 0 8px 40px rgba(10,22,40,0.18);
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: 'Cairo', 'Tajawal', sans-serif;
      background: linear-gradient(135deg, #0a1628 0%, #1a3a6b 50%, #0f2244 100%);
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 20px;
      position: relative;
      overflow-x: hidden;
    }

    .card {
      background: var(--cream);
      border-radius: 24px;
      box-shadow: var(--shadow), 0 0 0 3px var(--gold);
      width: 100%;
      max-width: 480px;
      padding: 44px 36px 36px;
      animation: fadeUp 0.7s cubic-bezier(.22,1,.36,1) both;
      position: relative;
    }

    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(40px); }
      to   { opacity: 1; transform: translateY(0); }
    }

    .logo-area {
      text-align: center;
      margin-bottom: 28px;
    }

    .logo-icon {
      width: 72px; height: 72px;
      background: linear-gradient(135deg, var(--navy), var(--blue));
      border-radius: 50%;
      display: flex; align-items: center; justify-content: center;
      margin: 0 auto 14px;
      box-shadow: 0 4px 20px rgba(10,22,40,0.25), 0 0 0 4px var(--gold);
    }
    .logo-icon svg { width: 38px; height: 38px; fill: var(--gold-light); }

    .logo-area h1 {
      font-size: 1.6rem;
      font-weight: 900;
      color: var(--navy);
    }
    .logo-area p {
      font-size: 0.92rem;
      color: #5a6a80;
      margin-top: 4px;
    }

    .form-group {
      margin-bottom: 20px;
    }
    .form-group label {
      display: block;
      font-size: 0.97rem;
      font-weight: 700;
      color: var(--navy);
      margin-bottom: 8px;
    }
    .form-group input {
      width: 100%;
      padding: 13px 18px;
      border: 2px solid #d0dbe8;
      border-radius: 12px;
      font-family: 'Cairo', sans-serif;
      font-size: 1.05rem;
      color: var(--navy);
      background: var(--white);
      outline: none;
      text-align: right;
      direction: ltr;
    }

    .btn {
      width: 100%;
      padding: 14px;
      background: linear-gradient(135deg, var(--navy), var(--blue));
      color: var(--gold-light);
      border: none;
      border-radius: 12px;
      font-family: 'Cairo', sans-serif;
      font-size: 1.08rem;
      font-weight: 700;
      cursor: pointer;
      transition: all 0.2s;
      box-shadow: 0 4px 18px rgba(10,22,40,0.25);
    }
    .btn:hover { opacity: 0.9; transform: translateY(-2px); }

    .error-msg {
      background: #fdeaea;
      color: var(--danger);
      border: 1.5px solid #f5c6c6;
      border-radius: 10px;
      padding: 10px;
      font-size: 0.95rem;
      margin-top: 14px;
      display: none;
      text-align: center;
    }

    /* Results styling */
    .subjects-table {
      width: 100%;
      border-collapse: separate;
      border-spacing: 0 10px;
    }
    .subjects-table td {
      padding: 14px 16px;
      background: var(--white);
      border-radius: 12px;
      font-weight: 600;
    }
    .grade-badge {
      display: inline-block;
      padding: 4px 14px;
      border-radius: 20px;
      font-weight: 900;
    }
    .grade-high { background: #d4f5e2; color: #1a7a45; }
    .grade-mid  { background: #fff3cd; color: #856404; }
    .grade-low  { background: #fde8e8; color: #a11e1e; }

    .summary-box {
      background: linear-gradient(135deg, var(--navy), var(--blue));
      border-radius: 14px;
      padding: 18px 24px;
      display: flex;
      justify-content: space-between;
      color: var(--white);
      margin-top: 20px;
    }

    .screen { display: none; }
    .screen.active { display: block; }
  </style>
</head>
<body>

<div class="card">
  <div class="screen active" id="screen-login">
    <div class="logo-area">
      <div class="logo-icon">
        <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
          <path d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5"/>
        </svg>
      </div>
      <h1>بوابة النتائج المدرسية</h1>
      <p>أدخل الرموز للاطلاع على نتيجتك</p>
    </div>

    <div class="form-group">
      <label>🔐 رمز المدرسة</label>
      <input type="password" id="school-code" placeholder="أدخل رمز المدرسة" value="ali2026">
    </div>

    <div class="form-group">
      <label>🎓 الرمز الشخصي</label>
      <input type="password" id="student-code" placeholder="أدخل رمزك الشخصي">
    </div>

    <button class="btn" onclick="doLogin()">🔍 عرض النتيجة</button>
    <div class="error-msg" id="error-msg"></div>
  </div>

  <div class="screen" id="screen-results">
    <div style="text-align:center; margin-bottom: 20px;">
      <h2 id="res-name" style="color:var(--navy); font-size:1.5rem;"></h2>
      <p style="color:#5a6a80;">تفاصيل النتيجة النهائية</p>
    </div>

    <table class="subjects-table">
      <tbody id="res-subjects"></tbody>
    </table>

    <div class="summary-box">
      <div>
        <div style="font-size:0.8rem; opacity:0.8;">المجموع الكلي</div>
        <div id="res-total" style="font-size:1.6rem; font-weight:900; color:var(--gold-light);"></div>
      </div>
      <div style="text-align:left">
        <div style="font-size:0.8rem; opacity:0.8;">المعدل</div>
        <div id="res-avg" style="font-size:1.6rem; font-weight:900; color:var(--gold-light);"></div>
      </div>
    </div>

    <button class="btn" style="margin-top:20px; background:var(--navy);" onclick="location.reload()">← العودة</button>
  </div>
</div>

<script>
  const SCHOOL_CODE = "ali2026";

  const students = [
    {
      name: "عبدالقادر أحمد خلف",
      code: "2026",
      subjects: [
        { name: "الرياضيات", grade: 46 },
        { name: "التربية الإسلامية", grade: 90 },
        { name: "اللغة الإنجليزية", grade: 35 }
      ]
    },
    {
      name: "أنس عباس زيدان",
      code: "2090",
      subjects: [
        { name: "الرياضيات", grade: 43 },
        { name: "التربية الإسلامية", grade: 92 },
        { name: "اللغة الإنجليزية", grade: 76 }
      ]
    },
    {
      name: "جاسم عبدالمنعم",
      code: "2070", // تم تحديث الرقم السري من 2080 إلى 2070
      subjects: [
        { name: "الرياضيات", grade: 90 },
        { name: "اللغة الإنجليزية", grade: 43 },
        { name: "اللغة العربية", grade: 74 }
      ]
    },
    {
      name: "وليد سعد احمد",
      code: "2001", // الطالب الجديد
      subjects: [
        { name: "الرياضيات", grade: 47 },
        { name: "التربية الإسلامية", grade: 92 },
        { name: "اللغة العربية", grade: 75 }
      ]
    }
  ];

  function doLogin() {
    const sc = document.getElementById('school-code').value.trim();
    const uc = document.getElementById('student-code').value.trim();
    const err = document.getElementById('error-msg');
    
    err.style.display = 'none';

    if (sc !== SCHOOL_CODE) {
      err.textContent = '❌ رمز المدرسة غير صحيح';
      err.style.display = 'block';
      return;
    }

    const student = students.find(s => s.code === uc);
    if (!student) {
      err.textContent = '❌ الرمز الشخصي غير صحيح';
      err.style.display = 'block';
      return;
    }

    renderResults(student);
    document.getElementById('screen-login').classList.remove('active');
    document.getElementById('screen-results').classList.add('active');
  }

  function renderResults(student) {
    document.getElementById('res-name').textContent = student.name;
    const tbody = document.getElementById('res-subjects');
    tbody.innerHTML = '';
    let total = 0;

    student.subjects.forEach(sub => {
      total += sub.grade;
      const cls = sub.grade >= 75 ? 'grade-high' : sub.grade >= 50 ? 'grade-mid' : 'grade-low';
      tbody.innerHTML += `
        <tr>
          <td>${sub.name}</td>
          <td style="text-align:center">
            <span class="grade-badge ${cls}">${sub.grade}</span>
          </td>
        </tr>`;
    });

    const avg = (total / student.subjects.length).toFixed(1);
    document.getElementById('res-total').textContent = total;
    document.getElementById('res-avg').textContent = avg + '%';
  }
</script>

</body>
</html>
