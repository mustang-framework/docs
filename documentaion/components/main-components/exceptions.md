# Exceptions

Exceptions are used to handle errors and exceptions in the application.

To generate new exceptions you may use the `mustang:generate:exception` interactive command:

```
php artisan mustang:generate:exception
```

### Definition & Principles[​](https://apiato.io/docs/components/main-components/exceptions#definition--principles) <a href="#definition--principles" id="definition--principles"></a>

Read [**Porto SAP Documentation (#Exceptions)**](https://github.com/Mahmoudz/Porto#definitions--principles).

### Rules[​](https://apiato.io/docs/components/main-components/exceptions#rules) <a href="#rules" id="rules"></a>

* All container-specific Exceptions MUST be placed in the `app/Containers/{Section}/{Container}/Exceptions` directory.
* All general Exceptions MUST be placed in the `app/Ship/Exceptions` directory.
* All Exceptions MUST extend the `App\Ship\Parents\Exceptions\Exception` class.
  * The parent extension SHOULD be aliased as `ParentException`.
* Every Exception MUST have at least two properties: `code` and `message`.

### Folder Structure[​](https://apiato.io/docs/components/main-components/exceptions#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

```
app
├── Containers
│   └── Section
│       └── Container
│           └── Exceptions
│               ├── SpecificException.php
│               ├── AnotherSpecificException.php
│               └── ...
└── Ship
    └── Exceptions
        ├── GeneralException.php
        ├── AnotherGeneralException.php
        └── ...
```

### Code Example[​](https://apiato.io/docs/components/main-components/exceptions#code-example) <a href="#code-example" id="code-example"></a>

You can override those values while throwing the error.

```
use App\Ship\Parents\Exceptions\Exception as ParentException;

class DemoException extends ParentException
{
    protected $code = Response::HTTP_CONFLICT;
    protected $message = 'This is a demo exception.';
}
```

### Helpers Methods[​](https://apiato.io/docs/components/main-components/exceptions#helpers-methods) <a href="#helpers-methods" id="helpers-methods"></a>

#### withErrors[​](https://apiato.io/docs/components/main-components/exceptions#witherrors) <a href="#witherrors" id="witherrors"></a>

```
// Example 1
throw (new AccountFailedException())->withErrors(['email' => 'The email has already been taken.']);
// Example 2
throw (new AccountFailedException())->withErrors(['email' => ['The email has already been taken.', 'Another message']]);
```

You can also use translation strings. Translation strings are automatically translated if the translations are found. To handle localization, you can use the [Localization Container](https://apiato.io/docs/pacakges/localization).

```
// Example 1
throw (new AccountFailedException())->withErrors(['email' => 'appSection@user::exceptions.email-taken']);
// Example 2
throw (new AccountFailedException())->withErrors(['email' => 'appSection@user::exceptions.email-taken', 'Another not translated message']);
```

Response:

```
{
  "message": "The exception error message.",
  "errors": {
    "email": [
      "The email has already been taken.",
      "Another not translated message"
    ]
  }
}
```

#### debug[​](https://apiato.io/docs/components/main-components/exceptions#debug) <a href="#debug" id="debug"></a>

The `debug` method is used for logging error messages during debugging and development. The `debug` method accepts `string` or `\Exception` instance

```
throw (new AccountFailedException())->debug($e);
```
