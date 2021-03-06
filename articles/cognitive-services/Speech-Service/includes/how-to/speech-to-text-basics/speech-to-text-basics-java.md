---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/11/2020
ms.author: trbye
ms.openlocfilehash: d59f2ef564da710d084dc027950bc983a88143de
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "82204645"
---
## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u een Azure-account en een spraak service-abonnement hebt. Als u geen account en abonnement hebt, [kunt u de spraak service gratis uitproberen](../../../get-started.md).

## <a name="install-the-speech-sdk"></a>De Speech-SDK installeren

Voordat u iets kunt doen, moet u de Speech SDK installeren. Gebruik de volgende instructies, afhankelijk van uw platform:

* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=jre&pivots=programming-language-java" target="_blank">Java-runtime<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=android&pivots=programming-language-java" target="_blank">Android<span class="docon docon-navigate-external x-hidden-focus"></span></a>

## <a name="create-a-speech-configuration"></a>Een spraak configuratie maken

Als u de spraak service wilt aanroepen met behulp van de Speech SDK [`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable), moet u een maken. Deze klasse bevat informatie over uw abonnement, zoals uw sleutel en de bijbehorende regio, het eind punt, de host of het autorisatie token.

> [!NOTE]
> Ongeacht of u spraak herkenning, spraak synthese, vertaling of intentie herkenning uitvoert, maakt u altijd een configuratie.

Er zijn een paar manieren waarop u een [`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable)kunt initialiseren:

* Met een abonnement: Geef een sleutel en de bijbehorende regio door.
* Met een eind punt: Pass in een speech service-eind punt. Een sleutel-of autorisatie token is optioneel.
* Met een host: Geef een hostadres door. Een sleutel-of autorisatie token is optioneel.
* Met een autorisatie token: Geef een autorisatie token en de bijbehorende regio door.

