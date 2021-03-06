---
title: Azure Automation-taakgegevens doorsturen naar Azure Monitor-logboeken
description: In dit artikel leest u hoe u taak status-en runbook-taak stromen verzendt om logboeken te Azure Monitor.
services: automation
ms.subservice: process-automation
ms.date: 05/22/2020
ms.topic: conceptual
ms.openlocfilehash: ba498fe9f70664a801172a6ff3705ac41a6371ef
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/25/2020
ms.locfileid: "83835241"
---
# <a name="forward-azure-automation-job-data-to-azure-monitor-logs"></a>Azure Automation-taakgegevens doorsturen naar Azure Monitor-logboeken

Azure Automation kunt de status van de runbook-taak en taak stromen verzenden naar uw Log Analytics-werk ruimte. Voor dit proces is het koppelen van werk ruimten niet vereist en is het volledig onafhankelijk. Taak logboeken en taak stromen zijn zichtbaar in de Azure Portal, of met Power shell, voor afzonderlijke taken. Dit biedt u de mogelijkheid om eenvoudig onderzoek uit te voeren. U kunt nu met Azure Monitor-logboeken:

* Krijg inzicht in de status van uw automatiserings taken.
* Een e-mail of waarschuwing activeren op basis van de status van uw runbook-taak (bijvoorbeeld mislukt of onderbroken).
* Schrijf geavanceerde query's over uw werk stromen.
* Verdeel taken tussen Automation-accounts.
* Gebruik aangepaste weer gaven en zoek query's om uw runbook-resultaten, de status van de runbook-taak en andere gerelateerde sleutel indicatoren of metrische gegevens te visualiseren.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites-and-deployment-considerations"></a>Vereisten en overwegingen voor de implementatie

Als u uw Automation-logboeken wilt gaan verzenden naar Azure Monitor-logboeken, hebt u het volgende nodig:

* De nieuwste versie van [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/).
* Een Log Analytics-werkruimte. Zie [aan de slag met Azure monitor-logboeken](../log-analytics/log-analytics-get-started.md)voor meer informatie.
* De resource-ID voor uw Azure Automation-account.

Gebruik de volgende opdracht om de resource-ID te vinden voor uw Azure Automation-account:

```powershell-interactive
# Find the ResourceId for the Automation account
Get-AzResource -ResourceType "Microsoft.Automation/automationAccounts"
```

Voer de volgende Power shell-opdracht uit om de resource-ID voor uw Log Analytics-werk ruimte te vinden:

