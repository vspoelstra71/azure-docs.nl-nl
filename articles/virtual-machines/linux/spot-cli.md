---
title: CLI gebruiken voor het implementeren van Azure spot-Vm's
description: Meer informatie over hoe u de CLI kunt gebruiken om Azure spot-Vm's te implementeren om kosten te besparen.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 03/25/2020
ms.author: cynthn
ms.reviewer: jagaveer
ms.openlocfilehash: 3f341271c208cc56a704c836433c33af0129a4ac
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "81758375"
---
# <a name="deploy-spot-vms-using-the-azure-cli"></a>Spot-Vm's implementeren met behulp van de Azure CLI

Met [Azure spot vm's](spot-vms.md) kunt u profiteren van onze ongebruikte capaciteit tegen een aanzienlijke kosten besparing. Op elk moment dat Azure de capaciteit nodig heeft, verwijdert de Azure-infra structuur spot Vm's. Daarom zijn de virtuele machines geschikt voor werk belastingen die onderbrekingen kunnen afhandelen, zoals batch verwerkings taken, ontwikkel-en test omgevingen, grootschalige werk belastingen en meer.

Prijzen voor spot Vm's zijn variabel, op basis van de regio en de SKU. Zie prijzen voor VM'S voor [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) en [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)voor meer informatie. 

U hebt de mogelijkheid om een maximum prijs voor de virtuele machine in te stellen die u wilt betalen, per uur. De maximale prijs voor een steun-VM kan worden ingesteld in Amerikaanse dollars (USD), met Maxi maal vijf decimalen. De waarde `0.98765`is bijvoorbeeld een maximum prijs van $0,98765 USD per uur. Als u de maximale prijs instelt op `-1`, wordt de VM niet verwijderd op basis van de prijs. De prijs voor de virtuele machine is de huidige prijs voor steun of de prijs voor een standaard-VM, die ooit kleiner is, zolang er capaciteit en quota beschikbaar zijn. Zie [Spot vm's-prijzen](spot-vms.md#pricing)voor meer informatie over het instellen van de maximum prijs.

Het proces voor het maken van een virtuele machine met behulp van de Azure CLI is hetzelfde als in het Quick Start- [artikel](/azure/virtual-machines/linux/quick-create-cli). U hoeft alleen de para meter---Priority spot toe te voegen `-1`en een maximum prijs te bieden of.


## <a name="install-azure-cli"></a>Azure CLI installeren

Als u een spot-Vm's wilt maken, moet u de Azure CLI-versie 2.0.74 of hoger uitvoeren. Voer **az --version** uit om de versie op te vragen. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli). 

Meld u aan bij Azure met [AZ login](/cli/azure/reference-index#az-login).

```azurecli
az login
```

## <a name="create-a-spot-vm"></a>Een spot-VM maken

Dit voor beeld laat zien hoe u een Linux-spot-VM implementeert die niet wordt verwijderd op basis van de prijs. 

```azurecli
az group create -n mySpotGroup -l eastus
az vm create \
    --resource-group mySpotGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --priority Spot \
    --max-price -1
```

Nadat de VM is gemaakt, kunt u een query uitvoeren om de maximale facturerings prijs voor alle virtuele machines in de resource groep weer te geven.

```azurecli
az vm list \
   -g mySpotGroup \
   --query '[].{Name:name, MaxPrice:billingProfile.maxPrice}' \
   --output table
```

**Volgende stappen**

U kunt ook een spot-VM maken met behulp van [Azure PowerShell](../windows/spot-powershell.md) of een [sjabloon](spot-template.md).

Als er een fout optreedt, raadpleegt u [fout codes](../error-codes-spot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
