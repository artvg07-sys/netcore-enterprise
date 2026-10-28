# Registro de decisiones técnicas
## Proyecto: NetCore Enterprise

**Autor:** Arturo Villalba García  
**Ciclo formativo:** 2.º ASIR  
**Curso académico:** 2026/2027  
**Estado del documento:** En desarrollo  
**Versión:** 1.0

---

## 1. Objetivo del documento

Este documento recoge las principales decisiones técnicas adoptadas durante el diseño, desarrollo y despliegue de NetCore Enterprise.

Su finalidad es justificar las tecnologías seleccionadas, explicar las alternativas consideradas y mantener un registro de los cambios importantes realizados durante el proyecto.

El registro se actualizará cuando se adopten nuevas decisiones relevantes o se modifiquen las existentes.

## 2. Descripción general del proyecto

NetCore Enterprise es un proyecto de infraestructura informática empresarial que integra virtualización, administración de sistemas, redes, seguridad, monitorización, automatización y una plataforma web de gestión centralizada.

El escenario propuesto representa una empresa ficticia, NetCore Solutions, con diferentes departamentos y necesidades de administración de recursos, control de acceso, disponibilidad de servicios y protección de la información.

El proyecto se divide en dos componentes principales:

- **NetCore Infrastructure:** infraestructura virtualizada, redes, servidores, servicios corporativos, seguridad, monitorización y copias de seguridad.
- **NetCore Control:** plataforma web propia para consultar el estado de la infraestructura, visualizar métricas, registrar incidencias y ejecutar determinadas operaciones administrativas autorizadas.

La solución se desarrollará progresivamente, priorizando una infraestructura funcional y segura antes de incorporar funcionalidades avanzadas.

---

## 3. Registro de decisiones

### DEC-001. Sistema operativo del equipo anfitrión

**Estado:** Propuesta inicial.  
**Fecha:** 28/10/2026.

**Decisión**

Mantener Ubuntu como sistema operativo anfitrión para desarrollar y ejecutar el laboratorio de NetCore Enterprise.

**Justificación**

El equipo anfitrión proporciona el entorno desde el que se administrarán las máquinas virtuales, los contenedores, las herramientas de desarrollo y los repositorios del proyecto.

Mantener el sistema operativo actual evita reinstalaciones innecesarias y permite aprovechar las herramientas disponibles en el equipo.

**Alternativas consideradas**

- Instalar un sistema operativo específico para virtualización, como Proxmox VE.
- Mantener Ubuntu y utilizar herramientas de virtualización sobre él.

**Motivo de la elección**

Se selecciona inicialmente Ubuntu porque permite trabajar con KVM, QEMU, libvirt y Docker sin sustituir el sistema operativo del equipo.

**Consecuencias**

- Se aprovechará el hardware existente.
- Se podrán ejecutar máquinas virtuales y contenedores en el mismo equipo.
- Será necesario controlar el consumo de memoria y procesador.
- El laboratorio dependerá de un único equipo físico.

**Revisión pendiente**

Comprobar la configuración actual del sistema y la disponibilidad de las funciones de virtualización del procesador antes de instalar o modificar componentes.

---

### DEC-002. Tecnología de virtualización

**Estado:** Propuesta inicial.  
**Fecha:** 28/10/2026.

**Decisión**

Utilizar KVM/QEMU con libvirt para crear y administrar las máquinas virtuales del laboratorio.

**Justificación**

La virtualización permitirá representar diferentes servidores y componentes de una infraestructura empresarial sin necesidad de disponer de varios equipos físicos.

KVM/QEMU permite ejecutar máquinas virtuales Linux, mientras que libvirt proporciona herramientas para administrarlas mediante comandos, XML y otras interfaces.

**Alternativas consideradas**

- Proxmox VE.
- VirtualBox.
- VMware.
- KVM/QEMU con libvirt.

**Motivo de la elección**

KVM/QEMU con libvirt ofrece integración con Linux y permite practicar tareas de administración de máquinas virtuales, redes virtuales, almacenamiento y automatización.

**Consecuencias**

- Será posible crear y administrar máquinas virtuales desde la terminal.
- Se podrán automatizar operaciones mediante scripts.
- Será necesario configurar correctamente las redes y el almacenamiento virtual.
- El número de máquinas virtuales simultáneas dependerá de los recursos disponibles.

**Revisión pendiente**

Verificar que KVM/QEMU y libvirt funcionan correctamente en el equipo antes de desplegar la infraestructura.

---

### DEC-003. Arquitectura inicial de máquinas virtuales

**Estado:** Propuesta inicial.  
**Fecha:** 28/10/2026.

**Decisión**

Comenzar con una arquitectura reducida de tres máquinas virtuales principales y ampliarla cuando sea necesario.

