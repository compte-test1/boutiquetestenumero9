<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>NG_chopi_DIAMS - Boutique de Luxe</title>

    <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link
    href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap"
    rel="stylesheet"
  />

    <script src="https://cdn.tailwindcss.com"></script>

    <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            // 🎨 Palette NG_chopi_DIAMS
            primary: "#111822", // Fond principal
            secondary: "#333740", // Sections
            neutral: "#6C6A6A", // Texte secondaire
            brownDark: "#4F362A", // Accent chaud
            brownSoft: "#745850", // Survol, ombres
            gold: "#F3B699", // Accent luxe (boutons, logo)
          },
          fontFamily: {
            sans: ["Poppins", "sans-serif"],
          },
          boxShadow: {
            glow: "0 0 20px rgba(243, 182, 153, 0.3)",
          },
        },
      },
    };
  </script>
</head>
<body class="bg-primary text-gray-100 font-sans">
    <header class="bg-gradient-to-r from-black via-primary to-black border-b border-gold sticky top-0 z-50">
    <div class="container mx-auto px-4 py-4 flex justify-between items-center">
      <div class="flex items-center space-x-2">
        <h1 id="siteTitle" class="text-3xl font-bold text-gold">NG_chopi</h1>
        <span id="siteSubtitle" class="text-gold font-semibold">DIAMS</span>
      </div>
      <nav class="hidden md:flex space-x-6">
        <a href="#" id="navNew" class="text-gray-300 hover:text-gold">Nouveautés</a>
        <a href="#" id="navWomen" class="text-gray-300 hover:text-gold">Femmes</a>
        <a href="#" id="navMen" class="text-gray-300 hover:text-gold">Hommes</a>
        <a href="#" id="navCollection" class="text-gold font-bold">COLLECTION</a>
      </nav>
      <div class="flex items-center space-x-4">
        <button id="settingsBtn" class="text-gold hover:text-gold/80" title="Paramètres">⚙️</button>
        <button id="adminBtn" class="text-gold hover:text-gold/80" title="Admin">🛠️</button>
        <button id="cartBtn" class="relative text-gold hover:text-gold/80" title="Panier">🛒
          <span id="cartCount" class="absolute -top-2 -right-2 bg-gold text-primary text-xs rounded-full h-5 w-5 flex items-center justify-center font-bold hidden">0</span>
        </button>
      </div>
    </div>
  </header>

    <section class="bg-gradient-to-r from-gold to-brownSoft text-primary py-16 text-center">
    <div class="container mx-auto px-4">
      <h2 id="heroTitle" class="text-4xl md:text-5xl font-bold mb-4">COLLECTION EXCLUSIVE</h2>
      <p id="heroSubtitle" class="text-xl mb-8">Articles de luxe à prix exceptionnels</p>
      <button id="heroButton" class="bg-primary text-gold px-8 py-3 rounded-full font-bold hover:bg-secondary transition shadow-lg">Découvrir</button>
    </div>
  </section>

    <section class="container mx-auto px-4 py-12">
    <div id="productsContainer" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-8"></div>
  </section>

    <div id="settingsPanel" class="fixed inset-0 bg-primary bg-opacity-90 z-50 hidden backdrop-blur-sm">
    <div class="absolute right-0 top-0 h-full w-full max-w-md bg-secondary/90 border-l border-gold overflow-y-auto"
         style="background-image: url('https://placehold.co/600x1000/333740/F3B699?text=NG_chopi_DIAMS&font=poppins'); background-blend-mode: overlay; background-size: cover;">
      <div class="p-6">
        <div class="flex justify-between items-center mb-6">
          <h2 id="settingsTitle" class="text-xl font-bold text-gold">Paramètres</h2>
          <button id="closeSettings" class="text-gold hover:text-gold/80 text-2xl">✖️</button>
        </div>
        <div class="space-y-6">
          <div>
            <h3 id="languageLabel" class="text-lg font-semibold text-gold mb-3">Langue</h3>
            <select id="languageSelector" class="w-full bg-primary border border-gold/30 text-white rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-gold">
              <option value="fr">Français</option>
              <option value="en">English</option>
              <option value="ln">Lingala</option>
            </select>
          </div>
          <button id="adminAccessBtn" class="w-full bg-gradient-to-r from-gold to-brownSoft text-primary py-3 rounded-lg font-bold hover:opacity-90 transition">Accès Administrateur</button>
        </div>
      </div>
    </div>
  </div>

    <div id="cartOverlay" class="fixed inset-0 bg-black bg-opacity-75 z-50 hidden backdrop-blur-sm">
