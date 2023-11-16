# Actions

Actions serve as the embodiment of the application's Use Cases, encapsulating the various operations that users or software can execute within the application.

To generate new actions you may use the `mustang:generate:action` interactive command:

```
php artisan mustang:generate:action
```

Additionally, to retrieve a list of the existing actions in your Mustang application, use the `Mustang:list:actions` command.

```
php artisan mustang:list:actions
```

### Definition & Principles[​](https://apiato.io/docs/components/main-components/actions#definition--principles) <a href="#definition--principles" id="definition--principles"></a>

Read [**Porto SAP Documentation (#Actions)**](https://github.com/Mahmoudz/Porto#definitions--principles).

### Rules[​](https://apiato.io/docs/components/main-components/actions#rules) <a href="#rules" id="rules"></a>

* All Actions:
  * MUST be placed in the `app/Containers/{Section}/{Container}/Actions` directory.
  * MUST extend the `App\Ship\Parents\Actions\Action` class.
    * The parent extension SHOULD be aliased as `ParentAction`.
* The same Action MAY be called by multiple Controllers (API, Web, CLI)

### Folder Structure[​](https://apiato.io/docs/components/main-components/actions#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

```
app
└── Containers
    └── Section
        └── Container
            └── Actions
                ├── CreateResourceAction.php
                ├── DeleteResourceAction.php
                └── ...
```

### Code Example[​](https://apiato.io/docs/components/main-components/actions#code-example) <a href="#code-example" id="code-example"></a>

```
use ...
use App\Ship\Parents\Actions\Action as ParentAction;

class DemoAction extends ParentAction
{
    public function __construct(
        private readonly DemoTask $demoTask
    ) {
    }

    public function run(DemoRequest $request)
    {
        return $this->demoTask->run();
    }
}
```

**Calling multiple Tasks**[**​**](https://apiato.io/docs/components/main-components/actions#calling-multiple-tasks)

```
use ...
use App\Ship\Parents\Actions\Action as ParentAction;

class DemoAction extends ParentAction
{
    public function __construct(
        private readonly DemoATask $demoATask,
        private readonly DemoBTask $demoBTask
    ) {
    }
    
    public function run($xxx, $yyy, $zzz): void
    {
        $foo = $this->demoATask->run($xxx, $yyy);
        $bar = $this->demoBTask->run($zzz);
    }
}
```

### Handling Transactions[​](https://apiato.io/docs/components/main-components/actions#handling-transactions) <a href="#handling-transactions" id="handling-transactions"></a>

In certain scenarios, you may need to wrap a specific call within a `Database Transaction` to ensure data integrity (see [Laravel Documentation](https://laravel.com/docs/master/database#database-transactions)).

Mustang offers a `transactionalRun` method, which internally wraps the `run` method of the action within a `DB::Transaction` and passes all the parameters "as is" to it.

The beauty of using the `transactionalRun` method is that if any `Exception` occurs during the execution of the `run` method, everything performed in this context is automatically rolled back from the database. However, it's important to note that not all operations may be automatically rolled back. For example, file system operations, such as uploading an image, are typically not covered by the database transaction and would need to be handled manually.
