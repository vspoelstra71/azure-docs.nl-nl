---
title: Schema's in Azure Automation beheren
description: In dit artikel leest u hoe u een planning maakt en gebruikt in Azure Automation.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 04/04/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 557dc3ad48f8f21d8898e2beb5d940d66058e90c
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2020
ms.locfileid: "83744982"
---
# <a name="manage-schedules-in-azure-automation"></a>Schema's in Azure Automation beheren

Als u een runbook in Azure Automation wilt plannen om op een opgegeven tijdstip te starten, koppelt u dit aan een of meer planningen. Een planning kan worden geconfigureerd om één keer te worden uitgevoerd of op een terugkerende planning voor runbooks in de Azure Portal. U kunt ze ook plannen voor wekelijkse, maandelijkse, specifieke dagen van de week of dagen van de maand of een bepaalde dag van de maand. Een runbook kan worden gekoppeld aan meerdere planningen en er kunnen meerdere runbooks aan een planning worden gekoppeld.

> [!NOTE]
> Azure Automation ondersteunt zomer tijd en plant het op de juiste wijze voor automatiserings bewerkingen.

> [!NOTE]
> Schema's die momenteel niet zijn ingeschakeld voor Azure Automation DSC-configuraties.

## <a name="powershell-cmdlets-used-to-access-schedules"></a>Power shell-cmdlets die worden gebruikt voor toegang tot schema's

Met de cmdlets in de volgende tabel worden Automation-schema's gemaakt en beheerd met Power shell. Ze worden geleverd als onderdeel van de [AZ-modules](modules.md#az-modules). 

