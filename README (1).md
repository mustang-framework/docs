# Release Notes

### Versioning Scheme[​](https://apiato.io/docs/prologue/release-notes#versioning-scheme) <a href="#versioning-scheme" id="versioning-scheme"></a>

Mustang and its other first-party packages follow [Semantic Versioning](https://semver.org/). Major framework releases are released every year (\~Q1), while minor and patch releases may be released as often as every week. Minor and patch releases should never contain breaking changes.

When referencing the Mustang framework or its components from your application or package, you should always use a version constraint such as `^10.0`, since major releases of Mustang do include breaking changes. However, we strive to always ensure you may update to a new major release in one day or less.

### Support Policy[​](https://apiato.io/docs/prologue/release-notes#support-policy) <a href="#support-policy" id="support-policy"></a>

For all Mustang releases, bug fixes are provided for 18 months, and security fixes are provided for 2 years.

| Version | PHP (\*)  | Release        | Bug Fixes Until    | Security Fixes Until |
| ------- | --------- | -------------- | ------------------ | -------------------- |
| 10      | 8.1 - 8.2 | June 4th, 2023 | December 4th, 2024 | June 4th, 2025       |
| 11      | 8.2       | Q1 2024        | August 5th, 2025   | February 3rd, 2026   |

(\*) Supported PHP versions

### Mustang 10[​](https://apiato.io/docs/prologue/release-notes#apiato-12) <a href="#apiato-12" id="apiato-12"></a>

**PHP 8.1**[**​**](https://apiato.io/docs/prologue/release-notes#php-81)

Mustang 10.x requires a minimum PHP version of 8.1.

#### Breaking Changes[​](https://apiato.io/docs/prologue/release-notes#breaking-changes) <a href="#breaking-changes" id="breaking-changes"></a>

* Upgraded to Laravel v10 (All Laravel files (e.g. configs, .env, etc...) are now synced with the latest Laravel changes)
* Updated Composer dependencies to their latest version
* Laravel Passport route registration & customization has changed. Passport routes now reside in a dedicated [route file](https://github.com/apiato/apiato/blob/3d368c0ead610bfd9d5566ad7652419346732e53/app/Containers/AppSection/Authentication/UI/API/Routes/Passport.v1.private.php) (Instead of registering them in the provider).
* Middleware `$routeMiddleware` field is renamed to `$middlewareAliases`
* Trimmed down the TestCase by removing some useless traits including:

```
TestsMockHelperTrait
TestsResponseHelperTrait
```

* `encode()` method return value has changed -> In case of unencodable value (e.g. `null`), now returns `null` instead of `''`
* `decode()` method return value has changed -> In case of undecodable value (e.g. `null`), now returns `null` instead of `[]`
* [StateKeeperTrait](https://github.com/apiato/core/blob/cbf2acacf42ee442db5a301773c26944a049bfc1/Traits/StateKeeperTrait.php) is removed from `Request`

#### None Breaking Changes[​](https://apiato.io/docs/prologue/release-notes#none-breaking-changes) <a href="#none-breaking-changes" id="none-breaking-changes"></a>

* Everything is refactored to use `constructor` injection instead of directly using the Service Container like so `app(CreateUserByCredentialsTask::class)->run()`
* Added more tests and refactored the rest
* Switched to `invokable` controllers

```
\\ from
Route::get('profile', [GetAuthenticatedUserController::class, 'getAuthenticatedUser']);
\\ to
Route::get('profile', GetAuthenticatedUserController::class);
```

* All routes are moved into the private documentation. e.g. `RefreshProxyForWebClient.v1.public.php` -> `RefreshProxyForWebClient.v1.private.php`
* Added some getter methods to the [Request](https://github.com/apiato/core/blob/789606b41f1024c2da506bb6765d2fbfa85897cd/Abstracts/Requests/Request.php) including:

```
withUrlParameters()
getAccessArray()
getDecodeArray()
getUrlParametersArray()
```

* Added a `TestAssertionHelperTrait` to the TestCase which provides some usefull assertions

#### Bug Fixes[​](https://apiato.io/docs/prologue/release-notes#bug-fixes) <a href="#bug-fixes" id="bug-fixes"></a>

* `withMeta()` method on [ResponseTrait](https://github.com/apiato/core/blob/789606b41f1024c2da506bb6765d2fbfa85897cd/Traits/ResponseTrait.php) now correctly includes added metadata
* Calling invokable controllers from routes [#174](https://github.com/apiato/core/issues/174)
* Exception when trying to generate a WEB CRUD Controller from generator [#171](https://github.com/apiato/core/issues/171)
* PHP 8.1 warning on passing null to explode [#176](https://github.com/apiato/core/issues/176)
