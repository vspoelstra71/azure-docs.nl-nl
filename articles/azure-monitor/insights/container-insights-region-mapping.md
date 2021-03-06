---
title: Azure Monitor voor de regio toewijzingen voor containers
description: In dit artikel worden de regio toewijzingen beschreven die worden ondersteund tussen Azure Monitor voor containers, Log Analytics werk ruimte en aangepaste metrische gegevens.
ms.topic: conceptual
ms.date: 06/26/2019
ms.openlocfilehash: a058f9cac987bb5c7130019f50370c6a176b09ac
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "75403426"
---
# <a name="region-mappings-supported-by-azure-monitor-for-containers"></a>Regio toewijzingen die worden ondersteund door Azure Monitor voor containers

 Als Azure Monitor voor containers wordt ingeschakeld, worden alleen bepaalde regio's ondersteund voor het koppelen van een Log Analytics-werk ruimte en een AKS-cluster en het verzamelen van aangepaste metrische gegevens die aan Azure Monitor zijn verzonden.

## <a name="log-analytics-workspace-supported-mappings"></a>Ondersteunde toewijzingen Log Analytics werk ruimte

De AKS-cluster resources of Log Analytics werk ruimte kunnen zich in andere regio's bevinden, en in de volgende tabel ziet u de toewijzingen.

|**AKS-cluster regio** | **Log Analytics werkruimte regio** |
|-----------------------|------------------------------------|
|**Afrika** | |
|SouthAfricaNorth |West-Europa |
|SouthAfricaWest |West-Europa |
|**Australië** | |
|AustraliaEast |AustraliaEast |
|AustraliaCentral |AustraliaCentral |
|AustraliaCentral2 |AustraliaCentral |
|AustraliaEast |AustraliaEast |
|**Azië en Stille Oceaan** | |
|EastAsia |EastAsia |
|SoutheastAsia |SoutheastAsia |
|**Brazilië** | |
|BrazilSouth | SouthCentralUS |
|**Canada** ||
|CanadaCentral |CanadaCentral |
|CanadaEast |CanadaCentral |
|**Europa** | |
|FranceCentral |FranceCentral |
|FranceSouth |FranceCentral |
|NorthEurope |NorthEurope |
|UKSouth |UKSouth |
|UKWest |UKSouth |
|West-Europa |West-Europa |
|**India** | |
|CentralIndia |CentralIndia |
|SouthIndia |CentralIndia |
|WestIndia |CentralIndia |
|**Japan** | |
|JapanEast |JapanEast |
|JapanWest |JapanEast |
|**Korea** | |
|KoreaCentral |KoreaCentral |
|KoreaSouth |KoreaCentral |
|**VS** | |
|CentralUS |CentralUS|
|EastUS |EastUS |
|EastUS2 |EastUS2 |
|WestUS |WestUS |
|WestUS2 |WestUS2 |
|WestCentralUS<sup>1</sup>|Oost,<sup>1</sup>|
|VS (overheid) - Virginia |VS (overheid) - Virginia |

<sup>1</sup> vanwege capaciteits beperkingen is de regio niet beschikbaar bij het maken van nieuwe resources. Dit omvat een Log Analytics-werk ruimte. Bestaande gekoppelde resources in de regio moeten echter blijven werken.

## <a name="custom-metrics-supported-regions"></a>Ondersteunde regio's voor aangepaste metrische gegevens

Het verzamelen van metrische gegevens van knoop punten van Azure Kubernetes Services (AKS)-clusters en peulen wordt alleen ondersteund voor publicatie als aangepaste metrische gegevens in de volgende [Azure-regio's](../platform/metrics-custom-overview.md#supported-regions).

## <a name="next-steps"></a>Volgende stappen

Als u wilt beginnen met het bewaken van uw AKS-cluster, raadpleegt u [How to Enable the Azure monitor for containers](container-insights-onboard.md) voor meer informatie over de vereisten en beschik bare methoden voor het inschakelen van  
