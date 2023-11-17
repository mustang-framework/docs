# Authorization

Mustang provides a Role-Based Access Control (RBAC) through its Authorization Container. Behind the scenes, Mustang uses the [Laravel Permission](https://github.com/spatie/laravel-permission) package.

### How it works[​](https://apiato.io/docs/security/authorization#how-it-works) <a href="#how-it-works" id="how-it-works"></a>

Authorization in Mustang is indeed straightforward and easy. It operates by linking permissions to roles and then assigning roles to users.

To implement the authorization process, follow these steps:

1. Create Roles and Permissions
2. Attach Permissions to Roles
3. Attach Roles and/or Permissions to Users
4. Protect Endpoints with Permissions and/or Roles

To [protect your endpoints](https://apiato.io/docs/components/main-components/requests#request-properties), you have to specify the required permissions and/or roles in the `Request` class. In doing so, you can check whether the current user has the necessary access rights to reach a particular endpoint. By verifying permissions and roles at the request level, you ensure that unauthorized users are denied access before any further processing takes place.

Default Roles & Permissions

Mustang comes with some default Roles and Permissions. You can find them in `app/Containers/AppSection/Authorization/Data/Seeders`. You can use them as a starting point, or delete them and create your own.

### Code Example[​](https://apiato.io/docs/security/authorization#code-example) <a href="#code-example" id="code-example"></a>

Protecting the **delete user** endpoint with `delete-users` permission:

```
use App\Ship\Parents\Requests\Request as ParentRequest;

class DeleteUserRequest extends ParentRequest
{
    protected array $access = [
        'permissions' => 'delete-users',
        'roles' => '',
    ];

    public function authorize(): bool
    {
        return $this->check([
            'hasAccess',
        ]);
    }
}
```

Authorization failed JSON response:

```
{
  "message": "This action is unauthorized.",
  "errors": []
}
```
