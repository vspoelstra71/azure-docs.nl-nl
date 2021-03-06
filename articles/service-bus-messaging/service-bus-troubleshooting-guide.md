---
title: Gids voor probleem oplossing voor Azure Service Bus | Microsoft Docs
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
ms.date: 04/07/2020
ms.author: aschhab
ms.openlocfilehash: 63bf035d4e19cc1d64998a6ad533812e71ee71b8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80887773"
---
# <a name="troubleshooting-guide-for-azure-service-bus"></a>Gids voor probleem oplossing voor Azure Service Bus
In dit artikel vindt u tips en aanbevelingen voor het oplossen van problemen die kunnen optreden bij het gebruik van Azure Service Bus. 

## <a name="connectivity-certificate-or-timeout-issues"></a>Problemen met connectiviteit, certificaten of time-outs
De volgende stappen kunnen u helpen bij het oplossen van problemen met connectiviteit/certificaat/time-out voor alle services onder *. servicebus.windows.net. 

- Blader naar of [wget](https://www.gnu.org/software/wget/) `https://<yournamespace>.servicebus.windows.net/`. Het helpt te controleren of er problemen zijn met de IP-filtering of het virtuele netwerk of de certificaat keten (het meest gebruikelijk wanneer u Java SDK gebruikt).

    Een voor beeld van een geslaagd bericht:
    
    ```xml
    <feed xmlns="http://www.w3.org/2005/Atom"><title type="text">Publicly Listed Services</title><subtitle type="text">This is the list of publicly-listed services currently available.</subtitle><id>uuid:27fcd1e2-3a99-44b1-8f1e-3e92b52f0171;id=30</id><updated>2019-12-27T13:11:47Z</updated><generator>Service Bus 1.1</generator></feed>
    ```
    
    Een voor beeld van een fout bericht:

    ```json
    <Error>
        <Code>400</Code>
        <Detail>
            Bad Request. To know more visit https://aka.ms/sbResourceMgrExceptions. . TrackingId:b786d4d1-cbaf-47a8-a3d1-be689cda2a98_G22, SystemTracker:NoSystemTracker, Timestamp:2019-12-27T13:12:40
        </Detail>
    </Error>
    ```
- Voer de volgende opdracht uit om te controleren of een poort is geblokkeerd op de firewall. De gebruikte poorten zijn 443 (HTTPS), 5671 (AMQP) en 9354 (net Messa ging/SBMP). Afhankelijk van de bibliotheek die u gebruikt, worden ook andere poorten gebruikt. Hier volgt een voor beeld van de opdracht waarmee wordt gecontroleerd of de 5671-poort is geblokkeerd. 

    ```powershell
    tnc <yournamespacename>.servicebus.windows.net -port 5671
    ```

    Op Linux:

    ```shell
    telnet <yournamespacename>.servicebus.windows.net 5671
    ```
- Als er onregelmatige verbindings problemen zijn, voert u de volgende opdracht uit om te controleren of er verloren pakketten zijn. Met deze opdracht wordt geprobeerd 25 verschillende TCP-verbindingen elke 1 seconde te maken met de service. Vervolgens kunt u controleren of het aantal geslaagde/mislukte en ook TCP-verbindings latentie wordt weer geven. U kunt het `psping` hulp programma [hier](/sysinternals/downloads/psping)downloaden.

    ```shell
    .\psping.exe -n 25 -i 1 -q <yournamespace>.servicebus.windows.net:5671 -nobanner     
    ```
    U kunt gelijkwaardige opdrachten gebruiken als u andere hulp middelen gebruikt, `tnc`zoals `ping`,, enzovoort. 
- Verkrijg een netwerk tracering als de vorige stappen niet helpen en analyseren met behulp van hulpprogram ma's zoals [wireshark](https://www.wireshark.org/). Neem zo nodig contact op met [Microsoft ondersteuning](https://support.microsoft.com/) . 

## <a name="issues-that-may-occur-with-service-upgradesrestarts"></a>Problemen die zich kunnen voordoen met Service-upgrades/opnieuw opstarten
Het upgraden van de back-end-service en opnieuw opstarten kan de volgende invloed hebben op uw toepassingen:

- Aanvragen kunnen tijdelijk worden beperkt.
- Er is mogelijk een neerzetten in inkomende berichten/aanvragen.
- Het logboek bestand kan fout berichten bevatten.
- De verbinding van de toepassingen kan enkele seconden worden verbroken met de service.

Als de toepassings code gebruikmaakt van SDK, is het beleid voor opnieuw proberen al ingebouwd en actief. De toepassing zal opnieuw verbinding maken zonder belang rijke gevolgen voor de toepassing/werk stroom.

## <a name="unauthorized-access-send-claims-are-required"></a>Onbevoegde toegang: verzend claims zijn vereist
Deze fout kan optreden wanneer u probeert een Service Bus onderwerp te openen vanuit Visual Studio op een on-premises computer met behulp van een door de gebruiker toegewezen beheerde identiteit met verzend machtigingen.

```bash
Service Bus Error: Unauthorized access. 'Send' claim\(s\) are required to perform this operation.
```

Om deze fout op te lossen, installeert u de bibliotheek [micro soft. Azure. Services. AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) .  Zie [Local Development Authentication](..\key-vault\service-to-service-authentication.md#local-development-authentication)(Engelstalig) voor meer informatie. 

Zie [een beheerde identiteit verifiëren met Azure Active Directory om toegang te krijgen tot Azure service bus resources](service-bus-managed-service-identity.md)voor meer informatie over het toewijzen van machtigingen aan rollen.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen: 

- [Uitzonde ringen Azure Resource Manager](service-bus-resource-manager-exceptions.md). De lijst met uitzonde ringen die zijn gegenereerd bij het werken met Azure Service Bus met behulp van Azure Resource Manager (via sjablonen of directe aanroepen).
- [Messa ging-uitzonde ringen](service-bus-messaging-exceptions.md). Het bevat een lijst met uitzonde ringen die zijn gegenereerd door .NET Framework voor Azure Service Bus. 

