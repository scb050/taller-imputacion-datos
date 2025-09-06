# Clasificación del tipo de faltante

En esta sección evaluamos el mecanismo de ausencia de los datos (MCAR, MAR, MNAR) a partir de la **capacidad predictiva (AUC)** de cada variable con valores nulos.  



## AUC del modelo de ausencia


```python
# Aquí se calculó la capacidad predictiva (AUC) de cada variable faltante
# para determinar si los nulos son MCAR, MAR o MNAR

auc_results

# Visualización de la tabla con las variables, su AUC,
# la clasificación del mecanismo de ausencia y el % de nulos
auc_results.sort_values("AUC(modelo ausencia)", ascending=False)
```
