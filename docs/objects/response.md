# The Response Object

The Response object encapsulates the HTTP response returned by the Slim application. Each Slim application has one Response object. You use the Response object to set the HTTP response status, headers, and body. The Response object is registered as a Pimple service on the application instance. You can fetch the Response object like this:

    <?php
    $response = $app['response'];

## Response Status

The Response object has a numeric HTTP status code. The default status code is `200`, and you can change or inspect the status code with the Response object's `setStatus()` and `getStatus()` methods.

    <?php
    // Set status
    $app['response']->setStatus(302);

    // Get status
    $status = $app['response']->getStatus();

## Response Headers

The Response object manages a collection of headers to be returend with the HTTP response.

### Fetch All Headers

You can fetch an associative array of HTTP response headers with the Response object's `getHeaders()` method.

    <?php
    $headers = $app['response']->getHeaders();

### Fetch One Header

You can fetch a single HTTP response header with the Response object's `getHeader($key)` method.

    <?php
    $headerValue = $app['response']->getHeader('X-Api-Key');

### Detect Header

You can detect the presence of a HTTP response header with the Response object's `hasHeader($key)` method.

    <?php
    if ($app['response']->hasHeader('Vary') === true) {
        // Do something
    }

### Set Multiple Headers

You can set multiple HTTP response headers with the Response object's `setHeaders()` method. This method accepts an associative array of header names and values. These headers _replace_ existing HTTP response headers with the same names.

    <?php
    $app['response']->setHeaders([
        'X-Api-Remaining' => 300,
        'X-Api-Used' => 100
    ]);

### Set One Header

You can set one HTTP response header with the Response object's `setHeader()` method.

    <?php
    $app['response']->setHeader('Content-type', 'application/json');

### Add Multiple Headers

You can add multiple HTTP response headers with the Response object's `addHeaders()` method. This method accepts an associative array of header names and values. This method _appends_ data to existing HTTP response headers with the same names. A Response object header with multiple values serializes into a comma-delimited header value in the HTTP response.

    <?php
    $app['response']->addHeaders([
        'X-Api-Remaining' => 300,
        'X-Api-Used' => 100
    ]);

### Add One Header

You can add one HTTP response header with the Response object's `addHeader()` method. This method _appends_ data to an existing HTTP response header with the same name. A Response object header with multiple values serializes into a comma-delimited header value in the HTTP response.

    <?php
    $app['response']->addHeader('X-Api-Token', '1');

### Remove Header

You can remove a header from the HTTP response with the Response object's `removeHeader($key)` method.

    <?php
    $app['response']->removeHeader('X-Api-Token');

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
