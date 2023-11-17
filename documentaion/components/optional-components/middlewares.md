# Middlewares

Mustang middlewares are just [Laravel Middlewares](https://laravel.com/docs/middleware), and they function in the exact same way as Laravel middlewares. However, they come with additional rules and conventions specific to Mustang.

To generate new middlewares you may use the `mustang:generate:middleware` interactive command:

```
php artisan mustang:generate:middleware
```

### Rules[​](https://apiato.io/docs/components/optional-components/middlewares#rules) <a href="#rules" id="rules"></a>

* All container-specific Middlewares:
  * MUST be placed in the `app/Containers/{Section}/{Container}/Middlewares` directory.
  * MUST be registered in their respective container's `App\Containers\{Section}\{Container}\Providers\MiddlewareServiceProvider` class.
* All general Middlewares:
  * MUST be placed in the `app/Ship/Middlewares` directory.
  * MUST be registered in the `App\Ship\Kernels\HttpKernel` class.
* All non-Laravel or third-party package Middlewares MUST extend the `App\Ship\Parents\Middlewares\Middleware` class.
  * The parent extension SHOULD be aliased as `ParentMiddleware`.

### Folder Structure[​](https://apiato.io/docs/components/optional-components/middlewares#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

The highlighted sections showcase middleware registration points:

```
app
├── Containers
│   └── Section
│       └── Container
│           ├── Middlewares
│           │   ├── DemoMiddleware.php
│           │   └── ...
│           └── Providers
│               ├── MiddlewareServiceProvider.php
│               └── ...
└── Ship
    ├── Kernels
    │   └── HttpKernel.php
    └── Middlewares
        ├── AnotherMiddleware.php
        └── ...
```

### Code Example[​](https://apiato.io/docs/components/optional-components/middlewares#code-example) <a href="#code-example" id="code-example"></a>

Middlewares are defined exactly as you would define them in Laravel.

### Registering Middleware[​](https://apiato.io/docs/components/optional-components/middlewares#registering-middleware) <a href="#registering-middleware" id="registering-middleware"></a>

The registration process for a middleware varies depending on its intended scope within the application. Different places are designated for different levels of middleware usage.

In essence, the decision of where to register a middleware boils down to two key factors: the scope of middleware usage and the logical location for its registration.

#### Container Middlewares[​](https://apiato.io/docs/components/optional-components/middlewares#container-middlewares) <a href="#container-middlewares" id="container-middlewares"></a>

If a middleware usage is specific to a container, it must be registered in the `App\Containers\{Section}\{Container}\Providers\MiddlewareServiceProvider` class.

```
use ...
use App\Ship\Parents\Providers\MiddlewareServiceProvider as ParentMiddlewareServiceProvider;

class MiddlewareServiceProvider extends ParentMiddlewareServiceProvider
{
    protected array $middlewares = [];
    protected array $middlewareGroups = [];
    protected array $middlewarePriority = [];
    protected array $middlewareAliases = [];
}
```

To generate a middleware service provider you may use the `mustang:generate:provider` interactive command:

```
php artisan mustang:generate:provider
```

Remember to also register the `MiddlewareServiceProvider` in the container's `MainServiceProvider`:

```
use ...
use App\Ship\Parents\Providers\MainServiceProvider as ParentMainServiceProvider;

class MainServiceProvider extends ParentMainServiceProvider
{
    protected array $serviceProviders = [
        // ... Other service providers
        MiddlewareServiceProvider::class,
    ];
}
```

#### General Middlewares[​](https://apiato.io/docs/components/optional-components/middlewares#general-middlewares) <a href="#general-middlewares" id="general-middlewares"></a>

General middlewares must be registered in the `App\Ship\Kernels\HttpKernel` class.

#### Third Party Middlewares[​](https://apiato.io/docs/components/optional-components/middlewares#third-party-middlewares) <a href="#third-party-middlewares" id="third-party-middlewares"></a>

When dealing with third-party packages that require middleware registration in the `App\Ship\Kernels\HttpKernel` class, you should follow these guidelines:

* **Specific Container Usage**: If the package is used within a particular container, register its middleware in that container `App\Containers\{Section}\{Container}\Providers\MiddlewareServiceProvider` class.
* **Framework-wide Usage**: If the package is generic and used throughout the entire application, you can register its middleware in the `App\Ship\Kernels\HttpKernel` class.

### Middleware Registration Flow[​](https://apiato.io/docs/components/optional-components/middlewares#middleware-registration-flow) <a href="#middleware-registration-flow" id="middleware-registration-flow"></a>

If you want to understand the middleware registration process, here is a breakdown of the registration flow.

Consider the following folder structure:

```
app
├── Containers
│   └── Section
│       └── Container
│           ├── Middlewares
│           │   ├── DemoMiddleware.php ─►─┐
│           │   └── ...                   │
│           └── Providers                 ▼
│               ├── MiddlewareServiceProvider.php ─────►─────┐
│               ├── MainServiceProvider.php ◄─registered─in─◄┘
│               └── ...
└── Ship
    ├── Kernels
    │   ├── HttpKernel.php ◄─registered─in─◄┐
    │   └── ...                             │
    └── Middlewares                         │
        ├── AnotherMiddleware.php ─────►────┘
        └── ...
```

The following diagram illustrates the registration flow of middlewares in the above folder structure:

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>
