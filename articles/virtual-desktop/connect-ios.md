---
title: Verbinding maken met het virtuele bureau blad van Windows vanuit iOS-Azure
description: Verbinding maken met het virtuele bureau blad van Windows met behulp van de iOS-client.
services: virtual-desktop
author: heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 02/08/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 4239f5271e4df282cf667b4bf1eff5f55f6f6323
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2020
ms.locfileid: "82611701"
---
# <a name="connect-with-the-ios-client"></a>Verbinding maken met de iOS-client

> Van toepassing op: iOS 13,0 of hoger. Compatibel met iPhone, iPad en iPod Touch.

>[!IMPORTANT]
>Deze inhoud is van toepassing op de lente 2020-update met Azure Resource Manager virtueel-bureaublad objecten van Windows. Raadpleeg [dit artikel](./virtual-desktop-fall-2019/connect-ios-2019.md)als u de versie van het Windows-bureau blad van Virtual Desktop 2019 zonder Azure Resource Manager objecten gebruikt.
>
> De Windows Virtual Desktop lente 2020-update is momenteel beschikbaar als open bare preview. Deze preview-versie is beschikbaar zonder service level agreement. het wordt niet aangeraden deze te gebruiken voor productie werkbelastingen. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. 
> Zie voor meer informatie [aanvullende gebruiks voorwaarden voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

U kunt toegang krijgen tot virtuele bureau blad-resources van Windows vanaf uw iOS-apparaat met onze Download bare client. In deze hand leiding wordt uitgelegd hoe u de iOS-client kunt instellen.

## <a name="install-the-ios-client"></a>De iOS-client installeren

[Down load](https://aka.ms/rdios) en installeer de-client op uw IOS-apparaat om aan de slag te gaan.

## <a name="subscribe-to-a-feed"></a>Abonneren op een feed

Abonneer u op de feed van uw beheerder om de lijst met beheerde resources te verkrijgen die u op uw iOS-apparaat kunt gebruiken.

Abonneren op een feed:

1. Tik **+** in het verbindings centrum en tik vervolgens op **werk ruimte toevoegen**.
2. Voer de URL van de feed in het veld URL van de **feed** in. De feed-URL kan een URL of een e-mail adres zijn.
   - Als u een URL gebruikt, gebruikt u de beheerder die u hebt ontvangen. Normaal gesp roken is <https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery>de URL.
   - Als u e-mail wilt gebruiken, voert u uw e-mail adres in. Dit geeft de client de opdracht om te zoeken naar een URL die is gekoppeld aan uw e-mail adres als uw beheerder de server op die manier heeft geconfigureerd.
3. Tik op **volgende**.
4. Geef uw referenties op wanneer u hierom wordt gevraagd.
   - Geef voor de **gebruikers naam**de gebruikers naam op met machtigingen voor toegang tot resources.
   - Geef bij **wacht woord**het wacht woord op dat is gekoppeld aan de gebruikers naam.
   - U wordt mogelijk ook gevraagd extra factoren op te geven als uw beheerder de verificatie op die manier heeft geconfigureerd.
5. Tik op **Opslaan**.

Daarna moeten de externe bronnen worden weer gegeven in het verbindings centrum.

Zodra u bent geabonneerd op een feed, wordt de inhoud van de feed regel matig automatisch bijgewerkt. Resources kunnen worden toegevoegd, gewijzigd of verwijderd op basis van wijzigingen die zijn aangebracht door de beheerder.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de documentatie aan de [slag met de IOS-client](/windows-server/remote/remote-desktop-services/clients/remote-desktop-ios/) voor meer informatie over het gebruik van de IOS-client.
