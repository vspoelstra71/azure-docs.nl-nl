---
title: Overzicht van Azure Automation status configuratie
description: Dit artikel bevat een overzicht van Azure Automation status configuratie.
keywords: Power shell DSC, desired state Configuration, Power shell DSC Azure
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.date: 11/06/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9880915061c0639aebe30bdb33258d7c79e155d7
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/25/2020
ms.locfileid: "83836886"
---
# <a name="azure-automation-state-configuration-overview"></a>Overzicht van Azure Automation status configuratie

Azure Automation status configuratie is een Azure Configuration Management-service waarmee u Power shell desired state Configuration (DSC)- [configuraties](/powershell/scripting/dsc/configurations/configurations) kunt schrijven, beheren en compileren voor knoop punten in elke Cloud of on-premises Data Center. De service importeert ook [DSC-resources](/powershell/scripting/dsc/resources/resources)en wijst configuraties toe aan doel knooppunten, allemaal in de Cloud. U kunt de configuratie van Azure Automation status in de Azure Portal openen door **State Configuration (DSC)** te selecteren onder **configuratie beheer**. 

U kunt Azure Automation status configuratie gebruiken voor het beheren van verschillende computers:

- Virtuele machines van Azure
- Azure virtual machines (klassiek)
- Fysieke/virtuele Windows-machines on-premises of in een andere Cloud dan Azure (met inbegrip van AWS EC2-instanties)
- Fysieke/virtuele Linux-machines on-premises, in azure of in een andere Cloud dan Azure

Als u de configuratie van de computer niet wilt beheren vanuit de Cloud, kunt u de configuratie van Azure Automation status gebruiken als een eind punt voor alleen een rapport. Met deze functie kunt u configuratie (push) configuraties via DSC instellen en rapportage details weer geven in Azure Automation.

> [!NOTE]
> Het beheer van Azure-Vm's met de configuratie van Azure Automation status is gratis inbegrepen als de geïnstalleerde versie van de configuratie uitbreiding voor de gewenste Azure VM groter is dan 2,70. Zie de [**pagina met prijzen voor Automation**](https://azure.microsoft.com/pricing/details/automation/)voor meer informatie.

## <a name="why-use-azure-automation-state-configuration"></a>Waarom Azure Automation status configuratie gebruiken?

Azure Automation status configuratie biedt verschillende voor delen ten opzichte van het gebruik van DSC buiten Azure. Met deze service kan de schaal baarheid van duizenden machines snel en eenvoudig worden uitgebreid vanaf een centrale, veilige locatie. U kunt computers eenvoudig inschakelen, ze declaratieve configuraties toewijzen en rapporten weer geven waarin de naleving van elke computer wordt aangegeven met de gewenste status die u opgeeft.

De Azure Automation State Configuration-service is van DSC wat Azure Automation runbooks zijn voor het uitvoeren van Power shell-scripts. Met andere woorden, op dezelfde manier als Azure Automation u bij het beheren van Power shell-scripts, kunt u ook DSC-configuraties beheren. 

### <a name="built-in-pull-server"></a>Ingebouwde pull-server

Azure Automation status configuratie biedt een DSC-pull-server vergelijkbaar met de [Windows-functie DSC-service](/powershell/scripting/dsc/pull-server/pullserver). Doel knooppunten kunnen automatisch configuraties ontvangen, voldoen aan de gewenste status en rapporteren over hun naleving. De ingebouwde pull-server in Azure Automation elimineert de nood zaak om uw eigen pull-server in te stellen en te onderhouden. Azure Automation kunnen virtuele of fysieke Windows-of Linux-machines in de Cloud of on-premises zijn gericht.

### <a name="management-of-all-your-dsc-artifacts"></a>Beheer van al uw DSC-artefacten

Met de configuratie van de Azure Automation-status wordt dezelfde Management laag naar de [gewenste status configuratie van Power](/powershell/scripting/dsc/overview/overview) shell verzonden als voor Power shell-scripts. Vanuit de Azure Portal of vanuit Power shell kunt u al uw DSC-configuraties,-resources en-doel knooppunten beheren.

