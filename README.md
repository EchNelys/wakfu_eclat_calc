<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculateur d'Éclats Wakfu</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        .header {
            text-align: center;
            color: white;
            margin-bottom: 30px;
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .calculator-card {
            background: white;
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
            margin-bottom: 20px;
        }

        .form-section {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .form-group {
            display: flex;
            flex-direction: column;
        }

        label {
            font-weight: 600;
            color: #333;
            margin-bottom: 8px;
            font-size: 0.95em;
        }

        input, select {
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            font-size: 1em;
            transition: all 0.3s;
        }

        input:focus, select:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }

        select option {
            padding: 10px;
        }

        select option[value="0.25"] { color: #27ae60; font-weight: 600; }
        select option[value="1"] { color: #d68910; font-weight: 600; }
        select option[value="4"] { color: #f39c12; font-weight: 600; }
        select option.epique { color: #e91e63; font-weight: 600; }
        select option.relique { color: #7b2cbf; font-weight: 600; }

        .checkbox-group {
            display: flex;
            align-items: center;
            margin-top: 25px;
        }

        .checkbox-group input[type="checkbox"] {
            width: 20px;
            height: 20px;
            margin-right: 10px;
            margin-bottom: 15px;
            cursor: pointer;
        }

        .checkbox-group label {
            margin: 0;
            margin-bottom: 15px;
            cursor: pointer;
        }

        .btn-calculate {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 15px 40px;
            font-size: 1.1em;
            font-weight: 600;
            border-radius: 8px;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
            width: 100%;
        }

        .btn-calculate:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);
        }

        .results {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            border-radius: 12px;
            padding: 25px;
            margin-top: 20px;
            color: white;
        }

        .result-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px;
            background: rgba(255,255,255,0.2);
            border-radius: 8px;
            margin-bottom: 10px;
            backdrop-filter: blur(10px);
        }

        .result-item:last-child {
            margin-bottom: 0;
        }

        .result-label {
            font-weight: 600;
            font-size: 1.1em;
        }

        .result-value {
            font-size: 1.3em;
            font-weight: 700;
        }

        .history-section {
            background: white;
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.3);
        }

        .history-title {
            font-size: 1.5em;
            color: #333;
            margin-bottom: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .btn-clear {
            background: #f5576c;
            color: white;
            border: none;
            padding: 8px 20px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 0.9em;
        }

        .btn-clear:hover {
            background: #d64555;
        }

        table {
            width: 100%;
            border-collapse: collapse;
        }

        th, td {
            padding: 12px;
            text-align: left;
            border-bottom: 1px solid #e0e0e0;
        }

        th {
            background: #f5f5f5;
            font-weight: 600;
            color: #333;
        }

        tr:hover {
            background: #f9f9f9;
        }

        .rarity-rare { color: #27ae60; font-weight: 600; }
        .rarity-mythique { color: #d68910; font-weight: 600; }
        .rarity-legendaire { color: #f39c12; font-weight: 600; }
        .rarity-epique { color: #e91e63; font-weight: 600; }
        .rarity-relique { color: #7b2cbf; font-weight: 600; }

        @media (max-width: 768px) {
            .header h1 {
                font-size: 1.8em;
            }
            
            .form-section {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>⚡ Calculateur d'Éclats Wakfu</h1>
            <p>Calculez facilement la rentabilité de vos objets</p>
        </div>

        <div class="calculator-card">
            <form id="calcForm">
                <div class="form-section">
                    <div class="form-group">
                        <label for="itemName">Nom de l'objet (optionnel)</label>
                        <input type="text" id="itemName" placeholder="Ex: Chapeau Meulette">
                    </div>

                    <div class="form-group">
                        <label for="niveau">Niveau (1-245)</label>
                        <input type="number" id="niveau" min="1" max="245" value="200" required>
                    </div>

                    <div class="form-group">
                        <label for="rarete">Rareté</label>
                        <select id="rarete" required>
                            <option value="0.25">Rare (x1/4)</option>
                            <option value="1">Mythique (x1)</option>
                            <option value="4">Légendaire (x4)</option>
                            <option value="32" class="epique">Épique (x32)</option>
                            <option value="32" class="relique">Relique (x32)</option>
                        </select>
                    </div>

                    <div class="form-group">
                        <label for="valeurEclat">Valeur de l'Éclat (optionnel)</label>
                        <input type="number" id="valeurEclat" min="0" step="0.01" placeholder="Ex: 25">
                    </div>
                </div>

                <div class="checkbox-group">
                    <input type="checkbox" id="identifie">
                    <label for="identifie">Objet identifié (Non id = bonus x1.2)</label>
                </div>

                <button type="submit" class="btn-calculate">Calculer les Éclats</button>
            </form>

            <div id="results" style="display: none;" class="results">
                <div class="result-item">
                    <span class="result-label">💎 Éclats obtenus:</span>
                    <span class="result-value" id="eclatsValue">0</span>
                </div>
                <div class="result-item" id="kamasResult" style="display: none;">
                    <span class="result-label">💰 Valeur totale:</span>
                    <span class="result-value" id="kamasValue">0</span>
                </div>
                <div class="result-item" id="ratioResult" style="display: none;">
                    <span class="result-label">📊 Prix par Éclat:</span>
                    <span class="result-value" id="ratioValue">0</span>
                </div>
            </div>
        </div>

        <div class="history-section">
            <div class="history-title">
                <span>📜 Historique des calculs</span>
                <button class="btn-clear" onclick="clearHistory()">Effacer</button>
            </div>
            <table id="historyTable">
                <thead>
                    <tr>
                        <th>Objet</th>
                        <th>Niveau</th>
                        <th>Rareté</th>
                        <th>ID</th>
                        <th>Éclats</th>
                        <th>Valeur</th>
                        <th>Prix/éclat</th>
                    </tr>
                </thead>
                <tbody id="historyBody">
                </tbody>
            </table>
        </div>
    </div>

    <div class="header">        
        <p>Version biche.0</p>
    </div>

        <script>
            const history = [];



            function calculateEclats(niveau, rarete, identifie) {
                const N = niveau;
                const R = rarete;
                const I = identifie ? 1 : 1.2;

                const eclats = (4 ** ((N - 50) / 50)) * R * I;
                return eclats;
            }

            function getRareteText(value) {
                const raretes = {
                    '0.25': 'Rare',
                    '1': 'Mythique',
                    '4': 'Légendaire',
                    '32-epique': 'Épique',
                    '32-relique': 'Relique'
                };
                return raretes[value] || '';
            }

            function getRareteClass(value) {
                const classes = {
                    '0.25': 'rarity-rare',
                    '1': 'rarity-mythique',
                    '4': 'rarity-legendaire',
                    '32-epique': 'rarity-epique',
                    '32-relique': 'rarity-relique'
                };
                return classes[value] || '';
            }

            document.getElementById('calcForm').addEventListener('submit', function (e) {
                e.preventDefault();

                const itemName = document.getElementById('itemName').value || 'Sans nom';
                const niveau = parseInt(document.getElementById('niveau').value);
                const rareteSelect = document.getElementById('rarete');
                const rareteValue = parseFloat(rareteSelect.value);
                const rareteText = rareteSelect.options[rareteSelect.selectedIndex].text;
                let rareteKey = rareteValue.toString();

                // Distinguer Épique et Relique
                if (rareteValue === 32) {
                    if (rareteText.includes('Épique')) {
                        rareteKey = '32-epique';
                    } else if (rareteText.includes('Relique')) {
                        rareteKey = '32-relique';
                    }
                }

                const identifie = document.getElementById('identifie').checked;
                const valeurEclatInput = document.getElementById('valeurEclat').value;
                const valeurEclat = valeurEclatInput ? parseFloat(valeurEclatInput) : 0;

                const eclats = calculateEclats(niveau, rareteValue, identifie);

                document.getElementById('eclatsValue').textContent = eclats.toLocaleString('fr-FR', { minimumFractionDigits: 2, maximumFractionDigits: 2 });

                if (valeurEclat > 0) {
                    const valeurTotale = eclats * valeurEclat;
                    document.getElementById('kamasValue').textContent = valeurTotale.toLocaleString('fr-FR', { minimumFractionDigits: 2, maximumFractionDigits: 2 }) + ' K';
                    document.getElementById('kamasResult').style.display = 'flex';

                    const prixParEclat = valeurEclat.toFixed(2);
                    document.getElementById('ratioValue').textContent = prixParEclat + ' K/éclat';
                    document.getElementById('ratioResult').style.display = 'flex';
                } else {
                    document.getElementById('kamasResult').style.display = 'none';
                    document.getElementById('ratioResult').style.display = 'none';
                }

                document.getElementById('results').style.display = 'block';

                // Ajouter à l'historique
                history.unshift({
                    nom: itemName,
                    niveau: niveau,
                    rarete: rareteKey,
                    identifie: identifie,
                    eclats: eclats,
                    kamas: valeurEclat > 0 ? (eclats * valeurEclat) : 0,
                    ratio: valeurEclat > 0 ? valeurEclat.toFixed(2) : '-'
                });

                updateHistoryTable();
            });

            function updateHistoryTable() {
                const tbody = document.getElementById('historyBody');
                tbody.innerHTML = '';

                history.forEach(item => {
                    const row = tbody.insertRow();
                    row.innerHTML = `
                                    <td>${item.nom}</td>
                                    <td>${item.niveau}</td>
                                    <td class="${getRareteClass(item.rarete)}">${getRareteText(item.rarete)}</td>
                                    <td>${item.identifie ? '✓' : '✗'}</td>
                                    <td><strong>${item.eclats.toLocaleString('fr-FR', { minimumFractionDigits: 2, maximumFractionDigits: 2 })}</strong></td>
                                    <td>${item.kamas > 0 ? item.kamas.toLocaleString('fr-FR', { minimumFractionDigits: 2, maximumFractionDigits: 2 }) + ' K' : '-'}</td>
                                    <td>${item.ratio !== '-' ? item.ratio + ' K' : '-'}</td>
                                `;
                });
            }

            function clearHistory() {
                if (confirm('Voulez-vous vraiment effacer l\'historique ?')) {
                    history.length = 0;
                    updateHistoryTable();
                }
            }
        </script>
</body>
</html>
