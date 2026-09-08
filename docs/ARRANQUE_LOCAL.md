# Arranque local del servidor

Esta guía describe cómo iniciar localmente el Login Server y el Game Server desde Windows para revisar sus logs y conectarse con un cliente.

Los ejemplos usan `C:\proyect\L2JGerman-Interlude`. Reemplazá esa ruta si el repositorio está en otra carpeta. Completá la preparación antes de abrir las dos terminales de los servidores.

## Seguí estos pasos en orden

No inicies el cliente ni los servidores hasta completar los pasos 1 a 4.

1. **Instalá Java 11 y MariaDB.** Confirmá que `JAVA_HOME` apunta al JDK 11 y que MariaDB está iniciado.
2. **Creá la base de datos.** Abrí `tools\DatabasePanel.vbs`, configurá la conexión y cargá el esquema SQL en MariaDB.
3. **Revisá la conexión.** Los tres archivos `tools\mariadb.xml`, `auth\config\main\loginserver.properties` y `game\config\main\server.properties` deben indicar la misma base, usuario, contraseña, host y puerto.
4. **Registrá el Game Server una sola vez.** Ejecutá `auth\startLoginRegister.vbs`, elegí un ID y confirmá que se creó `game\config\other\hexid.txt`.
5. **Actualizá el JAR.** Ejecutá el bloque de compilación de esta guía para que `libs\La2Interlude.jar` incluya el código actual.
6. **Iniciá Login Server.** Abrí una terminal y ejecutá el comando de Login Server. Dejala abierta.
7. **Iniciá Game Server.** Abrí otra terminal y ejecutá el comando de Game Server. Dejala abierta.
8. **Configurá el cliente y conectate.** En el `l2.ini` del cliente configurá `127.0.0.1` y puerto `2106`; después abrí el cliente Interlude.

Si un paso falla, no continúes con el siguiente: revisá la sección correspondiente más abajo.

## Requisitos

- Java JDK 11 instalado y disponible mediante `JAVA_HOME`.
- MariaDB iniciado en el puerto configurado.
- Base de datos importada y configurada para el proyecto.
- `libs/La2Interlude.jar` actualizado a partir del código que querés probar y el conector MariaDB en `libs/`.

Comprobá Java desde una terminal:

```cmd
java -version
echo %JAVA_HOME%
```

En PowerShell, comprobá el JDK que usarán los comandos:

```powershell
& "$env:JAVA_HOME\bin\java.exe" -version
& "$env:JAVA_HOME\bin\javac.exe" -version
```

`JAVA_HOME` debe apuntar a la carpeta del JDK, sin agregar `\bin`.

## Configuración previa

Antes del arranque, revisá que la conexión a la base local sea consistente en estos archivos:

- `tools\mariadb.xml`
- `auth\config\main\loginserver.properties`
- `game\config\main\server.properties`

