---
title: Azure Cloud Shell Snelstartgids-bash
description: Meer informatie over het gebruik van de bash-opdracht regel in uw browser met Azure Cloud Shell.
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 03/12/2018
ms.author: damaerte
ms.openlocfilehash: 58d795a5aee79e4149864a79a923ce34950b31d6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "79458066"
---
# <a name="quickstart-for-bash-in-azure-cloud-shell"></a>Quick start voor bash in Azure Cloud Shell

Dit document bevat informatie over het gebruik van bash in Azure Cloud Shell in het [Azure Portal](https://ms.portal.azure.com/).

> [!NOTE]
> Er is ook een [Power shell in azure Cloud shell](quickstart-powershell.md) Quick Start beschikbaar.

## <a name="start-cloud-shell"></a>Cloud Shell starten
1. Start **Cloud shell** vanaf de bovenste navigatie van het Azure Portal. <br>
![](media/quickstart/shell-icon.png)

2. Selecteer een abonnement om een opslag account en Microsoft Azure bestands share te maken.
3. Selecteer opslag maken

> [!TIP]
> U wordt in elke sessie automatisch geverifieerd voor Azure CLI.

### <a name="select-the-bash-environment"></a>De bash omgeving selecteren
Controleer of de vervolg keuzelijst omgeving van de linkerkant van het shell venster wordt vermeld `Bash`. <br>
![](media/quickstart/env-selector.png)

### <a name="set-your-subscription"></a>Uw abonnement instellen
1. Abonnementen weer geven waartoe u toegang hebt.
   ```azurecli-interactive
   az account list
   ```

2. Stel uw voorkeurs abonnement in: <br>
```azurecli-interactive
az account set --subscription 'my-subscription-name'
```

> [!TIP]
> Uw abonnement wordt onthouden voor toekomstige sessies met `/home/<user>/.azure/azureProfile.json`.

### <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een nieuwe resource groep in Westus met de naam ' MyRG '.
```azurecli-interactive
az group create --location westus --name MyRG
```

### <a name="create-a-linux-vm"></a>Een Linux-VM maken
Maak een Ubuntu-VM in de nieuwe resource groep. Met de Azure CLI worden SSH-sleutels gemaakt en wordt de virtuele machine ingesteld. <br>

```azurecli-interactive
az vm create -n myVM -g MyRG --image UbuntuLTS --generate-ssh-keys
```

> [!NOTE]
> Met `--generate-ssh-keys` het gebruik van Azure cli kunt u open bare en persoonlijke sleutels maken en instellen in `$Home` uw VM en Directory. Standaard sleutels worden in Cloud Shell geplaatst op `/home/<user>/.ssh/id_rsa` en. `/home/<user>/.ssh/id_rsa.pub` De `.ssh` map wordt opgeslagen in de bestands share van 5 GB die wordt gebruikt om de installatie kopie `$Home`te behouden.

Uw gebruikers naam op deze VM is uw gebruikers naam die wordt gebruikt inUser@Azure:Cloud shell ($).

### <a name="ssh-into-your-linux-vm"></a>SSH in uw virtuele Linux-machine
1. Zoek de naam van uw virtuele machine in de zoek balk Azure Portal.
2. Klik op verbinding maken om de naam van uw virtuele machine en het open bare IP-adres op te halen. <br>
   ![](media/quickstart/sshcmd-copy.png)

3. SSH in uw virtuele machine met `ssh` de Cmd.
   ```
   ssh username@ipaddress
   ```

Wanneer u de SSH-verbinding tot stand brengt, wordt de welkomst prompt van Ubuntu weer gegeven. <br>
![](media/quickstart/ubuntu-welcome.png)

## <a name="cleaning-up"></a>Opschonen 
1. Sluit uw SSH-sessie af.
   ```
   exit
   ```

2. Verwijder de resource groep en alle resources hierin.
   ```azurecli-interactive
   az group delete -n MyRG
   ```

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het persistent maken van bestanden voor bash in Cloud Shell](persisting-shell-storage.md) <br>
[Meer informatie over Azure CLI](https://docs.microsoft.com/cli/azure/) <br>
[Meer informatie over Azure Files opslag](../storage/files/storage-files-introduction.md) <br>
