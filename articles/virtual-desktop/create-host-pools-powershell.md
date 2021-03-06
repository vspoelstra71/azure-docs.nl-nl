---
title: Windows Virtual Desktop-hostgroep maken Power shell-Azure
description: Een hostgroep in Windows Virtual Desktop maken met Power shell-cmdlets.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 0a4d0c22318399370b9ec11046c33a4eb5460eb3
ms.sourcegitcommit: 95269d1eae0f95d42d9de410f86e8e7b4fbbb049
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/26/2020
ms.locfileid: "83860117"
---
# <a name="create-a-host-pool-with-powershell"></a>Een hostpool maken met PowerShell

>[!IMPORTANT]
>Deze inhoud is van toepassing op de lente 2020-update met Azure Resource Manager virtueel-bureaublad objecten van Windows. Raadpleeg [dit artikel](./virtual-desktop-fall-2019/create-host-pools-powershell-2019.md)als u de versie van het Windows-bureau blad van Virtual Desktop 2019 zonder Azure Resource Manager objecten gebruikt.
>
> De Windows Virtual Desktop lente 2020-update is momenteel beschikbaar als open bare preview. Deze preview-versie is beschikbaar zonder service level agreement. het wordt niet aangeraden deze te gebruiken voor productie werkbelastingen. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. 
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Hostgroepen zijn een verzameling van een of meer identieke virtuele machines in Windows-Tenant omgevingen voor virtueel bureau blad. Elke hostgroep kan worden gekoppeld aan meerdere RemoteApp-groepen, één bureau blad-app-groep en meerdere sessie-hosts.

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u de instructies voor [het instellen van de Power shell-module](powershell-module.md)al hebt gevolgd.

## <a name="use-your-powershell-client-to-create-a-host-pool"></a>De Power shell-client gebruiken voor het maken van een hostgroep

Voer de volgende cmdlet uit om u aan te melden bij de virtuele Windows-bureaublad omgeving:

```powershell
New-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -WorkspaceName <workspacename> -HostPoolType <Pooled|Personal> -LoadBalancerType <BreadthFirst|DepthFirst|Persistent> -Location <region> -DesktopAppGroupName <appgroupname> 
```

Met deze cmdlet worden de groep voor de hostgroep, de werk ruimte en het bureau blad-app gemaakt. Daarnaast wordt de groep bureau blad-apps geregistreerd bij de werk ruimte. U kunt een werk ruimte met deze cmdlet maken of een bestaande werk ruimte gebruiken. 

Voer de volgende cmdlet uit om een registratie token te maken om een sessie-host te autoriseren om lid te worden van de hostgroep en deze op te slaan in een nieuw bestand op uw lokale computer. U kunt opgeven hoe lang het registratie token geldig is door gebruik te maken van de para meter-ExpirationHours.

>[!NOTE]
>De verval datum van het token kan niet minder dan een uur en niet meer dan een maand zijn. Als u *-ExpirationTime* buiten deze limiet hebt ingesteld, wordt het token niet door de cmdlet gemaakt.

```powershell
New-AzWvdRegistrationInfo -ResourceGroupName <resourcegroupname> -HostPoolName <hostpoolname> -ExpirationTime $((get-date).ToUniversalTime().AddDays(1).ToString('yyyy-MM-ddTHH:mm:ss.fffffffZ'))
```

Als u bijvoorbeeld een token wilt maken dat binnen twee uur verloopt, voert u deze cmdlet uit: 

```powershell
New-AzWvdRegistrationInfo -ResourceGroupName <resourcegroupname> -HostPoolName <hostpoolname> -ExpirationTime $((get-date).ToUniversalTime().AddHours(2).ToString('yyyy-MM-ddTHH:mm:ss.fffffffZ')) 
```

Daarna voert u deze cmdlet uit om Azure Active Directory gebruikers toe te voegen aan de standaard groep bureau blad-apps voor de hostgroep.

```powershell
New-AzRoleAssignment -SignInName <userupn> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <hostpoolname+"-DAG"> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups' 
```

Voer deze volgende cmdlet uit om Azure Active Directory gebruikers groepen toe te voegen aan de standaard groep bureau blad-apps voor de hostgroep:

```powershell
New-AzRoleAssignment -ObjectId <usergroupobjectid> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <hostpoolname+“-DAG”> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'
```

