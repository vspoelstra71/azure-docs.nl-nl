---
title: Toewijzings type voor het persoonlijk bureau blad van Windows Virtual Desktop-Azure
description: Het toewijzings type configureren voor een Windows virtueel bureau blad-hostgroep voor persoonlijk bureau blad.
services: virtual-desktop
author: HeidiLohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 05/22/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: ddfd9346f4a72ceb2e8bf5c336fb3de9b5c8c5c7
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/25/2020
ms.locfileid: "83827468"
---
# <a name="configure-the-personal-desktop-host-pool-assignment-type"></a>Het toewijzings type voor de hostgroep voor persoonlijk bureau blad configureren

>[!IMPORTANT]
>Deze inhoud is van toepassing op de najaar 2019-release die geen ondersteuning biedt voor Azure Resource Manager virtueel-bureaublad objecten van Windows. Raadpleeg [dit artikel](../configure-host-pool-personal-desktop-assignment-type.md)als u probeert Azure Resource Manager virtuele Windows-bureaublad objecten te beheren die zijn geïntroduceerd in de lente 2020-update.

U kunt het toewijzings type van uw persoonlijke bureau blad-hostgroep zodanig configureren dat uw Windows Virtual Desktop-omgeving beter aansluit bij uw behoeften. In dit onderwerp wordt uitgelegd hoe u een automatische of directe toewijzing voor uw gebruikers kunt configureren.

>[!NOTE]
> De instructies in dit artikel zijn alleen van toepassing op Personal Desktop-hostgroepen, geen gegroepeerde hostgroepen, omdat gebruikers in gegroepeerde hostgroepen niet zijn toegewezen aan specifieke sessie hosts.

## <a name="configure-automatic-assignment"></a>Automatische toewijzing configureren

Automatische toewijzing is het standaard toewijzings type voor nieuwe Personal Desktop-hostgroepen die zijn gemaakt in uw Windows Virtual Desktop-omgeving. Voor het automatisch toewijzen van gebruikers is geen specifieke sessiehost vereist.

Als u gebruikers automatisch wilt toewijzen, moet u ze eerst toewijzen aan de groep persoonlijke bureau blad-hosts zodat ze het bureau blad in hun feed kunnen zien. Wanneer een toegewezen gebruiker het bureau blad start in hun feed, claimt deze een beschik bare sessiehost als ze nog niet zijn verbonden met de hostgroep, waardoor het toewijzings proces wordt voltooid.

Voordat u begint, moet u [de Power shell-module van Windows virtueel bureau blad downloaden en importeren](/powershell/windows-virtual-desktop/overview/) als u dat nog niet hebt gedaan. 

> [!NOTE]
> Zorg ervoor dat u de Windows Virtual Desktop Power shell-module versie 1.0.1534.2001 of hoger hebt geïnstalleerd voordat u deze instructies volgt.

Daarna voert u de volgende cmdlet uit om u aan te melden bij uw account:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Als u een hostgroep wilt configureren om gebruikers automatisch toe te wijzen aan Vm's, voert u de volgende Power shell-cmdlet uit:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -AssignmentType Automatic
```

Voer de volgende Power shell-cmdlet uit om een gebruiker toe te wijzen aan de groep persoonlijke bureau blad-hosts:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

## <a name="configure-direct-assignment"></a>Directe toewijzing configureren

In tegens telling tot automatische toewijzing moet u, wanneer u directe toewijzing gebruikt, de gebruiker toewijzen aan de Personal Desktop-hostgroep en een specifieke sessiehost voordat ze verbinding kunnen maken met hun persoonlijke bureau blad. Als de gebruiker alleen is toegewezen aan een hostgroep zonder een sessietime-toewijzing, hebben ze geen toegang tot bronnen.

Voer de volgende Power shell-cmdlet uit om een hostgroep zo te configureren dat deze directe toewijzing van gebruikers aan sessiehost vereist:

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -AssignmentType Direct
```

Voer de volgende Power shell-cmdlet uit om een gebruiker toe te wijzen aan de groep persoonlijke bureau blad-hosts:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

Als u een gebruiker aan een specifieke sessiehost wilt toewijzen, voert u de volgende Power shell-cmdlet uit:

```powershell
Set-RdsSessionHost <tenantname> <hostpoolname> -Name <sessionhostname> -AssignedUser <userupn>
```

## <a name="remove-a-user-assignment"></a>Een gebruikers toewijzing verwijderen

U kunt een gebruikers toewijzing verwijderen omdat de gebruiker niet langer het persoonlijke bureau blad nodig heeft, de gebruiker het bedrijf heeft verlaten of als u het bureau blad voor iemand anders wilt gebruiken.

Op dit moment kunt u de gebruikers toewijzing voor een persoonlijk bureau blad het beste verwijderen door de sessiehost volledig te verwijderen. Als u de sessiehost wilt verwijderen, voert u de volgende cmdlet uit:

```powershell
Remove-RdsSessionHost
```

Als u de sessiemap weer wilt toevoegen aan de Personal Desktop-hostgroep, verwijdert u het virtuele bureau blad van Windows op die computer en volgt u de stappen in [een hostgroep met Power shell maken](create-host-pools-powershell-2019.md) om de sessiehost opnieuw te registreren.

## <a name="next-steps"></a>Volgende stappen

Nu u het persoonlijke bureau blad-toewijzings type hebt geconfigureerd, kunt u zich aanmelden bij een virtueel-bureaubladclient van Windows om het te testen als onderdeel van een gebruikers sessie. In de volgende twee procedures wordt uitgelegd hoe u verbinding maakt met een sessie met de client van uw keuze:

- [Verbinding maken met de Windows Desktop-client](../connect-windows-7-and-10.md)
- [Verbinding maken met de webclient](connect-web-2019.md)
