---
title: Windows-virtueel bureau blad Power shell-Azure
description: Problemen met Power shell oplossen bij het instellen van een virtuele Windows Desktop-Tenant omgeving.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: de3933b1686b433a720e78d89bd3e50c0ce5e5dd
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2020
ms.locfileid: "82615498"
---
# <a name="windows-virtual-desktop-powershell"></a>Windows Virtual Desktop PowerShell

>[!IMPORTANT]
>Deze inhoud is van toepassing op de najaar 2019-release die geen ondersteuning biedt voor Azure Resource Manager virtueel-bureaublad objecten van Windows. Raadpleeg [dit artikel](../troubleshoot-powershell.md)als u probeert Azure Resource Manager virtuele Windows-bureaublad objecten te beheren die zijn geïntroduceerd in de lente 2020-update.

Gebruik dit artikel om fouten en problemen op te lossen bij het gebruik van Power shell met Windows virtueel bureau blad. Zie [Windows Virtual Desktop Power shell](/powershell/module/windowsvirtualdesktop/)(Engelstalig) voor meer informatie over Extern bureaublad-services Power shell.

## <a name="provide-feedback"></a>Feedback geven

Ga naar de [technische community van Windows virtueel bureau blad](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) voor het bespreken van de Windows Virtual Desktop-service met het product team en de actieve leden van de community.

## <a name="powershell-commands-used-during-windows-virtual-desktop-setup"></a>Power shell-opdrachten die tijdens de installatie van Windows virtueel bureau blad worden gebruikt

In deze sectie vindt u Power shell-opdrachten die doorgaans worden gebruikt tijdens het instellen van Windows virtueel bureau blad en biedt manieren om problemen op te lossen die zich kunnen voordoen tijdens het gebruik van deze.

### <a name="error-add-rdsappgroupuser-command----the-specified-userprincipalname-is-already-assigned-to-a-remoteapp-app-group-in-the-specified-host-pool"></a>Fout: add-RdsAppGroupUser-opdracht--de opgegeven UserPrincipalName is al toegewezen aan een RemoteApp-app-groep in de opgegeven hostgroep

```Powershell
Add-RdsAppGroupUser -TenantName <TenantName> -HostPoolName <HostPoolName> -AppGroupName 'Desktop Application Group' -UserPrincipalName <UserName>
```

**Oorzaak:** De gebruikers naam die wordt gebruikt, is al toegewezen aan een app-groep van een ander type. Gebruikers kunnen niet worden toegewezen aan een extern bureau blad en een externe app-groep onder dezelfde sessiehost.

**Oplossen:** Als de gebruiker zowel externe apps als extern bureau blad nodig heeft, maakt u verschillende hostgroepen of verleent u gebruikers toegang tot het externe bureau blad, waardoor het gebruik van elke toepassing op de host-VM van de sessie wordt toegestaan.

### <a name="error-add-rdsappgroupuser-command----the-specified-userprincipalname-doesnt-exist-in-the-azure-active-directory-associated-with-the-remote-desktop-tenant"></a>Fout: add-RdsAppGroupUser-opdracht--de opgegeven UserPrincipalName bestaat niet in de Azure Active Directory gekoppeld aan de Extern bureaublad Tenant

```PowerShell
Add-RdsAppGroupUser -TenantName <TenantName> -HostPoolName <HostPoolName> -AppGroupName "Desktop Application Group" -UserPrincipalName <UserPrincipalName>
```

**Oorzaak:** De door de-UserPrincipalName opgegeven gebruiker is niet gevonden in de Azure Active Directory gekoppeld aan de Windows Virtual Desktop-Tenant.

**Oplossen:** Bevestig de items in de volgende lijst.

- De gebruiker is gesynchroniseerd met Azure Active Directory.
- De gebruiker is niet gekoppeld aan Business to consumer (B2C) of Business-to-Business (B2B) commerce.
- De Windows Virtual Desktop-Tenant is gekoppeld aan de juiste Azure Active Directory.

