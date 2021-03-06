---
title: Problemen met HDFS in azure HDInsight oplossen
description: Krijg antwoorden op veelgestelde vragen over het werken met HDFS en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 04/27/2020
ms.custom: seodec18
ms.openlocfilehash: 6de9e31c3e79f6d704ef8b4749d41329dcc0bddb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "82190674"
---
# <a name="troubleshoot-apache-hadoop-hdfs-by-using-azure-hdinsight"></a>Problemen met Apache Hive HDFS oplossen met behulp van Azure HDInsight

Meer informatie over de belangrijkste problemen en oplossingen bij het werken met Hadoop Distributed File System (HDFS). Voor een volledige lijst met opdrachten raadpleegt u de hand leiding voor [HDFS-opdrachten](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HDFSCommands.html) en de shell-gids voor [bestands systemen](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html).

## <a name="how-do-i-access-the-local-hdfs-from-inside-a-cluster"></a><a name="how-do-i-access-local-hdfs-from-inside-a-cluster"></a>Hoe kan ik toegang tot de lokale HDFS vanuit een cluster?

### <a name="issue"></a>Probleem

Open de lokale HDFS via de opdracht regel en toepassings code in plaats van met behulp van Azure Blob-opslag of Azure Data Lake Storage vanuit het HDInsight-cluster.

### <a name="resolution-steps"></a>Oplossingen

1. Gebruik `hdfs dfs -D "fs.default.name=hdfs://mycluster/" ...` in de opdracht prompt letterlijk, zoals in de volgende opdracht:

    ```output
    hdfs dfs -D "fs.default.name=hdfs://mycluster/" -ls /
    Found 3 items
    drwxr-xr-x   - hdiuser hdfs          0 2017-03-24 14:12 /EventCheckpoint-30-8-24-11102016-01
    drwx-wx-wx   - hive    hdfs          0 2016-11-10 18:42 /tmp
    drwx------   - hdiuser hdfs          0 2016-11-10 22:22 /user
    ```

2. Gebruik vanaf de bron code de URI `hdfs://mycluster/` letterlijk, zoals in de volgende voorbeeld toepassing:

    ```Java
    import java.io.IOException;
    import java.net.URI;
    import org.apache.commons.io.IOUtils;
    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.*;

    public class JavaUnitTests {

        public static void main(String[] args) throws Exception {

            Configuration conf = new Configuration();
            String hdfsUri = "hdfs://mycluster/";
            conf.set("fs.defaultFS", hdfsUri);
            FileSystem fileSystem = FileSystem.get(URI.create(hdfsUri), conf);
            RemoteIterator<LocatedFileStatus> fileStatusIterator = fileSystem.listFiles(new Path("/tmp"), true);
            while(fileStatusIterator.hasNext()) {
                System.out.println(fileStatusIterator.next().getPath().toString());
            }
        }
    }
    ```

3. Voer het gecompileerde jar-bestand (bijvoorbeeld een bestand met de `java-unit-tests-1.0.jar`naam) uit op het HDInsight-cluster met de volgende opdracht:

    ```apache
    hadoop jar java-unit-tests-1.0.jar JavaUnitTests
    hdfs://mycluster/tmp/hive/hive/5d9cf301-2503-48c7-9963-923fb5ef79a7/inuse.info
    hdfs://mycluster/tmp/hive/hive/5d9cf301-2503-48c7-9963-923fb5ef79a7/inuse.lck
    hdfs://mycluster/tmp/hive/hive/a0be04ea-ae01-4cc4-b56d-f263baf2e314/inuse.info
    hdfs://mycluster/tmp/hive/hive/a0be04ea-ae01-4cc4-b56d-f263baf2e314/inuse.lck
    ```

## <a name="storage-exception-for-write-on-blob"></a>Opslag uitzondering voor schrijven op BLOB

### <a name="issue"></a>Probleem

Wanneer u de `hadoop` opdrachten `hdfs dfs` of gebruikt om bestanden te schrijven die ~ 12 GB of groter zijn op een HBase-cluster, kunt u het volgende fout bericht over:

