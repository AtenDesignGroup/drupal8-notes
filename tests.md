# Tests

## Custom Module Tests

This example assumes that you have a module named `my_example`.

### Step 1: Create a test file

Create a new directory in your module: `tests/src/Unit`

Create a test file in that directory: `MyExampleTest.php`

```php
<?php

namespace Drupal\Tests\my_example\Unit;

use Drupal\Tests\UnitTestCase;
use Drupal\Core\DependencyInjection\Container;

/**
 * @group my_example_test
 */
class MyExampleTest extends UnitTestCase {
  public function setUp() {
    
  }

  /**
   * Test a thing
   */
  public function testTestAThing() {
    // fails!
    $this->assertEquals(1, 0);
  }

}
```

### Step 2: Run your tests

From your Drupal root:

```
pushd core; php ../vendor/bin/phpunit ../modules/custom/my_example/tests; popd
```

### Dependency injection

Uses mock objects: https://phpunit.de/manual/current/en/test-doubles.html

```php
public function setUp() {
  $this->database = $this->getMock('Drupal\Core\Database\ConnectionInterface');
  $this->invalidator = $this->getMock('Drupal\Core\Cache\CacheTagsInvalidator');
  $this->cache = $this->getMock('Drupal\Core\Cache\CacheFactoryInterface');
  $this->entityQuery = $this->getMock('Drupal\Core\Entity\Query\QueryFactoryInterface');

  $this->container = new Container();
  $this->container->set('database', $this->database);
  $this->container->set('cache_tags.invalidator', $this->invalidator);
  $this->container->set('cache_factory', $this->cache);
  $this->container->set('entity.query', $this->entityQuery);

  $this->mes = MyExampleService::create($this->container);
}
```
