---
title: Gegevens kopiëren en transformeren in Azure Data Lake Storage Gen2
description: Informatie over het kopiëren van gegevens van en naar Azure Data Lake Storage Gen2 en het transformeren van gegevens in Azure Data Lake Storage Gen2 met behulp van Azure Data Factory.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/25/2020
ms.openlocfilehash: 0689a7705e91b8fcaf9ca31887e6cefbf90f1e59
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2020
ms.locfileid: "84021004"
---
# <a name="copy-and-transform-data-in-azure-data-lake-storage-gen2-using-azure-data-factory"></a>Gegevens in Azure Data Lake Storage Gen2 kopiëren en transformeren met behulp van Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Data Lake Storage Gen2 (ADLS Gen2) is een set mogelijkheden die is toegewezen aan big data Analytics in [Azure Blob-opslag](../storage/blobs/storage-blobs-introduction.md). U kunt het gebruiken om de interface met uw gegevens te maken met behulp van zowel bestands systeem-als object opslag-modellen.

In dit artikel wordt beschreven hoe u de Kopieer activiteit in Azure Data Factory kunt gebruiken om gegevens te kopiëren van en naar Azure Data Lake Storage Gen2, en om gegevens in Azure Data Lake Storage Gen2 te transformeren met behulp van gegevens stromen. Lees het [artikel Inleiding](introduction.md)voor meer informatie over Azure Data Factory.

