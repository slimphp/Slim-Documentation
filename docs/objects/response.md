# The Response Object

The Response object encapsulates the HTTP response returned by the Slim application. You use the Response object to set the HTTP response status, headers, and body that are ultimately returned to the HTTP client.

The Response object is a _value object_, and it is immutable. You can never change a given Response object, but you can create a cloned Response object with new property values using any of the Response object's `with*()` methods.

Whereever you are within a Slim application (e.g. a middleware layer, a route callable, or a Not Found handler), you will be given the latest Request and Response objects.

## Response Status

The Response object has a numeric HTTP status code. The default status code is `200`. You can fetch the status code with the Response object's `getStatusCode()` method.

    <?php
    $status = $response->getStatusCode();

If you need to change a Response object's status code, you must request a new Response object that has the new status code with the Response object's `withStatus($code)` method.

    <?php
    $newResponse = $oldResponse->withStatus(404);

## Response Headers

The Response object manages a collection of headers that will be returned to the HTTP client. Each Response object provides the following methods to curate its collection of HTTP headers. Remember, the Response object is immutable, and you must use the appropriate `with*()` methods to fetch a _new_ Response object with modified headers.

### Get All Headers

You can fetch an associative array of HTTP response headers with the Response object's `getHeaders()` method. This returns an associative array whose keys are header names. The array's values are single dimensional arrays that contain one or more string values associated with each header name. This is an example data structure potentially returned by the Response object's `getHeaders()` method.

    [
        'Allow' => [
            'GET',
            'HEAD',
            'DELETE'
        ]
    ]

This example demonstrates how to fetch and iterate the Response object's headers.

    <?php
    // Iterate response headers
    foreach ($response->getHeaders() as $name => $values) {
        echo $name, PHP_EOL;
        foreach ($values as $value) {
            echo $value, PHP_EOL;
        }
    }

### Detect Header

You can detect the presence of an HTTP header with the Response object's `hasHeader($name)` method. This method returns `true` or `false`.

    <?php
    if ($response->hasHeader('Allow') === true) {
        // Do something
    }

### Get Header

You can fetch a single HTTP response header with the Response object's `getHeader($name)` method.

    <?php
    $headerValue = $response->getHeader('Allow');

This returns a string value. The returned string is a comma-concatenated string containing all values associated with the header name. For example, `$response->getHeader('Allow')` may return this string value:

    "GET,HEAD,DELETE"

Use the Response object's `getHeaderLines($name)` method to return the single-dimensional array associated with a given header name. 

    <?php
    $headerValue = $response->getHeaderLines('Allow');

This code may return this single-dimensional array:

    [
        'GET',
        'HEAD',
        'DELETE'
    ]

### Set Header

You can set a new header value with the Response object's `withHeader($name, $value)` method. Remember, the Response object is immutable. This method returns a new _copy_ of the Response object that has the new header value. **This method is destructive**, and it _replaces_ any existing header values that are associated with the same header name.

    <?php
    $newResponse = $oldResponse->withHeader(
        'Content-type',
        'application/json'
    );

### Add Header

You can add a new header value with the Response object's `withAddedHeader($name, $value)` method. Remember, the Response object is immutable. This method returns a new _copy_ of the Response object that has the added header value. **This method is non-destructive**, and it _appends_ the new header value to any existing header values that are `associated with the same header name.

    <?php
    $newResponse = $oldResponse->withAddedHeader(
        'Content-type',
        'application/json'
    );

### Remove Header

You can remove a header with the Response object's `withoutHeader($name)` method. Remember, the Response object is immutable. This method returns a new _copy_ of the Response object that does not have the specified header.

    <?php
    $newResponse = $oldResponse->withoutHeader('Allow');

## Response Cookies

The Response object manages a collection of cookies to be returned with the HTTP response.

### Fetch All Cookies

You can fetch an associative array of HTTP response cookies with the Response object's `getCookies()` method.

    <?php
    $cookies = $app['response']->getCookies();

### Detect Cookie

