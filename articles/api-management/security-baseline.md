---
title: Azure-beveiligings basislijn voor API Management
description: Azure-beveiligings basislijn voor API Management
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: ceaa44429d105c2a6a1d0883ab29ed11b63ad497
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2020
ms.locfileid: "83736072"
---
# <a name="azure-security-baseline-for-api-management"></a>Azure-beveiligings basislijn voor API Management

De Azure-beveiligings basislijn voor API Management bevat aanbevelingen waarmee u de beveiligings postuur van uw implementatie kunt verbeteren.

De basis lijn voor deze service wordt opgehaald uit de [Azure Security Bench Mark-versie 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview), die aanbevelingen biedt over hoe u uw cloud oplossingen kunt beveiligen in azure met onze richt lijnen voor best practices.

Zie het [overzicht van Azure Security-basis lijnen](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)voor meer informatie.

## <a name="network-security"></a>Netwerkbeveiliging

*Zie [beveiligings beheer: netwerk beveiliging](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security)voor meer informatie.*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1,1: Beveilig bronnen met behulp van netwerk beveiligings groepen of Azure Firewall op de Virtual Network

**Hulp**: Azure API Management kan worden geïmplementeerd in een Azure Virtual Network (Vnet), zodat het toegang heeft tot back-end-services in het netwerk. De ontwikkelaars Portal en de API Management Gateway kunnen zo worden geconfigureerd dat ze toegankelijk zijn vanaf internet (extern) of alleen binnen het Vnet (intern).
- Extern: de API Management Gateway en de ontwikkelaars Portal zijn toegankelijk via het open bare Internet via een extern load balancer. De gateway kan toegang krijgen tot bronnen in het virtuele netwerk.
- Intern: de API Management Gateway en de ontwikkelaars Portal zijn alleen toegankelijk vanuit het virtuele netwerk via een interne load balancer. De gateway kan toegang krijgen tot bronnen in het virtuele netwerk.

Binnenkomend en uitgaand verkeer in het subnet waarin API Management wordt geïmplementeerd, kunnen worden beheerd met behulp van de netwerk beveiligings groep.

