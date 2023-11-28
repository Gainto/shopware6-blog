---
title: EntityRepository dynamisch anhand eines Namens laden
layout: post
date: '2023-10-15 20:37:56 +0100'
cover: assets/images/repo-reload.png
categories:
- entwickler
---

Jede Entität in Shopware hat ein Repository.
Die `ProductEntity` kann mit einem `ProductRepository` geladen werden, die `CustomerEntity` kann mit einem `CustomerRepository` geladen werden, etc.

Wenn ein Service oder eine Klasse nun spezifische Entitäten laden muss, wird das jeweilige Repository über die `services.xml` bzw `services.yml` injected.

Was ist aber, wenn du zum Zeitpunkt des injectens noch gar nicht weißt, welche Entität du benötigst und du zusätzlich keinen compilerPass verwenden möchtest?

In dem Fall, kannst du dynamisch das benötigte repository laden, und das geht so:

Wir nutzen die `DefinitionInstanceRegistry` und laden das repository. Die Registry benötigt dazu nur den Namen der Entity. Dieser kann entweder direkt angegeben, oder aber über ein Entity-Objekt geholt werden.

Ich nutze diese Methode z.B. zum eine Entität mit weiteren Associations neu zu laden. Damit ich nicht für jede Entität eine eigene Methode schreiben muss, nimmt meine Methode einfach die Entität entgegen und lädt sich auf Basis des Namens das benötigte Repository. Anschließend werden die Associations hinzugefügt, mittels DAL die Entität neu geladen und zurückgegeben.

Das ganze sieht dann so aus:

```php
private function reloadEntityWithAssociations(
    Entity $entity,
    array $associations,
    DefinitionInstanceRegistry $registry,
    Context $context
) : Entity{

    /** @var string $entityName */
    $entityName = ($registry->getByEntityClass($entity))->getEntityName();

    /** @var EntityRepositoryInterface $repo */
    $repo = $registry->getRepository($entityName);

    $criteria = new Criteria();
    $criteria->addFilter(new EqualsFilter("id", $entity->getId()));
    $criteria->addAssociations($associations);
    $criteria->setLimit(1);

    return ($repo->search($criteria, $context))->first();
}
```
