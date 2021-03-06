---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: cedd302453e75c037f17cc47d8e00a06d3f3a8cd
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2020
ms.locfileid: "83996789"
---
Ga naar de Azure Portal en <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer" title=" Maak een nieuwe formulier Recognizer-resource " target="_blank"> een nieuwe formulier herkenner maken <span class="docon docon-navigate-external x-hidden-focus"></span> </a> . Geef in het deel venster **maken** de volgende informatie op:

|    |    |
|--|--|
| **Naam** | Een beschrijvende naam voor uw resource. U kunt het beste een beschrijvende naam gebruiken, bijvoorbeeld *MyNameFormRecognizer*. |
| **Abonnement** | Selecteer het Azure-abonnement waaraan toegang is verleend. |
| **Locatie** | De locatie van uw cognitieve service-exemplaar. Verschillende locaties kunnen latentie introduceren, maar hebben geen invloed op de runtime-Beschik baarheid van uw resource. |
| **Prijs categorie** | De kosten van uw resource zijn afhankelijk van de prijs categorie die u kiest en uw gebruik. Zie de [prijs informatie](https://azure.microsoft.com/pricing/details/cognitive-services/)voor de API voor meer informatie.
| **Resourcegroep** | De [Azure-resource groep](https://docs.microsoft.com/azure/cloud-adoption-framework/govern/resource-consistency/resource-access-management#what-is-an-azure-resource-group) die uw resource bevat. U kunt een nieuwe groep maken of toevoegen aan een bestaande groep. |

> [!NOTE]
> Normaal gesp roken wanneer u een cognitieve service resource in de Azure Portal maakt, hebt u de mogelijkheid om een sleutel voor meerdere service abonnementen te maken (die wordt gebruikt voor meerdere cognitieve Services) of een abonnements sleutel van één service (die alleen wordt gebruikt met een specifieke cognitieve service). Omdat de formulier Recognizer echter een preview-versie is, is deze niet opgenomen in het abonnement voor meerdere services en kunt u het abonnement met één service niet maken, tenzij u de koppeling in het welkomst bericht gebruikt.

Wanneer de resource voor de herkenner van het formulier is geïmplementeerd, zoekt en selecteert u deze in de lijst **alle resources** in de portal. Uw sleutel en eind punt bevinden zich op de pagina sleutel en eind punt van de resource, onder resource beheer. Sla beide op een tijdelijke locatie op voordat u verdergaat.
