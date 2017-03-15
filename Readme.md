Inwendo Auth User Bundle
========================

Installation
------------

### Step 1: Download the Bundle

Edit the composer.json and add the following information:

````json
    //...

    "repositories": [
        {
            "type": "vcs",
            "url": "https://github.com/inwendo/iw_server_auth_user_symfony_bundle"
        }
    ],
    //...
    "require": {
        "inwendo/iw_server_auth_user_symfony_bundle": "dev-master"
    }
    //...
````

Open a command console, enter your project directory and execute the
following command to download the latest stable version of this bundle:

```console
$ composer update
```

This command requires you to have Composer installed globally, as explained
in the [installation chapter](https://getcomposer.org/doc/00-intro.md)
of the Composer documentation.

### Step 2: Enable the Bundle

Then, enable the bundle by adding it to the list of registered bundles
in the `app/AppKernel.php` file of your project:

```php
<?php
// app/AppKernel.php

// ...
class AppKernel extends Kernel
{
    public function registerBundles()
    {
        $bundles = array(
            // ...

            new FOS\UserBundle\FOSUserBundle(),
            new FOS\OAuthServerBundle\FOSOAuthServerBundle(),
            new Inwendo\Auth\UserBundle\InwendoAuthUserBundle(),
        );

        // ...
    }

    // ...
}
```

### Step 3: Add the config to the config.yml file:

```yaml
fos_user:
    db_driver: orm # other valid values are 'mongodb' and 'couchdb'
    firewall_name: main
    user_class: Inwendo\Auth\UserBundle\Entity\User
    from_email:
        address: "%mailer_user%"
        sender_name: "%mailer_user%"

fos_oauth_server:
    db_driver: orm       # Drivers available: orm, mongodb, or propel
    client_class:        Inwendo\Auth\UserBundle\Entity\Client
    access_token_class:  Inwendo\Auth\UserBundle\Entity\AccessToken
    refresh_token_class: Inwendo\Auth\UserBundle\Entity\RefreshToken
    auth_code_class:     Inwendo\Auth\UserBundle\Entity\AuthCode
    service:
         user_provider: fos_user.user_provider.username
```

### Step 4: Add the routing to the routing.yml file:

````yaml
inwendo_auth_user:
    resource: "@InwendoAuthUserBundle/Action/"
    type:     'annotation'

fos_oauth_server_token:
    resource: "@FOSOAuthServerBundle/Resources/config/routing/token.xml"

fos_oauth_server_authorize:
    resource: "@FOSOAuthServerBundle/Resources/config/routing/authorize.xml"
````

### Step 5: Create an oAuth Client:

Open a command console, enter your project directory and execute the
following command :

```console
$ php bin/console inwendo:create_oauth_client
```

### (Optional) Step 6: Create a User:

```console
$ php bin/console fos:user:create
```