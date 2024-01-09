# AWS Containers Retail Sample

Este es un ejemplo de aplicación diseñado para ilustrar varios conceptos relacionados con contenedores en AWS. Presenta una aplicación de tienda minorista de muestra que incluye un catálogo de productos, un carrito de compras y un proceso de pago.

Ofrece:

- Una arquitectura de componentes distribuidos en varios lenguajes y frameworks.
- Utilización de diversos backends de persistencia para diferentes componentes, como MySQL, DynamoDB y Redis.
- La capacidad de ejecutarse en varias tecnologías de orquestación de contenedores como Docker Compose, Kubernetes, etc.
- Imágenes preconstruidas de contenedores para arquitecturas de CPU x86-64 y ARM64.
- Todos los componentes instrumentados para métricas de Prometheus y trazado OpenTelemetry OTLP.
- Soporte para Istio en Kubernetes.
- Generador de carga que ejercita toda la infraestructura.

Este proyecto está destinado únicamente con fines educativos y no para uso en producción.

![Screenshot](/docs/images/screenshot.png)

# Arquitectura de la Aplicación

La aplicación ha sido deliberadamente sobreingenierizada para generar múltiples componentes desacoplados. Estos componentes generalmente tienen diferentes dependencias de infraestructura y pueden admitir varios "backends" (por ejemplo: el servicio de Carritos admite MongoDB o DynamoDB).

```
.
├── README.md
├── docker-compose.yml
├── docs
│   └── images
└── src
    ├── assets
    ├── cart
    ├── catalog
    ├── checkout
    ├── orders
    └── ui
```


![Screenshot](/docs/images/arq.png)

# Docker Compose

Este método de implementación se ejecutará en la máquina local/server utilizando docker-compose y construirá los contenedores como parte de la implementación.

**Requisitos previos:**
- Docker instalado localmente

1. Docker Compose:

    ```bash
    docker-compose up -d --build
    ```

2. Abre el frontend en una ventana del navegador:

    [http://localhost:8888](http://localhost:8888)

3. Para detener los contenedores en docker-compose, utiliza Ctrl+C. Para eliminar todos los contenedores y recursos relacionados, ejecuta:

    ```bash
    docker compose down
    ```

--- 


# Desafío DevOps: Implementación de CI/CD con Jenkins

El objetivo de este desafío es establecer un flujo de Integración Continua (CI) y Entrega Continua (CD) para la aplicación "# AWS Containers Retail Sample" utilizando Jenkins.

## Tareas:

### 1. Configuración del Repositorio:
   - Crea un repositorio en un sistema de control de versiones de tu elección (GitHub, GitLab, Bitbucket, etc.).
   - Sube el código fuente de la aplicación y asegúrate de incluir el archivo `Dockerfile` y los scripts necesarios.

### 2. Configuración de Jenkins:
   - Instala Jenkins en un entorno adecuado.
   - Configura Jenkins para que se integre con tu sistema de control de versiones.
   - Configura Jenkins para que pueda acceder a los recursos necesarios de AWS.

### 3. Creación de Jobs en Jenkins:
   - Crea un job de CI para realizar la compilación de la aplicación cada vez que se realice un cambio en el repositorio.
   - Utiliza Jenkinsfile para definir el pipeline de CI.
   - Asegúrate de que la compilación sea exitosa antes de pasar a la etapa de entrega continua.
   - Construir imagen, versionarla y subir a docker-hub

### 4. Implementación de CD con Docker Compose:
   - Crea un job de CD que implemente la aplicación utilizando Docker Compose.
   - Utiliza variables de entorno o parámetros para manejar configuraciones específicas de entorno (por ejemplo, contraseñas de bases de datos).
   - Despliega la aplicación en un entorno de prueba.

### 5. Integración con Notificaciones:
   - Configura notificaciones en Jenkins para que informe sobre el estado de los pipelines.
   - Utiliza un servicio de mensajería (Slack, Discord, etc.) para recibir notificaciones sobre compilaciones y despliegues.

## Puntos Extra (Opcionales):

- **Implementación de CD en un Entorno de Producción:**
  - Crea un job de CD adicional para desplegar la aplicación en un entorno de producción simulado.

- **Pruebas Automatizadas:**
  - Integra pruebas automatizadas en el flujo de CI/CD.

- **Seguridad:**
  - Implementa medidas de seguridad en el flujo de CI/CD, como análisis estático de código o escaneo de vulnerabilidades en contenedores.

Recuerda documentar los pasos y decisiones tomadas durante la configuración del flujo de CI/CD. ¡Buena suerte!