a <div class="absolute right-0 top-0 h-full w-full max-w-md bg-primary border-l border-gold overflow-y-auto">
      <div class="p-6">
        <div class="flex justify-between items-center mb-6">
          <h2 id="cartTitle" class="text-xl font-bold text-gold">Votre Panier</h2>
          <button id="closeCart" class="text-gold hover:text-gold/80 text-2xl">✖️</button>
        </div>
        <div id="cartItems" class="space-y-4 mb-6">
          <p class="text-neutral text-center py-12" id="cartEmpty">Votre panier est vide</p>
        </div>
        <div class="border-t border-gold/30 pt-6">
          <div class="flex justify-between items-center mb-6">
            <span id="cartTotalLabel" class="text-lg font-semibold">Total:</span>
            <span id="cartTotal" class="text-2xl font-bold text-gold">0 FCFA</span>
          </div>
          <div id="clientForm" class="hidden">
            <div class="mb-4">
              <label id="clientNameLabel" class="block text-gold mb-2">Nom et Prénom</label>
              <input type="text" id="clientName" class="w-full bg-secondary border border-gold/30 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-gold" required />
            </div>
            <div class="mb-4">
              <label id="clientPhoneLabel" class="block text-gold mb-2">Numéro de téléphone</label>
              <input type="tel" id="clientPhone" class="w-full bg-secondary border border-gold/30 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-gold" required />
            </div>
            <div class="mb-4">
              <label id="clientCityLabel" class="block text-gold mb-2">Ville</label>
              <input type="text" id="clientCity" class="w-full bg-secondary border border-gold/30 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-gold" required />
            </div>
            <div class="mb-4">
              <label id="clientNeighborhoodLabel" class="block text-gold mb-2">Quartier</label>
              <input type="text"id="clientNeighborhood" class="w-full bg-secondary border border-gold/30 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-gold" required />
            </div>
            <div class="mb-6">
              <label id="clientStreetLabel" class="block text-gold mb-2">Rue et Numéro</label>
              <input type="text" id="clientStreet" class="w-full bg-secondary border border-gold/30 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-gold" required />
            </div>
            <button id="confirmOrderBtn" class="w-full bg-gradient-to-r from-green-500 to-green-600 text-white py-3 rounded-lg font-bold hover:opacity-90 transition">Confirmer la commande</button>
          </div>
          <button id="validateOrderBtn" class="w-full bg-gradient-to-r from-gold to-brownSoft text-primary py-3 rounded-lg font-bold hover:opacity-90 transition">Valider votre commande</button>
        </div>
      </div>
    </div>
  </div>

    <div id="adminPanel" class="fixed inset-0 bg-primary z-50 overflow-y-auto hidden">
    <div class="container mx-auto px-4 py-12">
      <div class="flex justify-between items-center mb-12">
        <h1 id="adminPanelTitle" class="text-3xl font-bold text-gold">Panel d'Administration</h1>
        <button id="closeAdmin" class="text-gold hover:text-gold/80 px-4 py-2 border border-gold rounded-lg">Fermer</button>
      </div>
      <div class="bg-secondary rounded-lg p-6 mb-8">
        <h2 id="productManagementTitle" class="text-xl font-bold text-gold mb-4">Gestion des Produits</h2>
        <button id="addProductBtn" class="bg-green-500 text-white px-4 py-2 rounded-lg font-bold mb-4">Nouveau produit</button>
        <form id="adminProductForm" class="mb-6">
          <input type="hidden" id="productId" />
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
            <div>
              <label id="productNameLabel" class="block text-gold mb-2">Nom du produit</label>
              <input type="text" id="productName" class="w-full bg-primary border border-gold/30 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-gold" required />
            </div>
            <div>
              <label id="productCategoryLabel" class="block text-gold mb-2">Catégorie</label>
              <select id="productCategory" class="w-full bg-primary border border-gold/30 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-gold" required>
                <option value="Robes">Robes</option>
                <option value="Hauts">Hauts</option>
                <option value="Accessoires">Accessoires</option>
              </select>
            </div>
          </div>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
            <div>
              <label id="productPriceLabel" class="block text-gold mb-2">Prix (FCFA)</label>
              <input type="number" id="productPrice" class="w-full bg-primary border border-gold/30 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-gold" required />
            </div>
            <div>
              <label id="productOriginalPriceLabel" class="block text-gold mb-2">Prix original</label>
              <input type="number" id="productOriginalPrice" class="w-full bg-primary border border-gold/30 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-gold" required />
            </div>
          </div>
          <div class="mb-4">
            <label id="productImageLabel" class="block text-gold mb-2">URL de l'image</label>
            <input type="text" id="productImage" class="w-full bg-primary border border-gold/30 rounded-lg px-4 py-2 mb-2 focus:outline-none focus:ring-2 focus:ring-gold" required />
          </div>
          <div class="flex space-x-3">
            <button id="saveProductBtn" type="submit" class="bg-gold text-primary px-6 py-2 rounded-lg font-bold">Enregistrer</button>
            <button id="cancelEdit" type="button" class="bg-neutral text-white px-6 py-2 rounded-lg font-bold">Annuler</button>
          </div>
        </form>
        <div class="bg-primary rounded-lg p-4 mb-4">
          <h3 id="productListTitle" class="text-lg font-semibold text-gold mb-3">Liste des produits</h3>
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
    // Classes mises à jour avec le nouveau design
    div.className = 'bg-secondary rounded-xl overflow-hidden border border-gold/20 shadow-lg hover:shadow-glow transition-all duration-300';
    div.innerHTML = `
      <div class="relative">
        <img src="${product.image}" alt="${product.name}" class="w-full h-64 object-cover" />
        ${discount > 0 ? `<span class="absolute top-3 left-3 bg-gold text-primary px-2 py-1 rounded text-xs font-bold">-${discount}%</span>` : ''}
      </div>
      <div class="p-4">
        <h3 class="font-semibold mb-2 h-12">${product.name}</h3>
        <div class="flex items-center mb-3">
          ${Array(5).fill().map((_, i) => `<svg class="w-4 h-4 ${i < Math.floor(product.rating) ? 'text-gold' : 'text-neutral'}" fill="currentColor" viewBox="0 0 24 24"><path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z"/></svg>`).join('')}
          <span class="text-xs text-neutral ml-2">${product.rating}</span>
        </div>
        <div class="flex justify-between items-center">
          <div>
            <p class="text-gold font-bold text-lg">${product.price.toLocaleString()} FCFA</p>
            ${product.price < product.originalPrice ? `<p class="text-neutral text-sm line-through">${product.originalPrice.toLocaleString()} FCFA</p>` : ''}
          </div>
          <button class="bg-gold hover:bg-gold/80 text-primary px-4 py-2 rounded text-sm font-semibold add-to-cart" data-id="${product.id}">Ajouter</button>
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
    cartItems.innerHTML = `<p class="text-neutral text-center py-12" id="cartEmpty">${translations[currentLang].cartEmpty}</p>`;
    document.getElementById('cartCount').classList.add('hidden');
    document.getElementById('cartTotal').textContent = '0 FCFA';
    document.getElementById('validateOrderBtn').classList.add('hidden');
    document.getElementById('clientForm').classList.add('hidden');
    return;
  }
  document.getElementById('cartCount').classList.remove('hidden');
  document.getElementById('validateOrderBtn').classList.remove('hidden');

  let total = 0;
  let itemsCount = 0;
  cart.forEach(item => {
    total += item.price * item.quantity;
    itemsCount += item.quantity;
    const div = document.createElement('div');
    div.className = 'flex justify-between items-center bg-secondary p-3 rounded-lg';
    div.innerHTML = `
      <div class="flex items-center space-x-3">
        <img src="${item.image}" alt="${item.name}" class="w-12 h-12 object-cover rounded-md border border-gold/20" />
        <div>
          <p class="font-medium">${item.name}</p>
          <p class="text-sm text-gold">${item.price.toLocaleString()} FCFA x ${item.quantity}</p>
        </div>
      </div>
      <button class="text-red-400 hover:text-red-300 remove-from-cart" data-id="${item.id}">🗑️</button>
    `;
    cartItems.appendChild(div);
  });
  document.getElementById('cartCount').textContent = itemsCount;
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

