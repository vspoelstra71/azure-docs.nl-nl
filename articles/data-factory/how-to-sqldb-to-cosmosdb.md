---
title: Azure SQL Database tabellen migreren naar Azure CosmosDB met Azure Data Factory
description: Maak een bestaand genormaliseerd database schema van Azure SQL Database en Migreer naar een genormaliseerde Azure CosmosDB-container met Azure Data Factory.
services: data-factory
author: kromerm
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/29/2020
ms.author: makromer
ms.openlocfilehash: 3d2ef6fb0cd7af444b9bff755eee4eee70d03d15
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2020
ms.locfileid: "82691902"
---
# <a name="migrate-normalized-database-schema-from-azure-sql-database-to-azure-cosmosdb-denormalized-container"></a>Genormaliseerd database schema migreren van Azure SQL Database naar een gedenormaliseerde Azure CosmosDB-container

In deze hand leiding wordt uitgelegd hoe u een bestaand genormaliseerd database schema in Azure SQL Database maakt en dit converteert naar een genormaliseerd Azure CosmosDB-schema voor het laden in azure CosmosDB.

SQL-schema's worden normaal gesp roken gemodelleerd met gebruikmaking van de derde normaal vorm, wat resulteert in genormaliseerde schema's die een hoge mate van gegevens integriteit bieden en minder dubbele gegevens waarden. Query's kunnen entiteiten samen voegen in verschillende tabellen om te lezen. CosmosDB is geoptimaliseerd voor super snelle trans acties en bij het uitvoeren van query's in een verzameling of container via gedenormaliseerde schema's met gegevens die zich in een document bevinden.

Met Azure Data Factory maakt u een pijp lijn die gebruikmaakt van één toewijzings gegevensstroom om te lezen uit twee Azure SQL Database genormaliseerde tabellen die primaire en refererende sleutels bevatten als entiteits relatie. ADF voegt deze tabellen samen tot één stroom met behulp van de data flow Spark-engine, verzamelt gekoppelde rijen in matrices en produceert afzonderlijke gereinigde documenten om in een nieuwe Azure CosmosDB-container in te voegen.

In deze hand leiding wordt een nieuwe container met de naam ' orders ' gemaakt waarmee de ```SalesOrderHeader``` -en ```SalesOrderDetail``` -tabellen worden gebruikt in de standaard SQL Server AdventureWorks-voorbeeld database. Deze tabellen vertegenwoordigen verkoop transacties die zijn ```SalesOrderID```gekoppeld door. Elke unieke detail record heeft een eigen primaire sleutel van ```SalesOrderDetailID```. De relatie tussen koptekst en detail is ```1:M```. We nemen deel aan ```SalesOrderID``` in de ADF en vervolgens draaien alle gerelateerde detail records naar een matrix met de naam ' Details '.

De representatieve SQL-query voor deze hand leiding is:

```
  SELECT
  o.SalesOrderID,
  o.OrderDate,
  o.Status,
  o.ShipDate,
  o.SalesOrderNumber,
  o.ShipMethod,
  o.SubTotal,
  (select SalesOrderDetailID, UnitPrice, OrderQty from SalesLT.SalesOrderDetail od where od.SalesOrderID = o.SalesOrderID for json auto) as OrderDetails
FROM SalesLT.SalesOrderHeader o;
```

In de resulterende CosmosDB-container wordt de interne query in één document Inge sloten. dit ziet er als volgt uit:

![Verzameling](media/data-flow/cosmosb3.png)

## <a name="create-a-pipeline"></a>Een pijplijn maken

1. Selecteer **+ nieuwe pijp lijn** om een nieuwe pijp lijn te maken.

2. Een gegevens stroom activiteit toevoegen

3. Selecteer in de activiteit gegevens stroom de optie **nieuwe toewijzings gegevens stroom**.

4. Hieronder wordt deze gegevensstroom grafiek samen stellen

![Grafiek van gegevens stroom](media/data-flow/cosmosb1.png)

5. Definieer de bron voor ' SourceOrderDetails '. Maak voor dataset een nieuwe Azure SQL Database-gegevensset die naar de ```SalesOrderDetail``` tabel verwijst.

6. Definieer de bron voor ' SourceOrderHeader '. Maak voor dataset een nieuwe Azure SQL Database-gegevensset die naar de ```SalesOrderHeader``` tabel verwijst.

7. Voeg op de bovenste bron een afgeleide kolom transformatie toe na "SourceOrderDetails". Roep de nieuwe trans formatie ' TypeCast ' aan. We moeten de ```UnitPrice``` kolom afronden en deze converteren naar een dubbel gegevens type voor CosmosDB. Stel de formule in op ```toDouble(round(UnitPrice,2))```:.

