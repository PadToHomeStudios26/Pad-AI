<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
<title>PAD AI – by Pad to Home Studios</title>
<style>
:root {
  --bg: #0b1020;
  --panel: #151d30;
  --text: #edf3ff;
  --muted: #9eacc7;
  --accent: #d4af37;
  --border: #2a3855;
  --user: #263b68;
  --ai: #202c43;
}
* { box-sizing: border-box; margin: 0; padding: 0; }
body {
  background: var(--bg);
  color: var(--text);
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", system-ui, sans-serif;
  font-size: 15px;
  min-height: 100vh;
  padding: 12px;
  padding-bottom: 70px;
}
.container { max-width: 820px; margin: 0 auto; }
header { margin-bottom: 14px; }
header h1 { font-size: 22px; color: var(--accent); }
header p { color: var(--muted); font-size: 13px; }

.layout {
  display: flex;
  gap: 12px;
  min-height: 65vh;
}
.sidebar {
  width: 180px;
  background: var(--panel);
  border: 1px solid var(--border);
  border-radius: 14px;
  padding: 10px;
  display: flex;
  flex-direction: column;
  flex-shrink: 0;
}
.new-btn {
  width: 100%;
  background: var(--accent);
  color: #000;
  border: none;
  padding: 10px;
  border-radius: 9px;
  font-weight: 600;
  cursor: pointer;
  margin-bottom: 10px;
}
.chat-list { flex: 1; overflow-y: auto; }
.chat-item {
  padding: 9px 10px;
  border-radius: 8px;
  font-size: 13px;
  color: var(--muted);
  cursor: pointer;
  margin-bottom: 3px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.chat-item:hover, .chat-item.active {
  background: #1b2740;
  color: var(--text);
}

.main {
  flex: 1;
  background: var(--panel);
  border: 1px solid var(--border);
  border-radius: 14px;
  padding: 12px;
  display: flex;
  flex-direction: column;
  min-width: 0;
}
.chat {
  flex: 1;
  overflow-y: auto;
  background: #0d1526;
  border: 1px solid var(--border);
  border-radius: 11px;
  padding: 12px;
  margin-bottom: 10px;
  min-height: 250px;
}
.msg {
  padding: 10px 12px;
  border-radius: 11px;
  margin: 8px 0;
  max-width: 92%;
  position: relative;
  user-select: text;
}
.msg.user { background: var(--user); margin-left: auto; }
.msg.ai { background: var(--ai); }
.msg-content {
  white-space: pre-wrap;
  overflow-wrap: anywhere;
}
.msg img {
  max-width: 100%;
  max-height: 220px;
  border-radius: 9px;
  margin-top: 6px;
  display: block;
}

/* Code Block */
.code-block {
  background: #0a0f1a;
  border: 1px solid var(--border);
  border-radius: 8px;
  margin: 8px 0;
  overflow: hidden;
}
.code-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 6px 10px;
  background: #121a2b;
  font-size: 12px;
  color: var(--muted);
}
.code-header button {
  background: var(--accent);
  color: #000;
  border: none;
  padding: 4px 10px;
  border-radius: 6px;
  font-size: 12px;
  font-weight: 600;
  cursor: pointer;
}
.code-body {
  padding: 10px;
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;
  font-size: 13px;
  white-space: pre-wrap;
  overflow-x: auto;
  color: #e0e6ff;
}

