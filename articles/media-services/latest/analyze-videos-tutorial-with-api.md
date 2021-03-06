---
title: Video's analyseren met Media Services v3
titleSuffix: Azure Media Services
description: Meer informatie over het analyseren van Video's met behulp van Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: tutorial
ms.date: 03/26/2020
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: b7864d89cc14a1473fd43e94bfe74c368bcb391d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80349483"
---
# <a name="tutorial-analyze-videos-with-media-services-v3"></a>Zelf studie: Video's analyseren met Media Services v3

> [!NOTE]
> Hoewel deze zelf studie de [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveevent?view=azure-dotnet) -voor beelden gebruikt, zijn de algemene stappen hetzelfde [voor rest API](https://docs.microsoft.com/rest/api/media/liveevents), [cli](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest)of andere ondersteunde [sdk's](media-services-apis-overview.md#sdks).

In deze zelfstudie ziet u hoe u video's kunt analyseren met Azure Media Services. Er zijn veel scenario's waarin u misschien inzicht wilt krijgen in opgenomen video's of audio-inhoud. Voor het bereiken van een hogere klanttevredenheid kunnen organisaties bijvoorbeeld spraak-naar-tekstverwerking uitvoeren om opnamen van de klantenondersteuning om te zetten in een catalogus van zoekmachines, met indexen en dashboards. Vervolgens kunnen ze inzicht krijgen in hun bedrijf. Deze inzichten bevatten een lijst met algemene klachten, bronnen van dergelijke klachten en andere nuttige informatie.

In deze handleiding ontdekt u hoe u:

> [!div class="checklist"]
> * Down load de voor beeld-app die wordt beschreven in het onderwerp.
> * Controleer de code die de opgegeven video analyseert.
> * Voer de app uit.
> * Controleer de uitvoer.
> * Resources opschonen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="compliance-privacy-and-security"></a>Compliance, privacy en beveiliging
 
Als belang rijke herinnering moet u zich houden aan alle toepasselijke wetgeving bij het gebruik van Video Indexer en mag u Video Indexer of een andere Azure-service niet gebruiken op een manier die de rechten van anderen schendt of schadelijk voor anderen is. Voordat u Video's, met inbegrip van biometrische gegevens, naar de Video Indexer-service voor de verwerking en opslag uploadt, moet u over alle juiste rechten beschikken, met inbegrip van alle toepasselijke toestemmingen, van de afzonderlijke (en) in de video. Voor meer informatie over naleving, privacy en beveiliging in Video Indexer, de micro soft [Cognitive Services-voor waarden](https://azure.microsoft.com/support/legal/cognitive-services-compliance-and-privacy/). Raadpleeg [de privacyverklaring van](https://privacy.microsoft.com/PrivacyStatement)micro soft voor de privacy van micro soft en de verwerking van uw gegevens, de [voor waarden voor Online Services](https://www.microsoft.com/licensing/product-licensing/products) ("Ost") en de [addendum op gegevens verwerking](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=67) ("DPA"). Aanvullende privacy-informatie, inclusief gegevens retentie, verwijdering/vernietiging, is beschikbaar in de OST en [hier](../video-indexer/faq.md). Door Video Indexer te gebruiken, gaat u akkoord met de Cognitive Services voor waarden, de OST, DPA en de privacyverklaring.

## <a name="prerequisites"></a>Vereisten

- Als Visual Studio niet is geïnstalleerd, kunt u [Visual Studio Community 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15)downloaden.
- [Een Azure Media Services-account maken](create-account-cli-how-to.md).<br/>Vergeet niet de waarden die u hebt gebruikt voor de namen van de resourcegroep en het Media Services-account.
- Volg de stappen in [Access Azure Media Services API with the Azure CLI](access-api-cli-how-to.md) (Toegang tot de Azure Media Services-API met de Azure CLI) en sla de referenties op. U moet deze gebruiken om toegang te krijgen tot de API.

## <a name="download-and-configure-the-sample"></a>Het voorbeeld downloaden en configureren

Kloon met de volgende opdracht een GitHub-opslagplaats met het .NET-voorbeeld op de computer:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```

Het voorbeeld bevindt zich in de map [AnalyzeVideos](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/AnalyzeVideos).

Open [appSettings. json](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/AnalyzeVideos/appsettings.json) in het gedownloade project. Vervang de waarden door de referenties die u hebt gekregen van [toegang tot api's](access-api-cli-how-to.md).

## <a name="examine-the-code-that-analyzes-the-specified-video"></a>De code onderzoeken die de opgegeven video analyseert

In dit gedeelte worden de functies bekeken die zijn gedefinieerd in het bestand [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/AnalyzeVideos/Program.cs) van het project *AnalyzeVideos*.

Het voor beeld heeft de volgende acties voltooid:

1. Een **Transformatie** en een **Taak** maken voor het analyseren van de video.
2. Een invoer**asset** maken en de video uploaden naar deze asset. De asset wordt gebruikt als de invoer voor de taak.
3. Een uitvoerasset maken waarin de uitvoer van de taak wordt opgeslagen.
4. De taak verzenden.
5. De status van de taak controleren.
6. De bestanden downloaden die het resultaat zijn van het uitvoeren van de taak.

> [!NOTE]
> Wanneer u voorinstellingen voor een Video of Audio Analyzer gebruikt, moet u de Azure-portal gebruiken om uw account in te stellen op 10 S3 Door media gereserveerde eenheden. Zie [Mediaverwerking schalen](media-reserved-units-cli-how-to.md) voor meer informatie.

### <a name="start-using-media-services-apis-with-net-sdk"></a>Starten met het gebruik van Media Services API's met .NET SDK

Als u wilt starten met Media Services API's met .NET, moet u een **AzureMediaServicesClient**-object maken. Als u het object wilt maken, moet u referenties opgeven die de client nodig heeft om verbinding te maken met Azure met behulp van Microsoft Azure Active Directory. In de code die u aan het begin van het artikel hebt gekloond, wordt met de functie **GetCredentialsAsync** het object ServiceClientCredentials gemaakt op basis van de referenties die zijn opgegeven in het lokale configuratiebestand. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateMediaServicesClient)]

### <a name="create-an-input-asset-and-upload-a-local-file-into-it"></a>Een invoerasset maken en er een lokaal bestand in uploaden 

Met de functie **CreateInputAsset** wordt een nieuwe [invoerasset](https://docs.microsoft.com/rest/api/media/assets) gemaakt en het opgegeven lokale videobestand wordt hierin geladen. Deze asset wordt gebruikt als invoer voor uw coderingstaak. In Media Services-v3 kan de invoer voor een taak een asset zijn of inhoud die u beschikbaar maakt voor uw Media Services-account via HTTPS-URL's. Zie [Dit](job-input-from-http-how-to.md) artikel voor meer informatie over het coderen van een HTTPS-URL.  

In Media Services v3 kunt u Azure Storage-API's gebruiken om bestanden te uploaden. Het volgende .NET-fragment laat zien hoe.

Met de volgende functie worden deze acties voltooid:

* Hiermee maakt u een Asset.
* Hiermee wordt een Beschrijf bare [SAS-URL](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) opgehaald naar de container van het activum [in de opslag](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet#upload-blobs-to-a-container).

    Als u de [ListContainerSas](https://docs.microsoft.com/rest/api/media/assets/listcontainersas) -functie van Asset gebruikt om SAS-url's op te halen, moet u er rekening mee houden dat de functie meerdere SAS-url's retourneert, aangezien er twee opslag account sleutels zijn voor elk opslag account. Een opslag account heeft twee sleutels omdat hiermee naadloze rotatie van de sleutel van het opslag account mogelijk is (bijvoorbeeld een van de sleutels wijzigen terwijl u de nieuwe sleutel gebruikt en de andere sleutel draait). De eerste SAS-URL vertegenwoordigt opslag Key1 en een tweede opslag-Key2.
* Hiermee wordt het bestand geüpload naar de container in Storage met behulp van de SAS-URL.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateInputAsset)]

### <a name="create-an-output-asset-to-store-the-result-of-the-job"></a>Een uitvoerasset maken voor het opslaan van het resultaat van de taak

In de [uitvoerasset](https://docs.microsoft.com/rest/api/media/assets) wordt het resultaat van de taak opgeslagen. Het project definieert de functie **DownloadResults** die de resultaten van deze uitvoerasset naar de uitvoermap downloadt zodat u kunt zien wat u hebt gekregen.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateOutputAsset)]

### <a name="create-a-transform-and-a-job-that-analyzes-videos"></a>Een transformatie en een taak maken die video's analyseert

Wanneer u inhoud codeert of verwerkt in Media Services, is dit een gemeen schappelijk patroon voor het instellen van de coderings instellingen als een recept. U dient vervolgens een **taak** in te dienen om het recept toe te passen op een video. Door nieuwe taken voor elke nieuwe video in te dienen, past u dat recept toe op alle Video's in uw bibliotheek. Een recept in Media Services wordt een **trans formatie**genoemd. Zie voor meer informatie [Transformaties en taken](transform-concept.md). Met het voorbeeld dat wordt beschreven in deze zelfstudie wordt een recept gedefinieerd waarmee de opgegeven video wordt geanalyseerd.

#### <a name="transform"></a>Transformeren

Bij het maken van een nieuw [transformatie](https://docs.microsoft.com/rest/api/media/transforms)-exemplaar, moet u opgeven wat u als uitvoer wilt maken. **TransformOutput** is een vereiste para meter. Elke **transformatie-uitvoer** bevat een **voorinstelling**. **Voorinstelling** bevat stapsgewijze instructies van de video- en/of audioverwerkingen die moeten worden gebruikt voor het genereren van de gewenste **TransformOutput**. In dit voor beeld wordt de **VideoAnalyzerPreset** -voor instelling gebruikt en de taal (en-US) wordt door gegeven aan de constructor (`new VideoAnalyzerPreset("en-US")`). Met deze voorinstelling kunt u meerdere audio- en video-inzichten uit een video extraheren. U kunt de voorinstelling **AudioAnalyzerPreset** gebruiken als u meerdere audio-inzichten wilt extraheren uit een video.

Wanneer u een **trans formatie**maakt, moet u eerst controleren of er al een bestaat met de **Get** -methode, zoals wordt weer gegeven in de volgende code. In Media Services-v3 retourneert de methode **Ophalen** van entiteiten **null** als de entiteit (een hoofdlettergevoelige controle van de naam).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#EnsureTransformExists)]

#### <a name="job"></a>Taak

Zoals eerder vermeld, is het [transformatie](https://docs.microsoft.com/rest/api/media/transforms)-object het recept en is de [taak](https://docs.microsoft.com/rest/api/media/jobs) de werkelijke aanvraag bij Media Services om deze **transformatie** toe te passen op een bepaalde invoervideo of audio-inhoud. De **taak** geeft informatie op zoals de locatie van de invoer video en de locatie voor de uitvoer. U kunt de locatie van uw video opgeven met: HTTPS-URL's, SAS-URL's of assets die zich in uw Media Service-account bevinden.

In dit voorbeeld is de taakinvoer een lokale video.  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#SubmitJob)]

### <a name="wait-for-the-job-to-complete"></a>Wacht tot de taak is voltooid

Het duurt enige tijd voordat de taak is voltooid. Wanneer dit het geval is, wilt u een melding ontvangen. U kunt op verschillende manieren een melding ontvangen over de voltooiing van de [taak](https://docs.microsoft.com/rest/api/media/jobs). De eenvoudigste optie (die hier wordt weer gegeven) is om polling te gebruiken.

Polling is geen aanbevolen best practice voor productie-apps vanwege een mogelijke latentie. Polling kan worden beperkt bij een te intensief gebruik op een account. Ontwikkelaars moeten in plaats daarvan Event Grid gebruiken.

Event Grid is ontworpen voor hoge beschikbaarheid, consistente prestaties en dynamisch schalen. Met Event Grid kunnen uw apps luisteren naar en reageren op gebeurtenissen uit vrijwel alle Azure-services, evenals aangepaste bronnen. Eenvoudige, op HTTP gebaseerde reactieve gebeurtenisafhandeling maakt het mogelijk om efficiënte oplossingen te bouwen met intelligente filtering en routering van gebeurtenissen. Zie voor meer informatie [route gebeurtenissen naar een aangepast webeindpunt](job-state-events-cli-how-to.md).

De **taak** doorloopt meestal de volgende statussen: **gepland**, **in wachtrij**, **wordt verwerkt**, **voltooid** (definitieve status). Als er een fout is opgetreden in de taak, krijgt u de **fout** status. Als de taak wordt geannuleerd, wordt deze **geannuleerd** en vervolgens **geannuleerd** zodra deze is voltooid...

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#WaitForJobToFinish)]

### <a name="job-error-codes"></a>Foutcodes in taak

Zie [Foutcodes](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode).

### <a name="download-the-result-of-the-job"></a>Het resultaat van de taak downloaden

Met de volgende functie worden de resultaten van de uitvoer [Asset](https://docs.microsoft.com/rest/api/media/assets) gedownload naar de map output, zodat u de resultaten van de taak kunt onderzoeken.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#DownloadResults)]

### <a name="clean-up-resource-in-your-media-services-account"></a>Opschonen van resources in uw Media Services-account

Over het algemeen moet u alles opschonen, behalve objecten die u wilt hergebruiken (meestal kunt u trans formaties opnieuw gebruiken en StreamingLocators behouden). Als u wilt dat uw account wordt gereinigd na het experimenteren, verwijdert u de resources die u niet wilt hergebruiken. Met de volgende code worden bijvoorbeeld taken verwijderd:

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CleanUp)]

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Druk op CTRL + F5 om de *AnalyzeVideos* -app uit te voeren.

Wanneer we het programma uitvoeren, produceert de taak miniaturen voor elk gezicht dat in de video wordt gevonden. Het produceert ook het insights.json-bestand.

## <a name="examine-the-output"></a>De uitvoer controleren

Het uitvoerbestand van het analyseren van video's heet insights.json. Dit bestand bevat informatie over uw video. U vindt de beschrijving van elementen die in het json-bestand zijn gevonden in het artikel [Media intelligence](intelligence-concept.md).

## <a name="clean-up-resources"></a>Resources opschonen

Als u de resources van de resourcegroep niet meer nodig hebt, met inbegrip van de Media Services en opslagaccounts die u hebt gemaakt voor deze zelfstudie, verwijdert u de resourcegroep die u eerder hebt gemaakt.

Voer de volgende CLI-opdracht uit:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="multithreading"></a>Multithreading

De Azure Media Services v3-Sdk's zijn niet thread-veilig. Wanneer u werkt met een app met meerdere threads, moet u een nieuw AzureMediaServicesClient-object per thread genereren.

## <a name="ask-questions-give-feedback-get-updates"></a>Vragen stellen, feedback geven, updates ophalen

Bekijk het [Azure Media Services Community](media-services-community.md) -artikel voor verschillende manieren om vragen te stellen, feedback te geven en updates te ontvangen over Media Services.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: Bestanden uploaden, coderen en streamen](stream-files-tutorial-with-api.md)
