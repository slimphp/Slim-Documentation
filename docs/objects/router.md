# The Router Object

Slim router under the hood make use of FastRoute.

## GET Route

You can add a route that handles only on `GET` method via Slim
application's `get` method.

```php
<?php
$app = new \Slim\Slim();
$app->get('/books/{id}', function ($request, $response, $args) {
    //Show book identified by $args['id']
});
```

You can get the arguments used in route via `$args`


## POST Route

You can add a route that handles only on `POST` method via Slim
application's `post` method.

```php
<?php
$app = new \Slim\Slim();
$app->post('/books', function ($request, $response, $args) {
    // create new book
});
```

You can get the arguments used in route via `$args`

## PUT Route

You can add a route that handles only on `PUT` method via Slim
application's `put` method.

```php
<?php
$app = new \Slim\Slim();
$app->put('/books/{id}', function ($request, $response, $args) {
    // Update book identified by $args['id']
});
```

You can get the arguments used in route via `$args`

## DELETE Route

You can add a route that handles only on `DELETE` method via Slim
application's `delete` method.

```php
<?php
$app = new \Slim\Slim();
$app->delete('/books/{id}', function ($request, $response, $args) {
    // Delete book identified by $args['id']
});
```

You can get the arguments used in route via `$args`

## OPTIONS Route

You can add a route that handles only on `DELETE` method via Slim
application's `delete` method.

```php
<?php
$app = new \Slim\Slim();
$app->options('/books/{id}', function ($request, $response, $args) {
    // Return response headers
});
```

You can get the arguments used in route via `$args`

## Advanced Usage

You can either make use of Slim application's map method to handle
different methods on the same callback.

```php
<?php
$app = new \Slim\Slim();
$app->map(['GET', 'POST'], '/books', function ($request, $response, $args) {
    // create new book or list all books
});
```

## Restricting place holders

By default the place holders are written inside `{}` and can accept any
values. But you can wrap it like `{id:[0-9]+}` to make sure the value of
`id` is always a digit.
