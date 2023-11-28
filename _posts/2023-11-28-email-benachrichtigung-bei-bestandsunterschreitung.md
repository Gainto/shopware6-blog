---
title: E-Mail Benachrichtigung bei Bestandsunterschreitung (OHNE PLUGIN)
layout: post
date: '2023-11-28 22:00:00'
cover: assets/images/bestand_flow.png
categories:
- entwickler
- shopbetreiber
---

**Ab der Version 6.5.5.0** gibt es ein neues, optionales Bestands Handling, welches es ermöglicht per E-Mail benachrichtigt zu werden, wenn der Bestand eines Produkts einen vordefinierten Schwellenwert unterschreitet.
  
Das Besondere dabei: Diese Funktion lässt sich ohne den Einsatz kostenpflichtiger Drittanbieter-Plug-Ins realisieren.  

**Hintergrund: Warum ab Shopware 6.5.5.0?**

Die bisherige Praxis, Bestandsbenachrichtigungen über kostenpflichtige Plug-Ins abzuwickeln, wird nun durch einen raffinierte "Trick" mit den nativen Bordmitteln von Shopware ersetzt.  
  
Die Vorgehensweise von Shopware im Umgang mit dem Lagerbestand mag auf den ersten Blick kompliziert erscheinen.  
Jedes Produkt verfügt über zwei Felder, den __Lagerbestand__ und den __Verfügbaren Lagerbestand__.  
Bei einer Bestellung wird nicht, wie angenommen, der Lagerbestand, sondern der verfügbare Lagerbestand reduziert.  
Dies ermöglicht es, das Produkt weiterhin als "reserviert" zu betrachten, während der eigentliche Lagerbestand erst nach Abschluss der Bestellung sinkt.  
  
Diese Herangehensweise ist zwar sinnvoll, aber für viele Shopbetreiber verwirrend.  
Hinzu kommt, dass nicht überall auf die Variable zugegriffen werden konnte, die den "tatsächlichen" Bestand anzeigt.  
Wenn die hier erklärte Vorgehensweise in einer älteren Shopware Version genutzt wird, kann nur der reservierte- aber nicht der reale Lagerbestand kontrolliert werden.  
Nach Rückmeldungen aus der Community hat Shopware das System überarbeitet: In der überarbeiteten Version repräsentiert der Lagerbestand den Echtzeit-Lagerbestand.  
  
**Was bedeutet das für Shopbetreiber?**  
 
Mit dem neuen Bestands-Handling, optional verfügbar ab Version 6.5.5.0 und ab Version 6.6.0.0 zum Standard, können Shopbetreiber nun direkt auf den Echtzeit-Bestand zugreifen. Dies eröffnet die Möglichkeit, den Shopbetreiber automatisch zu benachrichtigen, wenn der Bestand eines Produkts einen vordefinierten Wert erreicht oder unterschreitet.  
  
---
  
__In folgendem erkläre ich in 5 Schritten die Konfiguration der Bestandsbenachrichtigungs-E-Mail + Aktivierung des neuen Bestand Systems:__  

## 1. Neues Bestands-Handling-System aktvieren
   Um das neue System zu finden, verbinden wir uns mit SSH oder FTP auf den Server, auf dem der Shop liegt und öffnen die `<ShopInstallation>/.env` Datei.  
   hier fügen wir folgende Zeile ans Ende hinzu, bevor wir die Datei abspeichern und wieder schließen: `STOCK_HANDLING=1`  
   Wunderbar, das neue Stock Handling System ist jetzt aktiv.  
## 2. Neue Regel anlegen
   Als nächstes benötigen wir eine Regel, die greift, wenn ein Produkt einen gewissen Lagerbestand erreicht oder unterschreitet.  
   Im Admin navigieren wir zu `Einstellungen > Shop > Rule Builder > Neue Regel`.  
   Gib der Regel einen passenden Namen, wie "Lagerbestand kleiner 5" und setze die Priorität auf 100.  
   Bei den Bedingungen wählen wir nun aus:  
   `Position mit Lagerbestand | Mind. eine | Ist kleiner/gleich | 5`  
   Die 5 ersetzt du natürlich mit dem Wert, bei dem du benachrichtigt werden möchtest.  
   Nun können wir die Regel speichern und mit dem nächsten Schritt weiter machen.  
	 ![](/assets/images/bestand_rule.png)

## 3. Mail-Template kopieren & anpassen 
   Immer dann, wenn eine Bestellung aufgegbeben wird, wird der Lagerbestand der jeweiligen Produkte abgezogen.  
   Wenn eine Bestellung aufgebeben wird, wird ebenso eine Bestellbestätigung rausgesendet.  
   Da wir im nächsten Schritt genau an dieser Stelle eingreifen, macht es Sinn, das Bestellbestätigungs Mail-Template als Basis zu nutzen.  
   Navigiere zu `Einstellungen > Shop > E-Mail Templates` und kopiere das `Order confirmation` Template.  
   Öffne das duplizierte Template und schreib in die Beschreibung "Bestandsbenachrichtigung", sodass du es später von der Bestellbestätigung Unterscheiden kannst.  
   Wähle einen passenden Betreff (z.B. Bestandsbenachrichtigung) und scrolle weiter runter zu den Mail Texten.  
	 
