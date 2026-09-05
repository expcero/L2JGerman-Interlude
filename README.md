# ⚔️ L2J Interlude

### Proyecto de desarrollo para Lineage II Interlude

Servidor desarrollado con Java 11 y MariaDB, con herramientas auxiliares para el cliente, launcher, actualizador y panel web.

El [registro de análisis y cambios](CHANGELOG.md) documenta las correcciones, verificaciones y pendientes para continuar el trabajo entre equipos.

El [plan de mejoras](PLAN_DE_MEJORAS.md) conserva el paso a paso de la auditoría y permite marcar las tareas completadas.

---

## 📖 Acerca del proyecto

**L2J Interlude** es una base de desarrollo para servidores de **Lineage II Interlude**, creada con foco en:

- 🚀 Rendimiento
- 🔒 Seguridad
- 🛠️ Facilidad de mantenimiento
- 📦 Distribución simplificada
- 🌐 Integración con sitio web y launcher
- 🔄 Sistema de actualización automática

---

## ✨ Características

| Característica | Estado |
| --- | --- |
| Login Server | ✅ |
| Game Server | ✅ |
| Sistema de actualización | ✅ |
| Launcher personalizado | ✅ |
| Protección del cliente | ✅ |
| InterfaceBlock | ✅ |
| Panel web | ✅ |
| Base de datos MariaDB | ✅ |
| Java 11 | ✅ |

---

## 📥 Descargas

