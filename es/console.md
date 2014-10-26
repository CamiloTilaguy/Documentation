
#   La Consola

##  Introduccion

La consola, es una herramienta de linea de comandos de KumbiaPHP que permite
realizar tareas automatizadas en el ambito de tu aplicacion. En este sentido
KumbiaPHP incluye las siguientes consolas: Cache, Model y Controller.

Cada consola esta compuesta por un conjunto de comandos, cada comando puede
recibir argumentos secuenciales  y argumentos con nombre . Para indicar un
argumento con nombre se debe anteceder el prefijo " \--"  al argumento.

##  Como utilizar la Consola

Para utilizar la consola debes ejecutar el despachador de comandos de consola
de KumbiaPHP en un terminal, ubicarte en el directorio " app"  de tu
aplicacion y ejecutar la instruccion acorde al siguiente formato:

php ../../core/console/kumbia.php [ consola ] [ comando ] [ arg ] [ \--arg_nom
] =valor  
  
---  
  
Si no se especifica el comando ha ejecutar, entonces se ejecutara el comando "
main " de la consola.

Tambien es posible indicar la ruta al directorio app  de la aplicacion
explicitamente por medio del argumento con nombre " path ".

Ejemplos:

php ../../core/console/kumbia.php cache clean --driver=sqlite

php kumbia.php cache clean --driver=sqlite --path="/var/www/app"

##  Consolas de KumbiaPHP

###  Cache

Esta consola permite realizar tareas de control sobre la cache de aplicacion.

####  clean [group] [--driver]

###  Permite limpiar la cache.

###  Argumentos secuenciales:

  * group:  nombre de grupo de elementos de cache que se eliminara, si no se especifica valor, entonces se limpiara toda la cache.

###  Argumentos con nombre:

  * driver: manejador de cache correspondiente a la cache a limpiar (nixfile, file, sqlite, APC), si no se especifica, entonces se toma el manejador de cache predeterminado.

###  Ejemplo:

###  php ../../core/console/kumbia.php cache clean  
  
---  
  
####  remove [id] [group]

Elimina un elemento de la cache.

Argumentos secuenciales:

  * i d:  id de elemento en cache.
  * group: nombre de grupo al que pertenece el elemento, si no se especifica valor, entonces se utilizara el grupo 'default'.

Argumentos con nombre:

  * driver: manejador de cache correspondiente a la cache a limpiar (nixfile, file, sqlite, APC).

Ejemplo:

php ../../core/console/kumbia.php cache remove vista1 mis_vistas  
  
---  
  
###  Model

Permite manipular modelos de la aplicacion.

####  create [model]

Crea un modelo utilizando como base la plantilla ubicada en
"core/console/generators/model.php".

Argumentos secuenciales:

  *   model:  nombre de modelo en smallcase.

Ejemplo:

php ../../core/console/kumbia.php model create venta_vehiculo  
  
---  
  
####  delete [model]

Elimina un modelo.

Argumentos secuenciales:

  *   model:  nombre de modelo en smallcase.

Ejemplo:

php ../../core/console/kumbia.php model delete venta_vehiculo  
  
---  
  
###  Controller

Permite manipular controladores de la aplicacion.

####  create [controller]

Crea un controlador utilizando como base la plantilla ubicada en
'core/console/generators/controller.php'.

Argumentos secuenciales:

  * controller:  nombre de controlador en smallcase.

Ejemplo:

php ../../core/console/kumbia.php controller create venta_vehiculo  
  
---  
  
####  delete [controller]

Elimina un controlador.

Argumentos secuenciales:

  * controller:  nombre de controlador en smallcase.

Ejemplo:

php ../../core/console/kumbia.php controller delete venta_vehiculo  
  
---  
  
##

##  Desarrollando tus Consolas

Para desarrollar tus consolas debes de considerar lo siguiente:

  * Las consolas que desarrolles para tu aplicacion deben estar ubicadas en el directorio "app/extensions/console".
  * El archivo debe tener el sufijo "_console" y de igual manera la clase el sufijo "Console".
  * Cada comando de la consola equivale a un metodo de la clase.
  * Los argumentos con nombre que son enviados al invocar un comando se reciben en el primer argumento del metodo correspondiente al comando.
  * Los argumentos secuenciales que son enviados al invocar un comando se reciben como argumentos del metodo invocado posteriores al primer argumento.
  * Si no se especifica el comando a ejecutar, se ejecutara de manera predeterminada el metodo "main" de la clase.
  * Las clases  Load ,  Config y Util;  son cargadas automaticamente para la consola.
  * Las constantes APP_PATH, CORE_PATH y  PRODUCTION; se encuentran definidas para el entorno de la consola.  

Ejemplo:

Consideremos una parte del codigo de la consola cache cuya funcionalidad fue
explicada en la seccion anterior.

<?php

Load::lib('cache');

class CacheConsole

{

    public function clean($params, $group = FALSE)

    {

        // obtiene el driver de cache

        if (isset($params['driver'])) {

            $cache = Cache::driver($params['driver']);

        } else {

            $cache = Cache::driver()    

        }

        // limpia la cache

        if ($cache->clean($group)) {

            if ($group) {

                echo "-> Se ha limpiado el grupo $group", PHP_EOL;

                } else {

                echo "-> Se ha limpiado la cache", PHP_EOL;

                }

        } else {

            throw new KumbiaException('No se ha logrado eliminar el contenido');

        }

    }

}

?>  
  
---  
  
###  Console::input

Este metodo de la clase Console  permite leer una entrada desde el terminal,
se caracteriza por intentar leer la entrada hasta que esta sea valida.

Console::input($message, $values = null)

$message (string): mensaje a mostrar al momento de solicitar la entrada.

$values (array): conjunto de valores validos para la entrada.

Ejemplo:

$valor = Console::input('¿Desea continuar?', array('s', 'n'));  
  
---  