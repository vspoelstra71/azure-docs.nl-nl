---
title: Naleving met behulp van Azure Policy
description: Ingebouwde beleids regels toewijzen in Azure Policy om de naleving van uw Azure-container registers te controleren
ms.topic: article
ms.date: 02/26/2020
ms.openlocfilehash: a2bfdc18f4bbf16fe8fa6bcbcba7bab18aedabf1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "82145004"
---
# <a name="audit-compliance-of-azure-container-registries-using-azure-policy"></a>Controle van de naleving van Azure-container registers met behulp van Azure Policy

[Azure Policy](../governance/policy/overview.md) is een service in azure die u gebruikt om beleids regels te maken, toe te wijzen en te beheren. Met deze beleidsregels worden verschillende regels en effecten afgedwongen voor uw resources, zodat deze resources voldoen aan de standaarden en service level agreements in uw bedrijf.

In dit artikel worden ingebouwde beleids regels (preview-versie) geïntroduceerd voor Azure Container Registry. Gebruik deze beleids regels om nieuwe en bestaande registers te controleren op naleving.

Er worden geen kosten in rekening gebracht voor het gebruik van Azure Policy.

> [!IMPORTANT]
> Deze functie is momenteel beschikbaar als preview-product. Previews worden voor u beschikbaar gesteld op voorwaarde dat u akkoord gaat met de [aanvullende gebruiksvoorwaarden][terms-of-use]. Sommige aspecten van deze functionaliteit kunnen wijzigen voordat deze functionaliteit algemeen beschikbaar wordt.

## <a name="built-in-policy-definitions"></a>Ingebouwde beleids definities

De volgende ingebouwde beleids definities zijn specifiek voor Azure Container Registry:

[!INCLUDE [azure-policy-samples-policies-container-registry](../../includes/policy/samples/bycat/policies-container-registry.md)]

Zie ook de ingebouwde netwerk beleids definitie: [[Preview] Container Registry moet een service-eind punt voor een virtueel netwerk gebruiken](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc4857be7-912a-4c75-87e6-e30292bcdf78).

## <a name="assign-policies"></a>Beleid toewijzen

* Wijs beleid toe met behulp van de [Azure Portal](../governance/policy/assign-policy-portal.md), [Azure cli](../governance/policy/assign-policy-azurecli.md), een [Resource Manager-sjabloon](../governance/policy/assign-policy-template.md)of de Azure Policy sdk's.
* Bereik een beleids toewijzing aan een resource groep, een abonnement of een [Azure-beheer groep](../governance/management-groups/overview.md). Container register beleids toewijzingen zijn van toepassing op bestaande en nieuwe container registers binnen het bereik.
* Het [afdwingen van beleid](../governance/policy/concepts/assignment-structure.md#enforcement-mode) op elk gewenst moment in-of uitschakelen.

> [!NOTE]
> Wanneer u een beleid toewijst of bijwerkt, kan het enige tijd duren voordat de toewijzing wordt toegepast op resources in het gedefinieerde bereik. Zie informatie over [beleids evaluatie triggers](../governance/policy/how-to/get-compliance-data.md#evaluation-triggers).

## <a name="review-policy-compliance"></a>Naleving van beleid controleren

Toegang tot nalevings gegevens die zijn gegenereerd door uw beleids toewijzingen met behulp van de Azure Portal, Azure-opdracht regel Programma's of de Azure Policy Sdk's. Zie [nalevings gegevens van Azure-resources ophalen](../governance/policy/how-to/get-compliance-data.md)voor meer informatie.

Wanneer een resource niet-compatibel is, zijn er veel mogelijke redenen. Zie [niet-naleving bepalen](../governance/policy/how-to/determine-non-compliance.md)om de reden te bepalen of de verantwoordelijke wijziging te vinden.

### <a name="policy-compliance-in-the-portal"></a>Beleids naleving in de portal:

1. Selecteer **alle services**en zoek naar **beleid**.
1. Selecteer **naleving**.
1. Gebruik de filters om de nalevings status te beperken of om ![te zoeken naar naleving](./media/container-registry-azure-policy/azure-policy-compliance.png)van beleids regels in de portal.
1. Selecteer een beleid om statistische details en gebeurtenissen van de naleving te controleren. Selecteer desgewenst een specifiek REGI ster voor resource naleving.

### <a name="policy-compliance-in-the-azure-cli"></a>Beleids naleving in de Azure CLI

U kunt ook de Azure CLI gebruiken om compatibiliteits gegevens op te halen. Gebruik bijvoorbeeld de opdracht [AZ Policy Assignment List](/cli/azure/policy/assignment#az-policy-assignment-list) in de CLI om de beleids-id's op te halen van het Azure container Registry beleid dat wordt toegepast:

```azurecli
az policy assignment list --query "[?contains(displayName,'Container Registries')].{name:displayName, ID:id}" --output table
```

Voorbeelduitvoer:

```
Name                                                                                   ID
-------------------------------------------------------------------------------------  --------------------------------------------------------------------------------------------------------------------------------
[Preview]: Container Registries should not allow unrestricted network access           /subscriptions/<subscriptionID>/providers/Microsoft.Authorization/policyAssignments/b4faf132dc344b84ba68a441
[Preview]: Container Registries should be encrypted with a Customer-Managed Key (CMK)  /subscriptions/<subscriptionID>/providers/Microsoft.Authorization/policyAssignments/cce1ed4f38a147ad994ab60a
```

Voer vervolgens de [lijst AZ policy state](/cli/azure/policy/state#az-policy-state-list) uit om de nalevings status van JSON-indeling te retour neren voor alle resources onder een specifieke beleids-id:

```azurecli
az policy state list \
  --resource <policyID>
```

Of Voer [AZ policy state List](/cli/azure/policy/state#az-policy-state-list) uit om de nalevings status in JSON-indeling te retour neren van een specifieke register bron, zoals *myregistry*:

```azurecli
az policy state list \
 --resource myregistry \
 --namespace Microsoft.ContainerRegistry \
 --resource-type registries \
 --resource-group myresourcegroup
```

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over Azure Policy [definities](../governance/policy/concepts/definition-structure.md) en [effecten](../governance/policy/concepts/effects.md)

* Een [aangepaste beleids definitie](../governance/policy/tutorials/create-custom-policy-definition.md) maken

* Meer informatie over de beheer [mogelijkheden](../governance/index.yml) van Azure


<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/