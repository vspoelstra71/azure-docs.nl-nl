---
title: Azure Multi-Factor Authentication versies en verbruiks abonnementen
description: Meer informatie over de Azure multi-factor Authentication-client en verschillende methoden en versies beschikbaar.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 05/20/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 165b84ee6b124d3f6a04c8db177ef17e32784ff9
ms.sourcegitcommit: 0690ef3bee0b97d4e2d6f237833e6373127707a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2020
ms.locfileid: "83757412"
---
# <a name="features-and-licenses-for-azure-multi-factor-authentication"></a>Functies en licenties voor Azure Multi-Factor Authentication

Als u gebruikers accounts in uw organisatie wilt beveiligen, moet u multi-factor Authentication gebruiken. Deze functie is vooral belang rijk voor accounts met uitgebreide toegang tot resources. Eenvoudige multi-factor Authentication-functies zijn beschikbaar voor Office 365-en Azure Active Directory-beheerders (Azure AD) zonder extra kosten. Als u de functies voor uw beheerders wilt upgraden of multi-factor Authentication wilt uitbreiden naar de rest van uw gebruikers, kunt u Azure Multi-Factor Authentication op verschillende manieren kopen.

> [!IMPORTANT]
> In dit artikel vindt u meer informatie over de verschillende manieren waarop Azure Multi-Factor Authentication kunnen worden gelicentieerd en gebruikt. Zie de [pagina met prijzen voor Azure multi-factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)voor specifieke informatie over prijzen en facturering.

## <a name="available-versions-of-azure-multi-factor-authentication"></a>Beschik bare versies van Azure Multi-Factor Authentication

Azure Multi-Factor Authentication kan worden gebruikt en in licentie worden gegeven, afhankelijk van de behoeften van uw organisatie. Mogelijk hebt u al het recht om Azure Multi-Factor Authentication te gebruiken, afhankelijk van de Azure AD-, Office 365-, EMS-of Microsoft 365-licentie die u momenteel hebt. De volgende tabel bevat informatie over de verschillende manieren om Azure Multi-Factor Authentication te verkrijgen en enkele van de functies en voor elk gebruik.

