---
title: Resource limieten voor logische servers in azure
description: Dit artikel bevat een overzicht van de resource limieten voor de logische server in azure die wordt gebruikt door Azure SQL Database en Azure Synapse Analytics. Het bevat ook informatie over wat er gebeurt wanneer deze resource limieten worden bereikt of overschreden.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan,moslake,josack
ms.date: 11/19/2019
ms.openlocfilehash: f39faae6f9bda9efb814be8cdcbdac3ae80497ba
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2020
ms.locfileid: "84051001"
---
# <a name="resource-limits-for-azure-sql-database-and-azure-synapse-analytics-servers"></a>Resource limieten voor Azure SQL Database en Azure Synapse Analytics-servers
[!INCLUDE[appliesto-sqldb-asa](../includes/appliesto-sqldb-asa.md)]

Dit artikel bevat een overzicht van de resource limieten voor de logische server die wordt gebruikt door Azure SQL Database en Azure Synapse Analytics. Het bevat informatie over wat er gebeurt wanneer deze resource limieten worden bereikt of overschreden, en beschrijft de resource governance-mechanismen die worden gebruikt om deze limieten af te dwingen.

> [!NOTE]
> Zie [SQL database resource limieten voor beheerde instanties](../managed-instance/resource-limits.md)voor de limieten voor Azure SQL Managed instances.

## <a name="maximum-resource-limits"></a>Maximum aantal resource limieten

| Resource | Limiet |
| :--- | :--- |
| Data bases per server | 5000 |
| Standaard aantal servers per abonnement in een wille keurige regio | 20 |
| Maximum aantal servers per abonnement in een regio | 200 |  
| DTU/eDTU-quotum per server | 54.000 |  
| vCore quotum per Server/exemplaar | 540 |
| Maximum aantal groepen per server | Beperkt door het aantal Dtu's of vCores. Als bijvoorbeeld elke groep 1000 Dtu's is, kan een server 54 groepen ondersteunen.|
|||

> [!IMPORTANT]
> Wanneer het aantal data bases de limiet per server nadert, kunnen de volgende problemen optreden:
>
> - De latentie verhogen bij het uitvoeren van query's op de hoofd database.  Dit geldt ook voor weer gaven van gegevens over het resource gebruik, zoals sys. resource_stats.
> - Het verg Roten van latentie in beheer bewerkingen en het weer geven van portal-gezichts punten waarbij de data bases op de server worden geïnventariseerd.

> [!NOTE]
> Als u meer DTU/eDTU-quotum, vCore quotum of meer servers dan het standaard aantal wilt verkrijgen, moet u een nieuwe ondersteunings aanvraag indienen in de Azure Portal. Zie [aanvraag quotum verhogingen voor Azure SQL database](quota-increase-request.md)voor meer informatie.

### <a name="storage-size"></a>Opslag grootte

Voor bron opslag van één data base, raadpleegt u op [DTU gebaseerde resource limieten](resource-limits-dtu-single-databases.md) of op [vCore gebaseerde resource limieten](resource-limits-vcore-single-databases.md) voor de maximale opslag ruimte per prijs categorie.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>Wat er gebeurt wanneer de database resource limieten zijn bereikt

### <a name="compute-dtus-and-edtus--vcores"></a>Compute (Dtu's en Edtu's/vCores)

Wanneer database Compute-gebruik (gemeten door Dtu's en Edtu's, of vCores) hoog wordt, wordt de query latentie verhoogd en kunnen query's zelfs een time-out opleveren. Onder deze omstandigheden kunnen query's in de wachtrij worden geplaatst door de service en worden resources opgegeven voor uitvoering omdat resources gratis worden.
Wanneer het gebruik van hoge berekeningen wordt tegengekomen, zijn de volgende opties voor risico beperking:

