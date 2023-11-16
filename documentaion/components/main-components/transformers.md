# Transformers

Transformers, often referred to as Response Transformers, serve a similar purpose to Views, but specifically for JSON responses. While Views are responsible for presenting data in HTML format, Transformers take data and format it into JSON representation.

For more detailed information about transformers and their usage, you can refer to the [official documentation of Fractal](https://fractal.thephpleague.com/transformers/), which is the underlying library used for handling transformations in Mustang.

To generate new transformers you may use the `mustang:generate:transformer` interactive command:

```
php artisan mustang:generate:transformer
```

### Definition & Principles[​](https://apiato.io/docs/components/main-components/transformers#definition--principles) <a href="#definition--principles" id="definition--principles"></a>

Read [**Porto SAP Documentation (#Transformers)**](https://github.com/Mahmoudz/Porto#definitions--principles).

### Rules[​](https://apiato.io/docs/components/main-components/transformers#rules) <a href="#rules" id="rules"></a>

* All Transformers:
  * MUST be placed in the `app/Containers/{Section}/{Container}/UI/API/Transformers` directory.
  * MUST extend the `App\Ship\Parents\Transformers\Transformer` class.
    * The parent extension SHOULD be aliased as `ParentTransformer`.
  * MUST have a public `transform` method returning an array.

### Folder Structure[​](https://apiato.io/docs/components/main-components/transformers#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

```
app
└── Containers
    └── Section
        └── Container
            └── UI
                └── API
                    └── Transformers
                        ├── TransformerA.php
                        ├── TransformerB.php
                        └── ...
```

### Code Example[​](https://apiato.io/docs/components/main-components/transformers#code-example) <a href="#code-example" id="code-example"></a>

```
use ...
use App\Ship\Parents\Transformers\Transformer as ParentTransformer;

class UserTransformer extends ParentTransformer
{
    protected $availableIncludes = [];

    protected $defaultIncludes = [];

    public function transform(User $user)
    {
        return [
            'object' => $user->getResourceKey(),
            'id' => $user->getHashedKey(),
            'name' => $user->name,
            // ...
        ];
    }
}
```

### Including Relationships[​](https://apiato.io/docs/components/main-components/transformers#including-relationships) <a href="#including-relationships" id="including-relationships"></a>

You can include model relationships for complex data structures using the `include` query parameter. These relationships can be included in the response either [per API consumer request](https://apiato.io/docs/components/main-components/transformers#include-per-api-consumer-request) or [by default](https://apiato.io/docs/components/main-components/transformers#include-by-default). The `include` parameter can be used on any endpoint that has [relationships defined](https://apiato.io/docs/components/main-components/transformers#defining-relationships) in its transformer.

#### Defining Relationships[​](https://apiato.io/docs/components/main-components/transformers#defining-relationships) <a href="#defining-relationships" id="defining-relationships"></a>

To define relationships in the transformer, follow these two steps:

1. Define the relationship method in the transformer.
2. Add the relationship to the `availableIncludes` or `defaultIncludes` property.

* `availableIncludes` can be included in the response per API consumer request.
* `defaultIncludes` are included in the response by default.

Note

Any relationships not defined in the `availableIncludes` or `defaultIncludes` properties will be ignored.

The relationship method should be named `include{RelationshipName}` and return a Fractal `Item` or `Collection` object. The `include{RelationshipName}` method will be called automatically by the Transformer when the relationship is requested.

For example, let's assume we have a `User` model with a `roles` relationship. The `UserTransformer` would have an `includeRoles` method that returns a `Collection` object.

```
public function includeRoles(User $user): Collection
{
    return $this->collection($user->roles, new RoleTransformer());
}
```

Now, the `roles` relationship can be included in the response by adding it to the `availableIncludes` or `defaultIncludes` property.

```
protected array $availableIncludes = [
    'roles',
];

// or

protected array $defaultIncludes = [
    'roles',
];
```

#### Include Per API Consumer Request[​](https://apiato.io/docs/components/main-components/transformers#include-per-api-consumer-request) <a href="#include-per-api-consumer-request" id="include-per-api-consumer-request"></a>

In cases where you have multiple relationships for a model, such as `User` with `Roles` and `Avatar` relationships, you can include specific relationships in the response based on the API consumer's request. To enable this, you add the desired relationships to the `availableIncludes` property in the transformer and create corresponding methods for each relationship in the transformer to specify how to include that data.

Here's an example using a `UserTransformer` with `roles` and `avatar` relationships added to the `availableIncludes` property:

```
protected array $availableIncludes = [
    'roles',
    'avatar',
];

public function includeRoles(User $user): Collection
{
    return $this->collection($user->roles, new RoleTransformer());
}

public function includeAvatar(User $user): Item
{
    return $this->item($user->avatar, new AvatarTransformer());
}
```

To request the `roles` data along with the `User` resource, you can pass the `include=roles` query parameter with the request:

```
api.mustang.test/v1/users?include=roles
```

This will include the `roles` data in the response:

```
{
  "data": [
    {
      "object": "User",
      "id": "0one37vjk49rp5ym",
      "roles": [
        {
          "object": "Role",
          "id": "bmo7y84xpgeza06k"
        },
        // ...
      ]
    },
    // ...
  ]
}
```

You can also include multiple relationships by separating them with a comma:

```
api.mustang.test/v1/users?include=roles,avatar
```

This includes both the `roles` and `avatar` relationships in the response.

Nested includes are also possible. If, for instance, the `Avatar` model has a relationship with an `Image` object, you can request nested includes using dot notation:

```
api.mustang.test/v1/users?include=avatar,avatar.image
```

This includes the `Avatar` relationship with the `Image` nested under it in the response.

It's important to note that for nested includes, the nested relationship must also be defined. In this example, the `AvatarTransformer` would need to have an `includeImage` method defined and the `image` relationship added to it's `availableIncludes` property.

By defining the `availableIncludes` and implementing the corresponding `include{RelationshipName}` methods, you allow API consumers to specify which related data they want in the response, enhancing the flexibility and efficiency of your API.

#### Include By Default[​](https://apiato.io/docs/components/main-components/transformers#include-by-default) <a href="#include-by-default" id="include-by-default"></a>

To automatically include a relationship in every response generated by the transformer, you can define the relationship in the transformer's `defaultIncludes` property. This means that the specified relationship will be included by default without the need for API consumers to request it explicitly.

Here's an example using a `UserTransformer` where the `avatar` relationship is defined as a default include:

```
protected array $defaultIncludes = [
    'avatar',
];

public function includeAvatar(User $user): Item
{
    return $this->item($user->avatar, new AvatarTransformer());
}
```

By setting the default includes in this manner, the `avatar` relationship will automatically be included in every response created by this transformer. This can simplify responses and reduce the need for additional API requests for related data, ultimately enhancing the efficiency and usability of your API.

### Resource Key[​](https://apiato.io/docs/components/main-components/transformers#resource-key) <a href="#resource-key" id="resource-key"></a>

The transformer allows appending a resource key to the transformed resource. This is useful when you want to have a consistent response payload format for all your resources.

#### Setting the Resource Key[​](https://apiato.io/docs/components/main-components/transformers#setting-the-resource-key) <a href="#setting-the-resource-key" id="setting-the-resource-key"></a>

You can set the resource key in your response payload in two ways:

**Via Model**[**​**](https://apiato.io/docs/components/main-components/transformers#via-model)

Specify the resource key on the respective model by setting the `resourceKey` property:

```
class User extends ParentUserModel
{
    protected $resourceKey = 'User';
}
```

**Via Controller**[**​**](https://apiato.io/docs/components/main-components/transformers#via-controller)

Manually set the resource key using the `resourceKey` parameter in the controller's `transform` method:

```
$this->transform($model, ModelTransformer::class, resourceKey: 'User');
```

Note

It's important to note that setting the `resourceKey` using the `transform` method will only impact the `top level` resource key and will not affect the resource keys of `included` resources.

#### Getting the Resource Key[​](https://apiato.io/docs/components/main-components/transformers#getting-the-resource-key) <a href="#getting-the-resource-key" id="getting-the-resource-key"></a>

Retrieve the resource key from the model by calling the `getResourceKey` method.

If no `resourceKey` is defined on the model, the `getResourceKey` method will return the short class name of the model. For instance, if no resource key is defined for `App\Containers\AppSection\User\Models\User::class`, the default resource key will be `User`.

**Transformer Example**[**​**](https://apiato.io/docs/components/main-components/transformers#transformer-example)

```
class UserTransformer extends ParentTransformer
{
    // ...
    public function transform(User $user)
    {
        return [
            'object' => $user->getResourceKey(), // <-- here
            'id' => $user->getHashedKey(),
            'name' => $user->name,
            // ...
        ];
    }
    // ...
}
```

**Response Example**[**​**](https://apiato.io/docs/components/main-components/transformers#response-example)

```
{
  "data": {
    "object": "User", // <-- ResourceKey
    "id": "XbPW7awNkzl83LD6",
    "name": "Mohammad Alavi"
  }
}
```

### Helper Methods[​](https://apiato.io/docs/components/main-components/transformers#helper-methods) <a href="#helper-methods" id="helper-methods"></a>

#### ifAdmin[​](https://apiato.io/docs/components/main-components/transformers#ifadmin) <a href="#ifadmin" id="ifadmin"></a>

The `ifAdmin` method is used to merge the normal client response with additional or modified results intended for admin users.

```
public function transform(Model $model)
{
    $response = [
        'object' => $model->getResourceKey(),
        'id' => $model->getHashedKey(),
        'another_field' => $model->anotherField,
    ];

    return $this->ifAdmin([
        'real_id' => $model->id,
        'created_at' => $model->created_at,
        'updated_at' => $model->updated_at,
        'readable_created_at' => $model->created_at->diffForHumans(),
        'readable_updated_at' => $model->updated_at->diffForHumans(),
    ], $response);
}
```

#### nullableItem[​](https://apiato.io/docs/components/main-components/transformers#nullableitem) <a href="#nullableitem" id="nullableitem"></a>

The `nullableItem` method returns an item if the model has a specific relationship, otherwise, it returns `null`.

```
use League\Fractal\Resource\Item;
use League\Fractal\Resource\Primitive;

public function includeRelation(Model $model): Primitive|Item
{
    return $this->nullableItem($model->relation, new RelationTransformer();
}
```

If `$model->relation` is not null (meaning it has a related model), the method returns an item formatted using the specified transformer. Otherwise, it returns `null`.

The `nullableItem` method is a shortcut for the following code:

```
use League\Fractal\Resource\Item;
use League\Fractal\Resource\Primitive;

public function includeRelation(Model $model): Primitive|Item
{
    return $model->relation ? $this->item($model->relation, new RelationTransformer()) : $this->primitive(null)
}
```

### Response Payload[​](https://apiato.io/docs/components/main-components/transformers#response-payload) <a href="#response-payload" id="response-payload"></a>

You have the flexibility to define your own custom response payload or utilize one of the supported serializers. Serializer classes let you switch between various output formats with minimal effect on your Transformers.

Current [supported serializers](https://fractal.thephpleague.com/serializers/):

* `ArraySerializer`
* `DataArraySerializer`
* `JsonApiSerializer`

To modify the default Fractal serializer, access the `app/Ship/Configs/fractal.php` configuration file and update the `default_serializer` setting to your preferred serializer.

By default, Mustang uses `DataArraySerializer`. This serializer is not to everyone’s tastes, because it adds a `data` namespace to the output. A very basic response of the `DataArraySerializer` will look like this:

```
{
  "data": {
    "object": "User",
    "id": "XbPW7awNkzl83LD6",
    "name": "Mohammad Alavi"
  }
}
```

The `DataArraySerializer` is handy because it allows space for `meta` data (like pagination, or totals) in both Items and Collections.

```
{
  "data": [ ... ],
  "meta": {
    "include": [
      "xxx",
      "yyy"
    ],
    "custom": [],
    "pagination": {
      "total": 999,
      "count": 999,
      "per_page": 999,
      "current_page": 999,
      "total_pages": 999,
      "links": {
        "next": "http://api.mustang.test/v1/accounts?page=999"
      }
    }
  }
}
```

Further Reading

For more detailed information, please refer to [Fractal](https://fractal.thephpleague.com/transformers/) and [Laravel Fractal Wrapper](https://github.com/spatie/laravel-fractal) documentations.
