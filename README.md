# budget-tracker
Aplikasi Budget Tracker untuk mengelola pemasukan &amp; pengeluaran bulanan &amp; tahunan dengan riwayat transaksi
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Budget Tracker</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0d0f14;
    --surface: #161a23;
    --surface2: #1e2330;
    --border: #2a3042;
    --accent-green: #00e5a0;
    --accent-red: #ff4d6d;
    --accent-blue: #4d9fff;
    --accent-yellow: #ffd166;
    --text: #e8eaf2;
    --text-muted: #7a8199;
    --font-display: 'Syne', sans-serif;
    --font-body: 'DM Sans', sans-serif;
    --radius: 14px;
    --radius-sm: 8px;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--font-body);
    min-height: 100vh;
    padding: 0;
  }

  /* Noise overlay */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 0;
    opacity: 0.3;
  }

  .container {
    max-width: 1100px;
    margin: 0 auto;
    padding: 32px 20px 60px;
    position: relative;
    z-index: 1;
  }

  /* HEADER */
  header {
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
    margin-bottom: 36px;
    flex-wrap: wrap;
    gap: 16px;
  }

  .header-left h1 {
    font-family: var(--font-display);
    font-size: 2.2rem;
    font-weight: 800;
    letter-spacing: -0.5px;
    line-height: 1.1;
  }

  .header-left h1 span { color: var(--accent-green); }

  .header-left p {
    color: var(--text-muted);
    font-size: 0.9rem;
    margin-top: 6px;
  }

  .header-actions {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
    align-items: center;
  }

  /* TABS */
  .tabs {
    display: flex;
    gap: 4px;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 5px;
    margin-bottom: 28px;
  }

  .tab-btn {
    flex: 1;
    padding: 10px 20px;
    border: none;
    border-radius: var(--radius-sm);
    background: transparent;
    color: var(--text-muted);
    font-family: var(--font-display);
    font-size: 0.9rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s;
    letter-spacing: 0.3px;
  }

  .tab-btn.active {
    background: var(--surface2);
    color: var(--text);
    box-shadow: 0 2px 8px rgba(0,0,0,0.3);
  }

  /* SUMMARY CARDS */
  .summary-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 16px;
    margin-bottom: 28px;
  }

  @media (max-width: 640px) { .summary-grid { grid-template-columns: 1fr; } }

  .summary-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 22px;
    position: relative;
    overflow: hidden;
    transition: transform 0.2s;
  }

  .summary-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0;
    width: 100%; height: 3px;
  }

  .summary-card.income::before { background: var(--accent-green); }
  .summary-card.expense::before { background: var(--accent-red); }
  .summary-card.balance::before { background: var(--accent-blue); }

  .summary-card:hover { transform: translateY(-2px); }

  .card-label {
    font-size: 0.75rem;
    text-transform: uppercase;
    letter-spacing: 1.5px;
    color: var(--text-muted);
    margin-bottom: 10px;
    font-weight: 500;
  }

  .card-value {
    font-family: var(--font-display);
    font-size: 1.6rem;
    font-weight: 700;
  }

  .card-value.income { color: var(--accent-green); }
  .card-value.expense { color: var(--accent-red); }
  .card-value.balance { color: var(--accent-blue); }

  /* MAIN LAYOUT */
  .main-grid {
    display: grid;
    grid-template-columns: 1fr 340px;
    gap: 20px;
    align-items: start;
  }

  @media (max-width: 860px) { .main-grid { grid-template-columns: 1fr; } }

  /* PANEL */
  .panel {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    overflow: hidden;
  }

  .panel-header {
    padding: 18px 22px;
    border-bottom: 1px solid var(--border);
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .panel-title {
    font-family: var(--font-display);
    font-size: 1rem;
    font-weight: 700;
  }

  /* TABLE */
  .entries-table {
    width: 100%;
    border-collapse: collapse;
  }

  .entries-table th {
    text-align: left;
    padding: 12px 22px;
    font-size: 0.72rem;
    text-transform: uppercase;
    letter-spacing: 1.2px;
    color: var(--text-muted);
    font-weight: 500;
    border-bottom: 1px solid var(--border);
    background: var(--surface2);
  }

  .entries-table td {
    padding: 13px 22px;
    font-size: 0.9rem;
    border-bottom: 1px solid var(--border);
    vertical-align: middle;
  }

  .entries-table tr:last-child td { border-bottom: none; }

  .entries-table tr:hover td { background: rgba(255,255,255,0.02); }

  .type-badge {
    display: inline-flex;
    align-items: center;
    gap: 5px;
    padding: 3px 10px;
    border-radius: 20px;
    font-size: 0.75rem;
    font-weight: 600;
  }

  .type-badge.income { background: rgba(0,229,160,0.12); color: var(--accent-green); }
  .type-badge.expense { background: rgba(255,77,109,0.12); color: var(--accent-red); }

  .amount-cell.income { color: var(--accent-green); font-weight: 600; }
  .amount-cell.expense { color: var(--accent-red); font-weight: 600; }

  .delete-btn {
    background: none;
    border: none;
    color: var(--text-muted);
    cursor: pointer;
    padding: 4px;
    border-radius: 6px;
    transition: all 0.15s;
    font-size: 1rem;
  }

  .delete-btn:hover { color: var(--accent-red); background: rgba(255,77,109,0.1); }

  .empty-state {
    padding: 48px 22px;
    text-align: center;
    color: var(--text-muted);
    font-size: 0.9rem;
  }

  .empty-state .empty-icon {
    font-size: 2.5rem;
    margin-bottom: 12px;
    display: block;
  }

  /* ADD FORM */
  .form-inner { padding: 20px; display: flex; flex-direction: column; gap: 14px; }

  .form-row { display: flex; flex-direction: column; gap: 6px; }

  .form-label {
    font-size: 0.75rem;
    text-transform: uppercase;
    letter-spacing: 1px;
    color: var(--text-muted);
    font-weight: 500;
  }

  .form-input, .form-select {
    width: 100%;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    padding: 10px 14px;
    color: var(--text);
    font-family: var(--font-body);
    font-size: 0.9rem;
    outline: none;
    transition: border-color 0.2s;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
  }

  .form-input:focus, .form-select:focus {
    border-color: var(--accent-green);
    box-shadow: 0 0 0 3px rgba(0,229,160,0.1);
  }

  .form-select option { background: var(--surface2); }

  .type-toggle {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 6px;
  }

  .type-toggle-btn {
    padding: 9px;
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    background: transparent;
    color: var(--text-muted);
    font-family: var(--font-display);
    font-size: 0.85rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s;
  }

  .type-toggle-btn.active-income {
    border-color: var(--accent-green);
    background: rgba(0,229,160,0.1);
    color: var(--accent-green);
  }

  .type-toggle-btn.active-expense {
    border-color: var(--accent-red);
    background: rgba(255,77,109,0.1);
    color: var(--accent-red);
  }

  /* BUTTONS */
  .btn {
    padding: 10px 18px;
    border-radius: var(--radius-sm);
    border: none;
    font-family: var(--font-display);
    font-weight: 700;
    font-size: 0.85rem;
    cursor: pointer;
    transition: all 0.2s;
    letter-spacing: 0.3px;
    display: inline-flex;
    align-items: center;
    gap: 6px;
    white-space: nowrap;
  }

  .btn-primary {
    background: var(--accent-green);
    color: #0d0f14;
    width: 100%;
    justify-content: center;
    padding: 13px;
    font-size: 0.9rem;
  }

  .btn-primary:hover { background: #00c98e; transform: translateY(-1px); }

  .btn-secondary {
    background: var(--surface2);
    color: var(--text);
    border: 1px solid var(--border);
  }

  .btn-secondary:hover { border-color: var(--text-muted); }

  .btn-danger {
    background: transparent;
    color: var(--accent-red);
    border: 1px solid rgba(255,77,109,0.3);
  }

  .btn-danger:hover { background: rgba(255,77,109,0.1); }

  .btn-save {
    background: var(--accent-blue);
    color: #fff;
  }

  .btn-save:hover { background: #3a8fee; }

  /* CATEGORY PILLS */
  .category-list {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
    padding: 14px 22px;
  }

  .cat-pill {
    padding: 4px 12px;
    border-radius: 20px;
    font-size: 0.78rem;
    font-weight: 600;
    border: 1px solid var(--border);
    background: var(--surface2);
    color: var(--text-muted);
    display: flex;
    align-items: center;
    gap: 5px;
    cursor: default;
  }

  .cat-pill .cat-remove {
    cursor: pointer;
    opacity: 0.5;
    font-size: 0.85rem;
    line-height: 1;
    transition: opacity 0.15s;
  }

  .cat-pill .cat-remove:hover { opacity: 1; }

  /* MONTH SELECTOR */
  .month-bar {
    display: flex;
    gap: 8px;
    align-items: center;
    flex-wrap: wrap;
    margin-bottom: 20px;
  }

  .month-bar label {
    font-size: 0.8rem;
    color: var(--text-muted);
    font-weight: 500;
  }

  .month-select {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    padding: 8px 14px;
    color: var(--text);
    font-family: var(--font-body);
    font-size: 0.9rem;
    outline: none;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    cursor: pointer;
  }

  .month-select:focus { border-color: var(--accent-green); }

  /* TOAST */
  #toast {
    position: fixed;
    bottom: 28px;
    right: 28px;
    padding: 13px 20px;
    border-radius: var(--radius-sm);
    background: var(--surface2);
    border: 1px solid var(--border);
    color: var(--text);
    font-size: 0.88rem;
    font-family: var(--font-body);
    z-index: 100;
    opacity: 0;
    transform: translateY(12px);
    transition: all 0.3s;
    pointer-events: none;
    display: flex;
    align-items: center;
    gap: 8px;
    min-width: 220px;
    box-shadow: 0 8px 24px rgba(0,0,0,0.4);
  }

  #toast.show { opacity: 1; transform: translateY(0); }

  /* ANNUAL VIEW */
  .annual-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
  }

  @media (max-width: 640px) { .annual-grid { grid-template-columns: repeat(2, 1fr); } }

  .month-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 16px;
    cursor: pointer;
    transition: all 0.2s;
  }

  .month-card:hover {
    border-color: var(--accent-green);
    transform: translateY(-2px);
  }

  .month-card .mc-name {
    font-family: var(--font-display);
    font-weight: 700;
    font-size: 0.95rem;
    margin-bottom: 10px;
  }

  .month-card .mc-row {
    display: flex;
    justify-content: space-between;
    font-size: 0.78rem;
    margin-bottom: 4px;
  }

  .mc-row .mc-label { color: var(--text-muted); }
  .mc-row .mc-val.inc { color: var(--accent-green); font-weight: 600; }
  .mc-row .mc-val.exp { color: var(--accent-red); font-weight: 600; }

  .mc-balance {
    margin-top: 10px;
    padding-top: 10px;
    border-top: 1px solid var(--border);
    font-family: var(--font-display);
    font-size: 0.85rem;
    font-weight: 700;
    display: flex;
    justify-content: space-between;
  }

  /* SCROLLABLE TABLE */
  .table-wrap { overflow-x: auto; max-height: 420px; overflow-y: auto; }

  .table-wrap::-webkit-scrollbar { width: 6px; height: 6px; }
  .table-wrap::-webkit-scrollbar-track { background: transparent; }
  .table-wrap::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }

  /* ADD CATEGORY INLINE */
  .cat-add-row {
    display: flex;
    gap: 8px;
    padding: 12px 22px;
    border-top: 1px solid var(--border);
  }

  .cat-add-row input {
    flex: 1;
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    padding: 8px 12px;
    color: var(--text);
    font-size: 0.85rem;
    outline: none;
    font-family: var(--font-body);
  }

  .cat-add-row input:focus { border-color: var(--accent-green); }

  .hidden { display: none !important; }

  .year-input {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    padding: 8px 12px;
    color: var(--text);
    font-size: 0.9rem;
    width: 90px;
    text-align: center;
    font-family: var(--font-display);
    font-weight: 700;
    outline: none;
  }

  .year-input:focus { border-color: var(--accent-green); }
