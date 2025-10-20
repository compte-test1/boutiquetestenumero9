// ----------------------- src/index.js -----------------------
import React from "react";
import { createRoot } from "react-dom/client";
import App from "./App";
import "./index.css";

const container = document.getElementById("root");
const root = createRoot(container);
root.render(<App />);

// ----------------------- src/index.css -----------------------
:root{
  --primary:#111822;
  --secondary:#333740;
  --neutral:#6C6A6A;
  --brownDark:#4F362A;
  --brownSoft:#745850;
  --gold:#F3B699;
  --bg: var(--primary);
  --text: #ffffff;
}

*{box-sizing:border-box;margin:0;padding:0;font-family:Inter, Poppins, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;}
body{background:var(--bg); color:var(--text); -webkit-font-smoothing:antialiased; -moz-osx-font-smoothing:grayscale;}
.container{max-width:1100px;margin:24px auto;padding:16px;}
.card{background:var(--secondary);border-radius:16px;padding:16px;box-shadow:0 6px 18px rgba(0,0,0,0.6);}
.btn{background:var(--gold);color:var(--primary);padding:10px 18px;border-radius:12px;border:none;font-weight:600;cursor:pointer;box-shadow:0 6px 16px rgba(0,0,0,0.4);}
.btn:hover{transform:translateY(-2px);box-shadow:0 10px 22px rgba(0,0,0,0.6);}
.nav{display:flex;align-items:center;justify-content:space-between;padding:12px 20px;background:linear-gradient(180deg,var(--primary),#0f1418);border-bottom:1px solid rgba(255,255,255,0.03);}
.nav .brand{color:var(--gold);font-weight:800;font-size:20px;letter-spacing:0.6px;}
.products{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:16px;margin-top:18px;}
.product img{width:100%;height:180px;object-fit:cover;border-radius:12px;}
.product h3{color:var(--gold);margin:8px 0 4px;font-size:18px;}
.small{color:var(--neutral);font-size:14px;}
.cart-summary{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(0,0,0,0.12));padding:12px;border-radius:12px;}
.footer{margin-top:28px;color:var(--neutral);text-align:center;font-size:13px;}

// ----------------------- src/firebaseConfig.js -----------------------
/*
  Firebase configuration (déjà fourni).
  Si tu veux activer Firebase Auth / Firestore / Storage, vérifie la console Firebase
  et configure les règles / utilisateurs.
*/
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";
import { getAuth } from "firebase/auth";
import { getStorage } from "firebase/storage";

const firebaseConfig = {
  apiKey: "AIzaSyA5nLBZ3E4t70TRj1woslLS6rIo5hrEoSY",
  authDomain: "ng-chopi-diams-824fa.firebaseapp.com",
  projectId: "ng-chopi-diams-824fa",
  storageBucket: "ng-chopi-diams-824fa.firebasestorage.app",
  messagingSenderId: "18876016875",
  appId: "1:18876016875:web:ad2a6f936aae06096ff6ae"
};

const app = initializeApp(firebaseConfig);
export const db = getFirestore(app);
export const auth = getAuth(app);
export const storage = getStorage(app);
export default app;

// ----------------------- src/i18n.js -----------------------
import i18n from "i18next";
import { initReactI18next } from "react-i18next";

const resources = {
  fr: { translation: {
    welcome: "Bienvenue sur NG_chopi_DIAMS",
    shopNow: "Acheter maintenant",
    addToCart: "Ajouter au panier",
    pay: "Payer par MoMo",
    contact: "Contacter sur WhatsApp",
    address: "Adresse de livraison",
    adminLogin: "Connexion Admin",
    cart: "Panier"
  }},
  en: { translation: {
    welcome: "Welcome to NG_chopi_DIAMS",
    shopNow: "Shop now",
    addToCart: "Add to cart",
    pay: "Pay with MoMo",
    contact: "Contact on WhatsApp",
    address: "Delivery address",
    adminLogin: "Admin Login",
    cart: "Cart"
  }},
  ln: { translation: {
    welcome: "Boyei bolamu na NG_chopi_DIAMS",
    shopNow: "Zua maki ya sikoyo",
    addToCart: "Bakisa na panier",
    pay: "Kobanda na MoMo",
    contact: "Lobela na WhatsApp",
    address: "Adresi ya kobakisa",
    adminLogin: "Kokota na admin",
    cart: "Panier"
  }}
};

i18n.use(initReactI18next).init({
  resources,
  lng: "fr",
  interpolation: { escapeValue: false }
});
export default i18n;

// ----------------------- src/App.js -----------------------
import React, { useState, useEffect } from "react";
import "./i18n";
import { useTranslation } from "react-i18next";
import Navbar from "./components/Navbar";
import Home from "./pages/Home";
import Cart from "./pages/Cart";
import Admin from "./pages/Admin";
import ChatBot from "./components/ChatBot";

function App(){
  const { t } = useTranslation();
  const [page, setPage] = useState("home");
  useEffect(()=>{document.title="NG_chopi_DIAMS"},[]);

  return (
    <div className="min-h-screen" style={{background:"var(--primary)", color:"#fff"}}>
      <Navbar setPage={setPage} />
      <main className="container">
        {page==="home" && <Home t={t} setPage={setPage} />}
        {page==="cart" && <Cart t={t} setPage={setPage} />}
        {page==="admin" && <Admin t={t} setPage={setPage} />}
      </main>
      <ChatBot />
      <footer className="footer">© NG_chopi_DIAMS - Tous droits réservés</footer>
    </div>
  );
}
export default App;

// ----------------------- src/components/Navbar.js -----------------------
import React from "react";
import { useTranslation } from "react-i18next";

export default function Navbar({ setPage }){
  const { i18n } = useTranslation();
  return (
    <nav className="nav">
      <div className="brand" onClick={()=>setPage("home")}>NG_chopi_DIAMS</div>
      <div style={{display:"flex",gap:12,alignItems:"center"}}>
        <button className="btn" onClick={()=>setPage("cart")}>Panier</button>
        <select defaultValue="fr" onChange={(e)=>i18n.changeLanguage(e.target.value)} style={{background:"transparent",color:"var(--gold)",border:"1px solid rgba(255,255,255,0.04)",padding:"6px 8px",borderRadius:8}}>
          <option value="fr">FR</option>
          <option value="en">EN</option>
          <option value="ln">LN</option>
        </select>
        <button className="btn" onClick={()=>setPage("admin")} style={{background:"transparent",border:"1px solid var(--gold)",color:"var(--gold)"}}>Admin</button>
      </div>
    </nav>
  );
}

// ----------------------- src/components/ChatBot.js -----------------------
import React, { useState } from "react";

export default function ChatBot(){
  const [open,setOpen]=useState(false);
  const [msg,setMsg]=useState("");
  return (
    <div style={{position:"fixed",right:18,bottom:18}}>
      {open && (
        <div className="card" style={{width:320}}>
          <div style={{display:"flex",justifyContent:"space-between",alignItems:"center"}}>
            <strong style={{color:"var(--gold)"}}>Support</strong>
            <button onClick={()=>setOpen(false)} style={{background:"transparent",border:"none",color:"var(--neutral)"}}>×</button>
          </div>
          <div style={{marginTop:8}} className="small">Bonjour! Comment puis-je vous aider?</div>
          <textarea value={msg} onChange={e=>setMsg(e.target.value)} placeholder="Écrire..." style={{width:"100%",marginTop:8,borderRadius:8,padding:8,background:"#0e1418",color:"#fff",border:"1px solid rgba(255,255,255,0.03)"}}/>
          <div style={{display:"flex",justifyContent:"flex-end",marginTop:8}}>
            <button className="btn" onClick={()=>{ alert("Merci — le message n'est pas réellement envoyé dans cette version demo."); setMsg(""); }}>Envoyer</button>
          </div>
        </div>
      )}
      <button className="btn" onClick={()=>setOpen(s=>!s)}>{open?"Fermer":"Support"}</button>
    </div>
  );
}

// ----------------------- src/components/ProductCard.js -----------------------
import React from "react";

export default function ProductCard({product, onAdd}){
  return (
    <div className="product card">
      <img src={product.image} alt={product.name} />
      <h3>{product.name}</h3>
      <div className="small">{product.desc}</div>
      <div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginTop:8}}>
        <strong>{product.price} FCFA</strong>
        <button className="btn" onClick={()=>onAdd(product)}>Ajouter</button>
      </div>
    </div>
  );
}

