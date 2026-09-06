# Arranque local del servidor

Esta guía describe cómo iniciar localmente el Login Server y el Game Server desde Windows para revisar sus logs y conectarse con un cliente.

## Requisitos

- Java JDK 11 instalado y disponible mediante `JAVA_HOME`.
- MariaDB iniciado en el puerto configurado.
- Base de datos importada y configurada para el proyecto.
- `libs/La2Interlude.jar` y las dependencias en `libs/` disponibles.

Comprobá Java desde una terminal:

```cmd
java -version
echo %JAVA_HOME%
```

## Configuración previa

Antes del arranque, revisá que la conexión a la base local sea consistente en estos archivos:

- `tools\mariadb.xml`
- `auth\config\main\loginserver.properties`
- `game\config\main\server.properties`

No inicies los servidores si MariaDB no está activo. La guía de instalación y creación de la base se encuentra en el [README](../README.md#3-configurar-y-crear-la-base-de-datos).

## Iniciar el Login Server

Abrí una terminal independiente y ejecutá lo siguiente desde **CMD**:

```cmd
cd /d C:\proyect\L2JGerman-Interlude\auth
"%JAVA_HOME%\bin\java.exe" -Duser.timezone=Etc/GMT+3 -Xmx128m -cp "..\libs\*" net.sf.l2j.loginserver.L2LoginServer
```

En **PowerShell**:

```powershell
Set-Location C:\proyect\L2JGerman-Interlude\auth
& "$env:JAVA_HOME\bin\java.exe" -Duser.timezone=Etc/GMT+3 -Xmx128m -cp '..\libs\*' net.sf.l2j.loginserver.L2LoginServer
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
& "$env:JAVA_HOME\bin\java.exe" -Duser.timezone=Etc/GMT+3 -Xmx2000m -cp '..\libs\*' net.sf.l2j.gameserver.GameServer
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
- Puerto ocupado: verificá que ningún proceso anterior esté usando `2106` o `7777` antes de arrancar el servicio correspondiente.
- El Game Server no se registra: revisá primero que el Login Server permanezca activo y que `LoginHost` apunte a `127.0.0.1` para el entorno local.