No inicies los servidores si MariaDB no está activo. La guía de instalación y creación de la base se encuentra en el [README](../README.md#3-configurar-y-crear-la-base-de-datos).

## Compilar el código que vas a probar

Los comandos de arranque ejecutan el JAR de `libs/`; editar Java o compilar en Eclipse no actualiza necesariamente ese archivo. En particular, el JAR revisado el 2026-09-06 todavía contenía el SQL anterior de penalizaciones de clan.

Mientras se corrige el build de Ant (paso 7 del plan), ejecutá este bloque completo en **PowerShell**, con los servidores y las herramientas Java del proyecto detenidos. Compila en una carpeta temporal nueva, sin usar el JAR anterior como dependencia. Solo reemplaza el JAR de `libs/` si la compilación y el empaquetado terminan correctamente; guarda una copia previa en esa carpeta temporal.

```powershell
Set-Location C:\proyect\L2JGerman-Interlude
$buildLocal = Join-Path $env:TEMP ('l2j-build-' + [guid]::NewGuid().ToString('N'))
$classesLocal = Join-Path $buildLocal 'classes'
New-Item -ItemType Directory -Path $classesLocal -ErrorAction Stop | Out-Null
$sourcesLocal = Join-Path $buildLocal 'sources.txt'
$sourceLines = Get-ChildItem -LiteralPath java -Recurse -Filter *.java |
    ForEach-Object { '"' + $_.FullName.Replace('\', '/') + '"' }
[System.IO.File]::WriteAllLines($sourcesLocal, [string[]]$sourceLines, [System.Text.UTF8Encoding]::new($false))
& "$env:JAVA_HOME\bin\javac.exe" --release 11 -g -encoding UTF-8 -cp 'libs/mariadb-java-client-3.1.4.jar' -d $classesLocal "@$sourcesLocal"
if ($LASTEXITCODE -ne 0) { throw 'Falló la compilación. No continuar con el arranque.' }
$jarLocal = Join-Path $buildLocal 'La2Interlude.jar'
& "$env:JAVA_HOME\bin\jar.exe" --create --file $jarLocal -C $classesLocal .
if ($LASTEXITCODE -ne 0) { throw 'Falló el empaquetado. No continuar con el arranque.' }
if (Test-Path -LiteralPath 'libs/La2Interlude.jar') {
    Copy-Item -LiteralPath 'libs/La2Interlude.jar' -Destination (Join-Path $buildLocal 'La2Interlude.previous.jar') -ErrorAction Stop
}
Copy-Item -LiteralPath $jarLocal -Destination 'libs/La2Interlude.jar' -Force -ErrorAction Stop
Write-Host "JAR actualizado. Compilación y respaldo en: $buildLocal"
```

Si hay un error, detenete antes de iniciar los servicios. No uses `ant compile` sin preparación: actualmente falla por propiedades/directorios sin inicializar. El target predeterminado de Ant también elimina el JAR previo antes de compilar.

## Registrar el Game Server en una instalación nueva

El archivo `game\config\other\hexid.txt` es local y no se descarga por Git. Debe contener un identificador que corresponda al registro de la base local. La configuración actual tiene `AcceptNewGameServer = False`, por lo que arrancar los procesos no sustituye este registro.

Si el servidor ya está registrado y su `hexid.txt` corresponde a esa base, conservá ambos. Para preparar una instalación nueva, con MariaDB activo y Login Server y Game Server detenidos:

1. Abrí el panel desde PowerShell:

   ```powershell
   Set-Location C:\proyect\L2JGerman-Interlude\auth
   & "$env:JAVA_HOME\bin\java.exe" '-Duser.timezone=Etc/GMT+3' -Xmx512m -cp '..\libs\*' net.sf.l2j.gsregistering.ui.GameServerRegisterUI
   ```

   También podés ejecutar `startLoginRegister.vbs` desde la carpeta `auth`.

2. Seleccioná un ID disponible y pulsá **Registrar ID**. El panel registra el ID en MariaDB y genera `game\config\other\hexid.txt`.
3. Confirmá el mensaje de éxito y la existencia del archivo. No uses las opciones de limpieza para preparar un servidor que ya tiene registro.
4. Cerrá el panel y comprobá que su proceso terminó antes de iniciar los servicios.

No publiques ni copies el identificador al changelog; pertenece a cada instalación.

## Iniciar el Login Server

Abrí una terminal independiente y ejecutá lo siguiente desde **CMD**:

```cmd
cd /d C:\proyect\L2JGerman-Interlude\auth
"%JAVA_HOME%\bin\java.exe" -Duser.timezone=Etc/GMT+3 -Xmx128m -cp "..\libs\*" net.sf.l2j.loginserver.L2LoginServer
```

En **PowerShell**:

```powershell
Set-Location C:\proyect\L2JGerman-Interlude\auth
& "$env:JAVA_HOME\bin\java.exe" '-Duser.timezone=Etc/GMT+3' -Xmx128m -cp '..\libs\*' net.sf.l2j.loginserver.L2LoginServer
```

Esperá a que el proceso se mantenga en ejecución sin errores antes de continuar. Conservá esta ventana abierta para revisar el log del Login Server.

## Iniciar el Game Server

Abrí una segunda terminal y ejecutá lo siguiente desde **CMD**:

```cmd
cd /d C:\proyect\L2JGerman-Interlude\game
"%JAVA_HOME%\bin\java.exe" -Duser.timezone=Etc/GMT+3 -Xmx2000m -cp "..\libs\*" net.sf.l2j.gameserver.GameServer
```

En **PowerShell**:

```powershell
Set-Location C:\proyect\L2JGerman-Interlude\game
& "$env:JAVA_HOME\bin\java.exe" '-Duser.timezone=Etc/GMT+3' -Xmx2000m -cp '..\libs\*' net.sf.l2j.gameserver.GameServer
```

Conservá también esta ventana abierta. El Game Server debe completar la conexión con el Login Server y la base de datos sin excepciones.

## Comprobar conexión local

La configuración local actual utiliza estos destinos:

| Servicio | Dirección |
| --- | --- |
| Login Server | `127.0.0.1:2106` |
| Game Server | `127.0.0.1:7777` |
| MariaDB | `127.0.0.1:3306` |

Configurá el cliente Interlude para conectarse a `127.0.0.1`. Iniciá sesión y seleccioná el servidor solo después de que los dos procesos estén activos.

## Detener los servicios

En cada terminal, presioná `Ctrl+C` y esperá que el proceso termine. Detené primero el Game Server y luego el Login Server.

## Problemas comunes

- Error de conexión a MariaDB: comprobá que el servicio esté activo y que las tres configuraciones previas usen la misma base, host y puerto.
- `JAVA_HOME` vacío o Java no encontrado: instalá JDK 11 y configurá `JAVA_HOME` antes de abrir una nueva terminal.
- Error de clase principal `.timezone=...` en PowerShell: conservá las comillas del argumento `'-Duser.timezone=Etc/GMT+3'`.
- Puerto ocupado: verificá que ningún proceso anterior esté usando `2106` o `7777` antes de arrancar el servicio correspondiente.
- El Game Server no se registra: revisá que el Login Server permanezca activo, que `LoginHost` apunte a `127.0.0.1`, que el puerto interno `9014` esté disponible para esa conexión y que `hexid.txt` corresponda al registro de la base local.
