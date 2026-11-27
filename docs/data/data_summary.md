# Reporte de Datos

Este documento resume la información inicial del conjunto de datos EuroSAT (RGB) tras la fase de carga. El análisis exploratorio detallado (estadísticas de color, histogramas y visualización de muestras) se completará en la semana 2 (`scripts/eda/main.py`).

## Resumen general de los datos

- **Total de observaciones:** 27,000 imágenes satelitales.
- **Formato y dimensiones:** imágenes `.jpg` de 64×64 píxeles con 3 canales (RGB), lo que equivale a 12,288 valores numéricos por imagen en escala `[0, 255]`.
- **Variable objetivo:** clasificación multiclase con 10 categorías de uso del suelo.

## Resumen de calidad de los datos

- **Valores faltantes:** el dataset público no presenta etiquetas faltantes ni registros incompletos, ya que cada imagen está asignada a una subcarpeta de clase.
- **Integridad de formato:** en la fase de carga (`scripts/data_acquisition/carga_datos.ipynb`) se verifica el conteo de archivos por clase y que las imágenes cumplan con el tamaño esperado (64×64 píxeles en modo RGB).
- **Duplicados y valores atípicos:** se revisarán en detalle durante el EDA de la semana 2.

## Variable objetivo

La variable objetivo es `label` (10 clases de uso del suelo). Las clases tienen un balance razonable entre **2,000 y 3,000 imágenes** cada una (la clase mayoritaria representa aproximadamente el 11.1 % del total):

- 3,000 imágenes: `AnnualCrop`, `Forest`, `HerbaceousVegetation`, `Residential`, `SeaLake`
- 2,500 imágenes: `Highway`, `Industrial`, `PermanentCrop`, `River`
- 2,000 imágenes: `Pasture`

## Variables individuales

Al tratarse de imágenes, las variables de entrada no son columnas tabulares independientes sino los píxeles de los 3 canales (rojo, verde y azul) en una grilla espacial de 64×64. En la fase de preprocesamiento se normalizarán al rango `[0, 1]` (o según las estadísticas del modelo preentrenado a usar) y se aplicará aumento de datos solo sobre el conjunto de entrenamiento.

## Ranking de variables

No aplica un ranking de variables tabulares (como correlación o PCA sobre columnas), dado que la información relevante proviene de patrones espaciales y espectrales (texturas, formas y colores) que serán extraídos automáticamente por las redes neuronales convolucionales.

## Relación entre variables explicativas y variable objetivo

Se espera que ciertas clases compartan características visuales similares en RGB (por ejemplo, `AnnualCrop` frente a `PermanentCrop` o `HerbaceousVegetation`, y `Highway` frente a `River`), lo cual se analizará visualmente en el EDA y posteriormente mediante la matriz de confusión en la fase de modelamiento.
