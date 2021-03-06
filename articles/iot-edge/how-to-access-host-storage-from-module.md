---
title: Lokale opslag van IoT Edge apparaat gebruiken via een module-Azure IoT Edge | Microsoft Docs
description: Gebruik omgevings variabelen en maak opties voor het inschakelen van module toegang tot lokale opslag van apparaten IoT Edge.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/18/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 079d5845917e63fadcf0466e5a744ed637d704ca
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "75434523"
---
# <a name="give-modules-access-to-a-devices-local-storage"></a>Modules toegang geven tot de lokale opslag van een apparaat

Naast het opslaan van gegevens met behulp van Azure Storage-services of in de container opslag van uw apparaat, kunt u ook opslag op de host IoT Edge apparaat zelf reserveren voor verbeterde betrouw baarheid, vooral wanneer u offline werkt.

## <a name="link-module-storage-to-device-storage"></a>Module opslag koppelen aan opslag van apparaten

Als u een koppeling van module opslag naar de opslag op het hostsysteem wilt inschakelen, maakt u een omgevings variabele voor uw module die verwijst naar een opslagmap in de container. Gebruik vervolgens de opties voor het maken van die opslagmap om deze map te binden aan een map op de hostmachine.

Als u bijvoorbeeld de IoT Edge hub wilt inschakelen om berichten op te slaan op de lokale opslag van uw apparaat en deze later op te halen, kunt u de omgevings variabelen en de opties voor het maken in de Azure Portal in de sectie **runtime-instellingen** configureren.

1. Voor zowel IoT Edge hub als IoT Edge agent voegt u een omgevings variabele toe met de naam **storageFolder** die verwijst naar een map in de module.
1. Voeg voor zowel IoT Edge hub als IoT Edge agent bindingen toe om een lokale map op de hostcomputer te verbinden met een map in de module. Bijvoorbeeld:

   ![Opties voor maken en omgevings variabelen voor lokale opslag toevoegen](./media/how-to-access-host-storage-from-module/offline-storage.png)

U kunt de lokale opslag ook rechtstreeks in het implementatie manifest configureren. Bijvoorbeeld:

```json
"systemModules": {
    "edgeAgent": {
        "settings": {
            "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
            "createOptions": {
                "HostConfig": {
                    "Binds":["<HostStoragePath>:<ModuleStoragePath>"]
                }
            }
        },
        "type": "docker",
        "env": {
            "storageFolder": {
                "value": "<ModuleStoragePath>"
            }
        }
    },
    "edgeHub": {
        "settings": {
            "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
            "createOptions": {
                "HostConfig": {
                    "Binds":["<HostStoragePath>:<ModuleStoragePath>"],
                    "PortBindings":{"5671/tcp":[{"HostPort":"5671"}],"8883/tcp":[{"HostPort":"8883"}],"443/tcp":[{"HostPort":"443"}]}}}
        },
        "type": "docker",
        "env": {
            "storageFolder": {
                "value": "<ModuleStoragePath>"
            }
        },
        "status": "running",
        "restartPolicy": "always"
    }
}
```

Vervang `<HostStoragePath>` en `<ModuleStoragePath>` door het opslagpad van uw host en module; beide waarden moeten een absoluut pad zijn.

Bijvoorbeeld: op een Linux- `"Binds":["/etc/iotedge/storage/:/iotedge/storage/"]` systeem wordt de map **/etc/iotedge/Storage** op uw hostsysteem toegewezen aan de Directory **/iotedge/Storage/** in de container. In een Windows-systeem `"Binds":["C:\\temp:C:\\contemp"]` betekent een ander voor beeld dat de map **c:\\Temp** op uw hostsysteem is toegewezen aan de Directory **c\\:** in de container.

Daarnaast moet u op Linux-apparaten ervoor zorgen dat het gebruikers profiel voor uw module de vereiste machtigingen lezen, schrijven en uitvoeren heeft voor de map van het hostsysteem. Als u terugkeert naar het vorige voor beeld van het inschakelen van IoT Edge hub om berichten op te slaan in de lokale opslag van uw apparaat, moet u machtigingen verlenen aan het gebruikers profiel, UID 1000. (De IoT Edge-agent fungeert als root, zodat er geen aanvullende machtigingen nodig zijn.) Er zijn verschillende manieren om mapmachtigingen te beheren op Linux-systemen, waaronder `chown` het gebruik van om de Directory- `chmod` eigenaar te wijzigen en de machtigingen te wijzigen, zoals:

```bash
sudo chown 1000 <HostStoragePath>
sudo chmod 700 <HostStoragePath>
```

Meer informatie over de opties voor het maken van [docker-documenten](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate)vindt u in.

## <a name="next-steps"></a>Volgende stappen

Zie voor een extra voor beeld van toegang tot host-opslag van een module [gegevens opslaan aan de rand met Azure Blob Storage op IOT Edge](how-to-store-data-blob.md).
