# Apache Airflow  with Java, Scala and Python Spark Jobs

Este proyecto coordina trabajos de Spark escritos en diferentes lenguajes de programación utilizando Apache Airflow, todo dentro de un entorno Dockerizado. El DAG `sparking_flow` está diseñado para enviar trabajos de Spark escritos en Python, Scala y Java, garantizando que el procesamiento de datos se realice de manera eficiente y confiable según un horario diario.

## Estructura del proyecto

El DAG `sparking_flow` incluye las siguientes tareas:

- `start`: Un PythonOperator que imprime "Trabajos iniciados".
- `python_job`: Un SparkSubmitOperator que envía un trabajo de Spark escrito en Python.
- `scala_job`: Un SparkSubmitOperator que envía un trabajo de Spark escrito en Scala.
- `java_job`: Un SparkSubmitOperator que envía un trabajo de Spark escrito en Java.
- `end`: Un PythonOperator que imprime "Trabajos completados exitosamente".

Estas tareas se ejecutan en secuencia, donde la tarea `start` activa los trabajos de Spark en paralelo, y al completarse, se ejecuta la tarea `end`.

## Prerequisitos

Antes de configurar el proyecto, asegúrate de tener lo siguiente:

- Docker y Docker Compose instalados en tu sistema.
- Imagen de Docker de Apache Airflow o una imagen personalizada con Airflow instalado.
- Imagen de Docker de Apache Spark o una imagen personalizada con Spark instalado y configurado para funcionar con Airflow.
- Volúmenes de Docker para DAGs de Airflow, logs y trabajos de Spark correctamente configurados.

## Configuración de Docker

Para ejecutar este proyecto utilizando Docker, sigue estos pasos:

1. Clona este repositorio en tu máquina local.
2. Navega al directorio que contiene el archivo `docker-compose.yml`.
3. Construye y ejecuta los contenedores utilizando Docker Compose:

```bash
docker-compose up -d --build
```

Este comando iniciará los servicios necesarios definidos en tu archivo docker-compose.yml, como el servidor web de Airflow, el planificador, el maestro de Spark y los contenedores de trabajo.

## Estructura de Directorios para Trabajos

Asegúrate de que tus archivos de trabajo de Spark estén ubicados en los siguientes directorios y sean accesibles para el contenedor de Airflow:

- Trabajo en Python: jobs/python/wordcountjob.py
- Trabajo en Scala: jobs/scala/target/scala-2.12/word-count_2.12-0.1.jar
- Trabajo en Java: jobs/java/spark-job/target/spark-job-1.0-SNAPSHOT.jar

Estas rutas deben ser relativas al volumen Docker montado para los DAGs de Airflow.

## Uso

Después de configurar el entorno Docker, el DAG `sparking_flow` estará disponible en la interfaz web de Airflow [localhost:8080](localhost:8080), donde se puede activar manualmente o ejecutar según su programación diaria.

### El DAG ejecutará los siguientes pasos

- Imprimir "Trabajos iniciados" en los logs de Airflow.
- Enviar el trabajo de Spark en Python al clúster de Spark.
- Enviar el trabajo de Spark en Scala al clúster de Spark.
- Enviar el trabajo de Spark en Java al clúster de Spark.
- Imprimir "Trabajos completados exitosamente" en los logs de Airflow después de que todos los trabajos hayan terminado.

### Nota

Debes agregar la URL del clúster de Spark a la conexión de Spark en la configuración de la interfaz de usuario de Airflow.
