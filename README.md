# SO2-2026-HIPERCONVERGENCIA_PROXMOX_HA
## 1. Descripción del proyecto

El proyecto implementa un clúster hiperconvergente de Proxmox VE con alta disponibilidad (HA), para que los servicios de una infraestructura sobrevivan a la caída de un servidor.

Actualmente, cuando un único servidor de virtualización se apaga o se daña, todas las máquinas virtuales que aloja dejan de funcionar y alguien debe restaurarlas a mano. Esto provoca interrupciones largas, dependencia de una persona concreta y pérdida de los datos posteriores al último respaldo.

El clúster replicará las máquinas virtuales entre dos nodos y, si un nodo falla, el gestor HA reiniciará la máquina protegida en el otro nodo de forma automática. La recuperación toma minutos y no es instantánea: la máquina se reinicia desde la última réplica.

## 2. Objetivo general

Implementar un clúster hiperconvergente de Proxmox VE con alta disponibilidad, en el que las máquinas virtuales se repliquen entre nodos y se reinicien automáticamente en otro nodo cuando uno falle.

## 3. Alcance del MVP

La primera versión prioriza lo necesario para demostrar la recuperación automática ante la caída de un nodo:

- Dos nodos Proxmox VE anidados dentro de un mismo equipo físico, formando un clúster.
- QDevice (dispositivo de quórum externo) para mantener el quórum con dos nodos.
- Almacenamiento ZFS con replicación periódica de una máquina de prueba (Alpine Linux).
- Esa máquina registrada como recurso HA.
- Una página web simple (nginx) dentro de la máquina, que muestra la hora y su tiempo encendida.
- Migración en vivo manual entre los nodos.
- Pruebas de caída y recuperación de un nodo, con medición de tiempos.

**Criterio principal de éxito:** al apagar de forma abrupta el nodo que ejecuta la máquina protegida, ella se inicia sola en el otro nodo en menos de 3 minutos (valor a confirmar con las mediciones) y el servicio web vuelve a responder.

## 4. Funcionalidades complementarias

La base de datos de latidos para comprobar la persistencia de los datos, la prueba ampliada con VPN entre los equipos de los integrantes, Ceph como almacenamiento sincrónico, un dashboard propio y las notificaciones de eventos HA se incorporarán solo si el tiempo y la validación técnica lo permiten.

## 5. Tecnologías y entorno

- Virtualización: Proxmox VE (KVM y contenedores LXC), con virtualización anidada.
- Clúster y quórum: Corosync y QDevice (corosync-qnetd).
- Almacenamiento y replicación: ZFS y replicación de Proxmox.
- Alta disponibilidad: gestor HA de Proxmox, con watchdog.
- Servicio de demostración: Alpine Linux y nginx.
- Base de datos de demostración (complementaria): SQLite, propuesta.
- Control de versiones y respaldo: Git y GitHub.

| Máquina | Sistema operativo | Rol |
| --- | --- | --- |
| Anfitrión | Proxmox VE | Aloja las máquinas del laboratorio; no forma parte del clúster. |
| nodo-1 | Proxmox VE (anidado) | Nodo del clúster. |
| nodo-2 | Proxmox VE (anidado) | Nodo del clúster. |
| qdevice | Debian o Alpine mínima | Aporta el tercer voto de quórum. |
| web-ha | Alpine Linux | Máquina protegida por HA con el servicio web. |

El intervalo de replicación, el tiempo máximo de recuperación aceptable y la tecnología de la base de datos de demostración deben documentarse cuando el equipo cierre esas decisiones.

## 6. Equipo y responsabilidades

- **Edilson Lazarte García:** análisis, requisitos, topología, coordinación y documentación.
- **Josías Gabriel Arze Giron:** infraestructura, nodos anidados, clúster, QDevice y alta disponibilidad.
- **Luis Fernando Callejas:** almacenamiento ZFS, replicación, servicio de demostración y pruebas de fallo.

El equipo coordina los avances y los bloqueos en una reunión semanal.

## 7. Plan de trabajo

El proyecto se organiza en análisis, diseño, implementación, pruebas y documentación. El Gantt contempla 160 horas estimadas y una entrega final prevista para el 18 de diciembre de 2026.

Orden de trabajo recomendado:

1. Confirmar requisitos, topología (nombres de nodo, direcciones IP y RAM) y diseño.
2. Verificar la virtualización anidada y el entorno base.
3. Crear los nodos Proxmox anidados y formar el clúster.
4. Configurar el QDevice y verificar el quórum.
5. Configurar ZFS y la replicación.
6. Configurar HA con la máquina de prueba y el servicio web.
7. Ejecutar las pruebas de fallo y recuperación, y registrar los tiempos.
8. Preparar la documentación y la demostración final.
9. Validar las mejoras (VPN, base de datos, Ceph) sin bloquear el MVP.

## 8. Preparación del entorno

Antes de trabajar, cada integrante debe:

1. Instalar Git.
2. Clonar el repositorio.
3. Confirmar el acceso al panel web (puerto 8006) y al SSH del anfitrión Proxmox.
4. Verificar que la virtualización anidada esté activa en el anfitrión.
5. Revisar los requisitos, el Gantt y las tareas asignadas.
6. Consultar al equipo antes de cambiar nombres de nodo, direcciones IP o almacenamiento, porque no se pueden modificar después de formar el clúster.

Los comandos exactos para crear los nodos, el clúster, el QDevice, ZFS, la replicación y HA se añadirán a la documentación a medida que se ejecuten.

## 9. Criterios de aceptación

La versión se considerará preparada para la demostración cuando:

- El panel Datacenter muestre los dos nodos en línea y el quórum activo con el voto del QDevice.
- La máquina de prueba aparezca replicada en el segundo nodo con sincronizaciones dentro del intervalo configurado.
- La migración en vivo cambie la máquina de nodo y el servicio siga respondiendo.
- Al apagar el nodo que ejecuta la máquina, ella se inicie sola en el otro en menos de 3 minutos.
- La página web responda tras el reinicio y su tiempo encendida vuelva a cero.
- Al encender de nuevo el nodo caído, el clúster recupere su estado normal sin máquinas duplicadas.
- La demostración se ejecute en el entorno Proxmox del laboratorio.

## 10. Gestión del proyecto

Los scripts, las configuraciones y los cambios deben respaldarse en GitHub. Los bloqueos, las decisiones técnicas y las tareas pendientes se revisarán semanalmente. Si una funcionalidad complementaria pone en riesgo la entrega, se priorizará el alcance del MVP.

## 11. Documentación

La carpeta `documentacion/` contiene las presentaciones, el cuestionario de aprobación y el diagrama de Gantt del proyecto. Antes de la entrega final, se actualizará con las decisiones técnicas, la guía de instalación paso a paso, los tiempos de recuperación medidos y las limitaciones conocidas.
