# Services

## Roll your own service

In a custom module, you can create your own service by doing the following steps:

### Create a class

In your module's `src` folder, create a service class. For this example, we'll make a `OrderManager` class in `src/OrderManager.php`:

```php
<?php

namespace Drupal\my_module;

use Symfony\Component\DependencyInjection\ContainerInterface;
use Drupal\Core\DependencyInjection\ContainerInjectionInterface;

/**
 * Order manager service.
 */
class OrderManager implements ContainerInjectionInterface {
  public function __construct() {
  }
  
  /**
   * {@inheritDoc}
   */
  public static function create(ContainerInterface $container) {
    return new static();
  }
}
```

### Update your module services.yml file

Create or update your module's service YAML file, in our case `my_module.services.yml`:

```yaml
services:
  my_module.order_manager:
    class: Drupal\my_module\OrderManager
```

Clear/rebuild the Drupal cache: `drush cr`

### Use your new service

```php
$orders = \Drupal::service('my_module.order_manager');
// do things with your service!
```
