---
title: Parameterbestand maken
description: Parameter bestand maken voor het door geven van waarden tijdens de implementatie van een Azure Resource Manager sjabloon
ms.topic: conceptual
ms.date: 04/20/2020
ms.openlocfilehash: a9845bbb9e14288a01fb7836db260a2baf484395
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2020
ms.locfileid: "83873093"
---
# <a name="create-resource-manager-parameter-file"></a>Resource Manager-parameter bestand maken

In plaats van para meters als inline waarden door te geven in uw script, is het wellicht eenvoudiger een JSON-bestand te gebruiken dat de parameter waarden bevat. In dit artikel wordt beschreven hoe u het parameter bestand maakt.

## <a name="parameter-file"></a>Parameter bestand

Het parameter bestand heeft de volgende indeling:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "<first-parameter-name>": {
      "value": "<first-value>"
    },
    "<second-parameter-name>": {
      "value": "<second-value>"
    }
  }
}
```

U ziet dat de parameter waarden worden opgeslagen als tekst zonder opmaak in het parameter bestand. Deze aanpak werkt voor waarden die niet gevoelig zijn, zoals het opgeven van de SKU voor een resource. Het werkt niet voor gevoelige waarden, zoals wacht woorden. Als u een gevoelige waarde wilt door geven als een para meter, slaat u de waarde op in een sleutel kluis en verwijst u naar de sleutel kluis in het parameter bestand. De gevoelige waarde wordt veilig opgehaald tijdens de implementatie.

Het volgende parameter bestand bevat een onbewerkte tekst waarde en een waarde die is opgeslagen in een sleutel kluis.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "<first-parameter-name>": {
      "value": "<first-value>"
    },
    "<second-parameter-name>": {
      "reference": {
        "keyVault": {
          "id": "<resource-id-key-vault>"
        },
        "secretName": "<secret-name>"
      }
    }
  }
}
```

Zie [Azure Key Vault gebruiken om de waarde van een beveiligde para meter door te geven tijdens de implementatie](key-vault-parameter.md)voor meer informatie over het gebruik van waarden uit een sleutel kluis.

## <a name="define-parameter-values"></a>Parameter waarden definiëren

Als u wilt weten hoe u de parameter waarden definieert, opent u de sjabloon die u implementeert. Bekijk de sectie para meters van de sjabloon. In het volgende voor beeld worden de para meters uit een sjabloon weer gegeven.

```json
"parameters": {
  "storagePrefix": {
    "type": "string",
    "maxLength": 11
  },
  "storageAccountType": {
    "type": "string",
    "defaultValue": "Standard_LRS",
    "allowedValues": [
    "Standard_LRS",
    "Standard_GRS",
    "Standard_ZRS",
    "Premium_LRS"
    ]
  }
}
```

De eerste details die u ziet, is de naam van elke para meter. De waarden in het parameter bestand moeten overeenkomen met de namen.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
    },
    "storageAccountType": {
    }
  }
}
```

Let op het type van de para meter. De waarden in het parameter bestand moeten van hetzelfde type zijn. Voor deze sjabloon kunt u beide para meters opgeven als teken reeksen.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
      "value": ""
    },
    "storageAccountType": {
      "value": ""
    }
  }
}
```

Vervolgens zoekt u naar een standaard waarde. Als een para meter een standaard waarde heeft, kunt u een waarde opgeven, maar hoeft u dit niet te doen.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
      "value": "" // This value must be provided.
    },
    "storageAccountType": {
      "value": "" // This value is optional. Template will use default value if not provided.
    }
  }
}
```

Bekijk ten slotte de toegestane waarden en eventuele beperkingen, zoals de maximale lengte. Ze geven u het bereik van de waarden die u kunt opgeven voor de para meter.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
      "value": "storage"
    },
    "storageAccountType": {
      "value": "Standard_ZRS"
    }
  }
}
```

## <a name="parameter-type-formats"></a>Notaties van het parameter type

In het volgende voor beeld ziet u de indelingen van verschillende parameter typen.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "exampleString": {
      "value": "test string"
    },
    "exampleInt": {
      "value": 4
    },
    "exampleBool": {
      "value": true
    },
    "exampleArray": {
      "value": [
        "value 1",
        "value 2"
      ]
    },
    "exampleObject": {
      "value": {
        "property1": "value1",
        "property2": "value2"
      }
    }
   }
}
```

## <a name="file-name"></a>Bestandsnaam

De algemene Conventie voor het benoemen van het parameter bestand is het toevoegen van **. para meters** aan de naam van de sjabloon. Als uw sjabloon bijvoorbeeld de naam **azuredeploy. json**heeft, heet het parameter bestand **azuredeploy. para meters. json**. Deze naamgevings Conventie helpt u de verbinding tussen de sjabloon en de para meters te zien.

Als u wilt implementeren in verschillende omgevingen, moet u meer dan één parameter bestand maken. Wanneer u het parameter bestand een naam geeft, voegt u een manier toe om het te identificeren. Gebruik bijvoorbeeld **azuredeploy. para meters-dev. json** en **azuredeploy. para meters-Prod. json**


## <a name="parameter-precedence"></a>Parameter prioriteit

U kunt inline-para meters en een lokaal parameter bestand gebruiken in dezelfde implementatie bewerking. U kunt bijvoorbeeld een aantal waarden opgeven in het lokale parameter bestand en andere waarden inline toevoegen tijdens de implementatie. Als u waarden opgeeft voor een para meter in zowel het lokale parameter bestand als inline, heeft de inline-waarde prioriteit.

Het is mogelijk om een extern parameter bestand te gebruiken door de URI naar het bestand op te geven. Wanneer u dit doet, kunt u geen andere waarden door geven in line of vanuit een lokaal bestand. Alle inline-para meters worden genegeerd. Geef alle parameter waarden op in het externe bestand.

## <a name="parameter-name-conflicts"></a>Conflicten met parameter namen

Als uw sjabloon een para meter bevat met dezelfde naam als een van de para meters in de Power shell-opdracht, geeft Power shell de para meter van uw sjabloon weer met de achtervoegsel **FromTemplate**. Een para meter met de naam **ResourceGroupName** in uw sjabloon is bijvoorbeeld strijdig met de para meter **ResourceGroupName** in de cmdlet [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) . U wordt gevraagd een waarde voor **ResourceGroupNameFromTemplate**op te geven. U kunt deze Verwar ring vermijden door parameter namen te gebruiken die niet worden gebruikt voor implementatie opdrachten.

## <a name="next-steps"></a>Volgende stappen

- Zie [para meters in azure Resource Manager sjablonen](template-parameters.md)voor meer informatie over het definiëren van para meters in uw sjabloon.
- Zie [Azure Key Vault gebruiken om de waarde van een beveiligde para meter door te geven tijdens de implementatie](key-vault-parameter.md)voor meer informatie over het gebruik van waarden uit een sleutel kluis.
- Zie [para meters in azure Resource Manager-sjablonen](template-parameters.md)voor meer informatie over para meters.
