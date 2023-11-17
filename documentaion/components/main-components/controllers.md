# Controllers

[Controllers](https://laravel.com/docs/controllers) are tasked with two primary responsibilities: serving the requested data and constructing the corresponding response.

To generate new controllers you may use the `mustang:generate:controller` interactive command:

```bash
php artisan mustang:generate:controller
```

### Definition & Principles[​](https://apiato.io/docs/components/main-components/controllers#definition--principles) <a href="#definition--principles" id="definition--principles"></a>

Read [**Porto SAP Documentation (#Controllers)**](https://github.com/Mahmoudz/Porto#definitions--principles).

### Rules[​](https://apiato.io/docs/components/main-components/controllers#rules) <a href="#rules" id="rules"></a>

* All API Controllers:
  * MUST be placed in the `app/Containers/{Section}/{Container}/UI/API/Controllers` directory.
  * MUST extend the `App\Ship\Parents\Controllers\ApiController` class.
  * MUST format their responses via a [Transformer](transformers.md).
* All Web Controllers:
  * MUST be placed in the `app/Containers/{Section}/{Container}/UI/WEB/Controllers` directory.
  * MUST extend the `App\Ship\Parents\Controllers\WebController` class.
* Controllers:
  * MUST only call the `run` or `transactionalRun` method of Actions.
  * SHOULD pass the Request object to the Action instead of passing data from the request.

### Folder Structure[​](https://apiato.io/docs/components/main-components/controllers#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

```
app
└── Containers
    └── Section
        └── Container
            └── UI
                ├── API
                │   └── Controllers
                │       ├── ControllerA.php
                │       ├── ControllerB.php
                │       └── ...
                └── WEB
                    └── Controllers
                        ├── ControllerA.php
                        ├── ControllerB.php
                        └── ...
```

### Code Example[​](https://apiato.io/docs/components/main-components/controllers#code-example) <a href="#code-example" id="code-example"></a>

**API Controller**[**​**](https://apiato.io/docs/components/main-components/controllers#api-controller)

```php
use App\Ship\Parents\Controllers\ApiController;

class Controller extends ApiController
{
    public function __construct(
        private readonly SampleAction $sampleAction,
    ) {
    }
    
    public function __invoke(SampleRequest $request): array
    {
        $sample = $this->sampleAction->run($request);
        
        return $this->transform($sample, SampleTransformer::class);
    }
}
```

**Web Controller**[**​**](https://apiato.io/docs/components/main-components/controllers#web-controller)

```php
use App\Ship\Parents\Controllers\WebController;

class Controller extends WebController
{
    public function show(): Factory|View|Application
    {
        return view('sectionName@containerName::view-name');
    }
}
```

{% hint style="info" %}
In case you want to handle the same Action differently based on the UI type (e.g., API, Web, CLI), you can set the UI on Action with `setUI` method.

```php
$action = app(Action::class);
$action->setUI('web');
```

and get the UI in your Action with `getUI` method.

```php
$action->getUI(); // will return 'web'
```
{% endhint %}

### Response Helpers Methods[​](https://apiato.io/docs/components/main-components/controllers#response-helpers-methods) <a href="#response-helpers-methods" id="response-helpers-methods"></a>

#### transform[​](https://apiato.io/docs/components/main-components/controllers#transform) <a href="#transform" id="transform"></a>

This method is incredibly useful and will be used in most cases.

* The first required parameter accepts data as an object or a Collection of objects.
* The second required parameter is the transformer class.
* The third optional parameter allows you to specify the [includes](https://apiato.io/docs/components/main-components/transformers#including-relationships) that should be returned in the response.
* The fourth optional parameter lets you include metadata in the response. This metadata will be returned under the `meta` key in the `custom` key.

```php
// With Includes
$this->transform($resouce, ResourceTransformer::class, ['foo', 'bar']);
```

```php
// With Meta
$this->transform($resouce, ResourceTransformer::class, meta: ['foo' => 'bar', 'baz' => 1]);

// Response
{
  "data": {},
  "meta": {
    "include": [...],
    "custom": {
      "foo": "bar",
      "baz": 1
    },
    "pagination": {}
  }
}
```

#### withMeta[​](https://apiato.io/docs/components/main-components/controllers#withmeta) <a href="#withmeta" id="withmeta"></a>

This method enables you to add metadata to the response, and it MUST be used in conjunction with the `transform` method. This is different from the `meta` parameter in the `transform` method. This metadata will be returned directly under the `meta` key.

You can use this method in conjunction with the `meta` parameter in the `transform` method.

```php
$metaData = ['foo' => 999, 'bar'];

$this->withMeta($metaData)->transform($sample, SampleTransformer::class, meta: ['foo' => 'bar', 'baz' => 1]);

// Response
{
  "data": {},
    "meta": {
      "foo": 999,
      "0": "bar",
      "include": [...],
      "custom": {
        "foo": "bar",
        "baz": 1
      },
      "pagination": {}
  }
}
```

#### json[​](https://apiato.io/docs/components/main-components/controllers#json) <a href="#json" id="json"></a>

This method allows you to pass an array of data that will be represented as JSON.

```php
$this->json($data)
```

#### created[​](https://apiato.io/docs/components/main-components/controllers#created) <a href="#created" id="created"></a>

This method allows you to return a response with a `201` status code.

```php
$this->created($data)
```

#### deleted[​](https://apiato.io/docs/components/main-components/controllers#deleted) <a href="#deleted" id="deleted"></a>

This method allows you to return a response with a `202` status code.

```php
$this->deleted($deletedModel)

// Response
{
  "message": "Model (1) Deleted Successfully."
}
```

#### accepted[​](https://apiato.io/docs/components/main-components/controllers#accepted) <a href="#accepted" id="accepted"></a>

This method allows you to return a response with a `202` status code.

```php
$this->accepted($data)
```

#### noContent[​](https://apiato.io/docs/components/main-components/controllers#nocontent) <a href="#nocontent" id="nocontent"></a>

This method allows you to return a response with a `204` status code.

```php
$this->noContent()
```
