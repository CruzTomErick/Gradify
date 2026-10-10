<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Gradify — Student Grade Management System</title>
<style>
:root{
  --navy:#152c4f;--navy2:#1d3c67;--blue:#3469c8;--blue-light:#edf4ff;
  --bg:#f5f7fb;--surface:#fff;--text:#1e293b;--muted:#6b7890;--line:#e4e9f1;
  --green:#16845b;--green-bg:#e8f7ef;--red:#c2414b;--red-bg:#fff0f0;
  --amber:#a66b12;--amber-bg:#fff6df;--radius:15px;--shadow:0 5px 22px rgba(26,48,82,.045)
}
*{box-sizing:border-box}
html{scroll-behavior:smooth}
body{margin:0;background:var(--bg);font-family:Inter,ui-sans-serif,system-ui,-apple-system,"Segoe UI",Arial,sans-serif;color:var(--text);font-size:14px}
button,input,select,textarea{font:inherit}
button{cursor:pointer}
.app{min-height:100vh;display:grid;grid-template-columns:248px minmax(0,1fr)}
.sidebar{background:var(--navy);color:#fff;padding:25px 16px 18px;display:flex;flex-direction:column;min-height:100vh;position:sticky;top:0;height:100vh;overflow-y:auto}
.brand{display:flex;gap:11px;align-items:center;padding:0 10px 26px;border-bottom:1px solid rgba(255,255,255,.13)}
.brand-mark{width:38px;height:38px;border-radius:12px;background:#fff;color:var(--blue);display:grid;place-items:center;font-weight:850;font-size:19px}
.brand-name{font-weight:800;font-size:19px;letter-spacing:.1px}.brand-sub{font-size:10px;color:#c8d5e8;margin-top:3px}
.school{margin:20px 10px 8px;color:#aebfd7;font-size:10px;text-transform:uppercase;letter-spacing:1.3px;font-weight:750}
.school-name{margin:0 10px 20px;font-size:12px;line-height:1.5;color:#f2f6fc;font-weight:650}
.nav-label{color:#91a7c7;font-size:10px;font-weight:750;letter-spacing:1.2px;padding:0 12px 9px}
.nav{display:grid;gap:5px}
.nav button{display:flex;align-items:center;gap:12px;border:0;background:transparent;color:#d2ddef;padding:12px;border-radius:10px;text-align:left;width:100%;font-weight:600;transition:.15s}
.nav button:hover,.nav button.active{background:#2c4b77;color:white}
.nav .ico{width:19px;text-align:center;font-size:16px}
.side-bottom{margin-top:auto;padding-top:22px}
.period{border:1px solid rgba(255,255,255,.16);background:rgba(255,255,255,.055);border-radius:12px;padding:14px;margin:0 4px 16px}
.period .tiny{font-size:10px;color:#aebfd7;font-weight:750;letter-spacing:1px}.period strong{display:block;margin:7px 0 5px;font-size:13px}.period p{font-size:11px;color:#c4d2e5;margin:0;line-height:1.5}
.teacher-mini{display:flex;gap:10px;align-items:center;padding:14px 8px 2px;border-top:1px solid rgba(255,255,255,.13)}
.avatar{width:36px;height:36px;border-radius:50%;background:#dce9ff;color:#244f92;display:grid;place-items:center;font-weight:800;flex-shrink:0}
.teacher-mini strong{font-size:12px;display:block}.teacher-mini small{font-size:11px;color:#aebfd7}
main{min-width:0}
.topbar{height:72px;background:#fff;border-bottom:1px solid var(--line);display:flex;align-items:center;justify-content:space-between;padding:0 clamp(18px,3vw,38px);gap:16px}
.breadcrumb{font-size:12px;color:var(--muted)}.breadcrumb b{color:#293a55}
.top-actions{display:flex;align-items:center;gap:13px;color:#4b5870;font-size:12px;flex-wrap:wrap;justify-content:flex-end}
.pill{background:#f1f5fb;border:1px solid var(--line);padding:8px 10px;border-radius:9px;white-space:nowrap}
.top-teacher{display:flex;gap:9px;align-items:center}.top-teacher .avatar{width:32px;height:32px}
.content{padding:28px clamp(18px,3vw,38px) 32px;max-width:1600px;margin:auto}
.page-heading{display:flex;align-items:flex-start;justify-content:space-between;gap:16px;margin-bottom:24px;flex-wrap:wrap}
.eyebrow{font-size:11px;text-transform:uppercase;letter-spacing:1.15px;font-weight:800;color:var(--blue);margin-bottom:7px}
h1{font-size:clamp(23px,2.2vw,29px);letter-spacing:-.7px;margin:0;color:#172943}
.subtitle{color:var(--muted);margin:8px 0 0;line-height:1.55;font-size:13px}
.btn{border:1px solid var(--line);background:#fff;color:#34435b;border-radius:9px;padding:10px 14px;font-weight:700;display:inline-flex;gap:8px;align-items:center;justify-content:center;transition:.15s}
.btn:hover{transform:translateY(-1px);box-shadow:var(--shadow)}.btn-primary{background:var(--blue);border-color:var(--blue);color:#fff}.btn-dark{background:var(--navy);border-color:var(--navy);color:#fff}.btn-danger{color:var(--red);border-color:#f2cccc}
.cards{display:grid;grid-template-columns:repeat(4,minmax(0,1fr));gap:15px;margin-bottom:20px}
.card,.panel{background:var(--surface);border:1px solid var(--line);border-radius:var(--radius);box-shadow:var(--shadow);min-width:0}
.metric{padding:19px;position:relative;overflow:hidden}.metric-top{display:flex;align-items:center;justify-content:space-between;gap:8px;color:var(--muted);font-size:12px;font-weight:650}
.metric-icon{height:34px;width:34px;border-radius:10px;background:var(--blue-light);color:var(--blue);display:grid;place-items:center;font-size:17px;flex-shrink:0}
.metric-value{font-size:28px;font-weight:800;letter-spacing:-.8px;margin:15px 0 5px;color:#192b45}
.metric-note{font-size:11px;color:var(--muted);line-height:1.45}
.good{color:var(--green)}.bad{color:var(--red)}
.two-col{display:grid;grid-template-columns:minmax(0,1.05fr) minmax(0,1fr);gap:17px;margin-bottom:20px}
.panel-head{display:flex;align-items:flex-start;justify-content:space-between;gap:12px;padding:20px 21px 14px}
.panel h2{font-size:15px;margin:0;font-weight:800;color:#1c2d47}.panel-desc{font-size:11px;color:var(--muted);margin:6px 0 0}
.panel-body{padding:0 21px 20px;min-width:0}
.performance{display:grid;gap:17px;padding-top:9px}
.bar-row{display:grid;grid-template-columns:76px minmax(0,1fr) auto;gap:10px;align-items:center;font-size:12px}
.bar-track{height:9px;background:#edf1f7;border-radius:99px;overflow:hidden}.bar-fill{height:100%;border-radius:99px;background:var(--green)}.bar-fill.fail{background:#e16b71}
.bar-meta{color:var(--muted);font-size:11px;white-space:nowrap}
.note{background:#f7f9fd;border:1px solid var(--line);padding:12px;border-radius:10px;font-size:11px;line-height:1.55;color:#63718a;margin-top:18px}
.table-wrap{width:100%;overflow-x:auto}
table{border-collapse:collapse;width:100%;min-width:540px;text-align:left}
th{font-size:10px;color:#77849a;letter-spacing:.65px;text-transform:uppercase;background:#fafbfd;font-weight:800;padding:13px 16px;border-top:1px solid var(--line);border-bottom:1px solid var(--line);white-space:nowrap}
td{padding:14px 16px;border-bottom:1px solid #edf0f5;font-size:12px;color:#35435a;vertical-align:middle}
tbody tr:last-child td{border-bottom:0}tbody tr:hover{background:#fbfcff}
.status{display:inline-flex;padding:5px 8px;border-radius:20px;font-size:10px;font-weight:800;white-space:nowrap}
.status.pass{background:var(--green-bg);color:var(--green)}.status.fail{background:var(--red-bg);color:var(--red)}.status.neutral{background:#edf2fa;color:#52637c}
.student-name{font-weight:750;color:#20324e}.student-id{font-size:10px;color:var(--muted);margin-top:4px}
.action-link{background:none;border:0;color:var(--blue);font-weight:750;font-size:11px;padding:4px 0}
.table-tools{display:flex;align-items:center;justify-content:space-between;gap:12px;padding:0 20px 15px;flex-wrap:wrap}
.field{border:1px solid #dbe2ec;background:#fff;border-radius:9px;padding:10px 12px;min-width:0;color:#334155;outline:none}
.field:focus{border-color:#7ca1e9;box-shadow:0 0 0 3px #e8efff}
.search{width:min(280px,100%)}
.toolbar{display:flex;gap:9px;flex-wrap:wrap}
.form-grid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:17px}
.form-field{display:grid;gap:7px}.form-field label{font-size:12px;font-weight:750;color:#45546b}.form-field input,.form-field select,.form-field textarea{width:100%}
.section-cards{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:14px;margin-bottom:20px}
.section-card{padding:18px;cursor:pointer;border:1px solid var(--line);border-radius:13px;background:#fff}.section-card.selected{border-color:#8eafea;background:#f5f8ff}.section-card h3{margin:0 0 8px;font-size:15px}.section-card p{margin:0;color:var(--muted);font-size:11px}
.section-stats{display:flex;justify-content:space-between;gap:10px;margin-top:17px;padding-top:13px;border-top:1px solid var(--line);font-size:11px;color:#53627a}
.empty{padding:35px;text-align:center;color:var(--muted)}
.footer{color:#8a95a8;font-size:10px;text-align:center;margin-top:28px;padding-top:18px;border-top:1px solid var(--line)}
.toast{position:fixed;right:22px;bottom:22px;background:#17375f;color:#fff;padding:13px 18px;border-radius:11px;box-shadow:0 12px 35px #0f274433;z-index:10;opacity:0;transform:translateY(10px);pointer-events:none;transition:.25s}.toast.show{opacity:1;transform:translateY(0)}
.mobile-menu{display:none}
@media(max-width:1120px){.app{grid-template-columns:220px minmax(0,1fr)}.sidebar{padding-inline:12px}.cards{grid-template-columns:repeat(2,minmax(0,1fr))}.two-col{grid-template-columns:minmax(0,1fr)}}
@media(max-width:760px){.app{display:block}.sidebar{position:relative;height:auto;min-height:0;padding:13px 14px}.brand{padding:0 5px 12px;border:0}.school,.school-name,.nav-label,.side-bottom{display:none}.nav{grid-template-columns:repeat(3,minmax(0,1fr));margin-top:12px}.nav button{justify-content:center;padding:10px 5px;font-size:11px;gap:6px}.nav .ico{font-size:14px;width:auto}.topbar{height:auto;min-height:60px;padding:13px 16px}.top-actions{gap:7px}.top-actions .pill:nth-child(2){display:none}.content{padding:22px 15px}.cards{grid-template-columns:repeat(2,minmax(0,1fr));gap:10px}.metric{padding:14px}.metric-value{font-size:24px}.metric-top{font-size:11px}.metric-icon{width:29px;height:29px}.page-heading{margin-bottom:19px}.section-cards,.form-grid{grid-template-columns:minmax(0,1fr)}.panel-head{padding:17px 16px 13px}.panel-body{padding:0 16px 17px}.table-tools{padding:0 16px 14px}.breadcrumb{display:none}}
@media(max-width:390px){.cards{grid-template-columns:minmax(0,1fr)}.nav button{font-size:10px}.top-teacher span{display:none}}
</style>
</head>
<body>
<div class="app">
<aside class="sidebar">
  <div class="brand"><div class="brand-mark">G</div><div><div class="brand-name">Gradify</div><div class="brand-sub">Student Grade Management System</div></div></div>
  <div class="school">School workspace</div><div class="school-name">Asia Source iCollege<br>Pasig City Branch</div>
  <div class="nav-label">TEACHER WORKSPACE</div>
  <nav class="nav" id="nav">
    <button class="active" data-page="Dashboard"><span class="ico">▦</span>Dashboard</button>
    <button data-page="Students"><span class="ico">♙</span>Students</button>
    <button data-page="Grades"><span class="ico">▤</span>Grades</button>
    <button data-page="Reports"><span class="ico">▥</span>Reports</button>
    <button data-page="Profile"><span class="ico">⚙</span>Profile</button>
    <button data-page="Logout"><span class="ico">↪</span>Logout</button>
  </nav>
  <div class="side-bottom">
    <div class="period"><div class="tiny">ACADEMIC PERIOD</div><strong>2026–2027 · Quarter 1</strong><p>Passing threshold: 75</p></div>
    <div class="teacher-mini"><div class="avatar">MC</div><div><strong>Maria D. Cruz</strong><small>Grade 7 teacher</small></div></div>
  </div>
</aside>
<main>
<header class="topbar">
  <div class="breadcrumb">Teacher portal&nbsp; / &nbsp;<b id="crumb">Dashboard</b></div>
  <div class="top-actions"><span class="pill">Quarter 1</span><span class="pill">SY 2026–2027</span><div class="top-teacher"><div class="avatar">MC</div><span>Maria D. Cruz</span></div></div>
</header>
<div class="content" id="view"></div>
</main>
</div>
<div class="toast" id="toast" role="status"></div>
<script>
const students=[
{id:"2026-001",name:"Emma Santos",full:"Emma Louise Santos",section:"7 · Acacia",avg:88,subjects:[88,90,86,92,89,83]},
{id:"2026-002",name:"Noah Reyes",full:"Noah Reyes",section:"7 · Acacia",avg:72,subjects:[70,74,72,76,73,67]},
{id:"2026-003",name:"Olivia Cruz",full:"Olivia Cruz",section:"7 · Acacia",avg:84,subjects:[84,86,82,87,85,80]},
{id:"2026-004",name:"Liam Garcia",full:"Liam Garcia",section:"7 · Acacia",avg:91,subjects:[91,92,89,93,90,91]},
{id:"2026-005",name:"Jester Cruz",full:"Jester Cruz",section:"7 · Narra",avg:88,subjects:[88,90,86,89,87,88]},
{id:"2026-006",name:"Thomas Erickson",full:"Thomas Erickson",section:"7 · Narra",avg:82,subjects:[80,82,84,83,81,82]},
{id:"2026-007",name:"Isabella Torres",full:"Isabella Torres",section:"7 · Narra",avg:85,subjects:[84,87,85,86,83,85]},
{id:"2026-008",name:"Ethan Flores",full:"Ethan Flores",section:"7 · Narra",avg:81,subjects:[79,83,82,80,81,81]}
];
const subjects=["English","Mathematics","Science","Filipino","Araling Panlipunan","MAPEH"];
let page="Dashboard",selectedStudent=students[1],selectedSection="7 · Acacia";
const view=document.getElementById("view");
const pass=s=>s.avg>=75;
const status=s=>`<span class="status ${pass(s)?"pass":"fail"}">${pass(s)?"Passing":"Failing"}</span>`;
const avg=s=>(s.subjects.reduce((a,b)=>a+b,0)/6).toFixed(2);
function toast(msg){const t=document.getElementById("toast");t.textContent=msg;t.classList.add("show");setTimeout(()=>t.classList.remove("show"),2400)}
function heading(title,desc,button=""){return `<div class="page-heading"><div><div class="eyebrow">Gradify · Teacher workspace</div><h1>${title}</h1><p class="subtitle">${desc}</p></div>${button}</div>`}
function tableRows(list,mode="students"){return list.map(s=>`<tr><td><div class="student-name">${s.name}</div><div class="student-id">${s.id}</div></td><td>${s.section}</td><td><b>${avg(s)}</b></td><td>${status(s)}</td><td><button class="action-link" data-student="${s.id}">${mode==="grades"?"View Grades":"View Student"} →</button></td></tr>`).join("")}
function dashboard(){let failing=students.filter(s=>!pass(s)).length;return `${heading("Good morning, Maria","Here is your classroom overview for Quarter 1, SY 2026–2027.",'<button class="btn btn-primary" data-go="Grades">＋ Manage Grades</button>')}
<div class="cards">
<div class="card metric"><div class="metric-top">Enrolled students <span class="metric-icon">♙</span></div><div class="metric-value">8</div><div class="metric-note">Across 2 Grade 7 sections</div></div>
<div class="card metric"><div class="metric-top">Class general average <span class="metric-icon">▥</span></div><div class="metric-value">82.00</div><div class="metric-note">8 students · 6 subjects each</div></div>
<div class="card metric"><div class="metric-top">Passing students <span class="metric-icon">✓</span></div><div class="metric-value">7/8</div><div class="metric-note"><span class="good">87.5%</span> · threshold 75</div></div>
<div class="card metric"><div class="metric-top">Needs support <span class="metric-icon">!</span></div><div class="metric-value">${failing}</div><div class="metric-note">General average below 75</div></div>
</div>
<div class="two-col">
<section class="panel"><div class="panel-head"><div><h2>Quarter performance</h2><p class="panel-desc">General average distribution</p></div><span class="status neutral">Quarter 1</span></div><div class="panel-body"><div class="performance"><div class="bar-row"><b>Passing</b><div class="bar-track"><div class="bar-fill" style="width:87.5%"></div></div><span class="bar-meta">7 · 87.5%</span></div><div class="bar-row"><b>Failing</b><div class="bar-track"><div class="bar-fill fail" style="width:12.5%"></div></div><span class="bar-meta">1 · 12.5%</span></div></div><div class="note">Equal-weight subject averages. Passing is a general average of 75 or higher.</div></div></section>
<section class="panel"><div class="panel-head"><div><h2>Your sections</h2><p class="panel-desc">Section summary for the current quarter</p></div><button class="action-link" data-go="Students">View Students →</button></div><div class="table-wrap"><table style="min-width:410px"><thead><tr><th>Section</th><th>Students</th><th>Average</th><th>Passing</th></tr></thead><tbody><tr><td><b>7 · Acacia</b></td><td>4</td><td>83.75</td><td>3 of 4</td></tr><tr><td><b>7 · Narra</b></td><td>4</td><td>84.00</td><td>4 of 4</td></tr></tbody></table></div></section>
</div>
<section class="panel"><div class="panel-head"><div><h2>Student snapshot</h2><p class="panel-desc">Recently reviewed academic records · October 5, 2026</p></div><button class="btn" data-go="Reports">View reports</button></div><div class="table-wrap"><table><thead><tr><th>Student</th><th>Section</th><th>General average</th><th>Status</th><th>Action</th></tr></thead><tbody>${tableRows(students.slice(0,4))}</tbody></table></div><div class="panel-body"><div class="note"><b>Follow up with Noah Reyes</b><br>General average: 72.00. Review English, Mathematics, Science, Araling Panlipunan, and MAPEH before the next parent conference.</div></div></section>
${footer()}`}
function studentsPage(){return `${heading("Students","Manage student information and academic records by section.",'<button class="btn btn-primary" id="addStudent">＋ Add Student</button>')}
<div class="section-cards"><div class="section-card ${selectedSection==="7 · Acacia"?"selected":""}" data-section="7 · Acacia"><h3>7 · Acacia</h3><p>Grade 7 · Section overview</p><div class="section-stats"><span>4 students</span><b>Average 83.75</b><span>3 passing · 1 failing</span></div></div><div class="section-card ${selectedSection==="7 · Narra"?"selected":""}" data-section="7 · Narra"><h3>7 · Narra</h3><p>Grade 7 · Section overview</p><div class="section-stats"><span>4 students</span><b>Average 84.00</b><span>4 passing · 0 failing</span></div></div></div>
<section class="panel"><div class="panel-head"><div><h2>Student register</h2><p class="panel-desc">Grade 7 · ${selectedSection} · Quarter 1, SY 2026–2027</p></div></div><div class="table-tools"><input class="field search" id="studentSearch" placeholder="Search by name or student ID"><select class="field" id="statusFilter"><option value="all">All statuses</option><option value="pass">Passing</option><option value="fail">Failing</option></select></div><div class="table-wrap"><table><thead><tr><th>Student ID / Name</th><th>Section</th><th>Average</th><th>Status</th><th>Action</th></tr></thead><tbody id="studentRows">${tableRows(students.filter(s=>s.section===selectedSection))}</tbody></table></div><div class="panel-body"><div class="note">Open a student record to review grades, edit contact details, or view academic performance. Passing threshold: 75.</div></div></section>${footer()}`}
function gradesPage(){return `${heading("Grades","Select a section and student to view, enter, and calculate quarterly grades.")}
<div class="panel"><div class="panel-head"><div><h2>Grade register</h2><p class="panel-desc">Quarter 1 · SY 2026–2027</p></div></div><div class="table-tools"><select class="field" id="gradeSection"><option>7 · Acacia</option><option>7 · Narra</option></select><select class="field" id="gradeStudent">${students.map(s=>`<option value="${s.id}" ${s.id===selectedStudent.id?"selected":""}>${s.name} · ${s.id}</option>`).join("")}</select><button class="btn btn-primary" id="openEditor">Input / Edit Grades</button></div><div class="table-wrap"><table><thead><tr><th>Student</th><th>Subjects saved</th><th>General average</th><th>Status</th><th>Action</th></tr></thead><tbody>${students.map(s=>`<tr><td><div class="student-name">${s.name}</div><div class="student-id">${s.id}</div></td><td>6 / 6 saved</td><td><b>${avg(s)}</b></td><td>${status(s)}</td><td><button class="action-link" data-edit="${s.id}">View Grades →</button></td></tr>`).join("")}</tbody></table></div><div class="panel-body"><div class="note"><b>How the general average is calculated</b><br>Add the six subject averages and divide by 6. Each subject has equal weight. A general average of 75 or higher is Passing; below 75 is Failing.</div></div></div>${footer()}`}
function editorPage(){let s=selectedStudent;return `${heading("Student grade editor",`Review and enter ${s.name}’s Quarter 1 subject averages.`,'<button class="btn" data-go="Grades">← Back to Grades</button>')}
<div class="panel"><div class="panel-head"><div><h2>${s.name}</h2><p class="panel-desc">Student ID ${s.id} · Grade 7 · ${s.section} · Quarter 1</p></div>${status(s)}</div><div class="panel-body"><div class="form-grid" id="gradeFields">${subjects.map((sub,i)=>`<div class="form-field"><label>${sub} average *</label><input class="field grade-input" type="number" min="0" max="100" value="${s.subjects[i]}" data-index="${i}" required></div>`).join("")}</div><div class="note">Valid range: 0–100. All six fields are required. Each subject contributes equally to the general average.</div><div style="display:flex;justify-content:flex-end;gap:10px;margin-top:18px;flex-wrap:wrap"><button class="btn" data-go="Grades">Cancel</button><button class="btn btn-primary" id="saveGrades">Save Grades & Calculate Average</button></div></div></div>${footer()}`}
function reportsPage(){return `${heading("Reports","Review student academic performance reports for your sections.",'<button class="btn" id="exportReport">⇩ Export Section Summary</button>')}<div class="cards"><div class="card metric"><div class="metric-top">Total reports</div><div class="metric-value">8</div><div class="metric-note">All student records</div></div><div class="card metric"><div class="metric-top">Passing</div><div class="metric-value">7</div><div class="metric-note good">87.5% of students</div></div><div class="card metric"><div class="metric-top">Failing</div><div class="metric-value">1</div><div class="metric-note bad">Needs support</div></div><div class="card metric"><div class="metric-top">Class average</div><div class="metric-value">82.00</div><div class="metric-note">Passing threshold: 75</div></div></div><section class="panel"><div class="panel-head"><div><h2>Student academic reports</h2><p class="panel-desc">Quarter 1 · SY 2026–2027</p></div></div><div class="table-tools"><input class="field search" id="reportSearch" placeholder="Search student name or ID"></div><div class="table-wrap"><table><thead><tr><th>Student / Report ID</th><th>Section</th><th>Average</th><th>Status</th><th>Last updated</th><th>Action</th></tr></thead><tbody id="reportRows">${students.map(s=>`<tr><td><div class="student-name">${s.name}</div><div class="student-id">Q1-${s.id}</div></td><td>${s.section}</td><td>${avg(s)}</td><td>${status(s)}</td><td>Oct 5, 2026</td><td><button class="action-link" data-report="${s.id}">View Report →</button></td></tr>`).join("")}</tbody></table></div><div class="panel-body"><div class="note">Reports use the six saved, equally weighted subject averages. Passing threshold: 75.</div></div></section>${footer()}`}
function reportDetail(){let s=selectedStudent;return `${heading("Student academic report",`Quarter 1 report · Q1-${s.id} · Generated October 5, 2026`,'<button class="btn" data-go="Reports">← Back to Reports</button>')}<section class="panel"><div class="panel-head"><div><div class="eyebrow">Asia Source iCollege – Pasig City Branch</div><h2>Student Academic Performance Report</h2><p class="panel-desc">SY 2026–2027 · Quarter 1 · Issued October 5, 2026</p></div><button class="btn" onclick="window.print()">Print / Save PDF</button></div><div class="panel-body"><div class="two-col"><div><div class="panel-desc">STUDENT NAME</div><h2 style="margin-top:8px">${s.name}</h2><p class="subtitle">Student ID ${s.id} · Grade 7, ${s.section}</p></div><div><div class="panel-desc">PREPARED BY</div><h2 style="margin-top:8px">Maria D. Cruz</h2><p class="subtitle">Grade 7 teacher</p></div></div><div class="table-wrap"><table><thead><tr><th>Subject</th><th>Quarter 1 average</th><th>Weight</th><th>Remarks</th></tr></thead><tbody>${subjects.map((sub,i)=>`<tr><td>${sub}</td><td>${s.subjects[i].toFixed(2)}</td><td>Equal · 1 / 6</td><td><span class="status ${s.subjects[i]>=75?"pass":"fail"}">${s.subjects[i]>=75?"Passing":"Failing"}</span></td></tr>`).join("")}</tbody></table></div><div class="note"><b>General average: ${avg(s)} — ${pass(s)?"Passing":"Failing"}</b><br>${pass(s)?"Student is at or above the passing threshold of 75.":"Student is below the passing threshold of 75. Review subjects below 75 and plan appropriate support."}</div></div></section>${footer()}`}
function profilePage(){return `${heading("My profile","Manage your teacher account and contact information.",'<button class="btn btn-primary" id="editProfile">Edit Profile</button>')}<div class="two-col"><section class="panel"><div class="panel-head"><h2>Teacher account</h2></div><div class="panel-body"><div style="display:flex;align-items:center;gap:14px;margin-bottom:22px"><div class="avatar" style="width:56px;height:56px;font-size:18px">MC</div><div><h2>Maria D. Cruz</h2><p class="panel-desc">Grade 7 teacher · Active faculty account</p></div></div><div class="form-field"><label>Faculty ID</label><div class="field">ASIC-T-014</div></div><div class="form-field" style="margin-top:14px"><label>School</label><div class="field">Asia Source iCollege – Pasig City Branch</div></div><div class="form-field" style="margin-top:14px"><label>Assigned sections</label><div class="field">7 · Acacia &nbsp; / &nbsp; 7 · Narra</div></div></div></section><section class="panel"><div class="panel-head"><h2>Personal and contact information</h2></div><div class="panel-body"><div class="form-grid"><div class="form-field"><label>First name</label><input class="field" value="Maria"></div><div class="form-field"><label>Middle initial</label><input class="field" value="D."></div><div class="form-field"><label>Last name</label><input class="field" value="Cruz"></div><div class="form-field"><label>School email</label><input class="field" value="maria.cruz@school.edu.ph"></div><div class="form-field"><label>Mobile number</label><input class="field" value="0917 555 0140"></div><div class="form-field"><label>Role</label><input class="field" value="Grade 7 teacher" disabled></div></div><div style="display:flex;justify-content:flex-end;margin-top:18px"><button class="btn btn-primary" id="saveProfile">Save Changes</button></div></div></section></div>${footer()}`}
function logoutPage(){return `${heading("Log out","Are you sure you want to leave your Gradify teacher workspace?")}<section class="panel" style="max-width:560px;margin:30px auto"><div class="panel-body" style="padding:30px;text-align:center"><div class="metric-icon" style="margin:0 auto 18px;width:52px;height:52px;font-size:23px">↪</div><h2 style="font-size:21px">Logging out of Gradify?</h2><p class="subtitle">You are signed in as Maria D. Cruz. Your student records, grades, and profile updates are safe.</p><div style="display:flex;justify-content:center;gap:10px;margin-top:22px;flex-wrap:wrap"><button class="btn" data-go="Dashboard">Back to Dashboard</button><button class="btn btn-primary" id="confirmLogout">Log out</button></div></div></section>${footer()}`}
function loginPage(){return `<div style="max-width:470px;margin:5vh auto"><section class="panel"><div class="panel-body" style="padding:30px"><div class="brand" style="padding:0 0 25px;border-bottom:1px solid var(--line);color:var(--navy)"><div class="brand-mark" style="background:var(--blue);color:#fff">G</div><div><div class="brand-name">Gradify</div><div class="brand-sub" style="color:var(--muted)">Student Grade Management System</div></div></div><h1 style="margin-top:28px">Welcome back, teacher</h1><p class="subtitle">Log in to manage your classroom records.</p><form id="loginForm" style="display:grid;gap:15px;margin-top:25px"><div class="form-field"><label>School email</label><input class="field" type="email" placeholder="Enter your school email" required></div><div class="form-field"><label>Password</label><input class="field" type="password" placeholder="Enter your password" required></div><label style="display:flex;gap:8px;align-items:center;color:var(--muted);font-size:12px"><input type="checkbox"> Remember this device</label><button class="btn btn-primary" type="submit">Login</button></form><p class="subtitle" style="text-align:center;margin-top:22px">Secure access for authorized school faculty only.</p><p style="text-align:center;font-size:12px;color:var(--muted)">Need access to your account? Contact the school administrator.</p></div></section></div>`}
function footer(){return `<div class="footer">Gradify · Student Grade Management System · Asia Source iCollege – Pasig City Branch<br>© 2026 · Teacher workspace · Passing threshold: 75</div>`}
function render(p=page){page=p;document.querySelectorAll("#nav button").forEach(b=>b.classList.toggle("active",b.dataset.page===p||(p==="Grade Editor"&&b.dataset.page==="Grades")||(p==="Student Report"&&b.dataset.page==="Reports")));document.getElementById("crumb").textContent=p;let pages={Dashboard:dashboard,Students:studentsPage,Grades:gradesPage,"Grade Editor":editorPage,Reports:reportsPage,"Student Report":reportDetail,Profile:profilePage,Logout:logoutPage,Login:loginPage};view.innerHTML=(pages[p]||dashboard)();window.scrollTo({top:0,behavior:"smooth"});wire()}
function wire(){
 document.querySelectorAll("[data-page]").forEach(b=>b.onclick=()=>render(b.dataset.page));
 document.querySelectorAll("[data-go]").forEach(b=>b.onclick=()=>render(b.dataset.go));
 document.querySelectorAll("[data-student]").forEach(b=>b.onclick=()=>{selectedStudent=students.find(s=>s.id===b.dataset.student);render("Student Report")});
 document.querySelectorAll("[data-report]").forEach(b=>b.onclick=()=>{selectedStudent=students.find(s=>s.id===b.dataset.report);render("Student Report")});
 document.querySelectorAll("[data-edit]").forEach(b=>b.onclick=()=>{selectedStudent=students.find(s=>s.id===b.dataset.edit);render("Grade Editor")});
 document.querySelectorAll("[data-section]").forEach(b=>b.onclick=()=>{selectedSection=b.dataset.section;render("Students")});
 const search=document.getElementById("studentSearch"),filter=document.getElementById("statusFilter");
 function filterStudents(){if(!search||!filter)return;let q=search.value.toLowerCase();let f=filter.value;let list=students.filter(s=>s.section===selectedSection&&(`${s.name} ${s.id}`.toLowerCase().includes(q))&&(f==="all"||(f==="pass"&&pass(s))||(f==="fail"&&!pass(s))));document.getElementById("studentRows").innerHTML=list.length?tableRows(list):'<tr><td colspan="5" class="empty">No matching students found.</td></tr>';wire()}
 if(search)search.oninput=filterStudents;if(filter)filter.onchange=filterStudents;
 const rs=document.getElementById("reportSearch");if(rs)rs.oninput=()=>{let q=rs.value.toLowerCase();document.getElementById("reportRows").innerHTML=students.filter(s=>(s.name+" "+s.id).toLowerCase().includes(q)).map(s=>`<tr><td><div class="student-name">${s.name}</div><div class="student-id">Q1-${s.id}</div></td><td>${s.section}</td><td>${avg(s)}</td><td>${status(s)}</td><td>Oct 5, 2026</td><td><button class="action-link" data-report="${s.id}">View Report →</button></td></tr>`).join("")||'<tr><td colspan="6" class="empty">No matching reports.</td></tr>';wire()};
 const add=document.getElementById("addStudent");if(add)add.onclick=()=>toast("Add Student form is ready to be connected to your database.");
 const open=document.getElementById("openEditor");if(open)open.onclick=()=>{let id=document.getElementById("gradeStudent").value;selectedStudent=students.find(s=>s.id===id);render("Grade Editor")};
 const gs=document.getElementById("gradeSection");if(gs)gs.onchange=()=>{let sel=document.getElementById("gradeStudent");sel.innerHTML=students.filter(s=>s.section===gs.value).map(s=>`<option value="${s.id}">${s.name} · ${s.id}</option>`).join("")};
 const save=document.getElementById("saveGrades");if(save)save.onclick=()=>{let inputs=[...document.querySelectorAll(".grade-input")];if(inputs.some(i=>i.value===""||+i.value<0||+i.value>100)){toast("Enter a valid grade from 0 to 100 for every subject.");return}selectedStudent.subjects=inputs.map(i=>+i.value);selectedStudent.avg=+avg(selectedStudent);toast("Grades saved and general average calculated.");render("Grade Editor")};
 const exportBtn=document.getElementById("exportReport");if(exportBtn)exportBtn.onclick=()=>{const csv="Student ID,Student Name,Section,General Average,Status\n"+students.map(s=>`${s.id},${s.name},${s.section},${avg(s)},${pass(s)?"Passing":"Failing"}`).join("\n");const a=document.createElement("a");a.href=URL.createObjectURL(new Blob([csv],{type:"text/csv"}));a.download="Gradify-Section-Summary.csv";a.click();URL.revokeObjectURL(a.href);toast("Section summary exported.")};
 const ep=document.getElementById("editProfile");if(ep)ep.onclick=()=>toast("Profile fields can now be edited.");
 const sp=document.getElementById("saveProfile");if(sp)sp.onclick=()=>toast("Profile changes saved in this demo.");
 const lo=document.getElementById("confirmLogout");if(lo)lo.onclick=()=>render("Login");
 const lf=document.getElementById("loginForm");if(lf)lf.onsubmit=e=>{e.preventDefault();render("Dashboard");toast("Welcome back to Gradify.")};
}
render("Dashboard");
</script>
</body>
</html>
