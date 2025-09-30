## 📊 Dataset

Este proyecto combina datasets públicos y generados para entrenar y validar modelos de optimización topológica en puentes reticulados.

- **CEM Dataset** – Topologías estructurales generadas mediante el *Combinatorial Equilibrium Modelling (CEM)*.  
  Fuente: [GitHub – arpastrana/cem_dataset](https://github.com/arpastrana/cem_dataset)  
  Contiene configuraciones estructurales base que pueden adaptarse a tipologías de puentes.

- **2D Analysis of Frame and Truss** – Dataset de análisis de pórticos y cerchas en 2D.  
  Fuente: [Kaggle – 2D Analysis of Frame and Truss](https://www.kaggle.com/datasets/sarankanna/2d-analysis-of-frame-and-truss)  
  Incluye resultados de esfuerzos, desplazamientos y fuerzas internas en elementos.

- **Zenodo Multi-material Optimization Dataset** – Dataset asociado a experimentos de optimización estructural con múltiples materiales.  
  Fuente: [Zenodo – DOI:10.5281/zenodo.14773973](https://zenodo.org/records/14773973)  
  Contiene datos de instancias estructurales optimizadas bajo distintos criterios.

### 🔧 Uso de datasets
- Los datasets serán **preprocesados** para extraer variables clave:  
  - Nodos, barras y conectividad topológica.  
  - Propiedades mecánicas (módulo E, carga distribuida, límites de esbeltez).  
  - Respuesta estructural (desplazamientos, esfuerzos, masa normalizada).  
- Se generará un dataset integrado (`data/processed/bridges_truss.csv`) que servirá de entrada a los modelos de ML y a los algoritmos de optimización bioinspirada.
