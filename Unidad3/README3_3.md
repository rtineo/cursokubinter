# Práctica 3.3. Docker Registry

## Objetivos de la práctica:
Al finalizar esta práctica, serás capaz de:

- Crear una imagen Docker a partir del código de los microservicios y publicarla en Docker Hub, utilizando buenas prácticas para el etiquetado y manejo de versiones.

## Duración aproximada:
- 25 minutos.

  
## Objetivo visual

![Docker Image](../images/u3_3_3.png)

<br/>

## Instrucciones

### Paso 1. Verificar el estado del Docker Daemon
Asegúrate de que Docker esté ejecutándose en la máquina de desarrollo Windows, en el entorno de curso.

```bash
docker info
```
Si Docker está corriendo correctamente, verás información del sistema. Si no, iniciar Docker.

<br/>

### Paso 2. Construir la imagen del microservicio

1. Acceder al directorio de cada microservicio (por ejemplo, `ms-productos`).

   ```bash
   cd /ruta/a/ms-productos
   ```
   
<br/>

2. Construir la imagen Docker desde el archivo `Dockerfile`:

   ```bash
   docker build -t ms-productos:1.0 .

   ```
   **Salida esperada:**
   ```
   Successfully built <IMAGE_ID>
   Successfully tagged ms-productos:1.0
   ```

<br/>

3. Repetir el proceso para el segundo microservicio (`ms-deseos`):

   ```bash
   cd /ruta/a/ms-deseos
   docker build -t ms-deseos:1.0 .
   ```

<br/>

### Paso 3. Verificar las imágenes construidas

Listar las imágenes locales para confirmar que las imágenes han sido creadas correctamente:

```bash
docker images
```

**Salida esperada:**
```
REPOSITORY       TAG       IMAGE ID       CREATED          SIZE
ms-productos     1.0       <IMAGE_ID>     X seconds ago    X MB
ms-deseos        1.0       <IMAGE_ID>     X seconds ago    X MB
```

<br/>


### Paso 4. Iniciar sesión en Docker Hub

Iniciar sesión con tu cuenta de Docker Hub:
```bash
docker login
```

Ingresar tu nombre de usuario y contraseña de Docker Hub cuando se solicite. Si la autenticación es exitosa:

```
Login Succeeded
```

<br/>

### Paso 5. Etiquetar las imágenes para Docker Hub

Asignar etiquetas a las imágenes con el formato adecuado para Docker Hub. Reemplazar `TU_USUARIO` por tu nombre de usuario en Docker Hub.

1. Para `ms-productos`:

   ```bash
   docker tag ms-productos:1.0 TU_USUARIO/ms-productos:1.0
   ```

2. Para `ms-deseos`:

   ```bash
   docker tag ms-deseos:1.0 TU_USUARIO/ms-deseos:1.0
   ```

<br/>

### Paso 6. Publicar las imágenes en Docker Hub

Subir las imágenes al Docker Registry:

1. Para `ms-productos`:

   ```bash
   docker push TU_USUARIO/ms-productos:1.0
   ```

   **Salida esperada:**
   ```
   The push refers to repository [docker.io/TU_USUARIO/ms-productos]
   X.X MB [==================================================] 100%
   latest: digest: sha256:<DIGEST_ID> size: X
   ```

2. Para `ms-deseos`:

   ```bash
   docker push TU_USUARIO/ms-deseos:1.0
   ```
   **Salida esperada:**
   ```
   The push refers to repository [docker.io/TU_USUARIO/ms-deseos]
   X.X MB [==================================================] 100%
   latest: digest: sha256:<DIGEST_ID> size: X
   ```

<br/>

### Paso 7. Verificar las imágenes en Docker Hub

Utilizar el comando `docker search` para buscar las imágenes en Docker Hub:

1. Para `ms-productos`:
   ```bash
   docker search TU_USUARIO/ms-productos
   ```

   **Salida esperada:**

   ```
   NAME                         DESCRIPTION   STARS   OFFICIAL   AUTOMATED
   TU_USUARIO/ms-productos      ...           0
   ```

2. Para `ms-deseos`:

   ```bash
   docker search TU_USUARIO/ms-deseos
   ```

   **Salida esperada:**

   ```
   NAME                         DESCRIPTION   STARS   OFFICIAL   AUTOMATED
   TU_USUARIO/ms-deseos         ...           0
   ```

<br/>
<br/>

### Recomendadiones

1. Utilizar etiquetas descriptivas y versiones en las imágenes (`:1.0`, `:1.1`, `:latest`).

2. Repetir el proceso para cada versión nueva del microservicio, asegurándote de incrementar las versiones de las etiquetas.

3. Documentar los pasos realizados y los comandos utilizados en un archivo `README` en los repositorios de los microservicios.

<br/>
<br/>

### Referencias

- Este es el sitio oficial que define las reglas y especificaciones del versionado semántico. Ofrece una guía detallada sobre cómo asignar e incrementar los números de versión de manera coherente.

    [Versionado Semático](https://semver.org/lang/es/)

<br/>

- Este artículo aborda la implementación del versionado semántico en arquitecturas de microservicios, destacando su importancia para la gestión de dependencias y la compatibilidad entre servicios.

    [Mejores Prácticas de Versionado Semático para Microservicios](https://peerdh.com/es/blogs/programming-insights/semantic-versioning-best-practices-for-microservices)


<br/>
<br/>

## Resultado esperado

- Captura de pantalla que muestra si Docker esta corriendo correctamente.

![](../images/u3_3_1.png)

