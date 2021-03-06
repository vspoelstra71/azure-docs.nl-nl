---
title: Beschikbaarheid en beperkingen van Microsoft Authenticator voor Android in China | Microsoft Docs
description: 'Meer informatie over het verkrijgen van de app Microsoft Authenticator: beschikbaarheid in China'
services: active-directory
author: curtand
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 05/20/2020
ms.author: curtand
ms.openlocfilehash: 6d6d32a7a340ef4b07138d5ec12c561e958cf739
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2020
ms.locfileid: "83739495"
---
# <a name="microsoft-authenticator-for-android-in-the-public-cloud-in-china"></a>Microsoft Authenticator voor Android in de openbare cloud in China

De app Microsoft Authenticator voor Android is beschikbaar om te worden gedownload in China. Google Play Store is niet beschikbaar in China, dus de app moet worden gedownload in andere Chinese app-marketplaces. De app Microsoft Authenticator voor Android is in China momenteel beschikbaar in de volgende winkels:

- [Baidu](https://shouji.baidu.com/software/26638379.html)
- [Lenovo](https://www.lenovomm.com/appdetail/com.azure.authenticator/20197724)
- [Huawei](https://appgallery.cloud.huawei.com/uowap/index.html#/detailApp/C100262999?source=appshare&subsource=C100262999&shareTo=weixin&locale=zh_CN)
- [Samsung Galaxy Store](http://apps.samsung.com/appquery/appDetail.as?appId=com.azure.authenticator)

De meest recente build van de app bevindt zich in Google Play Store, maar de app wordt zo snel mogelijk in alle andere app-stores bijgewerkt. Aangezien er geen aangepast Android-toepassingspakket (APK) is geïmplementeerd in een app-store, kan de app naadloos worden bijgewerkt vanuit een van de volgende locaties:

- De store waarin de app is gedownload
- Google Play Store als de gebruiker regio's doorkruist

## <a name="limitations"></a>Beperkingen

De app Microsoft Authenticator voor Android maakt gebruik van het Firebase Cloud Messaging-systeem van Google en Google Play Services om pushmeldingen te ontvangen. Geen van beide services is in China beschikbaar, dus er zijn enkele beperkingen ten aanzien van de functionaliteit van de app:

- Registratie van de Authenticator-app als een methode voor meervoudige verificatie (MFA) met behulp van pushmeldingen werkt niet.

- [Aanmelden via de telefoon](../authentication/howto-authentication-sms-signin.md) kan niet worden ingesteld. Hiervoor moet de gebruiker de app instellen als een MFA-methode met pushmeldingen, wat momenteel niet werkt.

Als een gebruiker eerder in staat is geweest aanmelden via de telefoon of meervoudige verificatie met behulp van de app in te stellen, kan de gebruiker een handmatige controle uitvoeren op het aanvragen van meldingen in de app en deze gebruiken voor identiteitsverificatie.

## <a name="multi-factor-authentication-workaround"></a>Tijdelijke oplossing voor meervoudige verificatie

In plaats van pushmeldingen voor meervoudige verificatie te gebruiken, kunnen gebruikers hun MFA instellen om verificatiecodes op hun apparaat te ontvangen, die ze vervolgens kunnen gebruiken om hun identiteit te verifiëren. Deze verificatiecodes zijn gedurende dertig seconden geldig en om ze te kunnen gebruiken, moeten beheerders hun tenant in staat stellen verificatie uit te voeren met behulp van verificatiecodes op basis van Time-Based One-Time Passwords (TOTP).

## <a name="availability"></a>Beschikbaarheid

Microsoft Authenticator-functie | Beschikbaarheid in China
------------------------------- | ---------------------
MFA-registratie met pushmeldingen | Nee
Eerder bestaand MFA-account dat identiteit verifieert met pushmeldingen | Nee
Eerder bestaand MFA-account dat handmatige controle op meldingen uitvoert | Ja
MFA-registratie/-verificatie met alleen mobiele TOTP-/verificatie-codes | Ja
Registratie via telefonische aanmelding | Nee
Bestaande telefonische aanmelding met pushmeldingen | Nee
Bestaande telefonische verificatie van aanmeldingen door uitvoeren van handmatige controle op verificatieaanvragen | Ja

## <a name="next-steps"></a>Volgende stappen

- [De app Microsoft Authenticator downloaden en installeren](user-help-auth-app-download-install.md)
