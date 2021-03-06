---
title: Identiteits gegevens opslag voor Australische klanten-Azure AD
description: Meer informatie over waar Azure Active Directory identiteits gegevens voor de Australische klanten opslaat.
services: active-directory
author: msaburnley
manager: daveba
ms.author: ajburnle
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 12/13/2019
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0dc659f3d580646c1e86e5e97940268311e5546f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "79460531"
---
# <a name="identity-data-storage-for-australian-and-new-zealand-customers-in-azure-active-directory"></a>Identiteits gegevens opslag voor Australische en Nieuw-Zeelandse klanten in Azure Active Directory

Identiteits gegevens worden opgeslagen door Azure AD op een geografische locatie op basis van het adres van uw organisatie bij het abonneren op een micro soft online service, zoals Office 365 en Azure. Voor informatie over waar uw identiteits klant gegevens zijn opgeslagen, kunt u de sectie [waar bevinden zich uw gegevens?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) in het micro soft vertrouwens centrum.

> [!NOTE]
> Services en toepassingen die met Azure AD worden geïntegreerd, hebben toegang tot klant gegevens van de identiteit. Evalueer elke service en toepassing die u gebruikt om te bepalen hoe klant gegevens van de identiteit worden verwerkt door die specifieke service en toepassing, en of ze voldoen aan de vereisten voor gegevens opslag van uw bedrijf. Zie de sectie Waar bevinden uw gegevens zich? van het Microsoft Trust Center voor meer informatie over de gegevenslocatie van Microsoft-services.

Voor klanten die een adres hebben ontvangen in Australië en Nieuw-Zeeland en gebruikmaakt van Azure AD Free Edition, blijven PII-gegevens op rest in Australische data centers. 

Alle andere Azure AD Premium-Services slaan klant gegevens op in wereld wijde data centers. Als u het Data Center voor een service wilt zoeken, raadpleegt u [Azure Active Directory – waar bevinden zich uw gegevens?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located)

## <a name="microsoft-azure-multi-factor-authentication-mfa"></a>Multi-factor Authentication (MFA) Microsoft Azure

MFA-service in azure AD slaat identiteits gegevens van klanten op in wereld wijde data centers. Zie [azure multi-factor Authentication verzameling van gebruikers gegevens](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-data-residency)voor meer informatie over de gebruikers gegevens die worden verzameld en opgeslagen door Azure MFA en Azure MFA-server op basis van de Cloud. Als klanten MFA gebruiken, worden hun gegevens buiten Australië-data centers in rust opgeslagen. 

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over de functies en functionaliteit die hierboven worden beschreven:
- [Wat is Multi-Factor Authentication?](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication)
