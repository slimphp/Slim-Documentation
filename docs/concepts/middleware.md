# What is middleware?

A middleware is a _callable_ that is invoked with three arguments: a Request object, a Response object, and a _next_ middleware. It can do whatever is appropriate with these objects. The only hard requirement is that a middleware MUST return a Response object. That's it.

Each middleware may (or may not) choose to invoke the next middleware argument and pass it Request and Response objects as arguments.

# How does middleware work?

Different frameworks use middleware differently. Slim adds middleware as concentric layers surrounding your core application. Each new middleware layer surrounds any existing middleware layers. The concentric structure expands outwardly as additional middleware layers are added.

When you run the Slim application, the Request and Response objects traverse the middleware structure from the outside in. They first enter the outer-most middleware, then the next outer-most middleware, (and so on), until they ultimately arrive at the Slim application itself. After the Slim application dispatches the appropriate route, the resultant Response object exits the Slim application and traverse the middleware structure from the inside out. Ultimately, a final Response object exits the outer-most middleware, is serialized into a raw HTTP response, and is returned to the HTTP client.

Each middleware _may_ invoke its next interior middlware and pass it the current Request and Response objects as arguments. Each middlware layer MUST return a Response object.

Here's a diagram that hopefully illustrates the middleware process flow:

![Middleware flow](../images/middleware.png 'Middleware')

# How do I write middleware?

Middleware is any _callable_ that receives three arguments: a Request object, a Response object, and a _next_ middleware. The middleware MUST return a Response object. This example middleware is a Closure.

    <?php
    function ($request, $response, $next) {
        $response->write('BEFORE');
        $response = $next($request, $response);
        $response->write('AFTER');

        return $response;
    };

This example middleware is an invokable class that implements the magic `__invoke()` method.

    <?php
    class ExampleMiddleware
    {
        public function __invoke($request, $response, $next)
        {
            $response->write('BEFORE');
            $response = $next($request, $response);
            $response->write('AFTER');

            return $response;
        }
    }

# How do I add middleware?

Add middleware to your Slim application with the `\Slim\App` instance's `add()` method. This example adds the Closure middleware example above:

    <?php
    $app = new \Slim\App();
    $app->add(function ($request, $response, $next) {
        $response->write('BEFORE');
        $response = $next($request, $response);
        $response->write('AFTER');

        return $response;
    });
    $app->get('/', function ($req, $res) {
        echo ' Hello ';
    });
    $app->run();

This would output this HTTP response body:

    BEFORE Hello AFTER


