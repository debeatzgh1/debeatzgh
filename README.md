<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>6-Button Live Carousel + Fullscreen Iframe</title>
<style>
body { font-family: Arial,sans-serif; background:#0a0a0a; color:#fff; display:flex; flex-direction:column; align-items:center; padding:20px; }
h1 { color:#00ff88; text-align:center; margin-bottom:20px; }
textarea, input[type="text"] { width:100%; max-width:800px; padding:10px; margin-bottom:12px; border-radius:8px; border:1px solid #444; background:#111; color:#fff; font-family:monospace; }
.button-config { background:#111; padding:15px; border-radius:10px; margin-bottom:15px; width:100%; max-width:800px; }
.button-config label { display:block; margin-bottom:6px; font-weight:bold; }
.colors { display:flex; gap:12px; margin-bottom:8px; }
.colors label { cursor:pointer; display:flex; align-items:center; gap:6px; }
.colors input[type="radio"] { accent-color:currentColor; }
button.update-btn { padding:10px 20px; background:#00ff88; border:none; border-radius:8px; cursor:pointer; font-weight:bold; color:#000; margin-bottom:12px; transition:transform .1s; }
button.update-btn:hover { transform:scale(1.05); }
pre#output { width:100%; max-width:800px; background:#111; padding:12px; border-radius:8px; border:1px solid #444; white-space:pre-wrap; overflow-wrap:break-word; cursor:pointer; }
#preview { display:flex; gap:12px; flex-wrap:wrap; justify-content:center; margin-top:12px; padding:12px; background:#111; border-radius:8px; width:100%; max-width:800px; border:1px solid #444; }
#preview a { padding:10px 20px; border-radius:8px; color:#000; text-decoration:none; font-weight:bold; cursor:pointer; }

/* FULLSCREEN MENU + IFRAME */
@keyframes fadeSlideUp {0%{opacity:0;transform:translateY(20px);}100%{opacity:1;transform:translateY(0);}}
@keyframes heartbeat {0%{transform:scale(1);}25%{transform:scale(1.05);}50%{transform:scale(1);}75%{transform:scale(1.05);}100%{transform:scale(1);}}
#iframe-modal{display:none;position:fixed;z-index:99999;right:0;top:0;width:100%;height:100%;background:rgba(0,0,0,0.7);backdrop-filter:blur(5px);}
.modal-content{width:100%;height:100%;background:#fff;animation:fadeSlideUp 0.4s ease;position:relative;}
#modal-iframe{width:100%;height:100%;border:none;}
.close-btn{position:absolute;top:12px;right:20px;font-size:32px;font-weight:bold;cursor:pointer;z-index:999999;}
.trigger-toggle{position:fixed;top:15px;right:15px;z-index:999999;background:#2563eb;color:#fff;padding:10px 16px;border-radius:10px;font-size:14px;font-weight:700;cursor:pointer;animation:heartbeat 2.5s infinite ease-in-out,fadeSlideUp 0.6s ease-out forwards;box-shadow:0 4px 10px rgba(0,0,0,0.25);}
#button-menu{display:none;position:fixed;z-index:99998;inset:0;background:rgba(0,0,0,0.85);backdrop-filter:blur(6px);padding:40px 20px;overflow-y:auto;}
#button-menu .menu-btn{display:block;width:100%;margin:10px auto;padding:15px;font-size:17px;font-weight:700;border-radius:12px;text-align:center;background:#2563eb;color:#fff;text-decoration:none;}
</style>
</head>
<body>

<h1>6-Button Live Carousel + Fullscreen Iframe</h1>

<label for="htmlInput">Paste your HTML code here:</label>
<textarea id="htmlInput" placeholder="Paste your HTML code with 6 buttons here..."></textarea>

<!-- Button Configs -->
<div id="buttonConfigs"></div>

<button class="update-btn" id="updateBtn">Update HTML Script</button>

<h3>Live Preview:</h3>
<div id="preview"></div>

<h3>Updated HTML Script:</h3>
<pre id="output">Click "Update HTML Script" to generate code here. Click box to copy.</pre>

<!-- TRIGGER BUTTON + FULLSCREEN MENU + IFRAME -->
<div class="trigger-toggle">☰ Apps</div>
<div id="button-menu"></div>
<div id="iframe-modal">
  <div class="modal-content">
    <span class="close-btn">&times;</span>
    <iframe id="modal-iframe"></iframe>
  </div>
</div>

<script>
// Generate 6 button configs dynamically
const buttonConfigsDiv = document.getElementById('buttonConfigs');
const previewDiv = document.getElementById('preview');

for(let i=1;i<=6;i++){
  const div = document.createElement('div');
  div.className = 'button-config';
  div.dataset.btn = i;
  div.innerHTML = `
    <label>Button ${i} URL:</label>
    <input type="text" class="urlInput" placeholder="https://example.com/${i}">
    <div class="colors">
      <label><input type="radio" name="color${i}" value="red"> Red</label>
      <label><input type="radio" name="color${i}" value="yellow"> Yellow</label>
      <label><input type="radio" name="color${i}" value="green" checked> Green</label>
    </div>`;
  buttonConfigsDiv.appendChild(div);

  // Create live preview button
  const previewBtn = document.createElement('a');
  previewBtn.href = '#';
  previewBtn.id = `previewBtn${i}`;
  previewBtn.textContent = `Button ${i}`;
  previewBtn.style.background = 'green';
  previewBtn.target = '_blank';
  previewBtn.onclick = ()=>openIframe(previewBtn.href);
  previewDiv.appendChild(previewBtn);
}

const htmlInput = document.getElementById('htmlInput');
const updateBtn = document.getElementById('updateBtn');
const output = document.getElementById('output');

// Update live preview and HTML
function updatePreviewAndHTML(){
  let html = htmlInput.value;

  for(let i=1;i<=6;i++){
    const btnConfig = document.querySelector(`.button-config[data-btn="${i}"]`);
    const url = btnConfig.querySelector('.urlInput').value.trim() || '#';
    const color = btnConfig.querySelector(`input[name="color${i}"]:checked`).value;

    // Update live preview
    const previewBtn = document.getElementById(`previewBtn${i}`);
    previewBtn.href = url;
    previewBtn.style.background = color;

    // Replace URLs in HTML
    const regexUrl = new RegExp(`(href|src)=(["'])(.*?button${i}.*?)\\2`, 'gi');
    html = html.replace(regexUrl, `$1="$2${url}$2`);

    // Replace inline colors
    const regexColor = new RegExp(`(button\\s*\\#?button${i}.*?)(background\\s*:\\s*.*?;?)`,'gi');
    html = html.replace(regexColor, `$1background:${color};`);
    const regexClass = new RegExp(`(class\\s*=\\s*["'][^"']*button${i}-)(red|yellow|green)([^"']*["'])`, 'gi');
    html = html.replace(regexClass, `$1${color}$3`);
  }

  output.textContent = html;
}

updateBtn.addEventListener('click', updatePreviewAndHTML);
output.addEventListener('click', ()=>navigator.clipboard.writeText(output.textContent).then(()=>alert('Updated HTML copied!')));

// FULLSCREEN IFRAME MENU
const trigger = document.querySelector(".trigger-toggle");
const menu = document.getElementById("button-menu");
const iframeModal = document.getElementById("iframe-modal");
const iframe = document.getElementById("modal-iframe");

// Default 6 apps
const apps = [
  { name: "Button 1", url: "#" },
  { name: "Button 2", url: "#" },
  { name: "Button 3", url: "#" },
  { name: "Button 4", url: "#" },
  { name: "Button 5", url: "#" },
  { name: "Button 6", url: "#" }
];

// Generate fullscreen menu buttons
apps.forEach(app=>{
  const btn = document.createElement("a");
  btn.className = "menu-btn";
  btn.innerText = app.name;
  btn.href="#";
  btn.onclick = () => { openIframe(app.url); return false; };
  menu.appendChild(btn);
});

// Trigger menu
trigger.onclick = ()=>{ menu.style.display="block"; };

// Open iframe function
function openIframe(url){
  menu.style.display="none";
  iframe.src = url;
  iframeModal.style.display="block";
}

// Close iframe
document.addEventListener("click", e=>{
  if(e.target.classList.contains("close-btn") || e.target.id==="iframe-modal"){
    iframeModal.style.display="none";
    iframe.src="";
  }
});
</script>
</body>
</html>