</style>
</head>
<body>
<div class="container">
  <header>
    <div class="header-left">
      <h1>Budget <span>Tracker</span></h1>
      <p>Kelola pemasukan & pengeluaran bulanan & tahunan Anda</p>
    </div>
    <div class="header-actions">
      <button class="btn btn-save" onclick="saveData()">💾 Simpan Data</button>
      <button class="btn btn-danger" onclick="confirmReset()">🔄 Reset Semua</button>
    </div>
  </header>

  <!-- TABS -->
  <div class="tabs">
    <button class="tab-btn active" onclick="switchTab('monthly')">📅 Bulanan</button>
    <button class="tab-btn" onclick="switchTab('annual')">📊 Tahunan</button>
  </div>

  <!-- MONTHLY VIEW -->
  <div id="tab-monthly">
    <!-- Month & Year Selector -->
    <div class="month-bar">
      <label>Periode:</label>
      <select class="month-select" id="monthSelect" onchange="renderMonthly()">
        <option value="0">Januari</option><option value="1">Februari</option>
        <option value="2">Maret</option><option value="3">April</option>
        <option value="4">Mei</option><option value="5">Juni</option>
        <option value="6">Juli</option><option value="7">Agustus</option>
        <option value="8">September</option><option value="9">Oktober</option>
        <option value="10">November</option><option value="11">Desember</option>
      </select>
      <input type="number" class="year-input" id="yearInput" value="" onchange="renderMonthly()" min="2000" max="2100">
    </div>

    <!-- Summary -->
    <div class="summary-grid">
      <div class="summary-card income">
        <div class="card-label">Total Pemasukan</div>
        <div class="card-value income" id="totalIncome">Rp 0</div>
      </div>
      <div class="summary-card expense">
        <div class="card-label">Total Pengeluaran</div>
        <div class="card-value expense" id="totalExpense">Rp 0</div>
      </div>
      <div class="summary-card balance">
        <div class="card-label">Saldo</div>
        <div class="card-value balance" id="totalBalance">Rp 0</div>
      </div>
    </div>

    <!-- Main Grid -->
    <div class="main-grid">
      <!-- Entries Table -->
      <div class="panel">
        <div class="panel-header">
          <span class="panel-title">📋 Daftar Transaksi</span>
          <span style="font-size:0.8rem;color:var(--text-muted)" id="entryCount">0 entri</span>
        </div>
        <div class="table-wrap">
          <table class="entries-table">
            <thead>
              <tr>
                <th>Tanggal</th>
                <th>Kategori</th>
                <th>Keterangan</th>
                <th>Tipe</th>
                <th>Jumlah</th>
                <th></th>
              </tr>
            </thead>
            <tbody id="entriesBody"></tbody>
          </table>
        </div>
      </div>

      <!-- Sidebar -->
      <div style="display:flex;flex-direction:column;gap:16px;">
        <!-- Add Transaction -->
        <div class="panel">
          <div class="panel-header">
            <span class="panel-title">➕ Tambah Transaksi</span>
          </div>
          <div class="form-inner">
            <div class="form-row">
              <span class="form-label">Tipe</span>
              <div class="type-toggle">
                <button class="type-toggle-btn active-income" id="btnIncome" onclick="setType('income')">↑ Pemasukan</button>
                <button class="type-toggle-btn" id="btnExpense" onclick="setType('expense')">↓ Pengeluaran</button>
              </div>
            </div>
            <div class="form-row">
              <label class="form-label">Tanggal</label>
              <input type="date" class="form-input" id="inputDate">
            </div>
            <div class="form-row">
              <label class="form-label">Kategori</label>
              <select class="form-select" id="inputCategory"></select>
            </div>
            <div class="form-row">
              <label class="form-label">Keterangan</label>
              <input type="text" class="form-input" id="inputDesc" placeholder="Misal: belanja bulanan...">
            </div>
            <div class="form-row">
              <label class="form-label">Jumlah (Rp)</label>
              <input type="number" class="form-input" id="inputAmount" placeholder="0" min="0">
            </div>
            <button class="btn btn-primary" onclick="addEntry()">+ Tambah</button>
          </div>
        </div>

        <!-- Categories -->
        <div class="panel">
          <div class="panel-header">
            <span class="panel-title">🏷️ Kategori</span>
          </div>
          <div class="category-list" id="categoryList"></div>
          <div class="cat-add-row">
            <input type="text" id="newCatInput" placeholder="Tambah kategori baru..." onkeydown="if(event.key==='Enter')addCategory()">
            <button class="btn btn-secondary" onclick="addCategory()">+ Tambah</button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- ANNUAL VIEW -->
  <div id="tab-annual" class="hidden">
    <div class="month-bar">
      <label>Tahun:</label>
      <input type="number" class="year-input" id="annualYear" value="" onchange="renderAnnual()" min="2000" max="2100">
    </div>
    <!-- Annual Summary -->
    <div class="summary-grid" style="margin-bottom:28px;">
      <div class="summary-card income">
        <div class="card-label">Total Pemasukan Tahunan</div>
        <div class="card-value income" id="annualIncome">Rp 0</div>
      </div>
      <div class="summary-card expense">
        <div class="card-label">Total Pengeluaran Tahunan</div>
        <div class="card-value expense" id="annualExpense">Rp 0</div>
      </div>
      <div class="summary-card balance">
        <div class="card-label">Saldo Tahunan</div>
        <div class="card-value balance" id="annualBalance">Rp 0</div>
      </div>
    </div>
    <div class="annual-grid" id="annualGrid"></div>
  </div>
