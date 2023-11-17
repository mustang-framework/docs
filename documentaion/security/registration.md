# Registration

Apiato supports two default user registration methods:

1. [Register by Credentials](https://apiato.io/docs/security/registration#register-by-credentials)
2. [Register by Social Account](https://apiato.io/docs/security/registration#register-by-social-account)

You can also extend these methods or add new ones to customize your registration process.

### Register by Credentials[​](https://apiato.io/docs/security/registration#register-by-credentials) <a href="#register-by-credentials" id="register-by-credentials"></a>

To register a new user, send a `POST` request to the `/register` endpoint.

```
api.mustang.test/v1/register
```

Tip

Don't forget to add `Accept: application/json` header to your request.

The `/register` endpoint expects a string email address and a string password field.

```
{
  "email": "gandalg@the.grey",
  "password": "password"
}
```

You should receive a response similar to the following:

```
{
  "data": {
    "object": "User",
    "id": "XbPW7awNkzl83LD6",
    "name": null,
    "email": "john@doe.com",
    "email_verified_at": null,
    "gender": null,
    "birth": null
  },
  "meta": {
    "include": [
        "roles",
        "permissions"
    ],
    "custom": []
  }
}
```

### Register by Social Account[​](https://apiato.io/docs/security/registration#register-by-social-account) <a href="#register-by-social-account" id="register-by-social-account"></a>

> (Facebook, Twitter, Google, etc...)

Checkout the [Social Authentication](https://apiato.io/docs/pacakges/social-authentication) documentation.
