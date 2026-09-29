<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<meta name="theme-color" content="#0b1020">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<title>WorkTime · 출퇴근 관리</title>
<style>
  :root{
    --bg:#f4f6fb;
    --card:rgba(255,255,255,.92);
    --ink:#111827;
    --muted:#6b7280;
    --line:#e8ebf2;
    --navy:#0b1020;
    --blue:#3b82f6;
    --green:#10b981;
    --red:#ef4444;
    --shadow:0 14px 34px rgba(16,24,40,.08);
    --radius:24px;
  }
  *{box-sizing:border-box}
  html,body{margin:0;padding:0;background:var(--bg);color:var(--ink);font-family:-apple-system,BlinkMacSystemFont,"Segoe UI","Noto Sans KR",sans-serif}
  body{min-height:100vh}
  button,input,select{font:inherit}
  .app{max-width:520px;margin:0 auto;min-height:100vh;padding-bottom:92px}
  .hero{
    background:
      radial-gradient(circle at 85% 0%, rgba(96,165,250,.3), transparent 32%),
      linear-gradient(145deg,#080d1a 0%,#141c35 58%,#172a4d 100%);
    color:white;padding:calc(18px + env(safe-area-inset-top)) 20px 24px;
    border-bottom-left-radius:30px;border-bottom-right-radius:30px;
  }
  .top{display:flex;align-items:center;justify-content:space-between;margin-bottom:22px}
  .brand{font-weight:800;letter-spacing:-.03em;font-size:20px}
  .status-pill{font-size:12px;padding:8px 11px;border:1px solid rgba(255,255,255,.16);border-radius:999px;background:rgba(255,255,255,.08)}
  .date{font-size:13px;color:rgba(255,255,255,.62);margin-bottom:6px}
  .clock{font-size:48px;line-height:1;font-weight:800;letter-spacing:-.055em}
  .clock small{font-size:15px;color:rgba(255,255,255,.55);font-weight:500;letter-spacing:0}
  .hero-grid{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-top:24px}
  .hero-stat{background:rgba(255,255,255,.08);border:1px solid rgba(255,255,255,.08);border-radius:18px;padding:14px}
  .hero-stat span{display:block;color:rgba(255,255,255,.54);font-size:12px;margin-bottom:6px}
  .hero-stat strong{font-size:17px}
  main{padding:18px 16px 0}
  .section-title{display:flex;align-items:center;justify-content:space-between;margin:2px 4px 10px}
  .section-title h2{font-size:17px;margin:0;letter-spacing:-.03em}
  .section-title p{margin:0;color:var(--muted);font-size:12px}
  .card{background:var(--card);border:1px solid rgba(255,255,255,.65);border-radius:var(--radius);box-shadow:var(--shadow);padding:16px;margin-bottom:16px;backdrop-filter:blur(10px)}
  .attendance{
    display:grid;grid-template-columns:1fr 1fr;gap:12px
  }
  .action{
    border:0;border-radius:22px;padding:18px 12px;min-height:118px;color:white;text-align:left;cursor:pointer;position:relative;overflow:hidden;
    box-shadow:0 12px 24px rgba(17,24,39,.13)
  }
  .action:active{transform:translateY(1px)}
  .action .label{font-size:14px;opacity:.84}
  .action strong{display:block;font-size:24px;margin-top:8px;letter-spacing:-.04em}
  .action .sub{font-size:11px;opacity:.7;margin-top:6px}
  .in{background:linear-gradient(145deg,#0ea5e9,#2563eb)}
  .out{background:linear-gradient(145deg,#111827,#334155)}
  .action:disabled{opacity:.42;cursor:not-allowed}
  .summary{display:grid;grid-template-columns:repeat(3,1fr);gap:10px}
  .summary-box{background:#f8f9fc;border:1px solid var(--line);border-radius:18px;padding:14px 10px}
  .summary-box .k{font-size:11px;color:var(--muted);margin-bottom:7px}
  .summary-box .v{font-size:20px;font-weight:800;letter-spacing:-.03em}
  .summary-box .u{font-size:11px;color:var(--muted);margin-left:2px}
  .today-row{display:flex;justify-content:space-between;align-items:center;padding:3px 2px}
  .today-time{font-size:27px;font-weight:800;letter-spacing:-.04em}
  .today-label{font-size:12px;color:var(--muted);margin-top:3px}
  .badge{padding:7px 10px;border-radius:999px;background:#ecfdf5;color:#047857;font-size:11px;font-weight:700}
  .history-list{display:flex;flex-direction:column}
  .history-item{display:flex;align-items:center;justify-content:space-between;padding:13px 0;border-bottom:1px solid var(--line)}
  .history-item:last-child{border-bottom:0}
  .history-left{display:flex;align-items:center;gap:11px}
  .dot{width:10px;height:10px;border-radius:50%;flex:0 0 auto}
  .dot.work{background:var(--green)}
  .dot.off{background:#cbd5e1}
  .history-date{font-size:13px;font-weight:700}
  .history-sub{font-size:11px;color:var(--muted);margin-top:3px;max-width:220px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
  .history-right{text-align:right}
  .history-time{font-size:13px;font-weight:700}
  .empty{padding:24px 8px;text-align:center;color:var(--muted);font-size:13px}
  .bottom{
    position:fixed;left:50%;transform:translateX(-50%);bottom:0;width:min(520px,100%);
    padding:10px 14px calc(10px + env(safe-area-inset-bottom));background:rgba(244,246,251,.88);backdrop-filter:blur(18px);border-top:1px solid rgba(17,24,39,.06)
  }
  .nav{display:grid;grid-template-columns:repeat(4,1fr);background:white;border:1px solid var(--line);border-radius:20px;padding:5px;box-shadow:0 12px 30px rgba(16,24,40,.08)}
  .nav button{border:0;background:transparent;padding:11px 6px;border-radius:15px;color:#7b8495;font-size:11px;font-weight:700;cursor:pointer}
  .nav button.active{background:#0b1020;color:#fff}
  .panel{display:none}
  .panel.active{display:block}
  .form-row{display:grid;grid-template-columns:1fr 1fr;gap:10px}
  .user-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:14px}
  .user-card{
    border:1px solid var(--line);background:#fafbfe;border-radius:18px;padding:13px;cursor:pointer;
    text-align:left;transition:.18s;position:relative
  }
  .user-card.active{border-color:#93c5fd;background:#eff6ff;box-shadow:0 0 0 2px rgba(59,130,246,.08)}
  .user-card .uid{font-size:10px;color:#6b7280;font-weight:700;letter-spacing:.04em}
  .user-card .uname{display:block;font-size:14px;font-weight:800;margin-top:5px;color:#111827}
  .user-card .udept{display:block;font-size:11px;color:#6b7280;margin-top:4px}
  .user-card .check{
    position:absolute;right:9px;top:9px;width:18px;height:18px;border-radius:50%;
    background:#0b1020;color:#fff;font-size:11px;display:none;align-items:center;justify-content:center
  }
  .user-card.active .check{display:flex}
  .persist-note{font-size:11px;color:#64748b;line-height:1.6;background:#f8fafc;border:1px solid var(--line);border-radius:14px;padding:11px 12px;margin-top:10px}
  .backup-row{display:grid;grid-template-columns:1fr 1fr;gap:9px;margin-top:9px}
  .secondary-btn{
    width:100%;border:1px solid var(--line);border-radius:14px;padding:12px;background:#fff;color:#334155;font-weight:700;cursor:pointer
  }
  .field{margin-bottom:12px}
  .field label{display:block;font-size:11px;color:var(--muted);margin-bottom:7px}
  .field input,.field select{
    width:100%;border:1px solid var(--line);background:#fafbfe;border-radius:14px;padding:12px 13px;outline:none;color:var(--ink)
  }
  .field input:focus,.field select:focus{border-color:#93c5fd;background:#fff}
  .field select{cursor:pointer}
  .primary{width:100%;border:0;border-radius:15px;padding:13px;background:#0b1020;color:white;font-weight:700;cursor:pointer}
  .danger{background:#fff1f2;color:#be123c;border:1px solid #fecdd3}
  .share-top{
    border:1px solid rgba(255,255,255,.16);background:rgba(255,255,255,.08);
    color:#fff;border-radius:999px;padding:8px 10px;font-size:11px;font-weight:700;cursor:pointer;
  }
  .modal-backdrop{
    position:fixed;inset:0;z-index:30;background:rgba(3,7,18,.58);backdrop-filter:blur(8px);
    display:none;align-items:flex-end;justify-content:center;
  }
  .modal-backdrop.show{display:flex}
  .modal{
    width:min(520px,100%);background:#fff;border-radius:28px 28px 0 0;
    padding:20px 18px calc(18px + env(safe-area-inset-bottom));box-shadow:0 -18px 45px rgba(0,0,0,.2);
    animation:sheet .22s ease-out;
  }
  @keyframes sheet{from{transform:translateY(20px);opacity:.6}to{transform:translateY(0);opacity:1}}
  .modal-head{display:flex;align-items:center;justify-content:space-between;margin-bottom:14px}
  .modal-head strong{font-size:18px;letter-spacing:-.03em}
  .close-btn{border:0;background:#f3f4f6;color:#374151;width:34px;height:34px;border-radius:50%;font-size:18px;cursor:pointer}
  .modal-sub{font-size:12px;color:#6b7280;line-height:1.6;margin-bottom:14px}
  .modal-input{
    width:100%;border:1px solid #e5e7eb;background:#fafbfc;border-radius:15px;
    padding:14px 15px;font-size:15px;outline:none;margin-bottom:12px;
  }
  .modal-input:focus{border-color:#93c5fd;background:#fff}
  .modal-actions{display:grid;grid-template-columns:1fr 1fr;gap:9px}
  .modal-actions button{border:0;border-radius:15px;padding:13px;font-weight:700;cursor:pointer}
  .cancel-btn{background:#f3f4f6;color:#374151}
  .confirm-btn{background:#0b1020;color:#fff}
  .toast{
    position:fixed;left:50%;transform:translate(-50%,20px);bottom:86px;z-index:10;
    background:#111827;color:#fff;padding:12px 15px;border-radius:14px;font-size:12px;opacity:0;pointer-events:none;transition:.25s
  }
  .toast.show{opacity:1;transform:translate(-50%,0)}
  .weekbar{height:140px;display:flex;align-items:flex-end;gap:8px;margin-top:6px}
  .bar-col{flex:1;text-align:center;font-size:10px;color:var(--muted)}
  .bar-wrap{height:100px;display:flex;align-items:flex-end;justify-content:center}
  .bar{width:18px;border-radius:999px 999px 5px 5px;background:linear-gradient(#60a5fa,#2563eb);min-height:4px}
  .month-title{display:flex;align-items:center;justify-content:space-between;margin-bottom:4px}
  .month-title strong{font-size:15px}
  .small-btn{border:1px solid var(--line);background:#fff;border-radius:12px;padding:7px 10px;font-size:11px;color:#4b5563;cursor:pointer}
  @media (max-width:360px){
    .clock{font-size:41px}.action strong{font-size:21px}.summary-box .v{font-size:18px}
  }
</style>
</head>
<body>
<div class="app">
  <header class="hero">
    <div class="top">
      <div>
        <div class="brand">WORKTIME</div>
        <div id="currentUserLabel" style="font-size:10px;color:rgba(255,255,255,.5);margin-top:2px"></div>
      </div>
      <div style="display:flex;align-items:center;gap:7px">
        <button class="share-top" onclick="shareApp()" aria-label="웹앱 공유">↗ 공유</button>
        <div class="status-pill" id="statusPill">오늘 미출근</div>
      </div>
    </div>
    <div class="date" id="dateText">-</div>
    <div class="clock" id="clockText">--:--<small> : --</small></div>
    <div class="hero-grid">
      <div class="hero-stat"><span>오늘 출근</span><strong id="heroIn">--:--</strong></div>
      <div class="hero-stat"><span>오늘 퇴근</span><strong id="heroOut">--:--</strong></div>
    </div>
  </header>

  <main>
    <section id="home" class="panel active">
      <div class="section-title"><h2>오늘의 근무</h2><p id="todayMsg">출근 시간을 기록하세요</p></div>
      <div class="card">
        <div class="attendance">
          <button class="action in" id="inBtn" onclick="clockIn()">
            <span class="label">출근</span>
            <strong>출근하기</strong>
            <span class="sub">현재 시간을 기록합니다</span>
          </button>
          <button class="action out" id="outBtn" onclick="clockOut()" disabled>
            <span class="label">퇴근</span>
            <strong>퇴근하기</strong>
            <span class="sub">근무 종료 시간을 기록합니다</span>
          </button>
        </div>
      </div>

      <div class="section-title"><h2>이번 달 요약</h2><p id="monthLabel">-</p></div>
      <div class="card">
        <div class="summary">
          <div class="summary-box"><div class="k">근무일</div><div class="v" id="workDays">0<span class="u">일</span></div></div>
          <div class="summary-box"><div class="k">근무시간</div><div class="v" id="workHours">0<span class="u">시간</span></div></div>
          <div class="summary-box"><div class="k">평균</div><div class="v" id="avgHours">0<span class="u">시간</span></div></div>
        </div>
      </div>

      <div class="section-title"><h2>오늘 기록</h2><p id="todayStatus">대기</p></div>
      <div class="card">
        <div class="today-row">
          <div>
            <div class="today-time" id="todayInOut">--:--  →  --:--</div>
            <div class="today-label" id="todayDuration">퇴근 후 근무시간이 계산됩니다</div>
            <div class="today-label" id="todayStore" style="margin-top:7px">가맹점: -</div>
          </div>
          <div class="badge" id="todayBadge">미출근</div>
        </div>
      </div>
    </section>

    <section id="history" class="panel">
      <div class="section-title"><h2>근무 기록</h2><p>최근 30일</p></div>
      <div class="card">
        <div class="history-list" id="historyList"></div>
      </div>
    </section>

    <section id="report" class="panel">
      <div class="section-title"><h2>근무 리포트</h2><p>월간 현황</p></div>
      <div class="card">
        <div class="month-title"><strong id="reportMonth">이번 달</strong><button class="small-btn" onclick="exportCSV()">CSV 내보내기</button></div>
        <div class="weekbar" id="weekbar"></div>
      </div>
      <div class="card">
        <div class="summary">
          <div class="summary-box"><div class="k">출근기록</div><div class="v" id="reportCount">0<span class="u">건</span></div></div>
          <div class="summary-box"><div class="k">정상 퇴근</div><div class="v" id="reportClosed">0<span class="u">건</span></div></div>
          <div class="summary-box"><div class="k">누적시간</div><div class="v" id="reportTotal">0<span class="u">시간</span></div></div>
        </div>
      </div>
    </section>

    <section id="settings" class="panel">
      <div class="section-title"><h2>설정</h2><p>사용자 관리</p></div>

      <div class="card">
        <div class="field"><label>사용자 선택 · 사용자 ID를 눌러 선택하세요</label></div>
        <div class="user-grid" id="userGrid">
          <button class="user-card" type="button" data-user-id="001" onclick="selectUser('001')">
            <span class="check">✓</span><span class="uid">USER 001</span>
            <span class="uname">최경원</span><span class="udept">필드팀</span>
          </button>
          <button class="user-card" type="button" data-user-id="002" onclick="selectUser('002')">
            <span class="check">✓</span><span class="uid">USER 002</span>
            <span class="uname">최우주</span><span class="udept">필드팀</span>
          </button>
          <button class="user-card" type="button" data-user-id="003" onclick="selectUser('003')">
            <span class="check">✓</span><span class="uid">USER 003</span>
            <span class="uname">최주노</span><span class="udept">필드팀</span>
          </button>
          <button class="user-card" type="button" data-user-id="004" onclick="selectUser('004')">
            <span class="check">✓</span><span class="uid">USER 004</span>
            <span class="uname">조희원</span><span class="udept">필드팀</span>
          </button>
          <button class="user-card" type="button" data-user-id="005" onclick="selectUser('005')">
            <span class="check">✓</span><span class="uid">USER 005</span>
            <span class="uname">전용식</span><span class="udept">필드팀</span>
          </button>
          <button class="user-card" type="button" data-user-id="006" onclick="selectUser('006')">
            <span class="check">✓</span><span class="uid">USER 006</span>
            <span class="uname">양준용</span><span class="udept">필드팀</span>
          </button>
        </div>

        <div class="form-row">
          <div class="field">
            <label>사용자 ID</label>
            <select id="userId" onchange="selectUser(this.value)">
              <option value="001">001</option>
              <option value="002">002</option>
              <option value="003">003</option>
              <option value="004">004</option>
              <option value="005">005</option>
              <option value="006">006</option>
            </select>
          </div>
          <div class="field"><label>사용자 이름</label><input id="userName" readonly></div>
        </div>
        <div class="field"><label>소속 / 부서</label><input id="userDept" readonly></div>

        <button class="primary" onclick="saveProfile()">현재 사용자 저장</button>

        <div class="persist-note">
          <b>업데이트 보호</b><br>
          사용자 001~006 정보와 출퇴근 기록은 앱 파일과 분리된 브라우저 저장공간에 저장됩니다.
          새 버전으로 파일을 교체해도 기존 저장 데이터를 삭제하거나 초기화하지 않습니다.
        </div>
      </div>

      <div class="card">
        <div class="field"><label>데이터 백업 / 복원</label></div>
        <div class="backup-row">
          <button class="secondary-btn" onclick="backupData()">백업 저장</button>
          <button class="secondary-btn" onclick="restoreData()">백업 불러오기</button>
        </div>
        <div class="persist-note">휴대폰 변경 또는 사이트 데이터 삭제에 대비해 백업 파일을 따로 보관할 수 있습니다.</div>
      </div>

      <div class="card">
        <div class="field"><label>기록 초기화</label></div>
        <button class="primary danger" onclick="resetData()">출퇴근 기록만 초기화</button>
        <div class="persist-note">사용자 001~006 정보는 초기화되지 않습니다.</div>
      </div>
    </section>
  </main>

  <div class="bottom">
    <div class="nav">
      <button class="active" data-tab="home">오늘</button>
      <button data-tab="history">기록</button>
      <button data-tab="report">리포트</button>
      <button data-tab="settings">설정</button>
    </div>
  </div>
</div>

<div class="modal-backdrop" id="storeModal" onclick="if(event.target===this) closeStoreModal()">
  <div class="modal" role="dialog" aria-modal="true" aria-labelledby="storeModalTitle">
    <div class="modal-head">
      <strong id="storeModalTitle">퇴근 기록</strong>
      <button class="close-btn" onclick="closeStoreModal()" aria-label="닫기">×</button>
    </div>
    <div class="modal-sub">퇴근 시간을 저장하기 전에 오늘 방문한 <b>가맹점명</b>을 입력해주세요.</div>
    <input class="modal-input" id="storeNameInput" maxlength="80" placeholder="예: OO가맹점" autocomplete="organization">
    <div class="modal-actions">
      <button class="cancel-btn" onclick="closeStoreModal()">취소</button>
      <button class="confirm-btn" onclick="confirmClockOut()">퇴근 저장</button>
    </div>
  </div>
</div>
<div class="toast" id="toast"></div>

<script>
const KEY='worktime_records_v1';
const PROFILE='worktime_profile_v1';
const USERS_KEY='worktime_users_v2';
const APP_DATA_KEY='worktime_app_data_v3';
const APP_BACKUP_KEY='worktime_app_data_backup_v3';
const APP_DATA_VERSION='3.0';

const DEFAULT_USERS=[
  {id:'001',name:'최경원',dept:'필드팀'},
  {id:'002',name:'최우주',dept:'필드팀'},
  {id:'003',name:'최주노',dept:'필드팀'},
  {id:'004',name:'조희원',dept:'필드팀'},
  {id:'005',name:'전용식',dept:'필드팀'},
  {id:'006',name:'양준용',dept:'필드팀'}
];

function readJSON(key,fallback){
  try{
    const v=safeGet(key);
    return v===null?fallback:JSON.parse(v);
  }catch(e){
    return fallback;
  }
}
function safeStorage(){
  try{
    const s=window.localStorage;
    const test='__worktime_test__';
    s.setItem(test,'1'); s.removeItem(test);
    return s;
  }catch(e){ return null; }
}
function safeGet(key){
  try{
    const s=safeStorage();
    return s ? s.getItem(key) : null;
  }catch(e){ return null; }
}
function safeSet(key,value){
  try{
    const s=safeStorage();
    if(!s) return false;
    s.setItem(key,typeof value==='string'?value:JSON.stringify(value));
    return true;
  }catch(e){ return false; }
}

let records=readJSON(KEY,[]);
let users=readJSON(USERS_KEY,[]);
let profile=readJSON(PROFILE,{});

function mergeUsers(saved){
  const map=new Map();
  (Array.isArray(saved)?saved:[]).forEach(u=>{
    if(u && u.id) map.set(String(u.id),{id:String(u.id),name:String(u.name||''),dept:String(u.dept||'')});
  });
  DEFAULT_USERS.forEach(def=>{
    if(!map.has(def.id)) map.set(def.id,{...def});
  });
  return DEFAULT_USERS.map(def=>map.get(def.id));
}

function migrateState(){
  const v3=readJSON(APP_DATA_KEY,null);
  const backup=readJSON(APP_BACKUP_KEY,null);

  // Prefer the current saved dataset; fall back to old keys; finally use backup.
  if(v3 && Array.isArray(v3.records)){
    records=v3.records;
    users=mergeUsers(v3.users);
    profile=v3.profile||profile||{};
  }else if(Array.isArray(records)){
    users=mergeUsers(users);
    if(!profile || !profile.id){
      profile={id:'001',name:'최경원',dept:'필드팀'};
    }
  }else if(backup && Array.isArray(backup.records)){
    records=backup.records;
    users=mergeUsers(backup.users);
    profile=backup.profile||{id:'001',name:'최경원',dept:'필드팀'};
  }else{
    records=[];
    users=mergeUsers([]);
    profile={id:'001',name:'최경원',dept:'필드팀'};
  }

  // Keep legacy records instead of deleting them.
  records=(Array.isArray(records)?records:[]).map(r=>{
    if(!r || typeof r!=='object') return null;
    if(!r.userId){
      return {...r,userId:profile.id||'001',userName:profile.name||'최경원',dept:profile.dept||'필드팀'};
    }
    return r;
  }).filter(Boolean);

  users=mergeUsers(users);
  let active=users.find(u=>String(u.id)===String(profile.id||'001'))||users[0];
  profile={id:active.id,name:active.name,dept:active.dept};

  persistAppState(false);
}
migrateState();

function pad(n){return String(n).padStart(2,'0')}
function now(){return new Date()}
function keyOf(d){return `${d.getFullYear()}-${pad(d.getMonth()+1)}-${pad(d.getDate())}`}
function hm(d){return `${pad(d.getHours())}:${pad(d.getMinutes())}`}
function fmtDate(d){return d.toLocaleDateString('ko-KR',{year:'numeric',month:'long',day:'numeric',weekday:'long'})}
function durationMin(r){
  if(!r.in) return 0;
  const a=new Date(r.in), b=r.out?new Date(r.out):new Date();
  return Math.max(0,Math.round((b-a)/60000));
}
function hhm(min){
  const h=Math.floor(min/60), m=min%60;
  return `${h}시간 ${m}분`;
}
function currentUserId(){
  return profile.id || '001';
}
function getToday(){
  return records.find(r=>r.date===keyOf(now()) && String(r.userId||'001')===String(currentUserId()));
}
function persistAppState(makeBackup=true){
  const state={
    app:'WORKTIME',
    version:APP_DATA_VERSION,
    updatedAt:new Date().toISOString(),
    users,
    profile,
    records
  };
  if(makeBackup){
    const current=safeGet(APP_DATA_KEY);
    if(current) safeSet(APP_BACKUP_KEY,current);
  }
  safeSet(APP_DATA_KEY,state);
  safeSet(KEY,records);
  safeSet(USERS_KEY,users);
  safeSet(PROFILE,profile);
}
function save(){ persistAppState(true); }
function saveUsers(){ persistAppState(true); }
function toast(msg){
  const el=document.getElementById('toast'); el.textContent=msg; el.classList.add('show');
  clearTimeout(window._toast); window._toast=setTimeout(()=>el.classList.remove('show'),1800);
}

function renderClock(){
  const d=now();
  document.getElementById('dateText').textContent=fmtDate(d);
  document.getElementById('clockText').innerHTML=`${hm(d)}<small> : ${pad(d.getSeconds())}</small>`;
  render();
}

function render(){
  const d=now(), t=getToday();
  document.getElementById('monthLabel').textContent=d.toLocaleDateString('ko-KR',{year:'numeric',month:'long'});
  document.getElementById('reportMonth').textContent=d.toLocaleDateString('ko-KR',{year:'numeric',month:'long'});
  document.getElementById('heroIn').textContent=t?.in?hm(new Date(t.in)):'--:--';
  document.getElementById('heroOut').textContent=t?.out?hm(new Date(t.out)):'--:--';
  const inBtn=document.getElementById('inBtn'), outBtn=document.getElementById('outBtn');
  inBtn.disabled=!!t?.in;
  outBtn.disabled=!t?.in || !!t?.out;
  let status='오늘 미출근', badge='미출근', msg='출근 시간을 기록하세요';
  if(t?.in && !t?.out){status='근무 중';badge='근무 중';msg='현재 근무시간을 계산 중입니다'}
  if(t?.in && t?.out){status='근무 완료';badge='근무 완료';msg='오늘 근무가 완료되었습니다'}
  document.getElementById('statusPill').textContent=status;
  const userLabel=document.getElementById('currentUserLabel');
  if(userLabel) userLabel.textContent=`${profile.id||''} · ${profile.name||''} · ${profile.dept||''}`;
  document.getElementById('todayMsg').textContent=msg;
  document.getElementById('todayStatus').textContent=status;
  document.getElementById('todayBadge').textContent=badge;
  document.getElementById('todayInOut').textContent=`${t?.in?hm(new Date(t.in)):'--:--'}  →  ${t?.out?hm(new Date(t.out)):'--:--'}`;
  document.getElementById('todayDuration').textContent=t?.in?`근무시간 ${hhm(durationMin(t))}`:'퇴근 후 근무시간이 계산됩니다';
  document.getElementById('todayStore').textContent=`가맹점: ${t?.store?escapeHtml(t.store):'-'}`;

  const monthPrefix=`${d.getFullYear()}-${pad(d.getMonth()+1)}-`;
  const monthRecs=records.filter(r=>r.date.startsWith(monthPrefix) && String(r.userId||'001')===String(currentUserId()));
  const total=monthRecs.reduce((s,r)=>s+durationMin(r),0);
  const closed=monthRecs.filter(r=>r.out).length;
  document.getElementById('workDays').innerHTML=`${monthRecs.length}<span class="u">일</span>`;
  document.getElementById('workHours').innerHTML=`${Math.round(total/60*10)/10}<span class="u">시간</span>`;
  document.getElementById('avgHours').innerHTML=`${monthRecs.length?Math.round(total/monthRecs.length/60*10)/10:0}<span class="u">시간</span>`;
  document.getElementById('reportCount').innerHTML=`${monthRecs.length}<span class="u">건</span>`;
  document.getElementById('reportClosed').innerHTML=`${closed}<span class="u">건</span>`;
  document.getElementById('reportTotal').innerHTML=`${Math.round(total/60*10)/10}<span class="u">시간</span>`;

  const list=document.getElementById('historyList');
  const recent=[...records]
    .filter(r=>String(r.userId||'001')===String(currentUserId()))
    .sort((a,b)=>b.date.localeCompare(a.date)).slice(0,30);
  list.innerHTML=recent.length?recent.map(r=>{
    const dd=new Date(r.date+'T00:00:00');
    return `<div class="history-item">
      <div class="history-left"><span class="dot ${r.out?'work':'off'}"></span>
        <div><div class="history-date">${dd.toLocaleDateString('ko-KR',{month:'numeric',day:'numeric',weekday:'short'})}</div>
        <div class="history-sub">${r.out?'정상 퇴근':'퇴근 미기록'}${r.store?` · ${escapeHtml(r.store)}`:''}</div></div>
      </div>
      <div class="history-right"><div class="history-time">${r.in?hm(new Date(r.in)):'--:--'} → ${r.out?hm(new Date(r.out)):'--:--'}</div>
      <div class="history-sub">${hhm(durationMin(r))}</div></div>
    </div>`
  }).join(''):'<div class="empty">아직 출퇴근 기록이 없습니다.</div>';

  const bar=document.getElementById('weekbar');
  const first=new Date(d.getFullYear(),d.getMonth(),1);
  const days=new Date(d.getFullYear(),d.getMonth()+1,0).getDate();
  const sample=[];
  for(let day=Math.max(1,days-6);day<=days;day++){
    const date=`${d.getFullYear()}-${pad(d.getMonth()+1)}-${pad(day)}`;
    const r=records.find(x=>x.date===date);
    sample.push({day,r,min:r?durationMin(r):0});
  }
  const max=Math.max(1,...sample.map(x=>x.min));
  bar.innerHTML=sample.map(x=>`<div class="bar-col"><div class="bar-wrap"><div class="bar" style="height:${Math.max(4,Math.round(x.min/max*88))}px"></div></div>${x.day}일</div>`).join('');

  if(profile.name) document.title=`${profile.name} · WorkTime`;
}

function clockIn(){
  if(getToday()) return;
  const d=now();
  records.push({
    date:keyOf(d),
    userId:currentUserId(),
    userName:profile.name||'',
    dept:profile.dept||'',
    in:d.toISOString(),
    out:null,
    store:''
  });
  persistAppState(); render(); toast(`${profile.name||'사용자'}님 출근이 기록되었습니다.`);
}
function clockOut(){
  const t=getToday();
  if(!t?.in || t.out) return;
  document.getElementById('storeNameInput').value=t.store||'';
  document.getElementById('storeModal').classList.add('show');
  setTimeout(()=>document.getElementById('storeNameInput').focus(),80);
}
function closeStoreModal(){
  document.getElementById('storeModal').classList.remove('show');
}
function confirmClockOut(){
  const t=getToday();
  if(!t?.in || t.out) return;
  const store=document.getElementById('storeNameInput').value.trim();
  if(!store){
    toast('가맹점명을 입력해주세요.');
    document.getElementById('storeNameInput').focus();
    return;
  }
  t.out=now().toISOString();
  t.store=store;
  t.userId=currentUserId();
  t.userName=profile.name||t.userName||'';
  t.dept=profile.dept||t.dept||'';
  persistAppState(); render(); closeStoreModal(); toast('퇴근 및 가맹점이 기록되었습니다.');
}
function escapeHtml(value){
  return String(value).replace(/[&<>"']/g, ch => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[ch]));
}
async function shareApp(){
  const shareData={
    title:'WORKTIME · 출퇴근 관리',
    text:'출퇴근을 간편하게 기록하는 모바일 웹앱입니다.',
    url:location.href
  };
  try{
    if(navigator.share){
      await navigator.share(shareData);
      return;
    }
    await navigator.clipboard.writeText(location.href);
    toast('웹앱 주소가 복사되었습니다.');
  }catch(err){
    if(err && err.name==='AbortError') return;
    try{
      const ta=document.createElement('textarea');
      ta.value=location.href; document.body.appendChild(ta); ta.select();
      document.execCommand('copy'); ta.remove();
      toast('웹앱 주소가 복사되었습니다.');
    }catch(e){
      toast('공유 기능을 사용할 수 없습니다.');
    }
  }
}
function renderUserGrid(){
  document.querySelectorAll('.user-card[data-user-id]').forEach(card=>{
    card.classList.toggle('active',String(card.dataset.userId)===String(profile.id));
  });
}
function selectUser(id, notify=true){
  const u=users.find(x=>String(x.id)===String(id)) || DEFAULT_USERS.find(x=>x.id===String(id));
  if(!u) return;
  profile={id:u.id,name:u.name,dept:u.dept};
  const idEl=document.getElementById('userId');
  const nameEl=document.getElementById('userName');
  const deptEl=document.getElementById('userDept');
  // 화면을 먼저 갱신하여 저장소 접근 실패와 무관하게 이름/부서가 바로 표시됩니다.
  if(idEl) idEl.value=u.id;
  if(nameEl) nameEl.value=u.name;
  if(deptEl) deptEl.value=u.dept;
  renderUserGrid();
  render();
  persistAppState(true);
  if(notify) toast(`${u.name}님이 선택되었습니다.`);
}
function saveProfile(){
  const id=document.getElementById('userId')?.value||'001';
  selectUser(id,false);
  toast('사용자 정보가 저장되었습니다.');
}
function backupData(){


  const payload={
    app:'WORKTIME',
    version:APP_DATA_VERSION,
    exportedAt:new Date().toISOString(),
    users,
    profile,
    records
  };
  const blob=new Blob([JSON.stringify(payload,null,2)],{type:'application/json'});
  const a=document.createElement('a');
  a.href=URL.createObjectURL(blob);
  a.download=`WORKTIME_백업_${keyOf(now())}.json`;
  a.click();
  setTimeout(()=>URL.revokeObjectURL(a.href),1000);
  toast('백업 파일을 저장했습니다.');
}
function restoreData(){
  const input=document.createElement('input');
  input.type='file'; input.accept='.json,application/json';
  input.onchange=()=>{
    const file=input.files?.[0];
    if(!file) return;
    const reader=new FileReader();
    reader.onload=()=>{
      try{
        const data=JSON.parse(reader.result);
        if(!data || !Array.isArray(data.records) || !Array.isArray(data.users)) throw new Error('invalid');
        records=data.records;
        users=data.users;
        profile=data.profile||profile;
        mergeDefaultUsers();
        persistAppState();
        loadProfileUI();
        render();
        toast('백업 데이터를 복원했습니다.');
      }catch(e){toast('백업 파일 형식을 확인해주세요.')}
    };
    reader.readAsText(file,'utf-8');
  };
  input.click();
}
function loadProfileUI(showToast=true){
  users=mergeUsers(users);
  let u=users.find(x=>String(x.id)===String(profile.id||'001'))||users[0];
  profile={id:u.id,name:u.name,dept:u.dept};
  const idEl=document.getElementById('userId');
  const nameEl=document.getElementById('userName');
  const deptEl=document.getElementById('userDept');
  if(idEl) idEl.value=u.id;
  if(nameEl) nameEl.value=u.name;
  if(deptEl) deptEl.value=u.dept;
  renderUserGrid();
  persistAppState(false);
  if(showToast) toast(`${u.name}님이 현재 사용자입니다.`);
}
function resetData(){
  if(confirm('모든 출퇴근 기록을 삭제할까요? 사용자 정보는 유지됩니다.')){
    records=[]; persistAppState(true); render(); toast('출퇴근 기록만 초기화되었습니다. 사용자 정보는 유지됩니다.');
  }
}
function exportCSV(){
  const rows=[['사용자ID','사용자이름','부서','날짜','출근','퇴근','가맹점','근무시간']];
  [...records]
    .filter(r=>String(r.userId||'001')===String(currentUserId()))
    .sort((a,b)=>a.date.localeCompare(b.date))
    .forEach(r=>rows.push([r.userId||'',r.userName||'',r.dept||'',r.date,r.in?hm(new Date(r.in)):'',r.out?hm(new Date(r.out)):'',r.store||'',hhm(durationMin(r))]));
  const csv='\uFEFF'+rows.map(x=>x.map(v=>`"${String(v).replaceAll('"','""')}"`).join(',')).join('\n');
  const blob=new Blob([csv],{type:'text/csv;charset=utf-8;'});
  const a=document.createElement('a');a.href=URL.createObjectURL(blob);a.download=`근무기록_${keyOf(now())}.csv`;a.click();
  setTimeout(()=>URL.revokeObjectURL(a.href),1000);
}
document.querySelectorAll('.nav button').forEach(btn=>{
  btn.addEventListener('click',()=>{
    document.querySelectorAll('.nav button').forEach(b=>b.classList.remove('active'));
    document.querySelectorAll('.panel').forEach(p=>p.classList.remove('active'));
    btn.classList.add('active'); document.getElementById(btn.dataset.tab).classList.add('active');
  });
});
loadProfileUI(false);
setInterval(renderClock,1000);
renderClock();
</script>
</body>
</html>
