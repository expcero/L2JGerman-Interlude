# Registro temporal de repositorios relacionados

Este documento vincula los repositorios que estamos evaluando para trabajar con
`L2JGerman-Interlude`. Es un registro documental: no agrega submódulos Git ni
convierte estos proyectos en dependencias del servidor.

Los candidatos pueden descartarse si no sirven. Antes de borrar una copia local,
registrar aquí el motivo y comprobar si contiene cambios que deban conservarse.

## Proyecto y cliente de referencia

- Servidor: `C:\proyect\L2JGerman-Interlude`.
- Cliente indicado por el usuario: `C:\game\l2`; carpeta de sistema: `C:\game\l2\system`.
- Otra copia de system encontrada: `C:\proyect\System\System`.
- Guías existentes: [arranque local](ARRANQUE_LOCAL.md) y
  [patch de cliente requerido](PATCH_CLIENTE_REQUERIDO.md).

## Repositorios en evaluación

| Repositorio | Ruta local | Propósito para este proyecto | Estado |
| --- | --- | --- | --- |
| [L2Protection](https://github.com/expcero/L2Protection) | `C:\proyect\L2Protection` | Protección del cliente que envía el bloque HWID requerido por el servidor. | Candidato prioritario; código revisado, integración pendiente. |
| [L2AntiCheat](https://github.com/expcero/L2AntiCheat) | `C:\proyect\L2AntiCheat` | Alternativa de protección con componentes adicionales, incluidos voz y manejo de archivos del cliente. | Pendiente de evaluar compatibilidad completa. |
| [L2FileEdit](https://github.com/expcero/L2FileEdit) | `C:\proyect\L2FileEdit` | Herramientas para editar archivos `dat`, `ini` e `int` del cliente. | Disponible; compatibilidad con nuestros archivos pendiente de probar. |
| [compileFilles](https://github.com/expcero/compileFilles) | `C:\proyect\compileFilles` | Scripts para preparar archivos de distribución y actualizaciones. | Utilidad potencial posterior; no contiene un cliente completo en las carpetas revisadas. |

## Titularidad y origen verificados (2026-09-09)

Los seis repositorios locales revisados tienen `origin` bajo la cuenta GitHub
`expcero`; la API pública confirma ese propietario. No se verificó el correo
privado asociado a la cuenta. La identidad usada por Git para autenticarse es
independiente del propietario del repositorio.

| Repositorio en expcero | Relación de fork actual en GitHub | Rama principal remota |
| --- | --- | --- |
| L2JGerman-Interlude | Independiente; esto no establece la autoría histórica del código. | develop |
| L2Protection | Fork de JulioPradoL2j/L2Protection | main |
| L2AntiCheat | Fork de L2JPremium-stack/L2AntiCheat | main |
| L2FileEdit | Fork de L2jBrasil/L2FileEdit | master |
| compileFilles | Fork de L2JPremium-stack/compileFilles | main |
| L2crypt | Independiente; origen histórico acmi/L2crypt. | main |
| [L2J_Mobius](https://github.com/expcero/L2J_Mobius) | Copia independiente del repositorio Mobius de GitLab; conserva el historial descargado. | master |
| [l2jvarios](https://github.com/expcero/l2jvarios) | Copia independiente que contiene L2JLisvus Chronicle 4: Scions of Destiny. | main |

Otros repositorios de `expcero` revisados el 2026-09-10:

| Repositorio | Utilidad posible | Estado |
| --- | --- | --- |
| [L2Interlude](https://github.com/expcero/L2Interlude) | Base Interlude Java 11/MariaDB con launcher, actualizador y panel web. | Referencia útil para comparar arquitectura; no mezclar código sin revisar licencias y diferencias. |
| [dev_master](https://github.com/expcero/dev_master) | Otra base NEXORA Interlude Java 11/MariaDB. | Referencia secundaria; repositorio muy grande, no candidato para reemplazar el servidor actual. |
| [L2JServer_C6_Interlude](https://github.com/expcero/L2JServer_C6_Interlude) | Servidor Interlude basado en archivos L2J Mobius, con eventos y mods. | Referencia de features; no es compatible automáticamente con `net.sf.l2j` ni con nuestro HWID. |
| [L2HwidWeb](https://github.com/expcero/L2HwidWeb) | Panel web de administración y funciones relacionadas con servidor/HWID. | Útil para una etapa web posterior; no envía el payload `BHWD` del cliente. |
| [L2Launcher](https://github.com/expcero/L2Launcher) | Launcher Windows, manifest, reparación y descarga de parches; inicia `system/l2.exe`. | Útil después de resolver el cliente; no sustituye `dsetup.dll`. |
| [xdat_editor](https://github.com/expcero/xdat_editor) | Edición de `interface.xdat`. | Herramienta de cliente posterior; no afecta el protocolo ni HWID. |
| [l2ce](https://github.com/expcero/l2ce), [dateditor](https://github.com/expcero/dateditor) | Editores de archivos del cliente. | Evaluar solo si `L2FileEdit` o `L2ClientDat` no cubren el archivo concreto. |
| [Datapack](https://github.com/expcero/Datapack) | Datos de servidor de otra base L2J. | Referencia selectiva; riesgo alto de incompatibilidad con nuestro core y esquema. |

Los demás repositorios del inventario son de Aion, High Five, Essence, bots,
votación, donaciones, sitios generales o bases de otras crónicas. No aportan una
solución directa al bloqueo actual y quedan fuera del grupo prioritario.

## Nuevas copias en GitHub (2026-09-09)

Verificados los forks [expcero/Dependencies](https://github.com/expcero/Dependencies),
[expcero/L2ClientDat](https://github.com/expcero/L2ClientDat) y
[expcero/L2unreal](https://github.com/expcero/L2unreal). Sus originales son,
respectivamente, lucasg/Dependencies, MobiusDevelopment/L2ClientDat y acmi/L2unreal.
Los tres tienen master como rama principal. No se encontraron aún sus clones
en C:\proyect. L2ClientDat incluye una definición específica para Interlude.

La evaluación de Mobius en GitLab, su inventario de 37 crónicas y la comparación
con nuestro servidor están en [ANALISIS_MOBIUS.md](ANALISIS_MOBIUS.md).

Referencias externas registradas:
[proyectos de MobiusDevelopment en GitLab](https://gitlab.com/users/MobiusDevelopment/projects),
[L2J_Mobius](https://gitlab.com/MobiusDevelopment/L2J_Mobius) y
[tutorial de instalación](https://l2jmobius.org/forum/index.php?topic=3231.0).

## Herramientas adicionales (copias remotas verificadas; integración pendiente)

| Repositorio original | Utilidad posible | Prioridad y límites |
| --- | --- | --- |
| [lucasg/Dependencies](https://github.com/lucasg/Dependencies) | Inspeccionar importaciones y dependencias de ejecutables y DLL de Windows. | Primera opción para investigar dependencias de dsetup.dll; no detecta cargas dinámicas mediante LoadLibrary ni demuestra por sí sola que el HWID funcione. |
| [MobiusDevelopment/L2ClientDat](https://github.com/MobiusDevelopment/L2ClientDat) | Abrir y guardar archivos .dat del cliente. | Evaluar cuando necesitemos editar datos; confirmar soporte para nuestros archivos Interlude. |
| [acmi/L2unreal](https://github.com/acmi/L2unreal) | Leer y modificar objetos UnrealScript de Lineage II. | Posterior, si necesitamos trabajar con paquetes del cliente; no resuelve la conexión HWID. |

Se revisaron las descripciones de los proyectos originales y los forks del
usuario. No se clonaron ni probaron estas tres herramientas durante esta revisión.
No son dependencias nuevas del servidor.

## Hallazgos iniciales

### Herramienta adicional: L2crypt (revisión del 2026-09-09)

- Procedencia del proyecto: nuestro repositorio
  [expcero/L2crypt](https://github.com/expcero/L2crypt) se creó como fork del
  original [acmi/L2crypt](https://github.com/acmi/L2crypt). Conservar esta
  atribución aunque GitHub deje de mostrar la relación de fork.
- El 2026-09-09 se confirmó mediante la API de GitHub la desvinculación:
  `fork=false`, sin repositorio padre y con `main` como rama principal.
  La separación no cambia la procedencia del código ni su licencia MIT.
- Repositorio consultado: [JekaKlever/L2crypt](https://github.com/JekaKlever/L2crypt),
  fork de [acmi/L2crypt](https://github.com/acmi/L2crypt). Se revisó como referencia;
  no fue el origen elegido para nuestro fork.
- Copia ya existente: `C:\proyect\L2crypt`. El 2026-09-09 se cambió `origin` a
  [expcero/L2crypt](https://github.com/expcero/L2crypt) y se conservó
  `acmi/L2crypt` como `upstream`.
  Los 17 archivos Java y `build.gradle` comparados coinciden con el fork consultado.
- Biblioteca Java con licencia MIT para cifrar y descifrar archivos de Lineage II.
  Se incorpora al registro como herramienta auxiliar, no como protección HWID.
- Se compiló el código local con Java 11 y se probó la lectura del
  `C:\game\l2\system\l2.ini` usando salidas temporales. La lectura con la clave
  413 predeterminada falló con `block data size too large`; al seleccionar la
  variante L2ENCDEC mediante `set41xPrivateKey`, funcionó.
- La prueba de descifrar, recifrar y volver a descifrar conservó los 9.336 bytes
  del contenido. Se confirmó `ServerAddr=127.0.0.1`. No se reemplazó el archivo
  original ni se verificó en el juego la carga del archivo recifrado.
- Los auxiliares locales `DecryptL2.java` y `EncryptL2.java` no forman parte del
  árbol remoto revisado. El descifrador requiere seleccionar la variante adecuada
  para este cliente; no asumir que la cabecera 413 determina por sí sola la clave.

### Servidor y candidatos iniciales

- Los logs del 7 de septiembre de 2026 muestran que Login Server inició y que
  Game Server se registró como Sieghardt. Una conexión local fue rechazada durante
  `ProtocolVersion` con `missing HWID payload`.
- El servidor exige el marcador `BHWD`, una longitud de cuatro bytes en
  little-endian y el texto ASCII `cpu|hdd|mac|key`. Después valida la clave y el
  estado del dispositivo en la base. La entrada al mundo aún no está verificada.
- El código revisado de `L2Protection/dsetup/main.cpp` construye ese formato y
  utiliza una clave coincidente con el servidor. Esto no demuestra que el binario
  publicado corresponda exactamente al código ni que nuestro ejecutable lo cargue.
- `L2Protection/build/dsetup.dll` tiene 2.417.664 bytes;
  `L2AntiCheat/build/dsetup.dll`, 720.384 bytes. Son candidatos distintos y no se
  deben tratar como intercambiables sin revisar su integración.
- La DLL actual de `C:\game\l2\system` tiene 62.996 bytes. La comparación SHA-256
  confirmó que `L2.exe`, `dsetup.dll`, `engine.dll` y `l2.ini` son idénticos a los
  de `C:\proyect\System\System`.
- El cliente contiene mapas, texturas, animaciones y otros recursos. Se verificó
  su presencia, no la integridad completa de la instalación.
- `l2.ini` está cifrado y presenta la cabecera `Lineage2Ver413`.
- El nombre del binario encontrado es `dsetup.dll`. La guía anterior menciona
  `DStupe.dll`; esa referencia debe aclararse al documentar el patch definitivo.
- Hay un error independiente en el evento Last Man (`LMManager.startReg`),
  registrado en el log del servidor. No es el motivo del rechazo HWID observado.

## Próximos pasos

1. Revisar cómo el ejecutable actual carga `dsetup.dll` y comprobar los requisitos
   del candidato de `L2Protection`, incluida su correspondencia con el código.
2. Preparar una prueba con respaldo de los archivos que se vayan a reemplazar.
3. Verificar carga de la protección, conexión, selección de personaje y entrada
   al mundo. Registrar archivos usados, revisiones Git, resultados y logs.
4. Evaluar los otros repositorios cuando exista una necesidad concreta.
5. Actualizar este documento al aceptar, modificar o descartar un candidato.

## Historial

| Fecha | Acción | Resultado |
| --- | --- | --- |
| 2026-09-09 | Verificación de tres nuevos forks y evaluación de Mobius en GitLab. | Confirmados Dependencies, L2ClientDat y L2unreal bajo expcero. Inventariadas las 37 crónicas de Mobius; CT_0_Interlude priorizado como referencia. Informe en ANALISIS_MOBIUS.md. |
| 2026-09-10 | Registro de L2J_Mobius y l2jvarios bajo expcero. | L2J_Mobius: copia independiente de Mobius con master. l2jvarios: copia independiente de L2JLisvus Chronicle 4 con main; utilidad como referencia, no como reemplazo de Interlude. |
| 2026-09-09 | Auditoría de remotos y titularidad de los seis repositorios. | Todos bajo expcero. Confirmados L2crypt independiente y main predeterminada; esto no verifica la publicación del commit local f95d9ba. Registrados orígenes de los otros forks y tres herramientas candidatas adicionales. |
| 2026-09-09 | Registro de procedencia tras la desvinculación indicada por el usuario. | Origen de expcero/L2crypt: acmi/L2crypt. JekaKlever/L2crypt fue una referencia consultada. Desvinculación remota pendiente de verificar; atribución y licencia conservadas. |
| 2026-09-09 | Creación de la rama local main en L2crypt. | Commit `f95d9ba`: auxiliares DecryptL2.java y EncryptL2.java agregados, .class ignorados. Push rechazado (403): Git autenticado como germankay sin permiso de escritura en expcero/L2crypt. Pendientes publicar main y establecerla como rama predeterminada en GitHub; master conservada. |
| 2026-09-09 | Vinculación de L2crypt con el fork del usuario. | `origin`: expcero/L2crypt; `upstream`: acmi/L2crypt. Fetch verificado usando certificados de Windows (`http.sslBackend=schannel` solo para esa llamada); master sin diferencias con origin/master. Auxiliares locales sin seguimiento conservados; sin push. |
| 2026-09-09 | Revisión de JekaKlever/L2crypt y prueba de la copia local de acmi. | Código comparado coincidente; lectura y recifrado temporal del INI verificados con la variante L2ENCDEC. |
| 2026-09-07 | Revisión inicial de servidor, cliente y repositorios públicos. | Identificado el bloqueo HWID y localizado `L2Protection` como candidato. |
| 2026-09-07 | Inspección de los cuatro clones locales indicados por el usuario. | Encontrados bajo `C:\proyect`; sin cambios locales al inspeccionarlos. |
| 2026-09-07 | Creación de este registro temporal. | Vinculación documental; no se ejecutaron los binarios candidatos ni se modificaron archivos del juego durante esta revisión. |

Para cada intervención posterior, agregar una entrada con fecha, repositorio,
archivos o revisión implicados, resultado de la prueba y pendientes. No registrar
contraseñas, claves de protección ni identificadores HWID personales.
