# netcore-enterprise
Para el trabajo de fin de grado de 2 de asir


# NetCore Enterprise
**Proyecto final de 2.º ASIR — Curso 2026/2027**

## 1. Descripción
NetCore Enterprise es un proyecto de administración de sistemas que tiene como objetivo diseñar y desplegar una infraestructura informática empresarial segura, automatizada y monitorizada mediante virtualización, contenedores y software libre.

El proyecto incluye una infraestructura de laboratorio y una plataforma web propia denominada **NetCore Control** para centralizar información sobre servidores, servicios, incidencias y operaciones administrativas autorizadas.

---

## 2. Objetivos principales
* Diseñar una arquitectura de red segmentada.
* Desplegar servidores Linux mediante virtualización.
* Configurar servicios de infraestructura e identidad.
* Desplegar aplicaciones con Docker Compose.
* Automatizar tareas mediante Ansible.
* Monitorizar sistemas y servicios con Prometheus y Grafana.
* Implementar copias de seguridad y probar su restauración.
* Desarrollar una plataforma web para consultar y gestionar información de la infraestructura.
* Documentar y validar el funcionamiento mediante pruebas reproducibles.

---

## 3. Tecnologías previstas
* **Sistemas Operativos:** Ubuntu, Debian o Ubuntu Server
* **Virtualización:** KVM/QEMU y libvirt
* **Contenedores:** Docker y Docker Compose
* **Identidad y Red:** OpenLDAP, Nginx
* **Automatización:** Ansible
* **Monitorización:** Prometheus y Grafana
* **Desarrollo y Base de Datos:** Python (FastAPI), PostgreSQL
* **Copias de seguridad:** Restic
* **Control de versiones:** Git y GitHub

*(Las versiones definitivas se seleccionarán y documentarán durante el diseño técnico).*

---

## 4. Estructura del repositorio
```text
.
├── docs/           # Documentación, análisis, arquitectura, pruebas y memoria
├── infrastructure/ # Virtualización, redes y automatización
├── services/       # Configuración de los servicios de infraestructura
├── platform/       # Desarrollo de NetCore Control
├── compose/        # Despliegues mediante Docker Compose
├── scripts/        # Scripts auxiliares
├── tests/          # Pruebas del proyecto
└── backups/        # Documentación y ejemplos no sensibles relacionados con las copias de seguridad