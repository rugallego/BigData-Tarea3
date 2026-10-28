# UNAD
## Ruben Dario Gallego Tellez
## BigData Tarea3
# Pipeline de Procesamiento Batch con PySpark: Análisis del PIB en Colombia

Este repositorio contiene la solución en batch para la ingesta, limpieza, estandarización y análisis exploratorio de datos (EDA) del Producto Interno Bruto (PIB) departamental y sectorial en Colombia mediante **Apache Spark (PySpark)** y **Hadoop HDFS**.

## 🚀 Arquitectura y Solución

La arquitectura sigue una canalización batch distribuida organizada en las siguientes fases:

1. **Ingesta desde HDFS**: Carga del conjunto de datos `query.csv` almacenado en la ruta distribuida `hdfs://localhost:9000/Tarea32/query.csv` infiriendo tipos y encabezados.
2. **Limpieza y Estandarización (Spark DataFrames)**:
   - Conversión explícita de tipos de datos (`int`, `string`, `double`).
   - Normalización de texto y eliminación de espacios en blanco mediante `trim()`.
   - Filtrado de valores atípicos o nulos (`valor_pib > 0` y registros válidos por departamento).
   - Imputación de datos faltantes para sectores y actividades económicas.
   - **Persistencia (`cache`)**: Almacenamiento en memoria para optimizar la velocidad de ejecución de las consultas analíticas.
3. **Análisis Exploratorio de Datos (EDA)**:
   - **Filtrado y proyección de alto impacto**: Identificación de registros con PIB superior a $2 billones de COP.
   - **Agregación por DataFrames**: Consolidado del PIB acumulado por departamento.
   - **Paradigma MapReduce con RDDs**: Implementación distribuida mediante `map()` y `reduceByKey()` para computar el conteo de registros y la suma agregada del PIB por cada sector económico.

---

## 🛠️ Requisitos Previos

- **Java JDK** 8 u 11
- **Hadoop** 3.x con servicios HDFS activos (NameNode en `localhost:9000`)
- **Apache Spark** 3.x / **PySpark**
- **Python** 3.8+

---

## 📋 Instrucciones de Ejecución

### 1. Iniciar servicios HDFS y crear estructura de directorios
```bash
start-dfs.sh
hdfs dfs -mkdir -p /Tarea32
