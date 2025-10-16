### ***Multi-Material Topology Optimization — EDA & Baseline***

### **Descripción del proyecto**

Este proyecto aborda el análisis y modelado del conjunto de datos asociado al paper “Multi-material topology optimization of structural load-bearing capacity using limit analysis”, buscando predecir la variable pobj (capacidad portante óptima) a partir de variables estructurales y de configuración del problema.

El trabajo incluye:

-   Exploratory Data Analysis (EDA) detallado,

-   Identificación de riesgos del dataset,

-   Modelado baseline con dos algoritmos simples de regresión,

-   Evaluación inicial y generación de métricas base,

-   Plan de acciones para el siguiente sprint.

### **Dataset**

- **Fuente:** Journal of Theoretical, Computational and Applied Mechanics

- **Registros:** ≈ 209 observaciones

- **Tipo de problema:** regresión supervisada

- **Variable objetivo:** `pobj`

Predictoras: p, pmax, criterion, problem_type, experiment, gray_level, iteration (excluida por riesgo de leakage).

Los archivos originales se encuentran en la carpeta:
```
├── data/
│ ├── bimaterial_fc_1.0_ft_1.0_cost_0.1/
│ │ ├── parameters.json
│ │ ├── results.h5
│ │ ├── results.json
│ │ └── results.xdmf
│ ├── bimaterial_fc_1.0_ft_1.0_cost_0.5/
│ │ ├── parameters.json
│ │ ├── results.h5
│ │ ├── results.json
│ │ └── results.xdmf
│
| ...
|
│ ├── combined_parameters.json
│ └── combined_results.json
|
├── notebooks/ 
│ ├── _01_EDA.ipynb
│ └── 02_Baseline.ipynb
│
├── results/ 
│ ├── all_results.csv
│ ├── baseline_metrics.csv
│ ├── cross_validation_metrics.csv 
│ ├── eda_results.csv
│ ├── experiments_log.csv
│ └── images/ 
| │ ├── EDA/
| │ │ ├── distribucion_boxplot_criterion.png
| │ │ ├── distribucion_boxplot_experiment.png
| │ │ ├── distribucion_boxplot_gray_level.png
| │ │ ├── distribucion_boxplot_iteration.png
| │ │ ├── distribucion_boxplot_p.png
| │ │ ├── distribucion_boxplot_pmax.png
| │ │ ├── distribucion_boxplot_pobj.png
| │ │ ├── distribucion_boxplot_problem_type.png
| │ │ └── mapa_correlacion.png
| │ ├── Baseline/
| │ | ├── pred_vs_real_Regresión_Lineal.png
| │ | └── pred_vs_real_KNN(k=5).png
| │ └── Experiments/
| │ | └── metrics_comparison.png
| │ └── residuales_KNN.png
|
├── requirements.txt 
└── README.md 

```
## **Exploratory Data Analysis (EDA)**

El análisis exploratorio se realizó en el notebook \_01_EDA.ipynb, generando estadísticas descriptivas, análisis de correlaciones y visualizaciones.

*Hallazgos principales:*

-   iteration está altamente correlacionada con p.

-   pobj y p presentan distribución diferente entre experimentos (riesgo de drift).

-   p y pmax muestran correlación moderada (r ≈ 0.33).

-   Variables con escalas muy diferentes (gray_level vs iteration).

-   Algunos valores nulos (alpha) en experimentos single_material. No outliers severos.

| Riesgo | Descripción | Implicación |
|:-----------------------|:-----------------------|:-----------------------|
| **Leakage (`iteration`)** | Refleja el avance del solver | Puede inducir aprendizaje del proceso |
| **Drift entre experimentos** | Diferencias en `p` y `pobj` | Mala generalización |
| **Correlación moderada (`p`–`pmax`)** | Variables parcialmente redundantes | Posible varianza |
| **Escala heterogénea** | Rangos dispares entre variables | Afecta modelos sensibles a la escala |
| **Muestra pequeña (\~209)** | Pocos datos | Riesgo de sobreajuste |



# **Baseline**

El baseline se implementó en \_02_Baseline.ipynb, evaluando dos modelos simples de regresión:

-   Regresión Lineal

