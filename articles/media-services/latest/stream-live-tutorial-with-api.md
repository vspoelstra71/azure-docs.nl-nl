---
title: Live streamen met Media Services v3
titleSuffix: Azure Media Services
description: Meer informatie over het streamen van Live met Azure Media Services v3.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/13/2019
ms.author: juliako
ms.openlocfilehash: 0b6667965ddd1fce30bb2da2593e2a9274b595ed
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "79472013"
---
# <a name="tutorial-stream-live-with-media-services"></a>Zelf studie: live streamen met Media Services

> [!NOTE]
> Hoewel in de zelf studie [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveevent?view=azure-dotnet) -voor beelden worden gebruikt, zijn de algemene stappen hetzelfde voor [rest API](https://docs.microsoft.com/rest/api/media/liveevents), [cli](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest)of andere ondersteunde [sdk's](media-services-apis-overview.md#sdks).

In Azure Media Services zijn [livegebeurtenissen](https://docs.microsoft.com/rest/api/media/liveevents) verantwoordelijk voor het verwerken inhoud voor live streamen. Een livegebeurtenis biedt een invoereindpunt (de URL voor opnemen) dat u vervolgens doorgeeft aan een live-encoder. De livegebeurtenis ontvangt live-invoerstromen van de live-encoder en maakt deze beschikbaar voor streaming via een of meer [streaming-eindpunten](https://docs.microsoft.com/rest/api/media/streamingendpoints). Livegebeurtenissen bieden ook een preview-eindpunt (voorbeeld-URL) dat u kunt gebruiken om een voorbeeld van de stream te bekijken en deze te valideren voordat deze verder wordt verwerkt en geleverd. In deze zelfstudie ziet u hoe u .NET Core gebruikt om een **pass-through**-type van een live-gebeurtenis te maken.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Down load de voor beeld-app die wordt beschreven in het onderwerp.
> * Bekijk de code waarmee live streamen wordt uitgevoerd.
> * Bekijk de gebeurtenis met [Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/index.html) op [https://ampdemo.azureedge.net](https://ampdemo.azureedge.net).
> * Resources opschonen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Hieronder wordt aangegeven wat de vereisten zijn om de zelfstudie te voltooien:

- Installeer Visual Studio Code of Visual Studio.
- [Een Azure Media Services-account maken](create-account-cli-how-to.md).<br/>Zorg ervoor dat u de waarden die u gebruikt voor de naam van de resource groep en de naam van de Media Services account onthouden.
- Volg de stappen in [Access Azure Media Services API with the Azure CLI](access-api-cli-how-to.md) (Toegang tot de Azure Media Services-API met de Azure CLI) en sla de referenties op. U moet deze gebruiken om toegang te krijgen tot de API.
- Een camera of een apparaat (zoals een laptop) dat wordt gebruikt om een gebeurtenis te verzenden.
- Zie [Aanbevolen on-premises Live coderings](recommended-on-premises-live-encoders.md)Programma's voor een on-premises Live coderings programma dat signalen van de camera converteert naar stromen die worden verzonden naar de Media Services live streaming-service. De stroom moet de **RTMP**- of **Smooth Streaming**-indeling hebben.

> [!TIP]
> Zorg dat u [Live streamen met Media Services v3](live-streaming-overview.md) hebt gelezen voordat u verder gaat. 

## <a name="download-and-configure-the-sample"></a>Het voorbeeld downloaden en configureren

Gebruik de volgende opdracht om een GitHub-opslagplaats te klonen op uw computer die het .NET-voorbeeld voor het streamen van video bevat:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```

Het voorbeeld voor live streamen staat in de map [Live](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/Live/MediaV3LiveApp).

Open [appSettings. json](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/appsettings.json) in het gedownloade project. Vervang de waarden door de referenties die u hebt gekregen van [toegang tot api's](access-api-cli-how-to.md).

> [!IMPORTANT]
> In dit voor beeld wordt een uniek achtervoegsel gebruikt voor elke resource. Als u de fout opsporing annuleert of de app afsluit zonder deze uit te voeren, worden er meerdere Live gebeurtenissen in uw account opgevolgd. <br/>Zorg dat u de actieve livegebeurtenissen stopt. Anders wordt u **gefactureerd**.

## <a name="examine-the-code-that-performs-live-streaming"></a>De code bestuderen die live streamen uitvoert

In dit gedeelte worden de functies bekeken die zijn gedefinieerd in het bestand [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs) van het project *MediaV3LiveApp*.

Het voor beeld maakt een uniek achtervoegsel voor elke resource, zodat u geen naam conflicten meer hebt als u het voor beeld meerdere keren uitvoert zonder op te schonen.

> [!IMPORTANT]
> In dit voor beeld wordt een uniek achtervoegsel gebruikt voor elke resource. Als u de fout opsporing annuleert of de app afsluit zonder deze uit te voeren, worden er meerdere Live gebeurtenissen in uw account opgevolgd. <br/>
> Zorg dat u de actieve livegebeurtenissen stopt. Anders wordt u **gefactureerd**.

### <a name="start-using-media-services-apis-with-net-sdk"></a>Starten met het gebruik van Media Services API's met .NET SDK

Als u wilt starten met Media Services API's met .NET, moet u een **AzureMediaServicesClient**-object maken. Als u het object wilt maken, moet u referenties opgeven die de client nodig heeft om verbinding te maken met Azure met behulp van Microsoft Azure Active Directory. In de code die u aan het begin van het artikel hebt gekloond, wordt met de functie **GetCredentialsAsync** het object ServiceClientCredentials gemaakt op basis van de referenties die zijn opgegeven in het lokale configuratiebestand. 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateMediaServicesClient)]

### <a name="create-a-live-event"></a>Een livegebeurtenis maken

In deze sectie wordt beschreven hoe u een livegebeurtenis van het type **pass-through** maakt (LiveEventEncodingType ingesteld op None). Zie [Live Event types](live-events-outputs-concept.md#live-event-types)(Engelstalig) voor meer informatie over de beschik bare typen Live-gebeurtenissen. 
 
U kunt bijvoorbeeld de volgende zaken opgeven wanneer u de live-gebeurtenis wilt maken:

* Media Services locatie.
* Het streaming-protocol voor de livegebeurtenis (momenteel worden de protocollen RTMP en Smooth Streaming ondersteund).<br/>U kunt de protocol optie niet wijzigen terwijl de live-gebeurtenis of de eraan gekoppelde actieve uitvoer bewerkingen worden uitgevoerd. Als u verschillende protocollen nodig hebt, maakt u afzonderlijke Live-gebeurtenissen voor elk streaming-protocol.  
* IP-beperkingen voor de opname en voorbeeldweergave. U kunt de IP-adressen definiëren die zijn toegestaan om een video van deze livegebeurtenis op te nemen. Toegestane IP-adressen kunnen worden opgegeven als één IP-adres (bijvoorbeeld 10.0.0.1), een IP-adresbereik met een IP-adres en een CIDR-subnetmasker (bijvoorbeeld 10.0.0.1/22) of een IP-adresbereik met een IP-adres en een decimaal subnetmasker met punten (bijvoorbeeld , ' 10.0.0.1(255.255.252.0)').<br/>Als er geen IP-adressen zijn opgegeven en er geen regel definitie is, is er geen IP-adres toegestaan. Als u IP-adres(sen) wilt toestaan, maakt u een regel en stelt u 0.0.0.0/0 in.<br/>De IP-adressen moeten een van de volgende indelingen hebben: IpV4-adres met vier getallen of CIDR-adres bereik.
* Wanneer u de gebeurtenis maakt, kunt u opgeven dat deze automatisch moet worden gestart. <br/>Wanneer autostart is ingesteld op True, wordt de Live gebeurtenis gestart na het maken ervan. Dit betekent dat de facturering begint zodra de live-gebeurtenis wordt gestart. U moet expliciet Stop aanroepen in de resource van de livegebeurtenis om verdere facturering stop te zetten. Zie [Live Event states and billing](live-event-states-billing.md) (Statussen en facturering voor livegebeurtenissen) voor meer informatie.
* Stel de ' Vanity-modus in om een opname-URL voorspellend te maken. Zie [url's voor Live Event-opname](live-events-outputs-concept.md#live-event-ingest-urls)voor gedetailleerde informatie.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveEvent)]

### <a name="get-ingest-urls"></a>URL’s voor opnemen ophalen

Zodra de live-gebeurtenis is gemaakt, kunt u opname-Url's ophalen die u aan de Live Encoder wilt leveren. Het coderingsprogramma gebruikt deze URL's voor het invoeren van een live stream.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetIngestURL)]

### <a name="get-the-preview-url"></a>De voorbeeld-URL ophalen

Gebruik het previewEndpoint als u een voorbeeld wilt bekijken en wilt controleren of de invoer van de encoder daadwerkelijk wordt ontvangen.

> [!IMPORTANT]
> Zorg ervoor dat de video stroomt naar de Preview-URL voordat u doorgaat.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetPreviewURLs)]

### <a name="create-and-manage-live-events-and-live-outputs"></a>Livegebeurtenissen en live-uitvoer maken en beheren

Wanneer de stream naar de livegebeurtenis stroomt, kunt u de streaming-gebeurtenis starten door een Asset, live-uitvoer en streaming-locator te maken. Hiermee wordt de stream gearchiveerd en beschikbaar gesteld aan kijkers via het streaming-eindpunt.

#### <a name="create-an-asset"></a>Een Asset maken

Maak een Asset die u met de live-uitvoer wilt gebruiken.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateAsset)]

#### <a name="create-a-live-output"></a>Een live-uitvoer maken

Live-uitvoer starten zodra ze zijn gemaakt en stoppen wanneer ze worden verwijderd. Wanneer u de live uitvoer verwijdert, worden de onderliggende activa en inhoud in de Asset niet verwijderd.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveOutput)]

#### <a name="create-a-streaming-locator"></a>Een streaming-locator maken

> [!NOTE]
> Wanneer uw Media Services-account wordt gemaakt, wordt er een **standaard** streaming-eind punt aan uw account toegevoegd met de status **gestopt** . Als u inhoud wilt streamen en gebruik wilt maken van [dynamische pakketten](dynamic-packaging-overview.md) en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben.

Als u de Asset van de live-uitvoer publiceert met een streaming-locator, blijft de livegebeurtenis (tot de maximale DVR-duur) zichtbaar tot de streaming-locator verloopt of wordt verwijderd (welke het eerst van toepassing is).

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateStreamingLocator)]

```csharp

// Get the url to stream the output
ListPathsResponse paths = await client.StreamingLocators.ListPathsAsync(resourceGroupName, accountName, locatorName);

foreach (StreamingPath path in paths.StreamingPaths)
{
    UriBuilder uriBuilder = new UriBuilder();
    uriBuilder.Scheme = "https";
    uriBuilder.Host = streamingEndpoint.HostName;

    uriBuilder.Path = path.Paths[0];
    // Get the URL from the uriBuilder: uriBuilder.ToString()
}
```

### <a name="cleaning-up-resources-in-your-media-services-account"></a>Resources in uw Media Services-account opschonen

Als u klaar bent met het streamen van gebeurtenissen en wilt opschonen van de resources die u eerder hebt ingericht, volgt u de volgende procedure:

* Stop het pushen van de stream vanuit het coderingsprogramma.
* Stop de livegebeurtenis. Zodra de live-gebeurtenis is gestopt, worden er geen kosten in rekening gebracht. Als u het kanaal opnieuw wilt starten, wordt dezelfde URL voor opnemen gebruikt, zodat u het coderingsprogramma niet opnieuw hoeft te configureren.
* U kunt uw streaming-eindpunt stoppen, tenzij u het archief van uw live gebeurtenis wilt blijven leveren als stream op aanvraag. Als de live-gebeurtenis is gestopt, worden er geen kosten in rekening gebracht.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLiveEventAndOutput)]

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLocatorAssetAndStreamingEndpoint)]

## <a name="watch-the-event"></a>De gebeurtenis bekijken

Als u de gebeurtenis wilt bekijken, kopieert u de streaming-URL die u hebt ontvangen tijdens het uitvoeren van de code die wordt beschreven in een streaming-Locator maken. U kunt een media speler van uw keuze gebruiken. [Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/index.html) is beschikbaar om uw stream te testen https://ampdemo.azureedge.netop.

De livegebeurtenis wordt automatisch geconverteerd naar inhoud op aanvraag wanneer deze wordt gestopt. Zelfs nadat u het evenement hebt gestopt en verwijderd, kunnen gebruikers de gearchiveerde inhoud als een video op aanvraag streamen, mits u de Asset niet verwijdert. Een Asset kan niet worden verwijderd als deze wordt gebruikt door een gebeurtenis; de gebeurtenis moet eerst worden verwijderd.

## <a name="clean-up-resources"></a>Resources opschonen

Als u de resources van de resourcegroep niet meer nodig hebt, met inbegrip van de Media Services en opslagaccounts die u hebt gemaakt voor deze zelfstudie, verwijdert u de resourcegroep die u eerder hebt gemaakt.

Voer de volgende CLI-opdracht uit:

```azurecli-interactive
az group delete --name amsResourceGroup
```

> [!IMPORTANT]
> Zolang de livegebeurtenis loopt, worden er kosten in rekening gebracht. Als het project/programma vastloopt of om welke reden dan ook wordt afgesloten, is het mogelijk dat de livegebeurtenis in een factureringsstaat staat.

## <a name="ask-questions-give-feedback-get-updates"></a>Vragen stellen, feedback geven, updates ophalen

Bekijk het [Azure Media Services Community](media-services-community.md) -artikel voor verschillende manieren om vragen te stellen, feedback te geven en updates te ontvangen over Media Services.

## <a name="next-steps"></a>Volgende stappen

[Bestanden streamen](stream-files-tutorial-with-api.md)
 
