# CreditRisk360
### Sistema profesional de medición de riesgo crediticio
**Rolando Rodriguez** | Analista Cuantitativo | Bilbao, España  
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Rolando_Stiwan_Rodriguez-blue)](https://www.linkedin.com/in/rolando-stiwan-rodriguez/)

---

## Descripción

CreditRisk360 es un sistema completo de medición de riesgo 
crediticio construido sobre datos reales de LendingClub 
(2007-2018), estructurado como lo haría un equipo de riesgo 
en una entidad financiera.

El proyecto cubre el ciclo completo de riesgo crediticio:
desde el análisis exploratorio hasta la simulación Monte Carlo 
de pérdidas y el stress testing macroeconómico.

---

## Arquitectura del proyecto
```
PD Model → LGD Estimation → Expected Loss → Monte Carlo → Stress Testing
```

| Fase | Contenido | Estado |
|------|-----------|--------|
| Fase 1 | EDA profesional — análisis de cartera | ✅ Completado |
| Fase 2 | Modelo PD — Logistic Regression + XGBoost | 🔄 En progreso |
| Fase 3 | Estimación LGD + Expected Loss | ⏳ Pendiente |
| Fase 4 | Simulación Monte Carlo + VaR 99% | ⏳ Pendiente |
| Fase 5 | Stress Testing macroeconómico | ⏳ Pendiente |
| Fase 6 | Informe técnico tipo risk report | ⏳ Pendiente |

---

## Hallazgos principales — Fase 1

Análisis sobre 549.566 préstamos reales (2007-2018):

**Tasa de default de la cartera: 21.5%**

| Variable | Hallazgo |
|----------|----------|
| Grade | Progresión monotónica perfecta: 6.7% (A) → 50.4% (G) |
| DTI | Relación monotónica, se acelera a partir de DTI > 15% |
| FICO | Rango 8.5% (770+) → 28.8% (640-660) con sesgo de selección |
| Propósito | Small business lidera el riesgo con 31.3% |
| Temporal | Efecto vintage: vintages crisis (2009) más seguros que expansión (2017) |

---

## Dataset

**LendingClub Loan Data 2007-2018**  
Fuente: [Kaggle — Loan Data](https://www.kaggle.com/datasets/adarshsng/lending-club-loan-data-csv)  
Tamaño original: 1.55GB — 2.26M préstamos — 151 variables  
Muestra utilizada: 549.566 préstamos (40% estratificado) — 37 variables seleccionadas. Se realizo la estratificación por limitaciones de infraestructura.

> El dataset no está incluido en el repositorio por su tamaño.
> 

---

## Stack tecnológico
```
Python 3.12+
pandas · numpy · scikit-learn
matplotlib · seaborn
xgboost · shap
pyarrow (parquet)
jupyter notebook
```

---

## Estructura del repositorio
```
creditrisk360/
│
├── notebooks/
│   ├── 01_EDA.ipynb              ✅ Análisis exploratorio
│   ├── 02_PD_model.ipynb         🔄 Modelo de probabilidad de default
│   ├── 03_LGD_EL.ipynb           ⏳ LGD y Expected Loss
│   ├── 04_MonteCarlo.ipynb       ⏳ Simulación Monte Carlo
│   └── 05_StressTesting.ipynb    ⏳ Stress testing
│
├── data/
│   └── creditrisk360_eda.parquet  (generado localmente)
│
├── outputs/
│   ├── creditrisk360_default_by_grade.png
│   ├── creditrisk360_dti_analysis.png
│   ├── creditrisk360_fico_analysis.png
│   ├── creditrisk360_purpose_analysis.png
│   └── creditrisk360_temporal_analysis.png
│
├── README.md
```

---

## Cómo ejecutar
```bash
# 1. Clonar el repositorio
git clone https://github.com/Rolando-Stiwan/creditrisk360

# 2. Descargar el dataset
kaggle datasets download (Ver seccion dataset)

# 3. Ejecutar notebooks en orden
jupyter notebook notebooks/01_EDA.ipynb
```

---

## Contexto profesional

Este proyecto forma parte de mi transición hacia roles de 
riesgo cuantitativo en banca y fintech. Aplica conceptos del 
Máster en Ciencias Actuariales y Financieras (UPV/EHU) junto 
con desarrollo práctico en modelización estadística y Python.

**Áreas de aplicación:**
- Credit Risk Analyst
- Quantitative Risk
- Actuarial / Solvencia II
- Data Scientist en banca

---

*Proyecto en construcción  
*Contacto: [LinkedIn](https://www.linkedin.com/in/rolando-stiwan-rodriguez/)*