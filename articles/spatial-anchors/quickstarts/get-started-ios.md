---
title: 'Snelstartgids: een iOS-app maken'
description: In deze quickstart leert u een iOS-app maken met behulp van Spatial Anchors.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 02/24/2019
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: 6304077a26f5c0ecb91e1ec4936bd79b3d839d95
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "79471214"
---
# <a name="quickstart-create-an-ios-app-with-azure-spatial-anchors-in-either-swift-or-objective-c"></a>Snelstartgids: een iOS-app maken met behulp van ruimtelijke Azure-ankers in SWIFT of objectief-C

In deze quickstart wordt besproken hoe u een iOS-app maakt met behulp van [Azure Spatial Anchors](../overview.md) in Swift of Objective-C. Azure Spatial Anchors is een platformoverstijgende ontwikkelaarsservice waarmee u mixed reality-ervaringen kunt maken met behulp van objecten die hun locatie in de loop van de tijd op meerdere apparaten behouden. Als u klaar bent, hebt u een ARKit iOS-app gemaakt waarmee een ruimtelijk anker kan worden opgeslagen en teruggehaald.

U leert het volgende:

> [!div class="checklist"]
> * Een Spatial Anchors-account maken
> * De Spatial Anchors-account-id en -accountsleutel configureren
> * Implementeren en uitvoeren op een iOS-apparaat

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u over het volgende beschikt om deze snelstart te voltooien:

- Er is een macOS-computer ingeschakeld met de meest recente versie van <a href="https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12" target="_blank">Xcode</a> en <a href="https://cocoapods.org" target="_blank">CocoaPods</a> geïnstalleerd.
- Git geïnstalleerd via HomeBrew:
  1. Voer de volgende opdracht in als één regel in de terminal: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`. 
  1. Voer `brew install git` en `brew install git-lfs` uit.
  1. Werk uw Git-configuratie `git lfs install` bij met (voor de huidige gebruiker `git lfs install --system` ) of (voor het hele systeem).
- Een door een ontwikkelaar geactiveerd en <a href="https://developer.apple.com/documentation/arkit/verifying_device_support_and_user_permission" target="_blank">met ARKit compatibel</a> iOS-apparaat.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>Voorbeeldproject openen

Gebruik de terminal om de volgende acties uit te voeren.

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

De benodigde pods installeren met CocoaPods:

# <a name="swift"></a>[Swift](#tab/openproject-swift)

Navigeer naar `iOS/Swift/`.

```bash
cd ./iOS/Swift/
```

# <a name="objective-c"></a>[Objective-C](#tab/openproject-objc)

Navigeer naar `iOS/Objective-C/`.

```bash
cd ./iOS/Objective-C/
```

---

Voer `pod install --repo-update` uit om de CocoaPods voor het project te installeren.

Open nu `.xcworkspace` in Xcode.

> [!NOTE]
> Zie de stappen voor het oplossen van [problemen als u](#cocoapods-issues-on-macos-catalina-1015) CocoaPod problemen ondervindt na een upgrade naar macOS Catalina (10,15).

# <a name="swift"></a>[Swift](#tab/openproject-swift)

```bash
open ./SampleSwift.xcworkspace
```

# <a name="objective-c"></a>[Objective-C](#tab/openproject-objc)

```bash
open ./SampleObjC.xcworkspace
```

---

## <a name="configure-account-identifier-and-key"></a>Account-id en -sleutel configureren

De volgende stap is het configureren van de app voor het gebruik van uw account-id en de account sleutel. U hebt deze in een tekst editor gekopieerd bij [het instellen van de bron voor ruimtelijke ankers](#create-a-spatial-anchors-resource).

# <a name="swift"></a>[Swift](#tab/openproject-swift)

Open `iOS/Swift/SampleSwift/ViewControllers/BaseViewController.swift`.

Zoek het veld `spatialAnchorsAccountKey` en vervang `Set me` met de accountsleutel.

Zoek het `spatialAnchorsAccountId`-veld en vervang `Set me` met de account-id.

# <a name="objective-c"></a>[Objective-C](#tab/openproject-objc)

Open `iOS/Objective-C/SampleObjC/BaseViewController.m`.

Zoek het veld `SpatialAnchorsAccountKey` en vervang `Set me` met de accountsleutel.

Zoek het `SpatialAnchorsAccountId`-veld en vervang `Set me` met de account-id.

---

## <a name="deploy-the-app-to-your-ios-device"></a>De app implementeren op uw iOS-apparaat

Verbind het iOS-apparaat met de Mac en stel het **actieve schema** in voor uw iOS-apparaat.

![Apparaat selecteren](./media/get-started-ios/select-device.png)

Selecteer **Build and then run the current scheme** (Het huidige schema compileren en uitvoeren).

![Implementeren en uitvoeren](./media/get-started-ios/deploy-run.png)

> [!NOTE]
> Als u de fout `library not found for -lPods-SampleObjC` ziet, hebt u waarschijnlijk het bestand `.xcodeproj` geopend in plaats van `.xcworkspace`. Open `.xcworkspace` en probeer het opnieuw.

In Xcode: stop de app door op **Stop** te drukken.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="cocoapods-issues-on-macos-catalina-1015"></a>CocoaPods problemen met macOS Catalina (10,15)

Als u onlangs een update hebt uitgevoerd voor macOS Catalina (10,15) en CocoaPods al eerder hebt geïnstalleerd, is CocoaPods mogelijk beschadigd en kan uw peul-en `.xcworkspace` project bestanden niet goed worden geconfigureerd. U kunt dit probleem oplossen door de volgende opdrachten uit te voeren om CocoaPods opnieuw te installeren:

```shell
brew update
brew install cocoapods --build-from-source
brew link --overwrite cocoapods
```

### <a name="app-crashes-when-deploying-to-ios-1031-from-a-personal-provisioning-profiledeveloper-account"></a>App loopt vast bij het implementeren van iOS-10.3.1 vanuit een persoonlijk inrichtings profiel/ontwikkelaars account 

Als u uw iOS-app op iOS 10.3.1 implementeert vanuit een persoonlijk inrichtings profiel/ontwikkelaars account, ziet u mogelijk `Library not loaded: @rpath/ADAL...`de volgende fout:. 

Om het probleem op te lossen:

- Gebruik een inrichtings profiel dat geen persoonlijk team profiel (betaald ontwikkelaars account) is.
- Implementeer uw app op een iOS-apparaat met iOS 13,3 of eerder of op een van de iOS 13,4 bèta-of release versie.
- Meer informatie over dit probleem vindt u op [stack overflow](https://stackoverflow.com/questions/60015309/running-ios-apps-causes-runtime-error-for-frameworks-code-signature-invalid).


[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Zelf studie: ruimtelijke ankers delen op meerdere apparaten](../tutorials/tutorial-share-anchors-across-devices.md)
