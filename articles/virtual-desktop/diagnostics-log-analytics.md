---
title: Logboek analyse van diagnostische gegevens van Windows Virtual Desktop-Azure
description: Log Analytics gebruiken met de Windows-functie diagnostische gegevens over virtueel bureau blad.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 05/27/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: bd28117350913bc25f5bf7cec08d28683ad9daca
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2020
ms.locfileid: "84020061"
---
# <a name="use-log-analytics-for-the-diagnostics-feature"></a>Log Analytics gebruiken voor de functie voor diagnostische gegevens

>[!IMPORTANT]
>Deze inhoud is van toepassing op de lente 2020-update met Azure Resource Manager virtueel-bureaublad objecten van Windows. Raadpleeg [dit artikel](./virtual-desktop-fall-2019/diagnostics-log-analytics-2019.md)als u de versie van het Windows-bureau blad van Virtual Desktop 2019 zonder Azure Resource Manager objecten gebruikt.
>
> De Windows Virtual Desktop lente 2020-update is momenteel beschikbaar als open bare preview. Deze preview-versie is beschikbaar zonder service level agreement. het wordt niet aangeraden deze te gebruiken voor productie werkbelastingen. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. 
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Het virtuele bureau blad van Windows maakt gebruik van [Azure monitor](../azure-monitor/overview.md) voor bewaking en waarschuwingen, zoals veel andere Azure-Services. Hiermee kunnen beheerders problemen identificeren via één interface. De service maakt activiteiten logboeken voor zowel gebruikers-als beheer acties. Elk activiteiten logboek valt onder de volgende categorieën:  

- Beheer activiteiten:
    - Controleren of er wordt geprobeerd Windows virtueel-bureaublad objecten te wijzigen met behulp van Api's of Power shell. Kan iemand bijvoorbeeld een hostgroep maken met behulp van Power shell?
- Voerder 
    - Kunnen gebruikers zich abonneren op werk ruimten? 
    - Zien gebruikers alle resources die zijn gepubliceerd in de Extern bureaublad-client?
- Verbindingen: 
    - Wanneer gebruikers verbindingen met de service initiëren en volt ooien. 
- Registratie van de host: 
    - Is de sessiehost geregistreerd bij de service bij het verbinden?
- Bufferoverschrijdingsfouten 
    - Ondervinden gebruikers problemen met specifieke activiteiten? Met deze functie kan een tabel worden gegenereerd die de activiteit gegevens voor u registreert, zolang de gegevens worden gekoppeld aan de activiteiten.
- Controle punten  
    - Specifieke stappen in de levens duur van een activiteit die is bereikt. Tijdens een sessie is een gebruiker bijvoorbeeld gelijkmatig verdeeld over een bepaalde host. vervolgens is de gebruiker aangemeld tijdens een verbinding, enzovoort.

Verbindingen die zich niet in het virtuele bureau blad van Windows bevinden, worden niet weer gegeven in de resultaten van diagnostische gegevens omdat de functie Service diagnostiek zelf deel uitmaakt van Windows virtueel bureau blad. Er kunnen problemen met de Windows-verbinding met virtueel bureau blad optreden wanneer de gebruiker problemen met de netwerk verbinding ondervindt.

Met Azure Monitor kunt u gegevens van Windows virtueel bureau blad analyseren en de prestatie meter items van virtuele machines (VM) controleren, allemaal binnen hetzelfde hulp programma. In dit artikel vindt u meer informatie over het inschakelen van diagnostische gegevens voor uw virtuele Windows-desktop omgeving. 

>[!NOTE] 
>Zie [Azure virtual machines bewaken met Azure monitor](../azure-monitor/insights/monitor-vm-azure.md)voor meer informatie over het bewaken van uw Vm's in Azure. Zorg er ook voor dat u [de drempel waarden voor prestatie meter items bekijkt](../virtual-desktop/virtual-desktop-fall-2019/deploy-diagnostics.md#windows-performance-counter-thresholds) voor een beter inzicht in uw gebruikers ervaring op de sessiehost.

## <a name="before-you-get-started"></a>Voordat u aan de slag gaat

Voordat u Log Analytics kunt gebruiken, moet u een werk ruimte maken. Volg hiervoor de instructies in een van de volgende twee artikelen:

- Als u liever Azure Portal gebruikt, raadpleegt u [een log Analytics-werk ruimte maken in azure Portal](../azure-monitor/learn/quick-create-workspace.md).
- Zie [een log Analytics-werk ruimte maken met Power shell als u de voor](../azure-monitor/learn/quick-create-workspace-posh.md)keur geeft aan Power shell.

