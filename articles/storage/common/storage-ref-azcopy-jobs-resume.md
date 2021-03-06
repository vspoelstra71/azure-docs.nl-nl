---
title: azcopy-taken hervatten | Microsoft Docs
description: In dit artikel vindt u Naslag informatie voor de opdracht azcopy Jobs hervatten.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 3ee7879475801660b5200dddca88a0a81b2b6b9b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/27/2020
ms.locfileid: "74034146"
---
# <a name="azcopy-jobs-resume"></a>azcopy jobs resume

Hiermee wordt de bestaande taak hervat met de opgegeven taak-ID.

## <a name="synopsis"></a>Samen vatting

```azcopy
azcopy jobs resume [jobID] [flags]
```

## <a name="related-conceptual-articles"></a>Gerelateerde conceptuele artikelen

- [Aan de slag met AzCopy](storage-use-azcopy-v10.md)
- [Gegevens overdragen met AzCopy en Blob Storage](storage-use-azcopy-blobs.md)
- [Gegevens overdragen met AzCopy en File Storage](storage-use-azcopy-files.md)
- [AzCopy configureren, optimaliseren en problemen oplossen](storage-use-azcopy-configure.md)

## <a name="options"></a>Opties

|Optie|Beschrijving|
|--|--|
|--doel-SAS-teken reeks|De doel-SAS van het doel voor de opgegeven JobId.|
|--teken reeks uitsluiten|Filter: deze mislukte overdracht (en) uitsluiten wanneer de taak wordt hervat. Bestanden moeten worden gescheiden door '; '.|
|-h,--Help|Help-inhoud weer geven voor de opdracht hervatten.|
|--reeks toevoegen|Filter: deze mislukte overdracht (en) zijn alleen beschikbaar wanneer de taak wordt hervat. Bestanden moeten worden gescheiden door '; '.|
|--SAS-teken reeks |Bron-SAS van de bron voor de opgegeven JobId.|

## <a name="options-inherited-from-parent-commands"></a>Opties overgenomen van bovenliggende opdrachten

|Optie|Beschrijving|
|---|---|
|--Cap-Mbps uint32|De overdrachts frequentie in megabits per seconde. Even door Voer kan enigszins afwijken van het kapje. Als deze optie is ingesteld op nul of wordt wegge laten, wordt de door Voer niet afgetopt.|
|--type teken reeks voor uitvoer|De indeling van de uitvoer van de opdracht. De opties zijn onder andere: Text, JSON. De standaard waarde is "text".|

## <a name="see-also"></a>Zie ook

- [azcopy jobs](storage-ref-azcopy-jobs.md)
