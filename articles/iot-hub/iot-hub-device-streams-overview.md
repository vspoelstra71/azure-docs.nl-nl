---
title: Streams van Azure IoT Hub-apparaten | Microsoft Docs
description: Overzicht van Azure IoT Hub-streams, waarmee u eenvoudigere TCP-tunnels kunt beveiligen tegen verschillende scenario's voor communicatie tussen Cloud en apparaat.
author: robinsh
services: iot-hub
ms.service: iot-hub
ms.topic: conceptual
ms.date: 01/15/2019
ms.author: robinsh
ms.openlocfilehash: ff738e56226f7cbb720a754573a9d8607e0e3247
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "73890458"
---
# <a name="iot-hub-device-streams-preview"></a>Streams van IoT Hub-apparaten (preview-versie)

Azure IoT Hub *device streams* vergemakkelijken het maken van veilige bidirectionele TCP-tunnels voor verschillende scenario's met betrekking tot Cloud-naar-apparaat-communicatie. Een apparaatgegevens stroom wordt uitgevoerd door een IoT Hub *streaming-eind punt* dat fungeert als een proxy tussen uw apparaat en service-eind punten. Deze instelling, die in het onderstaande diagram wordt weer gegeven, is vooral nuttig wanneer apparaten zich achter een netwerk firewall bevinden of zich in een particulier netwerk bevinden. Als zodanig kunnen IoT Hub-apparaten van het apparaat klanten helpen om IoT-apparaten te bereiken op een firewall-vriendelijke manier en zonder dat ze algemeen de firewall poorten van het binnenkomende of uitgaande netwerk hoeven te openen.

![Het overzicht van IoT Hub apparaten](./media/iot-hub-device-streams-overview/iot-hub-device-streams-overview.png )

Apparaten blijven beveiligd met IoT Hub-streams en zijn alleen beschikbaar voor het openen van uitgaande TCP-verbindingen naar het streaming-eind punt van de IoT-hub via poort 443. Zodra een stroom tot stand is gebracht, hebben de toepassingen aan de service zijde en aan het apparaat elke programmatische toegang tot een WebSocket-client object voor het verzenden en ontvangen van onbewerkte bytes aan elkaar. De betrouw baarheid en volg orde van de door deze tunnel verstrekte garanties is op pari met TCP.

## <a name="benefits"></a>Voordelen

IoT Hub apparaten streamen bieden de volgende voor delen:

* **Firewall vriendelijke beveiligde connectiviteit:** IoT-apparaten kunnen worden bereikt vanuit service-eind punten zonder dat er een binnenkomende firewall poort op het apparaat of in netwerk verbindingen wordt geopend (er is alleen een uitgaande verbinding met IoT Hub nodig via poort 443).

* **Verificatie:** Zowel apparaat-als service zijden van de tunnel moeten worden geverifieerd met IoT Hub met behulp van de bijbehorende referenties.

* **Versleuteling:** IoT Hub apparaat-streams gebruiken standaard TLS-verbindingen. Dit zorgt ervoor dat het verkeer altijd versleuteld is, ongeacht of de toepassing versleuteling gebruikt of niet.

* **Eenvoud van connectiviteit:** In veel gevallen elimineert het gebruik van de streams geen complexe installatie van virtuele particuliere netwerken om connectiviteit met IoT-apparaten mogelijk te maken.

* **Compatibiliteit met TCP/IP-stack:** IoT Hub streams kunnen worden toegepast op het verkeer van de TCP/IP-toepassing. Dit betekent dat een breed scala aan bedrijfs eigen en op standaarden gebaseerde protocollen gebruik kan maken van deze functie.

* **Gebruiks gemak in particuliere netwerk installaties:** Service kan communiceren met een apparaat door te verwijzen naar de apparaat-ID in plaats van het IP-adres van het apparaat. Dit is handig in situaties waarin een apparaat zich in een particulier netwerk bevindt en een privé-IP-adres heeft, of het IP-adres dynamisch wordt toegewezen en onbekend is bij de service zijde.

