---
title: Een Azure Stream Analytics-taak lokaal testen met voorbeeld gegevens met Visual Studio code
description: In dit artikel wordt beschreven hoe u query's lokaal kunt testen met voorbeeld gegevens met behulp van Azure Stream Analytics-Hulpprogram Ma's voor Visual Studio code.
ms.service: stream-analytics
author: su-jie
ms.author: sujie
ms.date: 11/10/2019
ms.topic: conceptual
ms.openlocfilehash: c29d0d9ecd856ee9611df21d23b1b2b763e24652
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "75486467"
---
# <a name="test-stream-analytics-queries-locally-with-sample-data-using-visual-studio-code"></a>Stream Analytics query's lokaal met voorbeeld gegevens testen met Visual Studio code

U kunt Azure Stream Analytics-hulpprogram ma's voor Visual Studio code gebruiken om uw Stream Analytics-taken lokaal te testen met voorbeeld gegevens. U vindt de query resultaten in JSON-bestanden in de map **LocalRunOutputs** van het project.

## <a name="prerequisites"></a>Vereisten

* Installeer de [.net core-SDK](https://dotnet.microsoft.com/download) en Start Visual Studio code opnieuw.

* Gebruik deze [Quick](quick-create-vs-code.md) start om te leren hoe u met Visual Studio Code een stream Analytics taak maakt.

## <a name="prepare-sample-data"></a>Voorbeeld gegevens voorbereiden

U moet eerst voorbeeld invoer gegevens bestanden voorbereiden. Als u al een aantal voorbeeld gegevens bestanden op uw computer hebt, kunt u deze stap overs Laan en door gaan naar de volgende.

1. Klik op **voor beeld van gegevens** in uw invoer configuratie bestand op de bovenste regel. Sommige invoer gegevens worden opgehaald uit IoT Hub en weer gegeven in het voorbeeld venster. Houd er rekening mee dat dit enige tijd kan duren.

2. Zodra de gegevens zijn weer gegeven, klikt u op **Opslaan als** om de gegevens op te slaan in een lokaal bestand.

 ![Live-invoer preview](./media/quick-create-vs-code/preview-live-input.png)

## <a name="define-a-local-input"></a>Een lokale invoer definiëren

1. Klik op **input. json** onder map met invoer in uw stream Analytics-project. Selecteer vervolgens **lokale invoer toevoegen** van de bovenste regel.

    ![Lokale invoer uit project toevoegen](./media/quick-create-vs-code/add-input-from-project.png)

    U kunt ook **CTRL + SHIFT + P** gebruiken om het opdracht palet te openen en **ASA: invoer toevoegen**.

   ![Stream Analytics invoer in VS code toevoegen](./media/quick-create-vs-code/add-input.png)

2. Selecteer **lokale invoer**.

    ![Lokale invoer van ASA toevoegen in Visual Studio code](./media/vscode-local-run/add-local-input.png)

3. Selecteer **+ nieuwe lokale invoer**.

    ![Een nieuwe lokale invoer van ASA toevoegen in Visual Studio code](./media/vscode-local-run/add-new-local-input.png)

4. Voer dezelfde invoer alias in die u in uw query hebt gebruikt.

    ![Een nieuwe lokale invoer-alias voor ASA toevoegen](./media/vscode-local-run/new-local-input-alias.png)

5. Voer in het zojuist gegenereerde **LocalInput_Input. json** -bestand het bestandspad in waar uw lokale gegevens bestand zich bevindt.

    ![Lokaal bestandspad invoeren in Visual Studio](./media/vscode-local-run/local-file-path.png)

6. Selecteer **Preview-gegevens** om de invoer gegevens te bekijken. Het type serialisatie voor uw gegevens wordt automatisch gedetecteerd als de JSON of CSV. Gebruik de selector om uw gegevens in **tabel** -of **RAW** -indeling weer te geven. De volgende tabel bevat een voor beeld van gegevens in de **tabel indeling**:

     ![Voor beeld van lokale gegevens in tabel indeling](./media/vscode-local-run/local-file-preview-table.png)

    De volgende tabel bevat een voor beeld van de gegevens in **RAW-indeling**:

    ![Voor beeld van lokale gegevens in RAW-indeling](./media/vscode-local-run/local-file-preview-raw.png)

## <a name="run-queries-locally"></a>Query's lokaal uitvoeren

Ga terug naar de query-editor en selecteer **lokaal uitvoeren**. Selecteer vervolgens **lokale invoer gebruiken** in de vervolg keuzelijst.

![Selecteer lokaal uitvoeren in de query-editor](./media/vscode-local-run/run-locally.png)

![Lokale invoer gebruiken](./media/vscode-local-run/run-locally-use-local-input.png)

Het resultaat wordt weer gegeven in het rechterdeel venster. U kunt op **uitvoeren** klikken om opnieuw te testen. U kunt ook **openen in map** selecteren om de resultaat bestanden in Verkenner te bekijken en deze vervolgens te openen met andere hulpprogram ma's. Houd er rekening mee dat de resultaat bestanden alleen beschikbaar zijn in JSON-indeling.

![Resultaat van lokale uitvoering weer geven](./media/vscode-local-run/run-locally-result.png)

## <a name="next-steps"></a>Volgende stappen

* [Azure Stream Analytics taken lokaal met live input testen met Visual Studio code](visual-studio-code-local-run-live-input.md)

* [Azure Stream Analytics-taken verkennen met Visual Studio code (preview)](visual-studio-code-explore-jobs.md)
