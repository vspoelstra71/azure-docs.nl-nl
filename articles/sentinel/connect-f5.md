---
title: F5 ASM-gegevens verbinden met Azure Sentinel | Microsoft Docs
description: Meer informatie over het verbinden van F5 ASM-gegevens naar Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 0001cad6-699c-4ca9-b66c-80c194e439a5
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/20/2020
ms.author: yelevin
ms.openlocfilehash: bee8ec9b6532042659d54d590a29d49e7ce07c92
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2020
ms.locfileid: "83715831"
---
# <a name="connect-f5-asm-to-azure-sentinel"></a>Verbinding maken met F5 ASM naar Azure Sentinel

In dit artikel wordt uitgelegd hoe u de F5 ASM Data Connector kunt gebruiken om uw F5 ASM-Logboeken eenvoudig in te trekken bij Azure Sentinel. Zo kunt u F5 ASM-gegevens weer geven in werkmappen, gebruiken om aangepaste waarschuwingen te maken en deze op te nemen om het onderzoek te verbeteren. Met F5 ASM-gegevens in azure Sentinel krijgt u meer inzicht in de beveiliging van de webtoepassingen van uw organisatie en worden de mogelijkheden voor beveiligings bewerkingen verbeterd. 

## <a name="configure-your-f5-asm-to-send-cef-messages"></a>Uw F5 ASM configureren voor het verzenden van CEF-berichten

1. Volg de instructies in [F5 logboek registratie van toepassings beveiligings gebeurtenissen configureren](https://techdocs.f5.com/kb/en-us/products/big-ip_asm/manuals/product/asm-implementations-11-5-0/12.html) om externe logboek registratie in te stellen met behulp van de volgende richt lijnen:
   - Stel het **externe-opslag type** in op **CEF**.
   - Stel het **protocol** in op **TCP**.
   - Stel het **IP-adres** in op het IP-adres van de syslog-server.
   - Stel het **poort nummer** in op **514**of de poort die u voor het gebruik van uw agent hebt ingesteld.
   - U kunt de **maximale grootte van de query teken reeks** instellen op de grootte die u in uw agent hebt ingesteld.

1. Zoek naar om het relevante schema in Log Analytics te gebruiken voor CEF-gebeurtenissen `CommonSecurityLog` .

1. Ga door naar [stap 3: de verbinding valideren](connect-cef-verify.md).


## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u met F5 ASM verbinding maakt met Azure Sentinel. Raadpleeg de volgende artikelen voor meer informatie over Azure Sentinel:
- Meer informatie over hoe u [inzicht krijgt in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Ga aan de slag [met het detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).
- [Gebruik werkmappen](tutorial-monitor-your-data.md) om uw gegevens te bewaken.

