---
title: Aanbiedings status ophalen | Azure Marketplace
description: API haalt de huidige status van de aanbieding op.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: 9cf6ca27101a08ff58f32dcd31413256762490a2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "81255908"
---
# <a name="retrieve-offer-status"></a>Aanbiedingsstatus ophalen

> [!NOTE]
> De Cloud Partner-portal-Api's zijn geïntegreerd met partner centrum en blijven werken nadat uw aanbiedingen zijn gemigreerd naar het partner centrum. De integratie introduceert kleine wijzigingen. Bekijk de wijzigingen die worden vermeld in [Cloud Partner-Portal API-referentie](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) om ervoor te zorgen dat uw code blijft werken na de migratie naar het partner centrum.

Hiermee wordt de huidige status van de aanbieding opgehaald.

  `GET  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/status?api-version=2017-10-31`

## <a name="uri-parameters"></a>URI-para meters

|  **Naam**       |   **Beschrijving**                            |  **Gegevens type** |
|  -------------  |  ------------------------------------------  |  ------------  |
|  publisherId    | Uitgevers-id, bijvoorbeeld`Contoso`  |     Tekenreeks     |
|  offerId        | GUID die de aanbieding uniek identificeert      |     Tekenreeks     |
|  api-versie    | Nieuwste versie van API                        |     Date       |
|  |  |


## <a name="header"></a>Header


|  Naam           |  Waarde               |
|  -------------  | -------------------  |
|  Content-Type   |  `application/json`  |
|  Autorisatie  | `Bearer YOUR_TOKEN`  |
|  |  |

## <a name="body-example"></a>Voor beeld van tekst


### <a name="response"></a>Antwoord

``` json
  {
      "status": "succeeded",
      "messages": [],
      "steps": [
      {
          "estimatedTimeFrame": "< 15 min",
          "id": "displaydummycertify",
          "stepName": "Validate Pre-Requisites",
          "description": "Offer settings provided are validated.",
          "status": "complete",
          "messages": [
              {
                  "messageHtml": "Step completed.",
                  "level": "information",
                  "timestamp": "2018-03-16T17:50:45.7215661Z"
              }
          ],       
          "progressPercentage": 100
      },
      {
          "estimatedTimeFrame": "~2-3 days",
          "id": "displaycertify",
          "stepName": "Certification",
          "description": "Your offer is analyzed by our certification systems for issues.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 day",
          "id": "displayprovision",
          "stepName": "Provisioning",
          "description": "Your virtual machine is being replicated in our production systems.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 hour",
          "id": "displaypackage",
          "stepName": "Packaging and Lead Generation Registration",
          "description": "Your virtual machine is being packaged for customers. Additionally, lead systems are being configured and set up.",
          "status": "notStarted",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "< 1 hour",
          "id": "publisher-signoff",
          "stepName": "Publisher signoff",
          "description": "Offer is available to preview. Ensure that everything looks good before making your offer live.",
          "status": "complete",
          "messages": [],
          "progressPercentage": 0
      },
      {
          "estimatedTimeFrame": "~2-5 days",
          "id": "live",
          "stepName": "Live",
          "description": "Offer is publicly visible and is available for purchase.",
          "status": "complete",
          "messages": [],
          "progressPercentage": 0
      }
      ],
      "previewLinks": [],
      liveLinks": [],
  }
```


### <a name="response-body-properties"></a>Eigenschappen van antwoord tekst

|  **Naam**             |    **Beschrijving**                                                                             |
| --------------------  |   -------------------------------------------------------------------------------------------- |
|  status               | De status van de aanbieding. Voor een lijst met mogelijke waarden, zie de status van de [aanbieding](#offer-status) hieronder. |
|  berichten             | Matrix van berichten die aan de aanbieding zijn gekoppeld                                                    |
|  stappen                | Matrix van de stappen die de aanbieding doorloopt tijdens een aanbiedings publicatie                      |
|  estimatedTimeFrame   | Schatting van de tijd die nodig is om deze stap te volt ooien, met een beschrijvende notatie                       |
|  id                   | Id van de stap                                                                         |
|  Stapnaam             | De naam van de stap                                                                               |
|  description          | Beschrijving van de stap                                                                        |
|  status               | De status van de stap. Zie [stap status](#step-status) hieronder voor een lijst met mogelijke waarden.    |
|  berichten             | Matrix van berichten met betrekking tot de stap                                                          |
|  processPercentage    | Voltooiings percentage van de stap                                                              |
|  previewLinks         | *Momenteel niet geïmplementeerd*                                                                    |
|  liveLinks            | *Momenteel niet geïmplementeerd*                                                                    |
|  notificationEmails   | Afgeschaft voor aanbiedingen die naar het partner centrum zijn gemigreerd. E-mail meldingen voor gemigreerde aanbiedingen worden verzonden naar de e-mail die is opgegeven onder de contact gegevens van de verkoper in account instellingen.<br><br>Voor niet-gemigreerde aanbiedingen, een door komma's gescheiden lijst met e-mail adressen die moeten worden gewaarschuwd voor de voortgang van de bewerking        |
|  |  |

### <a name="response-status-codes"></a>Antwoord status codes

| **Gecodeerd** |   **Beschrijving**                                                                                 |
| -------  |   ----------------------------------------------------------------------------------------------- |
|  200     |  `OK`-De aanvraag is verwerkt en de huidige status van de aanbieding is geretourneerd. |
|  400     | `Bad/Malformed request`-De hoofd tekst van het fout bericht bevat mogelijk meer informatie.                 |
|  404     | `Not found`-De opgegeven entiteit bestaat niet.                                                |
|  |  |

### <a name="offer-status"></a>Status van aanbieding

|  **Naam**                    |    **Beschrijving**                                       |
|  --------------------------  |  ------------------------------------------------------  |
|  NeverPublished              | De aanbieding is nooit gepubliceerd.                          |
|  NotStarted                  | De aanbieding is nieuw en niet gestart.                            |
|  WaitingForPublisherReview   | Aanbieding wacht op goed keuring van de uitgever.                 |
|  In uitvoering                     | Verzen ding van aanbieding wordt verwerkt.                     |
|  Geslaagd                   | Het verzenden van aanbiedingen is voltooid.               |
|  Geannuleerd                    | Het verzenden van het aanbod is geannuleerd.                           |
|  Mislukt                      | Kan de aanbieding niet verzenden.                                 |
|  |  |

### <a name="step-status"></a>Stap status

|  **Naam**                    |    **Beschrijving**                           |
|  -------------------------   |  ------------------------------------------  |
|  NotStarted                  | De stap is niet gestart.                        |
|  InProgress                  | Stap wordt uitgevoerd.                             |
|  WaitingForPublisherReview   | Stap wacht op goed keuring van uitgever.      |
|  WaitingForApproval          | De stap wacht op goed keuring van het proces.        |
|  Geblokkeerd                     | De stap is geblokkeerd.                             |
|  Geweigerd                    | De stap is geweigerd.                            |
|  Voltooien                    | Stap is voltooid.                            |
|  Geannuleerd                    | De stap is geannuleerd.                           |
|  |  |