| Cmdlets | Beschrijving |
|:--- |:--- |
| [Get-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationSchedule?view=azps-3.7.0) |Hiermee wordt een planning opgehaald. |
| [Get-AzAutomationScheduledRunbook](https://docs.microsoft.com/powershell/module/az.automation/get-azautomationscheduledrunbook?view=azps-3.7.0) |Hiermee worden geplande runbooks opgehaald. |
| [New-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationSchedule?view=azps-3.7.0) |Hiermee maakt u een nieuw schema. |
| [REGI ster-AzAutomationScheduledRunbook](https://docs.microsoft.com/powershell/module/Az.Automation/Register-AzAutomationScheduledRunbook?view=azps-3.7.0) |Hiermee wordt een runbook gekoppeld aan een schema. |
| [Remove-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/Remove-AzAutomationSchedule?view=azps-3.7.0) |Hiermee verwijdert u een schema. |
| [Set-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/Set-AzAutomationSchedule?view=azps-3.7.0) |Hiermee stelt u de eigenschappen voor een bestaande planning. |
| [Registratie ongedaan maken-AzAutomationScheduledRunbook](https://docs.microsoft.com/powershell/module/Az.Automation/Unregister-AzAutomationScheduledRunbook?view=azps-3.7.0) |Hiermee wordt een runbook ontkoppeld van een schema. |

## <a name="create-a-schedule"></a>Een planning maken

U kunt een nieuw schema maken voor uw runbooks in de Azure Portal of met Power shell. Als u wilt voor komen dat uw runbooks en de processen worden geautomatiseerd, moet u eerst runbooks met gekoppelde planningen testen met een Automation-account dat is toegewezen voor testen. Een test valideert dat uw geplande runbooks correct blijven werken. Als er een probleem wordt weer geven, kunt u problemen oplossen en alle vereiste wijzigingen Toep assen voordat u de bijgewerkte runbook-versie naar productie migreert.

> [!NOTE]
> Uw Automation-account haalt niet automatisch nieuwe versies van modules op, tenzij u deze hand matig hebt bijgewerkt door de optie [Azure-modules bijwerken](../automation-update-azure-modules.md) uit **modules**te selecteren. Azure Automation gebruikt de nieuwste modules in uw Automation-account wanneer een nieuwe geplande taak wordt uitgevoerd. 

### <a name="create-a-new-schedule-in-the-azure-portal"></a>Een nieuw schema maken in de Azure Portal

1. Selecteer in de Azure Portal, vanuit uw Automation-account, de optie **schema's** onder de sectie **gedeelde resources** aan de linkerkant.
1. Selecteer boven aan de pagina **een planning toevoegen** .
1. Voer in het deel venster **Nieuw schema** een naam in en Voer desgewenst een beschrijving in voor de nieuwe planning.
1. Selecteer of de planning één keer of volgens een terugkerende planning wordt uitgevoerd door een **keer** of **terugkerend**te selecteren. Als u **eenmaal**selecteert, geeft u een begin tijd op en selecteert u **maken**. Als u **terugkerend**selecteert, geeft u een begin tijd op. Voor **herhalen elke**, selecteert u hoe vaak het runbook moet worden herhaald. Selecteren per uur, dag, week of maand.
    1. Als u **week**selecteert, worden de dagen van de week weer gegeven waaruit u kunt kiezen. Selecteer zoveel dagen als u wilt. De eerste uitvoering van uw planning gebeurt op de eerste dag die na de begin tijd is geselecteerd. Als u bijvoorbeeld een weekend planning wilt kiezen, selecteert u zaterdag en zondag.
    
       ![Terugkerende weekend planning instellen](../media/schedules/week-end-weekly-recurrence.png)

    2. Als u **maand**selecteert, krijgt u verschillende opties. Voor de optie voor **maandelijkse exemplaren** selecteert u **maand dagen** of **week dagen**. Als u **maand dagen**selecteert, wordt er een kalender weer gegeven zodat u zoveel dagen kunt kiezen als u wilt. Als u een datum kiest zoals de 31e die niet voor komt in de huidige maand, wordt het schema niet uitgevoerd. Als u wilt dat het schema op de laatste dag wordt uitgevoerd, selecteert u **Ja** onder **uitvoeren op de laatste dag van de maand**. Als u **week dagen**selecteert, wordt **elke optie herhalen** weer gegeven. Kies **eerste**, **tweede**, **derde**, **vierde**of **laatste**. Kies ten slotte een dag om te herhalen.

       ![Maandelijks plannen op de eerste, vijftiende en laatste dag van de maand](../media/schedules/monthly-first-fifteenth-last.png)

1. Wanneer u klaar bent, selecteert u **maken**.

### <a name="create-a-new-schedule-with-powershell"></a>Een nieuw schema maken met Power shell

Gebruik de cmdlet [New-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationSchedule?view=azps-3.7.0) om planningen te maken. U geeft de begin tijd voor de planning op en de frequentie die moet worden uitgevoerd. In de volgende voor beelden ziet u hoe u veel verschillende plannings scenario's maakt.

#### <a name="create-a-one-time-schedule"></a>Een eenmalige planning maken

In het volgende voor beeld wordt een eenmalig schema gemaakt.

```azurepowershell-interactive
$TimeZone = ([System.TimeZoneInfo]::Local).Id
New-AzAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule01" -StartTime "23:00" -OneTime -ResourceGroupName "ResourceGroup01" -TimeZone $TimeZone
```

#### <a name="create-a-recurring-schedule"></a>Een terugkerend schema maken

In het volgende voor beeld ziet u hoe u een terugkerend schema maakt dat elke dag om 1:00 uur wordt uitgevoerd voor een jaar.

```azurepowershell-interactive
$StartTime = Get-Date "13:00:00"
$EndTime = $StartTime.AddYears(1)
New-AzAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule02" -StartTime $StartTime -ExpiryTime $EndTime -DayInterval 1 -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-weekly-recurring-schedule"></a>Een wekelijks terugkerende planning maken

In het volgende voor beeld ziet u hoe u een wekelijks schema maakt dat alleen op week dagen wordt uitgevoerd.

```azurepowershell-interactive
$StartTime = (Get-Date "13:00:00").AddDays(1)
[System.DayOfWeek[]]$WeekDays = @([System.DayOfWeek]::Monday..[System.DayOfWeek]::Friday)
New-AzAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Schedule03" -StartTime $StartTime -WeekInterval 1 -DaysOfWeek $WeekDays -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-weekly-recurring-schedule-for-weekends"></a>Een wekelijks terugkerende planning voor weekends maken

In het volgende voor beeld ziet u hoe u een wekelijks schema maakt dat alleen in weekends wordt uitgevoerd.

```azurepowershell-interactive
$StartTime = (Get-Date "18:00:00").AddDays(1)
[System.DayOfWeek[]]$WeekendDays = @([System.DayOfWeek]::Saturday,[System.DayOfWeek]::Sunday)
New-AzAutomationSchedule -AutomationAccountName "ContosoAutomation" -Name "Weekends 6PM" -StartTime $StartTime -WeekInterval 1 -DaysOfWeek $WeekendDays -ResourceGroupName "ResourceGroup01"
```

#### <a name="create-a-recurring-schedule-for-the-first-fifteenth-and-last-days-of-the-month"></a>Een terugkerende planning maken voor de eerste, vijftiende en laatste dag van de maand

In het volgende voor beeld ziet u hoe u een terugkerend schema maakt dat wordt uitgevoerd op de eerste, vijftiende en laatste dag van een maand.

```azurepowershell-interactive
$StartTime = (Get-Date "18:00:00").AddDays(1)
New-AzAutomationSchedule -AutomationAccountName "TestAzureAuto" -Name "1st, 15th and Last" -StartTime $StartTime -DaysOfMonth @("One", "Fifteenth", "Last") -ResourceGroupName "TestAzureAuto" -MonthInterval 1
```

## <a name="link-a-schedule-to-a-runbook"></a>Een planning aan een runbook koppelen

Een runbook kan worden gekoppeld aan meerdere planningen en er kunnen meerdere runbooks aan een planning worden gekoppeld. Als een runbook para meters heeft, kunt u waarden voor deze opgeven. U moet waarden opgeven voor verplichte para meters, en u kunt ook waarden opgeven voor optionele para meters. Deze waarden worden gebruikt wanneer het runbook wordt gestart door dit schema. U kunt hetzelfde runbook aan een ander schema koppelen en verschillende parameter waarden opgeven.

### <a name="link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Een planning aan een runbook koppelen met de Azure Portal

1. Selecteer in het Azure Portal van uw Automation-account **Runbooks** onder **proces automatisering**.
1. Selecteer de naam van het runbook dat u wilt plannen.
1. Als het runbook momenteel niet is gekoppeld aan een schema, krijgt u de optie om een nieuw schema te maken of een koppeling naar een bestaand schema te koppelen.
1. Als het runbook para meters heeft, kunt u de optie **instellingen voor uitvoeren wijzigen selecteren (standaard: Azure)** en het deel venster **para meters** wordt weer gegeven. U kunt hier parameter gegevens invoeren.

### <a name="link-a-schedule-to-a-runbook-with-powershell"></a>Een planning aan een runbook koppelen met Power shell

Gebruik de cmdlet [REGI ster-AzAutomationScheduledRunbook](https://docs.microsoft.com/powershell/module/Az.Automation/Register-AzAutomationScheduledRunbook?view=azps-3.7.0) om een schema te koppelen. U kunt waarden voor de parameters van het runbook opgeven met de parameter Parameters . Zie [starten van een Runbook in azure Automation](../automation-starting-a-runbook.md)voor meer informatie over het opgeven van parameter waarden.
In het volgende voor beeld ziet u hoe u een schema aan een runbook koppelt met behulp van een Azure Resource Manager-cmdlet met para meters.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$runbookName = "Test-Runbook"
$scheduleName = "Sample-DailySchedule"
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Register-AzAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
–Name $runbookName –ScheduleName $scheduleName –Parameters $params `
-ResourceGroupName "ResourceGroup01"
```

## <a name="schedule-runbooks-to-run-more-frequently"></a>Plannen van runbooks om vaker te worden uitgevoerd

Het meest frequente interval waarvoor een planning in Azure Automation kan worden geconfigureerd, is een uur. Als u wilt dat schema's vaker worden uitgevoerd, zijn er twee opties:

* Maak een [webhook](../automation-webhooks.md) voor het runbook en gebruik [Azure Logic apps](../../logic-apps/logic-apps-overview.md) om de webhook aan te roepen. Azure Logic Apps biedt meer verfijnde granulariteit om een schema te definiëren.

* Maak vier schema's die allemaal binnen vijf tien minuten van elkaar beginnen en voer elk uur één keer uit. Met dit scenario kan het runbook om de 15 minuten worden uitgevoerd met de verschillende schema's.

## <a name="disable-a-schedule"></a>Een planning uitschakelen

Wanneer u een planning uitschakelt, wordt elk runbook dat is gekoppeld aan het schema niet meer op dat schema uitgevoerd. U kunt een planning hand matig uitschakelen of een verloop tijd instellen voor schema's met een frequentie wanneer u deze maakt. Wanneer de verloop tijd is bereikt, wordt het schema uitgeschakeld.

### <a name="disable-a-schedule-from-the-azure-portal"></a>Een planning uit de Azure Portal uitschakelen

1. Selecteer in uw Automation-account **schema's** onder **gedeelde resources**.
1. Selecteer de naam van een planning om het deel venster Details te openen.
1. Wijzig **ingeschakeld** in **Nee**.

> [!NOTE]
> Als u een planning wilt uitschakelen die een begin tijd in het verleden heeft, moet u de begin datum wijzigen in een tijd in de toekomst voordat u deze opslaat.

### <a name="disable-a-schedule-with-powershell"></a>Een planning uitschakelen met Power shell

Gebruik de cmdlet [set-AzAutomationSchedule](https://docs.microsoft.com/powershell/module/Az.Automation/Set-AzAutomationSchedule?view=azps-3.7.0) om de eigenschappen van een bestaande planning te wijzigen. Als u het schema wilt uitschakelen, geeft u False op voor de `IsEnabled` para meter.

In het volgende voor beeld ziet u hoe u een planning voor een runbook kunt uitschakelen met behulp van een Azure Resource Manager-cmdlet.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
Set-AzAutomationSchedule –AutomationAccountName $automationAccountName `
–Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"
```

## <a name="remove-a-schedule"></a>Een planning verwijderen

Wanneer u klaar bent om uw planningen te verwijderen, kunt u de Azure Portal of Power shell gebruiken. Houd er rekening mee dat u alleen een schema kunt verwijderen dat is uitgeschakeld zoals beschreven in de vorige sectie.

### <a name="remove-a-schedule-using-the-azure-portal"></a>Een schema verwijderen met de Azure Portal

1. Selecteer in uw Automation-account **schema's** onder **gedeelde resources**.
2. Klik op de naam van een planning om het deel venster Details te openen.
3. Klik op **verwijderen**.

### <a name="remove-a-schedule-with-powershell"></a>Een schema verwijderen met Power shell

U kunt de `Remove-AzAutomationSchedule` cmdlet gebruiken zoals hieronder wordt weer gegeven om een bestaand schema te verwijderen. 

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
Remove-AzAutomationSchedule -AutomationAccountName $automationAccountName `
-Name $scheduleName -ResourceGroupName "ResourceGroup01"
```

## <a name="next-steps"></a>Volgende stappen

* Zie [modules beheren in azure Automation](modules.md)voor meer informatie over de cmdlets die worden gebruikt voor het openen van schema's.
* Zie voor algemene informatie over runbooks [Runbook-uitvoering in azure Automation](../automation-runbook-execution.md).