// ----------------------- src/pages/Home.js -----------------------
import React, { useState } from "react";
import ProductCard from "../components/ProductCard";

const demoProducts = [
  {id:"1",name:"Bracelet DIAMS", desc:"Bracelet artisanal élégant", price:12000, image:"https://images.unsplash.com/photo-1522312346375-d1a52e2b99b3?w=800&q=80"},
  {id:"2",name:"Collier LUX", desc:"Collier doré raffiné", price:45000, image:"https://images.unsplash.com/photo-1483985988355-763728e1935b?w=800&q=80"},
  {id:"3",name:"Boucles STAR", desc:"Boucles discrètes pour soirée", price:8000, image:"https://images.unsplash.com/photo-1541534401786-6e0f5f7c60d6?w=800&q=80"}
];

export default function Home({t, setPage}){
  const [cart, setCart] = useState([]);
  function handleAdd(p){
    setCart(prev=>[...prev,p]);
    alert(p.name + " ajouté au panier");
  }
  return (
    <div>
      <header style={{display:"flex",justifyContent:"space-between",alignItems:"center"}}>
        <div>
          <h1 style={{color:"var(--gold)"}}>{t("welcome")}</h1>
          <p className="small">Boutique officielle NG_chopi_DIAMS</p>
        </div>
        <div className="card cart-summary">
          <div>Panier: {cart.length} article(s)</div>
          <div style={{marginTop:8}}>
            <button className="btn" onClick={()=>setPage("cart")}>Voir le panier</button>
          </div>
        </div>
      </header>

      <section className="products" style={{marginTop:20}}>
        {demoProducts.map(p=> <ProductCard key={p.id} product={p} onAdd={handleAdd} />)}
      </section>
    </div>
  );
}