Laten we eens kijken hoe een [`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable) is gemaakt met behulp van een sleutel en regio. Bekijk de [ondersteunings](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk) pagina voor regio's om uw regio-id te vinden.

```java
SpeechConfig config = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

## <a name="initialize-a-recognizer"></a>Een herkenner initialiseren

Nadat u een [`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable)hebt gemaakt, is de volgende stap het initialiseren van [`SpeechRecognizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable)een. Wanneer u een [`SpeechRecognizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable)initialiseert, moet u het door geven aan uw `config`. Dit biedt de referenties die de speech-service nodig heeft om uw aanvraag te valideren.

Als u spraak wilt herkennen met de standaard microfoon van uw apparaat, ziet u het [`SpeechRecognizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable) volgende:

```java
SpeechRecognizer recognizer = new SpeechRecognizer(config);
```

Als u het audio-invoer apparaat wilt opgeven, moet u een [`AudioConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.audio.audioconfig?view=azure-java-stable) maken en de `audioConfig` para meter opgeven bij het initialiseren van uw [`SpeechRecognizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable).

> [!TIP]
> [Meer informatie over het ophalen van de apparaat-id voor het apparaat voor audio-invoer](../../../how-to-select-audio-input-devices.md).

Voeg eerst de volgende `import` -instructies toe.

```java
import java.util.concurrent.Future;
import com.microsoft.cognitiveservices.speech.*;
```

Daarna kunt u als volgt naar het `AudioConfig` object verwijzen:

```java
AudioConfig audioConfig = AudioConfig.fromDefaultMicrophoneInput();
SpeechRecognizer recognizer = new SpeechRecognizer(config, audioConfig);
```

Als u een audio bestand wilt opgeven in plaats van een microfoon te gebruiken, moet u nog steeds een `audioConfig`opgeven. Wanneer [`AudioConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.audio.audioconfig?view=azure-java-stable)u echter een maakt in plaats van het aanroepen `fromDefaultMicrophoneInput`van, roept `fromWavFileOutput` u de para `filename` meter op en geeft u deze door.

```java
AudioConfig audioConfig = AudioConfig.fromWavFileInput("YourAudioFile.wav");
SpeechRecognizer recognizer = new SpeechRecognizer(config, audioConfig);
```

## <a name="recognize-speech"></a>Spraak herkennen

De [klasse Recognizer](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable) voor de Speech-SDK voor Java bevat enkele methoden die u voor spraak herkenning kunt gebruiken.

* Eenmalige herkenning (asynchroon): voert herkenning uit in een niet-blokkerende (asynchrone) modus. Hiermee wordt één utterance herkend. Het einde van één utterance wordt bepaald door te Luis teren naar stilte aan het einde of tot een maximum van 15 seconden audio wordt verwerkt.
* Continue herkenning (asynchroon): initieert asynchrone herkennings bewerking. Als u een audio bestand wilt opgeven in plaats van een microfoon te gebruiken, moet u nog steeds een `audioConfig`opgeven. Roep [stopContinuousRecognitionAsync](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.stopcontinuousrecognitionasync)aan om asynchrone doorlopende herkenning te stoppen.

> [!NOTE]
> Meer informatie over het [kiezen van een modus voor spraak herkenning](../../../how-to-choose-recognition-mode.md).

### <a name="single-shot-recognition"></a>Eenmalige herkenning

Hier volgt een voor beeld van asynchrone eenmalige herkenning met [`recognizeOnceAsync`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.recognizeonceasync?view=azure-java-stable):

```java
Future<SpeechRecognitionResult> task = recognizer.recognizeOnceAsync();
SpeechRecognitionResult result = task.get();
```

U moet een code schrijven om het resultaat te kunnen afhandelen. In dit voor beeld wordt [`result.getReason()`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.resultreason?view=azure-java-stable)het volgende geëvalueerd:

* Het herkennings resultaat afdrukken:`ResultReason.RecognizedSpeech`
* Als er geen overeenkomst wordt gevonden, stelt u de gebruiker hiervan op de hoogte:`ResultReason.NoMatch`
* Als er een fout is opgetreden, drukt u het fout bericht af:`ResultReason.Canceled`

```java
switch (result.getReason()) {
    case ResultReason.RecognizedSpeech:
        System.out.println("We recognized: " + result.getText());
        exitCode = 0;
        break;
    case ResultReason.NoMatch:
        System.out.println("NOMATCH: Speech could not be recognized.");
        break;
    case ResultReason.Canceled: {
            CancellationDetails cancellation = CancellationDetails.fromResult(result);
            System.out.println("CANCELED: Reason=" + cancellation.getReason());

            if (cancellation.getReason() == CancellationReason.Error) {
                System.out.println("CANCELED: ErrorCode=" + cancellation.getErrorCode());
                System.out.println("CANCELED: ErrorDetails=" + cancellation.getErrorDetails());
                System.out.println("CANCELED: Did you update the subscription info?");
            }
        }
        break;
}
```

### <a name="continuous-recognition"></a>Doorlopende herkenning

Doorlopende herkenning is een beetje meer betrokken dan eenmalige herkenning. U moet zich abonneren op de `recognizing`-, `recognized`-en `canceled` -gebeurtenissen om de herkennings resultaten op te halen. Als u de herkenning wilt stoppen, [`stopContinuousRecognitionAsync`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.stopcontinuousrecognitionasync)moet u bellen. Hier volgt een voor beeld van hoe doorlopende herkenning wordt uitgevoerd op een audio-invoer bestand.

Laten we beginnen met het definiëren van de invoer en het [`SpeechRecognizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable)initialiseren van een:

```java
AudioConfig audioConfig = AudioConfig.fromWavFileInput("YourAudioFile.wav");
SpeechRecognizer recognizer = new SpeechRecognizer(config, audioConfig);
```

Vervolgens maken we een variabele voor het beheren van de status van spraak herkenning. Om te beginnen, declareren we `Semaphore` een in het bereik van de klasse.

```java
private static Semaphore stopTranslationWithFileSemaphore;
```

We nemen een abonnement op de gebeurtenissen die worden [`SpeechRecognizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable)verzonden vanuit de.

* [`recognizing`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.recognizing?view=azure-java-stable): Signaal voor gebeurtenissen met tussenliggende herkennings resultaten.
* [`recognized`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.recognized?view=azure-java-stable): Signaal voor gebeurtenissen met definitieve herkennings resultaten (wat een geslaagde herkennings poging aangeeft).
* [`sessionStopped`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.recognizer.sessionstopped?view=azure-java-stable): Signaal voor gebeurtenissen die het einde van een herkennings sessie (bewerking) aangeven.
* [`canceled`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.canceled?view=azure-java-stable): Signaal voor gebeurtenissen met Geannuleerde herkennings resultaten (waarmee een herkennings poging wordt aangegeven die is geannuleerd als gevolg van een aanvraag of een rechtstreekse annulering of een Trans Port-of protocol fout).

```java
// First initialize the semaphore.
stopTranslationWithFileSemaphore = new Semaphore(0);

recognizer.recognizing.addEventListener((s, e) -> {
    System.out.println("RECOGNIZING: Text=" + e.getResult().getText());
});

recognizer.recognized.addEventListener((s, e) -> {
    if (e.getResult().getReason() == ResultReason.RecognizedSpeech) {
        System.out.println("RECOGNIZED: Text=" + e.getResult().getText());
    }
    else if (e.getResult().getReason() == ResultReason.NoMatch) {
        System.out.println("NOMATCH: Speech could not be recognized.");
    }
});

recognizer.canceled.addEventListener((s, e) -> {
    System.out.println("CANCELED: Reason=" + e.getReason());

    if (e.getReason() == CancellationReason.Error) {
        System.out.println("CANCELED: ErrorCode=" + e.getErrorCode());
        System.out.println("CANCELED: ErrorDetails=" + e.getErrorDetails());
        System.out.println("CANCELED: Did you update the subscription info?");
    }

    stopTranslationWithFileSemaphore.release();
});

recognizer.sessionStopped.addEventListener((s, e) -> {
    System.out.println("\n    Session stopped event.");
    stopTranslationWithFileSemaphore.release();
});
```

Als alles is ingesteld, kunnen we bellen [`stopContinuousRecognitionAsync`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.stopcontinuousrecognitionasync).

```java
// Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
recognizer.startContinuousRecognitionAsync().get();

// Waits for completion.
stopTranslationWithFileSemaphore.acquire();

// Stops recognition.
recognizer.stopContinuousRecognitionAsync().get();
```

### <a name="dictation-mode"></a>Dicteer modus

Bij het gebruik van doorlopende herkenning kunt u de verwerking van spraak herkenning inschakelen met behulp van de bijbehorende functie ' dicteren inschakelen '. Deze modus zorgt ervoor dat het spraak configuratie-exemplaar Word-beschrijvingen interpreteert, zoals Lees tekens. Bijvoorbeeld: het utterance ' hebt u Live in het stads vraag teken ' zou worden geïnterpreteerd als de tekst ' wilt u in de stad wonen? '.

Als u de dicteer modus wilt inschakelen, [`enableDictation`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig.enabledictation?view=azure-java-stable) gebruikt u de [`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable)-methode op uw.

```java
config.enableDictation();
```

## <a name="change-source-language"></a>Bron taal wijzigen

Een veelvoorkomende taak voor spraak herkenning is het opgeven van de invoer-(of bron-) taal. Laten we eens kijken hoe u de invoer taal wijzigt in Frans. Zoek in uw code uw [`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable)en voeg deze regel vervolgens direct onder de lijn toe.

```java
config.setSpeechRecognitionLanguage("fr-FR");
```

[`setSpeechRecognitionLanguage`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig.setspeechrecognitionlanguage?view=azure-java-stable)is een para meter die een teken reeks als argument gebruikt. U kunt een wille keurige waarde opgeven in de lijst met ondersteunde [land instellingen/talen](../../../language-support.md).

## <a name="improve-recognition-accuracy"></a>Nauw keurigheid van herkenning verbeteren

Er zijn een aantal manieren om de nauw keurigheid van de herkenning te verbeteren met de spraak-SDK. Laten we eens kijken naar woordgroepen lijsten. Woordgroepen lijsten worden gebruikt voor het identificeren van bekende woord groepen in audio gegevens, zoals de naam van een persoon of een specifieke locatie. Enkele woorden of volledige woord groepen kunnen worden toegevoegd aan een woordgroepen lijst. Tijdens de herkenning wordt een vermelding in een woordgroepen lijst gebruikt als er een exacte overeenkomst voor de volledige woord groep wordt opgenomen in de audio. Als een exacte overeenkomst met de zin niet wordt gevonden, wordt de herkenning niet ondersteund.

> [!IMPORTANT]
> De functie woordgroepen lijst is alleen beschikbaar in het Engels.

Als u een woordgroepen lijst wilt gebruiken, [`PhraseListGrammar`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.phraselistgrammar?view=azure-java-stable) maakt u eerst een-object en voegt u [`AddPhrase`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.phraselistgrammar.addphrase?view=azure-java-stable#com_microsoft_cognitiveservices_speech_PhraseListGrammar_addPhrase_String_)vervolgens specifieke woorden en zinsdelen toe met.

Wijzigingen die [`PhraseListGrammar`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.phraselistgrammar?view=azure-java-stable) van kracht worden bij de volgende herkenning of na het opnieuw verbinden met de spraak service.

```java
PhraseListGrammar phraseList = PhraseListGrammar.fromRecognizer(recognizer);
phraseList.addPhrase("Supercalifragilisticexpialidocious");
```

Als u uw woordgroepen lijst wilt wissen: 

```java
phraseList.clear();
```

### <a name="other-options-to-improve-recognition-accuracy"></a>Andere opties om de nauw keurigheid van de herkenning te verbeteren

Woordgroepen lijsten zijn slechts één optie om de nauw keurigheid van de herkenning te verbeteren. U kunt ook het volgende doen: 

* [Nauwkeurigheid verbeteren met Custom Speech](../../../how-to-custom-speech.md)
* [Nauwkeurigheid verbeteren met tenantmodellen](../../../tutorial-tenant-model.md)
