<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>NG_chopi_DIAMS - Boutique de Luxe</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="styles.css" />
</head>
<body class="bg-gray-900 text-gray-100">
  <!-- Header -->
  <header class="bg-gradient-to-r from-black via-gray-900 to-black border-b border-yellow-400 sticky top-0 z-50">
    <div class="container mx-auto px-4 py-4 flex justify-between items-center">
      <div class="flex items-center space-x-2">
        <h1 id="siteTitle" class="text-3xl font-bold gold-text">NG_chopi</h1>
        <span id="siteSubtitle" class="text-yellow-400 font-semibold">DIAMS</span>
      </div>
      <nav class="hidden md:flex space-x-6">
        <a href="#" id="navNew" class="text-gray-300 hover:text-yellow-400">Nouveautés</a>
        <a href="#" id="navWomen" class="text-gray-300 hover:text-yellow-400">Femmes</a>
        <a href="#" id="navMen" class="text-gray-300 hover:text-yellow-400">Hommes</a>
        <a href="#" id="navCollection" class="text-yellow-400 font-bold">COLLECTION</a>
      </nav>
      <div class="flex items-center space-x-4">
        <button id="settingsBtn" class="text-yellow-400 hover:text-yellow-300" title="Paramètres">⚙️</button>
        <button id="adminBtn" class="text-yellow-400 hover:text-yellow-300" title="Admin">🛠️</button>
        <button id="cartBtn" class="relative text-yellow-400 hover:text-yellow-300" title="Panier">🛒
          <span id="cartCount" class="absolute -top-2 -right-2 bg-yellow-400 text-black text-xs rounded-full h-5 w-5 flex items-center justify-center font-bold hidden">0</span>
        </button>
      </div>
    </div>
  </header>

  <!-- Hero -->
  <section class="bg-gradient-to-r from-yellow-400 to-yellow-600 text-black py-16 text-center">
    <div class="container mx-auto px-4">
      <h2 id="heroTitle" class="text-4xl md:text-5xl font-bold mb-4">COLLECTION EXCLUSIVE</h2>
      <p id="heroSubtitle" class="text-xl mb-8">Articles de luxe à prix exceptionnels</p>
      <button id="heroButton" class="bg-black text-yellow-400 px-8 py-3 rounded-full font-bold hover:bg-gray-900 transition">Découvrir</button>
    </div>
  </section>

  <!-- Produits -->
  <section class="container mx-auto px-4 py-12">
    <div id="productsContainer" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-8"></div>
  </section>

  <!-- Paramètres -->
  <div id="settingsPanel" class="fixed inset-0 bg-gray-900 bg-opacity-90 z-50 hidden">
    <div class="absolute right-0 top-0 h-full w-full max-w-md bg-gray-800/90 border-l border-yellow-400 overflow-y-auto"
         style="background-image: url('https://placehold.co/600x1000/111827/FFF?text=NG_chopi_DIAMS&font=montserrat'); background-blend-mode: overlay; background-size: cover;">
      <div class="p-6">
        <div class="flex justify-between items-center mb-6">
          <h2 id="settingsTitle" class="text-xl font-bold text-yellow-400">Paramètres</h2>
          <button id="closeSettings" class="text-yellow-400 hover:text-yellow-300">✖️</button>
        </div>
        <div class="space-y-6">
          <div>
            <h3 id="languageLabel" class="text-lg font-semibold text-yellow-400 mb-3">Langue</h3>
            <select id="languageSelector" class="w-full language-selector text-white rounded-lg px-4 py-2">
              <option value="fr">Français</option>
              <option value="en">English</option>
              <option value="ln">Lingala</option>
            </select>
          </div>
          <button id="adminAccessBtn" class="w-full bg-gradient-to-r from-yellow-400 to-yellow-600 text-black py-3 rounded-lg font-bold">Accès Administrateur</button>
        </div>
      </div>
    </div>
  </div>

  <!-- Panier -->
  <div id="cartOverlay" class="fixed inset-0 bg-black bg-opacity-75 z-50 hidden">
    <div class="absolute right-0 top-0 h-full w-full max-w-md bg-gray-900 border-l border-yellow-400 overflow-y-auto">
      <div class="p-6">
        <div class="flex justify-between items-center mb-6">
          <h2 id="cartTitle" class="text-xl font-bold text-yellow-400">Votre Panier</h2>
          <button id="closeCart" class="text-yellow-400 hover:text-yellow-300">✖️</button>
        </div>
        <div id="cartItems" class="space-y-4 mb-6">
          <p class="text-gray-400 text-center py-12" id="cartEmpty">Votre panier est vide</p>
        </div>
        <div class="border-t border-yellow-400/30 pt-6">
          <div class="flex justify-between items-center mb-6">
            <span id="cartTotalLabel" class="text-lg font-semibold">Total:</span>
            <span id="cartTotal" class="text-xl font-bold gold-text">0 FCFA</span>
          </div>
          <div id="clientForm" class="hidden">
            <div class="mb-4">
              <label id="clientNameLabel" class="block text-yellow-400 mb-2">Nom et Prénom</label>
              <input type="text" id="clientName" class="w-full bg-gray-800 border border-yellow-400/30 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-yellow-400" required />
            </div>
            <div class="mb-4">
              <label id="clientPhoneLabel" class="block text-yellow-400 mb-2">Numéro de téléphone</label>
              <input type="tel" id="clientPhone" class="w-full bg-gray-800 border border-yellow-400/30 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-yellow-400" required />
            </div>
            <div class="mb-4">
              <label id="clientCityLabel" class="block text-yellow-400 mb-2">Ville</label>
              <input type="text" id="clientCity" class="w-full bg-gray-800 border border-yellow-400/30 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-yellow-400" required />
            </div>
            <div class="mb-4">
              <label id="clientNeighborhoodLabel" class="block text-yellow-400 mb-2">Quartier</label>
              <input type="text" id="clientNeighborhood" class="w-full bg-gray-800 border border-yellow-400/30 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-yellow-400" required />
            </div>
            <div class="mb-6">
              <label id="clientStreetLabel" class="block text-yellow-400 mb-2">Rue et Numéro</label>
              <input type="text" id="clientStreet" class="w-full bg-gray-800 border border-yellow-400/30 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-yellow-400" required />
            </div>
            <button id="confirmOrderBtn" class="w-full bg-gradient-to-r from-green-500 to-green-600 text-white py-3 rounded-lg font-bold hover:opacity-90 transition">Confirmer la commande</button>
          </div>
          <button id="validateOrderBtn" class="w-full bg-gradient-to-r from-yellow-400 to-yellow-600 text-black py-3 rounded-lg font-bold hover:opacity-90 transition">Valider votre commande</button>
        </div>
      </div>
    </div>
  </div>

  <!-- Espace Administrateur -->
  <div id="adminPanel" class="fixed inset-0 bg-gray-900 z-50 overflow-y-auto hidden">
    <div class="container mx-auto px-4 py-12">
      <div class="flex justify-between items-center mb-12">
        <h1 id="adminPanelTitle" class="text-3xl font-bold gold-text">Panel d'Administration</h1>
        <button id="closeAdmin" class="text-yellow-400 hover:text-yellow-300 px-4 py-2 border border-yellow-400 rounded-lg">Fermer</button>
      </div>
      <div class="bg-gray-800 rounded-lg p-6 mb-8">
        <h2 id="productManagementTitle" class="text-xl font-bold text-yellow-400 mb-4">Gestion des Produits</h2>
        <button id="addProductBtn" class="bg-green-500 text-white px-4 py-2 rounded-lg font-bold mb-4">Nouveau produit</button>
        <form id="adminProductForm" class="mb-6">
          <input type="hidden" id="productId" />
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
            <div>
              <label id="productNameLabel" class="block text-yellow-400 mb-2">Nom du produit</label>
              <input type="text" id="productName" class="w-full bg-gray-800 border border-yellow-400/30 rounded-lg px-4 py-2" required />
            </div>
            <div>
              <label id="productCategoryLabel" class="block text-yellow-400 mb-2">Catégorie</label>
              <select id="productCategory" class="w-full bg-gray-800 border border-yellow-400/30 rounded-lg px-4 py-2" required>
                <option value="Robes">Robes</option>
                <option value="Hauts">Hauts</option>
                <option value="Accessoires">Accessoires</option>
              </select>
            </div>
          </div>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
            <div>
              <label id="productPriceLabel" class="block text-yellow-400 mb-2">Prix (FCFA)</label>
              <input type="number" id="productPrice" class="w-full bg-gray-800 border border-yellow-400/30 rounded-lg px-4 py-2" required />
            </div>
            <div>
              <label id="productOriginalPriceLabel" class="block text-yellow-400 mb-2">Prix original</label>
              <input type="number" id="productOriginalPrice" class="w-full bg-gray-800 border border-yellow-400/30 rounded-lg px-4 py-2" required />
            </div>
          </div>
          <div class="mb-4">
            <label id="productImageLabel" class="block text-yellow-400 mb-2">URL de l'image</label>
            <input type="text" id="productImage" class="w-full bg-gray-800 border border-yellow-400/30 rounded-lg px-4 py-2 mb-2" required />
          </div>
          <div class="flex space-x-3">
            <button id="saveProductBtn" type="submit" class="bg-yellow-400 text-black px-6 py-2 rounded-lg font-bold">Enregistrer</button>
            <button id="cancelEdit" type="button" class="bg-gray-700 text-white px-6 py-2 rounded-lg font-bold">Annuler</button>
          </div>
        </form>
        <div class="bg-gray-900 rounded-lg p-4 mb-4">
          <h3 id="productListTitle" class="text-lg font-semibold text-yellow-400 mb-3">Liste des produits</h3>
          <div id="adminProductsList" class="space-y-3"></div>
        </div>
      </div>
    </div>
  </div>

