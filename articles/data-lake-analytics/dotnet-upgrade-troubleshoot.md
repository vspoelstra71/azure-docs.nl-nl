---
title: Problemen met de Azure Data Lake Analytics U-SQL-taak fouten oplossen vanwege .NET Framework 4.7.2-upgrade
description: Problemen met U-SQL-taak fouten oplossen vanwege de upgrade naar .NET Framework 4.7.2.
services: data-lake-analytics
author: guyhay
ms.author: guyhay
ms.reviewer: jasonwhowell
ms.service: data-lake-analytics
ms.topic: troubleshooting
ms.workload: big-data
ms.date: 10/11/2019
ms.openlocfilehash: f909419810cbd837e57b19a13b2df6ae9ad2ee97
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "79213588"
---
# <a name="azure-data-lake-analytics-is-upgrading-to-the-net-framework-v472"></a>Azure Data Lake Analytics wordt bijgewerkt naar de .NET Framework v-4.7.2

De Azure Data Lake Analytics standaard runtime wordt bijgewerkt van .NET Framework v 4.5.2 naar .NET Framework v-4.7.2. Met deze wijziging wordt een klein risico op het verbreken van wijzigingen geïntroduceerd als uw U-SQL-code aangepaste assembly's gebruikt, en deze aangepaste assembly's .NET-bibliotheken gebruiken.

Deze upgrade van .NET Framework 4.5.2 naar versie 4.7.2 betekent dat de .NET Framework geïmplementeerd in een U-SQL-runtime (de standaard runtime) nu altijd 4.7.2 is. Er is geen side-by-side optie voor .NET Framework versies.

Nadat deze upgrade naar .NET Framework 4.7.2 is voltooid, wordt de beheerde code van het systeem uitgevoerd als versie 4.7.2. door de gebruiker geleverde Bibliotheken, zoals de U-SQL-assembly's, worden uitgevoerd in de neerwaarts compatibele modus die geschikt is voor de versie waarvoor de assembly is gegenereerd.

- Als uw assembly-Dll's worden gegenereerd voor versie 4.5.2, behandelt het geïmplementeerde Framework deze als 4.5.2-Bibliotheken, voorzien van (met enkele uitzonde ringen) 4.5.2 semantiek.
- U kunt nu aangepaste U-SQL-assembly's gebruiken die gebruikmaken van versie 4.7.2-functies, als u de .NET Framework 4.7.2 richt.

Als gevolg van deze upgrade naar .NET Framework 4.7.2 is het mogelijk om belang rijke wijzigingen aan te brengen in uw U-SQL-taken die gebruikmaken van aangepaste .NET-assembly's. U wordt aangeraden te controleren of er problemen zijn met de compatibiliteit met behulp van de onderstaande procedure.

## <a name="how-to-check-for-backwards-compatibility-issues"></a>Controleren op achterwaartse compatibiliteits problemen

Controleer op de mogelijkheden van problemen met de compatibiliteit van het probleem door de .NET-compatibiliteits controles uit te voeren op uw .NET-code in uw U-SQL-aangepaste assembly's.

> [!Note]
> Het hulp programma detecteert geen wijzigingen in de werkelijke breuk. Er worden alleen .NET-Api's geïdentificeerd die mogelijk (voor bepaalde invoer) problemen veroorzaken. Als u een melding ontvangt van een probleem, kan uw code nog steeds goed zijn, maar u moet wel meer informatie controleren.

