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

