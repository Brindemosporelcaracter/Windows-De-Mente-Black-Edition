DeMente 1.0.0

Windows se puede cuidar sin pagar por miedo ni por magia.

Hay mil “optimizadores” en la web. Muchos cobran. Otros rompen. Casi todos te muestran un puntaje inventado (95/100) y un botón grande que “arregla todo” sin decirte qué tocó ni cuál era el valor de Windows antes.

DeMente es lo contrario.

Es un script gratuito de PowerShell, pensado para alumnos de secundaria y facultad, para padres que no quieren gastar en software dudoso, y para cualquiera que quiera entender su PC en lugar de entregársela a una caja negra. No promete milagros. No esconde cambios. No te vende una suscripción para escanear con YARA.

Si algo se puede mejorar, te lo dice.
Si Windows ya está bien, también te lo dice.
Si un ajuste se puede deshacer, DEFAULT WINDOWS existe para eso.



Qué es (y qué no es)







DeMente sí



DeMente no





Muestra el valor DEFAULT de Windows y lo que propone cambiar



Inventa un “score de salud” para asustarte





Deja que vos elijas qué aplicar



Obliga a un “optimizar todo” a ciegas





Explica en castellano claro



Esconde tweaks en jerga de foro





Incluye reversión donde tiene sentido



Es un “tuneador” irreversible de GitHub de moda





Usa YARA y Defender sin pagar



Un antivirus de reemplazo ni un sustituto de copias de seguridad

Inspiración honesta (lo bueno, sin copiar el modelo de pago): ideas útiles de herramientas como Winhance, BleachBit, WinToys o utilidades de mantenimiento conocidas. La diferencia es la filosofía: educar y no romper.



Requisitos





Windows 10 u 11



PowerShell (viene con el sistema)



Ejecutar como administrador para la mayoría de cambios de sistema



Conexión a internet solo si actualizás YARA o usás DISM RestoreHealth (Windows Update)

Cómo ejecutarlo

Clic derecho en DeMente_1_0_0.ps1 → Ejecutar con PowerShell (idealmente elevado), o desde una consola admin:

powershell -NoProfile -ExecutionPolicy Bypass -STA -File ".\DeMente_1_0_0.ps1"

La primera vez Windows puede advertir por scripts descargados: es normal. El código es legible; podés abrirlo con el Bloc de notas y revisar qué hace.



Cómo está organizado

Al abrir, DeMente hace un multiescaneo del equipo (hardware, disco, seguridad, privacidad, servicios, etc.) y te resume el estado sin inventar un número mágico.

Modos de cada sección







Modo



Idea





ESENCIAL



Lo seguro y de impacto claro. Ideal la primera vez o para alumnos.





COMPLETO



Todo lo razonable de esa sección. Revisá antes de ejecutar.





DEFAULT WINDOWS



Marca lo reversible y restaura el valor de fábrica de Windows (no deja la selección vacía “porque sí”).

F5 vuelve a diagnosticar de verdad y actualiza tilde/estado de lo ya aplicado.



Secciones: qué hace y por qué

Panel

Centro de mando: hallazgos con nombre (no solo “hay algo raro”), resumen por áreas y accesos rápidos.
Por qué: arrancar mirando evita tocar lo que no hace falta.

Limpieza

Temporales, Papelera, cachés de navegadores, DNS, reportes de error, recientes, etc.
Por qué: basura acumulada come disco y a veces frena actualizaciones. No toca tus documentos ni contraseñas de favoritos “a propósito”.

Rendimiento

Prioridad de CPU, efectos visuales, menús, plan de energía, Ultimate Performance, inicio rápido, Storage Sense, menú clásico, NTFS, Superfetch/SysMain, indexación, hibernación, red (QoS, Nagle, IPv6), apagado más ágil…
Cada ítem indica DEFAULT de Windows vs propuesta DeMente.
Por qué: muchos “boosters” cambian todo en silencio. Acá ves el antes/después y podés volver atrás.

Privacidad

ID de publicidad, telemetría, historial de actividad, micrófono/cámara, Bing en búsqueda, GameDVR, portapapeles en la nube, tips, Copilot del shell, apps en segundo plano, etc.
Por qué: menos ruido y menos datos de más, sin pretender que Windows se vuelva un sistema anónimo total.

Seguridad

Estado legible de Microsoft Defender y del motor YARA local (gratis, sin suscripción). Escaneos y actualización de reglas cuando corresponde.
Por qué: Defender ya está en el sistema; YARA suma reglas propias sin paywall absurdo.

Reparación (guiada)

No es un botón “reparar PC”. Es una ruta:





Guía: ¿hace falta reparar? — lectura del diagnóstico, en orden humano



Secuencia guiada DISM → SFC — CheckHealth → ScanHealth → RestoreHealth (si hace falta) → SFC



Al final: reiniciar, apagar o nada



Pasos sueltos: DISM Check / Scan / Restore / limpieza de componentes, SFC, CHKDSK, red, WMI

Por qué: el orden importa. SFC sobre una imagen rota a veces no alcanza; DISM primero es la práctica seria. CHKDSK solo si el disco es el sospechoso.

Información

Hardware, discos, eventos, programas al inicio, ¿Por qué está lenta mi PC? (causas probables, no otro optimizador disfrazado).
Por qué: diagnosticar antes de “tunear”.



Detalles que importan





No es demo: si aplicás un ajuste y el sistema lo acepta, al refrescar (F5) debería marcarse como aplicado (tilde) cuando DeMente puede comprobarlo en el registro o en la sesión.



Consola en castellano legible (Defender, YARA, guías): menos JSON crudo, más “qué significa esto”.



Peligrosas van marcadas; WMI y similares no son juguetes.



Proxy: si tu red (por ejemplo institucional) usa proxy, YARA puede detectarlo al actualizar.



Buenas prácticas (léelas en serio)





Creá un punto de restauración de Windows antes de un COMPLETO agresivo.



Empezá con ESENCIAL.



No marques todo “por las dudas”.



Si algo no te gustó: sección → DEFAULT WINDOWS → Restaurar DEFAULT.



Reparación: usá la secuencia guiada, no un festival de DISM+SFC+CHKDSK al mismo tiempo sin leer.



DeMente no reemplaza backup, sentido común ni un técnico cuando el disco está muriendo.



Línea de comandos (opcional)

.\DeMente_1_0_0.ps1 -ListTools
.\DeMente_1_0_0.ps1 -RunTool clean-temp
.\DeMente_1_0_0.ps1 -SelfTest



Filosofía en una frase



Si un ajuste no se puede explicar a un chicx o a un padre, no debería ser un botón mágico de pago.

DeMente 1.0.0 es un PS1 a propósito: portable en el sentido útil (un archivo, código abierto a la lectura, sin instalador oscuro). Si más adelante hace falta un launcher o empaquetado para aulas con ExecutionPolicy trabada, será evolución — no una excusa para ocultar lo que hace el programa.



Créditos y uso

Uso libre con responsabilidad.
Hecho para que dejen de circular “tweaks” que rompen equipos en silencio.

Ayudarnos es la única opción — y empezar por entender el propio Windows también.



Versión 1.0.0 — script PowerShell, sin puntajes inventados y con DEFAULT de Windows a la vista.
