# Definición de los datos

## Origen de los datos

El conjunto de datos utilizado es **EuroSAT** (Helber et al., 2019), en su versión **RGB**. Contiene 27,000 imágenes satelitales tomadas por el satélite Sentinel-2 sobre distintas zonas de Europa, etiquetadas en 10 clases de uso del suelo.

Los datos se obtienen de forma pública desde el repositorio oficial en Zenodo (`https://zenodo.org/record/7711810`, archivo `EuroSAT_RGB.zip`), con respaldo en Hugging Face Hub (`blanchon/EuroSAT_RGB`) en caso de que la descarga directa no esté disponible.

## Especificación de los scripts para la carga de datos

- `scripts/data_acquisition/carga_datos.ipynb`: notebook principal de carga (listo para ejecutar en Google Colab). Verifica si el dataset ya existe localmente en `data/raw/EuroSAT_RGB`; si no, descarga `EuroSAT_RGB.zip` desde Zenodo (o usa Hugging Face Hub como respaldo), descomprime los archivos, verifica el conteo por clase y dimensiones (`64x64` RGB) y muestra ejemplos de cada clase.

## Referencias a rutas o bases de datos origen y destino

### Rutas de origen de datos

- **Ubicación de los archivos de origen:** descarga directa desde Zenodo (`EuroSAT_RGB.zip`, ~94.7 MB) y almacenamiento local en `data/raw/EuroSAT_RGB/`.
- **Estructura de los archivos:** directorio raíz con 10 subcarpetas, una por cada clase de uso del suelo (`AnnualCrop`, `Forest`, `HerbaceousVegetation`, `Highway`, `Industrial`, `Pasture`, `PermanentCrop`, `Residential`, `River`, `SeaLake`). Cada subcarpeta contiene imágenes en formato `.jpg` de 64x64 píxeles en modo RGB.
- **Transformación y limpieza en esta etapa:** durante la carga no se alteran las imágenes originales; solo se comprueba la cantidad de archivos por carpeta y una muestra de dimensiones/canales. La normalización de píxeles, la partición estratificada (entrenamiento, validación y prueba) y el aumento de datos se realizarán en la siguiente fase (`scripts/preprocessing/main.py`).

### Base de datos de destino

- Este proyecto no utiliza una base de datos relacional ni un almacén de destino externo. Los datos se conservan en el sistema de archivos local (`data/raw/` para los datos crudos y `data/processed/` para las particiones preparadas en la siguiente fase), gestionados con `DVC` para garantizar la reproducibilidad.
