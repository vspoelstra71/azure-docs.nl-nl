---
title: SaaS-fulfillment-Api's v2 in micro soft Commercial Marketplace
description: Meer informatie over het maken en beheren van een SaaS-aanbieding op Microsoft AppSource en Azure Marketplace met behulp van de fulfillment Api's versie 2.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 10/18/2019
ms.author: dsindona
ms.openlocfilehash: 8f452349c0d636bc74a3722e2526623c955093f7
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2020
ms.locfileid: "82854185"
---
# <a name="saas-fulfillment-apis-version-2-in-microsoft-commercial-marketplace"></a>SaaS-fulfillment-Api's versie 2 in micro soft Commercial Marketplace

In dit artikel vindt u informatie over de fulfillment-Api's waarmee partners hun SaaS-toepassingen kunnen verkopen in Microsoft AppSource en Azure Marketplace. Deze Api's zijn vereist voor transactable SaaS-aanbiedingen.

## <a name="managing-the-saas-subscription-life-cycle"></a>De levens cyclus van het SaaS-abonnement beheren

Azure SaaS beheert de volledige levens cyclus van een SaaS-abonnements aankoop. Het gebruikt de fulfillment-Api's als mechanisme om de werkelijke uitvoering, wijzigingen in plannen en het verwijderen van het abonnement met de partner te best uren. De factuur van de klant is gebaseerd op de status van de SaaS-abonnement die micro soft onderhoudt. In het volgende diagram ziet u de statussen en de bewerkingen die de wijzigingen tussen de statussen aansturen.

![Levens cyclus status van SaaS-abonnement](./media/saas-subscription-lifecycle-api-v2.png)


### <a name="states-of-a-saas-subscription"></a>Statussen van een SaaS-abonnement

De volgende tabel geeft een lijst van de inrichtings statussen voor een SaaS-abonnement, met inbegrip van een beschrijving en sequentie diagram voor elke (indien van toepassing). 

#### <a name="provisioning"></a>Inrichten

Wanneer een klant een aankoop initieert, ontvangt de partner deze informatie in een autorisatie code op een klant-interactieve webpagina die gebruikmaakt van een URL-para meter. Een voor beeld `https://contoso.com/signup?token=..`is dat de URL van de landings pagina in `https://contoso.com/signup`het partner centrum. De autorisatie code kan worden gevalideerd en uitgewisseld voor de details van de inrichtings service door de API voor het oplossen van conflicten aan te roepen.  Wanneer een SaaS-service is ingericht, verzendt deze een activerings aanroep om aan te geven dat de uitvoering is voltooid en dat de klant kan worden gefactureerd. 

In het volgende diagram ziet u de volg orde van API-aanroepen voor een inrichtings scenario.  

![API-aanroepen voor het inrichten van een SaaS-service](./media/saas-post-provisioning-api-v2-calls.png)

#### <a name="provisioned"></a>Ingericht

Deze status is de stabiele status van een ingerichte service.

##### <a name="provisioning-for-update"></a>Inrichten voor update 

Deze status geeft aan dat een update voor een bestaande service in behandeling is. Een dergelijke update kan worden geïnitieerd door de klant, hetzij via de Marketplace ofwel via de SaaS-service (alleen voor directe trans acties van klanten).

##### <a name="provisioning-for-update-when-its-initiated-from-the-marketplace"></a>Inrichten voor update (wanneer deze vanuit Marketplace wordt gestart)

In het volgende diagram ziet u de volg orde van de acties wanneer een update wordt gestart vanuit de Marketplace.

![API-aanroepen wanneer de update wordt gestart vanuit Marketplace](./media/saas-update-api-v2-calls-from-marketplace-a.png)

##### <a name="provisioning-for-update-when-its-initiated-from-the-saas-service"></a>Inrichten voor update (wanneer deze wordt gestart vanuit de SaaS-service)

In het volgende diagram ziet u de acties wanneer een update wordt gestart vanuit de SaaS-service. (De webhook-aanroep wordt vervangen door een update van het abonnement dat door de SaaS-service is gestart.) 

![API-aanroepen wanneer de update wordt gestart vanuit de SaaS-service](./media/saas-update-api-v2-calls-from-saas-service-a.png) 

#### <a name="suspended"></a>Onderbroken

Deze status geeft aan dat de betaling van een klant niet is ontvangen. Op basis van het beleid geven we de klant een respijt periode voordat ze het abonnement annuleren. Wanneer een abonnement de volgende status heeft: 

