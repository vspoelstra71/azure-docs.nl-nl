---
title: Verbinding maken tussen Zimperium Mobile Threat Protection en Azure Sentinel | Microsoft Docs
description: Meer informatie over het verbinden van Zimperium Mobile Threat Protection naar Azure Sentinel.
services: sentinel
author: yelevin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/20/2020
ms.author: yelevin
ms.openlocfilehash: 86854fa22a49f09e5d3d2fc5fdb53c245850fbac
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "77587937"
---
# <a name="connect-your-zimperium-mobile-threat-defense-to-azure-sentinel"></a>Uw Zimperium Mobile Threat Protection koppelen aan Azure Sentinel


> [!IMPORTANT]
> De Zimperium Mobile Threat verdediging Data Connector in azure Sentinel is momenteel beschikbaar als open bare preview.
> Deze functie wordt zonder service level agreement gegeven en wordt niet aanbevolen voor productie werkbelastingen. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie voor meer informatie [aanvullende gebruiks voorwaarden voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



Met de Zimperium Mobile Threat verdedigings connector kunt u het Zimperium-bedreigings logboek verbinden met Azure Sentinel om Dash boards weer te geven, aangepaste waarschuwingen te maken en het onderzoek te verbeteren. Dit geeft u meer inzicht in het mobiele bedreigings land van uw organisatie en verbetert de mogelijkheden voor beveiligings bewerkingen.

> [!NOTE]
> Gegevens worden opgeslagen in de geografische locatie van de werk ruimte waarop u Azure Sentinel uitvoert.

## <a name="configure-and-connect-zimperium-mobile-threat-defense"></a>Zimperium Mobile Threat verdediging configureren en verbinden

Zimperium Mobile Threat verdediging kan Logboeken rechtstreeks integreren en exporteren naar **Azure Sentinel**.

1. Klik in de Azure-Sentinel Portal op Data connectors en selecteer **Zimperium Mobile Threat verdediging**.
2. Selecteer de **pagina connector openen**.
3. Volg de instructies op de pagina **Zimperium Mobile Threat verdediging** connector als volgt:
 1. Klik in zConsole op **beheren** op de navigatie balk.
 2. Klik op het tabblad **Integrations**.
 3. Klik op de knop **Threat Reporting** en vervolgens op de knop **integraties toevoegen** .
 4. Maak de integratie door **Microsoft Azure Sentinel** uit de beschik bare integraties te selecteren en de werk ruimte-id en de primaire sleutel op te geven om verbinding te maken met Azure Sentinel.
 5. Optie voor het selecteren van een filter niveau voor de bedreigings gegevens die naar Azure Sentinel moeten worden gepusht, is ook beschikbaar. 

4. Raadpleeg de [Zimperium-portal voor klanten ondersteuning](https://support.zimperium.com)voor meer informatie.


## <a name="find-your-data"></a>Uw gegevens zoeken

Nadat de verbinding tot stand is gebracht, worden de gegevens weer gegeven in Log Analytics onder CustomLogs ZimperiumThreatLog_CL en ZimperiumMitigationLog_CL.

Als u het relevante schema in Log Analytics wilt gebruiken voor de Zimperium Mobile Threat verdediging, zoekt u naar ZimperiumThreatLog_CL en ZimperiumMitigationLog_CL.


## <a name="validate-connectivity"></a>Connectiviteit valideren

Het kan 20 minuten duren voordat uw logboeken in Log Analytics worden weer gegeven.

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe u Zimperium Mobile Threat Protection kunt verbinden met Azure Sentinel. Raadpleeg de volgende artikelen voor meer informatie over Azure Sentinel:

- Meer informatie over hoe u [inzicht krijgt in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).

- Ga aan de slag [met het detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats-built-in.md).

- [Gebruik werkmappen](tutorial-monitor-your-data.md) om uw gegevens te bewaken.

Ga voor meer informatie over Zimperium naar het volgende:

- [Zimperium](https://zimperium.com)

- [Zimperium Mobile Security-Blog](https://blog.zimperium.com)

- [Zimperium-portal voor klant ondersteuning](https://support.zimperium.com)

