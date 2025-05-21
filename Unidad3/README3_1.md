# Práctica 3.1. Verificación del clúster

## Objetivos de la práctica:
Al finalizar esta práctica, serás capaz de:
- Realizar una validación completa de un clúster de Kubernetes para asegurar que está configurado correctamente y es funcional, comprobando que puede gestionar recursos esenciales como Deployments, ConfigMaps y Servicios.

## Duración aproximada:
- 30 minutos.
<br/>

## Instrucciones

### Paso 1. Verificar que los nodos estén listos

Ejecutar el comando:

```bash
kubectl get nodes
```

Asegúrate de que todos los nodos están en estado `Ready`.

<br/>

### Paso 2. Validar que los componentes del clúster estén funcionando

Ejecutar el comando:

```bash
kubectl get pods -n kube-system
```

Verificar que los Pods en el espacio de nombres `kube-system` están en estado Running o Completed, especialmente:

- kube-apiserver
- kube-controller-manager
- kube-scheduler
- etcd
 

<br/>

### Paso 3. Crear y probar un Deployment

Crear un Deployment mínimo como prueba:

```bash
kubectl create deployment nginx --image=nginx
```

Verificar que el Deployment se creó correctamente:

```bash
kubectl get deployments
kubectl get pods
```

Asegúrate de que el Pod asociado al Deployment está en estado Running.

<br/>

### Paso 4. Probar ConfigMaps

Crear un ConfigMap de prueba:

```bash
kubectl create configmap test-config --from-literal=key1=value1
```

Verificar que el ConfigMap fue creado:

```bash
kubectl get configmaps
kubectl describe configmap test-config
```

<br/>

### Paso 5. Probar servicios

Crear un servicio expuesto en ClusterIP para el Deployment de nginx:

```bash
kubectl expose deployment nginx --port=80 --target-port=80
```

Verificar el servicio:

```bash
kubectl get services
```

Opcionalmente, prueba el acceso al servicio dentro del clúster usando `kubectl` exec en un Pod.

<br/>

### Paso 6. Verificar que los recursos están funcionando juntos

Probar el acceso al Deployment usando el servicio creado:

```bash
kubectl run curl-test --image=curlimages/curl -it --rm -- curl nginx
```

Esto debería devolver una respuesta válida del servidor Nginx.


<br/>

### Paso 7. Verificar logs en caso de problemas

Si encuentras errores, inspeccionar los eventos y logs:

```bash
 
kubectl describe pod <nombre-del-pod>

kubectl logs <nombre-del-pod>
```

<br/>

### Paso 8. Verificar los objetos Kubernetes asociados a Oracle Database

Para asegurar que Oracle Database está correctamente desplegada en un clúster de Kubernetes, debes verificar los siguientes objetos de Kubernetes relacionados:

#### 1. **Pods**
   - Verificar que el Pod (o Pods) de Oracle Database esté corriendo.

   ```bash
   kubectl get pods 
   ```
   
   - Si no hay un namespace asociados pudes no usar -n <namespace>, en este y los demás comandos.
   - Si el Pod no está funcionando, utilizar el comando `kubectl describe pod <pod-name> -n <namespace>` o `kubectl logs <pod-name> -n <namespace>` para diagnosticar problemas.


#### 2. **Services**
   - Comprobar que haya un **Service** configurado para exponer Oracle Database (por ejemplo, `ClusterIP`, `NodePort` o `LoadBalancer`).
   
   ```bash
   kubectl get services  
   ```
   - Verificar que los puertos expuestos sean correctos para Oracle DB, como el puerto 1521 por defecto.


#### 3. **PersistentVolume (PV) y PersistentVolumeClaim (PVC)**
   - Verificar que los **PersistentVolumes** y **PersistentVolumeClaims** estén correctamente ligados y en estado "Bound".

   ```bash
   kubectl get pv
   kubectl get pvc  
   ```

#### 4. **ConfigMap**
   - Si configuraste Oracle DB con un **ConfigMap** para gestionar parámetros de configuración, asegúrate de que exista y que esté correctamente referenciado por los Pods.
   
   ```bash
   kubectl get configmap  
   ```

#### 5. **Logs**
   - Verificar los logs del Pod para identificar cualquier error durante el inicio o ejecución.

   ```bash
   kubectl logs <pod-name>  
   ```
   