Nadat u uw werk ruimte hebt gemaakt, volgt u de instructies in [Windows-computers verbinden met Azure monitor](../azure-monitor/platform/agent-windows.md#obtain-workspace-id-and-key) om de volgende informatie te verkrijgen:

- De werk ruimte-ID
- De primaire sleutel van uw werk ruimte

U hebt deze gegevens later nodig in het installatie proces.

Zorg ervoor dat u het machtigings beheer voor Azure Monitor controleert om gegevens toegang in te scha kelen voor degenen die uw virtuele Windows-desktop omgeving controleren en onderhouden. Zie [aan de slag met rollen, machtigingen en beveiliging met Azure monitor](../azure-monitor/platform/roles-permissions-security.md)voor meer informatie. 

## <a name="push-diagnostics-data-to-your-workspace"></a>Diagnostische gegevens naar uw werk ruimte pushen

U kunt Diagnostische gegevens van uw virtuele bureau blad-objecten van Windows naar de Log Analytics voor uw werk ruimte pushen. U kunt deze functie meteen instellen wanneer u uw objecten voor het eerst maakt.

Log Analytics instellen voor een nieuw object:

1. Meld u aan bij de Azure Portal en ga naar het **virtuele bureau blad van Windows**. 

2. Navigeer naar het object (zoals een hostgroep, app-groep of werk ruimte) waarvoor u Logboeken en gebeurtenissen wilt vastleggen. 

3. Selecteer **Diagnostische instellingen** in het menu aan de linkerkant van het scherm. 

4. Selecteer **Diagnostische instelling toevoegen** in het menu dat wordt weer gegeven aan de rechter kant van het scherm. 
   
    De opties die worden weer gegeven op de pagina Diagnostische instellingen variëren, afhankelijk van het soort object dat u bewerkt.

    Wanneer u bijvoorbeeld diagnostische gegevens inschakelt voor een app-groep, ziet u opties voor het configureren van controle punten, fouten en beheer. Voor werk ruimten configureren deze categorieën een feed om te volgen wanneer gebruikers zich abonneren op de lijst met apps. Zie voor meer informatie over diagnostische instellingen [Diagnostische instelling maken om bron logboeken en metrische gegevens in azure te verzamelen](../azure-monitor/platform/diagnostic-settings.md). 

     >[!IMPORTANT] 
     >Vergeet niet om diagnostische gegevens in te scha kelen voor elk Azure Resource Manager object dat u wilt bewaken. Er zijn gegevens beschikbaar voor activiteiten nadat de diagnose is ingeschakeld. Het kan enkele uren duren voordat de eerste keer is ingesteld.  

5. Voer een naam in voor de configuratie van uw instellingen en selecteer vervolgens **verzenden naar log Analytics**. De naam die u gebruikt, mag geen spaties bevatten en moet voldoen aan de [naamgevings conventies van Azure](../azure-resource-manager/management/resource-name-rules.md). Als onderdeel van de logboeken kunt u alle opties selecteren die u wilt toevoegen aan uw Log Analytics, zoals controle punt, fout, beheer, enzovoort.

6. Selecteer **Opslaan**.

>[!NOTE]
>Log Analytics biedt u de mogelijkheid om gegevens te streamen naar [Event hubs](../event-hubs/event-hubs-about.md) of deze te archiveren in een opslag account. Zie [Azure-bewakings gegevens streamen naar een event hub](../azure-monitor/platform/stream-monitoring-data-event-hubs.md) en [Azure-resource logboeken archiveren in een opslag account](../azure-monitor/platform/resource-logs-collect-storage.md)voor meer informatie over deze functie. 

## <a name="how-to-access-log-analytics"></a>Toegang tot Log Analytics

U hebt toegang tot Log Analytics-werk ruimten op het Azure Portal of Azure Monitor.

### <a name="access-log-analytics-on-a-log-analytics-workspace"></a>Toegang tot Log Analytics op een Log Analytics werk ruimte

1. Meld u aan bij Azure Portal.

2. Zoeken naar **log Analytics-werk ruimte**. 

3. Onder Services selecteert u **log Analytics werk ruimten**. 
   
4. Selecteer in de lijst de werk ruimte die u hebt geconfigureerd voor het virtueel-bureaublad object van Windows.

5. Selecteer **Logboeken**in uw werk ruimte. U kunt de menu lijst filteren met de functie **zoeken** . 

### <a name="access-log-analytics-on-azure-monitor"></a>Toegang tot Log Analytics op Azure Monitor

1. Aanmelden bij Azure Portal

2. Zoek en selecteer **monitor**. 

3. Selecteer **Logboeken**.

4. Volg de instructies op de pagina logboek registratie om het bereik van uw query in te stellen.  

5. U kunt een query uitvoeren op diagnostische gegevens. Alle diagnostische tabellen hebben het voor voegsel ' WVD '.

## <a name="cadence-for-sending-diagnostic-events"></a>Uitgebracht voor het verzenden van diagnostische gebeurtenissen

Diagnostische gebeurtenissen worden verzonden naar Log Analytics wanneer deze zijn voltooid.

Log Analytics alleen rapporten in deze tussenliggende status voor verbindings activiteiten:

- Gestart: wanneer een gebruiker een app of bureau blad in de Extern bureaublad-client selecteert en hiermee verbinding maakt.
- Verbonden: wanneer de gebruiker verbinding maakt met de virtuele machine waarop de app of het bureau blad wordt gehost.
- Voltooid: wanneer de gebruiker of de server de sessie verbreekt, wordt de activiteit uitgevoerd in.

## <a name="example-queries"></a>Voorbeelden van query's

De volgende voorbeeld query's laten zien hoe de diagnostische functie een rapport genereert voor de meest voorkomende activiteiten in uw systeem.

Voer de volgende cmdlet uit om een lijst op te halen met de verbindingen die uw gebruikers hebben gemaakt:

```kusto
WVDConnections 
| project-away TenantId,SourceSystem 
| summarize arg_max(TimeGenerated, *), StartTime =  min(iff(State== 'Started', TimeGenerated , datetime(null) )), ConnectTime = min(iff(State== 'Connected', TimeGenerated , datetime(null) ))   by CorrelationId 
| join kind=leftouter ( 
    WVDErrors 
    |summarize Errors=makelist(pack('Code', Code, 'CodeSymbolic', CodeSymbolic, 'Time', TimeGenerated, 'Message', Message ,'ServiceError', ServiceError, 'Source', Source)) by CorrelationId 
    ) on CorrelationId     
| join kind=leftouter ( 
   WVDCheckpoints 
   | summarize Checkpoints=makelist(pack('Time', TimeGenerated, 'Name', Name, 'Parameters', Parameters, 'Source', Source)) by CorrelationId 
   | mv-apply Checkpoints on 
    ( 
        order by todatetime(Checkpoints['Time']) asc 
        | summarize Checkpoints=makelist(Checkpoints) 
    ) 
   ) on CorrelationId 
| project-away CorrelationId1, CorrelationId2 
| order by  TimeGenerated desc 
```

De feed-activiteit van uw gebruikers weer geven:

```kusto
WVDFeeds  
| project-away TenantId,SourceSystem  
| join kind=leftouter (  
    WVDErrors  
    |summarize Errors=makelist(pack('Code', Code, 'CodeSymbolic', CodeSymbolic, 'Time', TimeGenerated, 'Message', Message ,'ServiceError', ServiceError, 'Source', Source)) by CorrelationId  
    ) on CorrelationId      
| join kind=leftouter (  
   WVDCheckpoints  
   | summarize Checkpoints=makelist(pack('Time', TimeGenerated, 'Name', Name, 'Parameters', Parameters, 'Source', Source)) by CorrelationId  
   | mv-apply Checkpoints on  
    (  
        order by todatetime(Checkpoints['Time']) asc  
        | summarize Checkpoints=makelist(Checkpoints)  
    )  
   ) on CorrelationId  
| project-away CorrelationId1, CorrelationId2  
| order by  TimeGenerated desc 
```

Alle verbindingen voor één gebruiker zoeken: 

```kusto
WVDConnections
|where UserName == "userupn" 
|take 100 
|sort by TimeGenerated asc, CorrelationId 
```
 

Zoeken naar het aantal keren dat een gebruiker is verbonden per dag:

```kusto
WVDConnections 
|where UserName == "userupn" 
|take 100 
|sort by TimeGenerated asc, CorrelationId 
|summarize dcount(CorrelationId) by bin(TimeGenerated, 1d) 
```
 

Sessie duur zoeken op gebruiker:

```kusto
let Events = WVDConnections | where UserName == "userupn" ; 
Events 
| where State == "Connected" 
| project CorrelationId , UserName, ResourceAlias , StartTime=TimeGenerated 
| join (Events 
| where State == "Completed" 
| project EndTime=TimeGenerated, CorrelationId) 
on CorrelationId 
| project Duration = EndTime - StartTime, ResourceAlias 
| sort by Duration asc 
```

Fouten voor een specifieke gebruiker zoeken:

```kusto
WVDErrors
| where UserName == "userupn" 
|take 100
```

Nagaan of er een specifieke fout is opgetreden:

```kusto
WVDErrors 
| where CodeSymbolic =="ErrorSymbolicCode" 
| summarize count(UserName) by CodeSymbolic 
```

Zoeken naar een fout voor alle gebruikers:

```kusto
WVDErrors 
| where ServiceError =="false" 
| summarize usercount = count(UserName) by CodeSymbolic 
| sort by usercount desc
| render barchart 
```

>[!NOTE]
>De belangrijkste tabel voor het oplossen van problemen is WVDErrors. Gebruik deze query om te begrijpen welke problemen optreden voor gebruikers activiteiten als verbindingen of feeds wanneer een gebruiker zich abonneert op de lijst met apps of Bureau bladen. In de tabel worden beheer fouten en registratie problemen van hosts weer gegeven.
>
>Als u tijdens de open bare preview hulp nodig hebt bij het oplossen van een probleem, zorg er dan voor dat u de CorrelationID voor de fout in uw Help-verzoek geeft. Zorg er ook voor dat de waarde van de service fout altijd ServiceError = "false" aangeeft. Een ' onwaar ' betekent dat het probleem kan worden opgelost door een beheer taak aan het eind. Als ServiceError = "True", moet u het probleem escaleren naar micro soft.

## <a name="next-steps"></a>Volgende stappen 

Zie [problemen identificeren en diagnosticeren](diagnostics-role-service.md#common-error-scenarios)als u veelvoorkomende fout scenario's wilt bekijken die door de diagnostische functie kunnen worden geïdentificeerd.
