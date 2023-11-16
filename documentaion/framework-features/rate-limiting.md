# Rate Limiting

Mustang uses [Laravel Rate Limiting](https://laravel.com/docs/rate-limiting) feature to protect your API from abuse and to ensure stability.

Note

This feature is only applied to API requests, and not to web requests and is _enabled_ by default.

### How it works[​](https://apiato.io/docs/framework-features/rate-limiting#how-it-works) <a href="#how-it-works" id="how-it-works"></a>

Given the rate limit window is `1` minute per endpoint, with individual calls allowing for `30` requests in each window each user is allowed to make `30` calls per endpoint every `1` minute (for each unique access token).

### Configuration[​](https://apiato.io/docs/framework-features/rate-limiting#configuration) <a href="#configuration" id="configuration"></a>

Rate limiting configuration is done in the `app/Ship/Configs/mustang.php` config file.

### Rate Limiting Headers[​](https://apiato.io/docs/framework-features/rate-limiting#rate-limiting-headers) <a href="#rate-limiting-headers" id="rate-limiting-headers"></a>

For how many hits you can perform on an endpoint, you can always check the header:

```
X-RateLimit-Limit → 30
X-RateLimit-Remaining → 29
```

### Disable Rate Limiting[​](https://apiato.io/docs/framework-features/rate-limiting#disable-rate-limiting) <a href="#disable-rate-limiting" id="disable-rate-limiting"></a>

The API rate limiting middleware name is `api` and is assigned to all API endpoints by default. You can disable it on specific endpoints or globally.

#### On a specific endpoint[​](https://apiato.io/docs/framework-features/rate-limiting#on-a-specific-endpoint) <a href="#on-a-specific-endpoint" id="on-a-specific-endpoint"></a>

Rate limiting can be disabled by removing the `api` middleware from an endpoint using `withoutMiddleware('throttle:api')` method. [Read More](https://laravel.com/docs/middleware#excluding-middleware).

#### Globally[​](https://apiato.io/docs/framework-features/rate-limiting#globally) <a href="#globally" id="globally"></a>

To disable rate limiting completely, set `GLOBAL_API_RATE_LIMIT_ENABLED` to `false` in the `.env` file.
