---
title: OPENROWSET gebruiken in SQL on demand (preview)
description: In dit artikel wordt de syntaxis beschreven van OPENROWSET in SQL on demand (preview) en wordt uitgelegd hoe u argumenten gebruikt.
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 05/07/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 4ec6e18aa4fa741ba784e68ccf9b5f87ad654eba
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/19/2020
ms.locfileid: "83591417"
---
# <a name="how-to-use-openrowset-with-sql-on-demand-preview"></a>OPENROWSET gebruiken met SQL on demand (preview)

Met de functie `OPENROWSET(BULK...)` kunt u toegang krijgen tot bestanden in Azure Storage. `OPENROWSET` leest de inhoud van een externe gegevensbron (bijvoorbeeld een bestand) en retourneert de inhoud als een set rijen. Binnen de resource van SQL on-demand (preview) wordt de bulksgewijze rijensetprovider van OPENROWSET benaderd door het aanroepen van de functie OPENROWSET en het opgeven van de optie BULK.  

Naar de functie `OPENROWSET` kan worden verwezen in de `FROM`-component van een query alsof het een tabelnaam `OPENROWSET` is. De functie ondersteunt bulkbewerkingen via een ingebouwde BULK-provider waarmee gegevens uit een bestand kunnen worden gelezen om deze vervolgens te retourneren als een rijenset.

## <a name="data-source"></a>Gegevensbron

De functie OPENROWSET in Synapse SQL leest de inhoud van een of meer bestanden uit een gegevensbron. De gegevensbron is een Azure-opslagaccount waarnaar expliciet kan worden verwezen in de functie `OPENROWSET` of die kan dynamisch worden afgeleid van de URL van de bestanden die u wilt lezen.
De functie `OPENROWSET` kan optioneel een parameter `DATA_SOURCE` bevatten om de gegevensbron op te geven die bestanden bevat.
- `OPENROWSET` zonder `DATA_SOURCE` kan worden gebruikt om de inhoud van de bestanden rechtstreeks te lezen vanaf de URL-locatie die is opgegeven via de optie `BULK`:

    ```sql
    SELECT *
    FROM OPENROWSET(BULK 'http://storage..../container/folder/*.parquet',
                    TYPE = 'PARQUET') AS file
    ```

Dit is een snelle en eenvoudige manier om de inhoud van de bestanden te lezen zonder voorafgaande configuratie. Met deze optie kunt u de optie voor basisverificatie gebruiken om toegang te krijgen tot de opslag (Azure AD Pass-Through voor Azure AD-aanmeldingen en SAS-token voor SQL-aanmeldingen). 

