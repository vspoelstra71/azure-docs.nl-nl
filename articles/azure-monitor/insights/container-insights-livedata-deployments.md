---
title: Azure Monitor voor implementaties van containers weer geven (preview) | Microsoft Docs
description: In dit artikel wordt een overzicht gegeven van de real-time weergave van Kubernetes-implementaties zonder kubectl te gebruiken in Azure Monitor voor containers.
ms.topic: conceptual
ms.date: 10/15/2019
ms.openlocfilehash: 7d0344851e1db8c014a1bb16b228a0c2f76444d5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "75404779"
---
# <a name="how-to-view-deployments-preview-in-real-time"></a>Implementaties (preview) in realtime weer geven

Met Azure Monitor voor containers wordt met de functie voor weer gave-implementaties (preview) direct toegang tot Kubernetes-implementatie objecten in realtime geëmuleerd door `kubeclt get deployments` de `kubectl describe deployment {your deployment}` opdrachten en weer te geven. 

>[!NOTE]
>AKS-clusters die zijn ingeschakeld als [persoonlijke clusters](https://azure.microsoft.com/updates/aks-private-cluster/) , worden niet ondersteund met deze functie. Deze functie is afhankelijk van het rechtstreeks openen van de Kubernetes-API via een proxy server vanuit uw browser. Door netwerk beveiliging in te scha kelen, wordt dit verkeer geblokkeerd door de Kubernetes-API van deze proxy te blok keren. 

>[!NOTE]
>Deze functie is beschikbaar in alle Azure-regio's, inclusief Azure China. Het is momenteel niet beschikbaar in de Amerikaanse overheid van Azure.

Raadpleeg de Kubernetes-documentatie over [implementaties](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)voor meer informatie. 

## <a name="how-it-works"></a>Hoe werkt het?

De functie voor Live gegevens (preview) heeft rechtstreeks toegang tot de Kubernetes-API en aanvullende informatie over het verificatie model vindt u [hier](https://kubernetes.io/docs/concepts/overview/kubernetes-api/). 

De implementatie van de functie (preview-versie) voert een eenmalige laad bewerking uit voor het eind punt `/apis/apps/v1/deployments`implementaties. Hiermee kunt u een bepaalde implementatie selecteren en de details van het beschrijven voor die specifieke implementatie op basis van het `/apis/apps/v1/namespaces/${nameSpace}/deployments/${deploymentName}`implementatie-eind punt laden. 

Als u **vernieuwen** in de linkerbovenhoek van de pagina selecteert, wordt de implementatie lijst vernieuwd. Hiermee simuleert u het opnieuw uitvoeren `kubectl` van de opdracht. 

>[!IMPORTANT]
>Er worden geen gegevens permanent opgeslagen tijdens de werking van deze functie. Alle gegevens die tijdens de sessie zijn vastgelegd, worden verwijderd wanneer u de browser sluit of verlaat.  

>[!NOTE]
>U kunt geen gegevens van live data (preview) aan een Azure-dash board vastmaken vanuit de-console.

## <a name="deployments-describe"></a>Implementaties beschrijven

Voer de volgende stappen uit om de details van de beschrijving voor een `kubectl describe deployment`implementatie weer te geven. Dit is gelijk aan.

1. Blader in het Azure Portal naar de cluster resource groep AKS en selecteer uw AKS-resource.

2. Kies op het AKS-cluster dashboard onder **bewaking** aan de linkerkant de optie **inzichten**. 

3. Selecteer het tabblad **implementaties (voor beeld)** .

    ![De weer gave implementaties in de Azure Portal](./media/container-insights-livedata-deployments/deployment-view.png)

In de weer gave ziet u een lijst met alle actieve implementaties samen met de naam ruimte en andere gedetailleerde informatie, waardoor de uitvoering `kubectl get deployments –all-namespaces`van de opdracht wordt geëmuleerd. U kunt de resultaten sorteren door een van de kolommen te selecteren. 

![Details van het deel venster implementaties eigenschappen](./media/container-insights-livedata-deployments/deployment-properties-pane-details.png)

Wanneer u een implementatie uit de lijst selecteert, wordt er automatisch een eigenschappen venster weer gegeven aan de rechter kant van de pagina. Hierin wordt informatie weer gegeven met betrekking tot de geselecteerde implementatie die u zou zien als u `kubectl describe deployment {deploymentName}`de opdracht hebt uitgevoerd. Mogelijk hebt u opgemerkt dat de informatie over de beschrijving geen details bevat. Met name de **sjabloon** ontbreekt. Als u het tabblad **RAW** selecteert, kunt u naar de niet-geparseerde beschrijvings gegevens navigeren.  

![Eigenschappen venster implementaties onbewerkte gegevens](./media/container-insights-livedata-deployments/deployment-properties-pane-raw.png)

Wanneer u de implementatie details bekijkt, kunt u de container logboeken en gebeurtenissen in realtime zien. Selecteer de console venster **Live-console weer geven** en de weer gave live data (preview) onder het gegevens raster implementaties waarin u live-logboek gegevens in een doorlopende stroom kunt bekijken. Als de status indicator ophalen een groen vinkje bevat dat helemaal rechts in het deel venster staat, betekent dit dat de gegevens kunnen worden opgehaald en streamen naar uw-console.

U kunt ook filteren op naam ruimte of gebeurtenissen op cluster niveau. Zie voor meer informatie over de realtime weergave gegevens in de-console [Live gegevens weer geven (preview) met Azure monitor voor containers](container-insights-livedata-overview.md). 

![Implementaties Live-gegevens weer geven in de-console](./media/container-insights-livedata-deployments/deployments-console-view-events.png)

## <a name="next-steps"></a>Volgende stappen

- Zie [Azure Kubernetes service Health weer geven](container-insights-analyze.md)voor meer informatie over het gebruik van Azure monitor en het controleren van andere aspecten van uw AKS-cluster.

- Bekijk de [voor beelden van logboek query's](container-insights-log-search.md#search-logs-to-analyze-data) om vooraf gedefinieerde query's en voor beelden te bekijken om waarschuwingen, visualisaties of verdere analyse van uw clusters te maken.
