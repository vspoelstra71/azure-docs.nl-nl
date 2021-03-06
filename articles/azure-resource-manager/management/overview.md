---
title: Overzicht
description: Hierin wordt beschreven hoe u Azure Resource Manager kunt gebruiken voor implementatie, beheer, en beheer van toegang tot resources in Azure.
ms.topic: overview
ms.date: 04/21/2020
ms.openlocfilehash: 253fc2f296fa764a6c22fa1331221df60ca21bb5
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "81870494"
---
# <a name="what-is-azure-resource-manager"></a>Wat is Azure Resource Manager?

Azure Resource Manager is de implementatie- en beheersservice voor Azure. Er wordt een Management-laag geboden waarmee u resources in uw Azure-account kunt maken, bijwerken en verwijderen. U kunt beheer functies, zoals toegangs beheer, vergren delingen en tags, gebruiken om uw resources na de implementatie te beveiligen en te organiseren.

Zie [Sjabloonimlementatie Overview](../templates/overview.md)voor meer informatie over Azure Resource Manager sjablonen.

## <a name="consistent-management-layer"></a>Consistente beheerlaag

Wanneer een gebruiker een aanvraag verzendt vanuit een van de Azure-hulpprogram ma's, Api's of Sdk's, ontvangt de aanvraag een resource manager. De aanvraag wordt geverifieerd en geautoriseerd. Resource Manager verzendt de aanvraag naar de Azure-service, die de aangevraagde actie onderneemt. Omdat alle aanvragen worden verwerkt via dezelfde API, ziet u consistente resultaten en mogelijkheden in de verschillende hulpprogramma's.

In de volgende afbeelding ziet u de rol Azure Resource Manager speelt bij het verwerken van Azure-aanvragen.

![Resource Manager-aanvraagmodel](./media/overview/consistent-management-layer.png)

Alle mogelijkheden die beschikbaar zijn in de Azure-portal zijn ook beschikbaar via Azure PowerShell, Azure CLI, de Azure REST API's en client-SDK's. Functionaliteit die oorspronkelijk wordt uitgegeven via API's, is binnen 180 dagen na de eerste release beschikbaar in de portal.

## <a name="terminology"></a>Terminologie

Als u nog geen ervaring hebt met de Azure Resource Manager, zijn er enkele termen die u mogelijk niet kent.

