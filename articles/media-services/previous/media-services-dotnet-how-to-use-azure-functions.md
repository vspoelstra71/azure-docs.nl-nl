---
title: Azure Functions ontwikkelen met Media Services
description: In dit onderwerp wordt beschreven hoe u Azure Functions ontwikkelt met Media Services met behulp van de Azure Portal.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 51bdcb01-1846-4e1f-bd90-70020ab471b0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 618acae10b874eb5ebd5b6da7fe081368528dbd8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "79251165"
---
# <a name="develop-azure-functions-with-media-services"></a>Azure Functions ontwikkelen met Media Services

In dit artikel wordt beschreven hoe u aan de slag gaat met het maken van Azure Functions die gebruikmaken van Media Services. Met de Azure-functie die in dit artikel is gedefinieerd, wordt een opslag account container met de naam **input** voor nieuwe MP4-bestanden bewaakt. Zodra een bestand in de opslag container wordt neergezet, voert de BLOB-trigger de functie uit. Zie overzicht en andere onderwerpen in het gedeelte **Azure functions** voor meer [informatie](../../azure-functions/functions-overview.md) over Azure functions.

Als u bestaande Azure Functions wilt verkennen en implementeren die gebruikmaken van Azure Media Services, raadpleegt u [Media Services Azure functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration). Deze opslag plaats bevat voor beelden die Media Services gebruiken om werk stromen weer te geven die zijn gerelateerd aan het opnemen van inhoud rechtstreeks vanuit Blob Storage, versleuteling en het schrijven van inhoud naar Blob Storage. Het bevat ook voor beelden van het bewaken van taak meldingen via webhooks en Azure queues. U kunt ook uw functies ontwikkelen op basis van de voor beelden in de [Media Services Azure functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) opslag plaats. Als u de functies wilt implementeren, klikt u op de knop **implementeren naar Azure** .

## <a name="prerequisites"></a>Vereisten

