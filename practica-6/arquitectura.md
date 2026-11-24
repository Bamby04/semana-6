# Arquitectura del Proyecto - Modelo C4

## Nivel 1: Diagrama de Contexto

```mermaid
C4Context
    title Diagrama de Contexto del Sistema
    Person(usuario, "Usuario Final", "Persona que utiliza el sistema para realizar sus operaciones")
    Person(admin, "Administrador", "Persona encargada de gestionar y supervisar el sistema")
    System(sistema, "Sistema Principal", "Aplicación web del proyecto que centraliza la lógica y datos")
    System_Ext(servicioExterno, "Servicio Externo", "Sistema externo de apoyo (autenticación, notificaciones o pagos)")

    Rel(usuario, sistema, "Interactúa con el sistema", "HTTPS")
    Rel(admin, sistema, "Administra y consulta información", "HTTPS")
    Rel(sistema, servicioExterno, "Intercambia información", "API REST / HTTPS")
```

### Justificación - Nivel 1

En este nivel de contexto se muestra al sistema como una caja principal junto con los actores y sistemas externos con los que se relaciona. Los usuarios y administradores acceden mediante HTTPS para operar dentro de la plataforma, mientras que la integración con un servicio externo permite delegar funciones complementarias (como notificaciones o validaciones externas) sin mezclar responsabilidades ajenas al núcleo del sistema.

---

## Nivel 2: Diagrama de Contenedores

```mermaid
C4Container
    title Diagrama de Contenedores del Sistema
    Person(usuario, "Usuario Final", "Accede desde el navegador web")
    Person(admin, "Administrador", "Accede desde el navegador web")

    Container_Boundary(c1, "Sistema Principal") {
        Container(frontend, "Aplicación Web (Frontend)", "HTML, CSS, JavaScript / React", "Provee la interfaz gráfica y maneja la interacción del usuario")
        Container(backend, "API / Servidor (Backend)", "Node.js / Python / Java", "Ejecuta la lógica de negocio, validaciones y reglas del sistema")
        ContainerDb(bd, "Base de Datos", "PostgreSQL / MySQL", "Almacena de forma persistente usuarios, registros y estados")
    }

    System_Ext(servicioExterno, "Servicio Externo", "API de terceros")

    Rel(usuario, frontend, "Usa la interfaz", "HTTPS")
    Rel(admin, frontend, "Gestiona desde la web", "HTTPS")
    Rel(frontend, backend, "Consume servicios y envía datos", "JSON / HTTPS")
    Rel(backend, bd, "Lee y escribe información", "SQL / TCP")
    Rel(backend, servicioExterno, "Consulta o envía datos", "REST API / HTTPS")
```

### Justificación - Nivel 2

Se dividió el sistema en tres contenedores principales para mantener una separación clara de responsabilidades:

* El **Frontend** se encarga únicamente de la presentación y experiencia del usuario en el navegador.
* El **Backend** concentra las reglas de negocio, autenticación y comunicación segura, evitando que la interfaz acceda directamente a los datos.
* La **Base de Datos** centraliza el almacenamiento persistente con integridad y consultas estructuradas.
* Las conexiones usan protocolos estándar (`HTTPS` con `JSON` entre cliente y servidor, y `SQL` hacia la base de datos) para facilitar el mantenimiento y la escalabilidad.