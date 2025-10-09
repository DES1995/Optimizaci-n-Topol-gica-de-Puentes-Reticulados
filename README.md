### Multi-Material Topology Optimization — EDA & Baseline


**Descripción del proyecto**

Este proyecto aborda el análisis y modelado del conjunto de datos asociado al paper
“Multi-material topology optimization of structural load-bearing capacity using limit analysis”,
buscando predecir la variable pobj (capacidad portante óptima) a partir de variables estructurales y de configuración del problema.

El trabajo incluye:

- Exploratory Data Analysis (EDA) detallado,

- Identificación de riesgos del dataset,

- Modelado baseline con dos algoritmos simples de regresión,

- Evaluación inicial y generación de métricas base,

- Plan de acciones para el siguiente sprint.

**Dataset**

Fuente: Journal of Theoretical, Computational and Applied Mechanics

Registros: ≈ 209 observaciones

Tipo de problema: regresión supervisada

Variable objetivo: pobj

Predictoras: p, pmax, criterion, problem_type, experiment, gray_level, iteration (excluida por riesgo de leakage).

Los archivos originales se encuentran en la carpeta:

├── .venv/ (entorno virtual)
├── data/
|   ├── bimaterial_fc_1.0_ft_1.0_cost_0.1
|       ├── parameters.json
|       ├── results.h5
|       ├── results.json
|       └── results.xdmf
|   ├── bimaterial_fc_1.0_ft_1.0_cost_0.5
|       ├── parameters.json
|       ├── results.h5
|       ├── results.json
|       └── results.xdmf
...
|   ├── combined_parameters.json
|   └── combined_results.json
├── notebooks/
│   ├── _01_EDA.ipynb
│   ├── _02_Baseline.ipynb
├── results/
│   ├── eda_results.csv
│   ├── baseline_metrics.csv
│   └── images/
|       ├── EDA/
│           ├── distribucion_boxplot_criterion.png
│           ├── distribucion_boxplot_experiment.png
│           ├── distribucion_boxplot_gray_level.png
│           ├── distribucion_boxplot_iteration.png
│           ├── distribucion_boxplot_p.png
│           ├── distribucion_boxplot_pmax.png
│           ├── distribucion_boxplot_pobj.png
│           ├── distribucion_boxplot_problem_type.png
│           └── mapa_correlacion.png
│       └── Baseline/
│           ├── pred_vs_real_Regresión Lineal.png
│           └── pred_vs_real_KNN (k=5).png
├── requirements.txt
└── README.md

## Exploratory Data Analysis (EDA)

El análisis exploratorio se realizó en el notebook _01_EDA.ipynb, generando estadísticas descriptivas, análisis de correlaciones y visualizaciones.

_Hallazgos principales:_

- iteration está altamente correlacionada con p.

- pobj y p presentan distribución diferente entre experimentos (riesgo de drift).

- p y pmax muestran correlación moderada (r ≈ 0.33).

- Variables con escalas muy diferentes (gray_level vs iteration).

- Algunos valores nulos (alpha) en experimentos single_material. No outliers severos.

| Riesgo                                | Descripción                        | Implicación                           |
| ------------------------------------- | ---------------------------------- | ------------------------------------- |
| **Leakage (`iteration`)**             | Refleja el avance del solver       | Puede inducir aprendizaje del proceso |
| **Drift entre experimentos**          | Diferencias en `p` y `pobj`        | Mala generalización                   |
| **Correlación moderada (`p`–`pmax`)** | Variables parcialmente redundantes | Posible varianza                      |
| **Escala heterogénea**                | Rangos dispares entre variables    | Afecta modelos sensibles a la escala  |
| **Muestra pequeña (~209)**            | Pocos datos                        | Riesgo de sobreajuste                 |

## Baseline

El baseline se implementó en _02_Baseline.ipynb, evaluando dos modelos simples de regresión:

- Regresión Lineal

- KNN (k = 5)

# Decisiones implementadas

| Aspecto            | Decisión                                      | Justificación            |
| ------------------ | --------------------------------------------- | ------------------------ |
| Variables          | Se eliminó `iteration`                        | Evitar leakage           |
| División           | `train_test_split` (80/20, random_state = 42) | Métricas reproducibles   |
| Escalado           | `StandardScaler` solo para KNN                | Sensibilidad a la escala |
| Métricas           | MAE, RMSE, R²                                 | Adecuadas para regresión |

# Resultados

| Modelo           | MAE    | RMSE   | R²    |
| ---------------- | ------ | ------ | ----- |
| Regresión Lineal | 0.0196 | 0.0259 | 0.864 |
| KNN (k = 5)      | 0.0090 | 0.0206 | 0.914 |

**Conclusión:**

KNN (k = 5) obtiene mejor desempeño (R² ≈ 0.91), indicando una relación no lineal entre las variables y pobj.
Esto servirá como punto de referencia para comparar futuras mejoras en el segundo sprint.

## Plan de trabajo — Segundo Sprint

Acciones derivadas del EDA y baseline:

1. Normalizar y codificar variables categóricas.

2. Implementar validación cruzada o GroupKFold por experiment.

3. Evaluar modelos más robustos: Ridge, Random Forest, SVR.

4. Aplicar ingeniería de características (nuevas combinaciones o PCA).

5. Analizar estabilidad de métricas frente al baseline.

## Instalación de dependencias y subir a GitHub

- Clonar el repositorio:
git clone <repo_url>
cd <repo_name>

- Subir archivos a Github
git add .
git commit -m "mensaje"
git push

- Instalar dependencias:
pip install -r requirements.txt

- Ejecutar notebooks en orden:
1. notebooks/_01_EDA.ipynb
2. notebooks/_02_Baseline.ipynb

Autores:

- 