- Voordat u uw eerste functie kunt maken, moet u een actief Azure-account hebben. Als u nog geen Azure-account hebt, zijn er [gratis accounts beschikbaar](https://azure.microsoft.com/free/).
- Als u Azure Functions wilt maken die acties uitvoeren op uw Azure Media Services-account (AMS) of als u wilt Luis teren naar gebeurtenissen die door Media Services worden verzonden, moet u een AMS-account maken, zoals [hier](media-services-portal-create-account.md)wordt beschreven.
    
## <a name="create-a-function-app"></a>Een functie-app maken

1. Ga naar de [Azure-portal](https://portal.azure.com) en meld u aan met uw Azure-account.
2. Maak een functie-app, zoals [hier](../../azure-functions/functions-create-function-app-portal.md)wordt beschreven.

>[!NOTE]
> Een opslag account dat u opgeeft in de omgevings variabele **StorageConnection** (Zie de volgende stap) moet zich in dezelfde regio bevinden als uw app.

## <a name="configure-function-app-settings"></a>Instellingen voor de functie-app configureren

Wanneer u Media Services functies ontwikkelt, is het handig om omgevings variabelen toe te voegen die tijdens uw functies worden gebruikt. Als u de app-instellingen wilt configureren, klikt u op de koppeling app-instellingen configureren. Zie [How to configure Azure function app Settings](../../azure-functions/functions-how-to-use-azure-function-app-settings.md)(Engelstalig) voor meer informatie. 

De functie die in dit artikel is gedefinieerd, veronderstelt dat u de volgende omgevings variabelen in de app-instellingen hebt:

**AMSAADTenantDomain**: Azure AD-Tenant eindpunt. Zie [Dit](media-services-use-aad-auth-to-access-ams-api.md) artikel voor meer informatie over het maken van verbinding met de AMS-API.

**AMSRESTAPIEndpoint**: een URI die het rest API-eind punt vertegenwoordigt. 

**AMSClientId**: client-id van Azure AD-toepassing.

**AMSClientSecret**: client geheim van Azure AD-toepassing.

**StorageConnection**: opslag verbinding van het account dat is gekoppeld aan het Media Services-account. Deze waarde wordt gebruikt in het bestand **Function. json** en **Run. CSX** (zie hieronder).

## <a name="create-a-function"></a>Een functie maken

Als uw functie-app is geïmplementeerd, kunt u deze vinden in **App Services** Azure functions.

1. Selecteer de functie-app en klik op **nieuwe functie**.
2. Kies de **C#** -taal en het scenario voor **gegevens verwerking** .
3. Kies **sjabloon blobtrigger** -sjabloon. Deze functie wordt geactiveerd wanneer een BLOB wordt geüpload naar de **invoer** container. De naam van de **invoer** wordt in de volgende stap in het **pad**opgegeven.

    ![bestanden](./media/media-services-azure-functions/media-services-azure-functions004.png)

4. Wanneer u **sjabloon blobtrigger**selecteert, worden er nog meer besturings elementen op de pagina weer gegeven.

    ![bestanden](./media/media-services-azure-functions/media-services-azure-functions005.png)

4. Klik op **maken**. 

## <a name="files"></a>Bestanden

Uw Azure-functie is gekoppeld aan code bestanden en andere bestanden die in deze sectie worden beschreven. Wanneer u de Azure Portal gebruikt om een functie, **Function. json** en **Run. CSX** te maken, worden voor u gemaakt. U moet een **project. json** -bestand toevoegen of uploaden. In de rest van deze sectie vindt u een korte uitleg van elk bestand en worden de bijbehorende definities weer gegeven.

![bestanden](./media/media-services-azure-functions/media-services-azure-functions003.png)

### <a name="functionjson"></a>function.json

Het bestand function. json definieert de functie bindingen en andere configuratie-instellingen. De runtime gebruikt dit bestand om te bepalen welke gebeurtenissen moeten worden bewaakt en hoe gegevens moeten worden door gegeven en hoe de uitvoering van de functie kan worden geretourneerd. Zie [Azure functions http en webhook-bindingen](../../azure-functions/functions-reference.md#function-code)voor meer informatie.

>[!NOTE]
>Stel de eigenschap **disabled** in op **True** om te voor komen dat de functie wordt uitgevoerd. 

Vervang de inhoud van het bestaande function. JSON-bestand door de volgende code:

```json
{
  "bindings": [
    {
      "name": "myBlob",
      "type": "blobTrigger",
      "direction": "in",
      "path": "input/{filename}.mp4",
      "connection": "ConnectionString"
    }
  ],
  "disabled": false
}
```

### <a name="projectjson"></a>project. json

Het bestand project. json bevat afhankelijkheden. Hier volgt een voor beeld van het bestand **project. json** dat de vereiste .net Azure Media Services-pakketten van Nuget bevat. Houd er rekening mee dat de versie nummers worden gewijzigd met de meest recente updates voor de pakketten. Daarom moet u de meest recente versies bevestigen. 

Voeg de volgende definitie toe aan project. json. 

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "windowsazure.mediaservices": "4.0.0.4",
        "windowsazure.mediaservices.extensions": "4.0.0.4",
        "Microsoft.IdentityModel.Clients.ActiveDirectory": "3.13.1",
        "Microsoft.IdentityModel.Protocol.Extensions": "1.0.2.206221351"
      }
    }
   }
}

```
    
### <a name="runcsx"></a>run. CSX

Dit is de C#-code voor uw functie.  Met de functie die hieronder is gedefinieerd, wordt een opslag account container met de naam **input** (die is opgegeven in het pad) voor nieuwe MP4-bestanden gecontroleerd. Zodra een bestand in de opslag container wordt neergezet, voert de BLOB-trigger de functie uit.
    
In het voor beeld dat in deze sectie is gedefinieerd, ziet u 

1. het opnemen van een asset in een Media Services-account (door een BLOB te omgaan in een AMS-Asset) en 
2. een coderings taak verzenden die gebruikmaakt van de standaard instelling ' adaptieve streaming ' van Media Encoder Standard.

In het scenario voor een echt leven wilt u waarschijnlijk de voortgang van de taak bijhouden en vervolgens uw versleutelde Asset publiceren. Zie [Azure-Webhooks gebruiken voor het bewaken van Media Services taak meldingen](media-services-dotnet-check-job-progress-with-webhooks.md)voor meer informatie. Zie [Media Services Azure functions](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)voor meer voor beelden.  

Vervang de inhoud van het bestaande run. CSX-bestand door de volgende code: wanneer u klaar bent met het definiëren van de functie, klikt u op **opslaan en uitvoeren**.

```csharp
#r "Microsoft.WindowsAzure.Storage"
#r "Newtonsoft.Json"
#r "System.Web"

