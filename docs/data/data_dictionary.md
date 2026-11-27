# Diccionario de datos

## Conjunto de datos: EuroSAT (RGB)

Cada observación corresponde a un parche de imagen satelital Sentinel-2 almacenado en disco y organizado por carpeta según su etiqueta de uso del suelo.

| Variable | Descripción | Tipo de dato | Rango/Valores posibles | Fuente de datos |
| --- | --- | --- | --- | --- |
| `image_path` | Ruta del archivo de imagen `.jpg` dentro de la carpeta de su clase | `string` | `data/raw/EuroSAT_RGB/<clase>/<archivo>.jpg` | EuroSAT RGB (Zenodo) |
| `image_rgb` | Matriz de píxeles de la imagen a color (alto × ancho × canales) | `uint8` (array `64x64x3`) | `0` a `255` por canal (R, G, B) | Sentinel-2 / EuroSAT RGB |
| `width` | Ancho de la imagen en píxeles | `int` | `64` | Metadatos de imagen |
| `height` | Alto de la imagen en píxeles | `int` | `64` | Metadatos de imagen |
| `channels` | Número de canales de color | `int` | `3` (RGB) | Metadatos de imagen |
| `label` | Clase de uso del suelo (variable objetivo, definida por el nombre de la subcarpeta) | `string` / categórica | 10 clases (ver tabla siguiente) | EuroSAT RGB (Zenodo) |

## Clases de la variable objetivo (`label`)

| Clase | Descripción | Cantidad aproximada |
| --- | --- | --- |
| `AnnualCrop` | Cultivos anuales | 3,000 |
| `Forest` | Zonas boscosas | 3,000 |
| `HerbaceousVegetation` | Vegetación herbácea | 3,000 |
| `Highway` | Autopistas y vías principales | 2,500 |
| `Industrial` | Zonas industriales | 2,500 |
| `Pasture` | Pastizales | 2,000 |
| `PermanentCrop` | Cultivos permanentes | 2,500 |
| `Residential` | Zonas residenciales | 3,000 |
| `River` | Ríos y cauces de agua | 2,500 |
| `SeaLake` | Mares y lagos | 3,000 |