```powershell-interactive
# Find the ResourceId for the Log Analytics workspace
Get-AzResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Als u meer dan één Automation-account of werk ruimte hebt in de uitvoer van de voor gaande opdrachten, kunt u de naam en andere gerelateerde eigenschappen vinden die deel uitmaken van de volledige Resource-ID van uw Automation-account door het volgende uit te voeren:

1. Selecteer in de Azure Portal uw Automation-account op de pagina **Automation-accounts** . 
2. Selecteer op de pagina van het geselecteerde Automation-account onder **account instellingen**de optie **Eigenschappen**.  
3. Op de pagina **Eigenschappen** ziet u de details die hieronder worden weer gegeven.

    ![Eigenschappen van Automation-account](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).

## <a name="azure-monitor-log-records"></a>Azure Monitor logboek records

Met Azure Automation diagnostische gegevens worden twee typen records gemaakt in Azure Monitor logboeken, gelabeld als `AzureDiagnostics` . De tabellen in de volgende secties zijn voor beelden van records die Azure Automation gegenereerd en de gegevens typen die worden weer gegeven in de zoek resultaten van het logboek.

### <a name="job-logs"></a>Taaklogboeken

| Eigenschap | Beschrijving |
| --- | --- |
| TimeGenerated |Datum en tijd van uitvoering van de runbooktaak. |
| RunbookName_s |De naam van het runbook. |
| Caller_s |De aanroeper die de bewerking heeft gestart. Mogelijke waarden zijn een e-mailadres of het systeem voor geplande taken. |
| Tenant_g | GUID waarmee de Tenant voor de oproepende functie wordt geïdentificeerd. |
| JobId_g |GUID waarmee de runbook-taak wordt geïdentificeerd. |
| ResultType |De status van de runbooktaak. Mogelijke waarden zijn:<br>-Nieuw<br>-Gemaakt<br>- Gestart<br>- Gestopt<br>- Onderbroken<br>- Mislukt<br>-Voltooid |
| Categorie | Classificatie van het type gegevens. Voor Automation is de waarde JobLogs. |
| OperationName | Het type bewerking dat in azure wordt uitgevoerd. Voor Automation is de waarde taak. |
| Resource | De naam van het Automation-account |
| SourceSystem | Systeem dat Azure Monitor logboeken gebruikt om de gegevens te verzamelen. De waarde is altijd Azure voor Azure Diagnostics. |
| ResultDescription |De resulterende status van de runbook-taak. Mogelijke waarden zijn:<br>- Taak is gestart<br>- Taak is mislukt<br>- Taak is voltooid |
| CorrelationId |De correlatie-GUID van de runbook-taak. |
| ResourceId |De resource-ID van het Azure Automation-account van het runbook. |
| SubscriptionId | De GUID van het Azure-abonnement voor het Automation-account. |
| ResourceGroup | De naam van de resource groep voor het Automation-account. |
| ResourceProvider | De resource provider. De waarde is micro soft. Automat. |
| ResourceType | Het resource type. De waarde is AUTOMATIONACCOUNTS. |

### <a name="job-streams"></a>Taakstromen
| Eigenschap | Beschrijving |
| --- | --- |
| TimeGenerated |Datum en tijd van uitvoering van de runbooktaak. |
| RunbookName_s |De naam van het runbook. |
| Caller_s |De aanroeper die de bewerking heeft gestart. Mogelijke waarden zijn een e-mailadres of het systeem voor geplande taken. |
| StreamType_s |Het type taakstroom. Mogelijke waarden zijn:<br>- Voortgang<br>- Uitvoer<br>- Waarschuwing<br>- Fout<br>- Foutopsporing<br>- Uitgebreid |
| Tenant_g | GUID waarmee de Tenant voor de oproepende functie wordt geïdentificeerd. |
| JobId_g |GUID waarmee de runbook-taak wordt geïdentificeerd. |
| ResultType |De status van de runbooktaak. Mogelijke waarden zijn:<br>-Wordt uitgevoerd |
| Categorie | Classificatie van het type gegevens. Voor Automation is de waarde JobStreams. |
| OperationName | Het type bewerking dat in azure wordt uitgevoerd. Voor Automation is de waarde taak. |
| Resource | De naam van het Automation-account. |
| SourceSystem | Systeem dat Azure Monitor logboeken gebruikt om de gegevens te verzamelen. De waarde is altijd Azure voor Azure Diagnostics. |
| ResultDescription |Beschrijving die de uitvoer stroom van het runbook bevat. |
| CorrelationId |De correlatie-GUID van de runbook-taak. |
| ResourceId |De resource-ID van het Azure Automation-account van het runbook. |
| SubscriptionId | De GUID van het Azure-abonnement voor het Automation-account. |
| ResourceGroup | De naam van de resource groep voor het Automation-account. |
| ResourceProvider | De resource provider. De waarde is micro soft. Automat. |
| ResourceType | Het resource type. De waarde is AUTOMATIONACCOUNTS. |

## <a name="set-up-integration-with-azure-monitor-logs"></a>Integratie met Azure Monitor-logboeken instellen

1. Start Windows Power shell op uw computer vanuit het **Start** scherm.
2. Voer de volgende Power shell-opdrachten uit en bewerk de waarden voor `$automationAccountId` en `$workspaceId` met de waarden uit de voor gaande sectie.

   ```powershell-interactive
   $workspaceId = "resource ID of the log analytics workspace"
   $automationAccountId = "resource ID of your Automation account"

   Set-AzDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled 1
   ```

Na het uitvoeren van dit script kan het een uur duren voordat u begint met het weer geven van records in Azure Monitor logboeken van nieuw `JobLogs` of `JobStreams` wordt geschreven.

Als u de logboeken wilt weer geven, voert u de volgende query uit in log Analytics zoeken in Logboeken:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="verify-configuration"></a>Configuratie controleren

Als u wilt controleren of uw Automation-account logboeken naar uw Log Analytics-werk ruimte verzendt, controleert u of de diagnostische gegevens correct zijn geconfigureerd voor het Automation-account met behulp van de volgende Power shell-opdracht.

```powershell-interactive
Get-AzDiagnosticSetting -ResourceId $automationAccountId
```

Controleer in de uitvoer:

* Onder `Logs` is de waarde voor ingesteld op `Enabled` ' True '.
* `WorkspaceId`is ingesteld op de `ResourceId` waarde voor uw log Analytics-werk ruimte.

## <a name="view-automation-logs-in-azure-monitor-logs"></a>Automatiserings logboeken weer geven in Azure Monitor-logboeken

Nu u bent begonnen met het verzenden van uw Automation-taak logboeken naar Azure Monitor-logboeken, ziet u wat u met deze logboeken kunt doen in Azure Monitor-Logboeken.

Voer de volgende query uit om de logboeken weer te geven:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>Een e-mail verzenden wanneer een runbook-taak is mislukt of onderbroken

De volgende stappen laten zien hoe u waarschuwingen instelt in Azure Monitor om u te waarschuwen wanneer er iets mis gaat met een runbook-taak.

Als u een waarschuwings regel wilt maken, moet u beginnen met het maken van een zoek opdracht in het logboek voor de runbook-taak records die de waarschuwing moeten aanroepen. Klik op de knop **waarschuwing** om de waarschuwings regel te maken en te configureren.

1. Klik op de pagina overzicht van Log Analytics werk ruimte op **Logboeken weer geven**.

2. Maak een zoek opdracht in het logboek voor uw waarschuwing door de volgende zoek opdracht in het query veld in te voeren:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended")`<br><br>U kunt ook groeperen op de naam van het runbook met behulp van:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended") | summarize AggregatedValue = count() by RunbookName_s`

   Als u logboeken van meer dan één Automation-account of-abonnement op uw werk ruimte instelt, kunt u uw waarschuwingen groeperen op abonnement en Automation-account. De naam van het Automation-account vindt u in het `Resource` veld in de zoek actie van `JobLogs` .

