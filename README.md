# Optimización Topológica de Puentes Reticulados Asistida por Aprendizaje Automático y Algoritmos de Optimización Bioinspirada

## 🎯 Objetivo
Este proyecto busca desarrollar un pipeline que combine técnicas de **optimización topológica** con modelos de **aprendizaje automático** y **algoritmos de optimización bioinspirada** (GA, PSO, HHO, etc.) para obtener **configuraciones eficientes de puentes reticulados**, minimizando el peso estructural y garantizando su desempeño bajo criterios normativos.

## 👥 Autor
- Ing. Deiby Estacio Sánchez  
  - Email: deiby.estacio.s@uni.pe  
  - GitHub: [DEIBY_1995](https://github.com/DEIBY-1995)

---

## 📊 Dataset
Este proyecto combina datasets públicos y generados para entrenar y validar modelos de optimización topológica en puentes reticulados:

- **CEM Dataset** – Topologías estructurales generadas mediante el *Combinatorial Equilibrium Modelling (CEM)*.  
  Fuente: [GitHub – arpastrana/cem_dataset](https://github.com/arpastrana/cem_dataset)  

- **2D Analysis of Frame and Truss** – Dataset de análisis de pórticos y cerchas en 2D.  
  Fuente: [Kaggle – 2D Analysis of Frame and Truss](https://www.kaggle.com/datasets/sarankanna/2d-analysis-of-frame-and-truss)  

- **Zenodo Multi-material Optimization Dataset** – Dataset asociado a experimentos de optimización estructural con múltiples materiales.  
  Fuente: [Zenodo – DOI:10.5281/zenodo.14773973](https://zenodo.org/records/14773973)  

### 🔧 Uso de datasets
- Los datasets serán **preprocesados** para extraer variables clave:  
  - Nodos, barras y conectividad topológica.  
  - Propiedades mecánicas (módulo E, cargas, límites de esbeltez).  
  - Respuesta estructural (desplazamientos, esfuerzos, masa normalizada).  
- Se generará un dataset integrado en `data/processed/bridges_truss.csv` que servirá como entrada a los modelos de ML y a los algoritmos de optimización bioinspirada.

---

## 📂 Estructura del repositorio
```
data/
 ├── raw/                       # Datasets originales descargados (CEM, Kaggle, Zenodo)
 ├── processed/                 # Dataset limpio e integrado para ML

notebooks/
 ├── EDA_basico.ipynb           # Análisis exploratorio inicial (EDA, riesgos)
 └── Baseline_basico.ipynb      # Entrenamiento de baselines (Dummy + kNN)

src/
 ├── ingesta.py                 # Script de ingesta (raw → staging)
 ├── preprocesamiento.py        # Limpieza, integración y feature engineering
 └── modelo_baseline.py         # Modelos baseline (Dummy, kNN, métricas iniciales)

logs/                           # Archivos de logging y métricas
slides/                         # Presentaciones de resultados
README.md                       # Documentación principal
pyproject.toml                  # Configuración del proyecto (Poetry)
poetry.lock / requirements.txt  # Dependencias del proyecto
.gitignore                      # Exclusiones de Git
```

---

## ⚙️ Requisitos
Instala dependencias con:
```bash
pip install -r requirements.txt
```
o, si usas Poetry:
```bash
poetry install
```

---

## ▶️ Cómo correr el pipeline
1. Ingesta y preprocesamiento de datos:
```bash
python src/ingesta.py
python src/preprocesamiento.py
```

2. Entrenamiento baseline:
```bash
python src/modelo_baseline.py
```

Esto genera:
- Métricas iniciales en `logs/`  
- Gráficos comparativos en `results/`  
- Dataset limpio en `data/processed/`

---

## 📈 Resultados esperados (mínimos)
- Estadísticas descriptivas y distribuciones de variables clave (EDA).  
- Identificación de riesgos: **leakage, desbalance, drift**.  
- Modelos baseline simples (Dummy, kNN).  
- Métricas iniciales coherentes con el problema (F1/ROC-AUC para clasificación, MAE/RMSE para regresión).  
- Gráficos y tablas de desempeño inicial.  

---

## 🧭 Próximos pasos
- Sustituir datasets de prueba por instancias FEM/optimización topológica generadas.  
- Probar algoritmos bioinspirados (GA, PSO, HHO) para exploración del espacio de soluciones.  
- Integrar modelos de **Graph Neural Networks (GNN)** para representar mejor la conectividad estructural.  
- Optimizar pipeline para escalabilidad y eficiencia computacional.
