<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Agence de Services Graphiques</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&family=Playfair+Display:wght@700&display=swap');
        body {
            font-family: 'Inter', sans-serif;
        }
        h1, h2 {
            font-family: 'Playfair Display', serif;
        }
        .aristocrate-bg {
            background-color: #0d0d0d;
        }
        .aristocrate-border {
            border-color: #ffd700;
        }
        .aristocrate-gold-text {
            color: #ffd700; /* Gold */
        }
        .aristocrate-button {
            background-color: #ffd700;
            color: #0d0d0d;
            transition: all 0.3s ease;
        }
        .aristocrate-button:hover {
            background-color: #e6b800;
            transform: translateY(-2px);
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        .chat-container {
            max-height: 500px;
            overflow-y: auto;
            scroll-behavior: smooth;
        }
        /* Custom scrollbar for elegance */
        .chat-container::-webkit-scrollbar {
            width: 8px;
        }
        .chat-container::-webkit-scrollbar-track {
            background: #1a1a1a;
            border-radius: 10px;
        }
        .chat-container::-webkit-scrollbar-thumb {
            background-color: #4a4a4a;
            border-radius: 10px;
            border: 2px solid #1a1a1a;
        }
        .error-message {
            background-color: #991b1b;
            color: white;
            font-weight: bold;
        }
    </style>
</head>
<body class="aristocrate-bg text-white flex items-center justify-center min-h-screen p-4">

    <!-- Main Container -->
    <div class="relative w-full max-w-2xl bg-black rounded-xl shadow-2xl overflow-hidden aristocrate-border border-4">
        
        <!-- Header -->
        <header class="relative p-6 border-b-2 aristocrate-border text-center">
            <!-- Bouton de retour à l'accueil -->
            <button id="back-button" class="hidden absolute top-1/2 left-4 -translate-y-1/2 p-2 rounded-full hover:bg-gray-800 transition-colors">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M3 12l2-2m0 0l7-7m-7 7v10a1 1 0 001 1h10a1 1 0 001-1v-2" />
                </svg>
            </button>
            <h1 class="text-4xl md:text-5xl font-bold aristocrate-gold-text">CSR buisiness</h1>
            <p class="text-gray-400 mt-2">Design | Communication | Numérique</p>
        </header>

        <!-- Chatbot Area -->
        <div id="chatbot-main-area" class="p-6">
            
            <!-- Welcome View (Initial state) -->
            <div id="welcome-view" class="text-center">
                <h2 class="text-3xl font-bold mb-4">Bonjour !</h2>
                <p class="text-gray-300 mb-6">Je suis votre assistant. Pour commencer, veuillez choisir l'un de nos services ci-dessous pour votre projet.</p>
                <div id="service-buttons" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <!-- Service buttons will be inserted here by JavaScript -->
                </div>
            </div>

            <!-- Chat View (after service selection) -->
            <div id="chat-view" class="hidden flex flex-col h-[500px]">
                <div id="chat-container" class="flex-grow p-4 space-y-4 chat-container rounded-lg bg-[#0e0e0e]">
                    <!-- Chat messages will be added here -->
                </div>
                <div id="input-area" class="flex p-4 border-t border-gray-800 space-x-2">
                    <input type="text" id="user-input" placeholder="Tapez votre réponse ici..." class="flex-grow rounded-lg p-3 bg-[#1e1e1e] text-white placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-yellow-400">
                    <button id="send-button" class="aristocrate-button px-6 py-3 rounded-lg font-bold">Envoyer</button>
                </div>
            </div>

            <!-- Final Summary View -->
            <div id="summary-view" class="hidden text-center p-6">
                <h2 class="text-3xl font-bold mb-4 aristocrate-gold-text">Demande Récapitulative</h2>
                <p class="text-gray-300 mb-6">Merci ! Voici le résumé de votre demande. Veuillez cliquer sur le bouton ci-dessous pour nous l'envoyer sur WhatsApp.</p>
                <div id="summary-details" class="bg-[#1e1e1e] p-6 rounded-lg text-left text-gray-200 mb-6 space-y-2">
                    <!-- Summary details will be inserted here by JavaScript -->
                </div>
                <a id="whatsapp-link" href="#" target="_blank" class="w-full inline-block aristocrate-button py-4 px-6 rounded-lg font-bold text-center">
                    Envoyer sur WhatsApp
                </a>
            </div>

        </div>
    </div>

    <script>
        // --- Configuration du Chatbot ---
        // Ce numéro WhatsApp recevra le récapitulatif de la demande.
        const WHATSAPP_NUMBER = '242067698030';
        
        // Liste de vos services et des questions initiales
        const services = {
            'logo-creation': { name: "Création de Logo", questions: [
                "Quel est le nom de l'entreprise ou de la marque ?",
                "Quel est son secteur d'activité ?",
                "Avez-vous des préférences de style (minimaliste, classique, moderne, etc.) ?",
                "Y a-t-il des couleurs spécifiques que vous souhaitez inclure ou éviter ?",
                "Y a-t-il un slogan ou un texte additionnel à ajouter ?",
                "Quel est votre public cible ?",
                "Quels sont vos principaux concurrents ?",
                "Avez-vous un budget approximatif pour ce projet ?",
                "Souhaitez-vous un logo avec un symbole ou uniquement du texte ?",
                "Dans quels contextes ce logo sera-t-il utilisé (web, impression, produits) ?",
            ]},
            'photo-editing': { name: "Montage Photo", questions: [
                "Combien de photos souhaitez-vous faire retoucher ?",
                "Quel type de retouche souhaitez-vous (ajustement de couleur, suppression d'objets, retouche de portrait, etc.) ?",
                "Avez-vous un délai spécifique pour ce projet ?",
                "Pouvez-vous nous donner un exemple de style que vous aimez ?",
                "Les photos seront-elles utilisées pour un usage personnel ou professionnel ?",
                "Quel est le format de fichier souhaité (JPEG, PNG, RAW) ?",
                "Souhaitez-vous des retouches sur des photos de produits ?",
                "Avez-vous des instructions particulières pour chaque photo ?",
                "Le projet nécessite-t-il la création d'un montage photo, comme un collage ?",
                "Avez-vous des images de référence à nous montrer ?",
            ]},
            'video-editing': { name: "Montage Vidéo", questions: [
                "Quelle est la durée approximative de la vidéo finale ?",
                "Quel est le but de la vidéo (publicité, événement personnel, réseaux sociaux) ?",
                "Disposez-vous déjà des images brutes à utiliser ? (Si non, nous pouvons vous en proposer)",
                "Souhaitez-vous de la musique de fond, des effets sonores, ou des animations ?",
                "Quelle est la résolution vidéo souhaitée (HD, 4K) ?",
                "Quel est votre public cible ?",
                "Avez-vous un script ou un storyboard ?",
                "Y a-t-il un appel à l'action que vous souhaitez inclure à la fin de la vidéo ?",
                "Souhaitez-vous des sous-titres ou du texte incrusté ?",
                "Avez-vous des exemples de vidéos que vous aimez ?",
            ]},
            'online-ad': { name: "Affiche de Vente en Ligne", questions: [
                "Quel produit ou service voulez-vous promouvoir ?",
                "Quel est le message principal que vous souhaitez communiquer (promo, nouveau produit, événement) ?",
                "Avez-vous des images ou un texte spécifique à utiliser ?",
                "Pour quelle plateforme de réseaux sociaux est destinée cette affiche ?",
                "Avez-vous des éléments de marque (logo, couleurs) à intégrer ?",
                "Quel est le format de l'affiche (story, post, bannière) ?",
                "Quel est votre budget pour la création ?",
                "Quel est le ton de la campagne (professionnel, amusant, chic) ?",
                "Souhaitez-vous plusieurs variations de l'affiche ?",
                "Y a-t-il une date limite pour la création de cette affiche ?",
            ]},
            'custom-wallpaper': { name: "Fond d'Écran Personnalisé", questions: [
                "Pour quel appareil est le fond d'écran (téléphone, ordinateur, tablette, etc.) ?",
                "Quel est le thème ou le style que vous désirez (nature, abstrait, science-fiction, etc.) ?",
                "Avez-vous des images, des citations ou des couleurs spécifiques à inclure ?",
                "Quel est le format d'image souhaité (portrait, paysage) ?",
                "Souhaitez-vous un style minimaliste ou détaillé ?",
                "Le fond d'écran est-il pour un usage personnel ou commercial ?",
                "Avez-vous une résolution d'écran spécifique ?",
                "Y a-t-il un message ou un nom à inclure ?",
                "Voulez-vous des effets de profondeur ou de 3D ?",
                "Avez-vous des images de référence à nous montrer ?",
            ]},
            'invitation-card': { name: "Carte d'Invitation", questions: [
                "Pour quel événement est la carte (anniversaire, mariage, événement d'entreprise) ?",
                "Combien d'invités sont attendus ?",
                "Quelles informations doivent être incluses (date, lieu, heure, dress code) ?",
                "Quel est le style de l'événement (formel, décontracté, festif) ?",
                "Quel est le format de la carte (numérique, physique) ?",
                "Avez-vous des photos à inclure ?",
                "Souhaitez-vous un thème de couleur particulier ?",
                "Avez-vous des préférences typographiques ?",
                "Y a-t-il des informations de contact pour les RSVP ?",
                "Avez-vous des exemples de cartes que vous aimez ?",
            ]},
            'business-publicity': { name: "Publicité pour votre Business", questions: [
                "Quel est le nom de votre entreprise ?",
                "Quel est le produit ou service à promouvoir ?",
                "Sur quelles plateformes souhaitez-vous la publicité (réseaux sociaux, affichage, impression) ?",
                "Quel est le public cible de cette publicité (âge, localisation, intérêts) ?",
                "Quel est votre message principal ou slogan ?",
                "Avez-vous des images, vidéos ou un texte à utiliser ?",
                "Quel est le budget alloué à cette campagne ?",
                "Quels sont les objectifs de cette publicité (notoriété, ventes, trafic) ?",
                "Souhaitez-vous que nous gérions la diffusion de la publicité également ?",
                "Y a-t-il des délais à respecter pour le lancement ?",
            ]},
            'business-card': { name: "Carte de Visite", questions: [
                "Quel nom et titre voulez-vous inclure ?",
                "Quelles sont les coordonnées complètes (téléphone, email, adresse, site web) ?",
                "Avez-vous un logo ou des préférences de design ?",
                "Souhaitez-vous un format spécifique (rectangulaire, carré, etc.) ?",
                "Voulez-vous des informations additionnelles au dos de la carte ?",
                "Quel est le style que vous recherchez (professionnel, créatif, élégant) ?",
                "Y a-t-il des couleurs de marque à respecter ?",
                "Souhaitez-vous une carte de visite avec un code QR ?",
                "Combien d'exemplaires souhaitez-vous ?",
                "Quel est le délai de livraison souhaité ?",
            ]},
            'custom-contract': { name: "Création de Contrat sur Mesure", questions: [
                "Quel est le but du contrat (prestation de service, vente, location) ?",
                "Quelles sont les parties impliquées ?",
                "Y a-t-il des clauses spécifiques que vous souhaitez ajouter ?",
                "Avez-vous des documents ou des informations de référence ?",
                "S'agit-il d'un contrat pour une seule transaction ou une relation à long terme ?",
                "Quelle est la juridiction applicable ?",
                "Avez-vous besoin d'une clause de confidentialité ?",
                "Y a-t-il des modalités de paiement ou des échéances à spécifier ?",
                "Le contrat doit-il être rédigé en une ou plusieurs langues ?",
                "Avez-vous besoin d'une traduction certifiée si nécessaire ?",
            ]},
        };

        // --- Éléments du DOM ---
        const welcomeView = document.getElementById('welcome-view');
        const serviceButtonsContainer = document.getElementById('service-buttons');
        const chatView = document.getElementById('chat-view');
        const chatContainer = document.getElementById('chat-container');
        const userInput = document.getElementById('user-input');
        const sendButton = document.getElementById('send-button');
        const summaryView = document.getElementById('summary-view');
        const summaryDetails = document.getElementById('summary-details');
        const whatsappLink = document.getElementById('whatsapp-link');
        const backButton = document.getElementById('back-button');

        // --- État du Chatbot ---
        let chatState = {
            currentService: null,
            currentQuestionIndex: -1,
            answers: {},
            chatHistory: [],
            isAwaitingResponse: false,
        };

        // --- Fonctions d'affichage ---

        function renderServiceButtons() {
            serviceButtonsContainer.innerHTML = '';
            for (const key in services) {
                const service = services[key];
                const button = document.createElement('button');
                button.className = 'aristocrate-button px-6 py-4 rounded-lg font-bold text-lg';
                button.textContent = service.name;
                button.dataset.serviceKey = key;
                button.addEventListener('click', () => startChat(key));
                serviceButtonsContainer.appendChild(button);
            }
        }

        function addMessage(message, sender = 'bot') {
            const messageElement = document.createElement('div');
            messageElement.className = `p-4 rounded-lg shadow-sm max-w-[85%] ${sender === 'bot' ? 'bg-gray-800 self-start text-gray-200' : 'bg-yellow-400 text-black self-end'}`;
            messageElement.textContent = message;
            chatContainer.appendChild(messageElement);
            chatContainer.scrollTop = chatContainer.scrollHeight;
        }

        function showLoading(show = true) {
            const loadingMessageId = 'loading-message';
            let loadingElement = document.getElementById(loadingMessageId);
            
            if (show && !loadingElement) {
                loadingElement = document.createElement('div');
                loadingElement.id = loadingMessageId;
                loadingElement.className = 'p-4 rounded-lg shadow-sm max-w-[85%] bg-gray-800 self-start text-gray-400 animate-pulse';
                loadingElement.textContent = "L'assistant réfléchit...";
                chatContainer.appendChild(loadingElement);
                chatContainer.scrollTop = chatContainer.scrollHeight;
            } else if (!show && loadingElement) {
                loadingElement.remove();
            }
        }
        
        function showErrorMessage(message) {
            const messageElement = document.createElement('div');
            messageElement.className = 'p-4 rounded-lg shadow-sm max-w-[85%] self-start error-message';
            messageElement.innerHTML = `<strong>Erreur:</strong> ${message}`;
            chatContainer.appendChild(messageElement);
            chatContainer.scrollTop = chatContainer.scrollHeight;
        }

        // --- Logique du Chatbot ---
        function resetChat() {
            // Réinitialise l'état du chat
            chatState = {
                currentService: null,
                currentQuestionIndex: -1,
                answers: {},
                chatHistory: [],
                isAwaitingResponse: false,
            };

            // Masque toutes les vues et n'affiche que la page d'accueil
            welcomeView.classList.remove('hidden');
            chatView.classList.add('hidden');
            summaryView.classList.add('hidden');

            // Masque le bouton de retour
            backButton.classList.add('hidden');

            // Vide le conteneur du chat
            chatContainer.innerHTML = '';

            // S'assure que les boutons des services sont bien affichés
            renderServiceButtons();
        }

        function startChat(serviceKey) {
            resetChat(); // S'assure de réinitialiser avant de commencer un nouveau chat
            
            chatState.currentService = serviceKey;
            chatState.currentQuestionIndex = 0;
            
            welcomeView.classList.add('hidden');
            chatView.classList.remove('hidden');
            backButton.classList.remove('hidden');

            const serviceName = services[serviceKey].name;
            const firstQuestion = services[serviceKey].questions[0];

            // Initialiser l'historique du chat pour l'IA
            const initialPrompt = `Bonjour, je suis votre assistant virtuel pour le service "${serviceName}". Je vais vous poser quelques questions pour mieux comprendre votre projet. À la fin, je vous préparerai un récapitulatif à envoyer à un de nos experts. Veuillez répondre brièvement. Première question : ${firstQuestion}`;
            chatState.chatHistory.push({ role: "user", parts: [{ text: initialPrompt }] });

            addMessage(`Excellent choix ! Vous avez sélectionné le service : ${serviceName}.`, 'bot');
            addMessage(`Bonjour, je suis votre assistant virtuel pour le service "${serviceName}". Nous allons discuter de votre projet. Pour commencer : ${firstQuestion}`, 'bot');
            userInput.focus();
        }

        async function getLLMResponse(prompt) {
            if (chatState.isAwaitingResponse) return;
            chatState.isAwaitingResponse = true;
            showLoading(true);

            // Add user prompt to history
            chatState.chatHistory.push({ role: "user", parts: [{ text: prompt }] });

            const payload = {
                contents: chatState.chatHistory
            };
            // --- REMPLACEZ CE TEXTE PAR VOTRE CLÉ API ---
            // Vous devez obtenir cette clé sur Google AI Studio.
            const apiKey = "AIzaSyCjwSIiLt6suaQGnEQqurTJFNGahCIyZhE"; 

            if (!apiKey) {
                showErrorMessage("La clé API est manquante. Veuillez l'insérer dans le code.");
                showLoading(false);
                chatState.isAwaitingResponse = false;
                return "Désolé, je ne peux pas me connecter sans la clé API.";
            }

            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

            try {
                let response = await fetchWithExponentialBackoff(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                if (!response.ok) {
                    const errorText = await response.text();
                    console.error("Erreur de l'API:", response.status, response.statusText, errorText);
                    showErrorMessage(`Connexion à l'API échouée. Erreur ${response.status}. Veuillez vérifier la console pour plus de détails.`);
                    return null; // Return null to indicate a failure
                }
                
                const result = await response.json();
                
                let botResponse = 'Désolé, une erreur est survenue. Pourriez-vous reformuler ?';
                if (result.candidates && result.candidates.length > 0 && result.candidates[0].content && result.candidates[0].content.parts && result.candidates[0].content.parts.length > 0) {
                    botResponse = result.candidates[0].content.parts[0].text;
                }
                
                // Add the bot's response to the chat history
                chatState.chatHistory.push({ role: "model", parts: [{ text: botResponse }] });

                return botResponse;

            } catch (error) {
                console.error("Erreur de l'appel API:", error);
                showErrorMessage(`Une erreur est survenue. Veuillez vérifier la console pour plus de détails.`);
                return null; // Return null to indicate a failure
            } finally {
                showLoading(false);
                chatState.isAwaitingResponse = false;
            }
        }

        async function fetchWithExponentialBackoff(url, options, maxRetries = 5, delay = 1000) {
            for (let i = 0; i < maxRetries; i++) {
                try {
                    const response = await fetch(url, options);
                    if (response.status !== 429) {
                        return response;
                    }
                    console.warn(`API rate limit exceeded. Retrying in ${delay / 1000}s...`);
                    await new Promise(resolve => setTimeout(resolve, delay));
                    delay *= 2; // Exponential backoff
                } catch (error) {
                    console.error("Fetch failed:", error);
                    throw error;
                }
            }
            throw new Error(`Failed to fetch after ${maxRetries} retries.`);
        }

        async function handleUserResponse() {
            const response = userInput.value.trim();
            if (response === '' || chatState.isAwaitingResponse) return;

            addMessage(response, 'user');
            
            // Stocker la réponse dans l'état du chatbot
            const questionsList = services[chatState.currentService].questions;
            const questionKey = questionsList[chatState.currentQuestionIndex] || `Réponse libre ${chatState.currentQuestionIndex}`;
            chatState.answers[questionKey] = response;
            
            userInput.value = '';

            const totalQuestionsAnswered = Object.keys(chatState.answers).length;
            const isCheckPoint = (totalQuestionsAnswered === 8) || (totalQuestionsAnswered > 8 && (totalQuestionsAnswered - 8) % 4 === 0);

            let botPrompt;
            if (response.toLowerCase().includes('terminer') || response.toLowerCase().includes('stop')) {
                generateSummary();
                return;
            } else if (isCheckPoint) {
                botPrompt = `L'utilisateur a répondu à la question "${questionKey}" par "${response}". C'est le moment de lui demander s'il a assez d'informations et s'il souhaite finaliser la demande. Si oui, dites-lui que vous allez préparer le récapitulatif. Si non, continuez à poser des questions pour affiner la demande.`;
            } else if (chatState.currentQuestionIndex < questionsList.length - 1) {
                chatState.currentQuestionIndex++;
                botPrompt = `L'utilisateur a répondu à la question "${questionKey}" par "${response}". Posez la question suivante de manière concise : "${questionsList[chatState.currentQuestionIndex]}"`;
            } else {
                botPrompt = `L'utilisateur a répondu à toutes les questions de la liste. Sa dernière réponse est : "${response}". Posez une question libre pour obtenir plus de détails, ou proposez de finaliser le récapitulatif.`;
            }

            const botResponse = await getLLMResponse(botPrompt);
            // Only add the message if the API call was successful
            if (botResponse) {
                addMessage(botResponse, 'bot');
                // If the bot has finished the conversation, display the summary
                if (botResponse.toLowerCase().includes('prêt à être envoyé') || botResponse.toLowerCase().includes('récapitulatif') || botResponse.toLowerCase().includes('terminons')) {
                    generateSummary();
                }
            }
        }


        function generateSummary() {
            chatView.classList.add('hidden');
            summaryView.classList.remove('hidden');
            
            const serviceName = services[chatState.currentService].name;
            let summaryHTML = `<p><strong>Service demandé :</strong> ${serviceName}</p><hr class="my-2 border-gray-700">`;
            let whatsappMessage = `*Nouvelle demande de service : ${serviceName}* ✨\n\n`;

            for (const question in chatState.answers) {
                const answer = chatState.answers[question];
                summaryHTML += `<p><strong>${question}</strong><br>${answer}</p>`;
                whatsappMessage += `*${question}*\n${answer}\n\n`;
            }

            summaryHTML += `<p class="mt-4">Un de nos experts vous répondra dans les plus brefs délais.</p>`;
            summaryDetails.innerHTML = summaryHTML;
            
            const encodedMessage = encodeURIComponent(whatsappMessage);
            const whatsappUrl = `https://wa.me/${WHATSAPP_NUMBER}?text=${encodedMessage}`;
            whatsappLink.href = whatsappUrl;
            
            backButton.classList.remove('hidden');
        }

        // --- Écouteurs d'événements ---
        sendButton.addEventListener('click', handleUserResponse);
        userInput.addEventListener('keydown', (e) => {
            if (e.key === 'Enter') {
                handleUserResponse();
            }
        });

        backButton.addEventListener('click', resetChat);

        // Initialisation de la page
        document.addEventListener('DOMContentLoaded', () => {
            renderServiceButtons();
        });
    </script>
</body>
</html>
