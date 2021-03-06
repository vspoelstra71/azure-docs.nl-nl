---
title: Door de gebruiker gedefinieerde schema's gebruiken
description: Tips voor het gebruik van door de gebruiker gedefinieerde T-SQL-schema's voor het ontwikkelen van oplossingen in de Synapse SQL-groep.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 7144fa75d156ca7aed9d8215592f89c167cfb221
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80633454"
---
# <a name="user-defined-schemas-in-synapse-sql-pool"></a>Door de gebruiker gedefinieerde schema's in Synapse SQL-groep
Dit artikel is gericht op het bieden van verschillende tips voor het gebruik van door de gebruiker gedefinieerde T-SQL-schema's voor het ontwikkelen van oplossingen in de Synapse SQL-groep.

## <a name="schemas-for-application-boundaries"></a>Schema's voor toepassings grenzen

Traditionele data warehouses gebruiken vaak afzonderlijke data bases om toepassings grenzen te maken op basis van de werk belasting, het domein of de beveiliging. 

Een voor beeld: een traditioneel SQL Server Data Warehouse kan een faserings database, een Data Warehouse-data base en enkele datamart-data bases bevatten. In deze topologie fungeert elke Data Base als een werk belasting en beveiligings grens in de architectuur.

De SQL-pool voert daarentegen de volledige werk belasting van het data warehouse in één Data Base uit. Cross-data base-samen voegingen zijn niet toegestaan. De SQL-pool verwacht dat alle tabellen die worden gebruikt door het magazijn, worden opgeslagen in de ene data base.

> [!NOTE]
> SQL-pool biedt geen ondersteuning voor query's van alle typen van meerdere data bases. Data Warehouse-implementaties die gebruikmaken van dit patroon moeten daarom worden gereviseerd.
> 
> 

## <a name="recommendations"></a>Aanbevelingen
Hieronder vindt u aanbevelingen voor het samen voegen van werk belastingen, beveiliging, domein en functionele grenzen door gebruik te maken van door de gebruiker gedefinieerde schema's:

- Gebruik één SQL-groeps database om uw hele Data Warehouse-werk belasting uit te voeren.
- Consolideer uw bestaande data warehouse-omgeving om één data base van SQL-groep te gebruiken.
- Gebruik door de **gebruiker gedefinieerde schema's** om de grens te leveren die eerder is geïmplementeerd met behulp van data bases.

Als door de gebruiker gedefinieerde schema's niet eerder zijn gebruikt, hebt u een schone pastel. Gebruik de oude database naam als basis voor uw door de gebruiker gedefinieerde schema's in de data base van de SQL-groep.

Als er al schema's zijn gebruikt, hebt u een aantal opties:

- Verwijder de oude schema namen en begin vers.
- Behoud de oude schema namen door de oude schema naam vooraf in behandeling te laten nemen aan de naam van de tabel.
- Behoud de oude schema namen door weer gaven te implementeren in de tabel in een extra schema om de oude schema structuur opnieuw te maken.

> [!NOTE]
> Op de eerste inspectie optie 3 lijkt de meest aantrekkelijke optie. De zon bevindt zich echter in het detail. Weer gaven zijn alleen-lezen in de SQL-groep. Eventuele gegevens of tabel wijzigingen moeten worden uitgevoerd voor de basis tabel. Met optie 3 wordt ook een laag van weer gaven in uw systeem geïntroduceerd. U kunt hiervan een extra idee geven als u al gebruikmaakt van weer gaven in uw architectuur.
> 
> 

### <a name="examples"></a>Voorbeelden:
Door de gebruiker gedefinieerde schema's implementeren op basis van database namen:

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Behoud oude schema namen door deze vooraf in te wachten op de tabel naam. Schema's gebruiken voor de grens van de werk belasting:

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Oude schema namen met behulp van weer gaven blijven gebruiken:

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [!NOTE]
> Voor elke wijziging in de schema strategie moet het beveiligings model voor de Data Base worden gecontroleerd. In veel gevallen kunt u het beveiligings model vereenvoudigen door machtigingen toe te wijzen op schema niveau. Als u meer gedetailleerde machtigingen nodig hebt, kunt u database rollen gebruiken.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Zie [ontwikkelings overzicht](sql-data-warehouse-overview-develop.md)voor meer tips voor ontwikkel aars.

