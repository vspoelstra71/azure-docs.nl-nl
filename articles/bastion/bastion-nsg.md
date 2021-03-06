---
title: Werken met Vm's en Nsg's in azure Bastion
description: In dit artikel wordt beschreven hoe u NSG Access integreert met Azure Bastion
services: bastion
author: charwen
ms.service: bastion
ms.topic: conceptual
ms.date: 04/20/2020
ms.author: charwen
ms.openlocfilehash: 0188f9bc1c7c0e8d7fed9f590d078085b175614f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "81732203"
---
# <a name="working-with-nsg-access-and-azure-bastion"></a>Werken met NSG-toegang en Azure Bastion

Wanneer u met Azure Bastion werkt, kunt u netwerk beveiligings groepen (Nsg's) gebruiken. Zie [beveiligings groepen](../virtual-network/security-overview.md)voor meer informatie. 

![Architectuur](./media/bastion-nsg/nsg-architecture.png)

In dit diagram:

* De bastion-host wordt geïmplementeerd in het virtuele netwerk.
* De gebruiker maakt verbinding met de Azure Portal met behulp van een HTML5-browser.
* De gebruiker navigeert naar de virtuele machine van Azure naar RDP/SSH.
* Connect Integration-een RDP/SSH-sessie met één klik binnen de browser
* Op de Azure-VM is geen openbaar IP-adres vereist.

## <a name="network-security-groups"></a><a name="nsg"></a>Netwerkbeveiligingsgroepen

In deze sectie ziet u het netwerk verkeer tussen de gebruiker en Azure Bastion, en tot doel-Vm's in uw virtuele netwerk:

### <a name="azurebastionsubnet"></a><a name="apply"></a>AzureBastionSubnet

Azure Bastion wordt specifiek geïmplementeerd voor ***AzureBastionSubnet***.

* **Binnenkomend verkeer:**

   * **Binnenkomend verkeer via het open bare Internet:** De Azure-Bastion maakt een openbaar IP-adres waarvoor poort 443 is ingeschakeld op het open bare IP-adres voor binnenkomend verkeer. Poort 3389/22 hoeft niet te worden geopend op de AzureBastionSubnet.
   * **Binnenkomend verkeer vanuit het Azure Bastion-besturings vlak:** Schakel voor de connectiviteit van het besturings element het argument poort 443 binnenkomend van de **GatewayManager** -service in. Hiermee wordt het besturings vlak ingeschakeld, dat wil zeggen dat gateway beheer met Azure Bastion kan communiceren.

* **Uitgaand verkeer:**

   * Uitgaand **verkeer naar doel-vm's:** Azure Bastion bereikt de doel-Vm's via privé-IP. De Nsg's moet uitgaand verkeer naar andere doel-VM-subnetten toestaan voor poort 3389 en 22.
   * Uitgaand **verkeer naar andere open bare eind punten in Azure:** Azure Bastion moet verbinding kunnen maken met verschillende open bare eind punten in azure (bijvoorbeeld voor het opslaan van Diagnostische logboeken en logboeken voor meters). Daarom heeft Azure Bastion een uitgaand verkeer naar 443 naar **Cloud** service-tag nodig.

### <a name="target-vm-subnet"></a>Doel-VM-subnet
Dit is het subnet dat de virtuele doel machine bevat waarnaar u RDP/SSH wilt.

   * **Binnenkomend verkeer van Azure Bastion:** Azure Bastion is bereikbaar voor de doel-VM via een particulier IP-adres. RDP/SSH-poorten (poorten 3389/22 respectievelijk) moeten worden geopend op de doel-VM naast het particuliere IP-adres. Als best practice kunt u het IP-adres bereik van het Azure Bastion-subnet in deze regel toevoegen zodat alleen Bastion deze poorten op de doel-Vm's in het subnet van de doel-VM kunnen openen.


## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [Veelgestelde vragen](bastion-faq.md)voor meer informatie over Azure Bastion.
