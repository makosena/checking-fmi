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
        document.getElementById('fmiCheckForm').addEventListener('submit', function(event) {
            event.preventDefault(); // Empêche le rechargement de la page
            const imeiSerial = document.getElementById('imeiSerial').value.trim();
            const resultDiv = document.getElementById('result');

            // Réinitialiser le style et le contenu du résultat
            resultDiv.className = '';
            resultDiv.innerHTML = '<p class="result-info">Vérification en cours...</p>';

            // Ici, vous intégreriez la logique de vérification réelle.
            // Pour cet exemple, nous allons simuler une réponse.
            setTimeout(() => {
                let checkStatus = "";
                let resultMessage = "";

                // Simulation basée sur une partie de l'IMEI/Numéro de Série
                if (imeiSerial.startsWith('123')) {
                    checkStatus = "locked";
                } else if (imeiSerial.startsWith('456')) {
                    checkStatus = "clean";
                } else if (imeiSerial.startsWith('789')) {
                    checkStatus = "unknown";
                } else {
                    checkStatus = "invalid";
                }

                switch (checkStatus) {
                    case "locked":
                        resultDiv.classList.add('result-danger');
                        resultMessage = "<b>Statut FMI : Verrouillé</b><br>L'appareil est associé à un compte Apple ID. L'activation FMI est active.";
                        break;
                    case "clean":
                        resultDiv.classList.add('result-success');
                        resultMessage = "<b>Statut FMI : Désactivé (Propre)</b><br>L'appareil n'est pas associé à un compte Apple ID. L'activation FMI est inactive.";
                        break;
                    case "unknown":
                        resultDiv.classList.add('result-info');
                        resultMessage = "<b>Statut FMI : Inconnu</b><br>Impossible de déterminer le statut d'activation FMI pour le moment. Veuillez réessayer.";
                        break;
                    case "invalid":
                    default:
                        resultDiv.classList.add('result-info');
                        resultMessage = "<b>Erreur :</b> IMEI/Numéro de Série invalide ou non trouvé. Veuillez vérifier et réessayer.";
                        break;
                }
                resultDiv.innerHTML = `<p>${resultMessage}</p>`;
            }, 2000); // Simule un délai de 2 secondes pour la vérification
        });
    </script>
</body>
</html>
