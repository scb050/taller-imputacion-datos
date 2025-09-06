# Técnicas de imputación a aplicar

## Imputación numérica
```` python 
from sklearn.impute import SimpleImputer, KNNImputer
from sklearn.experimental import enable_iterative_imputer
from sklearn.impute import IterativeImputer

def impute_numeric(df, cols):
    X = df.copy()
    imputaciones = {}

    # 1) media
    imp_mean = SimpleImputer(strategy="mean")
    X1 = X.copy()
    X1[cols] = imp_mean.fit_transform(X1[cols])
    imputaciones["num_media"] = X1

    # 2) mediana
    imp_med = SimpleImputer(strategy="median")
    X2 = X.copy()
    X2[cols] = imp_med.fit_transform(X2[cols])
    imputaciones["num_mediana"] = X2

    # 3) KNN
    knn = KNNImputer(n_neighbors=5)
    X3 = X.copy()
    X3[cols] = knn.fit_transform(X3[cols])
    imputaciones["num_knn"] = X3

    # 4) Iterative (regresión)
    it = IterativeImputer(random_state=0, max_iter=10, sample_posterior=False)
    X4 = X.copy()
    X4[cols] = it.fit_transform(X4[cols])
    imputaciones["num_iterative"] = X4

    return imputaciones

import numpy as np
import pandas as pd

def hot_deck_categorical(s):
    vals = s.dropna().values
    if len(vals) == 0:
        return s.fillna("DESCONOCIDO")
    probs = pd.Series(vals).value_counts(normalize=True)
    return s.apply(lambda x: np.random.choice(probs.index, p=probs.values) if pd.isna(x) else x)

def impute_categorical(df, cols):
    X = df.copy()
    imputaciones = {}

    # 1) moda
    X1 = X.copy()
    for c in cols:
        moda = X1[c].mode(dropna=True)
        fill_val = moda.iloc[0] if not moda.empty else "DESCONOCIDO"
        X1[c] = X1[c].fillna(fill_val)
    imputaciones["cat_moda"] = X1

    # 2) hot-deck
    X2 = X.copy()
    for c in cols:
        X2[c] = hot_deck_categorical(X2[c])
    imputaciones["cat_hotdeck"] = X2

    # 3) KNN (codificación ordinal + redondeo)
    X3 = X.copy()
    cat_maps = {}
    for c in cols:
        X3[c] = X3[c].astype("category")
        cat_maps[c] = dict(enumerate(X3[c].cat.categories))
        X3[c] = X3[c].cat.codes.replace(-1, np.nan)

    knn = KNNImputer(n_neighbors=5)
    X3[cols] = knn.fit_transform(X3[cols])
    for c in cols:
        X3[c] = np.round(X3[c]).astype(int).map(cat_maps[c])
    imputaciones["cat_knn"] = X3

    return imputaciones
```` 
