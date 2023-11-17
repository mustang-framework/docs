# Localization

Languages are not real Components, they are just files that holds translations.

Laravel localization features provide a convenient way to retrieve strings in various languages, allowing you to easily support multiple languages within your application.

### Rules[​](https://apiato.io/docs/pacakges/localization#rules) <a href="#rules" id="rules"></a>

* Languages CAN be placed inside the Containers. However, the default laravel `resources/lang` languages files are still loaded and can be used as well.
* All Translations are namespaced as the camelCase of its Section name + `@` + camelCase of its Container name.\
  For example, translation key inside a translation file named `messages` inside `MySection` > `MyContainer` can be accessed like this: `__(mySection@myContainer::messages.welcome)`

### Folder Structure[​](https://apiato.io/docs/pacakges/localization#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

```
- app
  - Containers
    - {Section}
      - {Container}
        - Resources
          - Languages
            - en
              - messages.php
              - users.php
            - ar
              - messages.php
              - users.php
```

### Usage[​](https://apiato.io/docs/pacakges/localization#usage) <a href="#usage" id="usage"></a>

```
__('myS
ection@myContainer::messages.welcome');
```

### Installation[​](https://apiato.io/docs/pacakges/localization#installation) <a href="#installation" id="installation"></a>

Note

Intl PHP Extension is required to use this feature.

```
composer require mustang/localization-container
```

### Creating new languages files[​](https://apiato.io/docs/pacakges/localization#create-new-languages-files) <a href="#create-new-languages-files" id="create-new-languages-files"></a>

Languages file can be placed in any container, not only the Localization Container.\
Place language files inside the `/Languages` folder of your container. For example `app/Containers/Section/Container/Languages`. You can also place general language files in `app/Ship/Languages`.

Note

Just create the `Languages` folder if it does not exist.

Example languages files are included at `\lang`. (default Laravel translations)

### Support new languages[​](https://apiato.io/docs/pacakges/localization#support-new-languages) <a href="#support-new-languages" id="support-new-languages"></a>

All supported languages must be added to the `supported_languages` array in the `vendor-localization.php` config file to prevent users from requesting unsupported languages. There are 2 ways to do this: Using the configs or by modifying the source code.

```
    'supported_languages' => [
        'ar',
        'en' => [
            'en-GB',
            'en-US',
        ],
        'es',
        'fr',
        'fa',
    ],
```

#### Publishing configs[​](https://apiato.io/docs/pacakges/localization#publishing-configs) <a href="#publishing-configs" id="publishing-configs"></a>

```
php artisan vendor:publish
```

Config file will be copied to `app/Ship/Configs/vendor-localization.php`

#### Modifying the source code[​](https://apiato.io/docs/pacakges/localization#modify-code) <a href="#modify-code" id="modify-code"></a>

1- Copy the container from `Vendor` to `AppSection` (or any of your custom sections) of your project\
2- Fix the namespaces\
3- Remove `mustang/localization-container` dependency from project root composer.json

### Select Request Language[​](https://apiato.io/docs/pacakges/localization#select-request-language) <a href="#select-request-language" id="select-request-language"></a>

You can select the language of the response by adding the header `Accept-Language` to the request. By default, the `Accept-Language` is set to the language defined in `config/app.php` `locale`.

Please note that `Accept-Language` only determines, that the client _would like_ to get the information in this specific language. It is not given, that the API responds in this language!

When the `Accept-Language` header is missing, the default locale will be applied.

Note

Please be sure that your client does not send an `Accept-Language` header automatically. Some REST clients (e.g., `POSTMAN`) automatically add header-fields based on the operating-systems settings. So your `Accept-Language` header may be set automatically without you knowing!

The API will answer with the applied language in the `Content-Language` header of the response.

If the requested language cannot be resolved (e.g. it is not defined) the API throws an `UnsupportedLanguageException` to tell the client about this.

The overall workflow of the Middleware is as follows: 1) Extract the `Accept-Language` field from the request header. If none is set, use the default locale from the config file 2) Build a list of all supported localizations based on the configuration of the respective container. If a language (top level) contains regions (sub-level), order them like this: `['en-GB', 'en-US', 'en']` (the regions before languages, as regions are more specific) 3) Check, if the value from 1) is within the list from 2). If the value is within this list, set it as application language, if not throw an `Exception`.

### Translating Strings[​](https://apiato.io/docs/pacakges/localization#translating-strings) <a href="#translating-strings" id="translating-strings"></a>

By default, all the Container translation files are namespaced as the camelCase of its Section name + `@` + camelCase of its Container name.

{% hint style="info" %}
Translation files in **Ship** folder are exception to this and will be namespaced with the word "**ship**" instead of section name, e.g. `__('ship::notifications.welcome')`
{% endhint %}

### Example[​](https://apiato.io/docs/pacakges/localization#example) <a href="#example" id="example"></a>

If a translation file called `notifications` is inside `MySection` > `MyContainer` that contains translation for `welcome` like "Welcome to our store :)". You can access this translation as follows `__('mySection@myContainer::notifications.welcome')`. If you remove the namespace (part before `::`) and try to access it like this `__('notifications.welcome')` it will not find your translation and will print `notifications.welcome` only.

Note

If you try to load a string for a language that is **not available** (e.g., there is no folder for this language), Mustang will stick to the default one that is defined in `app.locale` config file. This is also true, if the requested locale is present in the `supported_languages` array from the configuration file.

### Enable/Disable Localization[​](https://apiato.io/docs/pacakges/localization#enable-disable-localization) <a href="#enable-disable-localization" id="enable-disable-localization"></a>

When using this container, Localization is **enabled** by default. To disable it set `LOCALIZATION_ENABLED` to `false` in the `.env` file. You can also change this behaviour in the [configs](https://apiato.io/docs/pacakges/localization#publishing-configs). This will disable the localization middleware.

### Get Available Localizations[​](https://apiato.io/docs/pacakges/localization#get-available-localizations) <a href="#get-available-localizations" id="get-available-localizations"></a>

This container provides a convenient way to get all defined localizations. These localizations can be retrieved via `GET /localizations` by default. Note that this route only outputs the "top level" locales, like `en` from the example above. However, if specific regions (e.g., `en-US`) are defined, these will show up in the result as well.

The `Transformer` for the localizations not only provide the `language` (e.g., `de`), but also outputs the name of the language in this specific language (e.g., `locale_name => Deutsch`). Furthermore, the language name is outputted in the applications default name (e.g., configured in `app.locale`). This would result in `default_name => German`.

The same applies to the regions that are defined (e.g., `de-DE`). Consequently, this results in `locale_name => Deutschland` and `default_name = Germany`.

### Tests[​](https://apiato.io/docs/pacakges/localization#tests) <a href="#tests" id="tests"></a>

To change the default language in your tests requests. You can set the `env` language in the `phpunit.xml` file.
