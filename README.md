<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Floating Vertical Button Menu</title>
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

  /* Floating container */
  .floating-btn-group {
    position: fixed;
    top: 20px;
    left: 20px;
    z-index: 9999;
    display: flex;
    flex-direction: column;
    gap: 10px;
  }

  /* Trigger button */
  .trigger-btn {
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

  /* App buttons (hidden initially) */
  .app-btn {
    background: #2563eb;
    color: #fff;
    padding: 10px 16px;
    border-radius: 10px;
    font-size: 13px;
    font-weight: 700;
    cursor: pointer;
    display: none; /* Hidden until trigger clicked */
    box-shadow: 0 3px 8px rgba(0,0,0,0.25);
    white-space: nowrap;
  }

  /* Fullscreen modal iframe */
  #iframe-modal {
    display: none;
    position: fixed;
    inset: 0;
    z-index: 9998;
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

<!-- Floating button container -->
<div class="floating-btn-group">
  <div class="trigger-btn">☰ Apps</div>
</div>

<!-- Iframe modal -->
<div id="iframe-modal">
  <div class="modal-content">
    <span class="close-btn">&times;</span>
    <iframe id="modal-iframe" src="" loading="lazy"></iframe>
  </div>
</div>

<script>
document.addEventListener("DOMContentLoaded", () => {

  const triggerBtn = document.querySelector(".trigger-btn");
  const btnGroup = document.querySelector(".floating-btn-group");

  // List of apps with names and URLs
  const apps = [
    { name: "Firebase UI Widgets", url: "https://debeatzgh1.github.io/firebase-front-end-components/" },
    { name: "Interactive Quizzes", url: "https://debeatzgh1.github.io/-Interactive-Knowledge-Quizzes/" },
    { name: "My Brand Shop", url: "https://debeatzgh1.github.io/-My-Brand-Online-Digital-Products-Affiliate-Shop/" },
    { name: "Links Menu", url: "https://msha.ke/debeatzgh" },
    { name: "Iframe Generator", url: "https://debeatzgh1.github.io/Blogger-iframe-embed-generator/" },
    { name: "Home", url: "https://debeatzgh1.github.io/Home-/" }
  ];

  // Create app buttons
  apps.forEach(app => {
    const btn = document.createElement("div");
    btn.className = "app-btn";
    btn.innerText = app.name;
    btn.onclick = () => openIframe(app.url);
    btnGroup.appendChild(btn);
  });

  // Toggle app buttons visibility
  let isOpen = false;
  triggerBtn.addEventListener("click", () => {
    isOpen = !isOpen;
    const appBtns = document.querySelectorAll(".app-btn");
    appBtns.forEach(btn => btn.style.display = isOpen ? "block" : "none");
  });

  // Iframe modal elements
  const iframeModal = document.getElementById("iframe-modal");
  const iframe = document.getElementById("modal-iframe");

  function openIframe(url){
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