- De reken grootte van de data base of elastische pool verg Roten om de data base te voorzien van meer compute-resources. Zie bronnen van [één data base schalen](single-database-scale.md) en [elastische pool resources schalen](elastic-pool-scale.md).
- Query's optimaliseren om het resource gebruik van elke query te verminderen. Zie [query tuning/Hinting](performance-guidance.md#query-tuning-and-hinting)(Engelstalig) voor meer informatie.

### <a name="storage"></a>Storage

Wanneer gebruikte database ruimte de maximale grootte bereikt, worden invoeg bewerkingen en updates die de omvang van de gegevens overschrijden, door data bases ingevoegd en bijgewerkt en ontvangen clients een [fout bericht](troubleshoot-common-errors-issues.md). De instructies SELECT en DELETE blijven slagen.

Wanneer u gebruik maakt van hoge ruimte, kunt u onder andere het volgende doen:

- De maximale grootte van de data base of elastische pool verg Roten of meer opslag toevoegen. Zie bronnen van [één data base schalen](single-database-scale.md) en [elastische pool resources schalen](elastic-pool-scale.md).
- Als de data base zich in een elastische pool bevindt, kan de data base buiten de pool worden geplaatst, zodat de opslag ruimte niet wordt gedeeld met andere data bases.
- Een Data Base verkleinen om ongebruikte ruimte te claimen. Zie [Bestands ruimte beheren in Azure SQL database](file-space-manage.md) voor meer informatie.

### <a name="sessions-and-workers-requests"></a>Sessies en werk nemers (aanvragen)

Het maximum aantal sessies en werk rollen wordt bepaald door de servicelaag en de berekenings grootte (Dtu's/Edtu's of vCores). Nieuwe aanvragen worden afgewezen wanneer de limiet voor sessies of werk nemers is bereikt en clients ontvangen een fout bericht. Hoewel het aantal beschik bare verbindingen kan worden beheerd door de toepassing, is het aantal gelijktijdige werk nemers vaak moeilijker om te schatten en te beheren. Dit geldt met name voor piek belasting perioden wanneer database resource limieten zijn bereikt en werk nemers kunnen opvallen omdat er meer query's, grote blokkerende ketens of buitensporige query parallellisme worden uitgevoerd.

Wanneer u het gebruik van hoge sessies of werk nemers ondervindt, kunt u de volgende beperkende opties gebruiken:

