---
title: Een onderhouds configuratie verplaatsen naar een andere Azure-regio
description: Meer informatie over het verplaatsen van een VM-onderhouds configuratie naar een andere Azure-regio
services: virtual-machines
author: shants123
ms.service: virtual-machines
ms.topic: article
ms.tgt_pltfrm: vm
ms.date: 03/04/2020
ms.author: shants
ms.openlocfilehash: fe03bead238d3fb7bda3ee685bd5587c3e0dbc58
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "78304457"
---
# <a name="move-a-maintenance-control-configuration-to-another-region"></a>Een onderhouds beheer configuratie verplaatsen naar een andere regio

Volg dit artikel om een onderhouds beheer configuratie te verplaatsen naar een andere Azure-regio. Mogelijk wilt u een configuratie om een aantal redenen verplaatsen. Om bijvoorbeeld te profiteren van een nieuwe regio, voor het implementeren van functies of services die beschikbaar zijn in een bepaalde regio, om te voldoen aan de interne beleids-en beheer vereisten, of als reactie op de capaciteits planning.

Met onderhouds beheer met aangepaste onderhouds configuraties kunt u bepalen hoe platform updates worden toegepast op virtuele machines van [Windows](https://docs.microsoft.com/azure/virtual-machines/maintenance-control-cli?toc=/azure/virtual-machines/windows/toc.json&bc=/azure/virtual-machines/windows/breadcrumb/toc.json) en [Linux](https://docs.microsoft.com/azure/virtual-machines/maintenance-control-cli?toc=%2Fazure%2Fvirtual-machines%2Flinux%2Ftoc.json&bc=%2Fazure%2Fvirtual-machines%2Flinux%2Fbreadcrumb%2Ftoc.json&view=azure-java-stable) , en aan voor Azure toegewezen hosts. Er zijn enkele scenario's voor het verplaatsen van onderhouds beheer over regio's:

- Volg de instructies in dit artikel om de configuratie van het onderhouds beheer te verplaatsen, maar niet de resources die zijn gekoppeld aan de configuratie.
- Als u de resources wilt verplaatsen die zijn gekoppeld aan een onderhouds configuratie, maar niet de configuratie zelf, volgt u [deze instructies](move-region-maintenance-configuration-resources.md).
- Als u de onderhouds configuratie en de bijbehorende resources wilt verplaatsen, volgt u eerst de instructies in dit artikel. Volg vervolgens [deze instructies](move-region-maintenance-configuration-resources.md).

## <a name="prerequisites"></a>Vereisten

Voordat u begint met het verplaatsen van een onderhouds configuratie:

- Onderhouds configuraties zijn gekoppeld aan Azure Vm's of met Azure toegewezen hosts. Zorg ervoor dat de VM/host-resources zich in de nieuwe regio bevinden voordat u begint.
- Vinden 
    - Bestaande configuratie van onderhouds beheer.
    - De resource groepen waarin bestaande configuraties momenteel zich bevinden. 
    - De resource groepen waaraan de configuraties worden toegevoegd na het verplaatsen naar de nieuwe regio. 
    - De resources die zijn gekoppeld aan de onderhouds configuratie die u wilt verplaatsen.
    - Controleer of de resources in de nieuwe regio hetzelfde zijn als die van de huidige onderhouds configuraties. De configuraties kunnen dezelfde namen hebben in de nieuwe regio als die in de oude, maar dit is niet vereist.

## <a name="prepare-and-move"></a>Voorbereiden en verplaatsen 

1. Alle onderhouds configuraties in elk abonnement ophalen. Voer de opdracht CLI [AZ Maintenance Configuration List](https://docs.microsoft.com/cli/azure/ext/maintenance/maintenance/configuration?view=azure-cli-latest#ext-maintenance-az-maintenance-configuration-list) uit om dit te doen, waarbij u $subId vervangt door uw abonnements-id.

    ```
    az maintenance configuration list --subscription $subId --query "[*].{Name:name, Location:location, ResGroup:resourceGroup}" --output table
    ```
2. Bekijk de lijst met geretourneerde tabellen van configuratie records in het abonnement. Hier volgt een voorbeeld. De lijst bevat waarden voor uw specifieke omgeving.

    **Naam** | **Locatie** | **Resourcegroep**
    --- | --- | ---
    Onderhoud overs Laan | eastus2 | configuratie-Resource-Group
    IgniteDemoConfig | eastus2 | configuratie-Resource-Group
    defaultMaintenanceConfiguration-oostus | eastus | Test-configuratie
    

3. Sla uw lijst op als referentie. Wanneer u de configuraties verplaatst, kunt u controleren of alles is verplaatst.
4. Wijs als referentie elke configuratie/resource groep toe aan de nieuwe resource groep in de nieuwe regio.
5. Nieuwe onderhouds configuraties maken in de nieuwe regio met behulp van [Power shell](../virtual-machines/maintenance-control-powershell.md#create-a-maintenance-configuration)of [cli](../virtual-machines/maintenance-control-cli.md#create-a-maintenance-configuration).
6. Koppel de configuraties aan de resources in de nieuwe regio, met behulp van [Power shell](../virtual-machines/maintenance-control-powershell.md#assign-the-configuration)of [cli](../virtual-machines/maintenance-control-cli.md#assign-the-configuration).


## <a name="verify-the-move"></a>De verplaatsing controleren

Nadat u de configuraties hebt verplaatst, moet u de configuraties en resources in de nieuwe regio vergelijken met de lijst met tabellen die u hebt voor bereid.


## <a name="clean-up-source-resources"></a>Bron resources opschonen

Na de verplaatsing kunt u de verplaatste onderhouds configuraties verwijderen in de bron regio, [Power shell](../virtual-machines/maintenance-control-powershell.md#remove-a-maintenance-configuration)of [cli](../virtual-machines/maintenance-control-cli.md#delete-a-maintenance-configuration).


## <a name="next-steps"></a>Volgende stappen

Volg [deze instructies](move-region-maintenance-configuration-resources.md) als u resources moet verplaatsen die zijn gekoppeld aan onderhouds configuraties. 
