---
title: SQL Server Integration Services SSIS-taken (on-premises) migreren naar Azure Data Factory
description: In dit artikel wordt beschreven hoe u SQL Server Integration Services (SSIS)-taken migreert naar Azure Data Factory pijp lijnen/activiteiten/triggers met behulp van SQL Server Management Studio.
services: data-factory
documentationcenter: ''
author: chugugrace
ms.author: chugu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 4/7/2020
ms.openlocfilehash: b27fe2abc50396b527e61487acf9797db59c1cce
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2020
ms.locfileid: "82627582"
---
# <a name="migrate-sql-server-agent-jobs-to-adf-with-ssms"></a>SQL Server Agent-taken migreren naar ADF met SSMS

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bij het [migreren van de workloads van on-premises SQL Server Integration Services (SSIS) naar SSIS in ADF](scenario-ssis-migration-overview.md), kunt u nadat SSIS-pakketten zijn gemigreerd, batch migratie van SQL Server Agent taken uitvoeren met het type taak stap van SQL Server Integration services pakket naar Azure Data Factory (ADF) pijp lijnen/activiteiten/plannings triggers via SQL Server Management Studio (SSMS) **SSIS-taak migratie wizard**.

Over het algemeen kunt u voor geselecteerde SQL-Agent taken met de betreffende taak stap typen voor de **SSIS-taak migratie** het volgende doen:

- de locatie van het op-premises SSIS-pakket toewijzen aan waar de pakketten worden gemigreerd, die toegankelijk zijn voor SSIS in ADF.
    > [!NOTE]
    > De pakket locatie van het bestands systeem wordt alleen ondersteund.
- Migreer toepasselijke taken met de betreffende taak stappen naar de bijbehorende ADF-resources, zoals hieronder wordt beschreven:

|SQL Agent-taak object  |ADF-resource  |Opmerkingen|
|---------|---------|---------|
|SQL-Agent taak|pijp lijn     |De naam van de pijp lijn wordt *gegenereerd voor \<de taak naam>*. <br> <br> Ingebouwde Agent taken zijn niet van toepassing: <li> Onderhouds taak voor SSIS-server <li> syspolicy_purge_history <li> collection_set_ * <li> mdw_purge_data_ * <li> sysutility_ *|
|SSIS-taak stap|SSIS-pakket activiteit uitvoeren|<li> De naam van de activiteit wordt \<stap naam>. <li> Het proxy account dat in de taak stap wordt gebruikt, wordt gemigreerd als Windows-verificatie van deze activiteit. <li> *Uitvoerings opties* , met uitzonde ring van de *32-bits runtime* die is gedefinieerd in de taak stap, worden genegeerd tijdens de migratie. <li> De *verificatie* die in taak stap is gedefinieerd, wordt genegeerd tijdens de migratie.|
|schedule      |trigger voor schema        |De naam van de schema trigger wordt *gegenereerd voor \<de schema naam>*. <br> <br> De onderstaande opties in de SQL Agent-taak planning worden genegeerd tijdens de migratie: <li> Interval van het tweede niveau. <li> *Automatisch starten wanneer SQL Server Agent wordt gestart* <li> *Starten wanneer de Cpu's inactief worden* <li> dag van de *week* en het *weekend*<time zone> <br> Hieronder vindt u de verschillen nadat het taak schema van de SQL-Agent is gemigreerd naar een ADF-schema trigger: <li> De automatische uitvoering van de ADF-schema-trigger is onafhankelijk van de uitvoerings status van de door de voorafgaande trigger geactiveerde uitvoering. <li> De terugkeer patroon configuratie van de ADF-trigger wijkt af van de dagelijkse frequentie in de SQL-Agent taak.|

