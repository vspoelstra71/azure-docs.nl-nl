---
title: Schaal berekenen voor Synapse SQL-pool (Azure PowerShell)
description: U kunt Compute voor Synapse SQL-pool (Data Warehouse) schalen met behulp van Azure PowerShell.
services: synapse-analytics
author: Antvgski
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/17/2018
ms.author: anvang
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: e3038617c6270acf9af295c910e9fd5c7dae2043
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "80633785"
---
# <a name="quickstart-scale-compute-for-synapse-sql-pool-with-azure-powershell"></a>Snelstartgids: Compute Scale for Synapse SQL pool with Azure PowerShell

U kunt Compute voor Synapse SQL-pool (Data Warehouse) schalen met behulp van Azure PowerShell. [Vergroot de schaal van Compute](sql-data-warehouse-manage-compute-overview.md) voor betere prestaties of verklein de schaal juist om kosten te besparen.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis](https://azure.microsoft.com/free/) account aan voordat u begint.

## <a name="before-you-begin"></a>Voordat u begint

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

In deze Quick Start wordt ervan uitgegaan dat u al een SQL-groep hebt die u kunt schalen. Als u er een wilt maken, gebruikt u [maken en verbinden-Portal](create-data-warehouse-portal.md) om een SQL-groep met de naam **mySampleDataWarehouse**te maken.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) en volg de aanwijzingen op het scherm.

```powershell
Connect-AzAccount
```

Als u wilt zien welk abonnement u gebruikt, voert u [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)uit.

```powershell
Get-AzSubscription
```

Als u een ander abonnement wilt gebruiken dan de standaard instelling, voert u [set-AzContext](/powershell/module/az.accounts/set-azcontext?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

```powershell
Set-AzContext -SubscriptionName "MySubscription"
```

## <a name="look-up-data-warehouse-information"></a>Datawarehousegegevens opzoeken

Zoek de databasenaam, de servernaam en de resourcegroep op voor het datawarehouse dat u wilt onderbreken en hervatten.

Volg deze stappen om de locatiegegevens voor uw datawarehouse op te zoeken.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).
2. Klik op **Azure Synapse Analytics (voorheen SQL DW)** op de linker navigatie pagina van het Azure Portal.
3. Selecteer **mySampleDataWarehouse** op de pagina **Azure Synapse Analytics (voorheen SQL DW)** om het Data Warehouse te openen.

    ![Servernaam en resourcegroep](./media/quickstart-scale-compute-powershell/locate-data-warehouse-information.png)

4. Noteer de naam van het datawarehouse. Deze wordt gebruikt als de databasenaam. Een datawarehouse is een type database. Noteer ook de naam van de server en de resourcegroep. U gebruikt de server naam en de naam van de resource groep in de opdrachten Pause en resume.
5. Gebruik alleen het eerste deel van de server naam in de Power shell-cmdlets. In de voor gaande afbeelding is de volledige server naam sqlpoolservername.database.windows.net. We gebruiken **sqlpoolservername** als de server naam in de Power shell-cmdlet.

## <a name="scale-compute"></a>De schaal van Compute aanpassen

In SQL-pool kunt u de reken resources verg Roten of verkleinen door Data Warehouse-eenheden aan te passen. Met behulp van [Maken en verbinden - portal](create-data-warehouse-portal.md) is **mySampleDataWarehouse** gemaakt en vervolgens gestart met 400 DWU's. In de volgende stappen wordt het aantal DWU's voor **mySampleDataWarehouse** aangepast.

Als u Data Warehouse-eenheden wilt wijzigen, gebruikt u de Power shell [-cmdlet Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) . In het volgende voor beeld worden de Data Warehouse-eenheden ingesteld op DW300c voor de Data Base- **mySampleDataWarehouse**, die wordt gehost in de resource groep **resourcegroupname** op server **sqlpoolservername**.

```Powershell
Set-AzSqlDatabase -ResourceGroupName "resourcegroupname" -DatabaseName "mySampleDataWarehouse" -ServerName "sqlpoolservername" -RequestedServiceObjectiveName "DW300c"
```

## <a name="check-data-warehouse-state"></a>Status van datawarehouse controleren

Als u de huidige status van het Data Warehouse wilt zien, gebruikt u de Power shell [-cmdlet Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) . Met deze cmdlet wordt de status van de **mySampleDataWarehouse** -data base in ResourceGroup **resourcegroupname** en server **sqlpoolservername.database.Windows.net**weer gegeven.

```powershell
$database = Get-AzSqlDatabase -ResourceGroupName resourcegroupname -ServerName sqlpoolservername -DatabaseName mySampleDataWarehouse
```

Dit resulteert in ongeveer het volgende:

```powershell
ResourceGroupName             : resourcegroupname
ServerName                    : sqlpoolservername
DatabaseName                  : mySampleDataWarehouse
Location                      : North Europe
DatabaseId                    : 34d2ffb8-b70a-40b2-b4f9-b0a39833c974
Edition                       : DataWarehouse
CollationName                 : SQL_Latin1_General_CP1_CI_AS
CatalogCollation              :
MaxSizeBytes                  : 263882790666240
Status                        : Online
CreationDate                  : 11/20/2017 9:18:12 PM
CurrentServiceObjectiveId     : 284f1aff-fee7-4d3b-a211-5b8ebdd28fea
CurrentServiceObjectiveName   : DW300c
RequestedServiceObjectiveId   : 284f1aff-fee7-4d3b-a211-5b8ebdd28fea
RequestedServiceObjectiveName :
ElasticPoolName               :
EarliestRestoreDate           :
Tags                          :
ResourceId                    : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/
                                resourceGroups/resourcegroupname/providers/Microsoft.Sql/servers/sqlpoolservername/databases/mySampleDataWarehouse
CreateMode                    :
ReadScale                     : Disabled
ZoneRedundant                 : False
```

U ziet de **Status** van de database in de uitvoer. In dit geval kunt u zien dat deze database online is.  Wanneer u deze opdracht uitvoert, ontvangt u de statuswaarde Online, Onderbreken, Hervatten, Schalen of Onderbroken.

Voer de volgende opdracht uit om de status op zichzelf te bekijken:

```powershell
$database | Select-Object DatabaseName,Status
```

## <a name="next-steps"></a>Volgende stappen

U hebt nu geleerd hoe u de reken kracht voor de SQL-groep kunt schalen. Voor meer informatie over SQL-pool gaat u verder met de zelf studie voor het laden van gegevens.

> [!div class="nextstepaction"]
>[Gegevens laden in een SQL-groep](load-data-from-azure-blob-storage-using-polybase.md)
