---
title: Middleware
icon: fal fa-shield-alt
excerpt: Automatic actions per tenant
---

This package offers additional automation to make your life easier. Using
middlewares, you can mutate how specific requests are handled.

# Maintenance

Set the `under_maintenance_since` property of the hostname to show a generic
Laravel maintenance page.

# Redirection

Set the `redirect_to` property to a valid URL to automatically redirect all
requests to this hostname.

# Secure

Set the `force_https` property to `true` to force all connections to this
hostname to be handled using a SSL encrypted connection.

# Abort

In case no tenant was identified the application will generate a default
not found page. You can disable this functionality globally in
the tenancy configuration file `hostname > abort-without-identified-hostname`.
