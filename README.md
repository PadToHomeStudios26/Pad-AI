<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
<title>PAD AI – by Pad to Home Studios</title>
<style>
  :root {
    --bg: #050505;
    --panel: #0c0c0c;
    --sidebar: #080808;
    --text: #f0f0f0;
    --muted: #999;
    --accent: #d4af37;
    --accent-hover: #e8c547;
    --border: #1c1c1c;
    --user-bg: #1a1a1a;
    --ai-bg: #111;
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

  /* ===== SIDEBAR ===== */
  #sidebar {
    width: 280px;
    background: var(--sidebar);
    border-right: 1px solid var(--border);
    display: flex;
    flex-direction: column;
    transition: margin-left 0.25s ease;
    z-index: 50;
  }
  #sidebar.collapsed { margin-left: -280px; }

  .sidebar-header {
    padding: 18px 16px 14px;
    border-bottom: 1px solid var(--border);
  }
  .brand {
    font-weight: 700;
    font-size: 18px;
    color: var(--accent);
    letter-spacing: 0.3px;
  }
  .brand span {
    display: block;
    font-size: 11px;
    color: var(--muted);
    font-weight: 400;
    margin-top: 2px;
  }

  .new-btn {
    margin: 12px;
    background: var(--accent);
    color: #000;
    border: none;
    padding: 12px;
    border-radius: 10px;
    font-weight: 600;
    cursor: pointer;
    width: calc(100% - 24px);
    font-size: 14px;
  }
  .new-btn:hover { background: var(--accent-hover); }

  .chat-list {
    flex: 1;
    overflow-y: auto;
    padding: 6px 8px;
  }
  .chat-item {
    padding: 11px 12px;
    border-radius: 9px;
    cursor: pointer;
    margin-bottom: 3px;
    font-size: 13.5px;
    color: var(--muted);
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .chat-item:hover { background: #151515; color: var(--text); }
  .chat-item.active {
    background: #161616;
    color: var(--text);
    border-left: 3px solid var(--accent);
  }

  .sidebar-footer {
    padding: 10px;
    border-top: 1px solid var(--border);
  }
  .sidebar-footer button {
    width: 100%;
    background: transparent;
    border: 1px solid var(--border);
    color: var(--text);
    padding: 10px;
    border-radius: 8px;
    cursor: pointer;
    font-size: 13px;
  }

  /* ===== MAIN ===== */
  #main {
    flex: 1;
    display: flex;
    flex-direction: column;
    min-width: 0;
  }

  .topbar {
    height: 54px;
    border-bottom: 1px solid var(--border);
    display: flex;
    align-items: center;
    padding: 0 14px;
    gap: 12px;
    background: var(--panel);
  }
  .toggle-btn {
    background: transparent;
    border: 1px solid var(--border);
    color: var(--text);
    width: 36px;
    height: 36px;
    border-radius: 8px;
    cursor: pointer;
    font-size: 18px;
  }
  .title {
    font-weight: 600;
    flex: 1;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
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
    max-width: 760px;
    padding: 12px 16px;
    border-radius: 14px;
    line-height: 1.55;
  }
  .msg.user {
    background: var(--user-bg);
    align-self: flex-end;
    border: 1px solid var(--border);
  }
  .msg.ai {
    background: var(--ai-bg);
    align-self: flex-start;
    border: 1px solid var(--border);
  }
  .msg .role {
    font-size: 11px;
    color: var(--accent);
    font-weight: 600;
    margin-bottom: 5px;
  }
  .msg .content {
    white-space: pre-wrap;
    overflow-wrap: anywhere;
  }
  .msg img.chat-img {
    max-width: 100%;
    max-height: 280px;
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
  .preview-box {
    margin-bottom: 10px;
    position: relative;
    display: inline-block;
  }
  .preview-box img {
    max-height: 110px;
    border-radius: 10px;
    border: 1px solid var(--border);
  }
  .preview-box button {
    position: absolute;
    top: -8px;
    right: -8px;
    background: #e74c3c;
    color: white;
    border: none;
    width: 24px;
    height: 24px;
    border-radius: 50%;
    cursor: pointer;
    font-size: 14px;
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
    padding: 12px 14px;
    resize: none;
    min-height: 46px;
    max-height: 130px;
    font: inherit;
  }
  .icon-btn {
    width: 46px;
    height: 46px;
    border-radius: 12px;
    border: none;
    cursor: pointer;
    font-size: 18px;
    flex-shrink: 0;
  }
  .attach-btn {
    background: #1a1a1a;
    color: var(--text);
    border: 1px solid var(--border);
  }
  .send-btn {
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
    border-radius: 16px;
    width: 92%;
    max-width: 400px;
    max-height: 85vh;
    overflow-y: auto;
    padding: 20px;
  }
  .modal-box h2 {
    color: var(--accent);
    margin-bottom: 16px;
    font-size: 18px;
  }
  .field { margin-bottom: 14px; }
  .field label {
    display: block;
    font-size: 12px;
    color: var(--muted);
    margin-bottom: 5px;
  }
  .field input, .field select {
    width: 100%;
    background: #111;
    border: 1px solid var(--border);
    color: var(--text);
    padding: 10px 12px;
    border-radius: 8px;
    font: inherit;
  }
  .colors {
    display: flex;
    gap: 12px;
  }
  .colors input[type="color"] {
    width: 48px;
    height: 36px;
    border: none;
    background: none;
    padding: 0;
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
  .save-btn { background: var(--accent); color: #000; }
  .reset-btn { background: #222; color: var(--text); border: 1px solid var(--border); }
  .hint { font-size: 12px; color: var(--muted); margin-top: 10px; line-height: 1.4; }
</style>
</head>
<body>

<aside id="sidebar">
  <div class="sidebar-header">
    <div class="brand">
      PAD AI
      <span>by Pad to Home Studios</span>
    </div>
  </div>
  <button class="new-btn" id="newChat">＋ Neue Unterhaltung</button>
  <div class="chat-list" id="chatList"></div>
  <div class="sidebar-footer">
    <button id="openSettings">⚙️ Einstellungen</button>
  </div>
</aside>

<div id="main">
  <div class="topbar">
    <button class="toggle-btn" id="toggleSidebar">☰</button>
    <div class="title" id="chatTitle">Neue Unterhaltung</div>
  </div>

  <div id="chatArea"></div>

  <div class="input-area">
    <div class="preview-box" id="previewBox" style="display:none">
      <img id="previewImg" alt="Vorschau">
      <button id="removeImg">×</button>
    </div>
    <div class="input-row">
      <textarea id="userInput" placeholder="Nachricht schreiben..." rows="1"></textarea>
      <button class="icon-btn attach-btn" id="attachBtn">📷</button>
      <button class="icon-btn send-btn" id="sendBtn">➤</button>
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

    <div style="height:1px;background:var(--border);margin:16px 0"></div>

    <div class="field">
      <label>Schriftgröße</label>
      <input type="range" id="setFont" min="13" max="19" value="15">
    </div>

    <div class="field">
      <label>Farben</label>
      <div class="colors">
        <div>
          <small style="color:var(--muted)">Hintergrund</small><br>
          <input type="color" id="setBg" value="#050505">
        </div>
        <div>
          <small style="color:var(--muted)">Text</small><br>
          <input type="color" id="setText" value="#f0f0f0">
        </div>
        <div>
          <small style="color:var(--muted)">Akzent</small><br>
          <input type="color" id="setAccent" value="#d4af37">
        </div>
      </div>
    </div>

    <div class="btn-row">
      <button class="reset-btn" id="resetTheme">Standard</button>
      <button class="save-btn" id="saveSettings">Speichern</button>
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

function uid() {
  return Date.now().toString(36) + Math.random().toString(36).slice(2, 6);
}

function load() {
  try {
    const raw = localStorage.getItem("padai_v5");
    if (raw) state = JSON.parse(raw);
  } catch (e) {}
  applyTheme();
  if (state.chats.length === 0) createChat();
  else if (!state.currentId) state.currentId = state.chats[0].id;
  renderList();
  renderChat();
}

function save() {
  localStorage.setItem("padai_v5", JSON.stringify(state));
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

  $("setEndpoint").value = s.endpoint;
  $("setModel").value = s.model;
  $("setApiKey").value = s.apiKey || "";
  $("setFont").value = s.fontSize;
  $("setBg").value = s.bg;
  $("setText").value = s.text;
  $("setAccent").value = s.accent;
}

function createChat() {
  const chat = {
    id: uid(),
    title: "Neue Unterhaltung",
    messages: [],
    created: Date.now()
  };
  state.chats.unshift(chat);
  state.currentId = chat.id;
  save();
  renderList();
  renderChat();
}

function currentChat() {
  return state.chats.find(c => c.id === state.currentId);
}

function renderList() {
  const list = $("chatList");
  list.innerHTML = "";
  state.chats.forEach(c => {
    const el = document.createElement("div");
    el.className = "chat-item" + (c.id === state.currentId ? " active" : "");
    el.textContent = c.title || "Neue Unterhaltung";
    el.onclick = () => {
      state.currentId = c.id;
      save();
      renderList();
      renderChat();
    };
    list.appendChild(el);
  });
}

function escapeHtml(str) {
  if (typeof str !== "string") return "";
  return str.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;");
}

function renderChat() {
  const chat = currentChat();
  if (!chat) return;
  $("chatTitle").textContent = chat.title;
  const area = $("chatArea");
  area.innerHTML = "";

  if (chat.messages.length === 0) {
    const welcome = document.createElement("div");
    welcome.className = "msg ai";
    welcome.innerHTML = `<div class="role">PAD AI</div><div class="content">Hey! Ich bin PAD AI by Pad to Home Studios.\nSchreib mir etwas oder häng ein Bild an.</div>`;
    area.appendChild(welcome);
  } else {
    chat.messages.forEach(m => {
      const el = document.createElement("div");
      el.className = "msg " + (m.role === "user" ? "user" : "ai");
      let html = `<div class="role">${m.role === "user" ? "Du" : "PAD AI"}</div>`;
      html += `<div class="content">${escapeHtml(m.text || "")}</div>`;
      if (m.image) {
        html += `<img class="chat-img" src="${m.image}" alt="Bild">`;
      }
      el.innerHTML = html;
      area.appendChild(el);
    });
  }
  area.scrollTop = area.scrollHeight;
}

async function send() {
  const text = $("userInput").value.trim();
  if (!text && !pendingImage) return;

  const chat = currentChat();
  if (!chat) return;

  // Nachricht speichern (mit Bild falls vorhanden)
  chat.messages.push({
    role: "user",
    text: text || "",
    image: pendingImage || null
  });

  if (chat.title === "Neue Unterhaltung" && text) {
    chat.title = text.slice(0, 34) + (text.length > 34 ? "…" : "");
  }

  $("userInput").value = "";
  const imageToSend = pendingImage;
  pendingImage = null;
  $("previewBox").style.display = "none";
  $("fileInput").value = "";
  renderChat();
  renderList();

  // API-Nachrichten vorbereiten
  const apiMessages = [];
  chat.messages.forEach(m => {
    if (m.role === "user") {
      if (m.image) {
        const content = [];
        if (m.text) content.push({ type: "text", text: m.text });
        content.push({ type: "image_url", image_url: { url: m.image } });
        apiMessages.push({ role: "user", content });
      } else {
        apiMessages.push({ role: "user", content: m.text });
      }
    } else {
      apiMessages.push({ role: "assistant", content: m.text });
    }
  });

  $("sendBtn").disabled = true;

  try {
    const headers = { "Content-Type": "application/json" };
    if (state.settings.apiKey) {
      headers["Authorization"] = "Bearer " + state.settings.apiKey;
    }

    const res = await fetch(state.settings.endpoint, {
      method: "POST",
      headers,
      body: JSON.stringify({
        model: state.settings.model,
        messages: apiMessages.slice(-14),
        stream: false
      })
    });

    if (!res.ok) {
      const t = await res.text();
      throw new Error("HTTP " + res.status + " – " + t.slice(0, 160));
    }

    const data = await res.json();
    const answer = data.choices?.[0]?.message?.content || "Keine Antwort.";
    chat.messages.push({ role: "assistant", text: answer });
  } catch (err) {
    chat.messages.push({ role: "assistant", text: "Fehler: " + err.message });
  }

  $("sendBtn").disabled = false;
  save();
  renderChat();
  renderList();
}

// Events
$("newChat").onclick = createChat;
$("toggleSidebar").onclick = () => $("sidebar").classList.toggle("collapsed");
$("sendBtn").onclick = send;
$("userInput").addEventListener("keydown", e => {
  if (e.key === "Enter" && !e.shiftKey) {
    e.preventDefault();
    send();
  }
});

$("attachBtn").onclick = () => $("fileInput").click();
$("fileInput").onchange = e => {
  const file = e.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = ev => {
    pendingImage = ev.target.result;
    $("previewImg").src = pendingImage;
    $("previewBox").style.display = "block";
  };
  reader.readAsDataURL(file);
};
$("removeImg").onclick = () => {
  pendingImage = null;
  $("previewBox").style.display = "none";
  $("fileInput").value = "";
};

$("openSettings").onclick = () => $("settingsModal").classList.add("open");
$("settingsModal").onclick = e => {
  if (e.target === $("settingsModal")) $("settingsModal").classList.remove("open");
};

$("saveSettings").onclick = () => {
  state.settings.endpoint = $("setEndpoint").value.trim();
  state.settings.model = $("setModel").value.trim();
  state.settings.apiKey = $("setApiKey").value.trim();
  state.settings.fontSize = parseInt($("setFont").value);
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
  $("setFont").value = 15;
};

$("setFont").oninput = () => {
  document.documentElement.style.setProperty("--font-size", $("setFont").value + "px");
};

load();
</script>
</body>
</html>