- Als partner kunt u ervoor kiezen om de toegang van de gebruiker tot de service te degraderen of te blok keren.
- Het abonnement moet worden bewaard met een herstel bare status waarmee de volledige functionaliteit kan worden hersteld zonder verlies van gegevens of instellingen. 
- U ontvangt een aanvraag voor het opnieuw invoeren van dit abonnement via de fulfillment-Api's of een aanvraag voor het ongedaan maken van de inrichting aan het einde van de respijt periode. 

#### <a name="unsubscribed"></a>Afgemeld 

Abonnementen bereiken deze status als reactie op een expliciete aanvraag van een klant of op niet-betaalde contributie. De verwachting van de partner is dat de gegevens van de klant worden bewaard voor herstel op aanvraag voor een bepaald aantal dagen en vervolgens worden verwijderd. 


## <a name="api-reference"></a>API-verwijzing

In deze sectie worden de *API* voor SaaS-abonnementen en *Operations API*gedocumenteerd.  De waarde van de `api-version` para meter voor versie 2- `2018-08-31`api's is.  


### <a name="parameter-and-entity-definitions"></a>Para meter-en entiteits definities

De volgende tabel bevat de definities voor algemene para meters en entiteiten die worden gebruikt door fulfillment-Api's.

|     Entiteit/para meter     |     Definitie                         |
|     ----------------     |     ----------                         |
| `subscriptionId`         | De GUID-id voor een SaaS-resource.  |
| `name`                   | Een beschrijvende naam voor deze resource door de klant. |
| `publisherId`            | Een unieke teken reeks-id voor elke uitgever (bijvoorbeeld: contoso). |
| `offerId`                | Een unieke teken reeks-id voor elke aanbieding (bijvoorbeeld: "Offer1").  |
| `planId`                 | Een unieke teken reeks-id voor elk abonnement/SKU (bijvoorbeeld: ' zilver '). |
| `operationId`            | De GUID-id voor een bepaalde bewerking.  |
|  `action`                | De actie die wordt uitgevoerd voor een resource, `Unsubscribe`ofwel `Suspend`, `Reinstate`, of `ChangePlan`, `ChangeQuantity`, `Transfer`. |
|   |   |

