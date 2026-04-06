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
| Fase 2 | Modelo PD — Logistic Regression + XGBoost | ✅ Completado |
| Fase 3 | Estimación LGD + Expected Loss | ⏳ Pendiente |
| Fase 4 | Simulación Monte Carlo + VaR 99% | ⏳ Pendiente |
| Fase 5 | Stress Testing macroeconómico | ⏳ Pendiente |
| Fase 6 | Informe técnico tipo risk report | ⏳ Pendiente |

---

## Hallazgos principales — Fase 2

Modelos entrenados sobre 451.805 préstamos (2007-2016) y evaluados sobre 97.761 (2017-2018). Split temporal para evitar data leakage.

### Métricas de evaluación

| Modelo | ROC-AUC | KS Statistic | Gini | Brier |
|--------|---------|--------------|------|-------|
| Logística Baseline | 0.6876 | 0.2736 | 0.3753 | 0.2173 |
| XGBoost | 0.6955 | 0.2841 | 0.3903 | 0.2210 |

> Nota: accuracy no se usa como métrica principal dado el desbalance de clases (78.5% no-default / 21.5% default). Las métricas relevantes en banca son AUC, KS y Gini.

### Scorecard — Escala 300-850

La regresión logística se convierte en scorecard bancaria con escala estilo FICO (PDO=20, score de referencia=600):

| Tramo | n | Default rate |
|-------|---|--------------|
| 500-550 | 1.280 | 57.3% |
| 550-600 | 38.506 | 39.7% |
| 600-650 | 55.216 | 19.0% |
| 650-700 | 2.752 | 4.8% |
| 700-750 | 1 | 0.0% |

Monotonía perfecta validada — requerimiento regulatorio Basilea III IRB.

### Top features — SHAP Analysis

| Feature | SHAP medio | Dirección |
|---------|-----------|-----------|
| sub_grade_enc | 0.29 | ↑ riesgo |
| term_enc | 0.23 | ↑ riesgo (60 meses) |
| grade_dti | 0.17 | ↑ riesgo (interaction) |
| fico_range_low | 0.11 | ↓ riesgo |
| int_rate | 0.10 | ↑ riesgo |
| mort_acc | 0.07 | ↓ riesgo |

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
│   ├── CR360_fase1.ipynb                ✅ Análisis exploratorio
│   ├── CR360_fase2_1_Prep.ipynb         ✅ Preprocesamiento y encoding
│   ├── CR360_fase2_2_model.ipynb        ✅ Logística + XGBoost
│   ├── CR360_fase2_3_Eval.ipynb         ✅ ROC-AUC, KS, Gini, calibración
│   ├── CR360_fase2_4_Shap.ipynb         ✅ Interpretabilidad SHAP
│   ├── CR360_fase2_5_Scorecard.ipynb    ✅ Scorecard bancaria 300-850
│   ├── 03_LGD_EL.ipynb                  ⏳ LGD y Expected Loss
│   ├── 04_MonteCarlo.ipynb              ⏳ Simulación Monte Carlo
│   └── 05_StressTesting.ipynb           ⏳ Stress testing
│
├── data/
│   ├── creditrisk360_eda.parquet  (generado localmente)
│   ├── train_processed.parquet    (generado localmente)
│   └── test_processed.parquet     (generado localmente)
│
├── models/
│   ├── lr_baseline.pkl
│   └── xgb_creditrisk.pkl
│   
├── outputs/
│   ├── creditrisk360_default_by_grade.png
│   ├── creditrisk360_dti_analysis.png
│   ├── creditrisk360_fico_analysis.png
│   ├── creditrisk360_purpose_analysis.png
│   ├── creditrisk360_temporal_analysis.png
│   ├── scorecard_coeficientes.png
│   ├── scorecard_distribution.png
│   ├── semana2_evaluation.png
│   ├── shap_beeswarm.png
│   ├── shap_dependence_subgrade.png
│   ├── shap_importance.png
│   ├── shap_waterfall_high.png
│   └── shap_waterfall_low.png
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
jupyter notebook notebooks/CR360_fase1.ipynb
jupyter notebook notebooks/CR360_fase2_1_Prep.ipynb
jupyter notebook notebooks/CR360_fase2_2_model.ipynb 
jupyter notebook notebooks/CR360_fase2_3_Eval.ipynb 
jupyter notebook notebooks/CR360_fase2_4_Shap.ipynb 
jupyter notebook notebooks/CR360_fase2_5_Scorecard.ipynb
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