**Distribución inicial propuesta**

| Máquina virtual | Función principal |
|---|---|
| FW01 | Enrutamiento, filtrado de tráfico y control de comunicaciones. |
| SRV01 | Servicios de infraestructura, como DNS e identidad centralizada. |
| SRV02 | Aplicaciones en contenedores y componentes de NetCore Control. |

Esta distribución es provisional. Los servicios podrán separarse posteriormente en máquinas virtuales adicionales si los requisitos de seguridad, rendimiento o mantenimiento lo justifican.

**Justificación**

Una arquitectura inicial reducida permite desarrollar y probar los componentes esenciales sin consumir todos los recursos del equipo desde el principio.

**Consecuencias**

- Menor consumo inicial de memoria y almacenamiento.
- Menor complejidad durante las primeras fases.
- Posibilidad de ampliar la infraestructura de forma progresiva.
- Necesidad de documentar qué servicios comparten máquina virtual.

**Revisión pendiente**

Definir la distribución definitiva después de comprobar los recursos disponibles y los requisitos de cada servicio.

---

### DEC-004. Uso de Docker y Docker Compose

**Estado:** Propuesta inicial.  
**Fecha:** 28/10/2026.

**Decisión**

Utilizar Docker y Docker Compose para desplegar y administrar los servicios que se beneficien de una arquitectura basada en contenedores.

**Justificación**

Los contenedores permiten empaquetar aplicaciones y sus dependencias, facilitar su despliegue y mantener configuraciones reproducibles.

Docker Compose permitirá definir los servicios, redes y volúmenes de las aplicaciones mediante archivos declarativos.

**Alternativas consideradas**

- Instalar todos los servicios directamente en las máquinas virtuales.
- Crear una máquina virtual independiente para cada aplicación.
- Utilizar contenedores Docker administrados mediante Docker Compose.

**Motivo de la elección**

Se utilizarán contenedores para simplificar el despliegue y mantenimiento de las aplicaciones, manteniendo las máquinas virtuales como base de la infraestructura.

**Consecuencias**

- Configuración reproducible de los servicios.
- Mayor facilidad para actualizar y reconstruir aplicaciones.
- Necesidad de gestionar correctamente redes, volúmenes, credenciales y permisos.
- Los contenedores no sustituirán las medidas de seguridad necesarias en el sistema anfitrión.

**Revisión pendiente**

Definir qué servicios se ejecutarán en contenedores y cuáles se instalarán directamente en las máquinas virtuales.

---

### DEC-005. Diseño de red y segmentación

**Estado:** Propuesta inicial.  
**Fecha:** 28/10/2026.

**Decisión**

Diseñar una red lógica segmentada por departamentos y funciones, con reglas de comunicación controladas.

**Segmentos propuestos**

| VLAN | Nombre | Subred propuesta |
|---|---|---|
| 10 | Administración | 192.168.10.0/24 |
| 20 | Recursos Humanos | 192.168.20.0/24 |
| 30 | Informática | 192.168.30.0/24 |
| 40 | Servidores | 192.168.40.0/24 |
| 50 | Invitados | 192.168.50.0/24 |
| 60 | Gestión | 192.168.60.0/24 |

**Justificación**

La segmentación permite separar los equipos según su función y aplicar diferentes políticas de acceso.

Por ejemplo, la red de invitados no debería poder acceder directamente a los servicios de administración ni a los sistemas internos que contienen información sensible.

**Consideraciones técnicas**

Las subredes anteriores son una propuesta de direccionamiento, no una configuración ya desplegada.

Será necesario comprobar cómo implementar la segmentación en el laboratorio. Dependiendo de la arquitectura seleccionada, se podrán utilizar redes virtuales independientes, enrutamiento entre redes y, si se configura adecuadamente, VLAN etiquetadas.

**Consecuencias**

- Mayor control sobre las comunicaciones.
- Posibilidad de aplicar reglas de filtrado específicas.
- Necesidad de configurar rutas, puertas de enlace y reglas de firewall.
- Mayor complejidad de configuración y diagnóstico.

**Revisión pendiente**

Diseñar las reglas de comunicación permitidas y prohibidas para cada segmento.

---

### DEC-006. Plataforma web propia

**Estado:** Propuesta inicial.  
**Fecha:** 28/10/2026.

**Decisión**

Desarrollar NetCore Control como una aplicación web propia que centralice determinadas operaciones de consulta y administración de la infraestructura.

**Tecnologías propuestas**

- Backend: Python y FastAPI.
- Base de datos: PostgreSQL.
- Frontend: interfaz web adaptada a las necesidades del proyecto.
- Integración con servicios existentes mediante API, consultas controladas y herramientas de administración.

**Justificación**

