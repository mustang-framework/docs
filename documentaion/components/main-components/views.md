# Views

[Views](https://laravel.com/docs/views) offer a convenient mechanism for organizing HTML content in separate files. They facilitate the separation of your controller or application logic from the presentation logic.

### Definition & Principles[​](https://apiato.io/docs/components/main-components/views#definition--principles) <a href="#definition--principles" id="definition--principles"></a>

Read [**Porto SAP Documentation (#Views)**](https://github.com/Mahmoudz/Porto#definitions--principles).

### Rules[​](https://apiato.io/docs/components/main-components/views#rules) <a href="#rules" id="rules"></a>

* All container-specific Views MUST be placed in the `app/Containers/{Section}/{Container}/UI/WEB/Views` directory.
* All general Views MUST be placed in the `app/Ship/Views` directory.

### Folder Structure[​](https://apiato.io/docs/components/main-components/views#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

```
app
└── Containers
    └── Section
        └── Container
            └── UI
                └── WEB
                    └── Views
                        ├── view-a.php
                        ├── view-b.php
                        └── ...
```

### Code Example[​](https://apiato.io/docs/components/main-components/views#code-example) <a href="#code-example" id="code-example"></a>

Views are defined exactly as you would define them in Laravel.

### Namespaces[​](https://apiato.io/docs/components/main-components/views#namespaces) <a href="#namespaces" id="namespaces"></a>

All views are namespaced using the camelCase of their Section name followed by `@` and then the camelCase of their Container name.

For example, if you have a view named `welcome-page` in the `app/Containers/MySection/MyContainer/UI/WEB/Views` directory, you can access it like this: `view(mySection@myContainer::welcome-page)`.

Attempting to access the view without the namespace, such as `view('welcome-page')`, will result in the view not being found.

An exception to this namespace convention is for view files located in the `app/Ship/Views` and `app/Ship/Mails/Templates` directories. These views will be namespaced using the word `ship` instead of the Section and Container names.

For example, you would access such a view like this: `view(ship::welcome-page)`.

\
