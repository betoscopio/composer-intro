# Introducción a Composer

![Composer logo](images/logo-composer-transparent.png)

*Beto Arancibia*
*@betoscopio*


## Warning!!!
*There might be some nerd talk ahead...*
![Hackerman](images/hackerman.jpg)

## El problema

Hace no tanto tiempo...

```php
class Bootstrap extends Zend_Application_Bootstrap_Bootstrap
{
    protected function _initRequest()
    {
        // Ensure front controller instance is present, and fetch it
        $this->bootstrap('FrontController');
        $front = $this->getResource('FrontController');
 
        // Initialize the request object
        $request = new Zend_Controller_Request_Http();
        $request->setBaseUrl('/foo');
 
        // Add it to the front controller
        $front->setRequest($request);
 
        // Bootstrap will store this value in the 'request' key of its container
        return $request;
    }
}

```

```php
class My_Bootstrap_Resource_Layout
    extends Zend_Application_Resource_ResourceAbstract
{
    public function init()
    {
        // ensure view is initialized...
        $this->getBootstrap()->bootstrap('view');
 
        // Get view object:
        $view = $this->getBootstrap()->getResource('view');
 
        // ...
    }
}
```

```php
class ErrorController extends Zend_Controller_Action
{
 
    public function errorAction()
    {
        $errors = $this->_getParam('error_handler');
 
        switch ($errors->type) {
            case Zend_Controller_Plugin_ErrorHandler::EXCEPTION_NO_ROUTE:
            case Zend_Controller_Plugin_ErrorHandler::EXCEPTION_NO_CONTROLLER:
            case Zend_Controller_Plugin_ErrorHandler::EXCEPTION_NO_ACTION:
 
                // 404 error -- controller or action not found
                $this->getResponse()->setHttpResponseCode(404);
                $this->view->message = 'Page not found';
                break;
            default:
                // application error
                $this->getResponse()->setHttpResponseCode(500);
                $this->view->message = 'Application error';
                break;
        }
 
        $this->view->exception = $errors->exception;
        $this->view->request   = $errors->request;
    }
}
```

### PHP Pear?

### Pero... y el problema?