#### 6. **Docker Image**
   - Comprobar que la imagen de Docker utilizada para Oracle DB sea correcta y compatible con Kubernetes, recuerda que en el ambiente proporcionado usas containerd, en lugar de Docker.

   - Conectate con ssh al nodo worker y aplicar el siguiente comando:

  ```bash
   sudo crictl images 2>/dev/null | grep -i oracle

   sudo crictl ps 2>/dev/null | grep  -i oracle
   ```

    - **Nota**: Veras la imagen de Oracle Database 21.3.0-xe y el contenedor oracle-db en estado de Running.


<br/>

### Paso 9. Instrucciones para verificar conexiones a Oracle usando SQL Developer

#### Requisitos previos:
1. **SQL Developer instalado:** Asegúrate de tener instalado Oracle SQL Developer en la máquina Windows.

2. **Datos de conexión:** Verificar que tienes la siguiente información:
   - **Usuario 1:** `dkuser`
     - Contraseña: `dkpassword`

   - **Usuario 2:** `sys`
     - Contraseña: `dkpasswords`
     - Tipo de autenticación: **SYSDBA**

   - **Hostname**: Dirección IP o nombre del servidor Oracle.

   - **Servicio**: `XEPDB1`.

   - **Puerto**: `1521`.


<br/>

#### Paso 1. Iniciar SQL Developer
1. Abrir SQL Developer.

    - Verificar si ya tienes las conexiones hacia la base de datos Oracle, en caso contrario, agregar dos conexiones, una para el usuario dkuser y otra para sys con el rol de dba.

2. Hacer clic en el botón de conexión **+** para crear una nueva conexión.

<br/>

#### Paso 2: Configurar la conexión para el usuario `dkuser`

1. En la ventana de conexión, proporciona los siguientes detalles:
   - **Nombre de la conexión:** `dkuser-xepdb1`
   - **Usuario:** `dkuser`
   - **Contraseña:** `dkpassword`
   - **Hostname:** <Dirección IP o nombre del servidor>.
   - **Puerto:** `<puerto expuesto por servicio de Kubernetes>`
   - **SID/Servicio:** `XEPDB1` (elige "Servicio" en el tipo de conexión).

2. Hacer clic en **Probar conexión**.

3. Si la conexión es exitosa, guardar los detalles haciendo clic en **Conectar**.

<br/>

#### Paso 3: Configurar la conexión para el usuario `sys`

1. Crear una nueva conexión similar a la anterior con los siguientes detalles:
   - **Nombre de la conexión:** `dki-sys`
   - **Usuario:** `sys`
   - **Contraseña:** `dkpassword`
   - **Hostname:** <Dirección IP o nombre del servidor>
   - **Puerto:** `<puerto expuesto por servicio de Kubernetes>`
   - **SID/Servicio:** `XE`
   - **Rol:** Selecciona **SYSDBA**.

2. Hacer clic en **Probar conexión**.

3. Si la conexión es exitosa, guardar los detalles haciendo clic en **Conectar**.

<br/>

#### Verificaciones SQL para el usuario `dkuser`

Conectar a la base de datos usando la conexión `dkuser` y realizar las siguientes operaciones para verificar las capacidades del usuario.

##### 1. **Conexión a la base de datos**
Ejecutar el siguiente comando para asegurarte de que estás conectado:

```sql
SELECT SYS_CONTEXT('USERENV', 'SESSION_USER') AS CURRENT_USER FROM DUAL;
```
- El resultado debe mostrar `dkuser`.

<br/>

##### 2. **Crear una tabla**

Crear una tabla llamada `TEST_TABLE`:

```sql
CREATE TABLE TEST_TABLE (
    ID NUMBER PRIMARY KEY,
    NAME VARCHAR2(100)
);
```

<br/>


##### 3. **Insertar un registro**

Insertar un registro en la tabla:

```sql
INSERT INTO TEST_TABLE (ID, NAME) VALUES (1, 'Test User');
COMMIT;
```

<br/>

##### 4. **Realizar una búsqueda**

Realizar una consulta para verificar que el registro fue insertado:

```sql
SELECT * FROM TEST_TABLE;
```

<br/>

##### 5. **Eliminar la tabla**

Eliminar la tabla para limpiar el entorno:

```sql
DROP TABLE TEST_TABLE;
```

<br/>

Estas instrucciones te ayudarán a verificar que el usuario `dkuser` puede realizar operaciones básicas en la base de datos `XEPDB1` utilizando SQL Developer y comandos SQL, y que tus servicios podrán gestionar su persistencia de datos.


