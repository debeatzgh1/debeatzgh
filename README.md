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
    inset: 0;
    background: rgba(0,0,0,0.7);
    backdrop-filter: blur(5px);
  }
  .modal-content {
    width: 100%;
    height: 100%;
    background: #fff;
    animation: fadeSlideUp 0.4s ease;
    position: relative;
  }
  #modal-iframe {
    width: 100%;
    height: 100%;
    border: none;
  }
  .close-btn {
    position: absolute;
    top: 10px;
    right: 15px;
    font-size: 35px;
    font-weight: bold;
    cursor: pointer;
    z-index: 999999;
  }

  /* Fullscreen Button Menu */
  #button-menu {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.92);
    backdrop-filter: blur(8px);
    z-index: 99998;
    padding: 25px;
    overflow-y: auto;
    text-align: center;
  }

  /* Brand Logo */
  .brand-logo {
    width: 120px;
    height: auto;
    border-radius: 14px;
    margin: 10px auto 20px;
    display: block;
    box-shadow: 0 4px 14px rgba(255,255,255,0.35);
  }

  /* App Buttons Grid */
  .app-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
    gap: 18px;
    padding-bottom: 60px;
  }

  .menu-btn {
    background: #1e3a8a;
    padding: 10px;
    border-radius: 14px;
    color: #fff;
    text-decoration: none;
    display: block;
    transition: 0.25s;
    box-shadow: 0 4px 18px rgba(0,0,0,0.3);
  }

  .menu-btn:hover {
    background: #2563eb;
    transform: translateY(-3px);
  }

  /* Thumbnails */
  .app-thumb {
    width: 100%;
    height: 110px;
    border-radius: 10px;
    object-fit: cover;
    margin-bottom: 10px;
  }

  .app-title {
    font-size: 14px;
    font-weight: 700;
    margin-top: 5px;
  }

  /* Trigger Button */
  .trigger-toggle {
    position: fixed;
    top: 15px;
    left: 15px;
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

<!-- FLOATING TRIGGER BUTTON -->
<div class="trigger-toggle">☰ Apps</div>

<!-- BUTTON MENU -->
<div id="button-menu">
  <img class="brand-logo" src="https://debeatzgh.wordpress.com/wp-content/uploads/2025/11/screenshot_20251115-064239_16091878416894258095.png">
  <div class="app-grid"></div>
</div>

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
  const grid = document.querySelector(".app-grid");
  const iframeModal = document.getElementById("iframe-modal");
  const iframe = document.getElementById("modal-iframe");

  /* 🔥 APP LIST WITH THUMBNAILS */
  const apps = [
    {
      name: "Firebase UI Widgets",
      url: "https://debeatzgh1.github.io/firebase-front-end-components/",
      thumb: "https://i.ibb.co/7CQf7xF/firebase-ui.jpg"
    },
    {
      name: "Interactive Quizzes",
      url: "https://debeatzgh1.github.io/-Interactive-Knowledge-Quizzes/",
      thumb: "https://i.ibb.co/sVzcscY/quiz.jpg"
    },
    {
      name: "Brand Shop",
      url: "https://debeatzgh1.github.io/-My-Brand-Online-Digital-Products-Affiliate-Shop/",
      thumb: "https://i.ibb.co/Z6cqQnV/shop.jpg"
    },
    {
      name: "Links Menu",
      url: "https://msha.ke/debeatzgh",
      thumb: "https://i.ibb.co/1X7j0gb/links.jpg"
    },
    {
      name: "Iframe Generator",
      url: "https://debeatzgh1.github.io/Blogger-iframe-embed-generator/",
      thumb: "https://i.ibb.co/Tt6cbwX/iframe.jpg"
    }
  ];

  /* Generate App Buttons */
  apps.forEach(app => {
    const div = document.createElement("a");
    div.className = "menu-btn";
    div.innerHTML = `
      <img class="app-thumb" src="${app.thumb}">
      <div class="app-title">${app.name}</div>
    `;
    div.href = "#";
    div.onclick = () => {
      openIframe(app.url);
      return false;
    };
    grid.appendChild(div);
  });

  /* Open Menu */
  trigger.onclick = () => {
    menu.style.display = "block";
  };

  /* Open Link in Fullscreen Iframe */
  function openIframe(url){
    menu.style.display = "none";
    iframe.src = url;
    iframeModal.style.display = "block";
  }

  /* Close Iframe */
  document.addEventListener("click", e => {
    if (e.target.classList.contains("close-btn") || e.target.id === "iframe-modal") {
      iframeModal.style.display = "none";
      iframe.src = "";
    }
  });

});
</script>
