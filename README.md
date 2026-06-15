[index.html](https://github.com/user-attachments/files/28940790/index.html)
<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Breaking 動作分類庫 — Freeze 技術資料庫</title>
<style>
  :root {
    --bg: #f5f4f1;
    --surface: #ffffff;
    --surface2: #eeece8;
    --border: rgba(0,0,0,0.09);
    --border2: rgba(0,0,0,0.16);
    --text: #1a1a18;
    --text2: #58574f;
    --text3: #9a9890;
    --accent: #1D9E75;
    --accent-dark: #0F6E56;
    --teal:   #0F6E56; --teal-bg:   #E1F5EE; --teal-bd:   #5DCAA5;
    --amber:  #633806; --amber-bg:  #FAEEDA; --amber-bd:  #EF9F27;
    --coral:  #712B13; --coral-bg:  #FAECE7; --coral-bd:  #F0997B;
    --purple: #3C3489; --purple-bg: #EEEDFE; --purple-bd: #AFA9EC;
    --blue:   #0C447C; --blue-bg:   #E6F1FB; --blue-bd:   #85B7EB;
    --r: 10px;
  }
  @media (prefers-color-scheme: dark) {
    :root {
      --bg: #161614; --surface: #202020; --surface2: #2a2a28;
      --border: rgba(255,255,255,0.08); --border2: rgba(255,255,255,0.18);
      --text: #e6e4de; --text2: #a6a49c; --text3: #66645c;
      --accent: #5DCAA5; --accent-dark: #9FE1CB;
      --teal:   #9FE1CB; --teal-bg:   #04342C; --teal-bd:   #0F6E56;
      --amber:  #FAC775; --amber-bg:  #412402; --amber-bd:  #854F0B;
      --coral:  #F0997B; --coral-bg:  #4A1B0C; --coral-bd:  #993C1D;
      --purple: #CECBF6; --purple-bg: #26215C; --purple-bd: #534AB7;
      --blue:   #B5D4F4; --blue-bg:   #042C53; --blue-bd:   #185FA5;
    }
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang TC", "Microsoft JhengHei", sans-serif; background: var(--bg); color: var(--text); font-size: 15px; line-height: 1.6; }

  /* ── Header ── */
  .header { padding: 2rem 2rem 1.25rem; border-bottom: 0.5px solid var(--border); background: var(--surface); }
  .header-eyebrow { font-size: 11px; font-weight: 600; letter-spacing: .12em; color: var(--accent); text-transform: uppercase; margin-bottom: 6px; }
  .header-top { display: flex; align-items: baseline; gap: 12px; margin-bottom: 5px; flex-wrap: wrap; }
  .header h1 { font-size: 24px; font-weight: 700; letter-spacing: -0.5px; color: var(--text); }
  .header-sub { font-size: 13px; color: var(--text3); line-height: 1.5; }
  .version-tag { font-size: 10px; padding: 3px 9px; border-radius: 20px; border: 0.5px solid var(--border2); color: var(--text3); background: var(--surface2); letter-spacing: .04em; }
  .header-meta { display: flex; gap: 16px; margin-top: 12px; flex-wrap: wrap; }
  .header-meta-item { font-size: 12px; color: var(--text3); display: flex; align-items: center; gap: 5px; }
  .header-meta-dot { width: 6px; height: 6px; border-radius: 50%; background: var(--accent); display: inline-block; }

  /* ── Toolbar ── */
  .toolbar { padding: .85rem 2rem; display: flex; gap: 8px; flex-wrap: wrap; align-items: center; border-bottom: 0.5px solid var(--border); background: var(--surface); position: sticky; top: 0; z-index: 10; }
  .filter-btn { font-size: 12px; padding: 5px 14px; border-radius: 20px; border: 0.5px solid var(--border2); background: transparent; color: var(--text2); cursor: pointer; transition: all .15s; font-weight: 500; }
  .filter-btn:hover { background: var(--surface2); }
  .filter-btn.active { background: var(--text); color: var(--bg); border-color: var(--text); }
  .search-wrap { margin-left: auto; position: relative; }
  .search-icon { position: absolute; left: 10px; top: 50%; transform: translateY(-50%); font-size: 14px; color: var(--text3); pointer-events: none; }
  .search-wrap input { font-size: 13px; padding: 7px 12px 7px 30px; border-radius: 8px; border: 0.5px solid var(--border2); background: var(--surface2); color: var(--text); width: 200px; outline: none; transition: border-color .15s, background .15s; }
  .search-wrap input:focus { border-color: var(--accent); background: var(--surface); }
  .add-btn { font-size: 12px; padding: 6px 14px; border-radius: 8px; border: none; background: var(--accent); color: #fff; cursor: pointer; font-weight: 600; transition: opacity .15s; display: flex; align-items: center; gap: 5px; white-space: nowrap; }
  .add-btn:hover { opacity: .88; }

  /* ── Stats ── */
  .stats-bar { padding: .55rem 2rem; display: flex; gap: 20px; font-size: 12px; color: var(--text3); border-bottom: 0.5px solid var(--border); background: var(--bg); }
  .stats-bar b { color: var(--text2); font-weight: 600; }

  /* ── Grid ── */
  .grid-wrap { padding: 1.5rem 2rem; }
  .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(230px, 1fr)); gap: 12px; }
  .card { background: var(--surface); border: 0.5px solid var(--border); border-radius: 12px; padding: 1.1rem; cursor: pointer; transition: border-color .15s, box-shadow .15s, transform .1s; }
  .card:hover { border-color: var(--border2); transform: translateY(-2px); box-shadow: 0 4px 16px rgba(0,0,0,0.07); }
  .card-head { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 4px; }
  .card-name-en { font-size: 15px; font-weight: 700; color: var(--text); }
  .card-name-zh { font-size: 12px; color: var(--text3); margin-bottom: 8px; font-weight: 400; }
  .badge { font-size: 10px; padding: 2px 9px; border-radius: 20px; white-space: nowrap; font-weight: 600; letter-spacing: .03em; }
  .badge-beginner    { background: var(--teal-bg);   color: var(--teal);   border: 0.5px solid var(--teal-bd); }
  .badge-intermediate{ background: var(--amber-bg);  color: var(--amber);  border: 0.5px solid var(--amber-bd); }
  .badge-advanced    { background: var(--coral-bg);  color: var(--coral);  border: 0.5px solid var(--coral-bd); }
  .support-row { display: flex; gap: 5px; flex-wrap: wrap; margin-bottom: 9px; }
  .stag { font-size: 11px; padding: 2px 7px; border-radius: 5px; background: var(--surface2); color: var(--text2); }
  .pwr-row { display: flex; align-items: center; gap: 7px; margin-bottom: 8px; }
  .pwr-label { font-size: 11px; color: var(--text3); width: 28px; }
  .pwr-bar-bg { flex: 1; height: 4px; border-radius: 2px; background: var(--surface2); overflow: hidden; }
  .pwr-bar-fill { height: 100%; border-radius: 2px; transition: width .3s ease; }
  .pwr-val { font-size: 11px; color: var(--text2); font-weight: 600; min-width: 22px; text-align: right; }
  .card-footer { display: flex; align-items: center; gap: 5px; margin-top: 8px; padding-top: 8px; border-top: 0.5px solid var(--border); }
  .source-dot { width: 6px; height: 6px; border-radius: 50%; display: inline-block; flex-shrink: 0; }
  .source-estimated { background: var(--amber-bd); }
  .source-measured  { background: var(--teal-bd); }
  .card-source-txt { font-size: 10px; color: var(--text3); }
  .pending-badge { font-size: 10px; background: var(--amber-bg); color: var(--amber); border: 0.5px solid var(--amber-bd); border-radius: 5px; padding: 2px 6px; font-weight: 600; }

  /* ── Empty ── */
  .empty { padding: 3rem; text-align: center; color: var(--text3); grid-column: 1/-1; font-size: 14px; }

  /* ── Overlay ── */
  .detail-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.45); z-index: 100; overflow-y: auto; padding: 2rem 1rem; }
  .detail-overlay.open { display: flex; align-items: flex-start; justify-content: center; }
  .detail-panel { background: var(--surface); border-radius: 16px; border: 0.5px solid var(--border); width: 100%; max-width: 580px; padding: 1.75rem; margin: auto; }
  .detail-top { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 1.4rem; }
  .detail-titles { display: flex; flex-direction: column; gap: 3px; }
  .detail-name-en { font-size: 22px; font-weight: 700; color: var(--text); }
  .detail-name-zh { font-size: 14px; color: var(--text3); }
  .close-btn { background: var(--surface2); border: none; border-radius: 50%; width: 32px; height: 32px; cursor: pointer; font-size: 18px; color: var(--text2); display: flex; align-items: center; justify-content: center; flex-shrink: 0; margin-top: 3px; }
  .sec { margin-bottom: 1.2rem; }
  .sec-label { font-size: 10px; font-weight: 700; color: var(--text3); text-transform: uppercase; letter-spacing: .1em; margin-bottom: 7px; }
  .tag-row { display: flex; flex-wrap: wrap; gap: 5px; }
  .tag { font-size: 12px; padding: 3px 10px; border-radius: 6px; background: var(--surface2); color: var(--text2); }
  .muscle-tag { background: var(--purple-bg); color: var(--purple); border: 0.5px solid var(--purple-bd); }
  .variant-tag { background: var(--blue-bg); color: var(--blue); border: 0.5px solid var(--blue-bd); }
  .pwr-detail-row { display: flex; align-items: center; gap: 10px; margin-bottom: 9px; }
  .pwr-detail-label { font-size: 12px; color: var(--text2); width: 56px; }
  .pwr-detail-bar { flex: 1; height: 6px; border-radius: 3px; background: var(--surface2); overflow: hidden; }
  .pwr-detail-fill { height: 100%; border-radius: 3px; }
  .pwr-detail-val { font-size: 12px; font-weight: 700; min-width: 28px; text-align: right; }
  .pwr-total-row { display: flex; justify-content: space-between; padding-top: 9px; border-top: 0.5px solid var(--border); margin-top: 5px; font-size: 13px; }
  .pwr-total-row span:last-child { font-weight: 700; color: var(--text); }
  .step-list { display: flex; flex-direction: column; gap: 8px; }
  .step-item { display: flex; gap: 10px; align-items: flex-start; }
  .step-num { min-width: 22px; height: 22px; border-radius: 50%; background: var(--teal-bg); display: flex; align-items: center; justify-content: center; font-size: 11px; font-weight: 700; color: var(--teal); flex-shrink: 0; }
  .step-txt { font-size: 13px; color: var(--text2); line-height: 1.65; padding-top: 2px; }
  .note-box { background: var(--surface2); border-radius: 8px; padding: 11px 13px; font-size: 12px; color: var(--text2); line-height: 1.65; border-left: 2px solid var(--accent); }
  .source-info { display: flex; align-items: center; gap: 8px; font-size: 12px; color: var(--text3); margin-top: 8px; }

  /* ── Edit section ── */
  .edit-btn { margin-top: 1rem; width: 100%; padding: 10px; border-radius: 8px; border: 0.5px solid var(--border2); background: var(--surface); color: var(--text2); cursor: pointer; font-size: 13px; transition: background .15s; }
  .edit-btn:hover { background: var(--surface2); }
  .edit-section { margin-top: 1.1rem; padding-top: 1.1rem; border-top: 0.5px solid var(--border); }
  .edit-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-bottom: 8px; }
  .edit-field { display: flex; flex-direction: column; gap: 4px; }
  .edit-field label { font-size: 11px; color: var(--text3); font-weight: 500; }
  .edit-field input, .edit-field select { font-size: 13px; padding: 7px 9px; border-radius: 7px; border: 0.5px solid var(--border2); background: var(--surface); color: var(--text); outline: none; }
  .edit-field input:focus { border-color: var(--accent); }
  .save-btn { width: 100%; padding: 10px; border-radius: 8px; border: none; background: var(--accent); color: #fff; cursor: pointer; font-size: 13px; font-weight: 600; margin-top: 8px; transition: opacity .15s; }
  .save-btn:hover { opacity: .88; }
  .saved-msg { text-align: center; font-size: 12px; color: var(--teal); margin-top: 6px; display: none; }

  /* ── Submit Modal ── */
  .modal-bg { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.5); z-index: 200; overflow-y: auto; padding: 2rem 1rem; }
  .modal-bg.open { display: flex; align-items: flex-start; justify-content: center; }
  .modal { background: var(--surface); border-radius: 16px; border: 0.5px solid var(--border); width: 100%; max-width: 500px; padding: 1.75rem; margin: auto; }
  .modal h2 { font-size: 18px; font-weight: 700; margin-bottom: 4px; }
  .modal-sub { font-size: 13px; color: var(--text3); margin-bottom: 1.4rem; }
  .review-notice { background: var(--amber-bg); border: 0.5px solid var(--amber-bd); border-radius: 8px; padding: 10px 13px; font-size: 12px; color: var(--amber); margin-bottom: 1.2rem; line-height: 1.6; }
  .form-field { display: flex; flex-direction: column; gap: 5px; margin-bottom: 12px; }
  .form-field label { font-size: 12px; color: var(--text2); font-weight: 600; }
  .form-field input, .form-field select, .form-field textarea { font-size: 13px; padding: 8px 10px; border-radius: 8px; border: 0.5px solid var(--border2); background: var(--surface); color: var(--text); outline: none; font-family: inherit; }
  .form-field textarea { resize: vertical; min-height: 80px; line-height: 1.6; }
  .form-field input:focus, .form-field textarea:focus, .form-field select:focus { border-color: var(--accent); }
  .form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
  .modal-actions { display: flex; gap: 8px; margin-top: 1.2rem; }
  .btn-cancel { flex: 1; padding: 10px; border-radius: 8px; border: 0.5px solid var(--border2); background: transparent; color: var(--text2); cursor: pointer; font-size: 13px; }
  .btn-submit { flex: 2; padding: 10px; border-radius: 8px; border: none; background: var(--accent); color: #fff; cursor: pointer; font-size: 13px; font-weight: 600; }
  .btn-submit:hover { opacity: .88; }

  /* ── Pending review panel ── */
  .pending-panel { background: var(--amber-bg); border: 0.5px solid var(--amber-bd); border-radius: 12px; padding: 1.2rem; margin: 0 2rem 1rem; }
  .pending-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px; }
  .pending-title { font-size: 13px; font-weight: 700; color: var(--amber); }
  .pending-count { font-size: 11px; background: var(--amber-bd); color: #fff; border-radius: 20px; padding: 2px 8px; font-weight: 700; }
  .pending-list { display: flex; flex-direction: column; gap: 7px; }
  .pending-item { background: var(--surface); border-radius: 8px; padding: 10px 12px; display: flex; align-items: center; justify-content: space-between; border: 0.5px solid var(--border); }
  .pending-item-name { font-size: 13px; font-weight: 600; color: var(--text); }
  .pending-item-sub { font-size: 11px; color: var(--text3); }
  .pending-approve-btn { font-size: 11px; padding: 4px 10px; border-radius: 6px; border: 0.5px solid var(--teal-bd); background: var(--teal-bg); color: var(--teal); cursor: pointer; font-weight: 600; margin-left: 6px; }
  .pending-reject-btn  { font-size: 11px; padding: 4px 10px; border-radius: 6px; border: 0.5px solid var(--coral-bd); background: var(--coral-bg); color: var(--coral); cursor: pointer; font-weight: 600; }

  /* ── Toast ── */
  .toast { position: fixed; bottom: 1.5rem; left: 50%; transform: translateX(-50%) translateY(60px); background: var(--text); color: var(--bg); padding: 10px 20px; border-radius: 20px; font-size: 13px; font-weight: 500; z-index: 999; opacity: 0; transition: all .3s ease; pointer-events: none; white-space: nowrap; }
  .toast.show { opacity: 1; transform: translateX(-50%) translateY(0); }

  /* ── Footer ── */
  .footer { border-top: 0.5px solid var(--border); padding: 1.5rem 2rem; font-size: 12px; color: var(--text3); display: flex; gap: 16px; flex-wrap: wrap; align-items: center; }
  .footer-dot { width: 4px; height: 4px; border-radius: 50%; background: var(--border2); }

  @media (max-width: 600px) {
    .header, .toolbar, .grid-wrap, .stats-bar, .footer { padding-left: 1rem; padding-right: 1rem; }
    .grid { grid-template-columns: 1fr; }
    .form-row { grid-template-columns: 1fr; }
    .search-wrap input { width: 150px; }
    .pending-panel { margin: 0 1rem 1rem; }
  }
</style>
</head>
<body>

<div class="header">
  <div class="header-eyebrow">Breaking 動作分類庫</div>
  <div class="header-top">
    <h1>Freeze 技術資料庫</h1>
    <span class="version-tag">v1.1</span>
  </div>
  <div class="header-sub">Breaking 地板技術 — Freeze 動作完整分類 · 支撐點分析 · 肌群對應 · 功率指標</div>
  <div class="header-meta">
    <span class="header-meta-item"><span class="header-meta-dot"></span>BBoy / BGirl 訓練參考資料庫</span>
    <span class="header-meta-item"><span class="header-meta-dot" style="background:var(--amber-bd)"></span>支援社群投稿 · 審核後收錄</span>
  </div>
</div>

<div class="toolbar">
  <button class="filter-btn active" onclick="setDiff('all',this)">全部</button>
  <button class="filter-btn" onclick="setDiff('beginner',this)">入門</button>
  <button class="filter-btn" onclick="setDiff('intermediate',this)">中階</button>
  <button class="filter-btn" onclick="setDiff('advanced',this)">進階</button>
  <div class="search-wrap">
    <span class="search-icon">🔍</span>
    <input type="text" id="sq" placeholder="搜尋動作、肌群…" oninput="renderGrid()">
  </div>
  <button class="add-btn" onclick="openSubmit()">＋ 投稿動作</button>
</div>

<div class="stats-bar" id="stats-bar"></div>

<div id="pending-wrap"></div>

<div class="grid-wrap">
  <div class="grid" id="grid"></div>
</div>

<div class="footer">
  <span>Breaking 動作分類庫</span>
  <span class="footer-dot"></span>
  <span>僅供訓練參考，練習前請充分熱身</span>
  <span class="footer-dot"></span>
  <span>資料持續更新中</span>
</div>

<!-- Detail overlay -->
<div class="detail-overlay" id="overlay" onclick="handleOverlayClick(event)">
  <div class="detail-panel" id="detail-panel"></div>
</div>

<!-- Submit modal -->
<div class="modal-bg" id="submit-modal" onclick="handleModalClick(event)">
  <div class="modal" id="modal-inner">
    <h2>投稿新動作</h2>
    <p class="modal-sub">填寫動作資料，送出後將由管理員審核後上架</p>
    <div class="review-notice">
      ⏳ 審核說明：投稿內容將在 <strong>24–72 小時</strong>內完成審核。通過後動作才會正式收錄至資料庫。謝謝你的貢獻！
    </div>
    <div class="form-row">
      <div class="form-field">
        <label>動作英文名稱 *</label>
        <input type="text" id="f-name-en" placeholder="e.g. Baby Freeze">
      </div>
      <div class="form-field">
        <label>動作中文名稱 *</label>
        <input type="text" id="f-name-zh" placeholder="e.g. 嬰兒定格">
      </div>
    </div>
    <div class="form-row">
      <div class="form-field">
        <label>難度分級 *</label>
        <select id="f-diff">
          <option value="beginner">入門</option>
          <option value="intermediate">中階</option>
          <option value="advanced">進階</option>
        </select>
      </div>
      <div class="form-field">
        <label>主要支撐點</label>
        <input type="text" id="f-support" placeholder="e.g. 手肘, 頭側">
      </div>
    </div>
    <div class="form-field">
      <label>動作說明 / 教學要點 *</label>
      <textarea id="f-desc" placeholder="簡述動作要領、重心位置、常見錯誤等…"></textarea>
    </div>
    <div class="form-row">
      <div class="form-field">
        <label>主要肌群</label>
        <input type="text" id="f-muscles" placeholder="e.g. 三角肌, 核心">
      </div>
      <div class="form-field">
        <label>參考影片連結</label>
        <input type="text" id="f-video" placeholder="https://...">
      </div>
    </div>
    <div class="form-field">
      <label>你的名稱／暱稱</label>
      <input type="text" id="f-author" placeholder="e.g. BBoy Alex">
    </div>
    <div class="modal-actions">
      <button class="btn-cancel" onclick="closeSubmit()">取消</button>
      <button class="btn-submit" onclick="submitMove()">送出審核 →</button>
    </div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
const diffLabel = { beginner:'入門', intermediate:'中階', advanced:'進階' };
const pwrColors = { upper_limb:'#378ADD', core:'#7F77DD', lower_limb:'#1D9E75', balance:'#D85A30' };
const pwrNames  = { upper_limb:'上肢', core:'核心', lower_limb:'下肢', balance:'平衡' };

let db = [];
let pendingDB = JSON.parse(localStorage.getItem('breaking_pending') || '[]');
let activeDiff = 'all';
let showEdit = false;

// Admin password (demo)
const ADMIN_PW = 'breaking2025';

const FALLBACK_DB = [
  { id:1, name:"Baby Freeze", nameZh:"嬰兒定格", difficulty:"beginner", support_points:["手肘","頭側"], muscles:["三角肌","肱三頭肌","腹橫肌","髖屈肌"], power_index:{ upper_limb:30, core:40, lower_limb:20, balance:60, total:38, source:"estimated", measured_at:null, measured_by:null }, steps:["雙膝跪地，雙手撐地","右手肘彎曲，讓頭側靠在右手肘上","左手撐地保持平衡","慢慢讓雙腳離地，保持靜止","收緊核心維持3秒以上"], variants:["Double Baby Freeze","Elbow Baby","One Leg Baby"], note:"最推薦入門的 Freeze，手肘作為支點能大幅降低平衡難度。重心要壓低，不要聳肩。", video_url:"" },
  { id:2, name:"Chair Freeze", nameZh:"椅子定格", difficulty:"intermediate", support_points:["單手肘","腰側"], muscles:["肱三頭肌","前鋸肌","腹斜肌","臀中肌"], power_index:{ upper_limb:55, core:65, lower_limb:30, balance:70, total:55, source:"estimated", measured_at:null, measured_by:null }, steps:["側身，右手肘彎曲頂住右側腰部","左手撐地輔助平衡","核心收緊，讓身體側向離地","雙腳可伸直或彎曲","保持頭部放鬆，眼睛看地板"], variants:["High Chair","One Arm Chair","Chair to Headstand"], note:"手肘頂腰的位置非常關鍵，約在腰骨上方。初學者可先靠牆練習找到重心點。", video_url:"" },
  { id:3, name:"Headstand Freeze", nameZh:"頭倒立定格", difficulty:"intermediate", support_points:["頭頂","雙手"], muscles:["頸部肌群","斜方肌","三角肌","腹直肌"], power_index:{ upper_limb:50, core:70, lower_limb:40, balance:75, total:59, source:"estimated", measured_at:null, measured_by:null }, steps:["雙手與頭頂形成三角形支撐點","頭頂輕放地板，非頸部承重","核心用力將雙腿緩緩抬起","雙腿可合攏或張開","保持呼吸，不要屏氣"], variants:["Stab","Headstand to Spin","Tripod"], note:"頭頂接觸點應在頭頂前三分之一，三點要形成穩定等邊三角形。", video_url:"" },
  { id:4, name:"Elbow Freeze", nameZh:"手肘定格", difficulty:"intermediate", support_points:["雙手肘"], muscles:["肱三頭肌","胸大肌","前鋸肌","腹橫肌"], power_index:{ upper_limb:70, core:75, lower_limb:25, balance:72, total:61, source:"estimated", measured_at:null, measured_by:null }, steps:["雙手肘撐地，形成穩定三角架","前臂貼地，掌心朝下","核心繃緊，讓腹部靠近手肘","慢慢讓雙腿離地向後伸展","身體呈水平或斜向懸空"], variants:["Planche Freeze","Tuck Elbow","Elbow to Airchair"], note:"與體操中的俯臥撐不同，重心更偏後。肩膀保持穩定不聳起是關鍵。", video_url:"" },
  { id:5, name:"Shoulder Freeze", nameZh:"肩膀定格", difficulty:"intermediate", support_points:["肩膀","頸側"], muscles:["斜方肌","三角肌","頸側肌群","腹斜肌"], power_index:{ upper_limb:60, core:68, lower_limb:35, balance:65, total:57, source:"estimated", measured_at:null, measured_by:null }, steps:["側躺，以肩膀與頸側為支點","雙手輔助撐地穩定","核心用力讓下半身抬離地板","雙腿可併攏或張開造型","保持頭部不動，眼睛看天花板"], variants:["Shoulder Roll to Freeze","Shoulder to Headspin","Side Freeze"], note:"肩頸承壓需要充分熱身。初學者可先練習肩橋動作強化頸部穩定性。", video_url:"" },
  { id:6, name:"Airchair", nameZh:"空椅定格", difficulty:"advanced", support_points:["單手"], muscles:["肱三頭肌","三角肌","前鋸肌","腹斜肌","髖屈肌"], power_index:{ upper_limb:90, core:85, lower_limb:40, balance:88, total:76, source:"estimated", measured_at:null, measured_by:null }, steps:["從 Chair Freeze 開始建立基礎","單手撐地，移除手肘支撐","核心極度收緊，臀部上提","下方腿向外伸展，上方腿彎曲","全身重量由單手腕與手指承受"], variants:["Hollowback Airchair","1-Arm Airchair","Airchair to Flare"], note:"需要長期手腕強化訓練。建議從 Chair Freeze 熟練後再進階，腕部熱身必不可少。", video_url:"" },
  { id:7, name:"Pike Freeze", nameZh:"折刀定格", difficulty:"advanced", support_points:["雙手","頭"], muscles:["三角肌","肱三頭肌","腹直肌","豎脊肌","髖屈肌"], power_index:{ upper_limb:85, core:90, lower_limb:50, balance:85, total:78, source:"estimated", measured_at:null, measured_by:null }, steps:["完全的手倒立基礎必備","雙手撐地，雙腿伸直合攏","核心極度收緊讓身體筆直","定格時全身呈一直線","進階可用頭頸接觸地板輔助"], variants:["Handstand Freeze","Pike to Spin","V-Freeze"], note:"需要完整的倒立訓練基礎（建議可保持 10 秒以上穩定倒立後再嘗試）。", video_url:"" },
  { id:8, name:"1-Hand Freeze", nameZh:"單手定格", difficulty:"advanced", support_points:["單手"], muscles:["肱三頭肌","三角肌","前鋸肌","腕屈肌","腹橫肌"], power_index:{ upper_limb:95, core:88, lower_limb:45, balance:92, total:80, source:"estimated", measured_at:null, measured_by:null }, steps:["從雙手倒立開始","慢慢將重心移至慣用手","非慣用手緩緩抬起","全身重量集中在單手與手指","核心與臀肌持續用力維持直線"], variants:["1-Hand Press","Flag Freeze","1-Hand Airchair"], note:"此動作需數年訓練積累。手腕強化、手指力量與核心控制缺一不可。", video_url:"" }
];

async function loadDB() {
  try {
    const res = await fetch('moves_db.json');
    const json = await res.json();
    db = json.moves;
  } catch(e) {
    db = FALLBACK_DB;
  }
  renderPending();
  renderGrid();
}

function setDiff(d, btn) {
  activeDiff = d;
  document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  renderGrid();
}

function renderGrid() {
  const q = document.getElementById('sq').value.toLowerCase();
  const filtered = db.filter(m => {
    if (activeDiff !== 'all' && m.difficulty !== activeDiff) return false;
    if (q && !m.name.toLowerCase().includes(q) &&
            !(m.nameZh||'').includes(q) &&
            !m.muscles.join().toLowerCase().includes(q) &&
            !m.support_points.join().toLowerCase().includes(q)) return false;
    return true;
  });

  const estimated = filtered.filter(m => m.power_index.source === 'estimated').length;
  const measured  = filtered.filter(m => m.power_index.source === 'measured').length;
  document.getElementById('stats-bar').innerHTML =
    `<span>顯示 <b>${filtered.length}</b> 個動作</span>` +
    `<span><span class="source-dot source-measured" style="display:inline-block;margin-right:3px;vertical-align:middle"></span>實測 <b>${measured}</b></span>` +
    `<span><span class="source-dot source-estimated" style="display:inline-block;margin-right:3px;vertical-align:middle"></span>預估 <b>${estimated}</b></span>`;

  if (!filtered.length) {
    document.getElementById('grid').innerHTML = '<div class="empty">沒有符合條件的動作</div>';
    return;
  }

  document.getElementById('grid').innerHTML = filtered.map(m => {
    const pi = m.power_index;
    const barColor = pi.total >= 70 ? '#D85A30' : pi.total >= 50 ? '#EF9F27' : '#1D9E75';
    return `<div class="card" onclick="showDetail(${m.id})">
      <div class="card-head">
        <div>
          <div class="card-name-en">${m.name}</div>
          <div class="card-name-zh">${m.nameZh || ''}</div>
        </div>
        <span class="badge badge-${m.difficulty}">${diffLabel[m.difficulty]}</span>
      </div>
      <div class="support-row">${m.support_points.map(s=>`<span class="stag">${s}</span>`).join('')}</div>
      <div class="pwr-row">
        <span class="pwr-label">功率</span>
        <div class="pwr-bar-bg"><div class="pwr-bar-fill" style="width:${pi.total}%;background:${barColor}"></div></div>
        <span class="pwr-val">${pi.total}</span>
      </div>
      <div class="support-row">${m.muscles.slice(0,3).map(s=>`<span class="stag">${s}</span>`).join('')}${m.muscles.length>3?`<span class="stag">+${m.muscles.length-3}</span>`:''}</div>
      <div class="card-footer">
        <span class="source-dot ${pi.source==='measured'?'source-measured':'source-estimated'}"></span>
        <span class="card-source-txt">${pi.source==='measured'?`實測 · ${pi.measured_at||''}`:'預估值 — 待實測'}</span>
      </div>
    </div>`;
  }).join('');
}

function showDetail(id) {
  const m = db.find(x => x.id === id) || pendingDB.find(x => x.id === id);
  if (!m) return;
  showEdit = false;
  renderDetail(m);
  document.getElementById('overlay').classList.add('open');
}

function renderDetail(m) {
  const pi = m.power_index || { upper_limb:0, core:0, lower_limb:0, balance:0, total:0, source:'estimated' };
  const pwrRows = Object.entries(pwrNames).map(([k,label]) => `
    <div class="pwr-detail-row">
      <span class="pwr-detail-label">${label}</span>
      <div class="pwr-detail-bar"><div class="pwr-detail-fill" style="width:${pi[k]||0}%;background:${pwrColors[k]}"></div></div>
      <span class="pwr-detail-val" style="color:${pwrColors[k]}">${pi[k]||0}</span>
    </div>`).join('');

  const editSection = showEdit ? `
    <div class="edit-section">
      <div class="sec-label">回填實測數據</div>
      <div class="edit-grid">
        <div class="edit-field"><label>上肢功率</label><input type="number" id="ei-ul" min="0" max="100" value="${pi.upper_limb}"></div>
        <div class="edit-field"><label>核心功率</label><input type="number" id="ei-co" min="0" max="100" value="${pi.core}"></div>
        <div class="edit-field"><label>下肢功率</label><input type="number" id="ei-ll" min="0" max="100" value="${pi.lower_limb}"></div>
        <div class="edit-field"><label>平衡指數</label><input type="number" id="ei-ba" min="0" max="100" value="${pi.balance}"></div>
      </div>
      <div class="edit-grid">
        <div class="edit-field"><label>測量日期</label><input type="date" id="ei-date" value="${pi.measured_at||''}"></div>
        <div class="edit-field"><label>測量者</label><input type="text" id="ei-by" placeholder="教練姓名" value="${pi.measured_by||''}"></div>
      </div>
      <div class="edit-field" style="margin-bottom:8px"><label>影片連結</label><input type="text" id="ei-video" placeholder="https://..." value="${m.video_url||''}"></div>
      <button class="save-btn" onclick="saveData(${m.id})">儲存實測數據</button>
      <div class="saved-msg" id="saved-msg">已儲存！請手動更新 moves_db.json</div>
    </div>` : '';

  document.getElementById('detail-panel').innerHTML = `
    <div class="detail-top">
      <div class="detail-titles">
        <span class="detail-name-en">${m.name}</span>
        <span class="detail-name-zh">${m.nameZh || ''}</span>
      </div>
      <div style="display:flex;align-items:center;gap:8px">
        <span class="badge badge-${m.difficulty}">${diffLabel[m.difficulty]}</span>
        <button class="close-btn" onclick="closeDetail()">×</button>
      </div>
    </div>
    <div class="sec"><div class="sec-label">支撐點</div><div class="tag-row">${(m.support_points||[]).map(s=>`<span class="tag">${s}</span>`).join('')}</div></div>
    <div class="sec">
      <div class="sec-label">功率指標（0–100）</div>
      ${pwrRows}
      <div class="pwr-total-row"><span>綜合功率指數</span><span>${pi.total}</span></div>
      <div class="source-info">
        <span class="source-dot ${pi.source==='measured'?'source-measured':'source-estimated'}"></span>
        ${pi.source==='measured'?`實測數據 · ${pi.measured_at||''} · ${pi.measured_by||''}`:'目前為預估值，等待拍攝分析後回填'}
      </div>
    </div>
    <div class="sec"><div class="sec-label">主要肌群</div><div class="tag-row">${(m.muscles||[]).map(s=>`<span class="tag muscle-tag">${s}</span>`).join('')}</div></div>
    ${m.steps ? `<div class="sec"><div class="sec-label">教學步驟</div><div class="step-list">${m.steps.map((s,i)=>`<div class="step-item"><span class="step-num">${i+1}</span><span class="step-txt">${s}</span></div>`).join('')}</div></div>` : ''}
    ${m.variants ? `<div class="sec"><div class="sec-label">變化型</div><div class="tag-row">${m.variants.map(s=>`<span class="tag variant-tag">${s}</span>`).join('')}</div></div>` : ''}
    ${m.note ? `<div class="sec"><div class="sec-label">備注</div><div class="note-box">${m.note}</div></div>` : ''}
    ${db.find(x=>x.id===m.id) ? `<button class="edit-btn" onclick="toggleEdit(${m.id})">${showEdit?'收起':'回填實測數據 ↓'}</button>${editSection}` : ''}
  `;
}

function toggleEdit(id) {
  showEdit = !showEdit;
  renderDetail(db.find(x => x.id === id));
}

function saveData(id) {
  const m = db.find(x => x.id === id);
  m.power_index.upper_limb = parseInt(document.getElementById('ei-ul').value)||0;
  m.power_index.core = parseInt(document.getElementById('ei-co').value)||0;
  m.power_index.lower_limb = parseInt(document.getElementById('ei-ll').value)||0;
  m.power_index.balance = parseInt(document.getElementById('ei-ba').value)||0;
  m.power_index.total = Math.round((m.power_index.upper_limb + m.power_index.core + m.power_index.lower_limb + m.power_index.balance) / 4);
  m.power_index.source = 'measured';
  m.power_index.measured_at = document.getElementById('ei-date').value;
  m.power_index.measured_by = document.getElementById('ei-by').value;
  const vd = document.getElementById('ei-video').value;
  if (vd) m.video_url = vd;

  const blob = new Blob([JSON.stringify({ version:"1.1", last_updated: new Date().toISOString().slice(0,10), category:"freeze", moves: db }, null, 2)], {type:'application/json'});
  const a = document.createElement('a'); a.href = URL.createObjectURL(blob); a.download = 'moves_db.json'; a.click();
  document.getElementById('saved-msg').style.display = 'block';
  renderGrid();
  showToast('數據已儲存並下載 ✓');
}

function closeDetail() {
  document.getElementById('overlay').classList.remove('open');
  showEdit = false;
}

function handleOverlayClick(e) {
  if (e.target === document.getElementById('overlay')) closeDetail();
}

/* ── Submit ── */
function openSubmit() { document.getElementById('submit-modal').classList.add('open'); }
function closeSubmit() { document.getElementById('submit-modal').classList.remove('open'); }
function handleModalClick(e) { if (e.target === document.getElementById('submit-modal')) closeSubmit(); }

function submitMove() {
  const nameEn = document.getElementById('f-name-en').value.trim();
  const nameZh = document.getElementById('f-name-zh').value.trim();
  const desc   = document.getElementById('f-desc').value.trim();
  if (!nameEn || !nameZh || !desc) { showToast('請填寫必填欄位（*）'); return; }

  const entry = {
    id: Date.now(),
    name: nameEn,
    nameZh: nameZh,
    difficulty: document.getElementById('f-diff').value,
    support_points: document.getElementById('f-support').value.split(/[,，]/).map(s=>s.trim()).filter(Boolean),
    muscles: document.getElementById('f-muscles').value.split(/[,，]/).map(s=>s.trim()).filter(Boolean),
    note: desc,
    video_url: document.getElementById('f-video').value.trim(),
    author: document.getElementById('f-author').value.trim() || '匿名',
    submitted_at: new Date().toISOString().slice(0,10),
    status: 'pending',
    power_index: { upper_limb:0, core:0, lower_limb:0, balance:0, total:0, source:'estimated' },
    steps: [], variants: []
  };

  pendingDB.push(entry);
  localStorage.setItem('breaking_pending', JSON.stringify(pendingDB));
  closeSubmit();
  clearForm();
  renderPending();
  showToast('投稿成功！審核中 ⏳');
}

function clearForm() {
  ['f-name-en','f-name-zh','f-support','f-desc','f-muscles','f-video','f-author'].forEach(id => document.getElementById(id).value = '');
}

function renderPending() {
  const wrap = document.getElementById('pending-wrap');
  if (!pendingDB.length) { wrap.innerHTML = ''; return; }
  wrap.innerHTML = `
    <div class="pending-panel">
      <div class="pending-header">
        <span class="pending-title">⏳ 待審核投稿</span>
        <span class="pending-count">${pendingDB.length}</span>
      </div>
      <div class="pending-list">
        ${pendingDB.map(p => `
          <div class="pending-item">
            <div>
              <div class="pending-item-name">${p.name} <span style="font-weight:400;color:var(--text3)">${p.nameZh}</span></div>
              <div class="pending-item-sub">${diffLabel[p.difficulty]} · 投稿人：${p.author} · ${p.submitted_at}</div>
            </div>
            <div style="display:flex;gap:5px;flex-shrink:0">
              <button class="pending-approve-btn" onclick="approvePending(${p.id})">通過</button>
              <button class="pending-reject-btn" onclick="rejectPending(${p.id})">退回</button>
            </div>
          </div>`).join('')}
      </div>
      <div style="font-size:11px;color:var(--amber);margin-top:8px;opacity:.7">管理員操作：通過後動作將正式收錄</div>
    </div>`;
}

function approvePending(id) {
  const pw = prompt('請輸入管理員密碼：');
  if (pw !== ADMIN_PW) { showToast('密碼錯誤'); return; }
  const idx = pendingDB.findIndex(x => x.id === id);
  if (idx === -1) return;
  const m = { ...pendingDB[idx], status: 'approved' };
  db.push(m);
  pendingDB.splice(idx, 1);
  localStorage.setItem('breaking_pending', JSON.stringify(pendingDB));
  renderPending();
  renderGrid();
  showToast('已通過審核，動作已收錄 ✓');
}

function rejectPending(id) {
  const pw = prompt('請輸入管理員密碼：');
  if (pw !== ADMIN_PW) { showToast('密碼錯誤'); return; }
  pendingDB = pendingDB.filter(x => x.id !== id);
  localStorage.setItem('breaking_pending', JSON.stringify(pendingDB));
  renderPending();
  showToast('已退回投稿');
}

function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 2800);
}

loadDB();
</script>
</body>
</html>
