# Documentation

Every great API needs a great Documentation.

Mustang makes writing and generating documentation very easy with the `php artisan mustang:apidoc` command.

### Requirements[​](https://apiato.io/docs/pacakges/documentation#requirements) <a href="#requirements" id="requirements"></a>

* Install [ApiDocJs](https://apidocjs.com/) in the project directory
  * (`npm install apidoc`)
* (Recommended) read the [Routes](https://apiato.io/docs/components/main-components/routes) page first.

### Installation[​](https://apiato.io/docs/pacakges/documentation#installation) <a href="#installation" id="installation"></a>

```
composer require mustang/documentation-generator-container
```

Tip

This container is installed by default with an Mustang fresh installation.

### Usage[​](https://apiato.io/docs/pacakges/documentation#usage) <a href="#usage" id="usage"></a>

#### Write PHP **docblock**[​](https://apiato.io/docs/pacakges/documentation#write-php-docblock) <a href="#write-php-docblock" id="write-php-docblock"></a>

Write a PHP **docblock** on top of your endpoint like this:

_For more info about the parameters check out_ [_ApiDocJs_](https://apidocjs.com/#install) _documentation_

```
/**
 * @apiGroup           Authentication
 * @apiName            UserLogin
 * @api                {post} /clients/web/login User Login
 * @apiDescription     Description Here....
 * @apiVersion         1.0.0
 * @apiPermission      none
 *
 * @apiHeader          Accept application/json
 *
 * @apiParam           {String}     email
 * @apiParam           {String}     password
 *
 * @apiSuccessExample  {json}       Success-Response:
 *   HTTP/1.1 200 OK
 *   {
 *     "data": {
 *       "id": "XbPW7awNkzl83LD6",
 *       "name": "Super Admin",
 *       "email": "admin@admin.com"
 *       ...
 *   }
 *
 * @apiErrorExample  {json}       Error-Response:
 *   {
 *      "message":"401 Credentials Incorrect.",
 *      "status_code":401
 *   }
 *
 * @apiErrorExample  {json}       Error-Response:
 *   {
 *      "message":"Invalid Input.",
 *      "errors":{
 *         "email":[
 *            "The email field is required."
 *         ]
 *      },
 *      "status_code":422
 *   }
 */

use App\Containers\AppSection\Authentication\UI\API\Controllers\Controller;
use Illuminate\Support\Facades\Route;

Route::post('clients/web/login', Controller::class);
```

Note

All the Endpoint `DocBlocks` MUST be written inside Routes files, otherwise they won't be loaded.

#### Run Documentation Generator[​](https://apiato.io/docs/pacakges/documentation#run-documentation-generator) <a href="#run-documentation-generator" id="run-documentation-generator"></a>

Run the documentation generator command from the root directory:

```

php artisan mustang:apidoc

```

**Error: ApiDoc not found !!**[**​**](https://apiato.io/docs/pacakges/documentation#error-apidoc-not-found-)

If you get an error (`apidoc not found`),

1. [Publish the configs](https://apiato.io/docs/pacakges/documentation#publishing-configs)
2. Edit the `executable` path to **`$(npm bin)/apidoc`** or to however you access the `apidoc` tool on your machine.

```
    /*
    |--------------------------------------------------------------------------
    | Executable
    |--------------------------------------------------------------------------
    |
    | Specify how you run or access the `apidoc` tool on your machine.
    |
    */

    'executable' => 'node_modules/.bin/apidoc',
    // 'executable' => 'apidoc',
```

#### Visit Documentation URL's[​](https://apiato.io/docs/pacakges/documentation#visit-docs-urls) <a href="#visit-docs-urls" id="visit-docs-urls"></a>

Visit documentation URL's as shown in your terminal:

* Public (external) API at http://mustang.test/docs.
* Private (internal) API at http://mustang.test/docs/private.

Note

Every time you change the DocBlock of a Route file you need to run the `ustang:apidoc` command, to regenerate.

#### Shared Response[​](https://apiato.io/docs/pacakges/documentation#shared-response) <a href="#shared-response" id="shared-response"></a>

You can use shared responses to update the documentation faster, with less outdated responses and prevent duplicating the responses between routes.

Example: `_user.v1.public.php` will contain all responses (single, multiple...) of the User:

```
/**
 * @apiDefine UserSuccessSingleResponse
 * @apiSuccessExample {json} Success-Response:
HTTP/1.1 200 OK
{
   "data":{
      "object":"User",
      "id":eqwja3vw94kzmxr0,
   },
   "meta":{
      "include":[],
      "custom":[]
   }
}
 */
```

**Usage of the shared User response from any endpoint:**

```
* @apiUse UserSuccessSingleResponse
```

### Documentation Container Customization[​](https://apiato.io/docs/pacakges/documentation#documentation-customization) <a href="#documentation-customization" id="documentation-customization"></a>

There are 2 ways you can customize this container: Using its configs or by modifying the source code.

#### Publishing configs[​](https://apiato.io/docs/pacakges/documentation#publishing-configs) <a href="#publishing-configs" id="publishing-configs"></a>

```
php artisan vendor:publish
```

Config file will be copied to `app/Ship/Configs/vendor-documentation.php`

#### Modifying the source code[​](https://apiato.io/docs/pacakges/documentation#modify-code) <a href="#modify-code" id="modify-code"></a>

1. Copy the container from `Vendor` to `AppSection` (or any of your custom sections) of your project
2. Fix the namespaces
3. Remove `mustang/documentation-generator-container` dependency from project root composer.json
4. Update `section_name` & `html_files` in container configs
5. Update `apidoc.private.json` & `apidoc.public.json` files in `ApiDocJs/Configs` and fix the `filename`

```
{
    "header": {
        "filename": "Containers/NEW_SECTION_NAME/Documentation/UI/WEB/Views/documentation/header.md"
    }
}
```

#### Change the Documentations URL's[​](https://apiato.io/docs/pacakges/documentation#change-the-documentations-urls) <a href="#change-the-documentations-urls" id="change-the-documentations-urls"></a>

[Publish the configs](https://apiato.io/docs/pacakges/documentation#publishing-configs) and change `types.public.url` & `types.private.url`.

#### Private Documentation Protection[​](https://apiato.io/docs/pacakges/documentation#private-docs-protection) <a href="#private-docs-protection" id="private-docs-protection"></a>

By default, this feature is **disabled** in local environment and **enabled** in production.\
To change this behaviour [Publish the configs](https://apiato.io/docs/pacakges/documentation#publishing-configs) and change `protect-private-docs`.

Private documentations route is protected with the `auth:web` middleware. You can grant users access to the protected docs by updating `access-private-docs-roles` & `access-private-docs-permission` values in documentation config. By default, users need `access-private-docs` permission to access private docs.

#### Edit Default Generated Values in Templates[​](https://apiato.io/docs/pacakges/documentation#edit-default-generated-values-in-templates) <a href="#edit-default-generated-values-in-templates" id="edit-default-generated-values-in-templates"></a>

Mustang by defaults generates 2 API documentations, each one has its own `apidoc.json` file. Both can be modified from the Documentation Container in `app/Containers/Vendor/Documentation/ApiDocJs` and need Source code modification.

#### Edit the Documentation Header[​](https://apiato.io/docs/pacakges/documentation#edit-the-documentation-header) <a href="#edit-the-documentation-header" id="edit-the-documentation-header"></a>

The header is usually the Overview of your API. It contains Info about authenticating users, making requests, responses, potential errors, rate limiting, pagination, query parameters and anything you want.

All this information is written in `app/Containers/Vendor/Documentation/ApiDocJs/shared/header.template.en.md` file, and the same file is used as header for both private and public documentations.

To edit its content you need to modify its source code and open the markdown file in any markdown editor and edit it.

You will notice some variables like `{{rate-limit}}` and `{{token-expires}}`. Those are replaced when running `mustang:apidoc` with real values from your application configuration files.

Feel free to extend them to include more info about your API from the `app/Containers/Vendor/Documentation/Tasks/RenderTemplatesTask.php` class.

#### Localization for Documentation Header[​](https://apiato.io/docs/pacakges/documentation#localization-for-documentation-header) <a href="#localization-for-documentation-header" id="localization-for-documentation-header"></a>

Default, the documentation title is in English `en` localization.

See which locales are supported by going in `app/Containers/Vendor/Documentation/ApiDocJs/shared`

There will be some `header.template.{locale}.md` files in the folder.

You can change the language by adding `APIDOC_LOCALE=ru` to the `.env` file.

If you didn't find a file with your locale, you can create it. You need to modify its source code and create new file like `header.template.cn.md`

\
