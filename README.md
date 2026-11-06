<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
<title>PAD AI – by Pad to Home Studios</title>
<style>
  :root {
    --bg: #050505;
    --panel: #0d0d0d;
    --sidebar: #0a0a0a;
    --text: #f0f0f0;
    --muted: #a0a0a0;
    --accent: #d4af37;
    --accent-hover: #e8c547;
    --border: #1f1f1f;
    --user-bubble: #1a1a1a;
    --ai-bubble: #111111;
    --font-size: 15px;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    background: var(--bg);
    color: var(--text);
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", system-ui, sans-serif;
    font-size: var(--font-size);
    height: 100vh;
    overflow: hidden;
    display: flex;
  }

  /* Sidebar */
  #sidebar {
    width: 280px;
    background: var(--sidebar);
    border-right: 1px solid var(--border);
    display: flex;
    flex-direction: column;
    transition: margin-left 0.25s ease;
    z-index: 100;
  }
  #sidebar.collapsed { margin-left: -280px; }

  .sidebar-header {
    padding: 18px 16px;
    border-bottom: 1px solid var(--border);
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .brand {
    font-weight: 700;
    font-size: 17px;
    color: var(--accent);
    letter-spacing: 0.5px;
  }
  .brand small {
    display: block;
    font-size: 10px;
    color: var(--muted);
    font-weight: 400;
    margin-top: 2px;
  }

  .new-chat-btn {
    margin: 12px;
    background: var(--accent);
    color: #000;
    border: none;
    padding: 11px;
    border-radius: 10px;
    font-weight: 600;
    cursor: pointer;
    width: calc(100% - 24px);
  }
  .new-chat-btn:hover { background: var(--accent-hover); }

  .chat-list {
    flex: 1;
    overflow-y: auto;
    padding: 8px;
  }
  .chat-item {
    padding: 11px 12px;
    border-radius: 9px;
    cursor: pointer;
    margin-bottom: 4px;
    font-size: 13.5px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    color: var(--muted);
  }
  .chat-item:hover, .chat-item.active {
    background: #161616;
    color: var(--text);
  }
  .chat-item.active { border-left: 3px solid var(--accent); }

  .sidebar-footer {
    padding: 12px;
    border-top: 1px solid var(--border);
  }
  .sidebar-footer button {
    width: 100%;
    background: transparent;
    border: 1px solid var(--border);
    color: var(--text);
    padding: 9px;
    border-radius: 8px;
    cursor: pointer;
    margin-bottom: 6px;
    font-size: 13px;
  }

  /* Main */
  #main {
    flex: 1;
    display: flex;
    flex-direction: column;
    min-width: 0;
  }

  .topbar {
    height: 56px;
    border-bottom: 1px solid var(--border);
    display: flex;
    align-items: center;
    padding: 0 16px;
    gap: 12px;
    background: var(--panel);
  }
  .toggle-sidebar {
    background: transparent;
    border: 1px solid var(--border);
    color: var(--text);
    width: 36px;
    height: 36px;
    border-radius: 8px;
    cursor: pointer;
    font-size: 18px;
  }
  .current-title {
    font-weight: 600;
    flex: 1;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  /* Chat area */
  #chatArea {
    flex: 1;
    overflow-y: auto;
    padding: 20px 16px;
    display: flex;
    flex-direction: column;
    gap: 14px;
  }
  .msg {
    max-width: 780px;
    padding: 12px 16px;
    border-radius: 14px;
    line-height: 1.55;
    white-space: pre-wrap;
    overflow-wrap: anywhere;
  }
  .msg.user {
    background: var(--user-bubble);
    align-self: flex-end;
    border: 1px solid var(--border);
  }
  .msg.ai {
    background: var(--ai-bubble);
    align-self: flex-start;
    border: 1px solid var(--border);
  }
  .msg .role {
    font-size: 11px;
    color: var(--accent);
    margin-bottom: 4px;
    font-weight: 600;
  }

  /* Input */
  .input-area {
    border-top: 1px solid var(--border);
    padding: 14px 16px;
    background: var(--panel);
  }
  .input-row {
    display: flex;
    gap: 10px;
    align-items: flex-end;
  }
  #userInput {
    flex: 1;
    background: #111;
    border: 1px solid var(--border);
    color: var(--text);
    border-radius: 12px;
    padding: 12px 14px;
    resize: none;
    min-height: 48px;
    max-height: 140px;
    font: inherit;
  }
  .send-btn, .img-btn {
    background: var(--accent);
    color: #000;
    border: none;
    width: 48px;
    height: 48px;
    border-radius: 12px;
    cursor: pointer;
    font-size: 18px;
    flex-shrink: 0;
  }
  .img-btn { background: #222; color: var(--text); border: 1px solid var(--border); }

  .image-preview {
    margin-bottom: 10px;
    position: relative;
    display: inline-block;
  }
  .image-preview img {
    max-height: 120px;
    border-radius: 10px;
    border: 1px solid var(--border);
  }
  .image-preview button {
    position: absolute;
    top: -8px;
    right: -8px;
    background: #ff5555;
    border: none;
    color: white;
    width: 24px;
    height: 24px;
    border-radius: 50%;
    cursor: pointer;
  }

  /* Settings Modal */
  .modal {
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.75);
    display: none;
    align-items: center;
    justify-content: center;
    z-index: 200;
  }
  .modal.open { display: flex; }
  .modal-content {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 16px;
    width: 92%;
    max-width: 420px;
    max-height: 85vh;
    overflow-y: auto;
    padding: 20px;
  }
  .modal h2 { margin-bottom: 16px; color: var(--accent); }
  .field { margin-bottom: 14px; }
  .field label { display: block; font-size: 12px; color: var(--muted); margin-bottom: 5px; }
  .field input, .field select {
    width: 100%;
    background: #111;
    border: 1px solid var(--border);
    color: var(--text);
    padding: 10px 12px;
    border-radius: 8px;
    font: inherit;
  }
  .color-row {
    display: flex;
    gap: 10px;
  }
  .color-row input[type="color"] {
    width: 50px;
    height: 38px;
    padding: 2px;
    border: none;
    background: none;
  }
  .btn-row {
    display: flex;
    gap: 8px;
    margin-top: 18px;
  }
  .btn-row button {
    flex: 1;
    padding: 11px;
    border-radius: 9px;
    border: none;
    cursor: pointer;
    font-weight: 600;
  }
  .btn-primary { background: var(--accent); color: #000; }
  .btn-secondary { background: #222; color: var(--text); border: 1px solid var(--border); }

  .hint { font-size: 12px; color: var(--muted); margin-top: 8px; line-height: 1.4; }
</style>
</head>
<body>

<!-- SIDEBAR -->
<aside id="sidebar">
  <div class="sidebar-header">
    <div class="brand">
      PAD AI
      <small>by Pad to Home Studios</small>
    </div>
  </div>
  <button class="new-chat-btn" id="newChat">＋ Neue Unterhaltung</button>
  <div class="chat-list" id="chatList"></div>
  <div class="sidebar-footer">
    <button id="openSettings">⚙️ Einstellungen</button>
  </div>
</aside>

<!-- MAIN -->
<div id="main">
  <div class="topbar">
    <button class="toggle-sidebar" id="toggleSidebar">☰</button>
    <div class="current-title" id="currentTitle">Neue Unterhaltung</div>
  </div>

  <div id="chatArea"></div>

  <div class="input-area">
    <div id="imagePreviewBox" class="image-preview" style="display:none">
      <img id="previewImg" alt="Vorschau">
      <button id="removeImage">×</button>
    </div>
    <div class="input-row">
      <textarea id="userInput" placeholder="Nachricht schreiben..." rows="1"></textarea>
      <button class="img-btn" id="attachBtn" title="Bild anhängen">📷</button>
      <button class="send-btn" id="sendBtn">➤</button>
      <input type="file" id="fileInput" accept="image/*" hidden>
    </div>
  </div>
</div>

<!-- SETTINGS MODAL -->
<div class="modal" id="settingsModal">
  <div class="modal-content">
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

    <hr style="border-color:var(--border); margin:18px 0">

    <div class="field">
      <label>Schriftgröße</label>
      <input type="range" id="setFontSize" min="13" max="19" value="15">
    </div>

    <div class="field">
      <label>Farben anpassen</label>
      <div class="color-row">
        <div>
          <small style="color:var(--muted)">Hintergrund</small><br>
          <input type="color" id="setBg" value="#050505">
        </div>
        <div>
          <small style="color:var(--muted)">Text</small><br>
          <input type="color" id="setText" value="#f0f0f0">
        </div>
        <div>
          <small style="color:var(--muted)">Akzent (Gold)</small><br>
          <input type="color" id="setAccent" value="#d4af37">
        </div>
      </div>
    </div>

    <div class="btn-row">
      <button class="btn-secondary" id="resetTheme">Standard</button>
      <button class="btn-primary" id="saveSettings">Speichern</button>
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
    text: "#f0f0f0",
    accent: "#d4af37"
  }
};