</div>

<!-- Toast -->
<div id="toast">✓ <span id="toastMsg"></span></div>

<script>
const MONTHS = ['Januari','Februari','Maret','April','Mei','Juni','Juli','Agustus','September','Oktober','November','Desember'];

// STATE
let state = {
  entries: [],
  categories: ['Gaji','Freelance','Investasi','Makanan','Transport','Listrik','Air','Internet','Kesehatan','Hiburan','Belanja','Tabungan','Lainnya'],
  currentType: 'income'
};

// Init dates
const now = new Date();
document.getElementById('monthSelect').value = now.getMonth();
document.getElementById('yearInput').value = now.getFullYear();
document.getElementById('annualYear').value = now.getFullYear();
document.getElementById('inputDate').value = now.toISOString().split('T')[0];

// LOAD
function loadData() {
  try {
    const saved = localStorage.getItem('budgetTrackerData');
    if (saved) {
      const parsed = JSON.parse(saved);
      state.entries = parsed.entries || [];
      state.categories = parsed.categories || state.categories;
    }
  } catch(e) {}
}

// SAVE
function saveData() {
  try {
    localStorage.setItem('budgetTrackerData', JSON.stringify({
      entries: state.entries,
      categories: state.categories
    }));
    showToast('✅ Data berhasil disimpan!');
  } catch(e) {
    showToast('❌ Gagal menyimpan data');
  }
}