![Scherm afbeelding van de Azure Automation pagina](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-of-reporting-data-into-azure-monitor-logs"></a>Rapport gegevens importeren in Azure Monitor-logboeken

Knoop punten die worden beheerd met Azure Automation status configuratie, verzenden gedetailleerde rapportage status gegevens naar de ingebouwde pull-server. U kunt Azure Automation status configuratie configureren om deze gegevens te verzenden naar uw Log Analytics-werk ruimte. Zie [door sturen Azure Automation status configuratie rapport gegevens naar Azure monitor-logboeken](automation-dsc-diagnostics.md).

## <a name="prerequisites-for-using-azure-automation-state-configuration"></a>Vereisten voor het gebruik van Azure Automation status configuratie

Houd rekening met de vereisten in deze sectie wanneer u Azure Automation status configuratie gebruikt.

### <a name="operating-system-requirements"></a>Vereisten voor het besturingssysteem

Voor knoop punten waarop Windows worden uitgevoerd, worden de volgende versies ondersteund:

- Windows Server 2019
- Windows Server 2016
- Windows Server-2012R2
- Windows Server 2012
- Windows Server 2008 R2 SP1
- Windows 10
- Windows 8.1
- Windows 7

>[!NOTE]
>De SKU van de zelfstandige product [Microsoft Hyper-V Server](/windows-server/virtualization/hyper-v/hyper-v-server-2016) bevat geen implementatie van DSC. Het kan dus niet worden beheerd door de configuratie van de Power shell DSC-of Azure Automation status.

Voor knoop punten waarop Linux wordt uitgevoerd, ondersteunt de DSC Linux-extensie alle Linux-distributies die worden vermeld onder [ondersteunde Linux-distributies](https://github.com/Azure/azure-linux-extensions/tree/master/DSC#4-supported-linux-distributions).

### <a name="dsc-requirements"></a>DSC-vereisten

Voor alle Windows-knoop punten die worden uitgevoerd in azure, wordt [WMF 5,1](https://docs.microsoft.com/powershell/scripting/wmf/setup/install-configure) geïnstalleerd wanneer machines zijn ingeschakeld. Voor knoop punten met Windows Server 2012 en Windows 7 is [WinRM](https://docs.microsoft.com/powershell/scripting/dsc/troubleshooting/troubleshooting#winrm-dependency) ingeschakeld.

Voor alle Linux-knoop punten die worden uitgevoerd in azure, wordt [Power shell DSC voor Linux](https://github.com/Microsoft/PowerShell-DSC-for-Linux) geïnstalleerd wanneer machines zijn ingeschakeld.

### <a name="configuration-of-private-networks"></a><a name="network-planning"></a>Configuratie van particuliere netwerken

Als uw knoop punten zich in een particulier netwerk bevinden, zijn de volgende poort en Url's vereist. Deze resources bieden netwerk connectiviteit voor het beheerde knoop punt en toestaan dat DSC communiceert met Azure Automation.

* Poort: alleen TCP 443 vereist voor uitgaande internet toegang
* Globale URL: ***. Azure-Automation.net**
* Globale URL van US Gov-Virginia: ***. Azure-Automation.us**
* Agent service: **https:// \< workspaceId \> . agentsvc.Azure-Automation.net**

Als u gebruikmaakt van DSC-resources die communiceren tussen knoop punten, zoals de [WaitFor *-resources](https://docs.microsoft.com/powershell/scripting/dsc/reference/resources/windows/waitForAllResource), moet u ook verkeer tussen knoop punten toestaan. Raadpleeg de documentatie voor elke DSC-resource voor meer informatie over deze netwerk vereisten.

#### <a name="proxy-support"></a>Proxy ondersteuning

Proxy ondersteuning voor de DSC-agent is beschikbaar in Windows versie 1809 en hoger. Deze optie wordt ingeschakeld door de waarden voor `ProxyURL` en `ProxyCredential` Eigenschappen in te stellen in het- [configuratie script](automation-dsc-onboarding.md#generate-dsc-metaconfigurations) dat wordt gebruikt voor het registreren van knoop punten. 

>[!NOTE]
>Azure Automation status configuratie biedt geen ondersteuning voor DSC-proxy voor eerdere versies van Windows.

Voor Linux-knoop punten ondersteunt de DSC-agent proxy en maakt gebruik van de `http_proxy` variabele om de URL te bepalen. Zie [DSC-mailconfiguraties genereren](automation-dsc-onboarding.md#generate-dsc-metaconfigurations)voor meer informatie over proxy ondersteuning.

#### <a name="azure-automation-state-configuration-network-ranges-and-namespace"></a>Netwerkbereiken en naam ruimte Azure Automation status configuratie

Het is raadzaam om de hieronder vermelde adressen te gebruiken bij het definiëren van uitzonde ringen. Voor IP-adressen kunt u de [IP-adresbereiken van Microsoft Azure Data Center](https://www.microsoft.com/download/details.aspx?id=41653)downloaden. Dit bestand wordt wekelijks bijgewerkt en heeft de huidige geïmplementeerde bereiken en eventuele toekomstige wijzigingen in de IP-bereiken.

Als u een Automation-account hebt dat is gedefinieerd voor een specifieke regio, kunt u de communicatie beperken tot dat regionale Data Center. De volgende tabel bevat de DNS-record voor elke regio:

| **Regio** | **DNS-record** |
| --- | --- |
| VS - west-centraal | wcus-jobruntimedata-prod-su1.azure-automation.net</br>wcus-agentservice-prod-1.azure-automation.net |
| VS - zuid-centraal |scus-jobruntimedata-prod-su1.azure-automation.net</br>scus-agentservice-prod-1.azure-automation.net |
| VS - oost    | eus-jobruntimedata-prod-su1.azure-automation.net</br>eus-agentservice-prod-1.azure-automation.net |
| VS - oost 2 |eus2-jobruntimedata-prod-su1.azure-automation.net</br>eus2-agentservice-prod-1.azure-automation.net |
| Canada - midden |cc-jobruntimedata-prod-su1.azure-automation.net</br>cc-agentservice-prod-1.azure-automation.net |
| Europa -west |we-jobruntimedata-prod-su1.azure-automation.net</br>we-agentservice-prod-1.azure-automation.net |
| Europa - noord |ne-jobruntimedata-prod-su1.azure-automation.net</br>ne-agentservice-prod-1.azure-automation.net |
| Azië - zuidoost |sea-jobruntimedata-prod-su1.azure-automation.net</br>sea-agentservice-prod-1.azure-automation.net|
| India - centraal |cid-jobruntimedata-prod-su1.azure-automation.net</br>cid-agentservice-prod-1.azure-automation.net |
| Japan - oost |jpe-jobruntimedata-prod-su1.azure-automation.net</br>jpe-agentservice-prod-1.azure-automation.net |
| Australië - zuidoost |ase-jobruntimedata-prod-su1.azure-automation.net</br>ase-agentservice-prod-1.azure-automation.net |
| Verenigd Koninkrijk Zuid | uks-jobruntimedata-prod-su1.azure-automation.net</br>uks-agentservice-prod-1.azure-automation.net |
| VS (overheid) - Virginia | usge-jobruntimedata-prod-su1.azure-automation.us<br>usge-agentservice-prod-1.azure-automation.us |

Down load het [Azure Data Center IP-adres](https://www.microsoft.com/download/details.aspx?id=41653) XML-bestand in het micro soft Download centrum voor een lijst met IP-adressen van regio's in plaats van regio namen.

> [!NOTE]
> Het XML-bestand met IP-adressen van Azure Data Center geeft een lijst van de IP-adresbereiken die worden gebruikt in de Microsoft Azure data centers. Het bestand bevat compute-, SQL-en Storage-bereiken.
>
>Er wordt wekelijks een bijgewerkt bestand geplaatst. Het bestand weerspiegelt de huidige geïmplementeerde bereiken en eventuele toekomstige wijzigingen in de IP-bereiken. Nieuwe bereiken die in het bestand worden weer gegeven, worden gedurende ten minste één week niet gebruikt in de data centers. Het is een goed idee om elke week het nieuwe XML-bestand te downloaden. Werk vervolgens uw site bij om de services die in Azure worden uitgevoerd, correct te identificeren. 

Gebruikers van Azure ExpressRoute moeten weten dat dit bestand wordt gebruikt om de Border Gateway Protocol (BGP)-advertentie van Azure Space bij te werken in de eerste week van elke maand.

## <a name="next-steps"></a>Volgende stappen

- Zie aan de slag [met de configuratie van de Azure Automation-status](automation-dsc-getting-started.md)om aan de slag te gaan.
- Zie [Azure Automation status configuratie inschakelen](automation-dsc-onboarding.md)voor meer informatie over het inschakelen van knoop punten.
- Zie [DSC-configuraties compileren in azure Automation status configuratie](automation-dsc-compile.md)voor meer informatie over het compileren van DSC-configuraties zodat u ze aan doel knooppunten kunt toewijzen.
- Zie [continue implementatie instellen met chocolade](automation-dsc-cd-chocolatey.md)voor een voor beeld van het gebruik van Azure Automation status configuratie in een pijp lijn voor continue implementatie.
- Zie [prijzen voor Azure Automation status configuratie](https://azure.microsoft.com/pricing/details/automation/)voor prijs informatie.
- Zie [AZ. Automation](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
)(Engelstalig) voor een Power shell-cmdlet-verwijzing.