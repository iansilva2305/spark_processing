# Demostración de MapReduce

## Preparación del Entorno

### 1. Creación de directorios
Crear los siguientes directorios a partir del directorio de trabajo del usuario:
```bash
mkdir -p ~/hadoop/mr/data
mkdir -p ~/hadoop/mr/scripts
```

### 2. Gestión de archivos de datos
Copiar y descomprimir el archivo de datos:
```bash
unzip ~/hadoop/mr/datos_aleatorios.zip -d ~/hadoop/mr/data
```

Comprobar la carpeta destino de la extracción:
```bash
ls -ls ~/hadoop/mr/data
```

### 3. Carga de datos en HDFS
Crear directorio en HDFS y cargar los archivos:
```bash
hdfs dfs -mkdir -p /user/training/hadoop/mr/input
hdfs dfs -put ~/hadoop/mr/data/* hadoop/mr/input
```

Verificar el contenido:
```bash
hdfs dfs -ls -C /user/training/hadoop/mr/input
```

## Procesos y configuración de YARN

### 4. Verificación de versiones
Mostrar versiones de Hadoop y YARN:
```bash
hadoop version
yarn version
```

### 5. Procesos Java
Visualizar procesos Java relacionados con YARN:
```bash
sudo -u yarn jps | grep -v -i jps
```

### 6. Acceso a Cloudera Manager
Acceder mediante la URL:
```
http://localhost:7180
```

## Creación y Ejecución de la aplicación MapReduce

### 7. Script de Map
Crear script Python para el proceso Map:
```bash
vi ~/hadoop/mr/scripts/datos_map.py
```

### 8. Ejecución del Map
Asignar permisos y ejecutar:
```bash
chmod u+x ~/hadoop/mr/scripts/datos_map.py
cat ~/hadoop/mr/data/* | ~/hadoop/mr/scripts/datos_map.py
```

### 9. Comparación de datos
```bash
tail -n 5 ~/hadoop/mr/data/*
```

### 10. Script de Reduce
Crear y verificar script de Reduce:
```bash
cat ~/hadoop/mr/scripts/datos_reduce.py
```

### 11. Ejecución del Reduce
Preparar y ejecutar el proceso Reduce:
```bash
chmod u+x ~/hadoop/mr/scripts/datos_reduce.py
cd ~/hadoop/mr
cat ./data/* | ./scripts/datos_map.py > /tmp/datos_map.txt
cat /tmp/datos_map.txt | sort | ./scripts/datos_reduce.py
```

### 12. Ejecución conjunta
Ejecutar Map y Reduce en una sola línea:
```bash
cd ~/hadoop/mr
cat ./data/* | ./scripts/datos_map.py | sort | ./scripts/datos_reduce.py
```

## Ejecución en Hadoop

### 13. Ejecución con Hadoop Streaming
```bash
hadoop jar /opt/cloudera/parcels/CDH-7.1.7-1.cdh7.1.7.p0.15945976/lib/hadoop-mapreduce/hadoop-streaming-3.1.1.7.1.7.0-551.jar \
-files scripts/datos_map.py,scripts/datos_reduce.py \
-mapper datos_map.py \
-reducer datos_reduce.py \
-input /user/training/hadoop/mr/input \
-output /user/training/hadoop/mr/output
```

### 14. Visualización de resultados
```bash
hdfs dfs -ls -C /user/training/hadoop/mr/output
```

### 15. Información de la aplicación
```bash
yarn application -list -appTypes MAPREDUCE -appStates FINISHED
```

### 16. Visualización en Cloudera Manager
Acceder a:
```
http://localhost:7180
```

### 17. Job History
Acceder a:
```
http://localhost.localdomain:19888/jobhistory/job/job_1739435876247_0001
```

## Comparación de resultados

### 18. Resultados de Hadoop
```bash
hdfs dfs -cat hadoop/mr/output/* | sort > ~/hadoop/mr/mr_hadoop.txt
```

### 19. Resultados locales
```bash
cd ~/hadoop/mr
cat ./data/* | ./scripts/datos_map.py | sort | ./scripts/datos_reduce.py > ~/hadoop/mr/mr_local.txt
```

### 20. Comparación final
```bash
cd ~/hadoop/mr
diff ./mr_hadoop.txt ./mr_local.txt
```
