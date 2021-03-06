---
title: MSAL voor iOS & macOS-verschillen | Azure
titleSuffix: Microsoft identity platform
description: Hierin worden de verschillen in het gebruik van micro soft Authentication Library (MSAL) tussen iOS en macOS beschreven.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 08/28/2019
ms.author: marsma
ms.reviewer: oldalton
ms.custom: aaddev
ms.openlocfilehash: 62b79ee7398286b8e6c8ed8612bd001595e1f6ea
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "77084977"
---
# <a name="microsoft-authentication-library-for-ios-and-macos-differences"></a>Verschillen tussen de Microsoft-verificatiebibliotheek voor iOS en macOS

In dit artikel worden de verschillen in functionaliteit tussen de micro soft Authentication Library (MSAL) voor iOS en macOS beschreven.

> [!NOTE]
> Op de Mac ondersteunt MSAL alleen macOS-apps.

## <a name="general-differences"></a>Algemene verschillen

MSAL voor macOS is een subset van de functionaliteit die beschikbaar is voor iOS.

MSAL voor macOS biedt geen ondersteuning voor:

- verschillende browser typen `ASWebAuthenticationSession`, zoals, `SFAuthenticationSession`, `SFSafariViewController`,.
- brokered authenticatie via Microsoft Authenticator-app wordt niet ondersteund voor macOS.

Het delen van sleutel Hangers tussen apps van dezelfde uitgever is beperkter dan macOS 10,14 en eerder. Gebruik [toegangs beheer lijsten](https://developer.apple.com/documentation/security/keychain_services/access_control_lists?language=objc) om de paden op te geven naar de apps die de sleutel hanger moeten delen. De gebruiker kan extra aanvragen voor sleutel hanger bekijken.

Bij macOS 10.15 + is het gedrag van MSAL hetzelfde als bij iOS en macOS. MSAL maakt gebruik van [keten toegangs groepen](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps?language=objc) voor het delen van sleutel hanger. 

### <a name="conditional-access-authentication-differences"></a>Verschillen in de verificatie van voorwaardelijke toegang

Voor scenario's voor voorwaardelijke toegang worden er minder gebruikers prompts weer gegeven wanneer u MSAL voor iOS gebruikt. Dit komt doordat iOS de Broker-app (Microsoft Authenticator) gebruikt die de nood zaak om in bepaalde gevallen de gebruiker vraagt te vragen.

### <a name="project-setup-differences"></a>Verschillen in de project instellingen

**macOS**

- Wanneer u uw project instelt op macOS, moet u ervoor zorgen dat uw toepassing is ondertekend met een geldig ontwikkelings-of productie certificaat. MSAL werkt nog steeds in de niet-ondertekende modus, maar werkt anders met betrekking tot cache persistentie. De app mag alleen worden uitgevoerd als niet-ondertekend voor fout opsporing. Als u de app niet-ondertekend distribueert, geldt het volgende:
1. Op 10,14 en eerder wordt de gebruiker door MSAL gevraagd om een sleutel hanger wachtwoord telkens wanneer de app opnieuw wordt gestart.
2. Op 10.15 + wordt gebruiker door MSAL gevraagd om referenties voor elke verwerving van tokens. 

- macOS-apps hoeven de AppDelegate-aanroep niet te implementeren.

**iOS**

- Er zijn aanvullende stappen voor het instellen van het project ter ondersteuning van de stroom voor verificatie Broker. De stappen worden in de zelf studie genoemd.
- iOS-projecten moeten aangepaste schema's registreren in de info. plist. Dit is niet vereist voor macOS.
