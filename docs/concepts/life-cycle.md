# Application Lifecycle

## 1. Instantiation

First, you instantiate the `\Slim\App` class. This is the Slim application object. During instantiation, Slim registers default services for each application dependency. The application constructor accepts an optional settings array that configures the application's behavior.

## 2. Route Definitions

Second, you define routes using the application instance's `get()`, `post()`, `put()`, `delete()`, `patch()`, `head()`, and `options()` proxy methods. These instance methods register a route with the application's Router object. The Router object maintains a separate route queue for each HTTP method. Each queued route object has an HTTP method, an HTTP URI, and a callback.

## 3. Application Runner

Third, you invoke the application instance's `run()` method after you define the application's routes.

### A. Enter Middleware Stack

The `run()` method begins to inwardly traverse the application's middleware stack. This is a concentric structure of middleware layers that receive (and optionally manipulate) the Environment, Request, and Response objects before (and after) the Slim application runs. The Slim application is the inner-most layer of the concentric middleware structure. Each middleware layer is invoked inwardly beginning from the outer-most layer.

### B. Run Application

After the `run()` method reaches the inner-most middleware layer, it invokes the applciation instance itself by dispatching the current HTTP request to the appropriate application route object. To do so, the `run()` method invokes the application instance's `call()` method (this method has the same signature and behavior as middleware). The `call()` method invokes the application instance's `dispatch()` method.

### C. Iterate Matching Routes

The application instance's `dispatch()` method asks the Router object for all Route objects that match the current HTTP request method and URI. The Router object iterates the queue that matches the current HTTP request method. Each Route object in the queue is inspected in sequential order.

The application instance's `dispatch()` method iterates the matching routes returned from the Router object, and it invokes each Route object's `dispatch()` method. The Route object's `dispatch()` method invokes the Route object's callback routine.

Only one matching route is invoked unless the current iteration's Route object callback invokes the application instance's `pass()` method. If the `pass()` method is invoked, the subsequent matching Route object is dispatched until a Route callback successfully runs or no matching Route objects remain.

When a Route callback runs successfully, any `echo()` output emitted by the Route object's callback routine is captured into a temporary output buffer. This output is appended to the current Response object's body property.

If there are no matching Route objects, or if all matching Route object's invoke the `pass()` method, the application instance's `notFound()` method is invoked. This method invokes the registered "Not Found" handler which sets the Response object's status to `404` and appends the handler's captured output to the Response object's body.

### D. Exit Middleware Stack

After the application instance's `call()` method completes, the application instance's `run()` method exits the middleware stack. Remember, the middleware stack is a concentric structure. Each middleware layer reclaims control outwardly beginning from the inner-most layer.

### E. Send HTTP Response

After the outer-most middleware layer cedes control, the application instance prepares, serializes, and returns the HTTP response. This occurs in the application instance's `finalize()` method. This method encrypts and persists cookie and session data if present. Finally, the application instance's `finalize()` method invokes the Response object's `send()` method to serialize the Response object's headers with PHP's native `header()` method and output the Response object's body with the `echo()` method.