// RESET
function confirmReset() {
  if (confirm('⚠️ Yakin ingin mereset SEMUA data menjadi 0? Aksi ini tidak bisa dibatalkan!')) {
    state.entries = [];
    showToast('🔄 Semua data telah direset');
    renderAll();
  }
}

// TYPE
function setType(type) {
  state.currentType = type;
  document.getElementById('btnIncome').className = 'type-toggle-btn' + (type==='income' ? ' active-income' : '');
  document.getElementById('btnExpense').className = 'type-toggle-btn' + (type==='expense' ? ' active-expense' : '');
}

// ADD ENTRY
function addEntry() {
  const date = document.getElementById('inputDate').value;
  const cat = document.getElementById('inputCategory').value;
  const desc = document.getElementById('inputDesc').value.trim();
  const amount = parseFloat(document.getElementById('inputAmount').value);

  if (!date) { showToast('⚠️ Pilih tanggal terlebih dahulu'); return; }
  if (!amount || amount <= 0) { showToast('⚠️ Masukkan jumlah yang valid'); return; }

  state.entries.push({
    id: Date.now(),
    date, cat, desc,
    amount,
    type: state.currentType
  });

  document.getElementById('inputDesc').value = '';
  document.getElementById('inputAmount').value = '';
  renderMonthly();
  showToast('✅ Transaksi berhasil ditambahkan');
}

