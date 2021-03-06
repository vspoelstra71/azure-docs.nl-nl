---
title: Het inspectiehulpprogramma ArrInspector
description: Gebruikers handleiding van het hulp programma ArrInspector
author: florianborn71
ms.author: flborn
ms.date: 03/09/2020
ms.topic: article
ms.openlocfilehash: e3acfc15b0c12822e48009bef4aabadac701fb2d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80680074"
---
# <a name="the-arrinspector-inspection-tool"></a>Het inspectiehulpprogramma ArrInspector

De ArrInspector is een webgebaseerd hulp programma dat wordt gebruikt om een actieve Azure remote rendering-sessie te controleren. Het is bedoeld om te worden gebruikt voor fout opsporing, om de structuur van de weer gegeven scène te controleren, de logboek berichten weer te geven en de Live prestaties op de server te controleren.

![ArrInspector](./media/arr-inspector.png)

## <a name="connecting-to-the-arrinspector"></a>Verbinding maken met de ArrInspector

Wanneer u de hostnaam (eindigend op `mixedreality.azure.com`) van uw ARR-server hebt verkregen, maakt u verbinding met [ConnectToArrInspectorAsync](../../how-tos/frontend-apis.md#connect-to-arr-inspector). Deze functie maakt een `StartArrInspector.html` op het apparaat waarop de toepassing wordt uitgevoerd. Als u ArrInspector wilt starten, opent u dat bestand met een browser (Edge, Firefox of Chrome) op een PC. Het bestand is alleen 24 uur geldig.

Als de app die wordt `ConnectToArrInspectorAsync` aangeroepen, al wordt uitgevoerd op een PC:

* Als u de unit-integratie gebruikt, kan deze automatisch worden gestart.
* Als dat niet het geval is, vindt u het bestand in *gebruikers\\mappen\\LocalAppData\\\\[your_app] AC Temp*.

Als de app wordt uitgevoerd op een HoloLens:

1. Open de HoloLens met behulp van de [Windows-portal voor apparaten](https://docs.microsoft.com/windows/mixed-reality/using-the-windows-device-portal).
1. Ga naar *System > bestanden Verkenner*.
1. Navigeer naar *gebruikers mappen\\LocalAppData\\[your_app]\\AC\\Temp*.
1. Sla *StartArrInspector. html* op uw PC op.
1. Open *StartArrInspector. html* om de ArrInspector van de sessie te laden.

## <a name="the-performance-panel"></a>Het deel venster prestaties

![Het deel venster prestaties](./media/performance-panel.png)

Dit deel venster toont grafieken van alle prestatie waarden per frame die worden weer gegeven door de-server. De waarden bevatten momenteel de frame tijd, FPS, CPU-en geheugen gebruik, geheugen statistieken zoals het totale RAM-gebruik, het aantal objecten, enzovoort.

Als u een van deze para meters wilt visualiseren, klikt u op de knop **Nieuw toevoegen** en selecteert u een van de beschik bare waarden die worden weer gegeven in het dialoog venster. Met deze actie wordt een nieuwe schuif diagram aan het deel venster toegevoegd, waarbij de waarden in realtime worden getraceerd. Aan de rechter kant ziet u de *minimum*-, *maximum* -en *huidige* waarde.

U kunt de grafiek pannen door de inhoud ervan te slepen met de muis. horizon taal pannen echter alleen mogelijk wanneer ArrInspector de status onderbroken heeft.

Als u CTRL ingedrukt houdt tijdens het slepen, kunt u inzoomen. Horizon taal zoom niveau kan ook worden beheerd met de schuif regelaar aan de onderkant.

Het verticale bereik wordt standaard berekend op basis van de waarden die momenteel worden weer gegeven, en de minimum-en maximum waarden worden weer gegeven in de tekst vakken aan de rechter kant. Wanneer de waarden hand matig worden ingesteld, kunt u deze waarden in de grafiek gebruiken door deze rechtstreeks in het tekstvak te typen of door te pannen/zoomen. Als u de automatische verticale framing wilt herstellen, klikt u op het pictogram in de rechter bovenhoek.

![verticaal bereik](./media/vertical-range.png)

## <a name="the-log-panel"></a>Het deel venster Logboek

![Logboek paneel](./media/log-panel.png)

Het deel venster logboek bevat een lijst met logboek berichten die aan de server zijde zijn gegenereerd. Bij verbinding wordt het weer gegeven tot 200 eerdere logboek berichten en worden er nieuwe afgedrukt.

U kunt de lijst filteren op basis van het logboek `[Error/Warning/Info/Debug]` type met behulp van de knoppen aan de bovenkant.
![Knoppen voor logboek filter](./media/log-filter.png)

## <a name="the-timing-data-capture-panel"></a>Het deel venster timing Data Capture

![Timing gegevens vastleggen](./media/timing-data-capture.png)

Dit deel venster wordt gebruikt om informatie over de timing van de server vast te leggen en te downloaden. In het bestand wordt gebruikgemaakt van de [JSON-indeling voor Chrome-tracering](https://docs.google.com/document/d/1CvAClvFfyA5R-PhYUmn5OOQtYMH4h6I0nSsKchNAySU/edit). Als u de gegevens wilt controleren, opent u Chrome `Chrome://tracing` op de URL en sleept u het gedownloade bestand naar de pagina. De timing gegevens worden voortdurend verzameld in een ring buffer met een vaste grootte. Bij het schrijven bevat de vastleg ging alleen informatie over de onmiddellijke periode, wat een paar seconden tot enkele minuten duurt.

## <a name="the-scene-inspection-panel"></a>Het deel venster scène inspectie

![Het deel venster scène inspectie](./media/scene-inspection-panel.png)

Dit deel venster toont de structuur van de gerenderde scène. De object hiërarchie bevindt zich aan de linkerkant. de inhoud van het geselecteerde object bevindt zich aan de rechter kant. Het deel venster is alleen-lezen en wordt in realtime bijgewerkt.

## <a name="the-vm-debug-information-panel"></a>Het informatie paneel voor de VM-fout opsporing

![Informatie paneel voor VM-fout opsporing](./media/state-debugger-panel.png)

Dit deel venster biedt een aantal functies voor fout opsporing.

### <a name="restart-service"></a>Service opnieuw starten

Met de knop **service opnieuw** starten wordt de runtime opnieuw gestart op de virtuele machine waarmee arrInspector is verbonden. De gekoppelde client wordt losgekoppeld en de arrInspector-pagina moet opnieuw worden geladen om verbinding te maken met de service die opnieuw is gestart.

### <a name="collect-debug-information"></a>Fout opsporingsgegevens verzamelen

Met de knop **fout opsporingsgegevens voor VM verzamelen** wordt een dialoog venster geopend waarin u het ARR-exemplaar kunt activeren voor het verzamelen van foutopsporingsinformatie over fout opsporing op de VM:

![Dialoog venster informatie over fout opsporing voor VM](./media/state-debugger-dialog.png)

Foutopsporingsinformatie helpt het externe rendering team van Azure bij het analyseren van problemen die zich voordoen in een lopend ARR-exemplaar. Het dialoog venster bevat een tekst veld om aanvullende informatie te geven, bijvoorbeeld stappen voor het reproduceren van een probleem.

Nadat u op de knop **verzamelen starten** hebt geklikt, wordt het dialoog venster gesloten en wordt het verzamelings proces gestart. Het verzamelen van de gegevens op de virtuele machine kan enkele minuten duren.

![Verzameling van gegevens van de VM-fout opsporing wordt uitgevoerd](./media/state-debugger-panel-in-progress.png)

Zodra de verzameling is voltooid, ontvangt u een melding in het venster ArrInspector. Deze melding bevat een ID die deze specifieke verzameling aanduidt. Zorg ervoor dat u deze ID opslaat om deze door te geven aan het Azure remote rendering-team.

![Verzamelen van gegevens van VM-fout opsporing geslaagd](./media/state-debugger-snackbar-success.png)

> [!IMPORTANT]
> U kunt geen toegang krijgen tot gegevens van de VM-fout opsporing of anderszins. Alleen het externe rendering-team van Azure heeft toegang tot de verzamelde gegevens. U moet contact met ons opnemen en de verzameling-ID samen verzenden, zodat ons het probleem kan onderzoeken dat u ziet.

## <a name="pause-mode"></a>Pauze modus

In de rechter bovenhoek kunt u een live-update van de panelen onderbreken met een switch. Deze modus kan nuttig zijn om zorgvuldig een specifieke status te controleren.

![Pauze modus](./media/pause-mode.png)

Wanneer u live update opnieuw inschakelt, worden alle panels opnieuw ingesteld.

## <a name="host-configuration"></a>Hostconfiguratie

Het hulp programma maakt standaard verbinding met de ARR-server die wordt uitgevoerd op dezelfde host als voor de ArrInspector. U kunt deze echter configureren voor het controleren van een andere server, ervan uitgaande dat er een ARR-exemplaar wordt uitgevoerd terwijl de hulp poort is geopend.

Hiertoe opent u het hoofd menu aan de linkerkant van de koptekst balk en selecteert u *configuratie*van de host. Klik op **nieuwe host toevoegen**en voer de naam en hostnaam in. Voor *hostname* gebruikt u de hostnaam die eindigt `.mixedreality.azure.com`op, geen `http://` poort.

![Hostconfiguratie](./media/host-configuration.png)

Als u snel wilt overschakelen van de ene host naar een andere, gebruikt u de vervolg keuzelijst rechtsboven.

![Host-keuze lijst](./media/host-switch-combo.png)

De hostlijst wordt opgeslagen in de lokale opslag van de browser, zodat deze behouden blijft wanneer dezelfde browser opnieuw wordt geopend.
