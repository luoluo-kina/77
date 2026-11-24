# 77
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>GitHub 图床工作台</title>
<style>
  :root{
    --bg:#f5f7fa; --panel:#ffffff; --border:#e4e9f0; --text:#1f2733; --muted:#6b7686;
    --primary:#2f6fed; --primary-soft:#e8f0fe; --danger:#e5484d; --danger-soft:#fdecec;
    --success:#2ea44f; --warn:#d97706; --radius:12px; --shadow:0 1px 4px rgba(20,40,80,.08);
  }
  *{box-sizing:border-box;margin:0;padding:0}
  body{font-family:-apple-system,BlinkMacSystemFont,"Segoe UI","PingFang SC","Microsoft YaHei",sans-serif;background:var(--bg);color:var(--text);font-size:14px}
  button{font-family:inherit;cursor:pointer}
  input,select{font-family:inherit}

  /* ===== 顶栏 ===== */
  .topbar{display:flex;align-items:center;gap:16px;padding:12px 20px;background:var(--panel);border-bottom:1px solid var(--border);flex-wrap:wrap}
  .brand{font-weight:700;font-size:16px}
  .tabs{display:flex;gap:4px;flex-wrap:wrap}
  .tabs button{border:0;background:transparent;padding:8px 14px;border-radius:8px;color:var(--muted);font-size:14px}
