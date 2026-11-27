# 🛰️ Clasificación de Uso del Suelo con Imágenes Satelitales (EuroSAT)

Proyecto académico del curso **Metodologías Ágiles para el Desarrollo de Proyectos de Machine Learning** (Universidad Nacional de Colombia), desarrollado bajo el marco **TDSP (Team Data Science Process)**.

## 🎯 Objetivo

Desarrollar, documentar y desplegar un modelo de visión por computador capaz de clasificar imágenes satelitales **Sentinel-2** (parches RGB de 64×64 píxeles) en **10 categorías de uso del suelo**:

`AnnualCrop` · `Forest` · `HerbaceousVegetation` · `Highway` · `Industrial` · `Pasture` · `PermanentCrop` · `Residential` · `River` · `SeaLake`

## 📊 Datos

- **Dataset:** [EuroSAT RGB](https://zenodo.org/record/7711810) (Helber et al., 2019)
- **Volumen:** 27,000 imágenes `.jpg` de 64×64×3 (entre 2,000 y 3,000 imágenes por clase)

## 🛠️ Stack y Reproducibilidad

- **Entorno y Cómputo:** Python · Google Colab (GPU)
- **Versionado:** `Git` (código y documentación) · `DVC` (datos)
- **Seguimiento de Experimentos:** `MLflow`

## 📁 Estructura del Proyecto

- `docs/` — Documentación TDSP por fase (`business_understanding/`, `data/`, `modeling/`, `deployment/`, `acceptance/`).
- `scripts/` — Notebooks y scripts ejecutables por etapa (`data_acquisition/`, `eda/`, `preprocessing/`, `training/`, `evaluation/`).
- `src/` — Código fuente modular del paquete en Python.
- `pyproject.toml` — Definición y dependencias del proyecto.

## 👥 Integrantes

- **Daniel Camilo Muñoz Puentes**
- **Daniel Felipe Pacheco Monroy**
