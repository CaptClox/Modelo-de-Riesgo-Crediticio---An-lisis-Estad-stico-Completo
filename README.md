# Modelo-de-Riesgo-Crediticio-Analisis-Estadistico-Completo
📊 Modelo de Riesgo Crediticio
Análisis estadístico completo para la evaluación de riesgo en solicitudes de crédito bancario, implementado en Python (Jupyter Notebook) y R.


🎯 Objetivo
Clasificar si un solicitante representa riesgo crediticio alto (Risk = 1) o bajo (Risk = 0) a partir de variables sociodemográficas y financieras, como apoyo a la decisión de otorgamiento de crédito.

🗂️ Dataset
CampoDescripciónAgeEdad del solicitante (años)SexSexo (male / female)JobNivel de cualificación laboral (0–3)HousingSituación de vivienda (own / free / rent)Saving.accountsSaldo en cuentas de ahorro (ordinal)Checking.accountSaldo en cuenta corriente (ordinal)Credit.amountMonto del crédito solicitado (DM)DurationPlazo del crédito (meses)PurposePropósito del créditoRisk ⭐Variable objetivo — 0: bajo riesgo, 1: alto riesgo

1.000 observaciones
Fuente: German Credit Risk – Kaggle


🔬 Metodología
El análisis sigue 5 pasos estructurados:
Paso 0 → Entorno del estudio
Paso 1 → Análisis exploratorio
Paso 2 → Identificación de variables
Paso 3 → Estimación MCO (MPL)
Paso 4 → Validación
Paso 5 → Predicción y análisis
Paso 0 — Entorno
Definición del problema, diccionario de variables y preprocesamiento (recodificación ordinal, dummies, transformaciones logarítmicas).
Paso 1 — Análisis exploratorio

Estadísticas descriptivas por grupo de riesgo
Tests Chi² para variables categóricas
Tests t para variables numéricas
Correlación punto-biserial con Risk
Visualizaciones: histogramas, barras proporcionales, matriz de correlación

Paso 2 — Identificación de variables
Selección de variables mediante backward elimination (umbral p < 0.05 con errores estándar robustos HC3).
Paso 3 — Estimación

Modelo de Probabilidad Lineal (MCO / MPL) — variable dependiente binaria estimada por OLS
Errores estándar corregidos por heterocedasticidad (HC3 – White)
Logit como modelo de referencia + Odds Ratios con IC 95%

Paso 4 — Validación
DiagnósticoHerramientaAjuste globalR², R² ajustado, Pseudo R² McFaddenCalibraciónTest Hosmer-LemeshowColinealidadVIF (Variance Inflation Factor)HeterocedasticidadTest Breusch-PaganOutliersDistancias de Cook (umbral 4/n)DiscriminaciónCurva ROC + AUCClasificaciónMatriz de confusión + reporteRobustezValidación cruzada 5-Fold
Paso 5 — Predicción

Predicción sobre perfiles hipotéticos de solicitantes
Análisis de sensibilidad: P(riesgo) vs variable continua
Efectos marginales promedio (AME)
Comparativa MCO vs Logit


📦 Requisitos
bashpip install numpy pandas matplotlib seaborn scikit-learn statsmodels scipy
LibreríaUso principalstatsmodelsOLS, Logit, VIF, Breusch-Paganscikit-learnMatriz de confusión, ROC, validación cruzadaseaborn / matplotlibVisualizacionesscipyTests estadísticos (Chi², t-test)

▶️ Uso

Clona el repositorio y asegúrate de que datos.csv esté en la misma carpeta que el notebook.

bashgit clone https://github.com/tu-usuario/modelo-riesgo-crediticio.git
cd modelo-riesgo-crediticio

Lanza Jupyter:

bashjupyter notebook modelo_riesgo_bancario.ipynb
# o
jupyter lab modelo_riesgo_bancario.ipynb

Ejecuta las celdas en orden. Los gráficos se guardan automáticamente como .png en el directorio de trabajo.


📈 Outputs generados
ArchivoDescripciónfig1_numericas.pngHistogramas de variables numéricas por grupo de riesgofig2_categoricas.pngBarras proporcionales de variables categóricasfig3_correlacion.pngMatriz de correlaciónfig4_diagnosticos_mco.pngGráficos de diagnóstico del MCOfig5_cooks.pngDistancias de Cookfig6_roc.pngCurva ROCfig7_sensibilidad.pngSensibilidad de P(riesgo) a variable continuafig_confusion.pngMatriz de confusión

⚠️ Notas técnicas

Las variables ordinales (Saving.accounts, Checking.account) se recodifican como enteros (1–5 y 1–4) para permitir su uso en modelos lineales.
Todo el DataFrame se castea a float64 desde la celda 11 para evitar errores de dtype con statsmodels.
El MPL puede generar probabilidades fuera de [0, 1]; se aplica .clip(0, 1) en la predicción. Para producción se recomienda usar el Logit.