// Gérer la suppression d'articles du panier
document.getElementById('cartItems').addEventListener('click', (e) => {
  if (e.target.classList.contains('remove-from-cart') || e.target.parentElement.classList.contains('remove-from-cart')) {
    const btn = e.target.closest('.remove-from-cart');
    const productId = parseInt(btn.getAttribute('data-id'));
    const itemIndex = cart.findIndex(item => item.id === productId);
    if (itemIndex > -1) {
      cart[itemIndex].quantity--;
      if (cart[itemIndex].quantity === 0) {
        cart.splice(itemIndex, 1);
      }
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
  // Réinitialiser le formulaire
  document.getElementById('clientName').value = '';
  document.getElementById('clientPhone').value = '';
  document.getElementById('clientCity').value = '';
  document.getElementById('clientNeighborhood').value = '';
  document.getElementById('clientStreet').value = '';
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
      products[index] = { id: Number(id), name, price, originalPrice, image, category, rating: products[index].rating || 4.5 };
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
  document.getElementById('adminProductForm').reset();
  document.getElementById('saveProductBtn').textContent = translations[currentLang].saveBtn;
}
function loadAdminProducts() {
  const container = document.getElementById('adminProductsList');
  container.innerHTML = '';
  products.forEach(product => {
    const div = document.createElement('div');
    div.className = 'flex justify-between items-center bg-primary p-3 rounded-lg border border-gold/10';
    div.innerHTML = `
      <div class="flex items-center space-x-3">
        <img src="${product.image}" alt="${product.name}" class="w-10 h-10 object-cover rounded-md" />
        <div>
          <h4 class="font-medium">${product.name}</h4>
          <p class="text-sm text-gold">${product.price.toLocaleString()} FCFA</p>
        </div>
      </div>
      <div class="flex space-x-2">
        <button class="admin-edit-btn text-gold hover:text-gold/80" data-id="${product.id}">✏️</button>
        <button class="admin-delete-btn text-red-400 hover:text-red-300" data-id="${product.id}">🗑️</button>
      </div>
    `;
    container.appendChild(div);
  });
}
document.getElementById('adminProductsList').addEventListener('click', e => {
  if (e.target.classList.contains('admin-edit-btn') || e.target.parentElement.classList.contains('admin-edit-btn')) {
    const btn = e.target.closest('.admin-edit-btn');
    const id = parseInt(btn.getAttribute('data-id'));
    const product = products.find(p => p.id === id);
    if (!product) return;
    document.getElementById('productId').value = product.id;
    document.getElementById('productName').value = product.name;
    document.getElementById('productPrice').value = product.price;
    document.getElementById('productOriginalPrice').value = product.originalPrice;
    document.getElementById('productImage').value = product.image;
    document.getElementById('productCategory').value = product.category;
    document.getElementById('saveProductBtn').textContent = "Modifier";
    window.scrollTo(0, 0);
  }
  if (e.target.classList.contains('admin-delete-btn') || e.target.parentElement.classList.contains('admin-delete-btn')) {
    const btn = e.target.closest('.admin-delete-btn');
    const id = parseInt(btn.getAttribute('data-id'));
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
let currentLang = 'fr';
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
    clientNameLabel: "Nom et Prénom",
    clientPhoneLabel: "Numéro de téléphone",
    clientCityLabel: "Ville",
    clientNeighborhoodLabel: "Quartier",
    clientStreetLabel: "Rue et Numéro",
    adminPanelTitle: "Panel d'Administration",
    productManagementTitle: "Gestion des Produits",
    addProductBtn: "Nouveau produit",
    productNameLabel: "Nom du produit",
    productCategoryLabel: "Catégorie",
    productPriceLabel: "Prix (FCFA)",
    productOriginalPriceLabel: "Prix original",
    productImageLabel: "URL de l'image",
    saveBtn: "Enregistrer",
    cancelBtn: "Annuler",
    productListTitle: "Liste des produits",
  },
  en: {
    siteTitle: "NG_chopi",
    siteSubtitle: "DIAMS",
    navNew: "New",
    navWomen: "Women",
    navMen: "Men",
    navCollection: "COLLECTION",
    heroTitle: "EXCLUSIVE COLLECTION",
    heroSubtitle: "Luxury items at exceptional prices",
    heroButton: "Discover",
    cartEmpty: "Your cart is empty",
    cartTotalLabel: "Total:",
    validateOrderBtn: "Validate your order",
    confirmOrderBtn: "Confirm order",
    settingsTitle: "Settings",
    languageLabel: "Language",
    adminAccessBtn: "Admin Access",
    cartTitle: "Your Cart",
    clientNameLabel: "Full Name",
    clientPhoneLabel: "Phone number",
    clientCityLabel: "City",
    clientNeighborhoodLabel: "Neighborhood",
    clientStreetLabel: "Street and Number",
    adminPanelTitle: "Admin Panel",
    productManagementTitle: "Product Management",
    addProductBtn: "New product",
    productNameLabel: "Product name",
    productCategoryLabel: "Category",
    productPriceLabel: "Price (FCFA)",
    productOriginalPriceLabel: "Original price",
    productImageLabel: "Image URL",
    saveBtn: "Save",
    cancelBtn: "Cancel",
    productListTitle: "Product list",
  },
  ln: {
    siteTitle: "NG_chopi",
    siteSubtitle: "DIAMS",
    navNew: "Ya sika",
    navWomen: "Basi",
    navMen: "Mibali",
    navCollection: "COLLECTION",
    heroTitle: "COLLECTION YA MOTUYA",
    heroSubtitle: "Biloko ya luxe na ntalo kitoko",
    heroButton: "Tala",
    cartEmpty: "Panier na yo ezali polele",
    cartTotalLabel: "Total:",
    validateOrderBtn: "Ndimisa commande",
    confirmOrderBtn: "Tinda commande",
    settingsTitle: "Paramètres",
    languageLabel: "Lokota",
    adminAccessBtn: "Accès Admin",
    cartTitle: "Panier na Yo",
    clientNameLabel: "Nkombo na yo",
    clientPhoneLabel: "Numéro ya téléphone",
    clientCityLabel: "Mboka",
    clientNeighborhoodLabel: "Quartier",
    clientStreetLabel: "Balabala mpe Numéro",
    adminPanelTitle: "Panel Admin",
    productManagementTitle: "Gestion ya biloko",
    addProductBtn: "Eloko ya sika",
    productNameLabel: "Nkombo ya eloko",
    productCategoryLabel: "Catégorie",
    productPriceLabel: "Ntalo (FCFA)",
    productOriginalPriceLabel: "Ntalo ya yambo",
    productImageLabel: "URL ya elilingi",
    saveBtn: "Bomba",
    cancelBtn: "Zongisa",
    productListTitle: "Liste ya biloko",
  }
};
function updateLanguage(lang) {
  currentLang = lang;
  const t = translations[lang];
  
  document.getElementById('siteTitle').textContent = t.siteTitle;
  document.getElementById('siteSubtitle').textContent = t.siteSubtitle;
  document.getElementById('navNew').textContent = t.navNew;
  document.getElementById('navWomen').textContent = t.navWomen;
  document.getElementById('navMen').textContent = t.navMen;
  document.getElementById('navCollection').textContent = t.navCollection;
  
  document.getElementById('heroTitle').textContent = t.heroTitle;
  document.getElementById('heroSubtitle').textContent = t.heroSubtitle;
  document.getElementById('heroButton').textContent = t.heroButton;
  
  document.getElementById('cartEmpty').textContent = t.cartEmpty;
  document.getElementById('cartTotalLabel').textContent = t.cartTotalLabel;
  document.getElementById('validateOrderBtn').textContent = t.validateOrderBtn;
  document.getElementById('confirmOrderBtn').textContent = t.confirmOrderBtn;
  
  document.getElementById('settingsTitle').textContent = t.settingsTitle;
  document.getElementById('languageLabel').textContent = t.languageLabel;
  document.getElementById('adminAccessBtn').textContent = t.adminAccessBtn;
  
  document.getElementById('cartTitle').textContent = t.cartTitle;
  document.getElementById('clientNameLabel').textContent = t.clientNameLabel;
  document.getElementById('clientPhoneLabel').textContent = t.clientPhoneLabel;
  document.getElementById('clientCityLabel').textContent = t.clientCityLabel;
  document.getElementById('clientNeighborhoodLabel').textContent = t.clientNeighborhoodLabel;
  document.getElementById('clientStreetLabel').textContent = t.clientStreetLabel;
  
  document.getElementById('adminPanelTitle').textContent = t.adminPanelTitle;
  document.getElementById('productManagementTitle').textContent = t.productManagementTitle;
  document.getElementById('addProductBtn').textContent = t.addProductBtn;
  document.getElementById('productNameLabel').textContent = t.productNameLabel;
  document.getElementById('productCategoryLabel').textContent = t.productCategoryLabel;
  document.getElementById('productPriceLabel').textContent = t.productPriceLabel;
  document.getElementById('productOriginalPriceLabel').textContent = t.productOriginalPriceLabel;
  document.getElementById('productImageLabel').textContent = t.productImageLabel;
s document.getElementById('saveProductBtn').textContent = t.saveBtn;
  document.getElementById('cancelEdit').textContent = t.cancelBtn;
  document.getElementById('productListTitle').textContent = t.productListTitle;
}
document.getElementById('languageSelector').addEventListener('change', e => {
  updateLanguage(e.target.value);
});

// --------- Initialisation ---------
displayProducts();
updateCart();
updateLanguage('fr'); // Définit la langue par défaut au chargement
});
</script>
</body>
</html>
