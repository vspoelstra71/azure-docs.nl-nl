---
title: Prijs informatie & het beheren van kosten
description: Biedt aanbevolen procedures voor het kiezen van het juiste SQL Server prijs model voor de virtuele machine.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/09/2018
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 9b9e98b4e1f5c286acb9089893f169f2c52b855c
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2020
ms.locfileid: "84047270"
---
# <a name="pricing-guidance-for-azure-sql-server-vms"></a>Prijsrichtlijnen voor Azure SQL Server-VM's
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

Dit artikel bevat richt lijnen voor de prijs informatie voor [SQL Server virtuele machines](sql-server-on-azure-vm-iaas-what-is-overview.md) in Azure. Er zijn verschillende opties die van invloed zijn op de kosten. het is belang rijk dat u de juiste afbeelding kiest die de kosten in balans brengt met bedrijfs vereisten.

> [!TIP]
> Als u alleen een kosten raming voor een specifieke combi natie van SQL Server editie en grootte van de virtuele machine nodig hebt, raadpleegt u de pagina met prijzen voor [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows) of [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux). Selecteer uw platform en SQL Server editie in de lijst met **besturings systemen en software** .
>
> ![Prijs pagina voor de gebruikers interface op de VM](./media/pricing-guidance/virtual-machines-pricing-ui.png)
>
> Of gebruik de [prijs calculator](https://azure.microsoft.com/pricing/#explore-cost) om een virtuele machine toe te voegen en te configureren. 

## <a name="free-licensed-sql-server-editions"></a>Gratis licentie SQL Server-edities

Als u een concept wilt ontwikkelen, testen of maken, gebruikt u de gratis licentie **SQL Server Developer Edition**. Deze editie heeft alle functies van SQL Server Enterprise Edition, zodat u elk type toepassing kunt bouwen en testen. U kunt de Developer Edition echter niet in productie uitvoeren. Een SQL Server Developer Edition-VM brengt alleen kosten in rekening voor de kosten van de virtuele machine, omdat er geen bijbehorende SQL Server licentie kosten zijn.

Als u een licht gewicht werk belasting wilt uitvoeren in productie (<4 kernen, <geheugen van 1 GB, <10 GB/data base), gebruikt u de gratis licentie **SQL Server Express Edition**. Een SQL Server Express Edition-VM maakt ook alleen kosten in rekening voor de kost prijs van de virtuele machine.

Voor deze werk belastingen voor ontwikkelen en testen en lichte productie kunt u ook geld besparen door een kleinere VM-grootte te kiezen die overeenkomt met deze werk belastingen. Het DS1v2 is mogelijk een goede keuze in sommige scenario's.

Zie de volgende koppelingen voor het maken van een SQL Server 2017 Azure VM met een van deze installatie kopieën:

| Platform | Installatie kopieën met gratis licenties |
|---|---|
| Windows Server 2016 | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonWindowsServer2016)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonWindowsServer2016) |
| Red Hat Enterprise Linux | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonRedHatEnterpriseLinux74) |
| SUSE Linux Enterprise Server | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonSLES12SP2)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonSLES12SP2) |
| Ubuntu | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonUbuntuServer1604LTS)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonUbuntuServer1604LTS) |

## <a name="paid-sql-server-editions"></a>Betaalde SQL Server-edities

Als u een niet-lichte productie werk belasting hebt, gebruikt u een van de volgende SQL Server-edities:

| SQL Server-editie | Workload |
|-----|-----|
| Web | Kleine websites |
| Standard | Kleine tot middel grote werk belastingen |
| Enterprise | Grote of bedrijfskritische workloads|

U hebt twee opties om te betalen voor SQL Server licenties voor deze edities: u *betaalt per gebruik* of *u kunt uw eigen licentie (BYOL) meenemen*.

## <a name="pay-per-usage"></a>Betalen per gebruik

