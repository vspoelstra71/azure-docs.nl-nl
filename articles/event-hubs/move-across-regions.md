---
title: Een Azure Event Hubs-naam ruimte naar een andere regio verplaatsen | Microsoft Docs
description: In dit artikel leest u hoe u een Azure Event Hubs-naam ruimte van de huidige regio naar een andere regio kunt verplaatsen.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: how-to
ms.custom: subject-moving-resources
ms.date: 04/14/2020
ms.author: spelluru
ms.reviewer: shvija
ms.openlocfilehash: 5b96bf1c538b3c5589a1993a0353292fadd0936d
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2020
ms.locfileid: "82690494"
---
# <a name="move-an-azure-event-hubs-namespace-to-another-region"></a>Een Azure Event Hubs-naam ruimte verplaatsen naar een andere regio
Er zijn verschillende scenario's waarin u uw bestaande Event Hubs naam ruimte wilt verplaatsen van de ene regio naar een andere. Het is bijvoorbeeld mogelijk dat u een naam ruimte met dezelfde configuratie voor testen wilt maken. U kunt ook een secundaire naam ruimte in een andere regio maken als onderdeel van de [planning voor nood herstel](event-hubs-geo-dr.md#setup-and-failover-flow).

> [!NOTE]
> In dit artikel wordt beschreven hoe u een Azure Resource Manager sjabloon exporteert voor een bestaande Event Hubs naam ruimte en vervolgens de sjabloon gebruikt om een naam ruimte te maken met dezelfde configuratie-instellingen in een andere regio. Met dit proces worden echter geen gebeurtenissen verplaatst die nog niet zijn verwerkt. U moet de gebeurtenissen van de oorspronkelijke naam ruimte verwerken voordat u deze verwijdert.

## <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat de services en functies die uw account gebruikt worden ondersteund in de doel regio.
- Zorg ervoor dat uw abonnement white list is voor de doel regio voor preview-functies.
- Als u de **functie vastleggen** hebt ingeschakeld voor Event hubs in de naam ruimte, verplaatst u [Azure Storage of Azure data Lake Store gen 2](../storage/common/storage-account-move.md) -of [Azure data Lake Store gen 1](../data-lake-store/data-lake-store-migration-cross-region.md) -accounts voordat u de Event hubs naam ruimte verplaatst. U kunt ook de resource groep met zowel opslag-als Event Hubs-naam ruimten verplaatsen naar de andere regio door de stappen te volgen die vergelijkbaar zijn met die in dit artikel worden beschreven. 
- Als de naam ruimte van de Event Hubs zich in een **Event hubs cluster**bevindt, [maakt u een toegewezen cluster](event-hubs-dedicated-cluster-create-portal.md) in de **doel regio** voordat u de stappen in dit artikel uitvoert. U kunt ook de Quick Start- [sjabloon op github](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-cluster-namespace-eventhub/) gebruiken om een event hubs cluster te maken. Verwijder in de sjabloon het naam ruimte gedeelte van de JSON om alleen het cluster te maken. 

## <a name="prepare"></a>Voorbereiden
Exporteer een resource manager-sjabloon om aan de slag te gaan. Deze sjabloon bevat instellingen die uw Event Hubs naam ruimte beschrijven.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer **alle resources** en selecteer vervolgens uw event hubs naam ruimte.

3. Selecteer > **instellingen** > **sjabloon exporteren**.

4. Kies **downloaden** op de pagina **sjabloon exporteren** .

    ![Resource Manager-sjabloon downloaden](./media/move-across-regions/download-template.png)

5. Zoek het zip-bestand dat u hebt gedownload van de portal en pak het bestand uit naar een map van uw keuze.

   Dit zip-bestand bevat de. json-bestanden die de sjabloon en scripts bevatten voor het implementeren van de sjabloon.


## <a name="move"></a>Verplaatsen

Implementeer de sjabloon om een Event Hubs naam ruimte te maken in de doel regio. 