### <a name="error-get-rdsdiagnosticactivities----user-isnt-authorized-to-query-the-management-service"></a>Fout: Get-RdsDiagnosticActivities--gebruiker is niet gemachtigd om een query uit te geven op de beheer service

```PowerShell
Get-RdsDiagnosticActivities -ActivityId <ActivityId>
```

**Oorzaak:** -Tenant-para meter

**Oplossen:** Uitgifte Get-RdsDiagnosticActivities with-Tenantnaam \<tenantnaam>.

### <a name="error-get-rdsdiagnosticactivities----the-user-isnt-authorized-to-query-the-management-service"></a>Fout: Get-RdsDiagnosticActivities: de gebruiker is niet gemachtigd om een query uit te geven op de beheer service

```PowerShell
Get-RdsDiagnosticActivities -Deployment -username <username>
```

**Oorzaak:** Gebruik-implementatie switch.

**Fix:** -implementatie switch kan alleen worden gebruikt door implementatie beheerders. Deze beheerders zijn meestal leden van het virtueel-bureaublad team van Extern bureaublad-services/Windows. Vervang de switch-Deployment door-Tenantnaam \<tenantnaam>.

### <a name="error-new-rdsroleassignment----the-user-isnt-authorized-to-query-the-management-service"></a>Fout: New-RdsRoleAssignment: de gebruiker is niet gemachtigd om een query uit te zoeken op de beheer service

**Oorzaak 1:** Het account dat wordt gebruikt, heeft geen Extern bureaublad-services eigenaars machtigingen voor de Tenant.

**Oplossing 1:** Een gebruiker met Extern bureaublad-services eigenaars machtigingen moet de roltoewijzing uitvoeren.

**Oorzaak 2:** Het account dat wordt gebruikt, heeft Extern bureaublad-services eigenaars machtigingen, maar maakt geen deel uit van de Azure Active Directory van de Tenant of heeft geen machtigingen om de Azure Active Directory op te vragen waar de gebruiker zich bevindt.

**Oplossing 2:** Een gebruiker met Active Directory machtigingen moet de roltoewijzing uitvoeren.

>[!Note]
>New-RdsRoleAssignment kan geen machtigingen verlenen aan een gebruiker die zich niet in de Azure Active Directory (AD) bevinden.

## <a name="next-steps"></a>Volgende stappen

- Zie [probleemoplossings overzicht, feedback en ondersteuning](troubleshoot-set-up-overview-2019.md)voor een overzicht van het oplossen van problemen met het virtuele bureau blad van Windows en de escalatie trajecten.
- Zie [Tenant en hostgroep maken](troubleshoot-set-up-issues-2019.md)voor informatie over het oplossen van problemen bij het maken van een Tenant en een hostgroep in een virtueel-bureaublad omgeving van Windows.
- Zie voor het oplossen van problemen bij het configureren van een virtuele machine (VM) in Windows virtueel bureau blad de [virtuele machine configuratie](troubleshoot-vm-configuration-2019.md)van de host.
- Zie [Windows Virtual Desktop Service Connections](troubleshoot-service-connection-2019.md)(Engelstalig) voor het oplossen van problemen met Windows Virtual Desktop-Client verbindingen.
- Zie [problemen met de Extern bureaublad-client oplossen](../troubleshoot-client.md) om problemen met extern bureaublad-clients op te lossen
- Zie [Windows Virtual Desktop Environment](environment-setup-2019.md)(Engelstalig) voor meer informatie over de service.
- Zie [zelf studie: problemen met implementaties van Resource Manager-sjablonen oplossen](../../azure-resource-manager/templates/template-tutorial-troubleshoot.md)om de zelf studie voor problemen oplossen op te lossen.
- Zie [bewerkingen controleren met Resource Manager](../../azure-resource-manager/management/view-activity-logs.md)voor meer informatie over controle acties.
- Zie [implementatie bewerkingen weer geven](../../azure-resource-manager/templates/deployment-history.md)voor meer informatie over acties om de fouten te bepalen tijdens de implementatie.
