# Events

Mustang events are just [Laravel Events](https://laravel.com/docs/events), and they function in the exact same way as Laravel events. However, they come with additional rules and conventions specific to Mustang.

To generate new events and listeners you may use the following interactive commands:

```
php artisan mustang:generate:event
php artisan mustang:generate:listener
```

### Rules[​](https://apiato.io/docs/components/optional-components/events#rules) <a href="#rules" id="rules"></a>

* All
  * Events MUST extend the `App\Ship\Parents\Events\Event` class.
    * The parent extension SHOULD be aliased as `ParentEvent`.
  * Listeners MUST extend the `App\Ship\Parents\Listeners\Listener` class.
    * The parent extension SHOULD be aliased as `ParentListener`.
* All container-specific
  * Events MUST be placed in the `app/Containers/{Section}/{Container}/Events` directory.
  * Listeners MUST be placed in the `app/Containers/{Section}/{Container}/Listeners` directory.
* All general
  * Events MUST be placed in the `app/Ship/Events` directory.
  * Listeners MUST be placed in the `app/Ship/Listeners` directory.
* Listeners CAN listen to all cross-container & cross-section events.
* Events & Listeners MUST be registered in the location where you intend to handle that event.
  * If you intend to handle an event in:
    * A container, the Listener MUST be registered in `App\Containers\{Section}\{Container}\Providers\EventServiceProvider` class.
    * The Ship, the Listener MUST be registered in `App\Ship\Providers\EventServiceProvider` class.

### Folder Structure[​](https://apiato.io/docs/components/optional-components/events#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

The highlighted sections showcase event & listener registration points:

```
app
├── Containers
│   └── Section
│       └── Container
│           ├── Events
│           │   ├── DemoEvent.php
│           │   └── ...
│           ├── Listeners
│           │   ├── DemoListener.php
│           │   └── ...
│           └── Providers
│               ├── EventServiceProvider.php
│               └── ...
└── Ship
    ├── Events
    │   ├── ShipDemoEvent.php
    │   └── ...
    ├── Listeners
    │   ├── ShipDemoListener.php
    │   └── ...
    └── Providers
        ├── EventServiceProvider.php
        └── ...
```

### Code Example[​](https://apiato.io/docs/components/optional-components/events#code-example) <a href="#code-example" id="code-example"></a>

Events and Listeners are defined exactly as you would define them in Laravel.

### Registering Events & Listeners[​](https://apiato.io/docs/components/optional-components/events#registering-events--listeners) <a href="#registering-events--listeners" id="registering-events--listeners"></a>

The registration of events and listeners depends on where you intend to respond to events. Listeners can be registered in both container and Ship.

#### In The Container[​](https://apiato.io/docs/components/optional-components/events#in-the-container) <a href="#in-the-container" id="in-the-container"></a>

Registering events and listeners in the container can be done by adding them to the `listen` array in the `App\Containers\{Section}\{Container}\Providers\EventServiceProvider` class.

```
use ...
use App\Ship\Parents\Providers\EventServiceProvider as ParentEventServiceProvider;

class EventServiceProvider extends ParentEventServiceProvider
{
    protected $listen = [
        OrderShipped::class => [
            SendShipmentNotification::class,
        ],
    ];
}
```

To generate an event service provider you may use the `mustang:generate:provider` interactive command:

```
php artisan mustang:generate:provider
```

Remember to also register the `EventServiceProvider` in the container's `MainServiceProvider`:

```
use ...
use App\Ship\Parents\Providers\MainServiceProvider as ParentMainServiceProvider;

class MainServiceProvider extends ParentMainServiceProvider
{
    protected array $serviceProviders = [
        // ... Other service providers
        EventServiceProvider::class,
    ];
}
```

#### In The Ship[​](https://apiato.io/docs/components/optional-components/events#in-the-ship) <a href="#in-the-ship" id="in-the-ship"></a>

Registering events and listeners in the Ship can be done by adding them to the `listen` array in the `App\Ship\Providers\EventServiceProvider` class.

### Events & Listeners Registration Flow[​](https://apiato.io/docs/components/optional-components/events#events--listeners-registration-flow) <a href="#events--listeners-registration-flow" id="events--listeners-registration-flow"></a>

If you are manually registering events and listeners and wish to understand the registration process, here is a breakdown of the registration flow.

Consider the following folder structure:

```
app
├── Containers
│   └── Section
│       └── Container
│           ├── Events
│           │   ├── DemoEvent.php ────►─┐
│           │   └── ...                 │
│           ├── Listeners               │
│           │   ├── DemoListener.php ─►─┤
│           │   └── ...                 │
│           └── Providers               ▼
│               ├── EventServiceProvider.php ─────────►─────────┐
│               ├── MainServiceProvider.php ◄───registered─in─◄─┘
│               └── ...
└── Ship
    ├── Events
    │   ├── ShipDemoEvent.php ──►─┐
    │   └── ...                   │
    ├── Listeners                 │
    │   ├── ShipDemoListener.php ►┤
    │   └── ...                   │
    └── Providers                 ▼
        ├── EventServiceProvider.php ─────────►─────────┐
        ├── ShipProvider.php        ◄───registered─in─◄─┘
        └── ...
```

The following diagram illustrates the registration flow of events and listeners in the above folder structure:

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
