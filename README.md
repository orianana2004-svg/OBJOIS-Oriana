# OBJOIS-Oriana
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Beauté éphémère</title>
<style>
  body { 
    margin: 0; 
    height: 100vh; 
    display: flex; 
    justify-content: center; 
    align-items: center; 
    font-family: 'Georgia', serif; 
    overflow: hidden; 
    background: white; 
    transition: background 2.5s ease; 
  }

  /* Écran de démarrage */
  #startScreen {
    position: absolute;
    top: 0; left: 0; width: 100%; height: 100%;
    background: white;
    z-index: 100;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    transition: opacity 1s ease;
  }
  #startScreen h1 {
    font-size: 24px;
    color: #333;
    margin-bottom: 20px;
    text-align: center;
  }
  #startScreen p {
    font-size: 16px;
    color: #666;
    font-style: italic;
  }

  #intro { 
    font-size: 18px; 
    color: black; 
    transition: opacity 1.5s ease; 
    position: absolute; 
    text-align: center; 
    width: 90%; 
    z-index: 20;
    padding: 20px;
    box-sizing: border-box;
    line-height: 1.6;
    opacity: 0; 
    pointer-events: none;
  }

  /* Message d'inactivité */
  #pauseOverlay {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    text-align: center;
    color: white;
    font-size: 18px;
    font-style: italic;
    text-shadow: 0 2px 8px rgba(0,0,0,0.9);
    z-index: 30;
    opacity: 0;
    transition: opacity 1s ease;
    pointer-events: none;
    width: 80%;
    line-height: 1.5;
  }

  .slide-text { 
    font-size: 18px; 
    color: white; 
    transition: opacity 1.5s ease; 
    position: absolute; 
    text-align: center; 
    width: 90%; 
    z-index: 15;
    padding: 20px;
    box-sizing: border-box;
    text-shadow: 0 2px 6px rgba(0,0,0,0.8);
    font-style: italic;
    line-height: 1.4;
    pointer-events: none;
    opacity: 0;
  }

  .video { 
    position: absolute; 
    top: 0; 
    left: 0; 
    width: 100%; 
    height: 100%; 
    object-fit: cover; 
    opacity: 0; 
    transition: opacity 3s ease; 
    z-index: 10;
    pointer-events: none; 
  }

  #finalImg {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    opacity: 0;
    transition: opacity 3s ease;
    z-index: 10;
    pointer-events: none;
    filter: contrast(1.2) brightness(1.1) saturate(1.1); 
  }

  #manifesto {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 90%;
    max-width: 600px;
    color: #e0e0e0;
    font-size: 16px; 
    line-height: 1.6;
    text-align: center;
    z-index: 25;
    opacity: 0;
    transition: opacity 2.5s ease;
    font-family: 'Courier New', Courier, monospace; 
    white-space: pre-wrap; 
    padding: 10px;
    box-sizing: border-box;
    text-shadow: 0 1px 4px rgba(0,0,0,0.9);
    pointer-events: none;
  }

  .hidden { opacity: 0 !important; pointer-events: none; display: none !important; }
  .visible { opacity: 1 !important; }
  .final-bg { background-color: #1a251a !important; }
  
  #errorMsg {
    position: absolute;
    top: 20px;
    left: 50%;
    transform: translateX(-50%);
    background: red;
    color: white;
    padding: 10px;
    border-radius: 5px;
    z-index: 200;
    display: none;
    text-align: center;
    width: 90%;
  }
</style>
</head>
<body>

  <div id="startScreen">
    <h1>Beauté éphémère</h1>
    <p>Tap anywhere to start the experience</p>
  </div>

  <div id="intro">Stop moving, take the time to contemplate.<br>Feel the moment.</div>
  
  <!-- Message d'inactivité -->
  <div id="pauseOverlay">Stop moving, take the time to contemplate.<br>Feel the moment.</div>

  <video id="slideVideo" class="video hidden" muted playsinline preload="auto"></video>
  <div id="slideText" class="slide-text hidden"></div>

  <div id="manifesto">Beauté éphémère is the beauty that refuses to perform.
It lives in the soft, the marked, the fragile, the temporary.

Care is not sentiment but method: attention, resistance, presence.
The body, like a plant, a bruise, a stretch, a scar, 
softens, blooms again.
Nothing about it is permanent,
It's precisely where its beauty lies.