let pendingImage = null;

function uid() { return Date.now().toString(36) + Math.random().toString(36).slice(2,7); }

function load() {
  try {
    const raw = localStorage.getItem("padai_v4");
    if (raw) state = JSON.parse(raw);
  } catch(e) {}
  applyTheme();
  if (state.chats.length === 0) createNewChat();
  else if (!state.currentId) state.currentId = state.chats[0].id;
  renderChatList();
  renderCurrentChat();
}

function save() {
  localStorage.setItem("padai_v4", JSON.stringify(state));
}

function applyTheme() {
  const s = state.settings;
  document.documentElement.style.setProperty("--bg", s.bg);
  document.documentElement.style.setProperty("--panel", s.bg);
  document.documentElement.style.setProperty("--sidebar", s.bg);
  document.documentElement.style.setProperty("--text", s.text);
  document.documentElement.style.setProperty("--accent", s.accent);
  document.documentElement.style.setProperty("--accent-hover", s.accent);
  document.documentElement.style.setProperty("--font-size", s.fontSize + "px");
  $("setFontSize").value = s.fontSize;
  $("setBg").value = s.bg;
  $("setText").value = s.text;
  $("setAccent").value = s.accent;
  $("setEndpoint").value = s.endpoint;
  $("setModel").value = s.model;
  $("setApiKey").value = s.apiKey || "";
}

