# Documento de explicación - Práctica 2

## 1) ¿Cómo abordaron el proceso de despliegue?

Se trabajó en dos fases. En la primera fase, el equipo validó la ejecución local de la API ASP.NET Core y luego preparó la contenerización con un `Dockerfile` multi-stage para .NET 8. Después, se construyó y ejecutó la imagen con `docker compose`, incluyendo SQL Server 2022 como contenedor separado para eliminar dependencia de una base instalada fuera de Docker.

En la segunda fase, se migró el despliegue a Kubernetes local (Docker Desktop). Se definieron manifiestos YAML para `Secret`, `PVC`, `Deployment` y `Service` tanto de SQL Server como de la API. Se verificó el estado del clúster, se aplicaron manifiestos y se validó la operación con `kubectl get pods`, `kubectl get svc` y acceso a Swagger.

## 2) ¿Qué errores encontraron y cómo los resolvieron?

1. **Bloqueo de push por secretos detectados en GitHub**: se identificaron credenciales en `appsettings`. Se reemplazaron por valores seguros y se limpió el historial para poder publicar el repositorio.
2. **La API no iniciaba en contenedor por conexión SQL**: el proyecto usaba conexión local fuera del contenedor y autenticación no adecuada para Linux. Se dockerizó SQL Server y se pasó la cadena de conexión por variables de entorno.
3. **Error en Kubernetes por imagen de API (`ErrImagePull`)**: el clúster intentaba descargar `tradingjournal-api:local` desde Docker Hub. Se reconstruyó la imagen local y se configuró `imagePullPolicy: Never`.
4. **NodePort no accesible directamente en el entorno local**: se validó el acceso usando `kubectl port-forward`, opción permitida por el enunciado.

## 3) ¿Cómo se distribuyeron las responsabilidades del equipo?

- **Edwin**: líder de integración, configuración final de `docker-compose`, validación de Swagger y consolidación del repositorio.
- **Juan Jose**: soporte de API (.NET), revisión de configuración de `Program.cs` y variables de entorno.
- **Felipe**: definición y ajuste de manifiestos Kubernetes (`Deployment`, `Service`, `PVC`, `Secret`).
- **Julian Andres**: pruebas funcionales de Docker/Kubernetes, verificación de comandos `kubectl` y validación de evidencias técnicas.
- **Argenis*: documentación, organización de capturas, estructura del `README.md` y consolidación de entregables.

Con esta distribución se cubrió el ciclo completo de despliegue: contenerización, ejecución en Docker y orquestación en Kubernetes local.
