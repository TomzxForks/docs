---
title: Requirements
icon: fal fa-clipboard-check
---

- Latest Laravel Long Term Support (5.5) or latest (5.6).
- PHP 7.0 or up, check the requirements for your Laravel version.
- Preferably Ubuntu. But most linux-based operating systems should work.
- Apache 2.4+ or Nginx 1.12+.
- MySQL 5.7+, MariaDB 10+ or PostgreSQL 9+.

> Please note that MySQL limits username and database length to 32,
enable in the `tenancy.php` configuration file:  `website > uuid-limit-length-to-32`
to fix this.

# Elevated database user

Tenancy requires a system connection that allows creating new databases for the
tenants. In order to do so we need to have a database user with elevated
privileges.

A user for [MariaDB, MySQL][1] or [PostgreSQL][2] would require the "GRANT OPTION".
You can either use the root user (for PostgreSQL that is user postgres) or create
your own (recommended):

For MariaDB/MySQL:
```sql
CREATE DATABASE IF NOT EXISTS tenancy;
CREATE USER IF NOT EXISTS tenancy@localhost IDENTIFIED BY 'someRandomPassword';
GRANT ALL PRIVILEGES ON *.* TO tenancy@localhost WITH GRANT OPTION;
```

For PostgreSQL:
```sql
CREATE DATABASE tenancy;
CREATE USER tenancy WITH CREATEDB CREATEROLE PASSWORD 'someRandomPassword';
GRANT ALL PRIVILEGES ON tenancy to tenancy WITH GRANT OPTION;
```

Make sure you configure this user as your system connection in your `database.php`.
Under connections:

```php
'system' => [
    'driver' => 'mysql',
    'host' => env('TENANCY_HOST', '127.0.0.1'),
    'port' => env('TENANCY_PORT', '3306'),
    'database' => env('TENANCY_DATABASE', 'tenancy'),
    'username' => env('TENANCY_USERNAME', 'tenancy'),
    'password' => env('TENANCY_PASSWORD', ''),
    'unix_socket' => env('DB_SOCKET', ''),
    'charset' => 'utf8mb4',
    'collation' => 'utf8mb4_unicode_ci',
    'prefix' => '',
    'strict' => true,
    'engine' => null,
]
```

> There is no need to configure the `tenant` connection in the `database.php`
configuration file. This connection is set up automatically at runtime. Pre-
configuring this connection may cause unwanted behaviour.

[1]: https://mariadb.com/kb/en/library/grant/#the-grant-option-privilege
[2]: https://www.postgresql.org/docs/9.6/static/sql-grant.html
