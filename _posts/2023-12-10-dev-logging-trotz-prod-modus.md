---
title: DEV Logging trotz PROD Modus
layout: post
date: '2023-12-10 22:00:00'
cover: assets/images/monolog_yaml.png
categories:
- entwickler
- shopbetreiber
---

Als Entwickler stößt man manchmal auf die Herausforderung, Fehler im Live-System zu debuggen, insbesondere wenn sich der Fehler auf magische Weise in der Staging- oder Dev-Umgebung nicht reproduzieren lässt. Die naheliegendste Lösung besteht darin, den Modus in der .env einfach auf DEV zu setzen, den Fehler kurz zu debuggen und dann den Modus zurück auf prod zu stellen.  
  
__Diese Vorgehensweise birgt jedoch einige Herausforderungen und ein besonders großes Problem:__  

__Performanceeinbußen:__  
Die Performance sinkt erheblich.  

__Cache-Reduzierung:__  
Der Cache wird reduziert und kann sogar vollständig deaktiviert werden.  

__Verhalten des Shops:__  
Je nach Konfiguration kann sich das Shopverhalten ändern. Im schlimmsten Fall funktioniert der Checkout nicht mehr.  

__Leak sensibler Informationen:__  
Möglicherweise werden sensible Informationen preisgegeben.  
Der Symfony Profiler könnte aktiviert werden, was dazu führt, dass Umgebungsvariablen, einschließlich Datenbankzugangsdaten, frei einsehbar sind.  
In einem Live-Shop mit vielen Kundendaten wäre das eine absolute Katastrophe.  

Es gilt also, in einer laufenden Live-Umgebung zu vermeiden, den Dev-Modus zu aktivieren.  
  
Aber wie lässt sich das Logging trotzdem so anpassen, dass es sich wie im DEV-Modus verhält?  
  
## Der Shopware Logger:  
Abhängig von der Umgebung in der `.env` erstellt Shopware eine prod-datum.log und/oder eine dev.log im `<shop>/var/log` Ordner.  
Shopware protokolliert in diesen Dateien mit Monolog. Der Standard-Shopware-Logger wird im DI (Dependency Injection) wie folgt in Services injected:
`<argument type="service" id="logger"/>`  
  
Während im Produktivmodus nur Fehler protokolliert werden, werden im Dev Modus auch Infos, Notices und Debug-Meldungen protokolliert.  
  
__.env=dev loggt:__  
```php
$this->logger->error("ERROR");
$this->logger->warning("WARNING");
$this->logger->alert("ALERT");
$this->logger->debug("DEBUG");
$this->logger->critical("CRITICAL");
$this->logger->info("INFO");
$this->logger->notice("NOTICE");
```      

__.env=prod loggt:__  
```php
$this->logger->error("ERROR");
$this->logger->alert("ALERT");
$this->logger->critical("CRITICAL");
```

## DEV-Logging im Prod-Modus aktivieren:
Um auch in der PROD-Umgebung das gleiche wie in DEV zu protokollieren, muss das Log-Level für die PROD-Umgebung angepasst werden.  
Das geschieht in der `monolog.yaml`.  

Finde die `monolog.yaml` im `<root>/config/packages/prod` Ordner und ändere das Level von `error` im `nested` Handler und das `action_level` auf `debug`:
```yaml
<root>/config/packages/prod/monolog.yaml

monolog:
    handlers:
        main:
            type: fingers_crossed
            action_level: debug # hier !!!
            handler: nested
            excluded_http_codes: [404, 405]
            buffer_size: 30
        business_event_handler_buffer:
            level: error
        nested:
            type: rotating_file
            path: "%kernel.logs_dir%/%kernel.environment%.log"
            level: debug # hier !!!
        console:
            type: console
            process_psr_3_messages: false
            channels: ["!event", "!doctrine"]

```

__Beachte:__ In der Theorie sollten nun auch im Prod-Modus die DEV-Ausgaben protokolliert werden.  
Es kann jedoch sein, dass bestimmte Erweiterungen und Apps `%kernel.debug%` nutzen und auf dieser Basis protokollieren.  
In diesem Fall würde im Prod-Modus nicht protokolliert, weil `%kernel.debug% = false` ist.  


## In eigene Log Datei loggen
Neben den oben genannten Standard-Loggern ist es oft wünschenswert, eine eigene Log-Datei zu nutzen.  
In einem Symfony-Bundle könnten wir einfach die Symfony Monolog-Integration verwenden, um einen Logger zu erstellen.  
Allerdings hat Shopware eine eigene `LoggerFactory`, die als eine Art Wrapper fungiert.  
Mit dieser Factory können benutzerdefinierte Logger bequem erstellt werden.    
  
Erstelle dazu einfach einen neuen Logger-Service in der `src/Resources/config/services.xml` und nutze die `LoggerFactory`, um einen Logger zu erstellen:

```xml
src/Resources/config/services.xml

<service id="my.custom.logger" class="Monolog\Logger">
    <factory service="Shopware\Core\Framework\Log\LoggerFactory" method="createRotating"/>
    <argument type="string">my-custom-logger</argument>
</service>
```

- Über den Tag `id="my.custom.logger` kannst du diesen Logger in deine Services injecten (`<argument type='service' id='my.custom.logger'/>`)    
- `method="createRotating"` bedeutet, dass jeden Tag eine neue Log-Datei erstellt wird.
