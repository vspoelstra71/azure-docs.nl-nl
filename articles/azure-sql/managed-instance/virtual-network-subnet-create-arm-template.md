---
title: Een virtueel netwerk maken
titleSuffix: Azure SQL Managed Instance
description: In dit artikel wordt beschreven hoe u een virtueel netwerk maakt dat is geconfigureerd ter ondersteuning van de implementatie van een door Azure SQL beheerd exemplaar.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 09/12/2019
ms.openlocfilehash: 923f8b447b1103284b2c999a981826ef19a1c7d8
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2020
ms.locfileid: "84050749"
---
# <a name="create-a-virtual-network-for-azure-sql-managed-instance"></a>Een virtueel netwerk maken voor Azure SQL Managed instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

In dit artikel wordt uitgelegd hoe u een geldig virtueel netwerk en subnet maakt waar u Azure SQL Managed Instance kunt implementeren.

Azure SQL Managed instance moet worden geïmplementeerd in een [virtueel](../../virtual-network/virtual-networks-overview.md)Azure-netwerk. Deze implementatie maakt gebruik van de volgende scenario's:

- Beveiligd privé IP-adres
- Rechtstreeks vanuit een on-premises netwerk verbinding maken met een beheerd SQL-exemplaar
- Een SQL Managed instance koppelen aan een gekoppelde server of een ander on-premises gegevens archief
- Een SQL-beheerd exemplaar verbinden met Azure-resources  

> [!NOTE]
> U moet [de grootte van het subnet voor SQL Managed instance bepalen](vnet-subnet-determine-size.md) voordat u de eerste instantie implementeert. U kunt het formaat van het subnet niet wijzigen nadat u de resources in hebt geplaatst.
>
> Als u van plan bent een bestaand virtueel netwerk te gebruiken, moet u die netwerk configuratie aanpassen aan uw door SQL beheerd exemplaar. Zie [een bestaand virtueel netwerk voor SQL Managed instance wijzigen](vnet-existing-add-subnet.md)voor meer informatie.
>
> Nadat een door SQL beheerd exemplaar is gemaakt, wordt het door SQL beheerde exemplaar of VNet naar een andere resource groep of een ander abonnement niet ondersteund.  Het is ook niet mogelijk om het beheerde exemplaar van SQL te verplaatsen naar een ander subnet.
>

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

De eenvoudigste manier om een virtueel netwerk te maken en te configureren, is door een sjabloon voor Azure Resource Manager implementatie te gebruiken.

1. Meld u aan bij Azure Portal.

2. Selecteer de knop **implementeren naar Azure** :

   <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="https://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

   Met deze knop opent u een formulier dat u kunt gebruiken om de netwerk omgeving te configureren waarin u het door SQL beheerde exemplaar kunt implementeren.

   > [!Note]
   > Met deze Azure Resource Manager sjabloon wordt een virtueel netwerk met twee subnetten geïmplementeerd. Een subnet met de naam **ManagedInstances**is gereserveerd voor het beheerde exemplaar van SQL en heeft een vooraf geconfigureerde route tabel. Het andere subnet, dat **standaard**wordt genoemd, wordt gebruikt voor andere resources die toegang moeten hebben tot SQL Managed instance (bijvoorbeeld Azure virtual machines).

3. Configureer de netwerk omgeving. Op het volgende formulier kunt u para meters van uw netwerk omgeving configureren:

   ![Resource Manager-sjabloon voor het configureren van het Azure-netwerk](./media/virtual-network-subnet-create-arm-template/create-mi-network-arm.png)

   U kunt de namen van het virtuele netwerk en subnetten wijzigen en de IP-bereiken aanpassen die aan uw netwerk bronnen zijn gekoppeld. Nadat u de knop **aanschaffen** hebt geselecteerd, wordt in dit formulier uw omgeving gemaakt en geconfigureerd. Als u niet twee subnetten nodig hebt, kunt u de standaard waarde verwijderen.

## <a name="next-steps"></a>Volgende stappen

- Zie [Wat is een SQL beheerd exemplaar?](sql-managed-instance-paas-overview.md)voor een overzicht.
- Meer informatie over [connectiviteits architectuur in SQL Managed instance](connectivity-architecture-overview.md).
- Meer informatie over het [wijzigen van een bestaand virtueel netwerk voor een door SQL beheerd exemplaar](vnet-existing-add-subnet.md).
- Voor een zelf studie waarin wordt getoond hoe u een virtueel netwerk maakt, een SQL Managed instance maakt en een Data Base herstelt vanuit een back-up van een Data Base, raadpleegt u [een Azure SQL Managed instance maken](instance-create-quickstart.md).
- Zie [Configuring a Custom DNS](custom-dns-configure.md)(Engelstalig) voor DNS-problemen.
