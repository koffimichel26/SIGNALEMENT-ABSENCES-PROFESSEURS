# SIGNALEMENT-ABSENCES-PROFESSEURS
LYCÉE MODERNE DE TREICHVILLE 

<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tableau de Bord Absences Professeurs</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.28/jspdf.plugin.autotable.min.js"></script>
    <style>
        body { font-family: Arial, sans-serif; background-color: #f5f5f5; margin: 0; padding: 0; }
        h1 { text-align: center; color: #333; margin: 20px 0; }
        .container { max-width: 600px; margin: auto; padding: 15px; background: #fff; border-radius: 10px; }
        label { display: block; margin-top: 15px; font-weight: bold; }
        input, select, button { width: 100%; padding: 12px; margin-top: 5px; border-radius: 6px; border: 1px solid #ccc; font-size: 16px; }
        button { background-color: #FF9800; color: white; border: none; cursor: pointer; font-weight: bold; margin-top: 10px; }
        button:hover { background-color: #F57C00; }
        .btn-reset { background-color: #4CAF50; }
        .btn-reset:hover { background-color: #45A049; }
        table { width: 100%; border-collapse: collapse; margin-top: 15px; }
        th, td { padding: 10px; text-align: center; border: 1px solid #ccc; font-size: 14px; }
        th { background-color: #FF9800; color: white; }
        tr:nth-child(even) { background-color: #E8F5E9; }
        tr:nth-child(odd) { background-color: #FFFFFF; }
    </style>
</head>
<body>
    <h1>Tableau de Bord Absences Professeurs</h1>
    <div class="container">
        <label for="date">Date :</label>
        <input type="date" id="date" required>

        <label for="classe">Classe :</label>
        <input type="text" id="classe" placeholder="Ex: 6ème1, Tle D1" required>

        <label for="matiere">Matière :</label>
        <select id="matiere" required>
            <option value="Français">Français</option>
            <option value="Anglais">Anglais</option>
            <option value="Espagnol">Espagnol</option>
            <option value="Allemand">Allemand</option>
            <option value="Histoire/Géographie">Histoire/Géographie</option>
            <option value="Philosophie">Philosophie</option>
            <option value="Mathématiques">Mathématiques</option>
            <option value="Physique/Chimie">Physique/Chimie</option>
            <option value="SVT">SVT</option>
            <option value="Arts Plastiques">Arts Plastiques</option>
            <option value="Musique">Musique</option>
            <option value="EPS">EPS</option>
            <option value="EDHC">EDHC</option>
            <option value="TIC">TIC</option>
        </select>

        <label for="heure">Heure :</label>
        <select id="heure" required>
            <option value="1ère heure (Matin)">1ère heure (Matin)</option>
            <option value="2ème heure (Matin)">2ème heure (Matin)</option>
            <option value="3ème heure (Matin)">3ème heure (Matin)</option>
            <option value="4ème heure (Matin)">4ème heure (Matin)</option>
            <option value="5ème heure (Matin)">5ème heure (Matin)</option>
            <option value="1ère heure (Après-midi)">1ère heure (Après-midi)</option>
            <option value="2ème heure (Après-midi)">2ème heure (Après-midi)</option>
            <option value="3ème heure (Après-midi)">3ème heure (Après-midi)</option>
            <option value="4ème heure (Après-midi)">4ème heure (Après-midi)</option>
        </select>

        <button type="button" onclick="ajouterAbsence()">Ajouter l'absence</button>
        <button type="button" onclick="genererPDF()">Générer PDF</button>
        <button type="button" class="btn-reset" onclick="reinitialiser()">Réinitialiser</button>

        <table id="tableAbsences">
            <thead>
                <tr>
                    <th>Classe</th>
                    <th>Matière</th>
                    <th>Heure</th>
                    <th>Date</th>
                </tr>
            </thead>
            <tbody>
                <!-- Les absences apparaîtront ici -->
            </tbody>
        </table>
    </div>

    <script>
        const absences = [];

        function ajouterAbsence() {
            const date = document.getElementById("date").value;
            const classe = document.getElementById("classe").value.trim();
            const matiere = document.getElementById("matiere").value;
            const heure = document.getElementById("heure").value;

            if(!date || !classe) {
                alert("Veuillez remplir tous les champs !");
                return;
            }

            absences.push({date, classe, matiere, heure});
            miseAJourTableau();
        }

        function miseAJourTableau() {
            const tbody = document.getElementById("tableAbsences").querySelector("tbody");
            tbody.innerHTML = "";
            absences.forEach(a => {
                const tr = document.createElement("tr");
                tr.innerHTML = `<td>${a.classe}</td><td>${a.matiere}</td><td>${a.heure}</td><td>${a.date}</td>`;
                tbody.appendChild(tr);
            });
        }

        function reinitialiser() {
            if(confirm("Voulez-vous vraiment réinitialiser toutes les absences ?")) {
                absences.length = 0; // vide le tableau
                miseAJourTableau();
            }
        }

        async function genererPDF() {
            if(absences.length === 0){
                alert("Veuillez ajouter au moins une absence !");
                return;
            }

            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();
            const pageWidth = doc.internal.pageSize.getWidth();

            // Entête encadrée
            doc.setLineWidth(0.6);
            doc.rect(20, 10, pageWidth-40, 20);
            doc.setFontSize(14);
            doc.text("LYCÉE MODERNE DE TREICHVILLE", pageWidth/2, 23, { align: "center" });
            doc.setFontSize(11);
            doc.text("Année scolaire 2025-2026", pageWidth/2, 30, { align: "center" });

            // Titre absences
            doc.setFontSize(12);
            doc.text("ABSENCES PROFESSEURS", pageWidth/2, 40, { align: "center" });

            // Date du rapport
            const today = new Date().toLocaleDateString('fr-FR');
            doc.text(`Date du rapport : ${today}`, 14, 50);

            // Tableau des absences
            doc.autoTable({
                startY: 55,
                head: [['Classe', 'Matière', 'Heure', 'Date']],
                body: absences.map(a => [a.classe, a.matiere, a.heure, a.date]),
                headStyles: { fillColor: [255, 152, 0], textColor: 255 },
                alternateRowStyles: { fillColor: [232, 245, 233] },
                styles: { fontSize: 11 }
            });

            doc.save("rapport_absences.pdf");
        }
    </script>
</body>
</html>
