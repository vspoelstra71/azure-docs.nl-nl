---
title: Een T-SQL streaming-taak in Azure SQL Edge maken (preview)
description: Meer informatie over het maken van Stream Analytics taken in Azure SQL Edge (preview)
keywords: ''
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 7db7f9548a3daa86a53dd37fbe088661e8b7b17e
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2020
ms.locfileid: "83685175"
---
# <a name="create-stream-analytics-job-in-azure-sql-edge-preview"></a>Stream Analytics-taak in Azure SQL Edge maken (preview) 

In dit artikel wordt uitgelegd hoe u een T-SQL streaming-taak maakt in Azure SQL Edge (preview). Als u een streaming-taak wilt maken in SQL-rand, moeten de volgende stappen worden uitgevoerd

1. De externe stream-invoer-en uitvoer objecten maken
2. Definieer de streaming-taak query als onderdeel van de streaming-taak maken.

> [!NOTE]
> Als u de functie voor het streamen van T-SQL in Azure SQL Edge wilt inschakelen, schakelt u TF 11515 in als opstart optie of gebruikt u de [DBCC TRACEON]( https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql) -opdracht. Zie [configure using MSSQL. conf file](configure.md#configure-using-mssqlconf-file)voor meer informatie over het inschakelen van tracerings vlaggen met het bestand MSSQL. conf. Deze vereiste wordt verwijderd in toekomstige updates van Azure SQL Edge (preview).

## <a name="configure-an-external-stream-input-and-output-object"></a>Een invoer-en uitvoer object voor externe streams configureren

T-SQL streaming maakt gebruik van de functionaliteit van de externe gegevens bron van SQL Server om de gegevens bronnen te definiëren die zijn gekoppeld aan de externe stroom-invoer en uitvoer van de streaming-taak. De volgende T-SQL-opdrachten zijn vereist voor het maken van een externe stream-invoer of-uitvoer object.

[CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-external-file-format-transact-sql)

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-external-data-source-transact-sql)

