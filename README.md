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
/* === RESET DE BASE === */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Georgia', serif;
}

body {
  background-color: #f9f9f9;
  color: #1a1a1a;
}

/* === TOPBAR === */
.topbar {
  width: 100%;
  background: linear-gradient(to right, #08213a, #0a2a4d);
  padding: 0.5rem 1rem;
  display: flex;
  justify-content: flex-end;
  align-items: center;
}

.language-select select {
  background: #f0f0f0;
  border: none;
  padding: 0.4rem 0.7rem;
  border-radius: 8px;
  font-weight: bold;
  cursor: pointer;
}

/* === HERO BANNER === */
.hero-banner {
  background: url('assets/images/hero_background.jpg') center/cover no-repeat;
  padding: 4rem 1rem;
  text-align: center;
  color: white;
  position: relative;
}

.hero-logo {
  width: 120px;
  margin-bottom: 1rem;
}

.hero-title {
  font-size: 2.8rem;
  font-family: 'Playfair Display', serif;
  font-weight: 700;
  letter-spacing: 1px;
}

.hero-subtitle {
  font-size: 1.2rem;
  color: #ddd;
  margin-top: 0.5rem;
}

/* === SECTION PRODUITS === */
.products-section {
  padding: 2rem 1rem;
  max-width: 1200px;
  margin: auto;
}

.section-title {
  font-size: 1.8rem;
  margin-bottom: 1.2rem;
  font-family: 'Playfair Display', serif;
  text-align: center;
  color: #08213a;
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 2rem;
}

.product-card {
  background: white;
  border-radius: 16px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  overflow: hidden;
  transition: transform 0.3s ease;
  cursor: pointer;
}

.product-card:hover {
  transform: translateY(-6px);
}

.product-image {
  width: 100%;
  height: 240px;
  object-fit: cover;
}

.product-info {
  padding: 1rem;
  text-align: center;
}

.product-name {
  font-weight: bold;
  font-size: 1.1rem;
  color: #0a2a4d;
}

.product-price {
  margin-top: 0.3rem;
  font-size: 1rem;
  color: #555;
}

.badge-new {
  background: #08213a;
  color: white;
  padding: 0.2rem 0.5rem;
  border-radius: 6px;
  font-size: 0.75rem;
  position: absolute;
  top: 12px;
  left: 12px;
}

/* === FOOTER === */
.site-footer {
  text-align: center;
  padding: 1rem;
  margin-top: 3rem;
  background-color: #0a2a4d;
}

.settings-btn {
  padding: 0.6rem 1.2rem;
  font-size: 1rem;
  border: none;
  background: #f0f0f0;
  border-radius: 10px;
  cursor: pointer;
  font-weight: bold;
}

.settings-btn:hover {
  background-color: #ddd;
}
// === script.js ===

// 1. Importation de Firebase
import { db } from './firebase-config.js';
import {
  collection,
  getDocs,
  query,
  orderBy
} from 'https://www.gstatic.com/firebasejs/10.7.0/firebase-firestore.js';

// 2. Fonction pour charger les produits
async function loadProducts() {
  const productsRef = collection(db, 'products');
  const q = query(productsRef, orderBy("name"));
  const querySnapshot = await getDocs(q);

  const grid = document.getElementById('productsGrid');
  grid.innerHTML = ""; // On vide avant d'ajouter

  querySnapshot.forEach((doc) => {
    const product = doc.data();

    const card = document.createElement('div');
    card.classList.add('product-card');

    // Gestion de l'image de survol (hover)
    const img = document.createElement('img');
    img.className = 'product-image';
    img.src = product.imageUrl;
    img.alt = product.name;

    if (product.hoverImageUrl) {
      img.addEventListener('mouseover', () => {
        img.src = product.hoverImageUrl;
      });
      img.addEventListener('mouseout', () => {
        img.src = product.imageUrl;
      });
    }

    const info = document.createElement('div');
    info.className = 'product-info';

    const name = document.createElement('div');
    name.className = 'product-name';
    name.textContent = product.name;

    const price = document.createElement('div');
    price.className = 'product-price';
    if (product.flashSale?.isActive) {
      price.innerHTML = `<span style="text-decoration: line-through; color: #888;">${product.originalPrice.toLocaleString()} FCFA</span>
        <br><strong style="color: darkred">${product.flashSale.salePrice.toLocaleString()} FCFA</strong>`;
    } else {
      price.textContent = ${product.originalPrice.toLocaleString()} FCFA;
    }

    info.appendChild(name);
    info.appendChild(price);

    if (product.isNew) {
      const badge = document.createElement('div');
      badge.className = 'badge-new';
      badge.textContent = 'Nouveau';
      card.appendChild(badge);
    }

    card.appendChild(img);
    card.appendChild(info);
    grid.appendChild(card);
  });
}

// 3. Détecte le chargement de la page
window.addEventListener('DOMContentLoaded', () => {
  loadProducts();
});