.copy-msg-btn {
  position: absolute;
  top: 6px;
  right: 6px;
  background: transparent;
  border: 1px solid var(--border);
  color: var(--muted);
  font-size: 11px;
  padding: 3px 7px;
  border-radius: 6px;
  cursor: pointer;
  opacity: 0.7;
}
.copy-msg-btn:hover { opacity: 1; background: #1b2740; }

.input-area { display: flex; flex-direction: column; gap: 8px; }
textarea {
  width: 100%;
  background: #0d1526;
  border: 1px solid var(--border);
  color: var(--text);
  border-radius: 10px;
  padding: 11px;
  font: inherit;
  min-height: 55px;
  resize: vertical;
}
.row { display: flex; gap: 8px; flex-wrap: wrap; }
button {
  border: 1px solid var(--border);
  background: #1b2740;
  color: var(--text);
  padding: 9px 13px;
  border-radius: 9px;
  font-weight: 600;
  cursor: pointer;
}
button.primary {
  background: var(--accent);
  color: #000;
  border-color: transparent;
}
.preview {
  position: relative;
  display: inline-block;
}
.preview img {
  max-height: 90px;
  border-radius: 8px;
}
.preview button {
  position: absolute;
  top: -7px;
  right: -7px;
  background: #e74c3c;
  color: white;
  border: none;
  width: 22px;
  height: 22px;
  border-radius: 50%;
  padding: 0;
}

.settings-btn {
  position: fixed;
  bottom: 18px;
  right: 18px;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background: var(--accent);
  color: #000;
  border: none;
  font-size: 22px;
  cursor: pointer;
  box-shadow: 0 4px 14px rgba(0,0,0,0.4);
  z-index: 50;
}

.modal {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.65);
  display: none;
  align-items: center;
  justify-content: center;
  z-index: 100;
  padding: 16px;
}
.modal.open { display: flex; }
.modal-box {
  background: var(--panel);
  border: 1px solid var(--border);
  border-radius: 16px;
  width: 100%;
  max-width: 400px;
  max-height: 85vh;
  overflow-y: auto;
  padding: 18px;
}
.modal-box h3 { color: var(--accent); margin-bottom: 14px; }
.field { margin-bottom: 12px; }
.field label {
  display: block;
  font-size: 12px;
  color: var(--muted);
  margin-bottom: 4px;
}
.field input {
  width: 100%;
  background: #0d1526;
  border: 1px solid var(--border);
  color: var(--text);
  padding: 10px;
  border-radius: 8px;
  font: inherit;
}
.colors { display: flex; gap: 14px; }
.colors input[type="color"] {
  width: 46px;
  height: 34px;
  border: none;
  background: none;
  padding: 0;
}
.modal-actions { display: flex; gap: 8px; margin-top: 16px; }
.modal-actions button { flex: 1; }
.hint { font-size: 12px; color: var(--muted); margin-top: 10px; }

@media (max-width: 700px) {
  .layout { flex-direction: column; }
  .sidebar { width: 100%; max-height: 140px; }
  .chat-list { display: flex; gap: 6px; overflow-x: auto; }
  .chat-item { flex-shrink: 0; max-width: 130px; }
}
</style>
</head>
<body>
<div class="container">
  <header>
    <h1>PAD AI</h1>
    <p>by Pad to Home Studios</p>
  </header>

  <div class="layout">
    <div class="sidebar">
      <button class="new-btn" id="newChat">＋ Neuer Chat</button>
      <div class="chat-list" id="chatList"></div>
    </div>

    <div class="main">
      <div class="chat" id="chatLog"></div>
      <div class="input-area">
        <div class="preview" id="previewBox" style="display:none">
          <img id="previewImg" alt="Vorschau">
          <button id="removeImg">×</button>
        </div>
        <textarea id="userInput" placeholder="Nachricht schreiben..."></textarea>
        <div class="row">
          <button class="primary" id="sendBtn">Senden</button>
          <button id="attachBtn">📷 Bild</button>
          <button id="clearBtn">Chat leeren</button>
          <input type="file" id="fileInput" accept="image/*" hidden>
        </div>
      </div>
    </div>
  </div>
</div>

<button class="settings-btn" id="openSettings">⚙️</button>

<div class="modal" id="settingsModal">
  <div class="modal-box">
    <h3>Einstellungen</h3>
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
      <input id="setApiKey" type="password" placeholder="Dein Groq Key">
    </div>
    <div class="field">
      <label>Farben</label>
      <div class="colors">
        <div>
          <small style="color:var(--muted)">Hintergrund</small><br>
          <input type="color" id="setBg" value="#0b1020">
        </div>
        <div>
          <small style="color:var(--muted)">Text</small><br>
          <input type="color" id="setText" value="#edf3ff">
        </div>
        <div>
          <small style="color:var(--muted)">Akzent</small><br>
          <input type="color" id="setAccent" value="#d4af37">
        </div>
      </div>
    </div>
    <div class="modal-actions">
      <button id="resetBtn">Farben zurück</button>
      <button class="primary" id="saveBtn">Speichern</button>
    </div>
    <p class="hint">Chats, API-Schlüssel und Farben werden lokal gespeichert.</p>
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
    bg: "#0b1020",
    text: "#edf3ff",
    accent: "#d4af37"
  }
};
let pendingImage = null;

