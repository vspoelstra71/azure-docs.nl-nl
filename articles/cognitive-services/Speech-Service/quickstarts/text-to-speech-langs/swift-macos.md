---
title: 'Quick Start: spraak op snelle spraak service'
titleSuffix: Azure Cognitive Services
description: Meer informatie over het met behulp van de Speech SDK omzetten van spraak in Swift op macOS
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/09/2019
ms.author: yulili
ms.openlocfilehash: ea0f61ddd2c60d2806af0f6dcf97c7d2388335d0
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "74975870"
---
# <a name="quickstart-synthesize-speech-in-swift-on-macos-using-the-speech-sdk"></a>Quick Start: spraak op macOS in een snelle synthese met de Speech SDK

In dit artikel leert u hoe u in SWIFT een macOS-app kunt maken met behulp van de SDK van Cognitive Services speech om spraak vanuit tekst te synthesizeren en af te spelen met de standaard audio-uitvoer.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, bekijkt u de lijst vereisten:

* Een [abonnements sleutel](~/articles/cognitive-services/Speech-Service/get-started.md) voor de spraak service.
* Een macOS-computer met [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) of hoger en [CocoaPods](https://cocoapods.org/) geïnstalleerd.

## <a name="get-the-speech-sdk-for-macos"></a>De Speech SDK voor macOS ophalen

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

De Cognitive Services Speech SDK voor macOS wordt gedistribueerd als een framework-bundel.
Het kan worden gebruikt in Xcode-projecten als een [CocoaPod](https://cocoapods.org/), of worden https://aka.ms/csspeech/macosbinary gedownload en hand matig worden gekoppeld. Deze hand leiding maakt gebruik van een CocoaPod.

> [!NOTE] 
> Deze zelf studie werkt niet met versies van de SDK die ouder zijn dan 1.7.0.

## <a name="create-an-xcode-project"></a>Een Xcode-project maken

Start Xcode en start een nieuw project door te klikken op **bestand** > **Nieuw** > **project**.
Kies in het dialoog venster sjabloon selectie de sjabloon ' cacao-app '.

Maak in de dialoogvensters die volgen de volgende selecties:

1. Het dialoogvenster Project Options
    1. Voer een naam in voor de snelstart-app, bijvoorbeeld `helloworld`.
    1. Voer de juiste organisatie naam en een organisatie-id in als u al een Apple-ontwikkelaars account hebt. Voor testdoeleinden kunt u elke naam kiezen, zoals `testorg`. Om de app te kunnen ondertekenen, hebt u een geschikt inrichtingsprofiel nodig. Raadpleeg de [Apple-site voor ontwikkelaars](https://developer.apple.com/) voor meer informatie.
    1. Zorg ervoor dat SWIFT is gekozen als de taal voor het project.
    1. Schakel de selectie vakjes uit om Story boards te gebruiken en om een op documenten gebaseerde toepassing te maken. De eenvoudige gebruikers interface voor de voor beeld-app wordt programmatisch gemaakt.
    1. Schakel alle selectievakjes voor tests en essentiële gegevens uit.
1. Projectmap selecteren
    1. Kies een map waarin u het project wilt plaatsen. Hiermee maakt u `helloworld` een map in de gekozen map die alle bestanden voor het Xcode-project bevat.
    1. Schakel het maken van een Git-opslagplaats uit voor dit voorbeeldproject.
1. Sluit het project Xcode. Later gebruikt u een ander exemplaar na het instellen van de CocoaPods.

## <a name="add-the-sample-code"></a>De voorbeeldcode toevoegen

1. Plaats een nieuw header-bestand met de `MicrosoftCognitiveServicesSpeech-Bridging-Header.h` naam in `helloworld` de map in het HelloWorld-project en plak de volgende code in deze.

   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift/macos/text-to-speech/helloworld/helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h#code)]
1. Voeg het relatieve pad `helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h` naar de bridging-header toe aan de SWIFT-project instellingen voor het doel HelloWorld in de kop *-C bridging koptekst* veld ![koptekst eigenschappen](~/articles/cognitive-services/Speech-Service/media/sdk/qs-swift-macos-bridging-header.png)
1. Vervang de inhoud van het automatisch gegenereerde bestand `AppDelegate.swift` door:

   [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift/macos/text-to-speech/helloworld/helloworld/AppDelegate.swift#code)]
1. Vervang `AppDelegate.swift`in de teken reeks `YourSubscriptionKey` door de sleutel van uw abonnement.
1. Vervang de tekenreeks `YourServiceRegion` door de regio die aan uw abonnement is gekoppeld (bijvoorbeeld `westus` voor het gratis proefabonnement).

## <a name="install-the-sdk-as-a-cocoapod"></a>De SDK installeren als een CocoaPod

1. Installeer de CocoaPod dependency manager zoals beschreven in de [installatie-instructies](https://guides.cocoapods.org/using/getting-started.html).
1. Navigeer naar de map van uw voor beeld-`helloworld`app (). Plaats een tekst bestand met de naam `Podfile` en de volgende inhoud in die map:

   [!code-ruby[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift/macos/text-to-speech/helloworld/Podfile)]
1. Ga naar de `helloworld` map in een Terminal en voer de opdracht `pod install`uit. Hiermee wordt een `helloworld.xcworkspace` Xcode-werk ruimte gegenereerd met zowel de voor beeld-app als de spraak-SDK als een afhankelijkheid. Deze werk ruimte wordt gebruikt in het volgende.

## <a name="build-and-run-the-sample"></a>Het voorbeeldproject compileren en uitvoeren

1. Open de `helloworld.xcworkspace` werk ruimte in Xcode.
1. De uitvoer van de fout opsporing zichtbaar maken (de**console****fout opsporing** > **weer geven** > ).
1. Bouw de voorbeeld code en voer deze uit door in het menu **product** > **uitvoeren** te selecteren of te klikken op de knop **afspelen** .
1. Nadat u tekst hebt ingevoerd en op de knop in de app hebt geklikt, moet u horen dat de geteste audio is afgespeeld.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Onze C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

