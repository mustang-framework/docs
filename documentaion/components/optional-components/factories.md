# Factories

Mustang factories are just [Laravel Factories](https://laravel.com/docs/eloquent-factories), and they function in the exact same way as Laravel factories. However, they come with additional rules and conventions specific to Mustang.

To generate new factories you may use the `mustang:generate:factory` interactive command:

```
php artisan mustang:generate:factory
```

### Rules[​](https://apiato.io/docs/components/optional-components/factories#rules) <a href="#rules" id="rules"></a>

* Factory name MUST be the same as its corresponding model name and suffixed with `Factory`.
* All Factories:
  * MUST be placed in the `app/Containers/{Section}/{Container}/Data/Factories` directory.
  * MUST extend the `App\Ship\Parents\Factories\Factory` class.
    * The parent extension SHOULD be aliased as `ParentFactory`.

### Folder Structure[​](https://apiato.io/docs/components/optional-components/factories#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

```
app
└── Containers
    └── Section
        └── Container
            └── Data
                └── Factories
                    ├── ModelFactory.php
                    └── ...
```

### Code Example[​](https://apiato.io/docs/components/optional-components/factories#code-example) <a href="#code-example" id="code-example"></a>

Factories are defined exactly as you would define them in Laravel.

### Model & Factory Discovery Conventions[​](https://apiato.io/docs/components/optional-components/factories#model--factory-discovery-conventions) <a href="#model--factory-discovery-conventions" id="model--factory-discovery-conventions"></a>

When you use a factory, Laravel relies on conventions to determine the appropriate factory for the model. By default, Laravel will search for a factory in the `Database\Factories` namespace that has a class name matching the model name and is suffixed with `Factory`.

However, in the case of Mustang, factories are distributed across the Containers, and they are not all within the same namespace. As a result, Apiato follows a different convention to locate the appropriate factory for a model. This is achieved by utilizing the `Mustang\Core\Traits\FactoryLocatorTrait` trait, which is incorporated into the `Mustang\Core\Traits\ModelTrait` trait used by all Mustang models.

Therefore, if your model does not extend the `App\Ship\Parents\Models\Model` or the `App\Ship\Parents\Models\UserModel` class, it is essential to include the `ModelTrait` trait in your model. By doing so, Mustang will be able to locate the appropriate factory and use it for the model when needed.

```
use Mustang\Core\Traits\ModelTrait;

class Demo
{
    use ModelTrait;
    ...
}
```
