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
  padding: 16px;
}
header {
  max-width: 800px;
  margin: 0 auto 16px;
}
header h1 {
  font-size: 24px;
  color: var(--accent);
  margin-bottom: 4px;
}
header p {
  color: var(--muted);
  font-size: 13px;
}
.panel {
  max-width: 800px;
  margin: 0 auto;
  background: var(--panel);
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 16px;
}
.chat {
  height: 50vh;
  min-height: 280px;
  overflow-y: auto;
  background: #0d1526;
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 12px;
  margin-bottom: 12px;
}
.msg {
  padding: 10px 12px;
  border-radius: 12px;
  margin: 8px 0;
  max-width: 90%;
  white-space: pre-wrap;
  overflow-wrap: anywhere;
}
.msg.user {
  background: var(--user);
  margin-left: auto;
}
.msg.ai {
  background: var(--ai);
}
.msg img {
  max-width: 100%;
  max-height: 220px;
  border-radius: 10px;
  margin-top: 6px;
  display: block;
}
.input-area {
  display: flex;
  flex-direction: column;
  gap: 8px;
}
textarea {
  width: 100%;
  background: #0d1526;
  border: 1px solid var(--border);
  color: var(--text);
  border-radius: 10px;
  padding: 12px;
  font: inherit;
  min-height: 70px;
  resize: vertical;
}
.row {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}
button {
  border: 1px solid var(--border);
  background: #1b2740;
  color: var(--text);
  padding: 10px 14px;
  border-radius: 10px;
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
  margin-bottom: 6px;
}
.preview img {
  max-height: 100px;
  border-radius: 8px;
}
.preview button {
  position: absolute;
  top: -8px;
  right: -8px;
  background: #e74c3c;
  color: white;
  border: none;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  padding: 0;
}
.settings {
  margin-top: 20px;
  padding-top: 16px;
  border-top: 1px solid var(--border);
}
.field {
  margin-bottom: 12px;
}
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
.colors {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
}
.colors > div {
  text-align: center;
}
.colors input[type="color"] {
  width: 48px;
  height: 36px;
  border: none;
  background: none;
  padding: 0;
}
.hint {
  font-size: 12px;
  color: var(--muted);
  margin-top: 8px;
}
</style>
</head>
<body>
<header>
  <h1>PAD AI</h1>
  <p>by Pad to Home Studios</p>
</header>

<div class="panel">
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

  <div class="settings">
    <h3 style="margin-bottom:12px;color:var(--accent)">Einstellungen</h3>

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
      <label>Farben anpassen</label>
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

    <div class="row" style="margin-top:12px">
      <button class="primary" id="saveBtn">Speichern</button>
      <button id="resetBtn">Farben zurücksetzen</button>
    </div>
    <p class="hint">API-Schlüssel und Farben werden lokal gespeichert.</p>
  </div>
</div>

<script>
const $ = id => document.getElementById(id);
let messages = [];
let pendingImage = null;

const defaults = {
  endpoint: "https://api.groq.com/openai/v1/chat/completions",
  model: "qwen/qwen3.8-27b",
  apiKey: "",
  bg: "#0b1020",
  text: "#edf3ff",
  accent: "#d4af37"
};

function load() {
  try {
    const raw = localStorage.getItem("padai_stable");
    if (raw) {
      const data = JSON.parse(raw);
      if (data.endpoint) $("setEndpoint").value = data.endpoint;
      if (data.model) $("setModel").value = data.model;
      if (data.apiKey) $("setApiKey").value = data.apiKey;
      if (data.bg) $("setBg").value = data.bg;
      if (data.text) $("setText").value = data.text;
      if (data.accent) $("setAccent").value = data.accent;
    }
  } catch(e) {}
  applyColors();
  addMsg("ai", "Hey! Ich bin PAD AI by Pad to Home Studios.\nSchreib mir etwas oder häng ein Bild an.");
}

function save() {
  const data = {
    endpoint: $("setEndpoint").value.trim(),
    model: $("setModel").value.trim(),
    apiKey: $("setApiKey").value.trim(),
    bg: $("setBg").value,
    text: $("setText").value,
    accent: $("setAccent").value
  };
  localStorage.setItem("padai_stable", JSON.stringify(data));
}

function applyColors() {
  document.documentElement.style.setProperty("--bg", $("setBg").value);
  document.documentElement.style.setProperty("--text", $("setText").value);
  document.documentElement.style.setProperty("--accent", $("setAccent").value);
  document.body.style.background = $("setBg").value;
}

function addMsg(role, text, image) {
  const el = document.createElement("div");
  el.className = "msg " + role;
  el.textContent = text;
  if (image) {
    const img = document.createElement("img");
    img.src = image;
    el.appendChild(img);
  }
  $("chatLog").appendChild(el);
  $("chatLog").scrollTop = $("chatLog").scrollHeight;
}

async function send() {
  const text = $("userInput").value.trim();
  if (!text && !pendingImage) return;

  addMsg("user", text || "(Bild)", pendingImage);
  messages.push({
    role: "user",
    content: pendingImage
      ? [{ type: "text", text: text || "" }, { type: "image_url", image_url: { url: pendingImage } }]
      : text
  });

  $("userInput").value = "";
  const img = pendingImage;
  pendingImage = null;
  $("previewBox").style.display = "none";
  $("fileInput").value = "";
  $("sendBtn").disabled = true;

  try {
    const headers = { "Content-Type": "application/json" };
    const key = $("setApiKey").value.trim();
    if (key) headers.Authorization = "Bearer " + key;

    const res = await fetch($("setEndpoint").value.trim(), {
      method: "POST",
      headers,
      body: JSON.stringify({
        model: $("setModel").value.trim() || "qwen/qwen3.8-27b",
        messages: messages.slice(-12),
        stream: false
      })
    });

    if (!res.ok) {
      const t = await res.text();
      throw new Error("HTTP " + res.status + " – " + t.slice(0, 150));
    }
    const data = await res.json();
    const answer = data.choices?.[0]?.message?.content || "Keine Antwort.";
    messages.push({ role: "assistant", content: answer });
    addMsg("ai", answer);
  } catch (e) {
    addMsg("ai", "Fehler: " + e.message);
  }
  $("sendBtn").disabled = false;
}

$("sendBtn").onclick = send;
$("userInput").addEventListener("keydown", e => {
  if (e.key === "Enter" && !e.shiftKey) {
    e.preventDefault();
    send();
  }
});
$("clearBtn").onclick = () => {
  $("chatLog").innerHTML = "";
  messages = [];
  addMsg("ai", "Chat geleert.");
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
$("saveBtn").onclick = () => {
  save();
  applyColors();
  alert("Gespeichert!");
};
$("resetBtn").onclick = () => {
  $("setBg").value = defaults.bg;
  $("setText").value = defaults.text;
  $("setAccent").value = defaults.accent;
  applyColors();
};
$("setBg").oninput = applyColors;
$("setText").oninput = applyColors;
$("setAccent").oninput = applyColors;

load();
</script>
</body>
</html>