---
title: WHERE-component in Azure Cosmos DB
description: Meer informatie over SQL WHERE-component voor Azure Cosmos DB
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: tisande
ms.openlocfilehash: 483a0533eafc81ef8698d260a753062ae074f6d4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "78898787"
---
# <a name="where-clause-in-azure-cosmos-db"></a>WHERE-component in Azure Cosmos DB

De optionele WHERE-component`WHERE <filter_condition>`() geeft voor waarde (n) aan waaraan de bron-JSON-items moeten voldoen om ze in de resultaten op te vragen. Een JSON-item moet de opgegeven voor waarden `true` evalueren om in aanmerking te komen voor het resultaat. De index laag maakt gebruik van de WHERE-component om de kleinste subset van bron items te bepalen die deel kunnen uitmaken van het resultaat.
  
## <a name="syntax"></a>Syntaxis
  
```sql  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
## <a name="arguments"></a>Argumenten

- `<filter_condition>`  
  
   Hiermee geeft u de voor waarde op waaraan moet worden voldaan om de documenten te retour neren.  
  
- `<scalar_expression>`  
  
   Expressie die de waarde vertegenwoordigt die moet worden berekend. Zie [scalaire expressies](sql-query-scalar-expressions.md) voor meer informatie.  
  
## <a name="remarks"></a>Opmerkingen
  
  Om het document als resultaat te retour neren moet een expressie worden geretourneerd die is opgegeven als filter voorwaarde, waarna resulteert in waar. Alleen Booleaanse waarden `true` voldoen aan de voor waarde, een andere waarde: niet gedefinieerd, null, onwaar, getal, matrix of object voldoet niet aan de voor waarde.

  Als u de partitie sleutel in de `WHERE` -component opneemt als onderdeel van een gelijkheids filter, wordt de query automatisch gefilterd op de relevante partities.

## <a name="examples"></a>Voorbeelden

Met de volgende query worden items opgevraagd die `id` een eigenschap bevatten waarvan `AndersenFamily`de waarde is. Hiermee wordt een item uitgesloten dat geen `id` eigenschap heeft of waarvan de waarde niet overeenkomt `AndersenFamily`.

```sql
    SELECT f.address
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

U ziet deze uitvoer:

```json
    [{
      "address": {
        "state": "WA",
        "county": "King",
        "city": "Seattle"
      }
    }]
```

### <a name="scalar-expressions-in-the-where-clause"></a>Scalaire expressies in de component WHERE

Het vorige voorbeeld bevatte een eenvoudige gelijkheidsquery. De SQL-API ondersteunt ook verschillende [scalaire expressies](sql-query-scalar-expressions.md). Binaire en unaire expressies worden het meest gebruikt. Eigenschapsverwijzingen vanaf het JSON-bronobject zijn ook geldige expressies.

U kunt de volgende ondersteunde binaire Opera tors gebruiken:  

|**Operatortype**  | **Waarden** |
|---------|---------|
|Rekenkundig | +,-,*,/,% |
|Bitsgewijs    | \|, &, ^, <<, >>, >>> (opvulling met nullen shift-rechts) |
|Logisch    | EN, OF, NIET      |
|Vergelijking | =, !=, &lt;, &gt;, &lt;=, &gt;=, <> |
|Tekenreeks     |  \|\| (samenvoegen) |

De volgende query's gebruiken binaire Opera tors:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5    -- matching grades == 5
```

U kunt ook de unaire Opera Tors +,-, ~ en niet in query's gebruiken, zoals wordt weer gegeven in de volgende voor beelden:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5
```

U kunt ook eigenschaps verwijzingen gebruiken in query's. `SELECT * FROM Families f WHERE f.isRegistered` Retourneert bijvoorbeeld het JSON-item met de eigenschap `isRegistered` met de waarde gelijk aan `true`. Elke andere waarde, zoals `false`, `null` `Undefined` `<number>` `<string>` `<object>`,,,,, of `<array>`, sluit het item uit van het resultaat.

## <a name="next-steps"></a>Volgende stappen

- [Aan de slag](sql-query-getting-started.md)
- [Sleutelwoord IN](sql-query-keywords.md#in)
- [FROM-component](sql-query-from.md)