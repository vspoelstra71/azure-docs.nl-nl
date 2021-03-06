---
title: Een door de gebruiker toegewezen beheerde identiteit beheren in de Azure Portal-Azure AD
description: Stapsgewijze instructies voor het maken, vermelden, verwijderen en toewijzen van een rol aan een door de gebruiker toegewezen beheerde identiteit.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 10caa9e5e61dc1dd0c1062583f55a7357c643ce5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "79244132"
---
# <a name="create-list-delete-or-assign-a-role-to-a-user-assigned-managed-identity-using-the-azure-portal"></a>Een rol maken, weer geven, verwijderen of toewijzen aan een door de gebruiker toegewezen beheerde identiteit met behulp van de Azure Portal

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Beheerde identiteiten voor Azure-resources bieden Azure-Services met een beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken voor verificatie bij services die ondersteuning bieden voor Azure AD-verificatie, zonder dat er referenties in uw code nodig zijn. 

In dit artikel leert u hoe u een rol kunt maken, weer geven, verwijderen of toewijzen aan een door de gebruiker toegewezen beheerde identiteit met behulp van Azure Portal.

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend bent met beheerde identiteiten voor Azure-resources, raadpleegt u de [sectie Overzicht](overview.md). **Controleer het [verschil tussen een door het systeem toegewezen en door de gebruiker toegewezen beheerde identiteit](overview.md#how-does-the-managed-identities-for-azure-resources-work)**.
- Als u nog geen Azure-account hebt, [registreer u dan voor een gratis account](https://azure.microsoft.com/free/) voordat u verdergaat.

## <a name="create-a-user-assigned-managed-identity"></a>Een door de gebruiker toegewezen beheerde identiteit maken

Als u een door de gebruiker toegewezen beheerde identiteit wilt maken, moet uw account beschikken over de rol toewijzing [beheerde identiteits bijdrage](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) .

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) met behulp van een account dat is gekoppeld aan het Azure-abonnement om de door de gebruiker toegewezen beheerde identiteit te maken.
2. Typ *beheerde identiteiten*in het zoekvak en klik onder **Services**op **beheerde identiteiten**.
3. Klik op **toevoegen** en voer waarden in de volgende velden in het deel venster door **gebruiker toegewezen beheerde identiteit maken** in:
   - **Resource naam**: dit is de naam voor de door de gebruiker toegewezen beheerde identiteit, bijvoorbeeld UAI1.
   - **Abonnement**: Kies het abonnement voor het maken van de door de gebruiker toegewezen beheerde identiteit onder
   - **Resource groep**: Maak een nieuwe resource groep die uw door de gebruiker toegewezen beheerde identiteit bevat of kies **bestaande gebruiken** om de door de gebruiker toegewezen beheerde identiteit in een bestaande resource groep te maken.
   - **Locatie**: Kies een locatie om de door de gebruiker toegewezen beheerde identiteit te implementeren, bijvoorbeeld **VS-West**.
4. Klik op **maken**.

![Een door de gebruiker toegewezen beheerde identiteit maken](./media/how-to-manage-ua-identity-portal/create-user-assigned-managed-identity-portal.png)

## <a name="list-user-assigned-managed-identities"></a>Door de gebruiker toegewezen beheerde identiteiten weer geven

Als u een door de gebruiker toegewezen beheerde identiteit wilt weer geven/lezen, moet uw account beschikken over de rol [Managed Identity](/azure/role-based-access-control/built-in-roles#managed-identity-operator) of [Managed id contributor](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) .

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) met behulp van een account dat is gekoppeld aan het Azure-abonnement om de door de gebruiker toegewezen beheerde identiteiten weer te geven.
2. Typ *beheerde identiteiten*in het zoekvak en klik onder Services op **beheerde identiteiten**.
3. Er wordt een lijst geretourneerd met de door de gebruiker toegewezen beheerde identiteiten voor uw abonnement.  Als u de details van een door de gebruiker toegewezen beheerde identiteit wilt bekijken, klikt u op de naam.

![Door de gebruiker toegewezen beheerde identiteit weer geven](./media/how-to-manage-ua-identity-portal/list-user-assigned-managed-identity-portal.png)

## <a name="delete-a-user-assigned-managed-identity"></a>Een door de gebruiker toegewezen beheerde identiteit verwijderen

Als u een door de gebruiker toegewezen beheerde identiteit wilt verwijderen, moet uw account beschikken over de rol toewijzing [beheerde identiteits bijdrage](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) .

Als u een door de gebruiker toegewezen identiteit verwijdert, wordt deze niet verwijderd van de VM of de resource waaraan deze is toegewezen.  Zie een door de [gebruiker toegewezen beheerde identiteit verwijderen uit een virtuele machine](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#remove-a-user-assigned-managed-identity-from-a-vm)om de door de gebruiker toegewezen identiteit te verwijderen uit een virtuele machine.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) met behulp van een account dat is gekoppeld aan het Azure-abonnement om een door de gebruiker toegewezen beheerde identiteit te verwijderen.
2. Selecteer de door de gebruiker toegewezen beheerde identiteit en klik op **verwijderen**.
3. Kies **Ja**in het bevestigings venster.

![Door de gebruiker toegewezen beheerde identiteit verwijderen](./media/how-to-manage-ua-identity-portal/delete-user-assigned-managed-identity-portal.png)

## <a name="assign-a-role-to-a-user-assigned-managed-identity"></a>Een rol toewijzen aan een door de gebruiker toegewezen beheerde identiteit 

Als u een rol wilt toewijzen aan een door de gebruiker toegewezen beheerde identiteit, moet uw account beschikken over de toewijzing van de rol beheerder van de [gebruiker](/azure/role-based-access-control/built-in-roles#user-access-administrator) .

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) met behulp van een account dat is gekoppeld aan het Azure-abonnement om de door de gebruiker toegewezen beheerde identiteiten weer te geven.
2. Typ *beheerde identiteiten*in het zoekvak en klik onder Services op **beheerde identiteiten**.
3. Er wordt een lijst geretourneerd met de door de gebruiker toegewezen beheerde identiteiten voor uw abonnement.  Selecteer de door de gebruiker toegewezen beheerde identiteit waaraan u een rol wilt toewijzen.
4. Selecteer **toegangs beheer (IAM)** en selecteer vervolgens **functie toewijzing toevoegen**.

   ![Start van door de gebruiker toegewezen beheerde identiteit](./media/how-to-manage-ua-identity-portal/assign-role-screenshot1.png)

5. Configureer op de Blade roltoewijzing toevoegen de volgende waarden en klik vervolgens op **Opslaan**:
   - **Rol** : de rol die u wilt toewijzen
   - **Toegang toewijzen tot** de resource om de door de gebruiker toegewezen beheerde identiteit toe te wijzen
   - **Selecteer** -het lid om toegang toe te wijzen
   
   ![IAM van door de gebruiker toegewezen beheerde identiteit](./media/how-to-manage-ua-identity-portal/assign-role-screenshot2.png)  
