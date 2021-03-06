---
title: Azure Relay uitzonde ringen en oplossingen oplossen | Microsoft Docs
description: Lijst met Azure Relay uitzonde ringen en voorgestelde acties die u kunt uitvoeren om ze te helpen oplossen.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: 5f9dd02c-cce0-43b3-8eb8-744f0c27f38c
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2017
ms.author: spelluru
ms.openlocfilehash: fe8f057443b978e70e7cdd2591affd455fefdca8
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2020
ms.locfileid: "83210867"
---
# <a name="azure-relay-exceptions"></a>Azure Relay uitzonde ringen

In dit artikel worden enkele uitzonde ringen vermeld die kunnen worden gegenereerd door de Azure Relay-Api's. Deze verwijzing is onderhevig aan wijzigingen. Controleer daarom opnieuw op updates.

## <a name="exception-categories"></a>Uitzonderings Categorieën

De relay-Api's genereren uitzonde ringen die in de volgende categorieën kunnen vallen. Ook worden de voorgestelde acties beschreven die u kunt uitvoeren om de uitzonde ringen op te lossen.

*   **Gebruikers coderings fout**: [System. ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System. InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System. OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System. runtime. serialisatie. SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). 

    **Algemene actie**: Probeer de code te corrigeren voordat u doorgaat.
*   **Setup/configuratie fout**: [System. UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). 

    **Algemene actie**: Controleer uw configuratie. Wijzig indien nodig de configuratie.
