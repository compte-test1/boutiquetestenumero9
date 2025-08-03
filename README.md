// === firebase-config.js ===

import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.0/firebase-app.js";
import { getFirestore } from "https://www.gstatic.com/firebasejs/10.7.0/firebase-firestore.js";
import { getAuth } from "https://www.gstatic.com/firebasejs/10.7.0/firebase-auth.js";
import { getStorage } from "https://www.gstatic.com/firebasejs/10.7.0/firebase-storage.js";

const firebaseConfig = {
  apiKey: "AIzaSyA5nLBZ3E4t70TRj1woslLS6rIo5hrEoSY",
  authDomain: "ng-chopi-diams-824fa.firebaseapp.com",
  projectId: "ng-chopi-diams-824fa",
  storageBucket: "ng-chopi-diams-824fa.firebasestorage.app",
  messagingSenderId: "18876016875",
  appId: "1:18876016875:web:ad2a6f936aae06096ff6ae"
};

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);
const auth = getAuth(app);
const storage = getStorage(app);

export { db, auth, storage };

<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>NG_chopi_DIAMS</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>

    <!-- PARAMÈTRES (LANGUE + ADMIN) -->
    <header class="topbar">
      <div class="language-select">
        <select id="languageSelector">
          <option value="fr">🇫🇷 Français</option>
          <option value="en">🇬🇧 English</option>
          <option value="ln">🇨🇬🇨🇩 Lingala</option>
        </select>
      </div>
    </header>

    <!-- BANNIÈRE HERO -->
    <section class="hero-banner">
      <div class="hero-content">
        <img src="assets/images/logo_ng_chopi_diams.png" alt="NG_chopi_DIAMS Logo" class="hero-logo" />
        <h1 class="hero-title">L'élégance à l'état pur</h1>
        <p class="hero-subtitle">Bijoux et tenues de luxe en ligne</p>
      </div>
    </section>

    <!-- SECTION PRODUITS -->
    <main class="products-section">
      <h2 class="section-title">Nos Produits</h2>
      <div id="productsGrid" class="products-grid">
        <!-- Les produits seront injectés ici dynamiquement -->
      </div>
    </main>

    <!-- BOUTON PARAMÈTRES EN BAS -->
    <footer class="site-footer">
      <button id="openSettings" class="settings-btn">⚙ Paramètres</button>
    </footer>

    <!-- SCRIPT -->
    <script type="module" src="script.js"></script>
  </body>
</html>
