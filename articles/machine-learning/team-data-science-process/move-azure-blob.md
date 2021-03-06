---
title: Gegevens verplaatsen van en naar Azure Blob-opslag-team data Science process
description: Gegevens verplaatsen van en naar Azure Blob-opslag met behulp van Azure Storage Explorer, AzCopy, python en SSIS.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: fc58651bcb3b266b981fb953fd7341427d47fb2c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "76717587"
---
# <a name="move-data-to-and-from-azure-blob-storage"></a>Gegevens verplaatsen van en naar Azure Blob-opslag

Voor het proces van de team data Science moeten gegevens worden opgenomen of geladen in verschillende opslag omgevingen, zodat ze op de meest geschikte manier in elke fase van het proces kunnen worden verwerkt of geanalyseerd.

## <a name="different-technologies-for-moving-data"></a>Verschillende technologieën voor het verplaatsen van gegevens

In de volgende artikelen wordt beschreven hoe u gegevens kunt verplaatsen van en naar Azure Blob-opslag met behulp van verschillende technologieën.

* [Azure Storage-Verkenner](move-data-to-azure-blob-using-azure-storage-explorer.md)
* [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10)
* [Python](move-data-to-azure-blob-using-python.md)
* [SSIS](move-data-to-azure-blob-using-ssis.md)

Welke methode het beste voor u is, is afhankelijk van uw scenario. De [scenario's voor geavanceerde analyses in azure machine learning](plan-sample-scenarios.md) -artikel helpen u bij het bepalen van de bronnen die u nodig hebt voor diverse data Science-werk stromen die worden gebruikt in het geavanceerde analyse proces.

> [!NOTE]
> Raadpleeg Azure Blob- [basis beginselen](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) en [Azure Blob service](https://msdn.microsoft.com/library/azure/dd179376.aspx)voor een volledige inleiding tot Azure Blob-opslag.
> 
> 

## <a name="using-azure-data-factory"></a>Azure Data Factory gebruiken

Als alternatief kunt u [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) gebruiken voor het volgende: 

* een pijp lijn maken en plannen die gegevens uit Azure Blob-opslag downloadt, 
* door sturen naar een gepubliceerde Azure Machine Learning-webservice, 
* Ontvang de predictive analytics resultaten en 
* Upload de resultaten naar de opslag. 

Zie [voorspellende pijp lijnen maken met Azure Data Factory en Azure machine learning](../../data-factory/transform-data-using-machine-learning.md)voor meer informatie.

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u een Azure-abonnement, een opslag account en de bijbehorende opslag sleutel voor dat account hebt. Voordat u gegevens kunt uploaden/downloaden, moet u de account naam en de account sleutel van Azure Storage kennen.

* Zie [een gratis proef versie van één maand](https://azure.microsoft.com/pricing/free-trial/)voor meer informatie over het instellen van een Azure-abonnement.
* Zie [over Azure Storage-accounts](../../storage/common/storage-create-storage-account.md)voor instructies over het maken van een opslag account en voor het ophalen van account-en sleutel gegevens.

