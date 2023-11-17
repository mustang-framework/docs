# Migrations

Mustang migrations are just [Laravel Migrations](https://laravel.com/docs/migrations), and they function in the exact same way as Laravel migrations. However, they come with additional rules and conventions specific to Mustang.

To generate new migrations you may use the `mustang:generate:migration` interactive command:

```
php artisan mustang:generate:migration
```

### Rules[​](https://apiato.io/docs/components/optional-components/migrations#rules) <a href="#rules" id="rules"></a>

* All container-specific Migrations MUST be placed in the `app/Containers/{Section}/{Container}/Data/Migrations` directory.
* All general Migrations MUST be placed in the `app/Ship/Migrations` directory.

### Folder Structure[​](https://apiato.io/docs/components/optional-components/migrations#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

```
app
├── Containers
│   └── Section
│       └── Container
│           └── Data
│               └── Migrations
│                   ├── 0000_01_01_000001_create_things_table.php
│                   └── ...
└── Ship
    └── Migrations
        ├── 0000_02_02_000002_create_another_things_table.php
        └── ...
```

### Code Example[​](https://apiato.io/docs/components/optional-components/migrations#code-example) <a href="#code-example" id="code-example"></a>

Migrations are defined exactly as you would define them in Laravel.