function createNewChat() {
  const chat = {
    id: uid(),
    title: "Neue Unterhaltung",
    messages: [],
    created: Date.now()
  };
  state.chats.unshift(chat);
  state.currentId = chat.id;
  save();
  renderChatList();
  renderCurrentChat();
}

function getCurrentChat() {
  return state.chats.find(c => c.id === state.currentId);
}

function renderChatList() {
  const list = $("chatList");
  list.innerHTML = "";
  state.chats.forEach(chat => {
    const el = document.createElement("div");
    el.className = "chat-item" + (chat.id === state.currentId ? " active" : "");
    el.textContent = chat.title || "Neue Unterhaltung";
    el.onclick = () => {
      state.currentId = chat.id;
      save();
      renderChatList();
      renderCurrentChat();
    };
    list.appendChild(el);
  });
}

function renderCurrentChat() {
  const chat = getCurrentChat();
  if (!chat) return;
  $("currentTitle").textContent = chat.title;
  const area = $("chatArea");
  area.innerHTML = "";
  if (chat.messages.length === 0) {
    const welcome = document.createElement("div");
    welcome.className = "msg ai";
    welcome.innerHTML = `<div class="role">PAD AI</div>Hey! Ich bin PAD AI by Pad to Home Studios. Frag mich etwas oder häng ein Bild an.`;
    area.appendChild(welcome);
  } else {
    chat.messages.forEach(m => {
      const el = document.createElement("div");
      el.className = "msg " + (m.role === "user" ? "user" : "ai");
      el.innerHTML = `<div class="role">${m.role === "user" ? "Du" : "PAD AI"}</div>${escapeHtml(m.content)}`;
      area.appendChild(el);
    });
  }
  area.scrollTop = area.scrollHeight;
}

