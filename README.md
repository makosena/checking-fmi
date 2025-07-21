

<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vérification d'Activation FMI - MAKO SENA</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
        }
        .container {
            background-color: #ffffff;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 500px;
            text-align: center;
        }
        h1 {
            color: #333;
            margin-bottom: 20px;
        }
        .form-group {
            margin-bottom: 20px;
            text-align: left;
        }
        label {
            display: block;
            margin-bottom: 8px;
            color: #555;
            font-weight: bold;
        }
        input[type="text"] {
            width: calc(100% - 20px);
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 16px;
        }
        button {
            background-color: #007bff;
            color: white;
            padding: 12px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 18px;
            width: 100%;
            transition: background-color 0.3s ease;
        }
        button:hover {
            background-color: #0056b3;
        }
        #result {
            margin-top: 25px;
            padding: 15px;
            border-radius: 5px;
            font-size: 1.1em;
            text-align: left;
            word-wrap: break-word; /* To ensure long results wrap */
        }
        .result-success {
            background-color: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }
        .result-danger {
            background-color: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }
        .result-info {
            background-color: #d1ecf1;
            color: #0c5460;
            border: 1px solid #bee5eb;
        }
        footer {
            margin-top: 30px;
            color: #777;
            font-size: 0.9em;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Vérification d'Activation FMI</h1>
        <form id="fmiCheckForm">
            <div class="form-group">
                <label for="imeiSerial">Entrez l'IMEI ou le Numéro de Série :</label>
                <input type="text" id="imeiSerial" name="imeiSerial" placeholder="Ex: 35XXXXXXXXXXXXX ou C8xxxxxxxxX" required>
            </div>
            <button type="submit">Vérifier l'Activation FMI</button>
        </form>
        <div id="result">
            </div>
        <footer>
            <p>&copy; 2025 **MAKO SENA**. Tous droits réservés.</p>
        </footer>
    </div>

    <script>
        document.getElementById('fmiCheckForm').addEventListener('submit', async function(event) {
            event.preventDefault(); // Empêche le rechargement de la page
            const imeiSerial = document.getElementById('imeiSerial').value.trim();
            const resultDiv = document.getElementById('result');

            // Réinitialiser le style et le contenu du résultat
            resultDiv.className = '';
            resultDiv.innerHTML = '<p class="result-info">Vérification en cours...</p>';

            // ATTENTION : Cette requête directe va très probablement ÉCHOUER en raison de la politique CORS (Cross-Origin Resource Sharing) des navigateurs.
            // Les navigateurs empêchent le code JavaScript d'une page (votre HTML local) de faire des requêtes directes à un autre domaine (iunlocker.com)
            // à moins que le serveur cible n'ait explicitement autorisé ces requêtes.
            // Pour une intégration "réelle", vous auriez besoin d'un serveur intermédiaire (backend) qui ferait la requête à iunlocker.com
            // et renverrait le résultat à votre page HTML.

            try {
                // Construction de l'URL avec le paramètre 'imei' ou 'serial'.
                // L'API d'iunlocker.com n'est pas publique et ce n'est probablement pas le bon format de requête.
                // Ce lien est celui d'une page web interactive, pas un endpoint d'API.
                // Il se peut que le paramètre s'appelle 'imei', 'serial', ou autre chose, ou qu'il faille un POST request.
                const url = `https://iunlocker.com/fr/check_icloud.php?imei=${imeiSerial}`; // Exemple : Supposons qu'il utilise le paramètre 'imei'

                const response = await fetch(url, {
                    method: 'GET', // ou 'POST' si l'API le requiert
                    // mode: 'no-cors', // N'utilisez pas ceci en production, car cela rendrait la réponse opaque (impossible à lire)
                    // headers: {
                    //     'Content-Type': 'application/json', // ou 'application/x-www-form-urlencoded'
                    //     // Ajoutez ici d'autres en-têtes si l'API iunlocker.com en requiert (ex: une clé API)
                    // }
                });

                if (!response.ok) {
                    throw new Error(`Erreur HTTP: ${response.status} - Probablement un problème de CORS ou le serveur a refusé la requête.`);
                }

                // Pour iunlocker.com, la réponse ne sera probablement pas du JSON direct ou du texte structuré,
                // car c'est une page web complète. Vous devriez analyser le HTML si la requête réussit,
                // ce qui est complexe et non recommandé depuis le client pour des sites tiers.
                const data = await response.text(); // Lire la réponse en tant que texte (HTML)

                // Ici, vous devriez analyser 'data' (qui serait le HTML de la page iunlocker.com)
                // pour extraire le statut FMI. C'est très complexe et fragile.
                // Par exemple, rechercher des mots clés comme "Activé", "Désactivé", "Clean", "Locked".
                let checkStatus = "unknown";
                let resultMessage = "Impossible d'analyser le résultat de iunlocker.com directement depuis le navigateur.";

                if (data.includes("Activation Lock: ON")) { // Ceci est un exemple, le texte exact peut varier
                    checkStatus = "locked";
                    resultMessage = `<b>Statut FMI (via iunlocker.com) : Verrouillé</b><br>L'appareil est associé à un compte Apple ID.`;
                } else if (data.includes("Activation Lock: OFF")) { // Ceci est un exemple
                    checkStatus = "clean";
                    resultMessage = `<b>Statut FMI (via iunlocker.com) : Désactivé (Propre)</b><br>L'appareil n'est pas associé à un compte Apple ID.`;
                } else {
                    resultMessage = `<b>Statut FMI (via iunlocker.com) : Indéterminé</b><br>Veuillez consulter la page iunlocker.com manuellement pour l'IMEI/Numéro de Série : <a href="${url}" target="_blank">${imeiSerial}</a><br>Réponse brute (limitée) : ${data.substring(0, 200)}...`; // Afficher une partie de la réponse
                }


                switch (checkStatus) {
                    case "locked":
                        resultDiv.classList.add('result-danger');
                        break;
                    case "clean":
                        resultDiv.classList.add('result-success');
                        break;
                    default:
                        resultDiv.classList.add('result-info');
                        break;
                }
                resultDiv.innerHTML = `<p>${resultMessage}</p>`;

            } catch (error) {
                console.error('Erreur lors de la vérification FMI:', error);
                resultDiv.classList.add('result-danger');
                resultDiv.innerHTML = `
                    <p><b>Erreur de Vérification :</b> Impossible d'accéder au service de vérification.</p>
                    <p>Ceci est très probablement dû à la <b>politique de sécurité CORS</b> de votre navigateur, qui bloque les requêtes directes vers des sites externes.</p>
                    <p>Pour une intégration réelle, un <b>serveur intermédiaire (backend)</b> est nécessaire.</p>
                    <p>Détail de l'erreur : ${error.message}</p>
                    <p>Veuillez essayer la vérification manuellement sur <a href="https://iunlocker.com/fr/check_icloud.php" target="_blank">iunlocker.com</a>.</p>
                `;

                // --- Simulation de secours (décommentez pour utiliser la simulation si la requête réelle échoue) ---
                /*
                setTimeout(() => {
                    let checkStatusSim = "";
                    let resultMessageSim = "";

                    if (imeiSerial.startsWith('123')) {
                        checkStatusSim = "locked";
                    } else if (imeiSerial.startsWith('456')) {
                        checkStatusSim = "clean";
                    } else if (imeiSerial.startsWith('789')) {
                        checkStatusSim = "unknown";
                    } else {
                        checkStatusSim = "invalid";
                    }

                    switch (checkStatusSim) {
                        case "locked":
                            resultDiv.classList.add('result-danger');
                            resultMessageSim = "<b>[Simulation] Statut FMI : Verrouillé</b><br>L'appareil est associé à un compte Apple ID. L'activation FMI est active.";
                            break;
                        case "clean":
                            resultDiv.classList.add('result-success');
                            resultMessageSim = "<b>[Simulation] Statut FMI : Désactivé (Propre)</b><br>L'appareil n'est pas associé à un compte Apple ID. L'activation FMI est inactive.";
                            break;
                        case "unknown":
                            resultDiv.classList.add('result-info');
                            resultMessageSim = "<b>[Simulation] Statut FMI : Inconnu</b><br>Impossible de déterminer le statut d'activation FMI pour le moment. Veuillez réessayer.";
                            break;
                        case "invalid":
                        default:
                            resultDiv.classList.add('result-info');
                            resultMessageSim = "<b>[Simulation] Erreur :</b> IMEI/Numéro de Série invalide ou non trouvé. Veuillez vérifier et réessayer.";
                            break;
                    }
                    resultDiv.innerHTML = `<p>${resultMessageSim}</p>`;
                }, 2000); // Simule un délai de 2 secondes pour la vérification
                */
                // ------------------------------------------------------------------------------------------------
            }
        });
    </script>
</body>
</html>
