---

# README - API de Prédiction d’Éligibilité au Don de Sang

Bienvenue dans le dépôt de l’**API de Prédiction d’Éligibilité au Don de Sang**, une API REST développée pour prédire si un individu est éligible à donner du sang à partir de ses caractéristiques personnelles, physiques et sociales. Déployée sur Render, cette API alimente le module de prédiction du tableau de bord associé.

---

## 🌟 Fonctionnalités

- **Endpoint de Prédiction** : Fournit une prédiction d’éligibilité (`Éligible` ou `Non éligible`) via une requête POST.
- **Probabilités** : Retourne les probabilités associées à chaque classe (éligible/non éligible).
- **Déploiement Cloud** : Accessible publiquement sur `https://api-blood-donation.onrender.com/predict`.

---

## 🛠️ Outils Utilisés

- **FastAPI** : Framework Python pour une API REST rapide et légère.
- **Scikit-learn & Joblib** : Chargement et exécution du modèle Random Forest.
- **Pandas** : Traitement des données d’entrée.
- **Pydantic** : Validation des données JSON entrantes.
- **Render** : Hébergement cloud de l’API.
- **Uvicorn** : Serveur ASGI pour exécuter FastAPI.
- **Python 3.11** : Langage principal.

---

## ⚙️ Hypothèses

- Le modèle (`eligibility_model_rf_no_leak.pkl`) est entraîné sur des données fiables et sans fuite.
- Les fichiers `.pkl` (modèle et scaler) sont présents à la racine du dépôt.
- Les catégories envoyées dans les requêtes correspondent aux mappings définis dans `api.py`.

---

## 🚀 Instructions pour Exécuter l’API

### Prérequis
- **Python 3.11+** installé.
- **Git** pour cloner le dépôt.

### Installation
1. **Cloner le Dépôt** :
   ```bash
   git clone https://github.com/hyontnick/api_blood_donation.git
   cd mon-api
   ```

2. **Installer les Dépendances** :
   ```bash
   pip install -r requirements.txt
   ```
   Contenu de `requirements.txt` :
   ```
   fastapi==0.110.0
   uvicorn==0.29.0
   pandas==2.2.1
   joblib==1.4.0
   pydantic==2.6.4
   scikit-learn==1.4.1.post1
   ```

3. **Lancer l’API Localement** :
   ```bash
   uvicorn api:app --reload --host 0.0.0.0 --port 8000
   ```
   Accède à `http://localhost:8000/docs` pour tester via l’interface Swagger.

### Tester l’API
Envoie une requête POST à `/predict` :
```bash
curl -X POST "http://localhost:8000/predict" -H "Content-Type: application/json" -d '{"age": 25, "niveau_detude": "Universitaire", "genre": "Homme", "taille": 170.0, "poids": 70.0, "situation_matrimoniale_sm": "Célibataire", "profession": "Etudiant (e)", "arrondissement_de_residence": "Douala 3", "nationalite": "Camerounaise", "religion": "Chretien (Catholique)", "a_til_elle_deja_donne_le_sang": "Non", "si_oui_preciser_la_date_du_dernier_don": "1900-01-01", "taux_dhemoglobine": 13.5}'
```

Réponse attendue :
```json
{
  "result": "Éligible",
  "probability_eligible": 0.92,
  "probability_not_eligible": 0.08
}
```

### Déploiement sur Render
1. Pousse le dépôt sur GitHub.
2. Crée un service web sur Render :
   - **Build Command** : `pip install -r requirements.txt`
   - **Start Command** : `uvicorn api:app --host 0.0.0.0 --port 10000`
3. URL publique : `https://api-blood-donation.onrender.com`.

---

## 📝 Notes

- **Structure du Dépôt** :
  - `api.py` : Code principal de l’API.
  - `eligibility_model_rf_no_leak.pkl`, `scaler_no_leak.pkl` : Fichiers du modèle et scaler.
- **Limites** : Sur le tier gratuit de Render, l’API passe en sommeil après 15 min d’inactivité.

Pour toute question, ouvrez une issue ou contactez [hyontnick@gmail.com].

---