## <a name="device-stream-workflows"></a>Werk stromen van Device stream

Er wordt een apparaat stroom geïnitieerd wanneer de service aanvragen om verbinding te maken met een apparaat door de apparaat-ID op te geven. Deze werk stroom is met name van toepassing op een client/server-communicatie model, waaronder SSH en RDP, waarbij een gebruiker voornemens is om op afstand verbinding te maken met de SSH-of RDP-server die op het apparaat wordt uitgevoerd met behulp van een SSH-of RDP-client programma.

Het proces voor het maken van de apparaatgegevens omvat een onderhandeling tussen het apparaat, de hoofd-en streaming-eind punten van de IoT hub. Met het hoofd eindpunt van IoT hub wordt het maken van een apparaat stroom door het streaming-eind punt verwerkt, waarbij het verkeer tussen de service en het apparaat wordt afgehandeld.

### <a name="device-stream-creation-flow"></a>Stroom voor het maken van de apparaat stroom

Het programmatisch maken van een apparaat stroom met behulp van de SDK omvat de volgende stappen, die ook in de afbeelding hieronder worden weer gegeven:

!["Apparaat stream handshake-proces"](./media/iot-hub-device-streams-overview/iot-hub-device-streams-handshake.png)

1. De apparaat-app registreert vooraf een terugroeping om op de hoogte te worden gesteld wanneer een nieuwe apparaat stroom naar het apparaat wordt geïnitieerd. Deze stap vindt normaal gesp roken plaats wanneer het apparaat wordt opgestart en verbinding maakt met IoT Hub.

2. Het programma aan de service zijde initieert een apparaat stroom wanneer dit nodig is door de apparaat-ID (_niet_ het IP-adres) op te geven.

3. Met IoT hub wordt het apparaat aan het programma geïnformeerd door het aanroepen van de call back die is geregistreerd in stap 1. Het apparaat accepteert of weigert de aanvraag voor het initiëren van de stroom. Deze logica kan specifiek zijn voor uw toepassings scenario. Als de stroom aanvraag door het apparaat wordt afgewezen, IoT Hub de service dienovereenkomstig informeren. anders volgt u de onderstaande stappen.

4. Het apparaat maakt een beveiligde uitgaande TCP-verbinding met het streaming-eind punt via poort 443 en voert een upgrade uit voor de verbinding met een WebSocket. De URL van het streaming-eind punt en de referenties die moeten worden gebruikt voor verificatie, worden beide aan het apparaat door IoT Hub als onderdeel van de aanvraag die u in stap 3 hebt verzonden.

5. De service wordt op de hoogte gesteld van het resultaat van het apparaat dat de stroom accepteert en gaat verder met het maken van een eigen WebSocket-client naar het streaming-eind punt. Op dezelfde manier ontvangt de URL van het streaming-eind punt en de verificatie gegevens van IoT Hub.

In het onderstaande handshake-proces:

* Het handshake-proces moet binnen 60 seconden worden voltooid (stap 2 tot en met 5), anders mislukt de handshake met een time-out en wordt de service hiervan op de hoogte gesteld.

* Nadat de stroom voor het maken van de stroom hierboven is voltooid, fungeert het streaming-eind punt als een proxy en wordt het verkeer tussen de service en het apparaat via de respectievelijke websockets overgedragen.

* Apparaat en service hebben beide uitgaande verbindingen nodig voor het hoofd eindpunt van IoT Hub en het streaming-eind punt via poort 443. De URL van deze eind punten is beschikbaar op het tabblad *overzicht* op de portal van de IOT hub.

* De betrouw baarheid en best ellende garanties van een vastgelegde stroom zijn op pari met TCP.

* Alle verbindingen met IoT Hub-en streaming-eind punt gebruiken TLS en worden versleuteld.

