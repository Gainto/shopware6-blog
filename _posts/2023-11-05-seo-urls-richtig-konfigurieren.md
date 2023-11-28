---
title: Shopware6 SEO URL's richtig konfigurieren
layout: post
date: '2023-11-05 20:37:56 +0100'
cover: assets/images/seo-settings.png
categories:
- shopbetreiber
---

Verständliche SEO-URLs sind essenziell für eine erfolgreiche Suchmaschinenoptimierung.
In Shopware 6 steht dir standardmäßig eine vorgefertigte Lösung zur Verfügung, um benutzerfreundliche URLs zu erstellen.
Du kannst genau festlegen, wie die URLs für Produkte oder Kategorien strukturiert werden sollen. Die Anpassung der SEO-URL-Struktur erfolgt unter `Einstellungen > Shop > SEO`.
Hier sind die gängigsten Snippets aufgeführt, die du für die Gestaltung der SEO-URLs nutzen kannst:

**Standard:**
{% raw %}
`{{ product.translated.name }}/{{ product.productNumber }}`
{% endraw %}

**Alles klein geschrieben:**
{% raw %}
`{{ product.translated.name|lower }}/{{ product.productNumber|lower }}`
{% endraw %}

**Keine Produktnummer bei Nicht-Varianten:**
{% raw %}
`{{ product.translated.name }}{%if product.parentId %}/{{ product.productNumber }}{% endif %}`
{% endraw %}

**Keine Produktnummer bei Nicht-Varianten und alles klein geschrieben:**
{% raw %}
`{{ product.translated.name|lower }}{%if product.parentId %}/{{ product.productNumber|lower }}{% endif %}`
{% endraw %}

**Wie in SW5:**
{% raw %}
`{{ product.translated.name|lower }}{%if product.parentId %}/?number={{ product.productNumber }}{% endif %}`
{% endraw %}

**Wichtig**  
Nach dem Ändern der SEO URL's einmal den Index aktualisieren:  
`bin/console dal:refresh:index`
  
  
---
  
  
Individuelle SEO-URLs können auf der Ebene von Produkten oder Kategorien separat überschrieben werden.
Sobald eine solche manuelle Überschreibung erfolgt ist, unterliegen sie keiner automatischen Anpassung mehr im Zuge von Strukturänderungen.
Nach einer manuellen Aktualisierung wird ein Flag in der Datenbank gesetzt.
Dieses Flag verhindert, dass die manuell festgelegte URL bei späteren Strukturanpassungen überschrieben wird.
Um die manuelle Überschreibung rückgängig zu machen, kann das Flag `is_modified` in der Tabelle `seo_url` auf `0` zurückgesetzt werden.
