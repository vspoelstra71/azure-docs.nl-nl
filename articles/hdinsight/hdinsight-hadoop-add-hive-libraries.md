---
title: Apache Hive bibliotheken tijdens het maken van het cluster-Azure HDInsight
description: Meer informatie over het toevoegen van Apache Hive bibliotheken (JAR-bestanden) aan een HDInsight-cluster tijdens het maken van het cluster.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.date: 02/14/2020
ms.openlocfilehash: 0b746963cea5a950ba47d8b4dfeb074cb0910436
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "77471020"
---
# <a name="add-custom-apache-hive-libraries-when-creating-your-hdinsight-cluster"></a>Aangepaste Apache Hive bibliotheken toevoegen bij het maken van uw HDInsight-cluster

Meer informatie over het vooraf laden van [Apache Hive](https://hive.apache.org/) bibliotheken op HDInsight. Dit document bevat informatie over het gebruik van een script actie voor het vooraf laden van bibliotheken tijdens het maken van het cluster. Bibliotheken die zijn toegevoegd aan de hand van de stappen in dit document, zijn wereld wijd beschikbaar in Hive-er hoeft geen [jar](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) te worden gebruikt om ze te laden.

## <a name="how-it-works"></a>Hoe werkt het?

Wanneer u een cluster maakt, kunt u een script actie gebruiken om cluster knooppunten te wijzigen wanneer ze worden gemaakt. In het script in dit document wordt één para meter geaccepteerd. Dit is de locatie van de bibliotheken. Deze locatie moet zich in een Azure Storage-account bevinden en de bibliotheken moeten worden opgeslagen als jar-bestanden.

Tijdens het maken van het cluster worden de bestanden door het script opgesomd, worden `/usr/lib/customhivelibs/` deze gekopieerd naar de map op de hoofd-en worker `hive.aux.jars.path` -knoop punten `core-site.xml` en vervolgens toegevoegd aan de eigenschap in het bestand. Op Linux-gebaseerde clusters wordt het `hive-env.sh` bestand ook bijgewerkt met de locatie van de bestanden.

Met de script actie in dit artikel maakt u de bibliotheken beschikbaar wanneer u een Hive-client gebruikt voor **WebHCat**en **HiveServer2**.

## <a name="the-script"></a>Het script

**Scriptlocatie**

[https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

### <a name="requirements"></a>Vereisten

* De scripts moeten worden toegepast op zowel de **hoofd knooppunten** als de **worker-knoop punten**.

* De potten die u wilt installeren, moeten worden opgeslagen in Azure Blob Storage in **één container**.

* Het opslag account met de bibliotheek van jar-bestanden **moet** worden gekoppeld aan het HDInsight-cluster tijdens het maken. Dit moet het standaard opslag account zijn of een account dat is toegevoegd via de instellingen voor het __opslag account__.

* Het WASB-pad naar de container moet worden opgegeven als een para meter voor de script actie. Als de potten bijvoorbeeld worden opgeslagen in een container met de naam **bibliotheken** op een opslag account met de naam **mijn opslag**, is `wasbs://libs@mystorage.blob.core.windows.net/`de para meter.

  > [!NOTE]  
  > In dit document wordt ervan uitgegaan dat u al een opslag account, Blob-container hebt gemaakt en de bestanden erin hebt geladen.
  >
  > Als u nog geen opslag account hebt gemaakt, kunt u dit doen via de [Azure Portal](https://portal.azure.com). U kunt vervolgens een hulp programma zoals [Azure Storage Explorer](https://storageexplorer.com/) gebruiken om een container in het account te maken en bestanden te uploaden.

## <a name="create-a-cluster-using-the-script"></a>Een cluster maken met behulp van het script

1. Begin met het inrichten van een cluster met behulp van de stappen bij het [inrichten van HDInsight-clusters in Linux](hdinsight-hadoop-provision-linux-clusters.md), maar het inrichten niet volt ooien. U kunt ook Azure PowerShell of de HDInsight .NET SDK gebruiken om een cluster te maken met behulp van dit script. Zie [HDInsight-clusters aanpassen met script acties](hdinsight-hadoop-customize-cluster-linux.md)voor meer informatie over het gebruik van deze methoden. Voor de Azure Portal selecteert u op het tabblad **configuratie en prijzen** de **+ script actie toevoegen**.

1. Als het opslag account met de bibliotheek van **jar-bestanden**anders is dan het account dat voor het cluster wordt gebruikt, moet u **extra opslag accounts**volt ooien.

1. Geef voor **script acties**de volgende informatie op:

    |Eigenschap |Waarde |
    |---|---|
    |Script type|-Aangepast|
    |Naam|Bibliotheken |
    |Bash-script-URI|`https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh`|
    |Knooppunt type (n)|Hoofd, werk nemer|
    |Parameters|Voer het WASB-adres in voor de container en het opslag account dat de potten bevat. Bijvoorbeeld `wasbs://libs@mystorage.blob.core.windows.net/`.|

    > [!NOTE]
    > Voor Apache Spark 2,1 gebruikt u deze bash-script- `https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v00.sh`URI:.

1. Ga door met het inrichten van het cluster zoals beschreven in [HDInsight-clusters inrichten op Linux](hdinsight-hadoop-provision-linux-clusters.md).

Nadat het maken van het cluster is voltooid, kunt u de potten die via dit script zijn toegevoegd, uit de Hive `ADD JAR` gebruiken zonder dat u de instructie hoeft te gebruiken.

## <a name="next-steps"></a>Volgende stappen

Zie [Apache Hive gebruiken met HDInsight](hadoop/hdinsight-use-hive.md) voor meer informatie over het werken met hive