>[!TIP]
>Voor het migratie scenario van data Lake of Data Warehouse leert u meer over [het gebruik van Azure Data Factory om gegevens van uw data Lake of Data Warehouse te migreren naar Azure](data-migration-guidance-overview.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

Deze Azure Data Lake Storage Gen2-connector wordt ondersteund voor de volgende activiteiten:

- [Kopieer activiteit](copy-activity-overview.md) met een [ondersteunde bron/Sink-matrix](copy-activity-overview.md)
- [Gegevens stroom toewijzen](concepts-data-flow-overview.md)
- [Opzoek activiteit](control-flow-lookup-activity.md)
- [GetMetadata-activiteit](control-flow-get-metadata-activity.md)
- [Activiteit verwijderen](delete-activity.md)

Voor kopieer activiteit kunt u met deze connector het volgende doen:

- Gegevens kopiëren van/naar Azure Data Lake Storage Gen2 met behulp van account sleutel, Service-Principal of beheerde identiteiten voor Azure-bronnen verificaties.
- Bestanden kopiëren als of parseren of bestanden genereren met [ondersteunde bestands indelingen en compressie-codecs](supported-file-formats-and-compression-codecs.md).
- [Meta gegevens van bestand behouden tijdens kopiëren](#preserve-metadata-during-copy).
- [Acl's behouden](#preserve-acls) bij het kopiëren van Azure data Lake Storage gen1/Gen2.

>[!IMPORTANT]
>Als u de optie **vertrouwde micro soft-Services toegang geven tot dit opslag account hebt** ingeschakeld voor Azure Storage Firewall-instellingen en Azure Integration runtime wilt gebruiken om verbinding te maken met uw data Lake Storage Gen2, moet u [beheerde identiteits verificatie](#managed-identity) voor ADLS Gen2 gebruiken.


## <a name="get-started"></a>Aan de slag

>[!TIP]
>Zie [gegevens laden in azure data Lake Storage Gen2](load-azure-data-lake-storage-gen2.md)voor een overzicht van het gebruik van de data Lake Storage Gen2-connector.

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

De volgende secties bevatten informatie over eigenschappen die worden gebruikt voor het definiëren van Data Factory entiteiten die specifiek zijn voor Data Lake Storage Gen2.

## <a name="linked-service-properties"></a>Eigenschappen van gekoppelde service

De Azure Data Lake Storage Gen2-connector ondersteunt de volgende verificatie typen. Zie de betreffende secties voor meer informatie:

- [Verificatie van account sleutel](#account-key-authentication)
- [Verificatie van service-principal](#service-principal-authentication)
- [Beheerde identiteiten voor Azure-bronnen verificatie](#managed-identity)

>[!NOTE]
>Wanneer u poly base gebruikt voor het laden van gegevens in SQL Data Warehouse, als uw bron Data Lake Storage Gen2 is geconfigureerd met Virtual Network-eind punt, moet u beheerde identiteits verificatie gebruiken zoals vereist is door poly base. Zie de sectie [beheerde identiteits verificatie](#managed-identity) met meer configuratie vereisten.

### <a name="account-key-authentication"></a>Verificatie van account sleutel

De volgende eigenschappen worden ondersteund voor het gebruik van verificatie van de opslag account sleutel:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureBlobFS**. |Ja |
| URL | Eind punt voor Data Lake Storage Gen2 met het patroon van `https://<accountname>.dfs.core.windows.net` . | Ja |
| accountKey | Account sleutel voor Data Lake Storage Gen2. Markeer dit veld als SecureString om het veilig op te slaan in Data Factory, of om te [verwijzen naar een geheim dat is opgeslagen in azure Key Vault](store-credentials-in-key-vault.md). |Ja |
| connectVia | De [Integration runtime](concepts-integration-runtime.md) die moet worden gebruikt om verbinding te maken met het gegevens archief. U kunt de Azure Integration runtime of een zelf-hostende Integration runtime gebruiken als uw gegevens archief zich in een particulier netwerk bevindt. Als deze eigenschap niet is opgegeven, wordt de standaard Azure Integration runtime gebruikt. |Nee |

>[!NOTE]
>Het ADLS-eind punt van het secundaire bestands systeem wordt niet ondersteund bij het gebruik van account sleutel verificatie. U kunt andere verificatie typen gebruiken.

**Voorbeeld:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
            "accountkey": { 
                "type": "SecureString", 
                "value": "<accountkey>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="service-principal-authentication"></a>Verificatie van service-principal

Voer de volgende stappen uit om Service-Principal-verificatie te gebruiken.

1. Registreer een toepassings entiteit in Azure Active Directory (Azure AD) door de stappen te volgen in [uw toepassing registreren bij een Azure AD-Tenant](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Noteer de volgende waarden, die u gebruikt om de gekoppelde service te definiëren:

    - Toepassings-id
    - Toepassings sleutel
    - Tenant-id

2. Verleen de service-principal de juiste machtigingen. Voor beelden bekijken van de werking van machtigingen in Data Lake Storage Gen2 van [toegangs beheer lijsten voor bestanden en mappen](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories)

    - **Als bron**: ken in Storage Explorer ten minste de machtiging voor **uitvoeren** toe voor alle upstream-mappen en het bestands systeem, samen met de machtiging **lezen** voor de bestanden die moeten worden gekopieerd. U kunt ook in toegangs beheer (IAM) ten minste de rol **Storage BLOB data Reader** verlenen.
    - **Als Sink**: wijs in Storage Explorer ten minste **uitvoerings** machtiging toe voor alle upstream-mappen en het bestands systeem, samen met **Schrijf** machtiging voor de map sink. U kunt ook, in toegangs beheer (IAM), ten minste de rol van **BLOB voor gegevens opslag** verlenen.

>[!NOTE]
>Als u Data Factory gebruikers interface wilt ontwerpen en de Service-Principal niet is ingesteld met de rol ' Storage BLOB data Reader/Inzender ' in IAM, kiest u bij het testen van de verbinding of bladeren/navigeren in mappen de optie ' verbinding testen met bestandspad ' of ' bladeren vanuit opgegeven pad ' en geeft u een pad met de machtiging **lezen en uitvoeren** op om door te gaan.

Deze eigenschappen worden ondersteund voor de gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureBlobFS**. |Ja |
| URL | Eind punt voor Data Lake Storage Gen2 met het patroon van `https://<accountname>.dfs.core.windows.net` . | Ja |
| servicePrincipalId | Geef de client-ID van de toepassing op. | Ja |
| servicePrincipalKey | Geef de sleutel van de toepassing op. Markeer dit veld als een `SecureString` om het veilig op te slaan in Data Factory. U kunt ook [verwijzen naar een geheim dat is opgeslagen in azure Key Vault](store-credentials-in-key-vault.md). | Ja |
| tenant | Geef de Tenant gegevens op (domein naam of Tenant-ID) waaronder uw toepassing zich bevindt. U kunt deze ophalen door de muis in de rechter bovenhoek van de Azure Portal aan te wijzen. | Ja |
| connectVia | De [Integration runtime](concepts-integration-runtime.md) die moet worden gebruikt om verbinding te maken met het gegevens archief. U kunt de Azure Integration runtime of een zelf-hostende Integration runtime gebruiken als uw gegevens archief zich in een particulier netwerk bevindt. Als dat niet is opgegeven, wordt de standaard Azure Integration runtime gebruikt. |Nee |

**Voorbeeld:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>" 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identities-for-azure-resources-authentication"></a><a name="managed-identity"></a>Beheerde identiteiten voor Azure-bronnen verificatie

Een data factory kan worden gekoppeld aan een [beheerde identiteit voor Azure-resources](data-factory-service-identity.md)die deze specifieke Data Factory vertegenwoordigt. U kunt deze beheerde identiteit rechtstreeks gebruiken voor Data Lake Storage Gen2 verificatie, vergelijkbaar met het gebruik van uw eigen service-principal. Hiermee kan deze toegewezen Factory gegevens naar of van uw Data Lake Storage Gen2 benaderen en kopiëren.

Voer de volgende stappen uit om beheerde identiteiten voor Azure-resource verificatie te gebruiken.

1. [Haal de Data Factory beheerde identiteits gegevens](data-factory-service-identity.md#retrieve-managed-identity) op door de waarde van de **beheerde ID-object-id** die samen met uw fabriek is gegenereerd, te kopiëren.

2. Verleen de beheerde identiteit de juiste machtiging. Bekijk voor beelden van de werking van machtigingen in Data Lake Storage Gen2 van [toegangs beheer lijsten voor bestanden en mappen](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories).

    - **Als bron**: ken in Storage Explorer ten minste de machtiging voor **uitvoeren** toe voor alle upstream-mappen en het bestands systeem, samen met de machtiging **lezen** voor de bestanden die moeten worden gekopieerd. U kunt ook in toegangs beheer (IAM) ten minste de rol **Storage BLOB data Reader** verlenen.
    - **Als Sink**: wijs in Storage Explorer ten minste **uitvoerings** machtiging toe voor alle upstream-mappen en het bestands systeem, samen met **Schrijf** machtiging voor de map sink. U kunt ook, in toegangs beheer (IAM), ten minste de rol van **BLOB voor gegevens opslag** verlenen.

>[!NOTE]
>Als u Data Factory gebruikers interface wilt ontwerpen en de beheerde identiteit niet is ingesteld met de rol ' Storage BLOB data Reader/Inzender ' in IAM, kiest u bij het testen van verbinding of bladeren/navigeren in mappen de optie ' verbinding testen met bestandspad ' of ' bladeren vanuit opgegeven pad ' en geeft u een pad met de machtiging **lezen en uitvoeren** op om door te gaan.

>[!IMPORTANT]
>Als u poly base gebruikt voor het laden van gegevens van Data Lake Storage Gen2 naar SQL Data Warehouse, moet u, wanneer u beheerde identiteits verificatie voor Data Lake Storage Gen2 gebruikt, ervoor zorgen dat u de stappen 1 en 2 in [deze](../azure-sql/database/vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage) hand leiding kunt registreren voor 1. Als u uw met Azure Active Directory (Azure AD) en 2) registreert, wijst u de rol van BLOB data Inzender voor opslag toe aan uw server. de rest wordt afgehandeld door Data Factory. Als uw Data Lake Storage Gen2 is geconfigureerd met een Azure Virtual Network-eind punt, moet u de beheerde identiteits verificatie gebruiken zoals vereist is door poly Base om poly Base te gebruiken voor het laden van gegevens.

Deze eigenschappen worden ondersteund voor de gekoppelde service:

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type moet worden ingesteld op **AzureBlobFS**. |Ja |
| URL | Eind punt voor Data Lake Storage Gen2 met het patroon van `https://<accountname>.dfs.core.windows.net` . | Ja |
| connectVia | De [Integration runtime](concepts-integration-runtime.md) die moet worden gebruikt om verbinding te maken met het gegevens archief. U kunt de Azure Integration runtime of een zelf-hostende Integration runtime gebruiken als uw gegevens archief zich in een particulier netwerk bevindt. Als dat niet is opgegeven, wordt de standaard Azure Integration runtime gebruikt. |Nee |

**Voorbeeld:**

```json
{
    "name": "AzureDataLakeStorageGen2LinkedService",
    "properties": {
        "type": "AzureBlobFS",
        "typeProperties": {
            "url": "https://<accountname>.dfs.core.windows.net", 
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Eigenschappen van gegevensset

Zie [gegevens sets](concepts-datasets-linked-services.md)voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevens sets.

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

De volgende eigenschappen worden ondersteund voor Data Lake Storage Gen2 onder `location` instellingen in de gegevensset op basis van een indeling:

| Eigenschap   | Beschrijving                                                  | Vereist |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | De eigenschap type onder `location` in de gegevensset moet worden ingesteld op **AzureBlobFSLocation**. | Ja      |
| System | De naam van het Data Lake Storage Gen2-bestands systeem.                              | Nee       |
| folderPath | Het pad naar een map onder het opgegeven bestands systeem. Als u een Joker teken wilt gebruiken om mappen te filteren, slaat u deze instelling over en geeft u deze op in de activiteiten bron instellingen. | Nee       |
| fileName   | De bestands naam onder het opgegeven bestands systeem en folderPath. Als u een Joker teken wilt gebruiken om bestanden te filteren, slaat u deze instelling over en geeft u deze op in de activiteiten bron instellingen. | Nee       |

**Voorbeeld:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<Data Lake Storage Gen2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureBlobFSLocation",
                "fileSystem": "filesystemname",
                "folderPath": "folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschappen van de kopieeractiviteit

Zie [configuraties van activiteiten](copy-activity-overview.md#configuration) en [pijp lijnen en activiteiten](concepts-pipelines-activities.md)kopiëren voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. In deze sectie vindt u een lijst met eigenschappen die worden ondersteund door de Data Lake Storage Gen2 bron en Sink.

### <a name="azure-data-lake-storage-gen2-as-a-source-type"></a>Azure Data Lake Storage Gen2 als bron type

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

U hebt verschillende opties om gegevens te kopiëren van ADLS Gen2:

- Kopiëren vanuit het opgegeven pad dat is opgegeven in de gegevensset.
- Joker filter voor het mappad of de bestands naam, Zie `wildcardFolderPath` en `wildcardFileName` .
- De bestanden die in een opgegeven tekst bestand zijn gedefinieerd, kopiëren als bestanden instellen, Zie `fileListPath` .

De volgende eigenschappen worden ondersteund voor Data Lake Storage Gen2 onder `storeSettings` instellingen in een op indeling gebaseerde kopie bron:

| Eigenschap                 | Beschrijving                                                  | Vereist                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **AzureBlobFSReadSettings**. | Ja                                           |
| ***Zoek de bestanden die moeten worden gekopieerd:*** |  |  |
| OPTIE 1: statisch pad<br> | Kopiëren van het opgegeven bestands systeem of de map of het pad naar het bestand dat is opgegeven in de gegevensset. Als u alle bestanden uit een bestands systeem of map wilt kopiëren, moet u ook opgeven `wildcardFileName` als `*` . |  |
| OPTIE 2: Joker teken<br>- wildcardFolderPath | Het mappad met Joker tekens onder het opgegeven bestands systeem dat is geconfigureerd in de gegevensset om bron mappen te filteren. <br>Toegestane joker tekens zijn: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); gebruik `^` om te escapen als uw werkelijke mapnaam Joker teken of escape-teken bevat. <br>Bekijk meer voor beelden in [map-en bestands filter voorbeelden](#folder-and-file-filter-examples). | Nee                                            |
| OPTIE 2: Joker teken<br>- wildcardFileName | De naam van het bestand met Joker tekens onder het opgegeven bestands systeem + folderPath/wildcardFolderPath voor het filteren van bron bestanden. <br>Toegestane joker tekens zijn: `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken); gebruik `^` om te escapen als uw werkelijke mapnaam Joker teken of escape-teken bevat.  Bekijk meer voor beelden in [map-en bestands filter voorbeelden](#folder-and-file-filter-examples). | Ja |
| OPTIE 3: een lijst met bestanden<br>- fileListPath | Hiermee wordt aangegeven dat een opgegeven bestandenset moet worden gekopieerd. Wijs naar een tekst bestand met een lijst met bestanden die u wilt kopiëren, één bestand per regel dat het relatieve pad is naar het pad dat is geconfigureerd in de gegevensset.<br/>Wanneer u deze optie gebruikt, geeft u geen bestands naam op in DataSet. Meer voor beelden vindt u in [voor beelden van bestands lijsten](#file-list-examples). |Nee |
| ***Aanvullende instellingen:*** |  | |
| recursieve | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen vanuit de opgegeven map. Houd er rekening mee dat wanneer recursief is ingesteld op True en de Sink een archief op basis van bestanden is, een lege map of submap niet wordt gekopieerd of gemaakt bij de sink. <br>Toegestane waarden zijn **True** (standaard) en **Onwaar**.<br>Deze eigenschap is niet van toepassing wanneer u configureert `fileListPath` . |Nee |
| modifiedDatetimeStart    | Bestanden filteren op basis van het kenmerk: laatst gewijzigd. <br>De bestanden worden geselecteerd als het tijdstip van de laatste wijziging binnen het tijds bereik ligt tussen `modifiedDatetimeStart` en `modifiedDatetimeEnd` . De tijd wordt toegepast op UTC-tijd zone in de notatie "2018-12-01T05:00:00Z". <br> De eigenschappen kunnen NULL zijn, wat betekent dat er geen filter voor bestands kenmerken wordt toegepast op de gegevensset.  Wanneer `modifiedDatetimeStart` heeft datetime-waarde `modifiedDatetimeEnd` , maar is null, betekent dit dat de bestanden waarvan het kenmerk laatst gewijzigd is groter dan of gelijk is aan de datum/tijd-waarde wordt geselecteerd.  Wanneer `modifiedDatetimeEnd` heeft datetime-waarde `modifiedDatetimeStart` , maar is null, betekent dit dat de bestanden waarvan het kenmerk laatst gewijzigd is kleiner is dan de datum/tijd-waarde wordt geselecteerd.<br/>Deze eigenschap is niet van toepassing wanneer u configureert `fileListPath` . | Nee                                            |
| modifiedDatetimeEnd      | Hetzelfde als hierboven.                                               | Nee                                            |
| maxConcurrentConnections | Het aantal verbindingen dat gelijktijdig met een opslag archief moet worden verbonden. Geef alleen op wanneer u de gelijktijdige verbinding met het gegevens archief wilt beperken. | Nee                                            |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSettings",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "AzureBlobFSReadSettings",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-data-lake-storage-gen2-as-a-sink-type"></a>Azure Data Lake Storage Gen2 als Sink-type

[!INCLUDE [data-factory-v2-file-formats](../../includes/data-factory-v2-file-formats.md)] 

De volgende eigenschappen worden ondersteund voor Data Lake Storage Gen2 onder `storeSettings` instellingen in op indeling gebaseerde kopie-Sink:

| Eigenschap                 | Beschrijving                                                  | Vereist |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | De eigenschap type onder `storeSettings` moet worden ingesteld op **AzureBlobFSWriteSettings**. | Ja      |
| copyBehavior             | Hiermee wordt het Kopieer gedrag gedefinieerd wanneer de bron bestanden van een gegevens archief op basis van een bestand zijn.<br/><br/>Toegestane waarden zijn:<br/><b>-PreserveHierarchy (standaard instelling)</b>: behoudt de bestands hiërarchie in de doelmap. Het relatieve pad van het bron bestand naar de bronmap is identiek aan het relatieve pad van het doel bestand naar de doelmap.<br/><b>-FlattenHierarchy</b>: alle bestanden in de bronmap bevinden zich in het eerste niveau van de doelmap. De doel bestanden hebben automatisch gegenereerde namen. <br/><b>-MergeFiles</b>: alle bestanden van de bronmap worden samengevoegd met één bestand. Als de bestands naam is opgegeven, is de naam van het samengevoegde bestand de opgegeven naam. Anders is het een automatisch gegenereerde bestands naam. | Nee       |
| blockSizeInMB | Geef de blok grootte in MB op die moet worden gebruikt om gegevens naar ADLS Gen2 te schrijven. Meer informatie [over blok-blobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs). <br/>De toegestane waarde ligt **tussen 4 en 100 MB**. <br/>Standaard bepaalt ADF automatisch de blok grootte op basis van het type en de gegevens van het bron archief. Voor niet-binaire kopieën in ADLS Gen2 is de standaard blok grootte 100 MB, zodat deze in Maxi maal 4,95 TB gegevens past. Het is mogelijk niet optimaal wanneer uw gegevens niet groot zijn, met name wanneer u een zelf-hostend Integration Runtime gebruikt met een slecht netwerk als gevolg van een time-out voor de bewerking of het prestatie probleem. U kunt expliciet een blok grootte opgeven, terwijl u er zeker van wilt zijn dat blockSizeInMB * 50.000 groot genoeg is om de gegevens op te slaan, anders mislukt het uitvoeren van de Kopieer activiteit. | Nee |
| maxConcurrentConnections | Het aantal verbindingen dat gelijktijdig verbinding maakt met het gegevens archief. Geef alleen op wanneer u de gelijktijdige verbinding met het gegevens archief wilt beperken. | Nee       |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Parquet output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "ParquetSink",
                "storeSettings":{
                    "type": "AzureBlobFSWriteSettings",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Voor beelden van map-en bestands filter

In deze sectie wordt het resulterende gedrag van het mappad en de bestands naam met Joker teken filters beschreven.

| folderPath | fileName | recursieve | De structuur van de bronmap en het filter resultaat ( **vetgedrukte** bestanden worden opgehaald)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (Leeg, standaard instelling gebruiken) | onjuist | Mapa<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Bestand1. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Bestand2. json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3. CSV<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. CSV<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6. CSV |
| `Folder*` | (Leeg, standaard instelling gebruiken) | waar | Mapa<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Bestand1. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Bestand2. json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4. json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5. CSV**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6. CSV |
| `Folder*` | `*.csv` | onjuist | Mapa<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Bestand1. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2. json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3. CSV<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. CSV<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6. CSV |
| `Folder*` | `*.csv` | waar | Mapa<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Bestand1. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2. json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5. CSV**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6. CSV |

### <a name="file-list-examples"></a>Voor beelden van bestands lijst

In deze sectie wordt het resulterende gedrag van het gebruik van het pad naar een bestands lijst in de bron van de Kopieer activiteit beschreven.

Ervan uitgaande dat u de volgende structuur van de bronmap hebt en de bestanden vet wilt kopiëren:

| Voorbeeld bron structuur                                      | Inhoud in FileListToCopy. txt                             | ADF-configuratie                                            |
| ------------------------------------------------------------ | --------------------------------------------------------- | ------------------------------------------------------------ |
| bestandssysteem<br/>&nbsp;&nbsp;&nbsp;&nbsp;Mapa<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Bestand1. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Bestand2. json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4. json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5. CSV**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Metagegevensarchiefmethode<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;FileListToCopy. txt | Bestand1. CSV<br>Subfolder1/File3. CSV<br>Subfolder1/File5. CSV | **In gegevensset:**<br>-Bestands systeem:`filesystem`<br>-Mappad:`FolderA`<br><br>**Bron van Kopieer activiteit:**<br>-Pad naar bestands lijst:`filesystem/Metadata/FileListToCopy.txt` <br><br>Het pad naar de bestands lijst verwijst naar een tekst bestand in hetzelfde gegevens archief dat een lijst bevat met bestanden die u wilt kopiëren, één bestand per regel met het relatieve pad naar het pad dat is geconfigureerd in de gegevensset. |


### <a name="some-recursive-and-copybehavior-examples"></a>Enkele recursieve en copyBehavior-voor beelden

In deze sectie wordt het resulterende gedrag van de Kopieer bewerking voor verschillende combi Naties van recursieve en copyBehavior waarden beschreven.

| recursieve | copyBehavior | Structuur van bronmap | Resulterende doel |
|:--- |:--- |:--- |:--- |
| waar |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doel-Map1 is gemaakt met dezelfde structuur als de bron:<br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| waar |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doel-Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor File5 |
| waar |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doel-Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;De inhoud van bestand1 + Bestand2 + File3 + File4 + File5 worden samengevoegd met een bestand met een automatisch gegenereerde bestands naam. |
| onjuist |preserveHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doel-Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/><br/>Subfolder1 met File3, File4 en File5 worden niet opgehaald. |
| onjuist |flattenHierarchy | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doel-Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch gegenereerde naam voor Bestand2<br/><br/>Subfolder1 met File3, File4 en File5 worden niet opgehaald. |
| onjuist |mergeFiles | Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Bestand2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | De doel-Map1 is gemaakt met de volgende structuur: <br/><br/>Map1<br/>&nbsp;&nbsp;&nbsp;&nbsp;De inhoud van bestand1 + Bestand2 wordt samengevoegd met een bestand met een automatisch gegenereerde bestands naam. automatisch gegenereerde naam voor bestand1<br/><br/>Subfolder1 met File3, File4 en File5 worden niet opgehaald. |

## <a name="preserve-metadata-during-copy"></a>Meta gegevens bewaren tijdens kopiëren

Wanneer u bestanden kopieert van Amazon S3/Azure Blob/Azure Data Lake Storage Gen2 naar Azure Data Lake Storage Gen2/Azure Blob, kunt u ervoor kiezen om de meta gegevens van het bestand te behouden. Meer informatie over het [bewaren van meta gegevens](copy-activity-preserve-metadata.md#preserve-metadata).

## <a name="preserve-acls-from-data-lake-storage-gen1gen2"></a><a name="preserve-acls"></a>Acl's van Data Lake Storage Gen1/Gen2 behouden

Wanneer u bestanden kopieert van Azure Data Lake Storage Gen1/Gen2 naar Gen2, kunt u ervoor kiezen om de POSIX Access Control Lists (Acl's) samen met de gegevens te behouden. Meer informatie over [het behouden van acl's van data Lake Storage gen1/Gen2 naar Gen2](copy-activity-preserve-metadata.md#preserve-acls).

>[!TIP]
>Als u gegevens van Azure Data Lake Storage Gen1 wilt kopiëren naar Gen2 in het algemeen, raadpleegt u [gegevens kopiëren van Azure data Lake Storage gen1 naar Gen2 met Azure Data Factory](load-azure-data-lake-storage-gen2-from-gen1.md) voor een stapsgewijze en best practices.

## <a name="mapping-data-flow-properties"></a>Eigenschappen van gegevens stroom toewijzen

Wanneer gegevens worden getransformeerd in de toewijzing van gegevens stromen, kunt u bestanden lezen en schrijven van Azure Data Lake Storage Gen2 in JSON-, AVRO-, gescheiden tekst-of Parquet-indeling. Zie [bron transformatie](data-flow-source.md) en [sink-trans formatie](data-flow-sink.md) in de functie gegevens stroom toewijzen voor meer informatie.

### <a name="source-transformation"></a>Bron transformatie

In de bron transformatie kunt u lezen uit een container, map of afzonderlijk bestand in Azure Data Lake Storage Gen2. Op het tabblad **bron opties** kunt u beheren hoe de bestanden worden gelezen. 

![Bron opties](media/data-flow/sourceOptions1.png "Bron opties")

**Pad met Joker tekens:** Als u een Joker teken gebruikt, wordt ADF geadviseerd om elke overeenkomende map en elk bestand in één bron transformatie te verwerken. Dit is een efficiënte manier om meerdere bestanden binnen één stroom te verwerken. Voeg meerdere Joker teken patronen toe met het plus teken dat wordt weer gegeven bij het aanwijzen van het bestaande Joker teken patroon.

Kies in uw bron container een reeks bestanden die overeenkomen met een patroon. Alleen container kan worden opgegeven in de gegevensset. Het pad naar uw Joker teken moet daarom ook uw mappad van de hoofdmap bevatten.

Voor beelden van joker tekens:

* ```*```Vertegenwoordigt een wille keurige set tekens
* ```**```Recursief nesten van mappen
* ```?```Vervangt één teken
* ```[]```Komt overeen met een van de tekens in de vier Kante haken

* ```/data/sales/**/*.csv```Hiermee worden alle CSV-bestanden opgehaald onder/Data/Sales
* ```/data/sales/20??/**/```Hiermee worden alle bestanden in de twintigste eeuw opgehaald
* ```/data/sales/*/*/*.csv```Haalt CSV-bestanden op twee niveaus onder/Data/Sales
* ```/data/sales/2004/*/12/[XY]1?.csv```Haalt alle CSV-bestanden in 2004 december op, beginnend met X of Y, voorafgegaan door een getal van twee cijfers

Basispad **:** Als u mappen in de bestands bron hebt gepartitioneerd met een ```key=value``` indeling (bijvoorbeeld Year = 2019), kunt u het hoogste niveau van die partitie mappen structuur toewijzen aan een kolom naam in de gegevensstroom gegevens stroom.

Stel eerst een Joker teken in om alle paden op te nemen van de gepartitioneerde mappen plus de blad bestanden die u wilt lezen.

![Bron Bestands instellingen voor partitioneren](media/data-flow/partfile2.png "Instelling voor partitie bestand")

Gebruik de instelling basis pad partitie om te definiëren wat het hoogste niveau van de mappen structuur is. Wanneer u de inhoud van uw gegevens bekijkt via een voor beeld van de gegevens, ziet u dat de opgeloste partities in elk van uw mapniveau worden toegevoegd.

![Basispad voor partitie](media/data-flow/partfile1.png "Voor beeld van basispad")

**Lijst met bestanden:** Dit is een bestandenset. Maak een tekst bestand met een lijst met relatieve padgegevens die moeten worden verwerkt. Wijs dit tekst bestand aan.

**Kolom voor het opslaan van de bestands naam:** Sla de naam van het bron bestand op in een kolom in uw gegevens. Voer hier een nieuwe kolom naam in om de teken reeks voor de bestands naam op te slaan.

**Na voltooiing:** U kunt niets doen met het bron bestand nadat de gegevens stroom is uitgevoerd, het bron bestand te verwijderen of het bron bestand te verplaatsen. De paden voor de verplaatsing zijn relatief.

Als u bron bestanden naar een andere locatie wilt verplaatsen, selecteert u eerst verplaatsen voor bestands bewerking. Stel vervolgens de map uit. Als u geen joker tekens gebruikt voor uw pad, is de instelling van ' van ' dezelfde map als de bronmap.

Als u een bronpad met Joker teken hebt, ziet uw syntaxis er als volgt uit:

```/data/sales/20??/**/*.csv```

U kunt "van" opgeven als

```/data/sales```

En "aan" als

```/backup/priorSales```

In dit geval worden alle bestanden die zijn gebrond onder/Data/Sales verplaatst naar/backup/priorSales.

> [!NOTE]
> Bestands bewerkingen worden alleen uitgevoerd wanneer u de gegevens stroom start vanuit een pijplijn uitvoering (een uitvoering van een pijp lijn of uitvoering) die gebruikmaakt van de activiteit gegevens stroom uitvoeren in een pijp lijn. Bestands bewerkingen *worden niet* uitgevoerd in de modus voor fout opsporing van gegevens stromen.

**Filteren op laatst gewijzigd:** U kunt filteren op de bestanden die u verwerkt door een datum bereik op te geven wanneer ze voor het laatst zijn gewijzigd. Alle datum-tijden zijn in UTC. 

### <a name="sink-properties"></a>Eigenschappen van Sink

In de Sink-trans formatie kunt u schrijven naar een container of map in Azure Data Lake Storage Gen2. op het tabblad **instellingen** kunt u beheren hoe de bestanden worden geschreven.

![Sink-opties](media/data-flow/file-sink-settings.png "Sink-opties")

**De map wissen:** Hiermee wordt bepaald of de doelmap wordt gewist voordat de gegevens worden geschreven.

**Optie Bestands naam:** Hiermee wordt bepaald hoe de doel bestanden in de doelmap worden genoemd. De opties voor de bestands naam zijn:
   * **Standaard**: Spark toestaan om bestanden een naam te geven op basis van de standaard waarden van een deel.
   * **Patroon**: Voer een patroon in voor het inventariseren van de uitvoer bestanden per partitie. Bijvoorbeeld: **leningen [n]. CSV** maken loans1. CSV, loans2. CSV, enzovoort.
   * **Per partitie**: Voer één bestands naam per partitie in.
   * **Als gegevens in kolom**: Stel het uitvoer bestand in op de waarde van een kolom. Het pad is relatief ten opzichte van de container gegevensset, niet de doelmap. Als u een pad naar een map in uw gegevensset hebt, wordt deze overschreven.
   * **Uitvoer naar één bestand**: de gepartitioneerde uitvoer bestanden combi neren in één bestand met een naam. Het pad is relatief ten opzichte van de map DataSet. Houd er rekening mee dat de samenvoegings bewerking mogelijk kan mislukken op basis van de knooppunt grootte. Deze optie wordt niet aanbevolen voor grote gegevens sets.

**Alle aanhalings tekens:** Hiermee wordt bepaald of alle waarden tussen aanhalings tekens moeten worden inge sloten

## <a name="lookup-activity-properties"></a>Eigenschappen van opzoek activiteit

Controleer de [opzoek activiteit](control-flow-lookup-activity.md)voor meer informatie over de eigenschappen.

## <a name="getmetadata-activity-properties"></a>Eigenschappen van GetMetadata-activiteit

Als u meer wilt weten over de eigenschappen, controleert u de [GetMetadata-activiteit](control-flow-get-metadata-activity.md) 

## <a name="delete-activity-properties"></a>Eigenschappen van activiteit verwijderen

Als u meer wilt weten over de eigenschappen, controleert u de [activiteit verwijderen](delete-activity.md)

## <a name="legacy-models"></a>Verouderde modellen

>[!NOTE]
>De volgende modellen worden nog steeds ondersteund voor compatibiliteit met eerdere versies. U wordt aangeraden het nieuwe model te gebruiken dat hierboven wordt beschreven en de gebruikers interface van de ADF-ontwerp functie is overgeschakeld op het genereren van het nieuwe model.

### <a name="legacy-dataset-model"></a>Verouderd gegevensset-model

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de DataSet moet worden ingesteld op **AzureBlobFSFile**. |Ja |
| folderPath | Pad naar de map in Data Lake Storage Gen2. Als deze niet wordt opgegeven, verwijst deze naar de hoofdmap. <br/><br/>Het Joker teken filter wordt ondersteund. Toegestane joker tekens zijn `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken). Gebruik `^` om te escapen als uw werkelijke mapnaam een Joker teken bevat of dit escape-teken in. <br/><br/>Voor beelden: bestands systeem/map/. Bekijk meer voor beelden in [map-en bestands filter voorbeelden](#folder-and-file-filter-examples). |Nee |
| fileName | De naam of het Joker teken filter voor de bestanden onder het opgegeven folderPath. Als u geen waarde opgeeft voor deze eigenschap, wijst de gegevensset naar alle bestanden in de map. <br/><br/>Voor het filter zijn de toegestane joker tekens `*` (komt overeen met nul of meer tekens) en `?` (komt overeen met nul of één teken).<br/>-Voor beeld 1:`"fileName": "*.csv"`<br/>-Voor beeld 2:`"fileName": "???20180427.txt"`<br/>Gebruik `^` om te escapen als uw werkelijke bestands naam een Joker teken bevat of dit escape-teken in.<br/><br/>Als er geen bestands naam is opgegeven voor een uitvoer-gegevensset en **preserveHierarchy** niet is opgegeven in de Sink van de activiteit, genereert de Kopieer activiteit automatisch de bestands naam met het volgende patroon: "*gegevens. [ Run ID-GUID van activiteit]. [GUID if FlattenHierarchy]. [indeling indien geconfigureerd]. [compressie indien geconfigureerd]*', bijvoorbeeld ' data. 0a405f8a-93ff-4c6f-b3be-f69616f1df7a. txt. gz '. Als u vanuit een tabellaire bron kopieert met behulp van een tabel naam in plaats van een query, is het naam patroon '*[tabel naam]. [ indeling]. [compressie indien geconfigureerd]*', bijvoorbeeld ' mytable. csv '. |Nee |
| modifiedDatetimeStart | Bestanden filteren op basis van het kenmerk dat het laatst is gewijzigd. De bestanden worden geselecteerd als het tijdstip van de laatste wijziging binnen het tijds bereik ligt tussen `modifiedDatetimeStart` en `modifiedDatetimeEnd` . De tijd wordt toegepast op de UTC-tijd zone in de notatie "2018-12-01T05:00:00Z". <br/><br/> De algehele prestaties van het verplaatsen van gegevens worden beïnvloed door deze instelling in te scha kelen wanneer u bestands filter wilt uitvoeren met enorme hoeveel heden bestanden. <br/><br/> De eigenschappen kunnen NULL zijn, wat betekent dat er geen bestands kenmerk filter op de gegevensset wordt toegepast. Wanneer `modifiedDatetimeStart` heeft een datum/tijd `modifiedDatetimeEnd` -waarde, maar null is, betekent dit dat de bestanden waarvan het kenmerk laatst gewijzigd is groter is dan of gelijk is aan de datum/tijd-waarde zijn geselecteerd. Wanneer `modifiedDatetimeEnd` heeft een datum/tijd `modifiedDatetimeStart` -waarde, maar is null, betekent dit dat de bestanden waarvan het kenmerk laatst gewijzigd is, kleiner zijn dan de waarde voor datum/tijd.| Nee |
| modifiedDatetimeEnd | Bestanden filteren op basis van het kenmerk dat het laatst is gewijzigd. De bestanden worden geselecteerd als het tijdstip van de laatste wijziging binnen het tijds bereik ligt tussen `modifiedDatetimeStart` en `modifiedDatetimeEnd` . De tijd wordt toegepast op de UTC-tijd zone in de notatie "2018-12-01T05:00:00Z". <br/><br/> De algehele prestaties van het verplaatsen van gegevens worden beïnvloed door deze instelling in te scha kelen wanneer u bestands filter wilt uitvoeren met enorme hoeveel heden bestanden. <br/><br/> De eigenschappen kunnen NULL zijn, wat betekent dat er geen bestands kenmerk filter op de gegevensset wordt toegepast. Wanneer `modifiedDatetimeStart` heeft een datum/tijd `modifiedDatetimeEnd` -waarde, maar null is, betekent dit dat de bestanden waarvan het kenmerk laatst gewijzigd is groter is dan of gelijk is aan de datum/tijd-waarde zijn geselecteerd. Wanneer `modifiedDatetimeEnd` heeft een datum/tijd `modifiedDatetimeStart` -waarde, maar is null, betekent dit dat de bestanden waarvan het kenmerk laatst gewijzigd is, kleiner zijn dan de waarde voor datum/tijd.| Nee |
| formaat | Als u bestanden wilt kopiëren als zich bevindt tussen archieven op basis van bestanden (binaire kopie), slaat u de sectie opmaak in zowel de definitie van de invoer-als uitvoer gegevensset over.<br/><br/>Als u bestanden wilt parseren of genereren met een specifieke indeling, worden de volgende typen bestands indelingen ondersteund: **TextFormat**, **JsonFormat**, **Avro Format**, **OrcFormat**en **ParquetFormat**. Stel de eigenschap **type** onder **indeling** in op een van deze waarden. Zie de secties [tekst indeling](supported-file-formats-and-compression-codecs-legacy.md#text-format), [JSON-indeling](supported-file-formats-and-compression-codecs-legacy.md#json-format), [Avro](supported-file-formats-and-compression-codecs-legacy.md#avro-format)-indeling, [Orc-indeling](supported-file-formats-and-compression-codecs-legacy.md#orc-format)en [Parquet-indeling](supported-file-formats-and-compression-codecs-legacy.md#parquet-format) voor meer informatie. |Nee (alleen voor het scenario binair kopiëren) |
| compressie | Geef het type en compressie niveau voor de gegevens op. Zie [ondersteunde bestands indelingen en compressie-codecs](supported-file-formats-and-compression-codecs-legacy.md#compression-support)voor meer informatie.<br/>Ondersteunde typen zijn **gzip**, **Deflate**, **bzip2**en **ZipDeflate**.<br/>Ondersteunde niveaus zijn **optimaal** en **snelst**. |Nee |

>[!TIP]
>Als u alle bestanden in een map wilt kopiëren, geeft u alleen **FolderPath** op.<br>Als u één bestand met een bepaalde naam wilt kopiëren, geeft u **FolderPath** op met een mappen onderdeel en **filename** met een bestands naam.<br>Als u een subset van bestanden onder een map wilt kopiëren, geeft u **FolderPath** op met een deel van een map en een **Bestands naam** met een Joker teken filter. 

**Voorbeeld:**

```json
{
    "name": "ADLSGen2Dataset",
    "properties": {
        "type": "AzureBlobFSFile",
        "linkedServiceName": {
            "referenceName": "<Azure Data Lake Storage Gen2 linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "myfilesystem/myfolder",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

### <a name="legacy-copy-activity-source-model"></a>Bron model van verouderde Kopieer activiteit

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de bron van de Kopieer activiteit moet zijn ingesteld op **AzureBlobFSSource**. |Ja |
| recursieve | Geeft aan of de gegevens recursief worden gelezen uit de submappen of alleen vanuit de opgegeven map. Als recursief is ingesteld op True en de Sink een archief op basis van bestanden is, wordt een lege map of submap niet gekopieerd of gemaakt bij de sink.<br/>Toegestane waarden zijn **True** (standaard) en **Onwaar**. | Nee |
| maxConcurrentConnections | Het aantal verbindingen dat gelijktijdig verbinding maakt met het gegevens archief. Geef alleen op wanneer u de gelijktijdige verbinding met het gegevens archief wilt beperken. | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ADLS Gen2 input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "AzureBlobFSSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="legacy-copy-activity-sink-model"></a>Sink model voor verouderde Kopieer activiteit

| Eigenschap | Beschrijving | Vereist |
|:--- |:--- |:--- |
| type | De eigenschap type van de Sink voor kopieer activiteiten moet worden ingesteld op **AzureBlobFSSink**. |Ja |
| copyBehavior | Hiermee wordt het Kopieer gedrag gedefinieerd wanneer de bron bestanden van een gegevens archief op basis van een bestand zijn.<br/><br/>Toegestane waarden zijn:<br/><b>-PreserveHierarchy (standaard instelling)</b>: behoudt de bestands hiërarchie in de doelmap. Het relatieve pad van het bron bestand naar de bronmap is identiek aan het relatieve pad van het doel bestand naar de doelmap.<br/><b>-FlattenHierarchy</b>: alle bestanden in de bronmap bevinden zich in het eerste niveau van de doelmap. De doel bestanden hebben automatisch gegenereerde namen. <br/><b>-MergeFiles</b>: alle bestanden van de bronmap worden samengevoegd met één bestand. Als de bestands naam is opgegeven, is de naam van het samengevoegde bestand de opgegeven naam. Anders is het een automatisch gegenereerde bestands naam. | Nee |
| maxConcurrentConnections | Het aantal verbindingen dat gelijktijdig verbinding maakt met het gegevens archief. Geef alleen op wanneer u de gelijktijdige verbinding met het gegevens archief wilt beperken. | Nee |

**Voorbeeld:**

```json
"activities":[
    {
        "name": "CopyToADLSGen2",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ADLS Gen2 output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureBlobFSSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

## <a name="next-steps"></a>Volgende stappen

Zie [ondersteunde gegevens archieven](copy-activity-overview.md#supported-data-stores-and-formats)voor een lijst met gegevens archieven die worden ondersteund als bronnen en sinks op basis van de Kopieer activiteit in Data Factory.
