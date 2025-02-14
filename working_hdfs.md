# Trabajando con HDFS

## Verificación del Entorno

### 1. Comprobación de servicios Cloudera
Verificar servicios en ejecución:
```bash
sudo systemctl | sed 's/^ *//g' | grep "^cloudera-scm"
```

### 2. Verificación de versiones
Version de Hadoop:
```bash
hadoop version
```

Version de HDFS:
```bash
hdfs version
```

### 3. Procesos Java
Mostrar procesos Java relacionados con HDFS:
```bash
sudo -u hdfs jps | grep -v -i jps
```

### 4. Acceso Web UI
Acceder a Cloudera Manager:
```
http://localhost:7180
```

## Configuración del Clúster

### 5. Topología
Comprobar la topología del clúster:
```bash
sudo -u hdfs hdfs dfsadmin -printTopology
```

### 6. Nodos Master
Mostrar NameNodes del clúster:
```bash
hdfs getconf -namenodes
```

### 7. Informe de nodos
Visualizar características de los nodos:
```bash
sudo -u hdfs hdfs dfsadmin -report
```

### 8. Tamaño de bloque
Comprobar tamaño de bloque por defecto:
```bash
hdfs getconf -confKey dfs.blocksize
expr $(hdfs getconf -confKey dfs.blocksize) / 1048576
```

## Gestión de Archivos

### 9. Descarga de archivos
```bash
mkdir -p ~/hadoop/hdfs/data
wget https://s3.amazonaws.com/sty-hadoop/shakespeare/shakespeare.txt -P ~/hadoop/hdfs/data
```

### 10. Copia a HDFS
```bash
hdfs dfs -put ~/hadoop/hdfs/data/shakespeare.txt /tmp/shakespeare1.txt
```

### 11. Verificación de archivos
```bash
hdfs dfs -ls -C /tmp/shakespeare*
hdfs dfs -stat "%n %b %o" /tmp/shakespeare*
```

### 12. Copia con tamaño de bloque personalizado
```bash
hdfs dfs -D dfs.blocksize=1048576 -put ~/hadoop/hdfs/data/shakespeare.txt /tmp/shakespeare2.txt
```

Verificación:
```bash
hdfs dfs -ls -C /tmp/shakespeare*
hdfs dfs -stat "%n %b %o" /tmp/shakespeare*
```

### 13. Información de bloques
```bash
hdfs fsck /tmp/shakespeare1.txt -files -blocks
hdfs fsck /tmp/shakespeare2.txt -files -blocks
```

## Replicación de Datos

### 14. Factor de replicación
```bash
hdfs getconf -confKey dfs.replication
```

### 15. Copia con replicación personalizada
```bash
cd ~/hadoop/hdfs/data
hdfs dfs -D dfs.replication=5 -put shakespeare.txt /tmp/shakespeare3.txt
```

### 16. Verificación de replicación
```bash
hdfs dfs -ls /tmp/shakespeare*
```

### 17. Chequeo de integridad
```bash
hdfs fsck /tmp/shakespeare3.txt
```

### 18. Modificación de replicación
```bash
hdfs dfs -setrep -R 1 /tmp/shakespeare3.txt
hdfs dfs -ls /tmp/shakespeare*
hdfs dfs -stat "%n %r %b %o" /tmp/shakespeare*
```

### 19. Verificación de integridad
```bash
hdfs fsck /tmp/shakespeare3.txt
```

### 20. Copia de archivos
```bash
hdfs dfs -cp /tmp/shakespeare3.txt /tmp/shakespeare4.txt
hdfs dfs -ls /tmp/shakespeare*
```

## Gestión de Papelera

### 21. Configuración Cloudera Manager
Acceder a:
```
http://localhost:7180/cmf/services/14/config#q=Tras
```

### 22. Eliminación con papelera
```bash
hdfs dfs -rm /tmp/shakespeare1.txt
hdfs dfs -ls /tmp/shakespeare*
hdfs dfs -ls -R -C /user/training/.Trash/Current
```

### 23. Eliminación sin papelera
```bash
hdfs dfs -rm -skipTrash /tmp/shakespeare2.txt
hdfs dfs -ls /tmp/shakespeare*
hdfs dfs -ls -R -C /user/training/.Trash/Current
```

### 24. Recuperación de archivos
```bash
hdfs dfs -mv .Trash/250212120000/tmp/shakespeare1.txt /tmp/shakespeare1.txt
hdfs dfs -ls /tmp/shakespeare*
hdfs dfs -ls -R -C /user/training/.Trash/250212120000/tmp
```

### 25-29. Configuración de la papelera
Configurar los parámetros de la papelera desde Cloudera Manager según las instrucciones específicas.

### 30. Organización de archivos
```bash
hdfs dfs -mkdir -p /user/training/hadoop/hdfs/input
hdfs dfs -mv /tmp/shakespeare1.txt /user/training/hadoop/hdfs/input
hdfs dfs -ls -C /user/training/hadoop/hdfs/input
```
