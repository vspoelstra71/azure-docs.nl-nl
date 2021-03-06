---
title: Azure-resource rollen toewijzen in Privileged Identity Management-Azure Active Directory | Microsoft Docs
description: Meer informatie over het toewijzen van Azure-resource rollen in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 10/23/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 34051a31c6ccf69356f330d7c5ecb009f760857a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "79266557"
---
# <a name="assign-azure-resource-roles-in-privileged-identity-management"></a>Azure-resource rollen toewijzen in Privileged Identity Management

Azure Active Directory (Azure AD) Privileged Identity Management (PIM) kan de ingebouwde Azure-resource rollen beheren, evenals aangepaste rollen, inclusief (maar niet beperkt tot):

- Eigenaar
- Beheerder van gebruikerstoegang
- Inzender
- Beveiligingsbeheerder
- Beveiligings beheer

> [!NOTE]
> Gebruikers of leden van een groep die zijn toegewezen aan de rol van eigenaar of gebruikers toegangs beheerder abonnement, en globale Azure AD-beheerders die het beheer van abonnementen in azure AD inschakelen, hebben standaard resource Administrator-machtigingen. Deze beheerders kunnen rollen toewijzen, rolinstellingen configureren en toegang controleren met behulp van Privileged Identity Management voor Azure-resources. Een gebruiker kan Privileged Identity Management niet beheren voor resources zonder resource beheerders machtigingen. Bekijk de lijst met [ingebouwde rollen voor Azure-resources](../../role-based-access-control/built-in-roles.md).

## <a name="assign-a-role"></a>Een rol toewijzen

Volg deze stappen om een gebruiker in aanmerking te laten komen voor een Azure-resource functie.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/) met een gebruiker die lid is van de beheerdersrol [privileged Role](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) .

    Voor informatie over het verlenen van een andere beheerder toegang tot het beheren van Privileged Identity Management raadpleegt u [toegang verlenen aan andere beheerders om privileged Identity Management te beheren](pim-how-to-give-access-to-pim.md).

1. Open **Azure AD privileged Identity Management**.

1. Selecteer **Azure-resources**.

1. Gebruik het **resource filter** om de lijst met beheerde resources te filteren.

    ![Lijst met Azure-resources die u wilt beheren](./media/pim-resource-roles-assign-roles/resources-list.png)

1. Selecteer de resource die u wilt beheren, zoals een abonnement of beheer groep.

1. Selecteer onder beheren de optie **rollen** om de lijst met rollen voor Azure-resources weer te geven.

    ![Azure-resources rollen](./media/pim-resource-roles-assign-roles/resources-roles.png)

1. Selecteer **lid toevoegen** om het deel venster nieuwe toewijzing te openen.

1. Selecteer **een rol selecteren** om het deel venster een rol selecteren te openen.

    ![Deel venster nieuwe toewijzing](./media/pim-resource-roles-assign-roles/resources-select-role.png)

1. Selecteer een rol die u wilt toewijzen en klik vervolgens op **selecteren**.

    Het deel venster een lid of groep selecteren wordt geopend.

1. Selecteer een lid of groep die u aan de rol wilt toewijzen en klik vervolgens op **selecteren**.

    ![Een lid of groeps deel venster selecteren](./media/pim-resource-roles-assign-roles/resources-select-member-or-group.png)

    Het deel venster lidmaatschaps instellingen wordt geopend.

1. Selecteer in **aanmerking komend** of **actief**in de lijst **toewijzings type** .

    ![Deel venster instellingen voor lidmaatschappen](./media/pim-resource-roles-assign-roles/resources-membership-settings-type.png)

    Privileged Identity Management voor Azure-resources biedt twee verschillende toewijzings typen:

    - Voor **in aanmerking komende** toewijzingen moet het lid van de rol een actie uitvoeren om de rol te gebruiken. Acties kunnen bijvoorbeeld een meervoudige verificatiecontrole, het opgeven van een zakelijke reden of het vragen om toestemming bij aangewezen fiatteurs zijn.

    - Voor **actieve** toewijzingen hoeft het lid geen actie te ondernemen om de rol te gebruiken. Aan leden die als actief zijn toegewezen, zijn de bevoegdheden altijd toegewezen aan de rol.

1. Als de toewijzing permanent moet zijn (permanent in aanmerking komend of permanent toegewezen), schakelt u het selectie vakje **permanent** in.

    Afhankelijk van de rolinstellingen, wordt het selectie vakje mogelijk niet weer gegeven of kan het niet worden gewijzigd.

1. Als u een specifieke duur voor de toewijzing wilt opgeven, schakelt u het selectie vakje uit en wijzigt u de begin-en/of eind datum en-tijd vakken.

    ![Instellingen voor lidmaatschappen-datum en tijd](./media/pim-resource-roles-assign-roles/resources-membership-settings-date.png)

1. Als u klaar bent, selecteert u **gereed**.

    ![Nieuwe toewijzing-toevoegen](./media/pim-resource-roles-assign-roles/resources-new-assignment-add.png)

1. Als u de nieuwe roltoewijzing wilt maken, selecteert u **toevoegen**. Er wordt een melding van de status weer gegeven.

    ![Nieuwe toewijzing-melding](./media/pim-resource-roles-assign-roles/resources-new-assignment-notification.png)

## <a name="update-or-remove-an-existing-role-assignment"></a>Een bestaande roltoewijzing bijwerken of verwijderen

Volg deze stappen om een bestaande roltoewijzing bij te werken of te verwijderen.

1. Open **Azure AD privileged Identity Management**.

1. Selecteer **Azure-resources**.

1. Selecteer de resource die u wilt beheren, zoals een abonnement of beheer groep.

1. Selecteer onder beheren de optie **rollen** om de lijst met rollen voor Azure-resources weer te geven.

    ![Azure-resource rollen: rol selecteren](./media/pim-resource-roles-assign-roles/resources-update-select-role.png)

1. Selecteer de rol die u wilt bijwerken of verwijderen.

1. Zoek naar de roltoewijzing op de tabbladen **in aanmerking komende rollen** of **actieve rollen** .

    ![Roltoewijzing bijwerken of verwijderen](./media/pim-resource-roles-assign-roles/resources-update-remove.png)

1. Selecteer **bijwerken** of **verwijderen** om de roltoewijzing bij te werken of te verwijderen.

    Zie [Azure-resource rollen uitbreiden of vernieuwen in privileged Identity Management](pim-resource-roles-renew-extend.md)voor meer informatie over het uitbreiden van een roltoewijzing.

## <a name="next-steps"></a>Volgende stappen

- [Azure-resource rollen in Privileged Identity Management uitbreiden of vernieuwen](pim-resource-roles-renew-extend.md)
- [Instellingen voor Azure-resource-rollen configureren in Privileged Identity Management](pim-resource-roles-configure-role-settings.md)
- [Azure AD-rollen toewijzen in Privileged Identity Management](pim-how-to-add-role-to-user.md)
