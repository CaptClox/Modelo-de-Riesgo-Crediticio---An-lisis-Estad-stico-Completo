# 📊 Modelo de Riesgo Crediticio

Análisis estadístico completo para la evaluación de riesgo en solicitudes de crédito bancario, implementado en Python (Jupyter Notebook) y R.

---

---

## 🎯 Objetivo

Clasificar si un solicitante representa **riesgo crediticio alto** (`Risk = 1`) o **bajo** (`Risk = 0`) a partir de variables sociodemográficas y financieras, como apoyo a la decisión de otorgamiento de crédito.

---

## 🗂️ Dataset

| Campo | Descripción |
|---|---|
| `Age` | Edad del solicitante (años) |
| `Sex` | Sexo (male / female) |
| `Job` | Nivel de cualificación laboral (0–3) |
| `Housing` | Situación de vivienda (own / free / rent) |
| `Saving.accounts` | Saldo en cuentas de ahorro (ordinal) |
| `Checking.account` | Saldo en cuenta corriente (ordinal) |
| `Credit.amount` | Monto del crédito solicitado (DM) |
| `Duration` | Plazo del crédito (meses) |
| `Purpose` | Propósito del crédito |
| `Risk` ⭐ | **Variable objetivo** — 0: bajo riesgo, 1: alto riesgo |

- **1.000 observaciones**
- Fuente: [German Credit Risk – Kaggle](https://www.kaggle.com/datasets/uciml/german-credit)

---

## 🔬 Metodología

El análisis sigue 5 pasos estructurados:

```
Paso 0 → Entorno del estudio
Paso 1 → Análisis exploratorio
Paso 2 → Identificación de variables
Paso 3 → Estimación MCO (MPL)
Paso 4 → Validación
Paso 5 → Predicción y análisis
```

### Paso 0 — Entorno
Definición del problema, diccionario de variables y preprocesamiento (recodificación ordinal, dummies, transformaciones logarítmicas).

### Paso 1 — Análisis exploratorio
- Estadísticas descriptivas por grupo de riesgo
- Tests Chi² para variables categóricas
- Tests t para variables numéricas
- Correlación punto-biserial con `Risk`
- Visualizaciones: histogramas, barras proporcionales, matriz de correlación

### Paso 2 — Identificación de variables
Selección de variables mediante **backward elimination** (umbral p < 0.05 con errores estándar robustos HC3).

### Paso 3 — Estimación
- **Modelo de Probabilidad Lineal (MCO / MPL)** — variable dependiente binaria estimada por OLS
- Errores estándar corregidos por heterocedasticidad (**HC3 – White**)
- **Logit** como modelo de referencia + Odds Ratios con IC 95%

### Paso 4 — Validación

| Diagnóstico | Herramienta |
|---|---|
| Ajuste global | R², R² ajustado, Pseudo R² McFadden |
| Calibración | Test Hosmer-Lemeshow |
| Colinealidad | VIF (Variance Inflation Factor) |
| Heterocedasticidad | Test Breusch-Pagan |
| Outliers | Distancias de Cook (umbral 4/n) |
| Discriminación | Curva ROC + AUC |
| Clasificación | Matriz de confusión + reporte |
| Robustez | Validación cruzada 5-Fold |

### Paso 5 — Predicción
- Predicción sobre perfiles hipotéticos de solicitantes
- Análisis de sensibilidad: P(riesgo) vs variable continua
- Efectos marginales promedio (AME)
- Comparativa MCO vs Logit

---

## 📦 Requisitos

```bash
pip install numpy pandas matplotlib seaborn scikit-learn statsmodels scipy
```

| Librería | Uso principal |
|---|---|
| `statsmodels` | OLS, Logit, VIF, Breusch-Pagan |
| `scikit-learn` | Matriz de confusión, ROC, validación cruzada |
| `seaborn` / `matplotlib` | Visualizaciones |
| `scipy` | Tests estadísticos (Chi², t-test) |

---

## ▶️ Uso

1. Clona el repositorio y asegúrate de que `datos.csv` esté en la misma carpeta que el notebook.

```bash
git clone https://github.com/tu-usuario/modelo-riesgo-crediticio.git
cd modelo-riesgo-crediticio
```

2. Lanza Jupyter:

```bash
jupyter notebook modelo_riesgo_bancario.ipynb
# o
jupyter lab modelo_riesgo_bancario.ipynb
```

3. Ejecuta las celdas en orden. Los gráficos se guardan automáticamente como `.png` en el directorio de trabajo.

---

## 📈 Outputs generados

| Archivo | Descripción |
|---|---|
| `fig1_numericas.png` | Histogramas de variables numéricas por grupo de riesgo |
| `fig2_categoricas.png` | Barras proporcionales de variables categóricas |
| `fig3_correlacion.png` | Matriz de correlación |
| `fig4_diagnosticos_mco.png` | Gráficos de diagnóstico del MCO |
| `fig5_cooks.png` | Distancias de Cook |
| `fig6_roc.png` | Curva ROC |
| `fig7_sensibilidad.png` | Sensibilidad de P(riesgo) a variable continua |
| `fig_confusion.png` | Matriz de confusión |

---

## ⚠️ Notas técnicas

- Las variables ordinales (`Saving.accounts`, `Checking.account`) se recodifican como enteros (1–5 y 1–4) para permitir su uso en modelos lineales.
- Todo el DataFrame se castea a `float64` desde la celda 11 para evitar errores de dtype con `statsmodels`.
- El MPL puede generar probabilidades fuera de `[0, 1]`; se aplica `.clip(0, 1)` en la predicción. Para producción se recomienda usar el Logit.

---

## 📄 Licencia

MIT License — libre uso con atribución.