### <a name="termination-flow"></a>Beëindigings stroom

Een vastgelegde stroom wordt beëindigd wanneer een van de TCP-verbindingen met de gateway wordt verbroken (door de service of het apparaat). Dit kan vrijwillig worden gedaan door de WebSocket te sluiten op het apparaat of in de service of in het geval van een time-out of proces fout van een netwerk verbinding. Na beëindiging van de verbinding van een apparaat of service met het streaming-eind punt, wordt de andere TCP-verbinding ook beëindigd (geforceerd) en de service en het apparaat moeten de stream zo nodig opnieuw maken.

## <a name="connectivity-requirements"></a>Connectiviteits vereisten

Zowel het apparaat als de service zijde van een apparaatgegevens moeten in staat zijn om met TLS ingeschakelde verbindingen tot stand te brengen met IoT Hub en het streaming-eind punt. Hiervoor is een uitgaande verbinding vereist via poort 443 voor deze eind punten. De hostnaam die is gekoppeld aan deze eind punten vindt u op het tabblad *overzicht* van IOT hub, zoals wordt weer gegeven in de afbeelding hieronder:

!["Stream-eind punten van apparaat"](./media/iot-hub-device-streams-overview/device-stream-in-portal.png)

De gegevens van de eind punten kunnen ook worden opgehaald met behulp van Azure CLI onder de sectie eigenschappen van de hub `property.hostname` , `property.deviceStreams` met name en de sleutels.

```azurecli-interactive
az iot hub devicestream show --name <YourIoTHubName>
```

De uitvoer is een JSON-object van alle eind punten die het apparaat en de service van uw hub nodig hebben om verbinding te kunnen maken met een apparaat stroom.

```json
{
  "streamingEndpoints": [
    "https://<YourIoTHubName>.<region-stamp>.streams.azure-devices.net"
  ]
}
```

> [!NOTE]
> Zorg ervoor dat u Azure CLI-versie 2.0.57 of hoger hebt geïnstalleerd. U kunt de meest recente versie downloaden van de pagina [Azure cli installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) .
>

## <a name="allow-outbound-connectivity-to-the-device-streaming-endpoints"></a>Uitgaande connectiviteit met de streaming-eind punten van het apparaat toestaan

Zoals vermeld aan het begin van dit artikel, maakt het apparaat een uitgaande verbinding naar IoT Hub streaming-eind punt tijdens het initiëren van het apparaat. De firewalls op het apparaat of het netwerk moeten uitgaande verbindingen met de streaming-gateway via poort 443 toestaan (Houd er rekening mee dat communicatie plaatsvindt via een WebSocket-verbinding die is versleuteld met behulp van TLS).

De hostnaam van het eind punt van Device streaming bevindt zich op het tabblad Overzicht van de ![Azure IOT hub-Portal. "Stream-eind punten van apparaat"](./media/iot-hub-device-streams-overview/device-stream-in-portal.png)

U kunt deze informatie ook vinden met behulp van Azure CLI:

```azurecli-interactive
az iot hub devicestream show --name <YourIoTHubName>
```

> [!NOTE]
> Zorg ervoor dat u Azure CLI-versie 2.0.57 of hoger hebt geïnstalleerd. U kunt de meest recente versie downloaden van de pagina [Azure cli installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) .
>

## <a name="troubleshoot-via-device-streams-activity-logs"></a>Problemen oplossen via activiteiten logboeken van Device streams

U kunt Azure Monitor logboeken instellen om het activiteiten logboek van de streams van apparaten in uw IoT Hub te verzamelen. Dit kan zeer nuttig zijn in scenario's voor het oplossen van problemen.

Volg de onderstaande stappen om Azure Monitor logboeken te configureren voor de activiteiten van de apparaten stroom van uw IoT Hub:

1. Ga naar het tabblad *Diagnostische instellingen* in uw IOT hub en klik op *Diagnostische gegevens inschakelen* .

   ![' Diagnostische logboeken inschakelen '](./media/iot-hub-device-streams-overview/device-streams-diagnostics-settings-pane.png)

