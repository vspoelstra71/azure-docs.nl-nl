---
title: Transparent Data Encryption
titleSuffix: Azure SQL Database & SQL Managed Instance & Azure Synapse
description: Een overzicht van transparante gegevens versleuteling voor Azure SQL Database, Azure SQL Managed instance en Azure Synapse. In het document worden de voor delen en de opties voor de configuratie beschreven, waaronder door de service beheerde transparante gegevens versleuteling en Bring Your Own Key.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: seo-lt-2019 sqldbrb=3
ms.devlang: ''
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
ms.reviewer: vanto
ms.date: 04/10/2020
ms.openlocfilehash: 023f00e30809126a1be7df5330f27453477ed9cf
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2020
ms.locfileid: "84048033"
---
# <a name="transparent-data-encryption-for-sql-database-sql-managed-instance--azure-synapse"></a>Transparent Data Encryption for SQL Database, SQL Managed instance & Azure Synapse
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

[Transparent Data Encryption (TDE)](/sql/relational-databases/security/encryption/transparent-data-encryption) helpt bij het beschermen van Azure SQL database, Azure SQL Managed instance en Synapse SQL in azure Synapse Analytics tegen de dreiging van schadelijke offline activiteiten door het versleutelen van gegevens in rust. Het voert in realtime versleuteling en ontsleuteling van de database, bijbehorende back-ups en transactielogboekbestanden 'at-rest' uit, zonder dat er wijzigingen in de toepassing moeten worden aangebracht. TDE is standaard ingeschakeld voor alle zojuist geïmplementeerde data bases en moet hand matig worden ingeschakeld voor oudere data bases van Azure SQL Database, Azure SQL Managed instance of Azure Synapse.

TDE voert realtime-I/O-versleuteling en ontsleuteling van de gegevens op pagina niveau uit. Elke pagina wordt ontsleuteld wanneer deze wordt ingelezen in het geheugen en vervolgens versleuteld voordat deze naar een schijf wordt geschreven. TDE versleutelt de opslag van een volledige data base met behulp van een symmetrische sleutel die de database versleutelings sleutel (DEK) wordt genoemd. Bij het opstarten van de data base wordt de versleutelde DEK ontsleuteld en vervolgens gebruikt voor ontsleuteling en opnieuw versleutelen van de database bestanden in het proces van de SQL Server data base-engine. DEK wordt beveiligd door de TDE-Protector. TDE Protector is een door een service beheerd certificaat (door de service beheerde transparante gegevens versleuteling) of een asymmetrische sleutel die is opgeslagen in [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault) (door de klant beheerde transparante gegevens versleuteling).

Voor Azure SQL Database en Azure Synapse wordt de TDE-Protector ingesteld op [Server](logical-servers.md) niveau en overgenomen door alle data bases die aan die server zijn gekoppeld. Voor Azure SQL Managed instance (BYOK-functie in Preview) wordt de TDE-Protector ingesteld op het niveau van de instantie en overgenomen door alle versleutelde data bases op dat exemplaar. De term *Server* verwijst naar de server en het exemplaar in dit document, tenzij anders aangegeven.

> [!IMPORTANT]
> Alle nieuw gemaakte data bases in SQL Database en Azure Synapse worden standaard versleuteld met behulp van door de service beheerde transparante gegevens versleuteling. Bestaande SQL-data bases die zijn gemaakt vóór 2017 en SQL-data bases die zijn gemaakt via Restore, geo-replicatie en database kopie, worden niet standaard versleuteld. Bestaande beheerde exemplaar databases die vóór februari 2019 zijn gemaakt, worden niet standaard versleuteld. Beheerde exemplaar databases die zijn gemaakt via Restore, nemen de versleutelings status over van de bron.

> [!NOTE]
> TDE kan niet worden gebruikt voor het versleutelen van de **hoofd** database in SQL database.  De **hoofd** database bevat objecten die nodig zijn om de TDe-bewerkingen uit te voeren op de gebruikers databases.

## <a name="service-managed-transparent-data-encryption"></a>Door service beheerde transparante gegevens versleuteling

