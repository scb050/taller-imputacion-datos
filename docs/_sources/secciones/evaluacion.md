# Evaluación de la imputación

## Comparación de distribuciones

```python
import matplotlib.pyplot as plt

# Comparación de histogramas antes y después de la imputación
def comparar_distribuciones(df_original, df_imputado, columna, metodo):
    plt.figure(figsize=(8,5))
    df_original[columna].dropna().plot(kind="hist", density=True, alpha=0.5, label="Original (no nulos)")
    df_imputado[columna].plot(kind="hist", density=True, alpha=0.5, label=f"Imputado ({metodo})")
    plt.title(f"Distribución {columna}: antes vs después")
    plt.legend()
    plt.show()

# Ejemplo con edad, altura, ingresos
comparar_distribuciones(df, imputaciones["num_knn__cat_hotdeck"], "edad", "num_knn__cat_hotdeck")
comparar_distribuciones(df, imputaciones["num_knn__cat_hotdeck"], "altura_cm", "num_knn__cat_hotdeck")
comparar_distribuciones(df, imputaciones["num_knn__cat_hotdeck"], "ingresos", "num_knn__cat_hotdeck")

# Comparación de conteos en variables categóricas
def comparar_categoricas(df_original, df_imputado, columna, metodo):
    fig, ax = plt.subplots(figsize=(7,5))
    df_original[columna].value_counts().plot(kind="bar", alpha=0.5, label="Original (no nulos)", ax=ax)
    df_imputado[columna].value_counts().plot(kind="bar", alpha=0.5, label=f"Imputado ({metodo})", ax=ax)
    plt.title(f"{columna}: conteos antes vs después")
    plt.legend()
    plt.show()

# Ejemplo con sexo, ciudad y nivel educativo
comparar_categoricas(df, imputaciones["num_knn__cat_hotdeck"], "sexo", "num_knn__cat_hotdeck")
comparar_categoricas(df, imputaciones["num_knn__cat_hotdeck"], "ciudad", "num_knn__cat_hotdeck")
comparar_categoricas(df, imputaciones["num_knn__cat_hotdeck"], "nivel_educativo", "num_knn__cat_hotdeck")

from scipy.stats import shapiro, ks_2samp, ttest_ind, mannwhitneyu, chi2_contingency

resultados_eval = []

# Ejemplo numéricas
for col in ["edad", "altura_cm", "ingresos", "gasto_mensual", "puntuacion_credito", "demanda"]:
    orig = df[col].dropna()
    imp = imputaciones["num_knn__cat_hotdeck"][col]
    shapiro_orig = shapiro(orig)[1]
    shapiro_after = shapiro(imp)[1]
    ks_p = ks_2samp(orig, imp)[1]
    try:
        ttest_p = ttest_ind(orig, imp)[1]
    except:
        ttest_p = None
    try:
        mann_p = mannwhitneyu(orig, imp)[1]
    except:
        mann_p = None

    resultados_eval.append([col, shapiro_orig, shapiro_after, ks_p, ttest_p, mann_p])

# Ejemplo categóricas
for col in ["sexo", "ciudad", "nivel_educativo", "segmento", "estado_civil"]:
    tabla = pd.crosstab(df[col].dropna(), imputaciones["num_knn__cat_hotdeck"][col])
    chi2_p = chi2_contingency(tabla)[1]
    resultados_eval.append([col, None, None, None, None, chi2_p])

import pandas as pd
eval_df = pd.DataFrame(resultados_eval, columns=["variable","shapiro_orig_p","shapiro_after_p","ks_p","ttest_p","mannwhitney_p","chi2_p"])
eval_df
```
