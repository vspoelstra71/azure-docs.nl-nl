---
title: 'Quick Start: een synthese van spraak, C++ (Linux)-spraak service'
titleSuffix: Azure Cognitive Services
description: Meer informatie over het maken van spraak in C++ op Linux met behulp van de Speech SDK
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/04/2020
ms.author: yinhew
ms.openlocfilehash: 458dc6899b0d89fcda2ff989ae95523e12de13c5
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "81275215"
---
## <a name="prerequisites"></a>Vereisten

Voordat u aan de slag gaat, moet u het volgende doen:

> [!div class="checklist"]
> * [Een Azure-spraak resource maken](../../../../get-started.md)
> * [Stel uw ontwikkel omgeving in en maak een leeg project](../../../../quickstarts/setup-platform.md?tabs=linux&pivots=programming-language-cpp)

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Maak een C++ bronbestand met de naam `helloworld.cpp` en plak er de volgende code in.

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp/linux/text-to-speech/helloworld.cpp#code)]

1. Vervang in dit nieuwe bestand de tekenreeks `YourSubscriptionKey` door uw abonnementssleutel van de Speech-service.

1. Vervang de tekenreeks `YourServiceRegion` door de [regio](~/articles/cognitive-services/Speech-Service/regions.md) die aan uw abonnement is gekoppeld (bijvoorbeeld `westus` voor het gratis proefabonnement).

## <a name="build-the-app"></a>De app bouwen

> [!NOTE]
> Zorg ervoor dat u de onderstaande opdrachten als _één opdrachtregel_ invoert. De eenvoudigste manier om dat te doen is om de opdracht te kopiëren met behulp van de knop **Kopiëren** naast elke opdracht en deze vervolgens bij de shell-prompt te plakken.

* Voer op een **x64** (64-bits) systeem de volgende opdracht uit om de toepassing te compileren.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libasound.so.2
  ```

* Voer op een **x86** (32-bits) systeem de volgende opdracht uit om de toepassing te compileren.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libasound.so.2
  ```

* Voer op een **ARM64** -systeem (64-bits) de volgende opdracht uit om de toepassing te bouwen.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/arm64" -l:libasound.so.2
  ```

## <a name="run-the-app"></a>De app uitvoeren

1. Configureer het bibliotheekpad van het laadprogramma om te verwijzen naar de bibliotheek van de Speech SDK.

   * Voer op een **x64** (64-bits) systeem de volgende opdracht uit.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
     ```

   * Voer op een **x86** (32-bits) systeem de volgende opdracht uit.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
     ```

   * Voer de volgende opdracht in op een **ARM64** -systeem (64-bits).

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/arm64"
     ```

1. Voer de toepassing uit.

   ```sh
   ./helloworld
   ```

1. In het console venster wordt een prompt weer gegeven waarin u wordt gevraagd om tekst te typen. Typ enkele woorden of een zin. De tekst die u hebt getypt, wordt verzonden naar de speech-service en gesynthesizerd naar spraak, die op uw spreker wordt afgespeeld.

   ```text
   Type some text that you want to speak...
   > hello
   Speech synthesized to speaker for text [hello]
   Press enter to exit...
   ```

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]

## <a name="see-also"></a>Zie ook

- [Een aangepaste stem maken](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md)
- [Aangepaste spraak voorbeelden vastleggen](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
