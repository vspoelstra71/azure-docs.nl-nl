---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/05/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 5ddb8e8987e1bd42f28f98b6a609bb460ce4fc7e
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/05/2020
ms.locfileid: "82838063"
---
|Naam |Beschrijving |Effect (s) |Versie |GitHub |
|---|---|---|---|---|
|[Toegestane locaties](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe56962a6-4747-49cd-b67b-bf8b01975c4c) |Met dit beleid kunt u de locaties beperken die uw organisatie kan opgeven tijdens het implementeren van resources. Dit beleid wordt gebruikt om uw geografische nalevingsvereisten af te dwingen. Resourcegroepen, Microsoft.AzureActiveDirectory/b2cDirectories en resources die gebruikmaken van de regio 'global' worden uitgesloten. |toestaan |1.0.0 |[Koppeling](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/AllowedLocations_Deny.json) |
|[Toegestane locaties voor resource groepen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe765b5de-1225-4ba3-bd56-1ac6695af988) |Met dit beleid kunt u de locaties beperken waarop uw organisatie resource groepen kan maken in. Dit beleid wordt gebruikt om uw geografische nalevingsvereisten af te dwingen. |toestaan |1.0.0 |[Koppeling](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/ResourceGroupAllowedLocations_Deny.json) |
|[Toegestane brontypen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa08ec900-254a-4555-9bf5-e42af04b5c5c) |Met dit beleid kunt u de resource typen opgeven die uw organisatie kan implementeren. Dit beleid is alleen van invloed op resource typen die de tags ' labels ' en ' Location ' ondersteunen. Als u alle resources wilt beperken, moet u dit beleid dupliceren en de ' mode ' wijzigen in ' all '. |toestaan |1.0.0 |[Koppeling](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/AllowedResourceTypes_Deny.json) |
|[Locatie van de controle resource komt overeen met de locatie van de resource groep](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0a914e76-4921-4c19-b460-a2d36003525a) |Controleren of de resource locatie overeenkomt met de locatie van de resource groep |controle |1.0.0 |[Koppeling](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/ResourcesInResourceGroupLocation_Audit.json) |
|[Gebruik van aangepaste RBAC-regels controleren](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa451c1ef-c6ca-483d-87ed-f49761e3ffb5) |Audit ingebouwde rollen zoals ' eigenaar, Contributer, lezer ' in plaats van aangepaste RBAC-rollen, die gevoelig zijn voor fouten. Het gebruik van aangepaste rollen wordt behandeld als een uitzonde ring en vereist een rigoureuze beoordeling en bedreigings modellen |Controle, uitgeschakeld |1.0.0 |[Koppeling](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/Subscription_AuditCustomRBACRoles_Audit.json) |
|[De rollen van het aangepaste abonnements eigenaar mogen niet bestaan](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F10ee2ea2-fb4d-45b8-a7e9-a2e770044cd9) |Dit beleid zorgt ervoor dat er geen rollen voor aangepaste abonnements eigenaren bestaan. |Controle, uitgeschakeld |1.0.0 |[Koppeling](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/CustomSubscription_OwnerRole_Audit.json) |
|[Niet-toegestane resource typen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F6c112d4e-5bc7-47ae-a041-ea2d9dccd749) |Met dit beleid kunt u de resource typen opgeven die uw organisatie niet kan implementeren. |Weigeren |1.0.0 |[Koppeling](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/InvalidResourceTypes_Deny.json) |