function uid() {
  return Date.now().toString(36) + Math.random().toString(36).slice(2, 6);
}

function load() {
  try {
    const raw = localStorage.getItem("padai_v10");
    if (raw) state = JSON.parse(raw);
  } catch(e) {}
  applyColors();
  if (state.chats.length === 0) createChat();
  else if (!state.currentId) state.currentId = state.chats[0].id;
  renderList();
  renderChat();
}

function save() {
  localStorage.setItem("padai_v10", JSON.stringify(state));
}

function applyColors() {
  const s = state.settings;
  document.documentElement.style.setProperty("--bg", s.bg);
  document.documentElement.style.setProperty("--text", s.text);
  document.documentElement.style.setProperty("--accent", s.accent);
  document.body.style.background = s.bg;
  $("setEndpoint").value = s.endpoint;
  $("setModel").value = s.model;
  $("setApiKey").value = s.apiKey || "";
  $("setBg").value = s.bg;
  $("setText").value = s.text;
  $("setAccent").value = s.accent;
}

function createChat() {
  const chat = { id: uid(), title: "Neuer Chat", messages: [] };
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
    el.textContent = c.title || "Neuer Chat";
    el.onclick = () => {
      state.currentId = c.id;
      save();
      renderList();
      renderChat();
    };
    list.appendChild(el);
  });
}

function copyText(text) {
  navigator.clipboard.writeText(text).then(() => {
    // kurzes Feedback
  }).catch(() => {
    // Fallback
    const ta = document.createElement("textarea");
    ta.value = text;
    document.body.appendChild(ta);
    ta.select();
    document.execCommand("copy");
    document.body.removeChild(ta);
  });
}

function formatContent(text) {
  // Einfache Erkennung von Code-Blöcken mit ``` 
  if (!text.includes("```")) {
    return `<div class="msg-content">${escapeHtml(text)}</div>`;
  }

  let html = "";
  const parts = text.split("```");
  parts.forEach((part, i) => {
    if (i % 2 === 0) {
      // normaler Text
      if (part.trim()) html += `<div class="msg-content">${escapeHtml(part)}</div>`;
    } else {
      // Code
      let code = part;
      let lang = "";
      const firstLine = part.split("\n")[0];
      if (firstLine && !firstLine.includes(" ") && firstLine.length < 20) {
        lang = firstLine;
        code = part.slice(firstLine.length).replace(/^\n/, "");
      }
      const id = "code-" + Math.random().toString(36).slice(2, 8);
      html += `
        <div class="code-block">
          <div class="code-header">
            <span>${lang || "Code"}</span>
            <button onclick="copyText(document.getElementById('${id}').innerText)">Kopieren</button>
          </div>
          <div class="code-body" id="${id}">${escapeHtml(code.trim())}</div>
        </div>`;
    }
  });
  return html;
}

function escapeHtml(str) {
  return (str || "")
    .replace(/&/g, "&amp;")
    .replace(/</g, "&lt;")
    .replace(/>/g, "&gt;");
}