| Componente | Enlace |
| --- | --- |
| 🛡️ DStupe.dll (protección del cliente) | [L2Protection](https://github.com/JulioPradoL2j/L2Protection) |
| 🎮 Cliente Interlude + Patch | [Descargar desde MediaFire](https://www.mediafire.com/file/8bwfzaco9k7jqv4/Lineage_II_-_Chronicle_Interlude.zip/file) |
| 🖥️ InterfaceBlock | [InterfaceBlock](https://github.com/JulioPradoL2j/InterfaceBlock) |
| 🚀 Launcher | [L2Updater](https://github.com/JulioPradoL2j/L2Updater) |
| 🌐 Sitio web / Panel web | [L2UpdaterWeb](https://github.com/JulioPradoL2j/L2UpdaterWeb) |
| 🔄 Actualizador compilado | [CompiledFiles](https://github.com/JulioPradoL2j/CompiledFiles) |

---

## 🧩 Dependencias

| Software | Versión |
| --- | --- |
| ☕ Java JDK | 11 |
| 🛢️ MariaDB | 10.4 o superior |
| 🖥️ Eclipse IDE | Versión reciente |
| 🔨 Apache Ant | Compatible con Java 11 |

### Enlaces de descarga

- [Java 11](https://mega.nz/file/V7tj1arS#OKWaTzaCqYK0m3iMmR0kW3TddfAJoiu8a20kOFEKShk)
- [MariaDB 10.4](https://mega.nz/file/1jEykRgL#DDuIGktiFbmE-M0jMzhUvYVckw1U0ov-OnZEYS5vopU)

---

## 🖥️ Requisitos del sistema

### Entorno de desarrollo

| Componente | Recomendado |
| --- | --- |
| Sistema operativo | Windows 10 u 11 |
| Procesador | Intel i5 / Ryzen 5 |
| Memoria RAM | 8 GB |
| Almacenamiento | SSD con 20 GB libres |
| Java | JDK 11 |
| Base de datos | MariaDB 10.4 o superior |

### Entorno de producción

| Componente | Recomendado |
| --- | --- |
| Sistema operativo | Windows Server 2019 o superior |
| Procesador | Xeon / Ryzen |
| Memoria RAM | 16 GB |
| Almacenamiento | SSD |
| Red | Conexión dedicada |

---

## ⚙️ Instalación rápida

### 1. Instalar Java 11

Después de instalar Java, configurá la variable de entorno `JAVA_HOME`:

```text
C:\Program Files\Java\jdk-11
```

Agregá al `PATH`:

```text
%JAVA_HOME%\bin
```

Verificá la instalación:

```cmd
java -version
```

El resultado debe indicar una versión de Java 11.

### 2. Instalar MariaDB

Instalá MariaDB 10.4 o superior y comprobá que el servicio esté iniciado.

### 3. Configurar y crear la base de datos

Configurá la conexión en:

```text
tools\mariadb.xml
```

De forma predeterminada, el proyecto utiliza:

```text
Base de datos: l2jdb
Usuario: root
Contraseña: root
Host: 127.0.0.1
Puerto: 3306
```

> ⚠️ En producción, cambiá la contraseña predeterminada y restringí el acceso a la base de datos.

Para abrir el panel de administración de la base de datos, ejecutá:

```text
tools\DatabasePanel.vbs
```

También podés iniciarlo desde Eclipse utilizando:

```text
laucher\Database.launch
```

### 4. Importar el proyecto en Eclipse

En Eclipse, seleccioná:

```text
File → Import → Existing Projects into Workspace
```

Elegí la carpeta raíz del proyecto y completá la importación.

### 5. Iniciar el servidor

Dentro de la carpeta `laucher\`, ejecutá primero:

```text
Loginserver.launch
```

Después, ejecutá:

```text
Gameserver.launch
```

Los archivos `.launch` también se pueden ejecutar desde Eclipse mediante `Run As → Java Application`.

---

## 🔨 Compilación

Para recompilar la biblioteca principal:

```cmd
ant -f build.xml
```

Archivo generado:

```text
libs\La2Interlude.jar
```

Para crear el paquete completo de distribución:

```cmd
ant -f Mount.xml
```

Archivo generado:

```text
Zip\trunk.zip
```

---

## 🌐 Puertos utilizados

| Servicio | Puerto |
| --- | ---: |
| Login Server | 2106 |
| Game Server | 7777 |
| MariaDB | 3306 |

Si utilizás una VPS o un servidor dedicado, habilitá únicamente los puertos necesarios en el firewall. No expongas públicamente el puerto de MariaDB sin una configuración de seguridad adecuada.

---

## ❗ Problemas frecuentes

### Java no encontrado

Ejecutá:

```cmd
java -version
```

Comprobá también que `JAVA_HOME` y `PATH` estén configurados correctamente.

### `Access denied for user 'root'`

Revisá el usuario y la contraseña definidos en:

```text
tools\mariadb.xml
```

### No se crearon las tablas

Confirmá que existan los archivos SQL en:

```text
tools\sql\
```

Luego, volvé a abrir el panel mediante `tools\DatabasePanel.vbs` o `laucher\Database.launch`.

### El servidor no se conecta a la base de datos

Verificá:

- Que el servicio de MariaDB esté iniciado.
- Que el puerto `3306` sea correcto.
- Que exista la base de datos `l2jdb`.
- Que el usuario y la contraseña sean correctos.
- Que `tools\mariadb.xml` esté configurado correctamente.

---

## 📚 Repositorios relacionados

| Proyecto | Enlace |
| --- | --- |
| L2Protection | [GitHub](https://github.com/JulioPradoL2j/L2Protection) |
| InterfaceBlock | [GitHub](https://github.com/JulioPradoL2j/InterfaceBlock) |
| L2Updater | [GitHub](https://github.com/JulioPradoL2j/L2Updater) |
| L2UpdaterWeb | [GitHub](https://github.com/JulioPradoL2j/L2UpdaterWeb) |
| CompiledFiles | [GitHub](https://github.com/JulioPradoL2j/CompiledFiles) |

---

## 📞 Soporte

Al reportar un problema, incluí:

- Sistema operativo.
- Versión de Java.
- Mensaje de error completo.
- Log del Login Server.
- Log del Game Server.

Contacto: [juliopradol2j@gmail.com](mailto:juliopradol2j@gmail.com)

---

## 📜 Licencia

Este proyecto se distribuye bajo los términos de la licencia **GNU General Public License v3.0**. Consultá el archivo [LICENSE](LICENSE) para conocer los términos completos.

Los recursos relacionados con Lineage II pertenecen a sus respectivos propietarios. Asegurate de contar con autorización para utilizar y distribuir los archivos asociados al juego.

---

### © L2J Interlude

Desarrollado para la comunidad de Lineage II.
