---
title: Azure Stream Analytics-taken in Visual Studio code verkennen
description: In dit artikel wordt beschreven hoe u een Azure Stream Analytics-taak exporteert naar een lokaal project, taken lijst en taak entiteiten weer geven.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 05/15/2019
ms.topic: conceptual
ms.openlocfilehash: 1d3a02d3778f9b4113767c5f755d675aeadd901b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "75479239"
---
# <a name="explore-azure-stream-analytics-with-visual-studio-code-preview"></a>Azure Stream Analytics verkennen met Visual Studio code (preview)

De Azure Stream Analytics voor Visual Studio code-extensie biedt ontwikkel aars een licht gewicht ervaring voor het beheren van hun Stream Analytics taken. Het kan worden gebruikt in Windows, Mac en Linux. Met de Azure Stream Analytics extensie kunt u het volgende doen:

- Taken [maken](quick-create-vs-code.md), starten en stoppen
- Bestaande taken exporteren naar een lokaal project
- Taken weer geven en taak entiteiten bekijken

## <a name="export-a-job-to-a-local-project"></a>Een taak exporteren naar een lokaal project

Als u een taak wilt exporteren naar een lokaal project, zoekt u de taak die u wilt exporteren in de **Stream Analytics Explorer** in Visual Studio code. Selecteer vervolgens een map voor uw project. Het project wordt geëxporteerd naar de map die u selecteert, en u kunt de taak blijven beheren vanuit Visual Studio code. Zie Visual Studio code [Quick](quick-create-vs-code.md)start (Engelstalig) voor meer informatie over het gebruik van Visual Studio code voor het beheren van stream Analytics taken.

![De taak ASA exporteren in Visual Studio code](./media/vscode-explore-jobs/export-job.png)

## <a name="list-job-and-view-job-entities"></a>Taak weer geven en taak entiteiten bekijken

U kunt de taak weergave gebruiken om te communiceren met Azure Stream Analytics taken van Visual Studio.


1. Klik op het pictogram van **Azure** op de activiteiten balk van Visual Studio en vouw vervolgens **Stream Analytics knoop punt**uit. Uw taken moeten worden weer gegeven onder uw abonnementen.

   ![Stream Analytics Explorer openen](./media/vscode-explore-jobs/open-explorer.png)

2. Uw taak knooppunt uitvouwen. u kunt de taak query, configuratie, invoer, uitvoer en functies openen en weer geven. 

3. Klik met de rechter muisknop op uw taak knooppunt en kies de **taak weergave openen in het portal** -knoop punt om de taak weergave te openen in de Azure Portal.

   ![Taak weergave openen in de portal](./media/vscode-explore-jobs/open-job-view.png)

## <a name="next-steps"></a>Volgende stappen

* [Een Azure Stream Analytics Cloud taak maken in Visual Studio code (preview)](quick-create-vs-code.md)