using System;
using System.Net;
using System.Net.Http;
using Newtonsoft.Json;
using Microsoft.WindowsAzure.MediaServices.Client;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading;
using System.Threading.Tasks;
using System.IO;
using System.Web;
using Microsoft.Azure;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.Azure.WebJobs;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
  
// Read values from the App.config file.

static readonly string _AADTenantDomain = Environment.GetEnvironmentVariable("AMSAADTenantDomain");
static readonly string _RESTAPIEndpoint = Environment.GetEnvironmentVariable("AMSRESTAPIEndpoint");
 
static readonly string _mediaservicesClientId = Environment.GetEnvironmentVariable("AMSClientId");
static readonly string _mediaservicesClientSecret = Environment.GetEnvironmentVariable("AMSClientSecret");

static readonly string _connectionString = Environment.GetEnvironmentVariable("ConnectionString");  

private static CloudMediaContext _context = null;
private static CloudStorageAccount _destinationStorageAccount = null;

public static void Run(CloudBlockBlob myBlob, string fileName, TraceWriter log)
{
    // NOTE that the variables {fileName} here come from the path setting in function.json
    // and are passed into the  Run method signature above. We can use this to make decisions on what type of file
    // was dropped into the input container for the function. 

    // No need to do any Retry strategy in this function, By default, the SDK calls a function up to 5 times for a 
    // given blob. If the fifth try fails, the SDK adds a message to a queue named webjobs-blobtrigger-poison.

    log.Info($"C# Blob trigger function processed: {fileName}.mp4");
    log.Info($"Media Services REST endpoint : {_RESTAPIEndpoint}");

    try
    {
        AzureAdTokenCredentials tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain,
                            new AzureAdClientSymmetricKey(_mediaservicesClientId, _mediaservicesClientSecret),
                            AzureEnvironments.AzureCloudEnvironment);
 
        AzureAdTokenProvider tokenProvider = new AzureAdTokenProvider(tokenCredentials);
 
        _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

        IAsset newAsset = CreateAssetFromBlob(myBlob, fileName, log).GetAwaiter().GetResult();

        // Step 2: Create an Encoding Job

        // Declare a new encoding job with the Standard encoder
        IJob job = _context.Jobs.Create("Azure Function - MES Job");

        // Get a media processor reference, and pass to it the name of the 
        // processor to use for the specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

        // Create a task with the encoding details, using a custom preset
        ITask task = job.Tasks.AddNew("Encode with Adaptive Streaming",
            processor,
            "Adaptive Streaming",
            TaskOptions.None); 

        // Specify the input asset to be encoded.
        task.InputAssets.Add(newAsset);

        // Add an output asset to contain the results of the job. 
        // This output is specified as AssetCreationOptions.None, which 
        // means the output asset is not encrypted. 
        task.OutputAssets.AddNew(fileName, AssetCreationOptions.None);

        job.Submit();
        log.Info("Job Submitted");

    }
    catch (Exception ex)
    {
        log.Error("ERROR: failed.");
        log.Info($"StackTrace : {ex.StackTrace}");
        throw ex;
    }
}

private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
{
    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

    if (processor == null)
    throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

    return processor;
}

