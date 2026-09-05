# Plan de mejoras del proyecto

Este documento conserva el paso a paso propuesto en la auditoría de la notebook, compartida el 2026-09-04. Es una lista de trabajo pendiente, no un registro de correcciones realizadas.

El historial de análisis, cambios y verificaciones se mantiene en [CHANGELOG.md](CHANGELOG.md). Al completar un paso, marcarlo aquí y registrar allí qué se cambió y cómo se verificó.

## Paso a paso de la auditoría original

### 1. Corregir el SQL de penalización de clan

- [ ] Corregir el UPDATE de `AdminEditChar.java` para asignar cero a `clan_create_expiry_time` o `clan_join_expiry_time`.
- [ ] Comprobar ambas operaciones con personajes desconectados y preservar el comportamiento para conectados.
- [ ] Registrar la causa completa si falla la operación SQL.

### 2. Cerrar el bypass de rutas HTML

- [ ] Restringir los destinos HTML de dungeon a una lista de páginas permitidas.
- [ ] Aplicar una restricción equivalente al bypass de merchant, identificado durante el contraste local.
- [ ] Revisar las rutas de `playerHelp()` y `playerBook()`.
- [ ] Verificar que las páginas válidas funcionen y que no se puedan cargar páginas fuera del directorio autorizado.

### 3. Retirar las credenciales del paquete de producción

- [ ] Separar las configuraciones de ejemplo de las configuraciones privadas.
- [ ] Evitar el uso de la cuenta administradora de MariaDB para los servicios.
- [ ] Definir los permisos necesarios para Login Server, Game Server y herramientas.
- [ ] Excluir la configuración privada del versionado y de la distribución.

### 4. Revisar autenticación, creación de cuentas y puertos

- [ ] Documentar el formato actual de contraseñas y estudiar una migración compatible a un hash adecuado para contraseñas.
- [ ] Resolver la incompatibilidad entre `VoicedPassword.java` y `LoginController.java`, detectada en esta PC: usan hashes distintos.
- [ ] Verificar creación de cuentas, login y cambio de contraseña como un mismo flujo.
- [ ] Revisar `AutoCreateAccounts`, límites de intentos y duración de bloqueos según el despliegue previsto.
- [ ] Revisar interfaces y puertos expuestos para cada servicio.

### 5. Corregir la concurrencia de FloodProtectedListener

- [ ] Proteger el mapa y las actualizaciones de los contadores compartidos.
- [ ] Garantizar consistencia entre aceptación y cierre de conexiones.
- [ ] Comprobar conexiones simultáneas, desconexiones y rechazo por flood.

### 6. Reemplazar los pools cacheados del teleport

- [ ] Sustituir las tareas que duermen 450 ms por planificación mediante el scheduler compartido.
- [ ] Revisar tanto `RequestBypassToServer.java` como `L2TeleporterInstance.java`.
- [ ] Comprobar el estado del personaje al ejecutar la tarea diferida y preservar las restricciones del teleport.
- [ ] Revisar el cierre de los ejecutores que permanezcan en uso.

### 7. Reparar y hacer reproducible el build de Ant

- [ ] Definir correctamente propiedades y dependencias entre targets.
- [ ] Permitir compilar desde cero sin depender de `La2Interlude.jar` como entrada.
- [ ] Evitar perder el JAR previo si una compilación falla.
- [ ] Separar dependencias de artefactos generados.
- [ ] Recrear la distribución sin conservar entradas obsoletas del ZIP.
- [ ] Excluir secretos, logs, cachés e identificadores locales del paquete.
- [ ] Verificar compilación y contenido de la distribución.

### 8. Añadir CI y pruebas mínimas

- [ ] Automatizar la compilación en CI.
- [ ] Incorporar pruebas relevantes para login y SQL.
- [ ] Cubrir operaciones críticas de inventario, comercio y recompensas, especialmente duplicaciones o pérdidas.
- [ ] Verificar arranque con una base de datos de prueba y esquema limpio.

### 9. Separar los grandes routers y managers por dominio

- [ ] Comenzar por handlers específicos para las responsabilidades de `RequestBypassToServer`.
- [ ] Identificar separaciones acotadas en clases grandes como `Player`, `Creature` y `Config`.
- [ ] Mantener el comportamiento existente y verificar cada extracción antes de continuar.
- [ ] Mejorar el tratamiento de excepciones en los bloques intervenidos.

### 10. Añadir migraciones versionadas para MariaDB

- [ ] Definir cómo identificar la versión del esquema y aplicar actualizaciones ordenadas.
- [ ] Registrar cada modificación del esquema mediante una migración versionada.
- [ ] Comprobar instalación limpia y actualización desde una versión anterior.
- [ ] Documentar respaldo y recuperación ante una migración fallida.

## Cómo continuar entre equipos

Los avances se commitean en `develop`. `main` se conserva como rama principal; no integrar cambios allí sin una indicación explícita. Los pushes los realiza el usuario.

1. Trabajar en `develop`, actualizarla desde el remoto cuando esté publicada y revisar los cambios locales antes de integrar.
2. Leer este plan y las últimas entradas del changelog.
3. Elegir un paso pendiente y revisar el código actual antes de modificarlo.
4. Implementar y ejecutar verificaciones acordes al cambio.
5. Marcar solamente las tareas realmente completadas y actualizar el changelog con resultados y limitaciones.
6. Commitear el avance junto con su documentación en `develop`. El usuario se encarga de subirlo al remoto.

## Estado inicial

Los diez pasos siguen pendientes. La corrección de inicialización de MariaDB en el panel de registro de Game Server ya fue guardada en el commit `99c4db69`; es un cambio previo e independiente de estos pasos. El contraste local confirmó que el proyecto compila, pero no incluyó pruebas de ejecución contra MariaDB.
