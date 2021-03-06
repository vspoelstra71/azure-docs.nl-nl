---
title: Gedelegeerde toegang in het virtuele bureau blad van Windows-Azure
description: Beheer mogelijkheden delegeren voor een implementatie van Windows virtueel bureau blad, met inbegrip van voor beelden.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 16b4fca475f91a8cb5b7f9a20ea5aa74b6b674a3
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2020
ms.locfileid: "82612857"
---
# <a name="delegated-access-in-windows-virtual-desktop"></a>Gedelegeerde toegang in Windows Virtual Desktop

>[!IMPORTANT]
>Deze inhoud is van toepassing op de lente 2020-update met Azure Resource Manager virtueel-bureaublad objecten van Windows. Raadpleeg [dit artikel](./virtual-desktop-fall-2019/delegated-access-virtual-desktop-2019.md)als u de versie van het Windows-bureau blad van Virtual Desktop 2019 zonder Azure Resource Manager objecten gebruikt.
>
> De Windows Virtual Desktop lente 2020-update is momenteel beschikbaar als open bare preview. Deze preview-versie is beschikbaar zonder service level agreement. het wordt niet aangeraden deze te gebruiken voor productie werkbelastingen. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. 
> Zie voor meer informatie [aanvullende gebruiks voorwaarden voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Het virtuele bureau blad van Windows beschikt over een gedelegeerd toegangs model waarmee u de hoeveelheid toegang kunt definiëren die een bepaalde gebruiker mag hebben door hen een rol toe te wijzen. Een roltoewijzing heeft drie onderdelen: beveiligingsprincipal, roldefinitie en bereik. Het Windows-model voor gedelegeerde toegang voor virtueel bureau blad is gebaseerd op het Azure RBAC-model. Voor meer informatie over specifieke roltoewijzingen en de bijbehorende onderdelen raadpleegt u [het overzicht van toegangs beheer op basis van rollen in azure](../role-based-access-control/built-in-roles.md).

Het Windows-bureau blad gedelegeerde toegang ondersteunt de volgende waarden voor elk element van de roltoewijzing:

* Beveiligings-principal
    * Gebruikers
    * Gebruikersgroepen
    * Service-principals
* Roldefinitie ophalen
    * Ingebouwde rollen
    * Aangepaste rollen
* Bereik
    * Hostgroepen
    * App-groepen
    * Workspaces

## <a name="powershell-cmdlets-for-role-assignments"></a>Power shell-cmdlets voor roltoewijzingen

Voordat u begint, moet u de instructies in [de Power shell-module instellen](powershell-module.md) voor het instellen van de Windows-module voor virtueel bureau blad-Power shell als u dat nog niet hebt gedaan.

Virtueel bureau blad van Windows maakt gebruik van op rollen gebaseerd toegangs beheer (RBAC) van Azure bij het publiceren van app-groepen naar gebruikers of gebruikers groepen. De gebruikersrol bureau blad-Virtualisatiehost wordt toegewezen aan de gebruiker of gebruikers groep en het bereik is de app-groep. Deze rol geeft de gebruiker speciale toegang tot gegevens voor de app-groep.  

Voer de volgende cmdlet uit om Azure Active Directory gebruikers toe te voegen aan een app-groep:

```powershell
New-AzRoleAssignment -SignInName <userupn> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <hostpoolname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'  
```

Voer de volgende cmdlet uit om Azure Active Directory gebruikers groep toe te voegen aan een app-groep:

```powershell
New-AzRoleAssignment -ObjectId <usergroupobjectid> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <hostpoolname> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups' 
```

## <a name="next-steps"></a>Volgende stappen

Zie de [Power shell-referentie](/powershell/windows-virtual-desktop/overview)voor een volledige lijst met Power shell-cmdlets die elke rol kan gebruiken.

Zie [ingebouwde rollen van Azure](../role-based-access-control/built-in-roles.md)voor een volledige lijst met rollen die worden ondersteund in azure RBAC.

Zie voor richt lijnen voor het instellen van een virtuele Windows-desktop omgeving [Windows Virtual Desktop Environment](environment-setup.md)(Engelstalig).
