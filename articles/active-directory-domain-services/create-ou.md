---
title: Een organisatie-eenheid (OE) maken in Azure AD Domain Services | Microsoft Docs '
description: Meer informatie over het maken en beheren van een aangepaste organisatie-eenheid (OE) in een Azure AD Domain Services beheerd domein.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 52602ad8-2b93-4082-8487-427bdcfa8126
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 03/31/2020
ms.author: iainfou
ms.openlocfilehash: 61f951c0dd6561fc8d5a5de6b80e3759fd42eb78
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80655545"
---
# <a name="create-an-organizational-unit-ou-in-an-azure-ad-domain-services-managed-domain"></a>Een organisatie-eenheid (OE) maken in een Azure AD Domain Services beheerd domein

Organisatie-eenheden (Ou's) in Active Directory Domain Services (AD DS) kunt u objecten logisch groeperen, zoals gebruikers accounts, service accounts of computer accounts. U kunt vervolgens beheerders toewijzen aan specifieke organisatie-eenheden en groeps beleid Toep assen om specifieke configuratie-instellingen af te dwingen.

In azure AD DS beheerde domeinen zijn de volgende twee ingebouwde organisatie-eenheden:

* *AADDC-computers* : bevat computer objecten voor alle computers die zijn gekoppeld aan het beheerde domein.
* *AADDC-gebruikers* : bevat gebruikers en groepen die zijn gesynchroniseerd vanuit de Azure AD-Tenant.

Bij het maken en uitvoeren van werk belastingen die gebruikmaken van Azure AD DS, moet u mogelijk service accounts voor toepassingen maken om zichzelf te verifiëren. Als u deze service accounts wilt ordenen, maakt u vaak een aangepaste OE in het door Azure AD DS beheerde domein en maakt u vervolgens service accounts binnen die organisatie-eenheid.

In een hybride omgeving worden organisatie-eenheden die zijn gemaakt in een on-premises AD DS omgeving niet gesynchroniseerd met Azure AD DS. In azure AD DS beheerde domeinen wordt gebruikgemaakt van een platte OE-structuur. Alle gebruikers accounts en-groepen worden opgeslagen in de container *AADDC-gebruikers* , ondanks dat ze worden gesynchroniseerd vanuit verschillende on-premises domeinen of forests, zelfs als u daar een hiërarchische OE-structuur hebt geconfigureerd.

In dit artikel wordt beschreven hoe u een organisatie-eenheid maakt in uw Azure AD DS beheerde domein.

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

## <a name="custom-ou-considerations-and-limitations"></a>Overwegingen en beperkingen voor aangepaste OE

Wanneer u aangepaste organisatie-eenheden maakt in een door Azure AD DS beheerd domein, krijgt u extra beheer flexibiliteit voor gebruikers beheer en het Toep assen van groeps beleid. Vergeleken met een on-premises AD DS omgeving, zijn er enkele beperkingen en overwegingen bij het maken en beheren van een aangepaste OE-structuur in azure AD DS:

* Gebruikers moeten lid zijn van de groep *Aad DC-Administrators* om aangepaste organisatie-eenheden te maken.
* Een gebruiker die een aangepaste OE maakt, krijgt beheerders bevoegdheden (volledig beheer) over die organisatie-eenheid en is de eigenaar van de resource.
    * Standaard heeft de groep *Aad DC-Administrators* ook volledig beheer over de aangepaste organisatie-eenheid.
* Er wordt een standaard organisatie-eenheid voor *AADDC-gebruikers* gemaakt die alle gesynchroniseerde gebruikers accounts uit uw Azure AD-Tenant bevat.
    * U kunt geen gebruikers of groepen van de organisatie-eenheid *AADDC gebruikers* verplaatsen naar aangepaste organisatie-eenheden die u maakt. Alleen gebruikers accounts of resources die zijn gemaakt in het door Azure AD DS beheerde domein kunnen worden verplaatst naar aangepaste organisatie-eenheden.
* Gebruikers accounts, groepen, service accounts en computer objecten die u onder aangepaste organisatie-eenheden maakt, zijn niet beschikbaar in uw Azure AD-Tenant.
    * Deze objecten worden niet weer gegeven met behulp van de Microsoft Graph-API of in de Azure AD-gebruikers interface. ze zijn alleen beschikbaar in uw door Azure AD DS beheerde domein.

## <a name="create-a-custom-ou"></a>Een aangepaste OE maken

Als u een aangepaste OE wilt maken, gebruikt u de Active Directory-beheer Programma's van een VM die is gekoppeld aan een domein. Met de Active Directory-beheercentrum kunt u resources weer geven, bewerken en maken in een beheerd domein van Azure AD DS, met inbegrip van organisatie-eenheden.

> [!NOTE]
> Als u een aangepaste OE wilt maken in een door Azure AD DS beheerd domein, moet u zijn aangemeld bij een gebruikers account dat lid is van de groep *Aad DC-Administrators* .

1. Meld u aan bij uw beheer-VM. Zie [verbinding maken met een virtuele machine van Windows Server][connect-windows-server-vm]voor stappen voor het maken van verbinding met behulp van de Azure Portal.
1. Selecteer in het Start scherm de optie **systeem beheer**. Er wordt een lijst met beschik bare beheer hulpprogramma's weer gegeven die in de zelf studie zijn geïnstalleerd om [een beheer-VM te maken][tutorial-create-management-vm].
1. Als u organisatie-eenheden wilt maken en beheren, selecteert u **Active Directory-beheercentrum** in de lijst met beheer Programma's.
1. Kies in het linkerdeel venster uw door Azure AD DS beheerde domein, zoals *aaddscontoso.com*. Er wordt een lijst met bestaande Ou's en resources weer gegeven:

    ![Selecteer uw door Azure AD DS beheerde domein in het Active Directory-beheercentrum](./media/create-ou/create-ou-adac-overview.png)

1. Het deel venster **taken** wordt weer gegeven aan de rechter kant van de Active Directory-Beheercentrum. Selecteer onder het domein, zoals *aaddscontoso.com*, de optie **Nieuw > organisatie-eenheid**.

    ![Selecteer de optie voor het maken van een nieuwe organisatie-eenheid in de Active Directory-beheercentrum](./media/create-ou/create-ou-adac-new-ou.png)

1. Geef in het dialoog venster **organisatie-eenheid maken** een **naam** op voor de nieuwe OE, zoals *MyCustomOu*. Geef een korte beschrijving voor de OE, zoals een *aangepaste OE voor service accounts*. Desgewenst kunt u ook het veld **beheerd door** instellen voor de organisatie-eenheid. Selecteer **OK**om de aangepaste OE te maken.

    ![Een aangepaste OE maken op basis van de Active Directory-beheercentrum](./media/create-ou/create-ou-dialog.png)

1. In de Active Directory-beheercentrum wordt de aangepaste OE nu vermeld en is deze beschikbaar voor gebruik:

    ![Aangepaste organisatie-eenheid beschikbaar voor gebruik in de Active Directory-beheercentrum](./media/create-ou/create-ou-done.png)

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen voor meer informatie over het gebruik van de beheer Programma's of het maken en gebruiken van service accounts:

* [Active Directory-beheercentrum: aan de slag](https://technet.microsoft.com/library/dd560651.aspx)
* [Service Accounts Step-by-Step Guide (Engelstalig)](https://technet.microsoft.com/library/dd548356.aspx)

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
[connect-windows-server-vm]: join-windows-vm.md#connect-to-the-windows-server-vm
