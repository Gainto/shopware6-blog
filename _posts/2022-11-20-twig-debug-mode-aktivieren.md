---
title: Twig Debug Mode aktivieren (dump())
layout: post
date: '2022-11-20 20:37:56 +0100'
cover: assets/images/twig-dump.png
categories:
- entwickler
---

Erstelle eine `twig.yaml ` im `<root>/config ` Ordner mit folgendem Inhalt:

```yaml
twig:
  debug: "%kernel.debug%"
  strict_variables: false
  cache: false
```

Anschließend kannst du

```twig
  dump()
```

im Twig verwenden