[EXTERNE STREAM maken (Transact-SQL)](#example-create-an-external-stream-object-sql-database)

Daarnaast is in het geval van een SQL-rand (of SQL Server Azure SQL) die als uitvoer stroom wordt gebruikt, de T-SQL-opdracht [Create Data Base scoped CREDENTIAL (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-database-scoped-credential-transact-sql) vereist om de referenties voor toegang tot SQL database te definiëren.

### <a name="supported-input-and-output-stream-data-sources"></a>Ondersteunde gegevens bronnen voor invoer en uitvoer stroom

Azure SQL Edge ondersteunt momenteel alleen de volgende gegevens bronnen als invoer en uitvoer van streams.

| Gegevensbrontype | Invoer | Uitvoer | Beschrijving |
|------------------|-------|--------|------------------|
| Azure IoT Edge hub | J | J | Gegevens bron om streaminggegevens te lezen/schrijven naar een Azure IoT Edge hub. Raadpleeg [IOT Edge hub](https://docs.microsoft.com/azure/iot-edge/iot-edge-runtime#iot-edge-hub) voor meer informatie over Azure IOT Edge hub.|
| SQL Database | N | J | Gegevens bron verbinding om streaminggegevens te schrijven naar SQL Database. Het SQL Database kan een lokale SQL EDGE-Data Base of een externe SQL Server of Azure SQL Database|
| Azure Blob Storage | N | J | Gegevens bron voor het schrijven van gegevens naar een BLOB in een Azure-opslag account. |
| Kafka | J | N | Gegevens bron voor het lezen van streaminggegevens uit een Kafka-onderwerp. Deze adapter is momenteel alleen beschikbaar voor Intel/AMD-versie van Azure SQL Edge en is niet beschikbaar voor de ARM64-versie van SQL Edge.|

### <a name="example-create-an-external-stream-inputoutput-object-for-azure-iot-edge-hub"></a>Voor beeld: een extern stream-invoer/uitvoer-object maken voor Azure IoT Edge hub

In het volgende voor beeld wordt een extern Stream-object gemaakt voor de Edge hub. Als u een externe stream-invoer/uitvoer-gegevens bron voor Azure IoT Edge hub wilt maken, moet u eerst een externe bestands indeling maken voor SQL om inzicht te krijgen in de indeling van de gegevens die worden gelezen of geschreven.

1. Maak een externe bestands indeling met het notatie type JSON.

    ```sql
    Create External file format InputFileFormat
    WITH (  
       format_type = JSON,
    )
    go
    ```

2. Een externe gegevens bron maken voor de IoT Edge hub. Het T-SQL-script hieronder maakt u een gegevens bron verbinding met een Edge-hub die op dezelfde docker-host wordt uitgevoerd als SQL-rand.

    ```sql
    CREATE EXTERNAL DATA SOURCE EdgeHubInput WITH (
    LOCATION = 'edgehub://'
    )
    go
    ```

3. Maak het externe Stream-object voor de IoT Edge hub. Met T-SQL-script hieronder maakt u een Stream-object voor de Edge-hub. In het geval van een Edge hub Stream-object is de locatie parameter de naam van het onderwerp van de Edge hub/het kanaal dat wordt gelezen of waarnaar wordt geschreven.

    ```sql
    CREATE EXTERNAL STREAM MyTempSensors WITH (
    DATA_SOURCE = EdgeHubInput,
    FILE_FORMAT = InputFileFormat,
    LOCATION = N'TemperatureSensors',
    INPUT_OPTIONS = N'',
    OUTPUT_OPTIONS = N''
    )
    go
    ```

### <a name="example-create-an-external-stream-object-sql-database"></a>Voor beeld: een extern Stream-object maken SQL Database

In het volgende voor beeld wordt een extern Stream-object gemaakt voor de lokale SQL EDGE-Data Base. 

1. Maak een hoofd sleutel voor de data base. Dit is vereist om het referentie geheim te versleutelen.

    ```sql
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<<Strong_Password_For_Master_Key_Encryption>>';
    ```

2. Maak een Data Base-bereik referentie voor toegang tot de SQL Server bron. In het volgende voor beeld wordt een referentie met de externe gegevens bron gemaakt met identiteit = ' username ' en SECRET = ' password '.

    ```sql
    CREATE DATABASE SCOPED CREDENTIAL SQLCredential
    WITH IDENTITY = '<SQL_Login>', SECRET = '<SQL_Login_PASSWORD>'
    go
    ```

3. Een externe gegevens bron maken met externe gegevens bron maken. Het volgende voor beeld:

    * Hiermee maakt u een externe gegevens bron met de naam LocalSQLOutput
    * Hiermee wordt de externe gegevens bron geïdentificeerd (locatie = ' <vendor> :// <server> [: <port> ] '). In het voor beeld verwijst deze naar een lokaal exemplaar van SQL Edge.
    * Ten slotte maakt het voor beeld gebruik van de referentie die eerder is gemaakt.

    ```sql
    CREATE EXTERNAL DATA SOURCE LocalSQLOutput WITH (
    LOCATION = 'sqlserver://tcp:.,1433'
    ,CREDENTIAL = SQLCredential
    )
    go
    ```

4. Maak het externe Stream-object. In het volgende voor beeld wordt een extern Stream-object gemaakt dat verwijst naar een tabel *dbo. TemperatureMeasurements* in de Data Base- *MySQLDatabase*.

    ```sql
    CREATE EXTERNAL STREAM TemperatureMeasurements WITH (
    DATA_SOURCE = LocalSQLOutput,
    LOCATION = N'MySQLDatabase.dbo.TemperatureMeasurements',
    INPUT_OPTIONS = N'',
    OUTPUT_OPTIONS = N''
    )
    ```

## <a name="create-the-streaming-job-and-the-streaming-queries"></a>De streaming-taak en de streaming-query's maken

Gebruik de opgeslagen procedure **sys. sp_create_streaming_job** systeem om de streaming-query's te definiëren en de streaming-taak te maken. Voor de opgeslagen procedure **sp_create_streaming_job** worden twee para meters gebruikt

- job_name-naam van de streaming-taak. De namen van streaming-taken zijn uniek binnen het exemplaar.
- statement- [Stream Analytics op query's](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference?) gebaseerde streaming-query-instructies.

In het onderstaande voor beeld maakt u een eenvoudige streaming-taak met één streaming-query. Deze query leest de invoer van de Edge hub en schrijft deze naar de *dbo. TemperatureMeasurements* in de data base.

```sql
EXEC sys.sp_create_streaming_job @name=N'StreamingJob1',
@statement= N'Select * INTO TemperatureMeasurements from MyEdgeHubInput'
```

In het onderstaande voor beeld wordt een complexere streaming-taak gemaakt met meerdere verschillende query's, met inbegrip van een query die gebruikmaakt van de ingebouwde functie AnomalyDetection_ChangePoint om afwijkingen in de gegevens van de Tempe ratuur te identificeren.

```sql
EXEC sys.sp_create_streaming_job @name=N'StreamingJob2', @statement=
N' Select * INTO TemperatureMeasurements1 from MyEdgeHubInput1

Select * Into TemperatureMeasurements2 from MyEdgeHubInput2

Select * Into TemperatureMeasurements3 from MyEdgeHubInput3

SELECT
Timestamp as [Time],
[Temperature] As [Temperature],
GetRecordPropertyValue(AnomalyDetection_ChangePoint(Temperature, 80, 1200) OVER(LIMIT DURATION(minute, 20)), ''Score'') as ChangePointScore,
GetRecordPropertyValue(AnomalyDetection_ChangePoint(Temperature, 80, 1200) OVER(LIMIT DURATION(minute, 20)), ''IsAnomaly'') as IsChangePointAnomaly,
INTO TemperatureAnomalies FROM MyEdgeHubInput2;
'
go
```

## <a name="start-stop-drop-and-monitor-streaming-jobs"></a>Streaming-taken starten, stoppen, verwijderen en bewaken

Als u een streaming-taak in SQL-rand wilt starten, voert u de opgeslagen procedure **sys. sp_start_streaming_job** uit. Voor de opgeslagen procedure moet dezelfde van de streaming-taak worden gestart als invoer.

```sql
exec sys.sp_start_streaming_job @name=N'StreamingJob1'
go
```

Als u een streaming-taak in SQL Edge wilt stoppen, voert u de opgeslagen procedure **sys. sp_stop_streaming_job** uit. Voor de opgeslagen procedure moet dezelfde van de streaming-taak worden gestopt als invoer.

```sql
exec sys.sp_stop_streaming_job @name=N'StreamingJob1'
go
```

Voer de opgeslagen procedure **sys. sp_drop_streaming_job** uit als u een streaming-taak in SQL Edge wilt verwijderen. Voor de opgeslagen procedure moet hetzelfde van de streaming-taak worden verwijderd als invoer.

```sql
exec sys.sp_drop_streaming_job @name=N'StreamingJob1'
go
```

Als u de huidige status van een streaming-taak in SQL Edge wilt ophalen, voert u de opgeslagen procedure **sys. sp_get_streaming_job** uit. Voor de opgeslagen procedure moet hetzelfde van de streaming-taak worden verwijderd als invoer en wordt de naam en de huidige status van de streaming-taak uitgevoerd.

```sql
exec sys.sp_get_streaming_job @name=N'StreamingJob1'
        WITH RESULT SETS
(
       (
       name nvarchar(256),
       status nvarchar(256)
       )
)
```

De streaming-taak kan een van de volgende statussen hebben:

| Status | Beschrijving |
|--------| ------------|
| Gemaakt | De streaming-taak is gemaakt, maar is nog niet gestart |
| Starten | De streaming-taak bevindt zich in de start fase |
| Actieve | De streaming-taak wordt uitgevoerd, maar er is geen invoer voor het proces |
| Wordt verwerkt | De streaming-taak wordt uitgevoerd en verwerkt de invoer. Deze status geeft een goede status aan voor de streaming-taak |
| Verminderd beschikbaar | De streaming-taak wordt uitgevoerd, maar er zijn een aantal niet-fatale invoer/uitvoer-serialisatie/deserialisatie-fouten opgetreden tijdens de invoer verwerking. De invoer taak wordt nog steeds uitgevoerd, maar gaat weg van invoer die fouten ondervindt |
| Gestopt | De streaming-taak is gestopt |
| Mislukt | De streaming-taak is mislukt. Dit is in het algemeen een indicatie van een fatale fout tijdens de verwerking |

## <a name="next-steps"></a>Volgende stappen

- [Meta gegevens weer geven die zijn gekoppeld aan streaming-taken in Azure SQL Edge (preview)](streaming-catalog-views.md) 
- [Een externe stroom maken](create-external-stream-transact-sql.md)
