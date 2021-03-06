---
title: 'Snelstartgids: Text Analytics-client bibliotheek voor ruby | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: In deze Quick Start wordt taal gedetecteerd met behulp van de ruby Text Analytics-client bibliotheek van Azure Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 02/26/2020
ms.author: aahi
ms.openlocfilehash: 039a52c9ab0bfc460116e48086c854f4d7e8efb4
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2020
ms.locfileid: "82996970"
---
# <a name="quickstart-use-the-text-analytics-client-library-for-ruby"></a>Snelstartgids: de Text Analytics-client bibliotheek voor ruby gebruiken

Aan de slag met de Text Analytics-client bibliotheek. Volg deze stappen om het pakket te installeren en de voorbeeld code voor basis taken uit te proberen.

De Text Analytics-client bibliotheek gebruiken om uit te voeren:

* Sentimentanalyse
* Taaldetectie
* Herkenning van entiteiten
* Sleuteltermextractie

> [!NOTE]
> Deze Quick start is alleen van toepassing op Text Analytics versie 2,1. Op dit moment is er geen v3-client bibliotheek voor ruby beschikbaar.

[Bron code](https://github.com/Azure/azure-sdk-for-ruby/tree/master/data/azure_cognitiveservices_textanalytics)  |  van bibliotheek [Pakket (rubygems)](https://rubygems.org/gems/azure_cognitiveservices_textanalytics)  |  Voor [beelden](https://github.com/Azure-Samples/cognitive-services-quickstart-code)

<a name="HOLTop"></a>

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement: [Maak er gratis een](https://azure.microsoft.com/free/)
* De huidige versie van [ruby](https://www.ruby-lang.org/)
* Wanneer u uw Azure-abonnement hebt, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title=" maakt u een Text Analytics resource Maak "  target="_blank"> een Text Analytics resource <span class="docon docon-navigate-external x-hidden-focus"></span> </a> in de Azure Portal om uw sleutel en eind punt op te halen. 
    * U hebt de sleutel en het eind punt nodig van de resource die u maakt om de toepassing te verbinden met de Text Analytics-API. U gaat later in de Quick Start.
    * U kunt de gratis prijs categorie gebruiken om de service te proberen en later te upgraden naar een betaalde laag voor productie.

## <a name="setting-up"></a>Instellen

### <a name="create-a-new-ruby-application"></a>Een nieuwe ruby-toepassing maken

Maak in een console venster (zoals cmd, Power shell of bash) een nieuwe map voor uw app en navigeer ernaar. Maak vervolgens een bestand `GemFile` met de naam en een ruby-bestand voor uw code.

```console
mkdir myapp && cd myapp
```

Voeg in uw `GemFile` de volgende regels toe om de client bibliotheek toe te voegen als een afhankelijkheid.

```ruby
source 'https://rubygems.org'
gem 'azure_cognitiveservices_textanalytics', '~>0.17.3'
```

Importeer in uw ruby-bestand de volgende pakketten.

[!code-ruby[Import statements](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=includeStatement)]

Maak variabelen voor het Azure-eind punt en de sleutel van uw resource. 

[!INCLUDE [text-analytics-find-resource-information](../includes/find-azure-resource-info.md)]

```ruby
const subscription_key = '<paste-your-text-analytics-key-here>'
const endpoint = `<paste-your-text-analytics-endpoint-here>`
```

## <a name="object-model"></a>Object model 

De Text Analytics-client wordt met uw sleutel geverifieerd bij Azure. De-client biedt verschillende methoden voor het analyseren van tekst, als één teken reeks of een batch. 

Tekst wordt naar de API verzonden als een lijst met `documents` `dictionary` objecten die een combi natie van `id` , en kenmerken bevatten, afhankelijk van `text` `language` de gebruikte methode. Het `text` kenmerk slaat de tekst op die moet worden geanalyseerd in de oorsprong `language` , en `id` kan een wille keurige waarde zijn. 

Het antwoord object is een lijst met de analyse-informatie voor elk document. 

## <a name="code-examples"></a>Codevoorbeelden

Deze code fragmenten laten zien hoe u het volgende kunt doen met de Text Analytics-client bibliotheek voor ruby:

* [De client verifiëren](#authenticate-the-client)
* [Sentimentanalyse](#sentiment-analysis)
* [Taal detectie](#language-detection)
* [Entiteit herkenning](#entity-recognition)
* [Extractie van sleutel woorden](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>De client verifiëren

Maak een klasse met de naam `TextAnalyticsClient` . 

```ruby
class TextAnalyticsClient
  @textAnalyticsClient
  #...
end
```

Maak in deze klasse een functie die wordt aangeroepen `initialize` om de client te verifiëren met behulp van uw sleutel en eind punt. 

[!code-ruby[initialize function for authentication](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=initialize)]

Buiten de-klasse gebruikt u de functie van de client `new()` om deze te instantiëren.

[!code-ruby[client creation](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=clientCreation)] 

<a name="SentimentAnalysis"></a>

## <a name="sentiment-analysis"></a>Sentimentanalyse

Maak in het client object een functie met de naam `AnalyzeSentiment()` die een lijst met invoer documenten gebruikt die later zullen worden gemaakt. Roep de functie van de client `sentiment()` aan en ontvang het resultaat. Herhaal vervolgens de resultaten en druk de document-ID en sentiment Score af. Een score dichter bij 0 geeft aan dat er een negatieve sentiment is, terwijl een score dichter bij 1 wijst op een positieve sentiment.

[!code-ruby[client method for sentiment analysis](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=analyzeSentiment)] 

Maak buiten de client functie een nieuwe functie `SentimentAnalysisExample()` met de naam die het object maakt dat `TextAnalyticsClient` eerder is gemaakt. Maak een lijst met `MultiLanguageInput` objecten die de documenten bevatten die u wilt analyseren. Elk object bevat een `id` , `Language` en een- `text` kenmerk. Het `text` kenmerk slaat de tekst op die moet worden geanalyseerd, `language` is de taal van het document en de `id` kan een wille keurige waarde zijn. Roep vervolgens de functie van de client aan `AnalyzeSentiment()` .

[!code-ruby[sentiment analysis document creation and call](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=sentimentCall)] 

Roep de functie `SentimentAnalysisExample()` aan.

```ruby
SentimentAnalysisExample(textAnalyticsClient)
```

### <a name="output"></a>Uitvoer

```console
===== SENTIMENT ANALYSIS =====
Document ID: 1 , Sentiment Score: 0.87
Document ID: 2 , Sentiment Score: 0.11
Document ID: 3 , Sentiment Score: 0.44
Document ID: 4 , Sentiment Score: 1.00
```

<a name="LanguageDetection"></a>

## <a name="language-detection"></a>Taaldetectie

Maak in het client object een functie met de naam `DetectLanguage()` die een lijst met invoer documenten gebruikt die later zullen worden gemaakt. Roep de functie van de client `detect_language()` aan en ontvang het resultaat. Herhaal vervolgens de resultaten en druk de document-ID en de gedetecteerde taal af.

[!code-ruby[client method for language detection](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=detectLanguage)] 

Maak buiten de client functie een nieuwe functie `DetectLanguageExample()` met de naam die het object maakt dat `TextAnalyticsClient` eerder is gemaakt. Maak een lijst met `LanguageInput` objecten die de documenten bevatten die u wilt analyseren. Elk object bevat een `id` , en een- `text` kenmerk. Het `text` kenmerk slaat de tekst die moet worden geanalyseerd en de `id` kan een wille keurige waarde zijn. Roep vervolgens de functie van de client aan `DetectLanguage()` .

[!code-ruby[language detection document creation and call](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=detectLanguageCall)] 

Roep de functie `DetectLanguageExample()` aan.

```ruby
DetectLanguageExample(textAnalyticsClient)
```

### <a name="output"></a>Uitvoer

```console
===== LANGUAGE EXTRACTION ======
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

<a name="EntityRecognition"></a>

## <a name="entity-recognition"></a>Herkenning van entiteiten

Maak in het client object een functie met de naam `RecognizeEntities()` die een lijst met invoer documenten gebruikt die later zullen worden gemaakt. Roep de functie van de client `entities()` aan en ontvang het resultaat. Herhaal vervolgens de resultaten en druk de ID van elk document af en de herkende entiteiten.

[!code-ruby[client method for entity recognition](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=recognizeEntities)]

Maak buiten de client functie een nieuwe functie `RecognizeEntitiesExample()` met de naam die het object maakt dat `TextAnalyticsClient` eerder is gemaakt. Maak een lijst met `MultiLanguageInput` objecten die de documenten bevatten die u wilt analyseren. Elk object bevat een `id` , een `language` en een- `text` kenmerk. Het `text` kenmerk slaat de tekst op die moet worden geanalyseerd, `language` is de taal van de tekst en het `id` kan een wille keurige waarde zijn. Roep vervolgens de functie van de client aan `RecognizeEntities()` .

[!code-ruby[entity recognition documents and method call](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=recognizeEntitiesCall)] 

Roep de functie `RecognizeEntitiesExample()` aan.

```ruby
RecognizeEntitiesExample(textAnalyticsClient)
```

### <a name="output"></a>Uitvoer

```console
===== ENTITY RECOGNITION =====
Document ID: 1
        Name: Microsoft,        Type: Organization,     Sub-Type: N/A
        Offset: 0, Length: 9,   Score: 1.0

        Name: Bill Gates,       Type: Person,   Sub-Type: N/A
        Offset: 25, Length: 10, Score: 0.999847412109375

        Name: Paul Allen,       Type: Person,   Sub-Type: N/A
        Offset: 40, Length: 10, Score: 0.9988409876823425

        Name: April 4,  Type: Other,    Sub-Type: N/A
        Offset: 54, Length: 7,  Score: 0.8

        Name: April 4, 1975,    Type: DateTime, Sub-Type: Date
        Offset: 54, Length: 13, Score: 0.8

        Name: BASIC,    Type: Other,    Sub-Type: N/A
        Offset: 89, Length: 5,  Score: 0.8

        Name: Altair 8800,      Type: Other,    Sub-Type: N/A
        Offset: 116, Length: 11,        Score: 0.8

Document ID: 2
        Name: Microsoft,        Type: Organization,     Sub-Type: N/A
        Offset: 21, Length: 9,  Score: 0.999755859375

        Name: Redmond (Washington),     Type: Location, Sub-Type: N/A
        Offset: 60, Length: 7,  Score: 0.9911284446716309

        Name: 21 kilómetros,    Type: Quantity, Sub-Type: Dimension
        Offset: 71, Length: 13, Score: 0.8

        Name: Seattle,  Type: Location, Sub-Type: N/A
        Offset: 88, Length: 7,  Score: 0.9998779296875
```

<a name="KeyPhraseExtraction"></a>

## <a name="key-phrase-extraction"></a>Sleuteltermextractie

Maak in het client object een functie met de naam `ExtractKeyPhrases()` die een lijst met invoer documenten gebruikt die later zullen worden gemaakt. Roep de functie van de client `key_phrases()` aan en ontvang het resultaat. Herhaal vervolgens de resultaten en druk de document-ID en de geëxtraheerde sleutel zinnen af.

[!code-ruby[key phrase extraction client method](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=extractKeyPhrases)] 

Maak buiten de client functie een nieuwe functie `KeyPhraseExtractionExample()` met de naam die het object maakt dat `TextAnalyticsClient` eerder is gemaakt. Maak een lijst met `MultiLanguageInput` objecten die de documenten bevatten die u wilt analyseren. Elk object bevat een `id` , een `language` en een- `text` kenmerk. Het `text` kenmerk slaat de tekst op die moet worden geanalyseerd, `language` is de taal van de tekst en het `id` kan een wille keurige waarde zijn. Roep vervolgens de functie van de client aan `ExtractKeyPhrases()` .

[!code-ruby[key phrase document creation and call](~/cognitive-services-ruby-sdk-samples/samples/text_analytics.rb?name=keyPhrasesCall)]


Roep de functie `KeyPhraseExtractionExample()` aan.

```ruby
KeyPhraseExtractionExample(textAnalyticsClient)
```

### <a name="output"></a>Uitvoer

```console
Document ID: 1
         Key phrases:
                幸せ
Document ID: 2
         Key phrases:
                Stuttgart
                Hotel
                Fahrt
                Fu
Document ID: 3
         Key phrases:
                cat
                rock
Document ID: 4
         Key phrases:
                fútbol
```