function escapeHtml(text) {
  if (typeof text !== "string") return "";
  return text.replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;");
}

async function sendMessage() {
  const text = $("userInput").value.trim();
  if (!text && !pendingImage) return;

  const chat = getCurrentChat();
  if (!chat) return;

  const display = text || "(Bild)";
  chat.messages.push({ role: "user", content: display });
  if (chat.title === "Neue Unterhaltung" && text) {
    chat.title = text.slice(0, 32) + (text.length > 32 ? "…" : "");
  }
  $("userInput").value = "";
  renderCurrentChat();
  renderChatList();

  // Build content for API
  let apiContent;
  if (pendingImage) {
    apiContent = [];
    if (text) apiContent.push({ type: "text", text });
    apiContent.push({ type: "image_url", image_url: { url: pendingImage } });
  } else {
    apiContent = text;
  }

  // We keep a simple history of last messages for the API
  const apiMessages = chat.messages.slice(-12).map(m => ({
    role: m.role === "user" ? "user" : "assistant",
    content: m.content
  }));
  // Replace last user message with proper multimodal if needed
  if (pendingImage) {
    apiMessages[apiMessages.length - 1].content = apiContent;
  }

  pendingImage = null;
  $("imagePreviewBox").style.display = "none";
  $("sendBtn").disabled = true;

  try {
    const headers = { "Content-Type": "application/json" };
    if (state.settings.apiKey) headers["Authorization"] = "Bearer " + state.settings.apiKey;

    const res = await fetch(state.settings.endpoint, {
      method: "POST",
      headers,
      body: JSON.stringify({
        model: state.settings.model,
        messages: apiMessages,
        stream: false
      })
    });

    if (!res.ok) {
      const errText = await res.text();
      throw new Error("HTTP " + res.status + " – " + errText.slice(0, 180));
    }
    const data = await res.json();
    const answer = data.choices?.[0]?.message?.content || "Keine Antwort erhalten.";
    chat.messages.push({ role: "assistant", content: answer });
  } catch (err) {
    chat.messages.push({ role: "assistant", content: "Fehler: " + err.message });
  }

  $("sendBtn").disabled = false;
  save();
  renderCurrentChat();
  renderChatList();
}

// Events
$("newChat").onclick = createNewChat;
$("toggleSidebar").onclick = () => $("sidebar").classList.toggle("collapsed");
$("sendBtn").onclick = sendMessage;
$("userInput").addEventListener("keydown", e => {
  if (e.key === "Enter" && !e.shiftKey) { e.preventDefault(); sendMessage(); }
});

$("attachBtn").onclick = () => $("fileInput").click();
$("fileInput").onchange = e => {
  const file = e.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = ev => {
    pendingImage = ev.target.result;
    $("previewImg").src = pendingImage;
    $("imagePreviewBox").style.display = "block";
  };
  reader.readAsDataURL(file);
};
$("removeImage").onclick = () => {
  pendingImage = null;
  $("imagePreviewBox").style.display = "none";
  $("fileInput").value = "";
};

$("openSettings").onclick = () => $("settingsModal").classList.add("open");
$("settingsModal").onclick = e => { if (e.target === $("settingsModal")) $("settingsModal").classList.remove("open"); };

$("saveSettings").onclick = () => {
  state.settings.endpoint = $("setEndpoint").value.trim();
  state.settings.model = $("setModel").value.trim();
  state.settings.apiKey = $("setApiKey").value.trim();
  state.settings.fontSize = parseInt($("setFontSize").value);
  state.settings.bg = $("setBg").value;
  state.settings.text = $("setText").value;
  state.settings.accent = $("setAccent").value;
  applyTheme();
  save();
  $("settingsModal").classList.remove("open");
};

$("resetTheme").onclick = () => {
  $("setBg").value = "#050505";
  $("setText").value = "#f0f0f0";
  $("setAccent").value = "#d4af37";
  $("setFontSize").value = 15;
};

$("setFontSize").oninput = () => {
  document.documentElement.style.setProperty("--font-size", $("setFontSize").value + "px");
};

load();
</script>
</body>
</html>