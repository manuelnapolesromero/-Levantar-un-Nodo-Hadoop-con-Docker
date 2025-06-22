# **Levantar un Nodo Hadoop con Docker en Chromebook**

Levantar un nodo Hadoop con Docker en una Chromebook puede ser un poco más desafiante que en sistemas operativos tradicionales como macOS o Windows, principalmente debido a las limitaciones del sistema operativo Chrome OS. Sin embargo, con los pasos correctos, es posible.

**Requisitos Previos:**

**1. Activar el entorno Linux (Terminal):** Las Chromebooks modernas tienen la capacidad de ejecutar un entorno Linux dentro de una máquina virtual ligera. Debes habilitar esta función si aún no lo has hecho. Ve a **Configuración** > **Avanzado** > **Desarrolladores** > **Entorno de desarrollo Linux** y sigue las instrucciones para configurarlo. Esto te dará acceso a una terminal de Linux basada en Debian.

**2. Instalar Docker:** Una vez que tengas el entorno Linux habilitado, necesitarás instalar Docker dentro de él. Abre la terminal de Linux y sigue estos pasos:

sudo apt update
sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release
curl -fsSL https://download.docker.com/linux/debian/gpg1 | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg]2 https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null3
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin4
Después de la instalación, verifica que Docker esté funcionando:bash
sudo systemctl start docker
sudo systemctl enable docker
docker --version

# **Pasos para Levantar el Nodo Hadoop:**

**1. Clonar el Repositorio:** Abre la terminal de Linux en tu Chromebook y clona el repositorio:

Bash
git clone https://github.com/big-data-europe/docker-hadoop.git
cd docker-hadoop



**2. Iniciar los Contenedores Docker:** Dentro del directorio docker-hadoop, ejecuta el comando para iniciar los contenedores definidos en el archivo docker-compose.yml:

Bash
docker compose up -d

Este comando descargará las imágenes de Docker necesarias (si no las tienes ya) y creará e iniciará los contenedores para Hadoop (namenode, datanode, resourcemanager, nodemanager, historyserver). La primera vez puede tardar un poco debido a la descarga de las imágenes.

**3. Verificar el Estado de los Contenedores:** Puedes verificar si los contenedores se están ejecutando con el siguiente comando:

Bash
docker ps

Deberías ver una lista de contenedores en ejecución, incluyendo namenode, datanode, etc.

**4. Acceder al Nodo Maestro (namenode):** Para interactuar con el sistema de archivos HDFS, puedes entrar al contenedor namenode:

Bash
docker exec -it namenode bash

Esto te dará una terminal dentro del contenedor namenode.

**5. Visualizar el Panel de Control (Namenode UI):** Para acceder a la interfaz web del Namenode, que generalmente está en el puerto 9870, necesitarás hacer un forwarding de puertos desde el contenedor Docker a tu Chromebook.

**Identifica la dirección IP del contenedor:** Dentro del contenedor namenode, puedes intentar obtener su dirección IP con comandos como hostname -i o ip a. Sin embargo, la forma más sencilla suele ser verificar la configuración de red de Docker en tu Chromebook (aunque esto puede ser complejo).

**Usa la dirección IP de localhost:** En muchos casos, los servicios dentro de los contenedores son accesibles a través de localhost en tu Chromebook si Docker está configurado correctamente. Intenta abrir http://localhost:9870 en el navegador de Chrome de tu Chromebook.

**Nota importante sobre el Forwarding de Puertos en Chromebook:** Chrome OS puede tener restricciones en el acceso directo a los puertos de los contenedores Linux. Si localhost:9870 no funciona, es posible que necesites investigar herramientas de forwarding de puertos específicas para Chrome OS o configurar reglas de red más avanzadas (lo cual puede ser complejo y estar fuera del alcance de un tutorial básico). Sin embargo, en muchas configuraciones modernas, Docker y el entorno Linux facilitan el acceso a través de localhost.

**6. Interactuar con HDFS (dentro del contenedor namenode):** Una vez dentro del contenedor namenode (con el comando docker exec -it namenode bash), puedes usar los comandos de Hadoop HDFS:

Bash
hdfs dfs -ls /
hdfs dfs -mkdir /user/tu_usuario
# **Comandos de HDFS**
https://docs.google.com/document/d/1dIErbeosY1ANL1Sfgf9wqBl2NJLk7mpubypXxT4U62M/edit?tab=t.0



# **Detener los Contenedores** 
Cuando termines de trabajar con Hadoop, puedes detener los contenedores desde la terminal (en el directorio docker-hadoop):

Bash
docker-compose down

Esto detendrá y eliminará los contenedores creados por docker-compose.

**Consideraciones Específicas para Chromebook:**
**Recursos Limitados:** Las Chromebooks a menudo tienen recursos de hardware limitados (CPU, RAM, disco). Ejecutar un clúster Hadoop completo, incluso de un solo nodo en contenedores, puede ser lento o incluso causar problemas de rendimiento si tu Chromebook no tiene suficientes recursos. Monitorea el uso de la CPU y la memoria.

**Almacenamiento:** Asegúrate de tener suficiente espacio libre en tu Chromebook para descargar las imágenes de Docker y para los datos que vayas a procesar en Hadoop.

**Rendimiento:** No esperes el mismo rendimiento que obtendrías en una máquina con recursos dedicados. El entorno Docker en Linux dentro de Chrome OS agrega una capa de virtualización.

**Forwarding de Puertos:** Como se mencionó, el acceso a las interfaces web de los servicios Hadoop (como el Namenode UI en el puerto 9870 o el ResourceManager UI en el puerto 8088) podría requerir una configuración adicional de forwarding de puertos si el acceso directo a localhost no funciona.

Siguiendo estos pasos, deberías poder levantar un nodo Hadoop utilizando Docker en tu Chromebook. Recuerda ser paciente durante la descarga de las imágenes y la inicialización de los contenedores, y ten en cuenta las posibles limitaciones de rendimiento del entorno.
# Fuentes
1. https://github.com/powerticket/rpi-server
2. https://github.com/cypress-io/cypress/issues/25591
3. https://github.com/fullstackedorg/workspace sujeto a licencia (MIT)
4. https://github.com/apache/airflow/issues/15291
