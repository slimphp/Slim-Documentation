# Container

Each Slim application has _dependencies_. Examples include the Environment, Request, Response, and Router objects described above.

Out of the box, Slim provides a first-party implementation for each dependency. However, it's possible to provide custom implementations for each dependency because the Slim application is an instance of the [Pimple](http://pimple.sensiolabs.org/) dependency injection container. Each application dependency is a Pimple service that lazily instantiates and returns an  appropriate object upon request.

# Custom services

To override a Slim application dependency, inject your own Pimple service before you invoke the Slim application's `run()` method. You may override any of these default application services:

settings
:   This service must return a new instance of `\Slim\Interfaces\ConfigurationInterface`.

environment
:   This service must return a new instance of `\Slim\Interfaces\EnvironmentInterface`.

request
:   This service must return a new instance of `\Psr\Http\Message\RequestInterface`.

response
:   This service must return a new instance of `\Psr\Http\Message\ResponseInterface`.

router
:   This service must return a _shared_ instance of `\Slim\Interfaces\RouterInterface`.

view
:   This service must return a _shared_ instance of `\Slim\Interfaces\ViewInterface`.

errorHandler
:   This service must return a callable to be invoked if there is an application error. The callable **MUST** return an instance of `\Psr\Http\Message\ResponseInterface`. The callable **SHOULD** accept three arguments:

1. `\Psr\Http\RequestInterface`
2. `\Psr\Http\ResponseInterface`
3. `\Exception`

notFoundHandler
:   This service must return a callable to be invoked if the current HTTP request URI does not match an application route. The callable **MUST** return an instance of `\Psr\Http\Message\ResponseInterface`. The callable **SHOULD** accept two arguments:

1. `\Psr\Http\RequestInterface`
2. `\Psr\Http\ResponseInterface`

notAllowedHandler
:   This service must return a callable to be invoked if an application route matches the current HTTP request path but not its method. The callable **MUST** return an instance of `\Psr\Http\Message\ResponseInterface`. The callable **SHOULD** accept three arguments:

1. `\Psr\Http\RequestInterface`
2. `\Psr\Http\ResponseInterface`
3. Array of allowed HTTP methods

You most likely **do not** need to replace these dependencies. However, you can if necessary. Slim's default service implementations live in the Slim application's `__construct()` function.
