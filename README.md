
<html lang="ru">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Live Russia Forum — Community</title>
  <meta name="description" content="Live Russia / Black Russia — форум сообщества. Мобильная-first верстка, темы, ответы, профиль." />
  <style>
    /* =============================
       MOBILE-FIRST RESPONSIVE FORUM
       ============================= */

    :root{
      --bg:#0b0f14;
      --card:#0f1720;
      --muted:#9aa4b2;
      --accent:#ff4b4b;
      --accent-2:#1da1f2;
      --text:#e6eef8;
      --glass: rgba(255,255,255,0.03);
      --radius:14px;
      --max-width:1100px;
    }

    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0;
      font-family:Inter, Roboto, "Segoe UI", Arial, sans-serif;
      background:linear-gradient(180deg,var(--bg), #071021);
      color:var(--text);
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
      display:flex;
      justify-content:center;
      padding:18px;
    }

    .app {
      width:100%;
      max-width:var(--max-width);
      display:flex;
      flex-direction:column;
      gap:18px;
    }

    /* Header */
    header.app-header{
      display:flex;
      align-items:center;
      gap:12px;
      justify-content:space-between;
      background: linear-gradient(90deg,#0f1724 0%, #071133 100%);
      padding:14px;
      border-radius:12px;
      box-shadow:0 6px 24px rgba(3,6,14,0.6);
      border:1px solid rgba(255,255,255,0.03);
    }
    .logo {
      display:flex;
      gap:12px;
      align-items:center;
    }
    .logo .mark{
      width:40px;height:40px;border-radius:10px;
      background: linear-gradient(135deg,var(--accent), #0047ab);
      display:flex;align-items:center;justify-content:center;font-weight:700;color:white;
      box-shadow:0 6px 18px rgba(0,0,0,0.5);
    }
    .logo h1{font-size:18px;margin:0}
    .search {
      flex:1;
      margin:0 12px;
      display:flex;
      gap:8px;
      align-items:center;
    }
    .search input{
      width:100%;
      padding:10px 12px;
      border-radius:10px;
      border:1px solid rgba(255,255,255,0.04);
      background:var(--glass);
      color:var(--text);
      outline:none;
    }
    .header-actions{
      display:flex;
      gap:8px;
      align-items:center;
    }
    .btn {
      background:transparent;
      border:1px solid rgba(255,255,255,0.06);
      padding:8px 12px;border-radius:10px;color:var(--text);
      cursor:pointer;font-weight:600;
    }
    .btn.primary{
      background:linear-gradient(90deg,var(--accent),#0047ab);
      border:none;color:white;
      box-shadow:0 8px 20px rgba(0,0,0,0.5);
    }

    /* layout: content and right column on desktop */
    .container {
      display:flex;
      flex-direction:column;
      gap:18px;
    }

    /* Threads list */
    .panel {
      background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
      border-radius:var(--radius);
      padding:14px;
      border:1px solid rgba(255,255,255,0.03);
      box-shadow:0 6px 18px rgba(0,0,0,0.6);
    }

    .toolbar {
      display:flex;
      gap:8px;
      align-items:center;
      flex-wrap:wrap;
      margin-bottom:10px;
    }
    .select, .input-inline{
      padding:8px 10px;border-radius:10px;border:1px solid rgba(255,255,255,0.04);
      background:var(--glass);
      color:var(--text);
    }

    .threads {
      display:flex;
      flex-direction:column;
      gap:10px;
    }
    .thread {
      display:flex;
      gap:12px;
      align-items:flex-start;
      padding:12px;
      border-radius:10px;
      background: rgba(255,255,255,0.015);
      border:1px solid rgba(255,255,255,0.02);
      transition:transform .15s, box-shadow .15s;
    }
    .thread:hover{ transform: translateY(-4px); box-shadow:0 8px 24px rgba(0,0,0,0.6); }
    .avatar {
      width:54px;height:54px;border-radius:10px;
      background:linear-gradient(135deg,#1da1f2,#6f7cff);
      display:flex;align-items:center;justify-content:center;font-weight:700;color:white;font-size:18px;
      flex-shrink:0;
    }
    .thread-main{flex:1}
    .thread-title{
      font-size:16px;margin:0 0 6px 0;
      color:var(--text);
      display:flex;justify-content:space-between;gap:8px;align-items:center;
    }
    .meta{font-size:13px;color:var(--muted)}
    .thread-excerpt{color:var(--muted);margin-top:8px;font-size:14px}

    /* Thread view (post list) */
    .post {
      background: linear-gradient(180deg, rgba(255,255,255,0.01), rgba(255,255,255,0.005));
      border:1px solid rgba(255,255,255,0.03);
      padding:14px;border-radius:12px;margin-bottom:12px;
    }
    .post .post-header{display:flex;gap:12px;align-items:center;margin-bottom:8px}
    .post .post-body{color:var(--text);line-height:1.5}
    .post .post-meta{color:var(--muted);font-size:13px;margin-top:8px}

    /* Composer */
    .composer{
      display:flex;flex-direction:column;gap:8px;padding:12px;border-radius:12px;background:var(--card);border:1px solid rgba(255,255,255,0.03);
    }
    .composer input, .composer textarea{
      width:100%;padding:10px;border-radius:10px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:var(--text);
    }
    .composer textarea{min-height:100px;resize:vertical}
    .composer .actions{display:flex;gap:8px;justify-content:flex-end}

    /* Right column (profile + info) - will be below on mobile */
    .rightcol { display:none; } /* hide by default on mobile */

    /* breadcrumb */
    .crumbs { display:flex;gap:8px;align-items:center;color:var(--muted);font-size:13px;margin-bottom:10px; }
    .crumbs a{color:var(--muted);text-decoration:none}
    .empty { color:var(--muted); padding:18px; text-align:center; }

    /* Modals */
    .modal-backdrop{
      position:fixed;inset:0;background:rgba(2,4,8,0.6);display:none;align-items:center;justify-content:center;z-index:50;padding:20px;
    }
    .modal{
      background: linear-gradient(180deg, #071427, #08142b);
      max-width:720px;width:100%;border-radius:12px;padding:16px;border:1px solid rgba(255,255,255,0.05);box-shadow:0 20px 60px rgba(0,0,0,0.7);
    }

    /* Responsive for tablets/desktops */
    @media(min-width:900px){
      .container{
        flex-direction:row;
        gap:20px;
      }
      .maincol{flex:1}
      .rightcol{
        width:320px;display:block;position:sticky;top:18px;height:fit-content;
      }
      .hero{border-radius:12px;overflow:hidden}
      header.app-header{border-radius:14px}
    }

    /* small niceties */
    .chip{display:inline-block;padding:6px 10px;border-radius:999px;background:rgba(255,255,255,0.03);color:var(--muted);font-size:13px}

    /* theme light toggle (used by JS) */
    .light {
      --bg:#f4f7fb;
      --card:#fff;
      --muted:#6b7280;
      --accent:#ff4b4b;
      --text:#0b1220;
      --glass: rgba(11,18,32,0.03);
    }

  </style>
</head>
<body>
  <div class="app" id="app">
    <!-- HEADER -->
    <header class="app-header">
      <div class="logo">
        <div class="mark">LR</div>
        <div>
          <h1>Live Russia</h1>
          <div style="font-size:12px;color:var(--muted)">Community • Forum</div>
        </div>
      </div>

      <div class="search" style="margin-left:12px;margin-right:12px;">
        <input id="searchInput" placeholder="Поиск по темам — введите и нажмите Enter" />
      </div>

      <div class="header-actions">
        <button class="btn" id="themeToggle" title="Сменить тему">Тема</button>
        <button class="btn primary" id="newThreadBtn">+ Новая тема</button>
      </div>
    </header>

    <!-- CONTAINER: main + right -->
    <div class="container">
      <!-- MAIN COLUMN -->
      <div class="maincol">
        <!-- HERO / breadcrumbs -->
        <div id="heroPanel" class="panel" style="margin-bottom:12px;">
          <div style="display:flex;align-items:center;justify-content:space-between;gap:12px;flex-wrap:wrap;">
            <div>
              <h2 style="margin:0 0 6px 0">Последние темы</h2>
              <div style="color:var(--muted);font-size:13px">Здесь публикуются объявления, обсуждения и ответы</div>
            </div>

            <div style="display:flex;gap:8px;align-items:center">
              <select id="filterSelect" class="select" title="Фильтр по категории">
                <option value="all">Все категории</option>
                <option value="announcements">Announcements</option>
                <option value="events">Events</option>
                <option value="general">General</option>
                <option value="support">Support</option>
              </select>

              <select id="sortSelect" class="select" title="Сортировка">
                <option value="latest">По дате (новые сверху)</option>
                <option value="popular">По популярности</option>
              </select>
            </div>
          </div>
        </div>

        <!-- Threads list panel -->
        <div id="threadsPanel" class="panel">
          <div class="toolbar">
            <div class="chip" id="threadsCount">Тем: 0</div>
            <div style="flex:1"></div>
            <button class="btn" id="refreshBtn">Обновить</button>
          </div>

          <div class="threads" id="threadsList">
            <!-- threads inserted by JS -->
          </div>

          <div id="emptyThreads" class="empty" style="display:none">Тем пока нет. Нажмите «+ Новая тема», чтобы создать первую.</div>
        </div>

        <!-- Thread view panel (hidden by default) -->
        <div id="threadView" class="panel" style="display:none">
          <div class="crumbs"><a href="#" onclick="goHome()">Главная</a> › <span id="threadTitleCrumb" style="font-weight:700"></span></div>
          <div id="postsArea">
            <!-- posts injected here -->
          </div>

          <div class="composer" id="replyComposer">
            <input id="replyName" placeholder="Ваш ник (например Carlo)" />
            <textarea id="replyText" placeholder="Написать ответ..."></textarea>
            <div class="actions">
              <button class="btn" onclick="goHome()">Отмена</button>
              <button class="btn primary" onclick="submitReply()">Отправить</button>
            </div>
          </div>
        </div>

      </div>

      <!-- RIGHT COLUMN (profile, quick links) -->
      <aside class="rightcol">
        <div class="panel">
          <h3 style="margin-top:0">Профиль</h3>
          <div style="display:flex;gap:12px;align-items:center">
            <div class="avatar" id="profileAvatar">C</div>
            <div>
              <div id="profileName" style="font-weight:700">Гость</div>
              <div style="color:var(--muted);font-size:13px" id="profileBio">Локальная сессия</div>
            </div>
          </div>

          <div style="margin-top:12px;display:flex;gap:8px">
            <button class="btn" id="editProfileBtn">Редактировать</button>
            <button class="btn" id="logoutBtn">Очистить</button>
          </div>
        </div>

        <div class="panel" style="margin-top:12px">
          <h3 style="margin:0 0 8px 0">Быстрые ссылки</h3>
          <a href="https://forum.liverussia.online/index.php" target="_blank" class="chip">Официальный форум</a>
          <a href="https://t.me/Carlo_Litvinenko" target="_blank" class="chip" style="margin-left:8px">Telegram</a>
          <div style="margin-top:12px;color:var(--muted);font-size:13px">Поддержка и правила доступны в разделе "Announcements"</div>
        </div>
      </aside>
    </div>

    <!-- FOOTER -->
    <footer style="text-align:center;color:var(--muted);font-size:13px">© 2025 Live Russia Community — Static demo</footer>
  </div>

  <!-- MODAL: New Thread -->
  <div class="modal-backdrop" id="modalBackdrop">
    <div class="modal" role="dialog" aria-modal="true" aria-labelledby="modalTitle">
      <h3 id="modalTitle">Создать новую тему</h3>
      <div style="margin-top:8px">
        <label style="font-size:13px;color:var(--muted)">Название темы</label>
        <input id="threadTitle" class="input" style="width:100%;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.04);margin-top:6px" placeholder="Например: Набор в администрацию" />
      </div>
      <div style="margin-top:10px">
        <label style="font-size:13px;color:var(--muted)">Категория</label>
        <select id="threadCategory" style="margin-top:6px;padding:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.04);width:100%">
          <option value="announcements">Announcements</option>
          <option value="events">Events</option>
          <option value="general">General</option>
          <option value="support">Support</option>
        </select>
      </div>
      <div style="margin-top:10px">
        <label style="font-size:13px;color:var(--muted)">Текст</label>
        <textarea id="threadContent" placeholder="Опишите тему..." style="width:100%;min-height:130px;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.04);margin-top:6px"></textarea>
      </div>

      <div style="display:flex;gap:8px;justify-content:flex-end;margin-top:12px">
        <button class="btn" onclick="closeModal()">Отмена</button>
        <button class="btn primary" id="createThreadSubmit">Создать тему</button>
      </div>
    </div>
  </div>

<script>
/* =============================
   Forum SPA logic (no backend)
   Data stored in localStorage as "lr_forum_data"
   Structure:
   {
     threads: [{id, title, category, author, excerpt, createdAt, replies: [{id, author, content, createdAt}], views, likes}],
     profile: {name, bio, avatarLetter}
   }
   ============================= */

const STORAGE_KEY = 'lr_forum_data_v1';

// ---------- Utilities ----------
function uid(prefix='id'){
  return prefix + '_' + Math.random().toString(36).slice(2,9);
}
function now(){ return new Date().toISOString(); }
function formatDate(iso){
  const d = new Date(iso);
  return d.toLocaleString();
}
function safeHTML(str){
  // minimal escape
  if(!str) return '';
  return String(str).replace(/[&<>"]/g, s => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;'}[s]));
}

// ---------- Initial demo data ----------
const DEMO_DATA = {
  threads: [
    {
      id: uid('t'),
      title: "Открытие нового сервера — подробности",
      category: "announcements",
      author: "Admin",
      excerpt: "Запуск нового русскоязычного сервера состоится 10 ноября. Планируется конкурс...",
      createdAt: new Date(Date.now()-1000*60*60*24*3).toISOString(),
      views: 120,
      likes: 20,
      replies: [
        { id: uid('r'), author: 'Mod_Alex', content: 'Отличная новость! Жду ивенты.', createdAt: new Date(Date.now()-1000*60*60*24*2).toISOString() },
        { id: uid('r'), author: 'Player1', content: 'Как попасть в бета-тест?', createdAt: new Date(Date.now()-1000*60*60*24*1).toISOString() }
      ]
    },
    {
      id: uid('t'),
      title: "Набор в саппорты — инструкция",
      category: "support",
      author: "HeadSupport",
      excerpt: "Ищем ответственных игроков для команды саппортов. Требования: дисциплина...",
      createdAt: new Date(Date.now()-1000*60*60*24*8).toISOString(),
      views: 78, likes: 9,
      replies: []
    },
    {
      id: uid('t'),
      title: "Конкурс скриншотов: победы и награды",
      category: "events",
      author: "EventMaster",
      excerpt: "Присылайте свои лучшие кадры — призы за 1/2/3 места.",
      createdAt: new Date(Date.now()-1000*60*60*24*15).toISOString(),
      views: 54, likes: 6,
      replies: []
    }
  ],
  profile: {
    name: "Гость",
    bio: "Локальная сессия",
    avatarLetter: "G"
  }
};

// ---------- Storage helpers ----------
function loadData(){
  try {
    const raw = localStorage.getItem(STORAGE_KEY);
    if(raw) return JSON.parse(raw);
  } catch(e){}
  // copy demo
  const copy = JSON.parse(JSON.stringify(DEMO_DATA));
  saveData(copy);
  return copy;
}
function saveData(data){
  localStorage.setItem(STORAGE_KEY, JSON.stringify(data));
}

// ---------- App State ----------
let store = loadData();
let filteredThreads = []; // used for rendering

// ---------- DOM refs ----------
const threadsListEl = document.getElementById('threadsList');
const threadsCountEl = document.getElementById('threadsCount');
const emptyThreadsEl = document.getElementById('emptyThreads');
const modalBackdrop = document.getElementById('modalBackdrop');
const newThreadBtn = document.getElementById('newThreadBtn');
const createThreadSubmit = document.getElementById('createThreadSubmit');
const searchInput = document.getElementById('searchInput');
const filterSelect = document.getElementById('filterSelect');
const sortSelect = document.getElementById('sortSelect');
const themeToggle = document.getElementById('themeToggle');
const profileNameEl = document.getElementById('profileName');
const profileBioEl = document.getElementById('profileBio');
const profileAvatarEl = document.getElementById('profileAvatar');
const editProfileBtn = document.getElementById('editProfileBtn');
const logoutBtn = document.getElementById('logoutBtn');
const refreshBtn = document.getElementById('refreshBtn');

const threadViewEl = document.getElementById('threadView');
const postsAreaEl = document.getElementById('postsArea');
const replyNameEl = document.getElementById('replyName');
const replyTextEl = document.getElementById('replyText');

// ---------- Render functions ----------
function renderThreads(){
  // filter & sort
  const q = (searchInput.value || '').trim().toLowerCase();
  const cat = filterSelect.value;
  const sort = sortSelect.value;

  let list = store.threads.slice();

  if(cat && cat !== 'all'){
    list = list.filter(t => t.category === cat);
  }
  if(q){
    list = list.filter(t => (t.title + ' ' + t.excerpt + ' ' + (t.author||'')).toLowerCase().includes(q));
  }
  if(sort === 'latest'){
    list.sort((a,b)=> new Date(b.createdAt) - new Date(a.createdAt));
  } else {
    list.sort((a,b)=> (b.replies?.length || 0) - (a.replies?.length || 0));
  }

  filteredThreads = list;
  threadsListEl.innerHTML = '';
  threadsCountEl.textContent = 'Тем: ' + list.length;

  if(list.length === 0){
    emptyThreadsEl.style.display = 'block';
  } else {
    emptyThreadsEl.style.display = 'none';
  }

  list.forEach(thread => {
    const threadEl = document.createElement('div');
    threadEl.className = 'thread';
    threadEl.innerHTML = `
      <div class="avatar">${safeHTML((thread.author||'U').slice(0,1).toUpperCase())}</div>
      <div class="thread-main">
        <div class="thread-title">
          <span style="display:flex;flex-direction:column">
            <a href="#" data-id="${thread.id}" class="link-thread" style="color:inherit;text-decoration:none">${safeHTML(thread.title)}</a>
            <div style="font-size:12px;color:var(--muted)">${safeHTML(thread.category)} • ${formatDate(thread.createdAt)}</div>
          </span>
          <div style="text-align:right">
            <div class="meta">♥ ${thread.likes || 0} • 💬 ${thread.replies?.length || 0}</div>
          </div>
        </div>
        <div class="thread-excerpt">${safeHTML(thread.excerpt || thread.replies?.[0]?.content || '')}</div>
      </div>
    `;
    // click thread
    threadEl.querySelector('.link-thread').addEventListener('click', (e)=>{
      e.preventDefault();
      openThread(thread.id);
    });
    threadsListEl.appendChild(threadEl);
  });
}

function renderProfile(){
  const p = store.profile || {name:'Гость',bio:'Локальная сессия',avatarLetter:'G'};
  profileNameEl.textContent = p.name || 'Гость';
  profileBioEl.textContent = p.bio || '';
  profileAvatarEl.textContent = (p.avatarLetter || (p.name || 'Г').slice(0,1)).toUpperCase();
}

// ---------- Thread view ----------
let currentThreadId = null;
function openThread(threadId){
  const thread = store.threads.find(t => t.id === threadId);
  if(!thread) return alert('Тема не найдена');
  // increment views locally
  thread.views = (thread.views || 0) + 1;
  saveData(store);
  // hide main, show view
  document.getElementById('threadsPanel').style.display = 'none';
  document.getElementById('threadView').style.display = 'block';
  document.getElementById('threadTitleCrumb').textContent = thread.title;
  // render posts
  postsAreaEl.innerHTML = '';
  // original post as first post
  const orig = document.createElement('div');
  orig.className = 'post';
  orig.innerHTML = `
    <div class="post-header">
      <div class="avatar">${safeHTML((thread.author||'A').slice(0,1))}</div>
      <div>
        <div style="font-weight:700">${safeHTML(thread.author||'Автор')}</div>
        <div class="post-meta">${formatDate(thread.createdAt)} • ${thread.category}</div>
      </div>
    </div>
    <div class="post-body">${safeHTML(thread.excerpt)}</div>
  `;
  postsAreaEl.appendChild(orig);

  // replies
  (thread.replies || []).forEach(reply=>{
    const rEl = document.createElement('div');
    rEl.className='post';
    rEl.innerHTML = `
      <div class="post-header">
        <div class="avatar">${safeHTML((reply.author||'U').slice(0,1))}</div>
        <div>
          <div style="font-weight:700">${safeHTML(reply.author)}</div>
          <div class="post-meta">${formatDate(reply.createdAt)}</div>
        </div>
      </div>
      <div class="post-body">${safeHTML(reply.content)}</div>
    `;
    postsAreaEl.appendChild(rEl);
  });

  currentThreadId = threadId;
  // prefill reply name with profile name
  replyNameEl.value = store.profile?.name || '';
  replyTextEl.value = '';
  window.scrollTo({top:0,behavior:'smooth'});
}

// go back
function goHome(){
  document.getElementById('threadsPanel').style.display = 'block';
  document.getElementById('threadView').style.display = 'none';
  currentThreadId = null;
  renderThreads();
}

// submit reply
function submitReply(){
  const author = (replyNameEl.value || store.profile?.name || 'Гость').trim();
  const content = (replyTextEl.value || '').trim();
  if(!content) return alert('Введите текст ответа');
  const thread = store.threads.find(t => t.id === currentThreadId);
  if(!thread) return alert('Тема не найдена');

  const reply = { id: uid('r'), author, content, createdAt: now() };
  thread.replies = thread.replies || [];
  thread.replies.push(reply);
  saveData(store);
  openThread(currentThreadId);
  alert('Ответ опубликован (локально)');
}

// ---------- Modal logic ----------
function openModal(){
  modalBackdrop.style.display = 'flex';
}
function closeModal(){
  modalBackdrop.style.display = 'none';
  document.getElementById('threadTitle').value='';
  document.getElementById('threadContent').value='';
}
newThreadBtn.addEventListener('click', () => {
  openModal();
});

// create thread
createThreadSubmit.addEventListener('click', ()=>{
  const title = document.getElementById('threadTitle').value.trim();
  const content = document.getElementById('threadContent').value.trim();
  const category = document.getElementById('threadCategory').value;
  // author from profile
  const author = store.profile?.name || 'Гость';
  if(!title || !content) { alert('Заполните заголовок и текст темы'); return; }

  const thread = {
    id: uid('t'),
    title,
    category,
    author,
    excerpt: content,
    createdAt: now(),
    views:0,
    likes:0,
    replies:[]
  };
  store.threads.unshift(thread); // newest on top
  saveData(store);
  closeModal();
  renderThreads();
  // auto-open created thread
  openThread(thread.id);
});

// search & filter events
searchInput.addEventListener('keydown', (e)=>{ if(e.key === 'Enter'){ renderThreads(); }});
filterSelect.addEventListener('change', renderThreads);
sortSelect.addEventListener('change', renderThreads);
refreshBtn.addEventListener('click', renderThreads);

// theme toggle
themeToggle.addEventListener('click', ()=>{
  document.documentElement.classList.toggle('light');
});

// profile editing (simple prompt)
editProfileBtn.addEventListener('click', ()=>{
  const name = prompt('Введите ваше имя (ник):', store.profile.name || '');
  if(name === null) return;
  const bio = prompt('Короткая информация о себе:', store.profile.bio || '');
  if(bio === null) return;
  store.profile.name = name || 'Гость';
  store.profile.bio = bio || '';
  store.profile.avatarLetter = (store.profile.name || 'G').slice(0,1).toUpperCase();
  saveData(store);
  renderProfile();
});

// logout / reset local session
logoutBtn.addEventListener('click', ()=>{
  if(!confirm('Очистить локальные данные и восстановить демо?')) return;
  localStorage.removeItem(STORAGE_KEY);
  store = loadData();
  renderProfile();
  renderThreads();
});

// modal backdrop click to close
modalBackdrop.addEventListener('click', (e)=>{
  if(e.target === modalBackdrop) closeModal();
});

// refresh demo data (for testing)
refreshBtn.addEventListener('dblclick', ()=>{
  if(!confirm('Сбросить данные на демо?')) return;
  localStorage.removeItem(STORAGE_KEY);
  store = loadData();
  renderProfile();
  renderThreads();
  alert('Данные сброшены на демо-версию');
});

// ---------- Initialization ----------
function init(){
  renderProfile();
  renderThreads();
  // small UX: when click thread list link, just open
}
init();

</script>
</body>
</html>