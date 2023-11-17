# Models

[Models](https://laravel.com/docs/eloquent) are responsible for representing the data of the application and providing an abstraction for interacting with that data. They encapsulate the business logic and rules associated with the data, as well as facilitate the communication with the underlying database.

To generate new models you may use the `mustang:generate:model` interactive command:

```bash
php artisan mustang:generate:model
```

### Definition & Principles[​](https://apiato.io/docs/components/main-components/models#definition--principles) <a href="#definition--principles" id="definition--principles"></a>

Read [**Porto SAP Documentation (#Models)**](https://github.com/Mahmoudz/Porto#definitions--principles).

### Rules[​](https://apiato.io/docs/components/main-components/models#rules) <a href="#rules" id="rules"></a>

* All Models:
  * MUST be placed in the `app/Containers/{Section}/{Container}/Models` directory.
  * MUST extend the `App\Ship\Parents\Models\Model` class.
    * The parent extension SHOULD be aliased as `ParentModel`.
* All User Models MUST extend the `App\Ship\Parents\Models\UserModel` class.
  * The parent extension SHOULD be aliased as `ParentUserModel`.

### Folder Structure[​](https://apiato.io/docs/components/main-components/models#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

```
app
└── Containers
    └── Section
        └── Container
            └── Models
                ├── Model.php
                └── ...
```

### Code Example[​](https://apiato.io/docs/components/main-components/models#code-example) <a href="#code-example" id="code-example"></a>

Models are defined exactly as you would define them in Laravel.

### Model Trait[​](https://apiato.io/docs/components/main-components/models#model-trait) <a href="#model-trait" id="model-trait"></a>

If your model does not extend the `App\Ship\Parents\Models\Model` or the `App\Ship\Parents\Models\UserModel` class, it is essential to incorporate the `ModelTrait` trait into your model. By doing so, your model will benefit from various functionalities provided by the trait, such as hash ids and other features necessary for proper integration with the framework.

```php
use Mustang\Core\Traits\ModelTrait;

class Demo
{
    use ModelTrait;
    ...
}
```
