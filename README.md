<html lang="id">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Catatan Pengeluaran · Keluarga Febrian</title>
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;500;600&display=swap" rel="stylesheet" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
:root {
  --bg: #f8fafc; --surface: #ffffff; --surface2: #f1f5f9; --border: #e2e8f0;
  --text: #0f172a; --muted: #64748b;
  --income: #059669; --expense: #dc2626; --blue: #2563eb; --yellow: #d97706;
  --purple: #7c3aed; --teal: #0891b2;
  --shadow: 0 1px 3px rgba(0,0,0,.06), 0 4px 12px rgba(0,0,0,.04);
  --shadow-md: 0 4px 20px rgba(0,0,0,.08);
  --r: 12px; --rs: 8px;
}
html { font-size: 15px; }
body { font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: var(--text); min-height: 100vh; -webkit-font-smoothing: antialiased; }

/* Loader */
#loader { position:fixed;inset:0;background:var(--bg);display:flex;flex-direction:column;align-items:center;justify-content:center;z-index:9999;transition:opacity .4s; }
#loader.hide { opacity:0;pointer-events:none; }
.spin { width:40px;height:40px;border:3px solid var(--border);border-top-color:var(--income);border-radius:50%;animation:spin .75s linear infinite; }
@keyframes spin{to{transform:rotate(360deg)}}
.loader-txt { margin-top:14px;font-size:13px;color:var(--muted);font-weight:600; }

