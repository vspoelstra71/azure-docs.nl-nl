---
title: Beheerde identiteiten op een Azure-VM gebruiken voor aanmelding bij Azure AD
description: Stapsgewijze instructies en voor beelden voor het gebruik van een door Azure VM beheerde identiteit voor de Azure-service-principal voor het aanmelden bij clients en toegang tot bronnen.
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
ms.date: 12/01/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 34f4dc749c0254b5aa4e9ff018d2a869832de3f0
ms.sourcegitcommit: b1e25a8a442656e98343463aca706f4fde629867
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/27/2020
ms.locfileid: "74547392"
---
# <a name="how-to-use-managed-identities-for-azure-resources-on-an-azure-vm-for-sign-in"></a>Beheerde identiteiten voor Azure-resources gebruiken op een Azure-VM voor aanmelding 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
In dit artikel vindt u voor beelden van Power shell-en CLI-scripts voor aanmelding met beheerde identiteiten voor de service-principal van Azure resources en richt lijnen voor belang rijke onderwerpen zoals fout afhandeling.

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Als u van plan bent de Azure PowerShell of Azure CLI-voor beelden in dit artikel te gebruiken, moet u ervoor zorgen dat u de meest recente versie van [Azure PowerShell](/powershell/azure/install-az-ps) of [Azure cli](https://docs.microsoft.com/cli/azure/install-azure-cli)installeert. 

> [!IMPORTANT]
> - In elk voorbeeld script in dit artikel wordt ervan uitgegaan dat de opdracht regel-client wordt uitgevoerd op een virtuele machine met beheerde identiteiten voor Azure-resources ingeschakeld. Gebruik de VM-functie ' Connect ' in het Azure Portal om extern verbinding te maken met uw VM. Zie [beheerde identiteiten voor Azure-resources configureren op een virtuele machine met behulp van de Azure Portal](qs-configure-portal-windows-vm.md), of een van de variant artikelen (met behulp van Power shell, CLI, een sjabloon of een Azure SDK), voor meer informatie over het inschakelen van beheerde identiteiten voor Azure-resources op een virtuele machine. 
> - Om te voor komen dat er fouten optreden tijdens de toegang tot de bron, moet de beheerde identiteit van de virtuele machine ten minste ' lezer ' hebben op het juiste bereik (de virtuele machine of hoger) om Azure Resource Manager bewerkingen op de virtuele machine toe te staan. Zie [beheerde identiteiten voor Azure-resources toegang tot een resource toewijzen met behulp van de Azure Portal](howto-assign-access-portal.md) voor meer informatie.

## <a name="overview"></a>Overzicht

Beheerde identiteiten voor Azure-resources bieden een [Service-Principal-object](../develop/developer-glossary.md#service-principal-object) , dat wordt [gemaakt bij het inschakelen van beheerde identiteiten voor Azure-resources](overview.md#how-does-the-managed-identities-for-azure-resources-work) op de virtuele machine. De service-principal kan toegang krijgen tot Azure-resources en wordt gebruikt als identiteit door script/opdracht regel clients voor aanmelding en toegang tot bronnen. Traditioneel moet een script-client het volgende doen om toegang te krijgen tot beveiligde resources onder een eigen identiteit:  

   - als vertrouwelijk/webclient-toepassing worden geregistreerd en met Azure AD worden ingestemd
   - Meld u aan onder de bijbehorende service-principal met behulp van de referenties van de app (die waarschijnlijk zijn Inge sloten in het script)

Met beheerde identiteiten voor Azure-resources hoeft de script-client niet meer te worden uitgevoerd, omdat deze zich kan aanmelden onder de beheerde identiteiten voor de service-principal voor Azure-resources. 

## <a name="azure-cli"></a>Azure CLI

Het volgende script laat zien hoe u:

1. Meld u aan bij Azure AD onder de beheerde identiteit van de virtuele machine voor Azure resources Service-Principal  
2. Roep Azure Resource Manager aan en haal de Service-Principal-ID van de virtuele machine op. CLI zorgt voor het automatisch beheren van token-aanschaf/-gebruik. Zorg ervoor dat u de naam van `<VM-NAME>`uw virtuele machine vervangt door.  

   ```azurecli
   az login --identity
   
   spID=$(az resource list -n <VM-NAME> --query [*].identity.principalId --out tsv)
   echo The managed identity for Azure resources service principal ID is $spID
   ```

## <a name="azure-powershell"></a>Azure PowerShell

Het volgende script laat zien hoe u:

1. Meld u aan bij Azure AD onder de beheerde identiteit van de virtuele machine voor Azure resources Service-Principal  
2. Roep een Azure Resource Manager-cmdlet aan om informatie over de virtuele machine op te halen. Power shell zorgt ervoor dat het token gebruik automatisch voor u wordt beheerd.  

   ```azurepowershell
   Add-AzAccount -identity

   # Call Azure Resource Manager to get the service principal ID for the VM's managed identity for Azure resources. 
   $vmInfoPs = Get-AzVM -ResourceGroupName <RESOURCE-GROUP> -Name <VM-NAME>
   $spID = $vmInfoPs.Identity.PrincipalId
   echo "The managed identity for Azure resources service principal ID is $spID"
   ```

## <a name="resource-ids-for-azure-services"></a>Resource-Id's voor Azure-Services

Zie [Azure-Services die ondersteuning bieden voor Azure AD-verificatie](services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication) voor een lijst met resources die ondersteuning bieden voor Azure AD en die zijn getest met beheerde identiteiten voor Azure-resources en de bijbehorende resource-id's.

## <a name="error-handling-guidance"></a>Richt lijnen voor fout afhandeling 

Antwoorden zoals de volgende kunnen erop duiden dat de beheerde identiteit van de virtuele machine voor Azure-resources niet correct is geconfigureerd:

- Power shell: *invoke-WebRequest: kan geen verbinding maken met de externe server*
- CLI: *MSI: kan geen Token ophalen van `http://localhost:50342/oauth2/token` met de fout ' HTTPConnectionPool (host = ' localhost ', poort = 50342)* 

Als u een van deze fouten ontvangt, keert u terug naar de Azure VM in de [Azure Portal](https://portal.azure.com) en:

- Ga naar de pagina **identiteit** en zorg ervoor dat het **toegewezen systeem** is ingesteld op Ja.
- Ga naar de pagina **extensies** en zorg ervoor dat de beheerde identiteiten voor de Azure **-resources-extensie (gepland voor afschaffing in januari 2019)** zijn geïmplementeerd.

Als een van beide onjuist is, moet u de beheerde identiteiten voor Azure-resources in uw resource mogelijk opnieuw implementeren of de implementatie fout oplossen. Zie [beheerde identiteiten voor Azure-resources configureren op een virtuele machine met](qs-configure-portal-windows-vm.md) behulp van de Azure portal als u hulp nodig hebt bij de configuratie van de virtuele machine.

## <a name="next-steps"></a>Volgende stappen

- Als u beheerde identiteiten wilt inschakelen voor Azure-resources op een Azure-VM, raadpleegt u [Managed Identities voor Azure resources op een Azure-VM configureren met behulp van Power shell](qs-configure-powershell-windows-vm.md)of [beheerde identiteiten voor Azure-resources configureren op een Azure-VM met behulp van Azure cli](qs-configure-cli-windows-vm.md)






