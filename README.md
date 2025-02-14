# Procesamiento de datos con Spark

## 1. Creación de directorios
Crear los siguientes directorios locales a partir del directorio de trabajo del usuario:

```bash
mkdir -p ~/hadoop/spark/data
mkdir -p ~/hadoop/spark/scripts
```

## 2. Copia de archivos
Utilizando el método que consideres oportuno, copia el archivo countries.json en el directorio `/user/training/hadoop/spark/data`.
Comprueba el directorio destino de la copia:

```bash
ls -l ~/hadoop/spark/data
```

## 3. Carga de archivos en HDFS
Crear previamente un directorio en HDFS y cargar el archivo:

```bash
hdfs dfs -mkdir -p /user/training/hadoop/spark/input
hdfs dfs -put ~/hadoop/spark/data/* hadoop/spark/input
```

Comprobar el contenido del directorio:
```bash
hdfs dfs -ls /user/training/hadoop/spark/input
```

## Uso interactivo de la shell de Spark

### 4. Acceso a la shell de Spark
```bash
spark-shell
```

### 5. Carga de datos en DataFrame
```scala
val employeesDF = spark.read.json("/user/training/hadoop/spark/input/countries.json")
```

### 6. Visualización del schema
```scala
employeesDF.printSchema()
```

### 7. Visualización de datos
```scala
employeesDF.show(5)
```

### 8. Operaciones de transformación
Realizar agrupamiento por el campo "region-number" y calcular la cantidad de filas por grupo:

```scala
val statisticsDF = (employeesDF.groupBy("region-number")
                              .count()
                              .withColumnRenamed("count","total"))
```

### 9. Verificación de operaciones
```scala
statisticsDF.printSchema()
statisticsDF.show(5)
```

### 10. Grabación de datos
```scala
statisticsDF.coalesce(1).write.mode("overwrite").format("csv").save("/user/training/hadoop/spark/output1")
sys.exit()
```

### 11. Comprobación de resultados
```bash
hdfs dfs -ls -C /user/training/hadoop/spark/output1
hdfs dfs -cat /user/training/hadoop/spark/output1/* | head -n 5
```

## Ejecución de un script mediante la shell de Spark

### 12. Creación del script
Crear el archivo utilizando un editor de texto:

```bash
vim ~/hadoop/spark/scripts/datos_spark.scala
```

Contenido del script:
```scala
val countRDD = spark.sparkContext.textFile("/user/training/hadoop/hdfs/data/shakespeare.txt")
  .flatMap(line => line.split(' '))
  .map(word => (word,1))
  .reduceByKey((v1,v2) => v1 + v2)
  .sortBy(tuple => tuple._2, ascending=false)

countRDD.map(tuple => tuple._1 + "\t" + tuple._2)
  .coalesce(1)
  .saveAsTextFile("/user/training/hadoop/spark/output2/")

sys.exit()
```

### 13. Ejecución del script
```bash
spark-shell --name word_count -i ~/hadoop/spark/scripts/datos_spark.scala
```

### 14. Comprobación de resultados
```bash
hdfs dfs -ls -C /user/training/hadoop/spark/output2
hdfs dfs -cat /user/training/hadoop/spark/output2/* | head -n 5
```

### 15. Información de aplicaciones Spark
```bash
yarn application -list -appTypes SPARK -appStates FINISHED
```

### 16. Visualización en Cloudera Manager
Acceder a la siguiente URL para visualizar la información de las aplicaciones ejecutadas:
```
http://localhost:7180/
```
