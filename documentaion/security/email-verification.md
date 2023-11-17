# Email Verification

Many web applications require users to verify their email addresses before using the application. To simplify this process, Mustang offers built-in services for sending and verifying email verification requests.

### Configuration[​](https://apiato.io/docs/security/email-varification#configuration) <a href="#configuration" id="configuration"></a>

All the settings related to email verification are stored in the `app/Containers/AppSection/Authorization/Configs/appSection-authentication` configuration file.

Activate email verification by setting the `require_email_verification` option in the `appSection-authentication` configuration file to `true`.

Also, ensure that your application is configured to send email messages.

Tip

Detailed information on email configuration can be found in the [Laravel documentation](https://laravel.com/docs/mail).

Include your web app's email verification redirect page URL (e.g., `https://myapp.com/email/verify`) in the `allowed-verify-email-urls` array within the `appSection-authentication` config.

#### Model Preparation[​](https://apiato.io/docs/security/email-varification#model-preparation) <a href="#model-preparation" id="model-preparation"></a>

Confirm that your `User` model implements the `MustVerifyEmail` contract. The default `User` model in Mustang typically includes this requirement.

#### Database Preparation[​](https://apiato.io/docs/security/email-varification#database-preparation) <a href="#database-preparation" id="database-preparation"></a>

Ensure your `users` table contains an `email_verified_at` column to store the email verification date and time. The migration for the `users` table, included with Mustang, already includes this column.

### Routing[​](https://apiato.io/docs/security/email-varification#routing) <a href="#routing" id="routing"></a>

#### The Email Verification Notice[​](https://apiato.io/docs/security/email-varification#the-email-verification-notice) <a href="#the-email-verification-notice" id="the-email-verification-notice"></a>

If you've enabled email verification, the API will automatically send an email verification link to the user's provided email address upon user creation.

#### The Email Verification Handler[​](https://apiato.io/docs/security/email-varification#the-email-verification-handler) <a href="#the-email-verification-handler" id="the-email-verification-handler"></a>

Define a route to handle requests when the user clicks the email verification link sent via email. Mustang includes a route for this purpose, `email/verify/{id}/{hash}`.

#### Resending The Verification Email[​](https://apiato.io/docs/security/email-varification#resending-the-verification-email) <a href="#resending-the-verification-email" id="resending-the-verification-email"></a>

To resend the email verification link, use the `/email/verification-notification` endpoint.

#### Protecting Routes[​](https://apiato.io/docs/security/email-varification#protecting-routes) <a href="#protecting-routes" id="protecting-routes"></a>

Apply the `verified` middleware to restrict access to certain routes for confirmed users. When email verification is enabled, unverified users trying to access protected endpoints will trigger an exception, prompting them to confirm their email address.

Please note that if email verification is disabled, the `verified` middleware won't protect routes against unconfirmed users and will have no effect.

### Process Flow[​](https://apiato.io/docs/security/email-varification#process-flow) <a href="#process-flow" id="process-flow"></a>

1. Add your web app's email verification redirect page URL (e.g., `https://myapp.com/email/verify`) to the `allowed-verify-email-urls` array in the `appSection-authentication` config.
2. Send the email verification URL to the user by calling the `/email/verification-notification` endpoint using your web app. Pass one of the valid URLs added in step 1 to the `verification_url` field of the endpoint.
3. An email verification link will be sent to the user's email, resembling this format: `https://myapp.test/email/verify?url=https://api.myapi.test/v1/email/verify/XbPW7awNkzl83LD6/eaabd911e2e07ede6456d3bd5725c6d4a5c2dc0b?expires=1646913047&signature=232702865b8353c445b39c50397e66db33c74df80e3db5a7c0d46ef94c8ab6a9`.
4. When the user clicks the link, they'll be redirected to the URL specified in step 1, e.g., `https://myapp.com/email/verify`, with the `url` query string.
5. The `url` is the complete URL your web app needs to call to verify the user. Simply make a request to this URL, and the user will be verified.
6. At this point, you should receive a `200` OK response, confirming the user's email verification.

Note

If you encounter issues with the email verification link, such as a mismatched signature, when using a load balancer, set the `protected $proxies = '*'` in the `app/Ship/Middlewares/TrustProxies.php` or customize it according to your needs.
