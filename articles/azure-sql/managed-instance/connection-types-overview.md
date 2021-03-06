---
title: Verbindingstypen
titleSuffix: Azure SQL Managed Instance
description: Meer informatie over de verbindings typen van Azure SQL Managed instance
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto
ms.date: 10/07/2019
ms.openlocfilehash: cee913e846ebfef174a3cd6383401eace89187f0
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2020
ms.locfileid: "84044337"
---
# <a name="azure-sql-managed-instance-connection-types"></a>Verbindings typen van Azure SQL Managed instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

In dit artikel wordt uitgelegd hoe clients verbinding maken met een beheerd exemplaar van Azure SQL, afhankelijk van het verbindings type. Hieronder vindt u voor beelden van scripts voor het wijzigen van de verbindings typen, samen met overwegingen met betrekking tot het wijzigen van de standaard connectiviteits instellingen.

## <a name="connection-types"></a>Verbindingstypen

Azure SQL Managed instance ondersteunt de volgende twee verbindings typen:

- **Omleiden (aanbevolen):** Clients maken rechtstreeks verbinding met het knoop punt dat als host fungeert voor de data base. Als u connectiviteit via omleiding wilt inschakelen, moet u firewalls en netwerk beveiligings groepen (NSG) openen om toegang toe te staan op de poorten 1433 en 11000-11999. Pakketten gaan rechtstreeks naar de data base en daarom zijn er verbeteringen in latentie en doorvoer prestaties met behulp van omleiding via een proxy.
- **Proxy (standaard instelling):** In deze modus gebruiken alle verbindingen een proxy gateway onderdeel. Om connectiviteit in te scha kelen, moet alleen poort 1433 voor particuliere netwerken en poort 3342 voor open bare verbinding worden geopend. Het kiezen van deze modus kan leiden tot hogere latentie en een lagere door Voer, afhankelijk van de aard van de werk belasting. U wordt aangeraden om het beleid voor omleidings verbindingen uit te voeren via het beleid voor proxy verbindingen voor de laagste latentie en de hoogste door voer.

## <a name="redirect-connection-type"></a>Verbindings type omleiden

Verbindings type omleiden betekent dat wanneer de TCP-sessie tot stand is gebracht met de SQL-engine, de client sessie het virtuele IP-doel van het virtuele cluster knooppunt van de load balancer verkrijgt. Volgende pakketten stromen rechtstreeks naar het virtuele cluster knooppunt, waarbij de gateway wordt omzeild. In het volgende diagram ziet u deze verkeers stroom.

![omleiding. png](./media/connection-types-overview/redirect.png)

> [!IMPORTANT]
> Het verbindings type voor omleiding werkt momenteel alleen voor een persoonlijk eind punt. Ongeacht de instelling van het verbindings type, wordt de verbinding via het open bare eind punt tot stand gebracht via een proxy.

## <a name="proxy-connection-type"></a>Type proxy verbinding

Proxy Verbindings type betekent dat de TCP-sessie tot stand is gebracht met behulp van de gateway en dat alle volgende pakketten door de server worden gebruikt. In het volgende diagram ziet u deze verkeers stroom.

![proxy. png](./media/connection-types-overview/proxy.png)

## <a name="script-to-change-connection-type-settings-using-powershell"></a>Script voor het wijzigen van de instellingen van het verbindings type met Power shell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Het volgende Power shell-script laat zien hoe u het verbindings type voor een SQL Managed instance wijzigt in `Redirect` .

```powershell
Install-Module -Name Az
Import-Module Az.Accounts
Import-Module Az.Sql

Connect-AzAccount
# Get your SubscriptionId from the Get-AzSubscription command
Get-AzSubscription
# Use your SubscriptionId in place of {subscription-id} below
Select-AzSubscription -SubscriptionId {subscription-id}
# Replace {rg-name} with the resource group for your SQL Managed Instance, and replace {mi-name} with the name of your SQL Managed Instance
$mi = Get-AzSqlInstance -ResourceGroupName {rg-name} -Name {mi-name}
$mi = $mi | Set-AzSqlInstance -ProxyOverride "Redirect" -force
```

## <a name="next-steps"></a>Volgende stappen

- [Een Data Base herstellen naar een beheerd exemplaar van SQL](restore-sample-database-quickstart.md)
- Meer informatie over het [configureren van een openbaar eind punt op een SQL-beheerd exemplaar](public-endpoint-configure.md)
- Meer informatie over de [connectiviteits architectuur van SQL Managed instance](connectivity-architecture-overview.md)