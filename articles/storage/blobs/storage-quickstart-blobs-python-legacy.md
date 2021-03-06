---
title: 'Snelstartgids: Azure Blob Storage-client bibliotheek v 2.1 voor python'
description: In deze snelstart maakt u een opslagaccount en een container in object(blob)-opslag. Vervolgens gebruikt u de Storage-client bibliotheek v 2.1 voor python om een BLOB te uploaden naar Azure Storage, een BLOB te downloaden en de blobs in een container weer te geven.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 01/24/2020
ms.service: storage
ms.subservice: blobs
ms.topic: quickstart
ms.custom: seo-python-october2019
ms.openlocfilehash: 4b0248604b6e9189d5275177a4960e4c352e8215
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "76906433"
---
# <a name="quickstart-manage-blobs-with-python-v21-sdk"></a>Quick Start: blobs beheren met python v 2.1 SDK

In deze Quick Start leert u hoe u blobs beheert met behulp van python. Blobs zijn objecten die grote hoeveel heden tekst of binaire gegevens kunnen bevatten, zoals afbeeldingen, documenten, streaming media en gegevens archivering. U kunt blobs uploaden, downloaden en vermelden en u kunt containers maken en verwijderen.

## <a name="prerequisites"></a>Vereisten

- Een Azure-account met een actief abonnement. [Maak gratis een account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Een Azure Storage-account. [Maak een opslag account](../common/storage-account-create.md).
- [Python](https://www.python.org/downloads/).
- [Azure Storage SDK voor python](https://github.com/Azure/azure-sdk-for-python).

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="download-the-sample-application"></a>De voorbeeldtoepassing downloaden

De [voorbeeldtoepassing](https://github.com/Azure-Samples/storage-blobs-python-quickstart.git) in deze snelstart is een Python-basistoepassing.  

Gebruik de volgende [Git](https://git-scm.com/) -opdracht om de toepassing te downloaden naar uw ontwikkel omgeving. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-python-quickstart.git 
```

Als u het python-programma wilt bekijken, opent u het *example.py* -bestand in de hoofdmap van de opslag plaats.  

[!INCLUDE [storage-copy-account-key-portal](../../../includes/storage-copy-account-key-portal.md)]

## <a name="configure-your-storage-connection-string"></a>De opslagverbindingsreeks configureren

Geef in de toepassing de naam en accountsleutel van uw opslagaccount op om een `BlockBlobService`-object te maken.

1. Open het bestand *example.py* vanuit de Solution Explorer in uw IDE.

1. Vervang de `accountname` waarden `accountkey` en door de naam en sleutel van uw opslag account:

    ```python
    block_blob_service = BlockBlobService(
        account_name='accountname', account_key='accountkey')
    ```

1. Sla het bestand op en sluit het.

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

In het voorbeeld programma wordt een test bestand gemaakt in de map *documenten* , wordt het bestand geüpload naar Blob-opslag, worden de blobs in het bestand weer gegeven en wordt het bestand met een nieuwe naam gedownload.

1. De afhankelijkheden installeren:

    ```console
    pip install azure-storage-blob==2.1.0
    ```

1. Ga naar de voorbeeld toepassing:

    ```console
    cd storage-blobs-python-quickstart
    ```

1. Voer het voorbeeld uit:

    ```console
    python example.py
    ```

    U ziet berichten die vergelijkbaar zijn met de volgende uitvoer:
  
    ```output
    Temp file = C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

    Uploading to Blob storage as blobQuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

    List blobs in the container
             Blob name: QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

    Downloading blob to     C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078_DOWNLOADED.txt
    ```

1. Voordat u doorgaat, gaat u naar de map *documenten* en controleert u op de twee bestanden.

    * *QuickStart_\<Universele unieke id\>*
    * *QuickStart_\<Universele unieke id\>_DOWNLOADED*

1. Als u ze opent, ziet u dat ze hetzelfde zijn.

    U kunt ook een hulp programma gebruiken zoals de [Azure Storage Explorer](https://storageexplorer.com). Het is handig voor het weer geven van de bestanden in Blob Storage. Azure Storage Explorer is een gratis hulp programma voor meerdere platforms waarmee u toegang krijgt tot de gegevens van uw opslag account. 

1. Nadat u de bestanden hebt bekeken, drukt u op een wille keurige toets om het voor beeld te volt ooien en de test bestanden te verwijderen.

## <a name="learn-about-the-sample-code"></a>Meer informatie over de voorbeeld code

Nu u weet wat het voorbeeldprogramma doet, opent u het bestand *example.py* om de code te bekijken.

### <a name="get-references-to-the-storage-objects"></a>Verwijzingen naar de opslagobjecten ophalen

In deze sectie gaat u exemplaren maken van de objecten, een nieuwe container maken en vervolgens machtigingen instellen voor de container, zodat de blobs openbaar zijn. U belt de container `quickstartblobs`. 

```python
# Create the BlockBlockService that the system uses to call the Blob service for the storage account.
block_blob_service = BlockBlobService(
    account_name='accountname', account_key='accountkey')

# Create a container called 'quickstartblobs'.
container_name = 'quickstartblobs'
block_blob_service.create_container(container_name)

# Set the permission so the blobs are public.
block_blob_service.set_container_acl(
    container_name, public_access=PublicAccess.Container)
```

Eerst maakt u verwijzingen naar objecten die worden gebruikt om toegang te krijgen tot de Blob-opslag en deze te beheren. Deze objecten worden boven op elkaar gebouwd en elk ervan wordt door de volgende in de lijst gebruikt.

* Maak een exemplaar van het object **BlockBlobService** dat naar de Blob-service in uw opslagaccount verwijst. 

* Maak een exemplaar van het **CloudBlobContainer**-object dat de container vertegenwoordigt die u wilt openen. Het systeem gebruikt containers om uw blobs te organiseren, net zoals u mappen op uw computer gebruikt om uw bestanden te organiseren.

Wanneer u de Cloud Blob-container hebt, instantieert u het **CloudBlockBlob**-object dat wijst naar de specifieke blob waarin u bent geïnteresseerd. Daarna kunt u de blob uploaden, downloaden en kopiëren wanneer dat nodig is.

> [!IMPORTANT]
> Containernamen moeten uit kleine letters bestaan. Zie [containers, blobs en meta gegevens benoemen en hiernaar verwijzen](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)voor meer informatie over container-en BLOB-namen.

### <a name="upload-blobs-to-the-container"></a>Blobs uploaden naar de container

Blob-opslag ondersteunt blok-blobs, toevoeg-blobs en pagina-blobs. Blok-blobs kunnen tot 4,7 TB groot zijn en kunnen van alles zijn: van Excel-spreadsheets tot grote videobestanden. U kunt toevoeg-blobs gebruiken voor logboek registratie wanneer u naar een bestand wilt schrijven en vervolgens meer gegevens wilt blijven toevoegen. Pagina-blobs worden voornamelijk gebruikt voor de VHD-bestanden (virtuele harde schijf) die een back-infra structuur als een service-virtuele machine (IaaS Vm's) hebben. Blok-blobs worden het meest gebruikt. Deze Snelstartgids maakt gebruik van blok-blobs.

Als u een bestand naar een blob wilt uploaden, hebt u het volledige bestandspad nodig. U verkrijgt dit door de naam van de map en de bestandsnaam op uw lokale schijf samen te voegen. Daarna kunt u het bestand met de methode `create_blob_from_path` uploaden naar het opgegeven pad. 

Met de voorbeeld code wordt een lokaal bestand gemaakt dat door het systeem wordt gebruikt voor het uploaden en downloaden. het bestand wordt opgeslagen als *full_path_to_file* en de naam van de blob als *local_file_name*. In dit voor beeld wordt het bestand geüpload naar de container `quickstartblobs`met de naam:

```python
# Create a file in Documents to test the upload and download.
local_path = os.path.expanduser("~\Documents")
local_file_name = "QuickStart_" + str(uuid.uuid4()) + ".txt"
full_path_to_file = os.path.join(local_path, local_file_name)

# Write text to the file.
file = open(full_path_to_file, 'w')
file.write("Hello, World!")
file.close()

print("Temp file = " + full_path_to_file)
print("\nUploading to Blob storage as blob" + local_file_name)

# Upload the created file, use local_file_name for the blob name.
block_blob_service.create_blob_from_path(
    container_name, local_file_name, full_path_to_file)
```

Er zijn meerdere uploadmethoden die u kunt gebruiken met Blob-opslag. Als u bijvoorbeeld een geheugenstroom hebt, kunt u de methode `create_blob_from_stream` gebruiken in plaats van `create_blob_from_path`. 

### <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

Met de volgende code maakt `generator` u een `list_blobs` voor de-methode. De code loopt door in de lijst met blobs in de container en drukt hun namen af op de-console.

```python
# List the blobs in the container.
print("\nList blobs in the container")
generator = block_blob_service.list_blobs(container_name)
for blob in generator:
    print("\t Blob name: " + blob.name)
```

### <a name="download-the-blobs"></a>De blobs downloaden


Down load blobs naar uw lokale schijf met `get_blob_to_path` behulp van de methode.
Met de volgende code wordt de BLOB gedownload die u eerder hebt geüpload. Het systeem voegt *_DOWNLOADED* toe aan de naam van de blob, zodat u beide bestanden op de lokale schijf kunt zien.

```python
# Download the blob(s).
# Add '_DOWNLOADED' as prefix to '.txt' so you can see both files in Documents.
full_path_to_file2 = os.path.join(local_path, local_file_name.replace(
   '.txt', '_DOWNLOADED.txt'))
print("\nDownloading blob to " + full_path_to_file2)
block_blob_service.get_blob_to_path(
    container_name, local_file_name, full_path_to_file2)
```

### <a name="clean-up-resources"></a>Resources opschonen
Als u de blobs die in deze snelstart zijn geüpload niet langer nodig hebt, kunt u de volledige container verwijderen met de methode `delete_container`. Als u in plaats daarvan afzonderlijke bestanden wilt verwijderen, gebruikt u de methode `delete_blob`.

```python
# Clean up resources. This includes the container and the temp files.
block_blob_service.delete_container(container_name)
os.remove(full_path_to_file)
os.remove(full_path_to_file2)
```

## <a name="resources-for-developing-python-applications-with-blobs"></a>Resources voor het ontwikkelen van Python-toepassingen met blobs

Zie de volgende aanvullende bronnen voor meer informatie over python-ontwikkeling met Blob-opslag:

### <a name="binaries-and-source-code"></a>Binaire bestanden en broncode

- Bekijk, download en installeer de [broncode voor de Python-clientbibliotheek](https://github.com/Azure/azure-storage-python) voor Azure Storage op GitHub.

### <a name="client-library-reference-and-samples"></a>Naslaginformatie en voorbeelden voor de .NET-clientbibliotheek

- Zie de [Azure Storage libraries voor python](https://docs.microsoft.com/python/api/overview/azure/storage)voor meer informatie over de python-client bibliotheek.
- Verken met behulp van de Python-clientbibliotheek geschreven [Blob-opslagvoorbeelden](https://azure.microsoft.com/resources/samples/?sort=0&service=storage&platform=python&term=blob).

## <a name="next-steps"></a>Volgende stappen
 
In deze snelstart hebt u geleerd hoe u bestanden overbrengt tussen een lokale schijf en Azure Blob-opslag met behulp van Python. 

Zie [Azure Blob Storage-resources beheren met Storage Explorer](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)voor meer informatie over de Storage Explorer en blobs.