La plataforma permitirá integrar los distintos componentes del proyecto en una única interfaz y demostrar conocimientos de programación aplicada a la administración de sistemas.

No se pretende sustituir herramientas especializadas como Prometheus, Grafana o Ansible, sino integrarlas para facilitar determinadas tareas.

**Funcionalidades previstas**

1. Panel general con el estado de los servicios.
2. Inventario de servidores y máquinas virtuales.
3. Visualización de métricas de monitorización.
4. Consulta del estado de los contenedores.
5. Ejecución de tareas administrativas previamente definidas.
6. Registro de incidencias.
7. Historial de acciones administrativas.
8. Consulta del estado de las copias de seguridad.

**Seguridad**

Las operaciones administrativas deberán estar protegidas mediante autenticación, autorización por roles y registro de auditoría.

La interfaz web no tendrá acceso directo e ilimitado al socket de Docker ni permitirá ejecutar comandos arbitrarios como administrador. Las acciones se expondrán mediante un backend con permisos restringidos y operaciones expresamente autorizadas.

**Consecuencias**

- Mayor integración entre los componentes.
- Necesidad de desarrollar y probar una aplicación propia.
- Aumento de la complejidad del proyecto.
- Necesidad de diseñar medidas de seguridad para la plataforma.

**Revisión pendiente**

Definir el alcance mínimo funcional de la plataforma antes de desarrollar sus módulos avanzados.

---

### DEC-007. Monitorización de la infraestructura

**Estado:** Propuesta inicial.  
**Fecha:** 28/10/2026.

**Decisión**

Utilizar Prometheus y Grafana como herramientas principales de monitorización y visualización de métricas.

**Justificación**

La monitorización permitirá observar el comportamiento de los servidores y detectar problemas de disponibilidad o consumo de recursos.

Prometheus recopilará las métricas disponibles y Grafana permitirá representarlas mediante paneles.

**Objetivos**

- Supervisar el consumo de CPU y memoria.
- Controlar el espacio disponible en disco.
- Consultar el estado de los servidores.
- Supervisar los servicios y contenedores seleccionados.
- Configurar alertas para incidencias relevantes.

**Consecuencias**

- Mayor capacidad para diagnosticar problemas.
- Disponibilidad de datos para las pruebas del proyecto.
- Necesidad de configurar exportadores, objetivos de monitorización y reglas de alerta.
- Consumo adicional de recursos.

**Revisión pendiente**

Seleccionar las métricas y alertas que se utilizarán para evaluar objetivamente el funcionamiento de la infraestructura.

---

### DEC-008. Automatización mediante Ansible

**Estado:** Propuesta inicial.  
**Fecha:** 28/10/2026.

**Decisión**

Utilizar Ansible para automatizar tareas de configuración y administración de los sistemas que formen parte del laboratorio.

**Justificación**

La automatización permite reducir las tareas manuales repetitivas, mantener configuraciones coherentes y facilitar la reconstrucción del entorno.

**Tareas previstas**

- Comprobación de conectividad.
- Instalación y configuración de paquetes.
- Aplicación de configuraciones comunes.
- Comprobaciones del estado de los servicios.
- Ejecución de tareas de mantenimiento previamente definidas.

**Seguridad**

Las tareas deberán ejecutarse con los permisos estrictamente necesarios. La plataforma web no podrá lanzar playbooks arbitrarios ni acceder sin restricciones a las credenciales de administración.

**Consecuencias**

- Mayor reproducibilidad.
- Reducción de errores manuales.
- Necesidad de gestionar inventarios, variables, claves y permisos.
- Necesidad de probar los playbooks antes de utilizarlos sobre los sistemas de destino.

**Revisión pendiente**

Determinar las primeras tareas automatizables y desarrollar playbooks progresivamente.

---

### DEC-009. Copias de seguridad y recuperación

**Estado:** Propuesta inicial.  
**Fecha:** 28/10/2026.

**Decisión**

Incorporar un sistema de copias de seguridad y procedimientos documentados de restauración.

**Tecnologías candidatas**

Restic o BorgBackup, seleccionando una de ellas después de evaluar los requisitos del laboratorio.

**Justificación**

Las copias de seguridad son necesarias para recuperar configuraciones y datos después de errores, fallos de aplicaciones o pérdida de información.

**Objetivos**

- Definir qué información debe protegerse.
- Automatizar la creación de copias.
- Verificar que las copias se completan correctamente.
- Documentar el procedimiento de restauración.
- Realizar pruebas reales de recuperación.

**Seguridad**

Las credenciales y claves de cifrado no deberán almacenarse en el repositorio de código.

**Limitación**

Al ejecutarse inicialmente sobre un único equipo físico, el proyecto no contará con protección completa frente a la pérdida total de ese equipo. Esta limitación deberá documentarse y, si los recursos lo permiten, se estudiará una copia externa adicional.

