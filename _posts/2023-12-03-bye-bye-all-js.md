---
title: Bye bye all.js - Bessere performance in der Storefront!
layout: post
date: '2023-12-03 22:00:00'
cover: assets/images/all_js_rework.png
categories:
- entwickler
- shopbetreiber
---

In der Welt von Shopware stehen Entwickler und Shopbetreiber vor einem  Problem mit JavaScript-Dateien.  
Im Moment (Shopware < 6.6) landet jedes importierte JavaScript, sei es von der Haupt-JS-Datei (main.js) einer App oder eines Plugins, durch den Befehl `composer build:js`, letztendlich im allumfassenden all.js des Storefronts.  
Das gilt sogar für JavaScript, das dem Core des Storefronts hinzugefügt wird.  

## Das Problem: Ein steifes all.js auf jeder Seite

Das all.js-File ist steif und wird auf allen Seiten verwendet, da es fest in das Basis-Twig-Template eingebunden ist.  
Das führt dazu, dass der Shop eine Menge nicht genutzten JavaScript-Code mitliefert.  
Wenn dann noch mehr Apps installiert werden, wird das all.js-File richtig fett.  

## Die Lösung: Asynchrones JS-Plugin-System und dynamische Imports

Um das anzugehen, ka der Vorschlag auf, das JS-Plugin-System zu überarbeiten und dynamische Imports zuzulassen.  
Im Moment sind alle JS-Plugins synchron, was bedeutet, dass sie vor der Plugin-Registrierung in die main.js importiert werden und somit zwangsläufig im all.js landen.  
Egal, ob diese Plugins überhaupt auf ener bestimmten Seite des Storefronts verwendet werden.  

Die Lösung? Einen optionalen, asynchronen Ansatz in `window.PluginManager.register()` einführen.  
Anstelle alle JavaScript-Plugin-Dateien synchron in die main.js zu importieren, wird Shopware ab der Version 6.6 die Möglichkeit bieten, diese dynamisch zu importieren.  
Nach der asynchronen Plugin-Registrierung checkt die Initialisierungsphase automatisch, ob ein registriertes JS-Plugin asynchron ist und lädt es bei Bedarf, wenn der Plugin-Selektor (z.B. [data-cross-selling]) im aktuellen DOM gefunden wird.  

## Schluss mit all.js: Ein neuer Weg für JS-Dateien

Statt alle JS-Dateien aus dem Ordner `<app-root>/Resources/app/storefront/dist/storefront/js/` dynamisch in das all.js zu werfen, sollen sie stattdessen in den Ordner `<platform-root>/public/theme/<theme-hash>/js/` verschoben werden.  
Bisher hat Shopware alle JS-Bundles aus dem `/dist`-Ordner einer App in die all.js geschmissen.  
Mit dem neuen Ansatz bleiben die JS-Dateien der Apps getrennt und behalten ihre Originalform bei.  
Nur das "Haupt"-JS-Bundle, das den technischen Namen der App trägt, wird in das Template eingefügt.  
Die anderen Bundles können später vom Hauptbundle nach Bedarf abgerufen (gefetched) werden.  

## Vorteile des neuen Ansatzes

Dieser Ansatz bringt nicht nur eine effizientere Nutzung von JavaScript-Ressourcen mit sich, sondern erlaubt auch dynamische Imports auf Abruf.  
Das ist besonders cool, wenn nur bestimmte JS-Dateien nach Benutzerinteraktionen gebraucht werden.  
Insgesamt versprechen diese neuen Ideen für die JavaScript-Integration in Shopware mehr Flexibilität und Performance für Entwickler sowie eine schlanke Bereitstellung von JavaScript für die Benutzer.  

---

Wenn du tiefer in die Thematik eintauchen möchtest, wirf einen Blick auf die [GitHub Diskussion](https://github.com/shopware/shopware/discussions/3310), in der [@tobiasberge](https://github.com/tobiasberge) das ganze Thema nochmal kompakt vorstellt und auch auf technischer Ebene drauf eingeht.  
Dieser Post basiert auf den oben verlinkten Beitrag und wurde maßgeblich von diesem inspiriert.  
Die Ankündigung von Shopware, den Vorschlag mit der 6.6 in den Core zu intergieren, findest du [hier](https://www.shopware.com/en/news/shopware-6-6-rc/#66-rc-storefront-changes)   