<br/> 
<br/>

## Resultado esperado


- Captura de pantalla que muestra la versión de `kubectl`, el estado de los nodos en el clúster de Kubernetes, y el estado de los Pods en el namespace kube-system.


![kubectl](../images/u3_1_1.png)

<br/>

- Captura de pantalla que ilustra cómo, utilizando un enfoque imperativo, se crea un Deployment con la última versión de la imagen de Nginx, seguido de la visualización del estado del Deployment y de los Pods asociados a dicho Deployment.

![kubectl](../images/u3_1_2.png)

<br/>

- Captura de pantalla que muestra cómo, mediante un enfoque imperativo, se crea un ConfigMap, seguido de la verificación de los ConfigMaps existentes y la descripción detallada del ConfigMap, en la cual se puede observar el valor en texto plano de la clave `key1`.

![kubectl](../images/u3_1_3.png)

<br/>

- Captura de pantalla que muestra el proceso para exponer un servicio en Kubernetes, seguido de la salida del comando kubectl get services para listar los servicios creados, y finalmente el uso del comando kubectl run curl-test... para realizar una solicitud HTTP al servidor Nginx, evidenciado por la respuesta en formato HTML proporcionada por el servidor.

![kubectl](../images/u3_1_4.png)

**Nota**: El comando `kubectl run curl-test --image=curlimages/curl -it --rm -- curl nginx` se utiliza para ejecutar un contenedor temporal basado en la imagen `curlimages/curl` y realizar una solicitud HTTP al servicio nginx.

<br/>


- Captura de pantalla que muestra el estado de los Pods y un fragmento de la descripción del Pod asociado al Deployment de Nginx.

![kubectl](../images/u3_1_5.png)

<br/>

- Captura de pantalla que muestra la bitácora del Pod asociado al Deployment de Nginx, donde se observa en la última línea la solicitud enviada como parte de la práctica utilizando un contenedor temporal de prueba.

![kubectl](../images/u3_1_6.png)

 
<br/>

- Captura de pantalla que muestra la instancia de Oracle Database está desplegada correctamente en el clúster de Kubernetes:

    - El Pod está corriendo sin problemas.
    - El servicio está configurado y accesible a través del puerto `30011`.
    - El almacenamiento persistente está enlazado correctamente con un PVC y un PV.
    - Los ConfigMaps y Secrets están disponibles para la configuración y credenciales de la base de datos.
    - Los logs confirman que la instancia está operativa y funcional.

Esto indica que todos los recursos necesarios para el funcionamiento de Oracle Database en Kubernetes están configurados y operativos.


![kubectl](../images/u3_1_10.png)

 <br/>

- Captura de pantalla que muestra el estado del Worker Node.

1. **Imagen de Oracle Database**:
   - La imagen está correctamente descargada y almacenada en el nodo, con un tamaño de 3.75GB.
   - Está identificada como `21.3.0-xe`.

2. **Contenedor en Ejecución**:
   - Un contenedor basado en esta imagen está en ejecución, asociado con el nombre `oracle-db-cbf654876-z5kx9`.
   - Este contenedor está en estado `Running`, indicando que la base de datos Oracle está operativa.

![crictl](../images/u3_1_11.png)

 <br/>

- Captura de pantalla que muestra la configuración de una conexión en SQL Developer para el usuario `dkuser`, utilizando el servicio `XEPDB1`, alojado en el host `192.168.0.79` y el puerto `30011`. El estado de la conexión confirma que está configurada correctamente y es funcional. Es importante tener en cuenta que tanto la dirección IP como el puerto pueden variar, aunque en un entorno Kubernetes, el puerto generalmente estará dentro del rango de los treinta mil.

![kubectl](../images/u3_1_7.png)

 <br/>

- Captura de pantalla muestra la configuración de una conexión en SQL Developer para el usuario sys, con rol de SYSDBA, en una base de datos Oracle. Es importante destacar que tanto la dirección IP como el puerto pueden variar dependiendo de la configuración del clúster o entorno en el que esté desplegado Oracle. En Kubernetes, el puerto típicamente estará dentro del rango de los treinta.

![kubectl](../images/u3_1_8.png)

<br/>

- La captura de pantalla muestra la ejecución exitosa de comandos SQL en Oracle SQL Developer, conectados con el usuario dkuser en el servicio XEPDB1.

![kubectl](../images/u3_1_9.png)
