---
title: Een SQL-beheerd exemplaar implementeren op een exemplaar groep
titleSuffix: Azure SQL Managed Instance
description: In dit artikel wordt beschreven hoe u Azure SQL Managed instance-groepen (preview) maakt en beheert.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: sstein, carlrab
ms.date: 09/05/2019
ms.openlocfilehash: c781e23b23f5dbaf8eba9efe4c27428ef35c7be1
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2020
ms.locfileid: "84113630"
---
# <a name="deploy-an-azure-sql-managed-instance-to-an-instance-pool"></a>Een door Azure SQL beheerd exemplaar implementeren in een exemplaar groep
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

In dit artikel vindt u informatie over het maken van een [exemplaar groep](instance-pools-overview.md) en het implementeren van een Azure SQL Managed instance. 

## <a name="instance-pool-operations"></a>Bewerkingen voor exemplaar groepen

In de volgende tabel ziet u de beschik bare bewerkingen met betrekking tot exemplaar groepen en hun Beschik baarheid in de Azure Portal en Power shell.

|Opdracht|Azure Portal|PowerShell|
|:---|:---|:---|
|Instantie-pool maken|Nee|Ja|
|Exemplaar groep bijwerken (beperkt aantal eigenschappen)|Nee |Ja |
|Gebruik en eigenschappen van exemplaar groep controleren|Nee|Ja |
|Exemplaar groep verwijderen|Nee|Ja|
|Een door SQL beheerd exemplaar in een exemplaar groep maken|Nee|Ja|
|Resource gebruik van SQL Managed instance bijwerken|Ja |Ja|
|Gebruik en eigenschappen van SQL Managed instance controleren|Ja|Ja|
|Een door SQL beheerd exemplaar uit de groep verwijderen|Ja|Ja|
|Een Data Base maken in het exemplaar van de groep|Ja|Ja|
|Een Data Base verwijderen uit een SQL-beheerd exemplaar|Ja|Ja|

