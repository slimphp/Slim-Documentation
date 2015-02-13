# Value Objects

Each Slim application's Request and Response objects are [_immutable value objects_](http://en.wikipedia.org/wiki/Value_object). They can be "changed" only by requesting a cloned version that has updated property values.

Each Slim application starts with an initial Request and Response object pair. These objects are passed into each application middleware layer, and each middleware layer can use each object's `with*()` methods to request a copy of the object with updated properties. You could, for example, invoke the Response object's `withHeader('Content-Type', 'application/json')` method to return a new Response instance that has the updated content type header.

Each middleware is responsible for sending a Request and Response object into the next interior middleware, and each middleware should return the potentially new Response object returned by that same interior middleware. As the Request and Response objects descend into the concentric middleware layers, they will arrive at the Slim application itself. Each application route has a callable, and its callable will receive the most current Request and Response objects as arguments. The route callable can do what is necessary with the Request and Response arguments, but it ultimately must return an appropriate Response object; the returned Response object outwardly traverses the surrounding middleware layers until the final Response object manifestation is returned to the HTTP client.

The point is this. Each middleware layer and route callable receives the latest incarnations of the Request and Respone objects, and they can return an updated version of the Response object until the the ultimate Response object is serialized into an HTTP response that is delivered to the HTTP client.
