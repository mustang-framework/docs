# Installation

### Your First Mustang Project

Before creating your first Mustang project, you should ensure that your local machine has PHP and [Composer](https://getcomposer.org/) installed. If you are developing on macOS, PHP and Composer can be installed via [Homebrew](https://brew.sh/).

After you have installed PHP and Composer, you may create a new Mustang project via the Composer create-project command:

```
composer create-project mustang/mustang example-app
```

### Development Environment Setup

You can run Mustang in any environment that you can run Laravel.

:::tip Visit [Laravel Installation](https://laravel.com/docs/installation#laravel-and-docker) for more details. :::

### Initial Configuration

All the configuration files for the Laravel framework are stored in the `config` folder and all the configuration files for the Mustang framework are stored in `app/Ship/Configs`. Each option is documented, so feel free to look through the files and get familiar with the options available to you.

Mustang needs almost no additional configuration out of the box. You are free to get started developing! However, you may wish to review the `app/Ship/Configs/mustang.php` file and its documentation. It contains several options that you may wish to change according to your application.

#### Environment Based Configuration

Since many of Mustang configuration option values may vary depending on whether your application is running on your local machine or on a production web server, many important configuration values are defined using the `.env` file that exists at the root of your application.

Your `.env` file should not be committed to your application's source control, since each developer/server using your application could require a different environment configuration. Furthermore, this would be a security risk in the event an intruder gains access to your source control repository since any sensitive credentials would get exposed.

:::info For more information about the `.env` file and environment-based configuration, check out the Laravel full [configuration documentation](https://laravel.com/docs/configuration). :::

#### Databases & Migrations

Now that you have created your Mustang application, you probably want to store some data in a database. By default, your application's `.env` configuration file specifies that Mustang will be interacting with a MySQL database and will access the database at `127.0.0.1`. If you are developing on macOS and need to install MySQL, Postgres, or Redis locally, you may find it convenient to utilize `DBngin`.

If you do not want to install MySQL or Postgres on your local machine, you can always use a [SQLite](https://www.sqlite.org/index.html) database. SQLite is a small, fast, self-contained database engine. To get started, create a SQLite database by creating an empty SQLite file. Typically, this file will exist within the database directory of your Mustang application:

```
touch database/database.sqlite
```

Next, update your `.env` configuration file to use Laravel SQLite database driver. You may remove the other database configuration options:

```diff
+ DB_CONNECTION=sqlite
- DB_CONNECTION=mysql
- DB_HOST=127.0.0.1
- DB_PORT=3306
- DB_DATABASE=homestead
- DB_USERNAME=homestead
- DB_PASSWORD=secret
```

Once you have configured your SQLite database, you may run your application's [database migrations](https://laravel.com/docs/migrations), which will create your application's database tables:

```
php artisan migrate
```

**Default User, Roles & Permissions**

Mustang includes a default (Super Admin) user along with predefined roles and permissions. To populate the database with these default values, you may execute the `db:seed` command.

```
php artisan db:seed
```

**Default User Credentials:**

* email: {variables.defaults.admin.email}
* password: {variables.defaults.admin.password}

:::tip You can create a new admin user using the `mustang:create:admin` interactive command:

```
php artisan mustang:create:admin
```

:::

#### Authentication Configuration

Visit Authentication for more details.

#### Directory Configuration

Mustang should always be served out of the root of the "web directory" configured for your web server. You should not attempt to serve a Mustang application out of a subdirectory of the "web directory". Attempting to do so could expose sensitive files present within your application.

#### Subdomain and API Version Prefix <a href="#subdomain-and-api-version-prefix" id="subdomain-and-api-version-prefix"></a>

By default, Mustang uses `api` as a subdomain for all endpoints and adds only the API version as a prefix, resulting in URLs like `api.mustang.test/v1`. However, you can change this behavior.

For example, if you'd like to achieve urls like `mustang.test/api/`, follow these steps:

1. Open your `.env` file and modify the API domain by updating the `API_URL` value from `http://api.mustang.test` to `http://mustang.test` to remove the subdomain.
2. In the `app/Ship/Configs/mustang.php` configuration file:
   * Set the `prefix` to `api/`.
   * Set `enable_version_prefix` to `false`.

### Generating API Documentation

Mustang includes a convenient Documentation Generator package that utilizes [ApiDocJs](https://apidocjs.com/) for API documentation generation.

To get started, install ApiDocJs using NPM or your preferred dependency manager:

```
npm install
```

Next, generate the API documentation by executing the following command:

```
php artisan mustang:apidoc
```

### Let's Play

To witness Mustang in action, assuming you are using the default Subdomain and API Version Prefix configuration, you should be able to access the following URLs:

* http://mustang.test -> You should see an HTML page displaying the text Mustang in the center.
* http://api.mustang.test -> You should receive a JSON response like this:

```json
["Welcome to Mustang"]
```

Open your HTTP client and call:

http://api.mustang.test/ You should see a JSON response with the message: `"Welcome to Mustang."` http://api.mustang.test/v1 You should see a JSON response with the message: `"Welcome to` Mustang `(API V1)."`

### Next Steps <a href="#next-steps" id="next-steps"></a>

Now that you have created your Mustang project, you may be wondering what to learn next. If you're looking for a place to start, you should check out the following resources:

* Mustang Architecture
* Customized Laravel Components
* Framework Features
* Container Installer
