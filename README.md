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
