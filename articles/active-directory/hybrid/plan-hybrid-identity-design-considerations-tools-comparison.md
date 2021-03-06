---
title: 'Hybride identiteit: vergelijking van hulpprogramma’s voor directory-integratie | Microsoft Docs'
description: Deze pagina bevat een uitgebreide tabel waarin de verschillende hulpprogramma's voor directory-integratie worden vergeleken.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 1e62a4bd-4d55-4609-895e-70131dedbf52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/07/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 052d99a819aee415d5e7ad6dc00b8c786af0f636
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80811090"
---
# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Vergelijking van hulpprogramma’s voor directory-integratie voor hybride identiteit
In de afgelopen jaren hebben de hulpprogramma's voor directory-integratie zich enorm ontwikkeld.  


- [FIM](https://docs.microsoft.com/previous-versions/windows/desktop/forefront-2010/ff182370%28v%3dvs.100%29) en [MIM](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016) worden nog steeds ondersteund en scha kelen voornamelijk de synchronisatie tussen on-premises systemen.   De [FIM Windows Azure AD-connector](https://docs.microsoft.com/previous-versions/mim/dn511001(v=ws.10)?redirectedfrom=MSDN) wordt ondersteund in FIM en MIM, maar wordt niet aanbevolen voor nieuwe implementaties: klanten met on-premises bronnen zoals Notes of SAP HCM moeten MIM Active Directory Domain Services (AD DS) gebruiken en vervolgens Azure AD Connect sync of Azure AD Connect Cloud inrichting gebruiken om te synchroniseren vanaf AD DS met Azure AD.
- [Azure AD Connect-synchronisatie](how-to-connect-sync-whatis.md) bevat de onderdelen en functionaliteit die eerder in DirSync en Azure AD Sync zijn uitgebracht, voor het synchroniseren tussen AD DS forests en Azure AD.  
- [Azure AD Connect Cloud inrichting](../cloud-provisioning/what-is-cloud-provisioning.md) is een nieuwe micro soft-agent voor synchronisatie van AD DS naar Azure AD, die nuttig is voor scenario's zoals fusie en acquisitie, waarbij de AD-forests van het overgenomen bedrijf zijn geïsoleerd van de AD-forests van het bovenliggende bedrijf.

Zie het artikel [Wat is Azure AD Connect Cloud inrichting?](../cloud-provisioning/what-is-cloud-provisioning.md) voor meer informatie over de verschillen tussen Azure AD Connect synchronisatie en Azure AD Connect Cloud inrichting?

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](whatis-hybrid-identity.md).

