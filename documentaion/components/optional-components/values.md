# Values

Value Objects are short names for known "Value Objects", which are simple objects similar to Models in terms of representing data. However, unlike Models, Value Objects are not stored in the database and, therefore, do not have IDs. Additionally, they do not possess functionality or modify any state; their sole purpose is to hold data.

Value Objects are particularly well-suited for use with Laravel [attributes casting](https://laravel.com/docs/eloquent-mutators#value-object-casting), which allows us to cast a Value Object to a specific type, enabling seamless integration with Eloquent models and database operations.

To generate new values you may use the `mustang:generate:value` interactive command:

```bash
php artisan mustang:generate:value
```

### Rules[​](https://apiato.io/docs/components/optional-components/values#rules) <a href="#rules" id="rules"></a>

* All container-specific Values MUST be placed in the `app/Containers/{section}/{container}/Values` directory.
* All general Values MUST be placed in the `app/Ship/Values` directory.
* All Values MUST extend the `App\Ship\Parents\Values\Value` class.
  * The parent extension SHOULD be aliased as `ParentValue`.

### Folder Structure[​](https://apiato.io/docs/components/optional-components/values#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

```
app
└── Containers
    └── Section
        └── Container
            └── Values
                ├── Output.php
                ├── Region.php
                └── ...
```

### Code Example[​](https://apiato.io/docs/components/optional-components/values#code-example) <a href="#code-example" id="code-example"></a>

```
class Location extends Value
{
    public function __construct(
        public float $latitude,
        public float $longitude,
    ) {
    }
}
```
