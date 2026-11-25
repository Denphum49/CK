
<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #f3f6fc;
  color: #26354d;
}

.sidebar {
  width: 220px;
  height: 100vh;
  position: fixed;
  background: white;
  padding: 25px 15px;
  border-right: 1px solid #e5eaf2;
}

.logo {
  color: #2563eb;
  font-size: 22px;
  font-weight: bold;
  margin-bottom: 30px;
}

.sidebar button {
  display: block;
  width: 100%;
  padding: 13px;
  margin-bottom: 8px;
  border: none;
  border-radius: 9px;
  background: transparent;
  text-align: left;
  cursor: pointer;
  color: #526078;
}

.sidebar button:hover,
.sidebar button.active {
  background: #eaf2ff;
  color: #2563eb;
}

main {
  margin-left: 220px;
  padding: 30px;
}

h1 {
  margin-top: 0;
  font-size: 25px;
}

.page {
  display: none;
}

.page.active {
  display: block;
}

.card {
  background: white;
  padding: 20px;
  border-radius: 13px;
  margin-bottom: 18px;
  box-shadow: 0 2px 10px #172b4d0a;
}

.stats {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 15px;
}

.number {
  font-size: 30px;
  color: #2563eb;
  font-weight: bold;
}

input, select, textarea {
  width: 100%;
  padding: 11px;
  border: 1px solid #dce3ef;
  border-radius: 8px;
  margin: 7px 0 13px;
  font-size: 14px;
}

textarea {
  min-height: 75px;
}

label {
  font-size: 13px;
  color: #526078;
}

.primary {
  background: #2563eb;
  color: white;
  padding: 11px 18px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
}

table {
  width: 100%;
  border-collapse: collapse;
  font-size: 13px;
}

th, td {
  padding: 12px;
  border-bottom: 1px solid #edf0f5;
  text-align: left;
}

th {
  color: #667085;
  background: #f8faff;
}

.notice {
  background: #fff5d9;
  color: #765600;
  padding: 12px;
  border-radius: 8px;
  font-size: 12px;
  margin-bottom: 15px;
}

@media(max-width:700px) {
  .sidebar {
    width: 100%;
    height: auto;
    position: relative;
  }

  .sidebar button {
    display: inline-block;
    width: auto;
  }

  main {
    margin-left: 0;
    padding: 15px;
  }

  .stats {
    grid-template-columns: 1fr;
  }
}
</style>
</head>

<body>

<div class="sidebar">
  <div class="logo">✚ MediCare HIS</div>

  <button class="active" onclick="showPage('dashboard',this)">▦ ภาพรวม</button>
  <button onclick="showPage('patients',this)">♙ ผู้ป่วย</button>
  <button onclick="showPage('visits',this)">✚ การรักษา</button>
  <button onclick="showPage('meds',this)">▤ จ่ายยา</button>
  <button onclick="showPage('ai',this)">✧ AI สรุปข้อมูล</button>
</div>

<main>

<div class="notice">
  ระบบ DEMO ใช้ข้อมูลสมมติเท่านั้น ไม่มีระบบล็อกอินและการควบคุมสิทธิ์สำหรับเวชระเบียนจริง
</div>

<section id="dashboard" class="page active">
  <h1>ภาพรวมระบบ</h1>

  <div class="stats">
    <div class="card">
      ผู้ป่วยทั้งหมด
      <div class="number" id="countPatients">0</div>
    </div>

    <div class="card">
      ประวัติการรักษา
      <div class="number" id="countVisits">0</div>
    </div>

    <div class="card">
      รายการจ่ายยา
      <div class="number" id="countMeds">0</div>
    </div>
  </div>

  <div class="card">
    <h3>ผู้ป่วยล่าสุด</h3>
    <div id="latest"></div>
    <br>
    <a id="sheetLink" target="_blank">เปิด Google Sheets</a>
  </div>
</section>

