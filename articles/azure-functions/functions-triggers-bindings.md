---
title: Triggers en bindingen in Azure Functions
description: Meer informatie over het gebruik van triggers en bindingen om uw Azure-functie te koppelen aan online gebeurtenissen en Cloud Services.
author: craigshoemaker
ms.topic: reference
ms.date: 02/18/2019
ms.author: cshoe
ms.openlocfilehash: d41fd7f66ecef3a563345424d7dc4366e47d3f0e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "79276502"
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Concepten van Azure Functions-triggers en -bindingen

In dit artikel vindt u informatie over de concepten van het hoge niveau rond functies triggers en bindingen.

Triggers zijn de oorzaak dat een functie wordt uitgevoerd. Een trigger definieert hoe een functie wordt aangeroepen en een functie moet precies één trigger hebben. Aan triggers zijn gegevens gekoppeld. Meestal is dit de nettolading waarmee de functie is geactiveerd. 

Een binding met een functie is een manier om een andere resource declaratief aan de functie te koppelen. bindingen kunnen worden verbonden als *invoer bindingen*, *uitvoer bindingen*of beide. Gegevens van bindingen worden als parameters aan de functie geleverd.

U kunt verschillende bindingen mixen en matchen om aan uw behoeften te voldoen. Bindingen zijn optioneel en een functie kan een of meerdere invoer- en/of uitvoerbindingen hebben.

Met triggers en bindingen kunt u hardcoding toegang tot andere services voor komen. Uw functie ontvangt gegevens (bijvoorbeeld de inhoud van een wachtrijbericht) in functieparameters. U verzendt gegevens (bijvoorbeeld om een ​​wachtrijbericht te maken) door de retourwaarde van de functie te gebruiken. 

Bekijk de volgende voor beelden van hoe u verschillende functies kunt implementeren.

| Voorbeeldscenario | Trigger | Invoer binding | Uitvoer binding |
|-------------|---------|---------------|----------------|
| Er wordt een nieuw wachtrij bericht ontvangen waarmee een functie wordt uitgevoerd om naar een andere wachtrij te schrijven. | Wachtrij<sup>*</sup> | *Geen* | Wachtrij<sup>*</sup> |
|Met een geplande taak wordt Blob Storage inhoud gelezen en wordt er een nieuw Cosmos DB document gemaakt. | Timer | Blob Storage | Cosmos DB |
|De Event Grid wordt gebruikt voor het lezen van een afbeelding van Blob Storage en een document van Cosmos DB om een e-mail bericht te verzenden. | Event Grid | Blob Storage en Cosmos DB | SendGrid |
| Een webhook die gebruikmaakt van Microsoft Graph om een Excel-werk blad bij te werken. | HTTP | *Geen* | Microsoft Graph |

<sup>\*</sup>Vertegenwoordigt verschillende wacht rijen

Deze voor beelden zijn niet volledig, maar zijn bedoeld om te laten zien hoe u triggers en bindingen samen kunt gebruiken.

###  <a name="trigger-and-binding-definitions"></a>Definities van triggers en bindingen

Triggers en bindingen worden op verschillende manieren gedefinieerd, afhankelijk van de ontwikkelings benadering.

| Platform | Triggers en bindingen worden geconfigureerd door... |
|-------------|--------------------------------------------|
| C#-klassen bibliotheek | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;de methoden en para meters voor C#-kenmerken |
| Alle andere (inclusief Azure Portal) | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Function. json](./functions-reference.md) ([schema](http://json.schemastore.org/function)) bijwerken |

De portal biedt een gebruikers interface voor deze configuratie, maar u kunt het bestand rechtstreeks bewerken door de **Geavanceerde editor** te openen die beschikbaar is via het tabblad **integreren** van uw functie.

In .NET definieert het parameter type het gegevens type voor invoer gegevens. Gebruik `string` bijvoorbeeld om een binding te maken met de tekst van een wachtrij trigger, een byte matrix die als binair moet worden gelezen en een aangepast type om te deserialiseren naar een object.

Voor talen die dynamisch worden getypt, zoals Java script, `dataType` gebruikt u de eigenschap in het bestand *Function. json* . Als u bijvoorbeeld de inhoud van een HTTP-aanvraag in binaire indeling wilt lezen, `dataType` stelt `binary`u het volgende in:

```json
{
    "dataType": "binary",
    "type": "httpTrigger",
    "name": "req",
    "direction": "in"
}
```

Andere opties voor `dataType` zijn `stream` en `string`.

## <a name="binding-direction"></a>Bindings richting

Alle triggers en bindingen hebben een `direction` eigenschap in het bestand [Function. json](./functions-reference.md) :

- Voor triggers is de richting altijd`in`
- Invoer-en uitvoer bindingen `in` gebruiken en`out`
- Sommige bindingen bieden ondersteuning voor een `inout`speciale richting. Als u gebruikt `inout`, is alleen de **Geavanceerde editor** beschikbaar via het tabblad **integreren** in de portal.

Wanneer u [kenmerken in een klassen bibliotheek](functions-dotnet-class-library.md) gebruikt om triggers en bindingen te configureren, wordt de richting in een kenmerk-constructor of afgeleid van het parameter type gegeven.

## <a name="supported-bindings"></a>Ondersteunde bindingen

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

Zie [ondersteunde talen](supported-languages.md)voor informatie over welke bindingen in Preview zijn of die zijn goedgekeurd voor productie gebruik.

## <a name="resources"></a>Resources
- [Expressies en patronen binden](./functions-bindings-expressions-patterns.md)
- [De functie retour waarde van Azure gebruiken](./functions-bindings-return-value.md)
- [Een bindings expressie registreren](./functions-bindings-register.md)
- Testen
  - [Strategieën voor het testen van uw code in Azure Functions](functions-test-a-function.md)
  - [Handmatig een niet door HTTP geactiveerde functie uitvoeren](functions-manually-run-non-http.md)
- [Bindings fouten verwerken](./functions-bindings-errors.md)

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Azure Functions bindings uitbreidingen registreren](./functions-bindings-register.md)