Voer de volgende cmdlet uit om het registratie token te exporteren naar een variabele, die u later gaat gebruiken in [REGI Steren van de virtuele machines naar de hostgroep van het virtuele Windows-bureau blad](#register-the-virtual-machines-to-the-windows-virtual-desktop-host-pool).

```powershell
$token = Get-AzWvdRegistrationInfo -ResourceGroupName <resourcegroupname> -HostPoolName <hostpoolname> 
```

## <a name="create-virtual-machines-for-the-host-pool"></a>Virtuele machines voor de hostgroep maken

U kunt nu een virtuele Azure-machine maken die kan worden gekoppeld aan uw Windows Virtual Desktop-hostgroep.

U kunt op verschillende manieren een virtuele machine maken:

- [Een virtuele machine maken op basis van een installatie kopie van Azure galerie](../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Een virtuele machine maken op basis van een beheerde installatie kopie](../virtual-machines/windows/create-vm-generalized-managed.md)
- [Een virtuele machine maken op basis van een onbeheerde installatie kopie](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-user-image-data-disks)

>[!NOTE]
>Als u een virtuele machine implementeert met Windows 7 als besturings systeem van de host, is het proces voor maken en implementeren iets anders. Zie [een virtuele Windows 7-machine implementeren op het virtuele bureau blad van Windows](./virtual-desktop-fall-2019/deploy-windows-7-virtual-machine.md)voor meer informatie.

Nadat u de virtuele machines van de sessiehost hebt gemaakt, [past u een Windows-licentie toe op een host-VM](./apply-windows-license.md#apply-a-windows-license-to-a-session-host-vm) om uw Windows-of Windows Server-machines uit te voeren zonder dat u voor een andere licentie betaalt. 

## <a name="prepare-the-virtual-machines-for-windows-virtual-desktop-agent-installations"></a>De virtuele machines voorbereiden voor installaties van Virtual Desktop agent voor Windows

U moet de volgende stappen uitvoeren om uw virtuele machines voor te bereiden voordat u de virtuele bureau blad-agents van Windows kunt installeren en de virtuele machines aan uw Windows Virtual Desktop host-groep moet registreren:

- U moet een domein-lid worden van de computer. Hierdoor kunnen binnenkomende Windows-bureaublad gebruikers worden toegewezen vanuit hun Azure Active Directory-account aan hun Active Directory account en kunnen ze toegang krijgen tot de virtuele machine.
- U moet de rol van de Extern bureaublad Session Host (RDSH) installeren als op de virtuele machine een Windows Server-besturings systeem wordt uitgevoerd. Met de rol van RDSH kunnen de virtuele bureau blad-agents van Windows correct worden geïnstalleerd.

Ga als volgt te werk op elke virtuele machine voor een geslaagde domein koppeling:

1. [Maak verbinding met de virtuele machine](../virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine) met de referenties die u hebt ingevoerd tijdens het maken van de virtuele machine.
2. Start **configuratie scherm** op de virtuele machine en selecteer **systeem**.
3. Selecteer **computer naam**, selecteer **instellingen wijzigen**en selecteer vervolgens **wijzigen...**
4. Selecteer **domein** en voer vervolgens het Active Directory domein in op het virtuele netwerk.
5. Verifieer met een domein account dat bevoegdheden heeft voor computers die lid zijn van een domein.

    >[!NOTE]
    > Als u uw Vm's lid maakt van een Azure Active Directory Domain Services-omgeving (Azure AD DS), moet u ervoor zorgen dat uw domein deelname ook lid is van de [groep Aad DC-Administrators](../active-directory-domain-services/tutorial-create-instance-advanced.md#configure-an-administrative-group).

## <a name="register-the-virtual-machines-to-the-windows-virtual-desktop-host-pool"></a>De virtuele machines registreren bij de hostgroep voor virtuele Windows-Bureau bladen

Het registreren van de virtuele machines in een Windows-hostgroep voor virtueel bureau blad is net zo eenvoudig als het installeren van de virtuele bureau blad-agents van Windows.

Ga als volgt te werk op elke virtuele machine om de virtuele bureau blad-agents van Windows te registreren:

1. [Maak verbinding met de virtuele machine](../virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine) met de referenties die u hebt ingevoerd tijdens het maken van de virtuele machine.
2. Down load en installeer de virtuele bureau blad-agent van Windows.
   - Down load de [Windows Virtual Desktop agent](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrmXv).
   - Voer het installatieprogramma uit. Wanneer het installatie programma u vraagt om het registratie token, voert u de waarde in die u hebt ontvangen van de cmdlet **export-AzWVDRegistrationInfo** .
3. Down load en installeer de bootloader voor het virtuele bureau blad-agent van Windows.
   - Down load de bootloader voor de [virtuele Windows-bureau blad-agent](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrxrH).
   - Voer het installatieprogramma uit.

>[!IMPORTANT]
>Voor het beveiligen van uw virtuele bureau blad-omgeving in azure, raden we u aan om de binnenkomende poort 3389 niet te openen op uw virtuele machines. Voor het virtuele bureau blad van Windows is geen open bare poort 3389 voor gebruikers nodig om toegang te krijgen tot de virtuele machines van de hostgroep. Als u poort 3389 moet openen voor het oplossen van problemen, raden we u aan [just-in-time-VM-toegang](../security-center/security-center-just-in-time.md)te gebruiken. U wordt ook aangeraden uw Vm's niet toe te wijzen aan een openbaar IP-adres.

## <a name="next-steps"></a>Volgende stappen

Nu u een hostgroep hebt gemaakt, kunt u deze vullen met RemoteApps. Zie de zelf studie app-groepen beheren voor meer informatie over het beheren van apps in Windows virtueel bureau blad.

> [!div class="nextstepaction"]
> [Zelf studie app-groepen beheren](./manage-app-groups.md)