<section id="patients" class="page">
  <h1>ทะเบียนผู้ป่วย</h1>

  <div class="card">
    <h3>เพิ่มผู้ป่วย</h3>

    <form id="patientForm">
      <label>ชื่อ-นามสกุล</label>
      <input name="name" required>

      <label>อายุ</label>
      <input name="age" type="number">

      <label>เพศ</label>
      <select name="gender">
        <option>ไม่ระบุ</option>
        <option>ชาย</option>
        <option>หญิง</option>
        <option>อื่นๆ</option>
      </select>

      <label>ข้อมูลติดต่อสมมติ</label>
      <input name="contact">

      <label>แพ้ยา / โรคประจำตัว</label>
      <textarea name="allergy"></textarea>

      <button class="primary">บันทึกผู้ป่วย</button>
    </form>

    <p id="patientMsg"></p>
  </div>

  <div class="card">
    <h3>รายชื่อผู้ป่วย</h3>
    <div id="patientTable"></div>
  </div>
</section>

<section id="visits" class="page">
  <h1>บันทึกการรักษา</h1>

  <div class="card">
    <form id="visitForm">

      <label>HN ผู้ป่วย</label>
      <select name="hn" id="visitHN" required></select>

      <label>วันที่</label>
      <input name="visitDate" type="date" required>

      <label>อาการ</label>
      <textarea name="symptoms"></textarea>

      <label>สัญญาณชีพ</label>
      <input name="vitals">

      <label>การวินิจฉัย (ข้อมูลฝึก)</label>
      <input name="diagnosis">

      <label>ผู้บันทึก</label>
      <input name="recorder">

      <label>แผนการรักษา</label>
      <textarea name="plan"></textarea>

      <button class="primary">บันทึกการรักษา</button>
    </form>

    <p id="visitMsg"></p>
  </div>

  <div class="card">
    <h3>ประวัติการรักษา</h3>
    <div id="visitTable"></div>
  </div>
</section>

<section id="meds" class="page">
  <h1>ระบบจ่ายยา</h1>

  <div class="card">
    <form id="medForm">

      <label>HN ผู้ป่วย</label>
      <select name="hn" id="medHN" required></select>

      <label>วันที่</label>
      <input name="medDate" type="date" required>

      <label>ชื่อยา (ข้อมูลสมมติ)</label>
      <input name="drugName" required>

      <label>จำนวน</label>
      <input name="quantity">

      <label>วิธีใช้ตามใบสั่งจำลอง</label>
      <input name="instructions">

      <label>ผู้จ่ายยา</label>
      <input name="dispenser">

      <label>หมายเหตุ</label>
      <textarea name="note"></textarea>

      <button class="primary">บันทึกการจ่ายยา</button>
    </form>

    <p id="medMsg"></p>
  </div>

  <div class="card">
    <h3>ประวัติการจ่ายยา</h3>
    <div id="medTable"></div>
  </div>
</section>

<section id="ai" class="page">
  <h1>AI สรุปข้อมูลผู้ป่วย</h1>

  <div class="card">
    <label>เลือกผู้ป่วย</label>
    <select id="aiHN"></select>

    <button class="primary" onclick="analyze()">สรุปข้อมูล</button>

    <pre id="aiResult" style="white-space:pre-wrap"></pre>
  </div>
</section>

</main>

<script>
let db = {
  patients: [],
  visits: [],
  meds: []
};

function showPage(id,button) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.sidebar button').forEach(b => b.classList.remove('active'));

  document.getElementById(id).classList.add('active');
  button.classList.add('active');
}

function esc(value) {
  return String(value ?? '').replace(/[&<>"']/g, c => ({
    '&':'&amp;',
    '<':'&lt;',
    '>':'&gt;',
    '"':'&quot;',
    "'":'&#39;'
  }[c]));
}

function loadData() {
  google.script.run
    .withSuccessHandler(data => {
      db = data;
      render();
      document.getElementById('sheetLink').href = data.spreadsheetUrl;
    })
    .withFailureHandler(err => alert(err.message))
    .getInitialData();
}

