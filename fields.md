# Fields

## BaseFieldDefinition::create

See: https://www.drupal.org/node/2078241

## Field types

```php
$types = \Drupal::service('plugin.manager.field.field_type')->getDefinitions();
dpm(array_keys($types));
```
