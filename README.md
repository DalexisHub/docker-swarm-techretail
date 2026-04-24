# Docker Swarm - TechRetail

## Descripción del proyecto
Este proyecto corresponde a la implementación de un clúster Docker Swarm para el caso práctico de TechRetail, una empresa de comercio electrónico que necesita modernizar su infraestructura para soportar alta demanda, escalabilidad y disponibilidad.

La solución implementada utiliza una arquitectura de microservicios compuesta por frontend, backend, base de datos y caché, desplegada sobre un clúster Swarm con un nodo manager y dos nodos worker.

## Arquitectura implementada
El clúster fue configurado con la siguiente estructura:

- 1 nodo Manager
- 2 nodos Worker
- Red overlay para comunicación entre servicios
- Volumen para persistencia de la base de datos
- Docker Secrets para credenciales sensibles
- Docker Configs para la configuración del frontend

### Servicios desplegados
- **Frontend:** `nginx:alpine`
- **Backend:** `node:18-alpine`
- **Base de datos:** `mysql:8`
- **Cache:** `redis:7-alpine`

## Tecnologías utilizadas
- Docker Engine
- Docker Swarm
- Ubuntu Server 24.04.4 ARM64
- VMware Fusion
- iTerm2
- VS Code
- GitHub

## Estructura del repositorio
```text
docker-swarm-techretail/
├── docker-compose.yml
├── README.md
├── configs/
│   └── nginx.conf
├── capturas/
│   └── evidencias del clúster y servicios
└── informe/
    └── informe-techretail.pdf
```
## Requisitos previos

Antes de desplegar el proyecto se requiere:

-   Docker instalado en cada nodo
-   Clúster Swarm inicializado
-   1 nodo manager y 2 workers unidos al clúster
-   Secret creado para la contraseña de la base de datos

## Creación del clúster

En el manager:

docker swarm init --advertise-addr <IP\_MANAGER>

Para obtener el token de los workers:

docker swarm join-token worker

En cada worker:

docker swarm join --token <TOKEN> <IP\_MANAGER>:2377

Verificación:

docker node ls

## Despliegue del stack

Crear el secret:

echo "MiPasswordSegura123" | docker secret create db\_password -

Desplegar el stack:

docker stack deploy -c docker-compose.yml techretail

Verificar servicios:

docker stack services techretail

## Escalado dinámico

Para demostrar el escalado del frontend:

docker service scale techretail\_frontend=5

Verificación:

docker service ps techretail\_frontend

## Docker Configs y Secrets

### Secret utilizado

-   `db_password`

### Config utilizado

-   `nginx.conf` montado en `/etc/nginx/conf.d/default.conf`

## Evidencias obtenidas

Durante la implementación se verificó lo siguiente:

-   Clúster activo con 1 manager y 2 workers
-   Frontend desplegado correctamente
-   Backend desplegado con 2 réplicas
-   Base de datos desplegada con 1 réplica
-   Cache desplegado con 1 réplica
-   Uso de Docker Secrets
-   Uso de Docker Configs
-   Escalado dinámico del frontend

## Problemas encontrados

Durante la implementación se presentaron los siguientes inconvenientes:

-   Play with Docker ya no se encontraba disponible
-   Se presentó incompatibilidad inicial entre arquitecturas x86 y ARM64
-   Los workers clonados heredaron la misma identidad de Docker, lo que obligó a regenerar sus IDs
-   El servicio `dockersamples/visualizer` no fue compatible con ARM64
-   Se detectaron limitaciones puntuales en el acceso HTTP desde el navegador durante las pruebas, aunque el servicio frontend respondió correctamente mediante validación por terminal con `curl`

## Resultados

Se logró implementar correctamente un clúster Docker Swarm funcional con distribución de servicios entre nodos, uso de red overlay, persistencia de datos, gestión de configuraciones sensibles y escalado dinámico.

## Conclusión

La práctica permitió comprobar que Docker Swarm es una solución útil para desplegar servicios distribuidos de forma sencilla, permitiendo administrar múltiples nodos desde un manager y distribuir cargas entre workers. Además, el uso de Secrets, Configs, volúmenes y red overlay permitió construir una solución más organizada y cercana a un escenario real de infraestructura en la nube.

## Autor

Trabajo desarrollado por Alexis.