**Revisión pendiente**

Elegir la herramienta definitiva y establecer una política de retención y restauración.

---

### DEC-010. Gestión del código y documentación

**Estado:** Decisión inicial.  
**Fecha:** 28/10/2026.

**Decisión**

Utilizar Git y GitHub para versionar el código, la configuración reproducible y la documentación del proyecto.

**Justificación**

El control de versiones permitirá identificar cambios, recuperar versiones anteriores y documentar la evolución del proyecto.

**Criterios de organización**

- Mantener una estructura de directorios coherente.
- Realizar commits con mensajes descriptivos.
- Documentar las decisiones técnicas importantes.
- Separar la documentación de las configuraciones y del código.
- Evitar subir contraseñas, claves privadas, archivos `.env` con secretos, imágenes de máquinas virtuales y copias de seguridad reales.

**Consecuencias**

- Historial de cambios.
- Mayor facilidad para revisar el trabajo.
- Posibilidad de reconstruir partes del proyecto a partir de los archivos versionados.
- Necesidad de mantener una política adecuada de protección de credenciales.

**Limitación**

Git no sustituye a un sistema de copias de seguridad ni guarda automáticamente el estado completo de las máquinas virtuales.

---

## 4. Criterios generales para tomar decisiones

Las decisiones técnicas se evaluarán teniendo en cuenta los siguientes criterios:

1. **Adecuación a ASIR:** deben permitir demostrar competencias de administración de sistemas, redes, servicios y seguridad.
2. **Viabilidad técnica:** deben poder implementarse con el hardware y los conocimientos disponibles.
3. **Viabilidad temporal:** deben ajustarse al calendario académico y al tiempo efectivo de desarrollo.
4. **Seguridad:** deben aplicar el principio de mínimo privilegio y proteger la información.
5. **Coste:** se priorizarán tecnologías gratuitas y de código abierto cuando sean adecuadas.
6. **Mantenibilidad:** las configuraciones deben estar documentadas y ser comprensibles.
7. **Reproducibilidad:** siempre que sea posible, los despliegues deberán poder repetirse mediante archivos de configuración y automatización.
8. **Evaluación objetiva:** las funcionalidades importantes deberán disponer de pruebas verificables.

## 5. Limitaciones conocidas

En la fase inicial se identifican las siguientes limitaciones:

- El proyecto se desarrollará principalmente sobre un único equipo físico.
- La memoria RAM y el almacenamiento disponibles condicionarán el número de máquinas virtuales activas.
- No se podrá demostrar alta disponibilidad real entre varios servidores físicos sin infraestructura adicional.
- La plataforma web deberá desarrollarse de forma progresiva para evitar que su complejidad impida terminar la infraestructura.
- Algunas funcionalidades se implementarán únicamente si se completan los objetivos prioritarios.

Estas limitaciones se revisarán durante el desarrollo y se tendrán en cuenta al evaluar los resultados.

## 6. Orden de prioridades

El desarrollo seguirá este orden general:

**Prioridad 1. Infraestructura básica**
- Preparación del entorno de virtualización.
- Creación de las máquinas virtuales.
- Configuración de redes y conectividad.
- Despliegue de los servicios esenciales.

**Prioridad 2. Seguridad y administración**
- Segmentación de red y filtrado.
- Gestión de identidades y permisos.
- Configuración segura de los servicios.
- HTTPS y protección de credenciales.

**Prioridad 3. Monitorización y automatización**
- Recopilación de métricas.
- Paneles y alertas.
- Automatización de tareas repetitivas.
- Registro de resultados.

**Prioridad 4. NetCore Control**
- Backend y base de datos.
- Autenticación y roles.
- Interfaz web.
- Integración con los servicios de monitorización y automatización.

**Prioridad 5. Validación y documentación final**
- Pruebas funcionales y de seguridad.
- Simulación de fallos.
- Pruebas de recuperación.
- Análisis de resultados.
- Memoria y preparación de la defensa.

## 7. Historial de cambios

| Versión | Fecha | Descripción |
|---|---|---|
| 1.0 | 28/10/2026 | Creación inicial del registro de decisiones técnicas y definición de la arquitectura propuesta. |

---

## 8. Nota de mantenimiento

Este documento representa las decisiones y propuestas iniciales del proyecto. Las tecnologías y configuraciones indicadas no deben considerarse desplegadas ni verificadas hasta que se hayan implementado y probado.

Cuando una decisión cambie, se actualizará su apartado indicando la fecha, el motivo del cambio, las alternativas consideradas y las consecuencias técnicas.

Las decisiones que se descarten también podrán conservarse en el registro cuando su análisis aporte información relevante para justificar la arquitectura definitiva.