- Genereer Azure Resource Manager sjablonen (ARM) in een lokale uitvoermap en implementeer direct of later hand matig naar data factory. Zie [resource typen micro soft. DataFactory](https://docs.microsoft.com/azure/templates/microsoft.datafactory/allversions)voor meer informatie over ADF-Resource Manager-sjablonen.

## <a name="prerequisites"></a>Vereisten

Voor de functie die in dit artikel wordt beschreven, is SQL Server Management Studio versie 18,5 of hoger vereist. Zie [down load SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15)als u de meest recente versie van SSMS wilt ophalen.

## <a name="migrate-ssis-jobs-to-adf"></a>SSIS-taken migreren naar ADF

1. In SSMS selecteert u in Objectverkenner SQL Server Agent, selecteert u taken, klikt u met de rechter muisknop en selecteert **u SSIS-taken migreren naar ADF**.
![snelmenu's](media/how-to-migrate-ssis-job-ssms/menu.png)

1. Meld u aan bij Azure, selecteer Azure-abonnement, Data Factory en Integration Runtime. Azure Storage is optioneel. dit wordt gebruikt in de stap toewijzing van pakket locatie als SSIS-taken moeten worden gemigreerd SSIS-bestandssysteem pakketten.
![snelmenu's](media/how-to-migrate-ssis-job-ssms/step1.png)

1. Wijs de paden van SSIS-pakketten en configuratie bestanden in SSIS-taken toe aan doel paden waarvoor gemigreerde pijp lijnen toegang hebben. In deze toewijzings stap kunt u het volgende doen:

    1. Selecteer een bronmap en vervolgens **toewijzing toevoegen**.
    1. Pad naar de bronmap bijwerken. Geldige paden zijn mappaden of paden van bovenliggende mappen van pakketten.
    1. Het pad naar de doelmap bijwerken. De standaard waarde is een relatief pad naar het standaard opslag account dat is geselecteerd in stap 1.
    1. Een geselecteerde toewijzing verwijderen via **toewijzing verwijderen**.
![step2](media/how-to-migrate-ssis-job-ssms/step2.png)
![step2-1](media/how-to-migrate-ssis-job-ssms/step2-1.png)

1. Selecteer de van toepassing zijnde taken die moeten worden gemigreerd en configureer de instellingen van de bijbehorende *uitgevoerde SSIS-pakket activiteit*.

    - *Standaard instelling*, is standaard van toepassing op alle geselecteerde stappen. Zie het *tabblad Instellingen* voor de [activiteit uitvoeren SSIS-pakket](how-to-invoke-ssis-package-ssis-activity.md) wanneer de pakket locatie *Bestands systeem is (pakket)* voor meer informatie over elke eigenschap.
    ![stap 3-1](media/how-to-migrate-ssis-job-ssms/step3-1.png)
    - *Stap instelling*, Configureer de instelling voor een geselecteerde stap.
        
        **Standaard instelling Toep assen**: standaard is geselecteerd. Schakel het selectie vakje uit om de instelling alleen voor de geselecteerde stap te configureren.  
        Zie voor meer informatie over andere eigenschappen het *tabblad Instellingen* voor de [activiteit SSIS-pakket uitvoeren](how-to-invoke-ssis-package-ssis-activity.md) wanneer de pakket locatie *Bestands systeem is (pakket)*.
    ![stap 3-2](media/how-to-migrate-ssis-job-ssms/step3-2.png)

1. ARM-sjabloon genereren en implementeren.
    1. Het uitvoerpad selecteren of invoeren voor de ARM-sjablonen van de gemigreerde ADF-pijp lijnen. De map wordt automatisch gemaakt als deze nog niet bestaat.
    2. Selecteer de optie voor het **implementeren van arm-sjablonen voor uw Data Factory**:
        - De standaard instelling is niet geselecteerd. U kunt gegenereerde ARM-sjablonen later hand matig implementeren.
        - Selecteer deze optie om direct gegenereerde ARM-sjablonen te implementeren op data factory.
    ![step4](media/how-to-migrate-ssis-job-ssms/step4.png)

1. Migreer en controleer vervolgens de resultaten.
![step5](media/how-to-migrate-ssis-job-ssms/step5.png)

## <a name="next-steps"></a>Volgende stappen

[Pijp lijn uitvoeren en bewaken](how-to-invoke-ssis-package-ssis-activity.md)
