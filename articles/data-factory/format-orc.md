---
title: De indeling ORC in Azure Data Factory
description: In dit onderwerp wordt beschreven hoe u kunt omgaan met de ORC-indeling in Azure Data Factory.
author: linda33wj
manager: shwang
ms.reviewer: craigg
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: jingwang
ms.openlocfilehash: 9b68d3724c6390fc5d30745924451e27ef9855b3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "81417722"
---
# <a name="orc-format-in-azure-data-factory"></a>De indeling ORC in Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Volg dit artikel als u **de Orc-bestanden wilt parseren of de gegevens wilt schrijven naar de Orc-indeling**. 

De ORC-indeling wordt ondersteund voor de volgende connectors: [Amazon S3](connector-amazon-simple-storage-service.md), [Azure Blob](connector-azure-blob-storage.md), [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md), [Azure data Lake Storage Gen2](connector-azure-data-lake-storage.md), [Azure File Storage](connector-azure-file-storage.md), [Bestands systeem](connector-file-system.md), [FTP](connector-ftp.md), [Google Cloud Storage](connector-google-cloud-storage.md), [HDFS](connector-hdfs.md), [http](connector-http.md)en [SFTP](connector-sftp.md).

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie het artikel [gegevens sets](concepts-datasets-linked-services.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevens sets. Deze sectie bevat een lijst met eigenschappen die worden ondersteund door de ORC-gegevensset.

| Eigenschap         | Beschrijving                                                  | Vereist |
| ---------------- | ------------------------------------------------------------ | -------- |
| type             | De eigenschap type van de DataSet moet worden ingesteld op **Orc**. | Ja      |
| location         | Locatie-instellingen van bestand (en). Elke connector op basis van bestanden heeft een eigen locatie type en ondersteunde eigenschappen `location`onder. **Zie de sectie Details in connector artikel-> eigenschappen van gegevensset**. | Ja      |

Hieronder ziet u een voor beeld van een ORC-gegevensset op Azure Blob Storage:

```json
{
    "name": "ORCDataset",
    "properties": {
        "type": "Orc",
        "linkedServiceName": {
            "referenceName": "<Azure Blob Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobStorageLocation",
                "container": "containername",
                "folderPath": "folder/subfolder",
            }
        }
    }
}
```

Houd rekening met de volgende punten:

* Complexe gegevens typen worden niet ondersteund (STRUCT, kaart, lijst, samen VOEGing).
* Spaties in kolom naam wordt niet ondersteund.
* Een ORC-bestand heeft drie [opties voor compressie](https://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB, SNAPPY. Data Factory ondersteunt het lezen van gegevens uit ORC-bestanden in een van deze gecomprimeerde indelingen. Hierbij wordt de compressiecodec in de metagegevens gebruikt om de gegevens te lezen. Bij het schrijven naar een ORC-bestand kiest Data Factory echter ZLIB, de standaardinstelling voor ORC. Er is momenteel geen optie om dit gedrag te overschrijven.

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie het artikel [pijp lijnen](concepts-pipelines-activities.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. In deze sectie vindt u een lijst met eigenschappen die worden ondersteund door de ORC-bron en Sink.

### <a name="orc-as-source"></a>ORC als bron

De volgende eigenschappen worden ondersteund in de sectie *** \*bron\* *** van de Kopieer activiteit.

| Eigenschap      | Beschrijving                                                  | Vereist |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | De eigenschap type van de bron van de Kopieer activiteit moet zijn ingesteld op **OrcSource**. | Ja      |
| storeSettings | Een groep eigenschappen voor het lezen van gegevens uit een gegevens archief. Elke connector op basis van een bestand heeft zijn eigen ondersteunde Lees `storeSettings`instellingen onder. **Zie de sectie Details in connector artikel-> eigenschappen van de Kopieer activiteit**. | Nee       |

### <a name="orc-as-sink"></a>ORC als Sink

De volgende eigenschappen worden ondersteund in het gedeelte *** \*Sink\* *** van de Kopieer activiteit.

| Eigenschap      | Beschrijving                                                  | Vereist |
| ------------- | ------------------------------------------------------------ | -------- |
| type          | De eigenschap type van de bron van de Kopieer activiteit moet zijn ingesteld op **OrcSink**. | Ja      |
| storeSettings | Een groep eigenschappen voor het schrijven van gegevens naar een gegevens archief. Elke connector op basis van bestanden heeft eigen ondersteunde schrijf instellingen onder `storeSettings`. **Zie de sectie Details in connector artikel-> eigenschappen van de Kopieer activiteit**. | Nee       |

## <a name="using-self-hosted-integration-runtime"></a>Zelf-hostende Integration Runtime gebruiken

> [!IMPORTANT]
> Als u de kopie wilt voorzien van zelf-hostende Integration Runtime bijvoorbeeld tussen on-premises en gegevens opslag in de Cloud, moet u het pakket **64-bits jre 8 (Java Runtime Environment) of openjdk** en **micro soft Visual C++ 2010 Redistributable** installeren op uw **IR-computer**. Raadpleeg de volgende alinea met meer informatie.

Voor een kopie die wordt uitgevoerd op een zelf-hostende IR met ORC-serialisatie/deserialisatie, zoekt ADF de Java-runtime *`(SOFTWARE\JavaSoft\Java Runtime Environment\{Current Version}\JavaHome)`* door het REGI ster voor jre te controleren, indien niet *`JAVA_HOME`* gevonden, ter controle van de systeem variabele voor openjdk.

- **Jre gebruiken**: de 64-bits IR vereist een 64-bits jre. U kunt deze [hier](https://go.microsoft.com/fwlink/?LinkId=808605)vinden.
- **Als u openjdk wilt gebruiken**, wordt dit ondersteund sinds IR-versie 3,13. Verdeel het JVM. dll-bestand met alle andere vereiste assembly's van OpenJDK in een zelf-hostende IR-computer en stel de systeem omgevings variabele in JAVA_HOME dienovereenkomstig in.
- **Het herdistribueerbare pakket Visual c++ 2010 installeren: het**herdistribueerbare pakket visual c++ 2010 wordt niet geïnstalleerd met de zelf-HOSTende IR-installaties. U kunt deze [hier](https://www.microsoft.com/download/details.aspx?id=14632)vinden.

> [!TIP]
> Als u gegevens kopieert naar/van ORC-indeling met behulp van zelf-hostende Integration Runtime en de fout melding ' er is een fout opgetreden bij het aanroepen van Java, bericht: **Java. lang. OutOfMemoryError: Java-heap-ruimte**', kunt u een omgevings variabele `_JAVA_OPTIONS` toevoegen aan de computer die als host fungeert voor de zelf-hostende IR om een dergelijke kopie te maken,

![JVM-Heap-grootte instellen op zelf-hostende IR](./media/supported-file-formats-and-compression-codecs/set-jvm-heap-size-on-selfhosted-ir.png)

Voor beeld: Stel `_JAVA_OPTIONS` variabele met `-Xms256m -Xmx16g`waarde in. Met de `Xms` vlag wordt de eerste geheugen toewijzings groep opgegeven voor een Java Virtual Machine (JVM `Xmx` ), terwijl de maximale geheugen toewijzings groep wordt opgegeven. Dit betekent dat JVM wordt gestart met `Xms` de hoeveelheid geheugen en dat er een maximum `Xmx` hoeveelheid geheugen kan worden gebruikt. ADF gebruikt standaard min 64 MB en Max 1G.

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van de Kopieer activiteit](copy-activity-overview.md)
- [Opzoek activiteit](control-flow-lookup-activity.md)
- [GetMetadata-activiteit](control-flow-get-metadata-activity.md)
