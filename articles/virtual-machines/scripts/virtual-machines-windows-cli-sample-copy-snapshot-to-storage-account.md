---
title: Een moment opname kopiëren naar een opslag account in een andere regio-CLI-voor beeld
description: 'Azure CLI-voorbeeldscript: exporteer/kopieer een momentopname als VHD naar een opslagaccount in dezelfde of een andere regio.'
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc,seodec18
ms.openlocfilehash: d470c20718b007b05360ad539973cbc2a9d7ad98
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "81459573"
---
# <a name="exportcopy-a-snapshot-to-a-storage-account-in-different-region-with-cli"></a>Een momentopname exporteren/kopiëren naar een opslagaccount in een andere regio met CLI

Met dit script wordt een beheerde momentopname geëxporteerd naar een opslagaccount in een andere regio. Eerst wordt de SAS-URI van de momentopname gegenereerd, die vervolgens wordt gekopieerd naar een opslagaccount in een andere regio. Gebruik dit script voor het onderhouden van back-ups van uw beheerde schijven in een andere regio voor herstel na noodgevallen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshots-to-storage-account/copy-snapshots-to-storage-account.sh "Copy snapshot")]

## <a name="script-explanation"></a>Uitleg van het script

Dit script gebruikt de volgende opdrachten voor het genereren van de SAS-URI voor een beheerde momentopname en kopieert de momentopname naar een opslagaccount met behulp van de SAS-URI. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [az snapshot grant-access](https://docs.microsoft.com/cli/azure/snapshot) | Hiermee genereert u alleen-lezen SAS dat wordt gebruikt om het onderliggende VHD-bestand te kopiëren naar een opslagaccount of om dat bestand lokaal te downloaden  |
| [az storage blob copy start](https://docs.microsoft.com/cli/azure/storage/blob/copy) | Hiermee kopieert u een blob asynchroon vanuit het ene opslagaccount naar het andere |

## <a name="next-steps"></a>Volgende stappen

[Een beheerde schijf maken op basis van een VHD](virtual-machines-windows-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

Zie de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Meer informatie over de CLI-voorbeeld scripts voor virtuele machines en beheerde schijven vindt u in de documentatie van de [Azure Windows-VM](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
