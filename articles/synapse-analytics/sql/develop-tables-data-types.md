---
title: Gegevens typen definiëren
description: Aanbevelingen voor het definiëren van tabel gegevens typen in Synapse SQL.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.custom: ''
ms.openlocfilehash: 312c58a7df09ebe3e0bdf0749f902199723a331b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "81429003"
---
# <a name="table-data-types-in-synapse-sql"></a>Tabel gegevens typen in Synapse SQL

Aanbevelingen voor het definiëren van tabel gegevens typen in Synapse SQL. 

## <a name="what-are-the-data-types"></a>Wat zijn de gegevens typen?

Synapse SQL ondersteunt het meest gebruikte gegevens type. Zie [gegevens typen](/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes) in de CREATE TABLE-instructie voor een lijst met ondersteunde gegevens typen. 

## <a name="minimize-row-length"></a>Lengte van rij minimaliseren

Het minimaliseren van de grootte van gegevens typen verkort de lengte van de rij, wat leidt tot betere query prestaties. Gebruik het kleinste gegevens type dat voor uw gegevens werkt.

- Vermijd het definiëren van teken kolommen met een grote standaard lengte. Als de langste waarde bijvoorbeeld 25 tekens is, definieert u uw kolom als VARCHAR (25).
- Vermijd het gebruik van [NVARCHAR] [NVARCHAR] wanneer u alleen VARCHAR nodig hebt.
- Gebruik, indien mogelijk, NVARCHAR (4000) of VARCHAR (8000) in plaats van NVARCHAR (MAX) of VARCHAR (MAX).

> [!NOTE]
> Als u poly base externe tabellen gebruikt om uw SQL-groeps tabellen te laden, mag de gedefinieerde lengte van de tabelrij niet meer zijn dan 1 MB. Wanneer een rij met gegevens van variabele lengte meer dan 1 MB overschrijdt, kunt u de rij laden met BCP, maar niet met poly base.

## <a name="identify-unsupported-data-types"></a>Niet-ondersteunde gegevens typen identificeren

Als u uw data base migreert vanuit een andere SQL database, kunt u gegevens typen tegen komen die niet worden ondersteund in Synapse SQL. Gebruik deze query om niet-ondersteunde gegevens typen in uw bestaande SQL-schema te detecteren.

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','xml')
 AND  y.[is_user_defined] = 1;
```

## <a name="workarounds-for-unsupported-data-types"></a><a name="unsupported-data-types"></a>Tijdelijke oplossingen voor niet-ondersteunde gegevens typen

In de volgende lijst worden de gegevens typen weer gegeven die Synapse SQL ondersteunt en worden er alternatieven geboden die u kunt gebruiken in plaats van de niet-ondersteunde gegevens typen.

| Niet-ondersteund gegevens type | Tijdelijke oplossing |
| --- | --- |
| [geometrie](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [Geografie](/sql/t-sql/spatial-geography/spatial-types-geography) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [hierarchyid](/sql/t-sql/data-types/hierarchyid-data-type-method-reference) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)(4000) |
| [bitmapafbeelding](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [SMS](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [ntext](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [sql_variant](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |Kolom splitsen in meerdere sterk getypeerde kolommen. |
| [tabel](/sql/t-sql/data-types/table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |Als u een SQL-groep gebruikt, kunt u deze converteren naar tijdelijke tabellen. Als u SQL (preview) gebruikt, kunt u overwegen om gegevens op te slaan in de opslag met behulp van [CETAS](../sql/develop-tables-cetas.md). |
| [Neem](/sql/t-sql/data-types/date-and-time-types) |Herwerkings code voor het gebruik van [DATETIME2](/sql/t-sql/data-types/datetime2-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) en de functie [CURRENT_TIMESTAMP](/sql/t-sql/functions/current-timestamp-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) . Alleen constanten worden ondersteund als standaard waarden, dus current_timestamp kan niet worden gedefinieerd als een default-beperking. Als u de waarden van de rij-versie wilt migreren uit een opgegeven time stamp-kolom, gebruikt u [binary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)(8) of [varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)(8) voor versie waarden van null of null. |
| [xml](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [door de gebruiker gedefinieerd type](/sql/relational-databases/native-client/features/using-user-defined-types) |Converteer indien mogelijk terug naar het systeem eigen gegevens type. |
| standaard waarden | Standaard waarden ondersteunen alleen literals en constanten. |

## <a name="next-steps"></a>Volgende stappen

Zie [tabel Overzicht](develop-overview.md)voor meer informatie over het ontwikkelen van tabellen.