*   **Tijdelijke uitzonde ringen**: [micro soft. ServiceBus. Messa ging. MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [micro soft. ServiceBus. Messa ging. ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception), [micro soft. ServiceBus. Messa ging. MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). 

    **Algemene actie**: Probeer de bewerking opnieuw uit te voeren of gebruikers te informeren.
*   **Andere uitzonde ringen**: [System. Trans actions. TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System. TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx). 

    **Algemene actie**: specifiek voor het uitzonderings type. Zie de tabel in de volgende sectie. 

## <a name="exception-types"></a>Uitzonderings typen

De volgende tabel bevat een lijst met uitzonderings typen van berichten en de oorzaken daarvan. Er worden ook aanbevolen acties beschreven die u kunt uitvoeren om de uitzonde ringen op te lossen.

| **Uitzonderingstype** | **Beschrijving** | **Voorgestelde actie** | **Opmerking bij automatische of directe nieuwe poging** |
| --- | --- | --- | --- |
| [Out](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |De server heeft niet binnen de opgegeven tijd gereageerd op de aangevraagde bewerking, die wordt beheerd door [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout). De server heeft mogelijk de aangevraagde bewerking voltooid. Dit kan gebeuren vanwege vertragingen in het netwerk of andere infra structuur. |Controleer de systeem status op consistentie en probeer het vervolgens opnieuw, indien nodig. Zie [TimeoutException](#timeoutexception). |In sommige gevallen kan het nodig zijn om het opnieuw te proberen. Voeg logica voor opnieuw proberen toe aan code. |
| [Ongeldige bewerking](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |De aangevraagde gebruikers bewerking is niet toegestaan in de server of de service. Zie het uitzonderings bericht voor meer informatie. |Controleer de code en de documentatie. Zorg ervoor dat de aangevraagde bewerking geldig is. |Opnieuw proberen wordt niet geholpen. |
| [Bewerking is geannuleerd](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Er wordt geprobeerd een bewerking aan te roepen voor een object dat al is gesloten, afgebroken of verwijderd. In zeldzame gevallen is de ambient trans actie al verwijderd. |Controleer de code en zorg ervoor dat deze geen bewerkingen aanroept op een verwijderd object. |Opnieuw proberen wordt niet geholpen. |
| [Onbevoegde toegang](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |Het [token provider](/dotnet/api/microsoft.servicebus.tokenprovider) -object kan geen token verkrijgen, het token is ongeldig of het token bevat niet de claims die vereist zijn om de bewerking uit te voeren. |Zorg ervoor dat de token provider is gemaakt met de juiste waarden. Controleer de configuratie van de Access Control-service. |In sommige gevallen kan het nodig zijn om het opnieuw te proberen. Voeg logica voor opnieuw proberen toe aan code. |
| [Argument uitzondering](https://msdn.microsoft.com/library/system.argumentexception.aspx),<br /> [Argument null](https://msdn.microsoft.com/library/system.argumentnullexception.aspx),<br />[Het argument valt buiten het bereik](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Een of meer van de volgende fout is opgetreden:<br />Een of meer argumenten die zijn opgegeven voor de methode, zijn ongeldig.<br /> De URI die is geleverd aan [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) of [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) bevat een of meer padsegmenten.<br />Het URI-schema dat is opgegeven aan [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) of [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) , is ongeldig. <br />De waarde van de eigenschap is groter dan 32 KB. |Controleer de aanroepende code en controleer of de argumenten juist zijn. |Opnieuw proberen wordt niet geholpen. |
| [Server bezet](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) |De aanvraag kan op dit moment niet door de service worden verwerkt. |De client kan wachten op een bepaalde tijd en vervolgens de bewerking opnieuw uitvoeren. |De client kan na een bepaald interval opnieuw proberen. Als een nieuwe poging resulteert in een andere uitzonde ring, controleert u het gedrag voor opnieuw proberen van deze uitzonde ring. |
| [Quotum overschreden](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) |De bericht entiteit heeft de Maxi maal toegestane grootte bereikt. |Maak ruimte in de entiteit door berichten van de entiteit of de bijbehorende subwachtrijen te ontvangen. Zie [QuotaExceededException](#quotaexceededexception). |Opnieuw proberen kan helpen als er in de tussen tijd berichten zijn verwijderd. |
| [De bericht grootte is overschreden](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |Een bericht lading overschrijdt de limiet van 256 KB. Houd er rekening mee dat de limiet van 256 KB de totale grootte van het bericht is. De totale bericht grootte kan systeem eigenschappen en eventuele Microsoft .NET overhead bevatten. |Verminder de grootte van de bericht lading en voer de bewerking opnieuw uit. |Opnieuw proberen wordt niet geholpen. |

## <a name="quotaexceededexception"></a>QuotaExceededException

[QuotaExceededException](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) geeft aan dat een quotum voor een bepaalde entiteit is overschreden.

Voor relay verloopt deze uitzonde ring in het [System. service model. QuotaExceededException](https://msdn.microsoft.com/library/system.servicemodel.quotaexceededexception.aspx), wat aangeeft dat het maximum aantal listeners voor dit eind punt is overschreden. Dit wordt aangegeven in de waarde **MaximumListenersPerEndpoint** van het uitzonderings bericht.

## <a name="timeoutexception"></a>TimeoutException
Een [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) geeft aan dat een door de gebruiker geïnitieerde bewerking langer duurt dan de time-out van de bewerking. 

Controleer de waarde van de eigenschap [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) . Het bereiken van deze limiet kan ook een [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx)veroorzaken.

Voor relay kunt u time-outuitzonderingen ontvangen wanneer u voor het eerst een verbinding voor een relay-afzender opent. Er zijn twee veelvoorkomende oorzaken voor deze uitzonde ring:

*   De open [time-](https://msdn.microsoft.com/library/wcf.opentimeout.aspx) outwaarde is mogelijk te klein (als dit een fractie van een seconde is).
* Een on-premises relay-listener reageert mogelijk niet (of er kunnen problemen met de firewall regels optreden waardoor listeners geen nieuwe client verbindingen meer accepteren). de [Opentime-outwaarde](https://msdn.microsoft.com/library/wcf.opentimeout.aspx) is minder dan ongeveer 20 seconden.

Voorbeeld:

```
'System.TimeoutException’: The operation did not complete within the allotted timeout of 00:00:10.
The time allotted to this operation may have been a portion of a longer timeout.
```

### <a name="common-causes"></a>Veelvoorkomende oorzaken
Er zijn twee veelvoorkomende oorzaken voor deze fout:

*   **Onjuiste configuratie**
    
    De time-out van de bewerking is mogelijk te klein voor de operationele voor waarde. De standaard waarde voor de time-out van de bewerking in de client-SDK is 60 seconden. Controleer of de waarde in de code is ingesteld op iets te klein. Houd er rekening mee dat het CPU-gebruik en de voor waarde van het netwerk invloed kunnen hebben op de tijd die nodig is om een bewerking te volt ooien. Het is een goed idee om de time-out voor de bewerking niet in te stellen op een zeer kleine waarde.
*   **Tijdelijke service fout**

    In sommige gevallen kan de Relay-service vertragingen ondervinden bij het verwerken van aanvragen. Dit kan bijvoorbeeld gebeuren tijdens peri Oden met veel verkeer. Als dit het geval is, voert u de bewerking na een vertraging opnieuw uit totdat de bewerking is voltooid. Als dezelfde bewerking blijft mislukken na meerdere pogingen, controleert u de [Azure-service status site](https://azure.microsoft.com/status/) om te zien of er sprake is van bekende service storingen.

## <a name="next-steps"></a>Volgende stappen
* [Veelgestelde vragen over Azure Relay](relay-faq.md)
* [Een relay-naam ruimte maken](relay-create-namespace-portal.md)
* [Aan de slag met Azure Relay en .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Aan de slag met Azure Relay en het knoop punt](relay-hybrid-connections-node-get-started.md)

