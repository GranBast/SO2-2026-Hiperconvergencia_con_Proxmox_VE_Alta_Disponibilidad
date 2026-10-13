# SO2-2026-SISTEMA_MONITOREO_INFRAESTRUCTURA_TI
## 1. Descripción del proyecto

El Sistema de Monitoreo de Infraestructura TI busca centralizar la supervisión de dispositivos de una red y detectar fallas con mayor rapidez.

Actualmente, las fallas pueden descubrirse cuando un usuario informa que un equipo perdió la conexión. Esto retrasa el diagnóstico y no siempre permite conocer cuándo comenzó una interrupción ni cuánto tiempo duró.

El sistema permitirá consultar el estado de los dispositivos desde un dashboard web y conservar un historial de sus caídas y recuperaciones.

## 2. Objetivo general

Desarrollar un sistema web que centralice y automatice el monitoreo de la disponibilidad y el rendimiento de los dispositivos de una infraestructura TI.

## 3. Alcance del MVP

La primera versión prioriza las funcionalidades necesarias para detectar y registrar fallas:

- Registrar, editar, consultar y eliminar dispositivos.
- Validar las direcciones IP y evitar duplicados.
- Comprobar periódicamente la disponibilidad mediante ICMP/Ping.
- Mostrar el estado en línea o fuera de línea, la latencia y la pérdida de paquetes.
- Registrar las caídas y recuperaciones con fecha y hora.
- Mostrar los dispositivos en un dashboard web que se actualice automáticamente.
- Generar alertas visibles para el administrador.
- Proteger el acceso mediante autenticación y permisos básicos.

**Criterio principal de éxito:** detectar una caída en menos de un minuto, generar una alerta, actualizar el dashboard y guardar el evento de caída y recuperación.

## 4. Funcionalidades complementarias

SNMP para consultar interfaces y tráfico, reportes avanzados y gráficos adicionales se incorporarán solo si el tiempo y la validación técnica lo permiten. Las aplicaciones móviles y las notificaciones por servicios externos quedan fuera del MVP.

## 5. Tecnologías y entorno

- Lenguaje y plataforma: C# y .NET 10.
- Interfaz web: ASP.NET Core.
- Monitoreo de disponibilidad: ICMP/Ping.
- Monitoreo complementario: SNMP, sujeto a validación.
- Entorno de pruebas: servidor Proxmox y máquinas virtuales Linux.
- Control de versiones y respaldo: Git y GitHub.

La tecnología concreta de base de datos y el mecanismo de actualización del dashboard deben documentarse cuando el equipo cierre esas decisiones.

## 6. Equipo y responsabilidades

- **Edilson Lazarte García:** análisis, requisitos, coordinación y documentación.
- **Josías Gabriel Arze Giron:** lógica de monitoreo, ICMP/SNMP, procesamiento de datos y alertas.
- **Luis Fernando Callejas:** interfaz, dashboard, datos y pruebas de usabilidad.

El equipo coordina los avances y los bloqueos en una reunión semanal.

## 7. Plan de trabajo

El proyecto se organiza en análisis, diseño, desarrollo, pruebas y documentación. El Gantt contempla 240 horas estimadas y una entrega prevista para noviembre de 2026.

Orden de trabajo recomendado:

1. Confirmar requisitos, arquitectura y estructura de datos.
2. Verificar el entorno de pruebas y la conectividad.
3. Implementar y probar el monitoreo ICMP.
4. Integrar la persistencia del historial y las alertas.
5. Desarrollar el dashboard y la autenticación.
6. Ejecutar pruebas integrales y corregir errores.
7. Preparar la documentación y la demostración final.
8. Validar SNMP y otras mejoras sin bloquear el MVP.

## 8. Preparación del entorno

Antes de desarrollar, cada integrante debe:

1. Instalar Git y el SDK de .NET 10.
2. Clonar el repositorio.
3. Confirmar el acceso al servidor Proxmox o al entorno de pruebas acordado.
4. Verificar la conectividad con los dispositivos de prueba.
5. Revisar los requisitos, el Gantt y las tareas asignadas.
6. Consultar al equipo antes de cambiar decisiones compartidas de arquitectura o datos.

Los comandos exactos para compilar, ejecutar y probar se añadirán cuando estén creados los proyectos y se haya definido su estructura.

## 9. Criterios de aceptación

La versión se considerará preparada para la demostración cuando:

- Se puedan registrar y administrar dispositivos.
- El sistema detecte una caída en menos de un minuto en el entorno de prueba.
- El dashboard muestre al menos cinco dispositivos y se actualice sin recargar la página.
- Se genere una alerta visible al producirse una caída.
- El historial registre las caídas y recuperaciones con fecha y hora.
- Se pueda consultar la latencia y contrastarla con el comando `ping`.
- Las funciones protegidas requieran iniciar sesión.
- La demostración se ejecute en el entorno Proxmox del laboratorio.

## 10. Gestión del proyecto

El código y los cambios deben respaldarse en GitHub. Los bloqueos, las decisiones técnicas y las tareas pendientes se revisarán semanalmente. Si una funcionalidad complementaria pone en riesgo la entrega, se priorizará el alcance del MVP.

## 11. Documentación

La carpeta `documentacion/` contiene las presentaciones y el diagrama de Gantt del proyecto. Antes de la entrega final, se actualizará con las decisiones técnicas, las instrucciones de instalación, los resultados de las pruebas y las limitaciones conocidas.
