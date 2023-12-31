# Porto

Porto is a modern software architectural pattern that offers developers a comprehensive set of guidelines, principles, and patterns to organize their code in a highly maintainable and reusable way. The primary goal of Porto is to help developers create software that is scalable, flexible, and easy to maintain over time.

### Layers[​](https://apiato.io/docs/architecture-concepts/porto#layers) <a href="#layers" id="layers"></a>

Porto's architecture is based on two layers: Containers and Ship.

#### Containers[​](https://apiato.io/docs/architecture-concepts/porto#containers) <a href="#containers" id="containers"></a>

The Containers layer encompasses all the application's business logic code and consists of two primary concepts:

* Section
* Container

**Section**[**​**](https://apiato.io/docs/architecture-concepts/porto#section)

A Section refers to a collection of related Containers. These Containers can represent various entities such as services (either micro or larger in scale) or subsystems within the main system.

{% hint style="info" %}
A Section is not allowed to directly communicate with another Section, except via Events or Commands.
{% endhint %}

**Container**[**​**](https://apiato.io/docs/architecture-concepts/porto#container)

A Container represents a cohesive set of related functionalities. It can be a specific feature or a wrapper around a RESTful API resource.

{% hint style="info" %}
A Container is allowed to depend on other Containers in the same Section.
{% endhint %}

#### Ship[​](https://apiato.io/docs/architecture-concepts/porto#ship) <a href="#ship" id="ship"></a>

The Ship layer contains the infrastructure code, which consists of shared code utilized by all Containers.

### Typical Project Structure[​](https://apiato.io/docs/architecture-concepts/porto#typical-project-structure) <a href="#typical-project-structure" id="typical-project-structure"></a>

```
app
├── Containers
│   ├── Section
│   │   └── Container
│   │       ├── Actions
│   │       ├── Configs
│   │       ├── Data
│   │       │   ├── Factories
│   │       │   ├── Migrations
│   │       │   ├── Repositories
│   │       │   └── Seeders
│   │       ├── Mails
│   │       │   └── Templates
│   │       ├── Middlewares
│   │       ├── Models
│   │       ├── Notifications
│   │       ├── Providers
│   │       ├── Tasks
│   │       ├── Tests
│   │       │   └── Unit
│   │       ├── Traits
│   │       └── UI
│   │           ├── API
│   │           │   ├── Controllers
│   │           │   ├── Requests
│   │           │   ├── Routes
│   │           │   ├── Tests
│   │           │   │   └── Functional
│   │           │   └── Transformers
│   │           └── WEB
│   │           │   ├── Controllers
│   │           │   ├── Requests
│   │           │   ├── Routes
│   │           │   └── Views
│   │           └── CLI
│   │               └── Commands
│   └── Vendor `// All installed and reusable Containers`
│       ├── ContainerA
│       └── ContainerB
└── Ship `// All shared code between all Containers`
    ├── Broadcasts
    ├── Commands
    ├── Configs
    ├── Contracts
    ├── Criterias
    ├── Events
    ├── Exceptions
    ├── Generators
    ├── Helpers
    ├── Kernels
    ├── Listeners
    ├── Mails
    ├── Middlewares
    ├── Migrations
    ├── Notifications
    ├── Parents
    ├── Providers
    ├── Seeders
    └── Tests
```

### Default Sections[​](https://apiato.io/docs/architecture-concepts/porto#default-sections) <a href="#default-sections" id="default-sections"></a>

Mustang ships with two default Sections:

* **AppSection**: contains all the default Containers.
* **Vendor**: contains all the installed and reusable Containers.

{% hint style="success" %}
The **Vendor** section is a special Section within the Containers layer that holds installed and reusable Containers. It serves a similar purpose as the vendor folder located at the root. Any Section is permitted to depend on the Vendor Section, allowing for the utilization of its Containers.

Read more about the [Container Installer](https://apiato.io/docs/pacakges/) to learn how to install Vendor Containers.
{% endhint %}