![](/assets/images/bestand_mail1.png)
  
Mittels {% raw %}`{{ lineItem.payload.stock }}`{% endraw %} können wir auf den Lagerbestand der jeweiligen Produkte zurückgreifen.  
Ein minimalistisches Mail Template könnte folgendermaßen aussehen:  
![](/assets/images/bestand_mail2.png)

Text:
{% raw %}
```twig
{% for lineItem in order.lineItems %}
{% if lineItem.payload.stock == 0 || lineItem.payload.stock < 5%}{{ lineItem.label  }} {% if lineItem.payload.productNumber is defined %}({{ lineItem.payload.productNumber|u.wordwrap(80) }}){% endif %}: {{ lineItem.payload.stock  }} ){% endif %}
{% endfor %}
```
{% endraw %}

HTML:
{% raw %}
```twig
<div style="font-family:arial; font-size:12px;">
    <table border="0" style="font-family:Arial, Helvetica, sans-serif; font-size:12px;">
        <tr>
            <td bgcolor="#F7F7F2" style="border-bottom:1px solid #cccccc;"><strong>Prod. no.</strong></td>
            <td bgcolor="#F7F7F2" style="border-bottom:1px solid #cccccc;"><strong>Name</strong></td>
            <td bgcolor="#F7F7F2" style="border-bottom:1px solid #cccccc;"><strong>Stock</strong></td>
        </tr>

        {% for lineItem in order.lineItems %}
        {% if lineItem.payload.stock == 0 || lineItem.payload.stock < 5%}
            <tr>
                <td>{% if lineItem.payload.productNumber is defined %}{{ lineItem.payload.productNumber|u.wordwrap(80) }}{% endif %}</td>
                <td>{{ lineItem.label }}</td>
                <td>
                    {{ lineItem.payload.stock }}
                </td>
            </tr>
            {% endif %}
        {% endfor %}
    </table>
</div>
```
{% endraw %}

Passe in beiden Templates die Zeilen mit dem {% raw %}`lineItem.payload.stock == 0 || lineItem.payload.stock < 5`{% endraw %} an, sodass sie mit deiner bei 2. erstellten Regel übereinstimmen.  
Die Zeilen sorgen dafür, dass nicht alle Produkte aus einer Bestellung in der Mail landen, sondern nur die, bei denen der Lagerbestand unterschritten wurde.

## 4. Flow-Builder erweitern
   Wir haben nun das System aktiviert, eine Regel erstellt, die greift, wenn Produkte einen Lagerbestand unterschreiten und ein Mail-Template erstellt, welches wir nutzen möchten.  
   Im letzten Schritt müssen wir Shopware nur noch mitteilen, dass die Mail rausgesendet werden soll, wenn die bei 2. definierte Regel eintritt.
  
   Wie oben schon erwähnt, wird beim Eingang einer Bestellung der Lagerbestand abgezogen. Demnach macht es auch hier Sinn, genau an dieser Stelle die Regel zu evaluieren.  
   Hierzu öffnen wir den Flow-Builder unter `Einstellungen > Shop > Flow-Builder` und öffnen den `Order placed` Flow.  
  
![](/assets/images/bestand_flow2.png)

	
   Wenn wir nun den Tab **Flow** öffnen, sehen wir schon den Flow, der die Bestellbestätigung raussendet.  
   Klicke in der Timeline auf das **+** und wähle **Bedingung (WENN) hinzufügen** aus.  
   Wähle als Regel die bei 2. erstellte Regel aus.  
   Es erscheinen 2 neue Zweige __Wahr__ und __Falsch__.  
   Während __Wahr__ ausgeführt wird, wenn die Regel zutrifft, wird __Falsch__ ausgeführt, wenn die Regel NICHT zutrifft.  
   Wenn die Regel nicht zutrifft, möchten wir auch nichts machen. Demnach können wir hier die Aktion **Flow anhalten** auswählen.  
   Wenn die Regel zutrifft, möchten wir die Mail raussendet. Demnach können wir hier die Aktion **E-Mail verschicken** auswählen.  
   Wähle als Empfänger **Administrator** oder definiere alternativ eine andere E-Mail Adresse mittels **Anderer Empfänger**.  
   Wähle jetzt noch das E-Mail Template aus, welches wir bei 3. angelegt haben aus und speichere den Flow.  
![](/assets/images/bestand_flow.png)

## 5. Tests durchführen
   Leere sicherheitshalber einmal den Cache und Teste es aus!  
   Wenn du nun ein oder mehrere Produkte bestellst, wird die E-Mail rausgesendet sofern das Produkt den in der Regel definierten Lagerbestand unterschreitet.

---

Mehr über das neue Bestands System erfährst du auf der [Offiziellen Shopware Seite](https://developer.shopware.com/docs/guides/hosting/configurations/shopware/stock.html).