- `OPENROWSET` met `DATA_SOURCE` kan worden gebruikt om toegang te krijgen tot bestanden in het opgegeven opslagaccount:

    ```sql
    SELECT *
    FROM OPENROWSET(BULK '/folder/*.parquet',
                    DATA_SOURCE='storage', --> Root URL is in LOCATION of DATA SOURCE
                    TYPE = 'PARQUET') AS file
    ```

    Met deze optie kunt u de locatie van het opslagaccount in de gegevensbron configureren en de verificatiemethode opgeven die moet worden gebruikt voor toegang tot de opslag. 
    
    > [!IMPORTANT]
    > `OPENROWSET` zonder `DATA_SOURCE` biedt een snelle en eenvoudige manier om toegang te krijgen tot de opslagbestanden, maar met beperkte verificatieopties. Zo kunnen Azure AD-principals alleen toegang krijgen tot bestanden met behulp van hun [Azure AD-identiteit](develop-storage-files-storage-access-control.md?tabs=user-identity#force-azure-ad-pass-through) en hebben ze geen toegang tot openbaar beschikbare bestanden. Als u krachtigere verificatieopties nodig hebt, gebruikt u `DATA_SOURCE` optie en definieert u de referenties die u wilt gebruiken voor toegang tot de opslag.

## <a name="security"></a>Beveiliging

Een databasegebruiker moet beschikken over de machtiging `ADMINISTER BULK OPERATIONS` om de functie `OPENROWSET` te kunnen gebruiken.

De opslagbeheerder moet een gebruiker ook in staat stellen om toegang te krijgen tot de bestanden door een geldig SAS-token te verstrekken of door de Azure AD-principal toegang te geven tot opslagbestanden. Meer informatie over toegangsbeheer voor opslag vindt u in [dit artikel](develop-storage-files-storage-access-control.md).

`OPENROWSET` gebruikt de volgende regels om te bepalen hoe verificatie voor toegang tot opslag moet worden uitgevoerd:
- Het verificatiemechanisme `OPENROWSET` met `DATA_SOURCE` is afhankelijk van het type aanroeper.
  - AAD-aanmeldingen hebben alleen toegang tot bestanden via hun eigen [Azure AD-identiteit](develop-storage-files-storage-access-control.md?tabs=user-identity#force-azure-ad-pass-through) als Azure Storage de Azure AD-gebruiker toegang geeft tot de onderliggende bestanden (als de aanroeper bijvoorbeeld de machtiging Opslaglezer heeft voor opslag) en als u [verificatie via Azure AD Pass-Through inschakelt](develop-storage-files-storage-access-control.md#force-azure-ad-pass-through) in de Synapse SQL-service.
  - SQL-aanmeldingen kunnen ook `OPENROWSET` gebruiken zonder `DATA_SOURCE` om toegang te krijgen tot openbaar beschikbare bestanden, bestanden die zijn beveiligd met een SAS-token of beheerde identiteit van een Synapse-werkruimte. U moet [referenties binnen serverbereik maken](develop-storage-files-storage-access-control.md#examples) om toegang tot opslagbestanden toe te staan. 
- In `OPENROWSET` met `DATA_SOURCE` wordt het verificatiemechanisme gedefinieerd in de referentie binnen databasebereik die is toegewezen aan de gegevensbron waarnaar wordt verwezen. Met deze methode kunt u toegang krijgen tot openbaar beschikbare opslag, of toegang krijgen tot opslag met behulp van een SAS-token, een beheerde identiteit van de werkruimte of [Azure AD-identiteit van de aanroeper](develop-storage-files-storage-access-control.md?tabs=user-identity#) (als dat een Azure AD-principal is). Als `DATA_SOURCE` verwijst naar Azure-opslag die niet openbaar is, moet u [referenties binnen het databasebereik maken](develop-storage-files-storage-access-control.md#examples) en hiernaar verwijzen in `DATA SOURCE` om toegang tot opslagbestanden te geven.

De aanroeper moet beschikken over de machtiging `REFERENCES` voor de referenties om deze te kunnen gebruiken voor verificatie bij de opslag.

## <a name="syntax"></a>Syntaxis

```syntaxsql
--OPENROWSET syntax for reading Parquet files
OPENROWSET  
( { BULK 'unstructured_data_path' , [DATA_SOURCE = <data source name>, ]
    FORMAT='PARQUET' }  
)  
[WITH ( {'column_name' 'column_type' }) ]
[AS] table_alias(column_alias,...n)

--OPENROWSET syntax for reading delimited text files
OPENROWSET  
( { BULK 'unstructured_data_path' , [DATA_SOURCE = <data source name>, ] 
    FORMAT = 'CSV'
    [ <bulk_options> ] }  
)  
WITH ( {'column_name' 'column_type' [ 'column_ordinal'] })  
[AS] table_alias(column_alias,...n)
 
<bulk_options> ::=  
[ , FIELDTERMINATOR = 'char' ]    
[ , ROWTERMINATOR = 'char' ] 
[ , ESCAPE_CHAR = 'char' ] 
[ , FIRSTROW = 'first_row' ]     
[ , FIELDQUOTE = 'quote_characters' ]
[ , DATA_COMPRESSION = 'data_compression_method' ]
[ , PARSER_VERSION = 'parser_version' ]
```

## <a name="arguments"></a>Argumenten

U hebt twee keuzen voor invoerbestanden die de doelgegevens bevatten waarop query's worden uitgevoerd. Geldige waarden zijn:

- 'CSV': een tekstbestand met scheidingstekens voor rijen en kolommen. Elk teken kan worden gebruikt als veldscheidingsteken, zoals TSV: FIELDTERMINATOR = tab.

- 'PARQUET': binair bestand in Parquet-indeling 

**'unstructured_data_path'**

Het argument unstructured_data_path is een pad naar de gegevens in de vorm van een absolute of relatieve verwijzing:
- Een absoluut pad in de indeling '\<voorvoegsel>://\<pad_naar_opslagaccount>/\<pad_naar_opslag>' biedt een gebruiker de gelegenheid om bestanden rechtstreeks te lezen.
- Een relatief pad in de indeling <pad_naar_opslag> dat moet worden gebruikt met de parameter `DATA_SOURCE` en dat het bestandspatroon beschrijft op de locatie <pad_naar_opslagaccount> locatie die is gedefinieerd in `EXTERNAL DATA SOURCE`. 

 Hieronder vindt u de relevante <storage account path> waarden voor koppeling aan uw specifieke externe gegevensbron. 

| Externe gegevensbron       | Voorvoegsel | Pad van opslagaccount                                 |
| -------------------------- | ------ | ---------------------------------------------------- |
| Azure Blob Storage         | https  | \<opslagaccount>.blob.core.windows.net             |
| Azure Data Lake Store Gen1 | https  | \<opslagaccount>.azuredatalakestore.net/webhdfs/v1 |
| Azure Data Lake Store Gen2 | https  | \<opslagaccount>.dfs.core.windows.net              |
||||

'\<pad_naar_opslag>'

 Een pad binnen de opslag dat verwijst naar de map of het bestand dat u wilt lezen. Als het pad naar een container of map verwijst, worden alle bestanden in die specifieke container of map gelezen. Bestanden in submappen worden uitgesloten. 

 U kunt jokertekens gebruiken om meerdere bestanden of mappen op te geven. Het gebruik van meerdere, niet-opeenvolgende jokertekens is toegestaan.
Hieronder ziet u een voorbeeld waarmee alle *CSV-* bestanden worden gelezen die beginnen met *population* uit alle mappen die beginnen met */csv/population*:  
`https://sqlondemandstorage.blob.core.windows.net/csv/population*/population*.csv`

Als u opgeeft dat unstructured_data_path een map is, haalt een SQL on demand-query bestanden uit die map op. 

> [!NOTE]
> In tegenstelling tot Hadoop en PolyBase, retourneert SQL on-demand geen submappen. Eveneens in tegenstelling tot Hadoop en PolyBase, retourneert SQL on demand ook bestanden waarvan de bestandsnaam begint met een onderstrepingsteken (_) of een punt (.).

Als in het onderstaande voorbeeld unstructured_data_path=`https://mystorageaccount.dfs.core.windows.net/webdata/`, retourneert een SQL on-demand-query rijen uit mydata.txt en _hidden.txt. Er worden geen gegevens geretourneerd uit mydata2.txt en mydata3.txt omdat deze bestanden zich in een submap bevinden.

![Recursieve gegevens voor externe tabellen](./media/develop-openrowset/folder-traversal.png)

`[WITH ( {'column_name' 'column_type' [ 'column_ordinal'] }) ]`

Met de WITH-component kunt u opgeven welke kolommen u uit bestanden wilt lezen.

- Als u wilt dat alle kolommen worden gelezen uit CSV-gegevensbestanden, geeft u de kolomnamen en de bijbehorende gegevenstypen op. Als u een subset van deze kolommen wilt opvragen, gebruikt u rangtelwoorden om de kolommen uit de oorspronkelijke gegevensbestanden te kiezen op rangtelwoord. Kolommen worden gebonden op aanduiding via rangnummer. 

    > [!IMPORTANT]
    > De WITH-component is verplicht voor CSV-bestanden.
    >
    
- Voor Parquet-gegevensbestanden geeft u kolomnamen op die overeenkomen met de kolomnamen in de oorspronkelijke gegevensbestanden. Kolommen worden op naam gebonden. Als de WITH-component wordt weggelaten, worden alle kolommen uit Parquet-bestanden geretourneerd.

column_name = De naam voor de uitvoerkolom. Als u dit argument opgeeft, wordt deze naam gebruikt in plaats van de kolom in het bronbestand.

column_type = Het gegevenstype voor de uitvoerkolom. De impliciete conversie van het gegevenstype wordt hier uitgevoerd.

column_ordinal = Het rangnummer van de kolom in het bronbestand of de bronbestanden. Dit argument wordt genegeerd voor Parquet-bestanden omdat binding op naam wordt uitgevoerd. In het volgende voorbeeld wordt alleen een tweede kolom geretourneerd uit een CSV-bestand:

```sql
WITH (
    --[country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2 2
    --[year] smallint,
    --[population] bigint
)
```

**\<bulk_options>**

FIELDTERMINATOR ='eindteken_veld'

Geeft aan welk teken moet worden gebruikt om het einde van een veld aan te geven. Het standaardeindteken voor velden is een komma (' **,** ').

ROWTERMINATOR ='eindteken_rij'

Geeft aan welk teken moet worden gebruikt om het einde van een rij aan te geven. Als er geen rij-eindteken wordt opgegeven, wordt een van de standaardeindtekens gebruikt. De standaardeindtekens voor PARSER_VERSION = '1.0' zijn \r\n \n en \r. De standaardeindtekens voor PARSER_VERSION = '2.0' zijn \r\n en \n.

ESCAPE_CHAR = 'teken'

Het teken in het bestand dat wordt gebruikt om zichzelf te escapen evenals alle scheidingstekens in het bestand. Als het escape-teken wordt gevolgd door een andere waarde dan het teken zelf, of een van de scheidingstekens, wordt het escape-teken genegeerd bij het lezen van de waarde. 

De parameter ESCAPE_CHAR wordt toegepast ongeacht of FIELDQUOTE al dan niet is ingeschakeld. De parameter wordt niet gebruikt om het aanhalingsteken te escapen. Escapen van het aanhalingsteken gebeurt met dubbele aanhalingstekens, net zoals in CSV-bestanden van Excel.

FIRSTROW = 'eerste_rij' 

Het nummer van de eerste rij die moet worden geladen. De standaardwaarde is 1. Hiermee wordt de eerste rij in het opgegeven gegevensbestand aangegeven. De rijnummers worden bepaald door het tellen van het aantal eindtekens voor rijen. FIRSTROW begint bij 1.

FIELDQUOTE = 'aanhalingsteken_veld' 

Het teken op dat wordt gebruikt als het aanhalingsteken in het CSV-bestand. Als u niets opgeeft, wordt het aanhalingsteken " gebruikt. 

DATA_COMPRESSION = 'methode_voor_gegevenscompressie'

De compressiemethode. De volgende compressiemethode wordt ondersteund:

- org.apache.hadoop.io.compress.GzipCodec

PARSER_VERSION = 'versie_van_parser'

De parser-versie die moet worden gebruikt bij het lezen van bestanden. De momenteel ondersteunde parser-versies voor CSV zijn 1.0 en 2.0

- PARSER_VERSION = '1.0'
- PARSER_VERSION = '2.0'

De standaardparser voor CSV-bestanden is 1.0 en deze versie bevat een groot aantal functies, terwijl 2.0 is gebouwd voor prestaties en niet alle opties en coderingen ondersteunt. 

Kenmerken van parser-versie 2.0 voor CSV:

- Niet alle gegevenstypen worden ondersteund.
- De maximale limiet voor de rijgrootte is 8 MB.
- De volgende opties worden niet ondersteund: DATA_COMPRESSION.
- Een lege tekenreeks tussen aanhalingstekens ("") wordt geïnterpreteerd als een lege tekenreeks.

## <a name="examples"></a>Voorbeelden

In het volgende voorbeeld worden slechts twee kolommen met de rangnummers 1 en 4 geretourneerd uit de bestanden population*.csv. Omdat de bestanden geen veldnamenrij bevatten, begint het lezen bij de eerste regel:

```sql
SELECT * 
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population/population*.csv',
        FORMAT = 'CSV',
        FIRSTROW = 1
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2 1,
    [population] bigint 4
) AS [r]
```

In het volgende voorbeeld worden voor de gegevensset Census alle kolommen uit de eerste rij als resultaat gegeven in de Parquet-indeling zonder kolomnamen en gegevenstypen op te geven: 

```sql
SELECT 
    TOP 1 *
FROM  
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet',
        FORMAT='PARQUET'
    ) AS [r]
```

Als u een fout ziet dat de bestanden niet kunnen worden vermeld, moet u toegang tot openbare opslag in Synapse SQL on-demand inschakelen:
- Als u een SQL-aanmelding gebruikt, moet u [referenties binnen serverbereik maken die toegang bieden tot openbare opslag](develop-storage-files-storage-access-control.md#examples).
- Als u een principal van Azure AD gebruikt voor toegang tot openbare opslag, moet u [referenties binnen serverbereik maken die toegang bieden tot openbare opslag](develop-storage-files-storage-access-control.md#examples) en [verificatie via Azure AD Pass-Through](develop-storage-files-storage-access-control.md#disable-forcing-azure-ad-pass-through) uitschakelen.

## <a name="next-steps"></a>Volgende stappen

Ga naar de quickstart [Query data in storage](query-data-storage.md) (Query uitvoeren op gegevens in opslag) voor voorbeelden van OPENROWSET voor het lezen van de bestandsindelingen [CSV](query-single-csv-file.md), [PARQUET](query-parquet-files.md) en [JSON](query-json-files.md). U kunt ook lezen hoe u de resultaten van een query kunt opslaan in Azure Storage met behulp van [CETAS](develop-tables-cetas.md).
