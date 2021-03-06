---
title: URI van Azure Data Lake Storage Gen2 gebruiken
description: URI van Azure Data Lake Storage Gen2 gebruiken
author: normesta
ms.topic: conceptual
ms.author: normesta
ms.date: 12/06/2018
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.reviewer: jamesbak
ms.openlocfilehash: fa0f67e0d72ee5710a42b6de744ddae98e20220a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80437134"
---
# <a name="use-the-azure-data-lake-storage-gen2-uri"></a>URI van Azure Data Lake Storage Gen2 gebruiken

Het stuur programma [Hadoop-bestands](https://www.aosabook.org/en/hdfs.html) systeem dat compatibel is met Azure data Lake Storage Gen2, wordt aangeduid `abfs` met de schema-ID (Azure Blob File System). Het ABFS-stuur programma maakt consistent met andere Hadoop-bestandssysteem Stuur Programma's en maakt gebruik van een URI-indeling voor het adresseren van bestanden en mappen in een Data Lake Storage Gen2-account.

## <a name="uri-syntax"></a>URI-syntaxis

De syntaxis van de URI voor Data Lake Storage Gen2 is afhankelijk van het feit of uw opslag account is ingesteld op Data Lake Storage Gen2 als het standaard bestandssysteem.

Als het Data Lake Storage Gen2 geschikte account dat u wilt adresseren, **niet is** ingesteld als het standaard bestandssysteem tijdens het maken van het account, is de syntaxis van de steno-URI:

<pre>abfs[s]<sup>1</sup>://&lt;file_system&gt;<sup>2</sup>@&lt;account_name&gt;<sup>3</sup>.dfs.core.windows.net/&lt;path&gt;<sup>4</sup>/&lt;file_name&gt;<sup>5</sup></pre>

1. **Schema-ID**: `abfs` het protocol wordt als schema-ID gebruikt. U hebt de mogelijkheid om verbinding te maken met of zonder een Transport Layer Security (TLS), voorheen bekend als Secure Sockets Layer (SSL), verbinding. Gebruiken `abfss` om verbinding te maken met een TLS-verbinding.

2. **Bestands systeem**: de bovenliggende locatie van de bestanden en mappen. Dit is hetzelfde als containers in de Azure Storage blobs-service.

3. **Account naam**: de naam die aan uw opslag account is gegeven tijdens het maken.

4. **Paden**: een door slash gescheiden (`/`) weer gave van de mapstructuur.

5. **Bestands naam**: de naam van het afzonderlijke bestand. Deze para meter is optioneel als u een directory adresseert.

Als het account dat u wilt adresseren echter is ingesteld als het standaard bestandssysteem tijdens het maken van het account, is de syntaxis van de steno-URI:

<pre>/&lt;path&gt;<sup>1</sup>/&lt;file_name&gt;<sup>2</sup></pre>

1. **Pad**: een door slash gescheiden (`/`) weer gave van de mapstructuur.

2. **Bestands naam**: de naam van het afzonderlijke bestand.


## <a name="next-steps"></a>Volgende stappen

- [Azure Data Lake Storage Gen2 gebruiken met Azure HDInsight-clusters](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
