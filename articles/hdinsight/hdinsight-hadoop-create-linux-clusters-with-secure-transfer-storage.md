---
title: Apache Hadoop & beveiligde overdrachts opslag-Azure HDInsight
description: Informatie over het maken van HDInsight-clusters met Azure-opslag-accounts voor veilige overdracht.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/18/2020
ms.openlocfilehash: c1e5ca8b0bb828e5e8ce896bba6a5278266b118e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "77560079"
---
# <a name="apache-hadoop-clusters-with-secure-transfer-storage-accounts-in-azure-hdinsight"></a>Apache Hadoop clusters met opslag accounts voor veilige overdracht in azure HDInsight

De functie [Veilige overdracht vereist](../storage/common/storage-require-secure-transfer.md) verhoogt de beveiliging van uw Azure-opslagaccount door alle aanvragen naar uw account af te dwingen via een beveiligde verbinding. Deze functie en het wasbs-schema worden alleen ondersteund door HDInsight-clusterversie 3.6 of nieuwer.

> [!IMPORTANT]
> Het inschakelen van beveiligde opslag overdracht na het maken van een cluster kan leiden tot fouten bij het gebruik van uw opslag account en wordt niet aanbevolen. Het is beter om een nieuw cluster te maken met behulp van een opslag account waarvoor beveiligde overdracht al is ingeschakeld.

## <a name="storage-accounts"></a>Opslagaccounts

### <a name="azure-portal"></a>Azure Portal

Standaard is de eigenschap beveiligde overdracht vereist ingeschakeld wanneer u een opslag account maakt in Azure Portal.

Zie voor het bijwerken van een bestaand opslag account met Azure Portal [vereisen beveiligde overdracht met Azure Portal](../storage/common/storage-require-secure-transfer.md#require-secure-transfer-for-an-existing-storage-account).

### <a name="powershell"></a>PowerShell

Zorg ervoor dat de para meter `-EnableHttpsTrafficOnly` is ingesteld voor de Power shell- `1`cmdlet [New-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageaccount).

Zie [veilige overdracht vereisen met Power shell](../storage/common/storage-require-secure-transfer.md#require-secure-transfer-with-powershell)voor meer informatie over het bijwerken van een bestaand opslag account met Power shell.

### <a name="azure-cli"></a>Azure CLI

Voor de Azure CLI-opdracht [AZ Storage account create](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create), controleert `--https-only` u of de `true`para meter is ingesteld op.

Zie [veilige overdracht vereisen met Azure cli](../storage/common/storage-require-secure-transfer.md#require-secure-transfer-with-azure-cli)voor meer informatie over het bijwerken van een bestaand opslag account met Azure cli.

## <a name="add-additional-storage-accounts"></a>Extra opslagaccounts toevoegen

Er zijn verschillende opties om extra opslagaccounts met veilige overdracht toe te voegen:

* De Azure Resource Manager-sjabloon in de laatste sectie wijzigen.
* Een cluster maken met [Azure Portal](https://portal.azure.com) en een gekoppelde opslagaccount opgeven.
* Een scriptactie gebruiken om extra opslagaccounts met veilige overdracht toe te voegen aan een bestaand HDInsight-cluster. Zie [Extra opslagaccounts toevoegen aan HDInsight](hdinsight-hadoop-add-storage.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

* Het gebruik van Azure Storage (WASB) in plaats van [Apache HADOOP HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html) als de standaard gegevens opslag
* Zie [Azure Storage gebruiken met HDInsight](hdinsight-hadoop-use-blob-storage.md) voor meer informatie over hoe HDInsight Azure Storage gebruikt.
* Zie [Gegevens uploaden naar HDInsight](hdinsight-upload-data.md) voor meer informatie over het uploaden van gegevens naar HDInsight.
