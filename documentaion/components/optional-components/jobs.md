# Jobs

Mustang jobs are just [Laravel Jobs](https://laravel.com/docs/queues), and they function in the exact same way as Laravel jobs. However, they come with additional rules and conventions specific to Mustang.

To generate new jobs you may use the `mustang:generate:job` interactive command:

```
php artisan mustang:generate:job
```

### Rules[​](https://apiato.io/docs/components/optional-components/jobs#rules) <a href="#rules" id="rules"></a>

* All container-specific Jobs MUST be placed in the `app/Containers/{Section}/{Container}/Jobs` directory.
* All general Jobs MUST be placed in the `app/Ship/Jobs` directory.
* All Jobs MUST extend the `App\Ship\Parents\Jobs\Job` class.
  * The parent extension SHOULD be aliased as `ParentJob`.

### Folder Structure[​](https://apiato.io/docs/components/optional-components/jobs#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

```
app
├── Containers
│   └── Section
│       └── Container
│           └── Jobs
│               ├── FooJob.php
│               ├── BarJob.php
│               └── ...
└── Ship
    └── Jobs
        ├── BazJob.php
        └── ...
```

### Code Example[​](https://apiato.io/docs/components/optional-components/jobs#code-example) <a href="#code-example" id="code-example"></a>

Jobs are defined exactly as you would define them in Laravel.
