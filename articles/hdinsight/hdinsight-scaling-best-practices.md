---
title: Cluster grootten schalen-Azure HDInsight
description: Een Apache Hadoop cluster elastisch schalen zodat dit overeenkomt met uw werk belasting in azure HDInsight
author: ashishthaps
ms.author: ashish
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seoapr2020
ms.date: 04/29/2020
ms.openlocfilehash: 2dae0f662eefa7f7b1f56d057cd47f1cb92244ce
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2020
ms.locfileid: "82592057"
---
# <a name="scale-azure-hdinsight-clusters"></a>Azure HDInsight-clusters schalen

HDInsight biedt flexibiliteit met opties om het aantal worker-knoop punten in uw clusters omhoog en omlaag te schalen. Deze elasticiteit stelt u in staat om een cluster na uren of in het weekend te verkleinen. En breid het uit tijdens de piek behoeften van uw bedrijf.

U moet uw cluster omhoog schalen voor periodieke batch verwerking, zodat het cluster voldoende bronnen heeft. Wanneer de verwerking is voltooid en het gebruik uitvalt, schaalt u het HDInsight-cluster naar minder werk knooppunten.

U kunt een cluster hand matig schalen met behulp van een van de hieronder beschreven methoden. U kunt ook opties voor automatisch [schalen](hdinsight-autoscale-clusters.md) gebruiken om op basis van bepaalde metrische gegevens naar boven en beneden te schalen.

> [!NOTE]  
> Alleen clusters met HDInsight-versie 3.1.3 of hoger worden ondersteund. Als u niet zeker bent van de versie van uw cluster, kunt u de pagina eigenschappen controleren.

## <a name="utilities-to-scale-clusters"></a>Hulpprogram ma's voor het schalen van clusters

Micro soft biedt de volgende hulpprogram ma's om clusters te schalen:

|Utility | Beschrijving|
|---|---|
|[PowerShell Az](https://docs.microsoft.com/powershell/azure)|[`Set-AzHDInsightClusterSize`](https://docs.microsoft.com/powershell/module/az.hdinsight/set-azhdinsightclustersize) `-ClusterName CLUSTERNAME -TargetInstanceCount NEWSIZE`|
|[PowerShell AzureRM](https://docs.microsoft.com/powershell/azure/azurerm) |[`Set-AzureRmHDInsightClusterSize`](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/set-azurermhdinsightclustersize) `-ClusterName CLUSTERNAME -TargetInstanceCount NEWSIZE`|
|[Azure-CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) | [`az hdinsight resize`](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-resize) `--resource-group RESOURCEGROUP --name CLUSTERNAME --workernode-count NEWSIZE`|
|[Klassieke Azure-CLI](hdinsight-administer-use-command-line.md)|`azure hdinsight cluster resize CLUSTERNAME NEWSIZE` |
|[Azure Portal](https://portal.azure.com)|Open het deel venster HDInsight-cluster, selecteer **cluster grootte** in het menu aan de linkerkant en typ in het deel venster cluster grootte het aantal worker-knoop punten en selecteer Opslaan.|  

![Optie voor Azure Portal schaal cluster](./media/hdinsight-scaling-best-practices/azure-portal-settings-nodes.png)

Met een van deze methoden kunt u uw HDInsight-cluster binnen enkele minuten omhoog of omlaag schalen.

> [!IMPORTANT]  
> * De klassieke Azure-CLI is afgeschaft en mag alleen worden gebruikt met het klassieke implementatie model. Voor alle andere implementaties gebruikt u de [Azure cli](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).
> * De Power shell AzureRM-module is afgeschaft.  Gebruik zo mogelijk de [AZ-module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-1.4.0) .

## <a name="impact-of-scaling-operations"></a>Gevolgen van schaal bewerkingen

Wanneer u knoop punten **toevoegt** aan uw actieve HDInsight-cluster (omhoog schalen), worden de taken niet beïnvloed. Nieuwe taken kunnen veilig worden verzonden terwijl het schaal proces wordt uitgevoerd. Als de schaal bewerking mislukt, wordt het cluster in een functionele status verlaten.

Als u knoop punten **verwijdert** (omlaag schalen), worden wachtende of actieve taken mislukken wanneer de schaal bewerking is voltooid. Deze fout is opgetreden omdat sommige services tijdens het schaal proces opnieuw worden opgestart. Het cluster kan tijdens een hand matige schaal bewerking vastlopen in de veilige modus.

De impact van het wijzigen van het aantal gegevens knooppunten varieert voor elk type cluster dat door HDInsight wordt ondersteund:

* Apache Hadoop

    U kunt het aantal worker-knoop punten in een actief Hadoop-cluster probleemloos verhogen zonder dat dit van invloed is op taken. Nieuwe taken kunnen ook worden verzonden terwijl de bewerking wordt uitgevoerd. Fouten in een schaal bewerking worden op de juiste wijze afgehandeld. Het cluster blijft altijd in een functionele status.

    Wanneer een Hadoop-cluster omlaag wordt geschaald met minder gegevens knooppunten, worden sommige services opnieuw gestart. Dit gedrag zorgt ervoor dat alle actieve en in behandeling zijnde taken mislukken bij het volt ooien van de schaal bewerking. U kunt de taken echter opnieuw verzenden nadat de bewerking is voltooid.

* Apache HBase

    U kunt tijdens het uitvoeren naadloos knoop punten toevoegen aan of verwijderen uit uw HBase-cluster. Regionale servers worden automatisch gebalanceerd binnen enkele minuten van het volt ooien van de schaal bewerking. U kunt de regionale servers echter hand matig balanceren. Meld u aan bij het cluster hoofd knooppunt en voer de volgende opdrachten uit:

    ```bash
    pushd %HBASE_HOME%\bin
    hbase shell
    balancer
    ```

    Zie [aan de slag met een Apache HBase-voor beeld in HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md)voor meer informatie over het gebruik van de HBase-shell.

* Apache Storm

    U kunt eenvoudig gegevens knooppunten toevoegen of verwijderen terwijl Storm wordt uitgevoerd. Nadat de schaal bewerking is voltooid, moet u de topologie echter opnieuw verdelen. Met herverdeling kunt u de instellingen van het [parallellisme](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) aanpassen op basis van het nieuwe aantal knoop punten in het cluster. Gebruik een van de volgende opties om de uitvoering van topologieën te herverdelen:

  * Storm-webinterface

    Gebruik de volgende stappen om een topologie te herverdelen met behulp van de Storm-gebruikers interface.

    1. Open `https://CLUSTERNAME.azurehdinsight.net/stormui` in uw webbrowser, waarbij `CLUSTERNAME` de naam van uw Storm-cluster is. Als u hierom wordt gevraagd, voert u de naam en het wacht woord voor HDInsight Cluster Administrator (admin) in die u hebt opgegeven bij het maken van het cluster.

    1. Selecteer de topologie die u wilt herverdelen en selecteer vervolgens de knop opnieuw **verdelen** . Voer de vertraging in voordat de herbalans bewerking wordt uitgevoerd.

        ![Herverdeling van HDInsight Storm-schaal](./media/hdinsight-scaling-best-practices/hdinsight-portal-scale-cluster-storm-rebalance.png)

  * Opdracht regel interface (CLI)-hulp programma

    Maak verbinding met de server en gebruik de volgende opdracht om een topologie opnieuw te verdelen:

    ```bash
     storm rebalance TOPOLOGYNAME
    ```

    U kunt ook para meters opgeven voor het overschrijven van de parallellisme-hints die oorspronkelijk door de topologie worden opgegeven. Met de onderstaande code wordt de `mytopology` topologie bijvoorbeeld opnieuw geconfigureerd op 5 werk processen, 3 voor de uitvoering van het onderdeel Blue-Spout en 10 uitvoerendeers voor het onderdeel Yellow-schicht.

    ```bash
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

* Kafka

    U moet partitie replica's na schaal bewerkingen opnieuw verdelen. Zie voor meer informatie de [hoge Beschik baarheid van gegevens met Apache Kafka in HDInsight](./kafka/apache-kafka-high-availability.md) -document.

## <a name="how-to-safely-scale-down-a-cluster"></a>Een cluster veilig omlaag schalen

### <a name="scale-down-a-cluster-with-running-jobs"></a>Een cluster omlaag schalen met actieve taken

Om te voor komen dat uw actieve taken mislukken tijdens een schaal bare bewerking, kunt u drie dingen proberen:

1. Wacht totdat de taken zijn voltooid voordat u het cluster uitschaalt.
1. Beëindig de taken hand matig.
1. Verzend de taken opnieuw nadat de schaal bewerking is voltooid.

Als u een lijst met in behandeling zijnde en actieve taken wilt weer geven, kunt u de **gebruikers interface**van het garen gebruiken door de volgende stappen uit te voeren:

1. Selecteer uw cluster in de [Azure Portal](https://portal.azure.com/).  Het cluster wordt geopend op een nieuwe portal-pagina.
2. Ga in de hoofd weergave naar **cluster dashboards** > **Ambari Home**. Voer de referenties voor uw cluster in.
3. Selecteer in de Ambari-gebruikers interface de optie **garens** in de lijst met Services in het menu aan de linkerkant.  
4. Selecteer op de pagina GARENs de optie **snelle koppelingen** en beweeg de muis aanwijzer over het actieve hoofd knooppunt en selecteer vervolgens de **gebruikers interface van Resource Manager**.

    ![Resource Manager-gebruikers interface voor Apache Ambari Quick links](./media/hdinsight-scaling-best-practices/resource-manager-ui1.png)

U kunt rechtstreeks toegang krijgen tot de Resource Manager `https://<HDInsightClusterName>.azurehdinsight.net/yarnui/hn/cluster`-gebruikers interface met.

U ziet een lijst met taken, samen met de huidige status. In de scherm afbeelding wordt momenteel één taak uitgevoerd:

![Gebruikers interface toepassingen van Resource Manager](./media/hdinsight-scaling-best-practices/resourcemanager-ui-applications.png)

Als u de toepassing hand matig wilt afbreken, voert u de volgende opdracht uit vanaf de SSH-Shell:

```bash
yarn application -kill <application_id>
```

Bijvoorbeeld:

```bash
yarn application -kill "application_1499348398273_0003"
```

### <a name="getting-stuck-in-safe-mode"></a>Blijven hangen in de veilige modus

Wanneer u omlaag schaalt in een cluster, gebruikt HDInsight Apache Ambari-beheer interfaces om de extra worker-knoop punten uit bedrijf te nemen. De knoop punten repliceren hun HDFS-blokken naar andere online worker-knoop punten. Daarna wordt het cluster veilig door HDInsight geschaald. HDFS gaat tijdens de schaal bewerking naar de veilige modus. HDFS zou moeten worden opgehaald zodra het schalen is voltooid. In sommige gevallen wordt HDFS tijdens een schaal bewerking echter vastzitten in de veilige modus vanwege een bestands blokkering onder replicatie.

HDFS wordt standaard geconfigureerd met een `dfs.replication` instelling van 1, die bepaalt hoeveel exemplaren van elk bestands blok er beschikbaar zijn. Elk exemplaar van een bestands blok wordt opgeslagen op een ander knoop punt van het cluster.

Wanneer het verwachte aantal geblokkeerde blok exemplaren niet beschikbaar is, wordt in de veilige modus en Ambari waarschuwingen gegenereerd. HDFS kan in de veilige modus voor een schaal bewerking worden ingevoerd. Het cluster kan vastzitten in de veilige modus als het vereiste aantal knoop punten niet wordt gedetecteerd voor replicatie.

### <a name="example-errors-when-safe-mode-is-turned-on"></a>Voorbeeld fouten wanneer de veilige modus is ingeschakeld

```output
org.apache.hadoop.hdfs.server.namenode.SafeModeException: Cannot create directory /tmp/hive/hive/819c215c-6d87-4311-97c8-4f0b9d2adcf0. Name node is in safe mode.
```

```output
org.apache.http.conn.HttpHostConnectException: Connect to active-headnode-name.servername.internal.cloudapp.net:10001 [active-headnode-name.servername. internal.cloudapp.net/1.1.1.1] failed: Connection refused
```

U kunt de naam van de logboeken in `/var/log/hadoop/hdfs/` de map bekijken, in de buurt van het tijdstip waarop het cluster is geschaald, om te zien wanneer de veilige modus is geactiveerd. De logboek bestanden hebben de `Hadoop-hdfs-namenode-<active-headnode-name>.*`naam.

De hoofd oorzaak is dat de Hive afhankelijk is van tijdelijke bestanden in HDFS tijdens het uitvoeren van query's. Als HDFS de veilige modus wordt geactiveerd, kan Hive geen query's uitvoeren omdat er niet naar HDFS kan worden geschreven. Tijdelijke bestanden in HDFS bevinden zich in het lokale station dat is gekoppeld aan de Vm's van het afzonderlijke worker-knoop punt. De bestanden worden tussen andere werk knooppunten met drie replica's, mini maal gerepliceerd.

### <a name="how-to-prevent-hdinsight-from-getting-stuck-in-safe-mode"></a>Voor komen dat HDInsight vastloopt in veilige modus

Er zijn verschillende manieren om te voor komen dat HDInsight in de veilige modus wordt verlaten:

* Stop alle Hive-taken voordat u HDInsight uitschaalt. U kunt ook het proces voor omlaag schalen plannen om te voor komen dat er conflicten ontstaan met het uitvoeren van Hive-taken.
* Verwijder hand matig de Scratch `tmp` Directory-bestanden van de Hive in HDFS voordat u omlaag schaalt.
* Schaal HDInsight alleen naar drie worker-knoop punten, mini maal. Vermijd het gebruik van één worker-knoop punt.
* Voer de opdracht uit om de veilige modus te verlaten, indien nodig.

Deze opties worden beschreven in de volgende secties.

#### <a name="stop-all-hive-jobs"></a>Alle Hive-taken stoppen

Stop alle Hive-taken voordat u omlaag schaalt naar één worker-knoop punt. Als uw werk belasting is gepland, voert u de schaal uit nadat het Hive-werk is uitgevoerd.

Als u de Hive-taken voor het schalen stopt, beperkt u het aantal Scratch-bestanden in de map tmp (indien van toepassing).

#### <a name="manually-clean-up-hives-scratch-files"></a>De Scratch bestanden van de Hive hand matig opschonen

Als Hive zich achter tijdelijke bestanden bevindt, kunt u deze bestanden hand matig opschonen voordat u uitschaalt om de veilige modus te voor komen.

1. Controleer welke locatie wordt gebruikt voor tijdelijke bestanden van Hive door te kijken naar `hive.exec.scratchdir` de configuratie-eigenschap. Deze para meter is ingesteld `/etc/hive/conf/hive-site.xml`in:

    ```xml
    <property>
        <name>hive.exec.scratchdir</name>
        <value>hdfs://mycluster/tmp/hive</value>
    </property>
    ```

1. Stop Hive-Services en zorg ervoor dat alle query's en taken zijn voltooid.

1. Geef een lijst weer van de inhoud van de map `hdfs://mycluster/tmp/hive/` Scratch die hierboven is gevonden om te zien of deze bestanden bevat:

    ```bash
    hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    ```

    Hier volgt een voor beeld van uitvoer wanneer bestanden bestaan:

    ```output
    sshuser@scalin:~$ hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/_tmp_space.db
    -rw-r--r--   3 hive hdfs         27 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.info
    -rw-r--r--   3 hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.lck
    drwx------   - hive hdfs          0 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699
    -rw-r--r--   3 hive hdfs         26 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699/inuse.info
    ```

1. Als u weet dat de component is uitgevoerd met deze bestanden, kunt u deze verwijderen. Zorg ervoor dat Hive geen query's uitvoert door te kijken op de pagina voor de gebruikers interface van de garens van Resource Manager.

    Voor beeld van opdracht regel voor het verwijderen van bestanden van HDFS:

    ```bash
    hadoop fs -rm -r -skipTrash hdfs://mycluster/tmp/hive/
    ```

#### <a name="scale-hdinsight-to-three-or-more-worker-nodes"></a>HDInsight schalen naar drie of meer worker-knoop punten

Als uw clusters regel matig worden vastgehouden in de veilige modus bij het schalen naar minder dan drie werk knooppunten, moet u ten minste drie worker-knoop punten gebruiken.

Het hebben van drie worker-knoop punten is kostbaarer dan de schaal naar slechts één worker-knoop punt. Met deze actie wordt echter voor komen dat uw cluster vastloopt in de veilige modus.

### <a name="scale-hdinsight-down-to-one-worker-node"></a>HDInsight omlaag schalen naar één worker-knoop punt

Zelfs als het cluster naar een knoop punt wordt geschaald, blijft worker-knoop punt 0. Worker-knoop punt 0 kan nooit buiten gebruik worden gesteld.

#### <a name="run-the-command-to-leave-safe-mode"></a>Voer de opdracht uit om de veilige modus te verlaten

De laatste optie is om de opdracht veilige modus verlaten uit te voeren. Voer de volgende opdracht uit om de veilige modus te verlaten als HDFS de veilige modus heeft opgegeven vanwege het Hive-bestand van de replicatie:

```bash
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
```

### <a name="scale-down-an-apache-hbase-cluster"></a>Een Apache HBase-cluster omlaag schalen

Regio servers worden automatisch binnen enkele minuten na het volt ooien van een schaal bewerking gebalanceerd. Voer de volgende stappen uit om regio servers hand matig te verdelen:

1. Maak verbinding met het HDInsight-cluster via SSH. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)voor meer informatie.

2. Start de HBase-shell:

    ```bash
    hbase shell
    ```

3. Gebruik de volgende opdracht om hand matig de regio servers te salderen:

    ```bash
    balancer
    ```

## <a name="next-steps"></a>Volgende stappen

* [Automatisch schalen van Azure HDInsight-clusters](hdinsight-autoscale-clusters.md)

Zie voor specifieke informatie over het schalen van uw HDInsight-cluster:

* [Apache Hadoop clusters in HDInsight beheren door gebruik te maken van de Azure Portal](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [Apache Hadoop clusters in HDInsight beheren met behulp van Azure CLI](hdinsight-administer-use-command-line.md#scale-clusters)
