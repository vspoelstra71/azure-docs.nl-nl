---
title: Veelgestelde vragen over Windows Virtual Machines in Azure SQL Server | Microsoft Docs
description: In dit artikel vindt u antwoorden op veelgestelde vragen over het uitvoeren van SQL Server op Azure-Vm's.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: felixwu
editor: ''
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/05/2019
ms.author: mathoma
ms.openlocfilehash: 278f3a5109e638530a55f4b2a77cd6d28aa7ca54
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2020
ms.locfileid: "84047949"
---
# <a name="frequently-asked-questions-for-sql-server-running-on-windows-virtual-machines-in-azure"></a>Veelgestelde vragen over SQL Server op virtuele Windows-machines in Azure
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

> [!div class="op_single_selector"]
> * [Windows](frequently-asked-questions-faq.md)
> * [Linux](../linux/frequently-asked-questions-faq.md)

In dit artikel vindt u antwoorden op enkele van de meest voorkomende vragen over het uitvoeren van [SQL Server op Windows virtual machines in azure](https://azure.microsoft.com/services/virtual-machines/sql-server/).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="images"></a><a id="images"></a>Installatie kopieën

1. **Wat SQL Server afbeeldingen van de galerie met virtuele machines zijn beschikbaar?** 

   Azure beheert installatie kopieën van virtuele machines voor alle ondersteunde grote releases van SQL Server op alle edities van Windows en Linux. Zie de volledige lijst met [Windows VM-installatie](sql-server-on-azure-vm-iaas-what-is-overview.md#payasyougo) kopieën en [Linux VM-installatie kopieën](../linux/sql-server-on-linux-vm-what-is-iaas-overview.md#create)voor meer informatie.

1. **Zijn bestaande SQL Server afbeeldingen van de galerie met virtuele machines bijgewerkt?**

   SQL Server installatie kopieën in de galerie met virtuele machines worden elke twee maanden bijgewerkt met de nieuwste updates voor Windows en Linux. Voor Windows-installatie kopieën omvat dit alle updates die zijn gemarkeerd als belang rijk in Windows Update, met inbegrip van belang rijke SQL Server beveiligings updates en-service packs. Voor Linux-installatie kopieën bevat dit de meest recente systeem updates. SQL Server cumulatieve updates worden anders afgehandeld voor Linux en Windows. Voor Linux worden SQL Server cumulatieve updates ook opgenomen in het vernieuwen. Op dit moment worden Windows-Vm's niet bijgewerkt met SQL Server of cumulatieve updates voor Windows Server.

1. **Kunnen SQL Server installatie kopieën van virtuele machines worden verwijderd uit de galerie?**

   Ja. In azure wordt slechts één installatie kopie per primaire versie en editie bewaard. Wanneer bijvoorbeeld een nieuw SQL Server Service Pack wordt uitgebracht, voegt Azure een nieuwe installatie kopie toe aan de galerie voor dat Service Pack. De SQL Server-installatie kopie voor het vorige Service Pack wordt direct verwijderd uit de Azure Portal. Het is echter nog steeds beschikbaar voor het inrichten van Power shell voor de volgende drie maanden. Na drie maanden is de vorige installatie kopie van het Service Pack niet meer beschikbaar. Dit verwijderings beleid is ook van toepassing als een SQL Server-versie niet wordt ondersteund wanneer het einde van de levens cyclus wordt bereikt.


1. **Is het mogelijk om een oudere installatie kopie te implementeren van SQL Server die niet zichtbaar is in de Azure Portal?**

   Ja, met behulp van Power shell. Zie [SQL Server virtuele machines inrichten met Azure PowerShell](create-sql-vm-powershell.md)voor meer informatie over het implementeren van SQL Server-vm's met behulp van Power shell.
   
1. **Is het mogelijk om een gegeneraliseerde Azure SQL Server Marketplace-installatie kopie van mijn SQL Server VM te maken en deze te gebruiken om Vm's te implementeren?**

   Ja, maar u moet [elke SQL Server virtuele machine registreren bij de resource provider SQL Server VM](sql-vm-resource-provider-register.md) om uw SQL Server-VM in de portal te beheren, evenals functies als automatische patches en automatisch maken van back-ups. Wanneer u zich registreert bij de resource provider, moet u ook het licentie type opgeven voor elke SQL Server VM.

1. **Hoe kan ik generaliseer SQL Server op Azure VM en gebruiken om nieuwe virtuele machines te implementeren?**

   U kunt een Windows Server-VM implementeren (zonder SQL Server geïnstalleerd) en het [SQL Sysprep](/sql/database-engine/install-windows/install-sql-server-using-sysprep?view=sql-server-ver15) -proces gebruiken om SQL Server op Azure VM (Windows) met de SQL Server installatie media te generaliseren. Klanten die beschikken over [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?rtc=1&activetab=software-assurance-default-pivot%3aprimaryr3) kunnen de installatiemedia downloaden van het [Volume Licensing Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx). Klanten die geen Software Assurance hebben, kunnen het installatie medium van een Marketplace SQL Server VM-installatie kopie met de gewenste versie gebruiken.

   U kunt ook een van de SQL Server-installatie kopieën gebruiken Azure Marketplace om te generaliseren SQL Server op Azure VM. Houd er rekening mee dat u de volgende register sleutel in de bron installatie kopie moet verwijderen voordat u uw eigen installatie kopie maakt. Als u dit niet doet, kan dit ertoe leiden dat de SQL Server Setup Boots trap-map en/of de SQL IaaS-extensie in de status mislukt.

   Pad naar register sleutel:  
   `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Setup\SysPrepExternal\Specialize`

   > [!NOTE]
   > SQL Server op virtuele machines van Azure, met inbegrip van de Vm's die zijn geïmplementeerd vanuit aangepaste gegeneraliseerde installatie kopieën, moeten worden [geregistreerd bij de resource provider van de SQL-VM](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-register-with-resource-provider?tabs=azure-cli%2Cbash) om te voldoen aan de nalevings vereisten en om optionele functies te gebruiken, zoals automatische patches en automatische back-ups. De resource provider stelt u ook in staat [het licentie type](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-ahb?tabs=azure-portal) voor elke SQL Server virtuele machine op te geven.

1. **Kan ik mijn eigen VHD gebruiken om een SQL Server virtuele machine te implementeren?**

   Ja, maar u moet [elke SQL Server virtuele machine registreren bij de resource provider SQL Server VM](sql-vm-resource-provider-register.md) om uw SQL Server-VM in de portal te beheren, evenals functies als automatische patches en automatisch maken van back-ups.

1. **Is het mogelijk configuraties in te stellen die niet worden weer gegeven in de galerie met virtuele machines (bijvoorbeeld Windows 2008 R2 + SQL Server 2012)?**

   Nee. Voor installatie kopieën in de galerie met virtuele machines die SQL Server bevatten, moet u een van de installatie kopieën selecteren via de Azure Portal of via [Power shell](create-sql-vm-powershell.md). U hebt echter de mogelijkheid om een Windows-VM te implementeren en zelf SQL Server te installeren. U moet [uw SQL Server-VM vervolgens registreren bij de resource provider van SQL Server VM](sql-vm-resource-provider-register.md) om uw SQL Server-VM in de portal te beheren, evenals functies zoals automatische patches en automatische back-ups. 


## <a name="creation"></a>Maken

1. **Hoe kan ik een virtuele machine van Azure met SQL Server maken?**

   De eenvoudigste methode is het maken van een virtuele machine die SQL Server bevat. Zie [een SQL Server virtuele machine inrichten in de Azure Portal](create-sql-vm-portal.md)voor een zelf studie over het aanmelden voor Azure en het maken van een SQL Server-VM vanuit de portal. U kunt een installatie kopie van een virtuele machine selecteren die gebruikmaakt van betalen per seconde SQL Server-licentie verlening, of u kunt een installatie kopie gebruiken waarmee u uw eigen SQL Server-licentie meebrengt. U hebt ook de mogelijkheid om SQL Server hand matig te installeren op een virtuele machine met een gratis licentie (ontwikkelaar of Express) of door een on-premises licentie opnieuw te gebruiken. Zorg ervoor dat u [uw SQL Server-VM registreert bij de SQL Server VM-resource provider](sql-vm-resource-provider-register.md) om uw SQL Server-VM in de portal te beheren, en gebruik functies zoals automatische patching en automatische back-ups. Als u uw eigen licentie meebrengt, moet u beschikken over [License Mobility via Software Assurance op Azure](https://azure.microsoft.com/pricing/license-mobility/). Zie [Pricing guidance for SQL Server Azure VMs](pricing-guidance.md) (Prijsrichtlijnen voor SQL Server Azure VM's) voor meer informatie.

1. **Hoe kan ik mijn on-premises SQL Server Data Base naar de Cloud migreren?**

   Maak eerst een virtuele Azure-machine met een SQL Server-exemplaar. Migreer vervolgens uw on-premises data bases naar dat exemplaar. Zie [een SQL Server-Data Base migreren naar SQL Server in een Azure-VM](migrate-to-vm-from-sql-server.md)voor strategieën voor gegevens migratie.

## <a name="licensing"></a>Licentieverlening

1. **Hoe kan ik mijn gelicentieerd exemplaar van SQL Server installeren op een Azure-VM?**

   Er zijn drie manieren om dit te doen. Als u een Enter prise Agreement (EA)-klant bent, kunt u een van de [installatie kopieën van virtuele machines inrichten die ondersteuning bieden voor licenties](sql-server-on-azure-vm-iaas-what-is-overview.md#BYOL), ook wel bekend als uw eigen licentie (BYOL). Als u [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default)hebt, kunt u de [Azure Hybrid Benefit](licensing-model-azure-hybrid-benefit-ahb-change.md) inschakelen op een bestaande payg-installatie kopie (betalen per gebruik). Of u kunt de SQL Server-installatiemedia kopiëren naar een Windows Server-VM, en vervolgens SQL Server op de VM installeren. Zorg ervoor dat u uw SQL Server-VM registreert bij de [resource provider](sql-vm-resource-provider-register.md) voor functies zoals portal beheer, geautomatiseerde back-up en automatische patching. 

1. **Kan ik een VM wijzigen zodat mijn eigen SQL Server-licentie wordt gebruikt, wanneer de VM is gemaakt vanuit een van de Betalen per gebruik-installatiekopieën uit de galerie?**

   Ja. U kunt eenvoudig een PAYG-galerie (betalen naar gebruik) overschakelen naar uw eigen licentie (BYOL) door de [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/faq/)in te scha kelen.  Zie [How to Change the License model for a SQL Server VM](licensing-model-azure-hybrid-benefit-ahb-change.md)(Engelstalig) voor meer informatie. Deze faciliteit is momenteel alleen beschikbaar voor open bare en Azure Government Cloud-klanten.

1. **Treedt er downtime op voor SQL Server tijdens het schakelen tussen licentiemodellen?**

   Nee. [Het wijzigen van het licentie model](licensing-model-azure-hybrid-benefit-ahb-change.md) vereist geen uitval tijd voor SQL Server, omdat de wijziging onmiddellijk van kracht is en het opnieuw opstarten van de virtuele machine niet vereist is. Als u uw SQL Server virtuele machine echter wilt registreren bij de resource provider van SQL Server VM, is de [SQL IaaS-uitbrei ding](sql-server-iaas-agent-extension-automate-management.md) een vereiste en installeert u de SQL IaaS-extensie in de _volledige_ modus. Start de SQL Server-service opnieuw. Als de SQL IaaS-extensie moet worden geïnstalleerd, installeert u deze in de modus _Lightweight_ voor beperkte functionaliteit of installeert u deze in de _volledige_ modus tijdens een onderhouds venster. De SQL IaaS-uitbrei ding die is geïnstalleerd in de _licht_ modus kan op elk gewenst moment worden bijgewerkt naar de _volledige_ modus, maar vereist dat de SQL Server-service opnieuw wordt gestart. 
   
1. **Is het mogelijk om tussen licentiemodellen te schakelen op een SQL Server-VM die is geïmplementeerd volgens het klassieke model?**

   Nee. Het wijzigen van licentiemodellen wordt niet ondersteund op een klassieke VM. U kunt de VM migreren naar het Azure Resource Manager-model en registreren bij de SQL Server-VM-resourceprovider. Zodra de VM is geregistreerd bij de SQL Server-VM-resourceprovider, zijn wijzigingen in het licentiemodel beschikbaar op de VM.

1. **Kan ik de Azure-portal gebruiken om meerdere exemplaren op dezelfde VM te beheren?**

   Nee. Portalbeheer is een functie van de SQL Server-VM-resourceprovider, die afhankelijk is van de SQL Server IaaS-agentextensie. Als zodanig gelden dezelfde beperkingen voor de resourceprovider als voor de extensie. In de portal kan slechts één standaardexemplaar of één benoemd exemplaar worden beheerd, zolang het op de juiste wijze is geconfigureerd. Zie [SQL Server IaaS agent extension](sql-server-iaas-agent-extension-automate-management.md)(Engelstalig) voor meer informatie over deze beperkingen. 

1. **Kan Azure Hybrid Benefit worden geactiveerd met een CSP-abonnement?**

   Ja, Azure Hybrid Benefit is beschikbaar voor CSP-abonnementen. CSP-klanten moeten eerst een betalen per gebruik-installatie kopie implementeren en vervolgens [het licentie model wijzigen](licensing-model-azure-hybrid-benefit-ahb-change.md) in uw eigen licentie.
   
 
1. **Moet ik betalen om een SQL-server een licentie te verlenen op een Azure-VM, als deze SQL-server alleen wordt gebruikt als stand-by of om een failover uit te voeren?**

   Als u een gratis passieve licentie wilt voor een secundaire beschikbaarheids groep met een stand-by-of een failover-cluster, moet u voldoen aan de volgende criteria, zoals beschreven in de [product licentie voorwaarden](https://www.microsoft.com/licensing/product-licensing/products):

   1. U hebt [licentie mobiliteit](https://www.microsoft.com/licensing/licensing-programs/software-assurance-license-mobility?activetab=software-assurance-license-mobility-pivot:primaryr2) via [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?activetab=software-assurance-default-pivot%3aprimaryr3). 
   1. Het passieve SQL Server-exemplaar levert geen SQL Server-gegevens aan clients en voert geen actieve SQL Server-workloads uit. Dit exemplaar wordt alleen gebruikt om te synchroniseren met de primaire server, en behoudt de passieve database verder in een warme stand-by. Als het gaat om gegevens, zoals rapporten naar clients waarop actieve SQL Server werk belastingen worden uitgevoerd, of het uitvoeren van andere werk items dan is opgegeven in de product voorwaarden, moet het een betaalde gelicentieerde SQL Server instantie zijn. De volgende activiteiten zijn toegestaan op het secundaire exemplaar: consistentiecontroles voor databases of CheckDB, volledige back-ups, back-ups van transactielogboeken, en het bewaken van gegevens over resourcegebruik. U kunt het primaire exemplaar en het bijbehorende exemplaar voor herstel na een noodgeval ook één keer in de 90 dagen gedurende korte tijd tegelijk uitvoeren, om herstel na noodgeval te testen. 
   1. De actieve SQL Server-licentie wordt gedekt door Software Assurance en biedt **één** passieve secundaire SQL Server instantie, met Maxi maal dezelfde hoeveelheid reken kracht als de gelicentieerde actieve server. 
   1. De secundaire SQL Server VM maakt gebruik van de licentie voor [herstel na nood gevallen](business-continuity-high-availability-disaster-recovery-hadr-overview.md#free-dr-replica-in-azure) in de Azure Portal.
   
1. **Wat wordt beschouwd als een passief exemplaar?**

   Het passieve SQL Server-exemplaar levert geen SQL Server-gegevens aan clients en voert geen actieve SQL Server-workloads uit. Dit exemplaar wordt alleen gebruikt om te synchroniseren met de primaire server, en behoudt de passieve database verder in een warme stand-by. Als een exemplaar gegevens, zoals rapporten, levert aan clients waarop actieve SQL Server-workloads worden uitgevoerd, of als het andere werkzaamheden uitvoert dan de werkzaamheden die zijn opgegeven in de productvoorwaarden, moet het een betaald gelicentieerd SQL Server-exemplaar zijn. De volgende activiteiten zijn toegestaan op het secundaire exemplaar: consistentiecontroles voor databases of CheckDB, volledige back-ups, back-ups van transactielogboeken, en het bewaken van gegevens over resourcegebruik. U kunt het primaire exemplaar en het bijbehorende exemplaar voor herstel na een noodgeval ook één keer in de 90 dagen gedurende korte tijd tegelijk uitvoeren, om herstel na noodgeval te testen.
   

1. **Welke scenario's kunnen gebruikmaken van Disaster Recovery Benefit ?**

   De [licentie handleiding](https://aka.ms/sql2019licenseguide) bevat scenario's waarin het voor deel van herstel na nood gevallen kan worden gebruikt. Raadpleeg de productvoorwaarden en neem contact op met uw licentiecontactpersoon of accountmanager voor meer informatie.

1. **Welke abonnementen bieden ondersteuning voor Disaster Recovery Benefit?**

   Uitgebreide programma's die abonnementsrechten bieden die vergelijkbaar zijn met die van Software Assurance als een vast voordeel, bieden ondersteuning voor Disaster Recovery Benefit. Dit is inclusief, maar niet beperkt tot: OV (Open Value), OVS (Open Value Subscription), EA (Enterprise Agreement), EAS (Enterprise Subscription Agreement), en SCE (Server and Cloud Enrollment). Raadpleeg de [product voorwaarden](https://www.microsoft.com/licensing/product-licensing/products) en neem contact op met uw licentie contactpersonen of account manager voor meer informatie. 

   
 ## <a name="resource-provider"></a>Resourceprovider

1. **Gaat mijn VM registreren met de nieuwe provider van SQL Server VM-resource, worden extra kosten in rekening gebracht?**

   Nee. De resource provider van de SQL Server-VM biedt alleen extra beheer baarheid voor SQL Server op Azure VM zonder extra kosten. 

1. **Is de resource provider voor de SQL Server-VM beschikbaar voor alle klanten?**
 
   Ja, zolang de SQL Server virtuele machine is geïmplementeerd in de open bare Cloud met behulp van het Resource Manager-model en niet het klassieke model. Alle andere klanten kunnen zich registreren bij de nieuwe SQL Server VM-resource provider. Maar alleen klanten met het voor deel van [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?activetab=software-assurance-default-pivot%3aprimaryr3) kunnen hun eigen licentie gebruiken door de [Azure Hybrid Benefit (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/) te activeren op een SQL Server VM. 

1. **Wat gebeurt er met de resource provider (_micro soft. SqlVirtualMachine_) als de VM-resource wordt verplaatst of verwijderd?** 

   Wanneer de micro soft. Compute/VirtualMachine-resource wordt verwijderd of verplaatst, wordt de bijbehorende micro soft. SqlVirtualMachine-resource op de hoogte gesteld om de bewerking asynchroon te repliceren.

1. **Wat gebeurt er met de virtuele machine als de bron provider (_micro soft. SqlVirtualMachine_) wordt verwijderd?**

    De resource micro soft. Compute/VirtualMachine wordt niet beïnvloed wanneer de micro soft. SqlVirtualMachine-resource wordt verwijderd. De licentie wijzigingen worden echter standaard teruggezet naar de oorspronkelijke installatie kopie bron. 

1. **Is het mogelijk om zelf-geïmplementeerde SQL Server Vm's te registreren bij de resource provider van SQL Server VM?**

    Ja. Als u SQL Server vanaf uw eigen media hebt geïmplementeerd en de SQL IaaS-extensie hebt geïnstalleerd, kunt u uw SQL Server VM registreren bij de resource provider om de beheer baarheid van de SQL IaaS-uitbrei ding te verkrijgen.    


## <a name="administration"></a>Beheer

1. **Kan ik een tweede exemplaar van SQL Server op dezelfde VM installeren? Kan ik de geïnstalleerde functies van het standaard exemplaar wijzigen?**

   Ja. De SQL Server-installatie media bevindt zich in een map op station **C** . Voer **Setup. exe** vanaf die locatie uit om nieuwe exemplaren van SQL Server toe te voegen of om andere geïnstalleerde functies van SQL Server op de computer te wijzigen. Houd er rekening mee dat sommige functies, zoals geautomatiseerde back-ups, automatische patching en Azure Key Vault integratie, alleen kunnen worden uitgevoerd op het standaard exemplaar of een benoemd exemplaar dat correct is geconfigureerd (zie vraag 3). 

1. **Kan ik het standaardexemplaar van SQL Server verwijderen?**

   Ja, maar er is een aantal overwegingen. SQL Server-gerelateerde facturering kan eerst worden uitgevoerd, afhankelijk van het licentie model voor de virtuele machine. Ten tweede, zoals vermeld in het vorige antwoord, zijn er functies die afhankelijk zijn van de [SQL Server IaaS agent-extensie](sql-server-iaas-agent-extension-automate-management.md). Als u het standaard exemplaar verwijdert zonder de IaaS-extensie te verwijderen, blijft de uitbrei ding zoeken naar het standaard exemplaar en kunnen fouten in het gebeurtenis logboek worden gegenereerd. Deze fouten zijn afkomstig uit de volgende twee bronnen: **Microsoft SQL Server referentie beheer** en **Microsoft SQL Server IaaS-agent**. Een van de fouten lijkt mogelijk op de volgende:

      Een netwerkgerelateerde of exemplaarspecifieke fout is opgetreden bij het maken van een verbinding met SQL Server. De server wordt niet gevonden of toegang tot de server is niet mogelijk.

   Als u besluit om het standaard exemplaar te verwijderen, moet u ook de [uitbrei ding voor de SQL Server IaaS-agent](sql-server-iaas-agent-extension-automate-management.md) verwijderen. 

1. **Kan ik een benoemd exemplaar van SQL Server met de extensie IaaS gebruiken**?
   
   Ja, als het benoemde exemplaar het enige exemplaar op het SQL Server is, en als het oorspronkelijke standaard exemplaar [correct is verwijderd](sql-server-iaas-agent-extension-automate-management.md#install-on-a-vm-with-a-single-named-sql-server-instance). Als er geen standaard exemplaar is en er meerdere benoemde exemplaren op één SQL Server virtuele machine staan, kan de uitbrei ding van de SQL Server IaaS-agent niet worden geïnstalleerd. 

1. **Kan ik SQL Server volledig verwijderen van VM met SQL Server?**

   Ja, maar er worden nog steeds kosten in rekening gebracht voor uw SQL Server virtuele machine, zoals beschreven in [prijs informatie voor SQL Server virtuele machines van Azure](pricing-guidance.md). Als u de SQL Server niet langer nodig hebt, kunt u een nieuwe virtuele machine implementeren en de gegevens en toepassingen migreren naar de nieuwe virtuele machine. Vervolgens kunt u de virtuele SQL Server-machine verwijderen.
   
## <a name="updating-and-patching"></a>Updates en patches

1. **Hoe kan ik overschakelen naar een andere versie of editie van de SQL Server in een Azure-VM?**

   Klanten kunnen hun versie of editie van SQL Server wijzigen door gebruik te maken van een installatiemedium met de gewenste versie of editie van SQL Server. Zodra de versie of editie is gewijzigd, gebruikt u de Azure-portal om de versie- of editie-eigenschap van de VM te wijzigen zodat deze nauwkeurig de facturering voor de VM aangeeft. Zie [Change Edition of a SQL Server VM](change-sql-server-edition.md)(Engelstalig) voor meer informatie. Er is geen factureringsverschil voor de verschillende versies van SQL Server, dus wanneer de versie van SQL Server is gewijzigd, is er geen verdere actie nodig.

1. **Waar kan ik de installatiemedia downloaden om de editie of versie van SQL Server te wijzigen?**

   Klanten die beschikken over [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) kunnen de installatiemedia downloaden van het [Volume Licensing Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx). Klanten die geen Software Assurance hebben, kunnen de installatiemedia gebruiken door een SQL Server VM-installatiekopie met de gewenste versie te downloaden van Marketplace.
   
1. **Hoe worden updates en servicepacks toegepast op een VM met SQL Server?**

   virtuele machines geven u controle over de hostmachine, inclusief wanneer en hoe u updates wilt toepassen. U kunt handmatig de Windows-updates toepassen voor het besturingssysteem of u kunt de planningsservice [Automated Patching](automated-patching.md) inschakelen. Automated Patching installeert alle updates die als belangrijk zijn aangeduid, met inbegrip van SQL Server-updates in deze categorie. Overige optionele updates voor SQL Server moeten handmatig worden geïnstalleerd.

1. **Kan ik een upgrade uitvoeren van mijn SQL Server 2008/2008 R2-exemplaar nadat ik deze heb geregistreerd bij de resource provider van SQL Server VM?**

   Ja. U kunt elk installatie medium gebruiken om de versie en editie van SQL Server te upgraden, en vervolgens kunt u uw [SQL IaaS-uitbreidings modus](sql-vm-resource-provider-register.md#management-modes)upgraden van _geen enkele agent_ naar een _volle_. Op die manier krijgt u toegang tot alle voor delen van de SQL IaaS-extensie, zoals beheer baarheid van de portal, automatische back-ups en automatische patching. 

1. **Hoe krijg ik gratis uitgebreide beveiligingsupdates voor mijn end-of-support exemplaren van SQL Server 2008 en SQL Server 2008 R2?**

   U kunt in het bezit komen van [gratis uitgebreide beveiligingsupdates](sql-server-2008-extend-end-of-support.md) door uw SQL Server ongewijzigd te verplaatsen naar een virtuele machine van Azure SQL. Zie [de opties voor end-of-support](/sql/sql-server/end-of-support/sql-server-end-of-life-overview) voor meer informatie. 
  
   

## <a name="general"></a>Algemeen

1. **Worden SQL Server-FCI (failover cluster instances) ondersteund op virtuele machines van Azure?**

   Ja. U kunt een failover-cluster exemplaar installeren met behulp van [Premium-bestands shares (PFS)](failover-cluster-instance-premium-file-share-manually-configure.md) of [opslag ruimten direct (S2D)](failover-cluster-instance-storage-spaces-direct-manually-configure.md) voor het opslag subsysteem. Premium-bestands shares bieden IOPS en doorvoer capaciteit die voldoet aan de behoeften van veel werk belastingen. Voor i/o-intensieve workloads kunt u opslag ruimten direct gebruiken op basis van beheerd Premium of Ultra-disks. U kunt ook clustering-of opslag oplossingen van derden gebruiken, zoals beschreven in [hoge Beschik baarheid en herstel na nood gevallen voor SQL Server in Azure virtual machines](business-continuity-high-availability-disaster-recovery-hadr-overview.md#azure-only-high-availability-solutions).

   > [!IMPORTANT]
   > Op dit moment wordt de _volledige_ [SQL Server IaaS-agent extensie](sql-server-iaas-agent-extension-automate-management.md) niet ondersteund voor SQL Server FCI op Azure. U wordt aangeraden de _volledige_ uitbrei ding van vm's die deel uitmaken van de FCI te verwijderen en de uitbrei ding in de _licht_ modus te installeren. Deze uitbrei ding ondersteunt functies, zoals automatische back-ups en patches en bepaalde portal functies voor SQL Server. Deze functies werken niet voor SQL Server Vm's nadat de _volledige_ agent is verwijderd.

1. **Wat is het verschil tussen SQL Server Vm's en de SQL Database-Service?**

   In het algemeen is het uitvoeren van SQL Server op een virtuele machine van Azure niet hetzelfde als het uitvoeren van SQL Server in een extern Data Center. [SQL database](../../database/sql-database-paas-overview.md) biedt daarentegen data base-as-a-service. Met SQL Database hebt u geen toegang tot de computers die als host fungeren voor uw data bases. Zie voor een volledige vergelijking [kiezen een cloud SQL Server optie: Azure SQL (PaaS) data base of SQL Server op Azure vm's (IaaS)](../../azure-sql-iaas-vs-paas-what-is-overview.md).

1. **Hoe kan ik SQL data-hulpprogram ma's installeren op mijn Azure VM?**

    Down load en installeer de hulpprogram ma's voor SQL-gegevens van [Microsoft SQL Server Data tools-Business Intelligence voor Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313).

1. **Worden gedistribueerde trans acties met MSDTC ondersteund op SQL Server Vm's?**
   
    Ja. Lokale DTC wordt ondersteund voor SQL Server 2016 SP2 en hoger. Toepassingen moeten echter worden getest wanneer er gebruik wordt gemaakt van AlwaysOn-beschikbaarheids groepen, omdat trans acties in de vlucht tijdens een failover mislukken en opnieuw moeten worden geprobeerd. Geclusterde DTC is beschikbaar vanaf Windows Server 2019. 

## <a name="resources"></a>Resources

**Windows-vm's**:

* [Overzicht van SQL Server op een Windows-VM](sql-server-on-azure-vm-iaas-what-is-overview.md).
* [Een SQL Server Windows-VM inrichten](create-sql-vm-portal.md)
* [Een Data Base migreren naar SQL Server op een virtuele Azure-machine](migrate-to-vm-from-sql-server.md)
* [Hoge Beschik baarheid en herstel na nood gevallen voor SQL Server in azure Virtual Machines](business-continuity-high-availability-disaster-recovery-hadr-overview.md)
* [Aanbevolen procedures voor prestaties voor SQL Server op virtuele machines van Azure](performance-guidelines-best-practices.md)
* [Toepassings patronen en ontwikkelings strategieën voor SQL Server in azure Virtual Machines](application-patterns-development-strategies.md)

**Virtuele Linux-machines**:

* [Overzicht van SQL Server op een Linux-VM](../linux/sql-server-on-linux-vm-what-is-iaas-overview.md)
* [Een SQL Server Linux-VM inrichten](../linux/sql-vm-create-portal-quickstart.md)
* [Veelgestelde vragen (Linux)](../linux/frequently-asked-questions-faq.md)
* [Documentatie voor SQL Server op Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
