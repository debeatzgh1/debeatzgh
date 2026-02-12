
<html lang="en">
<head>
    <style>
        :root {
            --glass-bg: rgba(13, 17, 23, 0.98);
            --glass-border: rgba(88, 166, 255, 0.4);
            --accent-glow: #58a6ff;
        }

        /* 1. COMPACT CENTER OVERLAY */
        #promo-banner {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%) scale(0.8);
            opacity: 0;
            visibility: hidden;
            
            width: 85%;
            max-width: 340px; /* Reduced width */
            background: var(--glass-bg);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border: 1px solid var(--glass-border);
            border-radius: 20px;
            padding: 25px 20px;
            box-shadow: 0 20px 50px rgba(0,0,0,0.6);
            z-index: 10000;
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            text-align: center;
            cursor: pointer;
        }

        #promo-banner.active {
            transform: translate(-50%, -50%) scale(1);
            opacity: 1;
            visibility: visible;
        }

        /* 2. AUTO-SLIDE CONTENT */
        .slide-container {
            position: relative;
            height: 80px; /* Slimmer profile */
            overflow: hidden;
            margin-top: 10px;
        }

        .slide {
            position: absolute;
            width: 100%;
            opacity: 0;
            transform: scale(0.95);
            transition: all 0.5s ease;
        }

        .slide.current {
            opacity: 1;
            transform: scale(1);
        }

        #promo-banner p {
            margin: 0;
            font-size: 0.95rem;
            color: #ffffff;
            line-height: 1.4;
        }

        .highlight { color: var(--accent-glow); font-weight: 700; }

        .badge {
            background: var(--accent-glow);
            color: #000;
            font-size: 0.65rem;
            padding: 2px 10px;
            border-radius: 4px;
            font-weight: 800;
            text-transform: uppercase;
            margin-bottom: 8px;
            display: inline-block;
        }

        /* 3. RIGHT-CENTER FLOATING LAUNCHER */
        #floating-trigger {
            position: fixed;
            right: 0;
            top: 50%;
            transform: translateY(-50%);
            width: 45px;
            height: 60px;
            background: var(--accent-glow);
            border-radius: 30px 0 0 30px; /* Half-pill shape */
            display: none;
            align-items: center;
            justify-content: center;
            color: #0d1117;
            cursor: pointer;
            box-shadow: -4px 0 15px rgba(88, 166, 255, 0.4);
            z-index: 9997;
            transition: width 0.3s;
        }

        #floating-trigger:hover { width: 55px; }

        /* 4. UTILS */
        .close-btn {
            position: absolute;
            top: 10px;
            right: 10px;
            background: none;
            color: #666;
            border: none;
            font-size: 1.2rem;
            cursor: pointer;
        }

        #progress-bar {
            position: absolute;
            bottom: 0;
            left: 0;
            height: 3px;
            background: var(--accent-glow);
            width: 0%;
            transition: width 3s linear;
        }
    </style>
</head>
<body>

    <div id="promo-banner">
        <button class="close-btn" onclick="closeBanner(event)">×</button>
        <div id="progress-bar"></div>
        
        <div class="slide-container">
            <div class="slide current" data-link="https://debeatzgh1.github.io/Home-/">
                <span class="badge">Ecosystem</span>
                <p>All your <span class="highlight">lifestyle tools</span> in one sleek dashboard.</p>
            </div>
            <div class="slide" data-link="https://t.me/Miningstoncoin99_bot">
                <span class="badge">Mining</span>
                <p>Start earning with our <span class="highlight">verified crypto</span> Telegram bots.</p>
            </div>
            <div class="slide" data-link="https://mybrandsonline.blogspot.com/">
                <span class="badge">Updates</span>
                <p>Get the latest <span class="highlight">brand insights</span> and daily news.</p>
            </div>
        </div>
    </div>

    <div id="floating-trigger" onclick="showBannerSequence()">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><path d="M15 18l-6-6 6-6"/></svg>
    </div>

    <script>
        const banner = document.getElementById('promo-banner');
        const fab = document.getElementById('floating-trigger');
        const slides = document.querySelectorAll('.slide');
        const progress = document.getElementById('progress-bar');
        
        let currentSlide = 0;
        let slideInterval;

        banner.onclick = () => {
            window.open(slides[currentSlide].getAttribute('data-link'), '_blank');
        };

        function resetProgress() {
            progress.style.transition = 'none';
            progress.style.width = '0%';
            setTimeout(() => {
                progress.style.transition = 'width 3s linear';
                progress.style.width = '100%';
            }, 50);
        }

        function startAutoSlide() {
            resetProgress();
            slideInterval = setInterval(() => {
                slides[currentSlide].classList.remove('current');
                currentSlide = (currentSlide + 1) % slides.length;
                slides[currentSlide].classList.add('current');
                resetProgress();
            }, 3000);
        }

        function showBannerSequence() {
            fab.style.display = 'none';
            banner.classList.add('active');
            startAutoSlide();
        }

        function closeBanner(event) {
            event.stopPropagation(); 
            banner.classList.remove('active');
            fab.style.display = 'flex';
            clearInterval(slideInterval);
        }

        window.onload = () => setTimeout(showBannerSequence, 1500);
    </script>
</body>
</html>


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
