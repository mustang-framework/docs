# Configs

Mustang configs are just Laravel configs, and they function in the exact same way as Laravel configs. However, they come with additional rules and conventions specific to Mustang.

To generate new configs you may use the `mustang:generate:configuration` interactive command:

```
php artisan mustang:generate:configuration
```

### Rules[​](https://apiato.io/docs/components/optional-components/configs#rules) <a href="#rules" id="rules"></a>

* Containers MAY have as many config files as they need.
* All container-specific and third-party package config files MUST be placed in the `app/Containers/{Section}/{Container}/Configs` directory.
* All general config files MUST be placed in the `app/Ship/Configs` directory.
* All Laravel config files MUST be kept in the root `config` folder.
* You MUST NOT add any non-Laravel or third-party config files to the root `config` folder.

### Folder Structure[​](https://apiato.io/docs/components/optional-components/configs#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

```
app
├── Containers
│   └── Section
│       └── Container
│           └── Configs
│               ├── section-container.php
│               ├── another.php
│               └── ...
├── Ship
│   └── Configs
│       ├── another-thing.php
│       ├── and-another.php
│       └── ...
└── config
    ├── app.php
    └── ...
```

### Code Example[​](https://apiato.io/docs/components/optional-components/configs#code-example) <a href="#code-example" id="code-example"></a>

Configs are defined exactly as you would define them in Laravel.

### Container Main Config File[​](https://apiato.io/docs/components/optional-components/configs#container-main-config-file) <a href="#container-main-config-file" id="container-main-config-file"></a>

It is recommended that each container possesses a primary configuration file. While it is not obligatory, adhering to this practice prevents clashes between third-party package configurations and container-specific configurations.

The primary configuration file of a container should be named in accordance with a certain convention: `camelCase` representation of the container's Section name, succeeded by `-`, and then the `camelCase` representation of the Container name.

For instance, if you have a container named "MyContainer" within the "MySection" section, the configuration file would be named `mySection-myContainer.php`.

\
