---
title: Wisselen tussen besturingssysteem schijven met behulp van CLI
description: Wijzig de besturingssysteem schijf die wordt gebruikt door een virtuele Azure-machine met behulp van de CLI.
author: cynthn
ms.service: virtual-machines
ms.subservice: disks
ms.workload: infrastructure-services
ms.topic: article
ms.date: 04/24/2018
ms.author: cynthn
ms.openlocfilehash: a8c879ae56eb22d9817841667d33e37109a4f63d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "81759399"
---
# <a name="change-the-os-disk-used-by-an-azure-vm-using-the-cli"></a>De besturingssysteem schijf wijzigen die wordt gebruikt door een virtuele machine van Azure met de CLI


Als u een bestaande VM hebt, maar u de schijf wilt wisselen voor een back-upschijf of een andere besturingssysteem schijf, kunt u de Azure CLI gebruiken om de besturingssysteem schijven te wisselen. U hoeft de virtuele machine niet te verwijderen en opnieuw te maken. U kunt zelfs een beheerde schijf in een andere resource groep gebruiken, zolang deze nog niet in gebruik is.

De VM moet stopped\deallocated zijn. vervolgens kan de bron-ID van de beheerde schijf worden vervangen door de resource-ID van een andere beheerde schijf. 

Zorg ervoor dat de VM-grootte en het opslag type compatibel zijn met de schijf die u wilt koppelen. Als de schijf die u wilt gebruiken zich bijvoorbeeld in Premium Storage bevindt, moet de virtuele machine Premium Storage kunnen zijn (zoals een grootte van de DS-serie).

Voor dit artikel is Azure CLI versie 2.0.25 of hoger vereist. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 


Gebruik [AZ Disk List](/cli/azure/disk) om een lijst op te halen van de schijven in de resource groep.

```azurecli-interactive
az disk list \
   -g myResourceGroupDisk \
   --query '[*].{diskId:id}' \
   --output table
```


Gebruik [AZ VM stop](/cli/azure/vm) om de virtuele machine te stop\deallocateen voordat u de schijven verwisselt.

```azurecli-interactive
az vm stop \
   -n myVM \
   -g myResourceGroup
```


Gebruik [AZ VM update](/cli/azure/vm#az-vm-update) met de volledige resource-id van de nieuwe schijf voor `--osdisk` de para meter 

```azurecli-interactive 
az vm update \
   -g myResourceGroup \
   -n myVM \
   --os-disk /subscriptions/<subscription ID>/resourceGroups/swap/providers/Microsoft.Compute/disks/myDisk 
   ```
   
Start de VM opnieuw [op met AZ VM start](/cli/azure/vm).

```azurecli-interactive
az vm start \
   -n myVM \
   -g myResourceGroup
```

   
**Volgende stappen**

Als u een kopie van een schijf wilt maken, raadpleegt u [een moment opname van een schijf](snapshot-copy-managed-disk.md).
