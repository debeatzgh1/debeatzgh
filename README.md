<style>
/* Floating Trigger Button (Top-Left) */
#master-trigger {
  position: fixed;
  top: 12px;
  left: 12px;
  background: #0ea5e9;
  color: #fff;
  padding: 8px 12px;
  border-radius: 10px;
  font-size: 13px;
  font-weight: 700;
  cursor: pointer;
  z-index: 99999;
  box-shadow: 0 3px 7px rgba(0,0,0,0.25);
  animation: heartbeat 2.5s infinite ease-in-out, fadeSlideUp 0.6s ease-out forwards;
}

/* Buttons Panel */
#master-panel {
  position: fixed;
  top: 60px;
  left: 12px;
  background: #ffffff;
  padding: 15px;
  border-radius: 14px;
  box-shadow: 0 4px 14px rgba(0,0,0,0.3);
  z-index: 99999;
  display: none;
  width: 200px;
  animation: fadeSlideUp 0.4s ease-out;
}

.master-btn {
  display: block;
  width: 100%;
  background: #2563eb;
  color: #fff;
  padding: 8px 10px;
  margin-bottom: 10px;
  font-size: 13px;
  font-weight: 600;
  border-radius: 10px;
  text-align: center;
  box-shadow: 0 3px 7px rgba(0,0,0,0.25);
  text-decoration: none;
}

/* Fullscreen Modal */
#global-modal {
  display: none;
  position: fixed;
  z-index: 999999;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,0.65);
  backdrop-filter: blur(4px);
}

#global-modal iframe {
  width: 100%;
  height: 100%;
  border: none;
}

#close-global {
  position: absolute;
  top: 18px;
  right: 20px;
  font-size: 38px;
  color: #fff;
  font-weight: bold;
  cursor: pointer;
  z-index: 99999999;
}
</style>

<div id="master-trigger">Tools</div>

<div id="master-panel">
  <a class="master-btn" data-url="https://debeatzgh1.github.io/firebase-front-end-components/">Widget</a>

  <a class="master-btn" data-url="https://debeatzgh1.github.io/-Interactive-Knowledge-Quizzes/">Quiz</a>

  <a class="master-btn" data-url="https://debeatzgh1.github.io/63etx/">L-G</a>

  <a class="master-btn" data-url="https://debeatzgh1.github.io/-My-Brand-Online-Digital-Products-Affiliate-Shop/">Build</a>

  <a class="master-btn" data-url="https://debeatzgh1.github.io/Blogger-iframe-embed-generator/">Iframe Generator</a>
</div>

<!-- Global Fullscreen Iframe Modal -->
<div id="global-modal">
  <span id="close-global">&times;</span>
  <iframe id="global-iframe" loading="lazy"></iframe>
</div>

<script>
document.addEventListener("DOMContentLoaded", () => {

  const trigger = document.getElementById("master-trigger");
  const panel = document.getElementById("master-panel");
  const modal = document.getElementById("global-modal");
  const iframe = document.getElementById("global-iframe");
  const close = document.getElementById("close-global");

  // Toggle panel
  trigger.addEventListener("click", () => {
    panel.style.display = (panel.style.display === "block") ? "none" : "block";
  });

  // Open iframe fullscreen
  document.querySelectorAll(".master-btn").forEach(btn => {
    btn.addEventListener("click", e => {
      e.preventDefault();
      iframe.src = btn.dataset.url;
      modal.style.display = "block";
      panel.style.display = "none";
    });
  });

  // Close fullscreen
  close.addEventListener("click", () => {
    modal.style.display = "none";
    iframe.src = "";
  });

  modal.addEventListener("click", e => {
    if (e.target.id === "global-modal") {
      modal.style.display = "none";
      iframe.src = "";
    }
  });

});
</script>
