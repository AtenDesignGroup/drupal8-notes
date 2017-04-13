# Services

## Roll your own service

In a custom module, you can create your own service by doing the following steps:

### Create a class

In your module's `src` folder, create a service class. For this example, we'll make a `OrderManager` class in `src/OrderManager.php`:

```php
<?php

namespace Drupal\my_module;

use Drupal\Core\DependencyInjection\ContainerInjectionInterface;
use Symfony\Component\DependencyInjection\ContainerInterface;

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
use Drupal\Core\Entity\StorageInterface;
use Drupal\Core\DependencyInjection\ContainerInjectionInterface;
use Symfony\Component\DependencyInjection\ContainerInterface;

/**
 * Order manager service.
 */
class OrderManager implements ContainerInjectionInterface {

  /**
   * @var \Drupal\Core\Database\Connection
   */
  protected $database;

  /**
   * @var \Drupal\Core\Entity\StorageInterface
   */
  protected $nodeStorage;
  
  /**
   * {@inheritdoc}
   */
  public function __construct(Connection $database, StorageInterface $node_storage) {
    $this->database = $database;
    $this->nodeStorage = $node_storage;
  }
  
  /**
   * {@inheritDoc}
   */
  public static function create(ContainerInterface $container) {
    return new static(
      $container->get('database'),
      $container->get('entity_type.manager')->getStorage('node')
    );
  }
  
  /**
   * Get order nodes.
   */
  public function getOrders() {
    // Gets fully loaded entities.
    $orders = $this->nodeStorage->loadByProperties(['type' => 'order']);

    // More complex query.
    $query = $this->nodeStorage->getQuery()
      ->condition('type', 'blog')
      ->condition('created', '1234567', '<')
      ->sort('title');
    $result = $query->execute();
    $blogs = $this->nodeStorage->loadMultiple(array_values($result));
  }

}
```

You also need to update the services file:

```yaml
services:
  my_module.order_manager:
    class: Drupal\my_module\OrderManager
```

## Common services used in containers

| Container/Service | Class/Interface |
| ----------------- | --------------- |
| `database` | `Drupal\Core\Database\Connection` |
| `entity.query` | `Drupal\Core\Entity\Query\QueryFactory` |
| `entity_type.manager` | `Drupal\Core\Entity\EntityTypeManagerInterface` |
| `serializer` | `Symfony\Component\Serializer\SerializerInterface` |
| `serializer.normalizer.list` | `Symfony\Component\Serializer\Normalizer\NormalizerInterface` |
| `pathauto.alias_cleaner` | `Drupal\pathauto\AliasCleanerInterface` |
| `plugin.manager.mail` | `Drupal\Core\Mail\MailManagerInterface` |
| `current_user` | `Drupal\Core\Session\AccountInterface` |