// ----------------------- src/pages/Cart.js -----------------------
import React, { useState } from "react";

export default function Cart({t}){
  const [items, setItems] = useState([]); // demo: non persisté
  const [address, setAddress] = useState({country:"Congo", city:"Brazzaville", quartier:"", rue:"", numero:""});

  function handlePay(){
    // ouvre WhatsApp avec message de paiement (démo)
    const phone = "+242XXXXXXXX"; // remplace par ton numéro MoMo/WhatsApp
    const msg = `Je viens de payer pour mes achats. Adresse: ${address.country}, ${address.city}, ${address.quartier}, ${address.rue}, ${address.numero}`;
    const url = `https://wa.me/${phone.replace("+","")}?text=` + encodeURIComponent(msg);
    window.open(url, "_blank");
  }

  return (
    <div>
      <h2 style={{color:"var(--gold)"}}>Panier</h2>
      <div className="card" style={{marginTop:12}}>
        <p className="small">Ici apparaîtront les articles ajoutés (demo: aucun article persistant)</p>
        <div style={{marginTop:12}}>
          <h4 className="small">Adresse de livraison</h4>
          <input placeholder="Pays" value={address.country} onChange={e=>setAddress({...address,country:e.target.value})} style={{width:"100%",padding:8,borderRadius:8,marginTop:6,background:"#0b0f12",border:"1px solid rgba(255,255,255,0.03)",color:"#fff"}}/>
          <input placeholder="Ville" value={address.city} onChange={e=>setAddress({...address,city:e.target.value})} style={{width:"100%",padding:8,borderRadius:8,marginTop:6,background:"#0b0f12",border:"1px solid rgba(255,255,255,0.03)",color:"#fff"}}/>
          <input placeholder="Quartier" value={address.quartier} onChange={e=>setAddress({...address,quartier:e.target.value})} style={{width:"100%",padding:8,borderRadius:8,marginTop:6,background:"#0b0f12",border:"1px solid rgba(255,255,255,0.03)",color:"#fff"}}/>
          <input placeholder="Rue" value={address.rue} onChange={e=>setAddress({...address,rue:e.target.value})} style={{width:"100%",padding:8,borderRadius:8,marginTop:6,background:"#0b0f12",border:"1px solid rgba(255,255,255,0.03)",color:"#fff"}}/>
          <input placeholder="Numéro" value={address.numero} onChange={e=>setAddress({...address,numero:e.target.value})} style={{width:"100%",padding:8,borderRadius:8,marginTop:6,background:"#0b0f12",border:"1px solid rgba(255,255,255,0.03)",color:"#fff"}}/>
          <div style={{marginTop:12,display:"flex",gap:8}}>
            <button className="btn" onClick={handlePay}>Payer via WhatsApp / MoMo</button>
          </div>
        </div>
      </div>
    </div>
  );
}

// ----------------------- src/pages/Admin.js -----------------------
import React, { useState } from "react";
import { signInWithEmailAndPassword } from "firebase/auth";
import { auth } from "../firebaseConfig";

export default function Admin({t}){
  const [email,setEmail]=useState("");
  const [pass,setPass]=useState("");
  const [logged, setLogged] = useState(false);

  async function handleLogin(){
    try{
      // NOTE: si Firebase Auth n'est pas configuré, ceci retournera une erreur
      await signInWithEmailAndPassword(auth, email, pass);
      setLogged(true);
      alert("Connecté en tant qu'admin (si Firebase configuré).");
    }catch(e){
      console.error(e);
      alert("Impossible de se connecter (vérifier la configuration Firebase et les identifiants).");
    }
  }

  return (
    <div style={{maxWidth:720}}>
      <h2 style={{color:"var(--gold)"}}>{t("adminLogin")}</h2>
      {!logged ? (
        <div className="card" style={{marginTop:12}}>
          <input placeholder="Email" value={email} onChange={e=>setEmail(e.target.value)} style={{width:"100%",padding:8,borderRadius:8,marginTop:6,background:"#0b0f12",border:"1px solid rgba(255,255,255,0.03)",color:"#fff"}}/>
          <input placeholder="Mot de passe" value={pass} onChange={e=>setPass(e.target.value)} type="password" style={{width:"100%",padding:8,borderRadius:8,marginTop:6,background:"#0b0f12",border:"1px solid rgba(255,255,255,0.03)",color:"#fff"}}/>
          <div style={{marginTop:12,display:"flex",gap:8}}>
            <button className="btn" onClick={handleLogin}>Se connecter</button>
          </div>
          <p className="small" style={{marginTop:12}}>Compte admin par défaut : admin@ngchopi.com / motdepasse</p>
        </div>
      ) : (
        <div className="card" style={{marginTop:12}}>
          <h3 style={{color:"var(--gold)"}}>Tableau de bord (demo)</h3>
          <p className="small">Ici tu pourras gérer produits, commandes et paramètres (implémentation à faire côté Firestore).</p>
        </div>
      )}
    </div>
  );
}