function render() {
  document.getElementById('countPatients').textContent = db.patients.length;
  document.getElementById('countVisits').textContent = db.visits.length;
  document.getElementById('countMeds').textContent = db.meds.length;

  makeTable('latest',
    ['HN','ชื่อ','อายุ'],
    db.patients.slice(0,5).map(p => [p.HN,p.Name,p.Age])
  );

  makeTable('patientTable',
    ['HN','ชื่อ','อายุ','เพศ','แพ้ยา/โรคประจำตัว'],
    db.patients.map(p => [p.HN,p.Name,p.Age,p.Gender,p.Allergy])
  );

  makeTable('visitTable',
    ['วันที่','HN','อาการ','สัญญาณชีพ','การวินิจฉัย','ผู้บันทึก'],
    db.visits.map(v => [v.VisitDate,v.HN,v.Symptoms,v.Vitals,v.Diagnosis,v.Recorder])
  );

  makeTable('medTable',
    ['วันที่','HN','ยา','จำนวน','วิธีใช้'],
    db.meds.map(m => [m.MedDate,m.HN,m.DrugName,m.Quantity,m.Instructions])
  );

  ['visitHN','medHN','aiHN'].forEach(id => {
    const select = document.getElementById(id);
    const oldValue = select.value;

    select.innerHTML = '<option value="">-- เลือกผู้ป่วย --</option>';

    db.patients.forEach(p => {
      const option = document.createElement('option');
      option.value = p.HN;
      option.textContent = p.HN + ' - ' + p.Name;
      select.appendChild(option);
    });

    select.value = oldValue;
  });
}

function makeTable(id,headers,rows) {
  const el = document.getElementById(id);

  if (!rows.length) {
    el.innerHTML = '<p>ยังไม่มีข้อมูล</p>';
    return;
  }

  el.innerHTML = `
    <table>
      <tr>${headers.map(h => `<th>${esc(h)}</th>`).join('')}</tr>
      ${rows.map(row => `
        <tr>${row.map(v => `<td>${esc(v)}</td>`).join('')}</tr>
      `).join('')}
    </table>
  `;
}

function submitForm(formId,serverFunction,messageId) {
  const form = document.getElementById(formId);

  form.addEventListener('submit',event => {
    event.preventDefault();

    const data = Object.fromEntries(new FormData(form).entries());
    const message = document.getElementById(messageId);
    const button = form.querySelector('button');

    button.disabled = true;
    message.textContent = 'กำลังบันทึก...';

    google.script.run
      .withSuccessHandler(result => {
        message.textContent = 'บันทึกข้อมูลเรียบร้อยแล้ว';
        form.reset();
        button.disabled = false;
        loadData();

        if (result && result.hn) {
          message.textContent += ' HN: ' + result.hn;
        }
      })
      .withFailureHandler(err => {
        message.textContent = err.message;
        button.disabled = false;
      })[serverFunction](data);
  });
}

submitForm('patientForm','addPatient','patientMsg');
submitForm('visitForm','addVisit','visitMsg');
submitForm('medForm','addMedication','medMsg');

function analyze() {
  const hn = document.getElementById('aiHN').value;
  const patient = db.patients.find(p => p.HN === hn);

  if (!patient) {
    alert('กรุณาเลือกผู้ป่วย');
    return;
  }

  const visits = db.visits.filter(v => v.HN === hn);
  const meds = db.meds.filter(m => m.HN === hn);

  let result = `
สรุปข้อมูลผู้ป่วย
HN: ${patient.HN}
ชื่อ: ${patient.Name}
อายุ: ${patient.Age}
เพศ: ${patient.Gender}
แพ้ยา/โรคประจำตัว: ${patient.Allergy || 'ไม่มีข้อมูล'}

ประวัติการรักษา: ${visits.length} รายการ
รายการยา: ${meds.length} รายการ

รายการตรวจสอบ:
${patient.Allergy ? 'มีข้อมูลแพ้ยา/โรคประจำตัว' : 'ควรตรวจสอบข้อมูลแพ้ยา/โรคประจำตัว'}
${visits.length ? 'มีประวัติการรักษา' : 'ยังไม่มีประวัติการรักษา'}

หมายเหตุ: นี่เป็นเพียงการสรุปข้อมูลพื้นฐาน ไม่ใช่ AI วินิจฉัยโรค
  `;

  document.getElementById('aiResult').textContent = result;
}

loadData();
</script>

</body>
</html>

