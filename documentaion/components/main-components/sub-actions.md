# Sub Actions

SubActions are designed to eliminate code duplication within Actions. They enable Actions to share a sequence of Tasks, thus promoting reusability. Similar to how Tasks enable Actions to share specific functionalities, SubActions serve to share a predefined set of Tasks.

All the features and capabilities available for regular Actions are also applicable to SubActions.

To generate new SubActions you may use the `mustang:generate:subaction` interactive command:

```
php artisan mustang:generate:subaction
```

### Definition & Principles[​](https://apiato.io/docs/components/main-components/subactions#definition--principles) <a href="#definition--principles" id="definition--principles"></a>

Read [**Porto SAP Documentation (#Sub-Actions)**](https://github.com/Mahmoudz/Porto#definitions--principles).

### Rules[​](https://apiato.io/docs/components/main-components/subactions#rules) <a href="#rules" id="rules"></a>

* All SubActions:
  * MUST be placed in the `app/Containers/{Section}/{Container}/Actions` directory.
  * MUST extend the `App\Ship\Parents\Actions\SubAction` class.
    * The parent extension SHOULD be aliased as `ParentSubAction`.

### Folder Structure[​](https://apiato.io/docs/components/main-components/subactions#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

```
app
└── Containers
    └── Section
        └── Container
            └── Actions
                ├── ValidateAddressSubAction.php
                ├── BuildOrderSubAction.php
                └── ...
```

### Code Example[​](https://apiato.io/docs/components/main-components/subactions#code-example) <a href="#code-example" id="code-example"></a>

```
use ...
use App\Ship\Parents\Actions\SubAction as ParentSubAction;

class DemoSubAction extends ParentSubAction
{
    public function __construct(
        private readonly DemoTask $demoTask
    ) {
    }

    public function run(array $data)
    {
        return $this->demoTask->run($data);
    }
}
```

\
