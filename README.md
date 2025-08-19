# Evasión de clientes (Churn)

> Proyecto de analítica para identificar, explicar y reducir la evasión de clientes.

![status-badge](https://img.shields.io/badge/status-en%20progreso-blue)
![python-badge](https://img.shields.io/badge/python-3.10%2B-green)
![license-badge](https://img.shields.io/badge/license-MIT-lightgrey)

## Tabla de contenidos

* [Descripción](#descripción)
* [Objetivos](#objetivos)
* [Metodología](#metodología)

  * [Exploración (EDA)](#exploración-eda)
  * [Modelado](#modelado)
  * [Evaluación](#evaluación)
  * [Interpretabilidad](#interpretabilidad)
* [Resultados y hallazgos](#resultados-y-hallazgos)
* [Recomendaciones de negocio](#recomendaciones-de-negocio)
* [Contribuciones](#contribuciones)
* [Licencia](#licencia)

## Descripción

Este repositorio contiene el código y la documentación para analizar la **evasión (churn)** de clientes en una empresa de telecomunicaciones/servicios. El objetivo es entender qué factores explican la cancelación y construir modelos que permitan **predecir el riesgo** para accionar estrategias de retención.

## Objetivos

1. **Cuantificar** la tasa de evasión y perfilar clientes con mayor riesgo.
2. **Identificar** variables clave (edad, tipo de internet, servicios complementarios, contrato, cargos, etc.).
3. **Modelar** la probabilidad de churn para priorizar intervenciones.
4. **Recomendar** acciones tácticas (planes, precios, bundles, soporte) y **medir impacto**.

## Metodología

### Exploración (EDA)

* Distribuciones de `MonthlyCharges` y `TotalCharges` (boxplots e histograma). Se observó **sesgo a la izquierda** en algunos cargos.
* Comparativas por segmento de **edad** (jóvenes vs. no jóvenes).
* Análisis por **tipo de internet** (DSL/Fiber) y **servicios complementarios**: `OnlineSecurity`, `OnlineBackup`, `DeviceProtection`, `TechSupport`.
* Impacto de **tipo de contrato** (mensual vs. 1–2 años) y **método de pago**.

### Modelado

* **Clasificación primaria (Churn 0/1)**: Regresión **logística**, Árboles/Random Forest, Gradient Boosting (XGBoost/LightGBM opcional).
* **Modelos de apoyo**:

  * **Regresión lineal** sobre `MonthlyCharges`/`TotalCharges` para entender drivers de costo y su relación indirecta con el churn.
  * Manejo de desbalance con `class_weight='balanced'` o `SMOTE`.
* **Validación**: `StratifiedKFold` y búsqueda de hiperparámetros (`GridSearchCV`/`RandomizedSearchCV`).

### Evaluación

* Métricas principales: **ROC-AUC**, **Recall en clase positiva**, **Precision-Recall AUC**, **F1** y **Accuracy**.
* Curvas ROC y PR; matriz de confusión con umbral óptimo orientado a **retención** (maximizar recall manteniendo precisión mínima aceptable).

### Interpretabilidad

* Importancias de características (modelos de árbol) y **coeficientes** (logística).
* **SHAP values** para efectos marginales y explicaciones locales.

## Resultados y hallazgos

* Mayor propensión a churn en **clientes jóvenes**.
* **Internet DSL** sin servicios adicionales (seguridad en línea, respaldo, protección de dispositivos, soporte técnico) presenta **tasas de evasión superiores**.
* Los **contratos mensuales** concentran más cancelaciones que los de plazo fijo.
* Los **cargos** muestran **sesgo a la izquierda** en boxplots; revisar estructura tarifaria y descuentos.

> Nota: estos hallazgos son reproducibles ejecutando los notebooks y scripts con los datos provistos.

## Recomendaciones de negocio

1. **Bundles de valor**: incluir OnlineSecurity/Backup/DeviceProtection/TechSupport con precio preferencial para clientes en riesgo.
2. **Estrategia por segmento etario**: ofertas y onboarding específicos para clientes jóvenes.
3. **Contratos a corto plazo** con beneficios escalonados (evitar fricción de permanencia inicial y fomentar upgrade por valor percibido).
4. **Revisión de cargos**: simplificar tarifas, reducir dispersiones y comunicar beneficios.
5. **Programa de alerta temprana**: usar el modelo para disparar acciones de retención (contacto proactivo, ofertas personalizadas).

## Contribuciones

¡Contribuciones bienvenidas! Abre un **issue** o envía un **pull request** siguiendo la guía de estilo del proyecto.

## Licencia

Este proyecto se distribuye bajo la licencia **MIT**. Consulta el archivo `LICENSE`.
