---
title: Logische apps aanroepen, triggeren of nesten
description: HTTPS-eind punten instellen voor het aanroepen, activeren of nesten van logische app-werk stromen in Azure Logic Apps
services: logic-apps
ms.workload: integration
ms.reviewer: jonfan, logicappspm
ms.topic: article
ms.date: 05/28/2020
ms.openlocfilehash: b5c4005c95a88a40a836b9c0f6d1fd01e0417ed0
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2020
ms.locfileid: "84170270"
---
# <a name="call-trigger-or-nest-logic-apps-by-using-https-endpoints-in-azure-logic-apps"></a>Logische apps aanroepen, activeren of nesten met behulp van HTTPS-eind punten in Azure Logic Apps

Als u wilt dat uw logische app kan worden opgeroepen via een URL zodat uw logische app binnenkomende aanvragen van andere services kan ontvangen, kunt u een synchroon HTTPS-eind punt beschikbaar maken als een trigger voor die logische app. Wanneer u deze mogelijkheid instelt, kunt u ook uw logische app nesten in andere logische apps, waarmee u een patroon van aanroep bare eind punten maakt.

Als u een aanroepbaar eind punt wilt instellen, kunt u een van deze trigger typen gebruiken, waarmee logische apps binnenkomende aanvragen kunnen ontvangen:

