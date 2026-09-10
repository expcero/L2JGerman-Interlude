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

## Hallazgos iniciales

### Herramienta adicional: L2crypt (revisión del 2026-09-09)

- Procedencia del proyecto: nuestro repositorio
  [expcero/L2crypt](https://github.com/expcero/L2crypt) se creó como fork del
  original [acmi/L2crypt](https://github.com/acmi/L2crypt). Conservar esta
  atribución aunque GitHub deje de mostrar la relación de fork.
- El 2026-09-09 el usuario informó que cree haber completado la desvinculación
  de la red de forks en GitHub; ese estado remoto queda pendiente de verificar.
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