<script>
document.addEventListener('DOMContentLoaded', function() {
// --------- Données Produits ---------
let products = [
  {
    id: 1,
    name: "Robe de Soirée Élégante",
    price: 75000,
    originalPrice: 95000,
    image: "https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/4f6e42aa-1393-409a-8d75-35dc173309c3.png",
    category: "Robes",
    rating: 4.8,
  },
  {
    id: 2,
    name: "Top Couture Premium",
    price: 45000,
    originalPrice: 65000,
    image: "https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/a1a416cf-2ed9-4826-b0fe-b0bf9ac24fba.png",
    category: "Hauts",
    rating: 4.6,
  },
  {
    id: 3,
    name: "Robe Cocktail Diamants",
    price: 89000,
    originalPrice: 120000,
    image: "https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/dc732eb3-2b4e-46f9-bca1-86c42649759c.png",
    category: "Robes",
    rating: 4.9,
  },
  {
    id: 4,
    name: "Montre Prestige Or",
    price: 125000,
    originalPrice: 180000,
    image: "https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/58eefe4f-a865-4b1a-b96a-89a1659638b2.png",
    category: "Accessoires",
    rating: 5.0,
  },
];

// --------- Panier ---------
let cart = [];

// --------- Affichage Produits ---------
function displayProducts() {
  const container = document.getElementById('productsContainer');
  container.innerHTML = '';
  products.forEach(product => {
    const discount = Math.round((1 - product.price / product.originalPrice) * 100);
    const div = document.createElement('div');
    div.className = 'bg-gray-800 rounded-xl overflow-hidden border border-yellow-400/20 product-card transition duration-300';
    div.innerHTML = `
      <div class="relative">
        <img src="${product.image}" alt="${product.name}" class="w-full h-64 object-cover" />
        ${discount > 0 ? `<span class="absolute top-3 left-3 bg-yellow-400 text-black px-2 py-1 rounded text-xs font-bold">-${discount}%</span>` : ''}
      </div>
      <div class="p-4">
        <h3 class="font-semibold mb-2">${product.name}</h3>
        <div class="flex items-center mb-3">
          ${Array(5).fill().map((_, i) => `<svg class="w-4 h-4 ${i < Math.floor(product.rating) ? 'text-yellow-400' : 'text-gray-600'}" fill="currentColor" viewBox="0 0 24 24"><path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z"/></svg>`).join('')}
          <span class="text-xs text-gray-400 ml-2">${product.rating}</span>
        </div>
        <div class="flex justify-between items-center">
          <div>
            <p class="text-yellow-400 font-bold">${product.price.toLocaleString()} FCFA</p>
            ${product.price < product.originalPrice ? `<p class="text-gray-500 text-sm line-through">${product.originalPrice.toLocaleString()} FCFA</p>` : ''}
          </div>
          <button class="bg-yellow-400 hover:bg-yellow-500 text-black px-3 py-1 rounded text-sm add-to-cart" data-id="${product.id}">Ajouter</button>
        </div>
      </div>
    `;
    container.appendChild(div);
  });
}

// --------- Mise à jour Panier ---------
function updateCart() {
  const cartItems = document.getElementById('cartItems');
  cartItems.innerHTML = '';
  if (cart.length === 0) {
    cartItems.innerHTML = '<p class="text-gray-400 text-center py-12">Votre panier est vide</p>';
    document.getElementById('cartCount').classList.add('hidden');
    document.getElementById('cartTotal').textContent = '0 FCFA';
    return;
  }
  document.getElementById('cartCount').classList.remove('hidden');
  let total = 0;
  cart.forEach(item => {
    total += item.price * item.quantity;
    const div = document.createElement('div');
    div.className = 'flex justify-between items-center';
    div.innerHTML = `<span>${item.name} (x${item.quantity})</span><span>${(item.price * item.quantity).toLocaleString()} FCFA</span>`;
    cartItems.appendChild(div);
  });
  document.getElementById('cartCount').textContent = cart.length;
  document.getElementById('cartTotal').textContent = total.toLocaleString() + ' FCFA';
}

// --------- Evenements Produits / Panier ---------
document.getElementById('productsContainer').addEventListener('click', (e) => {
  if (e.target.classList.contains('add-to-cart')) {
    const productId = parseInt(e.target.getAttribute('data-id'));
    const product = products.find(p => p.id === productId);
    if (!product) return;
    const cartItem = cart.find(item => item.id === productId);
    if (cartItem) {
      cartItem.quantity++;
    } else {
      cart.push({...product, quantity: 1});
    }
    updateCart();
  }
});
document.getElementById('cartBtn').addEventListener('click', () => {
  document.getElementById('cartOverlay').classList.remove('hidden');
  updateCart();
});
document.getElementById('closeCart').addEventListener('click', () => {
  document.getElementById('cartOverlay').classList.add('hidden');
});
document.getElementById('validateOrderBtn').addEventListener('click', () => {
  document.getElementById('validateOrderBtn').classList.add('hidden');
  document.getElementById('clientForm').classList.remove('hidden');
});
document.getElementById('confirmOrderBtn').addEventListener('click', () => {
  const clientName = document.getElementById('clientName').value.trim();
  const clientPhone = document.getElementById('clientPhone').value.trim();
  const clientCity = document.getElementById('clientCity').value.trim();
  const clientNeighborhood = document.getElementById('clientNeighborhood').value.trim();
  const clientStreet = document.getElementById('clientStreet').value.trim();
  if (!clientName || !clientPhone || !clientCity || !clientNeighborhood || !clientStreet) {
    alert("Veuillez remplir tous les champs du formulaire");
    return;
  }
  if (cart.length === 0) {
    alert("Votre panier est vide");
    return;
  }
  const total = cart.reduce((sum, item) => sum + item.price * item.quantity, 0);
  const orderDetails = cart.map(item => `• ${item.name} (x${item.quantity}) - ${(item.price * item.quantity).toLocaleString()} FCFA`).join('\n');
  const message = `🌟 Nouvelle commande NG_chopi_DIAMS 🌟\n\nCOMMANDE PREMIUM:\n${orderDetails}\n\nTOTAL: ${total.toLocaleString()} FCFA\n\nCLIENT:\nNom: ${clientName}\nTéléphone: ${clientPhone}\nAdresse: ${clientCity}, ${clientNeighborhood}, ${clientStreet}\n\nMerci pour votre commande !`;
  const whatsappUrl = `https://wa.me/242064230404?text=${encodeURIComponent(message)}`;
  window.open(whatsappUrl, '_blank');
  cart = [];
  updateCart();
  document.getElementById('clientForm').classList.add('hidden');
  document.getElementById('validateOrderBtn').classList.remove('hidden');
  document.getElementById('cartOverlay').classList.add('hidden');
});

// --------- Admin ---------
document.getElementById('adminBtn').addEventListener('click', () => {
  const code = prompt("Entrez le code d'accès à l'espace administrateur:");
  if (code === "@inganibirds2007") {
    document.getElementById('adminPanel').classList.remove('hidden');
    loadAdminProducts();
  } else {
    alert("Code incorrect !");
  }
});
document.getElementById('closeAdmin').addEventListener('click', () => {
  document.getElementById('adminPanel').classList.add('hidden');
});
document.getElementById('addProductBtn').addEventListener('click', resetAdminForm);
document.getElementById('cancelEdit').addEventListener('click', resetAdminForm);
document.getElementById('adminProductForm').addEventListener('submit', function(e) {
  e.preventDefault();
  const id = document.getElementById('productId').value;
  const name = document.getElementById('productName').value.trim();
  const price = parseInt(document.getElementById('productPrice').value);
  const originalPrice = parseInt(document.getElementById('productOriginalPrice').value);
  const image = document.getElementById('productImage').value.trim();
  const category = document.getElementById('productCategory').value;
  if (!name || !price || !originalPrice || !image || !category) {
    alert("Veuillez remplir tous les champs");
    return;
  }
  if (id) {
    const index = products.findIndex(p => p.id == id);
    if (index !== -1) {
      products[index] = { id: Number(id), name, price, originalPrice, image, category, rating: 4.5 };
    }
  } else {
    products.push({ id: Date.now(), name, price, originalPrice, image, category, rating: 4.5 });
  }
  resetAdminForm();
  loadAdminProducts();
  displayProducts();
});
function resetAdminForm() {
  document.getElementById('productId').value = '';
  document.getElementById('productName').value = '';
  document.getElementById('productPrice').value = '';
  document.getElementById('productOriginalPrice').value = '';
  document.getElementById('productImage').value = '';
  document.getElementById('productCategory').value = 'Robes';
}
function loadAdminProducts() {
  const container = document.getElementById('adminProductsList');
  container.innerHTML = '';
  products.forEach(product => {
    const div = document.createElement('div');
    div.className = 'flex justify-between items-center bg-gray-800 p-3 rounded-lg';
    div.innerHTML = `
      <div>
        <h4 class="font-medium">${product.name}</h4>
        <p class="text-sm text-yellow-400">${product.price.toLocaleString()} FCFA</p>
      </div>
      <div class="flex space-x-2">
        <button class="admin-edit-btn text-yellow-400 hover:text-yellow-300" data-id="${product.id}">✏️</button>
        <button class="admin-delete-btn text-red-400 hover:text-red-300" data-id="${product.id}">🗑️</button>
      </div>
    `;
    container.appendChild(div);
  });
}
document.getElementById('adminProductsList').addEventListener('click', e => {
  if (e.target.classList.contains('admin-edit-btn')) {
    const id = parseInt(e.target.getAttribute('data-id'));
    const product = products.find(p => p.id === id);
    if (!product) return;
    document.getElementById('productId').value = product.id;
    document.getElementById('productName').value = product.name;
    document.getElementById('productPrice').value = product.price;
    document.getElementById('productOriginalPrice').value = product.originalPrice;
    document.getElementById('productImage').value = product.image;
    document.getElementById('productCategory').value = product.category;
  }
  if (e.target.classList.contains('admin-delete-btn')) {
    const id = parseInt(e.target.getAttribute('data-id'));
    if (confirm("Voulez-vous vraiment supprimer ce produit ?")) {
      products = products.filter(p => p.id !== id);
      loadAdminProducts();
      displayProducts();
    }
  }
});

// --------- Paramètres ---------
document.getElementById('settingsBtn').addEventListener('click', () => {
  document.getElementById('settingsPanel').classList.remove('hidden');
});
document.getElementById('closeSettings').addEventListener('click', () => {
  document.getElementById('settingsPanel').classList.add('hidden');
});
document.getElementById('adminAccessBtn').addEventListener('click', () => {
  document.getElementById('settingsPanel').classList.add('hidden');
  const code = prompt("Entrez le code d'accès à l'espace administrateur:");
  if (code === "@inganibirds2007") {
    document.getElementById('adminPanel').classList.remove('hidden');
    loadAdminProducts();
  } else {
    alert("Code incorrect !");
  }
});

// --------- Multilingue ---------
const translations = {
  fr: {
    siteTitle: "NG_chopi",
    siteSubtitle: "DIAMS",
    navNew: "Nouveautés",
    navWomen: "Femmes",
    navMen: "Hommes",
    navCollection: "COLLECTION",
    heroTitle: "COLLECTION EXCLUSIVE",
    heroSubtitle: "Articles de luxe à prix exceptionnels",
    heroButton: "Découvrir",
    cartEmpty: "Votre panier est vide",
    cartTotalLabel: "Total:",
    validateOrderBtn: "Valider votre commande",
    confirmOrderBtn: "Confirmer la commande",
    settingsTitle: "Paramètres",
    languageLabel: "Langue",
    adminAccessBtn: "Accès Administrateur",
    cartTitle: "Votre Panier",
    adminPanelTitle: "Panel d'Administration",
    productManagementTitle: "Gestion des Produits",
    productNameLabel: "Nom du produit",
    productCategoryLabel: "Catégorie",
    productPriceLabel: "Prix (FCFA)",
    productOriginalPriceLabel: "Prix original",
    productImageLabel: "URL de l'image",
    saveBtn: "Enregistrer",
    cancelBtn: "Annuler",
    productListTitle: "Liste des produits",
  },
  en: { /* ... comme avant ... */ },
  ln: { /* ... comme avant ... */ }
};
function updateLanguage(lang) {
  document.getElementById('siteTitle').textContent = translations[lang].siteTitle;
  document.getElementById('siteSubtitle').textContent = translations[lang].siteSubtitle;
  document.getElementById('navNew').textContent = translations[lang].navNew;
  document.getElementById('navWomen').textContent = translations[lang].navWomen;
  document.getElementById('navMen').textContent = translations[lang].navMen;
  document.getElementById('navCollection').textContent = translations[lang].navCollection;
  document.getElementById('heroTitle').textContent = translations[lang].heroTitle;
  document.getElementById('heroSubtitle').textContent = translations[lang].heroSubtitle;
  document.getElementById('heroButton').textContent = translations[lang].heroButton;
  document.getElementById('cartEmpty').textContent = translations[lang].cartEmpty;
  document.getElementById('cartTotalLabel').textContent = translations[lang].cartTotalLabel;
  document.getElementById('validateOrderBtn').textContent = translations[lang].validateOrderBtn;
  document.getElementById('confirmOrderBtn').textContent = translations[lang].confirmOrderBtn;
  document.getElementById('settingsTitle').textContent = translations[lang].settingsTitle;
  document.getElementById('languageLabel').textContent = translations[lang].languageLabel;
  document.getElementById('adminAccessBtn').textContent = translations[lang].adminAccessBtn;
  document.getElementById('cartTitle').textContent = translations[lang].cartTitle;
  document.getElementById('adminPanelTitle').textContent = translations[lang].adminPanelTitle;
  document.getElementById('productManagementTitle').textContent = translations[lang].productManagementTitle;
  document.getElementById('productNameLabel').textContent = translations[lang].productNameLabel;
  document.getElementById('productCategoryLabel').textContent = translations[lang].productCategoryLabel;
  document.getElementById('productPriceLabel').textContent = translations[lang].productPriceLabel;
  document.getElementById('productOriginalPriceLabel').textContent = translations[lang].productOriginalPriceLabel;
  document.getElementById('productImageLabel').textContent = translations[lang].productImageLabel;
  document.getElementById('saveProductBtn').textContent = translations[lang].saveBtn;
  document.getElementById('cancelEdit').textContent = translations[lang].cancelBtn;
  document.getElementById('productListTitle').textContent = translations[lang].productListTitle;
}
document.getElementById('languageSelector').addEventListener('change', e => {
  updateLanguage(e.target.value);
});

// --------- Initialisation ---------
displayProducts();
updateCart();
updateLanguage('fr');
});
</script>
</body>
</html>
