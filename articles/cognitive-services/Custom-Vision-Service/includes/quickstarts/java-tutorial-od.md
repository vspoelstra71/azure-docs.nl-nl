---
author: areddish
ms.author: areddish
ms.service: cognitive-services
ms.date: 04/14/2020
ms.openlocfilehash: cf25d69a1f3765613d0d19af8f8f1489d89db1ba
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "82134101"
---
In dit artikel leest u hoe u aan de slag kunt met de Custom Vision SDK met Java om een object detectie model te bouwen. Nadat u deze hebt gemaakt, kunt u gelabelde regio's toevoegen, afbeeldingen uploaden, het project trainen, de standaard eind punt-URL van het project ophalen en het eind punt gebruiken om een afbeelding programmatisch te testen. Gebruik dit voorbeeld als een sjabloon om uw eigen Java-toepassing te maken.

## <a name="prerequisites"></a>Vereisten

- Een Java IDE naar keuze
- [JDK 7 of 8](https://aka.ms/azure-jdks) is geïnstalleerd.
- [Maven](https://maven.apache.org/) geïnstalleerd
- [!INCLUDE [create-resources](../../includes/create-resources.md)]

## <a name="get-the-custom-vision-sdk-and-sample-code"></a>De Custom Vision SDK en voorbeeld code ophalen

Als u een Java-app wilt schrijven die gebruikmaakt van Custom Vision, hebt u de Maven-pakketten van Custom Vision nodig. Deze pakketten zijn opgenomen in het voorbeeld project dat u wilt downloaden, maar u kunt ze hier afzonderlijk openen.

U vindt de Custom Vision SDK in de centrale opslag plaats maven:
- [Training-SDK](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
- [Voorspelling-SDK](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

Kloon of download het project [Voorbeelden voor Cognitive Services Java SDK](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master). Ga naar de map **Vision/CustomVision/**.

Met dit Java-project wordt een nieuwe objectdetectieproject van Custom Vision gemaakt met de naam __Sample Java OD Project__. Dit kan worden geopend via de [Custom Vision-website](https://customvision.ai/). Vervolgens worden afbeeldingen geüpload om een classificatie te trainen en te testen. In dit project is de classificatie bedoeld om te bepalen of een object een **Fork** of **schaar**is.

[!INCLUDE [get-keys](../../includes/get-keys.md)]

Het programma is geconfigureerd om te verwijzen naar de belangrijkste gegevens als omgevings variabelen. Ga naar de map **Vision/CustomVision** en voer de volgende Power shell-opdrachten in om de omgevings variabelen in te stellen. 

> [!NOTE]
> Als u een niet-Windows-besturings systeem gebruikt, raadpleegt u [omgevings variabelen configureren](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account?tabs=multiservice%2Cwindows#configure-an-environment-variable-for-authentication) voor instructies.

```powershell
$env:AZURE_CUSTOMVISION_TRAINING_API_KEY ="<your training api key>"
$env:AZURE_CUSTOMVISION_PREDICTION_API_KEY ="<your prediction api key>"
```

## <a name="understand-the-code"></a>De code begrijpen

Laad het `Vision/CustomVision`-project in uw Java IDE en open het bestand _CustomVisionSamples.java_. Zoek de methode **runSample** en voer een opmerking uit bij de&mdash;aanroep van de **ImageClassification_Sample** methode met deze methode wordt het scenario voor afbeeldings classificatie uitgevoerd, dat niet in deze hand leiding wordt behandeld. De methode **ObjectDetection_Sample** implementeert de primaire functie van deze snelstart, namelijk het navigeren naar de definitie ervan en het inspecteren van de code. 

### <a name="create-a-new-custom-vision-service-project"></a>Een nieuw Custom Vision Service project maken

Ga naar het codeblok waarmee een trainingsclient en een objectdetectieproject worden gemaakt. Het project wordt weergegeven op de [Custom Vision-website](https://customvision.ai/), die u eerder hebt bezocht. Zie de methode [CreateProject](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.customvision.training.trainings.createproject?view=azure-java-stable#com_microsoft_azure_cognitiveservices_vision_customvision_training_Trainings_createProject_String_CreateProjectOptionalParameter_) Overloads om andere opties op te geven wanneer u uw project maakt (zoals beschreven in de hand leiding voor het [bouwen van een detector](../../get-started-build-detector.md) -webportal).

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_create_od)]

### <a name="add-tags-to-your-project"></a>Tags toevoegen aan uw project

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_tags_od)]

### <a name="upload-and-tag-images"></a>Afbeeldingen uploaden en coderen

Wanneer u afbeeldingen in object detectie projecten labelt, moet u de regio van elk gelabeld object opgeven met behulp van genormaliseerde coördinaten. Ga naar de definitie van de kaart `regionMap`. Met deze code wordt elk voorbeeld van een afbeelding aan de bijbehorende gelabelde regio gekoppeld.

> [!NOTE]
> Als u geen hulp programma voor klikken en slepen hebt om de coördinaten van regio's te markeren, kunt u de Web-UI gebruiken op [Customvision.ai](https://www.customvision.ai/). In dit voor beeld zijn de coördinaten al opgenomen.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_od_mapping)]

Ga vervolgens naar het codeblok waarmee de afbeeldingen aan het project worden toegevoegd. De afbeeldingen worden gelezen vanuit de map **src/main/resources** van het project en worden met de bijbehorende labels en regiocoördinaten naar de service geüpload.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_upload_od)]

Het vorige code fragment maakt gebruik van twee hulp functies waarmee de afbeeldingen als resource-streams worden opgehaald en geüpload naar de service (u kunt Maxi maal 64 installatie kopieën in één batch uploaden).

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_helpers)]

### <a name="train-the-project-and-publish"></a>Het project trainen en publiceren

Deze code maakt de eerste iteratie van het Voorspellings model en publiceert die iteratie vervolgens naar het Voorspellings eindpunt. De naam die is opgegeven voor de gepubliceerde herhaling kan worden gebruikt voor het verzenden van voorspellings aanvragen. Er is geen iteratie beschikbaar in het Voorspellings eindpunt totdat het is gepubliceerd.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_train_od)]

### <a name="use-the-prediction-endpoint"></a>Het Voorspellings eindpunt gebruiken

Het voorspellingseindpunt, dat door het `predictor`-object hier wordt weergegeven, is de verwijzing waarmee u een afbeelding naar het huidige model kunt verzenden en een classificatievoorspelling kunt verkrijgen. In dit voorbeeld wordt `predictor` elders gedefinieerd met de omgevingsvariabele voor de voorspellingssleutel.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_prediction_od)]

## <a name="run-the-application"></a>De toepassing uitvoeren

Als u de oplossing wilt compileren en uitvoeren met behulp van Maven, navigeert u naar de projectmap (**Vision/CustomVision**) in een opdracht prompt en voert u de volgende opdracht uit:

```bash
mvn compile exec:java
```

Bekijk de uitvoer in de console voor logboekregistratie en voorspellingsresultaten. Vervolgens kunt u controleren of de testafbeelding op de juiste wijze wordt gelabeld en of de detectieregio juist is.

[!INCLUDE [clean-od-project](../../includes/clean-od-project.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt nu gezien hoe elke stap van de object detectie kan worden uitgevoerd in code. In dit voor beeld wordt één trainings iteratie uitgevoerd, maar vaak moet u het model meerdere keren trainen en testen om het nauw keuriger te maken. De volgende trainings handleiding behandelt de afbeeldings classificatie, maar de principes zijn vergelijkbaar met object detectie.

> [!div class="nextstepaction"]
> [Een model testen en opnieuw trainen](../../test-your-model.md)
