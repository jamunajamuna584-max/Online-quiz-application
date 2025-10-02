
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Mini Blogger — Demo</title>
  <style>
    :root{
      --bg:#f5f7fb;
      --card:#ffffff;
      --muted:#6b7280;
      --accent:#2563eb;
      --radius:12px;
      --shadow: 0 6px 18px rgba(20,20,30,0.06);
      font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      background:linear-gradient(180deg,#f7fbff 0%,var(--bg) 100%);
      color:#0f172a;
      padding:20px;
    }
    header{
      display:flex;
      gap:16px;
      align-items:center;
      justify-content:space-between;
      max-width:1100px;
      margin:0 auto 20px;
    }
    .brand{
      display:flex;align-items:center;gap:12px;
    }
    .logo{
      width:56px;height:56px;border-radius:10px;background:linear-gradient(135deg,#60a5fa,#7c3aed);
      display:flex;align-items:center;justify-content:center;color:white;font-weight:700;font-size:18px;
      box-shadow:var(--shadow);
    }
    .title{font-size:20px;font-weight:700}
    .subtitle{font-size:12px;color:var(--muted);margin-top:2px}
    .controls{display:flex;gap:8px;align-items:center}

    .btn{
      border:none;padding:8px 12px;border-radius:10px;background:var(--card);cursor:pointer;
      box-shadow: var(--shadow);display:inline-flex;align-items:center;gap:8px;
    }
    .btn.primary{background:var(--accent);color:white}
    .container{
      max-width:1100px;margin:0 auto;display:grid;grid-template-columns:1fr 360px;gap:20px;
    }

    /* Editor card */
    .card{
      background:var(--card);padding:18px;border-radius:var(--radius);box-shadow:var(--shadow);
    }
    .form-row{display:flex;gap:8px;margin-bottom:10px}
    input[type="text"], select, textarea{
      width:100%;padding:10px;border-radius:8px;border:1px solid #e6e9ef;background:#fbfdff;
      font-size:14px;resize:vertical;
    }
    label.small{font-size:12px;color:var(--muted);margin-bottom:6px;display:block}

    .editor-toolbar{display:flex;gap:8px;margin-bottom:8px;flex-wrap:wrap}
    .posts-list{display:flex;flex-direction:column;gap:12px}
    .post-item{padding:12px;border-radius:10px;border:1px solid #eef2ff;background:linear-gradient(180deg,#fff,#fbfdff)}
    .post-item h3{margin:0 0 6px 0}
    .meta{font-size:12px;color:var(--muted)}
    .post-actions{display:flex;gap:8px;margin-top:8px}
    .preview{padding:12px;border-radius:10px;background:#fbfdff;border:1px dashed #e6eefb}

    /* right column */
    .sidebar-section{margin-bottom:16px}
    .tag{display:inline-block;padding:6px 8px;border-radius:999px;background:#eef2ff;margin:6px 6px 0 0;font-size:12px;color:#1e3a8a}
    .small-muted{font-size:13px;color:var(--muted)}

    /* post page */
    .post-content img{max-width:100%;height:auto;border-radius:8px;margin:8px 0}
    .comments {margin-top:12px}
    .comment {padding:10px;border-radius:8px;background:#fbfbff;border:1px solid #eef2ff;margin-bottom:8px}

    /* mobile */
    @media (max-width:980px){
      .container{grid-template-columns:1fr; padding-bottom:60px}
      .controls{flex-wrap:wrap}
    }
  </style>
</head>
<body>

<header>
  <div class="brand">
    <div class="logo">MB</div>
    <div>
      <div class="title">Mini Blogger</div>
      <div class="subtitle">Demo blogging platform (local only)</div>
    </div>
  </div>

  <div class="controls">
    <button class="btn" id="exportBtn" title="Export all posts">Export JSON</button>
    <input id="importFile" type="file" accept="application/json" style="display:none"/>
    <button class="btn" id="importBtn">Import JSON</button>
    <button class="btn" id="adminBtn">Admin</button>
  </div>
</header>

<main class="container">
  <!-- left: editor + posts -->
  <section>
    <div class="card" id="editorCard">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px">
        <div>
          <strong id="editorTitle">Create New Post</strong>
          <div class="small-muted">Write something valuable — hits are imaginary in this demo 😉</div>
        </div>
        <div class="small-muted" id="statusInfo"></div>
      </div>

      <div>
        <label class="small">Title</label>
        <input id="postTitle" type="text" placeholder="Post title">

        <div class="form-row">
          <div style="flex:1">
            <label class="small">Tags (comma separated)</label>
            <input id="postTags" type="text" placeholder="e.g. webdev, javascript">
          </div>
          <div style="width:160px">
            <label class="small">Category</label>
            <input id="postCategory" type="text" placeholder="e.g. Tutorial">
          </div>
        </div>

        <label class="small">Feature Image (optional)</label>
        <input id="postImage" type="file" accept="image/*">

        <label class="small" style="margin-top:8px">Content (supports simple markdown-like formatting)</label>
        <div class="editor-toolbar">
          <button class="btn" id="mdBold" title="Wrap selection with **">Bold</button>
          <button class="btn" id="mdItalic" title="Wrap selection with *">Italic</button>
          <button class="btn" id="mdH1" title="Insert # Heading">H1</button>
          <button class="btn" id="mdH2" title="Insert ## Heading">H2</button>
          <button class="btn" id="previewToggle">Toggle Preview</button>
        </div>
        <textarea id="postContent" rows="10" placeholder="Write your post here..."></textarea>

        <div style="display:flex;gap:8px;margin-top:10px">
          <button class="btn primary" id="publishBtn">Publish</button>
          <button class="btn" id="saveDraftBtn">Save Draft</button>
          <button class="btn" id="clearBtn">Clear</button>
        </div>
      </div>
      <div id="previewArea" style="margin-top:12px;display:none">
        <div class="preview" id="previewContent">Preview will appear here</div>
      </div>
    </div>

    <!-- posts list -->
    <div class="card" style="margin-top:16px">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px">
        <div><strong>Posts</strong> <span class="small-muted" id="postsCount"></span></div>
        <div style="display:flex;gap:8px;align-items:center">
          <input id="searchInput" type="text" placeholder="Search posts..." style="padding:8px;border-radius:8px;border:1px solid #e6e9ef">
          <select id="sortSelect" style="padding:8px;border-radius:8px;border:1px solid #e6e9ef">
            <option value="new">Newest</option>
            <option value="old">Oldest</option>
            <option value="title">Title A→Z</option>
          </select>
        </div>
      </div>

      <div id="postsList" class="posts-list"></div>

      <div style="display:flex;justify-content:center;margin-top:10px">
        <button class="btn" id="prevPage">Prev</button>
        <div style="width:12px"></div>
        <button class="btn" id="nextPage">Next</button>
      </div>
    </div>
  </section>

  <!-- right: sidebar -->
  <aside>
    <div class="card sidebar-section">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px">
        <div><strong>Filters</strong></div>
        <div class="small-muted"><span id="filterActive">All</span></div>
      </div>

      <div style="margin-bottom:8px">
        <label class="small">Filter by tag</label>
        <div id="tagsCloud"></div>
      </div>

      <div style="margin-bottom:8px">
        <label class="small">Filter by category</label>
        <div id="categoriesList"></div>
      </div>

      <div style="margin-top:8px">
        <label class="small">Admin</label>
        <div style="display:flex;gap:8px;margin-top:6px">
          <input id="adminPass" type="password" placeholder="Admin password" style="padding:8px;border-radius:8px;border:1px solid #e6e9ef">
          <button class="btn" id="loginBtn">Login</button>
        </div>
        <div id="adminStatus" class="small-muted" style="margin-top:6px"></div>
      </div>
    </div>

    <div class="card sidebar-section">
      <strong>About this demo</strong>
      <p class="small-muted" style="margin-top:8px;font-size:13px">
        This is a client-side demo: data is stored in your browser (<code>localStorage</code>). It's not secure for production.
      </p>
      <div style="margin-top:8px;display:flex;gap:8px">
        <button class="btn" id="clearAll">Clear all data</button>
      </div>
    </div>

    <div class="card">
      <strong>Quick help</strong>
      <ul style="padding-left:18px;margin-top:8px;color:var(--muted)">
        <li>Write posts and click Publish.</li>
        <li>Use image upload to add a feature image.</li>
        <li>Export JSON to backup; import to restore.</li>
      </ul>
    </div>
  </aside>
</main>

<!-- Post view modal (simple) -->
<div id="modal" style="position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:rgba(6,6,7,0.45);padding:20px">
  <div style="width:100%;max-width:880px;background:var(--card);border-radius:12px;padding:18px;box-shadow:var(--shadow);max-height:90vh;overflow:auto">
    <div style="display:flex;justify-content:space-between;align-items:center">
      <div>
        <h2 id="modalTitle" style="margin:0">Post title</h2>
        <div class="meta" id="modalMeta">date • category • tags</div>
      </div>
      <div style="display:flex;gap:8px">
        <button class="btn" id="modalEdit">Edit</button>
        <button class="btn" id="modalClose">Close</button>
      </div>
    </div>
    <hr style="margin:12px 0;border:none;border-top:1px solid #eef2ff">
    <div id="modalImage" style="margin-bottom:12px"></div>
    <article id="modalContent" class="post-content" style="line-height:1.65"></article>

    <section class="comments" id="modalComments">
      <h3 style="margin-top:18px">Comments</h3>
      <div id="commentsList" style="margin-top:8px"></div>

      <div style="margin-top:10px">
        <input id="commentAuthor" type="text" placeholder="Your name" style="padding:8px;border-radius:6px;border:1px solid #e6e9ef;width:40%;margin-right:8px">
        <input id="commentEmail" type="text" placeholder="Email (optional)" style="padding:8px;border-radius:6px;border:1px solid #e6e9ef;width:40%;margin-right:8px">
        <button class="btn" id="addCommentBtn">Add Comment</button>
        <div style="margin-top:8px">
          <textarea id="commentText" rows="3" placeholder="Write your comment..." style="width:100%;padding:8px;border-radius:6px;border:1px solid #e6e9ef;margin-top:8px"></textarea>
        </div>
      </div>
    </section>
  </div>
</div>

<script>
/**********************************************************************
 * Mini Blogger — client-side demo
 * Storage: localStorage key "miniBlogger.posts" and "miniBlogger.admin"
 * Author: demo code
 **********************************************************************/

/* Utility helpers */
const $ = id => document.getElementById(id);
const saveToLS = (k, v) => localStorage.setItem(k, JSON.stringify(v));
const loadFromLS = (k, fallback) => {
  try { const v = JSON.parse(localStorage.getItem(k)); return v === null ? fallback : v; }
  catch(e){ return fallback; }
};

/* Data model */
const LS_KEY = "miniBlogger.posts";
const LS_ADMIN = "miniBlogger.admin";

let posts = loadFromLS(LS_KEY, []); // array of {id,title,content,tags,category,image(base64),date,draft,comments:[]}
let admin = loadFromLS(LS_ADMIN, {logged:false,pass:null}); // simplistic
let currentEditId = null;
let perPage = 5;
let currentPage = 1;
let currentFilter = {tag:null,category:null,search:"",sort:"new"};

/* DOM nodes */
const postTitle = $("postTitle");
const postContent = $("postContent");
const postTags = $("postTags");
const postCategory = $("postCategory");
const postImage = $("postImage");
const publishBtn = $("publishBtn");
const saveDraftBtn = $("saveDraftBtn");
const clearBtn = $("clearBtn");
const postsList = $("postsList");
const postsCount = $("postsCount");
const searchInput = $("searchInput");
const sortSelect = $("sortSelect");
const tagsCloud = $("tagsCloud");
const categoriesList = $("categoriesList");
const prevPageBtn = $("prevPage");
const nextPageBtn = $("nextPage");
const previewToggle = $("previewToggle");
const previewArea = $("previewArea");
const previewContent = $("previewContent");
const adminBtn = $("adminBtn");
const adminPass = $("adminPass");
const loginBtn = $("loginBtn");
const adminStatus = $("adminStatus");
const exportBtn = $("exportBtn");
const importBtn = $("importBtn");
const importFile = $("importFile");
const clearAllBtn = $("clearAll");
const statusInfo = $("statusInfo");

/* Modal nodes */
const modal = $("modal");
const modalTitle = $("modalTitle");
const modalMeta = $("modalMeta");
const modalContent = $("modalContent");
const modalImage = $("modalImage");
const modalClose = $("modalClose");
const modalEdit = $("modalEdit");
const commentsList = $("commentsList");
const commentAuthor = $("commentAuthor");
const commentEmail = $("commentEmail");
const commentText = $("commentText");
const addCommentBtn = $("addCommentBtn");

/* Markdown-ish renderer (simple) */
function renderMarkdown(text){
  // escape HTML
  const esc = s => s.replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;");
  text = esc(text || "");

  // convert code blocks ```...``` (simple)
  text = text.replace(/```([\s\S]*?)```/g, (m, code) => `<pre style="background:#0f172a;color:#fff;padding:10px;border-radius:6px;overflow:auto">${code.replace(/&/g,"&amp;")}</pre>`);

  // headings
  text = text.replace(/^# (.*$)/gm, "<h1>$1</h1>");
  text = text.replace(/^## (.*$)/gm, "<h2>$1</h2>");
  text = text.replace(/^### (.*$)/gm, "<h3>$1</h3>");

  // bold **text**
  text = text.replace(/\*\*(.+?)\*\*/g, "<strong>$1</strong>");
  // italic *text*
  text = text.replace(/\*(.+?)\*/g, "<em>$1</em>");
  // links [text](url)
  text = text.replace(/\[([^\]]+)\]\((https?:\/\/[^\s)]+)\)/g, '<a href="$2" target="_blank" rel="noopener noreferrer">$1</a>');
  // line breaks to paragraphs
  const paragraphs = text.split(/\n{2,}/).map(p => `<p>${p.replace(/\n/g, "<br>")}</p>`).join("");
  return paragraphs;
}

/* ID generator */
function uid(prefix="p") {
  return prefix + Date.now().toString(36) + Math.random().toString(36).slice(2,8);
}

/* Save & load */
function persist(){
  saveToLS(LS_KEY, posts);
  statusInfo.textContent = `Saved ${new Date().toLocaleTimeString()}`;
}

/* UI: build posts listing based on filters */
function getFilteredSorted(){
  let list = posts.filter(p => !p.deleted);
  // search
  const q = currentFilter.search.trim().toLowerCase();
  if(q) list = list.filter(p => (p.title + " " + p.content + " " + (p.tags||'') + " " + (p.category||'')).toLowerCase().includes(q));
  // tag filter
  if(currentFilter.tag) list = list.filter(p => (p.tags||[]).includes(currentFilter.tag));
  if(currentFilter.category) list = list.filter(p => p.category === currentFilter.category);
  // sort
  if(currentFilter.sort === "new") list.sort((a,b)=> new Date(b.date) - new Date(a.date));
  else if(currentFilter.sort === "old") list.sort((a,b)=> new Date(a.date) - new Date(b.date));
  else if(currentFilter.sort === "title") list.sort((a,b)=> a.title.localeCompare(b.title));
  return list;
}

function renderPosts(){
  const list = getFilteredSorted();
  postsCount.textContent = `(${list.length})`;
  // pagination
  const totalPages = Math.max(1, Math.ceil(list.length / perPage));
  if(currentPage > totalPages) currentPage = totalPages;
  const start = (currentPage-1)*perPage;
  const pageItems = list.slice(start, start+perPage);

  postsList.innerHTML = "";
  if(pageItems.length === 0){
    postsList.innerHTML = `<div class="small-muted">No posts found. Create your first post!</div>`;
  } else {
    pageItems.forEach(p => {
      const div = document.createElement("div");
      div.className = "post-item";
      const tagsHtml = (p.tags || []).map(t => `<span class="tag">${t}</span>`).join(" ");
      div.innerHTML = `
        <h3>${escapeHtml(p.title || "(Untitled)")}</h3>
        <div class="meta">${new Date(p.date).toLocaleString()} • ${p.category || "Uncategorized"}</div>
        <div style="margin-top:8px">${(p.excerpt || createExcerpt(p.content || ""))}</div>
        <div style="margin-top:8px">${tagsHtml}</div>
        <div class="post-actions">
          <button class="btn" data-id="${p.id}" data-action="view">View</button>
          <button class="btn" data-id="${p.id}" data-action="edit">Edit</button>
          <button class="btn" data-id="${p.id}" data-action="delete">Delete</button>
        </div>
      `;
      postsList.appendChild(div);
    });
  }
  // page buttons enable/disable
  prevPageBtn.disabled = currentPage <= 1;
  nextPageBtn.disabled = currentPage >= totalPages;
  // refresh side filters
  renderTagsAndCategories();
}

/* create small excerpt */
function createExcerpt(md){
  const stripped = md.replace(/[#*_`\[\]\(\)]/g, "");
  return stripped.length > 200 ? stripped.slice(0,200) + "..." : stripped;
}

/* escape for html title */
function escapeHtml(s){ return (s || "").replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;"); }

/* render tags cloud and categories */
function renderTagsAndCategories(){
  const tagCounts = {};
  const catCounts = {};
  posts.forEach(p => {
    (p.tags||[]).forEach(t => tagCounts[t] = (tagCounts[t]||0) + 1);
    if(p.category) catCounts[p.category] = (catCounts[p.category]||0) + 1;
  });

  tagsCloud.innerHTML = "";
  Object.keys(tagCounts).sort().forEach(t => {
    const el = document.createElement("button");
    el.className = "tag";
    el.textContent = `${t} (${tagCounts[t]})`;
    el.onclick = () => { currentFilter.tag = currentFilter.tag === t ? null : t; updateFilterUI(); renderPosts(); };
    tagsCloud.appendChild(el);
  });
  categoriesList.innerHTML = "";
  Object.keys(catCounts).sort().forEach(c => {
    const btn = document.createElement("button");
    btn.className = "btn";
    btn.style.fontSize = "13px";
    btn.textContent = `${c} (${catCounts[c]})`;
    btn.onclick = () => { currentFilter.category = currentFilter.category === c ? null : c; updateFilterUI(); renderPosts(); };
    categoriesList.appendChild(btn);
  });

  // show active filter
  $("filterActive").textContent = currentFilter.tag ? `Tag: ${currentFilter.tag}` : currentFilter.category ? `Category: ${currentFilter.category}` : "All";
}

/* Filter UI */
function updateFilterUI(){
  // highlight search and sort controls if needed
}

/* Editor actions */
function resetEditor(){
  currentEditId = null;
  $("editorTitle").textContent = "Create New Post";
  postTitle.value = "";
  postContent.value = "";
  postTags.value = "";
  postCategory.value = "";
  postImage.value = "";
  previewArea.style.display = "none";
}

function loadPostIntoEditor(p){
  currentEditId = p.id;
  $("editorTitle").textContent = "Edit Post";
  postTitle.value = p.title || "";
  postContent.value = p.content || "";
  postTags.value = (p.tags || []).join(", ");
  postCategory.value = p.category || "";
  previewArea.style.display = "none";
}

/* handle image file -> base64 */
function fileToBase64(file){
  return new Promise((resolve, reject) => {
    if(!file) return resolve(null);
    const reader = new FileReader();
    reader.onload = () => resolve(reader.result);
    reader.onerror = (e) => reject(e);
    reader.readAsDataURL(file);
  });
}

/* Publish or save draft */
publishBtn.addEventListener("click", async () => {
  const title = postTitle.value.trim();
  const content = postContent.value.trim();
  if(!title || !content){ if(!confirm("Publish without title or content?")) return; }

  const tags = postTags.value.split(",").map(s=>s.trim()).filter(Boolean);
  const category = postCategory.value.trim();

  // process image
  const imgFile = postImage.files
