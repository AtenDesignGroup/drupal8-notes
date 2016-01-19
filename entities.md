# Entities

1. [Content vs Config Entity](#content-vs-config-entity)
1. [List Builder](#list-builder)
2. [Interface](#interface)
3. [Access Control Handler](#access-control-handler)

## Content vs Config Entity

???

## Entity

Location: `src/Entity`

A class that extends an entity base and [implements the interface for the entity](#interface). Includes an [annonation object](https://api.drupal.org/api/drupal/core%21core.api.php/group/annotation/8) for the entity which defines the metadata for this "plugin".

Example entity base: [`ContentEntityBase`](https://github.com/drupal/drupal/blob/8.0.x/core/lib/Drupal/Core/Entity/ContentEntityBase.php)

## Views data

Location: `src/Entity`

Provides data for the entity to Views. See [`EntityViewsData`](https://github.com/drupal/drupal/blob/8.0.x/core/modules/views/src/EntityViewsData.php) and [`EntityViewsDataInterface`](https://github.com/drupal/drupal/blob/8.0.x/core/modules/views/src/EntityViewsDataInterface.php).

## List Builder

Location: `src/`

Controls listing for the entity.

Parent list builder is usually: [`EntityListBuilder`](https://github.com/drupal/drupal/blob/8.0.x/core/lib/Drupal/Core/Entity/EntityListBuilder.php)

## Interface

Location: `src/`

Creates a programatic `interface` for the entity.

## Access Control Handler

Location: `src/`

Manages access to the entity and actions/operations on the entity.
