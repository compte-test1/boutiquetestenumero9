import React, { useState, useEffect, useRef } from 'react';
import { initializeApp } from 'firebase/app';
import { getFirestore, collection, doc, setDoc, onSnapshot, query, deleteDoc } from 'firebase/firestore';
import { getAuth, signInWithCustomToken, signInAnonymously, onAuthStateChanged, signOut } from 'firebase/auth';
import { getStorage, ref, uploadBytes, getDownloadURL } from 'firebase/storage';

// Fonction utilitaire pour générer des IDs uniques pour les produits si nécessaire
const generateUniqueId = () => `prod_${Math.random().toString(36).substring(2, 9)}`;

// Composant principal de l'application
function App() {
  const [firebaseData, setFirebaseData] = useState(null);
  const [userId, setUserId] = useState(null);
  const [loading, setLoading] = useState(true);
  const [isChatOpen, setIsChatOpen] = useState(false);
  const [chatInput, setChatInput] = useState('');
  const [messages, setMessages] = useState([]);
  const messagesEndRef = useRef(null);
  const [isChatLoading, setIsChatLoading] = useState(false);
  const [isAdminLoggedIn, setIsAdminLoggedIn] = useState(false);
  const [products, setProducts] = useState([]);
  const [newProduct, setNewProduct] = useState({
    name: '',
    category: '',
    originalPrice: '',
    currency: 'CFA',
    imageUrl: '',
    hoverImageUrl: '',
    colors: '',
    sizes: '',
    isNew: true,
    flashSale: {
      isActive: false,
      salePrice: '',
      endDate: ''
    }
  });
  const [editingProductId, setEditingProductId] = useState(null);
  const [showDeleteModal, setShowDeleteModal] = useState(false);
  const [productToDelete, setProductToDelete] = useState(null);
  const [cartItems, setCartItems] = useState([]);
  const [showModal, setShowModal] = useState(false);
  const [modalMessage, setModalMessage] = useState('');
  const [isImageUploading, setIsImageUploading] = useState(false);

  // États pour l'interface client
  const [currentPage, setCurrentPage] = useState('productGrid');
  const [selectedProduct, setSelectedProduct] = useState(null);

  // Gère l'envoi d'un message au chatbot
  const handleSendMessage = async () => {
    if (chatInput.trim() === '') return;
    
    const userMessage = { text: chatInput, sender: 'user' };
    const updatedMessages = [...messages, userMessage];
    setMessages(updatedMessages);
    setChatInput('');
    setIsChatLoading(true);

    try {
      let chatHistory = updatedMessages.map(msg => ({
        role: msg.sender === 'user' ? 'user' : 'model',
        parts: [{ text: msg.text }]
      }));
      chatHistory = [
        { role: 'user', parts: [{ text: "Tu es l'assistant de la boutique de luxe NG_chopi_DIAMS. Réponds de manière élégante et concise. Ne mentionne jamais que tu es une IA ou un modèle de langage." }]},
        ...chatHistory
      ];
      const payload = { contents: chatHistory };
      const apiKey = "";
      const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

      const callApiWithRetry = async (retries = 3, delay = 1000) => {
        try {
          const response = await fetch(apiUrl, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify(payload)
          });
          if (!response.ok) {
            if (response.status === 429 && retries > 0) {
              console.log(`Rate limit hit, retrying in ${delay}ms...`);
              await new Promise(res => setTimeout(res, delay));
              return callApiWithRetry(retries - 1, delay * 2);
            }
            throw new Error(`Erreur API: ${response.status}`);
          }
          const result = await response.json();
          if (result.candidates && result.candidates.length > 0 &&
              result.candidates[0].content && result.candidates[0].content.parts &&
              result.candidates[0].content.parts.length > 0) {
            const botResponseText = result.candidates[0].content.parts[0].text;
            setMessages(prevMessages => [...prevMessages, { text: botResponseText, sender: 'bot' }]);
          } else {
            setMessages(prevMessages => [...prevMessages, { text: "Désolé, je n'ai pas pu générer de réponse.", sender: 'bot' }]);
          }
        } catch (error) {
          console.error("Erreur lors de l'appel à l'API Gemini :", error);
          setMessages(prevMessages => [...prevMessages, { text: "Une erreur est survenue. Veuillez réessayer plus tard.", sender: 'bot' }]);
        } finally {
          setIsChatLoading(false);
        }
      };
      await callApiWithRetry();
    } catch (error) {
      console.error("Erreur fatale lors de l'envoi du message :", error);
      setIsChatLoading(false);
    }
  };

  // Gère la déconnexion de l'administrateur
  const handleLogout = () => {
    if (firebaseData && firebaseData.auth) {
      signOut(firebaseData.auth);
    }
  };

  // Gère les changements dans le formulaire d'ajout de produit
  const handleNewProductChange = (e) => {
    const { name, value, type, checked } = e.target;
    if (name === 'flashSaleIsActive') {
      setNewProduct(prev => ({
        ...prev,
        flashSale: {
          ...prev.flashSale,
          isActive: checked
        }
      }));
    } else if (name === 'salePrice' || name === 'endDate') {
      setNewProduct(prev => ({
        ...prev,
        flashSale: {
          ...prev.flashSale,
          [name]: value
        }
      }));
    } else {
      setNewProduct(prev => ({
        ...prev,
        [name]: type === 'checkbox' ? checked : value
      }));
    }
  };

  // Gère l'upload de l'image
  const handleImageUpload = async (file) => {
    if (!file) return null;

    if (!firebaseData || !firebaseData.storage) {
      console.error("Firebase Storage n'est pas initialisé.");
      setModalMessage("Erreur: Le service de stockage d'images n'est pas disponible.");
      setShowModal(true);
      return null;
    }

    setIsImageUploading(true);
    setModalMessage("Téléchargement de l'image en cours...");
    setShowModal(true);

    try {
      const storageRef = ref(firebaseData.storage, `product_images/${Date.now()}_${file.name}`);
      const snapshot = await uploadBytes(storageRef, file);
      const downloadUrl = await getDownloadURL(snapshot.ref);
      setModalMessage("Image téléchargée avec succès !");
      setTimeout(() => setShowModal(false), 2000);
      return downloadUrl;
    } catch (error) {
      console.error("Erreur lors de l'upload de l'image :", error);
      setModalMessage("Erreur lors du téléchargement de l'image. Veuillez vérifier les règles de sécurité de Firebase Storage.");
      setShowModal(true);
      return null;
    } finally {
      setIsImageUploading(false);
    }
  };

  // Gère la soumission du formulaire d'ajout/modification de produit
  const handleAddOrUpdateProduct = async (e) => {
    e.preventDefault();
    if (!firebaseData || !firebaseData.db) {
      console.error("Firestore n'est pas initialisé.");
      return;
    }
  
    setLoading(true);
    let newImageUrl = newProduct.imageUrl;
    let newHoverImageUrl = newProduct.hoverImageUrl;
  
    try {
      // Vérifier si un fichier a été sélectionné pour l'image principale
      const mainImageFile = document.getElementById('imageFile').files[0];
      if (mainImageFile) {
        newImageUrl = await handleImageUpload(mainImageFile);
        if (!newImageUrl) {
          setLoading(false);
          return; // Arrêter si l'upload a échoué
        }
      }
      // Vérifier si un fichier a été sélectionné pour l'image au survol
      const hoverImageFile = document.getElementById('hoverImageFile').files[0];
      if (hoverImageFile) {
        newHoverImageUrl = await handleImageUpload(hoverImageFile);
      }

      if (!newImageUrl) {
        setModalMessage("Erreur: L'URL de l'image principale est manquante ou l'upload a échoué.");
        setShowModal(true);
        setLoading(false);
        return;
      }
      
      const productData = {
        ...newProduct,
        imageUrl: newImageUrl,
        hoverImageUrl: newHoverImageUrl,
        originalPrice: parseFloat(newProduct.originalPrice),
        colors: newProduct.colors.split(',').map(c => c.trim()).filter(c => c),
        sizes: newProduct.sizes.split(',').map(s => s.trim()).filter(s => s),
        flashSale: {
          ...newProduct.flashSale,
          salePrice: newProduct.flashSale.salePrice ? parseFloat(newProduct.flashSale.salePrice) : null,
          endDate: newProduct.flashSale.endDate || null
        }
      };
      
      const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
      const productsCollection = collection(firebaseData.db, 'artifacts', appId, 'public', 'data', 'products');
      
      if (editingProductId) {
        // Mode modification
        const productDocRef = doc(productsCollection, editingProductId);
        await setDoc(productDocRef, productData);
        setModalMessage("Produit mis à jour avec succès !");
        setShowModal(true);
      } else {
        // Mode ajout
        const newProductId = generateUniqueId();
        const productDocRef = doc(productsCollection, newProductId);
        await setDoc(productDocRef, { ...productData, id: newProductId });
        setModalMessage("Produit ajouté avec succès !");
        setShowModal(true);
      }

      setNewProduct({ // Réinitialise le formulaire
        name: '', category: '', originalPrice: '', currency: 'CFA', imageUrl: '', hoverImageUrl: '', colors: '', sizes: '', isNew: true,
        flashSale: { isActive: false, salePrice: '', endDate: '' }
      });
      setEditingProductId(null); // Quitte le mode d'édition
    } catch (error) {
      console.error("Erreur lors de l'ajout/modification du produit :", error);
      setModalMessage("Erreur lors de l'enregistrement du produit. Veuillez vérifier les informations.");
      setShowModal(true);
    } finally {
      setLoading(false);
    }
  };

  // Gère le clic sur le bouton "Modifier" d'un produit
  const handleEditProduct = (product) => {
    setEditingProductId(product.id);
    setNewProduct({
      ...product,
      originalPrice: product.originalPrice.toString(),
      colors: Array.isArray(product.colors) ? product.colors.join(', ') : product.colors,
      sizes: Array.isArray(product.sizes) ? product.sizes.join(', ') : product.sizes,
      flashSale: {
        ...product.flashSale,
        salePrice: product.flashSale?.salePrice ? product.flashSale.salePrice.toString() : '',
        endDate: product.flashSale?.endDate || ''
      }
    });
  };

  // Gère l'annulation de la modification
  const handleCancelEdit = () => {
    setEditingProductId(null);
    setNewProduct({
      name: '', category: '', originalPrice: '', currency: 'CFA', imageUrl: '', hoverImageUrl: '', colors: '', sizes: '', isNew: true,
      flashSale: { isActive: false, salePrice: '', endDate: '' }
    });
  };

  // Ouvre le modal de confirmation de suppression
  const handleDeleteClick = (product) => {
    setProductToDelete(product);
    setShowDeleteModal(true);
  };

  // Gère la suppression du produit
  const confirmDeleteProduct = async () => {
    if (!firebaseData || !firebaseData.db || !productToDelete) {
      console.error("Firestore n'est pas initialisé ou aucun produit sélectionné.");
      return;
    }
    try {
      const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
      const productsCollection = collection(firebaseData.db, 'artifacts', appId, 'public', 'data', 'products');
      const productDocRef = doc(productsCollection, productToDelete.id);
      await deleteDoc(productDocRef);
      setModalMessage("Produit supprimé avec succès !");
      setShowModal(true);
    } catch (error) {
      console.error("Erreur lors de la suppression du produit :", error);
      setModalMessage("Erreur lors de la suppression du produit.");
      setShowModal(true);
    } finally {
      setShowDeleteModal(false);
      setProductToDelete(null);
    }
  };

  // Ajoute un produit au panier
  const addToCart = (product, quantity = 1, selectedColor = '', selectedSize = '') => {
    setCartItems(prevItems => {
      // Création d'une clé unique pour l'article en se basant sur l'ID, la couleur et la taille
      const uniqueCartItemId = `${product.id}-${selectedColor}-${selectedSize}`;
      const existingItem = prevItems.find(item => item.uniqueCartItemId === uniqueCartItemId);
      if (existingItem) {
        return prevItems.map(item =>
          item.uniqueCartItemId === uniqueCartItemId ? { ...item, quantity: item.quantity + quantity } : item
        );
      }
      return [...prevItems, { ...product, uniqueCartItemId, quantity, selectedColor, selectedSize }];
    });
    setModalMessage(`${product.name} a été ajouté au panier !`);
    setShowModal(true);
  };

  // Gère la mise à jour de la quantité d'un produit dans le panier
  const updateCartQuantity = (uniqueCartItemId, quantity) => {
    setCartItems(prevItems => {
      if (quantity <= 0) {
        return prevItems.filter(item => item.uniqueCartItemId !== uniqueCartItemId);
      }
      return prevItems.map(item =>
        item.uniqueCartItemId === uniqueCartItemId ? { ...item, quantity } : item
      );
    });
  };

  // Retire un produit du panier
  const removeFromCart = (uniqueCartItemId) => {
    setCartItems(prevItems => prevItems.filter(item => item.uniqueCartItemId !== uniqueCartItemId));
  };
  
  useEffect(() => {
    try {
      const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : {};

      if (Object.keys(firebaseConfig).length === 0) {
        console.error("Configuration Firebase non trouvée.");
        setLoading(false);
        return;
      }

      const app = initializeApp(firebaseConfig);
      const auth = getAuth(app);
      const db = getFirestore(app);
      const storage = getStorage(app);
      
      setFirebaseData({ db, auth, storage });

      const unsubscribe = onAuthStateChanged(auth, async (user) => {
        if (user) {
          console.log(`Utilisateur authentifié : ${user.uid}`);
          setUserId(user.uid);
          const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;
          if (initialAuthToken) {
            const tokenResult = await auth.currentUser.getIdTokenResult();
            if (tokenResult.claims.firebase.sign_in_provider === 'custom') {
              setIsAdminLoggedIn(true);
            }
          } else {
             setIsAdminLoggedIn(false);
          }

          const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
          const productsCollection = collection(db, 'artifacts', appId, 'public', 'data', 'products');
          const productsQuery = query(productsCollection);

          const unsubscribeProducts = onSnapshot(productsQuery, (snapshot) => {
            const productsList = snapshot.docs.map(doc => ({
              id: doc.id,
              ...doc.data()
            }));
            setProducts(productsList);
            setLoading(false);
          });
          
          return () => {
            unsubscribeProducts();
            unsubscribe();
          }

        } else {
          const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;
          
          if (initialAuthToken) {
            await signInWithCustomToken(auth, initialAuthToken).catch(error => {
              console.error("Erreur de connexion avec le token personnalisé :", error);
              signInAnonymously(auth).catch(anonError => console.error("Erreur de connexion anonyme :", anonError));
            });
          } else {
            signInAnonymously(auth).catch(error => console.error("Erreur de connexion anonyme :", error));
          }
          setUserId(null);
          setIsAdminLoggedIn(false);
          setLoading(false);
        }
      });
      return () => unsubscribe();
    } catch (error) {
      console.error("Erreur d'initialisation de Firebase :", error);
      setLoading(false);
    }
  }, []);

  useEffect(() => {
    if (messagesEndRef.current) {
      messagesEndRef.current.scrollIntoView({ behavior: 'smooth' });
    }
  }, [messages]);

  // Composant du modal de confirmation
  const ConfirmationModal = ({ message, onClose }) => (
    <div className="fixed inset-0 bg-gray-600 bg-opacity-50 flex items-center justify-center z-50 p-4">
      <div className="bg-[#FDF5E6] p-6 rounded-lg shadow-xl text-center max-w-sm w-full mx-auto">
        <h4 className="text-xl font-bold mb-4">Notification</h4>
        <p className="text-gray-700 mb-6">{message}</p>
        <button
          onClick={onClose}
          className="bg-[#D4AF37] text-white p-2 rounded-lg font-semibold hover:bg-[#800020]"
        >
          Fermer
        </button>
      </div>
    </div>
  );

  // Composant pour le tableau de bord administrateur
  const AdminDashboard = () => (
    <div className="p-4 sm:p-8 rounded-lg shadow-2xl border border-[#D4AF37] max-w-full lg:max-w-6xl w-full text-center bg-[#FDF5E6] mx-auto">
      <div className="flex flex-col sm:flex-row justify-between items-center mb-6">
        <h2 className="text-xl sm:text-3xl font-bold text-[#1C1C1C] mb-4 sm:mb-0">Tableau de Bord Administrateur</h2>
        <div className="flex flex-col sm:flex-row space-y-2 sm:space-y-0 sm:space-x-4">
          <button
            onClick={() => setIsAdminLoggedIn(false)}
            className="bg-red-500 text-white p-2 rounded-lg font-semibold hover:bg-red-600 transition-colors duration-300 shadow-md"
          >
            Retour à la boutique
          </button>
          <button 
            onClick={handleLogout}
            className="bg-gray-500 text-white p-2 rounded-lg font-semibold hover:bg-gray-600 transition-colors duration-300 shadow-md"
          >
            Déconnexion
          </button>
        </div>
      </div>
      <p className="text-xs sm:text-lg mb-4 text-[#D4AF37] break-words"><span className="font-semibold">ID Administrateur :</span> {userId}</p>
      {/* Section d'ajout/modification de produit */}
      <div className="mt-4 sm:mt-8 mb-6 sm:mb-12 p-4 sm:p-6 border border-gray-300 rounded-lg bg-gray-50">
        <h3 className="text-lg sm:text-2xl font-semibold mb-4 text-[#1C1C1C]">{editingProductId ? 'Modifier le Produit' : 'Ajouter un Nouveau Produit'}</h3>
        <form onSubmit={handleAddOrUpdateProduct} className="grid grid-cols-1 md:grid-cols-2 gap-4 text-left">
          {/* Nom du produit */}
          <div>
            <label htmlFor="name" className="block text-sm font-medium text-gray-700">Nom du produit</label>
            <input type="text" id="name" name="name" value={newProduct.name} onChange={handleNewProductChange} required className="w-full mt-1 p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-[#D4AF37]" />
          </div>
          {/* Catégorie */}
          <div>
            <label htmlFor="category" className="block text-sm font-medium text-gray-700">Catégorie</label>
            <input type="text" id="category" name="category" value={newProduct.category} onChange={handleNewProductChange} required className="w-full mt-1 p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-[#D4AF37]" />
          </div>
          {/* Prix original */}
          <div>
            <label htmlFor="originalPrice" className="block text-sm font-medium text-gray-700">Prix original (CFA)</label>
            <input type="number" id="originalPrice" name="originalPrice" value={newProduct.originalPrice} onChange={handleNewProductChange} required className="w-full mt-1 p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-[#D4AF37]" />
          </div>
          {/* URL de l'image OU upload de fichier */}
          <div className="col-span-1 md:col-span-2">
            <label htmlFor="imageFile" className="block text-sm font-medium text-gray-700">Image principale (choisir un fichier)</label>
            <input type="file" id="imageFile" name="imageFile" accept="image/*" className="w-full mt-1 p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-[#D4AF37]" />
            <p className="text-center text-gray-500 my-2">-- OU --</p>
            <input type="text" id="imageUrl" name="imageUrl" placeholder="Entrer une URL d'image" value={newProduct.imageUrl} onChange={handleNewProductChange} className="w-full mt-1 p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-[#D4AF37]" />
          </div>
          {/* URL de l'image au survol OU upload de fichier */}
          <div className="col-span-1 md:col-span-2">
            <label htmlFor="hoverImageFile" className="block text-sm font-medium text-gray-700">Image au survol (choisir un fichier)</label>
            <input type="file" id="hoverImageFile" name="hoverImageFile" accept="image/*" className="w-full mt-1 p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-[#D4AF37]" />
            <p className="text-center text-gray-500 my-2">-- OU --</p>
            <input type="text" id="hoverImageUrl" name="hoverImageUrl" placeholder="Entrer une URL d'image au survol" value={newProduct.hoverImageUrl} onChange={handleNewProductChange} className="w-full mt-1 p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-[#D4AF37]" />
          </div>
          {/* Couleurs (séparées par des virgules) */}
          <div>
            <label htmlFor="colors" className="block text-sm font-medium text-gray-700">Couleurs (ex: red, blue, #FF0000)</label>
            <input type="text" id="colors" name="colors" value={newProduct.colors} onChange={handleNewProductChange} className="w-full mt-1 p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-[#D4AF37]" />
          </div>
          {/* Tailles (séparées par des virgules) */}
          <div>
            <label htmlFor="sizes" className="block text-sm font-medium text-gray-700">Tailles (ex: S,M,L)</label>
            <input type="text" id="sizes" name="sizes" value={newProduct.sizes} onChange={handleNewProductChange} className="w-full mt-1 p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-[#D4AF37]" />
          </div>
          {/* Nouveau produit */}
          <div className="flex items-center">
            <input type="checkbox" id="isNew" name="isNew" checked={newProduct.isNew} onChange={handleNewProductChange} className="mr-2 h-4 w-4 text-[#D4AF37] focus:ring-[#D4AF37] border-gray-300 rounded" />
            <label htmlFor="isNew" className="text-sm font-medium text-gray-700">Nouveau produit ?</label>
          </div>
          {/* Boutons d'action */}
          <div className="col-span-1 md:col-span-2 flex flex-col sm:flex-row justify-between space-y-2 sm:space-y-0 sm:space-x-4">
            {editingProductId ? (
              <>
                <button
                  type="submit"
                  disabled={isImageUploading}
                  className="w-full bg-green-500 text-white p-3 rounded-lg font-semibold hover:bg-green-600 transition-colors duration-300 shadow-md disabled:bg-green-300"
                >
                  {isImageUploading ? "Téléchargement..." : "Mettre à jour le produit"}
                </button>
                <button
                  type="button"
                  onClick={handleCancelEdit}
                  className="w-full bg-gray-500 text-white p-3 rounded-lg font-semibold hover:bg-gray-600 transition-colors duration-300 shadow-md"
                >
                  Annuler
                </button>
              </>
            ) : (
              <button
                type="submit"
                disabled={isImageUploading}
                className="w-full bg-[#D4AF37] text-[#1C1C1C] p-3 rounded-lg font-semibold hover:bg-[#800020] transition-colors duration-300 shadow-md disabled:bg-[#D4AF37]/50"
              >
                {isImageUploading ? "Téléchargement..." : "Ajouter le produit"}
              </button>
            )}
          </div>
        </form>
      </div>

      {/* Section des produits existants */}
      <div className="mt-8">
        <h3 className="text-xl sm:text-2xl font-semibold mb-4 text-[#1C1C1C]">Produits existants</h3>
        {products.length === 0 ? (
          <p className="italic text-gray-500">Aucun produit trouvé. Ajoutez-en un ci-dessus.</p>
        ) : (
          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
            {products.map((product) => (
              <div key={product.id} className="bg-[#FDF5E6] rounded-lg shadow-md p-4 text-left border border-gray-200">
                {/* Aperçu de l'image ajoutée ici */}
                <img src={product.imageUrl} alt={product.name} className="w-full h-48 object-cover rounded-lg mb-4" />
                <h4 className="text-lg font-bold text-[#1C1C1C]">{product.name}</h4>
                <p className="text-gray-600 text-sm">{product.category}</p>
                <p className="text-lg font-semibold text-[#D4AF37] mt-2">{product.originalPrice} {product.currency}</p>
                <p className="text-xs text-gray-500 mt-2 break-all">ID: {product.id}</p>
                <div className="flex flex-col sm:flex-row mt-4 space-y-2 sm:space-y-0 sm:space-x-2">
                  <button onClick={() => handleEditProduct(product)} className="flex-1 bg-yellow-500 text-white p-2 rounded-lg text-sm font-semibold hover:bg-yellow-600 transition-colors duration-300">Modifier</button>
                  <button onClick={() => handleDeleteClick(product)} className="flex-1 bg-red-500 text-white p-2 rounded-lg text-sm font-semibold hover:bg-red-600 transition-colors duration-300">Supprimer</button>
                </div>
              </div>
            ))}
          </div>
        )}
      </div>

      {/* Modal de confirmation de suppression */}
      {showDeleteModal && (
        <div className="fixed inset-0 bg-gray-600 bg-opacity-50 flex items-center justify-center z-50 p-4">
          <div className="bg-[#FDF5E6] p-6 rounded-lg shadow-xl text-center max-w-sm w-full mx-auto">
            <h4 className="text-xl font-bold mb-4">Confirmation de suppression</h4>
            <p className="text-gray-700 mb-6">Êtes-vous sûr de vouloir supprimer le produit "{productToDelete?.name}" ? Cette action est irréversible.</p>
            <div className="flex flex-col sm:flex-row justify-center space-y-2 sm:space-y-0 sm:space-x-4">
              <button
                onClick={() => setShowDeleteModal(false)}
                className="bg-gray-300 text-gray-800 p-2 rounded-lg font-semibold hover:bg-gray-400"
              >
                Annuler
              </button>
              <button
                onClick={confirmDeleteProduct}
                className="bg-red-500 text-white p-2 rounded-lg font-semibold hover:bg-red-600"
              >
                Supprimer
              </button>
            </div>
          </div>
        </div>
      )}
    </div>
  );

  // Composant de la grille de produits (page d'accueil)
  const ProductGrid = ({ products, onProductClick }) => (
    <main className="p-4 sm:p-8">
      <h2 className="text-2xl sm:text-4xl font-bold text-center mb-8 sm:mb-12 text-[#FDF5E6]">Notre Collection</h2>
      <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4 sm:gap-8">
        {products.length === 0 ? (
          <p className="italic text-gray-400 col-span-full text-center">Aucun produit n'est disponible pour le moment.</p>
        ) : (
          products.map((product) => (
            <div key={product.id} className="bg-[#FDF5E6] rounded-lg shadow-md p-4 group overflow-hidden relative transform transition-transform duration-300 hover:scale-105 cursor-pointer" onClick={() => onProductClick(product)}>
              <div className="relative overflow-hidden rounded-lg">
                <img 
                  src={product.imageUrl} 
                  alt={product.name} 
                  className="w-full h-48 sm:h-64 object-cover transition-opacity duration-300 group-hover:opacity-0"
                />
                {product.hoverImageUrl && (
                  <img 
                    src={product.hoverImageUrl} 
                    alt={`Hover for ${product.name}`} 
                    className="w-full h-48 sm:h-64 object-cover absolute top-0 left-0 opacity-0 transition-opacity duration-300 group-hover:opacity-100"
                  />
                )}
              </div>
              <div className="mt-4 text-center">
                <h4 className="text-lg font-bold text-[#1C1C1C]">{product.name}</h4>
                <p className="text-sm text-gray-600">{product.category}</p>
                <p className="text-xl font-semibold text-[#D4AF37] mt-2">{product.originalPrice} {product.currency}</p>
              </div>
            </div>
          ))
        )}
      </div>
    </main>
  );

  // Composant de la page de détails d'un produit
  const ProductDetail = ({ product, onBackClick, onAddToCart }) => {
    // Initialise les états de sélection. Si le tableau d'options est vide, l'état est une chaîne vide.
    const [selectedColor, setSelectedColor] = useState(Array.isArray(product.colors) && product.colors.length > 0 ? product.colors[0] : '');
    const [selectedSize, setSelectedSize] = useState(Array.isArray(product.sizes) && product.sizes.length > 0 ? product.sizes[0] : '');
    const [quantity, setQuantity] = useState(1);
  
    // Logique pour déterminer si le bouton doit être désactivé
    // Le bouton est désactivé seulement si le produit a des options de couleur ET/OU de taille, mais qu'une sélection n'a pas été faite.
    const isAddToCartDisabled = (Array.isArray(product.colors) && product.colors.length > 0 && !selectedColor) || (Array.isArray(product.sizes) && product.sizes.length > 0 && !selectedSize);
  
    return (
      <main className="p-4 sm:p-8 max-w-5xl mx-auto">
        <button onClick={onBackClick} className="flex items-center text-[#D4AF37] font-semibold mb-4 sm:mb-8 hover:underline">
          <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5 mr-2" viewBox="0 0 20 20" fill="currentColor">
            <path fillRule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clipRule="evenodd" />
          </svg>
          Retour aux produits
        </button>
  
        <div className="grid grid-cols-1 md:grid-cols-2 gap-4 sm:gap-8 bg-[#FDF5E6] p-4 sm:p-6 rounded-lg shadow-xl">
          <div className="relative">
            <img src={product.imageUrl} alt={product.name} className="w-full h-auto rounded-lg" />
          </div>
          <div>
            <h1 className="text-2xl sm:text-4xl font-bold mb-2 text-[#1C1C1C]">{product.name}</h1>
            <p className="text-base sm:text-xl text-gray-600 mb-2 sm:mb-4">{product.category}</p>
            <p className="text-xl sm:text-3xl font-bold text-[#D4AF37] mb-4 sm:mb-6">{product.originalPrice} {product.currency}</p>
  
            {Array.isArray(product.colors) && product.colors.length > 0 && (
              <div className="mb-4 sm:mb-6">
                <h3 className="text-base sm:text-lg font-semibold text-[#1C1C1C] mb-2">Couleurs disponibles</h3>
                <div className="flex flex-wrap gap-2">
                  {product.colors.map(color => (
                    <div
                      key={color}
                      className={`h-6 w-6 sm:h-8 sm:w-8 rounded-full border-2 cursor-pointer ${selectedColor === color ? 'border-[#D4AF37]' : 'border-gray-300'}`}
                      style={{ backgroundColor: color }}
                      onClick={() => setSelectedColor(color)}
                    ></div>
                  ))}
                </div>
              </div>
            )}
  
            {Array.isArray(product.sizes) && product.sizes.length > 0 && (
              <div className="mb-4 sm:mb-8">
                <h3 className="text-base sm:text-lg font-semibold text-[#1C1C1C] mb-2">Tailles disponibles</h3>
                <div className="flex flex-wrap gap-2">
                  {product.sizes.map(size => (
                    <button
                      key={size}
                      className={`p-2 px-3 sm:px-4 border rounded-lg font-semibold text-sm transition-colors duration-200 ${selectedSize === size ? 'bg-[#D4AF37] text-[#1C1C1C]' : 'bg-gray-100 text-gray-800 hover:bg-gray-200'}`}
                      onClick={() => setSelectedSize(size)}
                    >
                      {size}
                    </button>
                  ))}
                </div>
              </div>
            )}
            
            <div className="flex items-center mb-4 sm:mb-6">
              <label htmlFor="quantity" className="text-base sm:text-lg font-semibold text-[#1C1C1C] mr-2 sm:mr-4">Quantité:</label>
              <input 
                type="number"
                id="quantity"
                value={quantity}
                min="1"
                onChange={(e) => setQuantity(Math.max(1, parseInt(e.target.value) || 1))}
                className="w-16 sm:w-20 p-2 border border-gray-300 rounded-lg text-center"
              />
            </div>
  
            <button 
              onClick={() => onAddToCart(product, quantity, selectedColor, selectedSize)}
              className="w-full bg-[#D4AF37] text-[#1C1C1C] p-3 sm:p-4 rounded-lg font-bold text-base sm:text-lg hover:bg-[#800020] transition-colors duration-300"
              disabled={isAddToCartDisabled}
            >
              Ajouter au panier
            </button>
          </div>
        </div>
      </main>
    );
  };
  
  // Composant de la page du panier
  const ShoppingCart = ({ cartItems, onUpdateQuantity, onRemoveItem, onGoToCheckout, onBackClick }) => {
    const total = cartItems.reduce((sum, item) => sum + item.originalPrice * item.quantity, 0);
  
    return (
      <main className="p-4 sm:p-8 max-w-5xl mx-auto">
        <div className="flex flex-col sm:flex-row justify-between items-center mb-4 sm:mb-8">
          <button onClick={onBackClick} className="flex items-center text-[#D4AF37] font-semibold mb-4 sm:mb-0 hover:underline">
            <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5 mr-2" viewBox="0 0 20 20" fill="currentColor">
              <path fillRule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clipRule="evenodd" />
            </svg>
            Continuer vos achats
          </button>
          <h2 className="text-2xl sm:text-3xl font-bold text-[#FDF5E6]">Votre Panier</h2>
        </div>
  
        {cartItems.length === 0 ? (
          <p className="text-center text-gray-400 italic text-lg mt-8 sm:mt-16">Votre panier est vide.</p>
        ) : (
          <div className="grid grid-cols-1 lg:grid-cols-3 gap-4 sm:gap-8">
            <div className="lg:col-span-2 space-y-4 sm:space-y-6">
              {cartItems.map(item => (
                <div key={item.uniqueCartItemId} className="flex flex-col sm:flex-row items-center justify-between p-4 bg-[#FDF5E6] rounded-lg shadow-md">
                  <div className="flex flex-col sm:flex-row items-center space-y-4 sm:space-y-0 sm:space-x-4 text-center sm:text-left">
                    <img src={item.imageUrl} alt={item.name} className="w-24 h-24 object-cover rounded-lg" />
                    <div>
                      <h4 className="font-bold text-lg text-[#1C1C1C]">{item.name}</h4>
                      <p className="text-gray-600 text-sm">
                        {item.category}
                        {item.selectedColor && ` | Couleur: ${item.selectedColor}`}
                        {item.selectedSize && ` | Taille: ${item.selectedSize}`}
                      </p>
                      <p className="font-semibold text-[#D4AF37] mt-1">{item.originalPrice} {item.currency}</p>
                    </div>
                  </div>
                  <div className="flex items-center space-x-4 mt-4 sm:mt-0">
                    <input
                      type="number"
                      value={item.quantity}
                      min="1"
                      onChange={(e) => onUpdateQuantity(item.uniqueCartItemId, parseInt(e.target.value) || 1)}
                      className="w-16 p-2 text-center border border-gray-300 rounded-lg"
                    />
                    <button onClick={() => onRemoveItem(item.uniqueCartItemId)} className="text-red-500 hover:text-red-700">
                      <svg xmlns="http://www.w3.org/2000/svg" className="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
                        <path strokeLinecap="round" strokeLinejoin="round" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
                      </svg>
                    </button>
                  </div>
                </div>
              ))}
            </div>
            <div className="bg-[#FDF5E6] p-6 rounded-lg shadow-md h-fit mt-4 lg:mt-0">
              <h3 className="text-xl sm:text-2xl font-bold mb-4 text-[#1C1C1C]">Résumé de la commande</h3>
              <div className="flex justify-between font-semibold text-base sm:text-lg text-gray-700 mb-4">
                <span>Total :</span>
                <span>{total.toFixed(2)} {cartItems[0]?.currency || 'CFA'}</span>
              </div>
              <button 
                onClick={onGoToCheckout}
                className="w-full bg-[#D4AF37] text-[#1C1C1C] p-3 rounded-lg font-bold text-base sm:text-lg hover:bg-[#800020] transition-colors duration-300"
              >
                Passer au paiement
              </button>
            </div>
          </div>
        )}
      </main>
    );
  };
  
  // Composant de la page de paiement
  const CheckoutPage = ({ cartItems, onBackToCart }) => {
    const total = cartItems.reduce((sum, item) => sum + item.originalPrice * item.quantity, 0);
    const [clientInfo, setClientInfo] = useState({
      fullName: '',
      ville: '',
      cartier: '',
      rueNumero: '',
      phone: ''
    });

    const handleInputChange = (e) => {
      const { name, value } = e.target;
      setClientInfo(prev => ({ ...prev, [name]: value }));
    };

    const handleCheckout = (e) => {
      e.preventDefault();
      // Création du message WhatsApp avec les informations du client et du panier
      let message = `NOUVELLE COMMANDE NG_chopi_DIAMS\n\n`;
      message += `Nom du client: ${clientInfo.fullName}\n`;
      message += `Adresse de livraison:\n`;
      message += `- Ville: ${clientInfo.ville}\n`;
      message += `- Cartier: ${clientInfo.cartier}\n`;
      message += `- Rue et Numéro: ${clientInfo.rueNumero}\n`;
      message += `Numéro de téléphone: ${clientInfo.phone}\n\n`;
      message += `Détails de la commande:\n`;

      cartItems.forEach(item => {
        message += `- ${item.name} (x${item.quantity})`;
        if (item.selectedColor) message += ` | Couleur: ${item.selectedColor}`;
        if (item.selectedSize) message += ` | Taille: ${item.selectedSize}`;
        message += ` - ${(item.originalPrice * item.quantity).toFixed(2)} ${item.currency}\n`;
      });

      message += `\nTotal: ${total.toFixed(2)} ${cartItems[0]?.currency || 'CFA'}\n\n`;
      message += `Merci pour votre commande !`;

      const whatsappUrl = `https://wa.me/242064230404?text=${encodeURIComponent(message)}`;
      window.open(whatsappUrl, '_blank');
    };
  
    return (
      <main className="p-4 sm:p-8 max-w-5xl mx-auto">
        <button onClick={onBackToCart} className="flex items-center text-[#D4AF37] font-semibold mb-4 sm:mb-8 hover:underline">
          <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5 mr-2" viewBox="0 0 20 20" fill="currentColor">
            <path fillRule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clipRule="evenodd" />
          </svg>
          Retour au panier
        </button>
        <h2 className="text-2xl sm:text-3xl font-bold text-center mb-4 sm:mb-8 text-[#FDF5E6]">Paiement</h2>
        <div className="grid grid-cols-1 md:grid-cols-2 gap-4 sm:gap-8">
          <div className="bg-[#FDF5E6] p-4 sm:p-6 rounded-lg shadow-md">
            <h3 className="text-lg sm:text-xl font-bold mb-4 text-[#1C1C1C]">Récapitulatif de la commande</h3>
            <ul className="space-y-2">
              {cartItems.map(item => (
                <li key={item.uniqueCartItemId} className="flex justify-between items-center text-gray-700 text-sm">
                  <span className="font-semibold">{item.name} ({item.selectedColor}, {item.selectedSize}, x{item.quantity})</span>
                  <span>{(item.originalPrice * item.quantity).toFixed(2)} {item.currency}</span>
                </li>
              ))}
            </ul>
            <div className="border-t border-gray-300 mt-4 pt-4 flex justify-between font-bold text-base sm:text-xl text-[#1C1C1C]">
              <span>Total final :</span>
              <span>{total.toFixed(2)} {cartItems[0]?.currency || 'CFA'}</span>
            </div>
          </div>
          <div className="bg-[#FDF5E6] p-4 sm:p-6 rounded-lg shadow-md">
            <h3 className="text-lg sm:text-xl font-bold mb-4 text-[#1C1C1C]">Informations de livraison</h3>
            <form onSubmit={handleCheckout} className="space-y-4">
              <div>
                <label className="block text-sm font-semibold mb-1">Nom complet</label>
                <input type="text" name="fullName" value={clientInfo.fullName} onChange={handleInputChange} required className="w-full p-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-[#D4AF37]" />
              </div>
              <div>
                <label className="block text-sm font-semibold mb-1">Ville</label>
                <input type="text" name="ville" value={clientInfo.ville} onChange={handleInputChange} required className="w-full p-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-[#D4AF37]" />
              </div>
              <div>
                <label className="block text-sm font-semibold mb-1">Cartier</label>
                <input type="text" name="cartier" value={clientInfo.cartier} onChange={handleInputChange} required className="w-full p-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-[#D4AF37]" />
              </div>
              <div>
                <label className="block text-sm font-semibold mb-1">Rue et Numéro</label>
                <input type="text" name="rueNumero" value={clientInfo.rueNumero} onChange={handleInputChange} required className="w-full p-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-[#D4AF37]" />
              </div>
              <div>
                <label className="block text-sm font-semibold mb-1">Numéro de téléphone</label>
                <input type="tel" name="phone" value={clientInfo.phone} onChange={handleInputChange} required className="w-full p-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-[#D4AF37]" />
              </div>
              <button type="submit" className="w-full bg-green-500 text-white p-3 rounded-lg font-bold text-lg hover:bg-green-600 transition-colors duration-300">
                Confirmer la commande
              </button>
            </form>
          </div>
        </div>
      </main>
    );
  };


  const ClientInterface = () => (
    <div className="min-h-screen bg-[#0D1B2A] text-[#1C1C1C] font-sans">
      <header className="p-4 sm:p-6 flex flex-col sm:flex-row justify-between items-center bg-white shadow-md">
        <h1 className="text-2xl sm:text-3xl font-bold text-[#1C1C1C] mb-2 sm:mb-0">NG_chopi_DIAMS</h1>
        <div className="flex items-center space-x-2 sm:space-x-4">
          <button
            onClick={() => setCurrentPage('productGrid')}
            className="text-[#1C1C1C] font-semibold hover:underline text-sm sm:text-base"
          >
            Boutique
          </button>
          <button 
            onClick={() => setCurrentPage('cart')}
            className="relative bg-[#D4AF37] text-white p-2 rounded-lg font-semibold hover:bg-[#800020] transition-colors duration-300"
          >
            <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5 sm:h-6 sm:w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
              <path strokeLinecap="round" strokeLinejoin="round" d="M3 3h2l.4 2M7 13h10l1.4-7H5.4m7.2 7a2 2 0 11-4 0 2 2 0 014 0zm3.6-7a2 2 0 11-4 0 2 2 0 014 0z" />
            </svg>
            {cartItems.length > 0 && (
              <span className="absolute -top-1 -right-1 sm:-top-2 sm:-right-2 bg-red-500 text-white text-xs font-bold rounded-full h-4 w-4 sm:h-5 sm:w-5 flex items-center justify-center">
                {cartItems.length}
              </span>
            )}
          </button>
          <button
            onClick={() => setIsAdminLoggedIn(true)}
            className="bg-[#D4AF37] text-[#1C1C1C] p-2 px-3 sm:px-4 rounded-lg font-semibold hover:bg-[#800020] transition-colors duration-300 text-sm sm:text-base"
          >
            Espace Admin
          </button>
        </div>
      </header>
      {currentPage === 'productGrid' && (
        <ProductGrid 
          products={products} 
          onProductClick={(product) => {
            setSelectedProduct(product);
            setCurrentPage('productDetail');
          }}
        />
      )}
      {currentPage === 'productDetail' && (
        <ProductDetail 
          product={selectedProduct} 
          onBackClick={() => setCurrentPage('productGrid')}
          onAddToCart={addToCart}
        />
      )}
      {currentPage === 'cart' && (
        <ShoppingCart
          cartItems={cartItems}
          onUpdateQuantity={updateCartQuantity}
          onRemoveItem={removeFromCart}
          onGoToCheckout={() => setCurrentPage('checkout')}
          onBackClick={() => setCurrentPage('productGrid')}
        />
      )}
      {currentPage === 'checkout' && (
        <CheckoutPage
          cartItems={cartItems}
          onBackToCart={() => setCurrentPage('cart')}
        />
      )}
    </div>
  );

  return (
    <div className="relative min-h-screen bg-[#0D1B2A] text-[#1C1C1C] font-sans flex items-center justify-center p-4 overflow-hidden">
      {/* Container pour le ciel étoilé */}
      <div className="star-field">
        <div className="stars"></div>
        <div className="twinkling"></div>
      </div>
      {loading ? (
        <p className="text-xl text-white">Connexion à Firebase en cours...</p>
      ) : (
        <>
          {isAdminLoggedIn ? (
            <AdminDashboard />
          ) : (
            <ClientInterface />
          )}
        </>
      )}
      {/* Bouton du chatbot */}
      <button 
        onClick={() => setIsChatOpen(!isChatOpen)}
        className="fixed bottom-6 right-6 p-3 sm:p-4 bg-[#D4AF37] text-[#1C1C1C] rounded-full shadow-lg hover:bg-[#800020] transition-colors duration-300 z-50 transform hover:scale-110"
      >
        <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5 sm:h-6 sm:w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
          <path strokeLinecap="round" strokeLinejoin="round" d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.896 9.896 0 01-4.81-1.341A5.986 5.986 0 005 18H4a1 1 0 01-1-1v-4a1 1 0 011-1h.01A5.986 5.986 0 003 12c0-4.418 4.03-8 9-8s9 3.582 9 8z" />
        </svg>
      </button>
      {/* Fenêtre du chatbot */}
      {isChatOpen && (
        <div className="fixed bottom-20 sm:bottom-24 right-4 sm:right-6 w-full max-w-xs sm:max-w-sm h-80 sm:h-96 bg-white rounded-lg shadow-2xl flex flex-col z-50 border border-[#D4AF37] animate-fade-in">
          <div className="flex justify-between items-center bg-[#1C1C1C] text-white p-3 sm:p-4 rounded-t-lg">
            <h3 className="text-base sm:text-lg font-bold">Assistant ✨ NG_chopi_DIAMS</h3>
            <button onClick={() => setIsChatOpen(false)} className="text-white hover:text-gray-400">
              <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5 sm:h-6 sm:w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
                <path strokeLinecap="round" strokeLinejoin="round" d="M6 18L18 6M6 6l12 12" />
              </svg>
            </button>
          </div>
          <div className="flex-1 overflow-y-auto p-4 space-y-4 custom-scrollbar">
            {messages.length === 0 ? (
              <p className="text-center text-gray-500 italic text-sm">Bonjour ! Comment puis-je vous aider ?</p>
            ) : (
              messages.map((msg, index) => (
                <div key={index} className={`flex ${msg.sender === 'user' ? 'justify-end' : 'justify-start'}`}>
                  <div className={`p-3 rounded-lg max-w-[80%] text-sm ${msg.sender === 'user' ? 'bg-[#D4AF37] text-[#1C1C1C]' : 'bg-gray-200 text-[#1C1C1C]'}`}>
                    {msg.text}
                  </div>
                </div>
              ))
            )}
            {isChatLoading && (
              <div className="flex justify-start">
                <div className="p-3 rounded-lg bg-gray-200 text-[#1C1C1C] text-sm">
                  <div className="animate-pulse">...</div>
                </div>
              </div>
            )}
            <div ref={messagesEndRef} />
          </div>
          <div className="p-3 border-t border-gray-200">
            <div className="flex">
              <input
                type="text"
                value={chatInput}
                onChange={(e) => setChatInput(e.target.value)}
                onKeyDown={(e) => { if (e.key === 'Enter') handleSendMessage(); }}
                className="flex-1 p-2 rounded-l-lg border border-gray-300 text-sm focus:outline-none focus:ring-2 focus:ring-[#D4AF37]"
                placeholder="Écrivez votre message..."
                disabled={isChatLoading}
              />
              <button
                onClick={handleSendMessage}
                disabled={isChatLoading}
                className="bg-[#D4AF37] text-white p-2 rounded-r-lg hover:bg-[#800020] transition-colors duration-300 disabled:bg-gray-400"
              >
                <svg xmlns="http://www.w3.org/2000/svg" className="h-5 w-5 sm:h-6 sm:w-6 transform rotate-90" fill="none" viewBox="0 0 24 24" stroke="currentColor" strokeWidth={2}>
                  <path d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8" />
                </svg>
              </button>
            </div>
          </div>
        </div>
      )}
      {showModal && (
        <ConfirmationModal 
          message={modalMessage} 
          onClose={() => setShowModal(false)} 
        />
      )}
      <style>{`
        .custom-scrollbar::-webkit-scrollbar { width: 8px; }
        .custom-scrollbar::-webkit-scrollbar-track { background: #f1f1f1; }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: #888; border-radius: 4px; }
        .custom-scrollbar::-webkit-scrollbar-thumb:hover { background: #555; }
        @keyframes fadeIn {
          from { opacity: 0; transform: translateY(20px); }
          to { opacity: 1; transform: translateY(0); }
        }
        .animate-fade-in { animation: fadeIn 0.3s ease-out; }

        /* Styles pour le ciel étoilé */
        .star-field {
          position: fixed;
          top: 0;
          left: 0;
          width: 100%;
          height: 100%;
          z-index: -1;
          background-color: #0D1B2A; /* Fond bleu nuit */
        }
        
        @keyframes twinkling {
          0% { opacity: 0.5; }
          50% { opacity: 1; }
          100% { opacity: 0.5; }
        }

        .stars, .twinkling {
          position: absolute;
          top: 0;
          left: 0;
          right: 0;
          bottom: 0;
          width: 100%;
          height: 100%;
          display: block;
        }

        .stars {
          background: transparent;
          box-shadow: 
            10px 10px #FFF, 
            20px 30px #FFF, 
            40px 50px #FFF, 
            70px 80px #FFF,
            90px 10px #FFF, 
            100px 30px #FFF, 
            120px 50px #FFF, 
            150px 80px #FFF,
            180px 100px #FFF,
            200px 150px #FFF,
            15px 120px #FFF, 
            35px 140px #FFF, 
            55px 160px #FFF, 
            85px 180px #FFF,
            115px 200px #FFF,
            135px 220px #FFF,
            165px 240px #FFF,
            195px 260px #FFF,
            215px 280px #FFF,
            235px 300px #FFF,
            255px 320px #FFF,
            275px 340px #FFF,
            295px 360px #FFF,
            315px 380px #FFF,
            335px 400px #FFF,
            355px 420px #FFF,
            375px 440px #FFF,
            395px 460px #FFF,
            415px 480px #FFF,
            435px 500px #FFF,
            455px 520px #FFF,
            475px 540px #FFF,
            495px 560px #FFF,
            515px 580px #FFF,
            535px 600px #FFF,
            555px 620px #FFF,
            575px 640px #FFF,
            595px 660px #FFF,
            615px 680px #FFF,
            635px 700px #FFF,
            655px 720px #FFF,
            675px 740px #FFF,
            695px 760px #FFF,
            715px 780px #FFF,
            735px 800px #FFF,
            755px 820px #FFF,
            775px 840px #FFF,
            795px 860px #FFF,
            815px 880px #FFF,
            835px 900px #FFF,
            855px 920px #FFF,
            875px 940px #FFF,
            895px 960px #FFF,
            915px 980px #FFF,
            935px 1000px #FFF,
            955px 1020px #FFF,
            975px 1040px #FFF,
            995px 1060px #FFF;
        }

      `}</style>
    </div>
  );
}

export default App;
