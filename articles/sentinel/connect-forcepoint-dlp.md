---
title: Force Point DLP verbinden met Azure Sentinel | Microsoft Docs
description: Meer informatie over het verbinden van Force Point DLP met Azure Sentinel.
services: sentinel
author: yelevin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/20/2020
ms.author: yelevin
ms.openlocfilehash: 3bdb9d2b23ce05929ba5612e0c6a03fe1aab05de
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "77588243"
---
# <a name="connect-your-forcepoint-dlp-to-azure-sentinel"></a>Uw Force Point DLP verbinden met Azure Sentinel

> [!IMPORTANT]
> De DLP-gegevens connector (Force Point data verlies preventie) in azure Sentinel is momenteel beschikbaar als open bare preview. Deze functie wordt zonder service level agreement gegeven en wordt niet aanbevolen voor productie werkbelastingen. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie voor meer informatie [aanvullende gebruiks voorwaarden voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



Met de Force Point DLP-connector kunt u automatisch DLP-incident gegevens exporteren naar Azure Sentinel. U kunt deze gebruiken om inzicht te krijgen in de gebruikers activiteiten en incidenten voor gegevens verlies. Het schakelt ook correlaties uit met gegevens van Azure-workloads en andere feeds, en verbetert de bewakings mogelijkheden met werkmappen binnen Azure Sentinel.

> [!NOTE]
> Gegevens worden opgeslagen in de geografische locatie van de werk ruimte waarop u Azure Sentinel uitvoert.

## <a name="configure-and-connect-forcepoint-dlp"></a>Force Point DLP configureren en verbinden

Configureer Force Point DLP om incident gegevens in JSON-indeling door te sturen naar uw Azure-werk ruimte via REST API zoals wordt uitgelegd in de [Force Point DLP-integratie handleiding](https://frcpnt.com/dlp-sentinel).


## <a name="find-your-data"></a>Uw gegevens zoeken

Nadat de Force Point DLP-connector is ingesteld, worden de gegevens weer gegeven in Log Analytics onder CustomLogs **ForcepointDLPEvents_CL**.


Als u het relevante schema in Log Analytics voor Force Point DLP wilt gebruiken, zoekt u naar **ForcepointDLPEvents_CL**.


## <a name="validate-connectivity"></a>Connectiviteit valideren

Het kan 20 minuten duren voordat uw logboeken in Log Analytics worden weer gegeven.

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe u Force Point DLP kunt verbinden met Azure Sentinel. Raadpleeg de volgende artikelen voor meer informatie over Azure Sentinel:

- Meer informatie over hoe u [inzicht krijgt in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Ga aan de slag [met het detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Gebruik werkmappen](tutorial-monitor-your-data.md) om uw gegevens te bewaken.