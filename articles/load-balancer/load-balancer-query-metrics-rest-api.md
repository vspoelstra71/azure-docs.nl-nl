---
title: Metrische gegevens ophalen met de REST API
titleSuffix: Azure Load Balancer
description: In dit artikel kunt u aan de slag met de Azure REST Api's voor het verzamelen van metrische gegevens over de status en het gebruik van Azure Load Balancer.
services: sql-database
author: asudbring
manager: KumudD
ms.service: load-balancer
ms.custom: REST, seodec18
ms.topic: article
ms.date: 11/19/2019
ms.author: allensu
ms.openlocfilehash: 760ec8a945ab88b63dde2de75f5354818facf4f2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "74225244"
---
# <a name="get-load-balancer-usage-metrics-using-the-rest-api"></a>Load Balancer metrische gegevens over het gebruik van de REST API ophalen

Het aantal bytes dat is verwerkt door een [Standard Load Balancer](/azure/load-balancer/load-balancer-standard-overview) , verzamelen gedurende een tijds interval met behulp van de [Azure rest API](/rest/api/azure/).

Volledige referentie documentatie en aanvullende voor beelden voor de REST API zijn beschikbaar in de [Azure monitor rest-referentie](/rest/api/monitor). 

## <a name="build-the-request"></a>De aanvraag maken

Gebruik de volgende GET-aanvraag om de [ByteCount-metriek](/azure/load-balancer/load-balancer-standard-diagnostics#multi-dimensional-metrics) van een Standard Load Balancer te verzamelen. 

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=ByteCount&timespan=2018-06-05T03:00:00Z/2018-06-07T03:00:00Z
```

### <a name="request-headers"></a>Aanvraagheaders

De volgende headers zijn vereist: 

|Aanvraagheader|Beschrijving|  
|--------------------|-----------------|  
|*Content-Type:*|Vereist. Ingesteld op `application/json`.|  
|*Authorization:*|Vereist. Ingesteld op een geldig `Bearer` [toegangstoken](/rest/api/azure/#authorization-code-grant-interactive-clients). |  

### <a name="uri-parameters"></a>URI-para meters

| Naam | Beschrijving |
| :--- | :---------- |
| subscriptionId | De abonnements-ID waarmee een Azure-abonnement wordt geïdentificeerd. Als u meerdere abonnementen hebt, raadpleegt u [werken met meerdere abonnementen](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest). |
| resourceGroupName | De naam van de resource groep die de resource bevat. U kunt deze waarde verkrijgen via de Azure Resource Manager-API, CLI of de portal. |
| loadBalancerName | De naam van de Azure Load Balancer. |
| metrische namen | Een door komma's gescheiden lijst met geldige [Load Balancer metrische gegevens](/azure/load-balancer/load-balancer-standard-diagnostics). |
| api-versie | De API-versie die voor de aanvraag moet worden gebruikt.<br /><br /> In dit document wordt de API `2018-01-01`-versie beschreven die is opgenomen in de bovenstaande URL.  |
| tijdsbestek | De time span van de query. Het is een teken reeks met de volgende `startDateTime_ISO/endDateTime_ISO`indeling. Deze optionele para meter is zo ingesteld dat de gegevens van een dag in het voor beeld worden geretourneerd. |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>Aanvraagbody

Er is geen aanvraag tekst nodig voor deze bewerking.

## <a name="handle-the-response"></a>Het antwoord verwerken

De status code 200 wordt geretourneerd wanneer de lijst met metrische waarden is geretourneerd. In de [referentie documentatie](/rest/api/monitor/metrics/list#errorresponse)vindt u een volledige lijst met fout codes.

## <a name="example-response"></a>Voorbeeld van een antwoord 

```json
{
    "cost": 0,
    "timespan": "2018-06-05T03:00:00Z/2018-06-07T03:00:00Z",
    "interval": "PT1M",
    "value": [
        {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}/providers/Microsoft.Insights/metrics/ByteCount",
            "type": "Microsoft.Insights/metrics",
            "name": {
                "value": "ByteCount",
                "localizedValue": "Byte Count"
            },
            "unit": "Count",
            "timeseries": [
                {
                    "metadatavalues": [],
                    "data": [
                        {
                            "timeStamp": "2018-06-06T17:24:00Z",
                            "total": 1067921034.0
                        },
                        {
                            "timeStamp": "2018-06-06T17:25:00Z",
                            "total": 0.0
                        },
                        {
                            "timeStamp": "2018-06-06T17:26:00Z",
                            "total": 3781344.0
                        },
                    ]
                }
            ]
        }
    ],
    "namespace": "Microsoft.Network/loadBalancers",
    "resourceregion": "eastus"
}
```
