---
title: Voor beeld van trigger en binding Azure Functions
description: Meer informatie over het configureren van Azure function-bindingen
author: craigshoemaker
ms.topic: reference
ms.date: 02/18/2019
ms.author: cshoe
ms.openlocfilehash: 8685c0fe02ad6c68918736e857c2015e2bfb4595
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "74227246"
---
# <a name="azure-functions-trigger-and-binding-example"></a>Voor beeld van trigger en binding Azure Functions

In dit artikel wordt beschreven hoe u een [trigger en bindingen](./functions-triggers-bindings.md) configureert in een Azure-functie.

Stel dat u een nieuwe rij wilt schrijven naar Azure-tabel opslag wanneer er een nieuw bericht wordt weer gegeven in de Azure-wachtrij opslag. Dit scenario kan worden geïmplementeerd met een Azure Queue Storage-trigger en een Azure Table Storage-uitvoerbinding. 

Hier is een *Function. json* -bestand voor dit scenario. 

```json
{
  "bindings": [
    {
      "type": "queueTrigger",
      "direction": "in",
      "name": "order",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "type": "table",
      "direction": "out",
      "name": "$return",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

Het eerste element in de `bindings` matrix is de opslag trigger voor de wachtrij. De `type` eigenschappen `direction` en identificeren de trigger. De `name` eigenschap identificeert de functie parameter die de inhoud van het wachtrij bericht ontvangt. De naam van de wachtrij die moet worden bewaakt `queueName`, en de Connection String bevindt zich in de app `connection`-instelling geïdentificeerd door.

Het tweede element in de `bindings` matrix is de Azure Table Storage-uitvoer binding. De `type` eigenschappen `direction` en identificeren de binding. De `name` eigenschap geeft aan hoe de functie de nieuwe tabelrij levert, in dit geval met behulp van de functie retour waarde. De naam van de tabel is in `tableName`en de Connection String bevindt zich in de app- `connection`instelling geïdentificeerd door.

Als u de inhoud van *Function. json* in de Azure Portal wilt bekijken en bewerken, klikt u op het tabblad **integreren** van uw functie op de optie **Geavanceerde editor** .

> [!NOTE]
> De waarde van `connection` is de naam van een app-instelling die de Connection String bevat, niet de connection string zelf. Bindingen gebruiken verbindings reeksen die zijn opgeslagen in de app-instellingen voor het afdwingen van de best practice die *Function. json* bevat geen service geheimen.

## <a name="c-script-example"></a>Voor beeld van C#-script

Hier volgt een C#-script code die werkt met deze trigger en binding. U ziet dat de naam van de para meter die de inhoud van het `order`wachtrij bericht levert, is; Deze naam is vereist omdat de `name` waarde van de eigenschap in *Function. json* is`order` 

```cs
#r "Newtonsoft.Json"

using Microsoft.Extensions.Logging;
using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table storage
// The method return value creates a new row in Table Storage
public static Person Run(JObject order, ILogger log)
{
    return new Person() { 
            PartitionKey = "Orders", 
            RowKey = Guid.NewGuid().ToString(),  
            Name = order["Name"].ToString(),
            MobileNumber = order["MobileNumber"].ToString() };  
}
 
public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

## <a name="javascript-example"></a>Java script-voor beeld

Hetzelfde *Function. json* -bestand kan worden gebruikt met een Java script-functie:

```javascript
// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The second parameter to context.done is used as the value for the new row
module.exports = function (context, order) {
    order.PartitionKey = "Orders";
    order.RowKey = generateRandomId(); 

    context.done(null, order);
};

function generateRandomId() {
    return Math.random().toString(36).substring(2, 15) +
        Math.random().toString(36).substring(2, 15);
}
```

## <a name="class-library-example"></a>Class Library-voor beeld

In een klassen bibliotheek worden dezelfde trigger-en binding informatie &mdash; wachtrij en tabel namen, opslag accounts, functie parameters voor invoer en uitvoer &mdash; verstrekt door kenmerken in plaats van een function. JSON-bestand. Hier volgt een voorbeeld:

```csharp
public static class QueueTriggerTableOutput
{
    [FunctionName("QueueTriggerTableOutput")]
    [return: Table("outTable", Connection = "MY_TABLE_STORAGE_ACCT_APP_SETTING")]
    public static Person Run(
        [QueueTrigger("myqueue-items", Connection = "MY_STORAGE_ACCT_APP_SETTING")]JObject order,
        ILogger log)
    {
        return new Person() {
                PartitionKey = "Orders",
                RowKey = Guid.NewGuid().ToString(),
                Name = order["Name"].ToString(),
                MobileNumber = order["MobileNumber"].ToString() };
    }
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

U hebt nu een werk functie die wordt geactiveerd door een Azure-wachtrij en waarmee gegevens worden uitgevoerd naar Azure Table Storage.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Expressie patronen voor de binding Azure Functions](./functions-bindings-expressions-patterns.md)
