---
id: introduction
title: Introduction
slug: /
---

# Manifest Documentation 👋

<span class="is-ib is-bordered">
![CASE App](./assets/images/cat-list.png ':class=is-bordered')
</span>

## What is Manifest ?

Manifest is the simplest **BaaS (Backend As A Service)** you will find.

It provides a complete backend to your client app without the hassle that comes with it. It actually fits into **a single YAML file** that generates a complete backend.

Here is an example of a complete Manifest app:

```yaml
# manifest/backend.yml
name: Healthcare application

entities:
  👩🏾‍⚕️ Doctor:
    properties:
      - fullName
      - avatar
      - { name: price, type: money, options: { currency: EUR } }
    belongsTo:
      - City

  🤒 Patient:
    properties:
      - fullName
      - { name: birthdate, type: date }
    belongsTo:
      - Doctor

  🌍 City:
    properties:
      - name
```

## Main features

- ⚡ **Instant backend with DB, REST API and Admin panel** without any configuration
- 🧠 **Smart SDK** to import in your favorite JS front-end
- 🛠️ **Essential features** for your backend
