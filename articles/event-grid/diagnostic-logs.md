---
title: Azure Event Grid-Diagnostische logboeken voor onderwerpen of domeinen
description: Dit artikel bevat conceptuele informatie over Diagnostische logboeken voor een Azure Event grid-onderwerp of een domein.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/29/2020
ms.author: spelluru
ms.openlocfilehash: 93e7e47cbcc1ab9542ba333b89f7dd655a412489
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2020
ms.locfileid: "82630250"
---
#  <a name="diagnostic-logs-for-azure-event-grid-topicsdomains"></a>Diagnostische logboeken voor Azure Event Grid onderwerpen/domeinen
Met Diagnostische instellingen kunnen Event Grid gebruikers publicatie- **en bezorg fouten** logboeken vastleggen en weer geven in een opslag account, een event hub of een log Analytics-werk ruimte. Dit artikel bevat een schema voor de logboeken en een voor beeld van een logboek vermelding.


## <a name="schema-for-publishdelivery-failure-logs"></a>Schema voor fouten logboeken voor publiceren/afleveren

| Naam van eigenschap | Gegevenstype | Beschrijving |
| ------------- | --------- | ----------- | 
| Time | DateTime | Het tijdstip waarop de logboek vermelding is gegenereerd <p>**Voorbeeld waarde:** 01-29-2020 09:52:02.700</p> |
| EventSubscriptionName | Tekenreeks | De naam van het gebeurtenis abonnement <p>**Voorbeeld waarde:** "EVENTSUB1"</p> <p>Deze eigenschap bestaat alleen voor het afleveren van fouten Logboeken.</p>  |
| Categorie | Tekenreeks | De naam van de logboek categorie. <p>**Voorbeeld waarden:** "DeliveryFailures" of "PublishFailures" | 
| OperationName | Tekenreeks | De naam van de bewerking die wordt uitgevoerd terwijl de fout optreedt.<p>**Voorbeeld waarden:** ' Leveren ' voor leverings fouten. |
| Bericht | Tekenreeks | Het logboek bericht voor de gebruiker waarin de reden voor de fout en andere aanvullende informatie wordt uitgelegd. |
| ResourceId | Tekenreeks | De resource-ID voor het onderwerp/de domein resource<p>**Voorbeeld waarden:**`/SUBSCRIPTIONS/SAMPLE-SUBSCRIPTION-ID/RESOURCEGROUPS/SAMPLE-RESOURCEGROUP/PROVIDERS/MICROSOFT.EVENTGRID/TOPICS/TOPIC1` |

## <a name="example"></a>Voorbeeld

```json
{
    "time": "2019-11-01T00:17:13.4389048Z",
    "resourceId": "/SUBSCRIPTIONS/SAMPLE-SUBSCTIPTION-ID /RESOURCEGROUPS/SAMPLE-RESOURCEGROUP-NAME/PROVIDERS/MICROSOFT.EVENTGRID/TOPICS/SAMPLE-TOPIC-NAME ",
    "eventSubscriptionName": "SAMPLEDESTINATION",
    "category": "DeliveryFailures",
    "operationName": "Deliver",
    "message": "Message:outcome=NotFound, latencyInMs=2635, systemId=17284f7c-0044-46fb-84b7-59fda5776017, state=FilteredFailingDelivery, deliveryTime=11/1/2019 12:17:10 AM, deliveryCount=0, probationCount=0, deliverySchema=EventGridEvent, eventSubscriptionDeliverySchema=EventGridEvent, fields=InputEvent, EventSubscriptionId, DeliveryTime, State, Id, DeliverySchema, LastDeliveryAttemptTime, SystemId, fieldCount=, requestExpiration=1/1/0001 12:00:00 AM, delivered=False publishTime=11/1/2019 12:17:10 AM, eventTime=11/1/2019 12:17:09 AM, eventType=Type, deliveryTime=11/1/2019 12:17:10 AM, filteringState=FilteredWithRpc, inputSchema=EventGridEvent, publisher=DIAGNOSTICLOGSTEST-EASTUS.EASTUS-1.EVENTGRID.AZURE.NET, size=363, fields=Id, PublishTime, SerializedBody, EventType, Topic, Subject, FilteringHashCode, SystemId, Publisher, FilteringTopic, TopicCategory, DataVersion, MetadataVersion, InputSchema, EventTime, fieldCount=15, url=sb://diagnosticlogstesting-eastus.servicebus.windows.net/, deliveryResponse=NotFound: The messaging entity 'sb://diagnosticlogstesting-eastus.servicebus.windows.net/eh-diagnosticlogstest' could not be found. TrackingId:c98c5af6-11f0-400b-8f56-c605662fb849_G14, SystemTracker:diagnosticlogstesting-eastus.servicebus.windows.net:eh-diagnosticlogstest, Timestamp:2019-11-01T00:17:13, referenceId: ac141738a9a54451b12b4cc31a10dedc_G14:"
}
```

## <a name="next-steps"></a>Volgende stappen
Zie [Diagnostische logboeken inschakelen](enable-diagnostic-logs-topic.md)voor meer informatie over het inschakelen van Diagnostische logboeken voor onderwerpen of domeinen.
