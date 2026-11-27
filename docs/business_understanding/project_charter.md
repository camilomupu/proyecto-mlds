# Project Charter - Entendimiento del Negocio

## Nombre del Proyecto

Clasificación de uso del suelo con imágenes satelitales (EuroSAT)

## Objetivo del Proyecto

Desarrollar un modelo de visión por computador que, dada una imagen satelital pequeña (un parche de 64x64 píxeles tomado por el satélite Sentinel-2), identifique automáticamente qué tipo de uso del suelo muestra: cultivos, bosque, zona residencial, industrial, río, etc. El proyecto incluye dejar el modelo documentado, versionado y desplegable siguiendo la metodología TDSP.

**Problema de negocio.** Conocer cómo se usa el territorio es la base de muchas decisiones: planeación urbana, seguimiento de cultivos, control de la expansión de las ciudades, protección de bosques y fuentes de agua. Los satélites producen imágenes de toda la superficie con mucha frecuencia, pero clasificarlas a mano es lento y costoso, y por eso los mapas de uso del suelo se desactualizan.

**Por qué es importante.** Un clasificador automático permite:

- Actualizar mapas de uso del suelo con mucha más frecuencia y a menor costo.
- Detectar cambios (por ejemplo, un bosque que pasa a ser zona de cultivo o una zona agrícola que pasa a ser residencial) comparando la clasificación de la misma zona en fechas distintas.
- Apoyar a quien analiza el territorio (planeación, agricultura, medio ambiente) con una primera clasificación que luego se revisa, en lugar de empezar de cero.
- Aplicar un criterio uniforme sobre grandes extensiones de terreno.

## Alcance del Proyecto

### Incluye:

- **Datos disponibles:** el conjunto público *EuroSAT* (Helber et al., 2019), versión RGB: 27 000 imágenes a color de 64x64 píxeles tomadas por Sentinel-2 sobre ciudades de Europa, cada una con su clase. Hay 10 clases: `AnnualCrop`, `Forest`, `HerbaceousVegetation`, `Highway`, `Industrial`, `Pasture`, `PermanentCrop`, `Residential`, `River` y `SeaLake`. Entre 2000 y 3000 imágenes por clase, es decir, un balance razonable (la clase mayoritaria es el 11.1 % del total).
- **Resultados esperados:**
  - Un clasificador de imágenes (modelo base de red convolucional y un modelo con transferencia de aprendizaje) entrenado y evaluado sobre EuroSAT.
  - Un flujo reproducible: código en `src/` y `scripts/`, datos versionados y experimentos registrados.
  - Un despliegue de demostración (no producción) que reciba una imagen y devuelva su clase. Usaremos Google Colab para ejecutar MLflow y registrar los experimentos y modelos. Para acceder a la interfaz web de MLflow, expondremos el puerto local del servidor (`127.0.0.1:5000`) con `ngrok` mediante la librería `pyngrok`, lo que nos dará una URL pública temporal para consultar el tablero desde el navegador. La demo será un notebook o una app sencilla que cargue el modelo y prediga.
  - Documentación de cada fase según la plantilla TDSP (`docs/`).
- **Criterios de éxito** (propuestos; se ajustarán cuando existan los modelos base):
  - Superar con holgura el modelo trivial que siempre responde la clase mayoritaria (exactitud de 11.1 %).
  - Exactitud de al menos 95 % y F1 macro de al menos 0.95 en un conjunto de prueba que no se use para entrenar ni para ajustar el modelo. Como referencia, los autores del conjunto reportan 98.57 % con redes convolucionales profundas.
  - Recall de al menos 0.90 en **cada** clase, no solo en promedio.
  - Analizar la matriz de confusión: se esperan errores entre clases visualmente parecidas (por ejemplo `Highway` y `River`, o los distintos tipos de cultivo y de vegetación).
  - Que cualquier resultado reportado se pueda reproducir a partir del repositorio.

### Excluye:

- Segmentación (delimitar zonas dentro de una imagen grande): el modelo clasifica un parche completo con una sola etiqueta.
- Uso del suelo fuera de Europa: el modelo se entrena con imágenes europeas y no se garantiza que funcione en otros paisajes. Tampoco se evalúa con imágenes de otros satélites.
- Las bandas espectrales adicionales de Sentinel-2 (la versión multiespectral del conjunto, con 13 bandas); se trabaja con la versión RGB.
- Series de tiempo y detección de cambios; se mencionan solo como aplicación posible.
- Decisiones oficiales o legales sobre el territorio: el modelo es una ayuda para el análisis y sus resultados deben validarse.
- Un despliegue real en producción (con monitoreo, alta disponibilidad y mantenimiento continuo); el despliegue será una demostración académica con MLflow y un notebook o app sencilla.

## Metodología

Se sigue el ciclo de vida de **Team Data Science Process (TDSP)**, con las mismas etapas de CRISP-DM: entendimiento del negocio, entendimiento y preparación de los datos, modelamiento, despliegue y evaluación.