2. Geef een naam op voor de diagnostische instellingen en kies *verzenden naar log Analytics* optie. U wordt begeleid bij het kiezen van een bestaande Log Analytics werkruimte resource of het maken van een nieuwe. Controleer ook de *DeviceStreams* in de lijst.

    !["Device streams logboeken inschakelen"](./media/iot-hub-device-streams-overview/device-streams-configure-diagnostics.png)

3. U hebt nu toegang tot uw stream-logboeken voor apparaten op het tabblad *Logboeken* in de portal van uw IOT hub. Activiteiten logboeken voor Device stream worden weer `AzureDiagnostics` gegeven in de `Category=DeviceStreams`tabel en hebben.

   Zoals hieronder wordt weer gegeven, is de identiteit van het doel apparaat en het resultaat van de bewerking ook beschikbaar in de logboeken.

   !["Stream-logboeken van apparaat openen"](./media/iot-hub-device-streams-overview/device-streams-view-logs.png)

## <a name="regional-availability"></a>Regionale beschikbaarheid

Tijdens de open bare preview zijn IoT Hub-streams beschikbaar in de regio's VS-centraal, centraal-EUAP, Europa-noord en Zuidoost-Azië. Zorg ervoor dat u uw hub in een van deze regio's maakt.

## <a name="sdk-availability"></a>SDK-Beschik baarheid

Twee zijden van elke stroom (op het apparaat en de service zijde) gebruik de IoT Hub SDK om de tunnel te maken. Tijdens de open bare Preview kunnen klanten kiezen uit de volgende SDK-talen:

* De ondersteunings apparaten van de C-en C#-SDK op het apparaat worden ondersteund.

* De NodeJS-en C#-SDK ondersteunen apparaat stromen aan de kant van de service.

## <a name="iot-hub-device-stream-samples"></a>Voor beelden van IoT Hub Device stream

Er zijn twee [Quick](/azure/iot-hub) start-voor beelden beschikbaar op de pagina IOT hub. Hierin wordt het gebruik van apparaten door toepassingen gedemonstreerd.

* Het *echo* voorbeeld demonstreert het programmatisch gebruik van streams (door de SDK API direct aan te roepen).

* In het voor beeld van een *lokale proxy* wordt gedemonstreerd dat de tunneling van het netwerk verkeer van de client/server (zoals SSH, RDP of web) via een apparaat wordt gestreamd.

Deze voor beelden worden hieronder nader besproken.

### <a name="echo-sample"></a>ECHO voorbeeld

Het voor beeld van de echo demonstreert het programmatisch gebruik van apparaten voor het verzenden en ontvangen van bytes tussen service-en apparaat-apps. Houd er rekening mee dat u service-en apparaat-Program ma's in verschillende talen kunt gebruiken. U kunt bijvoorbeeld het programma C-apparaat gebruiken met het C#-service programma.

Dit zijn de voor beelden van echo's:

* [C#-service en-service programma](quickstart-device-streams-echo-csharp.md)

* [Node. js-service programma](quickstart-device-streams-echo-nodejs.md)

* [C Device-programma](quickstart-device-streams-echo-c.md)

### <a name="local-proxy-sample-for-ssh-or-rdp"></a>Voor beeld van een lokale proxy (voor SSH of RDP)

Het voor beeld van een lokale proxy demonstreert een manier om tunneling in te scha kelen van het verkeer van een bestaande toepassing waarbij communicatie tussen een client en een server programma is betrokken. Deze instelling werkt voor client/server-protocollen zoals SSH en RDP, waarbij de service zijde fungeert als een client (waarop SSH-of RDP-client Programma's worden uitgevoerd), en het apparaat fungeert als de server (waarop SSH-daemon of RDP-server Programma's worden uitgevoerd).

In deze sectie wordt het gebruik van de streams beschreven om de gebruiker in staat te stellen SSH naar een apparaat via de streams van een apparaat te maken (de aanvraag voor RDP of andere client/server-toepassing is vergelijkbaar met de bijbehorende poort van het Protocol).

De installatie maakt gebruik van twee *lokale proxy* Programma's die worden weer gegeven in de onderstaande afbeelding, namelijk *apparaat-lokale proxy* en *service-Local proxy*. De lokale proxy Programma's zijn verantwoordelijk voor het uitvoeren van de handshake voor het [initiëren van de apparaatgegevens](#device-stream-creation-flow) met IOT hub en met behulp van reguliere client/server-sockets met de SSH-client en SSH-daemon.

!["Device stream-proxy-installatie voor SSH/RDP"](./media/iot-hub-device-streams-overview/iot-hub-device-streams-ssh.png)

1. De gebruiker voert service-Local proxy uit om een apparaat stroom naar het apparaat te initiëren.

2. De apparaat-Local proxy accepteert de aanvraag voor het initiëren van streams en de tunnel wordt ingesteld voor het streaming-eind punt van IoT Hub (zoals hierboven beschreven).

3. De apparaat-lokale proxy maakt verbinding met het SSH-daemon-eind punt dat luistert op poort 22 op het apparaat.

4. De service-Local proxy luistert op een aangewezen poort, in afwachting van nieuwe SSH-verbindingen van de gebruiker (poort 2222 gebruikt in het voor beeld, maar dit kan worden geconfigureerd voor elke andere beschik bare poort). De gebruiker wijst de SSH-client naar de service-lokale Proxy poort op localhost.

### <a name="notes"></a>Opmerkingen

* De bovenstaande stappen volt ooien een end-to-end-tunnel tussen de SSH-client (rechts) naar de SSH-daemon (aan de linkerkant). Onderdeel van deze end-to-end-connectiviteit is het verzenden van verkeer via een apparaat stroom naar IoT Hub.

* De pijlen in de afbeelding geven de richting aan waarin verbindingen tot stand worden gebracht tussen eind punten. Houd er rekening mee dat er geen binnenkomende verbindingen naar het apparaat worden verzonden (dit wordt vaak geblokkeerd door een firewall).

* De keuze van het gebruik van poort 2222 op de service-Local proxy is een wille keurige keuze. De proxy kan worden geconfigureerd voor het gebruik van elke andere beschik bare poort.

* De keuze van poort 22 is in dit geval protocol-afhankelijk en specifiek voor SSH. Voor het geval van RDP moet poort 3389 worden gebruikt. Dit kan in de opgegeven voorbeeld Programma's worden geconfigureerd.

Gebruik de onderstaande koppelingen voor instructies over het uitvoeren van de lokale proxy Programma's in de taal van uw keuze. Net als bij het voor beeld van de [echo](#echo-sample), kunt u de Program ma's voor het apparaat en de lokale proxy in verschillende talen uitvoeren, aangezien ze volledig compatibel zijn.

* [C#-service en-service programma](quickstart-device-streams-proxy-csharp.md)

* [Node. js-service programma](quickstart-device-streams-proxy-nodejs.md)

* [C Device-programma](quickstart-device-streams-proxy-c.md)

## <a name="next-steps"></a>Volgende stappen

Gebruik de onderstaande koppelingen voor meer informatie over het streamen van apparaten.

> [!div class="nextstepaction"]
> [Apparaatversleuteling op IoT show (Channel 9)](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fchannel9.msdn.com%2FShows%2FInternet-of-Things-Show%2FAzure-IoT-Hub-Device-Streams&data=02%7C01%7Crezas%40microsoft.com%7Cc3486254a89a43edea7c08d67a88bcea%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636831125031268909&sdata=S6u9qiehBN4tmgII637uJeVubUll0IZ4p2ddtG5pDBc%3D&reserved=0)
