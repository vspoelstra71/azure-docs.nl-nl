---
title: Kennismaking met de gebruikersinterface van Azure IoT Central | Microsoft Docs
description: Maak kennis met de belangrijkste gebieden van de Azure IoT Central gebruikers interface die u gebruikt voor het maken, beheren en gebruiken van uw IoT-oplossing.
author: lmasieri
ms.author: lmasieri
ms.date: 12/09/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: corywink
ms.openlocfilehash: b905b1e86810b25c4c94072d6cd414b993e2a883
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "77426007"
---
# <a name="take-a-tour-of-the-azure-iot-central-ui"></a>Kennismaking met de gebruikersinterface van Azure IoT Central



In dit artikel maakt u kennis met de gebruikersinterface van Microsoft Azure IoT Central. U kunt de gebruikersinterface gebruiken om een Azure IoT Central-oplossing te maken en de daarmee verbonden apparaten te beheren en gebruiken.

Als _opbouw functie voor oplossingen_gebruikt u de Azure IOT Central-gebruikers interface om uw Azure IOT Central-oplossing te definiëren. U kunt de gebruikersinterface gebruiken om:

* De typen apparaten te definiëren die met uw oplossing verbinding maken.
* De regels en acties voor uw apparaten te configureren. 
* De gebruikersinterface aan te passen voor een _operator_ die uw oplossing gebruikt.

Als _operator_ gebruikt u de gebruikersinterface van Azure IoT Central om uw Azure IoT Central-oplossing te beheren. U kunt de gebruikersinterface gebruiken om:

* Uw apparaten te controleren.
* Uw apparaten te configureren.
* Problemen met uw apparaten op te lossen.
* Nieuwe apparaten inrichten.

## <a name="iot-central-homepage"></a>Start pagina IoT Central