```error
ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
copyFromLocal: java.io.IOException
        at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
        at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
        at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
        at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
        at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
        at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
        at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
        at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
        at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
        ... 7 more
```

### <a name="cause"></a>Oorzaak

HBase op HDInsight-clusters worden standaard ingesteld op een blok grootte van 256 KB bij het schrijven naar Azure Storage. Hoewel het werkt voor HBase-Api's of REST-Api's, resulteert dit in een fout bij `hadoop` het `hdfs dfs` gebruik van de opdracht regel Programma's of.

### <a name="resolution"></a>Oplossing

Gebruiken `fs.azure.write.request.size` om een grotere blok grootte op te geven. U kunt deze wijziging per gebruik uitvoeren met behulp van de `-D` para meter. De volgende opdracht is een voor beeld van het gebruik van `hadoop` deze para meter met de opdracht:

```bash
hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data
```

U kunt ook de waarde `fs.azure.write.request.size` globaal verhogen door Apache Ambari te gebruiken. De volgende stappen kunnen worden gebruikt om de waarde in de Ambari-webgebruikersinterface te wijzigen:

1. Ga in uw browser naar de Ambari-webgebruikersinterface voor uw cluster. De URL is `https://CLUSTERNAME.azurehdinsight.net`, waarbij `CLUSTERNAME` de naam van het cluster is. Wanneer u hierom wordt gevraagd, voert u de naam en het wacht woord van de beheerder voor het cluster in.
2. Klik aan de linkerkant van het scherm op **HDFS**en selecteer vervolgens het tabblad **configuratie** .
3. Voer `fs.azure.write.request.size`in het veld **filter...** in.
4. Wijzig de waarde van 262144 (256 KB) in de nieuwe waarde. Bijvoorbeeld 4194304 (4 MB).

    ![Afbeelding van het wijzigen van de waarde via Ambari Web UI](./media/hdinsight-troubleshoot-hdfs/hbase-change-block-write-size.png)

Zie [HDInsight-clusters beheren met behulp van de Apache Ambari-webgebruikersinterface](hdinsight-hadoop-manage-ambari.md)voor meer informatie over het gebruik van Ambari.

## <a name="du"></a>du

Met [`-du`](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html#du) de opdracht worden de grootte van bestanden en mappen in de opgegeven map of de lengte van een bestand weer gegeven als dit een bestand is.

De `-s` optie produceert een statistische samen vatting van de bestands lengten die worden weer gegeven.  
Met `-h` de optie wordt de bestands grootte opgemaakt.

Voorbeeld:

```bash
hdfs dfs -du -s -h hdfs://mycluster/
hdfs dfs -du -s -h hdfs://mycluster/tmp
```

## <a name="rm"></a>RM

Met de opdracht [-RM](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html#rm) worden de bestanden die zijn opgegeven als argumenten, verwijderd.

Voorbeeld:

```bash
hdfs dfs -rm hdfs://mycluster/tmp/testfile
```

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet ziet of als u het probleem niet kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

* Krijg antwoorden van Azure-experts via de [ondersteuning van Azure Community](https://azure.microsoft.com/support/community/).

* Maak verbinding [@AzureSupport](https://twitter.com/azuresupport) met-het officiële Microsoft Azure account voor het verbeteren van de gebruikers ervaring. Verbinding maken met de Azure-community met de juiste resources: antwoorden, ondersteuning en experts.

* Als u meer hulp nodig hebt, kunt u een ondersteunings aanvraag indienen via de [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selecteer **ondersteuning** in de menu balk of open de hub **Help en ondersteuning** . Lees [hoe u een ondersteunings aanvraag voor Azure kunt maken](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)voor meer informatie. De toegang tot abonnementen voor abonnements beheer en facturering is inbegrepen bij uw Microsoft Azure-abonnement en technische ondersteuning wordt geleverd via een van de [ondersteunings abonnementen voor Azure](https://azure.microsoft.com/support/plans/).
