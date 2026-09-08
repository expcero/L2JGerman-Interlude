# Patch de cliente requerido para pruebas locales

El servidor local inicia correctamente, pero rechaza el cliente actual con este motivo:

```text
Client [IP: 127.0.0.1] rejected during ProtocolVersion: missing HWID payload.
```

Esto significa que el cliente Interlude debe enviar el bloque de identificacion HWID llamado `BHWD` durante el paquete `ProtocolVersion`.

## Que buscar

Busca un patch o `system` de cliente compatible con estos requisitos:

1. Cliente **Lineage II Interlude**.
2. Protocolo compatible con una de estas revisiones: **737**, **740**, **744** o **746**.
3. Proteccion de cliente que envie el payload **BHWD** al Game Server.
4. Patch que incluya o requiera **DStupe.dll**. En la documentacion original del proyecto aparece como `L2Protection`.
5. Ejecutable compatible con esa DLL, por ejemplo `l2.exe` o `L2.bin` modificado para cargarla.
6. Instrucciones de instalacion y todos los archivos requeridos por el patch, no solo una DLL aislada.

## Texto para buscar o pedir

Podes copiar este texto al buscar o consultar a quien te dio el proyecto:

```text
Necesito el system/patch de cliente Lineage II Interlude compatible con este servidor L2J.
El Game Server acepta protocolos 737, 740, 744 o 746 y exige que el cliente envie
el payload HWID BHWD durante ProtocolVersion. La proteccion mencionada por el proyecto
es DStupe.dll / L2Protection. Necesito el paquete completo: system, DLLs, ejecutable
compatible e instrucciones de instalacion.
```

## Archivos que deberia traer

El contenido exacto depende del proveedor, pero el paquete debe incluir al menos:

- `system\\DStupe.dll` o la DLL de proteccion equivalente.
- El ejecutable que carga la DLL: `l2.exe`, `L2.bin` o ambos.
- DLLs auxiliares requeridas por la proteccion.
- Los archivos del `system` que correspondan a la revision de protocolo.
- Un `l2.ini` compatible o instrucciones para configurarlo.

No alcanza con copiar `DStupe.dll` a un cliente Interlude normal: el ejecutable y los
archivos del system deben ser compatibles entre si.

## Como instalarlo para la prueba local

1. Hace una copia completa de la carpeta actual del cliente.
2. Aplica el patch completo sobre la carpeta del cliente, siguiendo las instrucciones de quien lo provea.
3. Configura el `l2.ini` del cliente con estos valores:

   ```ini
   ServerAddr=127.0.0.1
   ServerPort=2106
   ```

4. Deja Login Server y Game Server iniciados.
5. Abre el ejecutable indicado por el patch e intenta entrar a **Sieghardt**.

## Como saber si funciono

Con `PacketHandlerDebug = True` en `game/config/main/server.properties`:

- Si vuelve a aparecer `missing HWID payload`, el patch no esta instalado, no se esta cargando o no es compatible.
- Si ese mensaje desaparece y el personaje llega a la pantalla de seleccion o creacion, el cliente ya envio el payload requerido.
- Si aparece otro error, conserva las ultimas lineas de las consolas Auth y World para diagnosticarlo.

## Seguridad

Descarga archivos de cliente solo desde la fuente original del proyecto o un proveedor confiable. Un ejecutable o DLL de origen desconocido puede comprometer la PC. Analiza los archivos con tu antivirus antes de ejecutarlos.