Op de pagina [IOT Central Home](https://aka.ms/iotcentral-get-started) page vindt u meer informatie over het laatste nieuws en de beschik bare functies op IOT Central, het maken van nieuwe toepassingen en het weer geven en starten van uw bestaande toepassing.

> [!div class="mx-imgBorder"]
> ![Start pagina IoT Central](media/overview-iot-central-tour/iot-central-homepage-pnp.png)

### <a name="create-an-application"></a>Een app maken

In de sectie build kunt u bladeren door de lijst met branchespecifieke IoT Central sjablonen om snel aan de slag te gaan, of een volledig nieuwe app-sjabloon te gebruiken.  
> [!div class="mx-imgBorder"]
> ![Pagina bouwen IoT Central](media/overview-iot-central-tour/iot-central-build-pnp.png)

Zie de Snelstartgids [een Azure IOT Central-toepassing maken](quick-deploy-iot-central.md) voor meer informatie.

### <a name="launch-your-application"></a>Uw toepassing starten

U kunt uw IoT Central-toepassing starten door naar de URL te gaan die u of de opbouw functie voor oplossingen hebt gekozen tijdens het maken van de app. U kunt ook een lijst weer geven met alle toepassingen waartoe u toegang hebt in de [App Manager van IOT Central](https://aka.ms/iotcentral-apps).

> [!div class="mx-imgBorder"]
> ![App-beheer IoT Central](media/overview-iot-central-tour/app-manager-pnp.png)

## <a name="navigate-your-application"></a>Navigeren in uw toepassing

Als u zich in uw IoT-toepassing bevindt, gebruikt u het linkerdeel venster voor toegang tot de verschillende gebieden. U kunt het linkerdeel venster uitvouwen of samen vouwen door het pictogram met drie lijnen boven op het deel venster te selecteren:

> [!NOTE]
> De items die u in het linkerdeel venster ziet, zijn afhankelijk van uw gebruikersrol. Meer informatie over het [beheren van gebruikers en rollen](howto-manage-users-roles.md). 

:::row:::
  :::column span="":::
      > [!div class="mx-imgBorder"]
      > ![linkerdeel venster](media/overview-iot-central-tour/navigationbar-pnp.png)
  :::column-end:::
  :::column span="2":::
     Het **dash board** van uw toepassing wordt weer gegeven. Als *opbouw functie voor oplossingen*kunt u het globale dash board voor uw Opera tors aanpassen. Afhankelijk van hun gebruikersrol kunnen Opera tors ook hun eigen persoonlijke Dash boards maken.
     
     Met **apparaten** kunt u uw verbonden apparaten beheren-echt en gesimuleerd.

     Met **Apparaatgroepen** kunt u logische verzamelingen apparaten bekijken en maken die door een query zijn opgegeven. U kunt deze query opslaan en apparaatgroepen via de toepassing gebruiken om bulk bewerkingen uit te voeren.

     Met **regels** kunt u regels maken en bewerken om uw apparaten te bewaken. Regels worden geëvalueerd op basis van de telemetrie van het apparaat en het activeren van aanpas bare acties.

     Met **Analytics** kunt u aangepaste weer gaven maken boven op de apparaatgegevens om inzichten uit uw toepassing af te leiden.

     Met **taken** kunt u uw apparaten op schaal beheren door bulk bewerkingen uit te voeren.

     Met **apparaatprofielen** maakt en beheert u de kenmerken van de apparaten die verbinding maken met uw toepassing.

     Met **gegevens export** kunt u een continue export naar externe services configureren, zoals opslag en wacht rijen.

     Met **beheer** kunt u de instellingen, aanpassing, facturering, gebruikers en rollen van uw toepassing beheren.

     Met **IOT Central** kunnen *beheerders* teruggaan naar het app manager van IOT Central.
     
   :::column-end:::
:::row-end:::

### <a name="search-help-theme-and-support"></a>Zoeken, Help, thema en ondersteuning

Het bovenste menu wordt op elke pagina weergegeven:

> [!div class="mx-imgBorder"]
> ![Werkbalk](media/overview-iot-central-tour/toolbar-pnp.png)

* Als u wilt zoeken naar Apparaatinstellingen en apparaten, voert u een **Zoek** waarde in.
* Als u de taal of het thema van de gebruikers interface wilt wijzigen, kiest u het pictogram **instellingen** . Meer informatie over [het beheren van uw toepassings voorkeuren](howto-manage-preferences.md)
* Kies het **account** pictogram om u af te melden bij de toepassing.
* Als u hulp en ondersteuning nodig hebt, kiest u het keuzemenu **Help** voor een lijst met bronnen. In een toepassing op het prijs plan gratis, bevatten de ondersteunings bronnen toegang tot [Live Chat](howto-show-hide-chat.md).

Voor de gebruikersinterface kunt u kiezen tussen een licht thema of een donker thema:

> [!NOTE]
> De optie om te kiezen tussen lichte en donkere Thema's is niet beschikbaar als uw beheerder een aangepast thema voor de toepassing heeft geconfigureerd.

> [!div class="mx-imgBorder"]
> ![Een thema voor de gebruikersinterface kiezen](media/overview-iot-central-tour/themes-pnp.png)

### <a name="dashboard"></a>Dashboard
> [!div class="mx-imgBorder"]
> ![Dashboard](media/overview-iot-central-tour/dashboard-pnp.png)

* Het dash board is de eerste pagina die u ziet wanneer u zich aanmeldt bij uw Azure IoT Central-toepassing. Als *opbouw functie voor oplossingen*kunt u meerdere algemene toepassings dashboards maken en aanpassen voor andere gebruikers. Meer informatie over [het toevoegen van tegels aan uw dash board](howto-add-tiles-to-your-dashboard.md)

* Als *operator*kunt u, als uw gebruikersrol dit toestaat, persoonlijke Dash boards maken om te controleren wat u vindt. Zie het artikel [Azure IOT Central persoonlijke Dash boards maken](howto-create-personal-dashboards.md) voor meer informatie.

### <a name="devices"></a>Apparaten

> [!div class="mx-imgBorder"]
> ![Pagina apparaten](media/overview-iot-central-tour/devices-pnp.png)

Op de Device Explorer-pagina worden de _apparaten_ in uw Azure IoT Central-toepassing gegroepeerd weergegeven op _apparaatsjabloon_. 

* Met een apparaatsjabloon wordt een type apparaat gedefinieerd dat met uw toepassing verbinding kan maken.
* Een apparaat vertegenwoordigt een echt of een gesimuleerd apparaat in uw toepassing.

Zie de Snelstartgids [uw apparaten bewaken](./quick-monitor-devices.md) voor meer informatie. 

### <a name="device-groups"></a>Device groups

> [!div class="mx-imgBorder"]
> ![Pagina apparaatgroepen](media/overview-iot-central-tour/device-groups-pnp.png)

Apparaatgroep is een verzameling van gerelateerde apparaten. Een *oplossings functie voor oplossingen* definieert een query voor het identificeren van de apparaten die zijn opgenomen in een apparaatgroep. U gebruikt apparaatgroepen om bulk bewerkingen uit te voeren in uw toepassing. Zie voor meer informatie het artikel apparaatgroepen [gebruiken in uw Azure IOT Central-toepassing](tutorial-use-device-groups.md) .

### <a name="rules"></a>Regels
> [!div class="mx-imgBorder"]
> ![Pagina Regels](media/overview-iot-central-tour/rules-pnp.png)

Op de pagina regels kunt u regels definiëren op basis van de telemetrie, status of gebeurtenissen van apparaten. Wanneer een regel wordt geactiveerd, kan een of meer acties worden geactiveerd, zoals het verzenden van een e-mail bericht, het melden van een extern systeem via webhook-waarschuwingen, enzovoort. Zie de zelf studie [regels configureren](tutorial-create-telemetry-rules.md) voor meer informatie. 

### <a name="analytics"></a>Analyse

> [!div class="mx-imgBorder"]
> ![Analysepagina](media/overview-iot-central-tour/analytics-pnp.png)

Met de analyse kunt u aangepaste weer gaven maken boven op de apparaatgegevens om inzichten uit uw toepassing af te leiden. Zie het artikel [Analytics maken voor uw Azure IOT Central-toepassing](howto-create-analytics.md) voor meer informatie.

### <a name="jobs"></a>Taken

> [!div class="mx-imgBorder"]
> ![Pagina Taken](media/overview-iot-central-tour/jobs-pnp.png)

Op de pagina taken kunt u bulk bewerkingen voor Apparaatbeheer uitvoeren op uw apparaten. U kunt eigenschappen, instellingen en opdrachten voor het uitvoeren van apparaten bijwerken op basis van apparaatgroepen. Zie voor meer informatie, het artikel [Een taak uitvoeren](howto-run-a-job.md).

### <a name="device-templates"></a>Apparaatinstellingen

> [!div class="mx-imgBorder"]
> ![Pagina met Apparaatinstellingen](media/overview-iot-central-tour/templates-pnp.png)

De opbouwfunctie gebruikt de pagina Apparaatsjablonen om de apparaatsjablonen in de toepassing te maken en beheren. Met een apparaatprofiel worden de kenmerken van apparaten opgegeven, zoals:

* Telemetrie-, status-en gebeurtenis metingen
* Eigenschappen
* Opdrachten
* Weergaven

De *oplossings functie voor oplossingen* kan ook formulieren en dash boards maken die Opera tors kunnen gebruiken voor het beheren van apparaten.

Raadpleeg voor meer informatie de zelfstudie [Define a new device type in your Azure IoT Central application](howto-set-up-template.md) (Een nieuw apparaattype definiëren in uw Azure IoT Central-toepassing). 

### <a name="data-export"></a>Gegevensexport
> [!div class="mx-imgBorder"]
> ![Pagina voor het exporteren van gegevens](media/overview-iot-central-tour/export-pnp.png)

Met gegevens export kunt u gegevens stromen, zoals telemetrie, van de toepassing naar externe systemen instellen. Zie het artikel [Exporteren van gegevens in Azure IoT Central](./howto-export-data.md) voor meer informatie.

### <a name="administration"></a>Beheer
> [!div class="mx-imgBorder"]
> ![Beheerpagina](media/overview-iot-central-tour/administration-pnp.png)

Op de pagina beheer kunt u uw IoT Central-toepassing configureren en aanpassen. Hier kunt u de naam van uw toepassing, de URL, de functies, de gebruikers en rollen beheren, API-tokens maken en uw toepassing exporteren. Raadpleeg voor meer informatie het artikel [Administer your Azure IoT Central application](howto-administer.md) (Uw Azure IoT Central-toepassing beheren).

## <a name="next-steps"></a>Volgende stappen

Nu u een overzicht hebt van Azure IoT Central en bekend bent met de indeling van de gebruikersinterface, is de voorgestelde vervolgstap om de snelstart [Create an Azure IoT Central application](quick-deploy-iot-central.md) (Een Azure IoT Central-toepassing maken) te voltooien.