Beschik bare [Power shell-opdrachten](https://docs.microsoft.com/powershell/module/az.sql/)

|Cmdlet |Beschrijving |
|:---|:---|
|[New-AzSqlInstancePool](/powershell/module/az.sql/new-azsqlinstancepool/) | Hiermee maakt u een door SQL beheerde exemplaar groep. |
|[Get-AzSqlInstancePool](/powershell/module/az.sql/get-azsqlinstancepool/) | Retourneert informatie over de exemplaar groep. |
|[Set-AzSqlInstancePool](/powershell/module/az.sql/set-azsqlinstancepool/) | Hiermee stelt u eigenschappen in voor een exemplaar groep in het SQL-beheerde exemplaar. |
|[Remove-AzSqlInstancePool](/powershell/module/az.sql/remove-azsqlinstancepool/) | Hiermee verwijdert u een exemplaar groep in het door SQL beheerde exemplaar. |
|[Get-AzSqlInstancePoolUsage](/powershell/module/az.sql/get-azsqlinstancepoolusage/) | Retourneert informatie over het gebruik van SQL Managed instance-groep. |


Als u Power shell wilt gebruiken, [installeert u de nieuwste versie van Power shell core](https://docs.microsoft.com/powershell/scripting/install/installing-powershell#powershell)en volgt u de instructies om [de Azure PowerShell-module te installeren](https://docs.microsoft.com/powershell/azure/install-az-ps).

Voor bewerkingen met betrekking tot instanties in Pools en afzonderlijke instanties gebruikt u de standaard [Managed instance-opdrachten](api-references-create-manage-instance.md#powershell-create-and-manage-managed-instances), maar de eigenschap name van de *instantie groep* moet worden ingevuld wanneer u deze opdrachten gebruikt voor een instantie in een groep.

## <a name="deployment-process"></a>Implementatieproces

Als u een SQL-beheerd exemplaar in een exemplaar groep wilt implementeren, moet u eerst de exemplaar groep implementeren. Dit is een eenmalige, langdurige bewerking waarbij de duur hetzelfde is als de implementatie [van één exemplaar dat is gemaakt in een leeg subnet](sql-managed-instance-paas-overview.md#management-operations). Daarna kunt u SQL Managed instances in de groep implementeren. Dit is een relatief snelle bewerking die doorgaans vijf minuten in beslag neemt. De para meter voor de exemplaar groep moet expliciet worden opgegeven als onderdeel van deze bewerking.

In de open bare preview worden beide acties alleen ondersteund met Power shell en Resource Manager-sjablonen. De Azure Portal-ervaring is momenteel niet beschikbaar.

Nadat het beheerde exemplaar van SQL is geïmplementeerd in een pool, *kunt* u de Azure Portal gebruiken om de eigenschappen ervan te wijzigen op de pagina prijs categorie.

## <a name="create-virtual-network-with-a-subnet"></a>Een virtueel netwerk maken met een subnet 

Als u meerdere instantie groepen binnen hetzelfde virtuele netwerk wilt plaatsen, raadpleegt u de volgende artikelen:

- [De grootte van het VNet-subnet bepalen voor een door Azure SQL beheerd exemplaar](vnet-subnet-determine-size.md).
- Maak een nieuw virtueel netwerk en subnet met behulp van de [Azure Portal sjabloon](virtual-network-subnet-create-arm-template.md) of volg de instructies voor [het voorbereiden van een bestaand virtueel netwerk](vnet-existing-add-subnet.md).
 

## <a name="create-instance-pool"></a>Instantie-pool maken 

Nadat u de voor gaande stappen hebt voltooid, bent u klaar om een exemplaar groep te maken.

De volgende beperkingen zijn van toepassing op exemplaar groepen:

- Alleen Algemeen en GEN5 zijn beschikbaar als open bare preview.
- De groeps naam mag alleen kleine letters, cijfers en afbreek streepjes bevatten en mag niet beginnen met een koppel teken.
- Als u AHB (Azure Hybrid Benefit) wilt gebruiken, wordt dit toegepast op het niveau van de instantie groep. U kunt het licentie type instellen tijdens het maken van een groep of deze op elk gewenst moment bijwerken na het maken.

> [!IMPORTANT]
> Het implementeren van een exemplaar groep is een langlopende bewerking die ongeveer 4,5 uur in beslag neemt.

Netwerk parameters ophalen:

```powershell
$virtualNetwork = Get-AzVirtualNetwork -Name "miPoolVirtualNetwork" -ResourceGroupName "myResourceGroup"
$subnet = Get-AzVirtualNetworkSubnetConfig -Name "miPoolSubnet" -VirtualNetwork $virtualNetwork
```

Een exemplaar groep maken:

```powershell
$instancePool = New-AzSqlInstancePool `
  -ResourceGroupName "myResourceGroup" `
  -Name "mi-pool-name" `
  -SubnetId $subnet.Id `
  -LicenseType "LicenseIncluded" `
  -VCore 80 `
  -Edition "GeneralPurpose" `
  -ComputeGeneration "Gen5" `
  -Location "westeurope"
```

> [!IMPORTANT]
> Omdat het implementeren van een exemplaar groep een langlopende bewerking is, moet u wachten totdat deze is voltooid voordat u een van de volgende stappen in dit artikel uitvoert.

## <a name="create-sql-managed-instance"></a>Door SQL beheerd exemplaar maken

Na de geslaagde implementatie van de exemplaar groep is het tijd om een door SQL beheerd exemplaar te maken.

Als u een SQL Managed instance wilt maken, voert u de volgende opdracht uit:

```powershell
$instanceOne = $instancePool | New-AzSqlInstance -Name "mi-pool-name" -VCore 2 -StorageSizeInGB 256
```

Het implementeren van een exemplaar binnen een groep duurt enkele minuten. Nadat de eerste instantie is gemaakt, kunnen er extra instanties worden gemaakt:

```powershell
$instanceTwo = $instancePool | New-AzSqlInstance -Name "mi-pool-name" -VCore 4 -StorageSizeInGB 512
```

## <a name="create-a-database"></a>Een database maken 

Voor het maken en beheren van data bases in een SQL Managed instance die zich in een groep bevinden, gebruikt u de opdrachten voor één exemplaar.

Een Data Base maken binnen een SQL Managed instance:

```powershell
$poolinstancedb = New-AzSqlInstanceDatabase -Name "mipooldb1" -InstanceName "poolmi-001" -ResourceGroupName "myResourceGroup"
```


## <a name="get-pool-usage"></a>Gebruik van groep ophalen 
 
Een lijst met exemplaren in een groep ophalen:

```powershell
$instancePool | Get-AzSqlInstance
```


Resource gebruik van de groep ophalen:

```powershell
$instancePool | Get-AzSqlInstancePoolUsage
```


Voor een gedetailleerd gebruiks overzicht van de groep en instanties hierin:

```powershell
$instancePool | Get-AzSqlInstancePoolUsage –ExpandChildren
```

Een lijst met de data bases in een exemplaar:

```powershell
$databases = Get-AzSqlInstanceDatabase -InstanceName "pool-mi-001" -ResourceGroupName "resource-group-name"
```


> [!NOTE]
> Er geldt een limiet van 100 data bases per pool (niet per instantie).


## <a name="scale"></a>Schalen 


Nadat u een SQL-beheerd exemplaar hebt gevuld met data bases, kunt u de instantie limieten aanraken met betrekking tot opslag of prestaties. Als het gebruik van de groep niet is overschreden, kunt u in dat geval uw exemplaar schalen.
Het schalen van een SQL Managed instance binnen een pool is een bewerking die een paar minuten in beslag neemt. De vereiste voor schalen is beschikbaar vCores en opslag op het niveau van de instantie groep.

Het aantal vCores en de opslag grootte bijwerken:

```powershell
$instanceOne | Set-AzSqlInstance -VCore 8 -StorageSizeInGB 512 -InstancePoolName "mi-pool-name"
```


Alleen opslag grootte bijwerken:

```powershell
$instance | Set-AzSqlInstance -StorageSizeInGB 1024 -InstancePoolName "mi-pool-name"
```

## <a name="connect"></a>Verbinding maken 

Als u verbinding wilt maken met een door SQL beheerd exemplaar in een groep, zijn de volgende twee stappen vereist:

1. [Schakel het open bare eind punt in voor het exemplaar](#enable-public-endpoint).
2. [Voeg een regel voor binnenkomende verbindingen toe aan de netwerk beveiligings groep (NSG)](#add-an-inbound-rule-to-the-network-security-group).

Nadat beide stappen zijn voltooid, kunt u verbinding maken met het exemplaar met behulp van een openbaar eindpunt adres, poort en referenties die zijn gegeven tijdens het maken van het exemplaar. 

### <a name="enable-public-endpoint"></a>Openbaar eind punt inschakelen

Het inschakelen van het open bare eind punt voor een exemplaar kan worden uitgevoerd via de Azure Portal of met behulp van de volgende Power shell-opdracht:


```powershell
$instanceOne | Set-AzSqlInstance -InstancePoolName "pool-mi-001" -PublicDataEndpointEnabled $true
```

Deze para meter kan ook worden ingesteld tijdens het maken van een exemplaar.

### <a name="add-an-inbound-rule-to-the-network-security-group"></a>Een regel voor binnenkomende verbindingen toevoegen aan de netwerk beveiligings groep 

Deze stap kan worden uitgevoerd met behulp van de Azure Portal of met Power shell-opdrachten en kan op elk gewenst moment worden uitgevoerd nadat het subnet voor het beheerde exemplaar is voor bereid.

Zie [verkeer van open bare eind punten toestaan voor de netwerk beveiligings groep](public-endpoint-configure.md#allow-public-endpoint-traffic-on-the-network-security-group)voor meer informatie.


## <a name="move-existing-single-instance-to-pool"></a>Bestaande ene instantie verplaatsen naar groep
 
Het verplaatsen van exemplaren in en uit een pool is een van de beperkingen voor de open bare preview. Een tijdelijke oplossing is afhankelijk van herstel naar een bepaald tijdstip van data bases van een exemplaar buiten een groep naar een exemplaar dat zich al in een groep bevindt. 

Beide exemplaren moeten zich in hetzelfde abonnement en dezelfde regio bevinden. Het terugzetten van meerdere regio's en meerdere abonnementen wordt momenteel niet ondersteund.

Voor dit proces geldt een periode van uitval tijd.

Bestaande data bases verplaatsen:

1. Werk workloads voor het SQL-beheerde exemplaar dat u migreert,.
2. Genereer scripts om systeem databases te maken en voer deze uit op het exemplaar dat zich in de exemplaar groep bevindt.
3. Een herstel bewerking van elke Data Base naar het exemplaar in de groep uitvoeren.

    ```powershell
    $resourceGroupName = "my resource group name"
    $managedInstanceName = "my managed instance name"
    $databaseName = "my source database name"
    $pointInTime = "2019-08-21T08:51:39.3882806Z"
    $targetDatabase = "name of the new database that will be created"
    $targetResourceGroupName = "resource group of instance pool"
    $targetInstanceName = "pool instance name"
       
    Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
      -ResourceGroupName $resourceGroupName `
      -InstanceName $managedInstanceName `
      -Name $databaseName `
      -PointInTime $pointInTime `
      -TargetInstanceDatabaseName $targetDatabase `
      -TargetResourceGroupName $targetResourceGroupName `
      -TargetInstanceName $targetInstanceName
    ```

4. Ga naar het nieuwe exemplaar van uw toepassing en hervat de workloads.

Als er meerdere data bases zijn, herhaalt u het proces voor elke Data Base.


## <a name="next-steps"></a>Volgende stappen

- Zie [algemene SQL-functies](../database/features-comparison.md)voor een lijst met functies en vergelijkingen.
- Zie [vnet-configuratie voor SQL Managed instance](connectivity-architecture-overview.md)voor meer informatie over vnet-configuraties.
- Zie [een SQL Managed instance maken](instance-create-quickstart.md)voor een Snelstartgids die een beheerd exemplaar maakt en een Data Base herstelt vanuit een back-upbestand.
- Zie voor een zelf studie met behulp van de Azure Database Migration Service (DMS) voor migratie de [migratie van SQL-beheerde exemplaren met behulp van DMS](../../dms/tutorial-sql-server-to-managed-instance.md).
- Zie [Azure SQL Managed instance bewaken met Azure SQL-analyse](../../azure-monitor/insights/azure-sql.md)voor geavanceerde bewaking van prestaties van SQL Managed instance-data bases met ingebouwde probleemoplossings informatie.
- Zie [prijzen van SQL Managed instance](https://azure.microsoft.com/pricing/details/sql-database/managed/)voor prijs informatie.
