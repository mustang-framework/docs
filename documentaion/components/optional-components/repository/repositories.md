# Repositories

Mustang provides a powerful repository pattern implementation based on the [L5 Repository](https://github.com/andersao/l5-repository) package.

> To prevent overlap with the L5 Repository documentation, this page exclusively delves into Mustang distinct features and configurations, offering only a limited set of examples. To learn more about the L5 Repository package, please refer to its [documentation](https://github.com/andersao/l5-repository).

Repositories play a crucial role in software architecture by separating and abstracting the data layer from the business logic. They serve as an essential architectural pattern to promote code separation, flexibility, and maintainability in software development. Repositories help create clean and organized codebases by abstracting the intricacies of data access and manipulation from the core business logic.

To generate new repositories you may use the `mustang:generate:repository` interactive command:

```
php artisan mustang:generate:repository
```

You can also generate a model and its repository at the same time by using the `mustang:generate:model` interactive command:

```
php artisan mustang:generate:model
```

### Rules[​](https://apiato.io/docs/components/optional-components/repository/repositories#rules) <a href="#rules" id="rules"></a>

* All Repositories:
  * MUST be placed in the `app/Containers/{section}/{container}/Data/Repositories` directory.
  * MUST extend the `App\Ship\Parents\Repositories\Repository` class.
    * The parent extension SHOULD be aliased as `ParentRepository`.
  * SHOULD be named after the model they are associated with, followed by the `Repository` suffix. For instance, `UserRepository.php`.
* A Model MUST always get accessed through its Repository.
* Every Model MUST have a Repository.

### Folder Structure[​](https://apiato.io/docs/components/optional-components/repository/repositories#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

```
app
└── Containers
    └── Section
        └── Container
            └── Data
                └── Repositories
                    ├── UserRepository.php
                    └── ...
```

### Code Example[​](https://apiato.io/docs/components/optional-components/repository/repositories#code-example) <a href="#code-example" id="code-example"></a>

```
use App\Ship\Parents\Repositories\Repository as ParentRepository;

class UserRepository extends ParentRepository
{
    protected $fieldSearchable = [
        'id' => '=',
        'name' => 'like',
        'email' => '=',
        'email_verified_at' => '=',
        'created_at' => '=',
    ];
}
```

### Configuration[​](https://apiato.io/docs/components/optional-components/repository/repositories#configuration) <a href="#configuration" id="configuration"></a>

All the configuration options for the this feature are located in the `app/Ship/Configs/repository.php` config file.

### Linking Repositories & Models[​](https://apiato.io/docs/components/optional-components/repository/repositories#linking-repositories--models) <a href="#linking-repositories--models" id="linking-repositories--models"></a>

Once you have created a repository, you need to link it to its corresponding model. If you have followed the repository naming [rules](https://apiato.io/docs/components/optional-components/repository/repositories#rules) outlined above, Mustang will automatically link your repositories to their corresponding models.

However, if you have not followed the rules, and your repository name differs from the model name, you must implement the `model` method in the repository.

This method allows you to specify the correct model class that should be used for the repository operations.

```
use App\Ship\Parents\Repositories\Repository as ParentRepository;

class DemoRepository extends ParentRepository
{
    // ...
    
    public function model(): string
    {
        return AnotherDemo::class;
    }
}
```

#### Repositories & Models Auto-Linking[​](https://apiato.io/docs/components/optional-components/repository/repositories#repositories--models-auto-linking) <a href="#repositories--models-auto-linking" id="repositories--models-auto-linking"></a>

Mustang offers a repository auto-linking feature that eliminates the need for manual linking of repositories & models. This automatic linking process relies on adhering to standard Mustang naming conventions for repositories.

By following the repository naming [rules](https://apiato.io/docs/components/optional-components/repository/repositories#rules) outlined above, you allow Mustang to automatically link your repositories to their corresponding models.

To summarize:

* Repositories must be stored within the `app/Containers/{section}/{container}/Data/Repositories` directory.
* The repository name should mirror the corresponding model's name while appending a `Repository` suffix. For instance, a `User` model corresponds to a `UserRepository` repository class.

### Pagination[​](https://apiato.io/docs/components/optional-components/repository/repositories#pagination) <a href="#pagination" id="pagination"></a>

Pagination is automatically applied when you use the `paginate` method with a model repository:

```
{
    $this->userRepository->paginate();
}
```

To move between pages of results, you can use the `page` query parameter like this:

```
api.mustang.test/v1/users?page=200
```

You can use the `page` parameter in any `GET` request that lists records.

Whenever a request returns paginated results, you'll find information about the pagination in the **meta** section of the response, which tells you things like the total number of records, the number on the current page, and more.

```
"data": [...],
"meta": {
    "pagination": {
        "total": 2000,
        "count": 30,
        "per_page": 10,
        "current_page": 20,
        "total_pages": 200,
        "links": []
    }
}
```

#### Limiting Results Per Page[​](https://apiato.io/docs/components/optional-components/repository/repositories#limiting-results-per-page) <a href="#limiting-results-per-page" id="limiting-results-per-page"></a>

You can control the number of results displayed on a single page using the `limit` parameter.

By default, the pagination limit is set to 10. If you don't specify the `?limit=` parameter in your request, the response will contain 10 records per page.

You can adjust the default pagination limit in the `.env` file:

```
PAGINATION_LIMIT_DEFAULT=10
```

For instance, if you want to receive 100 resources per page, add the `?limit=100` query parameter to your request:

```
api.mustang.test/v1/users?limit=100
```

This will return 100 resources within a single page of results. You can also combine the `limit` and `page` query parameters to access the next 100 resources:

```
api.mustang.test/v1/users?limit=100&page=2
```

#### Maximum Pagination Limit[​](https://apiato.io/docs/components/optional-components/repository/repositories#maximum-pagination-limit) <a href="#maximum-pagination-limit" id="maximum-pagination-limit"></a>

You can also set the maximum number of resources that can be returned in a single page by setting the `maxPaginationLimit` property in your repository class.

For example, to set the maximum number of resources to 20, you can do the following:

```
protected $maxPaginationLimit = 20;
```

#### Disabling Pagination[​](https://apiato.io/docs/components/optional-components/repository/repositories#disabling-pagination) <a href="#disabling-pagination" id="disabling-pagination"></a>

You can allow clients to request all data that matches their criteria and disable pagination, which can be applied either project-wide or on a per-repository basis. This enables a request to retrieve all matching data by specifying `limit=0`.

To retrieve all matching entities in a single page, you can use the following query parameter:

```
api.mustang.test/v1/users?limit=0
```

**Project-Wide**[**​**](https://apiato.io/docs/components/optional-components/repository/repositories#project-wide)

To allow disabling pagination for the entire project, set `PAGINATION_SKIP=true` in the `.env` file.

**Per Repository**[**​**](https://apiato.io/docs/components/optional-components/repository/repositories#per-repository)

If you wish to allow disabling pagination for a specific repository, set the `$allowDisablePagination` property in your repository class.

Note

Per-repository configurations take precedence and override the global settings.

### RequestCriteria[​](https://apiato.io/docs/components/optional-components/repository/repositories#requestcriteria) <a href="#requestcriteria" id="requestcriteria"></a>

RequestCriteria is a standard Criteria implementation that enables filters to be applied to the repository based on parameters sent in the request. It allows for dynamic searches, data filtering, and query customization.

To utilize RequestCriteria, you need to apply it to the repository. Mustang provides two methods, `addRequestCriteria` and `removeRequestCriteria`, which are available on all repositories.

Here's an example of how to use RequestCriteria:

```
use App\Containers\AppSection\User\Data\Repositories\UserRepository;
use App\Ship\Parents\Tasks\Task as ParentTask;

class GetAllUsersTask extends ParentTask
{
    public function __construct(
        protected readonly UserRepository $repository
    ) {
    }

    public function run(): mixed
    {
        // $this->repository->removeRequestCriteria();
        return $this->repository->addRequestCriteria()->paginate();
    }
}
```

It's important to note that using the `removeRequestCriteria` method is only relevant if you have previously applied `RequestCriteria`. If it hasn't been applied, there is no need to remove it, as RequestCriteria is not automatically applied unless explicitly used.

#### Sorting & Ordering[​](https://apiato.io/docs/components/optional-components/repository/repositories#sorting--ordering) <a href="#sorting--ordering" id="sorting--ordering"></a>

You can use the `orderBy` and `sortedBy` parameters to sort the data in the response.

The `sortedBy` parameter is typically employed in conjunction with the `orderBy` parameter.

By default, the `orderBy` parameter sorts the data in **Ascending** order. To sort the data in **Descending** order, you can include `sortedBy=desc`.

For example:

```
?orderBy=id&sortedBy=asc
```

```
?orderBy=created_at&sortedBy=desc
?orderBy=name&sortedBy=asc
```

You can use the following values for the `sortedBy` parameter:

* `asc` for Ascending.
* `desc` for Descending.

#### Searching[​](https://apiato.io/docs/components/optional-components/repository/repositories#searching) <a href="#searching" id="searching"></a>

> There is much more to searching than what is covered here. Please refer to the [L5 Repository documentation](https://github.com/andersao/l5-repository) for more details.

When the RequestCriteria is enabled for a specific route, you can use the `search` parameter in `GET` HTTP requests on that route to perform searches.

To make the search feature work, you need to specify which fields from the model should be searchable. In your repository, set the `fieldSearchable` field with the names of the fields you want to make searchable or specify a relation to the fields.

Here's an example:

```
protected $fieldSearchable = [
    'name',
    'email',
    'product.name'
];
```

You can also set the type of condition that will be used to perform the query. The default condition is "=":

```
protected $fieldSearchable = [
    'name' => 'like',
    'email', // Default Condition "="
    'your_field' => 'condition'
];
```

You can use the `search` parameter in various ways:

```
?search=John
?search=name:John
?search=name:John%20Doe
```

> Replace spaces with `%20` in the URL (e.g., `search=keyword%20here`).

**Searching with Hashed IDs**[**​**](https://apiato.io/docs/components/optional-components/repository/repositories#searching-with-hashed-ids)

If you have [Hash ID](https://apiato.io/docs/security/hash-id) enabled and want to search a hashed field, like a user ID, you need to instruct the `RequestCriteria` to decode it before performing the search.

For example, if you have a search query like this:

```
`?search=id:XbPW7awNkzl83LD6;parent_id:aYOxlpzRMwrX3gD7;some_hashed_id:NxOpZowo9GmjKqdR`
```

You should update your `addRequestCriteria` method as follows:

```
$this->repository->addRequestCriteria(['id', 'parent_id', 'some_hashed_id'])->all();
```

#### Filtering[​](https://apiato.io/docs/components/optional-components/repository/repositories#filtering) <a href="#filtering" id="filtering"></a>

The `filter` parameter can be used in any HTTP request to control the response size by specifying the data you want in the response.

For example, to retrieve only the `id` and `status` fields from a Model, you can use the `filter` parameter like this:

```
api.mustang.test/v1/users?filter=id;status
```

The response will include only the specified fields, as shown in the example response:

```
{
  "data": [
    {
      "id": "0one37vjk49rp5ym",
      "status": "approved"
    }
  ]
}
```

It's important to note that the transformer used to format the data is also filtered. This means that only the fields specified in the filter are present, and all other fields are excluded. This filtering also applies to all [included relationships](https://apiato.io/docs/components/main-components/transformers#including-relationships) of the object.

```
{
  "data": [
    {
      "id": "0one37vjk49rp5ym",
      "status": "approved",
      "products": {
        "data": [
          {
            "id": "bmo7y84xpgeza06k",
            "status": "pending"
          },
          {
            "id": "o0wzxbg0q4k7jp9d",
            "status": "fulfilled"
          }
        ]
      },
      "recipients": {
        "data": [
          {
            "id": "r6lbekg8rv5ozyad"
          }
        ]
      },
      "store": {
        "data": {
          "id": "r6lbekg8rv5ozyad"
        }
      }
    }
  ]
}
```

### Caching[​](https://apiato.io/docs/components/optional-components/repository/repositories#caching) <a href="#caching" id="caching"></a>

L5 Repository supports caching of queries. This feature is `disabled` by default. You can enable caching in the `app/Ship/Configs/repository.php` config or the `.env` file.

```
'cache' => [
    'enabled' => true,
],
```

```
ELOQUENT_QUERY_CACHE=true
```

Note

As per the L5 Repository documentation, you need to implement the `CacheableRepository` interface and use the `CacheableRepositoryTrait` trait in your repository class to enable caching. However, Mustang already implements these two requirements in the parent `Repository` class,

#### Skipping Cache[​](https://apiato.io/docs/components/optional-components/repository/repositories#skipping-cache) <a href="#skipping-cache" id="skipping-cache"></a>

You can skip the cache for a specific request by adding the `?skipCache=true` query parameter to the request URL.

```
?skipCache=true
```

> It's not recommended to skip the cache, but it's useful for testing purposes.

\
