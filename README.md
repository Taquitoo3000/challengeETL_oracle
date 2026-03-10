# Telecom X - ETL
Este proyecto extrae una base de datos desde un api, en la siguiente url

`
https://raw.githubusercontent.com/ingridcristh/challenge2-data-science-LATAM/main/TelecomX_Data.json
`

Para posteriormente ser leída y analizada
El archivo es un python notebook `TelecomX_LATAM.ipynb` en el cual se encuentra un diccionario en markdown `TelecomX_diccionario.md` en el cual viene una descripcion del significado de cada columna.

El archivo hace una análisis exploratorio de datos, un ETL, creando insights para la empresa para tomar acción en reparar los fenómenos aquí mostrados.

Finalmente genera un archivo `datos_tratados.csv` para ser analizados poseriormente con el pipeline en `pipeline.ipynb`

# Telecom X - Predicción de Cancelación de Clientes (Churn)

Proyecto de Machine Learning para predecir qué clientes tienen mayor probabilidad de cancelar sus servicios en Telecom X.

---

## Objetivo

Desarrollar un pipeline de modelado predictivo capaz de anticipar la cancelación de clientes, identificar los factores más influyentes y proponer estrategias de retención basadas en datos.

---

## Estructura del Proyecto

```
challenge_telecomX/
│
├── data/
│   └── datos_tratados.csv         # Dataset original
│
├── pipeline.ipynb       # Notebook principal con todo el análisis
├── TelecomX_LATAM.ipynb       # Notebook de ETL
│
├── TelecomX_diccionario.md     # Diccionario del dataframe
├── informe.md     # Informe final con conclusiones
├── requirements.txt              # Dependencias del proyecto
└── README.md
```

---

## Pipeline del Proyecto

1. **Carga y exploración** — revisión de tipos, nulos y distribuciones
2. **Preprocesamiento**
   - Eliminación de columnas irrelevantes (`customerID`)
   - One-Hot Encoding de variables categóricas
   - Imputación de valores nulos con la mediana
   - Balanceo de clases con **SMOTE**
   - Normalización con **StandardScaler**
3. **Análisis de correlación** — matriz de correlación y variables vs Churn
4. **Entrenamiento de modelos** — Regresión Logística y Random Forest
5. **Evaluación** — accuracy, precision, recall, F1-score, matriz de confusión
6. **Importancia de variables** — coeficientes LR y feature importance RF
7. **Conclusión estratégica** — factores clave y recomendaciones de retención

---

## Resultados

| Modelo | Accuracy | F1-Score | Overfitting |
|---|---|---|---|
| Regresión Logística | 83.09% | 83.17% | No |
| Random Forest | 84.93% | 85.06% | Leve |

### Variables más influyentes

| Factor | Efecto |
|---|---|
| `tenure` | Mayor antigüedad → menos churn |
| `Total` | Mayor gasto acumulado → menos churn |
| `Monthly` | Mayor costo mensual → más churn |
| `InternetService_Fiber optic` | Fibra óptica → más churn |
| `PaymentMethod_Electronic check` | E-check → más churn |
| `Contract_Two year` | Contrato largo → menos churn |

---

## Cómo ejecutar el proyecto

### 1. Clonar el repositorio
```bash
git clone https://github.com/tu-usuario/challenge_telecomX.git
cd challenge_telecomX
```

### 2. Crear entorno virtual e instalar dependencias
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 3. Abrir el notebook
```bash
jupyter notebook notebooks/telecom_churn.ipynb
```

---

## Tecnologías utilizadas

- Python 3.10
- pandas / numpy
- scikit-learn
- imbalanced-learn (SMOTE)
- matplotlib / seaborn
- Jupyter Notebook

---

## Informe

El informe completo con análisis, métricas y estrategias de retención está disponible en [`informe.md`](./informe.md).

---

*Challenge Data Science — Oracle Next Education — Marzo 2026*