* **resource** - een beheerbaar item dat beschikbaar is via Azure. Virtuele machines, opslagaccounts, web-apps, databases en virtuele netwerken zijn voorbeelden van resources. Resource groepen, abonnementen, beheer groepen en tags zijn ook voor beelden van resources.
* **resourcegroep** - een container met gerelateerde resources voor een Azure-oplossing. De resourcegroep bevat die resources die u als groep wilt beheren. U bepaalt welke resources deel uitmaken van een resourcegroep op basis van wat voor uw organisatie het meest zinvol is. Zie [Resourcegroepen](#resource-groups).
* **resourceprovider** - een service die zorgt voor Azure-resources. Voorbeeld: een veelgebruikte resourceprovider is Microsoft.Compute, die de resource van de virtuele machine levert. Microsoft.Storage is een andere algemene resourceprovider. Zie [resource providers en-typen](resource-providers-and-types.md).
* **Resource Manager-sjabloon** : een JavaScript object Notation-bestand (JSON) waarmee een of meer resources worden gedefinieerd voor de implementatie van een resource groep, abonnement, beheer groep of Tenant. De sjabloon kan worden gebruikt om de resources consistent en herhaaldelijk te implementeren. Zie [overzicht van Sjabloonimlementatie](../templates/overview.md).
* **declaratieve syntaxis** - een syntaxis waarmee u kunt aangeven 'Dit is wat ik wil maken' zonder hiervoor de nodige reeks programmeeropdrachten te hoeven maken. De sjabloon Resource Manager is een voorbeeld van een declaratieve syntaxis. In het bestand definieert u de eigenschappen voor de infrastructuur te implementeren naar Azure.  Zie [overzicht van Sjabloonimlementatie](../templates/overview.md).

## <a name="the-benefits-of-using-resource-manager"></a>De voordelen van Resource Manager

Met Resource Manager kunt u het volgende doen:

* Uw infra structuur beheren via declaratieve sjablonen in plaats van scripts.

* U kunt alle resources voor uw oplossing implementeren, beheren en bewaken als groep, in plaats van deze resources afzonderlijk te verwerken.

* Implementeer uw oplossing opnieuw in de levens cyclus van de ontwikkeling en laat de betrouw baarheid van uw resources in een consistente staat worden geïmplementeerd.

* Definieer de afhankelijkheden tussen resources, zodat deze in de juiste volg orde worden geïmplementeerd.

* Toegangs beheer Toep assen op alle services omdat op rollen gebaseerde Access Control (RBAC) systeem eigen is geïntegreerd in het beheer platform.

* Pas Tags toe op resources om alle resources in uw abonnement logisch te organiseren.

* Verhelder de facturering van uw organisatie door de kosten voor een groep resources te bekijken die dezelfde tag delen.

## <a name="understand-scope"></a>Bereik

Azure biedt vier bereik niveaus: [beheer groepen](../../governance/management-groups/overview.md), abonnementen, [resource groepen](#resource-groups)en resources. In de volgende afbeelding ziet u een voorbeeld van deze lagen.

![Beheer niveaus](./media/overview/scope-levels.png)

U past beheerinstellingen toe op een of meer bereikniveaus. Het niveau dat u kiest, bepaalt de reikwijdte van de instelling. Lagere niveaus nemen instellingen over van hogere niveaus. Wanneer u bijvoorbeeld een [beleid](../../governance/policy/overview.md) toepast op het abonnement, wordt het beleid toegepast op alle resource groepen en resources in uw abonnement. Wanneer u een beleid toepast op de resource groep, wordt dat beleid toegepast op de resource groep en alle bijbehorende resources. Een andere resource groep beschikt echter niet over die beleids toewijzing.

U kunt sjablonen implementeren voor tenants, beheer groepen, abonnementen of resource groepen.

## <a name="resource-groups"></a>Resourcegroepen

Er zijn een aantal belangrijke factoren waarmee u rekening moet houden bij het definiëren van de resourcegroep:

* Alle resources in uw groep moeten dezelfde levenscyclus hebben. U implementeert ze samen, werkt ze samen bij en verwijdert ze samen. Als een resource, zoals een databaseserver, in een andere implementatiecyclus moet werken, moet deze in een andere resourcegroep worden geplaatst.

* Elke resource kan in slechte één resourcegroep voorkomen.

* Sommige resources kunnen buiten een resource groep bestaan. Deze resources worden geïmplementeerd in het [abonnement](../templates/deploy-to-subscription.md), de [beheer groep](../templates/deploy-to-management-group.md)of de [Tenant](../templates/deploy-to-tenant.md). In deze bereiken worden alleen specifieke resource typen ondersteund.

* U kunt een resource op elk gewenst moment toevoegen aan of verwijderen uit een resourcegroep.

* U kunt een resource van de ene naar de andere resourcegroep verplaatsen. Zie voor meer informatie [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](move-resource-group-and-subscription.md).

* Een resourcegroep kan resources uit verschillende regio’s bevatten.

* Een resourcegroep kan worden gebruikt voor het bepalen van de mate van toegangsbeheer voor beheertaken.

* Een resource kan communiceren met resources in andere resourcegroepen. Deze interactie is gebruikelijk wanneer er een relatie bestaat tussen de twee resources, maar deze niet dezelfde levenscyclus delen (bijvoorbeeld web-apps die verbinding maken met een database).

Als u een resourcegroep maakt, moet u voor die resourcegroep een locatie opgeven. U vraagt zich misschien af: 'Waarom heeft een resourcegroep een locatie nodig? En als de resources andere locaties kunnen hebben dan de resourcegroep, wat is dan het nut van een locatie voor de resourcegroep?' De resourcegroep slaat metagegevens op over de resources. Wanneer u een locatie voor de resource groep opgeeft, geeft u op waar de meta gegevens worden opgeslagen. In verband met nalevingsvereisten moet u er mogelijk voor zorgen dat uw gegevens worden opgeslagen in een bepaalde regio.

Als de regio van de resource groep tijdelijk niet beschikbaar is, kunt u resources in de resource groep niet bijwerken omdat de meta gegevens niet beschikbaar zijn. De resources in andere regio's zullen nog steeds werken zoals verwacht, maar u kunt ze niet bijwerken. Zie voor meer informatie over het bouwen van betrouw bare toepassingen [ontwerpen van betrouw bare Azure-toepassingen](/azure/architecture/checklist/resiliency-per-service).

## <a name="resiliency-of-azure-resource-manager"></a>Tolerantie van Azure Resource Manager

De Azure Resource Manager-service is ontworpen voor tolerantie en voortdurende Beschik baarheid. Resource Manager-en beheer vlak bewerkingen (aanvragen die zijn verzonden naar management.azure.com) in de REST API zijn:

* Verdeeld over de regio's. Sommige services zijn regionaal.

* Gedistribueerd over Beschikbaarheidszones (als de juiste regio's) op locaties met meerdere Beschikbaarheidszones.

* Niet afhankelijk van één logisch Data Center.

* Nooit ondernomen voor onderhouds activiteiten.

Deze tolerantie geldt voor services die aanvragen ontvangen via Resource Manager. Key Vault voor delen van deze tolerantie.

## <a name="next-steps"></a>Volgende stappen

* Zie [resources verplaatsen naar een nieuwe resource groep of een nieuw abonnement](move-resource-group-and-subscription.md)voor meer informatie over het verplaatsen van resources.

* Zie [Tags gebruiken om uw Azure-resources te organiseren](tag-resources.md)voor meer informatie over het labelen van resources.

* Zie [resources vergren delen om onverwachte wijzigingen te voor komen](lock-resources.md)voor meer informatie over het vergren delen van resources.
