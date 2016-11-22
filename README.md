#Introducción a Composer

![Composer logo](logo-composer-transparent.png)

*Beto Arancibia*
*@betoscopio*


##Warning!!!
*There might be some nerd talk ahead...*
![Hackerman](hackerman.jpg)

##El problema

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

###PHP Pear?

###Pero... y el problema?

* [ 16 PHP Frameworks To Consider For Your Next Project ](https://www.sitepoint.com/16-php-frameworks/)
* [The Big List of PHP Frameworks](http://ernieleseberg.com/big-list-of-php-frameworks/)

![Seriously](seriously-meme.png)

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

##Adios a los Frameworks (monolíticos)
*...bienvenidos los componentes*

###Estándares y componentes
La comunidad PHP ha evolucionado de un modelo de framework centralizado a un ecosistema distribuido de eficientes, interoperables y especializados componentes.

###PHP-FIG
Para que los frameworks puedan comunicarse en un idioma común, ellos necesitan estándares, para esto se ha creado[ PHP Framework Interop Group](http://www.php-fig.org/) 

###PSR
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

###Encontrando componentes

* [Packagist](https://packagist.org/)
* [Awesome PHP](https://github.com/ziadoz/awesome-php)
* [ The League of Extraordinary Packages](http://thephpleague.com/)

##Composer
Usando componentes PHP.

[Composer](https://getcomposer.org/) es un manejador de dependencias PHP, que se encargará de descargar y hacer autoloads de los componentes (y sus dependencias) en el proyecto. Composer automaticamente generará un autoloader PSR compatible para todos nuestros componentes PHP del proyecto. Abstrae efectivamente el manejo de dependencias y el autoloading.

###Instalación
```sh
$ curl -sS https://getcomposer.org/installer | php
$ sudo mv composer.phar /usr/local/bin/composer 
$ sudo chmod +x /usr/local/bin/composer
$ PATH=/usr/local/bin:$PATH 
```
###Uso
```
$ composer require vendor/package
//Ejemplo:
$ composer require league/flysystem
$ composer require monolog/monolog
```