// DELETE ENTRY
function deleteEntry(id) {
  state.entries = state.entries.filter(e => e.id !== id);
  renderMonthly();
  showToast('🗑️ Transaksi dihapus');
}

// ADD CATEGORY
function addCategory() {
  const val = document.getElementById('newCatInput').value.trim();
  if (!val) return;
  if (state.categories.includes(val)) { showToast('⚠️ Kategori sudah ada'); return; }
  state.categories.push(val);
  document.getElementById('newCatInput').value = '';
  renderCategories();
  showToast('✅ Kategori ditambahkan');
}

// DELETE CATEGORY
function deleteCategory(cat) {
  state.categories = state.categories.filter(c => c !== cat);
  renderCategories();
}

// FORMAT
function fmt(n) {
  return 'Rp ' + Math.abs(n).toLocaleString('id-ID');
}

// RENDER CATEGORIES
function renderCategories() {
  const list = document.getElementById('categoryList');
  list.innerHTML = state.categories.map(c =>
    `<div class="cat-pill">${c} <span class="cat-remove" onclick="deleteCategory('${c}')">×</span></div>`
  ).join('');

  const sel = document.getElementById('inputCategory');
  sel.innerHTML = state.categories.map(c => `<option value="${c}">${c}</option>`).join('');
}