You can detect the presence of a HTTP response cookie with the Response object's `hasCookie($key)` method.

    <?php
    if ($app['response']->hasCookie('authenticated') === 'yes') {
        // Do something
    }

### Set Multiple Cookies

You can set multiple HTTP response cookies with the Response object's `setCookies()` method. This method accepts an associative array of cookie names and values. These cookies _replace_ existing HTTP response cookies with the same names.

    <?php
    $app['response']->setCookies([
        'authenticated' => 'yes',
        'authenticatedHash' => 'wdfskldhuskdjh'
    ]);

### Set One Cookie

You can set one HTTP response cookie with the Response object's `setCookie()` method.

    <?php
    $app['response']->setCookie('splash', 'seen');

### Remove Cookie

You can remove a cookie from the HTTP response with the Response object's `removeCookie($key)` method.

    <?php
    $app['response']->removeCookie('splash');

## Response Body

The Response object's body is a PHP stream resource. _It is not a string_. By default, the Response object body is a stream readable from and writable to `php://temp`. However, you can substitute the default stream with any valid PHP stream resource. This lets you send a very large HTTP response body that may not otherwise fit into available memory. The Response object provides these methods to inspect and manipulate its body property.

### Append Body

You can append text to the end of the current HTTP response body with the Response object's `write()` method.

    <?php
    $app['response']->write('Content goes here');

### Fetch Body

You can fetch the current HTTP response body with the Response object's `getBody()` method. This returns a reference to the PHP stream resource.

    <?php
    $bodyStream = $app['response']->getBody();

### Set Body

You can replace the current HTTP response body with a new PHP stream resource using the Response object's `setBody()` method.

    <?php
    $newBody = fopen('/path/to/large/file.txt', 'rb');
    $app['response']->setBody($newBody);

<div class="wy-alert wy-alert-info">
This method <em>does not</em> initiate a file download. Instead, this method only specifies a new <em>origin</em> of data that will become the HTTP response body. We'll discuss file downloads momentarily.
</div>

## Response Helpers

Lorem ipsum dolor sit amet, consectetur adipisicing elit. Ea, sed? Sit aut est ipsam, accusamus molestias impedit assumenda veritatis, alias temporibus tempore unde quaerat eligendi odio nulla consequuntur sequi cumque.

### Write JSON

Slim provides first-class support for JSON data. You can write JSON data directly to the response object with the Response object's `writeJson()` method. This method accepts one argument, and the argument can be an array, a JSON string, or an object that implements a `toJson()` or `asJson()` method.
The Response object's `writeJson()` method automatically sets the appropriate `Content-Type` and `Content-Length` response headers.

    <?php
    // Write JSON from array
    $data = [
        'records' => [
            (object)[
                'id' => '1',
                'name' => 'John'
            ],
            (object)[
                'id' => '2',
                'name' => 'Sally'
            ],
            (object)[
                'id' => '3',
                'name' => 'Sue'
            ]
        ]
    ];
    $app['response']->writeJson($data);

    // Write JSON from string
    $data = '{"records": [{ "id": "1", "name": "John" }]}';
    $app['response']->writeJson($data);

### Write XML

Slim provides first-class support for XML data. You can write XML data directly to the response object with the Response object's `writeXml()` method. This method accepts one argument, and the argument can be an array, a XML string, or an object that implements a `toXml()` or `asXml()` method.
The Response object's `writeXml()` method automatically sets the appropriate `Content-Type` and `Content-Length` response headers.

    <?php
    // Write XML from array
    $data = [
        'records' => [
            'john' => (object)[
                'id' => '1',
                'name' => 'John'
            ],
            'sally' => (object)[
                'id' => '2',
                'name' => 'Sally'
            ],
            'sue' => (object)[
                'id' => '3',
                'name' => 'Sue'
            ]
        ]
    ];
    $app['response']->writeXml($data);

    // Write XML from string
    $data = '<records><john id="1" name="john"/></records>';
    $app['response']->writeXml($data);

### Initiate File Download