Het **betalen van de SQL Server licentie per gebruik** (ook wel bekend als **u betalen tijdens**de hand) betekent dat de kosten per seconde voor het uitvoeren van de Azure-VM de kosten van de SQL Server licentie bevatten. U kunt de prijzen voor de verschillende SQL Server-edities (Web, Standard, Enter prise) zien op de pagina met prijzen voor Azure VM voor [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows) of [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux).

De kosten zijn hetzelfde voor alle versies van SQL Server (2012 SP3 tot 2017). De licentie kosten per seconde zijn afhankelijk van het aantal VM-Vcpu's.

U wordt aangeraden de SQL Server licenties per gebruik te betalen voor:

- **Tijdelijke of periodieke werk belastingen**. Een app die bijvoorbeeld elk jaar een gebeurtenis moet ondersteunen voor een paar maanden of bedrijfs analyse op maandag.

- **Workloads met een onbekende levens duur of schaal**. Een app die bijvoorbeeld niet in een paar maanden is vereist of waarvoor meer of minder reken kracht nodig is, afhankelijk van de vraag.

Ga als volgt te werk om een SQL Server 2017 Azure VM te maken met een van deze installatie kopieën met betalen per gebruik:

| Platform | Gelicentieerde installatie kopieën |
|---|---|
| Windows Server 2016 | [SQL Server 2017 Web Azure-VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonWindowsServer2016)<br/>[SQL Server 2017 standaard-VM van Azure](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonWindowsServer2016)<br/>[SQL Server 2017 Enter prise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseWindowsServer2016) |
| Red Hat Enterprise Linux | [SQL Server 2017 Web Azure-VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 standaard-VM van Azure](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 Enter prise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonRedHatEnterpriseLinux74) |
| SUSE Linux Enterprise Server | [SQL Server 2017 Web Azure-VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonSLES12SP2)<br/>[SQL Server 2017 standaard-VM van Azure](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonSLES12SP2)<br/>[SQL Server 2017 Enter prise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonSLES12SP2) |
| Ubuntu | [SQL Server 2017 Web Azure-VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonUbuntuServer1604LTS)<br/>[SQL Server 2017 standaard-VM van Azure](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonUbuntuServer1604LTS)<br/>[SQL Server 2017 Enter prise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonUbuntuServer1604LTS) |