| Als u een gebruiker bent van | Mogelijkheden en use cases |
| --- | --- |
| EMS of Microsoft 365 E3 en E5 | EMS E3 of Microsoft 365 E3 (met inbegrip van EMS en Office 365), bevat Azure AD Premium P1. EMS E5 of Microsoft 365 E5 bevat Azure AD Premium P2. U kunt dezelfde functies voor voorwaardelijke toegang in de volgende secties gebruiken om multi-factor Authentication toe te voegen aan gebruikers. |
| Azure AD Premium P1 | U kunt [voorwaardelijke toegang van Azure AD](../conditional-access/howto-conditional-access-policy-all-users-mfa.md) gebruiken om gebruikers te vragen voor multi-factor Authentication tijdens bepaalde scenario's of gebeurtenissen die aan uw bedrijfs vereisten voldoen. |
| Azure AD Premium P2 | Biedt de sterkste beveiligings positie en verbeterde gebruikers ervaring. Voegt [op Risico's gebaseerde voorwaardelijke toegang](../conditional-access/howto-conditional-access-policy-risk.md) toe aan de Azure AD Premium P1-functies die worden aangepast aan de patronen van gebruikers en minimaliseert multi-factor Authentication-prompts. |
| Office 365 Business Premium, E3 of E5 | Azure Multi-Factor Authentication kan worden [ingeschakeld per gebruiker of worden in-](howto-mfa-userstates.md)of uitgeschakeld voor alle gebruikers, voor alle aanmeldings gebeurtenissen, met behulp van de standaard instellingen van de beveiliging. Beheer van Azure Multi-Factor Authentication is via de Office 365-Portal. Voor een betere gebruikers ervaring kunt u een upgrade uitvoeren naar Azure AD Premium P1 of P2 en voorwaardelijke toegang gebruiken. Zie voor meer informatie [beveiligde Office 365-resources met multi-factor Authentication](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6). |
| Azure AD Free | U kunt de [standaard instellingen voor beveiliging](../fundamentals/concept-fundamentals-security-defaults.md) gebruiken om multi-factor Authentication in te scha kelen voor alle gebruikers, telkens wanneer er een verificatie aanvraag wordt gedaan. U hebt geen nauw keurige controle van ingeschakelde gebruikers of scenario's, maar biedt wel de extra beveiligings stap.<br /> Zelfs als de standaard instellingen voor beveiliging niet worden gebruikt om multi-factor Authentication voor iedereen in te scha kelen, kunnen gebruikers die de rol van *Azure AD globale beheerder* hebben toegewezen, worden geconfigureerd voor het gebruik van multi-factor Authentication. Deze functie van de gratis laag zorgt ervoor dat de essentiële beheerders accounts worden beveiligd door multi-factor Authentication. |

## <a name="feature-comparison-of-versions"></a>Functie vergelijking van versies

De volgende tabel bevat een lijst met de functies die beschikbaar zijn in de verschillende versies van Azure Multi-Factor Authentication. Plan uw behoeften voor het beveiligen van gebruikers verificatie en bepaal vervolgens welke benadering aan deze vereisten voldoet. Hoewel Azure AD Free bijvoorbeeld beveiligings standaards biedt die Azure Multi-Factor Authentication bieden, kan alleen de mobiele verificator-app worden gebruikt voor de verificatie prompt, niet via een telefoon gesprek of SMS. Deze methode kan een beperking zijn als u niet zeker weet dat de mobiele verificatie-app is geïnstalleerd op het persoonlijke apparaat van de gebruiker.

| Functie | Azure AD Free-standaard instellingen voor beveiliging | Azure AD Free-globale beheerders van Azure AD | Office 365 Business Premium, E3 of E5 | Azure AD Premium P1 of P2 |
| --- |:---:|:---:|:---:|:---:|
| Azure AD-Tenant beheerders accounts beveiligen met MFA | ● | ● (Alleen*Azure AD Global Administrator* -accounts) | ● | ● |
| Mobiele app als een tweede factor | ● | ● | ● | ● |
| Telefoon gesprek als een tweede factor | | ● | ● | ● |
| SMS als een tweede factor | | ● | ● | ● |
| Beheer beheer via verificatie methoden | | ● | ● | ● |
| Fraudewaarschuwing | | | | ● |
| MFA-rapporten | | | | ● |
| Aangepaste begroeting voor telefoongesprekken | | | | ● |
| Aangepaste beller-ID voor telefoon gesprekken | | | | ● |
| Goedgekeurde IP-adressen | | | | ● |
| MFA herinneren voor vertrouwde apparaten | | ● | ● | ● |
| MFA voor on-premises toepassingen | | | | ● |

> [!IMPORTANT]
> Vanaf maart 2019 zijn de opties voor telefoon gesprekken niet langer beschikbaar voor Azure Multi-Factor Authentication en kunnen gebruikers van Azure self-service voor wacht woord opnieuw instellen in Azure AD Free/proef-tenants. SMS-berichten worden niet beïnvloed door deze wijziging. Telefoon gesprekken blijven beschikbaar voor gebruikers in Azure AD Premium P1-of P2-tenants of toepassingen of Office 365 Business Premium, E3 of E5.

## <a name="purchase-and-enable-azure-multi-factor-authentication"></a>Azure-Multi-Factor Authentication kopen en inschakelen

Als u Azure Multi-Factor Authentication wilt gebruiken, moet u zich registreren voor of een in aanmerking komend Azure AD-laag kopen. Azure AD is verkrijgbaar in vier edities: gratis, Office 365 apps Edition (voor Office 365 Business Premium E3, of E5 klanten), Premium P1 en Premium P2.

De gratis versie is opgenomen in een Azure-abonnement. Zie de [sectie hieronder](#azure-ad-free-tier) voor informatie over het gebruik van standaard instellingen voor beveiliging of het beveiligen van accounts met de rol *globale beheerder van Azure AD* .

De Azure AD Premium-edities zijn beschikbaar via uw micro soft-vertegenwoordiger, het [Open Volume License-programma](https://www.microsoft.com/licensing/licensing-programs/open-license.aspx)en het [programma Cloud Solution Providers](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409). Abonnees van Azure en Office 365 kunnen Azure Active Directory Premium P1 en P2 ook online aanschaffen. [Meld](https://portal.office.com/Commerce/Catalog.aspx) u aan om aan te schaffen.

> [!IMPORTANT]
> Licentie verlening op basis van verbruik is niet langer beschikbaar voor nieuwe klanten vanaf 1 september 2018. Bestaande klanten die gebruikmaken van het op verbruik gebaseerde model, kunnen per ingeschakelde gebruiker of per verificatie facturering blijven gebruiken.

Nadat u de vereiste Azure AD-laag hebt aangeschaft, moet u [Azure-multi-factor Authentication plannen en implementeren](howto-mfa-getstarted.md).

### <a name="azure-ad-free-tier"></a>Laag Azure AD Free

Alle gebruikers in een Azure AD Free-Tenant kunnen Azure multi-factor Authentication gebruiken met behulp van de standaard instellingen van de beveiliging. Met deze standaard instellingen voor beveiliging wordt Azure multi-factor Authentication ingeschakeld voor alle gebruikers, elke keer dat deze zich aanmeldt. De mobiele verificatie-app is de enige methode die kan worden gebruikt voor Azure-Multi-Factor Authentication wanneer u Azure AD Free standaard instellingen voor beveiliging gebruikt.

* [Meer informatie over de standaard instellingen voor Azure AD-beveiliging](../fundamentals/concept-fundamentals-security-defaults.md)
* [Standaard instellingen voor beveiliging inschakelen voor gebruikers in Azure AD Free](../fundamentals/concept-fundamentals-security-defaults.md#enabling-security-defaults)

Als u Azure Multi-Factor Authentication niet wilt inschakelen voor alle gebruikers en elk aanmeldings gebeurtenis, kunt u in plaats daarvan alleen gebruikers accounts beveiligen met de rol *globale beheerder van Azure AD* . Deze aanpak biedt extra verificatie prompts voor essentiële beheerders accounts. U schakelt Azure Multi-Factor Authentication op een van de volgende manieren in, afhankelijk van het type account dat u gebruikt:

* Als u een micro soft-account gebruikt, moet u [zich registreren voor multi-factor Authentication](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).
* Als u geen micro soft-account gebruikt, [schakelt u multi-factor Authentication in voor een gebruiker of groep in azure AD](howto-mfa-userstates.md).

## <a name="next-steps"></a>Volgende stappen

* Zie [prijzen voor Azure multi-factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)voor meer informatie over de kosten.
* [Wat is voorwaardelijke toegang?](../conditional-access/overview.md)

