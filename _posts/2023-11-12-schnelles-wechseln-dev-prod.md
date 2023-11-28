---
title: Schnelles wechseln zwischen DEV und PROD
layout: post
date: '2023-11-12 20:37:56 +0100'
cover: assets/images/env-switcher.png
categories:
- entwickler
---

Es gibt verschiedene Gründe dafür, den Betriebsmodus des Shops in der `.env`-Datei von `prod` auf `dev` umzustellen. Insbesondere Entwickler sind mit dieser Notwendigkeit vertraut.

Es kann jedoch mühsam sein, sich zunächst über SSH oder FTP beim entsprechenden Shop anzumelden, um dann die `.env`-Datei zu öffnen, zu bearbeiten und zu speichern.

Um diesen Prozess zu erleichtern, steht ein kleines, kostenfreies Hilfs-Plug-In für Shopware 6.5 zur Verfügung.
Dieses Plug-In integriert zwei switches im Administrationsbereich, mit denen der Wechsel zwischen den Modi "PROD" und "DEV" schnell und unkompliziert erfolgen kann.

Das Plug-In richtet sich an Entwickler und Testinstanzen und sollte nicht in einem produktiven Shop verwendet werden

**Hier gehts zum Plug-In:**  
[Gainto/JblEnvSwitcher](https://github.com/Gainto/JblEnvSwitcher)
