---
title: Expected input to be non empty non associative array.
layout: post
date: '2023-10-08 20:37:56 +0100'
cover: assets/images/shopware-exception-array.png
categories:
- entwickler
---

Dieser Fehler tritt häufig auf, wenn Daten über ein repository gespeichert werden sollen. Wenn die `upsert`, `update` oder `create` Methode des repositories verwendet wird, wird ein **verschachteltes** Array erwartet.

Nehmen wir folgenden Code als Beispiel:

```php
$data = [
    'id' => 'abcd123...',
    'name' => 'Beispiel Name'
];
$productRepository->update($data, $context);
```

Dieser Code würde den Fehler `Expected input to be non empty non associative array` verursachen.

**Die Lösung:**

```php
$data = [
    'id' => 'abcd123...',
    'name' => 'Beispiel Name'
];
$productRepository->update([$data], $context);
```

oder

```php
$data = [
    [
        'id' => 'abcd123...',
        'name' => 'Beispiel Name'
    ]
];
$productRepository->update($data, $context);
```

Der Grund, wieso ein verschachteltes Array übergeben muss, ist recht simpel:
Es können mehrere Entities auf einmal übergeben/bearbeitet werden.
Wenn kein verschachteltes Array erwartet werden würde, müsste für jede Entity ein eigenständiger Aufruf gemacht werden.
