---
title: Shopware6 Plug-Ins schneller builden
layout: post
date: '2023-10-29 20:37:56 +0100'
cover: assets/images/faster-plugin-build.png
categories:
- entwickler
---

Wenn ein Plug-In oder eine Erweiterung installiert wird, kopiert Shopware den Inhalt des `<plugin>/src/Resources/public` Ordners in das entsprechende Bundle `/public/bundles/plugin-name`.
Dieser Ordner enthält alle erforderlichen Assets (CSS, JS usw.) in komprimierter und minifizierter Form.

Der Erstellungsprozess dieser Dateien erfordert die Ausführung des Build-Vorgangs, der erfahrungsgemäß recht zeitaufwendig sein kann.
Insbesondere bei wiederholtem Anstoßen des Prozesses (aus welchen Gründen auch immer), kann das echt stören und einen Entwickler aus den "Flow" bringen.

Um den Build-Prozess etwas zu beschleunigen, können die folgenden Exports verwendet werden. Alternativ können die Werte auch in die .env-Datei aufgenommen werden:

```
export SHOPWARE_ADMIN_BUILD_ONLY_EXTENSIONS=1
export DISABLE_ADMIN_COMPILATION_TYPECHECK=1
export PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=1
```

Diese Maßnahmen tragen dazu bei, die Effizienz des Build-Prozesses zu verbessern und insbesondere bei wiederholter Ausführung die benötigte Zeit zu minimieren.
