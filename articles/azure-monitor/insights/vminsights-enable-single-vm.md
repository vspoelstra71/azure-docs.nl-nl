---
title: Azure Monitor voor VM's in het Azure Portal inschakelen
description: Meer informatie over het evalueren van Azure Monitor voor VM's op één virtuele machine in azure of op een schaalset voor virtuele machines.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2020
ms.openlocfilehash: 45bc8f16a547d4a95820f9dcd02132844b3be83c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "79480705"
---
# <a name="enable-azure-monitor-for-vms-in-the-azure-portal"></a>Azure Monitor voor VM's in het Azure Portal inschakelen

In dit artikel wordt beschreven hoe u Azure Monitor voor VM's in te scha kelen op een klein aantal virtuele machines van Azure (Vm's) met behulp van de Azure Portal. Het doel is uw Vm's te bewaken en eventuele problemen met prestaties of Beschik baarheid te detecteren. 

Voordat u begint, controleert u [de vereisten](vminsights-enable-overview.md) en zorgt u ervoor dat uw abonnement en de resources voldoen aan de vereisten.  

## <a name="enable-monitoring-for-a-single-azure-vm"></a>Bewaking inschakelen voor één Azure VM
Controle van uw virtuele Azure-machine inschakelen:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).

1. Selecteer **virtual machines**.

1. Selecteer een VM in de lijst.

1. Selecteer op de pagina VM, in de sectie **bewaking** , de optie **inzichten** en **Schakel**deze in.

    ![Azure Monitor voor VM's inschakelen voor een VM](media/vminsights-enable-single-vm/enable-vminsights-vm-portal.png)

1. Als u een bestaande Log Analytics-werk ruimte in hetzelfde abonnement hebt, selecteert u op de pagina **Azure monitor Insights voorbereiden** .  

    De lijst preselecteert de standaard werkruimte en de locatie waar de virtuele machine in het abonnement is geïmplementeerd. 

    >[!NOTE]
    >Zie [een log Analytics-werk ruimte maken](../../azure-monitor/learn/quick-create-workspace.md)om een nieuwe log Analytics-werk ruimte te maken voor het opslaan van de bewakings gegevens van de virtuele machine. Uw Log Analytics-werk ruimte moet deel uitmaken van een van de [ondersteunde regio's](vminsights-enable-overview.md#log-analytics).

6. U ontvangt status berichten wanneer de configuratie wordt uitgevoerd.

    ![Verwerking van de implementatie van Azure Monitor voor VM's-bewaking inschakelen](media/vminsights-enable-single-vm/onboard-vminsights-vm-portal-status.png)

## <a name="enable-monitoring-for-a-single-virtual-machine-scale-set"></a>Bewaking inschakelen voor één schaalset voor virtuele machines

De bewaking van uw Azure virtual machine-schaalset inschakelen:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).

2. Selecteer **Virtual Machine Scale sets**.

3. Selecteer een schaalset voor virtuele machines in de lijst.

4. Selecteer op de pagina schaalset voor virtuele machines in de sectie **controle** de optie **inzichten** en **Schakel**deze vervolgens in.

5. Als u een bestaande Log Analytics werk ruimte wilt gebruiken op de pagina **inzichten** , selecteert u deze in de vervolg keuzelijst.

    De lijst preselecteert de standaardwerk ruimte en de standaard locatie waar de virtuele machine wordt geïmplementeerd in het abonnement. 

    ![Azure Monitor voor VM's inschakelen voor een schaalset voor virtuele machines](media/vminsights-enable-single-vm/enable-vminsights-vmss-portal.png)

    >[!NOTE]
    >Zie [een log Analytics-werk ruimte maken](../learn/quick-create-workspace.md)om een nieuwe log Analytics-werk ruimte te maken voor het opslaan van de bewakings gegevens uit de virtuele-machine schaalset. Uw Log Analytics-werk ruimte moet deel uitmaken van een van de [ondersteunde regio's](vminsights-enable-overview.md#log-analytics).

6. U ontvangt status berichten wanneer de configuratie wordt uitgevoerd.

    >[!NOTE]
    >Als u een hand matig upgrade model gebruikt voor uw schaalset, moet u de exemplaren bijwerken om de installatie te volt ooien. U kunt de upgrades starten vanaf de pagina **instanties** , in de sectie **instellingen** .
    
    ![Verwerking van de implementatie van Azure Monitor voor VM's-bewaking inschakelen](media/vminsights-enable-single-vm/onboard-vminsights-vmss-portal-status.png)

Nu u bewaking hebt ingeschakeld voor uw VM of virtuele-machine schaalset, zijn de bewakings gegevens beschikbaar voor analyse in Azure Monitor voor VM's. 

## <a name="next-steps"></a>Volgende stappen

* Zie [Azure monitor voor VM's kaart gebruiken](vminsights-maps.md)om gedetecteerde toepassings afhankelijkheden weer te geven. 
* Zie [Azure-VM-prestaties weer geven](vminsights-performance.md)om knel punten, algemeen gebruik en de prestaties van uw virtuele machine te identificeren.
