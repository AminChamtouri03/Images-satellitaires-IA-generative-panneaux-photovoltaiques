# 🛰️ Images Satellitaires + IA Générative pour Optimiser l'Implantation de Panneaux Photovoltaïques

Ce projet combine la **super-résolution d'images satellitaires** et la **segmentation automatique des toitures** pour identifier les emplacements optimaux pour l'installation de panneaux photovoltaïques.

## 📋 Pipeline

```
Image Sentinel-2 (10m) → Super-Résolution (2.5m) → Segmentation des Toitures → GeoJSON
```

### 1️⃣ Super-Résolution (LDSR-S2)
- Modèle de diffusion latente développé par l'ESA
- Améliore la résolution des images Sentinel-2 de 10m à 2.5m (×4)
- Fonctionne sur les bandes RGB + NIR

### 2️⃣ Segmentation des Toitures
- Réseau U-Net entraîné sur des footprints OpenStreetMap/Overture
- Détection automatique des bâtiments
- Export en GeoJSON géoréférencé

---

## 🚀 Installation

```bash
# Cloner le repository
git clone https://github.com/AminChamtouri03/Images-satellitaires-IA-generative-panneaux-photovoltaiques.git
cd Images-satellitaires-IA-generative-panneaux-photovoltaiques

# Créer un environnement virtuel
python -m venv venv
venv\Scripts\activate  # Windows
# source venv/bin/activate  # Linux/Mac

# Installer les dépendances
pip install -r requirements.txt

# Installer le modèle de super-résolution
cd opensr-model
pip install -e .
cd ..
```

---

## 📥 Télécharger le Checkpoint du Modèle

Le fichier de poids **`opensr-ldsrs2_v1_0_0.ckpt`** (~1.5GB) doit être téléchargé séparément depuis HuggingFace :

### Option 1 : Téléchargement direct
👉 [Télécharger opensr-ldsrs2_v1_0_0.ckpt](https://huggingface.co/simon-donike/RS-SR-LTDF/resolve/main/opensr-ldsrs2_v1_0_0.ckpt)

Placez le fichier à la racine du projet.

### Option 2 : Avec wget/curl
```bash
# wget
wget https://huggingface.co/simon-donike/RS-SR-LTDF/resolve/main/opensr-ldsrs2_v1_0_0.ckpt

# curl
curl -L -o opensr-ldsrs2_v1_0_0.ckpt https://huggingface.co/simon-donike/RS-SR-LTDF/resolve/main/opensr-ldsrs2_v1_0_0.ckpt
```

### Option 3 : Automatique via Python
```python
import opensr_model
model = opensr_model.SRLatentDiffusion(config, device="cuda")
model.load_pretrained("v1_0_0")  # Télécharge automatiquement
```

---

## 📓 Notebooks

| Notebook | Description |
|----------|-------------|
| `Run_Super_Resolution.ipynb` | Applique la super-résolution sur une image Sentinel-2 |
| `rooftop_Seg_Final.ipynb` | Entraîne un U-Net et segmente les toitures |
| `Rooftop_Seg_Essayage.ipynb` | Expérimentations et tests |

---

## 📁 Structure du Projet

```
├── README.md
├── requirements.txt
├── opensr-ldsrs2_v1_0_0.ckpt    # À télécharger (HuggingFace)
├── Run_Super_Resolution.ipynb   # Notebook super-résolution
├── rooftop_Seg_Final.ipynb      # Notebook segmentation
├── opensr-model/                # Code du modèle LDSR-S2
│   ├── opensr_model/
│   ├── demo.py
│   └── requirements.txt
└── outputs/                     # Résultats (généré)
    ├── buildings.geojson
    ├── buildings_mask.tif
    └── unet_buildings.keras
```

---

## 🖼️ Données d'Entrée

### Pour la Super-Résolution
- Image GeoTIFF basse résolution (ex: `lr.tif`)
- 4 bandes : RGB + NIR
- Résolution : 10m (Sentinel-2)

### Pour la Segmentation
- Image GeoTIFF haute résolution (ex: `sr.tif` output de l'étape 1)
- Ou toute image satellite géoréférencée

---

## 📊 Résultats

Le pipeline génère :
- `sr.tif` : Image super-résolue (2.5m)
- `buildings_mask.tif` : Masque binaire des toitures
- `buildings.geojson` : Polygones vectoriels des bâtiments détectés

---

## 🔗 Références

- [LDSR-S2 Paper](https://ieeexplore.ieee.org/abstract/document/10887321) - Trustworthy Super-Resolution of Multispectral Sentinel-2 Imagery with Latent Diffusion
- [OpenSR Model GitHub](https://github.com/ESAOpenSR/opensr-model)
- [HuggingFace Checkpoints](https://huggingface.co/simon-donike/RS-SR-LTDF)

---

## 👤 Auteur

**Amin Chamtouri**

---

## 📄 License

Ce projet utilise le modèle LDSR-S2 sous licence de l'ESA. Voir `opensr-model/LICENSE` pour plus de détails.
