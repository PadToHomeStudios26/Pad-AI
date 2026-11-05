<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<meta name="theme-color" content="#101522">
<title>Meine eigene KI Plus 3.0</title>
<style>
:root{color-scheme:dark;--bg:#0b1020;--panel:#151d30;--panel2:#1b2740;--line:#2a3855;--text:#edf3ff;--muted:#9eacc7;--accent:#7c9cff;--good:#48d6a0;--bad:#ff7272}
*{box-sizing:border-box}body{margin:0;background:radial-gradient(circle at top,#182747 0,#0b1020 48%);color:var(--text);font:15px system-ui,-apple-system,Segoe UI,sans-serif;min-height:100vh}
header{padding:22px 18px 14px;max-width:1100px;margin:auto}h1{font-size:clamp(25px,5vw,38px);margin:0 0 5px}header p{color:var(--muted);margin:0}
nav{display:flex;gap:8px;flex-wrap:wrap;margin-top:18px}.tab,button{border:1px solid var(--line);background:var(--panel2);color:var(--text);padding:11px 15px;border-radius:12px;font-weight:650;cursor:pointer}.tab.active{background:var(--accent);color:#081126;border-color:transparent}
main{max-width:1100px;margin:auto;padding:8px 18px 40px}.panel{background:rgba(21,29,48,.94);border:1px solid var(--line);border-radius:18px;padding:17px;margin:12px 0;box-shadow:0 14px 40px #0003}.hidden{display:none!important}.muted,.hint{color:var(--muted);font-size:13px;line-height:1.5}.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(min(100%,300px),1fr));gap:12px}.field{display:flex;flex-direction:column;gap:6px;margin:10px 0}.field label{font-size:13px;color:var(--muted)}input,textarea,select{width:100%;background:#0d1526;color:var(--text);border:1px solid var(--line);border-radius:10px;padding:12px;font:inherit}textarea{min-height:95px;resize:vertical}.row{display:flex;gap:8px;flex-wrap:wrap;align-items:center}.row button{flex:0 0 auto}.primary{background:var(--accent);color:#081126;border-color:transparent}.danger{border-color:#743d4a;background:#3a1c2a}.status{padding:10px 12px;border-radius:10px;background:#0d1526;border:1px solid var(--line);margin:10px 0;font-size:13px}.chat{height:330px;overflow:auto;background:#0d1526;border:1px solid var(--line);border-radius:12px;padding:12px}.msg{padding:10px 12px;border-radius:12px;margin:8px 0;max-width:92%;white-space:pre-wrap;overflow-wrap:anywhere}.user{background:#263b68;margin-left:auto}.ai{background:#202c43}.videoWrap{background:#05080f;border-radius:14px;overflow:hidden;min-height:180px;display:grid;place-items:center;position:relative}video{width:100%;max-height:420px;object-fit:contain}#remoteAudio{display:none}.pill{display:inline-block;padding:4px 9px;border-radius:99px;background:#263653;color:#cbd8f5;font-size:12px}.foot{margin-top:18px;color:var(--muted);font-size:12px}button:disabled{opacity:.45;cursor:not-allowed}
.preset{display:flex;gap:8px;flex-wrap:wrap;margin:8px 0}.preset button{font-size:13px;padding:8px 12px}
.warning{background:#3a1c2a;border:1px solid #743d4a;color:#ffb4b4;padding:10px 12px;border-radius:10px;font-size:13px;margin:10px 0;line-height:1.45}
.image-preview-box{margin:10px 0;position:relative;display:inline-block}
.image-preview-box img{max-width:180px;max-height:140px;border-radius:12px;border:1px solid var(--line)}
.image-preview-box .remove-img{position:absolute;top:-8px;right:-8px;background:#ff7272;color:#000;border:none;border-radius:50%;width:26px;height:26px;font-size:16px;cursor:pointer;display:flex;align-items:center;justify-content:center}
@media(max-width:560px){header{padding-top:17px}.panel{padding:13px}.tab{flex:1;padding:10px 8px}}
</style>
</head>
<body>
<header>
  <h1>Meine eigene KI <span class="pill">PLUS 3.0</span></h1>
  <p>Dein persönliches KI-Studio – optimiert für iPad, iPhone und Computer.</p>
  <nav>
    <button class="tab active" data-tab="chat">💬 Chat</button>
    <button class="tab" data-tab="live">🎙️ Live-Modus</button>
    <button class="tab" data-tab="files">📁 Dateien & Kamera</button>
    <button class="tab" data-tab="settings">⚙️ Einstellungen</button>
  </nav>
</header>
<main>
<section id="chat" class="view">
 <div class="panel">
  <h2>Chat mit deiner KI</h2>
  <div class="row">
   <span class="pill">Online-API oder Ollama</span>
   <span class="pill">Bilder möglich</span>
  </div>
  <div id="chatLog" class="chat" aria-live="polite">
    <div class="msg ai">Hey! Ich bin deine eigene KI. Du kannst jetzt auch Bilder in den Chat schicken.</div>
  </div>

  <div id="chatImagePreview" class="image-preview-box hidden">
    <img id="chatImageImg" alt="Vorschau">
    <button class="remove-img" id="removeChatImage">×</button>
  </div>

  <div class="field">
    <label for="chatInput">Deine Nachricht</label>
    <textarea id="chatInput" placeholder="Was möchtest du wissen? (Bild optional anhängen)"></textarea>
  </div>
  <div class="row">
    <button class="primary" id="sendChat">Senden</button>
    <button id="attachImage">📷 Bild anhängen</button>
    <button id="clearChat">Chat leeren</button>
    <input type="file" id="chatImageInput" accept="image/*" class="hidden">
  </div>
  <p class="hint">Auf dem iPad am besten Online-API nutzen. Manche Modelle können Bilder verstehen (Vision).</p>
 </div>
</section>

<section id="live" class="view hidden">
 <div class="panel">
  <h2>🎙️ Echtzeit-Sprachgespräch</h2>
  <p class="muted">Starte eine Unterhaltung mit Sprache. Optional Kamera nutzen.</p>
  <div class="status" id="liveStatus">Status: Nicht verbunden</div>
  <div class="grid">
   <div>
    <div class="field">
      <label for="ephemeralEndpoint">Token-Endpunkt</label>
      <input id="ephemeralEndpoint" placeholder="https://dein-server.de/realtime-token">
    </div>
    <div class="field">
      <label for="realtimeModel">Realtime-Modell</label>
      <input id="realtimeModel" value="gpt-realtime">
    </div>
    <div class="field">
      <label for="voiceChoice">Stimme</label>
      <select id="voiceChoice">
        <option value="marin">Marin</option>
        <option value="cedar">Cedar</option>
        <option value="alloy">Alloy</option>
        <option value="verse">Verse</option>
        <option value="ash">Ash</option>
        <option value="ballad">Ballad</option>
      </select>
    </div>
    <div class="field">
      <label for="instructions">Anweisungen</label>
      <textarea id="instructions">Du bist meine persönliche KI. Antworte auf Deutsch, freundlich und klar.</textarea>
    </div>
    <div class="row">
      <button class="primary" id="startLive">Live starten</button>
      <button class="danger" id="stopLive" disabled>Beenden</button>
    </div>
   </div>
   <div>
    <div class="videoWrap">
      <video id="liveVideo" autoplay playsinline muted></video>
      <span id="videoPlaceholder" class="muted">Kamera aus</span>
    </div>
    <div class="row" style="margin-top:10px">
      <button id="cameraOn">Kamera an</button>
      <button id="cameraOff" disabled>Kamera aus</button>
      <button id="sendFrame" disabled>Bild senden</button>
    </div>
   </div>
  </div>
  <audio id="remoteAudio" autoplay></audio>
  <div class="field">
    <label>Live-Log</label>
    <div id="liveLog" class="chat" style="height:180px"></div>
  </div>
 </div>
</section>

<section id="files" class="view hidden">
 <div class="panel">
  <h2>📁 Dateien & Kamera</h2>
  <div class="field">
    <label for="fileInput">Datei auswählen</label>
    <input id="fileInput" type="file" accept="image/*,.txt,.md,.js,.html,.css,.json,.csv">
  </div>
  <div id="filePreview" class="status">Noch keine Datei ausgewählt.</div>
  <img id="imagePreview" class="hidden" alt="Vorschau" style="max-width:100%;max-height:360px;border-radius:12px;margin-top:10px">
  <div class="row">
    <button id="downloadFile" disabled>Herunterladen</button>
    <button id="useFileInChat" disabled>Text in Chat</button>
  </div>
 </div>
</section>

<section id="settings" class="view hidden">
 <div class="panel">
  <h2>⚙️ Verbindungen</h2>
  <div class="warning">
    <strong>iPad-Hinweis:</strong> Für Online-Chat Groq oder OpenRouter nutzen. Ollama braucht einen Computer im gleichen WLAN.
  </div>

  <h3>Online-Chat</h3>
  <div class="preset">
    <button type="button" id="presetGroq">Groq</button>
    <button type="button" id="presetOpenRouter">OpenRouter</button>
  </div>
  <div class="field">
    <label for="chatEndpoint">Chat-Endpunkt</label>
    <input id="chatEndpoint" value="https://api.groq.com/openai/v1/chat/completions">
  </div>
  <div class="field">
    <label for="chatModel">Chat-Modell</label>
    <input id="chatModel" value="qwen/qwen3.8-27b">
  </div>
  <div class="field">
    <label for="chatApiKey">API-Schlüssel</label>
    <input id="chatApiKey" type="password" placeholder="Dein Groq API-Key">
  </div>

  <h3 style="margin-top:24px">Ollama (Computer)</h3>
  <div class="field">
    <label for="ollamaUrl">Ollama-Adresse</label>
    <input id="ollamaUrl" value="http://localhost:11434">
  </div>
  <div class="field">
    <label for="ollamaModel">Ollama-Modell</label>
    <input id="ollamaModel" value="qwen2.5:7b">
  </div>

  <div class="row" style="margin-top:16px">
    <button class="primary" id="saveSettings">Speichern</button>
    <button id="testOllama">Ollama testen</button>
  </div>
  <div id="settingsStatus" class="status">Einstellungen werden lokal gespeichert.</div>
 </div>
</section>

<p class="foot">Meine eigene KI Plus 3.0 · Bilder im Chat möglich</p>
</main>

<script>
const $ = id => document.getElementById(id);
let pc=null,dc=null,micStream=null,camStream=null;
let selectedFile=null,selectedText="";
let chatHistory=[];
let chatImageBase64=null;

const settingsKeys=["ollamaUrl","ollamaModel","chatEndpoint","chatModel","chatApiKey","ephemeralEndpoint","realtimeModel","voiceChoice","instructions"];

function loadSettings(){
  settingsKeys.forEach(k=>{
    const v=localStorage.getItem("myai3_"+k);
    if(v!==null && $(k)) $(k).value=v;
  });
  // Fallback: immer diese Werte setzen, falls noch nichts gespeichert ist
  if(!$("chatEndpoint").value) $("chatEndpoint").value="https://api.groq.com/openai/v1/chat/completions";
  if(!$("chatModel").value) $("chatModel").value="qwen/qwen3.8-27b";
}
function saveSettings(){
  settingsKeys.forEach(k=>localStorage.setItem("myai3_"+k,$(k).value));
  $("settingsStatus").textContent="Gespeichert.";
}
loadSettings();

document.querySelectorAll(".tab").forEach(b=>b.addEventListener("click",()=>showTab(b.dataset.tab)));
function showTab(id){document.querySelectorAll(".view").forEach(v=>v.classList.toggle("hidden",v.id!==id));document.querySelectorAll(".tab").forEach(b=>b.classList.toggle("active",b.dataset.tab===id));}

$("saveSettings").onclick=saveSettings;
$("presetGroq").onclick=()=>{
  $("chatEndpoint").value="https://api.groq.com/openai/v1/chat/completions";
  $("chatModel").value="qwen/qwen3.8-27b";
  $("settingsStatus").textContent="Groq + qwen/qwen3.8-27b geladen. Key eintragen und speichern.";
};
$("presetOpenRouter").onclick=()=>{
  $("chatEndpoint").value="https://openrouter.ai/api/v1/chat/completions";
  $("chatModel").value="nvidia/nemotron-3.5-super-vl:free";
  $("settingsStatus").textContent="OpenRouter geladen.";
};

function addMsg(target,who,text){const el=document.createElement("div");el.className="msg "+who;el.textContent=text;$(target).appendChild(el);$(target).scrollTop=$(target).scrollHeight;}

/* Bild im Chat */
$("attachImage").onclick=()=>$("chatImageInput").click();
$("chatImageInput").onchange=e=>{
  const file=e.target.files[0];
  if(!file)return;
  const reader=new FileReader();
  reader.onload=ev=>{
    chatImageBase64=ev.target.result;
    $("chatImageImg").src=chatImageBase64;
    $("chatImagePreview").classList.remove("hidden");
  };
  reader.readAsDataURL(file);
};
$("removeChatImage").onclick=()=>{
  chatImageBase64=null;
  $("chatImagePreview").classList.add("hidden");
  $("chatImageInput").value="";
};

/* CHAT */
async function sendChat(){
  const text=$("chatInput").value.trim();
  if(!text && !chatImageBase64)return;

  $("chatInput").value="";
  const displayText = text || "(Bild gesendet)";
  addMsg("chatLog","user",displayText);

  let content;
  if(chatImageBase64){
    content = [];
    if(text) content.push({type:"text",text:text});
    content.push({type:"image_url",image_url:{url:chatImageBase64}});
  } else {
    content = text;
  }

  chatHistory.push({role:"user",content:content});
  const hadImage = !!chatImageBase64;
  chatImageBase64=null;
  $("chatImagePreview").classList.add("hidden");
  $("chatImageInput").value="";
  $("sendChat").disabled=true;

  try{
    let answer="";
    if($("chatEndpoint").value.trim()){
      const headers={"Content-Type":"application/json"};
      if($("chatApiKey").value.trim()) headers.Authorization="Bearer "+$("chatApiKey").value.trim();

      const r=await fetch($("chatEndpoint").value.trim(),{
        method:"POST",headers,
        body:JSON.stringify({
          model:$("chatModel").value.trim()||"qwen/qwen3.8-27b",
          messages:chatHistory,
          stream:false
        })
      });
      if(!r.ok){const t=await r.text();throw new Error("Online-API: HTTP "+r.status+" – "+t.slice(0,250));}
      const j=await r.json();
      answer=j.choices?.[0]?.message?.content||JSON.stringify(j);
    } else {
      const base=$("ollamaUrl").value.trim().replace(/\/$/,"");
      const r=await fetch(base+"/api/chat",{
        method:"POST",
        headers:{"Content-Type":"application/json"},
        body:JSON.stringify({
          model:$("ollamaModel").value.trim(),
          messages:chatHistory,
          stream:false
        })
      });
      if(!r.ok)throw new Error("Ollama: HTTP "+r.status);
      const j=await r.json();
      answer=j.message?.content||"Keine Antwort.";
    }
    chatHistory.push({role:"assistant",content:answer});
    addMsg("chatLog","ai",answer);
  }catch(e){
    let msg="Verbindung fehlgeschlagen: "+e.message;
    if(hadImage) msg+="\n\nHinweis: Nicht alle Modelle können Bilder verstehen.";
    addMsg("chatLog","ai",msg);
  }finally{
    $("sendChat").disabled=false;
  }
}

$("sendChat").onclick=sendChat;
$("chatInput").addEventListener("keydown",e=>{if(e.key==="Enter"&&!e.shiftKey){e.preventDefault();sendChat();}});
$("clearChat").onclick=()=>{$("chatLog").innerHTML="";chatHistory=[];addMsg("chatLog","ai","Chat geleert.");};

$("testOllama").onclick=async()=>{
  try{
    const r=await fetch($("ollamaUrl").value.replace(/\/$/,"")+"/api/tags");
    if(!r.ok)throw new Error("HTTP "+r.status);
    const j=await r.json();
    $("settingsStatus").textContent="Ollama erreichbar: "+(j.models||[]).map(x=>x.name).join(", ");
  }catch(e){$("settingsStatus").textContent="Ollama nicht erreichbar: "+e.message;}
};

/* LIVE */
function liveStatus(t){$("liveStatus").textContent="Status: "+t;}
function logLive(t,who="ai"){addMsg("liveLog",who,t);}
async function getEphemeralToken(){
  const endpoint=$("ephemeralEndpoint").value.trim();
  if(!endpoint)throw new Error("Token-Endpunkt fehlt");
  const r=await fetch(endpoint,{method:"POST",headers:{"Content-Type":"application/json"},body:JSON.stringify({model:$("realtimeModel").value.trim()})});
  if(!r.ok)throw new Error("Token HTTP "+r.status);
  const j=await r.json();
  const token=j.client_secret?.value||j.value||j.client_secret;
  if(!token)throw new Error("Kein Token");
  return token;
}
async function startLive(){
  $("startLive").disabled=true;liveStatus("Verbinde…");
  try{
    const token=await getEphemeralToken();
    micStream=await navigator.mediaDevices.getUserMedia({audio:true});
    pc=new RTCPeerConnection();
    pc.ontrack=e=>{$("remoteAudio").srcObject=e.streams[0];$("remoteAudio").play().catch(()=>{});};
    micStream.getTracks().forEach(t=>pc.addTrack(t,micStream));
    dc=pc.createDataChannel("oai-events");
    dc.onopen=()=>{dc.send(JSON.stringify({type:"session.update",session:{type:"realtime",instructions:$("instructions").value,output_modalities:["audio"],audio:{output:{voice:$("voiceChoice").value}}}}));logLive("Verbunden.");};
    const offer=await pc.createOffer();await pc.setLocalDescription(offer);
    await new Promise(r=>{if(pc.iceGatheringState==="complete")return r();const t=setTimeout(r,4000);pc.addEventListener("icegatheringstatechange",()=>{if(pc.iceGatheringState==="complete"){clearTimeout(t);r();}});});
    const sdpR=await fetch("https://api.openai.com/v1/realtime/calls?model="+encodeURIComponent($("realtimeModel").value.trim()||"gpt-realtime"),{method:"POST",body:pc.localDescription.sdp,headers:{"Authorization":"Bearer "+token,"Content-Type":"application/sdp"}});
    if(!sdpR.ok)throw new Error("Realtime HTTP "+sdpR.status);
    await pc.setRemoteDescription({type:"answer",sdp:await sdpR.text()});
    $("stopLive").disabled=false;liveStatus("Verbunden");
  }catch(e){logLive("Fehler: "+e.message);stopLive(false);liveStatus("Nicht verbunden");}
  finally{$("startLive").disabled=false;}
}
async function stopLive(update=true){if(dc)try{dc.close()}catch{}dc=null;if(pc)try{pc.close()}catch{}pc=null;if(micStream){micStream.getTracks().forEach(t=>t.stop());micStream=null;}$("remoteAudio").srcObject=null;$("stopLive").disabled=true;$("startLive").disabled=false;if(update)liveStatus("Beendet");}
$("startLive").onclick=startLive;$("stopLive").onclick=()=>stopLive(true);

async function cameraOn(){try{camStream=await navigator.mediaDevices.getUserMedia({video:{facingMode:"environment"},audio:false});$("liveVideo").srcObject=camStream;$("videoPlaceholder").classList.add("hidden");$("cameraOn").disabled=true;$("cameraOff").disabled=false;$("sendFrame").disabled=false;}catch(e){logLive("Kamera: "+e.message);}}
function cameraOff(){if(camStream)camStream.getTracks().forEach(t=>t.stop());camStream=null;$("liveVideo").srcObject=null;$("videoPlaceholder").classList.remove("hidden");$("cameraOn").disabled=false;$("cameraOff").disabled=true;$("sendFrame").disabled=true;}
$("cameraOn").onclick=cameraOn;$("cameraOff").onclick=cameraOff;
$("sendFrame").onclick=()=>{if(!camStream||!dc||dc.readyState!=="open"){logLive("Zuerst Live + Kamera starten");return;}const v=$("liveVideo");if(!v.videoWidth)return;const c=document.createElement("canvas");c.width=v.videoWidth;c.height=v.videoHeight;c.getContext("2d").drawImage(v,0,0);const data=c.toDataURL("image/jpeg",0.72).split(",")[1];dc.send(JSON.stringify({type:"conversation.item.create",item:{type:"message",role:"user",content:[{type:"input_text",text:"Was siehst du auf dem Bild?"},{type:"input_image",image_url:"data:image/jpeg;base64,"+data}]}}));dc.send(JSON.stringify({type:"response.create"}));logLive("Bild gesendet","user");};

/* Dateien */
$("fileInput").onchange=async e=>{selectedFile=e.target.files[0];selectedText="";$("imagePreview").classList.add("hidden");$("downloadFile").disabled=!selectedFile;$("useFileInChat").disabled=!selectedFile;if(!selectedFile){$("filePreview").textContent="Keine Datei";return;}$("filePreview").textContent=selectedFile.name+" · "+Math.round(selectedFile.size/1024)+" KB";if(selectedFile.type.startsWith("image/")){const url=URL.createObjectURL(selectedFile);$("imagePreview").src=url;$("imagePreview").classList.remove("hidden");$("useFileInChat").disabled=true;}else{selectedText=await selectedFile.text();$("filePreview").textContent=selectedFile.name+" · "+selectedText.length+" Zeichen";}};
$("downloadFile").onclick=()=>{if(!selectedFile)return;const a=document.createElement("a");a.href=URL.createObjectURL(selectedFile);a.download=selectedFile.name;a.click();};
$("useFileInChat").onclick=()=>{if(!selectedText)return;showTab("chat");$("chatInput").value="Analysiere bitte:\n\n"+selectedText.slice(0,15000);};

window.addEventListener("beforeunload",()=>{stopLive(false);cameraOff();});
</script>
</body>
</html>