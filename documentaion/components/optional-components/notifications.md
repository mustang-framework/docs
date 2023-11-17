# Notifications

Mustang notifications are just [Laravel Notifications](https://laravel.com/docs/notifications), and they function in the exact same way as Laravel notifications. However, they come with additional rules and conventions specific to Mustang.

To generate new notifications you may use the `mustang:generate:notification` interactive command:

```
php artisan mustang:generate:notification
```

### Rules[​](https://apiato.io/docs/components/optional-components/notifications#rules) <a href="#rules" id="rules"></a>

* All Notifications MUST extend the `App\Ship\Parents\Notifications\Notification` class.
* Containers MAY or MAY NOT have one or more Notification.
* Ship MAY contain Application general Notifications.

### Folder Structure[​](https://apiato.io/docs/components/optional-components/notifications#folder-structure) <a href="#folder-structure" id="folder-structure"></a>

```
app
├── Containers
│   └── Section
│       └── Container
│           └── Notifications
│               ├── UserRegisteredNotification.php
│               └── ...
└── Ship
    └── Notifications
        ├── SystemFailureNotification.php
        └── ...
```

### Code Example[​](https://apiato.io/docs/components/optional-components/notifications#code-example) <a href="#code-example" id="code-example"></a>

Notifications are defined exactly as you would define them in Laravel.

### Default Notification Channels[​](https://apiato.io/docs/components/optional-components/notifications#default-notification-channels) <a href="#default-notification-channels" id="default-notification-channels"></a>

In Mustang, you have the ability to set default notification channels that will be used for all your notifications. This configuration can be found in the `notification.php` file located in the `app/Ship/Configs` directory.

By specifying default notification channels in this configuration file, you establish a consistent choice of channels for your notifications throughout the application.

However, should you need to customize the notification channels for specific notification classes, or if you prefer to define channels on a per-notification basis, you can do so by overriding the `via` method within the respective notification class. This allows you to tailor the channel selection based on individual notification requirements or use cases.

### Database Notifications Migration[​](https://apiato.io/docs/components/optional-components/notifications#database-notifications-migration) <a href="#database-notifications-migration" id="database-notifications-migration"></a>

When setting up database notifications, the usual process involves generating a migration for notifications using the `php artisan notifications:table` command and then run the migration.

However, Mustang already provides a migration file named `xxxx_xx_xx_xxxxxx_create_notifications_table.php` in the `app/Ship/Migrations` directory. As a result, you don't need to manually generate the migration file. You can directly run the migrations using the `php artisan migrate` command, and the notifications table will be created for you.
