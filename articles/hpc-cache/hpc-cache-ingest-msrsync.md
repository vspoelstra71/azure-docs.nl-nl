---
title: Gegevens opname in de Azure HPC-cache-msrsync
description: Msrsync gebruiken om gegevens te verplaatsen naar een Blob Storage-doel in azure HPC cache
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: rohogue
ms.openlocfilehash: 2e0442b6aa1404ae5f57445179979496faa09863
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "82194972"
---
# <a name="azure-hpc-cache-data-ingest---msrsync-method"></a>Azure HPC cache data opname-msrsync-methode

In dit artikel vindt u gedetailleerde instructies voor ``msrsync`` het gebruik van het hulp programma voor het kopiëren van gegevens naar een Azure Blob Storage-container voor gebruik met de Azure HPC-cache.

Lees [gegevens verplaatsen naar Azure Blob Storage](hpc-cache-ingest.md)voor meer informatie over het verplaatsen van gegevens naar Blob Storage voor uw Azure HPC-cache.

Het ``msrsync`` hulp programma kan worden gebruikt om gegevens te verplaatsen naar een back-end-opslag doel voor de Azure HPC-cache. Dit hulp programma is ontworpen om het bandbreedte gebruik te optimaliseren door ``rsync`` meerdere parallelle processen uit te voeren. Het is beschikbaar via GitHub op https://github.com/jbd/msrsync.

``msrsync``Hiermee wordt de bron directory opgesplitst in afzonderlijke buckets en worden vervolgens afzonderlijke ``rsync`` processen uitgevoerd op elke Bucket.

Voor bereiding van het testen met behulp van een virtuele machine met vier kernen wordt de beste efficiëntie weer gegeven wanneer u 64 processen Gebruik de ``msrsync`` optie ``-p`` om het aantal processen in te stellen op 64.

Houd er ``msrsync`` rekening mee dat alleen naar en van lokale volumes kan worden geschreven. De bron en het doel moeten toegankelijk zijn als lokale koppels op het werk station dat wordt gebruikt voor het uitgeven van de opdracht.

Volg deze instructies voor ``msrsync`` het vullen van Azure Blob-opslag met Azure HPC cache:

1. Installeren ``msrsync`` en voldoen aan de vereisten (``rsync`` en Python 2,6 of hoger)
1. Bepaal het totale aantal bestanden en mappen dat moet worden gekopieerd.

   Gebruik bijvoorbeeld het hulp programma ``prime.py`` met de argumenten ```prime.py --directory /path/to/some/directory``` (beschikbaar door te <https://github.com/Azure/Avere/blob/master/src/clientapps/dataingestor/prime.py>downloaden).

   Als u dit ``prime.py``niet gebruikt, kunt u het aantal items met het hulp ``find`` programma GNU als volgt berekenen:

   ```bash
   find <path> -type f |wc -l         # (counts files)
   find <path> -type d |wc -l         # (counts directories)
   find <path> |wc -l                 # (counts both)
   ```

1. Deel het aantal items door 64 om het aantal items per proces te bepalen. Gebruik dit nummer met de ``-f`` optie om de grootte van de buckets in te stellen wanneer u de opdracht uitvoert.

1. Geef de ``msrsync`` opdracht om bestanden te kopiëren:

   ```bash
   msrsync -P --stats -p64 -f<ITEMS_DIV_64> --rsync "-ahv --inplace" <SOURCE_PATH> <DESTINATION_PATH>
   ```

   Deze opdracht is bijvoorbeeld ontworpen om 11.000-bestanden in 64 processen van/test/Source-Repository naar/mnt/hpccache/repository te verplaatsen:

   ``mrsync -P --stats -p64 -f170 --rsync "-ahv --inplace" /test/source-repository/ /mnt/hpccache/repository``
