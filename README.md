<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Floating Fullscreen Iframe Menu</title>
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

  /* Floating trigger button */
  .trigger-toggle {
    position: fixed;
    top: 20px;
    left: 20px;
    z-index: 9999;
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

  /* Fullscreen menu */
  #button-menu {
    display: none;
    position: fixed;
    inset: 0;
    z-index: 9998;
    background: rgba(0,0,0,0.85);
    backdrop-filter: blur(6px);
    padding: 50px 20px;
    overflow-y: auto;
  }
  #button-menu .menu-btn {
    display: block;
    width: 100%;
    max-width: 400px;
    margin: 15px auto;
    padding: 15px;
    font-size: 17px;
    font-weight: 700;
    border-radius: 12px;
    text-align: center;
    background: #2563eb;
    color: #fff;
    text-decoration: none;
    cursor: pointer;
  }

  /* Fullscreen modal iframe */
  #iframe-modal {
    display: none;
    position: fixed;
    inset: 0;
    z-index: 9999;
    background: rgba(0,0,0,0.6);
    backdrop-filter: blur(4px);
  }
  .modal-content {
    position: relative;
    margin: 2% auto;
    width: 95%;
    height: 90%;
    background: #fff;
    border-radius: 16px;
    box-shadow: 0 8px 24px rgba(0,0,0,0.3);
    overflow: hidden;
    animation: fadeSlideUp 0.3s ease;
  }
  #modal-iframe {
    width: 100%;
    height: 100%;
    border: none;
  }
  .close-btn {
    position: absolute;
    top: 10px;
    right: 18px;
    font-size: 30px;
    color: #333;
    cursor: pointer;
    transition: color 0.2s;
    z-index: 10;
  }
  .close-btn:hover { color: #e11d48; }
</style>
</head>
<body>

<!-- Trigger button -->
<div class="trigger-toggle">☰ Apps</div>

<!-- Fullscreen menu -->
<div id="button-menu"></div>

<!-- Iframe modal -->
<div id="iframe-modal">
  <div class="modal-content">
    <span class="close-btn">&times;</span>
    <iframe id="modal-iframe" src="" loading="lazy"></iframe>
  </div>
</div>

<script>
document.addEventListener("DOMContentLoaded", () => {

  const trigger = document.querySelector(".trigger-toggle");
  const menu = document.getElementById("button-menu");
  const iframeModal = document.getElementById("iframe-modal");
  const iframe = document.getElementById("modal-iframe");

  // List of all buttons with their URLs
  const apps = [
    { name: "Firebase UI Widgets", url: "https://debeatzgh1.github.io/firebase-front-end-components/" },
    { name: "Interactive Quizzes", url: "https://debeatzgh1.github.io/-Interactive-Knowledge-Quizzes/" },
    { name: "My Brand Shop", url: "https://debeatzgh1.github.io/-My-Brand-Online-Digital-Products-Affiliate-Shop/" },
    { name: "Links Menu", url: "https://msha.ke/debeatzgh" },
    { name: "Iframe Generator", url: "https://debeatzgh1.github.io/Blogger-iframe-embed-generator/" },
    { name: "Extra App", url: "https://debeatzgh1.github.io/Home-/" }
  ];

  // Generate buttons in the fullscreen menu
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

  // Show the fullscreen menu
  trigger.onclick = () => {
    menu.style.display = "block";
  };

  // Open URL in fullscreen iframe
  function openIframe(url){
    menu.style.display = "none";
    iframe.src = url;
    iframeModal.style.display = "block";
  }

  // Close iframe modal
  document.addEventListener("click", e => {
    if (e.target.classList.contains("close-btn") || e.target.id === "iframe-modal") {
      iframeModal.style.display = "none";
      iframe.src = "";
    }
  });

});
</script>

</body>
</html>
