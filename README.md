<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Sessions utilisateurs</title>
  <style>
    :root{
      --bg:#0f172a;
      --panel:#111827;
      --muted:#94a3b8;
      --text:#e5e7eb;
      --accent:#22c55e;    /* vert Start */
      --danger:#ef4444;    /* rouge Stop */
      --card:#0b1220;
      --border:#1f2937;
      --shadow: 0 10px 25px rgba(0,0,0,0.35);
      --radius: 14px;
      --radius-sm: 10px;
      --focus: 0 0 0 3px rgba(34,197,94,.35);
      font-synthesis-weight: none;
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0;
      font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, "Helvetica Neue", Arial, "Noto Sans", "Apple Color Emoji","Segoe UI Emoji";
      background: radial-gradient(1200px 800px at 20% -10%, #1b2a52 0%, rgba(15,23,42,0) 60%) no-repeat,
                  radial-gradient(900px 700px at 110% 10%, #153652 0%, rgba(15,23,42,0) 50%) no-repeat,
                  var(--bg);
      color: var(--text);
      line-height:1.5;
    }

    .container{
      max-width: 960px;
      margin: 48px auto;
      padding: 24px;
    }

    .header{
      display:flex; align-items:center; gap:14px; margin-bottom:28px;
    }
    .logo{
      width:38px; height:38px; border-radius:10px;
      background: linear-gradient(135deg,#22c55e 0%, #16a34a 56%, #0ea5e9 120%);
      box-shadow: 0 6px 18px rgba(34,197,94,.35), inset 0 0 18px rgba(255,255,255,.15);
    }
    h1{font-size: clamp(22px,3vw,28px); margin:0;}
    .sub{color:var(--muted); font-size:14px; margin-top:2px;}

    .panel{
      background: linear-gradient(180deg, rgba(255,255,255,.02), rgba(255,255,255,.005)) , var(--panel);
      border:1px solid var(--border);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      padding: 18px;
    }

    /* Ecran 1 : liste des utilisateurs */
    .users{
      display:grid;
      grid-template-columns: repeat(auto-fit, minmax(220px,1fr));
      gap:16px;
      margin-top:12px;
    }
    .user-card{
      background: linear-gradient(180deg, rgba(34,197,94,.06), rgba(34,197,94,0) 60%), var(--card);
      border:1px solid var(--border);
      border-radius: var(--radius-sm);
      padding:16px;
      display:flex; align-items:center; gap:14px;
      cursor:pointer;
      transition: transform .12s ease, border-color .12s ease, box-shadow .12s ease;
      width: 100%;
      text-align: left;
    }
    .user-card:hover{
      transform: translateY(-2px);
      border-color: #2563eb55;
      box-shadow: 0 8px 18px rgba(0,0,0,.28);
    }
    .avatar{
      width:44px; height:44px; border-radius:12px; flex:none;
      background: linear-gradient(135deg, #0ea5e9, #22c55e);
      display:grid; place-items:center;
      color:white; font-weight:700;
      letter-spacing:.3px;
      box-shadow: inset 0 0 16px rgba(255,255,255,.2);
    }
    /* Changement demandé : écriture en blanc pour le nom de l'utilisateur */
    .user-info strong{display:block; font-size:16px; color: #ffffff;}
    .user-info span{display:block; font-size:12px; color:var(--muted)}

    /* Ecran 2 : profil + bouton Start */
    .profile-head{
      display:flex; align-items:center; justify-content:space-between; gap:12px;
      padding:10px 2px 14px;
      border-bottom:1px dashed var(--border);
      margin-bottom:14px;
    }
    .back{
      appearance:none; border:1px solid var(--border); background:transparent; color:var(--text);
      border-radius:10px; padding:8px 12px; cursor:pointer; font-weight:600; font-size:14px;
    }
    .back:hover{border-color:#334155}
    .pill{
      color:var(--muted); font-size:13px;
    }

    .actions{display:flex; gap:10px; flex-wrap:wrap}

    .btn{
      appearance:none; border:none; cursor:pointer; font-weight:800; letter-spacing:.2px;
      padding:14px 18px; border-radius:12px; color:white;
      transition: transform .08s ease, box-shadow .12s ease, filter .1s ease;
      display:inline-flex; align-items:center; gap:10px;
    }
    .btn:active{transform: translateY(1px); filter: saturate(1.1)}
    .btn-start{background: linear-gradient(135deg, #22c55e, #16a34a);}
    .btn-start:focus{outline:none; box-shadow: var(--focus)}
    .btn-stop{background: linear-gradient(135deg, #ef4444, #dc2626);}
    .btn-stop:focus{outline:none; box-shadow: 0 0 0 3px rgba(239,68,68,.35)}

    /* Ecran 3 : session & Bilan */
    .session{
      display:grid; gap:14px;
      text-align:center;
      padding-top:8px;
    }
    .title{font-size:22px; font-weight:800;}
    .timer{
      font-variant-numeric: tabular-nums;
      font-feature-settings: "tnum";
      font-size: clamp(38px,6vw,54px);
      font-weight:900;
      letter-spacing: 1px;
      background: linear-gradient(180deg, #ffffff, #94a3b8);
      -webkit-background-clip:text; background-clip:text; color: transparent;
      text-shadow: 0 8px 24px rgba(15,23,42,.65);
      margin: 8px 0 2px;
    }
    .meta{
      color:var(--muted); font-size:13px; margin-bottom:4px;
    }

    /* Styles spécifiques pour la grille du bilan final */
    .summary-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
      gap: 16px;
      margin: 24px 0;
    }
    .summary-card {
      background: rgba(2, 6, 23, 0.4);
      border: 1px solid var(--border);
      border-radius: var(--radius-sm);
      padding: 20px;
    }
    .summary-val {
      font-size: 28px;
      font-weight: 800;
      color: #ffffff;
      margin-top: 4px;
    }
    .summary-unit {
      font-size: 14px;
      color: var(--muted);
    }

    /* Layout states */
    .hidden{display:none !important}
  </style>
</head>
<body>
  <div class="container">
    <div class="header">
      <div class="logo" aria-hidden="true"></div>
      <div>
        <h1>Gestion de sessions</h1>
        <div class="sub">Sélectionnez un utilisateur, démarrez une session, arrêtez-la pour voir la durée.</div>
      </div>
    </div>

    <section id="view-users" class="panel" aria-labelledby="users-title">
      <h2 id="users-title" style="margin:0 0 6px 0; font-size:18px">Utilisateurs enregistrés</h2>
      <div class="users" role="list">
        <button class="user-card" role="listitem" data-id="user_1" data-user="Utilisateur 1" aria-label="Ouvrir le profil Utilisateur 1">
          <div class="avatar">U1</div>
          <div class="user-info">
            <strong>Utilisateur 1</strong>
            <span>Profil démo</span>
          </div>
        </button>

        <button class="user-card" role="listitem" data-id="user_2" data-user="Utilisateur 2" aria-label="Ouvrir le profil Utilisateur 2">
          <div class="avatar">U2</div>
          <div class="user-info">
            <strong>Utilisateur 2</strong>
            <span>Profil démo</span>
          </div>
        </button>

        <button class="user-card" role="listitem" data-id="user_3" data-user="Utilisateur 3" aria-label="Ouvrir le profil Utilisateur 3">
          <div class="avatar">U3</div>
          <div class="user-info">
            <strong>Utilisateur 3</strong>
            <span>Profil démo</span>
          </div>
        </button>
      </div>
    </section>

    <section id="view-profile" class="panel hidden" aria-labelledby="profile-title">
      <div class="profile-head">
        <div style="display:flex; align-items:center; gap:10px">
          <button class="back" id="back-to-users" aria-label="Retour à la liste">← Retour</button>
          <div>
            <h2 id="profile-title" style="margin:0; font-size:18px"></h2>
            <div class="pill">Sélection du profil</div>
          </div>
        </div>
        <div class="actions">
          <button id="btn-start" class="btn btn-start" aria-label="Démarrer une nouvelle session">
            ▶ Start
          </button>
        </div>
      </div>

      <div class="session" id="session-block" aria-live="polite" aria-atomic="true">
        <div class="title">Nouvelle session</div>
        <div class="timer" id="timer">00:00:00</div>
        <div class="meta" id="session-meta">En attente de démarrage…</div>
        
        <div class="hidden">
          <span id="userName">-</span> | Statut: <span id="status">-</span> | Débit: <span id="flow">0</span> L/min
          <span id="volume">0</span> | <span id="duration">0</span> | <span id="price">0</span>
        </div>

        <div class="actions" style="justify-content:center">
          <button id="btn-stop" class="btn btn-stop" disabled aria-disabled="true" aria-label="Arrêter la session">■ Stop</button>
        </div>
      </div>
    </section>

    <section id="view-summary" class="panel hidden" aria-labelledby="summary-title">
      <div class="profile-head">
        <div style="display:flex; align-items:center; gap:10px">
          <button class="back" id="btn-summary-close" aria-label="Retourner à l'accueil">Fermer le bilan</button>
          <div>
            <h2 id="summary-title" style="margin:0; font-size:18px">Bilan de la session</h2>
            <div class="pill" id="summary-user-name">Utilisateur</div>
          </div>
        </div>
      </div>

      <div class="session">
        <div class="summary-grid">
          <div class="summary-card">
            <div class="summary-unit">Volume consommé</div>
            <div class="summary-val"><span id="sum-volume">0</span> <small style="font-size:16px">L</small></div>
          </div>
          <div class="summary-card">
            <div class="summary-unit">Durée de session</div>
            <div class="summary-val" id="sum-duration" style="font-size:24px">00:00:00</div>
          </div>
          <div class="summary-card">
            <div class="summary-unit">Prix estimé</div>
            <div class="summary-val"><span id="sum-price">0</span> <small style="font-size:16px">€</small></div>
          </div>
        </div>
      </div>
    </section>
  </div>

  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/12.13.0/firebase-app.js";
    import {
      getDatabase,
      ref,
      set,
      get,
      push,
      onValue
    } from "https://www.gstatic.com/firebasejs/12.13.0/firebase-database.js";

    const firebaseConfig = {
      apiKey: "AIzaSyA59CVLHSUyBqs_l1Zka6trBue6J01D1bc",
      authDomain: "projet-eau-84965.firebaseapp.com",
      databaseURL: "https://projet-eau-84965-default-rtdb.europe-west1.firebasedatabase.app",
      projectId: "projet-eau-84965",
      storageBucket: "projet-eau-84965.firebasestorage.app",
      messagingSenderId: "906648411400",
      appId: "1:906648411400:web:65497cacf365588606e0bd"
    };

    // Initialisation
    const app = initializeApp(firebaseConfig);
    const database = getDatabase(app);
    console.log("Firebase initialisé avec succès");

    // Éléments du DOM
    const viewUsers = document.getElementById('view-users');
    const viewProfile = document.getElementById('view-profile');
    const viewSummary = document.getElementById('view-summary');
    const profileTitle = document.getElementById('profile-title');
    const summaryUserName = document.getElementById('summary-user-name');
    const backBtn = document.getElementById('back-to-users');
    const summaryCloseBtn = document.getElementById('btn-summary-close');

    const btnStart = document.getElementById('btn-start');
    const btnStop = document.getElementById('btn-stop');
    const timerEl = document.getElementById('timer');
    const metaEl = document.getElementById('session-meta');

    // Éléments spécifiques à l'Écran 3 (Bilan)
    const sumVolume = document.getElementById('sum-volume');
    const sumDuration = document.getElementById('sum-duration');
    const sumPrice = document.getElementById('sum-price');

    // State de l'application
    let currentUserId = null;
    let currentUserName = null;
    let startTime = null;
    let timerId = null;
    let latestSessionData = null; // Stocke les données Firebase en direct

    // Fonctions d'aide (Formatage)
    function pad(n){ return n.toString().padStart(2,'0'); }
    function formatHMS(ms){
      const total = Math.floor(ms/1000);
      const h = Math.floor(total/3600);
      const m = Math.floor((total%3600)/60);
      const s = total%60;
      return `${pad(h)}:${pad(m)}:${pad(s)}`;
    }

    function show(el){ el.classList.remove('hidden'); }
    function hide(el){ el.classList.add('hidden'); }

    function resetSessionUI(){
      timerEl.textContent = '00:00:00';
      metaEl.textContent = 'En attente de démarrage…';
      btnStop.disabled = true;
      btnStop.setAttribute('aria-disabled','true');
    }

    function navigateToProfile(id, name){
      currentUserId = id;
      currentUserName = name;
      profileTitle.textContent = name;
      hide(viewUsers);
      hide(viewSummary);
      show(viewProfile);
      resetSessionUI();
    }

    function navigateToUsers(){
      show(viewUsers);
      hide(viewProfile);
      hide(viewSummary);
      stopTimer(true);
      currentUserId = null;
      currentUserName = null;
    }

    // Gestion du Timer local visuel
    function startTimer(){
      if(timerId) return;
      startTime = Date.now();
      metaEl.textContent = 'Session en cours…';
      btnStop.disabled = false;
      btnStop.setAttribute('aria-disabled','false');
      
      timerId = setInterval(()=>{
        const elapsed = Date.now() - startTime;
        timerEl.textContent = formatHMS(elapsed);
      }, 250);

      // Action Firebase : Démarrer la douche
      startShower(currentUserId, currentUserName);
    }

    function stopTimer(silent=false){
      if(timerId){
        clearInterval(timerId);
        timerId = null;
      }
      if(!startTime) return;
      
      const elapsed = Date.now() - startTime;
      const finalStr = formatHMS(elapsed);
      timerEl.textContent = finalStr;
      startTime = null;
      btnStop.disabled = true;
      btnStop.setAttribute('aria-disabled','true');

      if (!silent) {
        // Déclencher la fin sur Firebase et basculer sur l'Écran 3 (Bilan)
        stopShower(finalStr);
      }
    }

    // FONCTIONS FIREBASE SYNCHRONISÉES
    function startShower(userId, userName) {
      set(ref(database, "currentSession"), {
        status: "running",
        userId: userId,
        userName: userName,
        flowLMin: 0,
        volumeLitres: 0,
        durationSeconds: 0,
        priceEuro: 0,
        startTime: new Date().toISOString()
      })
      .then(() => { console.log("Douche démarrée pour", userName); })
      .catch((error) => { console.error("Erreur démarrage :", error); });
    }

    async function stopShower(finalTimerStr) {
      const currentSessionRef = ref(database, "currentSession");
      const snapshot = await get(currentSessionRef);
      const session = snapshot.val();

      if (!session || session.status !== "running") {
        alert("Aucune douche en cours.");
        return;
      }

      const finishedSession = {
        ...session,
        status: "finished",
        endTime: new Date().toISOString()
      };

      // Pousser dans l'historique global
      await push(ref(database, "sessions"), finishedSession);

      // Préparation de l'Écran 3 (Bilan) avec les données de Firebase ou du fallback local
      sumVolume.textContent = session.volumeLitres || 0;
      sumPrice.textContent = session.priceEuro || 0;
      sumDuration.textContent = finalTimerStr; // Récupère le format propre HH:MM:SS du chrono
      summaryUserName.textContent = session.userName || "Utilisateur";

      // Bascule sur l'Écran 3
      hide(viewProfile);
      show(viewSummary);

      // Reset de l'état en cours dans Firebase
      await set(currentSessionRef, {
        status: "stopped",
        userId: "",
        userName: "",
        flowLMin: 0,
        volumeLitres: 0,
        durationSeconds: 0,
        priceEuro: 0
      });

      console.log("Douche terminée, enregistrée, et redirection vers le bilan réussie.");
    }

    // Écouteur Firebase en temps réel pour lier l'interface
    onValue(ref(database, "currentSession"), (snapshot) => {
      const data = snapshot.val();
      if (!data) return;
      latestSessionData = data; // Sauvegarde locale en continu des volumes/prix captés

      // Remplissage des éléments fantômes du DOM demandés dans votre code d'origine
      document.getElementById("userName").textContent = data.userName || "-";
      document.getElementById("status").textContent = data.status || "-";
      document.getElementById("flow").textContent = data.flowLMin || 0;
      document.getElementById("volume").textContent = data.volumeLitres || 0;
      document.getElementById("duration").textContent = data.durationSeconds || 0;
      document.getElementById("price").textContent = data.priceEuro || 0;
    });

    // Événements : Clics utilisateurs Écran 1
    document.querySelectorAll('.user-card').forEach(card=>{
      card.addEventListener('click', ()=>{
        const id = card.getAttribute('data-id');
        const name = card.getAttribute('data-user');
        navigateToProfile(id, name);
      });
    });

    // Événements : Boutons Navigation / Actions
    backBtn.addEventListener('click', navigateToUsers);
    summaryCloseBtn.addEventListener('click', navigateToUsers);
    btnStart.addEventListener('click', startTimer);
    btnStop.addEventListener('click', () => stopTimer(false));
  </script>
</body>
</html>
