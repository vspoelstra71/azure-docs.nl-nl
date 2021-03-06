---
title: Containers controleren met Azure Monitor-logboeken
description: Gebruik Azure Monitor logboeken voor het controleren van containers die worden uitgevoerd op Azure Service Fabric-clusters.
author: srrengar
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: 8d4231de13da3f8b2960bd4852136f803a97a546
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "75614431"
---
# <a name="monitor-containers-with-azure-monitor-logs"></a>Containers controleren met Azure Monitor-logboeken
 
In dit artikel worden de stappen beschreven die nodig zijn voor het instellen van de Azure Monitor logboeken container bewakings oplossing om container gebeurtenissen weer te geven. Zie deze [Stapsgewijze zelf studie](service-fabric-tutorial-monitoring-wincontainers.md)om uw cluster in te stellen voor het verzamelen van container gebeurtenissen. 

[!INCLUDE [log-analytics-agent-note.md](../../includes/log-analytics-agent-note.md)]

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="set-up-the-container-monitoring-solution"></a>De container monitoring-oplossing instellen

> [!NOTE]
> U moet Azure Monitor-logboeken hebben ingesteld voor uw cluster en de Log Analytics-agent op uw knoop punten hebben geïmplementeerd. Als dat niet het geval is, volgt u de stappen in [Azure monitor logboeken instellen](service-fabric-diagnostics-oms-setup.md) en [de log Analytics agent eerst aan een cluster toevoegen](service-fabric-diagnostics-oms-agent.md) .

1. Als uw cluster is ingesteld met Azure Monitor-logboeken en de Log Analytics agent, implementeert u de containers. Wacht totdat de containers zijn geïmplementeerd voordat u verdergaat met de volgende stap.

2. Zoek in azure Marketplace naar de *oplossing voor container bewaking* en klik op de resource van de **container bewakings oplossing** die wordt weer gegeven onder de categorie controle en beheer.

    ![Containers-oplossing toevoegen](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. Maak de oplossing binnen dezelfde werk ruimte die al is gemaakt voor het cluster. Met deze wijziging wordt de agent automatisch geactiveerd om docker-gegevens op de containers te verzamelen. In ongeveer 15 minuten moet u het oplossings licht weer geven met behulp van binnenkomende logboeken en statistieken, zoals wordt weer gegeven in de onderstaande afbeelding.

    ![Basis Log Analytics-dash board](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

Met de agent kunt u een verzameling van verschillende container-specifieke logboeken maken die kunnen worden opgevraagd in Azure Monitor-Logboeken of worden gebruikt om prestatie-indica toren te visualiseren. De volgende logboek typen worden verzameld:

* ContainerInventory: bevat informatie over de locatie, naam en installatie kopieën van de container
* ContainerImageInventory: informatie over geïmplementeerde installatie kopieën, inclusief Id's of groottes
* ContainerLog: specifieke fout logboeken, docker-Logboeken (stdout, enzovoort) en andere vermeldingen
* ContainerServiceLog: docker daemon-opdrachten die zijn uitgevoerd
* Prestatie meter items, inclusief container CPU, geheugen, netwerk verkeer, schijf-i/o en aangepaste metrische gegevens van de hostcomputers.



## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de oplossing voor de [Azure monitor-logboeken](../azure-monitor/insights/containers.md).
* Meer informatie over container indeling op Service Fabric- [service Fabric en containers](service-fabric-containers-overview.md)
* Krijg vertrouwd met de functies voor [Zoeken in Logboeken en query's](../log-analytics/log-analytics-log-searches.md) die worden aangeboden als onderdeel van Azure monitor logboeken
* Azure Monitor logboeken configureren voor het instellen van [automatische waarschuwings](../log-analytics/log-analytics-alerts.md) regels om te helpen bij het detecteren en diagnosticeren