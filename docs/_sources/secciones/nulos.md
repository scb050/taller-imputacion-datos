# Análisis de valores nulos  

## Conteo y porcentaje de nulos 

```python
miss_cnt = df.isna().sum()
miss_pct = (miss_cnt / len(df)) * 100
pd.DataFrame({"variable": df.columns, "nulos": miss_cnt, "pct_nulos": miss_pct})

import seaborn as sns
sns.heatmap(df.isnull(), cbar=False)
```


