---
title: Details van de structuur van de beleids definitie
description: Hierin wordt beschreven hoe beleids definities worden gebruikt om conventies voor Azure-resources in uw organisatie in te richten.
ms.date: 04/03/2020
ms.topic: conceptual
ms.openlocfilehash: a4f136bc805cd48d05c2378b47966b4e4e4c60fb
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2020
ms.locfileid: "84168502"
---
# <a name="azure-policy-definition-structure"></a>Structuur van Azure-beleidsdefinities

Azure Policy legt conventies voor resources vast. Beleids definities beschrijven de nalevings [voorwaarden voor](#conditions) bronnen en het effect dat moet worden genomen als aan een voor waarde wordt voldaan. Met een voor waarde wordt een [veld](#fields) van een resource-eigenschap vergeleken met een vereiste waarde. Resource-eigenschaps velden worden geopend met behulp van [aliassen](#aliases). Een resource-eigenschaps veld is een veld met één waarde of een [matrix](#understanding-the--alias) met meerdere waarden. De evaluatie van de voor waarde wijkt af van matrices.
Meer informatie over [voor waarden](#conditions).

Als u conventies definieert, kunt u kosten besparen en uw resources eenvoudiger beheren. U kunt bijvoorbeeld opgeven dat alleen bepaalde typen virtuele machines zijn toegestaan. Of u kunt vereisen dat alle resources een bepaalde tag hebben. Beleids regels worden overgenomen door alle onderliggende resources. Als er een beleid wordt toegepast op een resource groep, is dit van toepassing op alle resources in die resource groep.

Het definitie schema voor het beleid vindt u hier:[https://schema.management.azure.com/schemas/2019-06-01/policyDefinition.json](https://schema.management.azure.com/schemas/2019-06-01/policyDefinition.json)

U gebruikt JSON om een beleids definitie te maken. De beleids definitie bevat elementen voor:

- mode
- parameters
- weergave naam
- description
- beleids regel
  - logische evaluatie
  - effect

In de volgende JSON wordt bijvoorbeeld een beleid weer gegeven dat de implementatie van resources beperkt:

```json
{
    "properties": {
        "mode": "all",
        "parameters": {
            "allowedLocations": {
                "type": "array",
                "metadata": {
                    "description": "The list of locations that can be specified when deploying resources",
                    "strongType": "location",
                    "displayName": "Allowed locations"
                },
                "defaultValue": [ "westus2" ]
            }
        },
        "displayName": "Allowed locations",
        "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
        "policyRule": {
            "if": {
                "not": {
                    "field": "location",
                    "in": "[parameters('allowedLocations')]"
                }
            },
            "then": {
                "effect": "deny"
            }
        }
    }
}
```

Alle Azure Policy-voor beelden zijn Azure Policy-voor [beelden](../samples/index.md).

## <a name="mode"></a>Modus

De **modus** is geconfigureerd, afhankelijk van of het beleid is gericht op een Azure Resource Manager eigenschap of een bron provider eigenschap.

### <a name="resource-manager-modes"></a>Resource Manager-modi

De **modus** bepaalt welke resource typen worden geëvalueerd voor een beleid. De ondersteunde modi zijn:

- `all`: resource groepen, abonnementen en alle resource typen evalueren
- `indexed`: alleen resource typen evalueren die ondersteuning bieden voor labels en locaties

Resource `Microsoft.Network/routeTables` ondersteunt bijvoorbeeld tags en locatie en wordt in beide modi geëvalueerd. De resource `Microsoft.Network/routeTables/routes` kan echter niet worden gelabeld en wordt niet geëvalueerd in de `Indexed` modus.

U wordt aangeraden de **modus** `all` in de meeste gevallen in te stellen. Alle beleids definities die via de portal zijn gemaakt, maken gebruik van de `all` modus. Als u Power shell of Azure CLI gebruikt, kunt u de para meter **mode** hand matig opgeven. Als de beleids definitie geen **modus** waarde bevat, wordt standaard `all` in azure PowerShell en naar `null` in azure cli. Een `null` modus is dezelfde als die gebruikt `indexed` om achterwaartse compatibiliteit te ondersteunen.

`indexed`moet worden gebruikt bij het maken van beleids regels voor het afdwingen van tags of locaties. Hoewel dit niet vereist is, voor komt u dat resources die tags en locaties ondersteunen, niet kunnen worden weer gegeven als niet-compatibel in de nalevings resultaten. De uitzonde ring is **resource groepen** en **abonnementen**. Met beleids definities die locatie of tags voor een resource groep of-abonnement afdwingen, moet u de **modus** instellen op `all` en het doel `Microsoft.Resources/subscriptions/resourceGroups` `Microsoft.Resources/subscriptions` opgeven. Zie voor een voor beeld [patroon: Labels-voor beeld #1](../samples/pattern-tags.md). Zie [tag-ondersteuning voor Azure-resources](../../../azure-resource-manager/management/tag-support.md)voor een lijst met resources die Tags ondersteunen.

### <a name="resource-provider-modes-preview"></a><a name="resource-provider-modes"></a>Resource provider modi (preview-versie)

De volgende resource provider modi worden momenteel ondersteund tijdens de preview-versie:

- `Microsoft.ContainerService.Data`voor het beheren van regels voor toegangs beheer in de [Azure Kubernetes-service](../../../aks/intro-kubernetes.md). Beleids regels die gebruikmaken van deze resource provider modus **moeten** het [EnforceRegoPolicy](./effects.md#enforceregopolicy) -effect gebruiken. Deze modus wordt _afgeschaft_.
- `Microsoft.Kubernetes.Data`voor het beheren van uw Kubernetes-clusters in of uit Azure. Beleids regels die gebruikmaken van deze resource provider modus **moeten** het [EnforceOPAConstraint](./effects.md#enforceopaconstraint) -effect gebruiken.
- `Microsoft.KeyVault.Data`voor het beheren van kluizen en certificaten in [Azure Key Vault](../../../key-vault/general/overview.md).

> [!NOTE]
> De resource provider modi bieden alleen ondersteuning voor ingebouwde beleids definities en bieden geen ondersteuning voor initiatieven als er een preview-versie beschikbaar is.

## <a name="parameters"></a>Parameters

Met para meters kunt u het beleids beheer vereenvoudigen door het aantal beleids definities te verminderen. U kunt para meters zien zoals de velden in een formulier – `name` , `address` ,, `city` , `state` . Deze para meters blijven altijd hetzelfde, maar hun waarden worden gewijzigd op basis van de persoon die het formulier invult.
Para meters werken op dezelfde manier als wanneer u beleid bouwt. Door para meters in een beleids definitie op te nemen, kunt u het beleid voor verschillende scenario's gebruiken door verschillende waarden te gebruiken.

> [!NOTE]
> Para meters kunnen worden toegevoegd aan een bestaande en toegewezen definitie. De nieuwe para meter moet de eigenschap **DefaultValue** bevatten. Dit voor komt dat bestaande toewijzingen van het beleid of initiatief indirect ongeldig worden gemaakt.

### <a name="parameter-properties"></a>Parameter eigenschappen

Een para meter heeft de volgende eigenschappen die worden gebruikt in de beleids definitie:

- **naam**: de naam van de para meter. Wordt gebruikt door de `parameters` implementatie functie binnen de beleids regel. Zie [een parameter waarde gebruiken](#using-a-parameter-value)voor meer informatie.
- `type`: Bepaalt of de para meter een **teken reeks**, een **matrix**, een **object**, een **Booleaanse waarde**, een **geheel getal**, een **float**of een **datum/tijd**is.
- `metadata`: Definieert subeigenschappen die voornamelijk worden gebruikt door de Azure Portal om gebruikers vriendelijke informatie weer te geven:
  - `description`: De uitleg van het gebruik van de para meter voor. Kan worden gebruikt om voor beelden te bieden van acceptabele waarden.
  - `displayName`: De beschrijvende naam die wordt weer gegeven in de portal voor de para meter.
  - `version`: (Optioneel) houdt informatie bij over de versie van de inhoud van een beleids definitie.

    > [!NOTE]
    > De Azure Policy-service gebruikt `version` , `preview` en `deprecated` Eigenschappen om het niveau van de wijziging in een ingebouwde beleids definitie of-initiatief en-status over te brengen. De indeling van `version` is: `{Major}.{Minor}.{Patch}` . Specifieke statussen, zoals _afgeschaft_ of _Preview_, worden toegevoegd aan de `version` eigenschap of een andere eigenschap als een **Booleaanse waarde**.

  - `category`: (Optioneel) bepaalt in welke categorie in Azure Portal de beleids definitie wordt weer gegeven.
  - `strongType`: (Optioneel) gebruikt bij het toewijzen van de beleids definitie via de portal. Biedt een context bewuste lijst. Zie [strongType](#strongtype)voor meer informatie.
  - `assignPermissions`: (Optioneel) Stel in op _waar_ als u wilt dat Azure Portal roltoewijzingen tijdens beleids toewijzing wilt maken. Deze eigenschap is handig voor het geval u machtigingen wilt toewijzen buiten het toewijzings bereik. Er is één roltoewijzing per roldefinitie in het beleid (of per functie definitie in alle beleids regels in het initiatief). De parameter waarde moet een geldige resource of een geldig bereik zijn.
- `defaultValue`: (Optioneel) Hiermee stelt u de waarde van de para meter in een toewijzing in als er geen waarde is opgegeven.
  Vereist bij het bijwerken van een bestaande beleids definitie die is toegewezen.
- `allowedValues`: (Optioneel) biedt een matrix van waarden die door de para meter worden geaccepteerd tijdens de toewijzing.

U kunt bijvoorbeeld een beleids definitie definiëren om de locaties te beperken waar resources kunnen worden geïmplementeerd. Een para meter voor die beleids definitie kan **allowedLocations**zijn. Deze para meter wordt gebruikt door elke toewijzing van de beleids definitie om de geaccepteerde waarden te beperken. Het gebruik van **strongType** biedt een verbeterde ervaring bij het volt ooien van de toewijzing via de portal:

```json
"parameters": {
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        },
        "defaultValue": [ "westus2" ],
        "allowedValues": [
            "eastus2",
            "westus2",
            "westus"
        ]
    }
}
```

### <a name="using-a-parameter-value"></a>Een parameter waarde gebruiken

In de beleids regel verwijst u naar para meters met de volgende `parameters` functie syntaxis:

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

Dit voor beeld verwijst naar de **allowedLocations** -para meter die is gedemonstreerd in [parameter eigenschappen](#parameter-properties).

### <a name="strongtype"></a>strongType

Binnen de `metadata` eigenschap kunt u **strongType** gebruiken om een multi-select lijst met opties in de Azure Portal op te geven. **strongType** kan een ondersteund _resource type_ of een toegestane waarde zijn. Als u wilt bepalen of een _resource type_ geldig is voor **strongType**, gebruikt u [Get-AzResourceProvider](/powershell/module/az.resources/get-azresourceprovider).

Sommige _resource typen_ die niet worden geretourneerd door **Get-AzResourceProvider** worden ondersteund. Dit zijn:

- `Microsoft.RecoveryServices/vaults/backupPolicies`

Het niet- _bron type_ toegestane waarden voor **strongType** zijn:

- `location`
- `resourceTypes`
- `storageSkus`
- `vmSKUs`
- `existingResourceGroups`

## <a name="definition-location"></a>Locatie van definitie

Tijdens het maken van een initiatief of beleid is het nood zakelijk om de definitie locatie op te geven. De definitie locatie moet een beheer groep of een abonnement zijn. Deze locatie bepaalt het bereik waaraan het initiatief of beleid kan worden toegewezen. Resources moeten rechtstreekse leden zijn van of onderliggende items binnen de hiërarchie van de definitie locatie naar het doel voor toewijzing.

Als de definitie locatie een:

- Voor alleen **abonnements** resources binnen dit abonnement kan het beleid worden toegewezen.
- **Beheer groep** -alleen resources binnen onderliggende beheer groepen en onderliggende abonnementen kunnen het beleid toewijzen. Als u van plan bent de beleids definitie toe te passen op verschillende abonnementen, moet de locatie een beheer groep zijn die die abonnementen bevat.

## <a name="display-name-and-description"></a>Weergave naam en beschrijving

U gebruikt **DisplayName** en **Beschrijving** om de beleids definitie te identificeren en context op te geven wanneer deze wordt gebruikt. **DisplayName** heeft een maximale lengte van _128_ tekens en een **Beschrijving** van Maxi maal _512_ tekens.

> [!NOTE]
> Tijdens het maken of bijwerken van een beleids definitie, de **id**, het **type**en de **naam** worden gedefinieerd door de Eigenschappen extern van de JSON en zijn deze niet nodig in het JSON-bestand. Het ophalen van de beleids definitie via SDK retourneert de eigenschappen **id**, **type**en **naam** als onderdeel van de JSON, maar elk is een alleen-lezen informatie met betrekking tot de beleids definitie.

## <a name="policy-rule"></a>Beleidsregel

De beleids regel bestaat uit **als** en **vervolgens** blokken. In het **if** -blok definieert u een of meer voor waarden die aangeven wanneer het beleid wordt afgedwongen. U kunt logische Opera tors Toep assen op deze voor waarden om het scenario voor een beleid precies te definiëren.

In de **blok kering** definieert u het effect dat optreedt wanneer aan de voor waarden voor de **if** wordt voldaan.

```json
{
    "if": {
        <condition> | <logical operator>
    },
    "then": {
        "effect": "deny | audit | append | auditIfNotExists | deployIfNotExists | disabled"
    }
}
```

### <a name="logical-operators"></a>Logische operators

Ondersteunde logische Opera tors zijn:

- `"not": {condition  or operator}`
- `"allOf": [{condition or operator},{condition or operator}]`
- `"anyOf": [{condition or operator},{condition or operator}]`

De syntaxis **not** keert het resultaat van de voor waarde. De **overzet** -syntaxis (vergelijkbaar met de logische **en** -bewerking) vereist dat alle voor waarden waar zijn. Voor de syntaxis van de **anyOf** (vergelijkbaar met de logische **of** bewerking) moeten een of meer voor waarden waar zijn.

U kunt logische Opera tors nesten. In het volgende voor beeld ziet u een **niet** -bewerking die is genest in een **overzet** -bewerking.

```json
"if": {
    "allOf": [{
            "not": {
                "field": "tags",
                "containsKey": "application"
            }
        },
        {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
        }
    ]
},
```

### <a name="conditions"></a>Voorwaarden

In een voor waarde wordt geëvalueerd of een **veld** of de **waarde** -accessor voldoet aan bepaalde criteria. De ondersteunde voor waarden zijn:

- `"equals": "stringValue"`
- `"notEquals": "stringValue"`
- `"like": "stringValue"`
- `"notLike": "stringValue"`
- `"match": "stringValue"`
- `"matchInsensitively": "stringValue"`
- `"notMatch": "stringValue"`
- `"notMatchInsensitively": "stringValue"`
- `"contains": "stringValue"`
- `"notContains": "stringValue"`
- `"in": ["stringValue1","stringValue2"]`
- `"notIn": ["stringValue1","stringValue2"]`
- `"containsKey": "keyName"`
- `"notContainsKey": "keyName"`
- `"less": "dateValue"` | `"less": "stringValue"` | `"less": intValue`
- `"lessOrEquals": "dateValue"` | `"lessOrEquals": "stringValue"` | `"lessOrEquals": intValue`
- `"greater": "dateValue"` | `"greater": "stringValue"` | `"greater": intValue`
- `"greaterOrEquals": "dateValue"` | `"greaterOrEquals": "stringValue"` | `"greaterOrEquals": intValue`
- `"exists": "bool"`

Voor **minder**, **lessOrEquals**, **meer**en **greaterOrEquals**als het eigenschaps type niet overeenkomt met het type voor waarde, wordt een fout gegenereerd. Teken reeks vergelijkingen worden gemaakt met `InvariantCultureIgnoreCase` .

Wanneer u de voor waarden **like** en **notLike** gebruikt, geeft u een Joker teken `*` op in de waarde.
De waarde mag niet meer dan één Joker teken bevatten `*` .

Wanneer u de voor waarden **match** en **notMatch** gebruikt, moet `#` u een cijfer, voor een letter, overeenkomen met een `?` `.` wille keurig teken en elk ander teken dat overeenkomt met het werkelijke teken. Hoewel **match** en **notMatch** hoofdletter gevoelig zijn, zijn alle andere voor waarden die een _stringValue_ evalueren niet hoofdletter gevoelig. Hoofdletter gevoelige alternatieven zijn beschikbaar in **matchInsensitively** en **notMatchInsensitively**.

In een ** \[ \* \] alias** matrix veld waarde wordt elk element in de matrix afzonderlijk geëvalueerd met logische **en** tussen elementen. Zie [de \[ \* \] alias evalueren](../how-to/author-policies-for-arrays.md#evaluating-the--alias)voor meer informatie.

### <a name="fields"></a>Velden

Voor waarden worden gevormd door velden te gebruiken. Een veld komt overeen met de eigenschappen in de nettolading van de resource aanvraag en beschrijft de status van de resource.

De volgende velden worden ondersteund:

- `name`
- `fullName`
  - Retourneert de volledige naam van de resource. De volledige naam van een resource is de resource naam die wordt voorafgegaan door bovenliggende resource namen (bijvoorbeeld ' mijn server/myDatabase ').
- `kind`
- `type`
- `location`
  - Gebruik **Global** voor resources die de locatie neutraal.
- `identity.type`
  - Retourneert het type [beheerde identiteit](../../../active-directory/managed-identities-azure-resources/overview.md) dat is ingeschakeld voor de bron.
- `tags`
- `tags['<tagName>']`
  - De syntaxis van het haakje ondersteunt label namen met lees tekens zoals een afbreek streepje, een punt of een spatie.
  - Waar de **\<tagName\>** naam van de tag is voor het valideren van de voor waarde voor.
  - Voor beelden: `tags['Acct.CostCenter']` waarbij **acct. CostCenter** de naam van de tag is.
- `tags['''<tagName>''']`
  - De syntaxis van het haakje ondersteunt label namen met apostrofs in het teken met dubbele apostrofs.
  - Waarbij **' \<tagName\> '** de naam van het label is voor het valideren van de voor waarde voor.
  - Voor beeld: `tags['''My.Apostrophe.Tag''']` waarbij **' My. apostrof. tag '** de naam van de tag is.
- eigenschaps aliassen: Zie [aliassen](#aliases)voor een lijst.

> [!NOTE]
> `tags.<tagName>`, `tags[tagName]` en `tags[tag.with.dots]` zijn nog steeds acceptabele manieren om een label veld te declareren. De voorkeurs expressies zijn echter die in de bovenstaande lijst.

#### <a name="use-tags-with-parameters"></a>Tags gebruiken met para meters

Een parameter waarde kan worden door gegeven aan een label veld. Door een para meter door te geven aan een label veld, verhoogt u de flexibiliteit van de beleids definitie tijdens beleids toewijzing.

In het volgende voor beeld `concat` wordt gebruikt om een label veld te maken voor de tag met de naam de waarde van de para meter **tagName** . Als deze tag niet bestaat, wordt het **wijzigings** effect gebruikt om de tag toe te voegen met de waarde van dezelfde benoemde tag die is ingesteld voor de bovenliggende resource groep van de gecontroleerde resources met behulp van de `resourcegroup()` functie lookup.

```json
{
    "if": {
        "field": "[concat('tags[', parameters('tagName'), ']')]",
        "exists": "false"
    },
    "then": {
        "effect": "modify",
        "details": {
            "operations": [{
                "operation": "add",
                "field": "[concat('tags[', parameters('tagName'), ']')]",
                "value": "[resourcegroup().tags[parameters('tagName')]]"
            }],
            "roleDefinitionIds": [
                "/providers/microsoft.authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c"
            ]
        }
    }
}
```

### <a name="value"></a>Waarde

Voor waarden kunnen ook worden gevormd met behulp van een **waarde**. **waarde** controleert de voor waarden op basis van [para meters](#parameters), [ondersteunde sjabloon functies](#policy-functions)of letterlijke tekens.
**waarde** wordt gekoppeld aan elke ondersteunde [voor waarde](#conditions).

> [!WARNING]
> Als het resultaat van een _sjabloon functie_ een fout is, mislukt de beleids evaluatie. Een mislukte evaluatie is een impliciete **weigering**. Zie voor meer informatie [sjabloon fouten voor komen](#avoiding-template-failures). Gebruik [enforcementMode](./assignment-structure.md#enforcement-mode) van **DoNotEnforce** om de impact van een mislukte evaluatie op nieuwe of bijgewerkte resources te voor komen tijdens het testen en valideren van een nieuwe beleids definitie.

#### <a name="value-examples"></a>Voor beelden van waarden

In dit voor beeld van een beleids regel wordt de **waarde** gebruikt om het resultaat van de `resourceGroup()` functie en de eigenschap geretourneerde **naam** te vergelijken met een **like** -voor waarde van `*netrg` . De regel weigert resources die niet van het `Microsoft.Network/*` **type** zijn in een resource groep waarvan de naam eindigt op `*netrg` .

```json
{
    "if": {
        "allOf": [{
                "value": "[resourceGroup().name]",
                "like": "*netrg"
            },
            {
                "field": "type",
                "notLike": "Microsoft.Network/*"
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

Dit voor beeld van een beleids regel gebruikt een **waarde** om te controleren of het resultaat van meerdere geneste functies **gelijk is aan** `true` . De regel weigert een resource die niet ten minste drie tags heeft.

```json
{
    "mode": "indexed",
    "policyRule": {
        "if": {
            "value": "[less(length(field('tags')), 3)]",
            "equals": "true"
        },
        "then": {
            "effect": "deny"
        }
    }
}
```

#### <a name="avoiding-template-failures"></a>Storingen in sjablonen voor komen

Het gebruik van _sjabloon functies_ in **waarde** biedt veel complexe geneste functies. Als het resultaat van een _sjabloon functie_ een fout is, mislukt de beleids evaluatie. Een mislukte evaluatie is een impliciete **weigering**. Een voor beeld van een **waarde** die mislukt in bepaalde scenario's:

```json
{
    "policyRule": {
        "if": {
            "value": "[substring(field('name'), 0, 3)]",
            "equals": "abc"
        },
        "then": {
            "effect": "audit"
        }
    }
}
```

De voorbeeld beleidsregel hierboven maakt gebruik van [subtekenreeks ()](../../../azure-resource-manager/templates/template-functions-string.md#substring) om de eerste drie tekens van de **naam** te vergelijken met **ABC**. Als de **naam** korter is dan drie tekens, `substring()` resulteert de functie in een fout. Als gevolg van deze fout wordt het beleid **geweigerd** .

Gebruik in plaats daarvan de functie [als ()](../../../azure-resource-manager/templates/template-functions-logical.md#if) om te controleren of de eerste drie tekens van de **naam** gelijk zijn aan **ABC** zonder dat een **naam** die korter is dan drie tekens, een fout veroorzaakt:

```json
{
    "policyRule": {
        "if": {
            "value": "[if(greaterOrEquals(length(field('name')), 3), substring(field('name'), 0, 3), 'not starting with abc')]",
            "equals": "abc"
        },
        "then": {
            "effect": "audit"
        }
    }
}
```

Met de gereviseerde beleids regel `if()` controleert u de lengte van de **naam** voordat u probeert een `substring()` waarde op te halen die korter is dan drie tekens. Als de **naam** te kort is, wordt de waarde ' niet beginnend met ABC ' geretourneerd in plaats van **ABC**. Een resource met een korte naam die niet met **ABC** begint, mislukt nog steeds de beleids regel, maar veroorzaakt geen fout meer tijdens de evaluatie.

### <a name="count"></a>Count

Voor waarden die tellen hoeveel leden van een matrix in de resource-nettolading voldoen aan een voor waarde-expressie, kunnen worden gevormd met de expressie **Count** . Bij algemene scenario's wordt gecontroleerd of ten minste één van ', ' precies één van ', ' alle of ' geen van ' de matrix leden voldoen aan de voor waarde. met **Count** wordt elk lid van een [ \[ \* \] alias](#understanding-the--alias) matrix geëvalueerd voor een voorwaarde expressie en worden de _werkelijke_ resultaten opgeteld, die vervolgens worden vergeleken met de operator voor expressies. Expressies voor **tellingen** kunnen Maxi maal drie keer worden toegevoegd aan een enkele **policyRule** -definitie.

De structuur van de **Count** -expressie is:

```json
{
    "count": {
        "field": "<[*] alias>",
        "where": {
            /* condition expression */
        }
    },
    "<condition>": "<compare the count of true condition expression array members to this value>"
}
```

De volgende eigenschappen worden gebruikt met **aantal**:

- **Count. Field** (vereist): bevat het pad naar de matrix en moet een matrix alias zijn. Als de matrix ontbreekt, wordt de expressie geëvalueerd naar _False_ zonder rekening te houden met de voor waarde-expressie.
- **Count. where** (optioneel): de voor waarde-expressie voor het afzonderlijk evalueren van elke [ \[ \* \] alias](#understanding-the--alias) matrix lid van het **veld Count.**. Als deze eigenschap niet is gegeven, worden alle matrix leden met het pad van ' Field ' geëvalueerd als _waar_. Elke [voor waarde](../concepts/definition-structure.md#conditions) kan worden gebruikt in deze eigenschap.
  [Logische Opera tors](#logical-operators) kunnen worden gebruikt in deze eigenschap om complexe evaluatie vereisten te maken.
- **\<condition\>**(vereist): de waarde wordt vergeleken met het aantal items dat aan het aantal is voldaan **. where** -voor waarde-expressie. Er moet een numerieke [voor waarde](../concepts/definition-structure.md#conditions) worden gebruikt.

#### <a name="count-examples"></a>Aantal voor beelden

Voor beeld 1: controleren of een matrix leeg is

```json
{
    "count": {
        "field": "Microsoft.Network/networkSecurityGroups/securityRules[*]"
    },
    "equals": 0
}
```

Voor beeld 2: controleren op slechts één matrixlid om te voldoen aan de voor waarde-expressie

```json
{
    "count": {
        "field": "Microsoft.Network/networkSecurityGroups/securityRules[*]",
        "where": {
            "field": "Microsoft.Network/networkSecurityGroups/securityRules[*].description",
            "equals": "My unique description"
        }
    },
    "equals": 1
}
```

Voor beeld 3: controleren of er ten minste één matrixlid is om te voldoen aan de voor waarde-expressie

```json
{
    "count": {
        "field": "Microsoft.Network/networkSecurityGroups/securityRules[*]",
        "where": {
            "field": "Microsoft.Network/networkSecurityGroups/securityRules[*].description",
            "equals": "My common description"
        }
    },
    "greaterOrEquals": 1
}
```

Voor beeld 4: controleren of alle leden van een object Matrix voldoen aan de voor waarde-expressie

```json
{
    "count": {
        "field": "Microsoft.Network/networkSecurityGroups/securityRules[*]",
        "where": {
            "field": "Microsoft.Network/networkSecurityGroups/securityRules[*].description",
            "equals": "description"
        }
    },
    "equals": "[length(field('Microsoft.Network/networkSecurityGroups/securityRules[*]'))]"
}
```

Voor beeld 5: controleren of alle leden van de teken reeks matrix voldoen aan de voor waarde-expressie

```json
{
    "count": {
        "field": "Microsoft.Sql/servers/securityAlertPolicies/emailAddresses[*]",
        "where": {
            "field": "Microsoft.Sql/servers/securityAlertPolicies/emailAddresses[*]",
            "like": "*@contoso.com"
        }
    },
    "equals": "[length(field('Microsoft.Sql/servers/securityAlertPolicies/emailAddresses[*]'))]"
}
```

Voor beeld 6: **veld** in **waarde** gebruiken om te controleren of alle matrix leden voldoen aan de voor waarde-expressie

```json
{
    "count": {
        "field": "Microsoft.Sql/servers/securityAlertPolicies/emailAddresses[*]",
        "where": {
            "value": "[last(split(first(field('Microsoft.Sql/servers/securityAlertPolicies/emailAddresses[*]')), '@'))]",
            "equals": "contoso.com"
        }
    },
    "equals": "[length(field('Microsoft.Sql/servers/securityAlertPolicies/emailAddresses[*]'))]"
}
```

Voor beeld 7: Controleer of ten minste één matrixlid overeenkomt met meerdere eigenschappen in de voor waarde-expressie

```json
{
    "count": {
        "field": "Microsoft.Network/networkSecurityGroups/securityRules[*]",
        "where": {
            "allOf": [
                {
                    "field": "Microsoft.Network/networkSecurityGroups/securityRules[*].direction",
                    "equals": "Inbound"
                },
                {
                    "field": "Microsoft.Network/networkSecurityGroups/securityRules[*].access",
                    "equals": "Allow"
                },
                {
                    "field": "Microsoft.Network/networkSecurityGroups/securityRules[*].destinationPortRange",
                    "equals": "3389"
                }
            ]
        }
    },
    "greater": 0
}
```

### <a name="effect"></a>Effect

Azure Policy ondersteunt de volgende typen effect:

- **Toevoegen**: Hiermee voegt u de gedefinieerde set velden toe aan de aanvraag
- **Controle**: er wordt een waarschuwings gebeurtenis gegenereerd in het activiteiten logboek, maar de aanvraag mislukt niet
- **AuditIfNotExists**: er wordt een waarschuwings gebeurtenis in het activiteiten logboek gegenereerd als een gerelateerde resource niet bestaat
- **Weigeren**: er wordt een gebeurtenis in het activiteiten logboek gegenereerd en de aanvraag mislukt
- **DeployIfNotExists**: implementeert een gerelateerde resource als deze nog niet bestaat
- **Uitgeschakeld**: resources worden niet geëvalueerd voor naleving van de beleids regel
- **EnforceOPAConstraint** (preview): Hiermee configureert u de open Policy Agent Admissions controller met gate keeper v3 voor zelf-beheerde Kubernetes-clusters in azure (preview-versie)
- **EnforceRegoPolicy** (preview): Hiermee configureert u de open Policy Agent Admissions-controller met gate keeper v2 in de Azure Kubernetes-service
- **Wijzigen**: de gedefinieerde labels worden toegevoegd, bijgewerkt of verwijderd uit een resource

Zie voor meer informatie over elk effect, de volg orde van evaluatie, eigenschappen en voor beelden [informatie over Azure Policy effecten](effects.md).

### <a name="policy-functions"></a>Beleids functies

Alle [Resource Manager-sjabloon functies](../../../azure-resource-manager/templates/template-functions.md) zijn beschikbaar voor gebruik in een beleids regel, met uitzonde ring van de volgende functies en door de gebruiker gedefinieerde functies:

- Functie copyindex ()
- implementatie ()
- orderverzamellijst
- newGuid()
- pickZones()
- providers ()
- verwijzing ()
- resourceId ()
- variabelen ()

> [!NOTE]
> Deze functies zijn nog steeds beschikbaar in het `details.deployment.properties.template` gedeelte van de sjabloon implementatie in een **deployIfNotExists** -beleids definitie.

De volgende functie is beschikbaar voor gebruik in een beleids regel, maar verschilt van gebruik in een Azure Resource Manager sjabloon:

- `utcNow()`-In tegens telling tot een resource manager-sjabloon, kan dit buiten de standaard waarde worden gebruikt.
  - Retourneert een teken reeks die is ingesteld op de huidige datum en tijd 8601 in de indeling JJJJ-MM-DDTuu: mm: SS. fffffffZ

De volgende functies zijn alleen beschikbaar in beleids regels:

- `addDays(dateTime, numberOfDaysToAdd)`
  - **DateTime**: [required] string-string in de indeling Universal ISO 8601 datetime ' jjjj-mm-ddTuu: mm: SS. fffffffZ '
  - **numberOfDaysToAdd**: [vereist] geheel getal-aantal dagen dat moet worden toegevoegd
- `field(fieldName)`
  - **FieldName**: [required] string: naam van het [veld](#fields) dat moet worden opgehaald
  - Retourneert de waarde van het veld van de resource die wordt geëvalueerd door de if-voor waarde
  - `field`wordt hoofd zakelijk gebruikt in combi natie met **AuditIfNotExists** en **DeployIfNotExists** om te verwijzen naar velden in de resource die worden geëvalueerd. Een voor beeld van dit gebruik is te zien in het [DeployIfNotExists-voor beeld](effects.md#deployifnotexists-example).
- `requestContext().apiVersion`
  - Retourneert de API-versie van de aanvraag waarvoor beleids evaluatie is geactiveerd (bijvoorbeeld: `2019-09-01` ).
    Dit is de API-versie die is gebruikt in de PUT/PATCH-aanvraag voor evaluaties voor het maken of bijwerken van de resource. De nieuwste API-versie wordt altijd gebruikt tijdens de evaluatie van de naleving van bestaande resources.
  
#### <a name="policy-function-example"></a>Voor beeld van beleids functie

In dit voor beeld van een beleids regel wordt de `resourceGroup` functie resource gebruikt voor het ophalen van de eigenschap **name** , gecombineerd met de `concat` functie Array en object, om een voor waarde op te bouwen `like` die de resource naam afdwingt om te beginnen met de naam van de resource groep.

```json
{
    "if": {
        "not": {
            "field": "name",
            "like": "[concat(resourceGroup().name,'*')]"
        }
    },
    "then": {
        "effect": "deny"
    }
}
```

## <a name="aliases"></a>Aliassen

U kunt eigenschaps aliassen gebruiken om toegang te krijgen tot specifieke eigenschappen voor een bron type. Met aliassen kunt u bepalen welke waarden of voor waarden zijn toegestaan voor een eigenschap van een resource. Elke alias wordt toegewezen aan paden in verschillende API-versies voor een bepaald bron type. Tijdens de beleids evaluatie haalt de beleids engine het pad naar de eigenschap voor die API-versie op.

De lijst met aliassen is altijd groeien. Als u wilt weten welke aliassen momenteel worden ondersteund door Azure Policy, gebruikt u een van de volgende methoden:

- Azure Policy-extensie voor Visual Studio code (aanbevolen)

  Gebruik de [extensie Azure Policy voor Visual Studio code](../how-to/extension-for-vscode.md) om aliassen voor resource-eigenschappen weer te geven en te detecteren.

  :::image type="content" source="../media/extension-for-vscode/extension-hover-shows-property-alias.png" alt-text="Azure Policy-extensie voor Visual Studio code" border="false":::

- Azure Resource Graph

  Gebruik de `project` operator om de **alias** van een resource weer te geven.

  ```kusto
  Resources
  | where type=~'microsoft.storage/storageaccounts'
  | limit 1
  | project aliases
  ```
  
  ```azurecli-interactive
  az graph query -q "Resources | where type=~'microsoft.storage/storageaccounts' | limit 1 | project aliases"
  ```
  
  ```azurepowershell-interactive
  Search-AzGraph -Query "Resources | where type=~'microsoft.storage/storageaccounts' | limit 1 | project aliases"
  ```

- Azure PowerShell

  ```azurepowershell-interactive
  # Login first with Connect-AzAccount if not using Cloud Shell

  # Use Get-AzPolicyAlias to list available providers
  Get-AzPolicyAlias -ListAvailable

  # Use Get-AzPolicyAlias to list aliases for a Namespace (such as Azure Compute -- Microsoft.Compute)
  (Get-AzPolicyAlias -NamespaceMatch 'compute').Aliases
  ```

- Azure CLI

  ```azurecli-interactive
  # Login first with az login if not using Cloud Shell

  # List namespaces
  az provider list --query [*].namespace

  # Get Azure Policy aliases for a specific Namespace (such as Azure Compute -- Microsoft.Compute)
  az provider show --namespace Microsoft.Compute --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
  ```

- REST API-ARMClient

  ```http
  GET https://management.azure.com/providers/?api-version=2019-10-01&$expand=resourceTypes/aliases
  ```

### <a name="understanding-the--alias"></a>Informatie over de alias [*]

Verschillende van de aliassen die beschikbaar zijn, hebben een versie die wordt weer gegeven als een ' normale ' naam en een andere die hieraan is **\[\*\]** gekoppeld. Bijvoorbeeld:

- `Microsoft.Storage/storageAccounts/networkAcls.ipRules`
- `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]`

De ' normale ' alias vertegenwoordigt het veld als een enkele waarde. Dit veld is bedoeld voor vergelijkings scenario's die exact overeenkomen wanneer de volledige set waarden exact zo moet worden gedefinieerd, niet meer en niet minder.

De **\[\*\]** alias maakt het mogelijk om te vergelijken met de waarde van elk element in de matrix en specifieke eigenschappen van elk element. Deze aanpak maakt het mogelijk om element eigenschappen te vergelijken voor ' If geen ', ' als een van ', ' of ' als alle scenario's. Gebruik de expressie [aantal](#count) voor waarde voor complexere scenario's. Met behulp van **ipRules \[ \* \] **wordt een voor beeld gevalideerd dat elke _actie_ wordt _geweigerd_, maar niet om te zorgen over hoeveel regels bestaan of wat de IP- _waarde_ is.
Deze voorbeeld regel controleert op eventuele overeenkomsten van **ipRules \[ \* \] . Value** to **10.0.4.1** en past de **effectType** alleen toe als er ten minste één overeenkomst wordt gevonden:

```json
"policyRule": {
    "if": {
        "allOf": [
            {
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
                "exists": "true"
            },
            {
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value",
                "notEquals": "10.0.4.1"
            }
        ]
    },
    "then": {
        "effect": "[parameters('effectType')]"
    }
}
```

Zie [de \* alias [] evalueren](../how-to/author-policies-for-arrays.md#evaluating-the--alias)voor meer informatie.

## <a name="initiatives"></a>Initiatieven

Met initiatieven kunt u verschillende gerelateerde beleids definities groeperen om toewijzingen en beheer te vereenvoudigen omdat u met een groep werkt als één item. U kunt bijvoorbeeld gerelateerde coderings beleidsregels in één initiatief groeperen. In plaats van elk beleid afzonderlijk toe te wijzen, past u het initiatief toe.

> [!NOTE]
> Zodra een initiatief is toegewezen, kunnen para meters op initiatief niveau niet worden gewijzigd. Daarom is het aanbeveling om een **DefaultValue** in te stellen bij het definiëren van de para meter.

In het volgende voor beeld ziet u hoe u een initiatief maakt voor het afhandelen van twee Tags: `costCenter` en `productName` . Er worden twee ingebouwde beleids regels gebruikt om de standaard label waarde toe te passen.

```json
{
    "properties": {
        "displayName": "Billing Tags Policy",
        "policyType": "Custom",
        "description": "Specify cost Center tag and product name tag",
        "parameters": {
            "costCenterValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for Cost Center tag"
                },
                "defaultValue": "DefaultCostCenter"
            },
            "productNameValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for product Name tag"
                },
                "defaultValue": "DefaultProduct"
            }
        },
        "policyDefinitions": [{
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            }
        ]
    }
}
```

## <a name="next-steps"></a>Volgende stappen

- Bekijk voor beelden op [Azure Policy voor beelden](../samples/index.md).
- Lees [Informatie over de effecten van het beleid](effects.md).
- Meer informatie over het [programmatisch maken van beleids regels](../how-to/programmatically-create.md).
- Meer informatie over het [ophalen van compatibiliteits gegevens](../how-to/get-compliance-data.md).
- Meer informatie over het [oplossen van niet-compatibele resources](../how-to/remediate-resources.md).
- Bekijk wat een beheer groep is met [het organiseren van uw resources met Azure-beheer groepen](../../management-groups/overview.md).
