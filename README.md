# rctodosync
Share tasks with multiple users using todo.txt

## Instalación

Instala **rcsync** en tu dispositivo con linux

```
git clone https://github.com/uGeek/rctodosync.git ~/rctodosync
```

```
sudo ln -s /home/$USER/rctodosync/rctodosync /usr/bin/rctodosync
sudo chmod +x /usr/bin/rctodosync
```

### Instala **RCLONE** y **curl**.


#### RCLONE
```
curl https://rclone.org/install.sh | sudo bash
```
#### curl

```
sudo apt install curl
```

## Sincronizar dos archivos todo.txt
Mediante el archivo de configuración, en este ejemplo `trabajo`, situado en la ruta `~/.config/rctodosync/trabajo`, añadamos las siguiente variables:

Especificamos el servidor 1 y 2 siguiendo las opciones que utiliza rclone.

`WORD` es el contexto, proyecto o palabra que utilizaremos para mantener sincronizados las tareas en ambos todo.txt
`TIEMPO_ACTUALIZACION` es el tiempo que esperará entre sincronización y sincronización
En la parte inferior, encontraremos las variables `TOKEN` y `ID`para las notificaciones en Telegram. En ese mismo archivo añadiremos las notificaciones para cualquier otro servicio de notificaciones.

```
SERVIDOR1=dropbox:todo      # Directorio (rclone) donde está situado el archivo todo.txt
SERVIDOR2=drive-trabajo:todo
WORD="@trabajo"             # Palabra en común para la sincronización entre ambos todo.txt
TIEMPO_ACTUALIZACION=5m     # Tiempo que se producirá la sincronización. 5m = 5 minutos.  s = segundos m = minutos h = horas

# Notificaciones Telegram
TOKEN="305386591:AAH5avqawerqerv8973451t4"
ID="5834877"
```

Es necesario otros dos archivos de configuración:
- editor: Añade variable del editor a utilizar para edición del archivo de configuración
- notification: Archivo de con variables para notificaciones en cualquier servicio de notificaciones


## Iniciar rctodosync

Para iniciar la sincronización del ejemplo trabajo, lo haremos del siguiente modo:

```
rctodosync trabajo
```

## Plugins
Es necesario crear una archivo de configuración para añadir mas funcionalidades.
Imagina que estás sincronizando con una máquina remota utilizando wireguard. Podríamos añadir estas líneas para comprobar si wireguard funciona y en caso que no, envie una notificación mediante un bot de matrix y detenga el script. De este modo, evitaremos posibles errores en la sincronización.

Este archivo, siguiendo el ejemplo anterior (trabajo) sería:


```
nano ~/.config/rctodosync/trabajo.pg
```

Copia en el archivo:

```
#!/bin/bash

echo -e "$(date +'%Y-%m-%d  %T')      Leyendo el plugin $1.pg"

if [ "$(sudo wg)" = "" ] ; then
echo "wireguard no funciona. La sincronización se detendrá"
matrix "rcsyntodo se ha detinido porque wg no funciona"
exit
fi

```





## Otras sincronizaciones

Crea tantos archivos de configuración como necesites.