function renderChat() {
  const chat = currentChat();
  if (!chat) return;
  const log = $("chatLog");
  log.innerHTML = "";

  if (chat.messages.length === 0) {
    const el = document.createElement("div");
    el.className = "msg ai";
    el.innerHTML = `<div class="msg-content">Hey! Ich bin PAD AI by Pad to Home Studios.\nSchreib mir etwas oder häng ein Bild an.</div>`;
    log.appendChild(el);
  } else {
    chat.messages.forEach(m => {
      const el = document.createElement("div");
      el.className = "msg " + m.role;

      if (m.role === "ai") {
        el.innerHTML = formatContent(m.text || "");
        // Kopieren-Button für die ganze Nachricht
        const btn = document.createElement("button");
        btn.className = "copy-msg-btn";
        btn.textContent = "Kopieren";
        btn.onclick = (e) => {
          e.stopPropagation();
          copyText(m.text || "");
          btn.textContent = "Kopiert!";
          setTimeout(() => btn.textContent = "Kopieren", 1200);
        };
        el.appendChild(btn);
      } else {
        el.innerHTML = `<div class="msg-content">${escapeHtml(m.text || "")}</div>`;
      }

      if (m.image) {
        const img = document.createElement("img");
        img.src = m.image;
        el.appendChild(img);
      }

      // Doppelklick = ganzen Text markieren
      el.addEventListener("dblclick", () => {
        const range = document.createRange();
        range.selectNodeContents(el);
        const sel = window.getSelection();
        sel.removeAllRanges();
        sel.addRange(range);
      });

      log.appendChild(el);
    });
  }
  log.scrollTop = log.scrollHeight;
}

async function send() {
  const text = $("userInput").value.trim();
  if (!text && !pendingImage) return;
  const chat = currentChat();
  if (!chat) return;

  chat.messages.push({ role: "user", text: text || "", image: pendingImage || null });
  if (chat.title === "Neuer Chat" && text) {
    chat.title = text.slice(0, 26) + (text.length > 26 ? "…" : "");
  }
  $("userInput").value = "";
  pendingImage = null;
  $("previewBox").style.display = "none";
  $("fileInput").value = "";
  renderChat();
  renderList();
  $("sendBtn").disabled = true;

  const apiMessages = chat.messages.map(m => {
    if (m.role === "user" && m.image) {
      const content = [];
      if (m.text) content.push({ type: "text", text: m.text });
      content.push({ type: "image_url", image_url: { url: m.image } });
      return { role: "user", content };
    }
    return { role: m.role === "user" ? "user" : "assistant", content: m.text || "" };
  });

  try {
    const headers = { "Content-Type": "application/json" };
    if (state.settings.apiKey) headers.Authorization = "Bearer " + state.settings.apiKey;
    const res = await fetch(state.settings.endpoint, {
      method: "POST",
      headers,
      body: JSON.stringify({
        model: state.settings.model,
        messages: apiMessages.slice(-12),
        stream: false
      })
    });
    if (!res.ok) {
      const t = await res.text();
      throw new Error("HTTP " + res.status + " – " + t.slice(0, 140));
    }
    const data = await res.json();
    chat.messages.push({ role: "ai", text: data.choices?.[0]?.message?.content || "Keine Antwort." });
  } catch (e) {
    chat.messages.push({ role: "ai", text: "Fehler: " + e.message });
  }
  $("sendBtn").disabled = false;
  save();
  renderChat();
  renderList();
}

$("newChat").onclick = createChat;
$("sendBtn").onclick = send;
$("userInput").addEventListener("keydown", e => {
  if (e.key === "Enter" && !e.shiftKey) { e.preventDefault(); send(); }
});
$("clearBtn").onclick = () => {
  const chat = currentChat();
  if (chat) { chat.messages = []; save(); renderChat(); }
};
$("attachBtn").onclick = () => $("fileInput").click();
$("fileInput").onchange = e => {
  const f = e.target.files[0];
  if (!f) return;
  const r = new FileReader();
  r.onload = ev => {
    pendingImage = ev.target.result;
    $("previewImg").src = pendingImage;
    $("previewBox").style.display = "block";
  };
  r.readAsDataURL(f);
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
$("saveBtn").onclick = () => {
  state.settings.endpoint = $("setEndpoint").value.trim();
  state.settings.model = $("setModel").value.trim();
  state.settings.apiKey = $("setApiKey").value.trim();
  state.settings.bg = $("setBg").value;
  state.settings.text = $("setText").value;
  state.settings.accent = $("setAccent").value;
  applyColors();
  save();
  $("settingsModal").classList.remove("open");
};
$("resetBtn").onclick = () => {
  $("setBg").value = "#0b1020";
  $("setText").value = "#edf3ff";
  $("setAccent").value = "#d4af37";
};

load();
</script>
</body>
</html>