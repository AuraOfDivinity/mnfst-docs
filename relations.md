---
id: relations
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Relations

Add a relationship between two [entities](entities.md), like an _Invoice_ that belongs to a _Customer_, a _Pet_ that belongs to an _Owner_, etc.

:::note

As of today, only the _BelongsTo_ and thus its opposite _hasMany_ are available.

_ManyToMany_ relationships are coming soon.

:::

## BelongsTo relationship

### Syntax

This is a standard _HasMany_ / _BelongsTo_ relationship between **User** and **Cat** entities.

Each **User** can have several **Cat** items. A **Cat** must have a **User**.

```yaml
😃 User:
properties:
  - name

😺 Cat:
  properties:
    - name
  belongsTo:
    - User
```

As for the [properties](properties.md), there is a short and a long syntax. The long syntax allows you pass params to it:

```yaml
🐶 Dog:
  properties:
    - name
  belongsTo:
    - { name: Owner, entity: User, eager: true }
```

When you define a **belongsTo** relationship, it implicitly set the opposite _hasMany_ relationship and allow [querying relations](#querying-relations) in both directions.

### Relation params

You can pass arguments using the long syntax:

| Option     | Default     | Type    | Description                                                                                                                      |
| ---------- | ----------- | ------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **name**   | Entity name | string  | The name of the relation                                                                                                         |
| **entity** | -           | string  | The class name of the entity that the relationship is with                                                                       |
| **eager**  | `false`     | boolean | Whether the relationship should be eager loaded. Otherwise, you need to explicitly request the relation in the client SDK or API |

## Querying relations

### Load relations

You can specify which relations you want to load with your entities in your query. **Eager relations** are loaded automatically.

<Tabs>
  <TabItem value="sdk" label="JS SDK" default>
    ```js
      // Fetch entities with 2 relations.
      const cities = await manifest
        .from('cities')
        .with(['region', 'mayor'])
        .find()

      // Fetch nested relations.
      const cities = await manifest
        .from('cities')
        .with(['region', 'region.country', 'region.country.planet'])
        .find()
    ```

  </TabItem>
  <TabItem value="rest" label="REST API" default>
    ```http

    // Fetch entities with 2 relations.
    GET http://localhost:111/api/dynamic/city?relations=region,mayor

    // Fetch nested relations.
    GET http://localhost:111/api/dynamic/city?relations=region,region.country,region.country.planet
    ```

  </TabItem>
</Tabs>

### Filter by relation

Once the relation is loaded, you can also filter items by their relation id or properties.

<Tabs>
  <TabItem value="sdk" label="JS SDK" default>
    ```js
      // Get all cats that belong to owner with id 1.
      const cats = await manifest
        .from('cats')
        .with(['owner'])
        .where('owner.id = 1')
        .find()

      // Get all cats that have an owner with name "Jorge".
      const cats = await manifest
        .from('cats')
        .with(['owner'])
        .where('owner.name = Jorge')
        .find()
    ```

  </TabItem>
  <TabItem value="rest" label="REST API" default>
    ```http
    // Get all cats that belong to owner with id 1.
    GET http://localhost:1111/api/dynamic/cats?relations=owner&owner.id_eq=1

    // Get all cats that have an owner with name "Jorge".
    GET http://localhost:1111/api/dynamic/cats?relations=owner&owner.name_eq=Jorge
    ```

  </TabItem>
</Tabs>
