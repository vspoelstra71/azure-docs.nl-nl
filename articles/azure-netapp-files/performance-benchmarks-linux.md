---
title: Azure NetApp Files prestatie benchmarks voor Linux | Microsoft Docs
description: Beschrijft de benchmarks voor prestaties Azure NetApp Files levert voor Linux.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/29/2020
ms.author: b-juche
ms.openlocfilehash: b763a734866dd5fed5bf0500d4d52b9324c92a79
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2020
ms.locfileid: "82614588"
---
# <a name="azure-netapp-files-performance-benchmarks-for-linux"></a>Azure NetApp Files prestatie benchmarks voor Linux

In dit artikel worden de benchmarks voor prestaties beschreven Azure NetApp Files levert voor Linux.

## <a name="linux-scale-out"></a>Linux-uitschalen

In deze sectie vindt u een beschrijving van de prestatie benchmarks van de Linux-werk belasting en IOPS-belasting.

### <a name="linux-workload-throughput"></a>Door Voer van Linux-werk belasting  

De onderstaande grafiek vertegenwoordigt een KiB-sequentiële werk belasting (64-kibibyte) en een 1-TiB werkset. Er wordt weer gegeven dat één Azure NetApp Files volume kan worden verwerkt tussen ~ 1.600 MiB/s zuivere opeenvolgende schrijf bewerkingen en ~ 4.500 MiB/s zuivere Lees bewerkingen.  

In de grafiek ziet u minder dan 10% per keer, van zuivere Lees bewerking tot zuivere schrijf bewerkingen. Er wordt gedemonstreerd wat u kunt verwachten wanneer u verschillende Lees-en schrijf ratio's gebruikt (100%: 0%, 90%: 10%, 80%: 20%, enzovoort).

![Door Voer van Linux-werk belasting](../media/azure-netapp-files/performance-benchmarks-linux-workload-throughput.png)  

### <a name="linux-workload-iops"></a>IOPS van de Linux-werk belasting  

De volgende grafiek vertegenwoordigt een wille keurige werk belasting van 4 kibibyte (KiB) en een met 1 TiB werkende set. In de grafiek ziet u dat een Azure NetApp Files volume kan worden verwerkt tussen ~ 130.000 zuivere wille keurige schrijf bewerkingen en ~ 460.000 zuivere wille keurige Lees bewerkingen.  

In deze grafiek ziet u minder dan 10% per keer, van zuivere Lees bewerking tot zuivere schrijf bewerkingen. Er wordt gedemonstreerd wat u kunt verwachten wanneer u verschillende Lees-en schrijf ratio's gebruikt (100%: 0%, 90%: 10%, 80%: 20%, enzovoort).

![IOPS van de Linux-werk belasting](../media/azure-netapp-files/performance-benchmarks-linux-workload-iops.png)  

## <a name="linux-scale-up"></a>Linux-schaalbaar  

Linux 5,3 kernel maakt scale-out netwerken met één client mogelijk voor NFS`nconnect`. In de grafieken in deze sectie worden de resultaten van de validatie tests voor de koppel optie aan client zijde weer gegeven met NFSv3. De functie is beschikbaar op SUSE (te beginnen met SLES12SP4) en Ubuntu (te beginnen met de 19,10-versie). Het is vergelijkbaar met het concept van SMB meerdere kanalen en Oracle direct NFS.

De grafieken vergelijken de voor delen `nconnect` van aan een niet-verbonden gekoppeld volume. In de grafieken heeft FIO de werk belasting van een enkele D32s_v3-instantie gegenereerd in de Azure-regio US-west2.

### <a name="linux-read-throughput"></a>Lees doorvoer voor Linux  

In de volgende grafieken worden de opeenvolgende Lees bewerkingen van de ~ 3.500 MiB `nconnect`/s weer gegeven met,`nconnect`ongeveer 2,3 x niet.

![Lees doorvoer voor Linux](../media/azure-netapp-files/performance-benchmarks-linux-read-throughput.png)  

### <a name="linux-write-throughput"></a>Linux-schrijf doorvoer  

In de volgende grafieken worden sequentiële Schrijf bewerkingen weer gegeven. Ze geven aan `nconnect` dat er geen merkbaar voor deel is voor opeenvolgende schrijf bewerkingen. 1.500 MiB/s is ongeveer de bovengrens van het sequentiële Schrijf volume en de limiet van het D32s_v3-exemplaar.

![Linux-schrijf doorvoer](../media/azure-netapp-files/performance-benchmarks-linux-write-throughput.png)  

### <a name="linux-read-iops"></a>Door Linux gelezen IOPS  

In de volgende grafieken ziet u wille keurige Lees bewerkingen `nconnect`van ~ 200.000 Read IOPS`nconnect`met, ongeveer 3x niet-.

![Door Linux gelezen IOPS](../media/azure-netapp-files/performance-benchmarks-linux-read-iops.png)  

### <a name="linux-write-iops"></a>Linux write IOPS  

In de volgende grafieken ziet u wille keurige schrijf bewerkingen `nconnect`van ~ 135.000 write IOPS`nconnect`met, ongeveer 3x niet-.

![Linux write IOPS](../media/azure-netapp-files/performance-benchmarks-linux-write-iops.png)  

## <a name="next-steps"></a>Volgende stappen

- [Azure NetApp Files: optimaal profiteren van uw Cloud opslag](https://cloud.netapp.com/hubfs/Resources/ANF%20PERFORMANCE%20TESTING%20IN%20TEMPLATE.pdf?hsCtaTracking=f2f560e9-9d13-4814-852d-cfc9bf736c6a%7C764e9d9c-9e6b-4549-97ec-af930247f22f)
