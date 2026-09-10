# Evaluación de Mobius y herramientas relacionadas

Fecha: 2026-09-09.

## Alcance y fuentes

Se inventariaron los proyectos públicos de la cuenta
[MobiusDevelopment en GitLab](https://gitlab.com/MobiusDevelopment).
La API devolvió un proyecto: [L2J_Mobius](https://gitlab.com/MobiusDevelopment/L2J_Mobius).
La captura del usuario muestra subcarpetas de ese repositorio, no repositorios
independientes. El inventario contiene 37 variantes de servidor, Account_Manager
y el README general.

Se consultaron los README y build.xml de las 37 variantes, sin errores de lectura.
Todas las compilaciones consultadas declaran Java 25 como source/target.
Se profundizó en ProtocolVersion, Server.ini, accounts.sql y la estructura de
Interlude, además de la estructura y configuración del gestor de cuentas.
No se compiló ni ejecutó Mobius: esta es una evaluación de pertinencia y una
comparación parcial de código, no una auditoría completa de las 37 implementaciones.

HEAD observado: `43ac8878f582ea792874eb74df16e2b0b64990e6`, del 2026-08-29.
Las consultas se hicieron contra master; los resultados describen esa revisión
observada y pueden cambiar con nuevas publicaciones.

## Inventario completo y utilidad

Los nombres siguientes corresponden a carpetas bajo el prefijo `L2J_Mobius_`.

| Familia | Carpetas revisadas | Utilidad para nuestro Interlude |
| --- | --- | --- |
| Interlude | CT_0_Interlude | Prioridad alta como servidor de referencia y fuente de comparaciones concretas. |
| Crónicas anteriores | C1_HarbingersOfWar, C4_ScionsOfDestiny | Referencia histórica; mecánicas y protocolos de otras crónicas. |
| Crónicas posteriores | CT_2.4_Epilogue, CT_2.6_HighFive | Ideas de arquitectura o funciones que habría que adaptar; no reemplazos directos. |
| Línea moderna, parte 1 | 01.0_Ertheia, 02.5_Underground, 03.0_Helios, 04.0_GrandCrusade, 05.0_Salvation | Funciones y clientes distintos; baja prioridad para el bloqueo actual. |
| Línea moderna, parte 2 | 06.0_Fafurion, 07.0_PreludeOfWar, 08.2_Homunculus, 09.2_ReturnOfTheQueenAnt, 10.3_MasterClass | Referencia selectiva; no mezclar sus datos o paquetes con Interlude. |
| Línea moderna, parte 3 | 11.3_Shinemaker, 12.3_Superion, 13.1_OrcVillage, 13.2_WolfWaker, 14.1_SamuraiCrow | Mismo criterio: comparar una función específica solo cuando haga falta. |
| Classic | Classic_1.0, Classic_1.5_AgeOfSplendor, Classic_2.0_Saviors, Classic_2.5_Zaken, Classic_2.7_Antharas, Classic_2.9_SecretOfEmpire, Classic_2.9.5_Saviors, Classic_3.0_TheKamael, Classic_3.5_TalesUntold | Classic es otra línea de cliente y reglas; no equivale a C6 Interlude. |
| Essence | Essence_04.2_DwellingOfSpirits, Essence_05.2_FrostLord, Essence_06.3_Crusader, Essence_07.3_SevenSigns, Essence_08.3_Guardians, Essence_09.1_Warg, Essence_09.2_RoseVain, Essence_10.1_SamuraiCrow | Baja prioridad; sistemas específicos que requerirían adaptación importante. |
| Herramienta web, sin prefijo | Account_Manager | Posible gestor de cuentas posterior; revisar consultas SQL, autenticación y compatibilidad antes de usar. |

## Comparación concreta con nuestro servidor

| Aspecto | L2JGerman-Interlude | Mobius CT_0_Interlude revisado |
| --- | --- | --- |
| Java usado/requerido | Java 11 en nuestra guía y pruebas | build.xml declara source/target 25 |
| Paquetes Java | net.sf.l2j | org.l2jmobius |
| Protocolo | 737, 740, 744, 746, dentro de límites configurados | AllowedProtocolRevisions = 746 por defecto |
| Inicio de conexión | Exige BHWD, clave y sesión HWID | ProtocolVersion revisado valida revisión y envía KeyPacket; no implementa nuestra exigencia BHWD |
| Cuenta SQL | access_level, lastactive DECIMAL(20) | accessLevel, lastactive BIGINT y campos adicionales |
| Distribución | auth, game, libs y tools | dist/login, dist/game, dist/libs y dist/db_installer |

Fuentes técnicas:
[ProtocolVersion](https://gitlab.com/MobiusDevelopment/L2J_Mobius/-/blob/master/L2J_Mobius_CT_0_Interlude/java/org/l2jmobius/gameserver/network/clientpackets/ProtocolVersion.java),
[Server.ini](https://gitlab.com/MobiusDevelopment/L2J_Mobius/-/blob/master/L2J_Mobius_CT_0_Interlude/dist/game/config/Server.ini),
[build.xml](https://gitlab.com/MobiusDevelopment/L2J_Mobius/-/blob/master/L2J_Mobius_CT_0_Interlude/build.xml),
[accounts.sql](https://gitlab.com/MobiusDevelopment/L2J_Mobius/-/blob/master/L2J_Mobius_CT_0_Interlude/dist/db_installer/sql/login/accounts.sql).

La diferencia de protocolo permite plantear una prueba independiente con un
cliente de revisión 746 para aislar problemas de conexión. No demuestra que
nuestro cliente ya funcione en Mobius. Tampoco se debe copiar su ProtocolVersion
sobre el nuestro: AuthLogin y EnterWorld locales dependen de la sesión HWID.

Mobius puede servir para comparar paquetes, reglas, skills, NPCs, quests y
persistencia ante un problema concreto. No hay evidencia en esta revisión de que
solucione un error específico de nuestras mecánicas ni de que su datapack pueda
copiarse directamente. Las APIs, formatos y esquemas requieren adaptación.

La inspección adicional de `Account_Manager/index.php` encontró dos problemas
concretos para usarlo tal cual: el registro inserta columnas `email` y `lastIP`
que no existen en nuestro accounts.sql, y concatena el email recibido por POST
directamente en SQL. También imprime cuenta, hash de contraseña y email en la
respuesta del registro. Por estos hallazgos, no se recomienda desplegar ese panel
sin corregir las consultas, eliminar esa salida y revisar el resto de los flujos.
El hash SHA-1 codificado en Base64 coincide conceptualmente con nuestro login,
pero esa coincidencia no resuelve los problemas de esquema ni del panel.

Fuente: [Account_Manager/index.php](https://gitlab.com/MobiusDevelopment/L2J_Mobius/-/blob/master/Account_Manager/index.php).

El README general indica que el proyecto no suministra software ni recursos del
cliente. Este repositorio no es el patch dsetup.dll que nos falta integrar.
El mismo README describe una transición de GPLv3 a MIT: al reutilizar código,
registrar la cabecera de licencia del archivo concreto y conservar atribuciones.

## Las tres nuevas copias en GitHub

Verificadas mediante la API de GitHub el 2026-09-09; todas tienen master como
rama principal. No se encontraron carpetas locales con esos nombres en C:\proyect
durante la inspección.

| Copia del usuario | Origen confirmado | Evaluación |
| --- | --- | --- |
| [expcero/Dependencies](https://github.com/expcero/Dependencies) | lucasg/Dependencies | Prioridad inmediata para inspeccionar dependencias e importaciones de DLL. No detecta cargas dinámicas por LoadLibrary ni valida BHWD. |
| [expcero/L2ClientDat](https://github.com/expcero/L2ClientDat) | MobiusDevelopment/L2ClientDat | Contiene dist/data/structure/06_interlude.xml: hay una definición específica de Interlude. Falta probar nuestros archivos, en particular si están modificados. |
| [expcero/L2unreal](https://github.com/expcero/L2unreal) | acmi/L2unreal | Biblioteca para leer y modificar objetos UnrealScript del cliente; útil para una necesidad posterior de paquetes. |

## Decisión de trabajo

Conservar nuestro servidor como proyecto principal. Incorporar Mobius como copia
de referencia separada, priorizando CT_0_Interlude. Una eventual ejecución debe
usar su propia base y puertos disponibles. Mantener el trabajo inmediato centrado
en la carga de L2Protection y el acceso al mundo.

Para traer todos los archivos se puede clonar el repositorio completo con Git.
Eso conserva el historial y el árbol de master; no implica importar todas las
crónicas dentro del proyecto L2JGerman-Interlude. Una futura copia en GitHub puede
usar origin para expcero y upstream para el repositorio original de GitLab.

## Copia local solicitada

El 2026-09-09 se inició un clon completo, sin limitar profundidad, en
`C:\proyect\L2J_Mobius`. El inventario Git de HEAD contiene 574.504 archivos,
4,71 GiB de contenido y 11.901 commits alcanzables desde HEAD. Ningún archivo de
ese árbol alcanza 100 MiB; no se hizo el mismo examen sobre cada blob histórico.
El HEAD descargado coincide con la revisión indicada arriba. El despliegue del
árbol de trabajo sigue en curso al registrar esta entrada; la verificación final
y la publicación en un repositorio del usuario están pendientes.
