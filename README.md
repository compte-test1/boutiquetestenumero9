# boutiquetestenumero9
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Boutique en Ligne</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" rel="stylesheet">
    <style>
        /* Styles spécifiques pour le toast */
        .toast-container {
            position: fixed;
            top: 20px;
            right: 20px;
            z-index: 1000;
        }

        .toast {
            background-color: #333;
            color: white;
            padding: 10px 20px;
            border-radius: 5px;
            margin-bottom: 10px;
            opacity: 0;
            transition: opacity 0.3s ease-in-out;
            min-width: 250px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        .toast.show {
            opacity: 1;
        }

        /* Styles pour le mode sombre par défaut */
        body.dark-mode {
            background-color: #1a202c; /* bg-gray-900 */
            color: #ffffff; /* text-white */
        }
        body.dark-mode .bg-gray-800 {
            background-color: #2d3748; /* bg-gray-800 */
        }
        body.dark-mode .bg-gray-700 {
            background-color: #4a5568; /* bg-gray-700 */
        }
        body.dark-mode .bg-gray-600 {
            background-color: #718096; /* bg-gray-600 */
        }
        body.dark-mode .border-gray-700 {
            border-color: #4a5568; /* border-gray-700 */
        }
        body.dark-mode .border-gray-600 {
            border-color: #718096; /* border-gray-600 */
        }
        body.dark-mode .text-gray-400 {
            color: #a0aec0; /* text-gray-400 */
        }
        body.dark-mode .text-gray-500 {
            color: #cbd5e0; /* text-gray-500 */
        }

        /* Styles pour le mode clair */
        body.light-mode {
            background-color: #f7fafc; /* bg-gray-100 */
            color: #1a202c; /* text-gray-900 */
        }
        body.light-mode .bg-gray-800 {
            background-color: #e2e8f0; /* lighter gray */
        }
        body.light-mode .bg-gray-700 {
            background-color: #cbd5e0; /* even lighter gray */
        }
        body.light-mode .bg-gray-600 {
            background-color: #edf2f7;
        }
        body.light-mode .border-gray-700 {
            border-color: #e2e8f0;
        }
        body.light-mode .border-gray-600 {
            border-color: #cbd5e0;
        }
        body.light-mode .text-white { /* Override for light mode */
            color: #1a202c;
        }
        body.light-mode .text-gray-400 {
            color: #4a5568;
        }
        body.light-mode .text-gray-500 {
            color: #718096;
        }
        /* Ajustements spécifiques pour le mode clair si nécessaire */
        body.light-mode .text-yellow-400 {
            color: #d69e2e; /* slightly darker yellow for contrast */
        }
        body.light-mode .text-red-400 {
            color: #e53e3e;
        }
        body.light-mode .text-green-400 {
            color: #38a169;
        }
        body.light-mode input,
        body.light-mode select {
            background-color: #edf2f7; /* bg-gray-200 */
            color: #1a202c;
        }
        body.light-mode button.bg-blue-600 {
            background-color: #3182ce;
        }
        body.light-mode button.bg-blue-600:hover {
            background-color: #2b6cb0;
        }
        body.light-mode button.bg-yellow-500 {
            background-color: #ecc94b;
        }
        body.light-mode button.bg-yellow-500:hover {
            background-color: #d69e2e;
        }
        body.light-mode .toast {
            background-color: #f7fafc;
            color: #1a202c;
            border: 1px solid #e2e8f0;
        }

    </style>
</head>
<body class="bg-gray-900 text-white font-sans">

    <div id="toast-container" class="toast-container"></div>

    <header class="bg-gray-800 shadow-lg py-4 px-6 md:px-12 flex justify-between items-center fixed w-full z-50">
        <h1 class="text-3xl font-bold text-yellow-400">LUXURY SHOP</h1>
        <div class="flex items-center space-x-6">
            <div class="relative hidden md:block">
                <input type="text" id="searchInput" placeholder="Rechercher des produits..." class="bg-gray-700 text-white rounded-full py-2 px-4 pl-10 focus:outline-none focus:ring-2 focus:ring-yellow-400">
                <svg class="w-5 h-5 text-gray-400 absolute left-3 top-1/2 transform -translate-y-1/2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
            </div>

            <div class="relative cursor-pointer" id="cart-icon">
                <svg class="w-8 h-8 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.182 1.767.707 1.767H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z"></path></svg>
                <span id="cart-count" class="absolute -top-2 -right-2 bg-red-600 text-white text-xs font-bold rounded-full h-5 w-5 flex items-center justify-center hidden">0</span>
            </div>

            <a href="admin.html" class="text-white hover:text-yellow-400 transition hidden md:block">
                <svg class="w-8 h-8" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z"></path><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"></path></svg>
            </a>


            <button id="theme-toggle" class="p-2 rounded-full bg-gray-700 hover:bg-gray-600 transition">
                <svg id="moon-icon" class="w-6 h-6 text-yellow-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"></path></svg>
                <svg id="sun-icon" class="w-6 h-6 text-yellow-400 hidden" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h1M3 12H2m15.325 5.232l-.707.707M6.775 6.775l-.707-.707M18.949 6.051l-.707-.707M5.051 18.949l-.707-.707M8.686 12.686l-.707-.707m-2.828 2.828l-.707-.707m-2.828 2.828l-.707-.707m11.314-11.314l.707-.707m-2.828 2.828l.707-.707m-2.828 2.828l.707-.707M12 15a3 3 0 110-6 3 3 0 010 6z"></path></svg>
            </button>
        </div>
    </header>

    <div id="cart-overlay" class="fixed inset-0 bg-black bg-opacity-75 z-50 flex justify-end hidden">
        <div class="bg-gray-800 w-full md:w-1/3 h-full shadow-lg flex flex-col">
            <div class="p-6 border-b border-gray-700 flex justify-between items-center">
                <h2 class="text-2xl font-bold text-white">Votre Panier</h2>
                <button id="close-cart" class="text-gray-400 hover:text-white">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
                </button>
            </div>
            <div id="cart-items" class="flex-grow p-6 overflow-y-auto">
                </div>
            <div class="p-6 border-t border-gray-700">
                <div class="flex justify-between items-center text-xl font-bold mb-4">
                    <span>Total:</span>
                    <span id="cart-total">0 FCFA</span>
                </div>
                <button class="w-full bg-yellow-500 text-gray-900 py-3 rounded-lg font-bold hover:bg-yellow-400 transition">Procéder au Paiement</button>
            </div>
        </div>
    </div>

    <main class="container mx-auto px-4 py-8 pt-28">
        <div class="flex flex-col md:flex-row justify-between items-center mb-8 space-y-4 md:space-y-0">
            <div class="relative w-full md:w-auto">
                <select id="categoryFilter" class="block appearance-none w-full bg-gray-700 border border-gray-600 text-white py-3 px-4 pr-8 rounded-lg leading-tight focus:outline-none focus:bg-gray-600 focus:border-yellow-400">
                    <option value="all">Toutes Catégories</option>
                    <option value="Robes">Robes</option>
                    <option value="Hauts">Hauts</option>
                    <option value="Accessoires">Accessoires</option>
                    </select>
                <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-400">
                    <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"/></svg>
                </div>
            </div>
            <div class="relative w-full md:hidden">
                <input type="text" id="searchInputMobile" placeholder="Rechercher des produits..." class="bg-gray-700 text-white rounded-full py-2 px-4 pl-10 focus:outline-none focus:ring-2 focus:ring-yellow-400 w-full">
                <svg class="w-5 h-5 text-gray-400 absolute left-3 top-1/2 transform -translate-y-1/2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
            </div>
        </div>

        <div id="products-container" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8">
            </div>
    </main>

    <script>
        // --- Éléments du DOM ---
        const productsContainer = document.getElementById('products-container');
        const cartIcon = document.getElementById('cart-icon');
        const cartOverlay = document.getElementById('cart-overlay');
        const closeCartButton = document.getElementById('close-cart');
        const cartItems = document.getElementById('cart-items');
        const cartTotal = document.getElementById('cart-total');
        const cartCount = document.getElementById('cart-count');
        const categoryFilter = document.getElementById('categoryFilter');
        const searchInput = document.getElementById('searchInput');
        const searchInputMobile = document.getElementById('searchInputMobile'); // Pour mobile
        const themeToggle = document.getElementById('theme-toggle');
        const moonIcon = document.getElementById('moon-icon');
        const sunIcon = document.getElementById('sun-icon');
        const toastContainer = document.getElementById('toast-container');

        // --- Données des produits et panier ---
        // Charger les produits depuis localStorage ou utiliser les produits par défaut si localStorage est vide
        let products = JSON.parse(localStorage.getItem('shopProducts')) || [
            {
                id: 1,
                name: "Robe de Soirée Élégante",
                price: 75000,
                originalPrice: 95000,
                image: "https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/4f6e42aa-1393-409a-8d75-35dc173309c3.png",
                category: "Robes",
                rating: 4.8,
                stock: 10
            },
            {
                id: 2,
                name: "Top Couture Premium",
                price: 45000,
                originalPrice: 65000,
                image: "https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/a1a416cf-2ed9-4826-b0fe-b0bf9ac24fba.png",
                category: "Hauts",
                rating: 4.6,
                stock: 15
            },
            {
                id: 3,
                name: "Robe Cocktail Diamants",
                price: 89000,
                originalPrice: 120000,
                image: "https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/dc732eb3-2b4e-46f9-bca1-86c42649759c.png",
                category: "Robes",
                rating: 4.9,
                stock: 5
            },
            {
                id: 4,
                name: "Montre Prestige Or",
                price: 125000,
                originalPrice: 180000,
                image: "https://storage.googleapis.com/workspace-0f70711f-8b4e-4d94-86f1-2a93ccde5887/image/58eefe4f-a865-4b1a-b96a-89a1659638b2.png",
                category: "Accessoires",
                rating: 5.0,
                stock: 3
            }
        ];

        // Charger le panier depuis localStorage
        let cart = JSON.parse(localStorage.getItem('shoppingCart')) || [];

        // --- Fonctions de sauvegarde ---
        function saveProducts() {
            localStorage.setItem('shopProducts', JSON.stringify(products));
        }

        function saveCart() {
            localStorage.setItem('shoppingCart', JSON.stringify(cart));
        }

        // --- Fonctions utilitaires ---

        // Fonction pour afficher un toast (notification)
        function showToast(message, duration = 3000) {
            const toast = document.createElement('div');
            toast.className = 'toast';
            toast.innerText = message;
            toastContainer.appendChild(toast);

            // Force reflow for CSS transition
            void toast.offsetWidth;

            toast.classList.add('show');

            setTimeout(() => {
                toast.classList.remove('show');
                toast.addEventListener('transitionend', () => {
                    toast.remove();
                }, { once: true });
            }, duration);
        }

        // --- Logique du Panier ---
        function updateCart() {
            cartItems.innerHTML = '';
            let total = 0;

            if (cart.length === 0) {
                cartItems.innerHTML = '<p class="text-gray-400 text-center py-12">Votre panier est vide</p>';
            } else {
                cart.forEach(item => {
                    const itemTotal = item.price * item.quantity;
                    total += itemTotal;
                    const cartItem = document.createElement('div');
                    cartItem.className = 'flex justify-between items-center bg-gray-800 p-3 rounded-lg mb-2';
                    cartItem.innerHTML = `
                        <div class="flex items-center space-x-3 w-3/5">
                            <img src="${item.image}" alt="${item.name}" class="w-12 h-12 object-cover rounded">
                            <div>
                                <span class="font-medium">${item.name}</span>
                                <p class="text-sm text-yellow-400">${item.price.toLocaleString()} FCFA</p>
                            </div>
                        </div>
                        <div class="flex items-center space-x-2">
                            <button class="quantity-btn bg-gray-700 text-white px-2 py-1 rounded hover:bg-gray-600 transition" data-id="${item.id}" data-action="decrease">-</button>
                            <span class="font-bold">${item.quantity}</span>
                            <button class="quantity-btn bg-gray-700 text-white px-2 py-1 rounded hover:bg-gray-600 transition" data-id="${item.id}" data-action="increase">+</button>
                            <button class="remove-from-cart text-red-400 hover:text-red-300 ml-2" data-id="${item.id}">
                                <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"/>
                                </svg>
                            </button>
                        </div>
                    `;
                    cartItems.appendChild(cartItem);
                });
            }

            cartTotal.innerText = total.toLocaleString() + ' FCFA';
            cartCount.innerText = cart.reduce((sum, item) => sum + item.quantity, 0);
            cartCount.classList.toggle('hidden', cart.reduce((sum, item) => sum + item.quantity, 0) === 0);

            saveCart();
        }

        // --- Logique d'affichage des produits ---
        function displayProducts() {
            productsContainer.innerHTML = '';
            const currentCategory = categoryFilter.value;
            const searchTerm = (searchInput.value || searchInputMobile.value).toLowerCase();

            const filteredProducts = products.filter(product => {
                const matchesCategory = currentCategory === 'all' || product.category === currentCategory;
                const matchesSearch = product.name.toLowerCase().includes(searchTerm) || product.category.toLowerCase().includes(searchTerm);
                return matchesCategory && matchesSearch;
            });

            if (filteredProducts.length === 0) {
                productsContainer.innerHTML = '<p class="text-white text-center py-10 col-span-full">Aucun produit trouvé pour cette sélection.</p>';
                return;
            }

            filteredProducts.forEach(product => {
                const isOutOfStock = product.stock <= 0;
                const productCard = document.createElement('div');
                productCard.className = `bg-gray-800 rounded-lg shadow-lg overflow-hidden flex flex-col transform hover:scale-105 transition-transform duration-300 relative ${isOutOfStock ? 'opacity-70 grayscale' : ''}`;

                productCard.innerHTML = `
                    ${isOutOfStock ? '<div class="absolute inset-0 bg-black bg-opacity-60 flex items-center justify-center z-10"><span class="text-white text-xl font-bold">Rupture de Stock</span></div>' : ''}
                    <img src="${product.image}" alt="${product.name}" class="w-full h-48 object-cover">
                    <div class="p-4 flex-grow">
                        <h3 class="text-lg font-semibold text-white mb-1">${product.name}</h3>
                        <p class="text-sm text-gray-400 mb-2">${product.category}</p>
                        <div class="flex items-center mb-2">
                            ${Array(Math.floor(product.rating)).fill().map(() => `<svg class="w-4 h-4 text-yellow-400 fill-current" viewBox="0 0 24 24"><path d="M12 17.27L18.18 21l-1.64-7.03L22 9.24l-7.19-.61L12 2 9.19 8.63 2 9.24l5.46 4.73L5.82 21z"/></svg>`).join('')}
                            ${Array(5 - Math.floor(product.rating)).fill().map(() => `<svg class="w-4 h-4 text-gray-600 fill-current" viewBox="0 0 24 24"><path d="M12 17.27L18.18 21l-1.64-7.03L22 9.24l-7.19-.61L12 2 9.19 8.63 2 9.24l5.46 4.73L5.82 21z"/></svg>`).join('')}
                            <span class="text-gray-400 text-sm ml-1">(${product.rating.toFixed(1)})</span>
                        </div>
                        <div class="flex items-baseline space-x-2 mb-3">
                            <span class="text-yellow-400 font-bold text-xl">${product.price.toLocaleString()} FCFA</span>
                            ${product.originalPrice ? `<span class="text-gray-500 line-through text-sm">${product.originalPrice.toLocaleString()} FCFA</span>` : ''}
                        </div>
                        ${product.stock > 0 ? `<p class="text-sm text-green-400">Stock: ${product.stock} disponibles</p>` : `<p class="text-sm text-red-400">Stock: 0 (Rupture)</p>`}
                    </div>
                    <div class="p-4 border-t border-gray-700">
                        <button class="add-to-cart w-full bg-blue-600 text-white py-2 px-4 rounded-lg hover:bg-blue-700 transition duration-300 ${isOutOfStock ? 'opacity-50 cursor-not-allowed' : ''}" data-id="${product.id}" ${isOutOfStock ? 'disabled' : ''}>
                            ${isOutOfStock ? 'Rupture de Stock' : 'Ajouter au Panier'}
                        </button>
                    </div>
                `;
                productsContainer.appendChild(productCard);
            });
        }

        // --- Écouteurs d'événements ---

        // Ouvrir le panier
        cartIcon.addEventListener('click', () => {
            cartOverlay.classList.remove('hidden');
        });

        // Fermer le panier
        closeCartButton.addEventListener('click', () => {
            cartOverlay.classList.add('hidden');
        });

        // Fermer le panier en cliquant en dehors
        cartOverlay.addEventListener('click', (e) => {
            if (e.target === cartOverlay) {
                cartOverlay.classList.add('hidden');
            }
        });

        // Ajouter au panier depuis la grille des produits
        productsContainer.addEventListener('click', (e) => {
            if (e.target.classList.contains('add-to-cart')) {
                const productId = e.target.getAttribute('data-id');
                const product = products.find(p => p.id == productId);

                if (product && product.stock > 0) {
                    const existingItem = cart.find(item => item.id === product.id);
                    if (existingItem) {
                        if (existingItem.quantity < product.stock) {
                            existingItem.quantity++;
                            showToast(`"${product.name}" (x${existingItem.quantity}) ajouté au panier !`);
                        } else {
                            showToast(`Désolé, il ne reste plus que ${product.stock} exemplaire(s) de "${product.name}" en stock.`, 5000);
                        }
                    } else {
                        cart.push({ ...product, quantity: 1 });
                        showToast(`"${product.name}" ajouté au panier !`);
                    }
                    updateCart();
                    displayProducts(); // Rafraîchit l'affichage des produits
                } else if (product && product.stock === 0) {
                    showToast(`Désolé, "${product.name}" est en rupture de stock.`, 5000);
                }
            }
        });

        // Gérer les quantités et la suppression dans le panier
        cartItems.addEventListener('click', (e) => {
            // Boutons d'augmentation/diminution de quantité
            if (e.target.classList.contains('quantity-btn')) {
                const productId = e.target.getAttribute('data-id');
                const action = e.target.getAttribute('data-action');
                const itemIndex = cart.findIndex(item => item.id == productId);

                if (itemIndex !== -1) {
                    const productInStock = products.find(p => p.id == productId); // Trouver le produit dans la liste originale

                    if (action === 'increase') {
                        if (productInStock && cart[itemIndex].quantity < productInStock.stock) {
                            cart[itemIndex].quantity++;
                            showToast(`Quantité de "${cart[itemIndex].name}" augmentée à ${cart[itemIndex].quantity}.`);
                        } else if (productInStock) {
                            showToast(`Désolé, il ne reste plus que ${productInStock.stock} exemplaire(s) de ${productInStock.name} en stock.`, 5000);
                        }
                    } else if (action === 'decrease') {
                        cart[itemIndex].quantity--;
                        if (cart[itemIndex].quantity <= 0) {
                            const removedItemName = cart[itemIndex].name; // Sauvegarder le nom avant de supprimer
                            cart.splice(itemIndex, 1);
                            showToast(`"${removedItemName}" retiré du panier.`);
                        } else {
                            showToast(`Quantité de "${cart[itemIndex].name}" diminuée à ${cart[itemIndex].quantity}.`);
                        }
                    }
                    updateCart();
                    displayProducts(); // Rafraîchit l'affichage des produits au cas où un stock change de 0
                }
            }

            // Bouton de suppression d'article (l'icône poubelle)
            if (e.target.closest('.remove-from-cart')) {
                const productId = e.target.closest('.remove-from-cart').getAttribute('data-id');
                const itemToRemove = cart.find(item => item.id == productId);
                if (confirm(`Voulez-vous vraiment retirer "${itemToRemove ? itemToRemove.name : 'cet article'}" du panier ?`)) {
                    cart = cart.filter(item => item.id != productId);
                    showToast(`"${itemToRemove ? itemToRemove.name : 'Cet article'}" a été retiré du panier.`);
                    updateCart();
                    displayProducts(); // Rafraîchit l'affichage des produits au cas où un stock change de 0
                }
            }
        });


        // Filtrer par catégorie
        categoryFilter.addEventListener('change', displayProducts);

        // Rechercher sur le grand écran
        searchInput.addEventListener('input', displayProducts);
        // Rechercher sur le petit écran
        searchInputMobile.addEventListener('input', displayProducts);

        // --- Gestion du Thème (Dark/Light Mode) ---
        function setDarkMode(isDark) {
            if (isDark) {
                document.body.classList.add('dark-mode');
                document.body.classList.remove('light-mode');
                moonIcon.classList.remove('hidden');
                sunIcon.classList.add('hidden');
                localStorage.setItem('theme', 'dark');
            } else {
                document.body.classList.add('light-mode');
                document.body.classList.remove('dark-mode');
                moonIcon.classList.add('hidden');
                sunIcon.classList.remove('hidden');
                localStorage.setItem('theme', 'light');
            }
        }

        themeToggle.addEventListener('click', () => {
            const isDarkMode = document.body.classList.contains('dark-mode');
            setDarkMode(!isDarkMode);
        });

        // --- Initialisation au chargement de la page ---
        document.addEventListener('DOMContentLoaded', () => {
            // Appliquer le thème sauvegardé
            const savedTheme = localStorage.getItem('theme');
            if (savedTheme === 'light') {
                setDarkMode(false);
            } else {
                setDarkMode(true); // Par défaut ou si 'dark'
            }

            // Sauvegarder les produits initiaux si c'est la première fois (pour avoir le stock)
            if (!localStorage.getItem('shopProducts')) {
                saveProducts();
            }

            displayProducts(); // Affiche les produits
            updateCart();      // Met à jour le panier
        });
    </script>
</body>
</html>