* [Aanvraag](../connectors/connectors-native-reqres.md)
* [HTTP-webhook](../connectors/connectors-native-webhook.md)
* Beheerde connector triggers die het [ApiConnectionWebhook-type](../logic-apps/logic-apps-workflow-actions-triggers.md#apiconnectionwebhook-trigger) hebben en binnenkomende HTTPS-aanvragen kunnen ontvangen

> [!NOTE]
> In deze voor beelden wordt de trigger voor aanvragen gebruikt, maar u kunt een HTTPS-aanvraag op basis van aanvragen gebruiken die in de vorige lijst voor komt. Alle principes zijn identiek van toepassing op deze andere trigger typen.

Als u geen ervaring hebt met Logic apps, raadpleegt u [Wat is Azure Logic apps](../logic-apps/logic-apps-overview.md) en [Quick Start: uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement hebt, [meld u dan aan voor een gratis Azure-account](https://azure.microsoft.com/free/).

* De logische app waar u de trigger wilt gebruiken om het aanroep bare eind punt te maken. U kunt beginnen met een lege logische app of een bestaande logische app waarvoor u de huidige trigger wilt vervangen. Dit voor beeld begint met een lege logische app.

## <a name="create-a-callable-endpoint"></a>Een aanroepbaar eind punt maken

1. Meld u aan bij de [Azure-portal](https://portal.azure.com). Een lege logische app maken en openen in de ontwerp functie voor logische apps.

   In dit voor beeld wordt de trigger voor aanvragen gebruikt, maar u kunt elke trigger gebruiken die binnenkomende HTTPS-aanvragen kan ontvangen. Alle principes zijn identiek van toepassing op deze triggers. Zie voor meer informatie over de aanvraag trigger [binnenkomende https-aanroepen ontvangen en erop reageren met behulp van Azure Logic apps](../connectors/connectors-native-reqres.md).

1. Selecteer in het zoekvak **ingebouwde**. Voer in het zoekvak in `request` als uw filter. Selecteer in de lijst triggers **Wanneer een HTTP-aanvraag wordt ontvangen**.

   ![De aanvraag trigger zoeken en selecteren](./media/logic-apps-http-endpoint/find-and-select-request-trigger.png)

1. Desgewenst kunt u in het vak JSON-schema van de **aanvraag tekst** een JSON-schema invoeren waarmee de payload of gegevens worden beschreven die u verwacht dat de trigger wordt ontvangen.

   De ontwerp functie gebruikt dit schema voor het genereren van tokens die trigger-uitvoer vertegenwoordigen. U kunt vervolgens eenvoudig naar deze uitvoer verwijzen tijdens de werk stroom van de logische app. Meer informatie over de [tokens die zijn gegenereerd op basis van JSON-schema's](#generated-tokens).

   Voor dit voor beeld voert u dit schema in:

   ```json
      {
      "type": "object",
      "properties": {
         "address": {
            "type": "object",
            "properties": {
               "streetNumber": {
                  "type": "string"
               },
               "streetName": {
                  "type": "string"
               },
               "town": {
                  "type": "string"
               },
               "postalCode": {
                  "type": "string"
               }
            }
         }
      }
   }
    ```

   ![Het JSON-schema voor de aanvraag actie opgeven](./media/logic-apps-http-endpoint/manual-request-trigger-schema.png)

   U kunt ook een JSON-schema genereren door een voor beeld van een nettolading te bieden:

   1. Selecteer in de trigger voor de **aanvraag** een **voor beeld-nettolading gebruiken om een schema te genereren**.

   1. In het vak **een voor beeld van een JSON-nettolading invoeren of plakken** voert u de voor beeld-nettolading in, bijvoorbeeld:

      ```json
      {
         "address": {
            "streetNumber": "00000",
            "streetName": "AnyStreet",
            "town": "AnyTown",
            "postalCode": "11111-1111"
        }
      }
      ```

   1. Wanneer u klaar bent, selecteert u **gereed**.

      In de vak **JSON schema van aanvraag tekst** wordt nu het gegenereerde schema weer gegeven.

1. Sla uw logische app op.

   In het vak **http post-URL** ziet u nu de gegenereerde call back-URL die andere services kunnen gebruiken om uw logische app aan te roepen en te activeren. Deze URL bevat query parameters waarmee een Shared Access Signature (SAS)-sleutel wordt opgegeven, die wordt gebruikt voor verificatie.

   ![Gegenereerde call back-URL voor eind punt](./media/logic-apps-http-endpoint/generated-endpoint-url.png)

1. U hebt de volgende opties om de call back-URL te kopiëren:

   * Klik rechts van het vak **http post-URL** op **kopiëren URL** (pictogram kopie bestanden).

   * Dit POST-gesprek doen:

     `POST https://management.azure.com/{logic-app-resource-ID}/triggers/{endpoint-trigger-name}/listCallbackURL?api-version=2016-06-01`

   * Kopieer de call back-URL vanuit het **overzichts** venster van uw logische app.

     1. Selecteer **overzicht**in het menu van de logische app.

     1. Selecteer in de sectie **samen vatting** de optie **trigger geschiedenis weer geven**.

        ![Eind punt-URL ophalen uit Azure Portal](./media/logic-apps-http-endpoint/find-manual-trigger-url.png)

     1. Kopieer onder **URL voor terugbellen [post]** de URL:

        ![Eind punt-URL van Azure Portal kopiëren](./media/logic-apps-http-endpoint/copy-manual-trigger-callback-url-post.png)

<a name="select-method"></a>

## <a name="select-expected-request-method"></a>Selecteer de verwachte aanvraag methode

De aanvraag trigger verwacht standaard een POST-aanvraag. U kunt een andere methode opgeven die u verwacht, maar slechts één methode.

1. Open in de trigger voor aanvragen de lijst **nieuwe para meter toevoegen** en selecteer **methode**, waarmee deze eigenschap wordt toegevoegd aan de trigger.

   ![Eigenschap ' methode ' toevoegen aan trigger](./media/logic-apps-http-endpoint/select-add-new-parameter-for-method.png)

1. Selecteer in de lijst **methode** de methode die door de trigger moet worden verwacht. U kunt ook een aangepaste methode opgeven.

   Selecteer bijvoorbeeld de **Get** -methode, zodat u de URL van uw eind punt later kunt testen.

   ![Selecteer een aanvraag methode die wordt verwacht door de trigger](./media/logic-apps-http-endpoint/select-method-request-trigger.png)

<a name="endpoint-url-parameters"></a>

## <a name="pass-parameters-through-endpoint-url"></a>Para meters door geven via eind punt-URL

Als u parameter waarden wilt accepteren via de URL van het eind punt, hebt u de volgende opties:

* [Waarden accepteren via Get para meters](#get-parameters) of URL-para meters.

  Deze waarden worden door gegeven als naam/waarde-paren in de URL van het eind punt. Voor deze optie moet u de methode GET gebruiken in uw aanvraag trigger. In een volgende actie kunt u de parameter waarden ophalen als trigger uitvoer met behulp van de `triggerOutputs()` functie in een expressie.

* [Accepteer waarden via een relatief pad](#relative-path) voor de para meters in de aanvraag trigger.

  Deze waarden worden door gegeven via een relatief pad in de URL van het eind punt. U moet ook expliciet [de methode selecteren](#select-method) die de trigger verwacht. Bij een volgende actie kunt u de parameter waarden ophalen als trigger uitvoer door rechtstreeks naar die uitvoer te verwijzen.

<a name="get-parameters"></a>

### <a name="accept-values-through-get-parameters"></a>Waarden accepteren via GET-para meters

1. In de trigger voor aanvragen opent u de **lijst nieuwe para meter toevoegen**, voegt u de eigenschap **Method** toe aan de trigger en selecteert u de **Get** -methode.

   Zie voor meer informatie [verwachte aanvraag methode selecteren](#select-method).

1. Voeg onder de trigger voor aanvragen de actie toe waarvoor u de parameter waarde wilt gebruiken. Voor dit voor beeld voegt u de **reactie** actie toe.

   1. Selecteer **nieuwe stap**  >  **een actie toevoegen**onder de trigger voor aanvragen.
   
   1. Voer onder **Kies een actie**in het zoekvak in `response` als uw filter. Selecteer in de lijst acties de actie **reactie** .

1. Voer de volgende stappen uit om de expressie op te bouwen `triggerOutputs()` waarmee de parameter waarde wordt opgehaald:

   1. Klik in de eigenschap **Body** van de antwoord actie, zodat de lijst met dynamische inhoud wordt weer gegeven en selecteer **expressie**.

   1. In het vak **expressie** voert u deze expressie in, vervangt `parameter-name` u door de naam van de para meter en selecteert u **OK**.

      `triggerOutputs()['queries']['parameter-name']`

      ![De expressie ' triggerOutputs () ' toevoegen aan trigger](./media/logic-apps-http-endpoint/trigger-outputs-expression.png)

      In de eigenschap **Body** wordt de expressie omgezet in het `triggerOutputs()` token.

      ![Opgeloste ' triggerOutputs () '-expressie](./media/logic-apps-http-endpoint/trigger-outputs-expression-token.png)

      Als u de logische app opslaat, navigeert u weg van de ontwerp functie en keert u terug naar de ontwerp functie, het token bevat de parameter naam die u hebt opgegeven, bijvoorbeeld:

      ![Omgezette expressie voor de parameter naam](./media/logic-apps-http-endpoint/resolved-expression-parameter-token.png)

      In de code weergave wordt de eigenschap **Body** in de definitie van de reactie actie als volgt weer gegeven:

      `"body": "@{triggerOutputs()['queries']['parameter-name']}",`

      Stel bijvoorbeeld dat u een waarde wilt door geven voor een para meter met de naam `postalCode` . De eigenschap **Body** geeft de teken reeks aan, `Postal Code: ` met een spatie, gevolgd door de bijbehorende expressie:

      ![Voor beeld ' triggerOutputs () '-expressie toevoegen aan trigger](./media/logic-apps-http-endpoint/trigger-outputs-expression-postal-code.png)

1. Als u uw aanroep bare eind punt wilt testen, kopieert u de call back-URL uit de trigger voor aanvragen en plakt u de URL in een ander browser venster. Voeg in de URL de parameter naam en de waarde achter het vraag teken ( `?` ) toe aan de URL in de volgende notatie en druk op ENTER.

   `...?{parameter-name=parameter-value}&api-version=2016-10-01...`

   `https://prod-07.westus.logic.azure.com:433/workflows/{logic-app-resource-ID}/triggers/manual/paths/invoke?{parameter-name=parameter-value}&api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig={shared-access-signature}`

   De browser retourneert een antwoord met deze tekst:`Postal Code: 123456`

   ![Antwoord van het verzenden van een aanvraag naar een call back-URL](./media/logic-apps-http-endpoint/callback-url-returned-response.png)

1. Als u de parameter naam en-waarde op een andere positie binnen de URL wilt plaatsen, moet u het `&` en-teken () als voor voegsel gebruiken, bijvoorbeeld:

   `...?api-version=2016-10-01&{parameter-name=parameter-value}&...`

   In dit voor beeld wordt de call back-URL met de naam en waarde van de voor beeld-para meter `postalCode=123456` in verschillende posities binnen de URL weer gegeven:

   * 1e positie:`https://prod-07.westus.logic.azure.com:433/workflows/{logic-app-resource-ID}/triggers/manual/paths/invoke?postalCode=123456&api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig={shared-access-signature}`

   * 2e positie:`https://prod-07.westus.logic.azure.com:433/workflows/{logic-app-resource-ID}/triggers/manual/paths/invoke?api-version=2016-10-01&postalCode=123456&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig={shared-access-signature}`

> [!NOTE]
> Als u het hash-of hekje-symbool ( **#** ) in de URI wilt toevoegen, gebruikt u in plaats daarvan deze gecodeerde versie:`%25%23`

<a name="relative-path"></a>

### <a name="accept-values-through-a-relative-path"></a>Waarden accepteren via een relatief pad

1. Open in de trigger voor aanvragen de lijst **nieuwe para meter toevoegen** en selecteer **relatief pad**, waarmee deze eigenschap wordt toegevoegd aan de trigger.

   ![Eigenschap relatief pad toevoegen aan trigger](./media/logic-apps-http-endpoint/select-add-new-parameter-for-relative-path.png)

1. Geef in de eigenschap **relatief pad** het relatieve pad voor de para meter in uw JSON-schema op dat u door de URL wilt laten accepteren, bijvoorbeeld `/address/{postalCode}` .

   ![Geef het relatieve pad op voor de para meter](./media/logic-apps-http-endpoint/relative-path-url-value.png)

1. Voeg onder de trigger voor aanvragen de actie toe waarvoor u de parameter waarde wilt gebruiken. Voor dit voor beeld voegt u de **reactie** actie toe.

   1. Selecteer **nieuwe stap**  >  **een actie toevoegen**onder de trigger voor aanvragen.

   1. Voer onder **Kies een actie**in het zoekvak in `response` als uw filter. Selecteer in de lijst acties de actie **reactie** .

1. Neem in de eigenschap **hoofd tekst** van de reactie actie het token op dat staat voor de para meter die u hebt opgegeven in het relatieve pad van uw trigger.

   Stel dat u wilt dat de reactie actie wordt geretourneerd `Postal Code: {postalCode}` .

   1. Voer in de eigenschap **Body** `Postal Code: ` een afsluitende spatie in. Bewaar de cursor binnen het invoervak zodat de lijst met dynamische inhoud geopend blijft.

   1. Selecteer in de lijst dynamische inhoud, in de sectie **Wanneer een HTTP-aanvraag wordt ontvangen** , het **Post code** -token.

      ![De opgegeven para meter toevoegen aan de antwoord tekst](./media/logic-apps-http-endpoint/relative-url-with-parameter-token.png)

      De eigenschap **Body** bevat nu de geselecteerde para meter:

      ![Voorbeeld tekst van antwoord met para meter](./media/logic-apps-http-endpoint/relative-url-with-parameter.png)

1. Sla uw logische app op.

   In de trigger voor aanvragen wordt de call back-URL bijgewerkt en wordt nu het relatieve pad opgenomen, bijvoorbeeld:

   `https://prod-07.westus.logic.azure.com/workflows/{logic-app-resource-ID}/triggers/manual/paths/invoke/address/{postalCode}?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig={shared-access-signature}`

1. Als u uw aanroep bare eind punt wilt testen, kopieert u de bijgewerkte call back-URL uit de trigger voor aanvragen, plakt u de URL in een ander browser venster, vervangt u `{postalCode}` in de URL door `123456` en drukt u op ENTER.

   De browser retourneert een antwoord met deze tekst:`Postal Code: 123456`

   ![Antwoord van het verzenden van een aanvraag naar een call back-URL](./media/logic-apps-http-endpoint/callback-url-returned-response.png)

> [!NOTE]
> Als u het hash-of hekje-symbool ( **#** ) in de URI wilt toevoegen, gebruikt u in plaats daarvan deze gecodeerde versie:`%25%23`

## <a name="call-logic-app-through-endpoint-url"></a>Logische app aanroepen via eind punt-URL

Nadat u het eind punt hebt gemaakt, kunt u de logische app activeren door een HTTPS- `POST` aanvraag naar de volledige URL van het eind punt te verzenden. Logic apps hebben ingebouwde ondersteuning voor directe toegangs punten.

<a name="generated-tokens"></a>

## <a name="tokens-generated-from-schema"></a>Tokens die zijn gegenereerd op basis van het schema

Wanneer u een JSON-schema in de aanvraag trigger opgeeft, worden in de ontwerp functie voor logische apps tokens voor de eigenschappen in dat schema gegenereerd. U kunt vervolgens deze tokens gebruiken voor het door geven van gegevens via de werk stroom van uw logische app.

Als u bijvoorbeeld meer eigenschappen toevoegt, zoals `"suite"` aan uw JSON-schema, zijn tokens voor die eigenschappen beschikbaar voor gebruik in de volgende stappen voor uw logische app. Dit is het volledige JSON-schema:

```json
   {
   "type": "object",
   "properties": {
      "address": {
         "type": "object",
         "properties": {
            "streetNumber": {
               "type": "string"
            },
            "streetName": {
               "type": "string"
            },
            "suite": {
               "type": "string"
            },
            "town": {
               "type": "string"
            },
            "postalCode": {
               "type": "string"
            }
         }
      }
   }
}
```

## <a name="create-nested-logic-apps"></a>Geneste Logic apps maken

U kunt werk stromen nesten in uw logische app door andere logische apps toe te voegen die aanvragen kunnen ontvangen. Voer de volgende stappen uit om deze Logic apps op te nemen:

1. Selecteer **nieuwe stap**  >  **een actie toevoegen**onder de stap waarin u een andere logische app wilt aanroepen.

1. Selecteer onder **Kies een actie de**optie **ingebouwd**. Voer in het zoekvak in `logic apps` als uw filter. Selecteer in de lijst acties de optie **een Logic apps werk stroom kiezen**.

   ![Logische app nesten in huidige logische app](./media/logic-apps-http-endpoint/choose-logic-apps-workflow.png)

   In de ontwerp functie worden de in aanmerking komende Logic apps weer gegeven die u kunt selecteren.

1. Selecteer de logische app die u wilt aanroepen vanuit uw huidige logische app.

   ![Logische app selecteren die moet worden aangeroepen vanuit de huidige logische app](./media/logic-apps-http-endpoint/select-logic-app-to-nest.png)

## <a name="reference-content-from-an-incoming-request"></a>Verwijzen naar inhoud van een binnenkomende aanvraag

Als het inhouds type van de inkomende aanvraag is `application/json` , kunt u naar de eigenschappen in de binnenkomende aanvraag verwijzen. Anders wordt deze inhoud behandeld als een enkele binaire eenheid die u kunt door geven aan andere Api's. Als u wilt verwijzen naar deze inhoud in de werk stroom van uw logische app, moet u deze inhoud eerst converteren.

Als u bijvoorbeeld inhoud `application/xml` doorgeeft die van het type is, kunt u de [ `@xpath()` expressie](../logic-apps/workflow-definition-language-functions-reference.md#xpath) gebruiken om een XPath-extractie uit te voeren of de [ `@json()` expressie](../logic-apps/workflow-definition-language-functions-reference.md#json) gebruiken voor het converteren van XML naar JSON. Meer informatie over het werken met ondersteunde [inhouds typen](../logic-apps/logic-apps-content-type.md).

Als u de uitvoer van een binnenkomende aanvraag wilt ophalen, kunt u de [ `@triggerOutputs` expressie](../logic-apps/workflow-definition-language-functions-reference.md#triggerOutputs)gebruiken. Stel dat u een uitvoer hebt die er ongeveer zo uitziet als in dit voor beeld:

```json
{
   "headers": {
      "content-type" : "application/json"
   },
   "body": {
      "myProperty" : "property value"
   }
}
```

Voor toegang tot specifiek de `body` eigenschap kunt u de [ `@triggerBody()` expressie](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody) gebruiken als een snelkoppeling.

## <a name="respond-to-requests"></a>Reageren op aanvragen

Soms wilt u reageren op bepaalde aanvragen die uw logische app activeren door inhoud te retour neren aan de aanroeper. Als u de status code, koptekst en hoofd tekst voor uw antwoord wilt maken, gebruikt u de reactie actie. Deze actie kan overal in uw logische app worden weer gegeven, niet alleen aan het einde van uw werk stroom. Als uw logische app geen reactie actie bevat, reageert het eind punt *onmiddellijk* met de **geaccepteerde status 202** .

De oorspronkelijke beller kan het antwoord alleen ophalen als alle vereiste stappen voor het antwoord binnen de [time-outlimiet](./logic-apps-limits-and-config.md) van de aanvraag zijn voltooid, tenzij de geactiveerde logische app wordt aangeroepen als een geneste logische app. Als er geen antwoord wordt geretourneerd binnen deze limiet, wordt er een time-out voor de inkomende aanvraag verzonden en ontvangt de **408-time-out** van de client.

Voor geneste logische apps blijft de bovenliggende logische app wachten op een reactie totdat alle stappen zijn voltooid, ongeacht de hoeveelheid tijd die nodig is.

### <a name="construct-the-response"></a>Het antwoord maken

In de antwoord tekst kunt u meerdere kopteksten en elk type inhoud toevoegen. De koptekst van dit antwoord geeft bijvoorbeeld aan dat het inhouds type van het antwoord is `application/json` en dat de hoofd tekst waarden bevat voor de `town` `postalCode` Eigenschappen en, op basis van het JSON-schema dat eerder in dit onderwerp voor de aanvraag trigger is beschreven.

![Reactie-inhoud voor HTTPS-reactie actie opgeven](./media/logic-apps-http-endpoint/content-for-response-action.png)

Antwoorden hebben de volgende eigenschappen:

| Eigenschap (weer geven) | Eigenschap (JSON) | Beschrijving |
|--------------------|-----------------|-------------|
| **Status code** | `statusCode` | De HTTPS-status code die moet worden gebruikt in het antwoord op de binnenkomende aanvraag. Deze code kan een geldige status code zijn die begint met 2xx, 4xx of 5xx. 3xx-status codes zijn echter niet toegestaan. |
| **Kopteksten** | `headers` | Een of meer headers die moeten worden meegenomen in het antwoord |
| **Hoofdtekst** | `body` | Een body-object dat een teken reeks, een JSON-object of zelfs binaire inhoud waarnaar wordt verwezen vanuit een vorige stap kan zijn. |
||||

Als u de JSON-definitie voor de reactie actie en de volledige JSON-definitie van de logische app wilt weer geven, selecteert u op de werk balk van de Logic app-ontwerp functie de **code weergave**.

``` json
"Response": {
   "type": "Response",
   "kind": "http",
   "inputs": {
      "body": {
         "postalCode": "@triggerBody()?['address']?['postalCode']",
         "town": "@triggerBody()?['address']?['town']"
      },
      "headers": {
         "content-type": "application/json"
      },
      "statusCode": 200
   },
   "runAfter": {}
}
```

## <a name="q--a"></a>Vragenronde

#### <a name="q-what-about-url-security"></a>V: wat gebeurt er met URL-beveiliging?

**A**: Azure maakt veilig-call back-url's voor logische apps met behulp van [Shared Access Signature (SAS)](https://docs.microsoft.com/rest/api/storageservices/delegate-access-with-shared-access-signature). Deze hand tekening wordt door gegeven als een query parameter en moet worden gevalideerd voordat de logische app kan worden uitgevoerd. Azure genereert de hand tekening met een unieke combi natie van een geheime sleutel per logische app, de naam van de trigger en de bewerking die wordt uitgevoerd. Tenzij iemand toegang heeft tot de sleutel van de geheime logische app, kan er dus geen geldige hand tekening worden gegenereerd.

> [!IMPORTANT]
> Voor productie-en hogere beveiligings systemen raden wij u ten zeerste aan om uw logische app rechtstreeks vanuit de browser aan te roepen om de volgende redenen:
>
> * De gedeelde toegangs sleutel wordt weer gegeven in de URL.
> * U kunt geen beleids regels voor beveiligings inhoud beheren vanwege gedeelde domeinen van Azure Logic Apps klanten.

#### <a name="q-can-i-configure-callable-endpoints-further"></a>V: kan ik aanroep bare eind punten verder configureren?

**A**: Ja, https-eind punten ondersteunen meer geavanceerde configuratie via [Azure API Management](../api-management/api-management-key-concepts.md). Deze service biedt ook de mogelijkheid om alle Api's consistent te beheren, met inbegrip van Logic apps, het instellen van aangepaste domein namen, het gebruiken van meer verificatie methoden en meer, bijvoorbeeld:

* [De aanvraag methode wijzigen](../api-management/api-management-advanced-policies.md#SetRequestMethod)
* [De URL-segmenten van de aanvraag wijzigen](../api-management/api-management-transformation-policies.md#RewriteURL)
* Uw API Management domeinen instellen in de [Azure Portal](https://portal.azure.com/)
* Beleid instellen om te controleren op basis verificatie

## <a name="next-steps"></a>Volgende stappen

* [Binnenkomende HTTPS-aanroepen ontvangen en erop reageren met behulp van Azure Logic Apps](../connectors/connectors-native-reqres.md)
