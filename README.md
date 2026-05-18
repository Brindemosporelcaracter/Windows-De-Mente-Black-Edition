# Windows De Mente — Black Edition

<div align="center">

```
╔══════════════════════════════════════════════════════════════════╗
║         WINDOWS DE MENTE — BLACK EDITION                   ║
║   "El diagnóstico del Arquitecto, la cirugía del Ingeniero       ║
║    y el exorcismo del Lógico."                                   ║
╚══════════════════════════════════════════════════════════════════╝
```

**Un solo archivo `.ps1`. Sin instaladores. Sin dependencias. Sin sorpresas.**

<img width="1131" height="642" alt="18-5-2026 6 5 10 1" src="https://github.com/user-attachments/assets/44f6e730-6b4c-48a5-aab4-7682892ec377" />
<img width="1131" height="642" alt="18-5-2026 6 5 22 3" src="https://github.com/user-attachments/assets/6e560d4e-20c6-4e1a-9da0-e9dab5475573" />
<img width="1131" height="642" alt="18-5-2026 6 5 16 2" src="https://github.com/user-attachments/assets/16ebcfa7-e280-4eb0-94bd-ef97dd322115" />
<img width="1131" height="642" alt="18-5-2026 6 5 32 5" src="https://github.com/user-attachments/assets/11122382-18c2-4dd4-8291-0c9f442fbf1a" />
<img width="1131" height="642" alt="18-5-2026 6 5 28 4" src="https://github.com/user-attachments/assets/eece84af-9190-4f21-9e7d-74285b9d45f5" />


---

## ¿Qué es esto?

**Windows De Mente Black Edition** es una herramienta de optimización, limpieza y privacidad para Windows escrita en PowerShell puro. Nació de la fusión de tres scripts desarrollados y probados durante meses — **Lazarus V2**,script que analiza optimizaciones a nivel kernel , **Lazarus-Nitidus** y **Windows De Mente v2** — tomando lo mejor de cada uno y construyendo una interfaz gráfica completa encima.

**84 ítems** distribuidos en 5 sectores: diagnóstico, kernel/rendimiento, limpieza profunda, privacidad/forense y reparación del sistema.

La diferencia con otras herramientas es una sola: **esta te explica exactamente qué hace antes de hacerlo.** Hacés clic en cualquier ítem y el panel derecho te dice qué es, por qué existe, y qué pasa si lo tocás. No es una caja negra.

---

## Por qué PowerShell y no una app instalable

Porque un script de PowerShell es código que podés leer línea por línea. No hay un ejecutable opaco que haga quién sabe qué en segundo plano. No hay actualizaciones automáticas que cambien el comportamiento sin avisarte. No hay dependencias. No hay registro de Windows que ensuciar. Un archivo — lo abrís con Notepad si querés verificar qué hace, y lo borrás cuando no lo necesitás más.

Las herramientas comerciales de optimización hacen muchas de estas mismas cosas, pero cobran por hacerlas, no te dicen cuáles exactamente, y algunas agregan su propia telemetría en el proceso. Esto no hace ninguna de esas tres cosas.

---

## Requisitos

| Requisito | Detalle |
|---|---|
| Sistema operativo | Windows 10 (build 19041+) o Windows 11 |
| PowerShell | 5.1 (incluido en Windows) **o** 7+ |
| Permisos | Administrador — el script se eleva solo |
| Internet | Solo para DISM /RestoreHealth y Winapp2 |

---

## Cómo usar

No hay instalación. Descargá el archivo y ejecutalo.

### Doble clic (más fácil)

```
Clic derecho sobre WindowsDeMente_BlackEdition.ps1
→ Ejecutar con PowerShell
→ Aceptar la elevación a Administrador
```

### Desde PowerShell

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\WindowsDeMente_BlackEdition.ps1
```

### Modo consola sin GUI

```powershell
# Solo diagnóstico — no toca nada
.\WindowsDeMente_BlackEdition.ps1 -NoGUI -Mode Diagnose

# Optimización completa sin interacción
.\WindowsDeMente_BlackEdition.ps1 -NoGUI -Mode FullOptimize

# Simular todo sin aplicar nada
.\WindowsDeMente_BlackEdition.ps1 -WhatIf

