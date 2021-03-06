---
title: 'Azure Security Center Veelgestelde vragen: gegevens verzameling en agents'
description: Veelgestelde vragen over het verzamelen van gegevens, agents en werk ruimten voor Azure Security Center, een product waarmee u bedreigingen kunt voor komen, detecteren en reageren
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: 022942778b714d5d66ce6eeb2c29351b11c66e40
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2020
ms.locfileid: "83996241"
---
# <a name="faq---questions-about-data-collection-agents-and-workspaces"></a>Veelgestelde vragen: vragen over het verzamelen van gegevens, agents en werk ruimten

Security Center verzamelt gegevens van uw virtuele machines van Azure (Vm's), schaal sets voor virtuele machines, IaaS containers en niet-Azure-computers (inclusief lokale computers) om te controleren op beveiligings problemen en bedreigingen. Gegevens worden verzameld met behulp van de Log Analytics-agent, die verschillende aan beveiliging gerelateerde configuraties en gebeurtenis logboeken van de computer leest en de gegevens naar uw werk ruimte kopieert voor analyse.


## <a name="am-i-billed-for-azure-monitor-logs-on-the-workspaces-created-by-security-center"></a>Ben ik gefactureerd voor Azure Monitor-logboeken op de werk ruimten die door Security Center zijn gemaakt?

Nee. Werk ruimten die zijn gemaakt door Security Center en die zijn geconfigureerd voor Azure Monitor-logboeken per knoop punt, worden niet in rekening gebracht Azure Monitor logboek kosten. Security Center facturering is altijd gebaseerd op uw Security Center beveiligings beleid en de oplossingen die zijn geïnstalleerd op een werk ruimte:

- **Gratis laag** – Security Center maakt de oplossing ' SecurityCenterFree ' in de standaardwerk ruimte. Er worden geen kosten in rekening gebracht voor de gratis laag.

- **Standard-laag** – Security Center maakt de oplossing beveiliging in de standaardwerk ruimte.

Zie [Security Center prijzen](https://azure.microsoft.com/pricing/details/security-center/)voor meer informatie over prijzen.

> [!NOTE]
> De prijs categorie voor log Analytics van werk ruimten die zijn gemaakt door Security Center heeft geen invloed op Security Center facturering.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]


## <a name="what-qualifies-a-vm-for-automatic-provisioning-of-the-log-analytics-agent-installation"></a>Wat is een virtuele machine die in aanmerking komt voor het automatisch inrichten van de Log Analytics agent-installatie?

Windows-of Linux IaaS-Vm's komen in aanmerking als:

- De uitbrei ding voor de Log Analytics agent is momenteel niet geïnstalleerd op de virtuele machine.
- De virtuele machine wordt uitgevoerd.
- De Windows-of Linux [Azure virtual machine-agent](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) is geïnstalleerd.
- De virtuele machine wordt niet gebruikt als een apparaat zoals Web Application Firewall of een firewall van de volgende generatie.


## <a name="where-is-the-default-log-analytics-workspace-created"></a>Waar is de standaard Log Analytics werkruimte gemaakt?

De locatie van de standaardwerk ruimte is afhankelijk van uw Azure-regio:

- Voor virtuele machines in de Verenigde Staten en Brazilië is de locatie van de werk ruimte de Verenigde Staten
- Voor Vm's in Canada is de locatie van de werk ruimte Canada
- Voor Vm's in Europa is de locatie van de werk ruimte Europa
- Voor Vm's in het Verenigd Konink rijk is de locatie van de werk ruimte het Verenigd Konink rijk
- Voor virtuele machines in Azië-oost en Zuidoost-Azië is de locatie van de werk ruimte Azië
- Voor Vm's in Korea is de locatie van de werk ruimte Korea
- Voor Vm's in India is de locatie van de werk ruimte India
- Voor Vm's in Japan is de locatie van de werk ruimte Japan
- Voor Vm's in China is de locatie van de werk ruimte China
- Voor Vm's in Australië is de locatie van de werk ruimte Australië


## <a name="what-data-is-collected-by-the-log-analytics-agent"></a>Welke gegevens worden verzameld door de Log Analytics-agent?

Zie [wat wordt bewaakt door Azure monitor?](https://docs.microsoft.com/azure/azure-monitor/monitor-reference#azure-services)voor een volledige lijst van de toepassingen en services die door de agent worden bewaakt.

> [!IMPORTANT]
> Houd er rekening mee dat voor sommige services, zoals Azure Firewall, als u logboek registratie hebt ingeschakeld en een intensieve-bron hebt gekozen voor aanmelding (bijvoorbeeld door het logboek in te stellen op *uitgebreid*), u mogelijk belang rijke gevolgen voor uw log Analytics werkruimte opslag nodig hebt. 


## <a name="can-i-delete-the-default-workspaces-created-by-security-center"></a>Kan ik de standaardwerk ruimten verwijderen die zijn gemaakt door Security Center?

**Het is niet raadzaam de standaardwerk ruimte te verwijderen.** Security Center gebruikt de standaardwerk ruimte voor het opslaan van beveiligings gegevens van uw virtuele machines. Als u een werk ruimte verwijdert, kan Security Center deze gegevens niet verzamelen. sommige beveiligings aanbevelingen en-waarschuwingen zijn niet beschikbaar.

Als u wilt herstellen, verwijdert u de Log Analytics-agent op de virtuele machines die zijn verbonden met de verwijderde werk ruimte. Security Center installeert de agent opnieuw en maakt nieuwe standaardwerk ruimten.

## <a name="how-can-i-use-my-existing-log-analytics-workspace"></a>Hoe kan ik mijn bestaande Log Analytics-werk ruimte gebruiken?

U kunt een bestaande Log Analytics-werk ruimte selecteren om gegevens op te slaan die zijn verzameld door Security Center. Uw bestaande Log Analytics-werk ruimte gebruiken:

- De werk ruimte moet worden gekoppeld aan uw geselecteerde Azure-abonnement.
- U moet mini maal lees machtigingen hebben voor toegang tot de werk ruimte.

Een bestaande Log Analytics-werk ruimte selecteren:

1. Onder **beveiligings beleid – gegevens verzamelen**selecteert **u een andere werk ruimte gebruiken**.

    ![Een andere werk ruimte gebruiken][4]

1. Selecteer in de vervolg keuzelijst een werk ruimte om de verzamelde gegevens op te slaan.

    > [!NOTE]
    > In het vervolg keuzemenu worden alleen werk ruimten weer gegeven waartoe u toegang hebt en die zich in uw Azure-abonnement bevinden.

1. Selecteer **Opslaan**. U wordt gevraagd of u de bewaakte Vm's opnieuw wilt configureren.

    - Selecteer **Nee** als u de nieuwe werkruimte instellingen **alleen op nieuwe vm's wilt Toep assen**. De nieuwe werk ruimte-instellingen zijn alleen van toepassing op nieuwe agent installaties. nieuw gedetecteerde Vm's waarop de Log Analytics-agent niet is geïnstalleerd.
    - Selecteer **Ja** als u de nieuwe werkruimte instellingen wilt **Toep assen op alle vm's**. Daarnaast wordt elke VM die is verbonden met een Security Center gemaakte werk ruimte opnieuw verbonden met de nieuwe doel werkruimte.

    > [!NOTE]
    > Als u **Ja**selecteert, verwijdert u geen werk ruimten die zijn gemaakt door Security Center totdat alle virtuele machines opnieuw zijn verbonden met de nieuwe doel werkruimte. Deze bewerking mislukt als er te vroeg een werk ruimte wordt verwijderd.

    - Als u de bewerking wilt annuleren, selecteert u **Annuleren**.

## <a name="what-if-the-log-analytics-agent-was-already-installed-as-an-extension-on-the-vm"></a>Wat gebeurt er als de Log Analytics-agent al is geïnstalleerd als een uitbrei ding op de VM?<a name="mmaextensioninstalled"></a>

Wanneer de bewakings agent is geïnstalleerd als een uitbrei ding, staat de extensie configuratie slechts aan één werk ruimte toe. Security Center worden bestaande verbindingen met gebruikers ruimten niet overschreven. Security Center worden beveiligings gegevens van een virtuele machine opgeslagen in een werk ruimte die al is verbonden, op voor waarde dat de oplossing Security of SecurityCenterFree is geïnstalleerd. Security Center kunt de extensie versie in dit proces upgraden naar de meest recente versie.

Zie [automatische inrichting in het geval van een bestaande Agent installatie](security-center-enable-data-collection.md#preexisting)voor meer informatie.



## <a name="what-if-a-log-analytics-agent-is-directly-installed-on-the-machine-but-not-as-an-extension-direct-agent"></a>Wat gebeurt er als een Log Analytics-agent rechtstreeks op de computer is geïnstalleerd, maar niet als een uitbrei ding (direct agent)?<a name="directagentinstalled"></a>

Als de Log Analytics-agent rechtstreeks op de virtuele machine is geïnstalleerd (niet als een Azure-extensie), wordt Security Center de Log Analytics agent-extensie geïnstalleerd en kan de Log Analytics agent worden bijgewerkt naar de nieuwste versie.

De agent die is geïnstalleerd, blijft rapporteren aan de al geconfigureerde werk ruimte (n) en rapporteert bovendien aan de werk ruimte die is geconfigureerd in Security Center (multi-multihoming wordt ondersteund op Windows-computers).

Als de geconfigureerde werk ruimte een gebruikers werkruimte is (niet Security Center de standaardwerk ruimte), moet u de oplossing ' Security/' SecurityCenterFree ' installeren voor Security Center om te beginnen met het verwerken van gebeurtenissen van Vm's en computers die aan die werk ruimte rapporteren.

Voor Linux-machines wordt agent-multi-multihoming nog niet ondersteund. als er een bestaande agent wordt gedetecteerd, treedt er geen automatische inrichting op en wordt de configuratie van de computer niet gewijzigd.

Voor bestaande machines op abonnementen die vóór 17 2019 maart op Security Center worden uitgevoerd, wordt de uitbrei ding van de Log Analytics agent niet geïnstalleerd en wordt de computer niet beïnvloed als er een bestaande agent wordt gedetecteerd. Voor deze computers raadpleegt u de aanbeveling bewakings agent status problemen op uw computers oplossen om de installatie problemen van de agent op deze computers op te lossen

Voor meer informatie raadpleegt u de volgende sectie [wat er gebeurt als er al een System Center Operations Manager of OMS direct-agent is geïnstalleerd op mijn VM?](#scomomsinstalled)

## <a name="what-if-a-system-center-operations-manager-agent-is-already-installed-on-my-vm"></a>Wat gebeurt er als er al een System Center Operations Manager-agent is geïnstalleerd op mijn VM?<a name="scomomsinstalled"></a>

In Security Center wordt de Log Analytics agent-extensie naast de bestaande System Center Operations Manager agent geïnstalleerd. De bestaande agent blijft normaal aan de System Center Operations Manager-server rapporteren. Houd er rekening mee dat de Operations Manager agent en Log Analytics agent gemeen schappelijke runtime-bibliotheken delen, die tijdens dit proces worden bijgewerkt naar de meest recente versie. Opmerking: als versie 2012 van de Operations Manager-agent is geïnstalleerd, moet u niet automatisch inrichten inschakelen (beheer mogelijkheden kunnen verloren gaan als de Operations Manager server ook versie 2012 is).


## <a name="what-is-the-impact-of-removing-these-extensions"></a>Wat is de impact van het verwijderen van deze uitbrei dingen?

Als u de micro soft-bewakings extensie verwijdert, kan Security Center geen beveiligings gegevens van de virtuele machine verzamelen. sommige beveiligings aanbevelingen en-waarschuwingen zijn niet beschikbaar. Binnen 24 uur bepaalt Security Center dat de VM de uitbrei ding mist en de uitbrei ding opnieuw installeert.


## <a name="how-do-i-stop-the-automatic-agent-installation-and-workspace-creation"></a>Hoe kan ik de automatische agent installatie en het maken van de werk ruimte stoppen?

U kunt automatische inrichting voor uw abonnementen uitschakelen in het beveiligings beleid, maar dit wordt niet aanbevolen. Automatische inrichtings limieten Security Center aanbevelingen en waarschuwingen uitschakelen. Automatische inrichting uitschakelen:

1. Als uw abonnement is geconfigureerd voor de Standard-laag, opent u het beveiligings beleid voor dat abonnement en selecteert u de laag **gratis** .

   ![Prijscategorie][1]

1. Schakel vervolgens automatische inrichting uit door **uit** te scha kelen op de pagina **beveiligings beleid – gegevens verzameling** .
   ![Gegevensverzameling][2]


## <a name="should-i-opt-out-of-the-automatic-agent-installation-and-workspace-creation"></a>Moet ik de automatische agent installatie en het maken van de werk ruimte afmelden?

> [!NOTE]
> Zorg ervoor dat u secties bekijkt [Wat zijn de gevolgen van het afmelden?](#what-are-the-implications-of-opting-out-of-automatic-provisioning) en de [Aanbevolen stappen bij](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning) het afmelden als u ervoor kiest om u af te melden voor automatische inrichting.

U kunt het beste niet automatisch inrichten als het volgende van toepassing is op u:

- Automatische agent installatie door Security Center is van toepassing op het hele abonnement. U kunt automatische installatie niet Toep assen op een subset van Vm's. Als er kritieke Vm's zijn die niet met de Log Analytics-agent kunnen worden geïnstalleerd, moet u zich afmelden voor automatische inrichting.
- Door de installatie van de Log Analytics agent-extensie wordt de versie van de agent bijgewerkt. Dit geldt voor een directe agent en een System Center Operations Manager-agent (in de laatste, de Operations Manager en Log Analytics agent delen common runtime-bibliotheken die in het proces worden bijgewerkt). Als de geïnstalleerde Operations Manager-agent versie 2012 is en is bijgewerkt, kunnen de beheer baarheids mogelijkheden verloren gaan wanneer de Operations Manager-server ook versie 2012 is. Houd er rekening mee dat u het automatisch inrichten kunt uitschakelen als de geïnstalleerde Operations Manager-agent versie 2012 is.
- Als u een aangepaste werk ruimte hebt die extern is voor het abonnement (een gecentraliseerde werk ruimte), moet u zich afmelden voor automatische inrichting. U kunt de Log Analytics agent-extensie hand matig installeren en verbinding maken met uw werk ruimte zonder de verbinding te Security Center vervangen.
- Als u wilt voor komen dat meerdere werk ruimten per abonnement worden gemaakt en u uw eigen aangepaste werk ruimte in het abonnement hebt, hebt u twee opties:

   1. U kunt zich afmelden voor automatische inrichting. Stel na de migratie de standaard instellingen voor de werk ruimte in, zoals wordt beschreven in [Hoe kan ik mijn bestaande log Analytics werkruimte gebruiken?](#how-can-i-use-my-existing-log-analytics-workspace)

   1. U kunt ook toestaan dat de migratie is voltooid, de Log Analytics-agent moet worden geïnstalleerd op de virtuele machines en de virtuele machines die zijn verbonden met de gemaakte werk ruimte. Selecteer vervolgens uw eigen aangepaste werk ruimte door de standaard instelling voor werk ruimten in te stellen met de optie voor het opnieuw configureren van de reeds geïnstalleerde agents. Zie [Hoe kan ik mijn bestaande log Analytics werkruimte gebruiken?](#how-can-i-use-my-existing-log-analytics-workspace) voor meer informatie.


## <a name="what-are-the-implications-of-opting-out-of-automatic-provisioning"></a>Wat zijn de gevolgen van het afmelden van automatische inrichting?

Wanneer de migratie is voltooid, kan Security Center geen beveiligings gegevens van de virtuele machine verzamelen en sommige beveiligings aanbevelingen en-waarschuwingen zijn niet beschikbaar. Als u zich afmeldt, installeert u de Log Analytics agent hand matig. Bekijk de [Aanbevolen stappen bij het afmelden](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning).


## <a name="what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning"></a>Wat zijn de aanbevolen stappen bij het afmelden van automatische inrichting?

Installeer de Log Analytics agent-extensie hand matig zodat Security Center beveiligings gegevens van uw virtuele machines kan verzamelen en aanbevelingen en waarschuwingen kunt geven. Zie [installatie van de agent voor](../virtual-machines/extensions/oms-windows.md) de installatie van de Windows-VM of de [agent voor Linux VM](../virtual-machines/extensions/oms-linux.md) voor meer informatie over de installatie.

U kunt de agent verbinden met een bestaande aangepaste werk ruimte of Security Center gemaakte werk ruimte. Als voor een aangepaste werk ruimte de oplossingen Security of SecurityCenterFree niet zijn ingeschakeld, moet u een oplossing Toep assen. Als u wilt Toep assen, selecteert u de aangepaste werk ruimte of het abonnement en past u een prijs categorie toe via de pagina **beveiligings beleid – prijs categorie** .

   ![Prijscategorie][1]

Met Security Center wordt de juiste oplossing op basis van de geselecteerde prijs categorie op de werk ruimte ingeschakeld.


## <a name="how-do-i-remove-oms-extensions-installed-by-security-center"></a>Hoe kan ik OMS-extensies die door Security Center zijn geïnstalleerd, verwijderen?<a name="remove-oms"></a>

U kunt de Log Analytics agent hand matig verwijderen. Dit wordt niet aanbevolen omdat deze Security Center aanbevelingen en waarschuwingen beperkt.

> [!NOTE]
> Als gegevens verzameling is ingeschakeld, wordt de agent door Security Center opnieuw geïnstalleerd nadat u deze hebt verwijderd.  U moet de gegevens verzameling uitschakelen voordat u de agent hand matig verwijdert. Zie Hoe kan ik de automatische agent installatie en het maken van de werk ruimte stoppen? voor instructies voor het uitschakelen van gegevens verzameling.

Hand matig verwijderen van de agent:

1.    Open **log Analytics**in de portal.

1.    Selecteer op de pagina Log Analytics een werk ruimte:

1.    Selecteer de virtuele machines die u niet wilt bewaken en selecteer **verbinding verbreken**.

   ![De agent verwijderen][3]

> [!NOTE]
> Als een virtuele Linux-machine al een OMS-agent heeft die niet uitbrei ding is en u de uitbrei ding verwijdert, wordt ook de agent verwijderd en moet u deze opnieuw installeren.


## <a name="how-do-i-disable-data-collection"></a>Gegevens verzameling Hoe kan ik uitschakelen?

Automatisch inrichten wordt sterk aanbevolen om beveiligings waarschuwingen en aanbevelingen te ontvangen over systeem updates, OS-beveiligings problemen en Endpoint Protection. Automatisch inrichten is standaard uitgeschakeld.

Als u het hebt ingeschakeld, maar nu wilt uitschakelen:

1. Open **Security Center** in [het Azure Portal](https://portal.azure.com)en selecteer **beveiligings beleid**.

1. Selecteer het abonnement waarvoor u automatisch inrichten wilt uitschakelen.

    **Beveiligings beleid: gegevens verzamelen** wordt geopend.

1. Onder **automatische inrichting**selecteert u **uit**.


## <a name="how-do-i-enable-data-collection"></a>Gegevens verzameling Hoe kan ik inschakelen?

U kunt het verzamelen van gegevens voor uw Azure-abonnement inschakelen in het beveiligings beleid. Om het verzamelen van gegevens in te scha kelen. [Meld u aan bij de Azure Portal](https://portal.azure.com), selecteer **bladeren**, selecteer **Security Center**en selecteer **beveiligings beleid**. Selecteer het abonnement waarvoor u automatische inrichting wilt inschakelen. Wanneer u een beveiligings beleid voor abonnementen selecteert **: gegevens verzamelen** wordt geopend. Onder **automatische inrichting**selecteert u **aan**.


## <a name="what-happens-when-data-collection-is-enabled"></a>Wat gebeurt er wanneer gegevens verzameling is ingeschakeld?

Als automatische inrichting is ingeschakeld, wordt de Log Analytics agent door Security Center ingericht op alle ondersteunde virtuele machines van Azure en nieuwe die worden gemaakt. Automatische inrichting wordt aanbevolen, maar er is ook hand matige installatie van de agent beschikbaar. [Meer informatie over het installeren van de log Analytics agent-extensie](../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension). 

De agent maakt de gebeurtenis 4688 voor het maken van processen en het veld *commandline* in gebeurtenis 4688 mogelijk. Nieuwe processen die op de virtuele machine worden gemaakt, worden vastgelegd door EventLog en gecontroleerd door de detectie services van Security Center. Zie voor meer informatie over de gegevens die voor elk nieuwe proces zijn vastgelegd de [velden Beschrijving in 4688](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4688#fields). De agent verzamelt ook de 4688-gebeurtenissen die zijn gemaakt op de virtuele machine en slaat ze op in de zoek opdracht.

De agent maakt ook het verzamelen van gegevens mogelijk voor [adaptieve toepassings besturings elementen](security-center-adaptive-application.md), Security Center configureert een lokaal AppLocker-beleid in de controle modus om alle toepassingen toe te staan. Dit beleid zorgt ervoor dat AppLocker gebeurtenissen genereert die vervolgens worden verzameld en gebruikt door Security Center. Het is belang rijk te weten dat dit beleid niet wordt geconfigureerd op computers waarop al een geconfigureerd AppLocker-beleid is ingesteld. 

Als Security Center verdachte activiteiten op de virtuele machine detecteert, wordt de klant per e-mail op de hoogte gesteld als er [contact gegevens](security-center-provide-security-contact-details.md) voor de beveiliging zijn verstrekt. Er is ook een waarschuwing zichtbaar in het dash board beveiligings waarschuwingen van Security Center.


## <a name="will-security-center-work-using-an-oms-gateway"></a>Werkt Security Center met behulp van een OMS-gateway?

Ja. Azure Security Center maakt gebruik van Azure Monitor voor het verzamelen van gegevens van Azure-Vm's en-servers, met behulp van de Log Analytics agent.
Voor het verzamelen van de gegevens moet elke virtuele machine en server verbinding maken met internet met behulp van HTTPS. De verbinding kan direct worden gebruikt, met behulp van een proxy of via de [OMS-gateway](../azure-monitor/platform/gateway.md).


## <a name="does-the-monitoring-agent-impact-the-performance-of-my-servers"></a>Heeft de bewakings agent invloed op de prestaties van mijn servers?

De agent verbruikt een nominale hoeveelheid systeem bronnen en moet weinig invloed hebben op de prestaties. Zie de [plannings-en bedienings handleiding](security-center-planning-and-operations-guide.md#data-collection-and-storage)voor meer informatie over de invloed op de prestaties en de agent en de uitbrei ding.




<!--Image references-->
[1]: ./media/security-center-platform-migration-faq/pricing-tier.png
[2]: ./media/security-center-platform-migration-faq/data-collection.png
[3]: ./media/security-center-platform-migration-faq/remove-the-agent.png
[4]: ./media/security-center-platform-migration-faq/use-another-workspace.png
