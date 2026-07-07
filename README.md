# 🔧 Innovatech Chile - Servicios Backend

Repositorio encargado de la capa de procesamiento de datos de Innovatech Chile. La solución se encuentra dividida en servicios independientes, cada uno con responsabilidades específicas dentro del flujo de ventas y distribución de productos.

## 📌 Componentes del Sistema

### 🛍️ Servicio de Gestión Comercial (`8082`)

Administra el registro de compras realizadas por los clientes. Permite almacenar nuevas órdenes y consultar información relacionada con el proceso de venta.

### 🚛 Servicio de Distribución (`8081`)

Gestiona las actividades logísticas posteriores a la compra, incluyendo la asignación de transporte, seguimiento de entregas y actualización de estados hasta completar el despacho.

## ☁️ Infraestructura

Los servicios se ejecutan dentro de un entorno Kubernetes administrado mediante AWS EKS. La comunicación externa se realiza utilizando balanceadores de carga proporcionados por AWS.

Para adaptarse a variaciones en la demanda, se implementó escalamiento automático horizontal (HPA), configurado para aumentar la cantidad de pods cuando el consumo promedio de CPU supera el 50%.

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

El ciclo de integración y despliegue se encuentra automatizado mediante GitHub Actions.

### Proceso de publicación

1. Validación y compilación del código fuente.
2. Generación de los artefactos ejecutables (`.jar`).
3. Construcción de imágenes Docker para cada servicio.
4. Publicación de imágenes en Amazon ECR.
5. Actualización de los recursos desplegados en AWS EKS mediante Kubernetes.

Las credenciales utilizadas durante el proceso se almacenan mediante GitHub Secrets para evitar la exposición de información sensible dentro del repositorio.