# Restaurar el último backup si algo salió mal
.\WindowsDeMente_BlackEdition.ps1 -Undo
```

---

## Parámetros disponibles

| Parámetro | Efecto |
|---|---|
| `-WhatIf` | Simula todo sin hacer ningún cambio real |
| `-Undo` | Restaura desde el último backup del registro |
| `-NoGUI` | Ejecuta en consola pura, sin ventana |
| `-Mode Diagnose` | Solo diagnóstico, no aplica nada |
| `-Mode OptimizeKernel` | Solo las optimizaciones de kernel |
| `-Mode CleanSystem` | Solo la limpieza del sistema |
| `-Mode PurgeForensics` | Solo privacidad y purga forense |
| `-Mode FullOptimize` | Todo: kernel + limpieza + privacidad |
| `-Mode FullPurge` | Limpieza + privacidad (sin kernel) |
| `-Reboot` | Reinicia al finalizar |
| `-Shutdown` | Apaga al finalizar |
| `-LogPath "C:\log.txt"` | Exporta el output completo a un archivo |
| `-Quiet` | Solo errores en consola |

---

## Qué hace cada sector

### Sector 0 — El Motor (invisible)

Antes de tocar cualquier cosa el script hace tres cosas que no se ven pero son las más importantes:

**Detección de hardware real.** No asume nada. Detecta si el disco es SSD o HDD por el modelo del fabricante, no por tests de escritura que dañan el disco. Lee los núcleos físicos del CPU, la RAM total y velocidad, si es laptop o desktop. Cada optimización usa estos datos para calibrarse — no aplica lo mismo en un i9 con NVMe que en un i5 con HDD y 4GB de RAM.

**Backup automático del registro.** Antes de cualquier cambio, exporta un archivo `.reg` completo a `Documentos\WDM_BlackEdition_Backups\`. Si algo sale mal, el botón "Restaurar Backup" o `-Undo` revierte todo. Los backups están numerados por fecha y hora.

**Forzado de STA sin matar la consola.** WinForms requiere Single Thread Apartment. PowerShell 7 no arranca en STA por defecto. La solución obvia — relanzar el proceso con `-STA` — mata la consola de origen. La solución real es crear un nuevo thread STA dentro del mismo proceso. Ese detalle es el que diferencia una herramienta que funciona de una que cierra PowerShell misteriosamente.

---

### Sector 1 — Diagnóstico Completo

Analiza el sistema antes de tocar nada. No es decorativo: los resultados calibran todo lo que viene después.

**Hardware detectado:**
- CPU: nombre completo, cores físicos y lógicos, frecuencia base
- RAM: total en GB, slots ocupados, velocidad en MHz, alerta si < 8GB
- Disco: SSD o HDD, espacio libre y porcentaje, alerta si < 10% libre
- Equipo: laptop o desktop, batería si aplica

**Windows y sistema:**
- Versión y build exacto
- Días de uptime (alerta si > 14 días sin reiniciar)
- Dirty bit del disco — señal de inconsistencias no resueltas
- Archivos críticos del sistema: ntoskrnl.exe, kernel32.dll
- Operaciones pendientes de reinicio

**11 parámetros de kernel verificados:**

| Parámetro | Qué mide |
|---|---|
| Timer Resolution | ¿El sistema reacciona cada 15.6ms o cada 0.5ms? |
| Core Parking | ¿Los cores están disponibles inmediatamente? |
| NTFS Last Access | ¿Cada lectura genera también una escritura? |
| Registry Lazy Flush | ¿Con qué frecuencia se escribe el registro al disco? |
| AlwaysUnloadDLL | ¿La RAM se libera al cerrar programas? |
| Power Throttling | ¿Windows limita el CPU en segundo plano? |
| Startup Delay | ¿Hay una espera artificial al arrancar programas? |
| I/O Priority | ¿La prioridad de disco está calibrada para tu hardware? |
| MMCSS | ¿El planificador multimedia está calibrado por cores? |
| Win32PrioritySeparation | ¿El CPU prioriza la app activa correctamente? |
| Plan de energía | ¿El plan frena el CPU para ahorrar energía? |

**Disco y arranque:**
- Tamaños reales de WinSxS, DriverStore, CBS Logs, Amcache, MFT
- Tiempo de boot en segundos desde el Event Log de Performance
- Errores críticos de las últimas 48hs con filtro inteligente: separa ruido normal del sistema de errores reales
- Latencia de WMI en milisegundos

Todo con código de color: verde = bien, naranja = atención, rojo = problema grave.

---

### Sector 2 — Kernel y Rendimiento

**28 optimizaciones** calibradas por hardware. No aplica lo mismo en todos los equipos.

#### Latencia y temporizador

**Timer Resolution (15.6ms → 0.5ms)**
El timer del sistema controla cada cuánto Windows "mira el reloj". Por defecto 15.6ms — reacciona 64 veces por segundo. Al bajarlo a 0.5ms reacciona 2000 veces por segundo. La diferencia se nota en gaming, audio profesional y cualquier tarea con requerimientos de tiempo real. Se implementa via `powercfg` en el plan de energía — el método correcto y persistente, no via `timeBeginPeriod` que solo dura hasta el próximo reinicio.

**Core Parking (solo desktop)**
Windows apaga cores del CPU cuando no los necesita para ahorrar energía. Al despertar un core hay latencia medible. En desktop se desactiva para que todos los cores respondan instantáneamente. En laptop se conserva — tiene sentido ahí.

> **Por qué no tocamos el BCD (bootloader)**
> Algunas guías recomiendan deshabilitar HPET, Dynamic Tick, etc. via `bcdedit`. No lo incluimos porque: el beneficio es marginal en hardware moderno, un error en el BCD puede dejar el sistema sin arrancar, y esas configuraciones varían según el hardware específico. El riesgo supera el beneficio para el público general.

#### Memoria RAM

**DisablePagingExecutive** (solo si ≥ 8GB)
Por defecto Windows puede mover partes del kernel al pagefile. Con 8GB o más hay suficiente RAM para mantenerlo todo en memoria. La detección es automática — si tenés 4GB el ítem aparece marcado como "no aplica" con la explicación de por qué.

**LargeSystemCache** (solo si ≥ 16GB)
Controla si Windows reserva casi toda la RAM libre como cache del sistema (modo servidor) o la deja para programas (modo workstation). Para escritorio con 16GB+ se optimiza en modo workstation.

**IoPageLockLimit**
El buffer de operaciones de E/S en curso. En SSD puede ser más grande (4GB) porque el disco maneja muchas operaciones simultáneas. En HDD se reduce (1GB) para no desperdiciar RAM en un disco lento. Calibrado automáticamente por tipo de disco detectado.

#### Sistema y usuario

**Win32PrioritySeparation**
Define la diferencia de prioridad entre el programa activo y los procesos en segundo plano. Se calibra por cores:
- 2 cores → 18 (conservador, el background también necesita recursos)
- 4 cores → 26 (balance)
- 6+ cores → 38 (máxima prioridad al foreground, sobran recursos)

**Bloatware UWP**
Windows viene con apps que nadie pidió: Bing News, Bing Weather, Skype, Clipchamp, Xbox Game Overlay, Solitario, Mixed Reality Portal y más. Cada una tiene procesos en segundo plano activos aunque nunca las hayas abierto. Se desinstalan via `Remove-AppxPackage` — se pueden reinstalar desde la Store si se necesitan.

> **Por qué no deshabilitamos el DNS Client**
> Algunas guías recomiendan deshabilitar el servicio DNS Client para "ahorrar RAM". Eso hace más lenta la resolución de DNS porque deshabilita el cache local. Solo deshabilitamos servicios donde el beneficio es claro y el riesgo mínimo: telemetría, servicios de Xbox, indexación en HDD, etc.

---

### Sector 3 — Limpieza Profunda

**27 categorías** ordenadas automáticamente por tamaño real. La más grande primero. Cada ítem muestra cuánto pesa antes de borrar.

#### Sistema

| Ítem | Qué es | Riesgo |
|---|---|---|
| Archivos temporales | Residuos de %TEMP% y C:\Windows\Temp | Ninguno |
| Cache Windows Update | Actualizaciones ya instaladas | Ninguno — se re-descargan si se necesitan |
| Cache de miniaturas | thumbcache_*.db del Explorer | Ninguno — se regeneran |
| Volcados de memoria | MEMORY.DMP y Minidump\*.dmp | Ninguno si no investigás BSODs |
| Logs CBS/DISM | Diarios de actualizaciones (hasta 500MB) | Ninguno |
| Logs de Event Log | Archivos .evtx de todos los canales | Ninguno |
| Prefetch | Archivos de precarga | Ninguno — se regeneran |
| Windows.old | Instalación anterior (5-25GB) | Ninguno si el sistema nuevo funciona bien |
| WinREAgent | Residuos del proceso de actualización | Ninguno |
| FontCache | Cache de fuentes del sistema | Ninguno — se regenera al reiniciar |
| Thumbs.db | Miniaturas ocultas en carpetas | Ninguno — se regeneran |

#### Apps

| Ítem | Por qué acumula tanto |
|---|---|
| Teams | Cache de reuniones, GPU cache, imágenes — puede superar 3GB |
| Chrome / Edge / Firefox | Cache de sitios web visitados |
| Discord | Cache de imágenes, GIFs y videos de servidores |
| Spotify | Canciones y podcasts en cache local |
| VS Code | Cache de extensiones y datos compilados |
| Office | Archivos de recuperación automática y documentos recientes |
| Shader Cache GPU | Shaders compilados — inútiles al cambiar drivers |
| Delivery Optimization | Cache P2P de actualizaciones de Windows |

#### Registro inteligente — 19 tipos verificados

La limpieza del registro verifica antes de borrar. Nunca elimina una entrada válida.

**MUICache** — nombres de apps desinstaladas que quedaron en el registro
**AppCompatFlags** — overrides de compatibilidad para programas que ya no existen
**Uninstall fantasma** — desinstaladores que apuntan a rutas inexistentes
**SharedDLLs** — referencias a DLLs con contador en cero y archivo eliminado
**BagMRU/Bags** — memoria visual de carpetas del Explorer
**RunMRU** — historial del cuadro Ejecutar (Win+R)
**TypedURLs** — URLs escritas en IE/Edge Legacy
**RecentDocs** — documentos recientes por categoría
**UserAssist** — contadores de uso de programas
**LastVisitedMRU** — rutas usadas en diálogos "Abrir"
**OpenSaveMRU** — rutas usadas en diálogos "Guardar como"
**WordWheelQuery** — búsquedas en el Explorer
**TypedPaths** — rutas escritas en la barra del Explorer
**Run/RunOnce huérfanos** — entradas de arranque que apuntan a exe eliminados
**Protocol handlers huérfanos** — spotify://, discord://, etc. de apps desinstaladas

#### Winapp2 — La base de datos extendida

Winapp2 es una base de datos comunitaria con más de 2400 reglas de limpieza, mantenida activamente en GitHub. Es la misma base que usa CCleaner en su versión extendida — sin instalar CCleaner, sin su telemetría, gratis.

El proceso en detalle:
1. Descarga `Winapp2.ini` de GitHub en tiempo real
2. Parsea las 2400+ reglas del archivo INI
3. Para cada regla, verifica si el programa está instalado en ESTE sistema via `DetectFile` o clave de registro — antes de intentar limpiar cualquier cosa
4. Ejecuta solo las que aplican y muestra el resultado

**Resultado real en pruebas:** 328 de 2400+ reglas aplicaron a un sistema típico con Windows 10, liberando 80MB adicionales de caches de programas no cubiertos por las reglas propias del script.

Hay dos opciones: **PREVIEW** — muestra qué va a limpiar sin tocar nada — y **Ejecutar** — aplica todo. Se recomienda empezar con el preview.

> **Por qué no incluimos las reglas de Winapp2 directamente en el script**
> Porque Winapp2 se actualiza constantemente — cada semana se agregan reglas para nuevas versiones de programas. Al descargarla en tiempo real siempre tenés la versión más reciente sin actualizar el script.

---

### Sector 4 — Privacidad y Purga Forense

Dos niveles: privacidad básica (lo que cualquier usuario debería tener) y purga forense (para quienes necesitan que el sistema no recuerde lo que hicieron).

#### Privacidad básica

**ID de Publicidad**
Windows le asigna un identificador único a tu PC — como una cookie, pero del sistema operativo — para que las apps te rastreen entre ellas y sirvan publicidad personalizada. Al desactivarlo dejás de ser parte de esa red de seguimiento sin afectar ninguna funcionalidad visible.

**Telemetría (DiagTrack)**
Windows envía constantemente datos a Microsoft: qué apps abrís, cuánto tiempo las usás, qué errores ocurren, cómo usás el menú inicio. El servicio DiagTrack ("Experiencia del usuario y telemetría de diagnóstico") se detiene, se deshabilita, y se corta la clave de política para que no se reactive con una actualización futura.

**Search Highlights**
La caja de búsqueda del menú inicio no solo busca en tu PC — también carga noticias, tendencias y contenido dinámico de internet en segundo plano, constantemente. Al desactivarlo la búsqueda queda 100% local y sin conexiones externas.

**GameDVR / Xbox Game Bar**
Graba los últimos X minutos de pantalla en segundo plano "por si querés capturar algo". Consume CPU, GPU y RAM aunque nunca la hayas usado conscientemente. Especialmente problemático en equipos con hardware justo donde cada recurso importa.

**Windows Recall** (Win11 solamente)
Recall es una función de IA que toma capturas de pantalla cada pocos segundos y las indexa con OCR para que puedas "buscar cosas que viste". Literalmente guarda una foto de todo lo que apareció en pantalla — contraseñas, datos bancarios, conversaciones privadas. Al desactivarla se eliminan todos los snapshots existentes y se bloquea el servicio via política.

**OneDrive KFM (Known Folder Move)**
OneDrive puede mover silenciosamente tus carpetas de Documentos, Escritorio e Imágenes a la nube, mostrando apenas un cartel que es fácil de aceptar sin leer. Al bloquearlo via política Windows deja de sugerirlo y de hacerlo automáticamente — sin desinstalar OneDrive.

#### Purga forense

Para quien necesita que el sistema no recuerde lo que hizo. Riesgo cero para el funcionamiento — solo borra rastros de actividad.

**Amcache.hve**
El historial de TODOS los ejecutables que corrieron en el sistema, con hash SHA-1 y fecha exacta. Es una de las primeras fuentes que consulta un analista forense. Al purgarlo se pierde ese historial — Windows lo vuelve a generar pero vacío.

**UserAssist**
El registro de cuántas veces abriste cada programa y cuándo fue la última vez. Está codificado en ROT13 en el registro — una codificación trivial que cualquier herramienta forense decodifica en segundos.

**ShellBags**
La "memoria visual" del Explorer: qué carpetas abriste, el tamaño y posición de cada ventana, el orden de las columnas. La particularidad de ShellBags es que recuerda rutas que ya no existen — un pendrive de hace meses, una carpeta de red, una ruta que borraste. Una de las fuentes de evidencia más ricas en análisis forense.

**USBSTOR**
Cada dispositivo USB que conectaste alguna vez deja una huella en el registro: modelo, número de serie, fecha de primera y última conexión. Al borrar USBSTOR se elimina esa lista completa.

**USN Journal**
El Update Sequence Number Journal es la bitácora donde NTFS anota cada creación, modificación, renombre y eliminación de archivo — un registro detallado de toda la actividad en el disco. No lo eliminamos (el sistema lo necesita) — lo reseteamos para que empiece desde cero.

> **Por qué no incluimos wipe de espacio libre**
> Algunas herramientas ofrecen "limpiar el espacio libre" sobreescribiendo con ceros para prevenir recuperación de archivos. En SSD esto es inútil (el controlador redistribuye las escrituras internamente) y reduce la vida útil del disco. En HDD el beneficio es real pero la operación puede tardar horas. No lo incluimos.

---

### Sector 5 — Reparación del Sistema

**Detección inteligente antes de pre-marcar.** Antes de sugerir DISM o SFC, el script lee el `CBS.log` de las últimas 24 horas. Si corrieron recientemente y fueron exitosos, aparecen en verde con el texto "Ejecutado recientemente — sistema OK" y sin marcar. Si hay problemas Y no corrieron recientemente, se marcan en naranja.

**DISM /RestoreHealth**
Descarga archivos del sistema desde los servidores de Microsoft y repara la imagen local. Paso recomendado antes de SFC cuando hay corrupción seria. Requiere internet. Tiempo: 8-35 minutos según la conexión.

Corre en un `Job` de PowerShell separado — no bloquea la interfaz. Un timer de 400ms lee el output del job y actualiza la barra de progreso en tiempo real. Ese diseño (Job + Timer en lugar del loop clásico de `ReadToEnd`) fue necesario para que la GUI no se congele en PowerShell 7. Tomó varias iteraciones llegar a esa solución.

**SFC /scannow**
Escanea todos los archivos protegidos de Windows y reemplaza los dañados con copias del cache local. Más rápido que DISM pero más limitado — solo puede reparar lo que tiene disponible localmente. Si SFC dice que no pudo reparar algo, el siguiente paso es DISM. Mismo diseño de Job + Timer.

**CHKDSK**
Verifica y repara el sistema de archivos a nivel físico. Si el diagnóstico detecta dirty bit activo lo pre-marca automáticamente. Se programa al próximo reinicio — necesita acceso exclusivo al disco.

**Reconstruir WMI**
WMI (Windows Management Instrumentation) es la base de datos de estado del sistema. Cuando se corrompe los síntomas son: scripts de administración que fallan, Task Manager que tarda en abrir, herramientas de diagnóstico que no responden. El script detiene el servicio, resetea el repositorio y recompila todos los archivos MOF.

**Consolidar WinSxS**
Puede liberar entre 2 y 8GB marcando la instalación actual como definitiva y eliminando los backups de actualizaciones anteriores. **Irreversible** — después no se pueden desinstalar las actualizaciones previas. Está claramente advertida en la interfaz y nunca se pre-marca automáticamente.

---

### Sector 6 — Interfaz Gráfica

- **Modo oscuro por defecto** con toggle a modo claro
- **Panel izquierdo**: hardware detectado, boot time, historial de las últimas 100 acciones con fecha
- **Panel central con 5 pestañas**: Diagnóstico, Kernel/Rendimiento, Limpieza, Privacidad/Forense, Reparación
- **Panel derecho**: descripción de cada ítem al hacer clic + log en tiempo real de las operaciones en curso
- **Vista Previa (WhatIf)**: toggle en la barra inferior — simula todo sin aplicar nada, desde la GUI
- **Resumen Final**: al terminar muestra GB liberados, optimizaciones aplicadas, y dónde está el backup
- **Limpieza ordenada por tamaño**: los ítems más grandes aparecen primero, con tamaño real calculado antes de mostrar

---

### Sector 7 — Modo Avanzado / Headless

Para administradores, scripting remoto o automatización:

```powershell
# Diagnóstico completo en consola
.\WindowsDeMente_BlackEdition.ps1 -NoGUI -Mode Diagnose