- **Datos:** análisis exploratorio de las imágenes (balance de clases, ejemplos por clase, estadísticas de color) y preparación (normalización de los píxeles y aumento de datos). El aumento de datos (data augmentation) puede incluir volteos horizontales y verticales, rotaciones de 90°, recortes pequeños y cambios suaves de brillo o contraste. Se aplica solo al conjunto de entrenamiento; validación y prueba se dejan sin transformaciones aleatorias, y se evitan cambios que alteren el significado de la clase. Para medir su efecto, entrenaremos el mismo modelo con y sin aumento de datos, usando la misma partición, arquitectura y configuración principal, y compararemos las métricas en validación y prueba.
- **Partición:** conjuntos de entrenamiento, validación y prueba estratificados por clase y con semilla fija. Los parches que vienen de la misma zona pueden parecerse entre sí; si quedan repartidos entre entrenamiento y prueba, la exactitud medida puede salir más alta que la real. Esto se tendrá en cuenta al interpretar los resultados.
- **Modelamiento:** partir de una red convolucional sencilla y compararla con transferencia de aprendizaje (ajuste fino de una red preentrenada).
- **Reproducibilidad:** `git` para el código, `DVC` para los datos y `MLflow` (dentro de Colab) para registrar experimentos, métricas y modelos.
- **Trabajo semanal:** varios commits por semana, de cualquiera de los dos integrantes, para fomentar el trabajo colaborativo y equitativo.

## Cronograma

| Semana | Etapa | Entregable |
|------|---------|-------|
| 1 | Entendimiento del negocio y carga de datos | Project charter, código de carga y diccionario de datos |
| 2 | Preprocesamiento y análisis exploratorio | EDA, limpieza, aumento de datos y partición de entrenamiento, validación y prueba |
| 3 | Modelamiento y extracción de características | Modelo base, transferencia de aprendizaje y primeras métricas |
| 4 | Despliegue | Demo en Colab/MLflow, modelo registrado y documentación del despliegue |
| 5 | Evaluación y entrega final | Evaluación sobre el conjunto de prueba, informe, documentación TDSP y presentación |

El límite semanal para registrar los avances es el jueves a las 12:00 p.m.

## Equipo del Proyecto

- **Daniel Camilo Muñoz Puentes**
- **Daniel Felipe Pacheco Monroy**

El equipo está conformado por dos integrantes que participarán de forma transversal en todas las fases del proyecto. Ambos compartiremos la responsabilidad de los roles, incluido el liderazgo, y nos repartiremos las tareas de coordinación, datos, modelamiento, despliegue y documentación. La idea es que los dos tengamos apropiación del proyecto y podamos explicar y defender cualquier parte de él.

Los roles de TDSP que cubriremos entre los dos son:

- **Líder de proyecto (compartido):** coordina el avance, prioriza tareas y vela por las entregas semanales.
- **Científico de datos:** diseña los experimentos, entrena los modelos y analiza los resultados.
- **Ingeniero de datos:** se encarga de la carga, limpieza, partición y versionado de los datos.
- **Arquitecto de soluciones:** define cómo se conectan los datos, el modelo y la demo, y documenta esa arquitectura.
- **Responsable de reproducibilidad y MLOps:** mantiene git, DVC, MLflow y la documentación de experimentos.

Estos roles no son cargos fijos: los dos los compartimos y nos los repartimos según la fase, con la idea de que ambos tengamos dominio completo del proyecto y podamos participar en cualquier parte.

## Presupuesto

En el contexto del curso, el proyecto no tiene presupuesto monetario asignado: los recursos son gratuitos (Google Colab con GPU para el cómputo, GitHub para el repositorio y datasets públicos). El costo real para el equipo es $0.

Para dimensionar lo que costaría si el proyecto se ofreciera a un cliente, usaremos como referencia valores estimados:

| Categoría | Descripción | Costo estimado (USD) | Notas |
| :----------------- | :---------- | :------------------- | :---------------------- |
| **I. Personal** | | | |
| Científico de datos senior | Diseño del modelo, experimentos y evaluación | $6,000 | 5 semanas a tiempo parcial |
| Ingeniero de ML | Datos, entrenamiento, demo y documentación | $4,000 | 5 semanas a tiempo completo |
| Gestión de proyecto | Coordinación, reuniones y entregas | $800 | Tiempo parcial |
| **II. Infraestructura y software** | | | |
| GPU en la nube | Entrenamientos y experimentos | $400 | ~100 horas de GPU |
| Almacenamiento | Dataset, checkpoints y artefactos | $30 | 100 GB por 2 meses |
| Licencias | Herramientas y librerías | $0 | Todo open source (Python, PyTorch/TensorFlow, scikit-learn) |
| **III. Misceláneos** | | | |
| Investigación y pruebas | Lectura, pruebas de concepto y ajustes | $500 | Horas de exploración |
| Contingencia | Imprevistos (aprox. 10 %) | $1,173 | Buffer para gastos inesperados |
| **TOTAL ESTIMADO DEL PROYECTO** | | **$12,903** | |

Si se quisiera vender, este presupuesto serviría como base para cotizar; habría que sumar el margen de la empresa y, si aplica, el costo de validación con usuarios.