Beauty is the trace, the fold, the stretch mark, the softness,
the weight, the thinness, the excess, the lack - of.
the evidence of having lived.

The plant, their bodies, our bodies are already transforming.
Their fragility is not a failure but a form of truth.

Take the time to feel.
Beauty is not captured; it is stayed with.

Beauté éphémère is not a hidden fact.
It is simply not rushed.</div>
  
  <img id="finalImg" class="hidden" src="fleur.jpg.jpg">
  <div id="errorMsg"></div>

<script>
  // CONFIGURATION
  const slides = [
    { video: "00002.mp4", text: "Beauty is there. It does not impose itself. It waits to be seen. It waits to be looked at." },
    { video: "00008.mp4", text: "It's like the fragile rhythm of presence" },
    { video: "00012.mp4", text: "The body is a temporary landscape. Fragility is not a deficiency: it is a form of truth" },
    { video: "00014.mp4", text: "Some forms of beauty shine only for as long as we pay attention to them." },
    { video: "00016.mp4", text: "Caring starts with what seems like the smallest of things, yet it changes everything." },
    { video: "00022.mp4", text: "Nothing needs to last to be worthy. Everything is enough if you take care." },
    { video: "00026.mp4", text: "The bodies we overlook, the ones we do not look at, hold beauties we have not yet named." },
    { video: "00030.mp4", text: "The fragile rhythm of life continues while we don't pay attention to it." },
    { video: "00035.mp4", text: "Beauty is the evidence of having lived. Take Care." }
  ];

  let phase = 0; // 0: Start, 1: Intro, 2: Slides, 3: Final
  let slideIndex = 0;
  let isAnimating = false;
  let slideTimer;

  // Variables d'inactivité
  let inactivityTimer;
  let resumeTimer;
  let isPausedByInactivity = false;
  const INACTIVITY_DELAY = 3000; // 3s d'arrêt pour afficher le message
  const RESUME_DELAY = 3000;     // 3s de calme après mouvement pour reprendre

  const startScreen = document.getElementById('startScreen');
  const introEl = document.getElementById('intro');
  const pauseOverlay = document.getElementById('pauseOverlay');
  const slideVideoEl = document.getElementById('slideVideo');
  const slideTextEl = document.getElementById('slideText');
  const manifestoEl = document.getElementById('manifesto');
  const finalImgEl = document.getElementById('finalImg');
  const errorMsg = document.getElementById('errorMsg');

  function showError(msg) {
    errorMsg.innerText = msg;
    errorMsg.style.display = 'block';
    console.error(msg);
  }

  // --- LOGIQUE D'INACTIVITÉ ---
  function resetInactivityTimer() {
    if (isPausedByInactivity) return;

    clearTimeout(inactivityTimer);
    clearTimeout(resumeTimer);
    pauseOverlay.style.opacity = '0';

    if (phase === 2 && !isAnimating) {
        inactivityTimer = setTimeout(() => {
            triggerPause();
        }, INACTIVITY_DELAY);
    }
  }

  function triggerPause() {
    if (phase !== 2) return;
    isPausedByInactivity = true;
    slideVideoEl.pause();
    pauseOverlay.style.opacity = '1';
  }

  function attemptResume() {
    if (!isPausedByInactivity) return;
    
    pauseOverlay.style.opacity = '0';
    
    resumeTimer = setTimeout(() => {
        isPausedByInactivity = false;
        slideVideoEl.play().catch(e => console.log("Erreur reprise:", e));
        
        // Relancer le compteur de 10s pour le slide actuel
        slideTimer = setTimeout(() => {
            slideVideoEl.style.opacity = '0';
            slideTextEl.style.opacity = '0';
            setTimeout(() => {
                slideVideoEl.pause();
                slideIndex++;
                isAnimating = false;
                showNextSlide();
            }, 3000);
        }, 10000);
    }, RESUME_DELAY);
  }

  // Écouteurs d'événements (souris, tactile, clavier)
  const events = ['mousedown', 'mousemove', 'touchstart', 'touchmove', 'keydown', 'scroll'];
  events.forEach(evt => {
      document.addEventListener(evt, resetInactivityTimer, { passive: true });
      document.addEventListener(evt, () => {
          if (isPausedByInactivity) {
              attemptResume();
          }
      }, { passive: true });
  });

  // Démarrage
  startScreen.addEventListener('click', () => {
    clearTimeout(inactivityTimer);
    clearTimeout(resumeTimer);
    isPausedByInactivity = false;
    
    startScreen.style.opacity = '0';
    setTimeout(() => {
      startScreen.style.display = 'none';
      phase = 1;
      introEl.style.opacity = '1';
      
      setTimeout(() => {
        introEl.style.opacity = '0';
        setTimeout(() => {
          introEl.classList.add('hidden');
          phase = 2;
          resetInactivityTimer(); // Activer le détecteur
          showNextSlide();
        }, 1500);
      }, 5000);
    }, 1000);
  });

  function showNextSlide() {
    if (phase !== 2 || isAnimating) return;
    
    if (slideIndex >= slides.length) {
        goToFinal();
        return;
    }

    isAnimating = true;
    const currentSlide = slides[slideIndex];
    
    slideVideoEl.pause();
    slideVideoEl.currentTime = 0;
    slideVideoEl.src = currentSlide.video;
    slideTextEl.innerText = currentSlide.text;
    
    slideVideoEl.classList.remove('hidden');
    slideTextEl.classList.remove('hidden');
    slideVideoEl.style.opacity = '0';
    slideTextEl.style.opacity = '0';

    slideVideoEl.load();
    
    slideVideoEl.oncanplay = () => {
        slideVideoEl.play().then(() => {
            slideVideoEl.style.opacity = '1';
            slideTextEl.style.opacity = '1';
            
            resetInactivityTimer(); // Reset timer au début du slide

            slideTimer = setTimeout(() => {
                slideVideoEl.style.opacity = '0';
                slideTextEl.style.opacity = '0';
                setTimeout(() => {
                    slideVideoEl.pause();
                    slideIndex++;
                    isAnimating = false;
                    showNextSlide();
                }, 3000);
            }, 10000);
        }).catch(e => {
            showError(`Erreur de lecture: ${currentSlide.video}. Vérifiez le nom.`);
            setTimeout(() => {
                slideVideoEl.style.opacity = '0';
                slideTextEl.style.opacity = '0';
                setTimeout(() => {
                    slideIndex++;
                    isAnimating = false;
                    showNextSlide();
                }, 3000);
            }, 3000);
        });
    };

    slideVideoEl.onerror = () => {
        showError(`Fichier introuvable: ${currentSlide.video}`);
        setTimeout(() => {
            slideVideoEl.style.opacity = '0';
            slideTextEl.style.opacity = '0';
            setTimeout(() => {
                slideIndex++;
                isAnimating = false;
                showNextSlide();
            }, 3000);
        }, 2000);
    };
  }

  function goToFinal() {
    phase = 3;
    clearTimeout(inactivityTimer);
    clearTimeout(resumeTimer);
    
    slideVideoEl.style.opacity = '0';
    slideTextEl.style.opacity = '0';

    setTimeout(() => {
        slideVideoEl.classList.add('hidden');
        slideVideoEl.pause();
        
        finalImgEl.classList.remove('hidden');
        finalImgEl.style.opacity = '1';
        
        manifestoEl.classList.remove('hidden');
        manifestoEl.style.opacity = '1';
        document.body.classList.add('final-bg');
    }, 3000);
  }

  // Clic global pour revenir à l'intro si on bouge trop pendant les slides
  document.addEventListener('click', (e) => {
    if (e.target.id === 'startScreen' || e.target.id === 'errorMsg') return;

    if (phase === 2) {
      // Retour à l'intro
      resetInactivityTimer();
      isPausedByInactivity = false;
      
      slideVideoEl.pause();
      slideVideoEl.style.opacity = '0';
      slideTextEl.style.opacity = '0';
      clearTimeout(slideTimer);

      slideIndex = 0;
      isAnimating = false;

      slideVideoEl.classList.add('hidden');
      slideTextEl.classList.add('hidden');
      
      introEl.classList.remove('hidden');
      introEl.style.opacity = '1';
      phase = 1;

      setTimeout(() => {
        introEl.style.opacity = '0';
        setTimeout(() => {
          introEl.classList.add('hidden');
          phase = 2;
          resetInactivityTimer();
          showNextSlide();
        }, 1500);
      }, 5000);
    }
  });

</script>
</body>
</html>
