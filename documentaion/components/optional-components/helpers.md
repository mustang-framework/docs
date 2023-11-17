# Helpers

You have the option to create your own global "helper" PHP functions in designated directories, and Mustang will automatically autoload them for you.

{% hint style="danger" %}
Deprecation Notice

Please be aware that Container Helpers are deprecated and will be removed in the next major release.
{% endhint %}

### Rules[​](https://apiato.io/docs/components/optional-components/helpers#rules) <a href="#rules" id="rules"></a>

* You MAY create as many helper files as you need per container.
* All container-specific helpers MUST be placed in the `app/Containers/{Section}/{Container}/Helpers` directory.
* All general helpers MUST be placed in the `app/Ship/Helpers` directory.

### Folder Structure[​](https://apiato.io/docs/components/optional-components/helpers#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

```
app
├── Containers
│   └── Section
│       └── Container
│           └── Helpers
│               ├── helpers.php
│               ├── mix.php
│               └── ...
└── Ship
    └── Helpers
        ├── another-helper.php
        ├── and-another.php
        └── ...
```

### Code Example[​](https://apiato.io/docs/components/optional-components/helpers#code-example) <a href="#code-example" id="code-example"></a>

```
if (!function_exists('add')) {
    function add(int $firstNumber, int $secondNumber): int
    {
        return $firstNumber + $secondNumber;
    }
}
```