* [ 16 PHP Frameworks To Consider For Your Next Project ](https://www.sitepoint.com/16-php-frameworks/)
* [The Big List of PHP Frameworks](http://ernieleseberg.com/big-list-of-php-frameworks/)

![Seriously](images/seriously-meme.png)

##El inicio de la Revolución

###PHP 5.3 (2009)

####Namespaces
Organiza el código PHP en una jerarquía virtual. Esto permite que distintos proveedores puedan tener clases del mismo nombre y no cause conflicitos. 
 
 [Ejemplo de uso]( https://github.com/symfony/http-foundation)
 
 
```php
namespace Symfony\Component\HttpFoundation;  //definición del namespace

//Definición de alias de clases para ser usados posteriormente.
use Symfony\Component\HttpFoundation\Exception\ConflictingHeadersException;
use Symfony\Component\HttpFoundation\Session\SessionInterface ;
```
Gracias a los namespaces, ya no existe el problema de colisiones de nombres de clases. Esto permitido que puedan existir herramientas como Composer, lo que ha generado una revolución en la comunidad y los proyectos PHP.

## Adios a los Frameworks (monolíticos)
*...bienvenidos los componentes*

### Estándares y componentes
La comunidad PHP ha evolucionado de un modelo de framework centralizado a un ecosistema distribuido de eficientes, interoperables y especializados componentes.

### PHP-FIG
Para que los frameworks puedan comunicarse en un idioma común, ellos necesitan estándares, para esto se ha creado[ PHP Framework Interop Group](http://www.php-fig.org/) 

### PSR
Es el acrónimo para *PHP standards recommendation*. Son recomendaciones a seguir por el framework
que resuelven distintos problemas, cáda uno es seguido por un número. Estas recomendaciones pueden
ser adoptadas por un *framework* y contruirlo sobre soluciones comunes.

Una lista completa de las recomendaciones aceptadas y en WIP se puede ver en [FIG-PSRs](http://www.php-fig.org/psr/).

Los estándares aceptados hasta el momento son:
* PSR-0: Autoloading (deprecated)
* [PSR-1: Basic code style](http://www.php-fig.org/psr/psr-1/)
* [PSR-2: Strict code style](http://www.php-fig.org/psr/psr-2/)
* [PSR-3: Logger interface](http://www.php-fig.org/psr/psr-3/)
* [PSR-4: Autoloading](http://www.php-fig.org/psr/psr-4/)
Nuevos:
* [PSR-6: Caching interface](http://www.php-fig.org/psr/psr-6/)
* [PSR-7: HTTP Message](http://www.php-fig.org/psr/psr-7/)
* [PSR-13: Hypermedia Links](http://www.php-fig.org/psr/psr-13/)

###Componentes
PHP moderno es más sobre componer soluciones con componenetes espcializados que usar frameworks monoliticos.
Actualmente en lugar de elegir un framework y seguir con esa elección, se puede elegir componentes especializados y agregar más cuando se vayan necesitando.
**Un componente es un conjunto de código que ayuda a resolver un problema especifico en una aplicación.**

###Frameworks vs Componentes
Los componentes entregan más flexibilidad, son prácticos para proyectos pequeños, los frameworks son últiles cuando se necesita extructura para proyectos más grandes y convenciones para equipos de trabajo mayores.

### Encontrando componentes

* [Packagist](https://packagist.org/)
* [Awesome PHP](https://github.com/ziadoz/awesome-php)
* [ The League of Extraordinary Packages](http://thephpleague.com/)

## Composer
Usando componentes PHP.

[Composer](https://getcomposer.org/) es un manejador de dependencias PHP, que se encargará de descargar y hacer autoloads de los componentes (y sus dependencias) en el proyecto. Composer automaticamente generará un autoloader PSR compatible para todos nuestros componentes PHP del proyecto. Abstrae efectivamente el manejo de dependencias y el autoloading.

### Instalación
```sh
$ curl -sS https://getcomposer.org/installer | php
$ sudo mv composer.phar /usr/local/bin/composer 
$ sudo chmod +x /usr/local/bin/composer
$ PATH=/usr/local/bin:$PATH 
```
### Uso
```
$ composer require vendor/package
//Ejemplo:
$ composer require league/flysystem
$ composer require monolog/monolog
```
#### composer.json
Definición de proyecto, define los requisitos de componentes para un proyecto de sofware.
Ejemplo:
```json
{
  "require": {
    "monolog/monolog": "^1.21*"
  }
}
```

#### composer.lock
Archivo de dependecias con versiones actualmente instaladas.
Ejemplo:
```json
{
    "_readme": [
        "This file locks the dependencies of your project to a known state",
        "Read more about it at https://getcomposer.org/doc/01-basic-usage.md#composer-lock-the-lock-file",
        "This file is @generated automatically"
    ],
    "hash": "56c6cf9379784d769e1ba332580ffc2b",
    "content-hash": "c7c031b116f4ce6208484f973071c70e",
    "packages": [
        {
            "name": "monolog/monolog",
            "version": "1.21.0",
            "source": {
                "type": "git",
                "url": "https://github.com/Seldaek/monolog.git",
                "reference": "f42fbdfd53e306bda545845e4dbfd3e72edb4952"
            },
            "dist": {
                "type": "zip",
                "url": "https://api.github.com/repos/Seldaek/monolog/zipball/f42fbdfd53e306bda545845e4dbfd3e72edb4952",
                "reference": "f42fbdfd53e306bda545845e4dbfd3e72edb4952",
                "shasum": ""
            },
            "require": {
                "php": ">=5.3.0",
                "psr/log": "~1.0"
            },
            "provide": {
                "psr/log-implementation": "1.0.0"
            },
            "require-dev": {
                "aws/aws-sdk-php": "^2.4.9",
                "doctrine/couchdb": "~1.0@dev",
                "graylog2/gelf-php": "~1.0",
                "jakub-onderka/php-parallel-lint": "0.9",
                "php-amqplib/php-amqplib": "~2.4",
                "php-console/php-console": "^3.1.3",
                "phpunit/phpunit": "~4.5",
                "phpunit/phpunit-mock-objects": "2.3.0",
                "ruflin/elastica": ">=0.90 <3.0",
                "sentry/sentry": "^0.13",
                "swiftmailer/swiftmailer": "~5.3"
            },
            "suggest": {
                "aws/aws-sdk-php": "Allow sending log messages to AWS services like DynamoDB",
                "doctrine/couchdb": "Allow sending log messages to a CouchDB server",
                "ext-amqp": "Allow sending log messages to an AMQP server (1.0+ required)",
                "ext-mongo": "Allow sending log messages to a MongoDB server",
                "graylog2/gelf-php": "Allow sending log messages to a GrayLog2 server",
                "mongodb/mongodb": "Allow sending log messages to a MongoDB server via PHP Driver",
                "php-amqplib/php-amqplib": "Allow sending log messages to an AMQP server using php-amqplib",
                "php-console/php-console": "Allow sending log messages to Google Chrome",
                "rollbar/rollbar": "Allow sending log messages to Rollbar",
                "ruflin/elastica": "Allow sending log messages to an Elastic Search server",
                "sentry/sentry": "Allow sending log messages to a Sentry server"
            },
            "type": "library",
            "extra": {
                "branch-alias": {
                    "dev-master": "2.0.x-dev"
                }
            },
            "autoload": {
                "psr-4": {
                    "Monolog\\": "src/Monolog"
                }
            },
            "notification-url": "https://packagist.org/downloads/",
            "license": [
                "MIT"
            ],
            "authors": [
                {
                    "name": "Jordi Boggiano",
                    "email": "j.boggiano@seld.be",
                    "homepage": "http://seld.be"
                }
            ],
            "description": "Sends your logs to files, sockets, inboxes, databases and various web services",
            "homepage": "http://github.com/Seldaek/monolog",
            "keywords": [
                "log",
                "logging",
                "psr-3"
            ],
            "time": "2016-07-29 03:23:52"
        },
        {
            "name": "psr/log",
            "version": "1.0.2",
            "source": {
                "type": "git",
                "url": "https://github.com/php-fig/log.git",
                "reference": "4ebe3a8bf773a19edfe0a84b6585ba3d401b724d"
            },
            "dist": {
                "type": "zip",
                "url": "https://api.github.com/repos/php-fig/log/zipball/4ebe3a8bf773a19edfe0a84b6585ba3d401b724d",
                "reference": "4ebe3a8bf773a19edfe0a84b6585ba3d401b724d",
                "shasum": ""
            },
            "require": {
                "php": ">=5.3.0"
            },
            "type": "library",
            "extra": {
                "branch-alias": {
                    "dev-master": "1.0.x-dev"
                }
            },
            "autoload": {
                "psr-4": {
                    "Psr\\Log\\": "Psr/Log/"
                }
            },
            "notification-url": "https://packagist.org/downloads/",
            "license": [
                "MIT"
            ],
            "authors": [
                {
                    "name": "PHP-FIG",
                    "homepage": "http://www.php-fig.org/"
                }
            ],
            "description": "Common interface for logging libraries",
            "homepage": "https://github.com/php-fig/log",
            "keywords": [
                "log",
                "psr",
                "psr-3"
            ],
            "time": "2016-10-10 12:19:37"
        }
    ],
    "packages-dev": [],
    "aliases": [],
    "minimum-stability": "stable",
    "stability-flags": [],
     "prefer-stable": false,
    "prefer-lowest": false,
    "platform": [],
    "platform-dev": []
}
```

#### Autoload (PSR-4)
Para componentes que lo especifican, composer genera un archivo `vendor/autoload.php`.

```php
require __DIR__ . '/vendor/autoload.php';
```

### Código de ejemplo
```php
$log = new Monolog\Logger('name');
$log->pushHandler(new Monolog\Handler\StreamHandler('app.log', Monolog\Logger::WARNING));
$log->addWarning('Foo');
```

## Y Drupal?
```sh
$ git clone --branch 8.3.x https://git.drupal.org/project/drupal.git
$ cd drupal
$ composer install
```
* [Usando Composer con
  Drupal](https://www.drupal.org/docs/develop/using-composer/using-composer-with-drupal)
* [Usando Composer para manejar dependencias](https://www.drupal.org/node/2718229)
* [Usando DO como fuente de paquetes Drupal](https://www.drupal.org/node/2718229#drupal-packagist)


### Exponer Componentes de Drupal

[[Meta] Expose Drupal Components outside of Drupal](https://www.drupal.org/node/1826054)

### Distribuyendo Drupal
[Drupal Composer](http://drupal-composer.org/)

```sh
composer create-project drupal-composer/drupal-project:8.x-dev some-dir --stability dev
--no-interaction
```

### Uso en Distribuciones
[Drupal Lightning](https://www.drupal.org/project/lightning)
```sh
$ composer create-project acquia/lightning-project:^8.1.0 MY_PROJECT --no-interaction
```

### Convenciones Drupal

* [Namespaces](https://www.drupal.org/docs/develop/coding-standards/namespaces)
* [PSR-4 namespaces and autoloading in Drupal
  8](https://www.drupal.org/docs/develop/coding-standards/psr-4-namespaces-and-autoloading-in-drupal-8)
* [Composer package naming
  conventions](https://www.drupal.org/docs/develop/coding-standards/composer-package-naming-conventions)

## Referencias

* https://github.com/codeguy/modern-php/tree/master/04-components
* [PHP: The Right Way](http://www.phptherightway.com/)
* [Composer](https://getcomposer.org/)