.tabs button.active{background:var(--primary-soft);color:var(--primary);font-weight:600}
  .auth-status{margin-left:auto;font-size:13px;color:var(--muted)}
  .auth-status.ok{color:var(--success)}

  /* ===== 布局 ===== */
  .layout{display:grid;grid-template-columns:300px 1fr 320px;gap:16px;padding:16px;min-height:calc(100vh - 60px)}
  .panel{background:var(--panel);border:1px solid var(--border);border-radius:var(--radius);box-shadow:var(--shadow);padding:16px;overflow:auto}
  .panel.hidden{display:none}
  .panel-center{min-width:0}
  @media (max-width:960px){
    .layout{grid-template-columns:1fr}
  }

  /* ===== 表单 ===== */
  .field{margin-bottom:14px}
  .field label{display:block;font-size:12px;color:var(--muted);margin-bottom:6px}
  .field input,.field select{width:100%;padding:9px 10px;border:1px solid var(--border);border-radius:8px;background:#fff;color:var(--text)}
  .field input:focus{outline:none;border-color:var(--primary)}
  .btn{border:1px solid var(--border);background:#fff;border-radius:8px;padding:8px 14px;font-size:13px;color:var(--text)}
.btn.primary{background:var(--primary);border-color:var(--primary);color:#fff}
  .btn.danger{background:var(--danger-soft);border-color:#f3b4b6;color:var(--danger)}
  .btn.small{padding:5px 10px;font-size:12px}
  .btn-row{display:flex;gap:8px;flex-wrap:wrap;margin-top:8px}
  .hint{font-size:12px;color:var(--muted);line-height:1.6;margin-top:6px}

  /* ===== 上传拖拽区 ===== */
  .dropzone{border:2px dashed var(--border);border-radius:var(--radius);padding:28px 16px;text-align:center;color:var(--muted);transition:.2s;cursor:pointer}
  .dropzone.dragover{border-color:var(--primary);background:var(--primary-soft)}
  .upload-list{margin-top:12px;display:flex;flex-direction:column;gap:8px}
  .upload-item{border:1px solid var(--border);border-radius:8px;padding:8px 10px;font-size:13px}
  .upload-item .meta{display:flex;justify-content:space-between;gap:8px}
  .upload-item .status{font-size:12px}
  .status.uploading{color:var(--warn)}
  .status.done{color:var(--success)}
  .status.fail{color:var(--danger)}
/* ===== 图库工具栏 ===== */
  .toolbar{display:flex;gap:10px;align-items:center;flex-wrap:wrap;margin-bottom:14px}
  .toolbar input[type=search]{flex:1;min-width:140px;padding:8px 10px;border:1px solid var(--border);border-radius:8px}
  .seg{display:flex;border:1px solid var(--border);border-radius:8px;overflow:hidden}
  .seg button{border:0;background:#fff;padding:7px 12px;font-size:12px;color:var(--muted)}
  .seg button.active{background:var(--primary-soft);color:var(--primary)}

  /* ===== 卡片 ===== */
  .grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(150px,1fr));gap:12px}
  .grid.list-view{grid-template-columns:1fr}
  .card{border:1px solid var(--border);border-radius:var(--radius);overflow:hidden;background:#fff;display:flex;flex-direction:column}
  .card .preview{height:120px;background:#f0f3f7;display:flex;align-items:center;justify-content:center;overflow:hidden}
  .card.list-view .preview{height:64px}
  .card .preview img{max-width:100%;max-height:100%;object-fit:contain}
.card .preview audio{width:90%;height:38px}
  .card .preview .font-icon{font-size:34px;color:var(--muted)}
  .card .body{padding:10px;display:flex;flex-direction:column;gap:6px}
  .card .name{font-size:13px;word-break:break-all;line-height:1.4}
  .card .sub{font-size:12px;color:var(--muted)}
  .card .actions{display:flex;gap:6px;flex-wrap:wrap}
  .card-top{display:flex;align-items:center;gap:6px}
  .empty{padding:40px 16px;text-align:center;color:var(--muted)}
  .empty .big{font-size:32px;margin-bottom:8px}

  /* ===== 详情 ===== */
  .detail-preview{background:#f0f3f7;border-radius:var(--radius);min-height:160px;display:flex;align-items:center;justify-content:center;margin-bottom:14px;overflow:hidden}
  .detail-preview img{max-width:100%;max-height:300px}
  .detail-preview audio{width:100%}
  .detail-row{display:flex;justify-content:space-between;gap:12px;padding:8px 0;border-bottom:1px solid var(--border);font-size:13px}
  .detail-row .k{color:var(--muted)}
  .detail-row .v{word-break:break-all;text-align:right}
.link-box{margin-top:12px;padding:10px;background:var(--bg);border-radius:8px;font-size:12px;word-break:break-all;color:var(--muted)}

  /* ===== 说明 ===== */
  .info-panel h2{margin-bottom:12px}
  .info-panel li{margin:8px 0 8px 18px;line-height:1.7;color:var(--text)}
  .info-panel code{background:var(--bg);padding:1px 5px;border-radius:4px;font-size:12px}

  /* ===== Toast / 弹窗 ===== */
  #toastWrap{position:fixed;top:20px;right:20px;display:flex;flex-direction:column;gap:8px;z-index:1000}
  .toast{padding:10px 14px;border-radius:8px;background:#1f2733;color:#fff;font-size:13px;box-shadow:var(--shadow);max-width:300px}
  .toast.success{background:var(--success)}
  .toast.error{background:var(--danger)}
  .toast.warn{background:var(--warn)}
  #modalMask{position:fixed;inset:0;background:rgba(20,30,50,.4);display:none;align-items:center;justify-content:center;z-index:900}
  #modalMask.show{display:flex}
  .modal{background:#fff;border-radius:var(--radius);padding:20px;width:min(360px,90vw)}
.modal h3{margin-bottom:10px;font-size:15px}
  .modal p{color:var(--muted);font-size:13px;line-height:1.6;margin-bottom:16px}
  .modal .btn-row{justify-content:flex-end}
</style>
</head>
<body>

<header class="topbar">
  <div class="brand">GitHub 图床工作台</div>
  <nav class="tabs" id="tabs">
    <button data-tab="info" class="active">说明</button>
    <button data-tab="config">配置 / 上传</button>
    <button data-tab="gallery">图库</button>
    <button data-tab="detail">详情</button>
  </nav>
  <div class="auth-status" id="authStatus">未授权</div>
</header>

<main class="layout">

  <!-- 左侧：配置 + 上传 -->
  <aside class="panel hidden" id="panel-config">
    <h3 style="margin-bottom:14px">仓库配置</h3>
    <div class="field">
      <label>Owner（用户名）</label>
      <input type="text" id="cfgOwner" autocomplete="off">
    </div>
    <div class="field">
      <label>仓库名</label>
      <input type="text" id="cfgRepo" autocomplete="off">
    </div>
    <div class="field">
      <label>分支</label>
      <input type="text" id="cfgBranch" autocomplete="off">
</div>
    <div class="field">
      <label>BASE_URL（站点根地址）</label>
      <input type="text" id="cfgBaseUrl" autocomplete="off">
    </div>
    <div class="btn-row">
      <button class="btn" id="btnExport">导出配置</button>
      <button class="btn" id="btnImport">导入配置</button>
      <input type="file" id="importFile" accept=".json" hidden>
    </div>

    <h3 style="margin:20px 0 12px">Token 授权</h3>
    <div class="field">
      <label>Personal Access Token</label>
      <input type="password" id="cfgToken" autocomplete="off" placeholder="github_pat_...">
    </div>
    <div class="btn-row">
      <button class="btn primary" id="btnVerify">验证并保存</button>
      <button class="btn danger" id="btnClearToken">清除 Token</button>
    </div>

    <h3 style="margin:20px 0 12px">新建子目录</h3>
    <div class="field">
      <label>目录名（可含子路径，如 assets/img）</label>
      <input type="text" id="cfgDir" autocomplete="off" placeholder="assets">
    </div>
    <div class="btn-row">
      <button class="btn" id="btnMkdir">创建目录</button>
</div>

    <h3 style="margin:20px 0 12px">上传文件</h3>
    <div class="dropzone" id="dropzone">
      点击选择文件，或拖拽文件到此处<br>
      <span class="hint">支持多选；图片 / 音频 / 字体</span>
    </div>
    <input type="file" id="fileInput" multiple hidden>
    <div class="upload-list" id="uploadList"></div>
  </aside>

  <!-- 中间：画廊 -->
  <section class="panel panel-center hidden" id="panel-gallery">
    <div class="toolbar">
      <input type="search" id="searchInput" placeholder="按文件名搜索…">
      <div class="seg" id="typeSeg">
        <button data-type="all" class="active">全部</button>
        <button data-type="image">图片</button>
        <button data-type="audio">音频</button>
        <button data-type="font">字体</button>
      </div>
      <div class="seg" id="viewSeg">
        <button data-view="grid" class="active">网格</button>
        <button data-view="list">列表</button>
      </div>
      <button class="btn small" id="btnRefresh">刷新</button>
      <button class="btn small" id="btnBatchCopy">批量复制外链</button>
<button class="btn small danger" id="btnBatchDelete">批量删除</button>
    </div>
    <div class="grid" id="assetGrid"></div>
  </section>

  <!-- 右侧：详情 -->
  <aside class="panel hidden" id="panel-detail">
    <h3 style="margin-bottom:14px">资产详情</h3>
    <div id="detailBody"></div>
  </aside>

  <!-- 说明（默认可见） -->
  <section class="panel info-panel" id="panel-info">
    <h2>GitHub 图床工作台 · 使用说明</h2>
    <ul>
      <li>在「配置 / 上传」页填写仓库 Owner、仓库名、分支与站点根地址，粘贴 GitHub Personal Access Token 后点「验证并保存」。</li>
      <li>Token 需要勾选目标仓库，并开启 <code>Contents</code> 读写权限；Token 仅保存在本机浏览器 localStorage，不会发送到 GitHub 以外的地址。</li>
      <li>「图库」页展示仓库内的图片、音频、字体资产，支持搜索、类型筛选、网格 / 列表切换、多选批量删除与批量复制外链。</li>
      <li>点击任意资产可在「详情」页查看预览、文件大小与对外链接。</li>
      <li>单文件建议不超过 5MB，GitHub 单文件上限为 100MB；超过 5MB 会给出提醒但仍可继续上传。</li>
      <li>配置可导出为 JSON，换电脑时通过「导入配置」一键恢复。</li>
    </ul>
  </section>

</main>

<div id="toastWrap"></div>
<div id="modalMask">
  <div class="modal">
    <h3 id="modalTitle">确认操作</h3>
    <p id="modalText"></p>
<div class="btn-row">
      <button class="btn" id="modalCancel">取消</button>
      <button class="btn danger" id="modalOk">确认</button>
    </div>
  </div>
</div>

<script>
/* ============================================================
 * 配置区（可按需修改）
 * ============================================================ */
const CONFIG = {
  GITHUB_OWNER: 'luoluo-kina',
  GITHUB_REPO: '77',
  GITHUB_BRANCH: 'main',
  BASE_URL: 'https://luoluo-kina.github.io/77/'
};

/* ============================================================
 * 常量与存储（localStorage 不可用时降级为内存存储）
 * ============================================================ */
const LS_TOKEN = 'gh_pat_token';
const LS_CONFIG = 'gh_config';
const IMG_EXT  = ['jpg','jpeg','png','gif','webp','svg','bmp','ico','avif'];
const AUDIO_EXT= ['mp3','wav','m4a','ogg','flac','aac'];
const FONT_EXT = ['ttf','otf','woff','woff2','eot'];

const memStore = {};
const store = {
  get(k){ try { return localStorage.getItem(k); } catch(e){ return (k in memStore ? memStore[k] : null); } },
set(k,v){ try { localStorage.setItem(k,v); } catch(e){ memStore[k]=v; } },
  del(k){ try { localStorage.removeItem(k); } catch(e){ delete memStore[k]; } }
};

/* 运行时状态 */
let state = {
  owner: CONFIG.GITHUB_OWNER,
  repo: CONFIG.GITHUB_REPO,
  branch: CONFIG.GITHUB_BRANCH,
  baseUrl: CONFIG.BASE_URL,
  token: store.get(LS_TOKEN) || '',
  assets: [],          // {path, sha, size, type, ext}
  filter: 'all',
  view: 'grid',
  keyword: '',
  selected: new Set(),
  current: null        // 当前详情资产
};

/* ============================================================
 * 工具函数
 * ============================================================ */
function extOf(path){
  const m = path.toLowerCase().split('.');
  return m.length > 1 ? m.pop() : '';
}
function classify(ext){
  if (IMG_EXT.includes(ext)) return 'image';
  if (AUDIO_EXT.includes(ext)) return 'audio';
  if (FONT_EXT.includes(ext)) return 'font';
  return 'other';
}
function fmtSize(bytes){
  if (bytes == null) return '-';
  if (bytes < 1024) return bytes + ' B';
if (bytes < 1024*1024) return (bytes/1024).toFixed(1) + ' KB';
  return (bytes/1024/1024).toFixed(2) + ' MB';
}
/* 路径逐段编码，兼容中文与空格 */
function encodePath(p){
  return String(p).split('/').filter(Boolean).map(encodeURIComponent).join('/');
}
function apiUrl(path){
  return 'https://api.github.com/repos/' + state.owner + '/' + state.repo + path;
}
function buildLink(path){
  const base = state.baseUrl.endsWith('/') ? state.baseUrl : state.baseUrl + '/';
  return base + path.replace(/^\//,'');
}
function arrayBufferToBase64(buf){
  const bytes = new Uint8Array(buf);
  let bin = '';
  const CH = 0x8000;
  for (let i=0;i<bytes.length;i+=CH){
    bin += String.fromCharCode.apply(null, bytes.subarray(i, i+CH));
  }
  return btoa(bin);
}

/* Toast */
function toast(msg, type){
  const wrap = document.getElementById('toastWrap');
  const el = document.createElement('div');
  el.className = 'toast' + (type ? ' ' + type : '');
  el.textContent = msg;
  wrap.appendChild(el);
  setTimeout(()=>{ el.remove(); }, 3200);
}

/* 二次确认弹窗（Promise） */
function confirmModal(title, text){
  return new Promise(resolve=>{
    document.getElementById('modalTitle').textContent = title;
    document.getElementById('modalText').textContent = text;
    document.getElementById('modalMask').classList.add('show');
    const ok = document.getElementById('modalOk');
    const cancel = document.getElementById('modalCancel');
    const done = (val)=>{
      document.getElementById('modalMask').classList.remove('show');
      ok.onclick = cancel.onclick = null;
      resolve(val);
    };
    ok.onclick = ()=>done(true);
    cancel.onclick = ()=>done(false);
  });
}

/* GitHub 请求封装，统一错误提示 */
async function ghFetch(url, options){
  options = options || {};
  const headers = Object.assign({
    'Accept': 'application/vnd.github.v3+json'
  }, options.headers || {});
  if (state.token){ headers['Authorization'] = 'token ' + state.token; }
  let resp;
  try {
    resp = await fetch(url, Object.assign({}, options, { headers }));
  } catch(e){
throw { code: 'network', message: '网络异常：无法连接 GitHub，请检查网络后重试' };
  }
  let data = null;
  const ct = resp.headers.get('content-type') || '';
  if (ct.includes('json')){ try { data = await resp.json(); } catch(e){ data = null; } }
  if (!resp.ok){
    let message;
    switch(resp.status){
      case 401: message = '401：Token 无效或已过期，请重新生成'; break;
      case 403: message = '403：无仓库权限或触发 GitHub 限流'; break;
      case 404: message = '404：仓库或路径不存在，请核对 Owner / 仓库名 / 分支'; break;
      case 409: message = '409：冲突，文件状态与远端不一致，请刷新后重试'; break;
      default:  message = (data && data.message) ? ('请求失败：' + data.message) : ('请求失败，HTTP ' + resp.status);
    }
    throw { code: resp.status, message, data };
  }
  return data;
}

/* ============================================================
 * Tab 切换
 * ============================================================ */
function switchTab(tab){
  document.querySelectorAll('#tabs button').forEach(b=>{
    b.classList.toggle('active', b.dataset.tab === tab);
});
  ['config','gallery','detail','info'].forEach(t=>{
    document.getElementById('panel-'+t).classList.toggle('hidden', t !== tab);
  });
}
document.getElementById('tabs').addEventListener('click', e=>{
  const btn = e.target.closest('button[data-tab]');
  if (btn) switchTab(btn.dataset.tab);
});

/* ============================================================
 * 配置读写 / 导入导出
 * ============================================================ */
function applyConfigToForm(){
  document.getElementById('cfgOwner').value = state.owner;
  document.getElementById('cfgRepo').value = state.repo;
  document.getElementById('cfgBranch').value = state.branch;
  document.getElementById('cfgBaseUrl').value = state.baseUrl;
  document.getElementById('cfgToken').value = state.token;
}
function saveConfigFromForm(){
  state.owner = document.getElementById('cfgOwner').value.trim() || CONFIG.GITHUB_OWNER;
  state.repo  = document.getElementById('cfgRepo').value.trim()  || CONFIG.GITHUB_REPO;
  state.branch= document.getElementById('cfgBranch').value.trim()|| CONFIG.GITHUB_BRANCH;
state.baseUrl = document.getElementById('cfgBaseUrl').value.trim() || CONFIG.BASE_URL;
  store.set(LS_CONFIG, JSON.stringify({
    owner: state.owner, repo: state.repo, branch: state.branch, baseUrl: state.baseUrl
  }));
}
function loadSavedConfig(){
  const raw = store.get(LS_CONFIG);
  if (raw){
    try {
      const c = JSON.parse(raw);
      if (c.owner) state.owner = c.owner;
      if (c.repo) state.repo = c.repo;
      if (c.branch) state.branch = c.branch;
      if (c.baseUrl) state.baseUrl = c.baseUrl;
    } catch(e){ /* 忽略损坏的配置 */ }
  }
}

document.getElementById('btnExport').addEventListener('click', ()=>{
  saveConfigFromForm();
  const obj = { owner: state.owner, repo: state.repo, branch: state.branch, baseUrl: state.baseUrl };
  const blob = new Blob([JSON.stringify(obj, null, 2)], { type: 'application/json' });
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'github-host-config.json';
  a.click();
  URL.revokeObjectURL(a.href);
toast('配置已导出', 'success');
});
document.getElementById('btnImport').addEventListener('click', ()=>{
  document.getElementById('importFile').click();
});
document.getElementById('importFile').addEventListener('change', async e=>{
  const file = e.target.files[0];
  if (!file) return;
  try {
    const text = await file.text();
    const c = JSON.parse(text);
    state.owner = c.owner || state.owner;
    state.repo = c.repo || state.repo;
    state.branch = c.branch || state.branch;
    state.baseUrl = c.baseUrl || state.baseUrl;
    store.set(LS_CONFIG, JSON.stringify({owner:state.owner,repo:state.repo,branch:state.branch,baseUrl:state.baseUrl}));
    applyConfigToForm();
    toast('配置导入成功', 'success');
  } catch(err){
    toast('导入失败：配置文件格式不正确', 'error');
  }
  e.target.value = '';
});

/* ============================================================
 * Token 验证 / 清除
 * ============================================================ */
function renderAuthStatus(){
  const el = document.getElementById('authStatus');
if (state.token){ el.textContent = '已授权'; el.classList.add('ok'); }
  else { el.textContent = '未授权'; el.classList.remove('ok'); }
}

document.getElementById('btnVerify').addEventListener('click', async ()=>{
  saveConfigFromForm();
  state.token = document.getElementById('cfgToken').value.trim();
  if (!state.token){ toast('请先填写 Token', 'warn'); return; }
  try {
    await ghFetch(apiUrl(''));
    store.set(LS_TOKEN, state.token);
    renderAuthStatus();
    toast('验证通过，已授权', 'success');
    loadAssets();
  } catch(err){
    toast(err.message, 'error');
  }
});
document.getElementById('btnClearToken').addEventListener('click', ()=>{
  state.token = '';
  store.del(LS_TOKEN);
  document.getElementById('cfgToken').value = '';
  renderAuthStatus();
  toast('Token 已清除', 'success');
});

/* ============================================================
 * 资产列表
 * ============================================================ */
async function loadAssets(){
  if (!state.token){ toast('请先完成 Token 授权', 'warn'); switchTab('config'); return; }
try {
    const data = await ghFetch(apiUrl('/git/trees/' + encodeURIComponent(state.branch) + '?recursive=1'));
    const assets = [];
    (data.tree || []).forEach(item=>{
      if (item.type !== 'blob') return;
      const ext = extOf(item.path);
      const type = classify(ext);
      if (type === 'other') return;
      assets.push({ path: item.path, sha: item.sha, size: item.size, type, ext });
    });
    state.assets = assets;
    state.sel
