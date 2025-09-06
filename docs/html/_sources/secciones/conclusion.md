# Conclusiones del proyecto de imputación

En este proyecto se abordó el análisis, clasificación y tratamiento de valores faltantes en un dataset mixto (numérico y categórico).  



## Principales hallazgos

- La mayoría de los valores faltantes siguen un **patrón MAR (Missing At Random)**, lo que significa que su ausencia puede explicarse a partir de otras variables observadas.  
- Variables críticas como **puntuacion_credito**, **estado_civil** y **gasto_mensual** presentan los mayores porcentajes de nulos (50%, 35% y 25% respectivamente).  
- Las técnicas de imputación aplicadas fueron:  
  - **Numéricas**: media, mediana, KNN e Iterative.  
  - **Categóricas**: moda, hot-deck y KNN categórico.  
- En la evaluación:  
  - **KNN** demostró ser el método más robusto para las numéricas, preservando mejor la distribución.  
  - En las categóricas, **moda** y **hot-deck** fueron simples y efectivas.  
  - Las pruebas estadísticas (Shapiro, KS, T-test y Chi²) confirmaron que en la mayoría de los casos las imputaciones no distorsionaron de forma significativa las distribuciones originales.  



## Conclusión final

La imputación permitió **recuperar información sin introducir sesgos graves**, mejorando la calidad del dataset y dejándolo listo para análisis posteriores.  

Se recomienda:  
1. Usar **KNN** en variables numéricas clave.  
2. Aplicar **moda** o **hot-deck** en categóricas.  
3. Documentar siempre los porcentajes de nulos y las técnicas usadas para asegurar la transparencia en el análisis.  