8. Voeg nog een afgeleide kolom toe en noem deze ' MakeStruct '. Hier gaan we een hiërarchische structuur maken voor de waarden uit de tabel Details. Let op: Details is ```M:1``` een relatie met de koptekst. Geef de nieuwe structuur ```orderdetailsstruct``` een naam en maak de hiërarchie op deze manier en stel elke subkolom in op de naam van de inkomende kolom:

![Structuur maken](media/data-flow/cosmosb9.png)

9. Nu gaan we naar de bron van de verkoopkop. Voeg een koppelings transformatie toe. Selecteer ' MakeStruct ' voor de rechter kant. Laat het item ingesteld op inner join en ```SalesOrderID``` Kies aan beide zijden van de voor waarde voor samen voegen.

10. Klik op het tabblad voor het voor beeld van de gegevens in de nieuwe koppeling die u hebt toegevoegd, zodat u de resultaten tot nu toe kunt zien. U ziet dat alle koptekst rijen worden samengevoegd met de detail rijen. Dit is het resultaat van de koppeling die wordt gevormd door ```SalesOrderID```de. Vervolgens combi neren we de gegevens uit de gemeen schappelijke rijen in de structuur Details en voegen we de algemene rijen samen.

![Koppelen](media/data-flow/cosmosb4.png)

11. Voordat we de matrices kunnen maken om deze rijen te denormaliseren, moeten we eerst ongewenste kolommen verwijderen en ervoor zorgen dat de gegevens waarden overeenkomen met CosmosDB-gegevens typen.

12. Voeg een trans formatie selecteren toe en stel de veld toewijzing zo in dat deze er als volgt uitziet:

![Kolom-reinigingser](media/data-flow/cosmosb5.png)

13. Nu gaan we deze keer ```TotalDue```een valuta kolom converteren. Zoals hierboven is beschreven in stap 7, stelt u de formule in ```toDouble(round(TotalDue,2))```op:.

14. Hier ziet u hoe de rijen worden genormaliseerd door deze te groeperen op basis ```SalesOrderID```van de gemeen schappelijke sleutel. Voeg een geaggregeerde trans formatie toe en stel de ```SalesOrderID```groep in op aan.

15. Voeg in de formule aggregate een nieuwe kolom met de naam ' Details ' toe en gebruik deze formule om de waarden in de structuur te verzamelen die ```orderdetailsstruct```u ```collect(orderdetailsstruct)```eerder hebt gemaakt:.

16. De cumulatieve trans formatie levert alleen kolommen op die deel uitmaken van aggregatie of groeperen op formules. Daarom moeten we ook de kolommen uit de verkoopkop toevoegen. U doet dit door een kolom patroon toe te voegen in diezelfde geaggregeerde trans formatie. Dit patroon bevat alle andere kolommen in de uitvoer:

```instr(name,'OrderQty')==0&&instr(name,'UnitPrice')==0&&instr(name,'SalesOrderID')==0```

17. Gebruik de syntaxis ' This ' in de andere eigenschappen, zodat we dezelfde kolom namen behouden en de ```first()``` functie als aggregatie gebruiken:

![Samenvoegen](media/data-flow/cosmosb6.png)

18. We zijn klaar om de migratie stroom te volt ooien door een Sink-trans formatie toe te voegen. Klik op nieuw naast gegevensset en voeg een CosmosDB-gegevensset toe die verwijst naar uw CosmosDB-data base. Voor de verzameling noemen we het ' orders ', maar er zijn geen schema's en geen documenten, omdat deze in de vlucht worden gemaakt.

19. In de instellingen voor wastafels, ```\SalesOrderID``` partitie sleutel naar en verzamelings actie om opnieuw te maken. Zorg ervoor dat het tabblad toewijzing er als volgt uitziet:

![Sink-instellingen](media/data-flow/cosmosb7.png)

20. Klik op voor beeld van gegevens om er zeker van te zijn dat deze 32 rijen zijn ingesteld om in te voegen als nieuwe documenten in de nieuwe container:

![Sink-instellingen](media/data-flow/cosmosb8.png)

Als alles er goed uitziet, bent u nu klaar om een nieuwe pijp lijn te maken, voegt u deze gegevensstroom activiteit toe aan de pijp lijn en voert u deze uit. U kunt uitvoeren vanuit debug of een geactiveerde uitvoering. Na een paar minuten moet u in uw CosmosDB-Data Base een nieuwe, genormaliseerde container met orders genaamd ' orders ' hebben.

## <a name="next-steps"></a>Volgende stappen

* Bouw de rest van uw gegevensstroom logica met behulp van trans [formaties](concepts-data-flow-overview.md)voor gegevens stromen.
* [Down load de voltooide pijplijn sjabloon](https://github.com/kromerm/adfdataflowdocs/blob/master/sampledata/SQL%20Orders%20to%20CosmosDB.zip) voor deze zelf studie en importeer de sjabloon in uw fabriek.