In Azure is de standaard instelling voor TDE dat de DEK wordt beveiligd door een ingebouwd server certificaat. Het ingebouwde server certificaat is uniek voor elke server en het gebruikte versleutelings algoritme is AES 256. Als een Data Base zich in een geo-replicatie relatie bevindt, worden zowel de primaire als de geo-secundaire data base beveiligd door de bovenliggende server sleutel van de primaire data base. Als twee data bases zijn verbonden met dezelfde server, delen ze ook hetzelfde ingebouwde certificaat. Micro soft roteert deze certificaten automatisch in overeenstemming met het interne beveiligings beleid en de basis sleutel wordt beveiligd door een micro soft interne geheime opslag. Klanten kunnen de naleving van SQL Database en SQL Managed instance met interne beveiligings beleidsregels controleren in onafhankelijke controle rapporten van derden die beschikbaar zijn in het [micro soft vertrouwens centrum](https://servicetrust.microsoft.com/).

Micro soft verplaatst en beheert ook de sleutels naar behoefte voor geo-replicatie en herstel bewerkingen.

## <a name="customer-managed-transparent-data-encryption---bring-your-own-key"></a>Door de klant beheerde transparante gegevens versleuteling-Bring Your Own Key

Door de klant beheerde TDE wordt ook wel BYOK-ondersteuning (Bring Your Own Key) genoemd voor TDE. In dit scenario is de TDE-Protector die de DEK versleutelt, een door de klant beheerde asymmetrische sleutel, die wordt opgeslagen in een door de klant eigendom en beheerd Azure Key Vault (extern beheer systeem op basis van de cloud van Azure) en de sleutel kluis nooit verlaat. De TDE-Protector kan worden [gegenereerd door de sleutel kluis of worden overgebracht](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys) van een on-premises HSM-apparaat (Hardware Security module). SQL Database, SQL Managed instance en Azure Synapse moeten machtigingen worden verleend aan de sleutel kluis van de klant om de DEK te ontsleutelen en te versleutelen. Als de machtigingen van de server aan de sleutel kluis zijn ingetrokken, is een Data Base niet toegankelijk en worden alle gegevens versleuteld

Met TDE met Azure Key Vault-integratie kunnen gebruikers belang rijke beheer taken beheren, zoals het draaien van sleutels, sleutel kluis machtigingen, sleutel back-ups en het inschakelen van controle/rapportage op alle TDE-beveiligingen met behulp van Azure Key Vault functionaliteit. Key Vault biedt centraal sleutel beheer, maakt gebruik van nauw keurig bewaakte Hsm's en stelt schei ding van taken mogelijk te maken tussen het beheer van sleutels en gegevens om te voldoen aan de naleving van het beveiligings beleid.
Zie [transparent Data Encryption with Azure Key Vault Integration](transparent-data-encryption-byok-overview.md)(Engelstalig) voor meer informatie over BYOK voor Azure SQL database en Azure Synapse.

Als u TDE wilt gaan gebruiken met Azure Key Vault integratie, raadpleegt u de hand leiding voor het [inschakelen van transparante gegevens versleuteling met behulp van uw eigen sleutel vanuit Key Vault](transparent-data-encryption-byok-configure.md).

## <a name="move-a-transparent-data-encryption-protected-database"></a>Een transparante gegevens versleuteling verplaatsen-beveiligde data base

U hoeft geen data bases te ontsleutelen voor bewerkingen binnen Azure. De TDE-instellingen voor de bron database of primaire Data Base zijn transparant overgenomen op het doel. Beschik bare bewerkingen omvatten:

- Geo-herstel
- Self-service herstel punt in de tijd
- Herstellen van een verwijderde data base
- Actieve geo-replicatie
- Maken van een database kopie
- Back-upbestand terugzetten naar Azure SQL Managed instance

> [!IMPORTANT]
> Het maken van een hand matige kopie alleen back-ups van een Data Base die is versleuteld door service beheerde TDE wordt niet ondersteund in Azure SQL Managed instance, omdat het certificaat dat voor versleuteling wordt gebruikt, niet toegankelijk is. Gebruik de functie punt-in-tijd-herstellen om dit type Data Base te verplaatsen naar een ander door SQL beheerd exemplaar.

Wanneer u een met TDE beveiligde data base exporteert, wordt de geëxporteerde inhoud van de data base niet versleuteld. Deze geëxporteerde inhoud wordt opgeslagen in niet-versleutelde BACPAC-bestanden. Zorg ervoor dat u de BACPAC-bestanden op de juiste wijze beveiligt en TDE na het importeren van de nieuwe Data Base hebt ingeschakeld.

Als het BACPAC-bestand bijvoorbeeld wordt geëxporteerd vanuit een on-premises SQL Server-exemplaar, wordt de geïmporteerde inhoud van de nieuwe Data Base niet automatisch versleuteld. Als het BACPAC-bestand wordt geëxporteerd naar een on-premises SQL Server-exemplaar, wordt de nieuwe data base ook niet automatisch versleuteld.

De enige uitzonde ring is wanneer u exporteert naar en van een SQL Database. TDE wordt ingeschakeld in de nieuwe Data Base, maar het BACPAC-bestand zelf is nog steeds niet versleuteld.

## <a name="manage-transparent-data-encryption"></a>Transparante gegevens versleuteling beheren

# <a name="portal"></a>[Portal](#tab/azure-portal)

TDE beheren in de Azure Portal.

Als u TDE wilt configureren via de Azure Portal, moet u zijn verbonden als Azure-eigenaar, Inzender of SQL Security Manager.

Schakel TDE in en uit op het niveau van de data base. Gebruik Transact-SQL (T-SQL) voor Azure SQL Managed instance om TDE in en uit te scha kelen voor een Data Base. Voor Azure SQL Database en Azure Synapse kunt u TDE voor de data base in de [Azure Portal](https://portal.azure.com) beheren nadat u zich hebt aangemeld met de Azure-beheerder of het Inzender-account. Zoek de TDE-instellingen onder uw gebruikers database. Standaard wordt door service beheerde transparante gegevens versleuteling gebruikt. Er wordt automatisch een TDE-certificaat gegenereerd voor de server die de Data Base bevat.

![Door service beheerde transparante gegevens versleuteling](./media/transparent-data-encryption-tde-overview/service-managed-transparent-data-encryption.png)  

U stelt de hoofd sleutel TDE, ook wel de TDE-Protector, op het niveau van de server of het exemplaar in. Als u TDE wilt gebruiken met BYOK-ondersteuning en uw data bases wilt beveiligen met een sleutel van Key Vault, opent u de TDE-instellingen onder uw server.

![Transparante gegevens versleuteling met Bring Your Own Key-ondersteuning](./media/transparent-data-encryption-tde-overview/tde-byok-support.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

TDE beheren met behulp van Power shell.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
> [!IMPORTANT]
> De Power shell-Azure Resource Manager module wordt nog steeds ondersteund, maar alle toekomstige ontwikkeling is voor de module AZ. SQL. Zie [AzureRM. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)voor deze cmdlets. De argumenten voor de opdrachten in de module AZ en in de AzureRm-modules zijn aanzienlijk identiek.

Als u TDE via Power shell wilt configureren, moet u zijn verbonden met de Azure-eigenaar, Inzender of SQL Security Manager.

### <a name="cmdlets-for-azure-sql-database-and-azure-synapse"></a>Cmdlets voor Azure SQL Database en Azure Synapse

Gebruik de volgende cmdlets voor Azure SQL Database en Azure Synapse:

| Cmdlet | Beschrijving |
| --- | --- |
| [Set-AzSqlDatabaseTransparentDataEncryption](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasetransparentdataencryption) |Hiermee wordt de transparante gegevens versleuteling voor een data base in-of uitgeschakeld.|
| [Get-AzSqlDatabaseTransparentDataEncryption](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption) |Hiermee wordt de transparante status van gegevens versleuteling opgehaald voor een Data Base. |
| [Get-AzSqlDatabaseTransparentDataEncryptionActivity](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasetransparentdataencryptionactivity) |Controleert de voortgang van de versleuteling voor een Data Base. |
| [Add-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/add-azsqlserverkeyvaultkey) |Hiermee voegt u een Key Vault sleutel toe aan een server. |
| [Get-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlserverkeyvaultkey) |Hiermee worden de Key Vault sleutels voor een server opgehaald  |
| [Set-AzSqlServerTransparentDataEncryptionProtector](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) |Hiermee stelt u de transparante beveiliging voor gegevens versleuteling in voor een-server. |
| [Get-AzSqlServerTransparentDataEncryptionProtector](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlservertransparentdataencryptionprotector) |Hiermee wordt de transparante beveiliging van gegevens versleuteling opgehaald |
| [Remove-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlserverkeyvaultkey) |Hiermee verwijdert u een Key Vault sleutel van een server. |
|  | |

> [!IMPORTANT]
> Voor Azure SQL Managed instance gebruikt u de T-SQL [ALTER data base](/sql/t-sql/statements/alter-database-azure-sql-database) -opdracht om TDe in en uit te scha kelen op een database niveau en voor [beeld Power shell-script](transparent-data-encryption-byok-configure.md) voor het beheren van TDe op instantie niveau te controleren.

# <a name="transact-sql"></a>[Transact-SQL](#tab/azure-TransactSQL)

TDE beheren met behulp van Transact-SQL.

Maak verbinding met de data base met behulp van een aanmelding die een beheerder of lid is van de rol **DBManager** in de hoofd database.

| Opdracht | Beschrijving |
| --- | --- |
| [ALTER data base (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) | VERSLEUTELING instellen in-of uitschakelen voor het versleutelen of ontsleutelen van een Data Base |
| [sys. dm_database_encryption_keys](/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql) |Retourneert informatie over de versleutelings status van een Data Base en de bijbehorende versleutelings sleutels voor de data base |
| [sys. dm_pdw_nodes_database_encryption_keys](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql) |Retourneert informatie over de versleutelings status van elk Azure Synapse-knoop punt en de bijbehorende versleutelings sleutels voor de data base |
|  | |

U kunt de TDE-Protector niet overschakelen naar een sleutel van Key Vault met behulp van Transact-SQL. Gebruik Power shell of de Azure Portal.

# <a name="rest-api"></a>[REST API](#tab/azure-RESTAPI)

TDE beheren met behulp van de REST API.

Als u TDE wilt configureren via de REST API, moet u zijn verbonden als Azure-eigenaar, Inzender of SQL Security Manager.
Gebruik de volgende reeks opdrachten voor Azure SQL Database en Azure Synapse:

| Opdracht | Beschrijving |
| --- | --- |
|[Server maken of bijwerken](https://docs.microsoft.com/rest/api/sql/servers/createorupdate)|Hiermee voegt u een Azure Active Directory identiteit toe aan een server. (wordt gebruikt om toegang te verlenen aan Key Vault)|
|[Server sleutel maken of bijwerken](https://docs.microsoft.com/rest/api/sql/serverkeys/createorupdate)|Hiermee voegt u een Key Vault sleutel toe aan een server.|
|[Server sleutel verwijderen](https://docs.microsoft.com/rest/api/sql/serverkeys/delete)|Hiermee verwijdert u een Key Vault sleutel van een server. |
|[Server sleutels ophalen](https://docs.microsoft.com/rest/api/sql/serverkeys/get)|Hiermee wordt een specifieke Key Vault sleutel van een server opgehaald.|
|[Server sleutels op server weer geven](https://docs.microsoft.com/rest/api/sql/serverkeys/listbyserver)|Hiermee worden de Key Vault sleutels voor een server opgehaald. |
|[Versleutelings beveiliging maken of bijwerken](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/createorupdate)|Hiermee stelt u de TDE-Protector voor een server in.|
|[Versleutelings beveiliging ophalen](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/get)|Hiermee wordt de TDE-Protector voor een server opgehaald.|
|[Versleutelings beveiligingen op server vermelden](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/listbyserver)|Hiermee worden de TDE-beveiligingen voor een server opgehaald. |
|[Transparent Data Encryption configuratie maken of bijwerken](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/createorupdate)|Hiermee wordt de TDE voor een data base in-of uitgeschakeld.|
|[Transparent Data Encryption-configuratie ophalen](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/get)|Hiermee wordt de TDE-configuratie voor een Data Base opgehaald.|
|[Resultaten van Transparent Data Encryption configuratie weer geven](https://docs.microsoft.com/rest/api/sql/transparentdataencryptionactivities/listbyconfiguration)|Hiermee wordt het versleutelings resultaat voor een Data Base opgehaald.|

## <a name="see-also"></a>Zie ook

- SQL Server die op een virtuele Azure-machine worden uitgevoerd, kunt u ook een asymmetrische sleutel van Key Vault gebruiken. De configuratie stappen verschillen van het gebruik van een asymmetrische sleutel in SQL Database en SQL Managed instance. Zie [Extensible Key Management met behulp van Azure Key Vault (SQL Server)](/sql/relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server)voor meer informatie.
- Zie [transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption)(Engelstalig) voor een algemene beschrijving van TDe.
- Zie voor meer informatie over TDE met BYOK-ondersteuning voor Azure SQL Database, Azure SQL Managed instance en Azure Synapse, [transparent Data Encryption with Bring your own key support](transparent-data-encryption-byok-overview.md).
- Als u TDE wilt gaan gebruiken met Bring Your Own Key-ondersteuning, raadpleegt u de hand leiding om [transparant gegevens versleuteling in te scha kelen met behulp van uw eigen sleutel vanuit Key Vault](transparent-data-encryption-byok-configure.md).
- Zie [Secure Access to a key kluis](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault)(Engelstalig) voor meer informatie over Key Vault.
