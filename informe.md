# Telecom X — Predicción de Cancelación de Clientes (Churn)
**Analista:** Junior ML — Challenge Data Science  
**Fecha:** Marzo 2026

---

## Resumen Ejecutivo

Este informe presenta los resultados del análisis predictivo de cancelación de clientes de Telecom X. Se entrenaron dos modelos de clasificación — Regresión Logística y Random Forest — sobre un dataset de 7,043 clientes, balanceado con SMOTE. El mejor modelo (Random Forest) alcanzó un **84.9% de accuracy**, identificando los principales factores de cancelación y proponiendo estrategias de retención basadas en datos.

---

## 1. Contexto del Proyecto

Telecom X enfrenta un problema crítico de cancelación de clientes (churn). La empresa busca anticiparse a este fenómeno mediante modelos predictivos que permitan identificar clientes en riesgo antes de que decidan cancelar, habilitando acciones preventivas de retención.

### Dataset

| Característica | Valor |
|---|---|
| Total de clientes | 7,043 |
| Variables originales | 22 |
| Variable objetivo | Churn (Canceló / No canceló) |
| Desbalanceo original | 73.5% No Churn / 26.5% Churn |
| Tras SMOTE | 50% / 50% (5,174 c/u) |
| División train/test | 80% / 20% |

---

## 2. Preprocesamiento de Datos

- **Eliminación de columnas irrelevantes:** Se eliminó `customerID` por ser un identificador único sin valor predictivo.
- **Codificación de variables categóricas:** Se aplicó One-Hot Encoding con `get_dummies(drop_first=True)` para evitar multicolinealidad. La variable objetivo `Churn` fue codificada con Label Encoding (0/1).
- **Tratamiento de valores nulos:** Se detectaron 11 valores nulos en la columna `Total`, imputados con la mediana de la variable.
- **Balanceo de clases con SMOTE:** El desbalanceo 73.5/26.5 fue corregido generando ejemplos sintéticos de la clase minoritaria hasta alcanzar paridad 50/50.
- **Normalización con StandardScaler:** Se aplicó estandarización (media=0, desv=1) requerida por la Regresión Logística para que todas las variables aporten equitativamente.

---

## 3. Modelos Entrenados

Se entrenaron dos modelos complementarios para comparar enfoques y garantizar robustez en los resultados.

### Regresión Logística
Modelo lineal de clasificación binaria. Requiere normalización de datos. Alta interpretabilidad mediante coeficientes. Sirve como línea base sólida para comparación.

### Random Forest
Ensemble de 100 árboles de decisión. No requiere normalización. Captura relaciones no lineales entre variables. Proporciona importancia de variables nativa.

---

## 4. Rendimiento de los Modelos

| Métrica | Regresión Logística | Random Forest | Ganador |
|---|---|---|---|
| Accuracy | 83.09% | 84.93% | Random Forest |
| Precision | 82.78% | 84.33% | Random Forest |
| Recall | 83.57% | 85.80% | Random Forest |
| F1-Score | 83.17% | 85.06% | Random Forest |
| Train Accuracy | 83.26% | 99.87% | — |
| Overfitting | No (diff: 0.2%) | Sí (diff: 15%) | Regresión Logística |

### Análisis de Overfitting

Random Forest presentó overfitting significativo (train 99.87% vs test 84.93%). Tras tuning de hiperparámetros (`max_depth=15`), la diferencia se redujo a ~9% manteniendo 84.7% en test. La Regresión Logística no presentó overfitting, con diferencia de solo 0.2% entre train y test.

### Matriz de Confusión

**Regresión Logística**
|  | Pred. No Churn | Pred. Churn |
|---|---|---|
| **Real No Churn** | 855 | 180 |
| **Real Churn** | 170 | 865 |

**Random Forest**
|  | Pred. No Churn | Pred. Churn |
|---|---|---|
| **Real No Churn** | 870 | 165 |
| **Real Churn** | 147 | 888 |

---

## 5. Variables más Influyentes en la Cancelación

