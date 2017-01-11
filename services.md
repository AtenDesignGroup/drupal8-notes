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

### Use your new service in procedural code (`.module` files)

```php
$orders = \Drupal::service('my_module.order_manager');
// do things with your service!
```

## Injecting other services into your service

If your service needs additional services (database, entity query, etc.), you can inject those in the `OrderManager::create()` method.

```php
use Drupal\Core\Database\Connection;
use Drupal\Core\Entity\QueryFactory;

/**
 * Order manager service.
 */
class OrderManager implements ContainerInjectionInterface {

  /**
   * @var \Drupal\Core\Database\Connection
   */
  protected $database;

  /**
   * @var \Drupal\Core\Entity\QueryFactory
   */
  protected $queryFactory;
  
  /**
   * {@inheritdoc}
   */
  public function __construct(\Drupal\Core\Database\Connection $database, \Drupal\Core\Entity\Query\QueryFactory $query_factory) {
    $this->database = $database;
    $this->queryFactory = $query_factory;
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
    // Gets fully loaded entities.
    $orders = $this->queryFactory->get('node')
      ->getByProperties(['type' => 'order']);

    // More complex query.
    $query = $this->queryFactory->get('node')
      ->condition('type', 'blog')
      ->condition('created', '1234567', '<')
      ->sort('title');
    $result = $query->execute();
    $nids = array_values($result);
  }

}
```

You also need to update the services file:

```yaml
services:
  my_module.order_manager:
    class: Drupal\my_module\OrderManager
    arguments: ['@database', '@entity.query']
```
