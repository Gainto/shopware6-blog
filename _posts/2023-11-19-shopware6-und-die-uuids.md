---
title: Shopware6 und die UUIDs
layout: post
date: '2023-11-19 20:37:56 +0100'
cover: assets/images/shopware-uuids.png
categories:
- entwickler
---

Shopware 6 setzt auf die Verwendung von UUIDs [Universally Unique Identifiers](https://de.wikipedia.org/wiki/Universally_Unique_Identifier) anstelle der klassischen sequentiellen IDs.
Obwohl UUIDs zahlreiche Vorteile bieten (im Vergleich zu sequentiellen IDs), können sie das Leben eines Entwicklers gelegentlich komplizierter gestalten. Insbesondere wenn man schnell auf Datenbankdaten zugreifen möchte, können UUIDs eine zusätzliche Herausforderung darstellen.

Um die Entwicklergemeinschaft bestmöglich zu unterstützen, werden im Folgenden Lösungsansätze vorgestellt, wie Datensätze in Shopware 6 anhand von UUIDs aktualisiert, erstellt oder transformiert werden können.

Allgemein werden UUIDs in der Datenbank als BINARY-Daten gespeichert und in der Regel in hexadezimaler Form dargestellt. Wenn eine ID in der Datenbank gespeichert werden soll, erfolgt dies als BINARY, und beim Auslesen werden die BINARY-Daten extrahiert und in Hex umgewandelt.

**Beispiele:**

**Cast as BINARY:**
```sql
UPDATE `order` SET `state_id` = 0xca58b0b780014a35b26291dfd7fbe6b3 WHERE `order`.`id` = CAST(0xfd9884b375d145389e4dd780b7e49f52 AS BINARY)
```

**Unhex:**
```sql
INSERT INTO `customer_tag` (`customer_id`, `tag_id`) VALUES (UNHEX('CFBD5018D38D41D8ADCA10D94FC8BDD6'),UNHEX('CFBD5018D38D41D8ADCA10D94FC8BDD2'));
```

**Hex:**
```sql
SELECT LOWER(HEX(cms_page_id)) as cms_page_id FROM cms_page_translation;
```

**UUID als BLOB in Hexadezimaler Form darstellen (UUID zu BINARY):**
```sql
SELECT id FROM customer WHERE id = X'6F9619FF8B86D011B42D00C04FC964FF';
```

Zur Erzeugung zufällig generierter, 32 Zeichen langer Hex-IDs kann der folgende Generator verwendet werden: [Random Hex Generator](https://www.browserling.com/tools/random-hex)

Wenn sich der Code im Shopware-Kontext befindet, bietet Shopware eine Klasse an, die die Arbeit erleichtert und das manuelle Konvertieren von A nach B überflüssig macht.
Hierbei kann die Klasse `Shopware\Core\Framework\Uuid\Uuid` genutzt werden, mit Methoden wie:
```php
Uuid::randomBytes()
Uuid::fromBytesToHex()
Uuid::fromHexToBytes()
Uuid::isValid()
```
