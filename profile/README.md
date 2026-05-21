# Ecosistema DevFlow  — Plataforma SaaS Inteligente de Gestión de Proyectos

Bienvenido al ecosistema oficial de desarrollo de **DevFlow**, una solución SaaS moderna y de alto rendimiento inspirada en Jira y Linear, diseñada para optimizar los flujos de trabajo de ingeniería de software a través de asistencia inteligente con Inteligencia Artificial (IA) y automatización en tiempo real.

---

## Misión del Proyecto

DevFlow busca revolucionar la administración ágil de tareas simplificando la burocracia de los sprints. Mediante el uso de modelos de lenguaje avanzados, estimación inteligente y una arquitectura distribuida sumamente responsiva, proporcionamos a los equipos de desarrollo un entorno limpio, rápido y centrado en la productividad real.

---

## Repositorios del Ecosistema

El proyecto está dividido y modularizado para facilitar el despliegue continuo, el testing aislado y el escalado independiente:

1. **[devflow-backend](https://github.com/tu-usuario/devflow-backend)** (Activo)
   * Repositorio del Core del backend basado en **Microservicios**.
   * Tecnologías: Java 21, Spring Boot 3, Spring Cloud Gateway, Kafka, Redis, Elasticsearch, Postgres, OpenAI.
2. **[devflow-frontend](https://github.com/tu-usuario/devflow-frontend)** (Siguiente fase)
   * Repositorio de la aplicación web del cliente SPA.
   * Tecnologías: Angular, TailwindCSS / Vanilla CSS, RxJS, STOMP.js client.
3. **[devflow-infrastructure](https://github.com/tu-usuario/devflow-infrastructure)** (Opcional)
   * Scripts de automatización y plantillas de despliegue en la nube.
   * Tecnologías: Terraform, Kubernetes manifests, Helm Charts, Ansible.

---

## Arquitectura y Stack Global de Tecnologías

```mermaid
graph LR
    subgraph Frontend [Capa de Presentación]
        UI[Angular SPA Client]
        Style[Vanilla CSS / Tailwind]
        WS[WebSockets - STOMP]
    end

    subgraph API_GW [Capa de Enlace]
        GW[Spring Cloud Gateway]
    end

    subgraph Microservicios [Capa de Negocio]
        Auth[Auth Service - Port 8081]
        Proj[Project Service - Port 8082]
        Notif[Notification Service - Port 8083]
        AISrv[AI Service - Port 8084]
    end

    subgraph Data [Capa de Persistencia e Infra]
        DB[(PostgreSQL)]
        Cache[(Redis Cache)]
        Search[(Elasticsearch)]
        Queue[(Apache Kafka)]
        SMTP[MailHog SMTP]
        LLM[OpenAI GPT-4o-mini]
    end

    UI -->|HTTPS Requests| GW
    GW -->|Enrutamiento y JWT Filter| Auth
    GW -->|Enrutamiento y JWT Filter| Proj
    GW -->|Enrutamiento y JWT Filter| Notif
    GW -->|Enrutamiento y JWT Filter| AISrv
    
    Auth --> DB
    Proj --> DB
    Proj --> Search
    Proj -->|Produce eventos| Queue
    
    Queue -->|Consume eventos| Notif
    Notif --> DB
    Notif -->|WebSocket Push| UI
    Notif -->|Mail Sender| SMTP
    
    AISrv --> Cache
    AISrv --> LLM