# Optimización completa con log
.\WindowsDeMente_BlackEdition.ps1 -NoGUI -Mode FullOptimize -LogPath "C:\wdm_$(Get-Date -f yyyyMMdd).txt"

# Solo limpieza, reiniciar al terminar
.\WindowsDeMente_BlackEdition.ps1 -NoGUI -Mode CleanSystem -Reboot

# Simular optimización completa sin tocar nada
.\WindowsDeMente_BlackEdition.ps1 -NoGUI -Mode FullOptimize -WhatIf

# Rollback completo
.\WindowsDeMente_BlackEdition.ps1 -Undo
```

---

## Preguntas frecuentes

**¿Esto puede romper Windows?**
Cada sesión de cambios tiene backup automático previo. Las operaciones irreversibles están advertidas. El modo WhatIf permite simular todo antes. Y el botón Restaurar Backup revierte los cambios de registro en cualquier momento. El riesgo real es mínimo — menor que instalar cualquier programa de terceros.

**¿Funciona en Windows 10 y 11?**
Sí. El script detecta la versión y adapta: algunas optimizaciones son exclusivas de Win11 (Recall, Widgets), otras de Win10. El diagnóstico muestra el build exacto y calibra en consecuencia.

**¿Necesito internet?**
Solo para DISM /RestoreHealth y Winapp2. Todo lo demás funciona sin internet.

**¿Qué pasa si deshabilité un servicio que necesito?**
Los servicios se pueden rehabilitar en cualquier momento desde `services.msc`. Si usás Xbox Game Pass o el buscador de Windows activamente, podés desmarcar esos ítems antes de aplicar — la interfaz permite seleccionar exactamente qué querés hacer.

**¿Por qué no optimiza la RAM con "liberadores de memoria"?**
Porque no funcionan. La RAM "usada" incluye el cache del sistema, que se libera automáticamente cuando una app lo necesita. Un "liberador de RAM" solo borra ese cache, haciendo que el sistema tarde más porque tiene que volver a leer del disco lo que ya tenía en memoria. Es una optimización que en realidad empeora el rendimiento.

**¿Por qué no incluye desfragmentación?**
En SSD la desfragmentación es perjudicial — reduce la vida útil sin beneficio real. En HDD Windows 10/11 ya la programa automáticamente. No hay nada útil que agregar.

---

## Comparativa con otras herramientas

| Característica | WDM Black Edition | CCleaner | Advanced SystemCare | Script manual |
|---|---|---|---|---|
| Explicación de cada acción | ✅ Completa en español | ❌ | ❌ | Depende |
| Backup automático previo | ✅ Siempre | ❌ | ❌ | Depende |
| Modo simulación (WhatIf) | ✅ GUI y consola | ❌ | ❌ | ❌ |
| Calibrado por hardware | ✅ RAM, disco, cores | ❌ | ❌ | ❌ |
| Purga forense completa | ✅ 7 tipos | Parcial | ❌ | Depende |
| Winapp2 integrado | ✅ Con preview | Versión propia | ❌ | ❌ |
| Sin telemetría propia | ✅ | ❌ | ❌ | ✅ |
| Sin instalación | ✅ Un archivo | ❌ | ❌ | ✅ |
| PowerShell 5 y 7 | ✅ Auto-detecta | N/A | N/A | Depende |
| Código auditable | ✅ 100% | ❌ | ❌ | ✅ |
| Precio | Gratis | Freemium | Freemium | Gratis |

---

## Seguridad y reversibilidad

**Nada es irreversible por defecto.** La única excepción es "Consolidar WinSxS" y está explícitamente advertida en la interfaz.

Los backups del registro se crean antes de cada sesión, no una vez al instalar. Si usás el script tres días distintos, tenés tres backups distintos. Se guardan en `Documentos\WDM_BlackEdition_Backups\` y podés abrirlos con doble clic para restaurarlos manualmente si preferís.

El modo WhatIf simula toda la ejecución sin tocar nada — muestra exactamente qué hubiera hecho.

---

## Historia del proyecto

Este script es la fusión evolucionada de tres proyectos:

**Windows De Mente v2** — la base de la interfaz gráfica y la filosofía de "explicar todo antes de hacerlo"

**Lazarus V2** — las optimizaciones de kernel calibradas por hardware y la limpieza de registro inteligente de 19 tipos

**Lazarus-Nitidus 2026** — la purga forense completa: Amcache, ShellBags, USBSTOR, USN Journal, TxfLog

La fusión no fue simplemente juntar los tres scripts. Fue reescribir todo desde cero tomando lo mejor de cada uno, resolviendo los conflictos entre sus arquitecturas, y construyendo encima una interfaz que cualquier usuario pueda usar sin necesitar saber qué es el registro de Windows.

El desarrollo incluyó decenas de pruebas reales en hardware variado y debugging de bugs específicos de PowerShell 7 + WinForms:
- El problema de STA que cerraba la consola de origen
- El crash de `ObjectDisposedException` en `SafeWaitHandle`
- El orden de `Dock` en WinForms que dejaba el panel central negro
- DISM y SFC que bloqueaban el hilo de la UI

Cada uno de esos bugs tiene su historia y su solución concreta en el código.

---

## Licencia

MIT — libre para usar, modificar y distribuir. Si lo mejorás, compartí los cambios.

---

## Contribuir

Issues y Pull Requests bienvenidos. Si encontrás un bug o querés agregar un ítem con su explicación, adelante.

La única condición: cada ítem nuevo necesita su campo `Explicacion` completo en español llano — esa es la parte que más valor aporta al usuario final.

---



**Windows De Mente — Black Edition** · PowerShell · Windows 10 / 11 · v3.0

*Hecho con mucho mate y rock and roll, demasiados `str_replace` fallidos, y la convicción de que las herramientas de sistema deberían explicar lo que hacen.*

</div>
