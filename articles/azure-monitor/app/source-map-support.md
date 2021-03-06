---
title: Ondersteuning van bron toewijzing voor Java script-toepassingen-Azure Monitor Application Insights
description: Meer informatie over het uploaden van bron kaarten naar uw eigen opslag account BLOB-container met behulp van Application Insights.
ms.topic: conceptual
author: markwolff
ms.author: marwolff
ms.date: 03/04/2020
ms.openlocfilehash: 4b452b31338760a8f53eed54420319101836bc00
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "79474880"
---
# <a name="source-map-support-for-javascript-applications"></a>Ondersteuning van bron toewijzing voor Java script-toepassingen

Application Insights ondersteunt het uploaden van bron kaarten naar uw eigen opslag account BLOB-container.
Bron kaarten kunnen worden gebruikt voor het unminify van aanroep stacks op de pagina end-to-end trans actie Details. Uitzonde ringen die worden verzonden door de [Java script-SDK][ApplicationInsights-JS] of de [node. js-SDK][ApplicationInsights-Node.js] kunnen worden unminified met bron kaarten.

![Een aanroep stack Unminify door te koppelen met een opslag account](./media/source-map-support/details-unminify.gif)

## <a name="create-a-new-storage-account-and-blob-container"></a>Een nieuw opslag account en BLOB-container maken

Als u al een bestaand opslag account of BLOB-container hebt, kunt u deze stap overs Laan.

1. [Een nieuw opslagaccount maken][create storage account]
2. [Maak een BLOB-container][create blob container] in uw opslag account. Stel het ' open bare toegangs niveau ' in `Private`om ervoor te zorgen dat uw bron kaarten niet openbaar toegankelijk zijn.

> [!div class="mx-imgBorder"]
>![Het toegangs niveau van de container moet worden ingesteld op privé](./media/source-map-support/container-access-level.png)

## <a name="push-your-source-maps-to-your-blob-container"></a>Uw bron kaarten naar uw BLOB-container pushen

U moet uw continue implementatie pijplijn met uw opslag account integreren door deze zo te configureren dat uw bron kaarten automatisch naar de geconfigureerde BLOB-container worden geüpload. Upload uw bron kaarten niet naar een submap in de BLOB-container. momenteel wordt de bron map alleen opgehaald uit de hoofdmap.

### <a name="upload-source-maps-via-azure-pipelines-recommended"></a>Bron kaarten uploaden via Azure-pijp lijnen (aanbevolen)

Als u Azure-pijp lijnen gebruikt om voortdurend uw toepassing te bouwen en te implementeren, voegt u een [Azure-Kopieer][azure file copy] taak toe aan uw pijp lijn om automatisch uw bron kaarten te uploaden.

> [!div class="mx-imgBorder"]
> ![Een Azure File Copy-taak toevoegen aan uw pijp lijn om uw bron kaarten te uploaden naar Azure Blob Storage](./media/source-map-support/azure-file-copy.png)

## <a name="configure-your-application-insights-resource-with-a-source-map-storage-account"></a>Uw Application Insights resource configureren met een opslag account voor de bron map

### <a name="from-the-end-to-end-transaction-details-page"></a>Van de pagina end-to-end trans actie Details

Op het tabblad end-to-end trans acties kunt u klikken op *Unminify* . er wordt dan een prompt weer gegeven om te configureren als uw bron niet is geconfigureerd.

1. Bekijk de details van een uitzonde ring die minified is in de portal.
2. Klik op *Unminify*
3. Als uw bron niet is geconfigureerd, wordt er een bericht weer gegeven waarin u wordt gevraagd om te configureren.

### <a name="from-the-properties-page"></a>Op de pagina eigenschappen

Als u het opslag account of de BLOB-container wilt configureren of wijzigen die aan uw Application Insights resource is gekoppeld, kunt u dit doen door het tabblad *Eigenschappen* van Application Insights resource weer te geven.

1. Ga naar het tabblad *Eigenschappen* van de Application Insights resource.
2. Klik op *wijzigen bron toewijzing BLOB container*.
3. Selecteer een andere blob-container als uw bron kaarten container.
4. Klik op `Apply`.

> [!div class="mx-imgBorder"]
> ![Configureer uw geselecteerde Azure Blob-container opnieuw door naar de Blade eigenschappen te navigeren](./media/source-map-support/reconfigure.png)

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="required-role-based-access-control-rbac-settings-on-your-blob-container"></a>Vereiste RBAC-instellingen (op rollen gebaseerd toegangs beheer) in de BLOB-container

Alle gebruikers op de portal die deze functie gebruiken, moeten ten minste worden toegewezen als een [gegevens lezer][storage blob data reader] voor de opslag-BLOB aan uw BLOB-container. U moet deze rol toewijzen aan iemand anders die de bron kaarten via deze functie gaat gebruiken.

> [!NOTE]
> Afhankelijk van hoe de container is gemaakt, is dit mogelijk niet automatisch toegewezen aan u of uw team.

### <a name="source-map-not-found"></a>Bron toewijzing is niet gevonden

1. Controleer of de bijbehorende bron toewijzing is geüpload naar de juiste BLOB-container
2. Controleer of het bron toewijzings bestand de naam heeft nadat het Java script-bestand is toegewezen aan `.map`, met een achtervoegsel.
    - Zoekt bijvoorbeeld `/static/js/main.4e2ca5fa.chunk.js` naar de blob met de naam`main.4e2ca5fa.chunk.js.map`
3. Controleer de console van uw browser om te zien of er fouten worden geregistreerd. Neem deze op in een ondersteunings ticket.

## <a name="next-steps"></a>Volgende stappen

* [Azure File Copy-taak](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-file-copy?view=azure-devops)


<!-- Remote URLs -->
[create storage account]: https://docs.microsoft.com/azure/storage/common/storage-account-create?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-portal
[create blob container]: https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal
[storage blob data reader]: https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader
[ApplicationInsights-JS]: https://github.com/microsoft/applicationinsights-js
[ApplicationInsights-Node.js]: https://github.com/microsoft/applicationinsights-node.js
[azure file copy]: https://aka.ms/azurefilecopyreadme