public static async Task<IAsset> CreateAssetFromBlob(CloudBlockBlob blob, string assetName, TraceWriter log){
    IAsset newAsset = null;

    try{
        Task<IAsset> copyAssetTask = CreateAssetFromBlobAsync(blob, assetName, log);
        newAsset = await copyAssetTask;
        log.Info($"Asset Copied : {newAsset.Id}");
    }
    catch(Exception ex){
        log.Info("Copy Failed");
        log.Info($"ERROR : {ex.Message}");
        throw ex;
    }

    return newAsset;
}

/// <summary>
/// Creates a new asset and copies blobs from the specifed storage account.
/// </summary>
/// <param name="blob">The specified blob.</param>
/// <returns>The new asset.</returns>
public static async Task<IAsset> CreateAssetFromBlobAsync(CloudBlockBlob blob, string assetName, TraceWriter log)
{
     //Get a reference to the storage account that is associated with the Media Services account. 
    _destinationStorageAccount = CloudStorageAccount.Parse(_connectionString);

    // Create a new asset. 
    var asset = _context.Assets.Create(blob.Name, AssetCreationOptions.None);
    log.Info($"Created new asset {asset.Name}");

    IAccessPolicy writePolicy = _context.AccessPolicies.Create("writePolicy",
    TimeSpan.FromHours(4), AccessPermissions.Write);
    ILocator destinationLocator = _context.Locators.CreateLocator(LocatorType.Sas, asset, writePolicy);
    CloudBlobClient destBlobStorage = _destinationStorageAccount.CreateCloudBlobClient();

    // Get the destination asset container reference
    string destinationContainerName = (new Uri(destinationLocator.Path)).Segments[1];
    CloudBlobContainer assetContainer = destBlobStorage.GetContainerReference(destinationContainerName);

    try{
    assetContainer.CreateIfNotExists();
    }
    catch (Exception ex)
    {
    log.Error ("ERROR:" + ex.Message);
    }

    log.Info("Created asset.");

    // Get hold of the destination blob
    CloudBlockBlob destinationBlob = assetContainer.GetBlockBlobReference(blob.Name);

    // Copy Blob
    try
    {
    using (var stream = await blob.OpenReadAsync()) 
    {            
        await destinationBlob.UploadFromStreamAsync(stream);          
    }

    log.Info("Copy Complete.");

    var assetFile = asset.AssetFiles.Create(blob.Name);
    assetFile.ContentFileSize = blob.Properties.Length;
    assetFile.IsPrimary = true;
    assetFile.Update();
    asset.Update();
    }
    catch (Exception ex)
    {
    log.Error(ex.Message);
    log.Info (ex.StackTrace);
    log.Info ("Copy Failed.");
    throw;
    }

    destinationLocator.Delete();
    writePolicy.Delete();

    return asset;
}
```

## <a name="test-your-function"></a>Uw functie testen

Als u uw functie wilt testen, moet u een MP4-bestand uploaden naar de **invoer** container van het opslag account dat u in de Connection String hebt opgegeven.  

1. Selecteer het opslag account dat u hebt opgegeven in de omgevings variabele **StorageConnection** .
2. Klik op **blobs**.
3. Klik op **+ Container**. Geef de container **invoer**een naam.
4. Druk op **uploaden** en blader naar een. MP4-bestand dat u wilt uploaden.

>[!NOTE]
> Wanneer u een BLOB-trigger in een verbruiks plan gebruikt, kan er een vertraging van 10 minuten zijn bij het verwerken van nieuwe blobs nadat een functie-app niet actief is geweest. Nadat de functie-app is uitgevoerd, worden de blobs onmiddellijk verwerkt. Zie [Blob Storage-triggers en-bindingen](https://docs.microsoft.com/azure/azure-functions/functions-bindings-storage-blob)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar om te beginnen met het ontwikkelen van een Media Services-toepassing. 
 
Zie voor meer informatie en voor beelden/oplossingen van het gebruik van Azure Functions en Logic Apps met Azure Media Services voor het maken van aangepaste werk stromen voor het maken van inhoud, het [Media Services .net functions Integration sample op github](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)

Zie ook [Azure-Webhooks gebruiken om Media Services taak meldingen te bewaken met .net](media-services-dotnet-check-job-progress-with-webhooks.md). 

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

