---
title: Wat is nieuw?
titleSuffix: Azure SQL Database & SQL Managed Instance
description: Meer informatie over de nieuwe functies en verbeteringen voor de Azure SQL Database & SQL Managed instance service en documentatie
services: sql-database
author: stevestein
ms.service: sql-database
ms.subservice: service
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: conceptual
ms.date: 05/13/2020
ms.author: sstein
ms.openlocfilehash: 07af9fa20fd6a331313050d3ba2cfbbe9c3fe7e8
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2020
ms.locfileid: "84050301"
---
# <a name="whats-new-in-azure-sql-database--sql-managed-instance"></a>Wat is er nieuw in Azure SQL Database & SQL Managed instance?
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Dit artikel bevat een overzicht van Azure SQL Database en Azure SQL Managed instance-functies die momenteel beschikbaar zijn in de open bare preview. Zie [SQL Database & SQL Managed instance service updates](https://azure.microsoft.com/updates/?product=sql-database)voor SQL database en de updates en verbeteringen van SQL Managed instance. Zie [service-updates](https://azure.microsoft.com/updates)voor updates en verbeteringen voor andere Azure-Services.

## <a name="whats-new"></a>Wat is nieuw?

Documentatie voor Azure SQL Database en Azure SQL Managed instance is in afzonderlijke secties gesplitst. We hebben ook bijgewerkt hoe we verwijzen naar een beheerd exemplaar van *Azure SQL database beheerde* instantie naar *Azure SQL Managed*instance.

We hebben dit gedaan omdat sommige functies en functionaliteit aanzienlijk verschillen tussen één data base en een beheerd exemplaar, en het is steeds meer een uitdaging om complexe nuances tussen Azure SQL Database en Azure SQL Managed instance in afzonderlijke ' gedeelde ' artikelen uit te leggen.

Deze verduidelijking tussen de verschillende Azure SQL-producten moet het proces van het werken met de SQL Server data base-engine in azure vereenvoudigen en stroom lijnen, ongeacht of het nu een beheerde data base in Azure SQL Database is, een volledig zelfstandig beheerd exemplaar dat meerdere data bases host in Azure SQL Managed instance of het vertrouwde on-premises SQL Server product, maar gehost op een virtuele machine in Azure.

Houd er rekening mee dat dit een onderhanden werk is en dat niet elk artikel nog is bijgewerkt. Bijvoorbeeld: Documentatie voor Transact-SQL (T-SQL)-instructies, opgeslagen procedures en veel functies die worden gedeeld tussen Azure SQL Database en Azure SQL Managed instance zijn nog niet voltooid, dus wij danken u voor uw geduld, omdat u doorgaat met het verduidelijken van de inhoud. 

Deze tabel bevat een snelle vergelijking voor de wijziging in de terminologie: 


|**Nieuwe term**  | **Vorige term**  |**Uitleg** |
|---------|---------|---------|
|**Azure SQL Managed Instance** | *Beheerde instantie* Azure SQL database| Azure SQL Managed instance is een eigen product in de Azure SQL-familie, in plaats van alleen een implementatie optie in Azure SQL Database. | 
|**Azure SQL Database**|Azure SQL Database *afzonderlijke data base*| Tenzij expliciet anders is opgegeven, bevat de product naam Azure SQL Database zowel afzonderlijke data bases als data bases die zijn geïmplementeerd op een elastische pool. |
|**Azure SQL Database**|*Elastische pool* Azure SQL database| Tenzij expliciet anders is opgegeven, bevat de product naam Azure SQL Database zowel afzonderlijke data bases als data bases die zijn geïmplementeerd op een elastische pool.  |
|**Azure SQL Database** |Azure SQL Database | Hoewel de term niet hetzelfde is, is deze nu alleen van toepassing op de implementatie van één data base en voor elastische Pools, en omvat het beheerde exemplaar niet. |
| **Azure SQL**| N.v.t. | Dit verwijst naar de familie van SQL Server data base engine-producten die beschikbaar zijn in Azure: Azure SQL Database, Azure SQL Managed instance en SQL Server op Azure Vm's. | 

## <a name="features-in-public-preview"></a>Functies in open bare preview

### <a name="azure-sql-database"></a>[Azure SQL Database](#tab/single-database)

| Functie | Details |
| ---| --- |
| Nieuwe hardware gegenereerd met de Fsv2-serie en M-serie| Zie [Hardware-generaties](service-tiers-vcore.md#hardware-generations)voor meer informatie.|
| Versneld database herstel met afzonderlijke data bases en elastische Pools | Zie [versneld database herstel](../accelerated-database-recovery.md)voor meer informatie.|
|Aantal benadering|Zie [geschatte aantallen DISTINCT](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#approximate-query-processing)voor meer informatie.|
|Batch-modus op Rowstore (onder compatibiliteits niveau 150)|Zie [batch-modus op Rowstore](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#batch-mode-on-rowstore)voor meer informatie.|
| Gegevensdetectie en -classificatie  |Zie [Azure SQL database en Azure Synapse Analytics data discovery & classificatie](data-discovery-and-classification-overview.md)voor meer informatie.|
| Taken voor Elastic Database | Zie [elastische taken maken, configureren en beheren](elastic-jobs-overview.md)voor meer informatie. |
| Elastische query’s | Zie [overzicht van elastische query's](elastic-query-overview.md)voor meer informatie. |
| Elastische transacties | [Gedistribueerde trans acties over Cloud databases](elastic-transactions-overview.md). |
|Feedback over geheugen toekenning (rij-modus) (onder compatibiliteits niveau 150)|Zie [feedback over geheugen toekenning (rij-modus)](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#row-mode-memory-grant-feedback)voor meer informatie.|
| Query-Editor in het Azure Portal |Zie [de SQL-query editor van het Azure Portal gebruiken om verbinding te maken en gegevens op te vragen](connect-query-portal.md)voor meer informatie.|
| R Services/machine learning met afzonderlijke data bases en elastische Pools |Zie [Machine Learning Services in Azure SQL database](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#machine-learning-services-in-azure-sql-database)voor meer informatie.|
|SQL Analytics|Zie [Azure SQL-analyse](../../azure-monitor/insights/azure-sql.md)voor meer informatie.|
|Uitgestelde compilatie van tabel variabele (onder compatibiliteits niveau 150)|Zie voor meer informatie [tabel variabele uitgestelde compilatie](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#table-variable-deferred-compilation).|
| &nbsp; |

### <a name="azure-sql-managed-instance"></a>[Azure SQL Managed Instance](#tab/managed-instance)

| Functie | Details |
| ---| --- |
| <a href="/azure/sql-database/sql-database-instance-pools">Exemplaargroepen</a> | Een handige en rendabele manier om kleinere SQL-instanties naar de cloud te migreren. |
| <a href="https://aka.ms/managed-instance-aadlogins">Azure AD server-principals op exemplaar niveau (aanmeldingen)</a> | Aanmeldingen op instantie niveau maken met behulp van de instructie <a href="https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">Aanmelden van de externe provider maken</a> . |
| [Transactionele replicatie](../managed-instance/replication-transactional-overview.md) | U kunt de wijzigingen van uw tabellen repliceren naar andere data bases die zijn geplaatst in door SQL beheerde exemplaren, SQL-data bases of SQL Server, of u kunt uw tabellen bijwerken wanneer sommige rijen worden gewijzigd in andere SQL Managed instances of SQL Server Instance. Zie [replicatie configureren in Azure SQL Managed instance](../managed-instance/replication-between-two-instances-configure-tutorial.md)voor meer informatie. |
| Detectie van bedreigingen |Zie [detectie van dreigingen configureren in Azure SQL Managed instance](../managed-instance/threat-detection-configure.md)voor meer informatie.|
| Langetermijnretentie van back-ups | Zie voor meer informatie [lange termijn back-up van Bewaar periode configureren in Azure SQL Managed instance](../managed-instance/long-term-backup-retention-configure.md), dat momenteel een beperkte open bare preview-versie is. | 

---

## <a name="sql-managed-instance---new-features-and-known-issues"></a>Door SQL beheerd exemplaar: nieuwe functies en bekende problemen

### <a name="sql-managed-instance-h2-2019-updates"></a>SQL Managed instance H2 2019-updates

- [Configuratie van geaidede subnetten](https://azure.microsoft.com/updates/service-aided-subnet-configuration-for-managed-instance-in-azure-sql-database-available/) Een veilige en handige manier om de subnet-configuratie te beheren, waar u gegevens verkeer beheert terwijl SQL Managed instance ervoor zorgt dat het beheer verkeer niet wordt onderbroken
- [Transparent Data Encryption (TDE) met Bring your own Key (BYOK)](https://azure.microsoft.com/updates/general-avilability-transparent-data-encryption-with-customer-managed-keys-for-azure-sql-database-managed-instance/) maakt gebruik van het scenario van uw eigen sleutel (BYOK) voor gegevens bescherming in rust en stelt organisaties in staat om beheer taken voor sleutels en gegevens te scheiden.
- Met [groepen voor automatische failover](https://azure.microsoft.com/updates/azure-sql-database-auto-failover-groups-feature-now-available-in-all-regions/) kunt u alle data bases van het primaire exemplaar repliceren naar een secundair exemplaar in een andere regio.
- Configureer het gedrag van SQL Managed instance met [globale traceer vlaggen](https://azure.microsoft.com/updates/global-trace-flags-are-now-available-in-azure-sql-database-managed-instance/).

### <a name="sql-managed-instance-h1-2019-updates"></a>SQL Managed instance H1 2019-updates

De volgende functies zijn ingeschakeld in het implementatie model voor SQL Managed instance in H1 2019:
  - Ondersteuning voor abonnementen met een <a href="https://aka.ms/sql-mi-visual-studio-subscribers">maandelijks Azure-tegoed voor Visual Studio-abonnees</a> en verhoogde [regionale limieten](../managed-instance/resource-limits.md#regional-resource-limitations).
  - Ondersteuning voor <a href="https://docs.microsoft.com/sharepoint/administration/deploy-azure-sql-managed-instance-with-sharepoint-servers-2016-2019"> SharePoint 2016 en SharePoint 2019 </a> en <a href="https://docs.microsoft.com/business-applications-release-notes/october18/dynamics365-business-central/support-for-azure-sql-database-managed-instance"> Dynamics 365 Business Central </a>
  - Maak SQL Managed instances met <a href="https://aka.ms/managed-instance-collation">sorteringen op exemplaar niveau</a> en <a href="https://azure.microsoft.com/updates/managed-instance-time-zone-ga/">tijd zone</a> van uw keuze.
  - SQL Managed nstances is nu beveiligd met [ingebouwde firewall](../managed-instance/management-endpoint-verify-built-in-firewall.md).
  - Configureer SQL Managed instances voor het gebruik van [open bare eind punten](../managed-instance/public-endpoint-configure.md), het negeren van de [proxy](connectivity-architecture.md#connection-policy) voor betere netwerk prestaties, <a href="https://aka.ms/four-cores-sql-mi-update">4 VCores bij het genereren van GEN5-hardware</a> of <a href="https://aka.ms/managed-instance-configurable-backup-retention">35 het configureren van back-ups voor herstel na een</a> bepaald tijdstip. [Lange termijn retentie van back-ups](long-term-retention-overview.md#managed-instance-support) (Maxi maal 10 jaar) is momenteel een beperkte open bare preview.  
  - Dankzij nieuwe functies kunt u <a href="https://medium.com/@jocapc/geo-restore-your-databases-on-azure-sql-instances-1451480e90fa">uw data base met behulp van Power shell naar een ander Data Center herstellen, de</a> [naam van de data base wijzigen](https://azure.microsoft.com/updates/azure-sql-database-managed-instance-database-rename-is-supported/), het [virtuele cluster verwijderen](../managed-instance/virtual-cluster-delete.md).
  - Met de nieuwe [rol Inzender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-managed-instance-contributor) voor ingebouwde instanties kunt u SoD-naleving met beveiligings principes en naleving van de bedrijfs normen.
  - SQL Managed instance is beschikbaar in de volgende Azure Government regio's voor GA (US Gov-Texas, US Gov-Arizona) en in China-noord 2 en China-oost 2. Het is ook beschikbaar in de volgende open bare regio's: Australië-centraal, Australië-centraal 2, Brazilië-zuid, Frankrijk-zuid, UAE-centraal, UAE-noord, Zuid-Afrika-noord, Zuid-Afrika-west.

### <a name="known-issues"></a>Bekende problemen

|Probleem  |Gedetecteerde datum  |Status  |Opgelost op  |
|---------|---------|---------|---------|
|[Hand matige back-up herstellen zonder CONTROLESOM kan mislukken](#restoring-manual-backup-without-checksum-might-fail)|Mei 2020|Heeft tijdelijke oplossing| |
|[Agent reageert niet meer wanneer u bestaande taken wijzigt, uitschakelt of inschakelt](#agent-becomes-unresponsive-upon-modifying-disabling-or-enabling-existing-jobs)|Mei 2020|Automatisch beperkt| |
|[Machtigingen voor de resource groep zijn niet toegepast op een SQL-beheerd exemplaar](#permissions-on-resource-group-not-applied-to-sql-managed-instance)|Feb 2020|Heeft tijdelijke oplossing||
|[Beperking van hand matige failover via de portal voor failover-groepen](#limitation-of-manual-failover-via-portal-for-failover-groups)|Jan 2020|Heeft tijdelijke oplossing||
|[SQL Agent-rollen hebben expliciete EXECUTE-machtigingen nodig voor niet-sysadmin-aanmeldingen](#in-memory-oltp-memory-limits-are-not-applied)|Dec 2019|Heeft tijdelijke oplossing||
|[SQL-Agent taken kunnen worden onderbroken door agent proces opnieuw te starten](#sql-agent-jobs-can-be-interrupted-by-agent-process-restart)|Dec 2019|Opgelost|Mrt 2020|
|[AAD-aanmeldingen en-gebruikers worden niet ondersteund in SSDT](#aad-logins-and-users-are-not-supported-in-ssdt)|Nov 2019|Geen tijdelijke oplossing||
|[In-Memory OLTP-geheugen limieten worden niet toegepast](#in-memory-oltp-memory-limits-are-not-applied)|Okt 2019|Heeft tijdelijke oplossing||
|[Verkeerde fout geretourneerd tijdens het verwijderen van een bestand dat niet leeg is](#wrong-error-returned-while-trying-to-remove-a-file-that-is-not-empty)|Okt 2019|Heeft tijdelijke oplossing||
|[Het wijzigen van de servicelaag en het maken van exemplaar bewerkingen worden geblokkeerd door de huidige Data Base te herstellen](#change-service-tier-and-create-instance-operations-are-blocked-by-ongoing-database-restore)|Sep 2019|Heeft tijdelijke oplossing||
|[Resource Governor op Bedrijfskritiek servicelaag moet mogelijk opnieuw worden geconfigureerd na een failover](#resource-governor-on-business-critical-service-tier-might-need-to-be-reconfigured-after-failover)|Sep 2019|Heeft tijdelijke oplossing||
|[Meerdere data base-Service Broker dialoog vensters moeten opnieuw worden geïnitialiseerd na een upgrade naar de servicelaag](#cross-database-service-broker-dialogs-must-be-reinitialized-after-service-tier-upgrade)|Aug 2019|Heeft tijdelijke oplossing||
|[Impersonification van Azure AD-aanmeldings typen wordt niet ondersteund](#impersonification-of-azure-ad-login-types-is-not-supported)|Jul 2019|Geen tijdelijke oplossing||
|[@queryde para meter wordt niet ondersteund in sp_send_db_mail](#-parameter-not-supported-in-sp_send_db_mail)|Apr 2019|Geen tijdelijke oplossing||
|[Transactionele replicatie moet opnieuw worden geconfigureerd na geo-failover](#transactional-replication-must-be-reconfigured-after-geo-failover)|Mrt 2019|Geen tijdelijke oplossing||
|[Er wordt een tijdelijke data base gebruikt tijdens de herstel bewerking](#temporary-database-is-used-during-restore-operation)||Heeft tijdelijke oplossing||
|[TEMPDB-structuur en-inhoud worden opnieuw gemaakt](#tempdb-structure-and-content-is-re-created)||Geen tijdelijke oplossing||
|[Opslag ruimte overschrijden met kleine database bestanden](#exceeding-storage-space-with-small-database-files)||Heeft tijdelijke oplossing||
|[GUID-waarden die worden weer gegeven in plaats van database namen](#guid-values-shown-instead-of-database-names)||Heeft tijdelijke oplossing||
|[Fouten logboeken zijn niet persistent gemaakt](#error-logs-arent-persisted)||Geen tijdelijke oplossing||
|[Het transactie bereik van twee data bases binnen hetzelfde exemplaar wordt niet ondersteund](#transaction-scope-on-two-databases-within-the-same-instance-isnt-supported)||Heeft tijdelijke oplossing|Mrt 2020|
|[CLR-modules en gekoppelde servers kunnen soms niet verwijzen naar een lokaal IP-adres](#clr-modules-and-linked-servers-sometimes-cant-reference-a-local-ip-address)||Heeft tijdelijke oplossing||
|Consistentie van de data base is niet geverifieerd met DBCC CHECKDB na het herstellen van de data base van Azure Blob Storage.||Opgelost|Nov 2019|
|Het terugzetten van een tijdgebonden data base van Bedrijfskritiek laag naar Algemeen laag mislukt als de bron database in-memory OLTP-objecten bevat.||Opgelost|Okt 2019|
|Database Mail functie met externe e-mail servers (niet-Azure) via een beveiligde verbinding||Opgelost|Okt 2019|
|Inge sloten data bases worden niet ondersteund in een SQL-beheerd exemplaar||Opgelost|Aug 2019|


### <a name="restoring-manual-backup-without-checksum-might-fail"></a>Hand matige back-up herstellen zonder CONTROLESOM kan mislukken

In bepaalde omstandigheden kan hand matige back-up van data bases die zijn gemaakt op het beheerde exemplaar zonder CONTROLESOM, mogelijk niet worden hersteld. In dergelijke gevallen moet u de back-up herstellen totdat deze is voltooid.

**Tijdelijke oplossing**: Maak hand matige back-ups van data bases op een beheerd exemplaar met controlesom ingeschakeld.

### <a name="agent-becomes-unresponsive-upon-modifying-disabling-or-enabling-existing-jobs"></a>Agent reageert niet meer wanneer u bestaande taken wijzigt, uitschakelt of inschakelt

In bepaalde omstandigheden het wijzigen van een bestaande taak, het uitschakelen of inschakelen ervan kan ervoor zorgen dat de agent niet meer reageert. Het probleem wordt automatisch opgelost wanneer de detectie resulteert in het opnieuw opstarten van het agent proces.

### <a name="permissions-on-resource-group-not-applied-to-sql-managed-instance"></a>Machtigingen voor de resource groep zijn niet toegepast op een SQL-beheerd exemplaar

De RBAC-rol van het SQL Managed instance-exemplaar wanneer deze wordt toegepast op een resource groep (RG), wordt niet toegepast op een beheerd exemplaar van SQL en heeft geen effect.

**Tijdelijke oplossing**: de rol van SQL Managed instance-Inzender voor gebruikers op abonnements niveau instellen.

### <a name="limitation-of-manual-failover-via-portal-for-failover-groups"></a>Beperking van hand matige failover via de portal voor failover-groepen

Als een failovergroep meerdere exemplaren in verschillende Azure-abonnementen of resource groepen omvat, kan hand matige failover niet worden gestart vanuit het primaire exemplaar in de failovergroep.

**Tijdelijke oplossing**: Start de failover via de portal vanuit het geo-secundaire exemplaar.

### <a name="sql-agent-roles-need-explicit-execute-permissions-for-non-sysadmin-logins"></a>SQL Agent-rollen hebben expliciete EXECUTE-machtigingen nodig voor niet-sysadmin-aanmeldingen

Als niet-sysadmin-aanmeldingen worden toegevoegd aan een van de [vaste database rollen van SQL-Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent-fixed-database-roles), bestaat er een probleem met de expliciete uitvoer machtigingen die moeten worden verleend aan de opgeslagen hoofd procedures voor deze aanmeldingen. Als dit probleem wordt aangetroffen, wordt het fout bericht ' de machtiging voor uitvoeren is geweigerd op het object <object_name> (Microsoft SQL Server, fout: 229) ' weer gegeven.

**Tijdelijke oplossing**: wanneer u aanmeldingen toevoegt aan een van de vaste database rollen van SQL-Agent: SQLAgentUserRole, SQLAgentReaderRole of SQLAgentOperatorRole, voert u het onderstaande T-SQL-script uit om uitvoerings machtigingen expliciet toe te kennen aan de vermelde opgeslagen procedures voor elk van de aanmeldingen die zijn toegevoegd aan deze rollen.

```tsql
USE [master]
GO
CREATE USER [login_name] FOR LOGIN [login_name]
GO
GRANT EXECUTE ON master.dbo.xp_sqlagent_enum_jobs TO [login_name]
GRANT EXECUTE ON master.dbo.xp_sqlagent_is_starting TO [login_name]
GRANT EXECUTE ON master.dbo.xp_sqlagent_notify TO [login_name]
```

### <a name="sql-agent-jobs-can-be-interrupted-by-agent-process-restart"></a>SQL-Agent taken kunnen worden onderbroken door agent proces opnieuw te starten

**(Opgelost in maart 2020)** Elke keer dat de taak wordt gestart, maakt SQL Agent een nieuwe sessie, waardoor het geheugen verbruik geleidelijk toeneemt. Om te voor komen dat de interne geheugen limiet wordt beïnvloed, waardoor de uitvoering van geplande taken wordt geblokkeerd, wordt het agent proces opnieuw gestart zodra het geheugen verbruik de drempel waarde bereikt. Dit kan leiden tot een onderbreking van de uitvoering van taken die worden uitgevoerd op het moment dat de computer opnieuw wordt opgestart.

### <a name="in-memory-oltp-memory-limits-are-not-applied"></a>In-Memory OLTP-geheugen limieten worden niet toegepast

Bedrijfskritiek service-laag past in sommige gevallen [Maxi maal geheugen limieten toe voor objecten die zijn geoptimaliseerd voor geheugen](../managed-instance/resource-limits.md#in-memory-oltp-available-space) . Met SQL Managed instance kan workload meer geheugen gebruiken voor OLTP-bewerkingen in het geheugen. Dit kan van invloed zijn op de beschik baarheid en stabiliteit van het exemplaar. In-Memory OLTP-query's die de limieten bereiken, mislukken mogelijk niet onmiddellijk. Dit probleem wordt binnenkort opgelost. De query's die gebruikmaken van meer in-Memory OLTP-geheugen, zullen eerder worden uitgevoerd als ze de [limieten](../managed-instance/resource-limits.md#in-memory-oltp-available-space)bereiken.

**Tijdelijke oplossing:** [Bewaak het gebruik van in-Memory OLTP-opslag](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory-oltp-monitor-space) met behulp van [SQL Server Management Studio](/sql/relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage#bkmk_Monitoring) om ervoor te zorgen dat de werk belasting niet meer dan het beschik bare geheugen gebruikt. Verhoog de geheugen limieten die afhankelijk zijn van het aantal vCores of Optimaliseer uw werk belasting zodat er minder geheugen wordt gebruikt.
 
### <a name="wrong-error-returned-while-trying-to-remove-a-file-that-is-not-empty"></a>Verkeerde fout geretourneerd tijdens het verwijderen van een bestand dat niet leeg is

Met SQL Server/SQL Managed instance [kan de gebruiker geen bestand verwijderen dat niet leeg is](/sql/relational-databases/databases/delete-data-or-log-files-from-a-database#Prerequisites). Als u probeert een niet-leeg gegevens bestand met behulp van een instructie te verwijderen `ALTER DATABASE REMOVE FILE` , wordt de fout `Msg 5042 – The file '<file_name>' cannot be removed because it is not empty` niet onmiddellijk geretourneerd. SQL Managed instance zal blijven proberen het bestand te verwijderen en de bewerking kan niet worden uitgevoerd na 30 minuten met `Internal server error` .

**Tijdelijke oplossing**: Verwijder de inhoud van het bestand met behulp van de `DBCC SHRINKFILE (N'<file_name>', EMPTYFILE)` opdracht. Als dit het enige bestand in de bestands groep is, moet u gegevens verwijderen uit de tabel of partitie die aan deze bestands groep is gekoppeld voordat u het bestand verkleint en deze gegevens optioneel laadt in een andere tabel/partitie.

### <a name="change-service-tier-and-create-instance-operations-are-blocked-by-ongoing-database-restore"></a>Het wijzigen van de servicelaag en het maken van exemplaar bewerkingen worden geblokkeerd door de huidige Data Base te herstellen

Door de doorlopende `RESTORE` instructie, het migratie proces van de gegevens migratie service en de ingebouwde tijd bij het herstellen wordt de service tier of de grootte van het bestaande exemplaar bijgewerkt en er worden nieuwe instanties gemaakt totdat het herstel proces is voltooid. Met het herstel proces worden deze bewerkingen geblokkeerd voor de beheerde instanties en exemplaar groepen in hetzelfde subnet waar het herstel proces wordt uitgevoerd. De exemplaren in exemplaar groepen worden niet beïnvloed. Het maken of wijzigen van service tier-bewerkingen mislukken of time-out: ze worden voortgezet zodra het herstel proces is voltooid of geannuleerd.

**Tijdelijke oplossing**: wacht tot het herstel proces is voltooid of Annuleer het herstel proces als de bewerking voor het maken of bijwerken van de service tier een hogere prioriteit heeft.

### <a name="resource-governor-on-business-critical-service-tier-might-need-to-be-reconfigured-after-failover"></a>Resource Governor op Bedrijfskritiek servicelaag moet mogelijk opnieuw worden geconfigureerd na een failover

[Resource Governor](/sql/relational-databases/resource-governor/resource-governor) functie waarmee u kunt beperken dat de resources die aan de werk belasting van de gebruiker zijn toegewezen, een bepaalde werk belasting onjuist classificeert na een failover of door de gebruiker geïnitieerde wijziging van de servicelaag (bijvoorbeeld de wijziging van de maximale vCore of de maximale opslag grootte voor instanties).

**Tijdelijke oplossing**: Voer `ALTER RESOURCE GOVERNOR RECONFIGURE` regel matig of als onderdeel van de SQL-Agent taak uit die de SQL-taak uitvoert wanneer het exemplaar wordt gestart als u [Resource Governor](/sql/relational-databases/resource-governor/resource-governor)gebruikt.

### <a name="cross-database-service-broker-dialogs-must-be-reinitialized-after-service-tier-upgrade"></a>Meerdere data base-Service Broker dialoog vensters moeten opnieuw worden geïnitialiseerd na een upgrade naar de servicelaag

Service Broker dialoog vensters voor meerdere data bases worden gestopt met het leveren van berichten aan de services in andere data bases nadat de bewerking van de service tier is gewijzigd. De berichten zijn **niet verloren gegaan** en kunnen worden gevonden in de wachtrij van de afzender. Elke wijziging van de opslag grootte van vCores of instance in een SQL Managed instance leidt ertoe dat de `service_broke_guid` waarde van de weer gave [sys. data bases](/sql/relational-databases/system-catalog-views/sys-databases-transact-sql) voor alle data bases wordt gewijzigd. Elke `DIALOG` gemaakt met de instructie [begin dialog](/sql/t-sql/statements/begin-dialog-conversation-transact-sql) , die verwijst naar service-Brokers in een andere data base, stopt met het leveren van berichten aan de doel service.

**Tijdelijke oplossing:** Stop alle activiteiten die gebruikmaken van cross-data base Service Broker dialoog venster gesprekken voordat u de servicelaag bijwerkt en opnieuw initialiseert. Als er nog andere berichten zijn die niet worden bezorgd na wijziging van de servicelaag, leest u de berichten van de bron wachtrij en verzendt u deze opnieuw naar de doel wachtrij.

### <a name="impersonification-of-azure-ad-login-types-is-not-supported"></a>Impersonification van Azure AD-aanmeldings typen wordt niet ondersteund

Imitatie met `EXECUTE AS USER` of `EXECUTE AS LOGIN` van volgende Aad-principals wordt niet ondersteund:
-    Aliased AAD-gebruikers. In dit geval wordt de volgende fout geretourneerd `15517` .
- AAD-aanmeldingen en-gebruikers op basis van AAD-toepassingen of service-principals. De volgende fouten worden in dit geval en worden geretourneerd `15517` `15406` .

### <a name="query-parameter-not-supported-in-sp_send_db_mail"></a>@queryde para meter wordt niet ondersteund in sp_send_db_mail

De `@query` para meter in de [sp_send_db_mail](/sql/relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql) procedure werkt niet.

### <a name="transactional-replication-must-be-reconfigured-after-geo-failover"></a>Transactionele replicatie moet opnieuw worden geconfigureerd na geo-failover

Als transactionele replicatie is ingeschakeld voor een data base in een groep met automatische failover, moet de beheerder van de SQL Managed instance alle publicaties opschonen op de oude primaire en opnieuw configureren op de nieuwe primaire versie nadat een failover naar een andere regio is uitgevoerd. Zie [replicatie](../managed-instance/transact-sql-tsql-differences-sql-server.md#replication) voor meer informatie.

### <a name="aad-logins-and-users-are-not-supported-in-ssdt"></a>AAD-aanmeldingen en-gebruikers worden niet ondersteund in SSDT

SQL Server Data Tools Azure Active Directory-aanmeldingen en-gebruikers niet volledig ondersteunen.

### <a name="temporary-database-is-used-during-restore-operation"></a>Er wordt een tijdelijke data base gebruikt tijdens de herstel bewerking

Wanneer een Data Base wordt hersteld op een SQL-beheerd exemplaar, wordt door de Restore-service eerst een lege data base met de gewenste naam gemaakt om de naam van het exemplaar toe te wijzen. Na enige tijd wordt deze data base verwijderd en wordt het herstellen van de werkelijke data base gestart. De data base die zich in de *herstel* status bevindt, heeft tijdelijk een wille KEURige GUID-waarde in plaats van een naam. De tijdelijke naam wordt gewijzigd in de gewenste naam die in `RESTORE` de instructie is opgegeven nadat het herstel proces is voltooid. In de eerste fase heeft de gebruiker toegang tot de lege data base en kan zelfs tabellen worden gemaakt of gegevens worden geladen in deze data base. Deze tijdelijke data base wordt verwijderd wanneer de Restore-service de tweede fase start.

**Tijdelijke oplossing**: Maak geen toegang tot de data base die u wilt herstellen, totdat u ziet dat de herstel bewerking is voltooid.

### <a name="tempdb-structure-and-content-is-re-created"></a>TEMPDB-structuur en-inhoud worden opnieuw gemaakt

De `tempdb` Data Base is altijd gesplitst in 12 gegevens bestanden en de bestands structuur kan niet worden gewijzigd. De maximale grootte per bestand kan niet worden gewijzigd en er kunnen geen nieuwe bestanden aan worden toegevoegd `tempdb` . `Tempdb`wordt altijd opnieuw gemaakt als een lege data base wanneer het exemplaar wordt gestart of een failover wordt uitgevoerd, en eventuele wijzigingen `tempdb` worden niet bewaard.

### <a name="exceeding-storage-space-with-small-database-files"></a>Opslag ruimte overschrijden met kleine database bestanden

`CREATE DATABASE`, `ALTER DATABASE ADD FILE` en- `RESTORE DATABASE` instructies kunnen mislukken omdat het exemplaar de Azure Storage limiet kan bereiken.

Elk Algemeen SQL Managed instance heeft tot 35 TB aan opslag gereserveerd voor Azure Premium-schijf ruimte. Elk database bestand wordt geplaatst op een afzonderlijke fysieke schijf. Schijf grootten kunnen 128 GB, 256 GB, 512 GB, 1 TB of 4 TB zijn. Voor ongebruikte ruimte op de schijf worden geen kosten in rekening gebracht, maar de totale som van Azure Premium-schijf grootten mag niet groter zijn dan 35 TB. In sommige gevallen kan een SQL Managed instance die geen 8 TB in totaal nodig heeft, de Azure-limiet van 35 TB overschrijden bij de opslag grootte vanwege interne fragmentatie.

Een Algemeen SQL Managed instance kan bijvoorbeeld één groot bestand hebben dat 1,2 TB groot is voor een schijf van 4 TB. Het kan ook 248 bestanden met een grootte van 1 GB hebben die allemaal op afzonderlijke 128 GB-schijven worden geplaatst. In dit voorbeeld:

- De totale toegewezen schijf ruimte is 1 x 4 TB + 248 x 128 GB = 35 TB.
- De totale gereserveerde ruimte voor data bases op het exemplaar is 1 x 1,2 TB + 248 x 1 GB = 1,4 TB.

In dit voor beeld ziet u dat als gevolg van een specifieke distributie van bestanden onder bepaalde omstandigheden, een SQL Managed instance de limiet van 35 TB kan bereiken die is gereserveerd voor een gekoppelde Azure Premium-schijf wanneer u dit mogelijk niet verwacht.

In dit voor beeld blijven bestaande data bases werken en kunnen ze zonder enig probleem groeien zolang er geen nieuwe bestanden worden toegevoegd. Nieuwe data bases kunnen niet worden gemaakt of hersteld omdat er onvoldoende ruimte is voor nieuwe schijf stations, zelfs als de totale grootte van alle data bases niet de limiet voor de exemplaar grootte bereikt. De fout die in dat geval wordt geretourneerd, is niet duidelijk.

U kunt [het aantal resterende bestanden identificeren](https://medium.com/azure-sqldb-managed-instance/how-many-files-you-can-create-in-general-purpose-azure-sql-managed-instance-e1c7c32886c1) met behulp van systeem weergaven. Als u deze limiet bereikt, kunt u [een aantal kleinere bestanden leegmaken en verwijderen met behulp van de DBCC SHRINKFILE-instructie](/sql/t-sql/database-console-commands/dbcc-shrinkfile-transact-sql#d-emptying-a-file) of overschakelen naar de [bedrijfskritiek-laag, die deze limiet niet overschrijdt](/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics).

### <a name="guid-values-shown-instead-of-database-names"></a>GUID-waarden die worden weer gegeven in plaats van database namen

In verschillende systeem weergaven, prestatie meter items, fout berichten, XEvents en fouten logboek vermeldingen worden GUID-database-id's weer gegeven in plaats van de werkelijke database namen. Vertrouw niet op deze GUID-id's, omdat deze in de toekomst worden vervangen door feitelijke database namen.

**Tijdelijke oplossing**: gebruik de sys. data bases-weer gave om de werkelijke database naam op te lossen op basis van de fysieke database naam, opgegeven in de vorm van GUID-database-id's

```tsql
SELECT name as ActualDatabaseName, physical_database_name as GUIDDatabaseIdentifier 
FROM sys.databases
WHERE database_id > 4
```

### <a name="error-logs-arent-persisted"></a>Fouten logboeken zijn niet persistent gemaakt

Fouten logboeken die beschikbaar zijn in een SQL Managed instance worden niet persistent gemaakt en hun grootte is niet opgenomen in de maximale opslag limiet. Fout logboeken kunnen automatisch worden gewist als er een failover wordt uitgevoerd. Er zijn mogelijk hiaten in de fouten logboek geschiedenis, omdat een door SQL beheerd exemplaar meerdere keren is verplaatst op verschillende virtuele machines.

### <a name="transaction-scope-on-two-databases-within-the-same-instance-isnt-supported"></a>Het transactie bereik van twee data bases binnen hetzelfde exemplaar wordt niet ondersteund

**(Opgelost in maart 2020)** De `TransactionScope` klasse in .net werkt niet als twee query's worden verzonden naar twee data bases binnen hetzelfde exemplaar onder hetzelfde transactie bereik:

```csharp
using (var scope = new TransactionScope())
{
    using (var conn1 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn1.Open();
        SqlCommand cmd1 = conn1.CreateCommand();
        cmd1.CommandText = string.Format("insert into T1 values(1)");
        cmd1.ExecuteNonQuery();
    }

    using (var conn2 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn2.Open();
        var cmd2 = conn2.CreateCommand();
        cmd2.CommandText = string.Format("insert into b.dbo.T2 values(2)");        cmd2.ExecuteNonQuery();
    }

    scope.Complete();
}

```

**Tijdelijke oplossing (niet nodig sinds 2020 maart):** Gebruik [SqlConnection. ChangeDatabase (teken reeks)](/dotnet/api/system.data.sqlclient.sqlconnection.changedatabase) als u een andere data base in een verbindings context wilt gebruiken in plaats van twee verbindingen te gebruiken.

### <a name="clr-modules-and-linked-servers-sometimes-cant-reference-a-local-ip-address"></a>CLR-modules en gekoppelde servers kunnen soms niet verwijzen naar een lokaal IP-adres

CLR-modules die worden geplaatst in een SQL-beheerd exemplaar en gekoppelde servers of gedistribueerde query's die verwijzen naar een huidige instantie, kunnen het IP-adres van een lokaal exemplaar soms niet omzetten. Deze fout is een tijdelijk probleem.

**Tijdelijke oplossing:** Gebruik, indien mogelijk, context verbindingen in een CLR-module.

## <a name="updates"></a>Updates

Zie [SQL database service-updates](https://azure.microsoft.com/updates/?product=sql-database)voor een lijst met updates en verbeteringen van SQL database.

Zie [service-updates](https://azure.microsoft.com/updates)voor updates en verbeteringen voor alle Azure-Services.

## <a name="contribute-to-content"></a>Bijdragen aan inhoud

Raadpleeg de [hand leiding voor docs-inzenders](https://docs.microsoft.com/contribute/)om een bijdrage te leveren aan de Azure SQL-documentatie.