- De servicelaag of de reken grootte van de data base of elastische pool wordt verhoogd. Zie bronnen van [één data base schalen](single-database-scale.md) en [elastische pool resources schalen](elastic-pool-scale.md).
- Het optimaliseren van query's om het resource gebruik van elke query te verminderen als de oorzaak van het verhoogde werk nemer is vanwege de conflicten voor reken bronnen. Zie [query tuning/Hinting](performance-guidance.md#query-tuning-and-hinting)(Engelstalig) voor meer informatie.
- De instelling van de [MAXDOP](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option#Guidelines) (maximale mate van parallellisme) verlagen.
- Optimaliseer de query werk belasting zodat het aantal keren en de duur van query's wordt beperkt.

### <a name="resource-consumption-by-user-workloads-and-internal-processes"></a>Resource verbruik door workloads van gebruikers en interne processen

Het CPU-en geheugen verbruik per werk belasting van de gebruiker in elke Data Base wordt gerapporteerd in de weer gaven [sys. dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database?view=azuresqldb-current) en [sys. resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database?view=azuresqldb-current) , in `avg_cpu_percent` en `avg_memory_usage_percent` kolommen. Voor elastische Pools wordt het gebruik van resource op pool niveau gerapporteerd in de weer gave [sys. elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) . Het CPU-verbruik van de werk belasting van de gebruiker wordt ook gerapporteerd via de `cpu_percent` Azure monitor metriek voor [afzonderlijke data bases](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserversdatabases) en [elastische Pools](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools) op groeps niveau.

Azure SQL Database hebt reken resources nodig om basis service functies te implementeren, zoals hoge Beschik baarheid en herstel na nood gevallen, database back-up en herstel, bewaking, query Store, automatische afstemming, enzovoort. Het systeem stelt een bepaald beperkt deel van de algemene resources voor deze interne processen in met behulp van [resource governance](#resource-governance) -mechanismen, waardoor de rest van resources beschikbaar is voor werk belastingen van gebruikers. Wanneer interne processen geen reken resources gebruiken, worden ze beschikbaar gemaakt voor werk belastingen van gebruikers.

Het totale CPU-en geheugen verbruik per werk belasting van de gebruiker en interne processen worden gerapporteerd in de weer gaven [sys. dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database?view=azuresqldb-current) en [sys. resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database?view=azuresqldb-current) , in `avg_instance_cpu_percent` en `avg_instance_memory_percent` kolommen. Deze gegevens worden ook gerapporteerd via de `sqlserver_process_core_percent` en `sqlserver_process_memory_percent` Azure monitor metrieken voor [afzonderlijke data bases](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserversdatabases) en [elastische Pools](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftsqlserverselasticpools) op groeps niveau.

In de weer gaven [sys. dm_resource_governor_resource_pools_history_ex](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-history-ex-azure-sql-database) en [sys. dm_resource_governor_workload_groups_history_ex](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-history-ex-azure-sql-database) vindt u meer gedetailleerde informatie over het gebruik van recente bronnen door werk belastingen van gebruikers en interne processen. Zie [resource governance](#resource-governance)voor meer informatie over resource groepen en werkbelasting groepen waarnaar wordt verwezen in deze weer gaven. Deze weer gaven rapporteren over het resource gebruik per werk belasting van de gebruiker en specifieke interne processen in de gekoppelde resource groepen en werkbelasting groepen.

In de context van het bewaken van de prestaties en het oplossen van problemen is het belang rijk om te overwegen om **CPU-verbruik** ( `avg_cpu_percent` , `cpu_percent` ) en het **totale CPU-verbruik** door werk belastingen van gebruikers en interne processen ( `avg_instance_cpu_percent` , `sqlserver_process_core_percent` ).

Het **CPU-verbruik** van de gebruiker wordt berekend als percentage van de werkbelasting limieten van de gebruiker in elke service doelstelling. **CPU-gebruik van gebruikers** bij 100% geeft aan dat de werk belasting van de gebruiker de limiet van de service doelstelling heeft bereikt. Wanneer het **totale CPU-verbruik** echter het bereik van 70-100% bereikt, is het mogelijk om de door Voer van de gebruiker te bekijken en de vertraging van query's te verg Roten, zelfs als gerapporteerd **CPU-verbruik** van de gebruiker aanzienlijk lager is dan 100%. Dit is waarschijnlijker bij het gebruik van kleinere service doelstellingen met een gemiddelde toewijzing van reken resources, maar relatief intensieve werk belastingen voor gebruikers, zoals in [compacte elastische Pools](elastic-pool-resource-management.md). Dit kan ook gebeuren met kleinere service doelstellingen wanneer interne processen tijdelijk extra bronnen nodig hebben, bijvoorbeeld bij het maken van een nieuwe replica van de data base.

Wanneer het **totale CPU-verbruik** hoog is, zijn de opties voor de risico beperking hetzelfde als eerder aangegeven, en zijn de optimalisatie van de service doelstelling en/of de werk belasting van de gebruiker opgenomen.

## <a name="resource-governance"></a>Resourcebeheer

Azure SQL Database maakt gebruik van een resource governance-implementatie die is gebaseerd op SQL Server [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor), gewijzigd en uitgebreid om uit te voeren in Azure SQL database, om resource limieten af te dwingen. In SQL Database, meerdere [resource groepen](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor-resource-pool) en [werkbelasting groepen](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor-workload-group), met resource limieten die zijn ingesteld op zowel groeps-als groeps niveau, bieden een [evenwichtige data base-as-a-Service](https://azure.microsoft.com/blog/resource-governance-in-azure-sql-database/). De werk belasting van gebruikers en interne workloads worden geclassificeerd in afzonderlijke resource groepen en werkbelasting groepen. De werk belasting van de gebruiker op de primaire en lees bare secundaire replica's, met inbegrip van geo-replica's, wordt geclassificeerd in de `SloSharedPool1` resource groep en `UserPrimaryGroup.DBId[N]` de werkbelasting groep waar `N` de data base-id-waarde staat. Daarnaast zijn er meerdere resource groepen en werkbelasting groepen voor verschillende interne workloads.

Naast het gebruik van Resource Governor voor het beheren van resources binnen het SQL-proces, gebruikt Azure SQL Database ook Windows- [taak objecten](https://docs.microsoft.com/windows/win32/procthread/job-objects) voor het beheer van resource beheer op proces niveau en Windows [File Server Resource Manager (FSRM)](https://docs.microsoft.com/windows-server/storage/fsrm/fsrm-overview) voor Storage quota management.

Azure SQL Database Resource governance is hiërarchisch van aard. Van boven naar beneden worden de limieten afgedwongen op het niveau van het besturings systeem en op het niveau van het opslag volume met behulp van de beheer mechanismen van het resource-en Resource Governor, op het niveau van de resource groep met behulp van Resource Governor en vervolgens op het niveau van de werkbelasting groep met behulp van Resource Governor. De beheer limieten voor resources die gelden voor de huidige data base of elastische pool worden opgehaald in de weer gave [sys. dm_user_db_resource_governance](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-user-db-resource-governor-azure-sql-database) .

### <a name="data-io-governance"></a>Data IO governance

Data IO governance is een proces in Azure SQL Database dat wordt gebruikt voor het beperken van fysieke IO-Lees-en schrijf bewerkingen op gegevens bestanden van een Data Base. IOPS-limieten worden voor elk service niveau ingesteld om het effect "ruis op de buur" zo klein mogelijk te maken, om resource toewijzings verdeling in de multi tenant-service te bieden en om binnen de mogelijkheden van de onderliggende hardware en opslag te blijven.

Voor afzonderlijke data bases worden de limieten voor werkbelasting groepen toegepast op alle opslag-i/o-bewerkingen op de data base, terwijl de limieten voor de resource groep van toepassing zijn op alle opslag-i/o-data bases in dezelfde SQL-groep, inclusief `tempdb` Voor elastische Pools gelden werkbelasting groeps limieten voor elke data base in de pool, terwijl de limiet voor de resource groep van toepassing is op de volledige elastische pool, met inbegrip van de `tempdb` Data Base, die wordt gedeeld door alle data bases in de groep. Over het algemeen kunnen limieten voor resource groepen niet worden behaald door de werk belasting op basis van een Data Base (enkel of gegroepeerd), omdat de limieten voor werkbelasting groepen lager zijn dan de limieten voor de resource groep en het aantal IOPS/door Voer is beperkt. Groeps limieten kunnen echter worden bereikt door de gecombineerde werk belasting op meerdere data bases in dezelfde groep.

Als een query bijvoorbeeld 1000 IOPS genereert zonder IO resource governance, maar de maximale IOPS-limiet van de werkbelasting groep is ingesteld op 900 IOPS, kan de query niet meer dan 900 IOPS genereren. Als de maximale IOPS-limiet van de resource groep is ingesteld op 1500 IOPS en het totale aantal IO-bewerkingen van alle werkbelasting groepen die zijn gekoppeld aan de resource groep meer dan 1500 IOPS overschrijdt, kan de i/o van dezelfde query worden verminderd onder de limiet van 900 IOPS voor de werk groep.

De min/max-waarden voor IOPS en door Voer die worden geretourneerd door de [sys. dm_user_db_resource_governance](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-user-db-resource-governor-azure-sql-database) -weer gave fungeren als limieten/cap's, niet als garanties. Verder biedt resource governance geen specifieke opslag latentie gegarandeerd. De best Haal bare latentie, IOPS en door Voer voor een bepaalde gebruikers werkbelasting zijn niet alleen afhankelijk van de beheer limieten voor i/o-resources, maar ook op basis van de combi natie van i/o-grootten en de mogelijkheden van de onderliggende opslag. SQL maakt gebruik van IOs die variëren van grootte tussen 512 KB en 4 MB. Met het oog op het afdwingen van IOPS-limieten wordt elke IO verwerkt ongeacht de grootte, met uitzonde ring van data bases met gegevens bestanden in Azure Storage. In dat geval wordt IOs groter dan 256 KB verwerkt als meerdere 256 KB IOs, om te worden uitgelijnd met Azure Storage IO-accounting.

Voor Basic-, Standard-en Algemeen-data bases, die gebruikmaken van gegevens bestanden in Azure Storage, `primary_group_max_io` kan de waarde mogelijk niet worden behaald als een Data Base niet genoeg gegevens bestanden bevat om cumulatief te bieden aan dit aantal IOPS, of als de gegevens niet gelijkmatig worden verdeeld over bestanden, of als de prestatie tier van onderliggende blobs de limiet voor IOPS/door Voer overschrijdt. Op dezelfde manier kan de waarde niet worden behaald door een werk belasting als gevolg van een kleine log IOs die door veelvuldige trans acties wordt gegenereerd, `primary_max_log_rate` vanwege de limiet voor IOPS voor de onderliggende Azure Storage-blob.

Bron gebruiks waarden zoals `avg_data_io_percent` en, die `avg_log_write_percent` zijn gerapporteerd in de weer gaven [sys. dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database), [sys. resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)en [sys. elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) , worden berekend als percentage van de maximale resource beheer limieten. Daarom is het mogelijk om te voor komen dat andere factoren dan het aantal IOPS/door Voer van de resource governance de IOPS/door Voer afvlakken en de latentie verhogen naarmate de werk belasting toeneemt, ook al is het gerapporteerde resource gebruik minder dan 100%.

Als u de IOPS, door Voer en latentie per database bestand voor lezen en schrijven wilt zien, gebruikt u de functie [sys. dm_io_virtual_file_stats ()](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql) . Deze functie behaalt alle i/o-bewerkingen op de data base, inclusief de i/o van de achtergrond, `avg_data_io_percent` maar gebruikt IOPS en door Voer van de onderliggende opslag, en kan invloed hebben op de waargenomen latentie van de opslag. De functie kan ook een extra latentie bedekken die kan worden geïntroduceerd door IO-resource beheer voor lees-en schrijf bewerkingen, respectievelijk in de `io_stall_queued_read_ms` `io_stall_queued_write_ms` kolommen en.

### <a name="transaction-log-rate-governance"></a>Beleid voor transactie logboek belasting

Het tarief voor transactie Logboeken is een proces in Azure SQL Database dat wordt gebruikt om een hoge opname snelheid te beperken voor werk belastingen, zoals bulksgewijs invoegen, selecteren en index builds. Deze limieten worden bijgehouden en afgedwongen op het subseconde op het niveau van de generatie van de logboek record, waardoor de door Voer wordt beperkt, ongeacht het aantal IOs-bestanden dat kan worden uitgegeven aan de hand van gegevens.  Tarieven voor het genereren van transactie logboeken worden momenteel lineair geschaald naar een punt dat is afhankelijk van de hardware, waarbij de maximale logboek snelheid van 96 MB/s met het vCore-aankoop model wordt toegestaan.

> [!NOTE]
> De daad werkelijke fysieke IOs-naar-transactie logboek bestanden zijn niet gebonden of beperkt.

De logboek tarieven zijn zodanig ingesteld dat ze in verschillende scenario's kunnen worden bereikt en onderhouden, terwijl het hele systeem de functionaliteit kan behouden met een geminimaliseerd effect op de belasting van de gebruiker. Log rate governance zorgt ervoor dat back-ups van transactie logboeken binnen de gepubliceerde herstel-Sla's blijven.  Dit bestuur voor komt ook een buitensporige achterstand op secundaire replica's.

Wanneer er logboek records worden gegenereerd, wordt elke bewerking geëvalueerd en beoordeeld of deze moet worden uitgesteld om een maximale gewenste logboek frequentie (MB/s per seconde) te behouden. De vertragingen worden niet toegevoegd wanneer de logboek records worden leeg gemaakt naar de opslag, maar de log rate governance wordt toegepast tijdens het genereren van de logboek registratie.

De werkelijke generatie tarieven voor logboek registratie die tijdens de uitvoering zijn opgelegd, kunnen ook worden beïnvloed door de feedback mechanismen, waardoor de toegestane logboek tarieven tijdelijk worden verminderd zodat het systeem kan stabiliseren. Beheer van de ruimte van het logboek bestand, voor komen dat de logboek ruimte wordt gebruikt en de replicatie mechanismen voor beschikbaarheids groepen kunnen de algehele systeem limieten tijdelijk verlagen.

De verkeers vormgeving van de logboek frequentie wordt geoppereerd via de volgende wacht typen (beschikbaar in de weer gaven [sys. dm_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) en [sys. dm_os_wait_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql) ):

| Wacht type | Opmerkingen |
| :--- | :--- |
| LOG_RATE_GOVERNOR | Data base beperken |
| POOL_LOG_RATE_GOVERNOR | Beperking van groepen |
| INSTANCE_LOG_RATE_GOVERNOR | Beperking op exemplaar niveau |  
| HADR_THROTTLE_LOG_RATE_SEND_RECV_QUEUE_SIZE | Feedback beheer, fysieke replicatie van beschikbaarheids groepen in Premium/Bedrijfskritiek niet bijhouden |  
| HADR_THROTTLE_LOG_RATE_LOG_SIZE | Feedback beheer, beperkende tarieven om te voor komen dat er een fout optreedt in de logboek ruimte |
| HADR_THROTTLE_LOG_RATE_MISMATCHED_SLO | Feedback beheer geo-replicatie, beperken van de logboek frequentie om te voor komen dat hoge gegevens latentie en niet-beschik baarheid van geo-inbaarten|
|||

Houd rekening met de volgende opties als er een frequentie limiet optreedt die de gewenste schaal baarheid belemmert:

- Schaal omhoog naar een hoger service niveau om de maximale logboek frequentie van 96 MB/s te verkrijgen, of schakel over naar een andere servicelaag. De [grootschalige](service-tier-hyperscale.md) -servicelaag levert een logboek frequentie van 100 MB per seconde, ongeacht het gekozen service niveau.
- Als gegevens die worden geladen tijdelijk zijn, zoals faserings gegevens in een ETL-proces, kan deze worden geladen in TempDB (dit is mini maal geregistreerd).
- Voor analytische scenario's laadt u in een geclusterde column Store-gedekte tabel. Dit reduceert de vereiste logboek frequentie vanwege compressie. Deze techniek verhoogt het CPU-gebruik en is alleen van toepassing op gegevens sets die profiteren van geclusterde column Store-indexen.

## <a name="next-steps"></a>Volgende stappen

- Zie [Azure-abonnement en service limieten, quota's en beperkingen](../../azure-resource-manager/management/azure-subscription-service-limits.md)voor meer informatie over algemene Azure-limieten.
- Zie [dtu's en edtu's](purchasing-models.md#dtu-based-purchasing-model)voor informatie over Dtu's en edtu's.
- Zie [tempdb in Azure SQL database](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database)voor meer informatie over de limieten voor TempDB-grootte.
