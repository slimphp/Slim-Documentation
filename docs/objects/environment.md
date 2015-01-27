# The Environment Object

The Environment object encapsulates the current global environment, including the HTTP request method, URI, and headers. It also includes several properties from the `$_SERVER` superglobal array. The Environment object effectively decouples a Slim application from the global environment. Decoupling the Slim application from the PHP global environment lets us create HTTP requests that may (or may not) resemble the global environment. This is particuarly useful for unit testing. The Environment object is registered as a Pimple service on the application instance. You can fetch the Environment object like this:

    <?php
    $environment = $app['environment'];

You rarely access the Environment object directly. Instead, the application's Environment object exists only to decouple the global environment and inform the Request object. It otherwise lives quietly in the background.

## Environment Properties

Each Slim application has an Environment object with various properties that determine application behavior. Some properties are required. Other properties are optional.

### Required Properties

REQUEST_METHOD
:   The HTTP request method. This must be one of "GET", "POST", "PUT", "DELETE", "HEAD", "PATCH", or "OPTIONS".

SCRIPT_NAME
:   The first part of the HTTP request's URI path that corresponds to the physical directory in which the Slim application is installed relative to the document root directory. This may be an empty string if the application is installed in the top-level of the document root directory. Non-empty values must begin with a `/` forward slash and must not end with a trailing forward slash.

PATH_INFO
:   The remaining part of the HTTP request's URI path that represents the Slim application route's "virtual" URI. This must begin with a `/` forward slash. A trailing slash is optional.

QUERY_STRING
:   The part of the HTTP request’s URI path after, but not including, the “?”. This may be an empty string if the current HTTP request does not specify a query string.

SERVER_NAME
:   This can create a fully qualified URL to a Slim application resource when combined with the Environment's `SCRIPT_NAME` and `PATH_INFO` values. This must not be an empty string. The `HTTP_HOST` value should be used instead if present. 

SERVER_PORT
:   This can create a fully qualified URL to a Slim application resource when combined with the Environment's `SERVER_NAME`, `SCRIPT_NAME` and `PATH_INFO` values. This must be an integer.

slim.url_scheme
:   The HTTP request scheme. This must be one of “http” or “https”.

slim.input
:   The HTTP request body. This may be an empty string if the current HTTP request does not contain a body (i.e., a GET request).

slim.errors
:   A writable stream resource. This points to `php://stderr` by default.

### Optional Properties

CONTENT_TYPE
:   The HTTP request content type (e.g., `application/json;charset=utf8`)

CONTENT_LENGTH
:   The HTTP request content length. This must be an integer if present.

HTTP_*
:   The HTTP request headers sent by the client. These values are identical to their counterparts in the `$_SERVER` superglobal array. If present, these values must retain the "HTTP_" prefix.

PHP_AUTH_USER
:   The HTTP `Authentication` header's decoded username.

PHP_AUTH_PW
:   The HTTP `Authentication` header's decoded password.

PHP_AUTH_DIGEST
:   The raw HTTP `Authentication` header as sent by the HTTP client.

AUTH_TYPE
:   The HTTP `Authentication` header's authentication type (e.g., "Basic" or "Digest").

## Environment Interface

The Environment object implements the `\Slim\Interfaces\CollectionInterface` interface and provides these methods:

* `set($key, $value)`
* `get($key, $defaultValue)`
* `replace(array $items)`
* `all()`
* `has($key)`
* `remove($key)`
* `clear()`
* `encrypt(CryptInterface $crypt)`
* `decrypt(CryptInterface $crypt)`

## Mock Environments

Each Slim application instantiates an Environment object using information from the current global environment. However, you may also create mock environment objects with custom information. Mock Environment objects are only useful when writing unit tests.

    <?php
    $env = \Slim\Environment::mock([
        'REQUEST_METHOD' => 'PUT',
        'REQUEST_URI' => '/foo/bar',
        'QUERY_STRING' => 'abc=123&foo=bar',
        'SERVER_NAME' => 'example.com',
        'CONTENT_TYPE' => 'application/json;charset=utf8',
        'CONTENT_LENGTH' => 15,
        'slim.input' => '{"test": "123"}'
    ]);
