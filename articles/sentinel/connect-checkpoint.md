---
title: Gegevens van Check Point verbinden met Azure Sentinel | Microsoft Docs
description: Meer informatie over het verbinden van Check Point-gegevens met Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 70836ec557eff1be035d92e8e7db30a882e05fc6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "77588413"
---
# <a name="connect-check-point-to-azure-sentinel"></a>Controle punt verbinden met Azure Sentinel



In dit artikel wordt uitgelegd hoe u uw Check Point-toestel verbindt met Azure Sentinel. Met de Data Connector voor controle punten kunt u eenvoudig uw controle punt logboeken verbinden met Azure Sentinel, voor het weer geven van Dash boards, het maken van aangepaste waarschuwingen en het verbeteren van onderzoek. Met behulp van Check Point op Azure Sentinel krijgt u meer inzicht in het Internet gebruik van uw organisatie en worden de mogelijkheden voor beveiligings bewerkingen verbeterd. 

## <a name="forward-check-point-logs-to-the-syslog-agent"></a>Controle logboeken naar de syslog-agent sturen

Configureer uw Check Point-toestel voor het door sturen van syslog-berichten in de CEF-indeling naar uw Azure-werk ruimte via de syslog-agent.

1. Ga naar [controle punt logboek exporteren](https://aka.ms/asi-syslog-checkpoint-forwarding).
1. Schuif omlaag naar de **basis implementatie** en volg de instructies voor het instellen van de verbinding, met behulp van de volgende richt lijnen:
   - Stel de **syslog-poort** in op **514** of de poort die u op de agent hebt ingesteld.
     - Vervang de **naam** en het IP-adres van de **doel server** in de CLI met de naam van de syslog-agent en het IP-adres.
     - Stel de indeling in op **CEF**.
1. Als u versie R 77.30 of R 80.10 gebruikt, schuift u omhoog naar **installaties** en volgt u de instructies voor het installeren van een logboek-export programma voor uw versie.
1. Ga door naar [stap 3: de verbinding valideren](connect-cef-verify.md).
 

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u apparaten van Check Point verbindt met Azure Sentinel. Raadpleeg de volgende artikelen voor meer informatie over Azure Sentinel:
- [Connectiviteit valideren](connect-cef-verify.md).
- Ga aan de slag [met het detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Gebruik werkmappen](tutorial-monitor-your-data.md) om uw gegevens te bewaken.


