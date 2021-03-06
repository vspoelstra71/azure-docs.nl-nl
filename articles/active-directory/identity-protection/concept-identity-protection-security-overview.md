---
title: Overzicht van Azure Active Directory Identity Protection beveiliging
description: Meer informatie over hoe het beveiligings overzicht u een inzicht geeft in de beveiligings postuur van uw organisatie.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9550684ad154f28a02ee347fd0a79c1ec286beb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "74382185"
---
# <a name="azure-active-directory-identity-protection---security-overview"></a>Azure Active Directory Identity Protection-beveiligings overzicht

Het [overzicht van beveiliging](https://aka.ms/IdentityProtectionRefresh) in de Azure Portal biedt u inzicht in de beveiligings postuur van uw organisatie. Het helpt mogelijke aanvallen te identificeren en inzicht te krijgen in de effectiviteit van uw beleid.

Het beveiligings overzicht is breed onderverdeeld in twee secties:

- Trends, aan de linkerkant, bieden een risico lijn voor uw organisatie.
- Naast tegels markeert u aan de rechter kant de belangrijkste problemen in uw organisatie en wordt voorgesteld hoe u snel actie kunt ondernemen.

![Beveiligingsoverzicht](./media/concept-identity-protection-security-overview/01.png)
  
## <a name="trends"></a>Trends

### <a name="new-risky-users-detected"></a>Er zijn nieuwe Risk ante gebruikers gedetecteerd

Dit diagram toont het aantal nieuwe Risk ante gebruikers dat gedurende de gekozen tijds periode is gedetecteerd. U kunt de weer gave van dit diagram filteren op risico niveau van de gebruiker (laag, gemiddeld, hoog). Beweeg de muis aanwijzer over de UTC-datum om het aantal Risk ante gebruikers te zien dat voor die dag is gedetecteerd. Als u op deze grafiek klikt, gaat u naar het rapport Risk ante gebruikers. Als u gebruikers die risico lopen, wilt oplossen, kunt u het wacht woord wijzigen.

### <a name="new-risky-sign-ins-detected"></a>Er zijn nieuwe Risk ante aanmeldingen gedetecteerd

Dit diagram toont het aantal Risk ante aanmeldingen dat tijdens de gekozen tijds periode is gedetecteerd. U kunt de weer gave van deze grafiek filteren op het type aanmeldings risico (realtime of aggregatie) en het risico niveau voor aanmelden (laag, gemiddeld, hoog). Niet-beveiligde aanmeldingen zijn geslaagde realtime-aanmeldingen waarvoor geen MFA-Challenge werd geleverd. (Opmerking: aanmeldingen die riskant zijn omdat offline detecties niet in realtime kunnen worden beveiligd met beleids regels voor aanmeldings Risico's). Beweeg de muis aanwijzer over de UTC-datum om het aantal aanmeldingen dat voor die dag is gedetecteerd, te bekijken. Als u op deze grafiek klikt, gaat u naar het rapport Risk ante aanmeldingen.

## <a name="tiles"></a>Tegels
 
### <a name="high-risk-users"></a>Gebruikers met een hoog risico

De tegel gebruikers met een hoog risico toont het nieuwste aantal gebruikers met een hoge kans op inbreuk op de identiteit. Deze moeten de hoogste prioriteit voor onderzoek zijn. Als u op de tegel ' gebruikers van hoog risico ' klikt, wordt een omleiding naar een gefilterde weer gave van het rapport Risk ante gebruikers weer gegeven met alleen gebruikers met een risico niveau hoog. Met dit rapport kunt u meer informatie vinden en deze gebruikers herstellen met een wacht woord opnieuw instellen.

![Beveiligingsoverzicht](./media/concept-identity-protection-security-overview/02.png)

### <a name="medium-risk-users"></a>Gebruikers met een gemiddeld risico
De tegel gebruikers met een gemiddeld risico toont het nieuwste aantal gebruikers met een gemiddelde kans op inbreuk op de identiteit. Als u op de tegel ' gebruikers van middel grote risico ' klikt, wordt een gefilterde weer gave van het rapport Risk ante gebruikers omgeleid naar een gemiddeld risico niveau. Met dit rapport kunt u deze gebruikers verder onderzoeken en oplossen.

### <a name="unprotected-risky-sign-ins"></a>Niet-beveiligde Risk ante aanmeldingen

In de tegel niet-beveiligde Risk ante aanmeldingen wordt het aantal geslaagde, realtime aanmeldingen van de afgelopen week weer gegeven die niet zijn geblokkeerd of waarvoor MFA is aangestuurd door een beleid voor voorwaardelijke toegang, een risico beleid voor identiteits beveiliging of MFA per gebruiker. Dit zijn mogelijk gemanipuleerde aanmeldingen die zijn geslaagd en geen enkele MFA-uitdaging. Als u dergelijke aanmeldingen in de toekomst wilt beveiligen, moet u een beleid voor aanmeldings Risico's Toep assen. Een klik op de tegel onbeveiligde Risk ante aanmeldingen wordt omgeleid naar de Blade configuratie van het aanmeldings risico beleid waar u het beleid voor aanmeldings Risico's kunt configureren om MFA te vereisen voor een aanmelding met een opgegeven risico niveau.

### <a name="legacy-authentication"></a>Verouderde verificatie

De tegel ' verouderde verificatie ' bevat het aantal verouderde authenticaties van de laatste week in uw organisatie. Verouderde verificatie protocollen bieden geen ondersteuning voor moderne beveiligings methoden zoals een MFA. Als u verouderde verificatie wilt voor komen, kunt u een beleid voor voorwaardelijke toegang Toep assen. Als u op de tegel ' oude verificatie ' klikt, wordt u omgeleid naar de beveiligde score voor identiteit.

### <a name="identity-secure-score"></a>Identiteits veilige Score

De identiteit van de beveiligde score meet en vergelijkt uw beveiligings postuur tot branche patronen. Als u op de tegel identiteits veilige Score (preview) klikt, wordt deze omgeleid naar de Blade identiteits veilige Score, waar u meer informatie kunt krijgen over het verbeteren van uw beveiligings postuur.

## <a name="next-steps"></a>Volgende stappen

- [Wat is risico](concept-identity-protection-risks.md)

- [Beleids regels beschikbaar om Risico's te beperken](concept-identity-protection-policies.md)
