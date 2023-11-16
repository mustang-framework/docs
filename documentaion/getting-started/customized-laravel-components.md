---
description: >-
  Apiato provides a refined organization for Laravel default class locations.
  Here, you can find the default Laravel components and their corresponding
  locations within Apiato.
---

# Customized Laravel Components

### Kernels[​](https://apiato.io/docs/getting-started/customized-laravel-components#kernels) <a href="#kernels" id="kernels"></a>

* **Http Kernel** is moved from `app/Http` to `app/Ship/Kernels` and renamed to `HttpKernel`.
* **Console Kernel** is moved from `app/Console` to `app/Ship/Kernels` and renamed to `ConsoleKernel`.

### Middlewares[​](https://apiato.io/docs/getting-started/customized-laravel-components#middlewares) <a href="#middlewares" id="middlewares"></a>

* **Middlewares** are moved from `app/Http/Middleware` to `app/Ship/Middlewares`.

### Handler[​](https://apiato.io/docs/getting-started/customized-laravel-components#handler) <a href="#handler" id="handler"></a>

* Exception **Handler** is moved from `app/Exceptions` to `app/Ship/Exceptions/Handlers` and renamed to `ExceptionsHandler`.

### Providers[​](https://apiato.io/docs/getting-started/customized-laravel-components#providers) <a href="#providers" id="providers"></a>

* For information about the new locations of **Providers**, please refer to [this link](../architecture-concepts/components.md).

### Routes[​](https://apiato.io/docs/getting-started/customized-laravel-components#routes) <a href="#routes" id="routes"></a>

#### Web and API[​](https://apiato.io/docs/getting-started/customized-laravel-components#web-and-api) <a href="#web-and-api" id="web-and-api"></a>

Apiato introduces a new approach to route organization and does not use the default `routes/web.php` and `routes/api.php` files. Therefore, you won't find these files in Apiato. To learn more, please visit [this link](https://apiato.io/docs/components/main-components/routes).

#### Channels[​](https://apiato.io/docs/getting-started/customized-laravel-components#channels) <a href="#channels" id="channels"></a>

* The **channels.php** file has been relocated from `routes` to `app/Ship/Broadcasts`.

#### Console[​](https://apiato.io/docs/getting-started/customized-laravel-components#console) <a href="#console" id="console"></a>

* The **console.php** file has been moved from `routes` to `app/Ship/Commands` and renamed to `closures.php`.
