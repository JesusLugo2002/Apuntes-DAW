# Resumen Sistemas

<div align=center>
<img src="https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExMmswYzNmeWVtaDY2b3p6ajBoMXh6NHNtbWp0bGJncGhkNG13NzE1ayZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/lgaHEvyUjdHY4/giphy.gif"/>
<br>
Aquí mis apuntes de sistemas.
</div>

<div align=justify>

## Índice

- [Servicios y demonios](#servicios-y-demonios)
- [Programación de tareas](#programación-de-tareas)

## Servicios y demonios

- Son procesos que llevan a cabo tareas especiales. En Linux se le conocen como 'demonios'.
- El nombre está inspirado en los _demonios de Maxwell_.
- Se ejecutan en segundo plano.
- Carecen de interfaz gráfica.
- No suelen hacer uso del input/output del usuario; si se da el caso, lo más común es que se realice mediante una aplicación _cliente_.
- Para indicar que un proceso es un servicio comúnmente se suele colocar una 'd' al final de su nombre. (Ex: cron / crond)
- Son gestionados y controlados por el sistema operativo (Sistema init de Linux, administrador de servicios de Windows...) y además de proporcionar operaciones básicas debe cumplir con ciertos protocolos.
- En entornos GNU/Linux, suelen ejecutarse con privilegios _root_ y la mayoría de las veces su proceso padre es el _init_ (PID 1).
- En la ejecución de un servicio existen dos tipos de usuarios: el _usuario real_ (quien ejecuta) y el _usuario efectivo_ (en donde se ejecuta).
- Al instalar aplicaciones de terceros pueden darse casos de la instalación de servicios asociados.

### Sistema Init de GNU/Linux

- Al encender un equipo UNIX se carga en memoria el kernel y luego se inicia, debiendo existir un proceso _padre_ que se encargue de iniciar directa o indirectamente a todos los procesos del sistema. Este proceso padre es el _init_, de PID 1, y es crucial para el funcionamiento del sistema operativo.
- El proceso _init_ también establece los __niveles de ejecución__ que indican qué servicios seran ejecutados. 
- Los sistemas de inicialización gestionan el proceso _init._
- El sistema de inicialización más actual es el __systemd__.
- La principal desventaja del _systemd_ es que es un sistema bastante complejo que acapara gran parte de las funciones de un kernel y rompe la filosofía de Linux de _"haz una sola cosa pero bien"_.

### Funcionamiento de los servicios 

- El servicio puede estar activado/desactivado (en funcionamiento o no), habilitado/deshabilitado (será iniciado durante el arranque, o no) y desenmascarado/enmascarado (puede o no ser iniciado por cualquier otro proceso o el arranque mismo).
- Al realizar los cambios de configuración en un servicio (normalmente encontrados en el directorio /etc) lo más probable es que sus cambios no sean aplicados hasta el siguiente inicio del servicio.
- El reinicio de un servicio es crítico y puede traer graves consecuencias por las interrupciones que pueden causarse. Para esto existen los comandos como `reload` que intentan recargar el servicio sin interrumpirlo para aplicar la nueva configuración.

### Runlevels (Niveles de ejecución)

Son 6 los niveles de ejecución:
- Runlevel 0: apagado o cierre del equipo
- Runlevel 1: Monousuario (solo root), sin red ni entorno gráfico. Usado como modo de emergencia para solucionar problemas.
- Runlevel 2: Multiusuario, sin red ni entorno gráfico.
- Runlevel 3: Multiusuario, con soporte de red pero sin entorno gráfico.
- Runlevel 4: Reservado.
- Runlevel 5: Modo normal de uso.
- Runlevel 6: Reboot del equipo.

## Programación de tareas

- Algunas tareas que suelen programarse por su larga duración o consumo de memoria/cpu, suelen ser las copias de seguridad, migraciones, baterías de tests, auditorías de seguridad...
- Hay varios tipos de tareas programadas, como las de __tiempo__ (se ejecutan en una fecha/hora determinada), como por __evento__ (se ejecutan bajo algún hecho determinado del equipo).
- Estás tareas son realizadas por servicios que estarán activas monitorizando el tiempo o los eventos que _disparen_ (__"Triggers"__) y desencadenan tareas.
- Si el servicio no estuviera activo al momento que debe ejecutarse la tarea, este puede ignorarse, ejecutarse al iniciarse nuevamente, reprogramarse... dependiendo de la configuración del servicio.
- Por lo general estas tareas programadas son ejecutadas en una _terminal virtual_ cuyos datos de output (stdout y stderr), se pierden al finalizar la ejecución.
- Por otro lado, el directorio en el que se ejecutan las tareas no suele ser el esperado y por eso se recomiendo el uso de rutas absolutas para especificar ficheros.
- Usa redirecciones (> >> <<<) para almacenar la información y evitar su pérdida en la terminal virtual. Si hubiesen entradas de teclado, también se debe redirigir la entrada (stdin).

### Tareas por tiempo: puntuales (Comando At)

- Se suele usar el comando `at` (normalmente no viene instalada por defecto en el sistema UNIX).
- La hora se puede indicar tanto en el formato de 12 horas como el de 24. Las fechas, como recomendación, se indican en el formato japonés (YY-MM-DD)
- También soporta el uso de 'lenguaje natural' como _tomorrow, tuesday, next week, now + 10 minutes_, y _noon, midnight, teatime..._
- Una vez se invoca el comando `at` con una fecha determinada (Ex: `at 20:00 tomorrow`), se abre un pequeño intérprete de código para programar las tareas.
- Después de especificar los comandos y su redirección (Ex: `who >> ~/usuarios.txt 2>> ~/usuarios.err`), sales del intérprete con CTRL+D.
- Para mostrar la lista de tareas programadas: `atq` o `at -l`.
- Para mostrar los detalles de una tarea por su id: `at -c id`.
- Para borrar una tarea por su id: `atrm id` o `at -r id` o `at -d id`.

### Tareas por tiempo: repetitivas (Comando Cron)

- Hace referencia a Cronos.
- No posee un cliente para especificar una tarea, sino que deben escribirse en una tabla (crontab) con formato especial.
- Esta tabla es leida por el demonio _cron_ (_crond_ en algunos sistemas)
- Para editar la tabla, se debe ejecutar `crontab -e`.
- El formato que sigue es:
    1. __m__: minutos (de 0 a 59)
    2. __h__: hora (de 0 a 23)
    3. __dom__: día del mes (del 1 al 31 dependiendo del mes)
    4. __mon__: mes (de 1 a 12)
    5. __dow__: día de la semana donde 0 es domingo, 1 es lunes, 2 es martes...
    6. __command__: comando o script a ejecutar.

Un ejemplo de aplicación es:

    Usar un script para borrar ficheros temporales todos los días a las 12:15
    m h dom mon dow cmd
    15 12 * * * ~/borrar_temporales.sh >> ~/borrar_temporales.out 2>> ~/borrar_temporales.err

Los valores también se pueden especificar en estos símbolos:
- Cualquier valor: *
- Rangos (-): 4-8 (ambos inclusive)
- Lista (,): 3,6,9,12
- Intervalos (/): */5 (cada 5 = 0,5,10,15)
- Son combinables: 6-12/2 = 6,8,10,12

Otra operaciones utiles:
- Listar todas las tareas: `crontab -l`
- Borrar todas las tareas: `crontab -r`
- Para borrar una tarea en específico, se debe ingresar a la tabla y borrar su línea o comentarla con #

### Comando Watch

- Cron tiene una precisión de minutos, por lo que no es capaz de ejecutar tareas rápidamente en segundos, para eso usamos el comando Watch
- Ejecuta un comando de forma periódica, mostrando la salida en pantalla completa.
- Entre sus opciones se encuentra `-n X` (ejecuta el comando cada X segundos), `-d` (resalta diferencias en la salida), `-p` (aumenta la precisión en el cálculo de intervalos del tiempo), `-b` (emite un sonido si el comando arroja algún error).
- Un ejemplo del comando watch, en el que se simula el funcionamiento de `top`: `watch -n 1 -d ps aux`

### Tareas por eventos

- __Tareas cuando el sistema está "ocioso" (con baja carga, _idle_):__ el comando `batch`, encontrado en el paquete de `at`.
- __Tareas relacionadas con el sistema de ficheros__: el paquete `incron` (ejecutar una tarea al crear o borrar ficheros...)
- __Durante el arranque__: con el atajo @reboot en cron, si este lo soporta (Ex: `@reboot sleep 300 && ~/mi_script.sh`)
- __Cuando un usuario se conecta__: escribiendo los comandos en /etc/profile.d/
- __Cuando se abre una terminal__: escribiendo los comandos al final del fichero ~/.bashrc

## Usuarios y grupos

- El concepto de usuarios en Linux permite separar entornos de ejecución para diferentes propositos.
- Es común que muchos servicios del sistema tengan su propio usuario para restringir el acceso como mecanismo de seguridad. 
- La configuración de los usuarios se maneja en los ficheros /etc/passwd y /etc/shadow

### /etc/passwd

- Contiene la información de las cuentas de usuario y características.
> El formato es name:password:UID:GID:GECOS:directory:shell

Un ejemplo de usuario es:
> avahi:x:115:120:Avahi mDNS daemon,,,:/var/run/avahi-daemon:/bin/false

### /etc/shadow

- Contiene la información de las contraseñas de forma cifrada.
> El formato es name:password:lastmod:min:max:aviso:inactividad:expiración:reservado

- Sobre el campo de password, puede haber un * (nunca ha tenido un password), o ! (ha sido deshabilitada).
- Cuando se bloquea un usuario con `usermod -l user`, se añade un ! al comienzo del hash del password para indicar que se ha bloqueado.

### Gestión de usuarios y contraseñas

- Los comandos básicos para la gestión de usuarios son:
    - Crear un usuario: `useradd`
    - Borrar un usuario: `userdel`
    - Modificar un usuario: `usermod` (Tiene una opción para todos los campos de /etc/shadow excepto GECOS. Incluye la opción de lock/unlock)
    - Modificar GECOS: `chfn`
    - Modificar shell: `chsh`
    - Modificar fechas de contraseña de /etc/shadow: `chage`
    - Para cambiar la contraseña: `passwd user`__
    - Para visualizar la información de GECOS: `finger user`.

### Gestión de grupos

Los grupos permiten conceder permisos a un conjunto de usuarios simultáneamente.
Existen los __grupos primarios__ (que consta como GID en /etc/passwd, pudiendo ser solo uno) y los __grupos secundarios__ (gestionados en el fichero /etc/groups, donde pueden haber varios usuarios).

### /etc/group

- Consta del siguiente formato: _name:passwd:GID:miembros_
- De forma similar a /etc/shadow, existe una versión para grupos, __/etc/gshadow__
- Los comandos básicos para la gestión del grupo son:
    - Crear un grupo: `groupadd`
    - Borrar un grupo: `groupdel`
    - Modificar un campo de /etc/groups: `groupmod`
    - Modificar el password del grupo: `gpasswd`
- Para cambiar los grupos de un usuario, se suelen usar las siguientes opciones del `usermod`:
    - Modificar grupo primario: `usermod -g GROUP user`
    - Modificar grupos secundarios: `usermod -G GROUP user`
    - Añadir a un grupo secundario SIN SOBRESCRIBIR: `usermod -a -G GROUP user`

</div>