---
title: Browser verbinding op Azure DevTest Labs virtuele machines inschakelen | Microsoft Docs
description: DevTest Labs kan nu worden geïntegreerd met Azure Bastion. als eigenaar van het lab kunt u toegang tot alle virtuele lab-machines via een browser inschakelen.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: tanmayeekamath
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2019
ms.author: takamath
ms.openlocfilehash: 074d512767769fe434cd4a6d4f4e8e6a88f7ed2a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80549122"
---
# <a name="enable-browser-connection-on-lab-virtual-machines"></a>Browser verbinding inschakelen op virtuele lab-machines 
DevTest Labs kan worden geïntegreerd met [Azure Bastion](https://docs.microsoft.com/azure/bastion/), waarmee u via een browser verbinding kunt maken met uw virtuele machines. U moet eerst de browser verbinding inschakelen op de virtuele lab-machines.

Als eigenaar van een lab kunt u de toegang tot alle virtuele lab-machines inschakelen via een browser. U hebt geen aanvullende client, agent of software nodig. Azure Bastion biedt rechtstreeks een veilige en naadloze RDP/SSH-verbinding met uw virtuele machines in de Azure Portal via TLS. Wanneer u verbinding maakt via Azure Bastion, hebt u geen openbaar IP-adres nodig voor uw virtuele machines. Zie [Wat is Azure Bastion?](../bastion/bastion-overview.md) voor meer informatie.


In dit artikel wordt beschreven hoe u browser verbinding kunt inschakelen voor virtuele lab-machines.

## <a name="prerequisites"></a>Vereisten 
Implementeer een bastion-host in uw bestaande virtuele netwerk van uw Lab **(of)** Verbind uw Lab met een bastion geconfigureerd virtueel netwerk. 

Zie [een Azure bastion-host maken](../bastion/bastion-create-host-portal.md)voor meer informatie over het implementeren van een bastion-host in een virtueel netwerk. Wanneer u de bastion-host maakt, selecteert u het virtuele netwerk van de test omgeving. 

Eerst moet u een tweede subnet in het virtuele netwerk van Bastion maken, omdat de AzureBastionSubnet geen niet-Bastion resources meer mag maken. 

## <a name="create-a-second-sub-net-in-the-bastion-virtual-network"></a>Een tweede sub-net in het virtuele netwerk van Bastion maken
U kunt geen Lab Vm's maken in een Azure Bastion-subnet. Maak een ander subnet in het virtuele Bastion-netwerk, zoals wordt weer gegeven in de volgende afbeelding:

![Tweede subnet in azure Bastion virtueel netwerk](./media/connect-virtual-machine-through-browser/second-subnet.png)

## <a name="enable-vm-creation-in-the-subnet"></a>Het maken van VM'S in het subnet inschakelen
Nu kunt u de virtuele machines in dit subnet maken door de volgende stappen uit te voeren: 

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
1. Selecteer **alle services** in het navigatie menu links. 
1. Selecteer **DevTest Labs** uit de lijst. 
1. Selecteer in de lijst met Labs *uw Lab*. 

    > [!NOTE]
    > Azure Bastion is nu algemeen verkrijgbaar in de volgende regio's: VS-West, VS-Oost, Europa-west, Zuid-Centraal VS, Australië-oost en Japan-Oost. Maak in een van deze regio's een Lab als uw Lab zich niet bevindt. 
    
1. Selecteer **configuratie en beleid** in de sectie **instellingen** in het menu links. 
1. Selecteer **virtuele netwerken**.
1. Selecteer **toevoegen** op de werk balk. 
1. Selecteer het **virtuele netwerk** waarop de bastion-host is geïmplementeerd. 
1. Selecteer het subnet voor virtuele machines, niet de **AzureBastionSubnet**, de andere die u eerder hebt gemaakt. Sluit de pagina en open deze opnieuw als u het subnet in de lijst onderaan niet ziet. 

    ![Het maken van VM'S in het subnet inschakelen](./media/connect-virtual-machine-through-browser/enable-vm-creation-subnet.png)
1. Selecteer **gebruiken in de optie voor het maken van virtuele machines** . 
1. Selecteer **Opslaan** op de werkbalk. 
1. Als u een oud virtueel netwerk voor het Lab hebt, verwijdert u dit door **...* te selecteren.  en **verwijderen**. 

## <a name="enable-browser-connection"></a>Browser verbinding inschakelen 

Zodra u een virtueel Bastion-netwerk hebt geconfigureerd in de test omgeving, kunt u de verbinding met de virtuele machine van Lab inschakelen voor de browser.

Voer de volgende stappen uit om browser verbinding te maken op virtuele lab-machines:

1. Ga in het Azure Portal naar *uw Lab*.
1. Selecteer **configuratie en beleid**.
1. Selecteer in **instellingen**de optie **browser verbinding maken**. Als u deze optie niet ziet, sluit u de pagina **configuratie beleid** en opent u deze opnieuw. 

    ![Browser verbinding inschakelen](./media/enable-browser-connection-lab-virtual-machines/browser-connect.png)

## <a name="next-steps"></a>Volgende stappen
Zie het volgende artikel voor meer informatie over het maken van verbinding met uw Vm's met behulp van een browser: [verbinding maken met uw virtuele machines via een browser](connect-virtual-machine-through-browser.md)
