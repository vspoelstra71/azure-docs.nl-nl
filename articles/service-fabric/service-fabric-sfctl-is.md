---
title: Azure Service Fabric CLI-sfctl is
description: Meer informatie over sfctl, de Azure Service Fabric-opdracht regel interface. Bevat een lijst met opdrachten voor het beheren van de infra structuur.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: a64aaca97c9df61d795c82d64e6048d6f9ae032f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "76906090"
---
# <a name="sfctl-is"></a>sfctl is
Query's uitvoeren en opdrachten verzenden naar de infrastructuur service.

## <a name="commands"></a>Opdrachten

|Opdracht|Beschrijving|
| --- | --- |
| command | Hiermee wordt een beheer opdracht aangeroepen voor het opgegeven infrastructuur service-exemplaar. |
| query | Hiermee wordt een alleen-lezen query aangeroepen voor het opgegeven infrastructuur service-exemplaar. |

## <a name="sfctl-is-command"></a>sfctl is opdracht
Hiermee wordt een beheer opdracht aangeroepen voor het opgegeven infrastructuur service-exemplaar.

Voor clusters waarvoor een of meer exemplaren van de infrastructuur service zijn geconfigureerd, biedt deze API een manier om met een infra structuur-specifieke opdracht naar een bepaald exemplaar van de infrastructuur service te verzenden. Beschik bare opdrachten en de bijbehorende antwoord indelingen variëren, afhankelijk van de infra structuur waarop het cluster wordt uitgevoerd. Deze API ondersteunt het Service Fabric-platform. het is niet bedoeld om rechtstreeks vanuit uw code te worden gebruikt.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --opdracht [vereist] | De tekst van de opdracht die moet worden aangeroepen. De inhoud van de opdracht is specifiek voor een infra structuur. |
| --Service-id | De identiteit van de infrastructuur service. <br><br> Dit is de volledige naam van de infrastructuur service zonder het URI-\:schema ' fabric '. Deze para meter is alleen vereist voor het cluster waarop meer dan één exemplaar van de infrastructuur service wordt uitgevoerd. |
| --time-out-t | Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --fouten opsporen | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |

## <a name="sfctl-is-query"></a>sfctl is query
Hiermee wordt een alleen-lezen query aangeroepen voor het opgegeven infrastructuur service-exemplaar.

Voor clusters waarvoor een of meer exemplaren van de infrastructuur service zijn geconfigureerd, biedt deze API een manier om infrastructuur query's te verzenden naar een bepaald exemplaar van de infrastructuur service. Beschik bare opdrachten en de bijbehorende antwoord indelingen variëren, afhankelijk van de infra structuur waarop het cluster wordt uitgevoerd. Deze API ondersteunt het Service Fabric-platform. het is niet bedoeld om rechtstreeks vanuit uw code te worden gebruikt.

### <a name="arguments"></a>Argumenten

|Argument|Beschrijving|
| --- | --- |
| --opdracht [vereist] | De tekst van de opdracht die moet worden aangeroepen. De inhoud van de opdracht is specifiek voor een infra structuur. |
| --Service-id | De identiteit van de infrastructuur service. <br><br> Dit is de volledige naam van de infrastructuur service zonder het URI-\:schema ' fabric '. Deze para meter is alleen vereist voor het cluster waarop meer dan één exemplaar van de infrastructuur service wordt uitgevoerd. |
| --time-out-t | Standaard\: 60. |

### <a name="global-arguments"></a>Algemene argumenten

|Argument|Beschrijving|
| --- | --- |
| --fouten opsporen | Verg root logboek registratie uitgebreid om alle logboeken voor fout opsporing weer te geven. |
| --Help-h | Dit Help-bericht weer geven en afsluiten. |
| --uitvoer-o | Uitvoer indeling.  Toegestane waarden\: JSON, jsonc, Table, TSV.  Standaard\: JSON. |
| --query | JMESPath-query reeks. Zie http\://jmespath.org/voor meer informatie en voor beelden. |
| --verbose | Uitgebreide logboek registratie verhogen. Gebruik--debug voor volledige logboeken voor fout opsporing. |


## <a name="next-steps"></a>Volgende stappen
- [Stel](service-fabric-cli.md) de service Fabric cli in.
- Meer informatie over het gebruik van de Service Fabric CLI met behulp van de [voorbeeld scripts](/azure/service-fabric/scripts/sfctl-upgrade-application).
