---
title: Verificatie
description: Meer informatie over het verifiëren van Azure Synapse Analytics met behulp van Azure Active Directory (Azure AD) of SQL Server-verificatie.
services: synapse-analytics
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/02/2019
ms.author: jrasnick
ms.reviewer: igorstan
ms.custom: seo-lt-2019
tag: azure-synapse
ms.openlocfilehash: 868130b23bc29acf0f1271f68a45f44cee98fe98
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2020
ms.locfileid: "84015621"
---
# <a name="authenticate-to-azure-synapse-analytics"></a>Verifiëren bij Azure Synapse Analytics

Meer informatie over het verifiëren van Synapse SQL in azure Synapse met behulp van Azure Active Directory (AAD) of SQL Server-verificatie.

Als u verbinding wilt maken met een SQL-groep, moet u beveiligings referenties door geven voor verificatie doeleinden. Bij het tot stand brengen van een verbinding worden bepaalde Verbindings instellingen geconfigureerd als onderdeel van het maken van de query sessie.  

Zie [securing a Data Base documentation](sql-data-warehouse-overview-manage-security.md)(Engelstalig) voor meer informatie over beveiliging en het inschakelen van verbindingen met uw data warehouse.

## <a name="sql-authentication"></a>SQL-verificatie

Als u verbinding wilt maken met de SQL-groep, moet u de volgende informatie opgeven:

* Volledig gekwalificeerde servername
* SQL-verificatie opgeven
* Gebruikersnaam
* Wachtwoord
* Standaard database (optioneel)

De verbinding maakt standaard verbinding met de *hoofd* database en niet op uw gebruikers database. Als u verbinding wilt maken met uw gebruikers database, kunt u kiezen uit een van de volgende twee dingen:

* Geef de standaard database op wanneer u uw server registreert bij de SQL Server-objectverkenner in SSDT, SSMS of in uw toepassings connection string. Neem bijvoorbeeld de para meter InitialCatalog op voor een ODBC-verbinding.
* Markeer de gebruikers database voordat u een sessie maakt in SSDT.

> [!NOTE]
> De Transact-SQL-instructie **use MyDatabase;** wordt niet ondersteund voor het wijzigen van de Data Base voor een verbinding. Raadpleeg het artikel [query with Visual Studio](sql-data-warehouse-query-visual-studio.md) voor hulp bij het maken van verbinding met een SQL-groep met SSDT.

## <a name="azure-active-directory-aad-authentication"></a>Azure Active Directory-verificatie (AAD)

[Azure Active Directory](../../active-directory/fundamentals/active-directory-whatis.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) -verificatie is een mechanisme om verbinding te maken met de SQL-groep met behulp van identiteiten in azure Active Directory (Azure AD). Met Azure Active Directory-verificatie kunt u de identiteiten van database gebruikers en andere micro soft-services centraal beheren op één centrale locatie. Centraal-ID-beheer biedt één locatie voor het beheren van Azure Synapse-gebruikers en vereenvoudigt het beheer van machtigingen.

### <a name="benefits"></a>Voordelen

Azure Active Directory voor delen zijn onder andere:

* Biedt een alternatief voor SQL Server-verificatie.
* Helpt de verspreiding van gebruikers identiteiten op meerdere servers te stoppen.
* Maakt rotatie van wachtwoorden op één plek mogelijk
* Beheer database machtigingen met externe groepen (Azure AD).
* Elimineert het opslaan van wacht woorden door geïntegreerde Windows-verificatie in te scha kelen en o Ndere-verificatie methoden die door Azure Active Directory worden ondersteund.
* Gebruikt Inge sloten database gebruikers voor het verifiëren van identiteiten op database niveau.
* Biedt ondersteuning voor verificatie op basis van tokens voor toepassingen die verbinding maken met de SQL-groep.
* Ondersteunt multi-factor Authentication via Active Directory universele verificatie voor verschillende hulpprogram ma's, waaronder [SQL Server Management Studio](../../azure-sql/database/authentication-mfa-ssms-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) en [SQL Server Data Tools](/sql/ssdt/azure-active-directory?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

> [!NOTE]
> Azure Active Directory nog steeds relatief nieuw is en enkele beperkingen heeft. Zie [Azure AD-functies en-beperkingen](../../azure-sql/database/authentication-aad-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#azure-ad-features-and-limitations), met name de aanvullende overwegingen, om ervoor te zorgen dat Azure Active Directory geschikt is voor uw omgeving.

### <a name="configuration-steps"></a>Configuratiestappen

Volg deze stappen om Azure Active Directory-verificatie te configureren.

1. Een Azure Active Directory maken en vullen
2. Optioneel: de Active Directory die momenteel aan uw Azure-abonnement is gekoppeld, koppelen of wijzigen
3. Een Azure Active Directory beheerder maken voor Azure Synapse
4. Uw client computers configureren
5. Inge sloten database gebruikers in uw data base maken die zijn toegewezen aan Azure AD-identiteiten
6. Verbinding maken met uw SQL-groep met behulp van Azure AD-identiteiten

Momenteel worden gebruikers Azure Active Directory niet weer gegeven in SSDT Objectverkenner. Als tijdelijke oplossing kunt u de gebruikers weer geven in [sys. database_principals](/sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).

### <a name="find-the-details"></a>De details zoeken

* De stappen voor het configureren en gebruiken van Azure Active Directory verificatie zijn bijna identiek voor Azure SQL Database en Synapse SQL in azure Synapse. Volg de gedetailleerde stappen in het onderwerp [verbinding maken met SQL database of SQL-groep met behulp van Azure Active Directory-verificatie](../../azure-sql/database/authentication-aad-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).
* Aangepaste database rollen maken en gebruikers toevoegen aan de rollen. Ken vervolgens gedetailleerde machtigingen toe aan de rollen. Zie aan de slag met de machtigingen voor de [Data base-engine](/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Zie Query's uitvoeren met [Visual Studio](sql-data-warehouse-query-visual-studio.md)voor informatie over het starten van Query's met Visual Studio en andere toepassingen.
