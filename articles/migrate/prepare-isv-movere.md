---
title: Azure Migrate voorbereiden om met een ISV-hulp programma te werken/overzetten
description: In dit artikel wordt beschreven hoe u Azure Migrate voorbereidt op samen werking met een ISV-hulp programma of door te verplaatsen, en hoe u het hulp programma kunt gebruiken.
ms.topic: how-to
ms.date: 05/07/2020
ms.openlocfilehash: 9513e783d4f9d7be83f1434d4dd9011844af8993
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2020
ms.locfileid: "83682647"
---
# <a name="prepare-to-work-with-an-isv-tool-or-movere"></a>Bereid u voor op het werken met een ISV-hulp programma of-overwerker

Als u een ISV- [hulp programma](migrate-services-overview.md#isv-integration) hebt toegevoegd of overwerkt naar een Azure migrate-project, moet u een paar stappen uitvoeren voordat u het hulp programma koppelt en gegevens naar Azure migrate verzendt. 

## <a name="check-azure-ad-permissions"></a>Azure AD-machtigingen controleren

Uw Azure-gebruikers account heeft de volgende machtigingen nodig:

- Machtiging voor het registreren van een Azure Active Directory-app (Azure AD) bij uw Azure-Tenant
- Machtiging voor het toewijzen van een rol aan de Azure AD-app op abonnements niveau


### <a name="set-permissions-to-register-an-azure-ad-app"></a>Machtigingen instellen voor het registreren van een Azure AD-app

1. Controleer in azure AD de rol voor uw account.
2. Als u de gebruikersrol hebt, selecteert u **gebruikers instellingen** aan de linkerkant en controleert u of gebruikers toepassingen kunnen registreren. Als deze is ingesteld op **Ja**, kunnen gebruikers in de Azure AD-Tenant een app registreren. Als de instelling is ingesteld op **Nee**, kunnen alleen gebruikers met beheerders rechten apps registreren.   
3. Als u geen machtigingen hebt, kan een gebruiker met beheerders rechten uw gebruikers account met de rol [toepassings beheerder](../active-directory/users-groups-roles/directory-assign-admin-roles.md#application-administrator) opgeven, zodat u de app kunt registreren.
4. Wanneer het hulp programma is gekoppeld aan Azure Migrate, kan de beheerder de rol verwijderen uit uw account.

### <a name="set-permissions-to-assign-a-role-to-an-azure-ad-app"></a>Machtigingen instellen om een rol toe te wijzen aan een Azure AD-app
 
Uw account heeft in uw Azure-abonnement **micro soft. Authorization/*/write** Access nodig om een rol toe te wijzen aan een Azure AD-app. 

1. Open in Azure Portal **Abonnementen**.
2. Selecteer het betreffende abonnement. Als u deze niet ziet, selecteert u het **Filter globale abonnementen**. 
3. Selecteer **Mijn machtigingen**. Selecteer vervolgens **Klik hier om de volledige toegangs gegevens voor dit abonnement weer te geven**.
4. **Role assignments**  >  Controleer in de**weer gave**roltoewijzingen de machtigingen. Als uw account geen machtigingen heeft, vraagt u de abonnements beheerder om u toe te voegen aan de rol van [beheerder voor gebruikers toegang](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#user-access-administrator) of de rol [eigenaar](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) .
 

## <a name="start-using-the-tool"></a>Beginnen met het gebruik van het hulp programma

1. Als u nog geen licentie of gratis proef versie voor het hulp programma hebt, selecteert u in het onderdeel **registreren**in azure migrate **meer informatie**.
2. Volg de instructies in het hulp programma om een koppeling te maken van het hulp programma naar het Azure Migrate project en om gegevens te verzenden naar Azure Migrate.

## <a name="next-steps"></a>Volgende stappen

Volg de instructies van uw ISV of door Voer om gegevens te verzenden naar Azure Migrate.

   
