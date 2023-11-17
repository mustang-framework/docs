# Tasks

Tasks are specialized classes that hold shared business logic, fostering code reuse and promoting efficient organization. They play a vital role in encapsulating functionalities that are utilized by multiple Actions spanning various Containers within your application.

To generate new tasks you may use the `mustang:generate:task` interactive command:

```bash
php artisan mustang:generate:task
```

Additionally, to retrieve a list of the existing tasks in your Mustang application, use the `mustang:list:tasks` command.

```bash
php artisan mustang:list:tasks
```

### Definition & Principles[​](https://apiato.io/docs/components/main-components/tasks#definition--principles) <a href="#definition--principles" id="definition--principles"></a>

Read [**Porto SAP Documentation (#Tasks)**](https://github.com/Mahmoudz/Porto#definitions--principles).

### Rules[​](https://apiato.io/docs/components/main-components/tasks#rules) <a href="#rules" id="rules"></a>

* All Tasks:
  * MUST be placed in the `app/Containers/{Section}/{Container}/Tasks` directory.
  * MUST extend the `App\Ship\Parents\Tasks\Task` class.
    * The parent extension SHOULD be aliased as `ParentTask`.

### Folder Structure[​](https://apiato.io/docs/components/main-components/tasks#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

```
app
└── Containers
    └── Section
        └── Container
            └── Tasks
                ├── CreateResourceTask.php
                ├── DeleteResourceTask.php
                └── ...
```

### Code Example[​](https://apiato.io/docs/components/main-components/tasks#code-example) <a href="#code-example" id="code-example"></a>

```php
use ...
use App\Ship\Parents\Tasks\Task as ParentTask;

class DemoTask extends ParentTask
{
    public function run(int $a, int $b): int
    {
        return $a + $b;
    }
}
```