-   KNN (k = 5)

### **Decisiones implementadas**

| Aspecto | Decisión | Justificación |
|:-----------------------|:-----------------------|:-----------------------|
| Variables | Se eliminó `iteration` | Evitar leakage |
| División | `train_test_split` (80/20, random_state = 42) | Métricas reproducibles |
| Escalado | `StandardScaler` solo para KNN | Sensibilidad a la escala |
| Métricas | MAE, RMSE, R² | Adecuadas para regresión |



### **Resultados**

| Modelo           | MAE    | RMSE   | R²    |
|:-----------------|:-------|:-------|:------|
| Regresión Lineal | 0.0196 | 0.0259 | 0.864 |
| KNN (k = 5)      | 0.0090 | 0.0206 | 0.914 |

### **Conclusión del Baseline original:**

KNN (k = 5) obtiene mejor desempeño (R² ≈ 0.91), indicando una relación no lineal entre las variables y pobj. Esto servirá como punto de referencia para comparar futuras mejoras en el segundo sprint.

## **Plan de trabajo — Segundo Sprint**

Acciones derivadas del EDA y baseline:

1.  Normalizar y codificar variables categóricas.

2.  Implementar validación cruzada o GroupKFold por experiment.

3.  Evaluar modelos más robustos: Ridge, Random Forest, SVR.

4.  Aplicar ingeniería de características (nuevas combinaciones o PCA).

5.  Analizar estabilidad de métricas frente al baseline.

### **Registro de experimentos**

-   **Baseline:** LinearRegression y KNN (k=5).\
-   **Variante A:** LinearRegression sin escalado → Sin diferencia.\
-   **Variante B:** KNN (k=9) → Ligera pérdida de desempeño.

Los registros de experimentos muestran que la **Regresión Lineal** no mejora con el escalado, manteniendo un R² ≈ 0.864.

El **modelo KNN (k=5)** sigue siendo el de mejor desempeño (R² ≈ 0.914), mientras que al aumentar vecinos a **k=9** el rendimiento disminuye (R² ≈ 0.886).

### **Resultados comparables**

-   El KNN (k=5) obtuvo el mejor desempeño (R² = 0.91, MAE = 0.009), superando a la regresión lineal (R² = 0.86).\
-   El escalado no afectó a la regresión lineal, y aumentar a k=9 redujo la precisión (R² = 0.89).\
-   Todos los modelos fueron rápidos (\<0.03 s).\
-   **Conclusión:** KNN (k=5) se adopta como baseline por su equilibrio entre precisión y eficiencia.

### **Feature set y pipeline**

-   **Variable objetivo:** `pobj`
-   **Variables excluidas:** `iteration` (para evitar **data leakage**).\
-   **Preprocesamiento:** división train/test (80/20) con `random_state=42` y escalado mediante `StandardScaler` (fit solo en train).\
-   **Modelos:** regresión lineal (con/sin escalado) y KNN (k=5, k=9).\
-   **Validación:** sin fugas de datos; partición aleatoria reproducible.


### **Validación cruzada**

> Nota: En el baseline inicial se empleó KNN(k=5) como configuración por defecto.  
> Tras la validación cruzada, se confirmó que KNN(k=9) mejora la estabilidad y el desempeño general (R² ≈ 0.878).  
> Este ajuste se considera una optimización del modelo base, sin alterar la naturaleza del baseline.


### **Instalación de dependencias y subir a GitHub**

Clonar el repositorio: git clone \<repo_url\> cd \<repo_name\>

**Para crear otra rama**

**Ver las ramas existentes**

    git branch

**Crear una nueva rama (por ejemplo 'documentacion')**

    git checkout -b documentacion

**Añadir los archivos modificados o nuevos**

    git add README.md

**Confirmar los cambios con un mensaje descriptivo**

    git commit -m "Actualiza README"

**Subir los cambios a la rama actual en el remoto**

    git push origin documentacion

**Instalar dependencias:**

    pip install -r requirements.txt

**Ejecutar notebooks en orden:**

    1.  notebooks/\_01_EDA.ipynb\
    2.  notebooks/\_02_Baseline.ipynb

**Autores:**
