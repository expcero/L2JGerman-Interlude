# Registro de análisis y cambios

Este archivo conserva el contexto del proyecto entre equipos. Actualizarlo en cada avance significativo y commitearlo junto con los cambios correspondientes. Los hallazgos pendientes no se consideran corregidos hasta registrar su implementación y validación.

Para nuevas entradas, usar fecha, cambio o análisis, archivos relevantes, verificaciones y pendientes. No registrar contraseñas, identificadores privados ni datos de cuentas. Mantener las entradas más recientes primero.

## 2026-09-04 — Flujo de ramas

- Acordar `develop` como rama para los commits de trabajo y conservar `main` como rama principal. No integrar en `main` sin una indicación explícita.
- Los pushes quedan a cargo del usuario.
- Verificación: `develop` está activa y parte de `438d1244`, incluyendo el plan de mejoras; árbol de trabajo limpio antes de esta actualización documental.

## 2026-09-04 — Plan de mejoras separado

- Crear `PLAN_DE_MEJORAS.md` con los diez pasos de la auditoría de la notebook, conservando el orden original e incorporando las precisiones del contraste local.
- Añadir casillas de seguimiento, criterios de verificación y un procedimiento para continuar entre equipos.
- Enlazar el plan desde `README.md`. Todos los pasos comienzan pendientes; esta actualización no modifica código ni corrige los hallazgos.
- Verificación: revisión del orden contra el chat original y comprobación de formato con `git diff --check`.

## 2026-09-04 — Registro de Game Server y contraste de auditoría

### Cambios realizados

- `java/net/sf/l2j/gsregistering/ui/GameServerRegisterUI.java`: inicializar `ConnectionPool` después de cargar la configuración y antes de abrir la interfaz. Si la inicialización falla, detener el arranque con una excepción explícita. Antes, el panel podía intentar consultar MariaDB con el pool sin inicializar.
- `libs/La2Interlude.jar`: incluye la corrección del registro de Game Server.
- `.gitignore`: excluir `game/config/other/hexid-backup.txt`, respaldo local del identificador del servidor. El archivo se conserva en la máquina.
- Crear este registro para continuar el trabajo desde otros equipos.
- `README.md`: enlazar este registro desde la presentación del proyecto.

### Verificaciones

- Compilación de los 2.172 archivos Java con JDK 11, `--release 11`, UTF-8 y únicamente `libs/mariadb-java-client-3.1.4.jar` en el classpath: correcta, código de salida 0. Salida generada en una carpeta temporal, sin reemplazar el JAR local.
- Revisión de `ConnectionPool`: la inicialización comprueba la conexión y `isInitialized()` permite detectar el fallo antes de crear la interfaz.
- Inspección con `javap`: el JAR local contiene la llamada a `ConnectionPool.init()` y la comprobación posterior.
- Recompilación con `-g` (depuración habilitada, como en Ant): correcta. Las 2.893 clases del JAR coinciden byte por byte mediante SHA-256 con las clases recompiladas; no faltan clases respecto de esa compilación.
- No se probó el panel contra una base de datos ni se arrancaron Login Server o Game Server. La compilación no demuestra funcionamiento en ejecución.

### Análisis contrastado con el código actual

Base del análisis: commit `39b518d3` y cambios locales del registro de Game Server. Se contrastó una auditoría realizada previamente desde otra máquina.

| Estado | Hallazgo y ubicación | Trabajo pendiente |
| --- | --- | --- |
| Confirmado | `AdminEditChar.java`: el UPDATE para quitar penalizaciones de clan a personajes desconectados no asigna un valor a la columna. | Corregir ambas variantes y comprobar su efecto en SQL. |
| Confirmado por revisión estática | `RequestBypassToServer.java`: dungeon y merchant concatenan una ruta HTML proporcionada por el cliente. `HtmCache` no restringe la ruta al directorio esperado. | Restringir destinos y verificar rechazo de rutas fuera del directorio. El filtro de extensión limita la lectura a HTML; no se demostró lectura de archivos arbitrarios ni se hizo una prueba de red. |
| Confirmado | Configuraciones de Game Server, Login Server y herramientas contienen credenciales predeterminadas de administrador. | Separar configuración privada y revisar privilegios antes del despliegue público. |
| Confirmado | `LoginController.java` y `SQLAccountManager.java` usan `SHA` sin sal. | Diseñar una migración compatible de contraseñas. |
| Hallazgo adicional | `VoicedPassword.java` usa SHA-256, incompatible con el hash del login. El handler está registrado. | Unificar el tratamiento: actualmente rechaza la contraseña correcta de cuentas creadas por el login. |
| Confirmado | `FloodProtectedListener.java`: mapa y contadores compartidos sin sincronización; `GameServerThread` llama a la eliminación. | Proteger el ciclo completo de actualización concurrente. |
| Confirmado | `RequestBypassToServer.java` y `L2TeleporterInstance.java`: pools cacheados con tareas que duermen 450 ms. No se encontraron llamadas a sus métodos de apagado. | Usar planificación compartida y revisar condiciones al ejecutar el teleport. |
| Confirmado | `build.xml`: targets individuales sin inicialización suficiente; el target predeterminado borra el JAR antes de compilar. | Hacer reproducible la compilación y preservar el artefacto previo ante fallos. |
| Confirmado | `Mount.xml`: copias amplias sin exclusiones específicas de datos privados y ZIP con actualización. | Revisar contenido de distribución y evitar entradas obsoletas. |
| No encontrado | Suite de pruebas automatizadas y GitHub Actions. Los nombres Test en quests no son pruebas automatizadas. | Incorporar validaciones de los flujos críticos. |

### Precisiones respecto de la auditoría anterior

- En esta máquina sí están disponibles Java y Ant; el código fuente compiló sin depender del JAR anterior. No se ejecutó el empaquetado de Ant.
- La ausencia de `Main-Class` no impide los arranques actuales: los scripts especifican la clase con `-cp`.
- `Etc/GMT+3` corresponde a UTC-3; no demuestra una hora incorrecta para Argentina.
- Creación automática de cuentas y escucha en todas las interfaces siguen configuradas. Su adecuación depende del despliegue.
- El repositorio tenía cambios locales; no estaba limpio como en la auditoría de la notebook.

### Próximo trabajo

1. Corregir el SQL de penalizaciones de clan.
2. Resolver la incompatibilidad del cambio de contraseña con el login.
3. Restringir las rutas HTML de dungeon y merchant.
4. Corregir la concurrencia de la protección contra flood.
5. Continuar con teletransportes, compilación, distribución y pruebas.

### Continuidad entre equipos

Los commits deben estar subidos al remoto para recuperarlos desde otra máquina; el usuario realiza los pushes. Trabajar y commitear en `develop`, conservando `main` como rama principal. Antes de continuar, actualizar `develop` con `git pull --ff-only` cuando tenga un remoto configurado y leer este archivo. Si hay cambios locales o divergencias, revisarlos antes de integrar. Las configuraciones e identificadores ignorados son propios de cada instalación y no viajan por Git.
