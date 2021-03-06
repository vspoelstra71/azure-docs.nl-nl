---
title: Share point-gebruikers profiel service inschakelen met Azure AD DS | Microsoft Docs
description: Meer informatie over het configureren van een Azure Active Directory Domain Services beheerd domein voor de ondersteuning van profiel synchronisatie voor share Point server
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 01/21/2020
ms.author: iainfou
ms.openlocfilehash: a684a669c491e35b5c6b62dd318b4fe61edeb52b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80655379"
---
# <a name="configure-azure-active-directory-domain-services-to-support-user-profile-synchronization-for-sharepoint-server"></a>Azure Active Directory Domain Services configureren voor de ondersteuning van synchronisatie van gebruikers profielen voor share Point server

Share Point server bevat een service voor het synchroniseren van gebruikers profielen. Met deze functie kunnen gebruikers profielen worden opgeslagen op een centrale locatie en toegankelijk via meerdere share point-sites en-farms. Als u de gebruikers profiel service van share Point server wilt configureren, moeten de juiste machtigingen worden verleend in een door Azure Active Directory Domain Services (Azure AD DS) beheerd domein. Zie [gebruikers profiel synchronisatie in share Point server](https://technet.microsoft.com/library/hh296982.aspx)voor meer informatie.

In dit artikel wordt beschreven hoe u Azure AD DS configureert om de synchronisatie service voor gebruikers profielen van share Point server toe te staan.

## <a name="before-you-begin"></a>Voordat u begint

U hebt de volgende resources en bevoegdheden nodig om dit artikel te volt ooien:

* Een actief Azure-abonnement.
    * Als u geen Azure-abonnement hebt, [maakt u een account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Een Azure Active Directory Tenant die aan uw abonnement is gekoppeld, gesynchroniseerd met een on-premises Directory of een alleen-Cloud Directory.
    * Als dat nodig is, [maakt u een Azure Active Directory-Tenant][create-azure-ad-tenant] of [koppelt u een Azure-abonnement aan uw account][associate-azure-ad-tenant].
* Een Azure Active Directory Domain Services beheerd domein ingeschakeld en geconfigureerd in uw Azure AD-Tenant.
    * Als dat nodig is, voltooit u de zelf studie voor het [maken en configureren van een Azure Active Directory Domain Services-exemplaar][create-azure-ad-ds-instance].
* Een Windows Server Management-VM die deel uitmaakt van het door Azure AD DS beheerde domein.
    * Als dat nodig is, voltooit u de zelf studie voor het [maken van een beheer-VM][tutorial-create-management-vm].
* Een gebruikers account dat lid is van de groep *Azure AD DC-Administrators* in uw Azure AD-Tenant.
* Een share point-service account voor de synchronisatie service voor gebruikers profielen.
    * Zie, indien nodig, [plan voor beheer-en service accounts in share Point server][sharepoint-service-account].

## <a name="service-accounts-overview"></a>Overzicht van service accounts

In een door Azure AD DS beheerd domein bestaat een beveiligings groep met de naam **Aad DC-service accounts** als onderdeel van de organisatie-eenheid (OE) van de *gebruiker* . Leden van deze beveiligings groep zijn gemachtigd om de volgende bevoegdheden:

- De bevoegdheid **Directory wijzigingen repliceren** op het hoofd DSE.
- De bevoegdheid **Directory wijzigingen repliceren** in de *configuratie* naamgevings`cn=configuration` context (container).

De beveiligings groep van de **Aad DC-service accounts** is ook lid van de ingebouwde groep **Pre-Windows 2000-compatibele toegang**.

Wanneer het service account voor de synchronisatie service van de share Point server-gebruikers profielen aan deze beveiligings groep wordt toegevoegd, worden de vereiste bevoegdheden verleend om correct te werken.

## <a name="enable-support-for-sharepoint-server-user-profile-sync"></a>Ondersteuning voor synchronisatie van gebruikers profielen van share Point server inschakelen

Het service account voor share Point server heeft voldoende bevoegdheden nodig om wijzigingen in de Directory te repliceren en de synchronisatie van het gebruikers profiel van share Point server correct te laten werken. Als u deze bevoegdheden wilt opgeven, voegt u het service account dat wordt gebruikt voor de synchronisatie van share point-gebruikers profielen toe aan de groep **Aad DC-service accounts** .

Voer de volgende stappen uit op de virtuele machine met Azure AD DS Management:

> [!NOTE]
> Als u groepslid maatschap wilt bewerken in een door Azure AD DS beheerd domein, moet u zijn aangemeld bij een gebruikers account dat lid is van de groep *Aad DC-Administrators* .

1. Selecteer in het Start scherm de optie **systeem beheer**. Er wordt een lijst met beschik bare beheer hulpprogramma's weer gegeven die in de zelf studie zijn geïnstalleerd om [een beheer-VM te maken][tutorial-create-management-vm].
1. Als u groepslid maatschap wilt beheren, selecteert u **Active Directory-beheercentrum** in de lijst met beheer Programma's.
1. Kies in het linkerdeel venster uw door Azure AD DS beheerde domein, zoals *aaddscontoso.com*. Er wordt een lijst met bestaande Ou's en resources weer gegeven.
1. Selecteer de organisatie-eenheid **gebruikers** en kies vervolgens de beveiligings groep *Aad DC-service accounts* .
1. Selecteer **leden**en klik vervolgens op **toevoegen...**.
1. Voer de naam van het share point-service account in en selecteer **OK**. In het volgende voor beeld heeft het share point-service account de naam *SPAdmin*:

    ![Het share point-service account toevoegen aan de beveiligings groep van de AAD DC-service accounts](./media/deploy-sp-profile-sync/add-member-to-aad-dc-service-accounts-group.png)

## <a name="next-steps"></a>Volgende stappen

Zie [Active Directory Domain Services machtigingen verlenen voor profiel synchronisatie in share Point server](https://technet.microsoft.com/library/hh296982.aspx) voor meer informatie.

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md

<!-- EXTERNAL LINKS -->
[sharepoint-service-account]: /sharepoint/security-for-sharepoint-server/plan-for-administrative-and-service-accounts
