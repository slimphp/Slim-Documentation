# Fundamental Objects

Each Slim application contains four fundamental objects. If you understand these four objects, you'll have a solid understanding of the framework architecture.

<dl>
    <dt><a href="environment">The Environment object</a></dt>
    <dd>
        This object encapsulates the current PHP global environment, including the HTTP request URI, headers, and method. It also contains server variables found in the `$_SERVER` superglobal array. The Environment object effectively decouples a Slim application from the global PHP environment.
    </dd>
</dl>

<dl>
    <dt><a href="request">The Request object</a></dt>
    <dd>
        This object encapsulates the current HTTP request using the information provided by the Environment object. The Request object provides the HTTP request method, headers, parameters, and body.
    </dd>
</dl>

<dl>
    <dt><a href="response">The Response object</a></dt>
    <dd>
        This object encapsulates an HTTP response to be returned to the HTTP client. It manages the HTTP response status, headers, and body.
    </dd>
</dl>

<dl>
    <dt><a href="router">The Router object</a></dt>
    <dd>
        This object manages application routes. A *route* includes three parts: the HTTP URI, the HTTP method, and the PHP callback. The Router object's routes are iterated when you invoke the Slim application's `run()` method; the Router object finds and invokes the first route that matches the current HTTP request method and URI. The Router can be accessed directly, but it is typically used via proxy methods on the Application instance. 
    </dd>
</dl>

# Dependency Injection Container

Each Slim application has _dependencies_. Examples include the Environment, Request, Response, and Router objects described above. Other dependencies are the application settings object, the cryptography object, and the session object.

Out of the box, Slim provides its own first-party implementation for each dependency. However, it's possible to provide custom implementations for each dependency because the Slim application is an instance of the [Pimple](http://pimple.sensiolabs.org/) dependency injection container. Each Slim application dependency is a Pimple service that lazily instantiates, prepares, and returns an  appropriate object upon request.

To override a Slim application dependency, provide your own custom Pimple service before you invoke the Slim application's `run()` method. You may override any of these default dependency services:

* `settings`
* `environment`
* `request`
* `response`
* `router`
* `view`
* `crypt`
* `session`
* `flash`
* `mode`

You most likely **do not** need to replace these dependencies. However, you can if necessary. Slim's default service implementations live in the Slim application's `__construct()` function.

# Dependency Interfaces

Slim does not require its dependencies to be a specific concrete class. Instead, Slim expects dependencies to implement appropriate _interfaces_.
For example, if you were to override the `request` dependency with a custom Pimple service, the custom service can return any object that implements the `Slim\Interfaces\Http\RequestInterface` interface.

Each application dependency has a matching interface. You can review the available interfaces in the [Slim Framework repository](https://github.com/codeguy/Slim).
