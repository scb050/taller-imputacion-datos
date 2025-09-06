


# Exploración inicial de las variables


En esta sección se analiza la distribución de las variables y se detectan valores faltantes.  



## Análisis de valores nulos

### Conteo y porcentaje de nulos

```python
miss_cnt = df.isna().sum()
miss_pct = (miss_cnt / len(df)) * 100
pd.DataFrame({"variable": df.columns, "nulos": miss_cnt, "pct_nulos": miss_pct})

import matplotlib.pyplot as plt

miss_pct.sort_values(ascending=False).plot(kind="bar", figsize=(10,5))
plt.title("Porcentaje de nulos por variable")
plt.ylabel("% nulos")
plt.show()

import seaborn as sns

sns.heatmap(df.isnull(), cbar=False)
```



