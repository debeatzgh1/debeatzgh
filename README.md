<!-- FULLSCREEN MENU + TRIGGER BUTTON + UNIVERSAL IFRAME VIEWER -->
<style>
  @keyframes fadeSlideUp {
    0% { opacity: 0; transform: translateY(20px); }
    100% { opacity: 1; transform: translateY(0); }
  }
  @keyframes heartbeat {
    0% { transform: scale(1); }
    25% { transform: scale(1.05); }
    50% { transform: scale(1); }
    75% { transform: scale(1.05); }
    100% { transform: scale(1); }
  }

  /* Fullscreen Iframe Modal */
  #iframe-modal {
    display: none;
    position: fixed;
    z-index: 99999;
    right: 0; top: 0;
    width: 100%; height: 100%;
    background: rgba(0,0,0,0.7);
    backdrop-filter: blur(5px);
  }
  .modal-content {
    width: 100%; height: 100%;
    background: #fff;
    animation: fadeSlideUp 0.4s ease;
    position: relative;
  }
  #modal-iframe {
    width: 100%; height: 100%;
    border: none;
  }
  .close-btn {
    position: absolute;
    top: 12px;
    right: 20px;
    font-size: 32px;
    font-weight: bold;
    cursor: pointer;
    z-index: 999999;
  }

  /* Fullscreen Button Menu */
  #button-menu {
    display: none;
    position: fixed;
    z-index: 99998;
    inset: 0;
    background: rgba(0,0,0,0.85);
    backdrop-filter: blur(6px);
    padding: 40px 20px;
    overflow-y: auto;
  }

  #button-menu .menu-btn {
    display: block;
    width: 100%;
    margin: 10px auto;
    padding: 15px;
    font-size: 17px;
    font-weight: 700;
    border-radius: 12px;
    text-align: center;
    background: #2563eb;
    color: #fff;
    text-decoration: none;
  }

  /* TRIGGER BUTTON */
  .trigger-toggle {
    position: fixed;
    top: 15px;
    Right: 15px;
    z-index: 999999;
    background: #2563eb;
    color: #fff;
    padding: 10px 16px;
    border-radius: 10px;
    font-size: 14px;
    font-weight: 700;
    cursor: pointer;
    animation: heartbeat 2.5s infinite ease-in-out, fadeSlideUp 0.6s ease-out forwards;
    box-shadow: 0 4px 10px rgba(0,0,0,0.25);
  }
</style>

<div class="trigger-toggle">☰ Apps</div>

<!-- BUTTON MENU -->
<div id="button-menu"></div>

<!-- UNIVERSAL IFRAME VIEWER -->
<div id="iframe-modal">
  <div class="modal-content">
    <span class="close-btn">&times;</span>
    <iframe id="modal-iframe"></iframe>
  </div>
</div>

<script>
document.addEventListener("DOMContentLoaded", () => {

  const trigger = document.querySelector(".trigger-toggle");
  const menu = document.getElementById("button-menu");
  const iframeModal = document.getElementById("iframe-modal");
  const iframe = document.getElementById("modal-iframe");

  /* 🔥 ALL YOUR BUTTONS + URLS ARE LISTED HERE */
  const apps = [
    { name: "Firebase UI Widgets", url: "https://debeatzgh1.github.io/firebase-front-end-components/" },
    { name: "Interactive Quizzes", url: "https://debeatzgh1.github.io/-Interactive-Knowledge-Quizzes/" },
    { name: "My Brand Shop", url: "https://debeatzgh1.github.io/-My-Brand-Online-Digital-Products-Affiliate-Shop/" },
    { name: "Links Menu", url: "https://msha.ke/debeatzgh" },
    { name: "Iframe Generator", url: "https://debeatzgh1.github.io/Blogger-iframe-embed-generator/" }
  ];

  /* 🔥 Generate Buttons Automatically */
  apps.forEach(app => {
    const btn = document.createElement("a");
    btn.className = "menu-btn";
    btn.innerText = app.name;
    btn.href = "#";
    btn.onclick = () => {
      openIframe(app.url);
      return false;
    };
    menu.appendChild(btn);
  });

  /* Open the fullscreen button menu */
  trigger.onclick = () => {
    menu.style.display = "block";
  };

  /* Open URL inside fullscreen iframe */
  function openIframe(url){
    menu.style.display = "none";
    iframe.src = url;
    iframeModal.style.display = "block";
  }

  /* Close iframe viewer */
  document.addEventListener("click", e => {
    if (e.target.classList.contains("close-btn") || e.target.id === "iframe-modal") {
      iframeModal.style.display = "none";
      iframe.src = "";
    }
  });

});
</script>
