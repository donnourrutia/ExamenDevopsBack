# 🔧 Innovatech Chile - Servicios Backend

Repositorio encargado de la capa de procesamiento de datos de Innovatech Chile. La solución está basada en una arquitectura de microservicios, donde cada componente ejecuta una función específica dentro del proceso de ventas y distribución de productos.

## 📌 Componentes del Sistema

### 🛍️ Servicio de Gestión Comercial (`8082`)

Administra el registro de compras realizadas por los clientes. Permite almacenar nuevas órdenes y consultar información relacionada con el proceso de venta.

### 🚛 Servicio de Distribución (`8081`)

Gestiona las actividades logísticas posteriores a la compra, incluyendo la asignación de transporte, seguimiento de entregas y actualización de estados hasta completar el despacho.

## ☁️ Infraestructura

Los servicios se ejecutan sobre un clúster de **Amazon EKS (Elastic Kubernetes Service)**, donde cada microservicio se despliega como un **Deployment** independiente.

El acceso externo se realiza mediante **Load Balancers** administrados por AWS, permitiendo distribuir las solicitudes entre las distintas réplicas disponibles de cada servicio.

Las imágenes Docker utilizadas por los microservicios se almacenan en **Amazon ECR**, desde donde Kubernetes obtiene automáticamente la versión correspondiente durante el despliegue.

## 📈 Escalamiento Automático

La plataforma utiliza **Horizontal Pod Autoscaler (HPA)** para ajustar automáticamente la cantidad de Pods de cada microservicio según la carga del sistema.

El HPA supervisa el consumo promedio de CPU y, cuando este supera el **50 %**, incrementa el número de réplicas para mantener el rendimiento de la aplicación. Cuando la demanda disminuye, el escalador reduce automáticamente la cantidad de Pods en ejecución, optimizando el uso de los recursos del clúster.

Esta estrategia proporciona los siguientes beneficios:

- Escalamiento automático según la carga de trabajo.
- Mayor disponibilidad de los microservicios.
- Distribución eficiente de las solicitudes entre múltiples Pods.
- Optimización del consumo de recursos en Kubernetes.

## 📋 Requisitos para Desarrollo

* Java JDK 17 o superior
* Apache Maven 3.8+
* MySQL Server

## 🗄️ Preparación de la Base de Datos

1. Iniciar una instancia de MySQL.
2. Crear la base de datos definida en el archivo de configuración de Spring.
3. Verificar las credenciales configuradas en `application.properties`.
4. Ejecutar la aplicación para que Hibernate genere automáticamente las estructuras necesarias.

Dependiendo de la configuración seleccionada, las tablas pueden crearse o actualizarse automáticamente durante el arranque del sistema.

## ▶️ Ejecución Local

### Obtener el código fuente

```bash
git clone https://github.com/DoomedPlayer/DevopsEV3-back.git
```

### Compilar el proyecto

```bash
mvn clean install
```

### Iniciar los servicios

```bash
mvn spring-boot:run
```

Cada microservicio debe ejecutarse desde su respectivo proyecto.

## ⚡ Automatización DevOps

El proceso de integración y despliegue continuo se encuentra automatizado mediante **GitHub Actions**, permitiendo publicar nuevas versiones de los microservicios de forma controlada.

### Flujo del pipeline

1. Validación y compilación del código fuente.
2. Generación de los archivos ejecutables (`.jar`).
3. Construcción de imágenes Docker para cada microservicio.
4. Publicación de las imágenes en Amazon ECR.
5. Actualización automática de los Deployments en Amazon EKS.
6. Escalamiento dinámico de los servicios mediante Horizontal Pod Autoscaler.

Las credenciales necesarias para la ejecución del pipeline se administran mediante **GitHub Secrets**, evitando exponer información sensible dentro del repositorio.