[Guid's](https://en.wikipedia.org/wiki/Universally_unique_identifier)(Globally Unique Identifiers) zijn 128-bits (32-hexadecimale) getallen die doorgaans automatisch worden gegenereerd. 

#### <a name="resolve-a-subscription"></a>Een abonnement oplossen 

Met het eind punt voor omzetten kan de uitgever een Marketplace-token omzetten in een permanente Resource-ID. De resource-ID is de unieke id voor een SaaS-abonnement. Wanneer een gebruiker wordt omgeleid naar de website van een partner, bevat de URL een token in de query parameters. De partner wordt verwacht dit token te gebruiken en een aanvraag te doen om het te verhelpen. Het antwoord bevat de unieke SaaS-abonnements-ID, de naam, de aanbiedings-ID en het plan voor de resource. Dit token is slechts één uur geldig. 

##### <a name="postbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionsresolveapi-versionapiversion"></a>Plaatsen<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=<ApiVersion>`

*Query parameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  De versie van de bewerking die moet worden gebruikt voor deze aanvraag.  |

*Aanvraag headers:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-MS-aanvraag-out    |  Een unieke teken reeks waarde voor het bijhouden van de aanvraag van de client, bij voor keur een GUID. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers. |
|  x-MS-correlationid |  Een unieke teken reeks waarde voor de bewerking op de client. Deze para meter verbindt alle gebeurtenissen van de client bewerking met gebeurtenissen aan de server zijde. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers.  |
|  autorisatie     |  Het [JWT-Bearer-token (JSON Web token) ophalen](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). Bijvoorbeeld: "`Bearer <access_token>`". |
|  x-MS-Marketplace-token  |  De token query-para meter in de URL wanneer de gebruiker wordt omgeleid naar de website van de SaaS-partner vanuit Azure ( `https://contoso.com/signup?token=..`bijvoorbeeld:). *Opmerking:* De URL decodeert de token waarde uit de browser voordat u deze gebruikt.  |

*Antwoord codes:*

Code: 200<br>
Hiermee wordt het ondoorzichtige token omgezet in een SaaS-abonnement. Antwoord tekst:
 

```json
{
    "id": "<guid>",  
    "subscriptionName": "Contoso Cloud Solution",
    "offerId": "offer1",
    "planId": "silver",
    "quantity": "20" 
}
```

Code: 400<br>
Ongeldige aanvraag. x-MS-Marketplace-token ontbreekt, is ongeldig of is verlopen.

Code: 403<br>
Gasten. Het verificatie token is niet opgegeven of is ongeldig, of er wordt geprobeerd toegang te krijgen tot een overname die geen deel uitmaakt van de huidige uitgever.

Code: 404<br>
Niet gevonden.

Code: 500<br>
Interne serverfout.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

### <a name="subscription-api"></a>API voor abonnementen

De API voor abonnementen ondersteunt de volgende HTTPS-bewerkingen: **Get**, **post**, **patch**en **Delete**.


#### <a name="list-subscriptions"></a>Abonnementen weer geven

Een lijst met alle SaaS-abonnementen voor een uitgever.

##### <a name="getbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionsapi-versionapiversion"></a>Ophalen<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=<ApiVersion>`

*Query parameters:*

|             |                   |
|  --------   |  ---------------  |
| ApiVersion  |  De versie van de bewerking die moet worden gebruikt voor deze aanvraag.  |

*Aanvraag headers:*

|                    |                   |
|  ---------------   |  ---------------  |
| Content-Type       |  `application/json`  |
| x-MS-aanvraag-out     |  Een unieke teken reeks waarde voor het bijhouden van de aanvraag van de client, bij voor keur een GUID. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers. |
| x-MS-correlationid |  Een unieke teken reeks waarde voor de bewerking op de client. Deze para meter verbindt alle gebeurtenissen van de client bewerking met gebeurtenissen aan de server zijde. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers.  |
| autorisatie      |  Het [JWT-Bearer-token (JSON Web token) ophalen](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). Bijvoorbeeld: "`Bearer <access_token>`".  |

*Antwoord codes:*

Code: 200 <br/>
Hiermee haalt u de uitgever en de bijbehorende abonnementen op voor alle aanbiedingen van de uitgever op basis van het verificatie token.

>[!Note]
>[Model-api's](#mock-apis) worden gebruikt wanneer u de aanbieding voor het eerst ontwikkelt, terwijl echte api's moeten worden gebruikt bij het publiceren van de aanbieding.  Echte Api's en model-Api's wijken af van de eerste regel van de code.  In de echte API bevindt `subscription` zich het gedeelte, terwijl deze sectie niet bestaat voor de model-API.

Reactie Payload voor de model-API:<br>

```json
{
  [
      {
          "id": "<guid>",
          "name": "Contoso Cloud Solution",
          "publisherId": "contoso",
          "offerId": "offer1",
          "planId": "silver",
          "quantity": "10",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "<guid>"
          },
            "term": {
                "startDate": "2019-05-31",
                "endDate": "2019-06-29",
                "termUnit": "P1M"
          },
          "allowedCustomerOperations": [
              "Read" // Possible Values: Read, Update, Delete.
          ], // Indicates operations allowed on the SaaS subscription. For CSP-initiated purchases, this will always be Read.
          "sessionMode": "None", // Possible Values: None, DryRun (Dry Run indicates all transactions run as Test-Mode in the commerce stack)
          "isFreeTrial": "true", // true - the customer subscription is currently in free trial, false - the customer subscription is not currently in free trial.
          "saasSubscriptionStatus": "Subscribed" // Indicates the status of the operation: [NotStarted, PendingFulfillmentStart, Subscribed, Suspended, Unsubscribed]
      }
  ],
  "continuationToken": ""
}
```
En voor echte API: <br>

```json
{
  "subscriptions": [
      {
          "id": "<guid>",
          "name": "Contoso Cloud Solution",
          "publisherId": "contoso",
          "offerId": "offer1",
          "planId": "silver",
          "quantity": "10",
          "beneficiary": { // Tenant, object id and email address for which SaaS subscription is purchased.
              "emailId": "<email>",
              "objectId": "<guid>",                     
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant, object id and email address that purchased the SaaS subscription. These could be different for reseller scenario
              "emailId": "<email>",
              "objectId": "<guid>",                      
              "tenantId": "<guid>"
          },
            "term": {
                "startDate": "2019-05-31",
                "endDate": "2019-06-29",
                "termUnit": "P1M"
          },
          "allowedCustomerOperations": [
              "Read" // Possible Values: Read, Update, Delete.
          ], // Indicates operations allowed on the SaaS subscription. For CSP-initiated purchases, this will always be Read.
          "sessionMode": "None", // Possible Values: None, DryRun (Dry Run indicates all transactions run as Test-Mode in the commerce stack)
          "isFreeTrial": true, // true - the customer subscription is currently in free trial, false - the customer subscription is not currently in free trial.(optional field - default false)
          "isTest": false, //indicating whether the current subscription is a test asset
          "sandboxType": "None", // Possible Values: None, Csp (Csp sandbox purchase)
          "saasSubscriptionStatus": "Subscribed" // Indicates the status of the operation: [NotStarted, PendingFulfillmentStart, Subscribed, Suspended, Unsubscribed]
      }
  ],
  "@nextLink": ""
}
```
Het vervolg token wordt alleen weer gegeven als er extra ' pagina's ' van de plannen zijn die moeten worden opgehaald. 

Code: 403 <br>
Gasten. Het verificatie token is niet opgegeven of is ongeldig, of er wordt geprobeerd toegang te krijgen tot een overname die geen deel uitmaakt van de huidige uitgever. 

Code: 500<br>
Interne serverfout.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="get-subscription"></a>Abonnement ophalen

Hiermee wordt het opgegeven SaaS-abonnement opgehaald. Gebruik deze aanroep om licentie gegevens op te halen en informatie te plannen.

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Ophalen<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Query parameters:*

|                    |                   |
|  ---------------   |  ---------------  |
| subscriptionId     |   Een unieke id van het SaaS-abonnement dat is verkregen na het oplossen van het token via de API voor omzetten.   |
|  ApiVersion        |   De versie van de bewerking die moet worden gebruikt voor deze aanvraag.   |

*Aanvraag headers:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      |  `application/json`  |
|  x-MS-aanvraag-out    |  Een unieke teken reeks waarde voor het bijhouden van de aanvraag van de client, bij voor keur een GUID. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers. |
|  x-MS-correlationid |  Een unieke teken reeks waarde voor de bewerking op de client. Deze para meter verbindt alle gebeurtenissen van de client bewerking met gebeurtenissen aan de server zijde. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers.  |
|  autorisatie     |  Het [JWT-Bearer-token (JSON Web token) ophalen](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). Bijvoorbeeld: "`Bearer <access_token>`".  |

*Antwoord codes:*

Code: 200<br>
Hiermee wordt het SaaS-abonnement opgehaald uit de id. Nettolading van reactie:<br>

```json
Response Body:
{ 
        "id":"",
        "name":"Contoso Cloud Solution",
        "publisherId": "contoso",
        "offerId": "offer1",
        "planId": "silver",
        "quantity": "10",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "<guid>"
          },
        "allowedCustomerOperations": ["Read"], // Indicates operations allowed on the SaaS subscription. For CSP-initiated purchases, this will always be Read.
        "sessionMode": "None", // Dry Run indicates all transactions run as Test-Mode in the commerce stack
        "isFreeTrial": "true", // true - customer subscription is currently in free trial, false - customer subscription is not currently in free trial.
        "status": "Subscribed", // Indicates the status of the operation.
          "term": { //This gives the free trial term start and end date
            "startDate": "2019-05-31",
            "endDate": "2019-06-29",
            "termUnit": "P1M" //where P1M: Monthly, P1Y: Yearly 
        },
}
```

Code: 403<br>
Gasten. Het verificatie token is niet opgegeven of is ongeldig, of er wordt geprobeerd toegang te krijgen tot een overname die geen deel uitmaakt van de huidige uitgever.

Code: 404<br>
Niet gevonden.<br> 

Code: 500<br>
Interne serverfout.<br>

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }  
```

#### <a name="list-available-plans"></a>Beschik bare abonnementen weer geven

Gebruik deze aanroep om te achterhalen of er privé-of open bare aanbiedingen voor de huidige uitgever zijn.

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidlistavailableplansapi-versionapiversion"></a>Ophalen<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/listAvailablePlans?api-version=<ApiVersion>`

*Query parameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |   De versie van de bewerking die moet worden gebruikt voor deze aanvraag.  |

*Aanvraag headers:*

|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|   x-MS-aanvraag-out   |   Een unieke teken reeks waarde voor het bijhouden van de aanvraag van de client, bij voor keur een GUID. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers. |
|  x-MS-correlationid  | Een unieke teken reeks waarde voor de bewerking op de client. Deze para meter verbindt alle gebeurtenissen van de client bewerking met gebeurtenissen aan de server zijde. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers. |
|  autorisatie     |  Het [JWT-Bearer-token (JSON Web token) ophalen](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  Bijvoorbeeld: "`Bearer <access_token>`". |

*Antwoord codes:*

Code: 200<br>
Hiermee wordt een lijst met beschik bare abonnementen voor een klant opgehaald. Antwoord tekst:

```json
{
    "plans": [{
        "planId": "Platinum001",
        "displayName": "Private platinum plan for Contoso",
        "isPrivate": true
    }]
}
```

Code: 404<br>
Niet gevonden.<br> 

Code: 403<br>
Gasten. Het verificatie token is niet opgegeven of is ongeldig, of er wordt geprobeerd toegang te krijgen tot een overname die geen deel uitmaakt van de huidige uitgever. <br> 

Code: 500<br>
Interne serverfout.<br>

```json
{ 
    "error": { 
      "code": "UnexpectedError", 
      "message": "An unexpected error has occurred." 
    } 
```

#### <a name="activate-a-subscription"></a>Een abonnement activeren

##### <a name="postbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidactivateapi-versionapiversion"></a>Plaatsen<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/activate?api-version=<ApiVersion>`

*Query parameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  De versie van de bewerking die moet worden gebruikt voor deze aanvraag.  |
| subscriptionId     | Een unieke id van het SaaS-abonnement dat is verkregen na het oplossen van het token met behulp van de API voor omzetten.  |

*Aanvraag headers:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json`  |
|  x-MS-aanvraag-out    | Een unieke teken reeks waarde voor het bijhouden van de aanvraag van de client, bij voor keur een GUID. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers.  |
|  x-MS-correlationid  | Een unieke teken reeks waarde voor de bewerking op de client. Deze teken reeks correleert alle gebeurtenissen van de client bewerking met gebeurtenissen aan de server zijde. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers.  |
|  autorisatie     |  Het [JWT-Bearer-token (JSON Web token) ophalen](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  Bijvoorbeeld: "`Bearer <access_token>`". |

*Lading van aanvraag:*

```json
{
    "planId": "gold",
    "quantity": ""
}
```

*Antwoord codes:*

Code: 200<br>
Hiermee activeert u het abonnement.<br>

Code: 400<br>
Ongeldige aanvraag: validatie mislukt.

Code: 403<br>
Gasten. Het verificatie token is niet opgegeven of is ongeldig, of er wordt geprobeerd toegang te krijgen tot een overname die geen deel uitmaakt van de huidige uitgever.

Code: 404<br>
Niet gevonden.

Code: 500<br>
Interne serverfout.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="change-the-plan-on-the-subscription"></a>Wijzig het abonnement

Werk het abonnement op de abonnementen bij.

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Patch<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Query parameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  De versie van de bewerking die moet worden gebruikt voor deze aanvraag.  |
| subscriptionId     | Een unieke id van het SaaS-abonnement dat is verkregen na het oplossen van het token met behulp van de API voor omzetten.  |

*Aanvraag headers:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-MS-aanvraag-out    |   Een unieke teken reeks waarde voor het bijhouden van de aanvraag van de client, bij voor keur een GUID. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers.  |
|  x-MS-correlationid  |  Een unieke teken reeks waarde voor de bewerking op de client. Deze para meter verbindt alle gebeurtenissen van de client bewerking met gebeurtenissen aan de server zijde. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers.    |
| autorisatie      |  Het [JWT-Bearer-token (JSON Web token) ophalen](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  Bijvoorbeeld: "`Bearer <access_token>`".  |

*Lading van aanvraag:*

```json
Request Body:
{
    "planId": "gold"
}
```

*Aanvraag headers:*

|                    |                   |
|  ---------------   |  ---------------  |
| Bewerkings locatie | De koppeling naar een resource om de status van de bewerking op te halen.   |

*Antwoord codes:*

Code: 202<br>
De aanvraag voor het wijzigings plan is geaccepteerd. Er wordt naar verwachting de bewerkings locatie moet worden gecontroleerd om te bepalen of de partner is geslaagd of mislukt. <br>

Code: 400<br>
Ongeldige aanvraag: validatie mislukt.

Code: 403<br>
Gasten. Het verificatie token is niet opgegeven of is ongeldig, of er wordt geprobeerd toegang te krijgen tot een overname die geen deel uitmaakt van de huidige uitgever.

Code: 404<br>
Niet gevonden.

Code: 500<br>
Interne serverfout.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

>[!Note]
>Alleen een plan of hoeveelheid kan in één keer worden bijgewerkt, niet beide. Bewerkingen voor een abonnement met **Update** zijn niet aanwezig `allowedCustomerOperations`in.

#### <a name="change-the-quantity-on-the-subscription"></a>De hoeveelheid op het abonnement wijzigen

Werk de hoeveelheid op het abonnement bij.

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Verzenden<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Query parameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  De versie van de bewerking die moet worden gebruikt voor deze aanvraag.  |
| subscriptionId     | Een unieke id van het SaaS-abonnement dat is verkregen na het oplossen van het token met behulp van de API voor omzetten.  |

*Aanvraag headers:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-MS-aanvraag-out    |   Een unieke teken reeks waarde voor het bijhouden van de aanvraag van de client, bij voor keur een GUID. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers.  |
|  x-MS-correlationid  |  Een unieke teken reeks waarde voor de bewerking op de client. Deze para meter verbindt alle gebeurtenissen van de client bewerking met gebeurtenissen aan de server zijde. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers.    |
| autorisatie      |  Het [JWT-Bearer-token (JSON Web token) ophalen](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  Bijvoorbeeld: "`Bearer <access_token>`".  |

*Lading van aanvraag:*

```json
Request Body:
{
    "quantity": 5
}
```

*Aanvraag headers:*

|                    |                   |
|  ---------------   |  ---------------  |
| Bewerkings locatie | Koppeling naar een resource om de status van de bewerking op te halen.   |

*Antwoord codes:*

Code: 202<br>
De aanvraag voor het wijzigen van de hoeveelheid is geaccepteerd. Er wordt naar verwachting de bewerkings locatie moet worden gecontroleerd om te bepalen of de partner is geslaagd of mislukt. <br>

Code: 400<br>
Ongeldige aanvraag: validatie mislukt.


Code: 403<br>
Gasten. Het verificatie token is niet opgegeven of is ongeldig, of er wordt geprobeerd toegang te krijgen tot een overname die geen deel uitmaakt van de huidige uitgever.

Code: 404<br>
Niet gevonden.

Code: 500<br>
Interne serverfout.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

>[!Note]
>Alleen een plan of hoeveelheid kan in één keer worden bijgewerkt, niet beide. Bewerkingen voor een abonnement met **Update** zijn niet aanwezig `allowedCustomerOperations`in.

#### <a name="delete-a-subscription"></a>Een abonnement verwijderen

Meld u af en verwijder het opgegeven abonnement.

##### <a name="deletebr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Verwijderen<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Query parameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  De versie van de bewerking die moet worden gebruikt voor deze aanvraag.  |
| subscriptionId     | Een unieke id van het SaaS-abonnement dat is verkregen na het oplossen van het token met behulp van de API voor omzetten.  |

*Aanvraag headers:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|  x-MS-aanvraag-out    |   Een unieke teken reeks waarde voor het bijhouden van de aanvraag van de client, bij voor keur een GUID. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers.   |
|  x-MS-correlationid  |  Een unieke teken reeks waarde voor de bewerking op de client. Deze para meter verbindt alle gebeurtenissen van de client bewerking met gebeurtenissen aan de server zijde. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers.   |
|  autorisatie     |  Het [JWT-Bearer-token (JSON Web token) ophalen](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  Bijvoorbeeld: "`Bearer <access_token>`".  |

*Antwoord codes:*

Code: 202<br>
De partner initieerde een aanroep voor het afmelden van een SaaS-abonnement.<br>

Code: 400<br>
Verwijderen van een abonnement met **verwijderen** niet in `allowedCustomerOperations`.

Code: 403<br>
Gasten. Het verificatie token is niet opgegeven of is ongeldig, of er wordt geprobeerd toegang te krijgen tot een overname die geen deel uitmaakt van de huidige uitgever.

Code: 404<br>
Niet gevonden.

Code: 500<br>
Interne serverfout.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```


### <a name="operations-api"></a>Operations-API

De operations API ondersteunt de volgende patch-en Get-bewerkingen.

#### <a name="list-outstanding-operations"></a>Openstaande bewerkingen weer geven 

Geeft een lijst van openstaande bewerkingen voor de huidige uitgever. 

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsapi-versionapiversion"></a>Ophalen<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations?api-version=<ApiVersion>`

*Query parameters:*

|             |        |
|  ---------------   |  ---------------  |
|    ApiVersion                |   De versie van de bewerking die moet worden gebruikt voor deze aanvraag.                |
| subscriptionId     | Een unieke id van het SaaS-abonnement dat is verkregen na het oplossen van het token met behulp van de API voor omzetten.  |

*Aanvraag headers:*
 
|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|  x-MS-aanvraag-out    |  Een unieke teken reeks waarde voor het bijhouden van de aanvraag van de client, bij voor keur een GUID. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers.  |
|  x-MS-correlationid |  Een unieke teken reeks waarde voor de bewerking op de client. Deze para meter verbindt alle gebeurtenissen van de client bewerking met gebeurtenissen aan de server zijde. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers.  |
|  autorisatie     |  Het [JWT-Bearer-token (JSON Web token) ophalen](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  Bijvoorbeeld: "`Bearer <access_token>`".  |

*Antwoord codes:*

Code: 200<br> Hiermee wordt de lijst met openstaande bewerkingen voor een abonnement opgehaald. Nettolading van reactie:

```json
[{
    "id": "<guid>",  
    "activityId": "<guid>",
    "subscriptionId": "<guid>",
    "offerId": "offer1",
    "publisherId": "contoso",  
    "planId": "silver",
    "quantity": "20",
    "action": "Convert",
    "timeStamp": "2018-12-01T00:00:00",  
    "status": "NotStarted"  
}]
```


Code: 400<br>
Ongeldige aanvraag: validatie mislukt.

Code: 403<br>
Gasten. Het verificatie token is niet opgegeven of is ongeldig, of er wordt geprobeerd toegang te krijgen tot een overname die geen deel uitmaakt van de huidige uitgever.

Code: 404<br>
Niet gevonden.

Code: 500<br>
Interne serverfout.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

#### <a name="get-operation-status"></a>Bewerkings status ophalen

Hiermee kan de uitgever de status van de opgegeven geactiveerde async-bewerking ( `Subscribe`zoals, `Unsubscribe`, `ChangePlan`, of `ChangeQuantity`) volgen.

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsoperationidapi-versionapiversion"></a>Ophalen<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`

*Query parameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  De versie van de bewerking die moet worden gebruikt voor deze aanvraag.  |

*Aanvraag headers:*

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      |  `application/json`   |
|  x-MS-aanvraag-out    |   Een unieke teken reeks waarde voor het bijhouden van de aanvraag van de client, bij voor keur een GUID. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers.  |
|  x-MS-correlationid |  Een unieke teken reeks waarde voor de bewerking op de client. Deze para meter verbindt alle gebeurtenissen van de client bewerking met gebeurtenissen aan de server zijde. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers.  |
|  autorisatie     |  Het [JWT-Bearer-token (JSON Web token) ophalen](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). Bijvoorbeeld: "`Bearer <access_token>`".  |

*Antwoord codes:*<br>

Code: 200<br> Hiermee wordt de opgegeven SaaS-bewerking in behandeling opgehaald. Nettolading van reactie:

```json
Response body:
{
    "id  ": "<guid>",
    "activityId": "<guid>",
    "subscriptionId":"<guid>",
    "offerId": "offer1",
    "publisherId": "contoso",  
    "planId": "silver",
    "quantity": "20",
    "action": "Convert",
    "timeStamp": "2018-12-01T00:00:00",
    "status": "NotStarted"
}

```

Code: 400<br>
Ongeldige aanvraag: validatie mislukt.

Code: 403<br>
Gasten. Het verificatie token is niet opgegeven of is ongeldig, of er wordt geprobeerd toegang te krijgen tot een overname die geen deel uitmaakt van de huidige uitgever.
 
Code: 404<br>
Niet gevonden.

Code: 500<br> Interne serverfout.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```
#### <a name="update-the-status-of-an-operation"></a>De status van een bewerking bijwerken

Werk de status van een bewerking bij om het slagen of mislukken van de geleverde waarden aan te geven.

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsoperationidapi-versionapiversion"></a>Patch<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`

*Query parameters:*

|                    |                   |
|  ---------------   |  ---------------  |
|   ApiVersion       |  De versie van de bewerking die moet worden gebruikt voor deze aanvraag.  |
| subscriptionId     | Een unieke id van het SaaS-abonnement dat is verkregen na het oplossen van het token met behulp van de API voor omzetten.  |
|  operationId       | De bewerking die wordt voltooid. |

*Aanvraag headers:*

|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     | `application/json`   |
|   x-MS-aanvraag-out   |   Een unieke teken reeks waarde voor het bijhouden van de aanvraag van de client, bij voor keur een GUID. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers. |
|  x-MS-correlationid |  Een unieke teken reeks waarde voor de bewerking op de client. Deze para meter verbindt alle gebeurtenissen van de client bewerking met gebeurtenissen aan de server zijde. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers. |
|  autorisatie     |  Het [JWT-Bearer-token (JSON Web token) ophalen](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  Bijvoorbeeld: "`Bearer <access_token>`".  |

*Lading van aanvraag:*

```json
{
    "planId": "offer1",
    "quantity": "44",
    "status": "Success"    // Allowed Values: Success/Failure. Indicates the status of the operation.
}

```

*Antwoord codes:*

Code: 200<br> Een aanroep om te informeren of een bewerking aan de partner zijde is voltooid. Deze reactie kan bijvoorbeeld de wijziging van de stoelen of plannen Signa leren.

Code: 400<br>
Ongeldige aanvraag: validatie mislukt.

Code: 403<br>
Gasten. Het verificatie token is niet opgegeven of is ongeldig, of er wordt geprobeerd toegang te krijgen tot een overname die geen deel uitmaakt van de huidige uitgever.

Code: 404<br>
Niet gevonden.

Code: 409<br>
Conflicteren. Er is bijvoorbeeld al aan een nieuwe trans actie voldaan.

Code: 500<br> Interne serverfout.

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

## <a name="implementing-a-webhook-on-the-saas-service"></a>Een webhook implementeren op de SaaS-service

De uitgever moet een webhook in deze SaaS-service implementeren om gebruikers proactief te informeren over wijzigingen in de service. De SaaS-service wordt verwacht de operations API aan te roepen om te valideren en goed te keuren voordat een actie wordt ondernomen voor de webhook-melding.


```json
{
  "id": "<this is a GUID operation id, you can call operations API with this to get status>",
  "activityId": "<this is a Guid correlation id>",
  "subscriptionId": "<Guid to uniquely identify this resource>",
  "publisherId": "<this is the publisher's name>",
  "offerId": "<this is the offer name>",
  "planId": "<this is the plan id>",
  "quantity": "<the number of seats, will be null if not per-seat saas offer>",
  "timeStamp": "2019-04-15T20:17:31.7350641Z",
  "action": "Unsubscribe",
  "status": "NotStarted"  

}
```
Waarbij de actie een van de volgende acties kan zijn: 
- `Unsubscribe`(wanneer de resource is verwijderd)
- `ChangePlan`(wanneer de bewerking voor het wijzigings plan is voltooid)
- `ChangeQuantity`(wanneer de bewerking hoeveelheid wijzigen is voltooid)
- `Suspend`(wanneer de resource is onderbroken)
- `Reinstate`(wanneer de bron na de onderbreking opnieuw is ingesteld)

Waar de status kan een van de volgende zijn: 
- **NotStarted** <br>
 - **InProgress** <br>
- **Is voltooid** <br>
- **Mislukt** <br>
- **Conflicteren** <br>

In een webhook-melding zijn de bewerkings statussen **geslaagd** en **mislukt**. De levens cyclus van een bewerking is van **NotStarted** naar een Terminal status, zoals **geslaagd**, **mislukt**of **conflict**. Als u **NotStarted** of **InProgress**ontvangt, kunt u door gaan met het aanvragen van de status via Get API totdat de bewerking een Terminal status bereikt voordat u actie onderneemt. 

## <a name="mock-apis"></a>Model-Api's

U kunt onze model-Api's gebruiken om aan de slag te gaan met ontwikkelen, met name prototypen, en projecten te testen. 

Host-eind `https://marketplaceapi.microsoft.com/api` punt: (geen verificatie vereist)<br/>
API-versie:`2018-09-15`<br/>
Voor beeld-URI:`https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2018-09-15` <br/>

De API-eindpunt paden zijn hetzelfde voor beide modellen en echte Api's, maar de API-versies verschillen. De versie is `2018-09-15` voor de model versie en `2018-08-31` voor de productie versie. 

Een van de API-aanroepen in dit artikel kan worden uitgevoerd naar het model host-eind punt. In het algemeen moet u gegevens weer geven als een antwoord. Aanroepen naar de methoden voor het bijwerken van abonnementen op de model-API altijd 500 retour neren. 

## <a name="next-steps"></a>Volgende stappen

Ontwikkel aars kunnen werk belastingen, aanbiedingen en Publisher-profielen ook programmatisch ophalen en manipuleren met behulp van de [Cloud Partner-Portal rest-api's](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview).
