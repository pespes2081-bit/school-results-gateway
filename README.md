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

    .step-label {
      font-size: 0.82rem;
      font-weight: 700;
      color: var(--gold);
      margin-bottom: 6px;
      text-transform: uppercase;
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
      margin-top: 4px;
    }
    .btn:hover { opacity: 0.92; transform: translateY(-2px); }

    .btn-admin {
      background: linear-gradient(135deg, #2c2c54, #474787);
      margin-top: 10px;
    }

    .error-msg {
      background: #fdeaea;
      color: var(--danger);
      border: 1.5px solid #f5c6c6;
      border-radius: 10px;
      padding: 10px 16px;
      font-size: 0.95rem;
      margin-top: 14px;
      display: none;
      text-align: center;
    }

    .subjects-table {
      width: 100%;
      border-collapse: separate;
      border-spacing: 0 10px;
      margin-bottom: 22px;
    }
    .subjects-table td {
      padding: 14px 16px;
      background: var(--white);
      font-weight: 600;
      color: var(--navy);
    }
    .subjects-table tr td:first-child { border-radius: 0 12px 12px 0; }
    .subjects-table tr td:last-child  { border-radius: 12px 0 0 12px; text-align: center; }

    .grade-badge {
      display: inline-block;
      padding: 4px 14px;
      border-radius: 20px;
      font-size: 1.05rem;
      font-weight: 900;
    }
    .grade-high   { background: #d4f5e2; color: #1a7a45; }
    .grade-mid    { background: #fff3cd; color: #856404; }
    .grade-low    { background: #fde8e8; color: #a11e1e; }

    .summary-box {
      background: linear-gradient(135deg, var(--navy), var(--blue));
      border-radius: 14px;
      padding: 18px 24px;
      display: flex;
      justify-content: space-between;
      color: var(--white);
      margin-bottom: 22px;
    }
    .summary-box .value { font-size: 1.8rem; font-weight: 900; color: var(--gold-light); }

    .screen { display: none; }
    .screen.active { display: block; }

    .logout-btn {
      background: transparent;
      border: 2px solid var(--navy);
      color: var(--navy);
      width: 100%;
      padding: 11px;
      border-radius: 10px;
      font-weight: 700;
      cursor: pointer;
    }
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
    </div>

    <div class="step-label">الخطوة ١</div>
    <div class="form-group">
      <label>🔐 رمز المدرسة</label>
      <input type="password" id="school-code" placeholder="أدخل رمز المدرسة" autocomplete="off" />
    </div>

    <div class="step-label">الخطوة ٢</div>
    <div class="form-group">
      <label>🎓 الرمز الشخصي</label>
      <input type="password" id="student-code" placeholder="أدخل رمزك الشخصي" autocomplete="off" />
    </div>

    <button class="btn" onclick="doLogin()">🔍 عرض النتيجة</button>
    <button class="btn btn-admin" onclick="doAdminLogin()">⚙️ دخول المدير</button>
    <div class="error-msg" id="error-msg"></div>
  </div>

  <div class="screen" id="screen-results">
    <div class="student-header" style="text-align:center; margin-bottom:28px;">
      <h2 id="res-name" style="color:var(--navy); font-size:1.4rem;"></h2>
      <p style="color:#5a6a80;">نتيجة الطالب النهائية</p>
    </div>

    <table class="subjects-table">
      <tbody id="res-subjects"></tbody>
    </table>

    <div class="summary-box">
      <div>
        <div style="font-size:0.9rem; opacity:0.8;">المجموع الكلي</div>
        <div class="value" id="res-total"></div>
      </div>
      <div style="text-align:left">
        <div style="font-size:0.9rem; opacity:0.8;">المعدل</div>
        <div class="value" id="res-avg"></div>
      </div>
    </div>

    <button class="logout-btn" onclick="location.reload()">← خروج</button>
  </div>
</div>

<script>
  const SCHOOL_CODE = "ali2026";
  const ADMIN_CODE  = "2027";

  const students = [
    {
      name: "عبدالقادر أحمد خلف",
      code: "2026",
      subjects: [
        { name: "الرياضيات",  grade: 46 },
        { name: "التربية الإسلامية", grade: 90 },
        { name: "اللغة الإنجليزية", grade: 35 }
      ]
    },
    {
      name: "أنس عباس زيدان",
      code: "2090",
      subjects: [
        { name: "الرياضيات",  grade: 43 },
        { name: "التربية الإسلامية", grade: 92 },
        { name: "اللغة الإنجليزية", grade: 76 }
      ]
    },
    {
      name: "جاسم عبدالمنعم",
      code: "2070", // تم تحديث الرمز
      subjects: [
        { name: "الرياضيات",  grade: 90 },
        { name: "اللغة الإنجليزية", grade: 43 },
        { name: "اللغة العربية", grade: 74 }
      ]
    },
    {
      name: "وليد سعد احمد", // الطالب الجديد
      code: "2001",
      subjects: [
        { name: "الرياضيات",  grade: 47 },
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
      err.style.display = 'block'; return;
    }

    const student = students.find(s => s.code === uc);
    if (!student) {
      err.textContent = '❌ الرمز الشخصي غير صحيح';
      err.style.display = 'block'; return;
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
      tbody.innerHTML += `<tr><td>${sub.name}</td><td><span class="grade-badge ${cls}">${sub.grade}</span></td></tr>`;
    });

    document.getElementById('res-total').textContent = total;
    document.getElementById('res-avg').textContent = (total / student.subjects.length).toFixed(1) + '%';
  }

  function doAdminLogin() {
    const sc = document.getElementById('school-code').value.trim();
    const ac = document.getElementById('student-code').value.trim();
    if (sc === SCHOOL_CODE && ac === ADMIN_CODE) {
        alert("قائمة الأكواد:\n" + students.map(s => s.name + ": " + s.code).join("\n"));
    } else {
        alert("بيانات الإدارة غير صحيحة");
    }
  }
</script>

</body>
</html>
