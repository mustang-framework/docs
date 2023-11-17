# Commands

Mustang commands are just [Laravel Commands](https://laravel.com/docs/artisan), and they function in the exact same way as Laravel commands. However, they come with additional rules and conventions specific to Mustang.

### Rules[​](https://apiato.io/docs/components/optional-components/commands#rules) <a href="#rules" id="rules"></a>

* All container-specific Commands MUST be placed in the `app/Containers/{Section}/{Container}/UI/CLI/Commands` directory.
* All general Commands MUST be placed in the `app/Ship/Commands` directory.
* All Commands:
  * MUST extend the `App\Ship\Parents\Commands\ConsoleCommand` class.
    * The parent extension SHOULD be aliased as `ConsoleCommand`.
  * SHOULD call an Action to perform its job, and SHOULD NOT contain any business logic.

### Folder Structure[​](https://apiato.io/docs/components/optional-components/commands#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

```
app
├── Containers
│   └── Section
│       └── Container
│           └── UI
│               └── CLI
│                   └── Commands
│                       ├── FirstCommand.php
│                       ├── SecondCommand.php
│                       └── ...
└── Ship
    └── Commands
        ├── FirstCommand.php
        ├── SecondCommand.php
        └── ...
```

### Code Example[​](https://apiato.io/docs/components/optional-components/commands#code-example) <a href="#code-example" id="code-example"></a>

Commands are defined exactly as you would define them in Laravel.

### Closure Commands[​](https://apiato.io/docs/components/optional-components/commands#closure-commands) <a href="#closure-commands" id="closure-commands"></a>

You can define your console closure commands in `app/Ship/Commands/closures.php`.
