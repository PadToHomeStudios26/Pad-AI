<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
<title>PAD AI – by Pad to Home Studios</title>
<style>
  :root {
    --bg: #050505;
    --sidebar: #0a0a0a;
    --panel: #0c0c0c;
    --text: #ececec;
    --muted: #8e8e8e;
    --accent: #d4af37;
    --hover: #161616;
    --border: #1f1f1f;
    --user: #1a1a1a;
    --ai: #111111;
    --font: 15px;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }
  html, body {
    height: 100%;
    background: var(--bg);
    color: var(--text);
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", system-ui, sans-serif;
    font-size: var(--font);
    overflow: hidden;
  }

  body { display: flex; }

  /* ========== SIDEBAR (ChatGPT-Style) ========== */
  #sidebar {
    width: 260px;
    background: var(--sidebar);
    border-right: 1px solid var(--border);
    display: flex;
    flex-direction: column;
    transition: margin-left 0.25s ease;
    z-index: 40;
  }
  #sidebar.collapsed { margin-left: -260px; }

  .sb-top {
    padding: 14px 12px 8px;
  }

  .brand {
    font-size: 15px;
    font-weight: 600;
    color: var(--accent);
    padding: 4px 8px 12px;
  }
  .brand small {
    display: block;
    font-size: 10px;
    color: var(--muted);
    font-weight: 400;
    margin-top: 1px;
  }

  .new-chat {
    width: 100%;
    background: transparent;
    border: 1px solid var(--border);
    color: var(--text);
    padding: 10px 12px;
    border-radius: 8px;
    font-size: 13.5px;
    cursor: pointer;
    text-align: left;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .new-chat:hover { background: var(--hover); }

  .chat-list {
    flex: 1;
    overflow-y: auto;
    padding: 8px 8px 12px;
  }

  .chat-item {
    padding: 9px 10px;
    border-radius: 8px;
    font-size: 13.5px;
    color: var(--muted);
    cursor: pointer;
    margin-bottom: 2px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .chat-item:hover { background: var(--hover); color: var(--text); }
  .chat-item.active {
    background: var(--hover);
    color: var(--text);
  }

  .sb-bottom {
    padding: 10px 12px 14px;
    border-top: 1px solid var(--border);
  }
  .sb-bottom button {
    width: 100%;
    background: transparent;
    border: none;
    color: var(--text);
    padding: 9px 10px;
    border-radius: 8px;
    font-size: 13.5px;
    cursor: pointer;
    text-align: left;
  }
  .sb-bottom button:hover { background: var(--hover); }

  /* ========== MAIN ========== */
  #main {
    flex: 1;
    display: flex;
    flex-direction: column;
    min-width: 0;
    height: 100%;
  }

  .topbar {
    height: 52px;
    border-bottom: 1px solid var(--border);
    display: flex;
    align-items: center;
    padding: 0 14px;
    gap: 12px;
    background: var(--panel);
  }
  .menu-btn {
    background: transparent;
    border: 1px solid var(--border);
    color: var(--text);
    width: 34px;
    height: 34px;
    border-radius: 7px;
    cursor: pointer;
    font-size: 17px;
  }
  .chat-title {
    font-weight: 500;
    flex: 1;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    font-size: 14.5px;
  }

  #chatArea {
    flex: 1;
    overflow-y: auto;
    padding: 20px 16px;
    display: flex;
    flex-direction: column;
    gap: 16px;
  }

  .msg {
    max-width: 740px;
    padding: 12px 15px;
    border-radius: 12px;
    line-height: 1.55;
  }
  .msg.user {
    background: var(--user);
    align-self: flex-end;
    border: 1px solid var(--border);
  }
  .msg.ai {
    background: var(--ai);
    align-self: flex-start;
    border: 1px solid var(--border);
  }
  .msg .role {
    font-size: 11px;
    color: var(--accent);
    font-weight: 600;
    margin-bottom: 4px;
  }
  .msg .content {
    white-space: pre-wrap;
    overflow-wrap: anywhere;
  }
  .msg img {
    max-width: 100%;
    max-height: 260px;
    border-radius: 10px;
    margin-top: 8px;
    display: block;
  }

  /* Input */
  .input-area {
    border-top: 1px solid var(--border);
    padding: 12px 14px;
    background: var(--panel);
  }
  .preview {
    margin-bottom: 10px;
    position: relative;
    display: inline-block;
  }
  .preview img {
    max-height: 100px;
    border-radius: 9px;
    border: 1px solid var(--border);
  }
  .preview button {
    position: absolute;
    top: -7px;
    right: -7px;
    background: #e74c3c;
    color: #fff;
    border: none;
    width: 22px;
    height: 22px;
    border-radius: 50%;
    cursor: pointer;
  }
  .input-row {
    display: flex;
    gap: 8px;
    align-items: flex-end;
  }
  #userInput {
    flex: 1;
    background: #111;
    border: 1px solid var(--border);
    color: var(--text);
    border-radius: 12px;
    padding: 11px 13px;
    resize: none;
    min-height: 44px;
    max-height: 120px;
    font: inherit;
  }
  .btn {
    width: 44px;
    height: 44px;
    border-radius: 11px;
    border: none;
    cursor: pointer;
    font-size: 17px;
    flex-shrink: 0;
  }
  .btn-attach {
    background: #1a1a1a;
    color: var(--text);
    border: 1px solid var(--border);
  }
  .btn-send {
    background: var(--accent);
    color: #000;
  }

  /* Modal */
  .modal {
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.7);
    display: none;
    align-items: center;
    justify-content: center;
    z-index: 100;
  }
  .modal.open { display: flex; }
  .modal-box {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 14px;
    width: 92%;
    max-width: 390px;
    max-height: 85vh;
    overflow-y: auto;
    padding: 18px;
  }
  .modal-box h2 {
    color: var(--accent);
    font-size: 17px;
    margin-bottom: 14px;
  }
  .field { margin-bottom: 13px; }
  .field label {
    display: block;
    font-size: 12px;
    color: var(--muted);
    margin-bottom: 4px;
  }
  .field input {
    width: 100%;
    background: #111;
    border: 1px solid var(--border);
    color: var(--text);
    padding: 9px 11px;
    border-radius: 8px;
    font: inherit;
  }
  .colors { display: flex; gap: 12px; }
  .colors input[type=color] {
    width: 46px;
    height: 34px;
    border: none;
    background: none;
    padding: 0;
  }
  .actions {
    display: flex;
    gap: 8px;
    margin-top: 16px;
  }
  .actions button {
    flex: 1;
    padding: 10px;
    border-radius: 8px;
    border: none;
    cursor: pointer;
    font-weight: 600;
  }
  .btn-save { background: var(--accent); color: #000; }
  .btn-reset { background: #222; color: var(--text); border: 1px solid var(--border); }
  .hint { font-size: 11.5px; color: var(--muted); margin-top: 10px; line-height: 1.4; }
</style>
</head>
<body>

<aside id="sidebar">
  <div class="sb-top">
    <div class="brand">
      PAD AI
      <small>by Pad to Home Studios</small>
    </div>
    <button class="new-chat" id="newChat">＋ Neue Unterhaltung</button>
  </div>

  <div class="chat-list" id="chatList"></div>

  <div class="sb-bottom">
    <button id="openSettings">⚙️ Einstellungen</button>
  </div>
</aside>

<div id="main">
  <div class="topbar">
    <button class="menu-btn" id="toggleSidebar">☰</button>
    <div class="chat-title" id="chatTitle">Neue Unterhaltung</div>
  </div>

  <div id="chatArea"></div>

  <div class="input-area">
    <div class="preview" id="previewBox" style="display:none">
      <img id="previewImg" alt="Vorschau">
      <button id="removeImg">×</button>
    </div>
    <div class="input-row">
      <textarea id="userInput" placeholder="Nachricht schreiben..." rows="1"></textarea>
      <button class="btn btn-attach" id="attachBtn">📷</button>
      <button class="btn btn-send" id="sendBtn">➤</button>
      <input type="file" id="fileInput" accept="image/*" hidden>
    </div>
  </div>
</div>

<div class="modal" id="settingsModal">
  <div class="modal-box">
    <h2>Einstellungen</h2>
    <div class="field">
      <label>Chat-Endpunkt</label>
      <input id="setEndpoint" value="https://api.groq.com/openai/v1/chat/completions">
    </div>
    <div class="field">
      <label>Modell</label>
      <input id="setModel" value="qwen/qwen3.8-27b">
    </div>
    <div class="field">
      <label>API-Schlüssel</label>
      <input id="setApiKey" type="password" placeholder="Dein Key">
    </div>
    <div style="height:1px;background:var(--border);margin:14px 0"></div>
    <div class="field">
      <label>Schriftgröße</label>
      <input type="range" id="setFont" min="13" max="19" value="15">
    </div>
    <div class="field">
      <label>Farben</label>
      <div class="colors">
        <div><small style="color:var(--muted)">Hintergrund</small><br><input type="color" id="setBg" value="#050505"></div>
        <div><small style="color:var(--muted)">Text</small><br><input type="color" id="setText" value="#ececec"></div>
        <div><small style="color:var(--muted)">Akzent</small><br><input type="color" id="setAccent" value="#d4af37"></div>
      </div>
    </div>
    <div class="actions">
      <button class="btn-reset" id="resetTheme">Standard</button>
      <button class="btn-save" id="saveSettings">Speichern</button>
    </div>
    <p class="hint">Alles wird nur lokal auf deinem Gerät gespeichert.</p>
  </div>
</div>

<script>
const $ = id => document.getElementById(id);
let state = {
  chats: [],
  currentId: null,
  settings: {
    endpoint: "https://api.groq.com/openai/v1/chat/completions",
    model: "qwen/qwen3.8-27b",
    apiKey: "",
    fontSize: 15,
    bg: "#050505",
    text: "#ececec",
    accent: "#d4af37"
  }
};
let pendingImage = null;

function uid(){return Date.now().toString(36)+Math.random().toString(36).slice(2,6)}
function load(){
  try{const r=localStorage.getItem("padai_v6");if(r)state=JSON.parse(r)}catch(e){}
  applyTheme();
  if(!state.chats.length)createChat();
  else if(!state.currentId)state.currentId=state.chats[0].id;
  renderList();renderChat();
}
function save(){localStorage.setItem("padai_v6",JSON.stringify(state))}
function applyTheme(){
  const s=state.settings;
  document.documentElement.style.setProperty("--bg",s.bg);
  document.documentElement.style.setProperty("--sidebar",s.bg);
  document.documentElement.style.setProperty("--panel",s.bg);
  document.documentElement.style.setProperty("--text",s.text);
  document.documentElement.style.setProperty("--accent",s.accent);
  document.documentElement.style.setProperty("--font",s.fontSize+"px");
  $("setEndpoint").value=s.endpoint;
  $("setModel").value=s.model;
  $("setApiKey").value=s.apiKey||"";
  $("setFont").value=s.fontSize;
  $("setBg").value=s.bg;
  $("setText").value=s.text;
  $("setAccent").value=s.accent;
}
function createChat(){
  const c={id:uid(),title:"Neue Unterhaltung",messages:[],created:Date.now()};
  state.chats.unshift(c);state.currentId=c.id;save();renderList();renderChat();
}
function current(){return state.chats.find(c=>c.id===state.currentId)}
function renderList(){
  const list=$("chatList");list.innerHTML="";
  state.chats.forEach(c=>{
    const el=document.createElement("div");
    el.className="chat-item"+(c.id===state.currentId?" active":"");
    el.textContent=c.title||"Neue Unterhaltung";
    el.onclick=()=>{state.currentId=c.id;save();renderList();renderChat()};
    list.appendChild(el);
  });
}
function esc(t){return(typeof t==="string"?t:"").replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;")}
function renderChat(){
  const chat=current();if(!chat)return;
  $("chatTitle").textContent=chat.title;
  const area=$("chatArea");area.innerHTML="";
  if(!chat.messages.length){
    const w=document.createElement("div");
    w.className="msg ai";
    w.innerHTML=`<div class="role">PAD AI</div><div class="content">Hey! Ich bin PAD AI by Pad to Home Studios.\nSchreib mir etwas oder häng ein Bild an.</div>`;
    area.appendChild(w);
  }else{
    chat.messages.forEach(m=>{
      const el=document.createElement("div");
      el.className="msg "+(m.role==="user"?"user":"ai");
      let h=`<div class="role">${m.role==="user"?"Du":"PAD AI"}</div><div class="content">${esc(m.text||"")}</div>`;
      if(m.image)h+=`<img src="${m.image}" alt="Bild">`;
      el.innerHTML=h;area.appendChild(el);
    });
  }
  area.scrollTop=area.scrollHeight;
}
async function send(){
  const text=$("userInput").value.trim();
  if(!text&&!pendingImage)return;
  const chat=current();if(!chat)return;
  chat.messages.push({role:"user",text:text||"",image:pendingImage||null});
  if(chat.title==="Neue Unterhaltung"&&text)chat.title=text.slice(0,32)+(text.length>32?"…":"");
  $("userInput").value="";
  const img=pendingImage;pendingImage=null;
  $("previewBox").style.display="none";$("fileInput").value="";
  renderChat();renderList();
  const apiMsgs=[];
  chat.messages.forEach(m=>{
    if(m.role==="user"){
      if(m.image){
        const c=[];if(m.text)c.push({type:"text",text:m.text});
        c.push({type:"image_url",image_url:{url:m.image}});
        apiMsgs.push({role:"user",content:c});
      }else apiMsgs.push({role:"user",content:m.text});
    }else apiMsgs.push({role:"assistant",content:m.text});
  });
  $("sendBtn").disabled=true;
  try{
    const headers={"Content-Type":"application/json"};
    if(state.settings.apiKey)headers.Authorization="Bearer "+state.settings.apiKey;
    const res=await fetch(state.settings.endpoint,{method:"POST",headers,body:JSON.stringify({model:state.settings.model,messages:apiMsgs.slice(-14),stream:false})});
    if(!res.ok){const t=await res.text();throw new Error("HTTP "+res.status+" – "+t.slice(0,150))}
    const data=await res.json();
    chat.messages.push({role:"assistant",text:data.choices?.[0]?.message?.content||"Keine Antwort."});
  }catch(e){chat.messages.push({role:"assistant",text:"Fehler: "+e.message})}
  $("sendBtn").disabled=false;save();renderChat();renderList();
}

$("newChat").onclick=createChat;
$("toggleSidebar").onclick=()=>$("sidebar").classList.toggle("collapsed");
$("sendBtn").onclick=send;
$("userInput").addEventListener("keydown",e=>{if(e.key==="Enter"&&!e.shiftKey){e.preventDefault();send()}});
$("attachBtn").onclick=()=>$("fileInput").click();
$("fileInput").onchange=e=>{
  const f=e.target.files[0];if(!f)return;
  const r=new FileReader();
  r.onload=ev=>{pendingImage=ev.target.result;$("previewImg").src=pendingImage;$("previewBox").style.display="block"};
  r.readAsDataURL(f);
};
$("removeImg").onclick=()=>{pendingImage=null;$("previewBox").style.display="none";$("fileInput").value=""};
$("openSettings").onclick=()=>$("settingsModal").classList.add("open");
$("settingsModal").onclick=e=>{if(e.target===$("settingsModal"))$("settingsModal").classList.remove("open")};
$("saveSettings").onclick=()=>{
  state.settings.endpoint=$("setEndpoint").value.trim();
  state.settings.model=$("setModel").value.trim();
  state.settings.apiKey=$("setApiKey").value.trim();
  state.settings.fontSize=parseInt($("setFont").value);
  state.settings.bg=$("setBg").value;
  state.settings.text=$("setText").value;
  state.settings.accent=$("setAccent").value;
  applyTheme();save();$("settingsModal").classList.remove("open");
};
$("resetTheme").onclick=()=>{
  $("setBg").value="#050505";$("setText").value="#ececec";$("setAccent").value="#d4af37";$("setFont").value=15;
};
$("setFont").oninput=()=>document.documentElement.style.setProperty("--font",$("setFont").value+"px");
load();
</script>
</body>
</html>