/* Topbar */
.topbar { position:sticky;top:0;z-index:100;background:var(--surface);border-bottom:1px solid var(--border);height:58px;padding:0 24px;display:flex;align-items:center;justify-content:space-between; }
.brand { font-size:15px;font-weight:800;letter-spacing:-.3px; }
.brand em { color:var(--income);font-style:normal; }
.topbar-r { display:flex;align-items:center;gap:12px; }
.live-pill { display:flex;align-items:center;gap:5px;background:#ecfdf5;color:var(--income);font-size:11px;font-weight:700;font-family:'JetBrains Mono',monospace;padding:4px 10px;border-radius:20px;letter-spacing:.5px; }
.live-dot { width:6px;height:6px;border-radius:50%;background:var(--income);animation:blink 2s infinite; }
@keyframes blink{0%,100%{opacity:1}50%{opacity:.3}}
.utime { font-size:11px;color:var(--muted);font-family:'JetBrains Mono',monospace; }
.rbar { height:2px;background:var(--border); }
.rfill { height:100%;background:var(--income);animation:cdown 30s linear infinite; }
@keyframes cdown{from{width:100%}to{width:0%}}

/* Main */
main { max-width:1300px;margin:0 auto;padding:22px 20px 56px; }
#err { background:#fef2f2;border:1px solid #fecaca;border-radius:var(--rs);padding:12px 16px;margin-bottom:18px;font-size:13px;color:#b91c1c;display:none;font-weight:500; }

/* Filter Bar */
.fbar { background:var(--surface);border:1px solid var(--border);border-radius:var(--r);padding:14px 16px;margin-bottom:14px;box-shadow:var(--shadow); }
.frow { display:flex;align-items:center;flex-wrap:wrap;gap:16px; }
.fgroup { display:flex;align-items:center;gap:6px; }
.plbl { font-size:11px;color:var(--muted);font-weight:700;text-transform:uppercase;letter-spacing:.8px;white-space:nowrap; }
select.psel { font-family:'Plus Jakarta Sans',sans-serif;font-size:13px;font-weight:600;color:var(--text);background:var(--surface);border:1px solid var(--border);border-radius:var(--rs);padding:6px 10px;cursor:pointer;outline:none;transition:border-color .15s; }
select.psel:focus { border-color:var(--income); }
.reset-btn { margin-left:auto;font-family:'Plus Jakarta Sans',sans-serif;font-size:12px;font-weight:700;color:var(--muted);background:var(--surface2);border:1px solid var(--border);border-radius:var(--rs);padding:6px 12px;cursor:pointer;transition:all .15s; }
.reset-btn:hover { color:var(--expense);border-color:var(--expense); }
.fbadge { display:flex;flex-wrap:wrap;gap:6px;margin-top:10px;padding-top:10px;border-top:1px solid var(--border); }
.ftag { display:flex;align-items:center;gap:4px;background:#ecfdf5;color:var(--income);font-size:11px;font-weight:700;padding:3px 8px;border-radius:12px;border:1px solid #a7f3d0; }

/* Tabs */
.tabs { display:flex;gap:3px;background:var(--surface);border:1px solid var(--border);border-radius:var(--rs);padding:3px;width:fit-content; }
.tab { font-family:'Plus Jakarta Sans',sans-serif;padding:7px 15px;border-radius:6px;font-size:13px;font-weight:600;color:var(--muted);cursor:pointer;border:none;background:transparent;transition:all .18s; }
.tab:hover { color:var(--text); }
.tab.on { background:var(--income);color:#fff;box-shadow:0 2px 6px rgba(5,150,105,.28); }

/* KPI */
.krow { display:grid;grid-template-columns:repeat(4,1fr);gap:14px;margin-bottom:18px; }
.kpi { background:var(--surface);border:1px solid var(--border);border-radius:var(--r);padding:18px;box-shadow:var(--shadow);position:relative;overflow:hidden;transition:transform .18s,box-shadow .18s; }
.kpi:hover { transform:translateY(-2px);box-shadow:var(--shadow-md); }
.kpi::after { content:'';position:absolute;bottom:0;left:0;right:0;height:3px;background:var(--c,var(--income)); }
.kico { font-size:22px;margin-bottom:8px; }
.klbl { font-size:11px;font-weight:700;color:var(--muted);text-transform:uppercase;letter-spacing:1px;margin-bottom:5px; }
.kval { font-size:21px;font-weight:800;color:var(--c,var(--income));letter-spacing:-.5px;line-height:1;font-family:'JetBrains Mono',monospace; }
.ksub { font-size:11px;color:var(--muted);margin-top:5px;font-weight:500; }

/* Grid */
.g2 { display:grid;grid-template-columns:1fr 1fr;gap:16px;margin-bottom:16px; }
.g3 { display:grid;grid-template-columns:1fr 1fr 1fr;gap:16px;margin-bottom:16px; }
.full { grid-column:1/-1; }

/* Card */
.card { background:var(--surface);border:1px solid var(--border);border-radius:var(--r);padding:18px;box-shadow:var(--shadow); }
.chd { font-size:11px;font-weight:700;color:var(--muted);text-transform:uppercase;letter-spacing:1.2px;margin-bottom:14px; }
.chd span { color:var(--income);font-family:'JetBrains Mono',monospace; }

/* Bar */
.bi { margin-bottom:11px; }
.bt { display:flex;justify-content:space-between;align-items:baseline;margin-bottom:4px; }
.bn { font-size:13px;font-weight:600;text-transform:capitalize; }
.bv { font-size:11px;color:var(--muted);font-family:'JetBrains Mono',monospace; }
.btr { background:var(--surface2);border-radius:4px;height:7px;overflow:hidden; }
.bf { height:100%;border-radius:4px;transition:width 1s cubic-bezier(.4,0,.2,1); }

/* TX */
.txs { max-height:440px;overflow-y:auto; }
.txs::-webkit-scrollbar { width:3px; }
.txs::-webkit-scrollbar-thumb { background:var(--border);border-radius:2px; }
.txr { display:flex;justify-content:space-between;align-items:center;padding:9px 0;border-bottom:1px solid var(--surface2); }
.txr:last-child { border-bottom:none; }
.txl { display:flex;align-items:center;gap:10px; }
.txd { width:9px;height:9px;border-radius:50%;flex-shrink:0; }
.txn { font-size:13px;font-weight:600;text-transform:capitalize; }
.txm { font-size:10px;color:var(--muted);margin-top:1px;font-family:'JetBrains Mono',monospace; }
.txa { font-size:13px;font-weight:700;font-family:'JetBrains Mono',monospace; }
.txa.i{color:var(--income)} .txa.o{color:var(--expense)} .txa.p{color:var(--blue)}

/* User cards */
.ucards { display:grid;grid-template-columns:1fr 1fr;gap:10px; }
.ucard { background:var(--surface2);border-radius:var(--rs);padding:12px 14px;border-left:3px solid var(--c,var(--income)); }
.uname { font-size:12px;font-weight:700;margin-bottom:3px; }
.uamt { font-size:17px;font-weight:800;font-family:'JetBrains Mono',monospace; }
.upct { font-size:11px;color:var(--muted);margin-top:2px; }

/* Gauge */
.gc { display:flex;flex-direction:column;align-items:center;padding:6px 0 14px; }
.gnum { font-size:46px;font-weight:800;font-family:'JetBrains Mono',monospace;letter-spacing:-2px;line-height:1; }
.glbl { font-size:12px;font-weight:600;color:var(--muted);margin-top:5px; }
.gtr { width:100%;height:12px;background:var(--surface2);border-radius:6px;overflow:hidden;margin-top:12px; }
.gb { height:100%;border-radius:6px;transition:width 1.2s cubic-bezier(.4,0,.2,1); }
.gsc { display:flex;justify-content:space-between;margin-top:4px;font-size:10px;color:var(--muted); }

/* Insight */
.ins { background:var(--surface2);border-left:3px solid var(--c,var(--income));border-radius:var(--rs);padding:11px 13px;margin-bottom:9px;font-size:13px;line-height:1.55; }
.ins strong { color:var(--c,var(--income)); }
.ins:last-child { margin-bottom:0; }

/* Akun */
.agrid { display:grid;grid-template-columns:repeat(4,1fr);gap:10px; }
.acard { background:var(--surface2);border-radius:var(--rs);padding:14px;text-align:center; }
.aname { font-size:11px;font-weight:700;text-transform:uppercase;letter-spacing:1px;color:var(--muted);margin-bottom:5px; }
.aval { font-size:16px;font-weight:800;font-family:'JetBrains Mono',monospace; }
.aval.pos{color:var(--income)} .aval.neg{color:var(--expense)}

/* Responsive */
@media(max-width:900px){.krow{grid-template-columns:repeat(2,1fr)}.g2,.g3{grid-template-columns:1fr}.agrid{grid-template-columns:repeat(2,1fr)}}
@media(max-width:900px){.frow{gap:10px}}
@media(max-width:520px){main{padding:12px 12px 40px}.kval{font-size:18px}.frow{flex-direction:column;align-items:flex-start}.fgroup{flex-wrap:wrap}.reset-btn{margin-left:0}}

/* Fade */
.fade{opacity:0;transform:translateY(12px);animation:fu .45s ease forwards}
.fade:nth-child(1){animation-delay:.04s}.fade:nth-child(2){animation-delay:.08s}
.fade:nth-child(3){animation-delay:.12s}.fade:nth-child(4){animation-delay:.16s}
@keyframes fu{to{opacity:1;transform:translateY(0)}}
</style>
</head>
<body>

<div id="loader"><div class="spin"></div><div class="loader-txt">Memuat data dari Google Sheets…</div></div>

<header class="topbar">
  <div class="brand">💰 Catatan <em>Pengeluaran</em> Keluarga Febrian</div>
  <div class="topbar-r">
    <div class="live-pill"><div class="live-dot"></div>LIVE</div>
    <div class="utime" id="utime">–</div>
  </div>
</header>
<div class="rbar"><div class="rfill" id="rfill"></div></div>

<main>
  <div id="err"></div>

  <!-- Filter Bar -->
  <div class="fbar">
    <div class="frow">
      <div class="fgroup">
        <span class="plbl">Periode</span>
        <select class="psel" id="sBulan" onchange="onFilter()">
          <option value="ALL">Semua Bulan</option>
        </select>
        <select class="psel" id="sTahun" onchange="onFilter()">
          <option value="ALL">Semua Tahun</option>
        </select>
      </div>
      <div class="fgroup">
        <span class="plbl">Anggota</span>
        <select class="psel" id="sUser" onchange="onFilter()">
          <option value="ALL">Semua</option>
        </select>
      </div>
      <div class="fgroup">
        <span class="plbl">Akun</span>
        <select class="psel" id="sAkun" onchange="onFilter()">
          <option value="ALL">Semua Akun</option>
        </select>
      </div>
      <button class="reset-btn" onclick="resetFilter()">&#8635; Reset Filter</button>
    </div>
    <div id="filter-badge" class="fbadge" style="display:none"></div>
  </div>
  <div class="tabs" style="margin-bottom:18px">
    <button class="tab on" onclick="goTab('ov',this)">Ringkasan</button>
    <button class="tab"    onclick="goTab('sp',this)">Pengeluaran</button>
    <button class="tab"    onclick="goTab('ic',this)">Pemasukan</button>
    <button class="tab"    onclick="goTab('ak',this)">Akun</button>
    <button class="tab"    onclick="goTab('tx',this)">Transaksi</button>
  </div>

  <div class="krow" id="kpis"></div>

  <div id="p-ov"></div>
  <div id="p-sp" style="display:none"></div>
  <div id="p-ic" style="display:none"></div>
  <div id="p-ak" style="display:none"></div>
  <div id="p-tx" style="display:none"></div>
</main>

<script>
// ── CONFIG ──
const SHEET_ID   = "1Wyo_rwqYJmQrTblXuGv5AX-dELF4OaFExD5jBv7OFXg";
const SHEET_NAME = "DATA";
const REFRESH_MS = 30000;
const UMAP = { "6281393436663":"Ayah Febrian", "628995263315":"Bunda Linda" };
const BLN  = ["Januari","Februari","Maret","April","Mei","Juni","Juli","Agustus","September","Oktober","November","Desember"];
const CLR  = ["#059669","#dc2626","#2563eb","#d97706","#7c3aed","#db2777","#0891b2","#65a30d","#ea580c","#6366f1","#84cc16","#14b8a6"];

// ── STATE ──
let ALL=[], CHARTS={};
let FILTER={ bulan:"ALL", tahun:"ALL", user:"ALL", akun:"ALL" };

// ── FETCH ──
async function load(){
  // Coba 3 metode berbeda — CORS bisa berbeda di tiap browser/hosting
  const methods = [fetchGViz, fetchCSV, fetchGVizNoCache];
  let lastErr = "";
  for (const method of methods) {
    try {
      const rows = await method();
      if (rows && rows.length > 0) {
        ALL = rows;
        hideErr(); initSelectors(); updateBadge(); render();
        document.getElementById("utime").textContent="Update: "+new Date().toLocaleTimeString("id-ID");
        const rf=document.getElementById("rfill"); rf.style.animation="none"; rf.offsetHeight; rf.style.animation="";
        document.getElementById("loader").classList.add("hide");
        return;
      }
    } catch(e) { lastErr = e.message; }
  }
  document.getElementById("loader").classList.add("hide");
  showErr("Gagal memuat data. Langkah: 1) Buka Sheets → Share → 'Anyone with the link' → Viewer. 2) Tunggu 1 menit lalu refresh. Detail: "+lastErr);
}

// Helper: parse satu baris gviz row → object
function gvizRowToObj(row, cols){
  const o={};
  row.c.forEach((c,i)=>{
    if(!c){ o[cols[i]]=null; return; }
    const col = cols[i];
    if(col==="tanggal"){
      // Prioritaskan c.f (formatted) karena langsung bisa di-parse
      o[col] = c.f || c.v || null;
    } else if(col==="nomor"){
      // Nomor HP bisa datang sebagai number (scientific notation) — paksa ke string
      const raw = (c.v !== undefined && c.v !== null) ? c.v : null;
      o[col] = raw !== null ? String(raw).replace(/[^0-9]/g,"") : null;
    } else if(col==="jumlah"){
      // Pastikan jumlah selalu angka
      o[col] = (c.v !== undefined && c.v !== null) ? parseFloat(c.v)||0 : 0;
    } else if(col==="akun"){
      // Akun ke lowercase string
      const raw = (c.v !== undefined && c.v !== null) ? c.v : null;
      o[col] = raw !== null ? String(raw).toLowerCase().trim() : null;
    } else {
      o[col] = (c.v !== undefined && c.v !== null) ? c.v : null;
    }
  });
  return o;
}

// Metode 1: Google Visualization API (gviz) — paling stabil
async function fetchGViz(){
  const url=`https://docs.google.com/spreadsheets/d/${SHEET_ID}/gviz/tq?tqx=out:json&sheet=${encodeURIComponent(SHEET_NAME)}&headers=1`;
  const res = await fetch(url, {cache:"no-store"});
  const txt = await res.text();
  const js  = JSON.parse(txt.match(/google\.visualization\.Query\.setResponse\(([\s\S]*?)\);?\s*$/)[1]);
  if (!js.table || !js.table.rows || js.table.rows.length===0) throw new Error("Tabel kosong");
  const cols = js.table.cols.map(c=>c.label.toLowerCase());
  return js.table.rows
    .map(row=>gvizRowToObj(row,cols))
    .filter(r=>r.tanggal&&r.tipe&&r.jumlah);
}

// Metode 2: Export CSV — fallback
async function fetchCSV(){
  const url = `https://docs.google.com/spreadsheets/d/${SHEET_ID}/export?format=csv&gid=0&t=${Date.now()}`;
  const res = await fetch(url, {cache:"no-store"});
  const txt = await res.text();
  return parseCSV(txt);
}

// Metode 3: gviz tanpa cache
async function fetchGVizNoCache(){
  const url=`https://docs.google.com/spreadsheets/d/${SHEET_ID}/gviz/tq?tqx=out:json&sheet=${encodeURIComponent(SHEET_NAME)}&headers=1&t=${Date.now()}`;
  const res = await fetch(url);
  const txt = await res.text();
  const js  = JSON.parse(txt.match(/google\.visualization\.Query\.setResponse\(([\s\S]*?)\);?\s*$/)[1]);
  if (!js.table || !js.table.rows || js.table.rows.length===0) throw new Error("Tabel kosong");
  const cols = js.table.cols.map(c=>c.label.toLowerCase());
  return js.table.rows
    .map(row=>gvizRowToObj(row,cols))
    .filter(r=>r.tanggal&&r.tipe&&r.jumlah);
}

// CSV Parser
function parseCSV(txt){
  const lines = txt.trim().split("\n");
  if (lines.length < 2) throw new Error("CSV kosong");
  const headers = splitCSVLine(lines[0]).map(h=>h.toLowerCase().trim().replace(/^"|"$/g,""));
  const rows = [];
  for (let i=1; i<lines.length; i++){
    const vals = splitCSVLine(lines[i]).map(v=>v.replace(/^"|"$/g,"").trim());
    if (!vals[0]) continue;
    const o = {};
    headers.forEach((h,j)=>{ o[h]=vals[j]||null; });
    // Normalisasi tipe data
    if (o.jumlah) o.jumlah = parseFloat(o.jumlah.replace(/[^0-9.]/g,""))||0;
    if (o.nomor)  o.nomor  = String(o.nomor).replace(/[^0-9]/g,"");
    if (o.akun)   o.akun   = (o.akun||"").toLowerCase().trim();
    if (o.tanggal && o.jumlah && o.tipe) rows.push(o);
  }
  return rows;
}
function splitCSVLine(line){
  const res=[]; let cur="", inQ=false;
  for(const ch of line){
    if(ch==='"'){inQ=!inQ;}
    else if(ch===","&&!inQ){res.push(cur);cur="";}
    else cur+=ch;
  }
  res.push(cur); return res;
}

// ── HELPERS ──
function toDate(v){
  if(!v) return null;
  if(typeof v==="string"){
    // Format gviz raw: "Date(2026,1,14,16,6,19)" — bulan 0-based
    const gviz = v.match(/Date\((\d+),(\d+),(\d+)/);
    if(gviz) return new Date(+gviz[1], +gviz[2], +gviz[3]);
    // Format Apps Script: "2/14/2026 16:06:19" atau "14 Feb 2026 16:06"
    const d = new Date(v);
    if(!isNaN(d)) return d;
    // Format "dd MMM yyyy HH:mm" dari Apps Script (Utilities.formatDate)
    // Contoh: "15 Feb 2026 15:55"
    const m2 = v.match(/(\d+)\s+(\w+)\s+(\d{4})/);
    if(m2){
      const months={"jan":0,"feb":1,"mar":2,"apr":3,"mei":4,"may":4,"jun":5,"jul":6,"agu":7,"aug":7,"sep":8,"okt":9,"oct":9,"nov":10,"des":11,"dec":11};
      const mo = months[m2[2].toLowerCase().slice(0,3)];
      if(mo!==undefined) return new Date(+m2[3], mo, +m2[1]);
    }
    return null;
  }
  const d = new Date(v);
  return isNaN(d) ? null : d;
}
const rp   = n=>"Rp "+Math.abs(n||0).toLocaleString("id-ID");
const rpS  = n=>(n<0?"-":"")+"Rp "+Math.abs(n||0).toLocaleString("id-ID");
const fmtK = n=>Math.abs(n||0)>=1e6?(n/1e6).toFixed(1)+"Jt":Math.abs(n||0)>=1e3?(n/1e3).toFixed(0)+"K":(n||0);
const sum  = arr=>arr.reduce((s,r)=>s+(parseFloat(r.jumlah)||0),0);
const uname= no=>UMAP[String(no||'').replace(/[^0-9]/g,'')]||no;
function grp(arr,key){ return arr.reduce((a,r)=>{const k=r[key]||"Lainnya";a[k]=(a[k]||0)+(parseFloat(r.jumlah)||0);return a;},{}); }
function srt(obj){ return Object.entries(obj).sort((a,b)=>b[1]-a[1]); }
// byPeriod & saldoBefore replaced by applyFilter & saldoSebelum
function saldoAkun(){ const m={}; ALL.forEach(r=>{const ak=(r.akun||"unknown").toString().toLowerCase().trim();if(!m[ak])m[ak]=0;if(r.tipe==="IN")m[ak]+=parseFloat(r.jumlah)||0;if(r.tipe==="OUT")m[ak]-=parseFloat(r.jumlah)||0;if(r.tipe==="PINDAH"){const sp=ak.split("→");if(sp.length===2){if(!m[sp[0]])m[sp[0]]=0;if(!m[sp[1]])m[sp[1]]=0;m[sp[0]]-=parseFloat(r.jumlah)||0;m[sp[1]]+=parseFloat(r.jumlah)||0;delete m[ak];}}}); return m; }
function dc(id){ if(CHARTS[id]){CHARTS[id].destroy();delete CHARTS[id];} }

// ── INIT SELECTORS ──
function initSelectors(){
  const now = new Date();
  // Kumpulkan nilai unik
  const years  = [...new Set(ALL.map(r=>{const d=toDate(r.tanggal);return d&&!isNaN(d)?d.getFullYear():null}).filter(Boolean))].sort();
  const users  = [...new Set(ALL.map(r=>r.nomor?String(r.nomor).replace(/[^0-9]/g,''):null).filter(Boolean))];
  const akuns  = [...new Set(ALL.map(r=>(r.akun||"").toLowerCase().trim()).filter(v=>v&&!v.includes("→")))].sort();

  // Bulan
  const sb = document.getElementById("sBulan");
  sb.innerHTML = '<option value="ALL">Semua Bulan</option>';
  BLN.forEach((n,i)=>{const o=document.createElement("option");o.value=i;o.textContent=n;sb.appendChild(o);});
  if(FILTER.bulan!=="ALL") sb.value=FILTER.bulan; else sb.value="ALL";

  // Tahun
  const st = document.getElementById("sTahun");
  const prevY = FILTER.tahun;
  st.innerHTML = '<option value="ALL">Semua Tahun</option>';
  years.forEach(y=>{const o=document.createElement("option");o.value=y;o.textContent=y;st.appendChild(o);});
  if(prevY!=="ALL" && years.includes(+prevY)) st.value=prevY;
  else { st.value=years[years.length-1]||now.getFullYear(); FILTER.tahun=st.value; }

  // User
  const su = document.getElementById("sUser");
  su.innerHTML = '<option value="ALL">Semua Anggota</option>';
  users.forEach(no=>{const o=document.createElement("option");o.value=no;o.textContent=UMAP[no]||no;su.appendChild(o);});
  su.value = FILTER.user;

  // Akun
  const sa = document.getElementById("sAkun");
  sa.innerHTML = '<option value="ALL">Semua Akun</option>';
  akuns.forEach(ak=>{const o=document.createElement("option");o.value=ak;o.textContent=ak.toUpperCase();sa.appendChild(o);});
  sa.value = FILTER.akun;
}

function onFilter(){
  FILTER.bulan = document.getElementById("sBulan").value;
  FILTER.tahun = document.getElementById("sTahun").value;
  FILTER.user  = document.getElementById("sUser").value;
  FILTER.akun  = document.getElementById("sAkun").value;
  updateBadge();
  render();
}

function resetFilter(){
  const now = new Date();
  FILTER = { bulan: now.getMonth().toString(), tahun: now.getFullYear().toString(), user:"ALL", akun:"ALL" };
  initSelectors();
  updateBadge();
  render();
}

function updateBadge(){
  const badge = document.getElementById("filter-badge");
  const tags = [];
  if(FILTER.bulan!=="ALL") tags.push(`📅 ${BLN[+FILTER.bulan]}`);
  if(FILTER.tahun!=="ALL") tags.push(`📅 ${FILTER.tahun}`);
  if(FILTER.bulan==="ALL"&&FILTER.tahun==="ALL") tags.push("📅 Semua Periode");
  if(FILTER.user!=="ALL")  tags.push(`👤 ${UMAP[FILTER.user]||FILTER.user}`);
  if(FILTER.akun!=="ALL")  tags.push(`🏦 ${FILTER.akun.toUpperCase()}`);
  if(tags.length===0||( tags.length===1&&tags[0].startsWith("📅") )){
    badge.style.display="none"; return;
  }
  badge.innerHTML = tags.map(t=>`<span class="ftag">${t}</span>`).join("");
  badge.style.display="flex";
}

// Terapkan filter ke data
function applyFilter(){
  return ALL.filter(r=>{
    const d = toDate(r.tanggal);
    if(!d||isNaN(d)) return false;
    if(FILTER.tahun!=="ALL" && d.getFullYear()!==+FILTER.tahun) return false;
    if(FILTER.bulan!=="ALL" && d.getMonth()!==+FILTER.bulan) return false;
    if(FILTER.user!=="ALL"  && String(r.nomor||"").replace(/[^0-9]/g,"")!==FILTER.user) return false;
    if(FILTER.akun!=="ALL"  && (r.akun||"").toLowerCase().trim()!==FILTER.akun) return false;
    return true;
  });
}

// Saldo sebelum periode filter (untuk saldo awal)
function saldoSebelum(){
  if(FILTER.tahun==="ALL"&&FILTER.bulan==="ALL") return 0;
  const y = FILTER.tahun!=="ALL" ? +FILTER.tahun : null;
  const b = FILTER.bulan!=="ALL" ? +FILTER.bulan : null;
  let s=0;
  ALL.forEach(r=>{
    if(FILTER.user!=="ALL"&&String(r.nomor||"").replace(/[^0-9]/g,"")!==FILTER.user) return;
    if(FILTER.akun!=="ALL"&&(r.akun||"").toLowerCase().trim()!==FILTER.akun) return;
    const d=toDate(r.tanggal); if(!d||isNaN(d)) return;
    const beforeY = y!==null && d.getFullYear()<y;
    const sameYbeforeB = y!==null && b!==null && d.getFullYear()===y && d.getMonth()<b;
    if(beforeY||sameYbeforeB){
      if(r.tipe==="IN") s+=parseFloat(r.jumlah)||0;
      if(r.tipe==="OUT") s-=parseFloat(r.jumlah)||0;
    }
  });
  return s;
}

// ── MAIN RENDER ──
function render(){
  const bd  = applyFilter();
  const OUT = bd.filter(r=>r.tipe==="OUT");
  const IN  = bd.filter(r=>r.tipe==="IN");
  const tIn = sum(IN), tOut=sum(OUT);
  const sA  = saldoSebelum();
  const sZ  = sA+tIn-tOut;
  const rasio = tIn>0?parseFloat((tOut/tIn*100).toFixed(1)):0;
  // Label periode untuk judul chart
  const periodLabel = FILTER.bulan!=="ALL"
    ? BLN[+FILTER.bulan]+(FILTER.tahun!=="ALL"?" "+FILTER.tahun:"")
    : (FILTER.tahun!=="ALL" ? "Tahun "+FILTER.tahun : "Semua Periode");
  renderKPI(tIn,tOut,sA,sZ,rasio,IN.length,OUT.length);
  renderOV(OUT,IN,tIn,tOut,sA,sZ,rasio,bd,periodLabel);
  renderSP(OUT,tOut);
  renderIC(IN,tIn);
  renderAK(bd,periodLabel);
  renderTX(bd,OUT,periodLabel);
}

// ── KPI ──
function renderKPI(tIn,tOut,sA,sZ,rasio,cIn,cOut){
  const rc=rasio>=80?"#dc2626":rasio>=60?"#d97706":"#059669";
  document.getElementById("kpis").innerHTML=[
    {ico:"💰",lbl:"Pemasukan Bulan Ini", val:rp(tIn),  sub:cIn+" transaksi",          c:"#059669"},
    {ico:"💸",lbl:"Pengeluaran Bulan Ini",val:rp(tOut), sub:cOut+" transaksi",         c:"#dc2626"},
    {ico:"🧮",lbl:"Saldo Akhir Bulan",   val:rpS(sZ),  sub:sZ>=0?"✓ Surplus":"⚠ Defisit", c:sZ>=0?"#059669":"#dc2626"},
    {ico:"📈",lbl:"Rasio Pengeluaran",   val:rasio+"%",sub:rasio>=80?"🔴 Bahaya":rasio>=60?"🟡 Waspada":"🟢 Sehat", c:rc},
  ].map(k=>`<div class="kpi fade" style="--c:${k.c}"><div class="kico">${k.ico}</div><div class="klbl">${k.lbl}</div><div class="kval">${k.val}</div><div class="ksub">${k.sub}</div></div>`).join("");
}

// ── OVERVIEW ──
function renderOV(OUT,IN,tIn,tOut,sA,sZ,rasio,bd,periodLabel){
  const kd=srt(grp(OUT,"kelompok"));
  const rc=rasio>=80?"#dc2626":rasio>=60?"#d97706":"#059669";
  const rl=rasio>=80?"🔴 Bahaya — pengeluaran sangat tinggi":rasio>=60?"🟡 Waspada — mulai perhatikan pengeluaran":"🟢 Sehat — kondisi keuangan baik";
  const uOut=srt(grp(OUT,"nomor"));

  // Daily map
  const dm={};
  bd.forEach(r=>{const d=toDate(r.tanggal);if(!d)return;const k=d.toISOString().slice(0,10);if(!dm[k])dm[k]={in:0,out:0};if(r.tipe==="IN")dm[k].in+=parseFloat(r.jumlah)||0;if(r.tipe==="OUT")dm[k].out+=parseFloat(r.jumlah)||0;});
  const days=Object.keys(dm).sort();
  let run=sA;
  const balLine=days.map(d=>{run+=dm[d].in-dm[d].out;return run;});

  document.getElementById("p-ov").innerHTML=`
  <div class="g2"><div class="card full">
    <div class="chd">Grafik Saldo Harian — <span>${periodLabel}</span></div>
    <canvas id="c-bal" height="85"></canvas>
    <div style="display:flex;gap:16px;justify-content:center;margin-top:10px">
      ${[["Saldo","#059669"],["Pemasukan","#2563eb"],["Pengeluaran","#dc2626"]].map(([l,c])=>`<div style="display:flex;align-items:center;gap:5px;font-size:11px;color:var(--muted)"><div style="width:18px;height:2px;background:${c};border-radius:1px"></div>${l}</div>`).join("")}
    </div>
  </div></div>
  <div class="g3">
    <div class="card">
      <div class="chd">Komposisi Kelompok</div>
      <canvas id="c-pie" height="195"></canvas>
    </div>
    <div class="card">
      <div class="chd">Rasio Pengeluaran</div>
      <div class="gc">
        <div class="gnum" style="color:${rc}">${rasio}%</div>
        <div class="glbl">${rl}</div>
        <div class="gtr"><div class="gb" style="width:${Math.min(rasio,100)}%;background:${rc}"></div></div>
        <div class="gsc"><span>0%</span><span>Max ideal: 80%</span><span>100%</span></div>
      </div>
      <div style="margin-top:12px">
        ${[["Saldo Awal","#2563eb",rpS(sA)],["+ Pemasukan","#059669",rp(tIn)],["– Pengeluaran","#dc2626",rp(tOut)],["= Saldo Akhir",sZ>=0?"#059669":"#dc2626",rpS(sZ)]].map(([l,c,v])=>`
        <div style="display:flex;justify-content:space-between;padding:6px 0;border-bottom:1px solid var(--surface2)">
          <span style="font-size:12px;color:${c};font-weight:700">${l}</span>
          <span style="font-size:13px;font-weight:800;color:${c};font-family:'JetBrains Mono',monospace">${v}</span>
        </div>`).join("")}
      </div>
    </div>
    <div class="card">
      <div class="chd">Per Anggota Keluarga</div>
      <div class="ucards" style="margin-bottom:14px">
        ${uOut.slice(0,2).map(([no,v],i)=>`<div class="ucard" style="--c:${CLR[i+1]}"><div class="uname">${uname(no)}</div><div class="uamt" style="color:${CLR[i+1]}">${rp(v)}</div><div class="upct">${tOut>0?(v/tOut*100).toFixed(0):0}% dari total</div></div>`).join("")}
      </div>
      <div class="chd">Top Kelompok</div>
      ${kd.slice(0,5).map(([n,v],i)=>`<div class="bi"><div class="bt"><div class="bn">${n}</div><div class="bv">${fmtK(v)}</div></div><div class="btr"><div class="bf" style="width:${kd[0][1]>0?(v/kd[0][1]*100).toFixed(0):0}%;background:${CLR[i+1]}"></div></div></div>`).join("")}
    </div>
  </div>
  <div class="g2">
    <div class="card"><div class="chd">Insight Otomatis</div>${buildIns(OUT,IN,tIn,tOut,sZ,rasio,kd)}</div>
    <div class="card"><div class="chd">Kewajiban · Kebutuhan · Keinginan</div>${buildKKK(grp(OUT,"kelompok"),tOut)}</div>
  </div>`;

  dc("c-bal");
  if(days.length) CHARTS["c-bal"]=new Chart(document.getElementById("c-bal"),{type:"line",data:{labels:days.map(d=>d.slice(5)),datasets:[{label:"Saldo",data:balLine,borderColor:"#059669",backgroundColor:"rgba(5,150,105,.07)",fill:true,tension:.4,pointRadius:3,borderWidth:2},{label:"Pemasukan",data:days.map(d=>dm[d].in),borderColor:"#2563eb",fill:false,tension:.4,pointRadius:2,borderWidth:1.5,borderDash:[4,4]},{label:"Pengeluaran",data:days.map(d=>dm[d].out),borderColor:"#dc2626",fill:false,tension:.4,pointRadius:2,borderWidth:1.5,borderDash:[4,4]}]},options:{plugins:{legend:{display:false},tooltip:{callbacks:{label:c=>`${c.dataset.label}: ${rp(c.raw)}`}}},scales:{x:{grid:{color:"#f1f5f9"},ticks:{font:{size:11}}},y:{grid:{color:"#f1f5f9"},ticks:{callback:v=>fmtK(v),font:{size:11}}}}}});

  dc("c-pie");
  if(kd.length) CHARTS["c-pie"]=new Chart(document.getElementById("c-pie"),{type:"doughnut",data:{labels:kd.map(([k])=>k),datasets:[{data:kd.map(([,v])=>v),backgroundColor:CLR.slice(1),borderWidth:0}]},options:{cutout:"55%",plugins:{legend:{position:"right",labels:{font:{size:11},padding:10}},tooltip:{callbacks:{label:c=>` ${c.label}: ${rp(c.raw)}`}}}}});
}

function buildIns(OUT,IN,tIn,tOut,sZ,rasio,kd){
  const tips=[];
  if(kd[0]) tips.push({c:"#dc2626",t:`Pengeluaran terbesar: <strong>${kd[0][0]}</strong> = <strong>${rp(kd[0][1])}</strong>`});
  const makanCnt=OUT.filter(r=>r.kategori==="makan").length;
  if(makanCnt>15) tips.push({c:"#d97706",t:`Makan di luar <strong>${makanCnt}x</strong> bulan ini — coba masak di rumah lebih sering 🍳`});
  if(rasio>=80) tips.push({c:"#dc2626",t:`⚠ Rasio <strong>${rasio}%</strong> — pengeluaran terlalu tinggi, perlu dikurangi!`});
  else if(rasio>=60) tips.push({c:"#d97706",t:`Rasio <strong>${rasio}%</strong> — waspada, mendekati batas aman.`});
  else tips.push({c:"#059669",t:`Rasio <strong>${rasio}%</strong> — pengeluaran terkendali 👍`});
  const kewajiban=grp(OUT,"kelompok")["Kewajiban"]||0;
  if(tIn>0) tips.push({c:"#7c3aed",t:`Kewajiban (cicilan/hutang) menyerap <strong>${(kewajiban/tIn*100).toFixed(0)}%</strong> dari pemasukan`});
  if(sZ<0) tips.push({c:"#dc2626",t:`🔴 <strong>Saldo akhir minus!</strong> Pengeluaran melebihi pemasukan.`});
  return tips.map(t=>`<div class="ins" style="--c:${t.c}">${t.t}</div>`).join("");
}

function buildKKK(gm,tot){
  const kw=gm["Kewajiban"]||0;
  const kb=(gm["Rumah Tangga"]||0)+(gm["Keluarga"]||0)+(gm["Transportasi"]||0)+(gm["Kesehatan"]||0);
  const ki=Math.max(0,tot-kw-kb);
  return[{l:"Kewajiban",d:"Cicilan & Hutang",v:kw,c:"#dc2626"},{l:"Kebutuhan",d:"RT, Keluarga, Transport, Kesehatan",v:kb,c:"#2563eb"},{l:"Keinginan",d:"Pribadi & Lainnya",v:ki,c:"#d97706"}]
    .map(k=>`<div style="margin-bottom:16px"><div style="display:flex;justify-content:space-between;margin-bottom:5px"><div><div style="font-size:13px;font-weight:700;color:${k.c}">${k.l}</div><div style="font-size:11px;color:var(--muted)">${k.d}</div></div><div style="text-align:right"><div style="font-size:14px;font-weight:800;font-family:'JetBrains Mono',monospace;color:${k.c}">${fmtK(k.v)}</div><div style="font-size:11px;color:var(--muted)">${tot>0?(k.v/tot*100).toFixed(0):0}%</div></div></div><div class="btr" style="height:10px"><div class="bf" style="width:${tot>0?(k.v/tot*100).toFixed(0):0}%;background:${k.c};height:10px"></div></div></div>`).join("");
}

// ── SPENDING ──
function renderSP(OUT,tot){
  const cd=srt(grp(OUT,"kategori")).slice(0,12);
  const kd=srt(grp(OUT,"kelompok"));
  document.getElementById("p-sp").innerHTML=`
  <div class="g2" style="margin-bottom:16px"><div class="card full"><div class="chd">Top Kategori Pengeluaran</div><canvas id="c-cat" height="78"></canvas></div></div>
  <div class="g2">
    <div class="card"><div class="chd">Per Kelompok</div>${kd.map(([n,v],i)=>`<div class="bi"><div class="bt"><div class="bn">${n}</div><div class="bv">${rp(v)} · <span style="color:var(--muted)">${tot>0?(v/tot*100).toFixed(0):0}%</span></div></div><div class="btr"><div class="bf" style="width:${tot>0?(v/tot*100).toFixed(0):0}%;background:${CLR[i%CLR.length]}"></div></div></div>`).join("")}</div>
    <div class="card"><div class="chd">Per Kategori — Detail</div>${cd.map(([n,v],i)=>`<div class="bi"><div class="bt"><div class="bn" style="color:${CLR[i%CLR.length]}">${n}</div><div class="bv">${rp(v)}</div></div><div class="btr"><div class="bf" style="width:${cd[0][1]>0?(v/cd[0][1]*100).toFixed(0):0}%;background:${CLR[i%CLR.length]}"></div></div></div>`).join("")}</div>
  </div>`;
  dc("c-cat");
  if(cd.length) CHARTS["c-cat"]=new Chart(document.getElementById("c-cat"),{type:"bar",data:{labels:cd.map(([k])=>k),datasets:[{data:cd.map(([,v])=>v),backgroundColor:cd.map((_,i)=>CLR[i%CLR.length]),borderRadius:6,borderSkipped:false}]},options:{plugins:{legend:{display:false},tooltip:{callbacks:{label:c=>rp(c.raw)}}},scales:{x:{grid:{display:false},ticks:{font:{size:11}}},y:{grid:{color:"#f1f5f9"},ticks:{callback:v=>fmtK(v),font:{size:11}}}}}});
}

// ── INCOME ──
function renderIC(IN,tot){
  const cd=srt(grp(IN,"kategori"));
  document.getElementById("p-ic").innerHTML=`
  <div class="g2">
    <div class="card"><div class="chd">Sumber Pemasukan</div><canvas id="c-inc" height="220"></canvas></div>
    <div class="card"><div class="chd">Detail Kategori</div>
      ${cd.map(([n,v],i)=>`<div class="bi"><div class="bt"><div class="bn">${n}</div><div class="bv">${rp(v)} · <span style="color:var(--income)">${tot>0?(v/tot*100).toFixed(1):0}%</span></div></div><div class="btr"><div class="bf" style="width:${tot>0?(v/tot*100).toFixed(0):0}%;background:${CLR[i%CLR.length]}"></div></div></div>`).join("")}
      <div style="margin-top:12px;padding-top:12px;border-top:1px solid var(--border);display:flex;justify-content:space-between">
        <span style="font-weight:700;font-size:13px">Total</span>
        <span style="font-weight:800;font-size:15px;color:var(--income);font-family:'JetBrains Mono',monospace">${rp(tot)}</span>
      </div>
    </div>
  </div>`;
  dc("c-inc");
  if(cd.length) CHARTS["c-inc"]=new Chart(document.getElementById("c-inc"),{type:"doughnut",data:{labels:cd.map(([k])=>k),datasets:[{data:cd.map(([,v])=>v),backgroundColor:CLR,borderWidth:0}]},options:{cutout:"60%",plugins:{legend:{position:"bottom",labels:{font:{size:12},padding:12}},tooltip:{callbacks:{label:c=>` ${c.label}: ${rp(c.raw)} (${tot>0?(c.raw/tot*100).toFixed(1):0}%)`}}}}});
}

// ── AKUN ──
function renderAK(bd,periodLabel){
  const sa=saldoAkun();
  const aList=Object.entries(sa).filter(([k])=>!k.includes("→")).sort((a,b)=>b[1]-a[1]);
  const aktB={};
  bd.forEach(r=>{const ak=(r.akun||"unknown").toString().toLowerCase().trim();if(!aktB[ak])aktB[ak]={in:0,out:0,n:0};if(r.tipe==="IN"){aktB[ak].in+=parseFloat(r.jumlah)||0;aktB[ak].n++;}if(r.tipe==="OUT"){aktB[ak].out+=parseFloat(r.jumlah)||0;aktB[ak].n++;}});
  const pindah=bd.filter(r=>r.tipe==="PINDAH");
  document.getElementById("p-ak").innerHTML=`
  <div class="g2" style="margin-bottom:16px"><div class="card full">
    <div class="chd">Saldo Real per Akun (seluruh periode)</div>
    <div class="agrid">${aList.map(([ak,sal])=>`<div class="acard"><div class="aname">${ak}</div><div class="aval ${sal>=0?"pos":"neg"}">${rpS(sal)}</div><div style="font-size:10px;color:var(--muted);margin-top:3px">${sal>=0?"✓ Positif":"⚠ Minus"}</div></div>`).join("")}</div>
  </div></div>
  <div class="g2">
    <div class="card"><div class="chd">Aktivitas Akun — <span>${periodLabel}</span></div>
      ${Object.entries(aktB).filter(([k])=>!k.includes("→")).map(([ak,v])=>`<div class="txr"><div class="txl"><div class="txd" style="background:var(--blue)"></div><div><div class="txn">${ak.toUpperCase()}</div><div class="txm">${v.n} transaksi bulan ini</div></div></div><div style="text-align:right"><div style="font-size:11px;color:var(--income);font-family:'JetBrains Mono',monospace">+${rp(v.in)}</div><div style="font-size:11px;color:var(--expense);font-family:'JetBrains Mono',monospace">-${rp(v.out)}</div></div></div>`).join("")}
    </div>
    <div class="card"><div class="chd">Transfer Antar Akun — <span>${periodLabel}</span></div>
      <div class="txs">${pindah.length===0?`<div style="text-align:center;padding:32px;color:var(--muted);font-size:13px">Tidak ada transfer bulan ini</div>`:pindah.map(r=>`<div class="txr"><div class="txl"><div class="txd" style="background:var(--blue)"></div><div><div class="txn">🔁 ${r.kategori||r.akun}</div><div class="txm">${toDate(r.tanggal)?.toLocaleDateString("id-ID")||""} · ${r.keterangan||""}</div></div></div><div class="txa p">${rp(r.jumlah)}</div></div>`).join("")}</div>
    </div>
  </div>`;
}

// ── TRANSAKSI ──
function renderTX(bd,OUT,periodLabel){
  const all=[...bd].filter(r=>r.tipe!=="PINDAH").sort((a,b)=>{const da=toDate(a.tanggal),db=toDate(b.tanggal);return(db||0)-(da||0);});
  const top8=[...OUT].sort((a,b)=>(parseFloat(b.jumlah)||0)-(parseFloat(a.jumlah)||0)).slice(0,8);
  document.getElementById("p-tx").innerHTML=`
  <div class="g2">
    <div class="card"><div class="chd">8 Pengeluaran Terbesar — <span>${periodLabel||""}</span></div>
      ${top8.map((r,i)=>`<div class="txr"><div class="txl"><div style="width:24px;height:24px;border-radius:6px;background:var(--surface2);display:flex;align-items:center;justify-content:center;font-size:11px;font-weight:700;color:var(--muted);flex-shrink:0">#${i+1}</div><div><div class="txn">${r.keterangan||r.kategori}</div><div class="txm">${toDate(r.tanggal)?.toLocaleDateString("id-ID")||""} · ${r.kategori} · ${r.akun||""} · ${uname(r.nomor)}</div></div></div><div class="txa o">-${rp(r.jumlah)}</div></div>`).join("")}
    </div>
    <div class="card"><div class="chd">Semua Transaksi <span>(${all.length})</span></div>
      <div class="txs">${all.map(r=>`<div class="txr"><div class="txl"><div class="txd" style="background:${r.tipe==="IN"?"var(--income)":"var(--expense)"}"></div><div><div class="txn">${r.keterangan||r.kategori}</div><div class="txm">${toDate(r.tanggal)?.toLocaleDateString("id-ID")||""} · ${r.kategori} · ${uname(r.nomor)}</div></div></div><div class="txa ${r.tipe==="IN"?"i":"o"}">${r.tipe==="IN"?"+":"-"}${rp(r.jumlah)}</div></div>`).join("")}</div>
    </div>
  </div>`;
}

// ── TAB ──
function goTab(name,el){
  ["ov","sp","ic","ak","tx"].forEach(t=>document.getElementById("p-"+t).style.display=t===name?"":"none");
  document.querySelectorAll(".tab").forEach(b=>b.classList.remove("on"));
  if(el)el.classList.add("on");
}

// ── ERR ──
function showErr(m){const e=document.getElementById("err");e.textContent="⚠️ "+m;e.style.display="block";}
function hideErr(){document.getElementById("err").style.display="none";}

// ── INIT ──
load();
setInterval(load,REFRESH_MS);
</script>
</body>
</html>
