# The console

## Intro

La consola, es una herramienta de linea de comandos de KumbiaPHP, que permite realizar tareas automatizadas en el ámbito de tu aplicación. In this sense KumbiaPHP includes the following consoles: Cache, Model and Controller.

Each console is composed of a set of commands, each command can receive sequential arguments and named arguments. To indicate a named argument is must precede the prefix "\--" to the argument.

## How to use the console

Para utilizar la consola debes ejecutar el despachador de comandos de consola de KumbiaPHP en un terminal, ubicarte en el directorio " app" de tu aplicación y ejecutar la instrucción acorde al siguiente formato:

`php .../../core/console/kumbia.php [console] [command] [arg] [\--arg_name] = value`

* * *

If the command to execute is not specified, then the "main" console command is executed.

It is also possible to specify the path to the app directory of the application by the argument explicitly named "path".

Examples:

`php .../../core/console/kumbia.php cache clean --driver=sqlite`

`php kumbia.php cache clean --driver=sqlite --path="/ var/www/app"`

## KumbiaPHP consoles

### Cache

This console allows you to perform control over the cache of application.

#### clean \[group\] \[--driver\]

### Allows you to clean the cache.

### Sequential arguments:

- group: group name of cache items that will be deleted, if value is not specified, then clean all cache.

### Named arguments:

- driver: driver cache corresponding to the cache cleaning (nixfile, file, sqlite, APC), if not specified, then the cache manager taken default.

### Example:

### php .../../core/console/kumbia.php cache clean

* * *

#### remove \[id\] \[group\]

Removes an element from the cache.

Sequential arguments:

- id: id element in the cache.
- group: name of group to which belongs the element, specifying no value, then use the 'default' group.

Named arguments:

- driver: driver cache corresponding to the cache cleaning (nixfile, file, sqlite, APC).

Example:

`php ../../core/console/kumbia.php cache remove viewclient my_views`

* * *

### Model

It allows you to manipulate the application models.

#### create [model]

Create a model using as a base the template located at "core/console/generators/model.php".

Sequential arguments:

- model: model name in smallcase.

Example:

`php ../../core/console/kumbia.php model create simple_auth`

* * *

#### delete [model]

Delete a model.

Sequential arguments:

- model: model name in smallcase.

Example:

`php ../../core/console/kumbia.php model delete simple_auth`

* * *

### Controller

It allows you to manipulate the application controllers.

#### create [controller]

Create a controller using as a basis the located in the 'core/console/generators/controller.php' template.

Sequential arguments:

- controller: controller name in smallcase.

Example:

`php ../../core/console/kumbia.php controller create product_sales`

* * *

#### delete [controller]

Delete controller.

Sequential arguments:

- controller: controller name in smallcase.

Example:

`php ../../core/console/kumbia.php controller delete product_sales`

* * *

# #

## Developing your consoles

To develop your consoles you should consider the following:

- Las consolas que desarrolles para tu aplicación deben estar ubicadas en el directorio "app/extensions/console".
- The file should have the suffix "_console" as well as the class the "Console" suffix.
- Cada comando de la consola equivale a un método de la clase.
- Los argumentos con nombre que son enviados al invocar un comando se reciben en el primer argumento del método correspondiente al comando.
- Sequential arguments, which are sent to invoke a command, are received as arguments to the invoked method subsequent to the first argument.
- If he it is not specified the command to run, run by default the "main" method of the class.
- Classes Load, Config and Util are loaded automatically to the console.
- The constants APP_PATH, CORE_PATH and PRODUCTION; they are defined for the console environment. 

Example:

Consider a part of the code, the cache console, whose functionality was explained in the previous section.

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
    
    

### Console::input

This method of the Console class, allows you to read an input from the terminal, characterized by trying to read the entry until it is valid.

`Console::input($message, $values = null)`

$message (string): message to be displayed when this method requests an entry.

$values (array): set of valid values for entry.

Example:

`$answer = Console:input ('Continue?', array ('s ', 'n'));`

* * *