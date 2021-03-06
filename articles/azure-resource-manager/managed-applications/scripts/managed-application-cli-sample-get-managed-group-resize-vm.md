---
title: Beheerde resource groep ophalen & grootte van Vm's wijzigen-Azure CLI
description: Biedt een Azure CLI-voorbeeld script voor het ophalen van een beheerde resource groep in een door Azure beheerde toepassing. De grootte van de virtuele machines wordt door het script aangepast.
author: tfitzmac
ms.devlang: azurecli
ms.topic: sample
ms.date: 10/25/2017
ms.author: tomfitz
ms.openlocfilehash: 878a8d660495c932cc8fa44bb1ed06e79576729b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "75650056"
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-azure-cli"></a>Resources in een beheerde resourcegroep opvragen en de grootte van virtuele machines wijzigen met Azure CLI

Met dit script worden resources opgehaald uit een beheerde resourcegroep en wordt vervolgens de grootte van de virtuele machines in die resourcegroep aangepast.


[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli[main](../../../../cli_scripts/managed-applications/get-application/get-application.sh "Get application")]


## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt om de beheerde toepassing te implementeren. Elke opdracht in de tabel is een koppeling naar opdracht-specifieke documentatie.

| Opdracht | Opmerkingen |
|---|---|
| [az managedapp list](https://docs.microsoft.com/cli/azure/managedapp#az-managedapp-list) | Hiermee vraagt u een lijst met de beheerde toepassingen op. Geef querywaarden op om gerichte resultaten te krijgen. |
| [az resource list](https://docs.microsoft.com/cli/azure/resource#az-resource-list) | Hiermee vraagt u een lijst met resources op. Geef een resourcegroep op en querywaarden om gerichte resultaten te krijgen. |
| [az vm resize](https://docs.microsoft.com/cli/azure/vm#az-vm-resize) | Hiermee werkt u de grootte van een virtuele machine bij. |


## <a name="next-steps"></a>Volgende stappen

* Zie [Overzicht van door Azure beheerde toepassingen](../overview.md) voor algemene informatie over beheerde toepassingen.
* Zie de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.
