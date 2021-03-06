---
title: Opnemen in Azure Data Lake Storage Gen2
description: Meer informatie over het opnemen van gegevens in Azure Data Lake Storage Gen2 in azure Synapse Analytics
services: synapse-analytics
author: djpmsft
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: daperlov
ms.reviewer: jrasnick
ms.openlocfilehash: 7b844bcf45417fefc7dd78a26d5dae0b2ce03249
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2020
ms.locfileid: "82982901"
---
# <a name="ingesting-data-into-azure-data-lake-storage-gen2"></a>Gegevens opnemen in Azure Data Lake Storage Gen2 

In dit artikel leert u hoe u gegevens opneemt van de ene locatie naar de andere in een opslag account van Azure Data Lake gen 2 (Azure Data Lake gen 2) met behulp van Azure Synapse Analytics.

## <a name="prerequisites"></a>Vereisten

* **Azure-abonnement**: als u nog geen Azure-abonnement hebt, maakt u een [gratis Azure-account](https://azure.microsoft.com/free/) voordat u begint.
* **Azure Storage account**: u Azure data Lake gen 2 gebruiken als een *brongegevens* opslag. Als u geen opslag account hebt, raadpleegt u [een Azure Storage-account maken](../../storage/blobs/data-lake-storage-quickstart-create-account.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) om er een te maken.

## <a name="create-linked-services"></a>Gekoppelde services maken

In azure Synapse Analytics is een gekoppelde service waar u de verbindings gegevens voor andere services definieert. In deze sectie voegt u Azure Synapse Analytics en Azure Data Lake gen 2 toe als gekoppelde services.

1. Open Azure Synapse Analytics UX en ga naar het tabblad **beheren** .
1. Onder **externe verbindingen**selecteert u **gekoppelde services**.
1. Klik op **Nieuw**om een gekoppelde service toe te voegen.
1. Selecteer de tegel Azure Data Lake Storage Gen2 in de lijst en klik op **door gaan**.
1. Voer uw verificatie referenties in. Account sleutel, Service-Principal en beheerde identiteit worden momenteel ondersteunde verificatie typen. Klik op verbinding testen om te controleren of uw referenties correct zijn. 
1. Klik op **maken** wanneer u klaar bent.

## <a name="create-pipeline"></a>Pijplijn maken

Een pijp lijn bevat de logische stroom voor het uitvoeren van een reeks activiteiten. In deze sectie maakt u een pijp lijn met een Kopieer activiteit waarmee gegevens uit Azure Data Lake gen 2 worden opgenomen in een SQL-groep.

1. Ga naar het tabblad **Orchestration** . Klik op het plus pictogram naast de pijp lijnen kop en selecteer **pijp lijn**.
1. Sleep onder **verplaatsen en transformeren** in het deel venster activiteiten de gegevens naar het pijp lijn papier **kopiëren** .
1. Klik op de Kopieer activiteit en ga naar het tabblad **bron** . Klik op **Nieuw** om een nieuwe bron gegevensset te maken.
1. Selecteer Azure Data Lake Storage Gen2 als uw gegevens archief en klik op door gaan.
1. Selecteer DelimitedText als uw indeling en klik op door gaan.
1. Selecteer in het deel venster set Properties de ADLS-gekoppelde service die u hebt gemaakt. Geef het bestandspad van de bron gegevens op en geef op of de eerste rij een koptekst heeft. U kunt het schema importeren uit het bestands archief of een voorbeeld bestand. Klik op OK wanneer u klaar bent.
1. Ga naar het tabblad **sink** . Klik op **Nieuw** om een nieuwe Sink-gegevensset te maken.
1. Selecteer Azure Data Lake Storage Gen2 als uw gegevens archief en klik op door gaan.
1. Selecteer DelimitedText als uw indeling en klik op door gaan.
1. Selecteer in het deel venster set Properties de ADLS-gekoppelde service die u hebt gemaakt. Geef het pad op van de map waarnaar u gegevens wilt schrijven. Klik op OK wanneer u klaar bent.

## <a name="debug-and-publish-pipeline"></a>Fouten opsporen en pijp lijn publiceren

Zodra u klaar bent met het configureren van de pijp lijn, kunt u een debug-uitvoering uitvoeren voordat u uw artefacten publiceert om te controleren of alles juist is.

1. Selecteer **Fouten opsporen** om fouten op te sporen in de pijplijn. De status van de pijplijnuitvoering wordt weergegeven op het tabblad **Uitvoer** onder in het venster. 
1. Zodra de pijp lijn kan worden uitgevoerd, selecteert u in de bovenste werk balk **Alles publiceren**. Deze actie publiceert entiteiten (gegevens sets en pijp lijnen) die u hebt gemaakt voor de Synapse Analytics-service.
1. Wacht tot u het bericht **Publiceren gelukt** ziet. Klik in de rechter bovenhoek op de knop Bell om meldings berichten weer te geven. 


## <a name="trigger-and-monitor-the-pipeline"></a>De pijp lijn activeren en bewaken

In deze stap moet u de pijp lijn die u in de vorige stap hebt gepubliceerd hand matig activeren. 

1. Selecteer **trigger toevoegen** op de werk balk en selecteer **nu activeren**. Klik op de pagina **Pijplijnuitvoering** op **Voltooien**.  
1. Ga naar het tabblad **monitor** in de zijbalk links. U ziet een pijplijn die wordt geactiveerd door een handmatige trigger. U kunt via de links in de kolom **Acties** details van de activiteiten bekijken en de pijplijn opnieuw uitvoeren.
1. Selecteer de link **Uitvoeringen van activiteit weergeven** in de kolom **Acties** om de activiteituitvoeringen te zien die zijn gekoppeld aan de pijplijnuitvoering. In dit voor beeld is er slechts één activiteit, zodat er slechts één vermelding in de lijst wordt weer gegeven. Selecteer de link **Details** (pictogram van een bril) in de kolom **Acties** om details over de kopieerbewerking te zien. Selecteer de **pijp lijn wordt** aan de bovenkant uitgevoerd om terug te gaan naar de weer gave pijplijn uitvoeringen. Selecteer **Vernieuwen** om de weergave te vernieuwen.
1. Controleer of uw gegevens correct zijn geschreven in de SQL-groep.


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over gegevens integratie voor Synapse Analytics het [opnemen van gegevens in een SQL-groeps](data-integration-sql-pool.md) artikel.
