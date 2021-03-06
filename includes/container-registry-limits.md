---
title: bestand opnemen
description: bestand opnemen
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 05/18/2020
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 19d0be6a032868c6683cd5b6bbfa7f07306171fb
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2020
ms.locfileid: "83683442"
---
| Resource | Basic | Standard | Premium |
|---|---|---|---|
| Opslag<sup>1</sup> | 10 GiB | 100 GiB| 500 GiB |
| Maximale grootte van de afbeelding slaag | 200 GiB | 200 GiB | 200 GiB |
| ReadOps per minuut<sup>2, 3</sup> | 1000 | 3000 | 10.000 |
| WriteOps per minuut<sup>2, 4</sup> | 100 | 500 | 2.000 |
| Down load bandbreedte MBps<sup>2</sup> | 30 | 60 | 100 |
| Upload bandbreedte MBps<sup>2</sup> | 10 | 20 | 50 |
| Webhooks | 2 | 10 | 500 |
| Geo-replicatie | N.v.t. | N.v.t. | [Geboden][geo-replication] |
| Inhoud vertrouwen | N.v.t. | N.v.t. | [Geboden][content-trust] |
| Persoonlijke koppeling met persoonlijke eind punten | N.v.t. | N.v.t. | [Geboden][plink] |
| Toegang tot de service Endpoint VNet | N.v.t. | N.v.t. | [Voorbeeld][vnet] |
| Door klant beheerde sleutels | N.v.t. | N.v.t. | [Geboden][cmk] |
| Machtigingen voor opslag plaatsen-scoped | N.v.t. | N.v.t. | [Voorbeeld][token]|
| &bull;Tokens | N.v.t. | N.v.t. | 20.000 |
| &bull;Scope toewijzingen | N.v.t. | N.v.t. | 20.000 |
| &bull;Opslag plaatsen per scope toewijzing | N.v.t. | N.v.t. | 500 |


<sup>1</sup> De opgegeven opslag limieten zijn de hoeveelheid *opgenomen* opslag ruimte voor elke laag. Boven deze limieten wordt een extra dagelijks tarief per GiB in rekening gebracht voor afbeeldings opslag. Zie voor informatie over de rente [Azure container Registry prijzen][pricing].

<sup>2</sup>*ReadOps*, *WriteOps*en *band breedte* zijn minimale schattingen. Azure Container Registry streeft naar het verbeteren van de prestaties als vereist gebruik.

<sup>3</sup> Een [docker-pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) -bewerking wordt omgezet naar meerdere Lees bewerkingen op basis van het aantal lagen in de afbeelding, plus het manifest dat wordt opgehaald.

<sup>4</sup> Een [docker-push](https://docs.docker.com/registry/spec/api/#pushing-an-image) wordt omgezet naar meerdere schrijf bewerkingen, op basis van het aantal lagen dat moet worden gepusht. A `docker push` bevat *ReadOps* voor het ophalen van een manifest voor een bestaande installatie kopie.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md
[vnet]: ../articles/container-registry/container-registry-vnet.md
[plink]: ../articles/container-registry/container-registry-private-link.md
[cmk]: ../articles/container-registry/container-registry-customer-managed-keys.md
[token]: ../articles/container-registry/container-registry-repository-scoped-permissions.md