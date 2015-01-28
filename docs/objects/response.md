# The Response Object

The Response object encapsulates the HTTP response returned by the Slim application. Each Slim application has one Response object. You use the Response object to set the HTTP response status, headers, and body. The Response object is registered as a Pimple service on the application instance. You can fetch the Response object like this:

    <?php
    $response = $app['response'];

## Response Status

The Response object has a numeric HTTP status code. The default status code is `200`, and you can change or inspect the status code with the Response object's getter and setter methods.

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

Lorem ipsum dolor sit amet, consectetur adipisicing elit. Ullam tempore esse officia porro ea animi, consectetur ad minima velit vero temporibus autem accusantium quae nulla quam distinctio nesciunt eaque nobis!

## Response Body

The Response object's body is a PHP stream resource. _It is not a string_. By default, the Response object body is a readable and writable stream at `php://temp`. However, you can substitute the default stream with any valid PHP stream resource. This lets you send a very large HTTP response body that may not otherwise fit into available memory. The Response object provides these methods to inspect and manipulate its body property.

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

### Output JSON

### Output XML

### Initiate File Download
