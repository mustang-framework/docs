# Requests

[Requests](https://laravel.com/docs/requests) components are a way to interact with the current HTTP request being handled by your application as well as retrieve the input, cookies, and files that were submitted with the request.

To generate new requests you may use the `mustang:generate:request` interactive command:

```
php artisan mustang:generate:request
```

### Definition & Principles[​](https://apiato.io/docs/components/main-components/requests#definition--principles) <a href="#definition--principles" id="definition--principles"></a>

Read [**Porto SAP Documentation (#Requests)**](https://github.com/Mahmoudz/Porto#definitions--principles).

### Rules[​](https://apiato.io/docs/components/main-components/requests#rules) <a href="#rules" id="rules"></a>

* Validation rules MUST be defined in the `rules` method.
* All API Requests MUST be placed in the `app/Containers/{Section}/{Container}/UI/API/Requests` directory.
* All Web Requests MUST be placed in the `app/Containers/{Section}/{Container}/UI/WEB/Requests` directory.
* All Requests:
  * MUST extend the `App\Ship\Parents\Requests\Request` class.
    * The parent extension SHOULD be aliased as `ParentRequest`.
  * MUST have a public `rules` method, returning an array of validation rules.
  * MUST have a public `authorize` method, returning a boolean value.

### Folder Structure[​](https://apiato.io/docs/components/main-components/requests#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

```
app
└── Containers
    └── Section
        └── Container
            └── UI
                ├── API
                │   └── Requests
                │       ├── CreateUserRequest.php
                │       ├── DeleteUserRequest.php
                │       └── ...
                └── WEB
                    └── Requests
                        ├── Login.php
                        ├── Logout.php
                        └── ...
```

### Code Example[​](https://apiato.io/docs/components/main-components/requests#code-example) <a href="#code-example" id="code-example"></a>

```
use App\Ship\Parents\Requests\Request as ParentRequest;

class DemoRequest extends ParentRequest
{
    protected array $access = [];
    protected array $decode = [];
    protected array $urlParameters = [];

    public function rules(): array
    {
        return [
            'field' => 'min:3|max:50',
        ];
    }

    public function authorize(): bool
    {
        return true;
    }
}
```

### Validation[​](https://apiato.io/docs/components/main-components/requests#validation) <a href="#validation" id="validation"></a>

In Mustang, validation of incoming requests follows the Laravel [Form Request Validation](https://laravel.com/docs/validation#form-request-validation) approach.

Validation rules are defined within the respective Request class. These rules are automatically enforced when a Request is injected into a Controller.

Here's an example of a Request class with validation rules:

```
use App\Ship\Parents\Requests\Request as ParentRequest;

class RegisterUserRequest extends ParentRequest
{
    public function rules(): array
    {
        return [
            'email'    => 'required|email|max:200|unique:users,email',
            'password' => 'required|min:20|max:300',
            'name'     => ['required', 'min:2', 'max:400'],
        ];
    }

}
```

And here's how you would use this Request class within a Controller:

```
public function __invoke(RegisterUserRequest $request)
{
    $user = app(RegisterUserAction::class)->run($request);
    
    return $this->transform($user, UserTransformer::class);
}
```

In this example, the validation rules defined in `RegisterUserRequest` will be automatically applied before the `__invoke` method is executed. If the validation fails, an appropriate error response will be generated.

### Request Properties[​](https://apiato.io/docs/components/main-components/requests#request-properties) <a href="#request-properties" id="request-properties"></a>

Mustang introduces new properties to the Request Class that enhance its functionality.

#### access[​](https://apiato.io/docs/components/main-components/requests#access) <a href="#access" id="access"></a>

The `access` property allows you to define Roles and Permissions that can access a specific endpoint. It's used by the `hasAccess` method to check if a user has the required Roles and Permissions to use that endpoint.

```
class DemoRequest extends ParentRequest
{
    protected array $access = [
        'permissions' => 'delete-users',
        'roles' => 'manager'
    ];

    public function authorize(): bool
    {
        return $this->check([
            'hasAccess',
        ]);
    }
}
```

You can also use the `array notation` or `pipe` to define multiple Roles and Permissions.

```
class DemoRequest extends ParentRequest
{
    protected $access = [
        'permissions' => ['delete-users', 'another-permissions'],
        'roles' => 'manager|admin',
    ];
        
    // ...
}
```

Tip

If there's no need to set any roles or permissions, you can simply set the `permissions` or `roles` property to an empty string `''`, an empty array `[]`, or `null`.

#### decode[​](https://apiato.io/docs/components/main-components/requests#decode) <a href="#decode" id="decode"></a>

The `decode` property is used to handle the decoding of Hashed IDs from the incoming Request.

When you enable the [Hash ID](https://apiato.io/docs/security/hash-id) feature, your application can receive Hashed IDs from users. These Hashed IDs need to be decoded before they can be effectively validated. Mustang facilitates this process by providing a property in its Requests class where you can specify which Hashed IDs need to be decoded. This ensures that the validation procedure seamlessly integrates with Hashed IDs.

```
class DemoRequest extends ParentRequest
{
    protected array $decode = [
        'user_id',
        'item_id',
    ];
    
    // ...
}
```

Note

Keep in mind that validation rules relying on your ID, such as `exists:users,id`, will not function correctly unless you decode the ID before passing it to the validation process.

#### urlParameters[​](https://apiato.io/docs/components/main-components/requests#urlparameters) <a href="#urlparameters" id="urlparameters"></a>

The `urlParameters` property simplifies the process of applying validation rules to URL parameters.

By default, Laravel doesn't provide validation for URL parameters (`/stores/999/items`). However, by using the `urlParameters` property, you can enable validation for these parameters. By specifying the desired URL parameters within this property, you not only enable validation but also gain direct access to these parameters from the Request object.

```
// URL: /stores/{id}/items
// GET /stores/999/items
class DemoRequest extends ParentRequest
{
    protected array $urlParameters = [
        'id',
    ];

    public function rules(): array
    {
        return [
            'id'   => 'integer', // url parameter
        ];
    }
}
```

### Helper Methods[​](https://apiato.io/docs/components/main-components/requests#helper-methods) <a href="#helper-methods" id="helper-methods"></a>

#### check[​](https://apiato.io/docs/components/main-components/requests#check) <a href="#check" id="check"></a>

The `check` method is used to authorize the user to access the endpoint. It accepts an array of methods names that will be called to check if the user has access or not. Each of those methods must return a boolean. Take a look at the following example:

```
use App\Ship\Parents\Requests\Request as ParentRequest;

class DemoRequest extends ParentRequest
{
    use IsAuthorTrait;

    // ...

    public function authorize(): bool
    {
        return $this->check([
            'hasAccess|isOwner',
            'isKing',
        ]);
    }
}
```

Here we are passing the the `hasAccess`, `isOwner` and `isKing` methods to the `check` method. Then the `check` method follows the following rules and checks if the user has access or not:

* The separator `|` between the methods indicates an `OR` operation.
* The default operation between all methods in the array is `AND`.

So in the above example, the call to the `check` method will be translated to:

```
return ($this->hasAccess() || $this->isOwner()) && $this->isKing();
```

And if the result of this operation is `true` then the user will be authorized to access the endpoint.

Note

* `hasAccess` method is a [built-in authorization method](https://apiato.io/docs/components/main-components/requests#hasaccess).
* `isOwner` and `isKing` methods are [custom authorization methods](https://apiato.io/docs/components/main-components/requests#custom-authorize-methods)

#### hasAccess[​](https://apiato.io/docs/components/main-components/requests#hasaccess) <a href="#hasaccess" id="hasaccess"></a>

The `hasAccess` method assesses a user's access rights based on the Request's `access` property. If the user has any of the specified Roles or Permissions, the method will return `true` otherwise it will return `false`.

#### sanitizeInput[​](https://apiato.io/docs/components/main-components/requests#sanitizeinput) <a href="#sanitizeinput" id="sanitizeinput"></a>

The `sanitizeInput` method is employed to cleanse request data before its utilization within the application.

Particularly useful for `PATCH` requests, where you may want to submit only the fields intended for modification to minimize traffic or perform partial updates to the corresponding database resource. Traditional checks for the presence or absence of specific keys in the request can lead to extensive `if` blocks, such as:

```
if ($request->has('data.name')) {
   $data['name'] = $request->input('data.name');
}
```

To circumvent these `if` blocks, you might utilize `array_filter($data)` to remove empty fields from the request. However, be aware that in PHP, both `false` and an empty string `''` are considered as `empty`.

For streamlining data sanitization when using `application/json` instead of `x-www-form-urlencoded`, Mustang provides the convenient `sanitizeInput` method.

Consider the following request:

```
{
  "data": {
    "name": "Demo",
    "description": "Some description",
    "is_private": false,
    "address": "",
    "foo": {
      "number": 1,
      "bar": "bar"
    }
  },
  "meta": "some meta data"
}
```

The `sanitizeInput` method enables you to specify a list of fields, employing dot notation, to be accessed and extracted from the request.

```
$data = $request->sanitizeInput([
    'data.description',
    'data.is_private',
    'data.address',
    'data.foo.number',
    'email', // will be ignored
    'meta',
]);
```

The extracted data will appear as follows:

```
[
  "data" => [
    "description" => "Some description"
    "is_private" => false,
    "address" => null, // empty string is converted to null by Laravel
    "foo" => [
      "number" => 1,
    ]
  ],
  "meta" => "some meta data",
]
```

Note that `email` is excluded from the sanitized array, as it was absent in the request. Additionally, any other fields not specified are omitted. In essence, the method filters the request, retaining only the defined values.

You can also assign default values during the data sanitization process:

```
$sanitizedData = $request->sanitizeInput([
    'name' => 'John', // If name is not provided, the default value will be set
    'product.company.address' => 'Somewhere in the world', // dot notation is supported
    'email',
    'password'
]);
```

#### getInputByKey[​](https://apiato.io/docs/components/main-components/requests#getinputbykey) <a href="#getinputbykey" id="getinputbykey"></a>

The `getInputByKey` method retrieves data from the `request` by specifying the field name. Similar to `$request->input('key.here')`, this method operates on the `decoded` values instead of the original data.

Consider the following request:

```
{
  "id": "XbPW7awNkzl83LD6"
}
```

While `$request->input('id')` would return `"XbPW7awNkzl83LD6"`, `$request->getInputByKey('id')` would return the decoded value (e.g., `4`).

Moreover, you can set a `default` value to be returned if the key is absent or unset, like this:

```
$request->getInputByKey('data.name', 'Undefined')
```

#### mapInput[​](https://apiato.io/docs/components/main-components/requests#mapinput) <a href="#mapinput" id="mapinput"></a>

In certain cases, you might need to remap input from the request to different fields. While manual field mapping is possible, you can also leverage the `mapInput` method for this purpose. This helper method allows you to "redefine" keys within the request, making subsequent processing easier.

Consider the following request:

```
{
  "data": {
    "name": "John Doe"
  }
}
```

However, for processing purposes, you require the `username` field instead of `data.name`.

You can use the helper as follows:

```
$request->mapInput([
    'data.name' => 'username',
]);
```

The resulting structure would be:

```
{
  "username": "John Doe"
}
```

And you can access the value as follows:

```
$request->input('username');
```

#### injectData[​](https://apiato.io/docs/components/main-components/requests#injectdata) <a href="#injectdata" id="injectdata"></a>

The `injectData` method allows you to inject data into the request. This can be particularly helpful during testing when you wish to provide data directly to the request instead of sending it through the request body.

```
$request = RegisterUserRequest::injectData($data);
```

#### withUrlParameters[​](https://apiato.io/docs/components/main-components/requests#withurlparameters) <a href="#withurlparameters" id="withurlparameters"></a>

The `withUrlParameters` method enables you to inject URL parameters into the request. This is especially useful when you need to include properties in the request that are not part of the request body but are required for the request to be processed. This method is often used in conjunction with the `injectData` method.

```
$request = RegisterUserRequest::injectData($data)
    ->withUrlParameters(['id' => 123]);
```

### Custom Authorize Methods[​](https://apiato.io/docs/components/main-components/requests#custom-authorize-methods) <a href="#custom-authorize-methods" id="custom-authorize-methods"></a>

The recommended approach for adding custom authorization functions is by using a Trait, which can be included in your Request classes.

For instance, let's create an `IsAuthorTrait` Trait with a single method named `isAuthor` to determine if the current user holds the role of an author.

```
trait IsAuthorTrait
{
    public function isAuthor(): bool
    {
        return $this->user()->hasRole('author');
    }
}
```

Subsequently, you can apply the `IsAuthorTrait` Trait to a Request class, allowing the utilization of the `isAuthor` function within the authorization process.

```
use App\Ship\Parents\Requests\Request as ParentRequest;

class DemoRequest extends ParentRequest
{
    use IsAuthorTrait;

    // ...

    public function authorize(): bool
    {
        return $this->check([
            'isAuthor',
        ]);
    }
}
```

### Bypass Authorization[​](https://apiato.io/docs/components/main-components/requests#bypass-authorization) <a href="#bypass-authorization" id="bypass-authorization"></a>

To grant certain Roles access to all endpoints within the system without the need to define the role in each Request object, you can follow this approach. This is particularly beneficial when you want to provide unrestricted access to users with the `admin` role. To implement this, define the relevant roles in `app/Ship/Configs/mustang.php` as shown below:

```
'requests' => [
    'allow-roles-to-access-all-routes' => ['admin'],
],
```

### Force Accept Header[​](https://apiato.io/docs/components/main-components/requests#force-accept-header) <a href="#force-accept-header" id="force-accept-header"></a>

Typically, when making calls to a JSON API, you should include the `accept: application/json` HTTP header. In Mustang, you have the option to either enforce users to send this header or allow them to skip it.

To enforce the `accept: application/json` header, navigate to the `app/Ship/Configs/mustang.php` configuration file and set the `force-accept-header` to `true`.

Conversely, if you wish to allow users to skip this header, set `force-accept-header` to `false`.

Info

Forcing the accept header is disabled by default.

### Etag[​](https://apiato.io/docs/components/main-components/requests#etag) <a href="#etag" id="etag"></a>

The **ETag** or **entity tag** is part of HTTP, the protocol for the World Wide Web. It is one of several mechanisms that HTTP provides for Web cache validation, which allows a client to make conditional requests. This mechanism allows caches to be more efficient and saves bandwidth, as a Web server does not need to send a full response if the content has not changed. ([Wikipedia](https://en.wikipedia.org/wiki/HTTP\_ETag))

Mustang offers support for Etag through the `Mustang\Core\Middlewares\HttpProcessETagHeadersMiddleware` middleware, which employs the Shallow technique. This middleware can be particularly valuable in reducing bandwidth usage for clients, especially on mobile devices.

Please note that this feature is **disabled by default**. To enable it, follow these steps:

1. Navigate to the `app/Ship/Configs/mustang.php` configuration file.
2. Inside the configuration file, locate the `use-etag` configuration parameter.
3. Set the `use-etag` parameter to `true`.

Keep in mind that for this feature to function correctly, the client must include the `If-None-Match` HTTP header, which corresponds to the ETag value, in their request.

\