* [Azure API Management gebruiken met virtuele netwerken](https://docs.microsoft.com/azure/api-management/api-management-using-with-vnet)

* [Azure API Management-service gebruiken met een intern virtueel netwerk](https://docs.microsoft.com/azure/api-management/api-management-using-with-internal-vnet)

* [API Management integreren in een intern VNET met Application Gateway](https://docs.microsoft.com/azure/api-management/api-management-howto-integrate-internal-vnet-appgateway)

**Azure Security Center bewaking**: momenteel niet beschikbaar

**Verantwoordelijkheid**: klant

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1,2: de configuratie en het verkeer van Vnets, subnetten en Nic's bewaken en vastleggen

**Richt lijnen**: binnenkomend en uitgaand verkeer in het subnet waarin API Management wordt geïmplementeerd, kunnen worden beheerd met behulp van netwerk beveiligings groepen (nsg's). Implementeer een NSG op uw API Management-subnet en schakel NSG-stroom Logboeken in en verzend logboeken naar een Azure Storage account voor verkeers controle. U kunt ook NSG-stroom logboeken naar een Log Analytics-werk ruimte verzenden en Traffic Analytics gebruiken om inzicht te krijgen in de verkeers stroom in uw Azure-Cloud. Enkele voor delen van Traffic Analytics zijn de mogelijkheid om netwerk activiteiten te visualiseren en HOTS pots te identificeren, beveiligings dreigingen te identificeren, verkeers patronen te begrijpen en netwerk configuraties te lokaliseren.

Let op: bij het configureren van een NSG op het subnet van API Management, moet er een set poorten zijn die open moeten zijn. Als een van deze poorten niet beschikbaar is, werkt API Management mogelijk niet goed en wordt deze mogelijk niet meer toegankelijk.

* [Meer informatie over NSG-configuraties voor Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-using-with-vnet#-common-network-configuration-issues)

* [NSG-stroom logboeken inschakelen](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Traffic Analytics inschakelen en gebruiken](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

**Azure Security Center bewaking**: momenteel niet beschikbaar

**Verantwoordelijkheid**: klant

### <a name="13-protect-critical-web-applications"></a>1,3: essentiële webtoepassingen beveiligen

**Richt lijnen**: voor het beveiligen van belang rijke Web/http-api's configureert u API Management in een Virtual Network (Vnet) in de interne modus en configureert u een Azure-toepassing gateway. Application Gateway is een PaaS-service. Het fungeert als een omgekeerde proxy en biedt N7-taak verdeling, route ring, Web Application Firewall (WAF) en andere services.

Als u API Management die is ingericht in een intern Vnet combineert met de Application Gateway front-end, worden de volgende scenario's ingeschakeld:
- Gebruik één API Management resource om alle Api's aan zowel interne consumenten als externe consumenten weer te geven.
- Gebruik één API Management resource om een subset van Api's aan externe gebruikers toe te staan.
- Bieden een manier om toegang tot API Management van het open bare Internet te scha kelen in-en uitschakelen.

Opmerking: deze functie is beschikbaar in de Premium-en Developer-laag van API Management.

* [API Management integreren in een intern VNET met Application Gateway](https://docs.microsoft.com/azure/api-management/api-management-howto-integrate-internal-vnet-appgateway)

* [Azure-toepassing gateway begrijpen](https://docs.microsoft.com/azure/application-gateway/)

**Azure Security Center bewaking**: momenteel niet beschikbaar

**Verantwoordelijkheid**: klant

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: communicatie met bekende schadelijke IP-adressen weigeren

**Hulp**: API Management configureren in een Virtual Network (Vnet) in de interne modus en een Azure-toepassing gateway configureren. Application Gateway is een PaaS-service. Het fungeert als een omgekeerde proxy en biedt N7-taak verdeling, route ring, Web Application Firewall (WAF) en andere services.

Als u API Management die is ingericht in een intern Vnet combineert met de Application Gateway front-end, worden de volgende scenario's ingeschakeld:
- Gebruik één API Management resource om alle Api's aan zowel interne consumenten als externe consumenten weer te geven.
- Gebruik één API Management resource om een subset van Api's aan externe gebruikers toe te staan.
- Bieden een manier om toegang tot API Management van het open bare Internet te scha kelen in-en uitschakelen.

Opmerking: deze functie is beschikbaar in de Premium-en Developer-laag van API Management.

Schakel Azure DDoS Protection standaard in op het Vnet dat is gekoppeld aan uw API Management-implementatie om te beschermen tegen DDoS-aanvallen (Distributed Denial of service).

Gebruik Azure Security Center geïntegreerde bedreigings informatie om communicatie met bekende of ongebruikte Internet-IP-adressen te weigeren.

* [API Management integreren in een intern VNET met Application Gateway](https://docs.microsoft.com/azure/api-management/api-management-howto-integrate-internal-vnet-appgateway)

* [Azure-toepassing gateway begrijpen](https://docs.microsoft.com/azure/application-gateway/)

* [Azure DDoS Protection Standard configureren](https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection)

* [Meer informatie over Azure Security Center geïntegreerde bedreigings informatie](https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer)

**Azure Security Center bewaking**: Ja

**Verantwoordelijkheid**: klant

### <a name="15-record-network-packets-and-flow-logs"></a>1,5: netwerk pakketten en stroom logboeken vastleggen

**Richt lijnen**: binnenkomend en uitgaand verkeer in het subnet waarin API Management wordt geïmplementeerd, kunnen worden beheerd met behulp van netwerk beveiligings groepen (NSG). Implementeer een NSG op uw API Management-subnet en schakel NSG-stroom Logboeken in en verzend logboeken naar een Azure Storage account voor verkeers controle. U kunt ook NSG-stroom logboeken naar een Log Analytics-werk ruimte verzenden en Traffic Analytics gebruiken om inzicht te krijgen in de verkeers stroom in uw Azure-Cloud. Enkele voor delen van Traffic Analytics zijn de mogelijkheid om netwerk activiteiten te visualiseren en HOTS pots te identificeren, beveiligings dreigingen te identificeren, verkeers patronen te begrijpen en netwerk configuraties te lokaliseren.

Let op: bij het configureren van een NSG op het subnet van API Management, moet er een set poorten zijn die open moeten zijn. Als een van deze poorten niet beschikbaar is, werkt API Management mogelijk niet goed en wordt deze mogelijk niet meer toegankelijk.

* [Meer informatie over NSG-configuraties voor Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-using-with-vnet#-common-network-configuration-issues)

* [NSG-stroom logboeken inschakelen](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal)

* [Traffic Analytics inschakelen en gebruiken](https://docs.microsoft.com/azure/network-watcher/traffic-analytics)

**Azure Security Center bewaking**: Ja

**Verantwoordelijkheid**: klant

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: op netwerk gebaseerde inbreuk detectie/indringings systemen (ID'S/IP-adressen) implementeren

**Hulp**: API Management configureren in een Virtual Network (Vnet) in de interne modus en een Azure-toepassing gateway configureren. Application Gateway is een PaaS-service. Het fungeert als een omgekeerde proxy en biedt N7-taak verdeling, route ring, Web Application Firewall (WAF) en andere services. Application Gateway WAF biedt beveiliging tegen veelvoorkomende beveiligings aanvallen en beveiligings problemen en kan in de volgende twee modi worden uitgevoerd:
- Detectie modus: Hiermee worden alle bedreigings waarschuwingen gecontroleerd en geregistreerd. U kunt logboek registratie diagnostiek voor Application Gateway inschakelen in de sectie diagnostische gegevens. U moet ervoor zorgen dat het WAF-logboek is geselecteerd en ingeschakeld. Web Application firewall blokkeert geen binnenkomende aanvragen wanneer deze in de detectie modus wordt uitgevoerd.
- Preventie modus: blokkeert indringingen en aanvallen die de regels detecteren. De aanvaller ontvangt een uitzonde ring "403 niet-geautoriseerde toegang" en de verbinding is gesloten. Met preventie modus worden dergelijke aanvallen in de WAF-logboeken vastgelegd.

Als u API Management die is ingericht in een intern Vnet combineert met de Application Gateway front-end, worden de volgende scenario's ingeschakeld:
- Gebruik één API Management resource om alle Api's aan zowel interne consumenten als externe consumenten weer te geven.
- Gebruik één API Management resource om een subset van Api's aan externe gebruikers toe te staan.
- Bieden een manier om toegang tot API Management van het open bare Internet te scha kelen in-en uitschakelen.

Opmerking: deze functie is beschikbaar in de Premium-en Developer-laag van API Management.

* [API Management integreren in een intern VNET met Application Gateway](https://docs.microsoft.com/azure/api-management/api-management-howto-integrate-internal-vnet-appgateway)

* [Azure-toepassing gateway WAF begrijpen](https://docs.microsoft.com/azure/web-application-firewall/ag/ag-overview)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="17-manage-traffic-to-web-applications"></a>1,7: verkeer naar webtoepassingen beheren

**Richt lijnen**: voor het beheren van verkeer dat wordt uitgevoerd op Web-en http-api's, implementeert u API Management op een Virtual Network (Vnet) dat is gekoppeld aan app service environment in de externe of interne modus.

Configureer in de interne modus een Azure-toepassing gateway vóór API Management. Application Gateway is een PaaS-service. Het fungeert als een omgekeerde proxy en biedt N7-taak verdeling, route ring, Web Application Firewall (WAF) en andere services. Application Gateway WAF biedt beveiliging tegen veelvoorkomende aanvallen en beveiligings problemen.

Als u API Management die is ingericht in een intern Vnet combineert met de Application Gateway front-end, worden de volgende scenario's ingeschakeld:
- Gebruik één API Management resource om alle Api's aan zowel interne consumenten als externe consumenten weer te geven.
- Gebruik één API Management resource om een subset van Api's aan externe gebruikers toe te staan.
- Bieden een manier om toegang tot API Management van het open bare Internet te scha kelen in-en uitschakelen.

Opmerking: deze functie is beschikbaar in de Premium-en Developer-laag van API Management.

* [Persoonlijke Api's beschikbaar stellen voor externe consumenten](https://docs.microsoft.com/azure/architecture/example-scenario/apps/publish-internal-apis-externally)

* [API Management binnen een Vnet gebruiken](https://docs.microsoft.com/azure/api-management/api-management-using-with-vnet)

* [Azure Web Application firewall op Azure-toepassing gateway](https://docs.microsoft.com/azure/web-application-firewall/ag/ag-overview)

* [Azure-toepassing gateway begrijpen](https://docs.microsoft.com/azure/application-gateway/overview)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: de complexiteit en administratieve overhead van netwerk beveiligings regels minimaliseren

**Hulp**: gebruik Virtual Network (Vnet)-service tags om netwerk toegangs beheer te definiëren voor netwerk beveiligings groepen (nsg's) die worden gebruikt op uw API Management subnetten. U kunt servicetags gebruiken in plaats van specifieke IP-adressen wanneer u beveiligingsregels maakt. Door de naam van de service label (bijvoorbeeld ApiManagement) op te geven in het juiste bron-of doel veld van een regel, kunt u het verkeer voor de bijbehorende service toestaan of weigeren. Micro soft beheert de adres voorvoegsels die zijn opgenomen in het servicetag van de service en werkt de servicetag automatisch bij met gewijzigde adressen.

Let op: bij het configureren van een NSG op het subnet van API Management, moet er een set poorten zijn die open moeten zijn. Als een van deze poorten niet beschikbaar is, werkt API Management mogelijk niet goed en wordt deze mogelijk niet meer toegankelijk.

* [Service Tags leren en gebruiken](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)

* [Poorten die vereist zijn voor de API Management](https://docs.microsoft.com/azure/api-management/api-management-using-with-vnet#-common-network-configuration-issues)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: standaard beveiligings configuraties voor netwerk apparaten onderhouden

**Hulp**: Definieer en implementeer standaard beveiligings configuraties voor netwerk instellingen met betrekking tot uw Azure API Management-implementaties. Gebruik Azure Policy aliassen in de naam ruimten ' micro soft. ApiManagement ' en ' micro soft. Network ' om aangepast beleid te maken om de netwerk configuratie van uw Azure API Management-implementaties en gerelateerde resources te controleren of af te dwingen. U kunt ook gebruikmaken van ingebouwde beleids definities voor virtuele Azure-netwerken, zoals:
- DDoS Protection standaard moet zijn ingeschakeld

U kunt ook Azure-blauw drukken gebruiken om grootschalige Azure-implementaties te vereenvoudigen door sleutel omgevings artefacten, zoals Azure Resource Manager sjablonen, op rollen gebaseerd toegangs beheer (RBAC) en beleids regels in één blauw druk-definitie. U kunt de blauw druk eenvoudig Toep assen op nieuwe abonnementen, omgevingen en het beheer en de verwerkings mogelijkheden van versies.

* [Azure Policy configureren en beheren](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Een Azure Blueprint maken](https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="110-document-traffic-configuration-rules"></a>1,10: configuratie regels voor het document verkeer

**Hulp**: Labels gebruiken voor netwerk beveiligings groepen (nsg's) en andere bronnen die betrekking hebben op netwerk beveiliging en verkeers stroom. Voor afzonderlijke NSG-regels kunt u het veld Beschrijving gebruiken om de bedrijfs behoefte en/of-duur op te geven voor alle regels die verkeer van of naar een netwerk toestaan.

* [Tags maken en gebruiken](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

* [Een Virtual Network maken](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

* [Een NSG maken met een beveiligings configuratie](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: gebruik automatische hulpprogram ma's om netwerk bron configuraties te bewaken en wijzigingen te detecteren

**Hulp**: Azure-activiteiten logboek gebruiken voor het bewaken van netwerk bron configuraties en het detecteren van wijzigingen aan netwerk bronnen die zijn gekoppeld aan uw Azure API Management-implementaties. Maak waarschuwingen in Azure Monitor die worden geactiveerd wanneer er wijzigingen in kritieke netwerk bronnen plaatsvinden.

* [Activiteiten logboek gebeurtenissen van Azure weer geven en ophalen](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Waarschuwingen maken in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

## <a name="logging-and-monitoring"></a>Logboekregistratie en bewaking

*Zie voor meer informatie [beveiligings beheer: logboek registratie en controle](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: goedgekeurde tijd synchronisatie bronnen gebruiken

**Richt lijnen**: micro soft onderhoudt tijd bronnen voor Azure API management.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: micro soft

### <a name="22-configure-central-security-log-management"></a>2,2: Centraal beveiligings logboek beheer configureren

**Richt lijnen**: u kunt in de Azure monitor log Analytics werk ruimte (n) gebruiken om een query uit te voeren, logboeken te verzenden naar Azure Storage voor langdurige, archiverings-of offline-analyses of Logboeken te exporteren naar een andere analyse oplossing in Azure en elders met behulp van Azure Event hubs. In azure API Management worden logboeken en metrische gegevens standaard naar Azure Monitor uitgevoerd. De uitgebreidheid van de logboek registratie kan worden geconfigureerd op basis van de gehele service en per API.

Azure API Management kan naast Azure Monitor worden geïntegreerd met een of meerdere Azure-toepassing Insights-Services. Registratie-instellingen voor Application Insights kunnen worden geconfigureerd op basis van een service of per API.

Indien gewenst, inschakelen en on-board gegevens voor Azure Sentinel of een beveiligings incident en gebeurtenis beheer van derden (SIEM).

* [Diagnostische instellingen configureren](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings#create-diagnostic-settings-in-azure-portal)

* [Azure-Sentinel onboarden](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [Aan de slag met Azure Monitor en integratie van SIEM van derden](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

* [Aangepaste logboek registratie en analyse pijplijn maken](https://docs.microsoft.com/azure/api-management/api-management-howto-log-event-hubs)

* [Integreren met Azure-toepassing Insights](https://docs.microsoft.com/azure/api-management/api-management-howto-app-insights)

**Azure Security Center bewaking**: momenteel niet beschikbaar

**Verantwoordelijkheid**: klant

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: controle logboek registratie inschakelen voor Azure-resources

**Richt lijnen**: voor het vastleggen van controle op vlak audits, de diagnostische instellingen van Azure-activiteiten logboek inschakelen en activiteiten logboeken verzenden naar een log Analytics-werk ruimte voor rapportage en analyse, tot Azure Storage voor lange termijn veilig bewaren, naar Azure Event hubs voor export in andere analyse oplossingen in Azure en elders. Met Azure-activiteiten logboek gegevens kunt u de ' What, wie en wanneer ' bepalen voor schrijf bewerkingen (PUT, POST, DELETE) die zijn uitgevoerd op het niveau van het besturings element van uw Azure API Management service.

Voor logboek registratie van gegevens vlak controle biedt Diagnostische logboeken uitgebreide informatie over bewerkingen en fouten die belang rijk zijn voor controle en probleem oplossing. Diagnoselogboeken verschillen van activiteitenlogboeken. Activiteitenlogboeken bieden inzicht in de bewerkingen die zijn uitgevoerd op uw Azure-resources. Diagnoselogboeken bieden inzicht in bewerkingen die door de resources zelf zijn uitgevoerd.

* [Diagnostische instellingen voor Azure-activiteiten logboek inschakelen](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Diagnostische instellingen inschakelen voor Azure API Management](https://docs.microsoft.com/Azure/api-management/api-management-howto-use-azure-monitor#diagnostic-logs)

**Azure Security Center bewaking**: momenteel niet beschikbaar

**Verantwoordelijkheid**: klant

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: beveiligings logboeken verzamelen van besturings systemen

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor reken resources.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: niet van toepassing

### <a name="25-configure-security-log-storage-retention"></a>2,5: Bewaar beveiliging van het beveiligings logboek configureren

**Hulp**: stel binnen Azure monitor uw Bewaar periode voor log Analytics werk ruimte in volgens de nalevings voorschriften van uw organisatie. Gebruik Azure Storage-accounts voor lange termijn/archiverings opslag.

* [Para meters voor het bewaren van Logboeken instellen voor Log Analytics-werk ruimten](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

* [Logboeken archiveren in een Azure Storage-account](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-collect-storage)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="26-monitor-and-review-logs"></a>2,6: Logboeken bewaken en controleren

**Hulp**: Azure API Management maakt voortdurend logboeken en metrische gegevens voor Azure monitor, waardoor u een bijna realtime inzicht krijgt in de status en status van uw api's. Met Azure Monitor en Log Analytics werk ruimte (n) kunt u de weer geven, query's uitvoeren, visualiseren, archiveren, waarschuwingen configureren en acties uitvoeren op metrische gegevens en logboeken die afkomstig zijn van API Management en gerelateerde resources. Analyseer en bewaak logboeken voor afwijkend gedrag en Bekijk regel matige resultaten.

U kunt API Management eventueel integreren met Azure-toepassing inzichten en deze gebruiken als primair of secundair bewakings-, tracerings-, rapportage-en waarschuwings programma.

* [Logboeken voor Azure API Management controleren en bekijken](https://docs.microsoft.com/Azure/api-management/api-management-howto-use-azure-monitor)

* [Aangepaste query's uitvoeren in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries)

* [Log Analytics-werk ruimte begrijpen](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)

* [Integreren met Azure-toepassing Insights](https://docs.microsoft.com/azure/api-management/api-management-howto-app-insights)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="27-enable-alerts-for-anomalous-activity"></a>2,7: waarschuwingen inschakelen voor afwijkende activiteit

**Hulp**: Schakel Diagnostische instellingen voor Azure-activiteiten logboek in, evenals de diagnostische instellingen voor uw Azure API Management-instanties en verzend de logboeken naar een log Analytics-werk ruimte. Query's uitvoeren in Log Analytics om termen te zoeken, trends te identificeren, patronen te analyseren en veel andere inzichten te bieden op basis van de verzamelde gegevens. U kunt waarschuwingen maken op basis van uw Log Analytics werkruimte query's.

Maak metrische waarschuwingen om u te laten weten wanneer er iets onverwachts plaatsvindt. U kunt bijvoorbeeld meldingen ontvangen wanneer uw Azure API Management-exemplaar de verwachte piek capaciteit over een bepaalde periode heeft overschreden of als er een bepaald aantal gateway-aanvragen of-fouten is opgetreden gedurende een vooraf gedefinieerde periode.

U kunt API Management eventueel integreren met Azure-toepassing inzichten en deze gebruiken als primair of secundair bewakings-, tracerings-, rapportage-en waarschuwings programma.

Optioneel kunt u gegevens in-en inschakelen voor Azure Sentinel of een SIEM van derden.

* [Diagnostische instellingen voor Azure-activiteiten logboek inschakelen](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy)

* [Diagnostische instellingen inschakelen voor Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-use-azure-monitor#diagnostic-logs)

* [Een waarschuwings regel configureren voor niet-geautoriseerde aanvragen](https://docs.microsoft.com/Azure/api-management/api-management-howto-use-azure-monitor#set-up-an-alert-rule-for-unauthorized-request)

* [Metrische gegevens over capaciteit van een Azure API Management-exemplaar weer geven](https://docs.microsoft.com/azure/api-management/api-management-capacity)

* [Integreren met Azure-toepassing Insights](https://docs.microsoft.com/azure/api-management/api-management-howto-app-insights)

* [Azure-Sentinel onboarden](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="28-centralize-anti-malware-logging"></a>2,8: registratie van anti-malware centraliseren

**Richt lijnen**: niet van toepassing; Azure API Management verwerkt of produceert geen anti-malware-gerelateerde Logboeken.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: niet van toepassing

### <a name="29-enable-dns-query-logging"></a>2,9: DNS-query logboek registratie inschakelen

**Richt lijnen**: niet van toepassing; Azure-API Management verwerkt of produceert geen door de gebruiker toegankelijke logboeken die betrekking hebben op DNS.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: niet van toepassing

### <a name="210-enable-command-line-audit-logging"></a>2,10: controle logboek registratie op opdracht regel inschakelen

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor reken resources.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: niet van toepassing

## <a name="identity-and-access-control"></a>Identiteits- en toegangsbeheer

*Zie [beveiligings beheer: identiteits-en toegangs beheer](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control)voor meer informatie.*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: een inventaris van beheerders accounts onderhouden

**Richt lijnen**: een inventaris van accounts met beheerders toegang tot de Azure API Management Control-besturings vlak (Azure Portal) onderhouden.

Azure Active Directory (AD) heeft ingebouwde rollen die expliciet moeten worden toegewezen en waarop query's kunnen worden doorzocht. API Management is afhankelijk van deze rollen en op rollen gebaseerde Access Control om verfijnd toegangs beheer voor API Management Services en entiteiten in te scha kelen.

Daarnaast bevat API Management een ingebouwde groep Administrators in het gebruikers systeem van de API Management. Groepen in API Management beheersen de zicht baarheid van Api's in de ontwikkelaars Portal en de leden van de groep Administrators kunnen alle Api's zien.

Volg de aanbevelingen van Azure Security Center voor het beheer en onderhoud van beheerders accounts.

* [Op rollen gebaseerd toegangsbeheer gebruiken in API Management](https://docs.microsoft.com/azure/api-management/api-management-role-based-access-control)

* [Een lijst met gebruikers verkrijgen onder een Azure API Management-exemplaar](https://docs.microsoft.com/powershell/module/az.apimanagement/get-azapimanagementuser?view=azps-3.1.0)

* [Een lijst weer geven met gebruikers die zijn toegewezen aan een directory-rol in azure AD met Power shell](https://docs.microsoft.com/powershell/module/az.resources/get-azroleassignment?view=azps-3.7.0)

* [Een directory-roldefinitie ophalen in azure AD met Power shell](https://docs.microsoft.com/powershell/module/az.resources/get-azroledefinition?view=azps-3.7.0)

* [Meer informatie over identiteits-en toegangs aanbevelingen van Azure Security Center](https://docs.microsoft.com/azure/security-center/recommendations-reference#recs-identity)

**Azure Security Center bewaking**: Ja

**Verantwoordelijkheid**: klant

### <a name="32-change-default-passwords-where-applicable"></a>3,2: standaard wachtwoorden wijzigen indien van toepassing

**Hulp**: Azure API Management heeft niet het concept van standaard wachtwoorden/-sleutel.

Azure API Management-abonnementen, wat een manier is om de toegang tot Api's te beveiligen, zijn echter wel een paar gegenereerde abonnements sleutels. Klanten kunnen deze abonnements sleutels op elk gewenst moment opnieuw genereren.

* [Informatie over Azure API Management-abonnementen](https://docs.microsoft.com/azure/api-management/api-management-subscriptions)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: speciale beheerders accounts gebruiken

**Richt lijnen**: Maak standaard procedures voor het gebruik van specifieke beheerders accounts. Gebruik Azure Security Center identiteits-en toegangs beheer om het aantal beheerders accounts te bewaken.

Daarnaast kunt u aanbevelingen van Azure Security Center of ingebouwde Azure-beleids regels gebruiken om u te helpen bij het bijhouden van specifieke beheerders accounts, zoals:
- Er moet meer dan één eigenaar aan uw abonnement zijn toegewezen
- Afgeschafte accounts met eigenaars machtigingen moeten worden verwijderd uit uw abonnement
- Externe accounts met eigenaars machtigingen moeten worden verwijderd uit uw abonnement

* [Azure Security Center gebruiken om identiteit en toegang te bewaken (preview)](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

* [Azure Policy gebruiken](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Security Center bewaking**: Ja

**Verantwoordelijkheid**: klant

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: eenmalige aanmelding (SSO) met Azure Active Directory gebruiken

**Hulp**: Azure API Management kan worden geconfigureerd om gebruik te maken van Azure Active Directory als een id-provider voor het verifiëren van gebruikers op de ontwikkelaars Portal om te profiteren van de SSO-mogelijkheden van Azure AD. Nadat de gebruikers zijn geconfigureerd, kunnen ze het out-of-the-box-aanmeldings proces volgen door eerst te verifiëren via Azure AD en vervolgens het aanmeldings proces op de portal uit te voeren nadat het is geverifieerd.

* [Ontwikkelaarsaccounts autoriseren met behulp van Azure Active Directory in Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-aad)

U kunt ook het aanmeldings-en aanmeldings proces verder aanpassen via delegering. Met delegering kunt u uw bestaande website gebruiken voor het afhandelen van ontwikkel aars die zich aanmelden/registreren en abonneren op producten, in tegens telling tot het gebruik van de ingebouwde functionaliteit in de ontwikkelaars Portal. Hiermee kan uw website eigenaar worden van de gebruikers gegevens en kan de validatie van deze stappen op een aangepaste manier worden uitgevoerd.

* [Gebruikers registratie en product abonnement delegeren](https://docs.microsoft.com/azure/api-management/api-management-howto-setup-delegation)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: multi-factor Authentication gebruiken voor alle op Azure Active Directory gebaseerde toegang

**Hulp**: Schakel Azure Active Directory (AD) multi-factor Authentication (MFA) in en volg Azure Security Center aanbevelingen voor identiteits-en toegangs beheer.

* [MFA inschakelen in azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

* [Identiteit en toegang bewaken in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Azure Security Center bewaking**: Ja

**Verantwoordelijkheid**: klant

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: gebruik speciale machines (privileged Access workstations) voor alle beheer taken

**Hulp**: gebruik paw (privileged Access workstations) met multi-factor Authentication (MFA) die zijn geconfigureerd voor aanmelding bij en configureren van Azure-resources.

* [Meer informatie over privileged Access workstations](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)

* [MFA inschakelen in azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3,7: logboek en waarschuwing voor verdachte activiteiten van beheerders accounts

**Hulp**: gebruik Azure Active Directory (AD) PRIVILEGED Identity Management (PIM) voor het genereren van Logboeken en waarschuwingen wanneer verdachte of onveilige activiteiten in de omgeving worden uitgevoerd.

Daarnaast kunt u met Azure AD-risico detectie waarschuwingen en rapporten bekijken over Risk ante gebruikers gedrag.

* [Privileged Identity Management implementeren (PIM)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan)

* [Meer informatie over Azure AD-risico detectie](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events)

**Azure Security Center bewaking**: Ja

**Verantwoordelijkheid**: klant

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure-resources alleen beheren vanaf goedgekeurde locaties

**Hulp**: gebruik benoemde locaties voor voorwaardelijke toegang om alleen toegang toe te staan tot de Azure Portal vanuit specifieke logische groepen met IP-adresbereiken of landen/regio's.

* [Benoemde locaties configureren in azure](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory gebruiken

**Richt lijnen**: als dat mogelijk is, kunt u Azure AD gebruiken als het centrale verificatie-en autorisatie systeem. AAD beveiligt gegevens door gebruik te maken van sterke versleuteling voor gegevens in rust en tijdens de overdracht. Azure AD bevat ook zouten, hashes en veilige gebruikers referenties.

Configureer uw Azure API Management-ontwikkelaars Portal om ontwikkelaars accounts te verifiëren met behulp van Azure Active Directory.

Configureer uw Azure API Management-exemplaar om uw Api's te beveiligen met behulp van het OAuth 2,0-protocol met Azure Active Directory (AD).

* [Ontwikkelaars accounts autoriseren met behulp van Azure Active Directory in azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-aad)

* [Een API beveiligen met behulp van OAuth 2,0 met Azure Active Directory en API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad)

* [Een AAD-exemplaar maken en configureren](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: regel matig gebruikers toegang controleren en afstemmen

**Hulp**: Azure Active Directory biedt logboeken waarmee u verlopen accounts kunt detecteren. Klanten kunnen Azure Identity Access revisies gebruiken om groepslid maatschappen, toegang tot bedrijfs toepassingen en roltoewijzingen op efficiënte wijze te beheren. Gebruikers toegang kan regel matig worden gecontroleerd om ervoor te zorgen dat alleen de juiste gebruikers toegang blijven houden.

Klanten kunnen de inventaris van API Management gebruikers accounts onderhouden en zo nodig de toegang afstemmen. In API Management zijn ontwikkel aars de gebruikers van de Api's die beschikbaar zijn gemaakt met API Management. Nieuw gemaakte ontwikkelaars accounts zijn standaard actief en zijn gekoppeld aan de groep ontwikkel aars. Ontwikkelaars accounts die zich in een actieve status bevinden, kunnen worden gebruikt voor toegang tot alle Api's waarvoor ze abonnementen hebben.

Beheerders kunnen aangepaste groepen maken of gebruikmaken van externe groepen in gekoppelde Azure Active Directory-tenants. Aangepaste en externe groepen kunnen naast systeemgroepen worden gebruikt om ontwikkelaars zichtbaarheid van en toegang tot API-producten te geven.

* [Gebruikersaccounts in Azure API Management beheren](https://docs.microsoft.com/azure/api-management/api-management-howto-create-or-invite-developers)

* [Een lijst met API Management gebruikers ophalen](https://docs.microsoft.com/powershell/module/az.apimanagement/get-azapimanagementuser?view=azps-3.1.0)

* [Groepen maken en gebruiken voor beheer van ontwikkelaarsaccounts in Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-create-groups)

* [Beoordelingen over Azure Identity Access gebruiken](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

**Azure Security Center bewaking**: Ja

**Verantwoordelijkheid**: klant

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3,11: controle pogingen om toegang te krijgen tot gedeactiveerde accounts

**Hulp**: Configureer uw Azure API Management-exemplaar om ontwikkelaars accounts te verifiëren met behulp van Azure Active Directory als een id-provider in azure API management.

Configureer uw Azure API Management-exemplaar om uw Api's te beveiligen met behulp van het OAuth 2,0-protocol met Azure Active Directory (AD).

Configureer het JWT-validatie beleid voor inkomende API-aanvragen om het bestaan en de geldigheid van een geldig token af te dwingen.

Diagnostische instellingen voor Azure AD-gebruikers accounts maken en de audit logboeken en aanmeldings logboeken verzenden naar een Log Analytics-werk ruimte. Gewenste waarschuwingen configureren in Log Analytics. Daarnaast kunt u de Log Analytics-werk ruimte onboarden naar Azure Sentinel of een SIEM van derden.

Configureer geavanceerde bewaking met API Management met behulp van het `log-to-eventhub` beleid, leg eventuele aanvullende context informatie vast die vereist is voor de analyse van de beveiliging en verzend een bericht naar een Siem van Azure Sentinel of van derden.

* [Ontwikkelaars accounts autoriseren met behulp van Azure Active Directory in azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-aad)

* [Een API beveiligen met behulp van OAuth 2,0 met Azure Active Directory en API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad)

* [API Management access restriction policies](https://docs.microsoft.com/azure/api-management/api-management-access-restriction-policies) (Beleid voor toegangsbeperking API Management)

* [Azure AD-logboeken integreren in Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

* [Azure-Sentinel aan de trein](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

* [Geavanceerde bewaking van Api's](https://docs.microsoft.com/azure/api-management/api-management-log-to-eventhub-sample)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: waarschuwing voor de afwijking van het aanmeldings gedrag van accounts

**Richt lijnen**: voor de afwijking van het aanmeldings gedrag van accounts op het besturings vlak (de Azure Portal), gebruikt u Azure Active Directory (AD) identiteits beveiliging en risico detectie functies voor het configureren van automatische antwoorden op gedetecteerde verdachte acties die betrekking hebben op gebruikers identiteiten. U kunt ook gegevens opnemen in azure Sentinel voor verder onderzoek.

* [Risk ante aanmeldingen voor Azure AD weer geven](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

* [Risico beleid voor identiteits beveiliging configureren en inschakelen](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies)

* [Azure-Sentinel onboarden](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: micro soft biedt toegang tot relevante klant gegevens tijdens ondersteunings scenario's

**Hulp**: momenteel niet beschikbaar; Klanten-lockbox wordt momenteel niet ondersteund voor Azure API Management.

* [Lijst met door Klanten-lockbox ondersteunde services](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

## <a name="data-protection"></a>Gegevensbeveiliging

*Zie [beveiligings beheer: gegevens beveiliging](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection)voor meer informatie.*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: een inventaris van gevoelige informatie onderhouden

**Hulp**: Tags gebruiken om Azure-resources te helpen bij het bijhouden of verwerken van gevoelige informatie.

* [Tags maken en gebruiken](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: systemen isoleren die gevoelige informatie opslaan of verwerken

**Richt lijnen**: afzonderlijke abonnementen en/of beheer groepen implementeren voor ontwikkeling, testen en productie. Azure API Management-exemplaren moeten worden gescheiden door de/subnet van het virtuele netwerk (VNet) en worden gelabeld.

* [Aanvullende Azure-abonnementen maken](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Beheergroepen maken](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Tags maken en gebruiken](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

* [Azure API Management gebruiken met virtuele netwerken](https://docs.microsoft.com/azure/api-management/api-management-using-with-vnet)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: niet-geautoriseerde overdracht van gevoelige gegevens controleren en blok keren

**Hulp**: momenteel niet beschikbaar; de functies voor gegevens identificatie, classificatie en verlies preventie zijn momenteel niet beschikbaar voor Azure API Management.

Micro soft beheert de onderliggende infra structuur voor Azure API Management en heeft strikte controles geïmplementeerd om verlies of bloot stelling van klant gegevens te voor komen.

* [Informatie over beveiliging van klanten in azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Security Center bewaking**: momenteel niet beschikbaar

**Verantwoordelijkheid**: klant

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: alle gevoelige gegevens in de overdracht versleutelen

**Hulp**: beheer vlak aanroepen worden gedaan via Azure Resource Manager via TLS. Er is een geldig JSON-webtoken (JWT) vereist. Aanroepen voor gegevens vlak kunnen worden beveiligd met TLS en een van de ondersteunde verificatie mechanismen (bijvoorbeeld client certificaat of JWT).

* [Gegevens beveiliging in azure API Management begrijpen](https://docs.microsoft.com/azure/api-management/api-management-security-controls#data-protection)

* [TLS-instellingen in azure API Management beheren](https://docs.microsoft.com/azure/api-management/api-management-howto-manage-protocols-ciphers)

* [Api's in azure API Management beveiligen met Azure Active Directory](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad)

* [Api's in azure API Management beveiligen met Azure Active Directory B2C](https://docs.microsoft.com/azure/api-management/howto-protect-backend-frontend-azure-ad-b2c)

**Azure Security Center bewaking**: Ja

**Verantwoordelijkheid**: gedeeld

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: een actief detectie hulpprogramma gebruiken om gevoelige gegevens te identificeren

**Hulp**: momenteel niet beschikbaar; de functies voor gegevens identificatie, classificatie en verlies preventie zijn momenteel niet beschikbaar voor Azure API Management. Codeer Azure API Management Services die mogelijk gevoelige informatie verwerken als zodanig en implementeer oplossingen van derden als dat nodig is voor nalevings doeleinden.

Voor het onderliggende platform dat door micro soft wordt beheerd, behandelt micro soft alle inhoud van de klant als gevoelig en gaat u naar een fantastische lengte om te beschermen tegen verlies en bloot stelling van klant gegevens. Om ervoor te zorgen dat klant gegevens binnen Azure veilig blijven, heeft micro soft een reeks robuuste besturings elementen en mogelijkheden voor gegevens bescherming geïmplementeerd en onderhouden.

* [Informatie over beveiliging van klanten in azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Security Center bewaking**: momenteel niet beschikbaar

**Verantwoordelijkheid**: klant

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: Azure RBAC gebruiken om de toegang tot resources te beheren

**Hulp**: gebruik op rollen gebaseerd toegangs beheer voor het beheren van toegang tot Azure API management. Azure API Management is afhankelijk van de op Azure Role gebaseerde Access Control (RBAC) om nauw keurig toegangs beheer mogelijk te maken voor API Management Services en entiteiten (bijvoorbeeld Api's en beleids regels).

* [Op rollen gebaseerd toegangsbeheer gebruiken in API Management](https://docs.microsoft.com/azure/api-management/api-management-role-based-access-control)

**Azure Security Center bewaking**: momenteel niet beschikbaar

**Verantwoordelijkheid**: klant

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: voor komen dat gegevens verlies op basis van host wordt gebruikt voor het afdwingen van toegangs beheer

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor reken resources.

Micro soft beheert de onderliggende infra structuur voor Azure API Management en heeft strikte controles geïmplementeerd om verlies of bloot stelling van klant gegevens te voor komen.

* [Informatie over beveiliging van klanten in azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: gevoelige informatie op rest versleutelen

**Richt lijnen**: gevoelige gegevens zoals certificaten, sleutels en geheime benoemde waarden worden versleuteld met Service-beheerde exemplaren per service-exemplaar. Alle versleutelings sleutels zijn per service-exemplaar en worden beheerd door de service.

* [Meer informatie over gegevens beveiliging/versleuteling in rust met Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-security-controls#data-protection)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: micro soft

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: wijzigingen in essentiële Azure-resources vastleggen en waarschuwen

**Hulp**: gebruik Azure monitor met het Azure-activiteiten logboek om waarschuwingen te maken voor wanneer wijzigingen worden aangebracht in productie-Azure functions apps, evenals andere kritieke of gerelateerde resources.

* [Waarschuwingen maken voor gebeurtenissen in het Azure-activiteiten logboek](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

* [Azure Monitor en Azure-activiteiten logboek gebruiken in azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-use-azure-monitor)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

## <a name="vulnerability-management"></a>Beheer van beveiligingsproblemen

*Zie [beveiligings beheer: beveiligingslek beheer](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management)voor meer informatie.*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: automatische hulpprogram ma's voor het scannen van beveiligings problemen uitvoeren

**Hulp**: momenteel niet beschikbaar; de evaluatie van beveiligings problemen in Azure Security Center is momenteel niet beschikbaar voor Azure API Management.

Onderliggend platform gescand en bijgewerkt door micro soft. Bekijk beveiligings controles die beschikbaar zijn om beveiligings problemen met betrekking tot de service configuratie te verminderen.

* [Informatie over beveiligings besturings elementen die beschikbaar zijn voor Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-security-controls)

**Azure Security Center bewaking**: momenteel niet beschikbaar

**Verantwoordelijkheid**: klant

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: geautomatiseerde oplossing voor patch beheer voor besturings systemen implementeren

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor reken resources.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5,3: Implementeer een geautomatiseerde oplossing voor software patch beheer van derden

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor reken resources.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: vergelijken van back-to-back-problemen

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor reken resources.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: een risico classificatie proces gebruiken om prioriteit te geven aan het herstel van ontdekte beveiligings problemen

**Hulp**: momenteel niet beschikbaar; de evaluatie van beveiligings problemen in Azure Security Center is momenteel niet beschikbaar voor Azure API Management.

Onderliggend platform gescand en bijgewerkt door micro soft. Klant voor het beoordelen van de beschik bare beveiligings controles om beveiligings problemen met betrekking tot de service configuratie te verminderen.

* [Informatie over beveiligings besturings elementen die beschikbaar zijn voor Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-security-controls)

**Azure Security Center bewaking**: momenteel niet beschikbaar

**Verantwoordelijkheid**: klant

## <a name="inventory-and-asset-management"></a>Inventarisatie en asset-management

*Zie voor meer informatie [beveiligings beheer: inventarisatie en activa beheer](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-azure-asset-discovery"></a>6,1: Azure Asset Discovery gebruiken

**Hulp**: Azure resource Graph gebruiken voor het opvragen/detecteren van alle resources (zoals compute, opslag, netwerk, poorten en protocollen enz.) binnen uw abonnement (en). Zorg ervoor dat de juiste (Lees-) machtigingen voor uw Tenant en alle Azure-abonnementen en resources in uw abonnementen inventariseren.

Hoewel klassieke Azure-resources kunnen worden gedetecteerd via resource grafiek, is het raadzaam om Azure Resource Manager resources te maken en te gebruiken.

* [Query's maken met Azure resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

* [Uw Azure-abonnementen weer geven](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

* [Meer informatie over Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/overview)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="62-maintain-asset-metadata"></a>6,2: meta gegevens van activa onderhouden

**Richt lijnen**: Tags Toep assen op Azure-resources die meta gegevens geven om ze logisch in een taxonomie te organiseren.

* [Tags maken en gebruiken](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: niet-geautoriseerde Azure-resources verwijderen

**Richt lijnen**: Gebruik labels, beheer groepen en afzonderlijke abonnementen, waar nodig, om Azure-resources te organiseren en bij te houden. Sluit de inventaris regel matig af en zorg ervoor dat niet-geautoriseerde resources tijdig worden verwijderd uit het abonnement.

Daarnaast kunt u met Azure Policy beperkingen opleggen aan het type resources dat kan worden gemaakt in klant abonnement (en) met de volgende ingebouwde beleids definities:
- Niet toegestane brontypen
- Toegestane brontypen

* [Aanvullende Azure-abonnementen maken](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Beheergroepen maken](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Tags maken en gebruiken](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6,4: een inventaris van goedgekeurde Azure-resources en software titels onderhouden

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor reken resources.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: monitor voor niet-goedgekeurde Azure-resources

**Hulp: gebruik**Azure Policy om beperkingen toe te voegen voor het type resources dat kan worden gemaakt in uw abonnement (en) met de volgende ingebouwde beleids definities:
- Niet toegestane brontypen
- Toegestane brontypen

Gebruik Azure resource Graph voor het opvragen/detecteren van resources binnen hun abonnement (en). Zorg ervoor dat alle Azure-resources die aanwezig zijn in de omgeving, zijn goedgekeurd.

* [Azure Policy configureren en beheren](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Query's maken met Azure Graph](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: monitor voor niet-goedgekeurde software toepassingen binnen reken resources

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor reken resources.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: niet-goedgekeurde Azure-resources en software toepassingen verwijderen

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor reken resources.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="68-use-only-approved-applications"></a>6,8: alleen goedgekeurde toepassingen gebruiken

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor reken resources.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="69-use-only-approved-azure-services"></a>6,9: alleen goedgekeurde Azure-Services gebruiken

**Hulp: gebruik**Azure Policy om beperkingen toe te voegen voor het type resources dat kan worden gemaakt in klant abonnement (en) met de volgende ingebouwde beleids definities:
- Niet toegestane brontypen
- Toegestane brontypen

* [Azure Policy configureren en beheren](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Een specifiek resource type weigeren met Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="610-implement-approved-application-list"></a>6,10: lijst met goedgekeurde toepassingen implementeren

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor reken resources.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="611-limit-users-ability-to-interact-with-azure-resources-manager-via-scripts"></a>6,11: de mogelijkheid van gebruikers om te communiceren met Azure Resource Manager via scripts beperken

**Hulp**bij het configureren van voorwaardelijke toegang van Azure om gebruikers de mogelijkheid te bieden om te communiceren met Azure Resource Manager door ' blok toegang ' te configureren voor de app Microsoft Azure management.

* [Voorwaardelijke toegang configureren om de toegang tot Azure Resource Manager te blok keren](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

* [Toegangs beheer op basis van rollen in azure API Management](https://docs.microsoft.com/azure/api-management/api-management-role-based-access-control)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: de mogelijkheid van gebruikers om scripts uit te voeren binnen reken bronnen beperken

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor reken resources.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: toepassingen met een hoog risico fysiek of logisch scheiden

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor webtoepassingen die worden uitgevoerd op Azure App Service of reken bronnen.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

## <a name="secure-configuration"></a>Veilige configuratie

*Zie [beveiligings beheer: beveiligde configuratie](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration)voor meer informatie.*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: veilige configuraties instellen voor alle Azure-resources

**Hulp**: Definieer en implementeer standaard beveiligings configuraties voor uw Azure API Management-service met Azure Policy. Gebruik Azure Policy aliassen in de naam ruimte ' micro soft. ApiManagement ' om aangepaste beleids regels te maken om de configuratie van uw Azure API Management-Services te controleren of af te dwingen.

* [Beschik bare Azure Policy aliassen weer geven](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

* [Azure Policy configureren en beheren](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: veilige configuraties van besturings systemen instellen

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor reken resources.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: Beveilig Azure-resource configuraties onderhouden

**Hulp**: Definieer en implementeer standaard beveiligings configuraties voor uw Azure API Management-services met Azure Policy. Gebruik Azure Policy aliassen in de naam ruimte ' micro soft. ApiManagement ' om aangepaste beleids regels te maken om de configuratie van Azure API Management-exemplaren te controleren of af te dwingen. Gebruik Azure Policy [deny] en [implementeren indien niet aanwezig] voor het afdwingen van beveiligde instellingen voor uw Azure-resources.

* [Azure Policy configureren en beheren](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Policy effecten begrijpen](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: veilige configuraties van besturings systemen onderhouden

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor reken resources.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: gedeeld

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: de configuratie van Azure-resources veilig opslaan

**Richt lijnen**: als u aangepaste definities van Azure-beleid gebruikt, kunt u Azure DevOps of Azure opslag plaatsen gebruiken om uw Azure API Management service-configuratie veilig op te slaan en te beheren.

* [Bestanden opslaan in azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Documentatie voor Azure opslag plaatsen](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

* [Meer informatie over de Azure API Management DevOps Resource Kit](https://docs.microsoft.com/azure/api-management/api-management-security-controls#configuration-management)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: aangepaste installatie kopieën van een besturings systeem veilig opslaan

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor reken resources.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="77-deploy-system-configuration-management-tools"></a>7,7: hulpprogram ma's voor het beheer van systeem configuratie implementeren

**Hulp**: Definieer en implementeer standaard beveiligings configuraties voor uw Azure API Management-services met Azure Policy. Gebruik Azure Policy aliassen in de naam ruimte ' micro soft. ApiManagement ' om aangepaste beleids regels te maken om de configuratie van Azure API Management-exemplaren te controleren of af te dwingen. Gebruik Azure Policy [deny] en [implementeren indien niet aanwezig] voor het afdwingen van beveiligde instellingen voor uw Azure-resources.

* [Azure Policy configureren en beheren](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Policy effecten begrijpen](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7,8: hulpprogram ma's voor het beheer van systeem configuratie implementeren voor besturings systemen

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor reken resources.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9: geautomatiseerde configuratie bewaking voor Azure-Services implementeren

**Hulp**: gebruik de Azure API Management DevOps Resource Kit om configuratie beheer voor Azure API Management uit te voeren.

Daarnaast definieert en implementeert u standaard beveiligings configuraties voor uw Azure API Management-Services met Azure Policy. Gebruik Azure Policy aliassen in de naam ruimte ' micro soft. ApiManagement ' om aangepaste beleids regels te maken om de configuratie van Azure API Management-exemplaren te controleren of af te dwingen. Gebruik Azure Policy [deny] en [implementeren indien niet aanwezig] voor het afdwingen van beveiligde instellingen voor uw Azure-resources.

* [Meer informatie over de Azure API Management DevOps Resource Kit](https://docs.microsoft.com/azure/api-management/api-management-security-controls#configuration-management)

* [Azure Policy configureren en beheren](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Azure Policy effecten begrijpen](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: geautomatiseerde configuratie bewaking voor besturings systemen implementeren

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor reken resources.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure-geheimen veilig beheren

**Hulp**: gebruik Key Vault voor het beheren van certificaten en stel ze in op automatisch draaien. Als u Azure Key Vault gebruikt voor het beheren van het SSL-certificaat van het aangepaste domein, moet u ervoor zorgen dat het certificaat wordt ingevoegd in Key Vault als een certificaat, niet als een geheim.

* [Aangepaste domein namen instellen met richt lijnen voor het draaien van Key Vault sleutels](https://docs.microsoft.com/azure/api-management/configure-custom-domain)

**Azure Security Center bewaking**: Ja

**Verantwoordelijkheid**: micro soft

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: identiteiten veilig en automatisch beheren

**Hulp**: Managed Service Identity gegenereerd door Azure Active Directory (AD) om uw API Management-exemplaar eenvoudig en veilig toegang te geven tot andere met Azure AD beveiligde resources, zoals Azure Key Vault.

* [Een beheerde identiteit voor een API Management-exemplaar maken](https://docs.microsoft.com/azure/api-management/api-management-howto-use-managed-service-identity)

* [Beleid voor verificatie met beheerde identiteit](https://docs.microsoft.com/azure/api-management/api-management-authentication-policies#ManagedIdentity)

**Azure Security Center bewaking**: Ja

**Verantwoordelijkheid**: klant

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: onbedoelde referentie blootstelling elimineren

**Richt lijnen**: referentie scanner implementeren om referenties in code te identificeren. Referentie scanner stimuleert ook het verplaatsen van gedetecteerde referenties naar veiliger locaties, zoals Azure Key Vault.

* [Referentie scanner instellen](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

## <a name="malware-defense"></a>Beveiliging tegen malware

*Zie [beveiligings beheer: verdediging tegen malware](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense)voor meer informatie.*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: centraal beheerde anti-malware-software gebruiken

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor reken resources.

Micro soft anti-malware is ingeschakeld op de onderliggende host die ondersteuning biedt voor Azure-Services (bijvoorbeeld Azure API Management), maar wordt niet uitgevoerd op de inhoud van de klant.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: scan bestanden die moeten worden geüpload naar niet-reken resources van Azure

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor niet-reken resources die zijn ontworpen om gegevens op te slaan.

Micro soft anti-malware is ingeschakeld op de onderliggende host die ondersteuning biedt voor Azure-Services (bijvoorbeeld Azure API Management), maar wordt niet uitgevoerd op de inhoud van de klant.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: controleren of anti-malware-software en hand tekeningen zijn bijgewerkt

**Richt lijnen**: niet van toepassing; deze aanbeveling is bedoeld voor niet-reken resources die zijn ontworpen om gegevens op te slaan.

Micro soft anti-malware is ingeschakeld op de onderliggende host die ondersteuning biedt voor Azure-Services (bijvoorbeeld Azure API Management), maar wordt niet uitgevoerd op de inhoud van de klant.

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

## <a name="data-recovery"></a>Gegevensherstel

*Zie [beveiligings beheer: gegevens herstel](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery)voor meer informatie.*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: controleren op regel matige automatische back-ups

**Richt lijnen**: door uw api's te publiceren en beheren via Azure API Management, profiteert u van de fout tolerantie en infrastructuur mogelijkheden die u op een andere manier hand matig ontwerpt, implementeert en beheert. API Management ondersteunt implementatie met meerdere regio's, waardoor het gegevens vlak ondoordringbaar is voor regionale storingen zonder dat er operationele overhead hoeft te worden toegevoegd.

De functies voor het maken en herstellen van back-ups van API Management bieden de benodigde bouw stenen voor het implementeren van een nood herstel strategie. Back-up-en herstel bewerkingen kunnen hand matig of automatisch worden uitgevoerd.

* [API Management data-vlak implementeren in meerdere regio's](https://docs.microsoft.com/azure/api-management/api-management-howto-deploy-multi-region)

* [Noodherstel implementeren met back-up en herstellen van services in Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-disaster-recovery-backup-restore#calling-the-backup-and-restore-operations)

* [De back-upbewerking van API Management aanroepen](/rest/api/apimanagement/2019-12-01/apimanagementservice/backup)

* [De herstel bewerking van API Management aanroepen](/rest/api/apimanagement/2019-12-01/apimanagementservice/restore)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: volledige back-ups van het systeem uitvoeren en een back-up maken van een door de klant beheerde sleutels

**Richt lijnen**: back-up-en herstel bewerkingen van Azure API Management volledige back-up en herstel van het systeem uitvoeren.

Beheerde identiteiten kunnen worden gebruikt voor het verkrijgen van certificaten van Azure Key Vault voor API Management aangepaste domein namen. Back-ups maken van certificaten die worden opgeslagen in Azure Key Vault.

* [Noodherstel implementeren met back-up en herstellen van services in Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-disaster-recovery-backup-restore#calling-the-backup-and-restore-operations)

* [Back-up maken van Azure Key Vault certificaten](https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultcertificate?view=azurermps-6.13.0)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: alle back-ups valideren, inclusief door de klant beheerde sleutels

**Hulp**bij het valideren van back-ups door het herstellen van de service en certificaten van back-ups te testen.

* [De herstel bewerking van API Management aanroepen](https://docs.microsoft.com/rest/api/apimanagement/2019-12-01/apimanagementservice/restore)

* [Azure Key Vault certificaten herstellen](https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultcertificate?view=azurermps-6.13.0)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: zorg voor de bescherming van back-ups en door de klant beheerde sleutels

**Hulp**: Azure API Management schrijft back-ups naar Azure Storage accounts van de klant. Volg de aanbevelingen van Azure Storage beveiliging om uw back-up te beveiligen.

* [Noodherstel implementeren met back-up en herstellen van services in Azure API Management](https://docs.microsoft.com/azure/api-management/api-management-howto-disaster-recovery-backup-restore#calling-the-backup-and-restore-operations)

* [Beveiligings aanbeveling voor Blob Storage](https://docs.microsoft.com/azure/storage/blobs/security-recommendations)

Schakel zacht verwijderen in Key Vault in om sleutels te beschermen tegen onbedoelde of schadelijke verwijdering.

* [Zacht verwijderen inschakelen in Key Vault](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

## <a name="incident-response"></a>Reageren op incidenten

*Zie voor meer informatie [beveiligings beheer: reactie op incidenten](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10,1: een hand leiding voor reactie op incidenten maken

**Hulp**: een antwoord gids voor incidenten maken voor uw organisatie. Zorg ervoor dat er schriftelijke incidenten abonnementen zijn die alle werk rollen definiëren, evenals fasen van incident handling/management van detectie tot een beoordeling van het incident.

* [Richt lijnen voor het bouwen van uw eigen beveiligings incident antwoord proces](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Micro soft Security Response Center anatomie van een incident](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

* [De verwerkings handleiding voor het computer beveiligings incident van het NIST gebruiken om u te helpen bij het maken van uw eigen reactie plan voor incidenten](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: een beoordelings procedure voor incidenten en prioriteits procedures maken

**Hulp**: Security Center wijst aan elke waarschuwing een Ernst toe om u te helpen bepalen welke waarschuwingen het eerst moeten worden onderzocht. De ernst is gebaseerd op de manier waarop vertrouwen Security Center is in de zoek actie of het analyse programma dat wordt gebruikt om de waarschuwing te geven, evenals het betrouwbaarheids niveau dat er schadelijke bedoelingen zijn achter de activiteit die tot de waarschuwing heeft geleid.

Daarnaast kunt u ook duidelijk abonnementen markeren (voor bijvoorbeeld productie, niet-productie) met behulp van tags en maak een naamgevings systeem om Azure-resources duidelijk te identificeren en te categoriseren, met name voor de verwerking van gevoelige gegevens. Het is uw verantwoordelijkheid om prioriteit te geven aan het herstel van waarschuwingen op basis van de ernst van de Azure-resources en-omgeving waar het incident heeft plaatsgevonden.

* [Beveiligingswaarschuwingen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-overview)

* [Tags gebruiken om uw Azure-resources te organiseren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Azure Security Center bewaking**: Ja

**Verantwoordelijkheid**: klant

### <a name="103-test-security-response-procedures"></a>10,3: procedures voor beveiligings antwoorden testen

**Richt lijnen**: oefent oefeningen uit om de respons mogelijkheden van uw systeem te testen op een reguliere uitgebracht om uw Azure-resources te beschermen. Identificeer zwakke punten en tussen ruimten en wijzig zo nodig het schema.

* [Publicatie van het NIST-hand leiding voor test-, trainings-en oefen Programma's voor IT-plannen en-mogelijkheden](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: contact gegevens van het beveiligings incident opgeven en waarschuwings meldingen configureren voor beveiligings incidenten

**Hulp**: contact gegevens van beveiligings incidenten worden door micro soft gebruikt om contact met u op te nemen als het micro soft Security Response Center (MSRC) detecteert dat uw gegevens zijn geopend door een onrecht matige of niet-gemachtigde partij. Bekijk incidenten na het feit om te controleren of de problemen zijn opgelost.

* [De Azure Security Center Security-contact persoon instellen](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details)

**Azure Security Center bewaking**: Ja

**Verantwoordelijkheid**: klant

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: beveiligings waarschuwingen opnemen in uw reactie systeem van uw incident

**Hulp**: exporteer uw Azure Security Center waarschuwingen en aanbevelingen met behulp van de functie continue export om Risico's voor Azure-resources te identificeren. Met doorlopend exporteren kunt u waarschuwingen en aanbevelingen hand matig of op een doorlopende manier exporteren. U kunt de Azure Security Center Data Connector gebruiken om de waarschuwingen naar Azure Sentinel te streamen.

* [Continue export configureren](https://docs.microsoft.com/azure/security-center/continuous-export)

* [Waarschuwingen streamen naar Azure Sentinel](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: de reactie op beveiligings waarschuwingen automatiseren

**Hulp**: gebruik de automatiserings functie voor werk stromen in azure Security Center om automatisch reacties te activeren via ' Logic apps ' in beveiligings waarschuwingen en aanbevelingen.

* [Werk stroom automatisering en Logic Apps configureren](https://docs.microsoft.com/azure/security-center/workflow-automation)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: klant

## <a name="penetration-tests-and-red-team-exercises"></a>Penetratietests en Red Team-oefeningen

*Zie voor meer informatie [Security Control: Indringings tests en Red team-oefeningen](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-to-remediate-all-critical-security-findings-within-60-days"></a>11,1: een regel matige indringings test van uw Azure-resources uitvoeren en ervoor zorgen dat alle essentiële beveiligings resultaten binnen 60 dagen worden hersteld

**Richt lijnen**: * [Volg de micro soft-regels voor betrokkenheid om ervoor te zorgen dat de indringings tests niet worden geschonden door het micro soft-beleid](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1.)

* [U vindt hier meer informatie over de strategie van micro soft en de uitvoering van Red Teaming en live site indringings tests ten opzichte van micro soft Managed Cloud Infrastructure, services en toepassingen.](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Security Center bewaking**: niet van toepassing

**Verantwoordelijkheid**: gedeeld

## <a name="next-steps"></a>Volgende stappen

- Zie de [Azure Security-Bench Mark](https://docs.microsoft.com/azure/security/benchmarks/overview)
- Meer informatie over [Azure-beveiligings basislijnen](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)
