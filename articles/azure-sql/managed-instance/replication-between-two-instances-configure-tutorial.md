---
title: Replicatie tussen twee door Azure SQL beheerde instanties configureren
titleSuffix: Azure SQL Managed Instance
description: In deze zelf studie leert u transactionele replicatie tussen een Azure SQL Managed instance/Distributor en een SQL Managed instance Subscriber te configureren.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: ferno
ms.reviewer: mathoma
ms.date: 04/28/2020
ms.openlocfilehash: 5603c6a828eb27bec43cf1fcb1924ad3ec430685
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2020
ms.locfileid: "84051827"
---
# <a name="tutorial-configure-replication-between-two-azure-sql-managed-instances"></a>Zelf studie: replicatie tussen twee door Azure SQL beheerde instanties configureren
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Met transactionele replicatie kunt u gegevens van de ene data base repliceren naar een andere die worden gehost op SQL Server, of een [Azure SQL Managed instance](sql-managed-instance-paas-overview.md) (open bare preview). Een door SQL beheerd exemplaar kan een uitgever, distributeur of abonnee zijn in de replicatie topologie. Zie [transactionele replicatie configuraties](replication-transactional-overview.md#common-configurations) voor beschik bare configuraties.

> [!NOTE]
> In dit artikel wordt het gebruik van [transactionele replicatie](https://docs.microsoft.com/sql/relational-databases/replication/transactional/transactional-replication) in Azure SQL Managed instance beschreven. Het heeft geen betrekking op [failover-groepen](https://docs.microsoft.com/azure/sql-database/sql-database-auto-failover-group), een Azure SQL Managed instance-functie waarmee u volledig Lees bare replica's van afzonderlijke instanties kunt maken.

In deze zelf studie leert u hoe u één SQL Managed instance als de uitgever en de Distributor configureert, en vervolgens een tweede SQL-beheerd exemplaar als de abonnee.  

![Repliceren tussen twee beheerde exemplaren](./media/replication-between-two-instances-configure-tutorial/sqlmi-sqlmi-repl.png)

  > [!NOTE]
  > - Dit artikel is bedoeld om een geavanceerde gebruiker te begeleiden bij het configureren van replicatie met een SQL Managed instance van end-to-end, te beginnen met het maken van de resource groep. Als er al beheerde instanties zijn geïmplementeerd, gaat u verder met [stap 4](#4---create-a-publisher-database) om uw Publisher-data base te maken, of [stap 6](#6---configure-distribution) als u al een Publisher-en abonnee database hebt en u klaar bent om de configuratie van de replicatie te starten.  
  > - In dit artikel worden uw uitgever en distributie server geconfigureerd op hetzelfde beheerde exemplaar. Als u de Distributor wilt plaatsen op een afzonderlijke beheerd-instantie, raadpleegt u de zelf studie [replicatie configureren tussen een mi-Uitgever en een mi-distributeur](replication-two-instances-and-sql-server-configure-tutorial.md). 

## <a name="requirements"></a>Vereisten

Het configureren van een SQL Managed instance voor als een uitgever en/of een distributeur vereist het volgende:

- Of het beheerde exemplaar van de Publisher SQL-instantie zich in hetzelfde virtuele netwerk bevindt als de Distributor en de abonnee of dat [vNet-peering](../../virtual-network/tutorial-connect-virtual-networks-powershell.md) is geconfigureerd tussen de virtuele netwerken van alle drie de entiteiten. 
- Connectiviteit maakt gebruik van SQL-verificatie tussen replicatiedeelnemers.
- Een Azure Storage-account share voor de werkmap voor replicatie.
- Poort 445 (TCP uitgaand) is geopend in de beveiligings regels van NSG voor de SQL Managed instances voor toegang tot de Azure-bestands share.  Als de fout zich voordoet `failed to connect to azure storage \<storage account name> with os error 53` , moet u een regel voor uitgaande verbindingen toevoegen aan de NSG van het betreffende SQL Managed instance-subnet.

## <a name="1---create-a-resource-group"></a>1: een resource groep maken

Gebruik de [Azure Portal](https://portal.azure.com) om een resource groep met de naam te maken `SQLMI-Repl` .  

## <a name="2---create-sql-managed-instances"></a>2-SQL Managed instances maken

Gebruik de [Azure Portal](https://portal.azure.com) om twee [SQL-beheerde exemplaren](instance-create-quickstart.md) op hetzelfde virtuele netwerk en subnet te maken. Noem bijvoorbeeld de twee SQL Managed instances:

- `sql-mi-pub`(samen met enkele tekens voor wille keurige)
- `sql-mi-sub`(samen met enkele tekens voor wille keurige)

U moet ook [een Azure VM configureren om verbinding te maken](connect-vm-instance-configure.md) met uw door SQL beheerde exemplaren. 

## <a name="3---create-azure-storage-account"></a>3-Azure Storage account maken

[Maak een Azure Storage-account](/azure/storage/common/storage-create-storage-account#create-a-storage-account) voor de werkmap en maak een [Bestands share](../../storage/files/storage-how-to-create-file-share.md) in het opslag account. 

Kopieer het pad naar de bestands share met de volgende indeling:`\\storage-account-name.file.core.windows.net\file-share-name`

Voorbeeld: `\\replstorage.file.core.windows.net\replshare`

Kopieer de toegangs sleutels voor opslag in de volgende indeling:`DefaultEndpointsProtocol=https;AccountName=<Storage-Account-Name>;AccountKey=****;EndpointSuffix=core.windows.net`

Voorbeeld: `DefaultEndpointsProtocol=https;AccountName=replstorage;AccountKey=dYT5hHZVu9aTgIteGfpYE64cfis0mpKTmmc8+EP53GxuRg6TCwe5eTYWrQM4AmQSG5lb3OBskhg==;EndpointSuffix=core.windows.net`

Zie [toegangs sleutels voor opslag accounts beheren](../../storage/common/storage-account-keys-manage.md)voor meer informatie. 

## <a name="4---create-a-publisher-database"></a>4-een Publisher-data base maken

Maak verbinding met uw `sql-mi-pub` door SQL beheerd exemplaar met behulp van SQL Server Management Studio en voer de volgende Transact-SQL-code (T-SQL) uit om uw Publisher-data base te maken:

```sql
USE [master]
GO

CREATE DATABASE [ReplTran_PUB]
GO

USE [ReplTran_PUB]
GO
CREATE TABLE ReplTest (
  ID INT NOT NULL PRIMARY KEY,
  c1 VARCHAR(100) NOT NULL,
  dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO


USE [ReplTran_PUB]
GO

INSERT INTO ReplTest (ID, c1) VALUES (6, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (2, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (3, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (4, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (5, 'pub')
GO
SELECT * FROM ReplTest
GO
```

## <a name="5---create-a-subscriber-database"></a>5-een abonnee database maken

Maak verbinding met uw `sql-mi-sub` door SQL beheerd exemplaar met behulp van SQL Server Management Studio en voer de volgende T-SQL-code uit om uw lege abonnee database te maken:

```sql
USE [master]
GO

CREATE DATABASE [ReplTran_SUB]
GO

USE [ReplTran_SUB]
GO
CREATE TABLE ReplTest (
  ID INT NOT NULL PRIMARY KEY,
  c1 VARCHAR(100) NOT NULL,
  dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO
```

## <a name="6---configure-distribution"></a>6-distributie configureren

Maak verbinding met uw `sql-mi-pub` door SQL beheerd exemplaar met behulp van SQL Server Management Studio en voer de volgende T-SQL-code uit om uw distributie database te configureren.

```sql
USE [master]
GO

EXEC sp_adddistributor @distributor = @@ServerName;
EXEC sp_adddistributiondb @database = N'distribution';
GO
```

## <a name="7---configure-publisher-to-use-distributor"></a>7: de uitgever configureren voor het gebruik van Distributor

Wijzig de uitvoering van de query in de Sqlcmd-modus van uw Publisher SQL Managed instance `sql-mi-pub` en voer de volgende code uit om de nieuwe Distributor bij uw uitgever te registreren. [SQLCMD](/sql/ssms/scripting/edit-sqlcmd-scripts-with-query-editor)

```sql
:setvar username loginUsedToAccessSourceManagedInstance
:setvar password passwordUsedToAccessSourceManagedInstance
:setvar file_storage "\\storage-account-name.file.core.windows.net\file-share-name"
-- example: file_storage "\\replstorage.file.core.windows.net\replshare"
:setvar file_storage_key "DefaultEndpointsProtocol=https;AccountName=<Storage-Account-Name>;AccountKey=****;EndpointSuffix=core.windows.net"
-- example: file_storage_key "DefaultEndpointsProtocol=https;AccountName=replstorage;AccountKey=dYT5hHZVu9aTgIteGfpYE64cfis0mpKTmmc8+EP53GxuRg6TCwe5eTYWrQM4AmQSG5lb3OBskhg==;EndpointSuffix=core.windows.net"

USE [master]
EXEC sp_adddistpublisher
  @publisher = @@ServerName,
  @distribution_db = N'distribution',
  @security_mode = 0,
  @login = N'$(username)',
  @password = N'$(password)',
  @working_directory = N'$(file_storage)',
  @storage_connection_string = N'$(file_storage_key)'; -- Remove this parameter for on-premises publishers
```

   > [!NOTE]
   > Zorg ervoor dat u alleen backslashes ( `\` ) voor de para meter file_storage gebruikt. Het gebruik van een slash ( `/` ) kan een fout veroorzaken wanneer er verbinding wordt gemaakt met de bestands share.

Met dit script wordt een lokale uitgever op het beheerde exemplaar van SQL geconfigureerd, wordt een gekoppelde server toegevoegd en wordt een set taken gemaakt voor de SQL Server Agent.

## <a name="8---create-publication-and-subscriber"></a>8-publicatie en abonnee maken

Gebruik de [Sqlcmd](/sql/ssms/scripting/edit-sqlcmd-scripts-with-query-editor) -modus om het volgende T-SQL-script uit te voeren om replicatie voor uw data base in te scha kelen en de replicatie tussen uw uitgever, distributeur en abonnee server te configureren.

```sql
-- Set variables
:setvar username sourceLogin
:setvar password sourcePassword
:setvar source_db ReplTran_PUB
:setvar publication_name PublishData
:setvar object ReplTest
:setvar schema dbo
:setvar target_server "sql-mi-sub.wdec33262scj9dr27.database.windows.net"
:setvar target_username targetLogin
:setvar target_password targetPassword
:setvar target_db ReplTran_SUB

-- Enable replication for your source database
USE [$(source_db)]
EXEC sp_replicationdboption
  @dbname = N'$(source_db)',
  @optname = N'publish',
  @value = N'true';

-- Create your publication
EXEC sp_addpublication
  @publication = N'$(publication_name)',
  @status = N'active';


-- Configure your log reader agent
EXEC sp_changelogreader_agent
  @publisher_security_mode = 0,
  @publisher_login = N'$(username)',
  @publisher_password = N'$(password)',
  @job_login = N'$(username)',
  @job_password = N'$(password)';

-- Add the publication snapshot
EXEC sp_addpublication_snapshot
  @publication = N'$(publication_name)',
  @frequency_type = 1,
  @publisher_security_mode = 0,
  @publisher_login = N'$(username)',
  @publisher_password = N'$(password)',
  @job_login = N'$(username)',
  @job_password = N'$(password)';

-- Add the ReplTest table to the publication
EXEC sp_addarticle
  @publication = N'$(publication_name)',
  @type = N'logbased',
  @article = N'$(object)',
  @source_object = N'$(object)',
  @source_owner = N'$(schema)';

-- Add the subscriber
EXEC sp_addsubscription
  @publication = N'$(publication_name)',
  @subscriber = N'$(target_server)',
  @destination_db = N'$(target_db)',
  @subscription_type = N'Push';

-- Create the push subscription agent
EXEC sp_addpushsubscription_agent
  @publication = N'$(publication_name)',
  @subscriber = N'$(target_server)',
  @subscriber_db = N'$(target_db)',
  @subscriber_security_mode = 0,
  @subscriber_login = N'$(target_username)',
  @subscriber_password = N'$(target_password)',
  @job_login = N'$(username)',
  @job_password = N'$(password)';

-- Initialize the snapshot
EXEC sp_startpublication_snapshot
  @publication = N'$(publication_name)';
```

## <a name="9---modify-agent-parameters"></a>9-agent parameters wijzigen

Er zijn op dit moment problemen met de back-end van een door Azure SQL beheerd exemplaar met de connectiviteit met de replicatie agenten. Hoewel dit probleem wordt opgelost, is de tijdelijke oplossing om de time-outwaarde voor de aanmelding voor de replicatie agenten te verhogen.

Voer de volgende T-SQL-opdracht uit op de Publisher om de time-out voor de aanmelding te verhogen:

```sql
-- Increase login timeout to 150s
update msdb..sysjobsteps set command = command + N' -LoginTimeout 150'
where subsystem in ('Distribution','LogReader','Snapshot') and command not like '%-LoginTimeout %'
```

Voer de volgende T-SQL-opdracht opnieuw uit om de time-out voor de aanmelding weer in te stellen op de standaard waarde. dit moet u doen:

```sql
-- Increase login timeout to 30
update msdb..sysjobsteps set command = command + N' -LoginTimeout 30'
where subsystem in ('Distribution','LogReader','Snapshot') and command not like '%-LoginTimeout %'
```

Start alle drie de agents opnieuw om deze wijzigingen toe te passen.

## <a name="10---test-replication"></a>10-replicatie testen

Zodra de replicatie is geconfigureerd, kunt u deze testen door nieuwe items in te voegen op de Publisher en de wijzigingen te bekijken die worden door gegeven aan de abonnee.

Voer het volgende T-SQL-code fragment uit om de rijen op de abonnee weer te geven:

```sql
select * from dbo.ReplTest
```

Voer het volgende T-SQL-code fragment uit om extra rijen in te voegen op de Publisher en controleer de rijen vervolgens opnieuw op de abonnee.

```sql
INSERT INTO ReplTest (ID, c1) VALUES (15, 'pub')
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u de publicatie wilt verwijderen, voert u de volgende T-SQL-opdracht uit:

```sql
-- Drops the publication
USE [ReplTran_PUB]
EXEC sp_droppublication @publication = N'PublishData'
GO
```

Als u de replicatie optie uit de Data Base wilt verwijderen, voert u de volgende T-SQL-opdracht uit:

```sql
-- Disables publishing of the database
USE [ReplTran_PUB]
EXEC sp_removedbreplication
GO
```

Als u publiceren en distribueren wilt uitschakelen, voert u de volgende T-SQL-opdracht uit:

```sql
-- Drops the distributor
USE [master]
EXEC sp_dropdistributor @no_checks = 1
GO
```

U kunt uw Azure-resources opschonen door [de resources van SQL Managed instance van de resource groep te verwijderen](../../azure-resource-manager/management/manage-resources-portal.md#delete-resources) en vervolgens de resource groep te verwijderen `SQLMI-Repl` . 

## <a name="next-steps"></a>Volgende stappen

U kunt ook meer informatie over [transactionele replicatie met Azure SQL Managed instance](replication-transactional-overview.md) of informatie over het configureren van replicatie tussen een [SQL Managed instance/DISTRIBUTOR en een SQL op een Azure VM-abonnee](replication-two-instances-and-sql-server-configure-tutorial.md). 