### 5.1 Factores que AUMENTAN la probabilidad de cancelación

| Variable | LR Coef. | RF Importance | Interpretación |
|---|---|---|---|
| Monthly / CuentasDiarias | 4.73 | 0.103 | Mayor costo mensual → más churn |
| InternetService_Fiber optic | 4.45 | 0.052 | Clientes de fibra óptica cancelan más |
| PaymentMethod_Electronic check | 0.46 | 0.096 | Pago por e-check asociado a churn |
| PaperlessBilling_Yes | 0.34 | 0.041 | Facturación digital → más cancelaciones |
| StreamingTV/Movies_Yes | 1.65–1.68 | 0.014 | Servicios de streaming asociados a churn |

### 5.2 Factores que REDUCEN la probabilidad de cancelación

| Variable | LR Coef. | RF Importance | Interpretación |
|---|---|---|---|
| tenure | -2.15 | 0.145 | Mayor antigüedad → mayor fidelidad |
| Total | -1.30 | 0.153 | Mayor gasto acumulado → menos churn |
| Contract_Two year | -0.37 | 0.039 | Contratos largos retienen clientes |
| OnlineSecurity_Yes | -0.12 | 0.013 | Seguridad online reduce cancelaciones |
| TechSupport_Yes | -0.10 | 0.013 | Soporte técnico mejora retención |

> **Nota técnica:** `Monthly` y `CuentasDiarias` tienen coeficientes idénticos en Regresión Logística (4.73) porque son la misma variable en diferente escala (`CuentasDiarias = Monthly / 30`). Esto indica multicolinealidad — en un pipeline de producción se debería eliminar una de las dos.

---

## 6. Estrategias de Retención Recomendadas

### 1. Programa de fidelización por antigüedad
Los primeros meses son críticos. Implementar onboarding activo con beneficios progresivos para clientes nuevos (0–12 meses). Ofrecer descuentos o upgrades al alcanzar 6, 12 y 24 meses.

### 2. Revisión de precios en Fibra Óptica
Los clientes de fibra óptica cancelan significativamente más. Revisar la relación calidad-precio del servicio, ofrecer planes con mayor valor percibido o descuentos de permanencia.

### 3. Incentivos para contratos de largo plazo
Los contratos de 2 años reducen drásticamente el churn. Crear ofertas atractivas para migrar clientes de month-to-month a contratos anuales o bianuales con beneficios tangibles.

### 4. Atención proactiva en método de pago electrónico
El pago por cheque electrónico es el mayor predictor de churn. Investigar si existe fricción en el proceso de pago y simplificarlo, o incentivar migración a débito automático.

### 5. Bundle de servicios de seguridad y soporte
`OnlineSecurity` y `TechSupport` reducen cancelaciones. Incluirlos como valor agregado en planes básicos o ofrecer períodos de prueba gratuitos para aumentar adopción.

### 6. Modelo de alerta temprana en producción
Implementar el modelo Random Forest para identificar mensualmente los clientes con mayor probabilidad de churn y activar protocolos de retención personalizados.

---

## 7. Conclusión

El análisis predictivo de churn de Telecom X reveló que la cancelación no es un evento aleatorio, sino un fenómeno altamente predecible con patrones claros. El modelo Random Forest logró un **84.9% de accuracy**, identificando que los factores más determinantes son la antigüedad del cliente, el gasto acumulado, el tipo de contrato y el método de pago.

**Perfil del cliente en riesgo:** cliente reciente (menos de 12 meses), con servicio de fibra óptica, contrato mensual, facturación digital y pago por cheque electrónico.

**Perfil del cliente fiel:** contratos de 2 años, mayor antigüedad, servicios adicionales como seguridad online y soporte técnico.

Se recomienda implementar el modelo en producción como sistema de alerta temprana, combinado con las estrategias de retención propuestas, con foco especial en los **primeros 12 meses del ciclo de vida del cliente**, que es cuando el riesgo de abandono es más elevado.

---

*Telecom X — Análisis de Churn — Marzo 2026*