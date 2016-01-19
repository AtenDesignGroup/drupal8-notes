# Entities

1. [Content vs Config Entity](#content-vs-config-entity)
1. [List Builder](#list-builder)
2. [Interface](#interface)
3. [Access Control Handler](#access-control-handler)

## Content vs Config Entity

???

## Entity

Location: `src/Entity`

A class that extends an entity base and [implements the interface for the entity](#interface).

Example entity base: [`ContentEntityBase`](https://github.com/drupal/drupal/blob/8.0.x/core/lib/Drupal/Core/Entity/ContentEntityBase.php)

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
