# Práctica 1.6. Contenedores Docker - Caso de estudio

## Objetivos de la práctica:

Al finalizar esta actividad, serás capaz de:
- Crear contenedores Docker para dos microservicios existentes a partir de sus archivos JAR.
- Configurar una red Docker para habilitar la comunicación entre ellos y utilizar la persistencia con Oracle DB mediante volúmenes Docker.

## Duración aproximada
- 25 minutos. 

<br/>

## Instrucciones

### Paso 1. Crear Dockerfile para `ms-productos`

1. Navegar al directorio/carpeta donde se encuentra el proyecto `ms-productos`.

2. Verificar que tengas el JAR del microservicio.

3. Crear un archivo `Dockerfile` con el siguiente contenido:

```Dockerfile
FROM openjdk:21-jdk-slim
WORKDIR /app
COPY target/ms-productos-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 9081
ENTRYPOINT ["java", "-jar", "app.jar"]
```

4. Guardar el archivo.


<br/>

### Paso 2. Crear el Dockerfile para `ms-deseos`

1. Navegar al directorio/carpeta donde se encuentra el proyecto `ms-deseos`.

2. Verificar que tengas el JAR del microservicio.

3. Crear un archivo `Dockerfile` con el siguiente contenido:

```Dockerfile
FROM openjdk:21-jdk-slim
WORKDIR /app
COPY target/ms-deseos-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 9084
ENTRYPOINT ["java", "-jar", "app.jar"]
```

4. Guardar el archivo.

<br/> 

### Paso 3. Crear las imágenes Docker

1. Abrir una terminal de comandos y navegar al directorio donde se encuentra el `Dockerfile` del microservicio `ms-productos`.

2. Construir la imagen de ms-productos con el siguiente comando:

```cmd
docker build -t ms-productos:1.0 .

# Verificar la creación de la imagen.
docker image ls
```

3. Navegar al directorio donde se encuentra el `Dockerfile` del microservicio `ms-deseos`.

4. Construir la imagen de ms-deseos con el siguiente comando:

```cmd
docker build -t ms-deseos:1.0 .

# Verificar la creación de la imagen.
docker images
```

<br/>

### Paso 4. Crear y levantar los contenedores

1. Crear el contenedor para `ms-productos` y conectarlo a la red dki-network.

```cmd
docker run -d --name ms-productos --network dki-network -p 9081:9081 ms-productos:1.0
```

2. Crear el contenedor para `ms-deseos` y conectarlo a la red `dki-network`.

```cmd
docker run -d --name ms-deseos --network dki-network -p 9084:9084 ms-deseos:1.0
```

<br/>


### Paso 5. Verificar los contenedores

1. Listar los contenedores en ejecución para confirmar que ambos están activos.

```cmd
docker ps
```

<br/>

### Notas adicionales:

- El contenedor de la base de datos Oracle DB debe estar conectado a la red `dki-network`.

- Asegúrate de que el volumen `dki-volume` esté asociado al contenedor de Oracle DB para la persistencia de datos.

```cmd
docker volume inspect dki-volume

# Mostrar los contenedores que tiene el volumen especificado montado.

docker ps --filter volume=dki-volume

```

- Si necesitas revisar los logs de algún contenedor para depuración, usar:

```cmd
docker logs <nombre_del_contenedor>
```

- Con estas instrucciones, los microservicios y la base de datos deberían estar completamente funcionales y listos para las pruebas de la siguiente práctica.

<br/>
<br/>

## Resultado esperado

- Captura de pantalla que muestra:

    - La verificación de la existencia del JAR para ms-productos.
    - La creación del archivo Dockerfile correspondiente.
    - La verificación de la inexistencia del JAR para ms-deseos, seguida de la instrucción que construye dicho JAR para solucionar la ausencia detectada.

![cmd](../images/u1_6_1.png)

<br/>


- Captura de pantalla que muestra la verificación del JAR para el microservicio ms-deseos y el contenido del Dockerfile.


![cmd](../images/u1_6_2.png)

<br/>

- Captura de pantalla que muestra la construcción y verificación de la imagen Docker `ms-productos`.

![cmd](../images/u1_6_3.png)

<br/>

- Captura de pantalla que muestra la construcción y verificación de la imagen Docker `ms-deseos`.

![cmd](../images/u1_6_4.png)

<br/>

- Captura de pantalla que muestra la creación y verificación de los dos contenedores, `ms-productos` & `ms-deseos`.


![cmd](../images/u1_6_9.png)

<br/>



- Captura de pantalla que muestra la bitácora del microservicio `ms-productos`, en el puerto 9081.

![cmd](../images/u1_6_10.png)

<br/>


- Captura de pantalla que muestra la bitácora del microservicio `ms-deseos`, en el puerto 9084.

![cmd](../images/u1_6_11.png)

<br/>

- Captura de pantalla que muestra algunos reportes y verificaciones adicionales sobre la práctica.

![cmd](../images/u1_6_12.png)

<br/>