> [!IMPORTANT]
> Wanneer u in de portal een SQL Server virtuele machine maakt, wordt in het venster **een grootte kiezen** de geschatte kosten weer gegeven. Het is belang rijk te weten dat deze schatting alleen de reken kosten voor het uitvoeren van de virtuele machine is, samen met eventuele licentie kosten van het besturings systeem (Windows of Linux-besturings systemen van derden).
>
> ![Blade VM-grootte kiezen](./media/pricing-guidance/sql-vm-choose-size-pricing-estimate.png)
>
>Het bevat geen aanvullende SQL Server licentie kosten voor web-, Standard-en Enter prise-edities. Als u de meest nauw keurige prijs schatting wilt krijgen, selecteert u uw besturings systeem en SQL Server editie op de pagina met prijzen voor [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) of [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

> [!NOTE]
> Het is nu mogelijk om het licentie model te wijzigen van betalen per gebruik om uw eigen licentie (BYOL) en terug te brengen. Zie [Het licentiemodel voor een SQL-VM wijzigen](licensing-model-azure-hybrid-benefit-ahb-change.md) voor meer informatie. 

## <a name="bring-your-own-license-byol"></a><a id="byol"></a>Bring Your Own License (BYOL)

Met behulp van een bestaande SQL Server volume licentie met Software Assurance in een Azure-VM kunt u **uw eigen SQL Server-licentie meenemen via License Mobility**, ook wel **BYOL**genoemd. Met een SQL Server-VM met BYOL worden alleen kosten in rekening gebracht voor de kosten van het uitvoeren van de virtuele machine, niet voor SQL Server licenties, gezien dat u al licenties en Software Assurance hebt aangeschaft via een volume licentie programma.

> [!IMPORTANT]
> Voor BYOL-installatie kopieën is een Enterprise Agreement met Software Assurance vereist. Ze zijn op dit moment niet beschikbaar als onderdeel van de Azure Cloud Solution partner (CSP). CSP-klanten kunnen hun eigen licentie gebruiken door een betalen per gebruik-installatie kopie te implementeren en vervolgens de [Azure Hybrid Benefit](licensing-model-azure-hybrid-benefit-ahb-change.md)in te scha kelen.

> [!NOTE]
> De BYOL-installatie kopieën zijn momenteel alleen beschikbaar voor virtuele Windows-machines. U kunt SQL Server echter hand matig installeren op een virtuele Linux-machine. Zie de richt lijnen in de [Veelgestelde vragen over Linux SQL-vm's](../linux/frequently-asked-questions-faq.md).

U wordt aangeraden uw eigen SQL-licentie te halen via License Mobility.

- **Doorlopende werk belastingen**. Bijvoorbeeld een app die de bedrijfs activiteiten 24x7 moet ondersteunen.

- **Werk belastingen met een bekende levens duur en schaal**. Een voor beeld: een app die is vereist voor het hele jaar en welke vraag is geforecastd.

Als u BYOL wilt gebruiken met een SQL Server-VM, moet u een licentie hebben voor SQL Server Standard of ENTER prise en [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default.aspx#tab=1), een vereiste optie via sommige volume licentie Programma's en een optionele aankoop met anderen. Het prijs niveau dat door de volume licentie Programma's wordt gegeven, is afhankelijk van het type overeenkomst en het aantal en of de toezeg ging naar SQL Server. Maar als vuist regel biedt uw eigen licentie voor continue productie werkbelastingen de volgende voor delen:

| BYOL voor deel | Beschrijving |
|-----|-----|
| **Kostenbesparingen** | De [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/) biedt een besparing van maxi maal 55%. Zie voor meer informatie [switching licentie model](licensing-model-azure-hybrid-benefit-ahb-change.md) |
| **Gratis passieve secundaire replica** | Een ander voor deel van het meebrengen van uw eigen licentie is de [gratis licentie voor één passieve secundaire replica](https://azure.microsoft.com/pricing/licensing-faq/) per SQL Server voor hoge Beschik baarheid. Dit is de helft van de licentie kosten van een Maxi maal beschik bare SQL Server implementatie (bijvoorbeeld met behulp van AlwaysOn-beschikbaarheids groepen). De rechten om de passieve secundaire uit te voeren, worden verschaft via de Software Assurance-voor delen van failover-servers. |

Voor het maken van een SQL Server 2017 Azure VM met een van deze kopieën van uw eigen licentie, raadpleegt u de Vm's die worden voorafgegaan door {BYOL}:

- [SQL Server 2017 Enter prise Azure VM](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017EnterpriseWindowsServer2016)
- [SQL Server 2017 standaard-VM van Azure](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017StandardonWindowsServer2016)

> [!IMPORTANT]
> Laat ons binnen tien dagen weten hoeveel SQL Server-licenties u in azure gebruikt. De koppelingen naar de vorige installatie kopieën bevatten instructies over hoe u dit doet.

> [!NOTE]
> Het is nu mogelijk om het licentie model te wijzigen van betalen per gebruik om uw eigen licentie (BYOL) en terug te brengen. Zie [Het licentiemodel voor een SQL-VM wijzigen](licensing-model-azure-hybrid-benefit-ahb-change.md) voor meer informatie. 



## <a name="reduce-costs"></a>Kosten verlagen

Om onnodige kosten te voor komen, kiest u een optimale grootte van de virtuele machine en overweegt u af en toe afsluiten voor niet-continue werk belastingen.

### <a name="correctly-size-your-vm"></a><a id="machinesize"></a>De grootte van uw virtuele machine correct aanpassen

De licentie kosten van SQL Server zijn direct gerelateerd aan het aantal Vcpu's. Kies een VM-grootte die overeenkomt met de verwachte behoeften voor CPU, geheugen, opslag en I/O-band breedte. Zie [Windows VM-grootten](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) en- [VM-grootten](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)voor een volledige lijst met opties voor de computer grootte.

Er zijn nieuwe machine grootten die geschikt zijn voor bepaalde soorten SQL Server werk belastingen. Deze computer grootten zorgen voor grote hoeveel heden geheugen, opslag ruimte en I/O-band breedte, maar ze hebben een lager gevirtualiseerde aantal gevirtualiseerd. Bekijk bijvoorbeeld het volgende voor beeld:

| VM-grootte | vCPUs | Geheugen | Maximum aantal schijven | Maximale I/O-door Voer | SQL-licentie kosten | Totale kosten (Compute + licentie verlening) |
|---|---|---|---|---|---|---|
| **Standard_DS14v2** | 16 | 112 GB | 32 | 51.200 IOP's of 768 MB/s | | |
| **Standard_DS14-4v2** | 4 | 112 GB | 32 | 51.200 IOP's of 768 MB/s | 75% lager | 57% lager |

> [!IMPORTANT]
> Dit is een voor beeld van een bepaald tijd punt. Raadpleeg het artikel over de computer grootte en de Azure-pagina met prijzen voor [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) en [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)voor de meest recente specificaties.

In het vorige voor beeld ziet u dat de specificaties voor **Standard_DS14v2** en **Standard_DS14-4v2** identiek zijn, met uitzonde ring van vcpu's. Het achtervoegsel **-4v2** aan het einde van de **Standard_DS14-4v2-** computer grootte geeft het aantal actieve vcpu's aan. Omdat SQL Server licentie kosten zijn gekoppeld aan het aantal Vcpu's, vermindert dit de kosten van de virtuele machine aanzienlijk in scenario's waarin de extra Vcpu's niet nodig zijn. Dit is een voor beeld en er zijn veel computer grootten met beperkte Vcpu's die met dit achtervoegsel patroon worden geïdentificeerd. Zie voor meer informatie het blog bericht [kondigt nieuwe Azure VM-grootten aan voor meer rendabele data base-werk](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/).

### <a name="shut-down-your-vm-when-possible"></a>Uw VM indien mogelijk afsluiten

Als u werk belastingen gebruikt die niet continu worden uitgevoerd, kunt u overwegen de virtuele machine tijdens de inactieve Peri Oden af te sluiten. U betaalt alleen voor wat u gebruikt.

Als u bijvoorbeeld gewoon probeert SQL Server op een virtuele machine van Azure, wilt u geen kosten in rekening brengen door per ongeluk de verwerking uit te voeren voor weken. Eén oplossing is het gebruik van de [functie voor automatisch afsluiten](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![SQL VM automatisch afsluiten](./media/pricing-guidance/sql-vm-auto-shutdown.png)

Automatisch afsluiten maakt deel uit van een groter aantal vergelijk bare functies van [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab).

Voor andere werk stromen kunt u overwegen om Azure-Vm's automatisch af te sluiten en opnieuw te starten met een script oplossing, zoals [Azure Automation](https://azure.microsoft.com/services/automation/).

> [!IMPORTANT]
> U kunt de virtuele machine niet afsluiten en de toewijzing ongedaan te zetten is de enige manier om kosten te voor komen. U kunt energie beheer stoppen of gebruiken om de virtuele machine af te sluiten van de gebruiks kosten.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de prijzen van Azure [onverwachte kosten voor komen bij facturering en kosten beheer van Azure](../../../cost-management-billing/manage/getting-started.md). Voor de nieuwste Virtual Machines prijzen, waaronder SQL Server, raadpleegt u de Azure VM Azure-pagina met prijzen voor virtuele [Windows-machines](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) en [Linux-vm's](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

Raadpleeg de volgende artikelen voor een overzicht van SQL Server die worden uitgevoerd op Azure Virtual Machines:

- [Overzicht van SQL Server op Windows-Vm's](sql-server-on-azure-vm-iaas-what-is-overview.md)
- [Overzicht van SQL Server on Linux Vm's](../linux/sql-server-on-linux-vm-what-is-iaas-overview.md)
