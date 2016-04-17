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

## Injecting other services into your service

If your service needs additional services (database, entity query, etc.), you can inject those in the `OrderManager::create()` method.

```php
/**
 * Order manager service.
 */
class OrderManager implements ContainerInjectionInterface {
  public $database;
  public $entityQuery;
  
  public function __construct(\Drupal\Core\Database\Connection $database, \Drupal\Core\Entity\Query\QueryFactory $entity_query) {
    $this->database = $database;
    $this->entityQuery = $entity_query;
  }
  
  /**
   * {@inheritDoc}
   */
  public static function create(ContainerInterface $container) {
    return new static(
      $container->get('database'),
      $container->get('entity.query')
    );
  }
  
  /**
   * Get order nodes.
   */
  public function getOrders() {
    $query = $this->entityQuery->get('node')
      ->condition('type', 'order');
    $orders = $query->execute();
  }
}
```