// RENDER MONTHLY
function renderMonthly() {
  const month = parseInt(document.getElementById('monthSelect').value);
  const year = parseInt(document.getElementById('yearInput').value);

  const filtered = state.entries.filter(e => {
    const d = new Date(e.date);
    return d.getMonth() === month && d.getFullYear() === year;
  });

  filtered.sort((a,b) => new Date(b.date) - new Date(a.date));

  const income = filtered.filter(e=>e.type==='income').reduce((s,e)=>s+e.amount,0);
  const expense = filtered.filter(e=>e.type==='expense').reduce((s,e)=>s+e.amount,0);
  const balance = income - expense;

  document.getElementById('totalIncome').textContent = fmt(income);
  document.getElementById('totalExpense').textContent = fmt(expense);
  document.getElementById('totalBalance').textContent = fmt(balance);
  document.getElementById('totalBalance').style.color = balance >= 0 ? 'var(--accent-blue)' : 'var(--accent-red)';

  document.getElementById('entryCount').textContent = filtered.length + ' entri';

  const tbody = document.getElementById('entriesBody');
  if (filtered.length === 0) {
    tbody.innerHTML = `<tr><td colspan="6"><div class="empty-state"><span class="empty-icon">📭</span>Belum ada transaksi bulan ini</div></td></tr>`;
  } else {
    tbody.innerHTML = filtered.map(e => {
      const d = new Date(e.date);
      const dateStr = d.toLocaleDateString('id-ID',{day:'2-digit',month:'short'});
      return `<tr>
        <td style="white-space:nowrap">${dateStr}</td>
        <td><span style="font-size:0.82rem;background:var(--surface2);padding:3px 8px;border-radius:20px;">${e.cat||'-'}</span></td>
        <td style="color:var(--text-muted);font-size:0.85rem">${e.desc||'-'}</td>
        <td><span class="type-badge ${e.type}">${e.type==='income'?'↑ Pemasukan':'↓ Pengeluaran'}</span></td>
        <td class="amount-cell ${e.type}">${e.type==='income'?'+':'-'} ${fmt(e.amount)}</td>
        <td><button class="delete-btn" onclick="deleteEntry(${e.id})">🗑️</button></td>
      </tr>`;
    }).join('');
  }
}

// RENDER ANNUAL
function renderAnnual() {
  const year = parseInt(document.getElementById('annualYear').value);
  let annualInc = 0, annualExp = 0;

  const grid = document.getElementById('annualGrid');
  grid.innerHTML = MONTHS.map((name, mi) => {
    const filtered = state.entries.filter(e => {
      const d = new Date(e.date);
      return d.getMonth() === mi && d.getFullYear() === year;
    });
    const inc = filtered.filter(e=>e.type==='income').reduce((s,e)=>s+e.amount,0);
    const exp = filtered.filter(e=>e.type==='expense').reduce((s,e)=>s+e.amount,0);
    const bal = inc - exp;
    annualInc += inc; annualExp += exp;

    return `<div class="month-card" onclick="goToMonth(${mi},${year})">
      <div class="mc-name">${name}</div>
      <div class="mc-row"><span class="mc-label">Pemasukan</span><span class="mc-val inc">${fmt(inc)}</span></div>
      <div class="mc-row"><span class="mc-label">Pengeluaran</span><span class="mc-val exp">${fmt(exp)}</span></div>
      <div class="mc-balance">
        <span style="color:var(--text-muted);font-weight:500">Saldo</span>
        <span style="color:${bal>=0?'var(--accent-green)':'var(--accent-red)'}">${bal>=0?'+':'-'} ${fmt(bal)}</span>
      </div>
    </div>`;
  }).join('');

  document.getElementById('annualIncome').textContent = fmt(annualInc);
  document.getElementById('annualExpense').textContent = fmt(annualExp);
  const annualBal = annualInc - annualExp;
  document.getElementById('annualBalance').textContent = fmt(annualBal);
  document.getElementById('annualBalance').style.color = annualBal >= 0 ? 'var(--accent-blue)' : 'var(--accent-red)';
}

function goToMonth(month, year) {
  document.getElementById('monthSelect').value = month;
  document.getElementById('yearInput').value = year;
  switchTab('monthly');
  renderMonthly();
}

// TABS
function switchTab(tab) {
  document.getElementById('tab-monthly').classList.toggle('hidden', tab !== 'monthly');
  document.getElementById('tab-annual').classList.toggle('hidden', tab !== 'annual');
  document.querySelectorAll('.tab-btn').forEach((b, i) => {
    b.classList.toggle('active', (i===0 && tab==='monthly') || (i===1 && tab==='annual'));
  });
  if (tab === 'annual') renderAnnual();
}

// TOAST
function showToast(msg) {
  const t = document.getElementById('toast');
  document.getElementById('toastMsg').textContent = msg;
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 2800);
}

// RENDER ALL
function renderAll() {
  renderCategories();
  renderMonthly();
}

// INIT
loadData();
renderAll();
</script>
</body>
</html>
