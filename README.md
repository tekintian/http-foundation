# The HttpFoundation Component

从symfony/http-foundation完全精简独立出来的 HttpFoundation Component.  版本 5.4.x-dev

支持PHP7.2 以上版本， php8.0  8.2也都支持



## 使用方法

~~~sh
# 安装
composer require tekintian/http-foundation

# 可选安装 这个仅在需要使用 请求频率限制时才需要安装
composer require symfony/rate-limiter

~~~



~~~php
<?php

require_once('/path/to/vendor/autoload.php'); //Load with ClassLoader

use Tekintian\HttpFoundation\Request;

use Tekintian\HttpFoundation\Response;


$request = Request::createFromGlobals();

$response = new Response();


........

~~~



## response
~~~php
$response = new StreamedResponse($callback, Response::HTTP_OK, array(
    'Content-Type' => 'text/event-stream',
    'Cache-Control' => 'no-cache',
    'X-Accel-Buffering' => 'no' // Disables FastCGI Buffering on Nginx
));

if($this->allow_cors){
    $response->headers->set('Access-Control-Allow-Origin', '*');
    $response->headers->set('Access-Control-Allow-Credentials', 'true');
}

if($this->use_chunked_encoding)
    $response->headers->set('Transfer-encoding', 'chunked');
~~~



## Request

The most common way to create a request is to base it on the current PHP global variables with createFromGlobals():

```
use Tekintian\HttpFoundation\Request;

$request = Request::createFromGlobals();
```

which is almost equivalent to the more verbose, but also more flexible, __construct() call:


```
$request = new Request(
    $_GET,
    $_POST,
    [],
    $_COOKIE,
    $_FILES,
    $_SERVER
);
```

### Accessing Request Data

A Request object holds information about the client request. This information can be accessed via several public properties:

- request: equivalent of $_POST;
- query: equivalent of $_GET ($request->query->get('name'));
- cookies: equivalent of $_COOKIE;
- attributes: no equivalent - used by your app to store other data (see below);
- files: equivalent of $_FILES;
- server: equivalent of $_SERVER;
- headers: mostly equivalent to a subset of $_SERVER ($request->headers->get('User-Agent')).

Each property is a ParameterBag instance (or a subclass of), which is a data holder class:

- request: ParameterBag or InputBag if the data is coming from $_POST parameters;
- query: InputBag;
- cookies: InputBag;
- attributes: ParameterBag;
- files: FileBag;
- server: ServerBag;
- headers: HeaderBag.

All ParameterBag instances have methods to retrieve and update their data:

- all()
  Returns the parameters.
- keys()
  Returns the parameter keys.
- replace()
  Replaces the current parameters by a new set.
- add()
  Adds parameters.
- get()
  Returns a parameter by name.
- set()
  Sets a parameter by name.
- has()
  Returns true if the parameter is defined.
- remove()
  Removes a parameter.

The ParameterBag instance also has some methods to filter the input values:

- getAlpha()
  Returns the alphabetic characters of the parameter value;
  
- getAlnum()
  Returns the alphabetic characters and digits of the parameter value;
  
- getBoolean()
  Returns the parameter value converted to boolean;
  
- getDigits()
  Returns the digits of the parameter value;
  
- getInt()
  Returns the parameter value converted to integer;
  
- getEnum()
  Returns the parameter value converted to a PHP enum;
  
- getString()
  Returns the parameter value as a string;
  
  


更多使用方法请参照官方文档：

https://symfony.com/doc/current/components/http_foundation.html



