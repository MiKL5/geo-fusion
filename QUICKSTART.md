# Guide d'Installation Rapide
## Système de Crawling Académique - France

---

## 🚀 Installation en 5 minutes

### 1. Prérequis

- Python 3.9 ou supérieur
- pip installé
- 2 GB RAM minimum

Vérification :
```bash
python3 --version  # Doit afficher >= 3.9
pip3 --version
```

### 2. Installation des dépendances

```bash
# Depuis le répertoire du projet
pip install -r requirements.txt --break-system-packages

# OU avec environnement virtuel (recommandé)
python3 -m venv venv
source venv/bin/activate  # Linux/Mac
# OU
venv\Scripts\activate  # Windows

pip install -r requirements.txt
```

### 3. Test rapide

```bash
# Test OpenStreetMap (le plus simple)
python3 osm_crawler.py

# OU test avec le script d'exemple
python3 example_usage.py --targets osm
```

### 4. Résultats

Les données seront dans :
- `./data/osm/` - Données OpenStreetMap
- `./data/airbnb/` - Données AirBNB
- `./data/pagesjaunes/` - Données Pages Jaunes
- `./logs/` - Fichiers de logs

---

## 📋 Commandes Principales

### Crawler OpenStreetMap
```bash
python3 osm_crawler.py
```

### Crawler AirBNB
```bash
python3 airbnb_crawler.py
```

### Crawler Pages Jaunes
```bash
python3 pagesjaunes_crawler.py
```

### Script d'orchestration
```bash
# Tous les crawlers
python3 example_usage.py --targets all

# Crawlers spécifiques
python3 example_usage.py --targets osm pj

# Avec output personnalisé
python3 example_usage.py --targets osm --output /mes/données
```

---

## ⚙️ Configuration

### Modifier les paramètres

Éditez directement dans les fichiers Python :

**osm_crawler.py** :
```python
# Ligne ~50
REQUEST_DELAY_MIN: float = 1.5  # Ajustez selon vos besoins
MAX_CONCURRENT_REQUESTS: int = 2
```

**airbnb_crawler.py** :
```python
# Ligne ~50
REQUEST_DELAY_MIN: float = 3.0
MAX_CONCURRENT_REQUESTS: int = 1  # Gardez à 1 pour éviter blocages
```

**pagesjaunes_crawler.py** :
```python
# Ligne ~100
CATEGORIES: List[str] = ["restaurant", "boulangerie", ...]
FRANCE_CITIES: List[str] = ["Paris", "Lyon", ...]
```

### Ou utilisez config.example.json

```bash
# Copiez et modifiez
cp config.example.json config.json
# Puis chargez dans vos scripts
```

---

## 🔍 Exemples d'Utilisation

### 1. Récupérer tous les restaurants à Paris

```python
from pagesjaunes_crawler import PJCrawler

crawler = PJCrawler()
await crawler.initialize()

restaurants = await crawler.search_all_pages(
    query="restaurant",
    location="Paris",
    max_pages=5
)

crawler.export_csv(restaurants, "restaurants_paris.csv")
```

### 2. Listings AirBNB à Lyon

```python
from airbnb_crawler import AirBNBCrawler

crawler = AirBNBCrawler()

region_lyon = {
    "name": "Lyon",
    "ne_lat": 45.8085,
    "ne_lng": 4.8975,
    "sw_lat": 45.7073,
    "sw_lng": 4.7768
}

listings = await crawler.search_region(region_lyon, guests=2)
```

### 3. POIs OpenStreetMap

```python
from osm_crawler import OSMCrawler

crawler = OSMCrawler()

await crawler.crawl_france_data([
    'amenity=cafe',
    'amenity=pharmacy',
    'tourism=hotel'
])
```

---

## 🐛 Dépannage

### Erreur: "Module not found"
```bash
pip install -r requirements.txt --break-system-packages
```

### Erreur: "Rate limit"
- Augmentez `REQUEST_DELAY_MIN` dans la config
- Réduisez `MAX_CONCURRENT_REQUESTS` à 1

### Erreur: "CAPTCHA detected" (AirBNB)
- C'est normal, pause automatique de 5 minutes
- Réduisez la fréquence des requêtes
- Changez de réseau si blocage persistant

### Mémoire insuffisante
```python
# Dans la config
MAX_MEMORY_MB: int = 300  # Réduire
BATCH_SIZE: int = 10      # Réduire
```

---

## 📊 Structure des Données Extraites

### OpenStreetMap
```json
{
  "elements": [
    {
      "type": "node",
      "id": 123456789,
      "lat": 48.8566,
      "lon": 2.3522,
      "tags": {
        "amenity": "restaurant",
        "name": "Le Bistrot",
        "cuisine": "french"
      }
    }
  ]
}
```

### AirBNB
```json
{
  "title": "Appartement Centre Paris",
  "price": 120,
  "rating": 4.8,
  "location": "Paris, France",
  "url": "https://www.airbnb.fr/rooms/...",
  "listing_id": "12345678"
}
```

### Pages Jaunes
```json
{
  "name": "Boulangerie Dupont",
  "category": "Boulangerie",
  "address": "10 Rue de la Paix",
  "postal_code": "75001",
  "city": "Paris",
  "phone": "01 42 60 12 34",
  "latitude": 48.8566,
  "longitude": 2.3522,
  "rating": 4.5,
  "review_count": 42
}
```

---

## 📞 Support

- **Documentation complète** : Voir README.md
- **Issues** : Créez une issue GitHub
- **Email** : contact@example.com

---

## ⚖️ Mentions Légales

- **OpenStreetMap** : © OpenStreetMap contributors (ODbL)
- **Usage éthique** : Respectez les CGU et robots.txt
- **RGPD** : Données personnelles à traiter avec précaution
- **Rate limiting** : Ne surchargez pas les serveurs

---

## 🎓 Pour aller plus loin

1. **Personnalisez les POIs** : Modifiez `OSM_POI_TYPES` dans les configs
2. **Ajoutez des villes** : Étendez `FRANCE_CITIES` dans Pages Jaunes
3. **Exportez en base de données** : Ajoutez PostgreSQL/MongoDB
4. **Visualisez** : Intégrez avec Plotly/Folium pour cartes interactives
5. **Automatisez** : Configurez des cron jobs pour crawl régulier

---

**Version** : 1.0.0  
**Dernière MAJ** : 02/02/2024  
**Licence** : MIT