1. Voer de neerwaartse compatibiliteits controle uit op uw .NET Dll's door
   1. De Visual Studio-uitbrei ding gebruiken op de Visual Studio-uitbrei [ding .net portabiliteit Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)
   1. Het zelfstandige hulp programma downloaden en gebruiken vanuit [github dotnetapiport](https://github.com/microsoft/dotnet-apiport). Instructies voor het uitvoeren van een zelfstandig hulp programma zijn te vinden op [github dotnetapiport](https://github.com/microsoft/dotnet-apiport/blob/dev/docs/HowTo/BreakingChanges.md) -belang rijke wijzigingen
   1. Voor 4.7.2. compatibiliteit, `read isRetargeting == True` identificeert mogelijke problemen.
2. Als het hulp programma aangeeft of uw code kan worden beïnvloed door een van de mogelijke achterwaartse compatibiliteits problemen (een aantal veelvoorkomende voor beelden van incompatibiliteit worden hieronder weer gegeven), kunt u verder controleren door
   1. De code analyseren en identificeren of uw code waarden door geven aan de betrokken Api's
   1. Voer een runtime controle uit. De runtime-implementatie wordt niet naast elkaar uitgevoerd in ADLA. U kunt een runtime controle uitvoeren vóór de upgrade, met behulp van de lokale uitvoering van Visual Studio met een lokale .NET Framework 4.7.2 op basis van een representatieve gegevensset.
3. Als u een probleem ondervindt met een achterwaartse compatibiliteit, neemt u de nodige maat regelen om deze op te lossen (bijvoorbeeld om uw gegevens of code logica te herstellen).

In de meeste gevallen moet u niet worden beïnvloed door achterwaartse compatibiliteit.

## <a name="timeline"></a>Tijdlijn

U kunt de implementatie van de nieuwe runtime controleren met behulp van [runtime problemen oplossen](runtime-troubleshoot.md)en een eerdere voltooide taak bekijken.

### <a name="what-if-i-cant-get-my-code-reviewed-in-time"></a>Wat gebeurt er als ik mijn code niet in tijd kan bekijken

U kunt uw taak verzenden op basis van de oude runtime versie (die is opgebouwd in plaats van 4.5.2), maar als gevolg van het ontbreken van .NET Framework side-by-side-mogelijkheden, wordt het nog steeds alleen uitgevoerd in de compatibiliteits modus 4.5.2. Mogelijk ondervindt u nog steeds enkele van de compatibiliteits problemen vanwege dit gedrag.

### <a name="what-are-the-most-common-backwards-compatibility-issues-you-may-encounter"></a>Wat zijn de meest voorkomende problemen met betrekking tot de compatibiliteit die u kunt tegen komen

De meest voorkomende achterwaartse incompatibiliteiten die de controle waarschijnlijk zal identificeren, zijn (we hebben deze lijst gegenereerd door de controle uit te voeren op onze eigen interne ADLA-taken), waardoor de tape wisselaars worden beïnvloed (Opmerking: u kunt de bibliotheken alleen indirect aanroepen. het is dus belang rijk om de vereiste actie te ondernemen #1 om te controleren of uw taken worden beïnvloed) en mogelijke acties Opmerking: in bijna alle gevallen voor onze eigen taken is de waarschuwing ingesteld op valse positieven als gevolg van de smalle veranderingen van de meeste breuk.

- De eigenschap IAsyncResult. CompletedSynchronously moet correct zijn voor het volt ooien van de resulterende taak
  - Bij het aanroepen van TaskFactory. FromAsync moet de implementatie van de eigenschap IAsyncResult. CompletedSynchronously juist zijn voor het volt ooien van de resulterende taak. Dat wil zeggen dat de eigenschap alleen waar retourneert als en alleen als de implementatie synchroon is voltooid. Voorheen werd de eigenschap niet ingecheckt.
  - Beïnvloede bibliotheken: mscorlib, System. Threading. tasks
  - Aanbevolen actie: Zorg ervoor dat TaskFactory. FromAsync correct retourneert

- Data object. GetData haalt gegevens nu op als UTF-8
  - Voor apps die zijn gericht op de .NET Framework 4 of die worden uitgevoerd op de .NET Framework 4.5.1 of eerdere versies, wordt met Data object. GetData HTML-gegevens opgehaald als een ASCII-teken reeks. Als gevolg hiervan worden niet-ASCII-tekens (tekens waarvan de ASCII-codes groter zijn dan 0x7F) weer gegeven met twee wille keurige tekens. #N # #N # voor apps die zijn gericht op de .NET Framework 4,5 of `DataObject.GetData` hoger en die worden uitgevoerd op de .NET Framework 4.5.2, haalt gegevens op die zijn OPGEMAAKT als UTF-8, wat tekens groter dan 0x7F bevat.
  - Beïnvloede bibliotheken: Glo
  - Aanbevolen actie: Zorg ervoor dat de opgehaalde gegevens de gewenste indeling hebben

- XmlWriter genereert ongeldige surrogaat paren
  - Voor apps die zijn gericht op de .NET Framework 4.5.2 of de vorige versies, wordt er niet altijd een uitzonde ring gegenereerd door een ongeldige surrogaat paar te schrijven met behulp van uitzonde ring voor terugval verwerking. Voor apps die zijn gericht op de .NET Framework 4,6, wordt geprobeerd een ongeldig surrogaat paar te schrijven `ArgumentException`.
  - Beïnvloede bibliotheken: System. XML, System. XML. ReaderWriter
  - Aanbevolen actie: Zorg ervoor dat u geen ongeldig surrogaat paar schrijft dat argument uitzondering veroorzaakt

- HtmlTextWriter geeft het element `<br/>` niet correct weer
  - Vanaf de .NET Framework 4,6 wordt het `HtmlTextWriter.RenderBeginTag()` aanroepen `HtmlTextWriter.RenderEndTag()` en met `<BR />` een `<BR />` -element op de juiste manier ingevoegd (in plaats van twee)
  - Beïnvloede bibliotheken: System. Web
  - Aanbevolen actie: Zorg ervoor dat u de verwachte hoeveelheid `<BR />` wilt zien, zodat er geen wille keurig gedrag wordt weer gegeven in de productie taak

- Het aanroepen van CreateDefaultAuthorizationContext met een null-argument is gewijzigd
  - De implementatie van de AuthorizationContext die wordt geretourneerd door een aanroep `CreateDefaultAuthorizationContext(IList<IAuthorizationPolicy>)` naar het argument met een null-authorizationPolicies-waarde, is gewijzigd in de .NET Framework 4,6.
  - Beïnvloede bibliotheken: System. Identity model
  - Aanbevolen actie: Zorg ervoor dat u het nieuwe verwachte gedrag afhandelt wanneer er sprake is van Null-autorisatie beleid
  
- RSACng laadt nu RSA-sleutels van de niet-standaard sleutel grootte
  - In .NET Framework versies voorafgaand aan 4.6.2 hebben klanten met een niet-standaard sleutel grootte voor RSA-certificaten geen toegang tot deze sleutels `GetRSAPublicKey()` via `GetRSAPrivateKey()` de extensie methoden en. A `CryptographicException` met het bericht ' de aangevraagde sleutel grootte wordt niet ondersteund ' wordt gegenereerd. Het .NET Framework 4.6.2 dit probleem is opgelost. En nu werken met niet-standaard sleutel formaten zonder dat u dat `CryptographicException`hoeft te doen. `RSACng.ImportParameters()` `RSA.ImportParameters()`
  - Beïnvloede bibliotheken: mscorlib, System. core
  - Aanbevolen actie: Zorg ervoor dat RSA-sleutels werken zoals verwacht

- Dubbele controles van paden zijn strikter
  - In .NET Framework 4.6.2 zijn er een aantal wijzigingen aangebracht ter ondersteuning van eerder niet-ondersteunde paden (zowel de lengte als de indeling). Controleren of de syntaxis van het juiste schijf scheidings teken (dubbele punt) is verbeterd, waardoor sommige URI-paden in enkele Select Path-Api's werden geblokkeerd, waar ze werden gebruikt om te worden toegestaan.
  - Beïnvloede bibliotheken: mscorlib, System. runtime. Extensions
  - Aanbevolen actie:

- Aanroepen naar claims-Constructors
  - Vanaf de .NET Framework 4.6.2 is er een wijziging in de manier waarop `T:System.Security.Claims.ClaimsIdentity` constructors met een `T:System.Security.Principal.IIdentity` para meter de `P:System.Security.Claims.ClaimsIdentify.Actor` eigenschap instellen. Als het `T:System.Security.Principal.IIdentity` argument een `T:System.Security.Claims.ClaimsIdentity` `P:System.Security.Claims.ClaimsIdentify.Actor` object is en de eigenschap van dat `T:System.Security.Claims.ClaimsIdentity` object niet `null`is, wordt de `P:System.Security.Claims.ClaimsIdentify.Actor` eigenschap gekoppeld met behulp van `M:System.Security.Claims.ClaimsIdentity.Clone` de methode. In de Framework-4.6.1 en eerdere versies wordt `P:System.Security.Claims.ClaimsIdentify.Actor` de eigenschap gekoppeld als een bestaande verwijzing. Als gevolg van deze wijziging, te beginnen met de .NET Framework 4.6.2 `P:System.Security.Claims.ClaimsIdentify.Actor` , is de eigenschap `T:System.Security.Claims.ClaimsIdentity` van het nieuwe object niet gelijk `P:System.Security.Claims.ClaimsIdentify.Actor` aan de eigenschap van het `T:System.Security.Principal.IIdentity` argument van de constructor. In de .NET Framework 4.6.1 en eerdere versies is deze gelijk.
  - Beïnvloede bibliotheken: mscorlib
  - Aanbevolen actie: Zorg ervoor dat claims werkt zoals verwacht bij een nieuwe runtime

- Serialisatie van besturings tekens met DataContractJsonSerializer is nu compatibel met ECMAScript V6 en V8
  - In de .NET Framework-4.6.2 en eerdere versies heeft de DataContractJsonSerializer geen speciale Stuur tekens, zoals \b, \f en \t, op een manier geserialiseerd die compatibel is met de ECMAScript V6-en V8-standaarden. Vanaf de .NET Framework 4,7 is serialisatie van deze Stuur codes compatibel met ECMAScript V6 en V8.
  - Beïnvloede bibliotheken: System. runtime. serialisatie. json
  - Aanbevolen actie: zorg voor hetzelfde gedrag met DataContractJsonSerializer
