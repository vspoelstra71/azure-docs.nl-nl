---
title: UserNameTextBox UI-element
description: Hierin wordt het UI-element micro soft. compute. UserNameTextBox voor Azure Portal beschreven. Hiermee kunnen gebruikers Windows-of Linux-gebruikers namen opgeven.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: c7544ae7d872a64547cb6c57ce8af9a09fc6c3d8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "75651902"
---
# <a name="microsoftcomputeusernametextbox-ui-element"></a>Micro soft. compute. UserNameTextBox UI-element

Een besturings element tekstvak met ingebouwde validatie voor Windows-en Linux-gebruikers namen.

## <a name="ui-sample"></a>UI-voor beeld

![Microsoft.Compute.UserNameTextBox](./media/managed-application-elements/microsoft.compute.usernametextbox.png)

## <a name="schema"></a>Schema

```json
{
  "name": "element1",
  "type": "Microsoft.Compute.UserNameTextBox",
  "label": "User name",
  "defaultValue": "",
  "toolTip": "",
  "constraints": {
    "required": true,
    "regex": "^[a-z0-9A-Z]{1,30}$",
    "validationMessage": "Only alphanumeric characters are allowed, and the value must be 1-30 characters long."
  },
  "osPlatform": "Windows",
  "visible": true
}
```

## <a name="sample-output"></a>Voorbeelduitvoer

```json
"Example name"
```

## <a name="remarks"></a>Opmerkingen

- Als `constraints.required` is ingesteld op **waar**, moet het tekstvak een waarde hebben om te kunnen valideren. De standaard waarde is **True**.
- `osPlatform`moet worden opgegeven, en kan **Windows** of **Linux**zijn.
- `constraints.regex`is een reguliere java script-expressie patroon. Indien opgegeven, moet de waarde van het tekstvak overeenkomen met het patroon om te valideren. De standaard waarde is **Null**.
- `constraints.validationMessage`is een teken reeks die moet worden weer gegeven wanneer de waarde van het tekstvak niet `constraints.regex`voldoet aan de validatie die is opgegeven door. Als dat niet is opgegeven, worden de ingebouwde validatie berichten van het tekstvak gebruikt. De standaard waarde is **Null**.
- Dit element heeft ingebouwde validatie die is gebaseerd op de waarde die is opgegeven voor `osPlatform`. De ingebouwde validatie kan worden gebruikt in combi natie met een aangepaste reguliere expressie. Als een waarde `constraints.regex` is opgegeven, worden de ingebouwde en aangepaste validaties geactiveerd.

## <a name="next-steps"></a>Volgende stappen

* Zie aan de slag [met CreateUiDefinition](create-uidefinition-overview.md)voor een inleiding tot het maken van UI-definities.
* Zie [CreateUiDefinition-elementen](create-uidefinition-elements.md)voor een beschrijving van algemene eigenschappen in UI-elementen.