3. Als u het scherm **regel maken** wilt openen, klikt u op **nieuwe waarschuwings regel** boven aan de pagina. Zie [logboek waarschuwingen in azure](../azure-monitor/platform/alerts-unified-log.md)voor meer informatie over de opties voor het configureren van de waarschuwing.

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Alle taken zoeken die zijn voltooid met fouten

Naast het melden van fouten, kunt u nagaan wanneer een runbook-taak een niet-afsluit fout heeft. In dergelijke gevallen produceert Power shell een fout stroom, maar de niet-afsluit fouten zorgen ervoor dat uw taak niet wordt onderbroken of mislukt.

1. Klik in de werk ruimte Log Analytics op **Logboeken**.

2. Typ in het veld query `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and StreamType_s == "Error" | summarize AggregatedValue = count() by JobId_g` .

3. Klik op de knop **zoeken** .

### <a name="view-job-streams-for-a-job"></a>Taak stromen voor een taak weer geven

Wanneer u fouten opspoort in een taak, wilt u mogelijk ook de taak stromen bekijken. Met de volgende query worden alle stromen weer gegeven voor één taak met GUID 2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0:

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and JobId_g == "2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort by TimeGenerated asc | project ResultDescription`

### <a name="view-historical-job-status"></a>Historische taak status weer geven

Ten slotte kunt u uw taak geschiedenis gedurende een periode visualiseren. U kunt deze query gebruiken om de status van uw taken in de loop van de tijd te zoeken.

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and ResultType != "started" | summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h)`
<br> ![Status diagram van Log Analytics historische taak](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="remove-diagnostic-settings"></a>Diagnostische instellingen verwijderen

Als u de diagnostische instelling van het Automation-account wilt verwijderen, voert u de volgende opdracht uit:

```powershell-interactive
$automationAccountId = "[resource ID of your Automation account]"

Remove-AzDiagnosticSetting -ResourceId $automationAccountId
```

## <a name="next-steps"></a>Volgende stappen

* Zie [Zoek opdrachten in Logboeken in azure monitor logboeken](../log-analytics/log-analytics-log-searches.md)voor meer informatie over het maken van zoek query's en het controleren van de Automation-taak logboeken met Azure monitor-Logboeken.
* Zie [runbook-uitvoer bewaken](automation-runbook-output-and-messages.md)voor meer informatie over het maken en ophalen van uitvoer-en fout berichten van runbooks.
* Voor meer informatie over het uitvoeren van een runbook, het controleren van runbook-taken en andere technische details raadpleegt u [runbook execution in azure Automation](automation-runbook-execution.md).
* Zie [Azure Storage-gegevens verzamelen in azure monitor logs Overview](../azure-monitor/platform/collect-azure-metrics-logs.md)voor meer informatie over Azure monitor logboeken en bronnen voor gegevens verzameling.
* Zie voor hulp bij het oplossen van problemen Log Analytics [problemen oplossen waarom log Analytics niet langer gegevens verzamelt](../azure-monitor/platform/manage-cost-storage.md#troubleshooting-why-log-analytics-is-no-longer-collecting-data).