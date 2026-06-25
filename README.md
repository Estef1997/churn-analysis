# Customer Churn Analysis — Telco (IBM Dataset)

Proyecto end-to-end de análisis de datos que predice qué clientes de telecomunicaciones tienen mayor probabilidad de abandonar el servicio, y **por qué** — desde el análisis exploratorio y la validación estadística hasta el modelado predictivo y las recomendaciones de negocio.

## Problema de negocio

Una empresa de telecomunicaciones está perdiendo clientes. Este proyecto identifica a los clientes con mayor riesgo de irse y los factores que explican ese riesgo, para que el equipo de retención actúe sobre los segmentos correctos en lugar de repartir esfuerzo de forma dispersa.

> **Pregunta de negocio:** ¿Qué características distinguen a los clientes que abandonan, y podemos predecir el abandono con tiempo suficiente para retenerlos?

## Dataset

- **Fuente:** Telco Customer Churn — IBM sample dataset (Kaggle)
- **Tamaño:** 7,043 clientes × 33 columnas
- **Variable objetivo:** `Churn Value` (1 = abandonó, 0 = permaneció)
- **Tasa de churn observada:** 26.5% (1,869 clientes)

## Herramientas

Python · pandas · numpy · matplotlib · seaborn · scikit-learn · Jupyter (VS Code)

## Metodología

1. **Limpieza de datos** — se corrigió `Total Charges` (venía como texto); los 11 valores no numéricos resultaron ser clientes nuevos con `Tenure = 0` y sin facturación, rellenados con 0.
2. **Análisis exploratorio (EDA)** — análisis univariado y bivariado de los factores de churn.
3. **Validación estadística** — chi-cuadrado (contrato vs. churn) y t-test (antigüedad vs. churn), ambos significativos con p < 0.001.
4. **Modelado predictivo** — Regresión Logística vs. Random Forest, con balanceo de clases para manejar el desbalance 26.5% / 73.5%.
5. **Comunicación** — hallazgos y recomendaciones escritos para una audiencia ejecutiva no técnica.

## Hallazgos principales

1. **El tipo de contrato es el factor más fuerte.** Los clientes mes-a-mes abandonan al 42.7% vs. 2.8% en contratos de dos años (~15× de diferencia).
2. **El riesgo se concentra en clientes nuevos.** Los que abandonan promedian 18 meses de antigüedad; los que permanecen, 37.6.
3. **El método de pago es un síntoma, no una causa.** El cheque electrónico mostraba 45% de churn, pero el 78% de esos clientes son mes-a-mes — el contrato explica la asociación.
4. **Las correlaciones numéricas confirman la antigüedad** como el predictor legítimo más fuerte (-0.35 con churn).

## Resultados del modelado

Se entrenaron dos modelos y ambos confirmaron de forma independiente los drivers del EDA (contrato, antigüedad, cargos).

| Modelo | Recall (churn) | Precision (churn) | F1 | Accuracy |
|-------|:---:|:---:|:---:|:---:|
| **Regresión Logística** (seleccionado) | **0.78** | 0.51 | 0.62 | 0.75 |
| Random Forest | 0.49 | 0.63 | 0.55 | 0.79 |

**Se seleccionó la Regresión Logística pese a su menor accuracy.** En un contexto de retención, el recall es la métrica prioritaria — no detectar a un cliente que está por irse cuesta más que una falsa alarma. El accuracy engaña con clases desbalanceadas.

> **Nota sobre data leakage:** se excluyó `Churn Score` del modelado. Fue calculado por el proveedor del dataset usando el resultado de churn, por lo que no estaría disponible al evaluar a un cliente activo. Incluirlo habría producido métricas artificialmente altas pero sin sentido.

## Recomendaciones de negocio

1. **Incentivos focalizados para clientes mes-a-mes de alto riesgo** — un beneficio condicionado a migrar a un contrato más largo, dirigido (vía el modelo) solo a los más propensos a irse.
2. **Programa de onboarding en los primeros 90 días** — dado que el churn se concentra en la antigüedad temprana, reforzar la experiencia inicial del cliente.
3. **Investigar el churn elevado en clientes de fibra óptica** — un patrón contraintuitivo en un servicio premium que requiere análisis de precio/valor y de experiencia técnica antes de actuar.

## Limitaciones

- Sin datos financieros (revenue por contrato, costos) para dimensionar el ROI de las acciones de retención.
- El dataset no especifica el período de medición del churn, lo que limita la comparación con benchmarks de industria.

## Cómo ejecutarlo

Cloná o descargá el repositorio, instalá las dependencias y abrí el notebook:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn openpyxl jupyter
```

Luego abrí `01_eda.ipynb` en VS Code (o Jupyter) y ejecutá todas las celdas.

## Autora

**Estef** — Analista de datos · proyecto de portafolio desarrollado para el Google Advanced Data Analytics Professional Certificate.