1. Selecteer in Azure Portal **Een resource maken**.

2. In **Marketplace doorzoeken** typt u **sjabloonimplementatie**. Druk vervolgens op **ENTER**.

3. Selecteer **Sjabloonimplementatie**.

4. Selecteer **Maken**.

5. Selecteer **Bouw uw eigen sjabloon in de editor**.

6. Selecteer **bestand laden**en volg de instructies voor het laden van het **sjabloon. json** -bestand dat u hebt gedownload in de laatste sectie.

7. Selecteer **Opslaan** om de sjabloon op te slaan. 

8. Voer op de pagina **aangepaste implementatie** de volgende stappen uit: 

    1. Selecteer een Azure- **abonnement**. 

    2. Selecteer een bestaande **resource groep** of maak een. Als de bron naam ruimte zich in een Event Hubs cluster bevindt, selecteert u de resource groep die het cluster in de doel regio bevat. 

    3. Selecteer de doel **locatie** of-regio. Als u een bestaande resource groep hebt geselecteerd, is deze instelling alleen-lezen. 

    4. Voer de volgende stappen uit in de sectie **instellingen** :
    
        1. Voer de naam van de nieuwe **naam ruimte**in. 

            ![Resource Manager-sjabloon implementeren](./media/move-across-regions/deploy-template.png)

        2. Als uw bron naam ruimte zich in een **Event hubs cluster**bevindt, voert u de namen van de **resource groep** en het **Event hubs cluster** in als onderdeel van de **externe ID**. 

              ```
              /subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<CLUSTER'S RESOURCE GROUP>/providers/Microsoft.EventHub/clusters/<CLUSTER NAME>
              ```   
        3. Als Event Hub in uw naam ruimte een opslag account gebruikt voor het vastleggen van gebeurtenissen, geeft u de naam van de resource groep `StorageAccounts_<original storage account name>_external` en het veld opslag account voor op. 
            
            ```
            /subscriptions/0000000000-0000-0000-0000-0000000000000/resourceGroups/<STORAGE'S RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>
            ```    
    5. Schakel het selectie vakje **Ik ga akkoord met de bovenstaande voor waarden in** . 
    
    6. Selecteer nu **aankoop selecteren** om het implementatie proces te starten. 

## <a name="discard-or-clean-up"></a>Verwijderen of opschonen
Als u na de implementatie wilt beginnen, kunt u de naam ruimte van het **doel event hubs**verwijderen en herhaalt u de stappen die worden beschreven in de secties [voorbereiden](#prepare) en [verplaatsen](#move) van dit artikel.

Als u de wijzigingen wilt door voeren en het verplaatsen van een Event Hubs naam ruimte wilt volt ooien, verwijdert u de **bron Event hubs naam ruimte**. Zorg ervoor dat u alle gebeurtenissen in de naam ruimte hebt verwerkt voordat u de naam ruimte verwijdert. 

Een Event Hubs naam ruimte (bron of doel) verwijderen met behulp van de Azure Portal:

1. Typ **Event hubs**in het venster zoeken aan de bovenkant van Azure Portal en selecteer **Event hubs** in de zoek resultaten. U ziet de Event Hubs naam ruimten in een lijst.

2. Selecteer de doel naam ruimte die u wilt verwijderen en selecteer **verwijderen** op de werk balk. 

    ![Naam ruimte verwijderen-knop](./media/move-across-regions/delete-namespace-button.png)

3. Controleer op de pagina **resources verwijderen*** de geselecteerde resources en bevestig de verwijdering door **Ja**te typen en vervolgens **verwijderen**te selecteren. 

## <a name="next-steps"></a>Volgende stappen

In deze zelf studie hebt u een Azure Event Hubs-naam ruimte verplaatst van de ene regio naar een andere en de bron resources opgeschoond.  Raadpleeg voor meer informatie over het verplaatsen van resources tussen regio's en herstel na nood gevallen in Azure:


- [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Virtuele Azure-machines verplaatsen naar een andere regio](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
