---
title: Azure Service Bus-Messa ging Exceptions | Microsoft Docs
description: Dit artikel bevat een lijst met Azure Service Bus Messa ging-uitzonde ringen en voorgestelde acties die moeten worden uitgevoerd wanneer de uitzonde ring optreedt.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 3d8526fe-6e47-4119-9f3e-c56d916a98f9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2020
ms.author: aschhab
ms.openlocfilehash: f1a4caf6ffd5740b4227aff2f38d9cb709c77b48
ms.sourcegitcommit: d9cd51c3a7ac46f256db575c1dfe1303b6460d04
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2020
ms.locfileid: "82739344"
---
# <a name="service-bus-messaging-exceptions"></a>Uitzonde ringen Service Bus berichten
Dit artikel bevat de .NET-uitzonde ringen die door .NET Framework Api's worden gegenereerd. 

## <a name="exception-categories"></a>Uitzonderings Categorieën
De Messa ging Api's genereren uitzonde ringen die kunnen vallen in de volgende categorieën, samen met de bijbehorende actie die u kunt uitvoeren om ze te herstellen. De betekenis en oorzaken van een uitzonde ring kunnen variëren, afhankelijk van het type Messa ging-entiteit:

1. Gebruikers coderings fout ([System. ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System. InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System. OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System. runtime. serialisatie. SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx)). Algemene actie: Probeer de code te corrigeren voordat u doorgaat.
2. Setup/configuratie fout ([Microsoft. ServiceBus. Messa ging. MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception), [System. UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Algemene actie: Controleer uw configuratie en wijzig deze indien nodig.
3. Tijdelijke uitzonde ringen ([Microsoft. ServiceBus. Messa ging. MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [micro soft. ServiceBus. Messa ging. ServerBusyException](/dotnet/api/microsoft.azure.servicebus.serverbusyexception), [micro soft. ServiceBus. Messa ging. MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception)). Algemene actie: Probeer de bewerking opnieuw uit te voeren of gebruikers te informeren. De `RetryPolicy` klasse in de client-SDK kan zodanig worden geconfigureerd dat nieuwe pogingen automatisch worden verwerkt. Zie [richt lijnen voor opnieuw proberen](/azure/architecture/best-practices/retry-service-specific#service-bus)voor meer informatie.
4. Andere uitzonde ringen ([System. Trans actions. TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System. TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx), [micro soft. ServiceBus. Messa ging. MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception), [micro soft. ServiceBus. Messa ging. SessionLockLostException](/dotnet/api/microsoft.azure.servicebus.sessionlocklostexception)). Algemene actie: specifiek voor het uitzonderings type; Raadpleeg de tabel in de volgende sectie: 

## <a name="exception-types"></a>Uitzonderings typen
De volgende tabel bevat een lijst met uitzonderings typen van berichten, en de oorzaken ervan, en de aanbevolen actie voor notities die u kunt uitvoeren.

| **Uitzonderings type** | **Beschrijving/oorzaak/voor beelden** | **Aanbevolen actie** | **Opmerking over automatische/directe nieuwe poging** |
| --- | --- | --- | --- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |De server heeft niet gereageerd op de aangevraagde bewerking binnen de opgegeven tijd, die wordt beheerd door [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings). De server heeft de aangevraagde bewerking mogelijk voltooid. Dit kan gebeuren vanwege vertragingen in het netwerk of andere infra structuur. |Controleer de systeem status op consistentie en probeer het opnieuw als dat nodig is. Zie [time-outuitzonderingen](#timeoutexception). |In sommige gevallen kan het nodig zijn om het opnieuw te proberen. Voeg logica voor opnieuw proberen toe aan code. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |De aangevraagde gebruikers bewerking is niet toegestaan binnen de server of service. Zie het uitzonderings bericht voor meer informatie. Met de [functie complete ()](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) wordt deze uitzonde ring gegenereerd als het bericht is ontvangen in de [ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode) -modus. |Controleer de code en de documentatie. Controleer of de aangevraagde bewerking geldig is. |Nieuwe poging is niet meer nodig. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Er wordt geprobeerd een bewerking aan te roepen voor een object dat al is gesloten, afgebroken of verwijderd. In zeldzame gevallen is de ambient trans actie al verwijderd. |Controleer de code en zorg ervoor dat deze geen bewerkingen aanroept op een verwijderd object. |Nieuwe poging is niet meer nodig. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |Het [token provider](/dotnet/api/microsoft.servicebus.tokenprovider) -object kan geen token verkrijgen, het token is ongeldig of het token bevat niet de claims die vereist zijn om de bewerking uit te voeren. |Zorg ervoor dat de token provider is gemaakt met de juiste waarden. Controleer de configuratie van de Access Control Service. |In sommige gevallen kan het nodig zijn om het opnieuw te proberen. Voeg logica voor opnieuw proberen toe aan code. |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[ArgumentOutOfRangeException](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Een of meer argumenten die zijn opgegeven voor de methode, zijn ongeldig.<br /> De URI die is geleverd aan [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) of [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) bevat een of meer paden.<br /> Het URI-schema dat is opgegeven aan [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) of [Create](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) , is ongeldig. <br />De waarde van de eigenschap is groter dan 32 KB. |Controleer de aanroepende code en controleer of de argumenten juist zijn. |Nieuwe poging is niet meer nodig. |
| [MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.servicebus.messagingentitynotfoundexception) |De entiteit die aan de bewerking is gekoppeld, bestaat niet of is verwijderd. |Controleer of de entiteit bestaat. |Nieuwe poging is niet meer nodig. |
| [MessageNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagenotfoundexception) |Poging tot het ontvangen van een bericht met een bepaald Volg nummer. Dit bericht is niet gevonden. |Zorg ervoor dat het bericht nog niet is ontvangen. Controleer de deadletter-wachtrij om te zien of het bericht is deadlettered. |Nieuwe poging is niet meer nodig. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) |De client kan geen verbinding tot stand brengen met Service Bus. |Controleer of de opgegeven hostnaam juist is en of de host bereikbaar is. |Opnieuw proberen kan helpen als er onregelmatige verbindings problemen zijn. |
| [ServerBusyException](/dotnet/api/microsoft.azure.servicebus.serverbusyexception) |De aanvraag kan op dit moment niet door de service worden verwerkt. |De client kan gedurende een bepaalde tijd wachten en vervolgens de bewerking opnieuw uitvoeren. |Client kan na een bepaald interval opnieuw proberen. Als een nieuwe poging resulteert in een andere uitzonde ring, controleert u het gedrag voor opnieuw proberen van deze uitzonde ring. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) |Generieke Messa ging-uitzonde ring die in de volgende gevallen kan worden gegenereerd:<p>Er wordt geprobeerd een [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) te maken met behulp van een naam of pad dat tot een ander type entiteit behoort (bijvoorbeeld een onderwerp).</p><p>Er is een poging gedaan om een bericht te verzenden dat groter is dan 256 KB. </p>Er is een fout opgetreden op de server of service tijdens het verwerken van de aanvraag. Zie het uitzonderings bericht voor meer informatie. Doorgaans is dit een tijdelijke uitzonde ring.</p><p>De aanvraag is beëindigd omdat de entiteit wordt beperkt. Fout code: 50001, 50002, 50008. </p> | Controleer de code en zorg ervoor dat alleen serialiseerbare objecten worden gebruikt voor de hoofd tekst van het bericht (of een aangepaste serialisatiefunctie te gebruiken). <p>Raadpleeg de documentatie voor de ondersteunde typen waarden van de eigenschappen en gebruik alleen ondersteunde typen.</p><p> Controleer de eigenschap [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception) . Als de **waarde True**is, kunt u de bewerking opnieuw proberen. </p>| Als de uitzonde ring wordt veroorzaakt door beperkingen, wacht u een paar seconden en voert u de bewerking opnieuw uit. Het gedrag voor opnieuw proberen is niet gedefinieerd en kan mogelijk niet worden geholpen in andere scenario's.|
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) |Poging een entiteit te maken met een naam die al wordt gebruikt door een andere entiteit in die service naam ruimte. |Verwijder de bestaande entiteit of kies een andere naam voor de entiteit die u wilt maken. |Nieuwe poging is niet meer nodig. |
| [QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) |De bericht entiteit heeft de Maxi maal toegestane grootte bereikt of het maximum aantal verbindingen met een naam ruimte is overschreden. |Maak ruimte in de entiteit door berichten van de entiteit of de bijbehorende subwachtrijen te ontvangen. Zie [QuotaExceededException](#quotaexceededexception). |Opnieuw proberen kan helpen als er in de tussen tijd berichten zijn verwijderd. |
| [RuleActionException](/dotnet/api/microsoft.servicebus.messaging.ruleactionexception) |Service Bus deze uitzonde ring wordt geretourneerd als u probeert een ongeldige regel actie te maken. Service Bus koppelt deze uitzonde ring aan een deadlettered-bericht als er een fout optreedt tijdens het verwerken van de regel actie voor dat bericht. |Controleer de regel actie op juistheid. |Nieuwe poging is niet meer nodig. |
| [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) |Service Bus deze uitzonde ring wordt geretourneerd als u probeert een ongeldig filter te maken. Service Bus koppelt deze uitzonde ring aan een deadlettered-bericht als er een fout is opgetreden bij het verwerken van het filter voor dat bericht. |Controleer het filter op juistheid. |Nieuwe poging is niet meer nodig. |
| [SessionCannotBeLockedException](/dotnet/api/microsoft.servicebus.messaging.sessioncannotbelockedexception) |Poging een sessie met een specifieke sessie-ID te accepteren, maar de sessie is momenteel vergrendeld door een andere client. |Zorg ervoor dat de sessie wordt ontgrendeld door andere clients. |Opnieuw proberen kan helpen als de sessie in de tijdelijke situatie is vrijgegeven. |
| [TransactionSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.transactionsizeexceededexception) |Te veel bewerkingen maken deel uit van de trans actie. |Verminder het aantal bewerkingen dat deel uitmaakt van deze trans actie. |Nieuwe poging is niet meer nodig. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.azure.servicebus.messagingentitydisabledexception) |Aanvraag voor een runtime-bewerking op een uitgeschakelde entiteit. |Activeer de entiteit. |Opnieuw proberen kan helpen als de entiteit is geactiveerd in de tussentijds. |
| [NoMatchingSubscriptionException](/dotnet/api/microsoft.servicebus.messaging.nomatchingsubscriptionexception) |Service Bus deze uitzonde ring wordt geretourneerd als u een bericht verzendt naar een onderwerp waarvoor vooraf filteren is ingeschakeld en er geen filters overeenkomen. |Zorg ervoor dat er ten minste één filter overeenkomt. |Nieuwe poging is niet meer nodig. |
| [MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |Een bericht lading overschrijdt de limiet van 256 KB. De limiet van 256 KB is de totale bericht grootte, die systeem eigenschappen en eventuele .NET-overhead kan bevatten. |Verminder de grootte van de bericht lading en voer de bewerking opnieuw uit. |Nieuwe poging is niet meer nodig. |
| [TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx) |De ambient trans actie (*Trans Action. current*) is ongeldig. Mogelijk is deze voltooid of afgebroken. Binnenste uitzonde ring kan aanvullende informatie worden verstrekt. | |Nieuwe poging is niet meer nodig. |
| [TransactionInDoubtException](https://msdn.microsoft.com/library/system.transactions.transactionindoubtexception.aspx) |Er wordt een bewerking uitgevoerd op een trans actie die onzeker is, of er wordt geprobeerd de trans actie door te voeren en de trans actie wordt onzeker. |Uw toepassing moet deze uitzonde ring afhandelen (als speciaal geval), omdat de trans actie mogelijk al is doorgevoerd. |- |

## <a name="quotaexceededexception"></a>QuotaExceededException
[QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) geeft aan dat een quotum voor een bepaalde entiteit is overschreden.

### <a name="queues-and-topics"></a>Wacht rijen en onderwerpen
Voor wacht rijen en onderwerpen is het vaak de grootte van de wachtrij. De eigenschap fout bericht bevat meer details, zoals in het volgende voor beeld:

```Output
Microsoft.ServiceBus.Messaging.QuotaExceededException
Message: The maximum entity size has been reached or exceeded for Topic: 'xxx-xxx-xxx'. 
    Size of entity in bytes:1073742326, Max entity size in bytes:
1073741824..TrackingId:xxxxxxxxxxxxxxxxxxxxxxxxxx, TimeStamp:3/15/2013 7:50:18 AM
```

In het bericht wordt vermeld dat het onderwerp de maximale grootte heeft overschreden, in dit geval 1 GB (de standaard grootte). 

### <a name="namespaces"></a>Naamruimten

Voor naam ruimten kan [QuotaExceededException](/dotnet/api/microsoft.azure.servicebus.quotaexceededexception) aangeven dat het maximum aantal verbindingen met een naam ruimte is overschreden voor een toepassing. Bijvoorbeeld:

```Output
Microsoft.ServiceBus.Messaging.QuotaExceededException: ConnectionsQuotaExceeded for namespace xxx.
<tracking-id-guid>_G12 ---> 
System.ServiceModel.FaultException`1[System.ServiceModel.ExceptionDetail]: 
ConnectionsQuotaExceeded for namespace xxx.
```

### <a name="common-causes"></a>Veelvoorkomende oorzaken
Er zijn twee veelvoorkomende oorzaken voor deze fout: de wachtrij voor onbestelbare berichten en niet-functionerende bericht ontvangers.

1. **[Wachtrij met onbestelbare berichten](service-bus-dead-letter-queues.md)** Een lezer kan berichten niet volt ooien en de berichten worden teruggestuurd naar de wachtrij/het onderwerp wanneer de vergren deling verloopt. Dit kan gebeuren als de lezer een uitzonde ring tegen komt die verhindert dat deze [BrokeredMessage. complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.complete)aanroept. Nadat een bericht 10 keer is gelezen, wordt het standaard naar de wachtrij voor onbestelbare berichten verplaatst. Dit gedrag wordt bepaald door de eigenschap [QueueDescription. MaxDeliveryCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.maxdeliverycount) en heeft een standaard waarde van 10. Wanneer berichten in de wachtrij voor onbestelbare meldingen worden gestapeld, nemen ze ruimte in beslag.
   
    Om het probleem op te lossen, leest en voltooit u de berichten van de wachtrij voor onbestelbare meldingen, zoals u zou doen vanuit een andere wachtrij. U kunt de [FormatDeadLetterPath](/dotnet/api/microsoft.azure.servicebus.entitynamehelper.formatdeadletterpath) -methode gebruiken om het wachtrij-pad voor onbestelbare berichten op te maken.
2. **Ontvanger gestopt**. Een ontvanger stopt met het ontvangen van berichten van een wachtrij of abonnement. De manier om dit te identificeren, is door te kijken naar de eigenschap [QueueDescription. MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) , waarin de volledige uitsplitsing van de berichten wordt weer gegeven. Als de eigenschap [ActiveMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount) hoog of groei is, worden de berichten niet zo snel gelezen als ze worden geschreven.

## <a name="timeoutexception"></a>TimeoutException
Een [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) geeft aan dat een door de gebruiker geïnitieerde bewerking langer duurt dan de time-out van de bewerking. 

U moet de waarde van de eigenschap [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) controleren, omdat deze limiet ook een [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx)kan veroorzaken.

### <a name="queues-and-topics"></a>Wacht rijen en onderwerpen
Voor wacht rijen en onderwerpen wordt de time-out opgegeven in de eigenschap [MessagingFactorySettings. OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings) , als onderdeel van de Connection String, of via [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.azure.servicebus.servicebusconnectionstringbuilder). Het fout bericht zelf kan variëren, maar het bevat altijd de time-outwaarde die voor de huidige bewerking is opgegeven. 

## <a name="messagelocklostexception"></a>MessageLockLostException

### <a name="cause"></a>Oorzaak

De **MessageLockLostException** wordt gegenereerd wanneer een bericht wordt ontvangen met behulp van de [PeekLock](message-transfers-locks-settlement.md#peeklock) receive-modus en de vergren deling van de client aan de kant van de service verloopt.

Het vergren delen van een bericht kan om verschillende redenen verlopen. 

  * De vergrendelings timer is verlopen voordat deze is vernieuwd door de client toepassing.
  * De client toepassing heeft de vergren deling verkregen, opgeslagen in een permanente opslag en opnieuw gestart. Zodra de client toepassing opnieuw is opgestart, wordt de Inflight-berichten bekeken en is geprobeerd deze te volt ooien.

### <a name="resolution"></a>Oplossing

In het geval van een **MessageLockLostException**kan de client toepassing het bericht niet meer verwerken. De client toepassing kan eventueel de uitzonde ring registreren voor analyse, maar de client *moet* het bericht verwijderen.

Omdat de vergren deling van het bericht is verlopen, gaat het terug naar de wachtrij (of het abonnement) en kan het worden verwerkt door de volgende client toepassing die wordt ontvangen.

Als de **MaxDeliveryCount** is overschreden, kan het bericht worden verplaatst naar de **DeadLetterQueue**.

## <a name="sessionlocklostexception"></a>SessionLockLostException

### <a name="cause"></a>Oorzaak

De **SessionLockLostException** wordt gegenereerd wanneer een sessie wordt geaccepteerd en de vergren deling van de client aan de kant van de service verloopt.

De vergren deling van een sessie kan om verschillende redenen verlopen. 

  * De vergrendelings timer is verlopen voordat deze is vernieuwd door de client toepassing.
  * De client toepassing heeft de vergren deling verkregen, opgeslagen in een permanente opslag en opnieuw gestart. Zodra de client toepassing opnieuw is opgestart, werd de invlucht-sessie uitgevoerd en probeerde de berichten in die sessies te verwerken.

### <a name="resolution"></a>Oplossing

In het geval van een **SessionLockLostException**kan de client toepassing de berichten in de sessie niet langer verwerken. De client toepassing kan de uitzonde ring voor analyse registreren, maar de client *moet* het bericht verwijderen.

Omdat de vergren deling van de sessie is verlopen, wordt deze teruggestuurd naar de wachtrij (of het abonnement) en kan deze worden vergrendeld door de volgende client toepassing die de sessie accepteert. Omdat de sessie vergrendeling op een bepaald moment door één client toepassing wordt vastgehouden, wordt de volg orde van de verwerking gegarandeerd.

## <a name="socketexception"></a>SocketException

### <a name="cause"></a>Oorzaak

In de onderstaande gevallen wordt een **SocketException** gegenereerd:
   * Wanneer een verbindings poging mislukt, omdat de host niet correct heeft gereageerd na een opgegeven tijd (TCP-fout code 10060).
   * Een tot stand gebrachte verbinding is mislukt omdat de verbonden host niet heeft gereageerd.
   * Er is een fout opgetreden bij het verwerken van het bericht of de time-out is overschreden door de externe host.
   * Onderliggend netwerk bron probleem.

### <a name="resolution"></a>Oplossing

De **SocketException** -fouten geven aan dat de VM die als host fungeert voor de toepassingen `<mynamespace>.servicebus.windows.net` de naam niet kan omzetten naar het bijbehorende IP-adres. 

Controleer of de volgende opdracht is geslaagd in toewijzing aan een IP-adres.

```Powershell
PS C:\> nslookup <mynamespace>.servicebus.windows.net
```

Dit moet een uitvoer zoals hieronder opgeven

```bash
Name:    <cloudappinstance>.cloudapp.net
Address:  XX.XX.XXX.240
Aliases:  <mynamespace>.servicebus.windows.net
```

Als de bovenstaande naam **niet** kan worden omgezet in een IP-adres en de naam ruimte alias, controleert u welke netwerk beheerder u verder wilt onderzoeken. Naam omzetting geschiedt via een DNS-server meestal een resource in het netwerk van de klant. Als de DNS-omzetting wordt uitgevoerd door Azure DNS, neemt u contact op met de ondersteuning van Azure.

Als de naam omzetting **werkt zoals verwacht**, controleert u of de verbindingen met Azure service bus [hier](service-bus-troubleshooting-guide.md#connectivity-certificate-or-timeout-issues) zijn toegestaan


## <a name="messagingexception"></a>MessagingException

### <a name="cause"></a>Oorzaak

**MessagingException** is een algemene uitzonde ring die om verschillende redenen kan worden gegenereerd. Hieronder vindt u een aantal redenen.

   * Er wordt een poging gedaan om een **QueueClient** te maken in een **onderwerp** of een **abonnement**.
   * De grootte van het verzonden bericht is groter dan de limiet voor de opgegeven laag. Meer informatie over de Service Bus [quota's en limieten](service-bus-quotas.md).
   * Specifieke aanvraag voor gegevens vlak (verzenden, ontvangen, volt ooien, afbreken) is beëindigd als gevolg van beperking.
   * Tijdelijke problemen die zijn veroorzaakt door service-upgrades en opnieuw opstarten.

> [!NOTE]
> De bovenstaande lijst met uitzonde ringen is niet limitatief.

### <a name="resolution"></a>Oplossing

De stappen voor de oplossing zijn afhankelijk van wat de oorzaak van de **MessagingException** heeft veroorzaakt.

   * Voor **tijdelijke problemen** (waarbij ***isTransient*** is ingesteld op ***True***) of voor het **beperken van problemen**, kunt u het opnieuw proberen door de bewerking uit te voeren. Het standaard beleid voor opnieuw proberen van de SDK kan hiervoor worden benut.
   * Voor andere problemen duiden de details in de uitzonde ring aan dat de stappen voor problemen en oplossingen kunnen worden afgeleid van hetzelfde.

## <a name="next-steps"></a>Volgende stappen
Zie de [Azure .net API-naslag](/dotnet/api/overview/azure/service-bus)informatie voor de volledige service bus .net API-referentie.
Raadpleeg de [hand leiding voor probleem](service-bus-troubleshooting-guide.md) oplossing voor tips voor probleem oplossing
