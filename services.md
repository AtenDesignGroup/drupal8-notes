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
  public static create(ContainerInterface $container) {
    return static();
  }
}
```
