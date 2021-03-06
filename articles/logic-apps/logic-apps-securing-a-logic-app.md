---
title: Toegang en gegevens beveiligen
description: Veilige toegang tot invoer, uitvoer, op aanvragen gebaseerde triggers, uitvoerings geschiedenis, beheer taken en toegang tot andere resources in Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 8fe53b7a27c922462f9134bc78ff648aca3aca62
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2020
ms.locfileid: "83715542"
---
# <a name="secure-access-and-data-in-azure-logic-apps"></a>Beveiligde toegang en gegevens in Azure Logic Apps

Voor het beheren van de toegang tot en het beveiligen van gevoelige gegevens in Azure Logic Apps, kunt u de beveiliging instellen voor de volgende gebieden:

* [Toegang tot activeringen op basis van een aanvraag](#secure-triggers)
* [Toegang tot logische app-bewerkingen](#secure-operations)
* [Toegang tot de invoer en uitvoer van de uitvoerings geschiedenis](#secure-run-history)
* [Toegang tot parameter invoer](#secure-action-parameters)
* [Toegang tot services en systemen die worden aangeroepen vanuit Logic apps](#secure-outbound-requests)

<a name="secure-triggers"></a>

## <a name="access-to-request-based-triggers"></a>Toegang tot activeringen op basis van een aanvraag

Als uw logische app gebruikmaakt van een trigger op basis van een aanvraag, die inkomende aanroepen of aanvragen ontvangt, zoals de trigger van de [aanvraag](../connectors/connectors-native-reqres.md) of de [webhook](../connectors/connectors-native-webhook.md) , kunt u de toegang beperken zodat alleen geautoriseerde clients uw logische app kunnen aanroepen. Alle aanvragen die door een logische app worden ontvangen, worden versleuteld en beveiligd met Transport Layer Security (TLS), voorheen bekend als Secure Sockets Layer (SSL), protocol.

Hier vindt u opties die u kunnen helpen bij het beveiligen van de toegang tot dit trigger type:

* [Hand tekeningen voor gedeelde toegang genereren](#sas)
* [Verificatie Azure Active Directory openen inschakelen (Azure AD OAuth)](#enable-oauth)
* [Inkomende IP-adressen beperken](#restrict-inbound-ip-addresses)
* [Azure Active Directory open-verificatie (Azure AD OAuth) of andere beveiliging toevoegen](#add-authentication)

<a name="sas"></a>

### <a name="generate-shared-access-signatures-sas"></a>Hand tekeningen voor gedeelde toegang (SAS) genereren

Elk aanvraag eindpunt op een logische app heeft een [Shared Access Signature (SAS)](https://docs.microsoft.com/rest/api/storageservices/constructing-a-service-sas) in de URL van het eind punt, die de volgende indeling heeft:

`https://<request-endpoint-URI>sp=<permissions>sv=<SAS-version>sig=<signature>`

Elke URL bevat de `sp` `sv` para meter,, en `sig` query zoals beschreven in deze tabel:

| Query parameter | Beschrijving |
|-----------------|-------------|
| `sp` | Hiermee geeft u de machtigingen op voor het gebruik van de toegestane HTTP-methoden. |
| `sv` | Hiermee geeft u de SAS-versie op die moet worden gebruikt voor het genereren van de hand tekening. |
| `sig` | Hiermee geeft u de hand tekening op die moet worden gebruikt voor het verifiëren van de toegang tot de trigger. Deze hand tekening wordt gegenereerd met behulp van de SHA256-algoritme met een geheime toegangs sleutel voor alle URL-paden en eigenschappen. Nooit beschikbaar of gepubliceerd, deze sleutel wordt versleuteld bewaard en opgeslagen met de logische app. Uw logische app machtigt alleen de triggers die een geldige hand tekening bevatten die is gemaakt met de geheime sleutel. |
|||

Zie de volgende secties in dit onderwerp voor meer informatie over het beveiligen van de toegang met SAS:

* [Toegangssleutels regenereren](#access-keys)
* [Url's voor verlopende call back maken](#expiring-urls)
* [Url's met een primaire of secundaire sleutel maken](#primary-secondary-key)

<a name="access-keys"></a>

#### <a name="regenerate-access-keys"></a>Toegangssleutels regenereren

Als u op elk gewenst moment een nieuwe beveiligings toegangs sleutel wilt genereren, gebruikt u de Azure REST API of Azure Portal. Alle eerder gegenereerde Url's die gebruikmaken van de oude sleutel, zijn ongeldig en hebben geen autorisatie meer om de logische app te activeren. De Url's die u na het opnieuw genereren hebt opgehaald, worden ondertekend met de nieuwe toegangs sleutel.

1. Open in de [Azure Portal](https://portal.azure.com)de logische app met de sleutel die u opnieuw wilt genereren.

1. Selecteer **toegangs sleutels**onder **instellingen**in het menu van de logische app.

1. Selecteer de sleutel die u opnieuw wilt genereren en voltooi het proces.

<a name="expiring-urls"></a>

#### <a name="create-expiring-callback-urls"></a>Url's voor verlopende call back maken

Als u de eind punt-URL voor een op aanvragen gebaseerde trigger met andere partijen deelt, kunt u Url's voor terugbellen genereren die specifieke sleutels gebruiken en verloop datums hebben. Op die manier kunt u sleutels naadloos draaien of de toegang beperken tot het activeren van uw logische app op basis van een specifieke tijds duur. Als u een verval datum voor een URL wilt opgeven, gebruikt u de [Logic Apps rest API](https://docs.microsoft.com/rest/api/logic/workflowtriggers), bijvoorbeeld:

```http
POST /subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<workflow-name>/triggers/<trigger-name>/listCallbackUrl?api-version=2016-06-01
```

Neem in de hoofd tekst de `NotAfter` eigenschap op met behulp van een JSON-datum teken reeks. Deze eigenschap retourneert een call back-URL die alleen geldig is voor de `NotAfter` datum en tijd.

<a name="primary-secondary-key"></a>

#### <a name="create-urls-with-primary-or-secondary-secret-key"></a>Url's met een primaire of secundaire sleutel maken

Wanneer u call back-Url's voor een op aanvragen gebaseerde trigger genereert of lijsteert, kunt u de sleutel opgeven die moet worden gebruikt voor het ondertekenen van de URL. Als u een URL wilt genereren die is ondertekend met een specifieke sleutel, gebruikt u de [Logic Apps rest API](https://docs.microsoft.com/rest/api/logic/workflowtriggers), bijvoorbeeld:

```http
POST /subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.Logic/workflows/<workflow-name>/triggers/<trigger-name>/listCallbackUrl?api-version=2016-06-01
```

In de hoofd tekst, neemt `KeyType` u de eigenschap op als `Primary` of `Secondary` . Deze eigenschap retourneert een URL die is ondertekend door de opgegeven beveiligings sleutel.

<a name="enable-oauth"></a>

### <a name="enable-azure-active-directory-oauth"></a>Azure Active Directory OAuth inschakelen

Als uw logische app begint met een trigger voor aanvragen, kunt u [Azure Active Directory open verificatie](../active-directory/develop/about-microsoft-identity-platform.md) (Azure AD OAuth) inschakelen voor het autoriseren van binnenkomende aanroepen aan de aanvraag trigger. Lees de volgende overwegingen voordat u deze verificatie inschakelt:

* Uw logische app is beperkt tot een maximum aantal autorisatie beleidsregels. Elk autorisatie beleid heeft ook een maximum aantal [claims](../active-directory/develop/developer-glossary.md#claim). Zie [limieten en configuratie voor Azure Logic apps](../logic-apps/logic-apps-limits-and-config.md#authentication-limits)voor meer informatie.

* Een autorisatie beleid moet ten minste de **Issuer** claim bevatten, die een waarde heeft die begint met `https://sts.windows.net/` als de id van de Azure AD-Uitgever.

* Een inkomende oproep naar uw logische app kan slechts één autorisatie schema, ofwel Azure AD OAuth of [Shared Access signatures (SAS)](#sas), gebruiken.

* OAuth-tokens worden alleen ondersteund voor de aanvraag trigger.

* Alleen autorisatie schema's van [Bearer-type](../active-directory/develop/active-directory-v2-protocols.md#tokens) worden ondersteund voor OAuth-tokens.

Als u Azure AD OAuth wilt inschakelen, volgt u deze stappen om een of meer autorisatie beleid toe te voegen aan uw logische app.

1. Zoek en open uw logische app in het [Azure Portal](https://portal.microsoft.com)in de ontwerp functie voor logische apps.

1. Selecteer in het menu van de logische app onder **instellingen**de optie **autorisatie**. Nadat het autorisatie venster is geopend, selecteert u **beleid toevoegen**.

   ![Selecteer autorisatie > beleid toevoegen](./media/logic-apps-securing-a-logic-app/add-azure-active-directory-authorization-policies.png)

1. Geef informatie op over het autorisatie beleid door de [claim typen](../active-directory/develop/developer-glossary.md#claim) en waarden op te geven die uw logische app verwacht in het verificatie token dat wordt gepresenteerd door elke binnenkomende aanroep naar de aanvraag trigger:

   ![Informatie opgeven voor autorisatie beleid](./media/logic-apps-securing-a-logic-app/set-up-authorization-policy.png)

   | Eigenschap | Vereist | Beschrijving |
   |----------|----------|-------------|
   | **Beleids naam** | Ja | De naam die u wilt gebruiken voor het autorisatie beleid |
   | **Claims** | Ja | De claim typen en-waarden die uw logische app accepteert van binnenkomende oproepen. Dit zijn de beschik bare claim typen: <p><p>- **Verlener** <br>- **Gericht** <br>- **Onderwerp** <br>- **JWT-id** (JSON Web token-id) <p><p>De **claim** lijst moet mini maal de claim van de **verlener** bevatten, die een waarde heeft die begint met de id van de `https://sts.windows.net/` Azure AD-Uitgever. Zie [claims in azure AD-beveiligings tokens](../active-directory/azuread-dev/v1-authentication-scenarios.md#claims-in-azure-ad-security-tokens)voor meer informatie over deze claim typen. U kunt ook uw eigen claim type en-waarde opgeven. |
   |||

1. Selecteer een van de volgende opties om een claim toe te voegen:

   * Als u een ander claim type wilt toevoegen, selecteert u **standaard claim toevoegen**, selecteert u het claim type en geeft u de claim waarde op.

   * Als u uw eigen claim wilt toevoegen, selecteert u **aangepaste claim toevoegen**en geeft u de aangepaste claim waarde op.

1. Selecteer **beleid toevoegen**om een ander autorisatie beleid toe te voegen. Herhaal de vorige stappen om het beleid in te stellen.

1. Selecteer **Opslaan** als u klaar bent.

Uw logische app is nu ingesteld voor het gebruik van Azure AD OAuth voor het machtigen van inkomende aanvragen. Wanneer uw logische app een binnenkomende aanvraag met een verificatie token ontvangt, vergelijkt Azure Logic Apps de claims van het token met de claims in elk autorisatie beleid. Als er een overeenkomst is tussen de claims van het token en alle claims in ten minste één beleid, is de autorisatie geslaagd voor de inkomende aanvraag. Het token kan meer claims hebben dan het aantal dat is opgegeven door het autorisatie beleid.

Stel bijvoorbeeld dat uw logische app een autorisatie beleid heeft dat twee claim typen vereist, verlener en publiek. Dit voor beeld van een gedecodeerd [toegangs token](../active-directory/develop/access-tokens.md) bevat beide claim typen:

```json
{
   "aud": "https://management.core.windows.net/",
   "iss": "https://sts.windows.net/<Azure-AD-issuer-ID>/",
   "iat": 1582056988,
   "nbf": 1582056988,
   "exp": 1582060888,
   "_claim_names": {
      "groups": "src1"
   },
   "_claim_sources": {
      "src1": {
         "endpoint": "https://graph.windows.net/7200000-86f1-41af-91ab-2d7cd011db47/users/00000-f433-403e-b3aa-7d8406464625d7/getMemberObjects"
    }
   },
   "acr": "1",
   "aio": "AVQAq/8OAAAA7k1O1C2fRfeG604U9e6EzYcy52wb65Cx2OkaHIqDOkuyyr0IBa/YuaImaydaf/twVaeW/etbzzlKFNI4Q=",
   "amr": [
      "rsa",
      "mfa"
   ],
   "appid": "c44b4083-3bb0-00001-b47d-97400853cbdf3c",
   "appidacr": "2",
   "deviceid": "bfk817a1-3d981-4dddf82-8ade-2bddd2f5f8172ab",
   "family_name": "Sophia Owen",
   "given_name": "Sophia Owen (Fabrikam)",
   "ipaddr": "167.220.2.46",
   "name": "sophiaowen",
   "oid": "3d5053d9-f433-00000e-b3aa-7d84041625d7",
   "onprem_sid": "S-1-5-21-2497521184-1604012920-1887927527-21913475",
   "puid": "1003000000098FE48CE",
   "scp": "user_impersonation",
   "sub": "KGlhIodTx3XCVIWjJarRfJbsLX9JcdYYWDPkufGVij7_7k",
   "tid": "72f988bf-86f1-41af-91ab-2d7cd011db47",
   "unique_name": "SophiaOwen@fabrikam.com",
   "upn": "SophiaOwen@fabrikam.com",
   "uti": "TPJ7nNNMMZkOSx6_uVczUAA",
   "ver": "1.0"
}
```

<a name="restrict-inbound-ip"></a>

### <a name="restrict-inbound-ip-addresses"></a>Inkomende IP-adressen beperken

Samen met Shared Access Signature (SAS) kunt u de clients die uw logische app kunnen aanroepen, het beste beperken. Als u bijvoorbeeld het eind punt van de aanvraag beheert met behulp van Azure API Management, kunt u de logische app beperken tot het accepteren van aanvragen van het IP-adres voor het API Management exemplaar.

#### <a name="restrict-inbound-ip-ranges-in-azure-portal"></a>Binnenkomende IP-bereiken in Azure Portal beperken

1. Open in de [Azure Portal](https://portal.azure.com)uw logische app in de ontwerp functie voor logische apps.

1. Selecteer **werk stroom instellingen**onder **instellingen**in het menu van de logische app.

1. **Access control configuration**  >  Selecteer **specifieke IP-bereiken**onder toegangs beheer configuratie**toegestane binnenkomende IP-adressen**.

1. Geef onder **IP-bereiken voor triggers**de IP-adresbereiken op die de trigger accepteert.

   Een geldig IP-bereik maakt gebruik van de volgende indelingen: *x. x. x. x/x* of *x. x. x. x-x. x.* x. x

Als u wilt dat uw logische app alleen als een geneste logische app wordt geactiveerd, selecteert u **alleen andere Logic apps**in de lijst **toegestane binnenkomende IP-adressen** . Met deze optie wordt een lege matrix naar de logische app-resource geschreven. Op die manier kunnen alleen aanroepen van de Logic Apps-service (bovenliggende Logic apps) de geneste logische app activeren.

> [!NOTE]
> Ongeacht het IP-adres kunt u nog steeds een logische app met een op aanvragen gebaseerde trigger uitvoeren met behulp van `/triggers/<trigger-name>/run` de Azure-rest API of via API management. Voor dit scenario is echter nog steeds [verificatie](../active-directory/develop/authentication-scenarios.md) vereist voor de Azure-rest API. Alle gebeurtenissen worden weer gegeven in het controle logboek van Azure. Zorg ervoor dat u de beleids regels voor toegangs beheer dienovereenkomstig instelt.

#### <a name="restrict-inbound-ip-ranges-in-azure-resource-manager-template"></a>Binnenkomende IP-bereiken in Azure Resource Manager sjabloon beperken

Als u de [implementatie van Logic apps automatiseert met behulp van Resource Manager-sjablonen](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), kunt u de IP-adresbereiken in *x. x. x. x/x* -of x. x. x. *x-x.* x. x notatie gebruiken met behulp van de `accessControl` sectie en door de `triggers` `actions` secties en in de resource definitie van de logische app op te nemen, bijvoorbeeld:

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {},
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {
               <workflow-definition>
            },
            "parameters": {
            },
            "accessControl": {
               "triggers": {
                  "allowedCallerIpAddresses": [
                     {
                        "addressRange": "192.168.12.0/23"
                     }
                  ]
               },
               "actions": {
                  "allowedCallerIpAddresses:" : []
               }
            },
            "endpointsConfiguration": {}
         }
      }
   ],
   "outputs": {}
}
```

<a name="add-authentication"></a>

### <a name="add-azure-active-directory-open-authentication-or-other-security"></a>Azure Active Directory open-verificatie of andere beveiliging toevoegen

Als u meer [verificatie](../active-directory/develop/authentication-scenarios.md) protocollen wilt toevoegen aan uw logische app, kunt u overwegen de [Azure API Management](../api-management/api-management-key-concepts.md) -service te gebruiken. Deze service helpt u uw logische app beschikbaar te maken als een API en biedt uitgebreide bewaking, beveiliging, beleid en documentatie voor elk eind punt. API Management kunt een openbaar of persoonlijk eind punt beschikbaar maken voor uw logische app. Als u toegang tot dit eind punt wilt verlenen, kunt u [Azure Active Directory open verificatie](#azure-active-directory-oauth-authentication) (Azure AD OAuth), [client certificaat](#client-certificate-authentication)of andere beveiligings normen gebruiken om toegang tot het eind punt te verlenen. Wanneer API Management een aanvraag ontvangt, stuurt de service de aanvraag naar uw logische app, worden ook de nodige trans formaties of beperkingen door lopen. Als u uw logische app alleen API Management activeren, kunt u de instellingen voor het binnenkomende IP-adres bereik van uw logische app gebruiken.

<a name="secure-operations"></a>

## <a name="access-to-logic-app-operations"></a>Toegang tot logische app-bewerkingen

U kunt alleen specifieke gebruikers of groepen toestaan specifieke taken uit te voeren, zoals het beheren, bewerken en weer geven van logische apps. Als u de machtigingen wilt beheren, moet u gebruikmaken [van Azure Role Access Control (RBAC)](../role-based-access-control/role-assignments-portal.md) , zodat u aangepaste of ingebouwde rollen kunt toewijzen aan de leden van uw Azure-abonnement:

* [Inzender van Logic](../role-based-access-control/built-in-roles.md#logic-app-contributor)apps: Hiermee kunt u logische apps beheren, maar u kunt de toegang niet wijzigen.

* [Logische app-operator](../role-based-access-control/built-in-roles.md#logic-app-operator): Hiermee kunt u logische apps lezen, inschakelen en uitschakelen, maar u kunt ze niet bewerken of bijwerken.

U kunt [Azure resource Lock](../azure-resource-manager/management/lock-resources.md)gebruiken om te voor komen dat anderen uw logische app wijzigen of verwijderen. Deze mogelijkheid voor komt dat anderen productie resources wijzigen of verwijderen.

<a name="secure-run-history"></a>

## <a name="access-to-run-history-data"></a>Toegang tot uitvoerings geschiedenis gegevens

Tijdens de uitvoering van een logische app worden alle gegevens [tijdens de overdracht versleuteld](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit) met behulp van Transport Layer Security (TLS) en [in rust](../security/fundamentals/encryption-atrest.md). Wanneer de logische app wordt uitgevoerd, kunt u de geschiedenis voor die uitvoering bekijken, met inbegrip van de stappen die samen met de status, duur, invoer en uitvoer voor elke actie worden uitgevoerd. Deze uitgebreide details bieden inzicht in hoe uw logische app wordt uitgevoerd en waar u kunt beginnen met het oplossen van problemen die zich voordoen.

Wanneer u de uitvoerings geschiedenis van de logische app bekijkt, wordt uw toegang door Logic Apps geverifieerd en vindt u koppelingen naar de invoer en uitvoer voor de aanvragen en antwoorden voor elke uitvoering. Voor acties waarbij wacht woorden, geheimen, sleutels of andere gevoelige informatie worden verwerkt, wilt u echter voor komen dat anderen deze gegevens kunnen weer geven en gebruiken. Als uw logische app bijvoorbeeld een geheim krijgt van [Azure Key Vault](../key-vault/general/overview.md) om te gebruiken bij het verifiëren van een http-actie, wilt u dat geheim verbergen in de weer gave.

Als u de toegang tot de invoer en uitvoer in de uitvoerings geschiedenis van de logische app wilt beheren, hebt u de volgende opties:

* De [toegang beperken op basis van het IP-adres bereik](#restrict-ip).

  Met deze optie kunt u de toegang tot de uitvoerings geschiedenis beveiligen op basis van de aanvragen van een specifiek IP-adres bereik.

* [Gegevens in de uitvoerings geschiedenis beveiligen door gebruik te maken van een behulp van een betrouw](#obfuscate)bare

  In veel triggers en acties kunt u de invoer, uitvoer of beide beveiligen in de uitvoerings geschiedenis van een logische app.

<a name="restrict-ip"></a>

### <a name="restrict-access-by-ip-address-range"></a>Toegang beperken op basis van IP-adres bereik

U kunt de toegang tot de invoer en uitvoer in de uitvoerings geschiedenis van de logische app beperken zodat alleen aanvragen van specifieke IP-adresbereiken die gegevens kunnen weer geven. Als u bijvoorbeeld wilt blok keren dat iedereen toegang heeft tot invoer en uitvoer, geeft u een IP-adres bereik op zoals `0.0.0.0-0.0.0.0` . Alleen gebruikers met beheerders machtigingen kunnen deze beperking verwijderen. Dit biedt de mogelijkheid om just-in-time toegang te krijgen tot de gegevens van uw logische app. U kunt de IP-bereiken opgeven die u wilt beperken door gebruik te maken van de Azure Portal of in een Azure Resource Manager sjabloon die u gebruikt voor de implementatie van logische apps.

#### <a name="restrict-ip-ranges-in-azure-portal"></a>IP-bereiken in Azure Portal beperken

1. Open in de Azure Portal uw logische app in de ontwerp functie voor logische apps.

1. Selecteer **werk stroom instellingen**onder **instellingen**in het menu van de logische app.

1. **Access control configuration**  >  Selecteer **specifieke IP-bereiken**onder toegangs beheer configuratie**toegestane binnenkomende IP-adressen**.

1. Geef onder **IP-adresbereiken voor inhoud**de IP-adresbereiken op die toegang hebben tot inhoud van invoer en uitvoer. 

   Een geldig IP-bereik maakt gebruik van de volgende indelingen: *x. x. x. x/x* of *x. x. x. x-x. x.* x. x

#### <a name="restrict-ip-ranges-in-azure-resource-manager-template"></a>IP-bereiken in Azure Resource Manager sjabloon beperken

Als u de [implementatie van Logic apps automatiseert met behulp van Resource Manager-sjablonen](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), kunt u de IP-adresbereiken opgeven met behulp van de `accessControl` sectie in de `contents` resource definitie van de logische app, bijvoorbeeld:

``` json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {},
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {<workflow-definition>},
            "parameters": {},
            "accessControl": {
               "contents": {
                  "allowedCallerIpAddresses": [
                     {
                        "addressRange": "192.168.12.0/23"
                     },
                     {
                        "addressRange": "2001:0db8::/64"
                     }
                  ]
               }
            }
         }
      }
   ],
   "outputs": {}
}
```

<a name="obfuscate"></a>

### <a name="secure-data-in-run-history-by-using-obfuscation"></a>Gegevens in de uitvoerings geschiedenis beveiligen met behulp van een afvallen

Veel triggers en acties hebben instellingen voor het beveiligen van invoer, uitvoer of beide vanuit de uitvoerings geschiedenis van een logische app. [Lees deze overwegingen](#obfuscation-considerations)voordat u deze instellingen gebruikt voor het beveiligen van deze gegevens.

#### <a name="secure-inputs-and-outputs-in-the-designer"></a>Invoer en uitvoer in de ontwerp functie beveiligen

1. Open in de [Azure Portal](https://portal.azure.com)uw logische app in de ontwerp functie voor logische apps.

   ![Logische app openen in de ontwerp functie voor logische apps](./media/logic-apps-securing-a-logic-app/open-sample-logic-app-in-designer.png)

1. Selecteer de knop met weglatings tekens (**...**) op de trigger of de actie waar u gevoelige gegevens wilt beveiligen en selecteer vervolgens **instellingen**.

   ![Trigger-of actie-instellingen openen](./media/logic-apps-securing-a-logic-app/open-action-trigger-settings.png)

1. Schakel **beveiligde invoer**, **beveiligde uitvoer**of beide in. Wanneer u klaar bent, selecteert u **Gereed**.

   ![Schakel beveiligde invoer of beveiligde uitvoer in](./media/logic-apps-securing-a-logic-app/turn-on-secure-inputs-outputs.png)

   In de actie of de trigger wordt nu een vergrendelings pictogram weer gegeven in de titel balk.

   ![De titel balk van de actie of de trigger toont het vergrendelings pictogram](./media/logic-apps-securing-a-logic-app/lock-icon-action-trigger-title-bar.png)

   Tokens die beveiligde uitvoer van vorige acties vertegenwoordigen, worden ook vergrendelings pictogrammen weer gegeven. Wanneer u bijvoorbeeld een dergelijke uitvoer van de lijst met dynamische inhoud selecteert die u wilt gebruiken in een actie, toont dat token een vergrendelings pictogram.

   ![Token selecteren voor beveiligde uitvoer](./media/logic-apps-securing-a-logic-app/select-secured-token.png)

1. Wanneer de logische app wordt uitgevoerd, kunt u de geschiedenis voor die uitvoering weer geven.

   1. Selecteer in het **overzichts** venster van de logische app de uitvoering die u wilt weer geven.

   1. Vouw in het deel venster voor het uitvoeren van de **logische app** de acties uit die u wilt controleren.

      Als u hebt gekozen voor het verbergen van beide invoer en uitvoer, worden deze waarden nu verborgen weer gegeven.

      ![Verborgen invoer en uitvoer in uitvoerings geschiedenis](./media/logic-apps-securing-a-logic-app/hidden-data-run-history.png)

<a name="secure-data-code-view"></a>

#### <a name="secure-inputs-and-outputs-in-code-view"></a>Invoer en uitvoer in de code weergave beveiligen

In de onderliggende trigger of actie definitie kunt u de matrix toevoegen of bijwerken `runtimeConfiguration.secureData.properties` met een van beide of beide van de volgende waarden:

* `"inputs"`: Beveiligt invoer in uitvoerings geschiedenis.
* `"outputs"`: Hiermee worden uitvoer in uitvoerings geschiedenis beveiligd.

Hier volgen enkele [aandachtspunten om te controleren](#obfuscation-considerations) wanneer u deze instellingen gebruikt om u te helpen bij het beveiligen van deze gegevens.

```json
"<trigger-or-action-name>": {
   "type": "<trigger-or-action-type>",
   "inputs": {
      <trigger-or-action-inputs>
   },
   "runtimeConfiguration": {
      "secureData": {
         "properties": [
            "inputs",
            "outputs"
         ]
      }
   },
   <other-attributes>
}
```

<a name="obfuscation-considerations"></a>

#### <a name="considerations-when-securing-inputs-and-outputs"></a>Overwegingen bij het beveiligen van invoer en uitvoer

* Wanneer u de invoer of uitvoer van een trigger of actie verduidelijkt, worden Logic Apps de beveiligde gegevens niet naar Azure Log Analytics verzonden. U kunt ook geen [bijgehouden eigenschappen](../logic-apps/monitor-logic-apps-log-analytics.md#extend-data) toevoegen aan deze trigger of actie voor bewaking.

* De [Logic apps-API voor het verwerken van de werk stroom geschiedenis](https://docs.microsoft.com/rest/api/logic/) retourneert geen beveiligde uitvoer.

* Als u uitvoer wilt beveiligen op basis van een actie die invoer verduidelijkt of als deze expliciet worden uitgevoerd, schakelt u de **beveiligde uitvoer** in die actie hand matig in.

* Zorg ervoor dat u **beveiligde invoer** of **beveiligde uitvoer** inschakelt in downstream-acties waarbij u verwacht dat de uitvoerings geschiedenis deze gegevens verduidelijkt.

  **Instelling voor beveiligde uitvoer**

  Wanneer u **beveiligde uitvoer** in een trigger of actie hand matig inschakelt, Logic apps deze uitvoer in de uitvoerings geschiedenis verbergen. Als een stroomafwaartse actie deze beveiligde uitvoer expliciet gebruikt als invoer, Logic Apps de invoer van deze actie in de uitvoerings geschiedenis verbergen, maar wordt de instelling **beveiligde invoer** van de actie *niet ingeschakeld* .

  ![Beveiligde uitvoer als invoer en stroomafwaartse impact op de meeste acties](./media/logic-apps-securing-a-logic-app/secure-outputs-as-inputs-flow.png)

  De acties opstellen, parseren JSON en Response hebben alleen de instelling **beveiligde invoer** . Wanneer deze optie is ingeschakeld, worden de uitvoer van deze acties ook verborgen. Als deze acties expliciet gebruikmaken van de upstream beveiligde uitvoer als invoer, worden de invoer en uitvoer van deze acties door Logic Apps verborgen, maar worden de instelling **beveiligde invoer waarden** *niet ingeschakeld* . Als een stroomafwaartse actie expliciet gebruikmaakt van de verborgen uitvoer van de acties opstellen, parseren van JSON of respons als invoer, Logic Apps *de invoer of uitvoer van deze stroomafwaartse actie niet verbergen*.

  ![Beveiligde uitvoer als invoer met downstream-impact op specifieke acties](./media/logic-apps-securing-a-logic-app/secure-outputs-as-inputs-flow-special.png)

  **Instelling voor beveiligde invoer**

  Wanneer u **beveiligde invoer** in een trigger of actie hand matig inschakelt, Logic apps deze invoer in de uitvoerings geschiedenis verbergen. Als een stroomafwaartse actie expliciet gebruikmaakt van de zicht bare uitvoer van die trigger of actie als invoer, Logic Apps de invoer van deze stroomafwaartse actie verbergen in de uitvoerings geschiedenis, maar worden *geen* **beveiligde invoer** in deze actie ingeschakeld en worden de uitvoer van deze actie niet verborgen.

  ![Beveiligde invoer en downstream van invloed op de meeste acties](./media/logic-apps-securing-a-logic-app/secure-inputs-impact-on-downstream.png)

  Als de acties opstellen, parseren van JSON en respons expliciet gebruikmaken van de zicht bare uitvoer van de trigger of actie die de beveiligde invoer heeft, Logic Apps de invoer en uitvoer van deze acties verbergen, maar schakelt de instelling **beveiligde invoer** van deze actie *niet in* . Als een stroomafwaartse actie expliciet gebruikmaakt van de verborgen uitvoer van de acties opstellen, parseren van JSON of respons als invoer, Logic Apps *de invoer of uitvoer van deze stroomafwaartse actie niet verbergen*.

  ![Beveiligde invoer en downstream-impact op specifieke acties](./media/logic-apps-securing-a-logic-app/secure-inputs-flow-special.png)

<a name="secure-action-parameters"></a>

## <a name="access-to-parameter-inputs"></a>Toegang tot parameter invoer

Als u in verschillende omgevingen implementeert, kunt u de waarden in uw werk stroom definitie parameterizingen die variëren op basis van deze omgevingen. Op die manier kunt u hardcoded gegevens voor komen door een [Azure Resource Manager sjabloon](../azure-resource-manager/templates/overview.md) te gebruiken om uw logische app te implementeren, gevoelige gegevens te beschermen door beveiligde para meters te definiëren en deze gegevens als afzonderlijke invoer door te geven via de [para meters van de sjabloon](../azure-resource-manager/templates/template-parameters.md) met behulp van een [parameter bestand](../azure-resource-manager/templates/parameter-files.md).

Als u bijvoorbeeld HTTP-acties met [Azure Active Directory open-verificatie](#azure-active-directory-oauth-authentication) (Azure AD OAuth) verifieert, kunt u de para meters die de client-id en het client geheim accepteren die worden gebruikt voor verificatie, definiëren en verbergen. Als u deze para meters in uw logische app wilt definiëren, gebruikt u de `parameters` sectie in de werk stroom definitie van de logische app en de Resource Manager-sjabloon voor implementatie. Als u de parameter waarden die u niet wilt weer geven tijdens het bewerken van de logische app of de weer gave van de uitvoerings geschiedenis wilt helpen beveiligen, definieert u de para meters met behulp van de- `securestring` of- `secureobject` type en gebruikt u code ring. Para meters van dit type worden niet geretourneerd met de resource definitie en zijn niet toegankelijk wanneer de resource wordt weer gegeven na de implementatie. Als u tijdens runtime toegang wilt krijgen tot deze parameter waarden, gebruikt u de `@parameters('<parameter-name>')` expressie in de definitie van uw werk stroom. Deze expressie wordt alleen geëvalueerd tijdens runtime en wordt beschreven door de [taal van de werk stroom definitie](../logic-apps/logic-apps-workflow-definition-language.md).

> [!NOTE]
> Als u een para meter gebruikt in een aanvraag header of hoofd tekst, is die para meter mogelijk zichtbaar wanneer u de uitvoerings geschiedenis van de logische app en uitgaande HTTP-aanvraag weergeeft. Zorg ervoor dat u ook uw beleids regels voor inhouds toegang dienovereenkomstig instelt. U kunt ook een afkorting [gebruiken om](#obfuscate) invoer en uitvoer in de uitvoerings geschiedenis te verbergen. Autorisatie headers worden nooit weer gegeven via invoer of uitvoer. Dus als er een geheim wordt gebruikt, kan dat geheim niet worden opgehaald.

Zie voor meer informatie deze secties in dit onderwerp:

* [Veilige para meters in werk stroom definities](#secure-parameters-workflow)
* [Gegevens in de uitvoerings geschiedenis beveiligen met behulp van een afvallen](#obfuscate)

Als u de [implementatie van Logic apps automatiseert met behulp van Resource Manager-sjablonen](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), kunt u de [para meters](../azure-resource-manager/templates/template-parameters.md)voor beveiligde sjablonen definiëren die tijdens de implementatie worden geëvalueerd met behulp van de-en- `securestring` `secureobject` typen. Voor het definiëren van sjabloon parameters gebruikt u de sectie van het hoogste niveau van uw sjabloon `parameters` . deze is gescheiden en verschilt van de sectie van de werk stroom definitie `parameters` . Als u de waarden voor sjabloon parameters wilt opgeven, gebruikt u een afzonderlijk [parameter bestand](../azure-resource-manager/templates/parameter-files.md).

Als u bijvoorbeeld geheimen gebruikt, kunt u de para meters voor beveiligde sjablonen definiëren en gebruiken om deze geheimen op te halen uit [Azure Key Vault](../key-vault/general/overview.md) tijdens de implementatie. U kunt vervolgens naar de sleutel kluis en het geheim in het parameter bestand verwijzen. Zie deze onderwerpen voor meer informatie:

* [Gevoelige waarden door geven tijdens de implementatie met behulp van Azure Key Vault](../azure-resource-manager/templates/key-vault-parameter.md)
* [Veilige para meters in azure Resource Manager sjablonen](#secure-parameters-deployment-template) verderop in dit onderwerp

<a name="secure-parameters-workflow"></a>

### <a name="secure-parameters-in-workflow-definitions"></a>Veilige para meters in werk stroom definities

Als u gevoelige informatie in de werk stroom definitie van de logische app wilt beveiligen, gebruikt u beveiligde para meters zodat deze informatie niet zichtbaar is nadat u uw logische app hebt opgeslagen. Stel dat u voor een HTTP-actie basis verificatie nodig hebt, waarbij een gebruikers naam en wacht woord worden gebruikt. In de definitie van de werk stroom `parameters` definieert de sectie de `basicAuthPasswordParam` en- `basicAuthUsernameParam` para meters met behulp van het `securestring` type. De actie definitie verwijst vervolgens naar deze para meters in de `authentication` sectie.

```json
"definition": {
   "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
   "actions": {
      "HTTP": {
         "type": "Http",
         "inputs": {
            "method": "GET",
            "uri": "https://www.microsoft.com",
            "authentication": {
               "type": "Basic",
               "username": "@parameters('basicAuthUsernameParam')",
               "password": "@parameters('basicAuthPasswordParam')"
            }
         },
         "runAfter": {}
      }
   },
   "parameters": {
      "basicAuthPasswordParam": {
         "type": "securestring"
      },
      "basicAuthUsernameParam": {
         "type": "securestring"
      }
   },
   "triggers": {
      "manual": {
         "type": "Request",
         "kind": "Http",
         "inputs": {
            "schema": {}
         }
      }
   },
   "contentVersion": "1.0.0.0",
   "outputs": {}
}
```

<a name="secure-parameters-deployment-template"></a>

### <a name="secure-parameters-in-azure-resource-manager-templates"></a>Veilige para meters in Azure Resource Manager sjablonen

Een [Resource Manager-sjabloon](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md) voor een logische app heeft meerdere `parameters` secties. Als u wacht woorden, sleutels, geheimen en andere gevoelige informatie wilt beveiligen, definieert u de beveiligde para meters op sjabloon niveau en op het niveau van de werk stroom definitie met behulp van het `securestring` of- `secureobject` type. U kunt deze waarden vervolgens opslaan in [Azure Key Vault](../key-vault/general/overview.md) en het [parameter bestand](../azure-resource-manager/templates/parameter-files.md) gebruiken om te verwijzen naar de sleutel kluis en het geheim. Uw sjabloon haalt vervolgens die informatie op tijdens de implementatie. Zie voor meer informatie [gevoelige waarden door geven tijdens de implementatie met behulp van Azure Key Vault](../azure-resource-manager/templates/key-vault-parameter.md).

Hier vindt u meer informatie over deze `parameters` secties:

* Op het hoogste niveau van de sjabloon `parameters` definieert een sectie de para meters voor de waarden die de sjabloon gebruikt bij de *implementatie*. Deze waarden kunnen bijvoorbeeld verbindings reeksen bevatten voor een specifieke implementatie omgeving. U kunt deze waarden vervolgens opslaan in een afzonderlijk [parameter bestand](../azure-resource-manager/templates/parameter-files.md), waardoor het wijzigen van deze waarden eenvoudiger wordt.

* In de resource definitie van uw logische app, maar buiten uw werk stroom definitie, `parameters` geeft een sectie de waarden voor de para meters van uw werk stroom definitie op. In deze sectie kunt u deze waarden toewijzen met behulp van sjabloon expressies die verwijzen naar de para meters van uw sjabloon. Deze expressies worden geëvalueerd tijdens de implementatie.

* In de definitie van de werk stroom `parameters` definieert een sectie de para meters die door de logische app worden gebruikt tijdens runtime. U kunt vervolgens naar deze para meters in de werk stroom van de logische app verwijzen met behulp van definitie expressies voor werk stromen, die tijdens runtime worden geëvalueerd.

Deze voorbeeld sjabloon met meerdere beveiligde parameter definities die gebruikmaken van het `securestring` type:

| Parameternaam | Beschrijving |
|----------------|-------------|
| `TemplatePasswordParam` | Een sjabloon parameter die vervolgens een wacht woord accepteert dat vervolgens wordt door gegeven aan de para meter van de werk stroom definitie `basicAuthPasswordParam` |
| `TemplateUsernameParam` | Een sjabloon parameter die vervolgens een gebruikers naam accepteert die vervolgens wordt door gegeven aan de para meter van de werk stroom definitie `basicAuthUserNameParam` |
| `basicAuthPasswordParam` | Een para meter voor de werk stroom definitie die het wacht woord voor basis verificatie in een HTTP-actie accepteert |
| `basicAuthUserNameParam` | Een para meter voor de werk stroom definitie die de gebruikers naam voor basis verificatie in een HTTP-actie accepteert |
|||

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "LogicAppName": {
         "type": "string",
         "minLength": 1,
         "maxLength": 80,
         "metadata": {
            "description": "Name of the Logic App."
         }
      },
      "TemplatePasswordParam": {
         "type": "securestring"
      },
      "TemplateUsernameParam": {
         "type": "securestring"
      },
      "LogicAppLocation": {
         "type": "string",
         "defaultValue": "[resourceGroup().location]",
         "allowedValues": [
            "[resourceGroup().location]",
            "eastasia",
            "southeastasia",
            "centralus",
            "eastus",
            "eastus2",
            "westus",
            "northcentralus",
            "southcentralus",
            "northeurope",
            "westeurope",
            "japanwest",
            "japaneast",
            "brazilsouth",
            "australiaeast",
            "australiasoutheast",
            "southindia",
            "centralindia",
            "westindia",
            "canadacentral",
            "canadaeast",
            "uksouth",
            "ukwest",
            "westcentralus",
            "westus2"
         ],
         "metadata": {
            "description": "Location of the Logic App."
         }
      }
   },
   "variables": {},
   "resources": [
      {
         "name": "[parameters('LogicAppName')]",
         "type": "Microsoft.Logic/workflows",
         "location": "[parameters('LogicAppLocation')]",
         "tags": {
            "displayName": "LogicApp"
         },
         "apiVersion": "2016-06-01",
         "properties": {
            "definition": {
               "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-0601/workflowdefinition.json#",
               "actions": {
                  "HTTP": {
                     "type": "Http",
                     "inputs": {
                        "method": "GET",
                        "uri": "https://www.microsoft.com",
                        "authentication": {
                           "type": "Basic",
                           "username": "@parameters('basicAuthUsernameParam')",
                           "password": "@parameters('basicAuthPasswordParam')"
                        }
                     },
                  "runAfter": {}
                  }
               },
               "parameters": {
                  "basicAuthPasswordParam": {
                     "type": "securestring"
                  },
                  "basicAuthUsernameParam": {
                     "type": "securestring"
                  }
               },
               "triggers": {
                  "manual": {
                     "type": "Request",
                     "kind": "Http",
                     "inputs": {
                        "schema": {}
                     }
                  }
               },
               "contentVersion": "1.0.0.0",
               "outputs": {}
            },
            "parameters": {
               "basicAuthPasswordParam": {
                  "value": "[parameters('TemplatePasswordParam')]"
               },
               "basicAuthUsernameParam": {
                  "value": "[parameters('TemplateUsernameParam')]"
               }
            }
         }
      }
   ],
   "outputs": {}
}
```

<a name="secure-outbound-requests"></a>

## <a name="access-to-services-and-systems-called-from-logic-apps"></a>Toegang tot services en systemen die worden aangeroepen vanuit Logic apps

Hier volgen enkele manieren waarop u eind punten kunt beveiligen die oproepen of aanvragen ontvangen van uw logische app:

* Verificatie toevoegen aan uitgaande aanvragen.

  Wanneer u werkt met een HTTP-trigger of actie die uitgaande aanroepen uitvoert, zoals HTTP, HTTP + Swagger of webhook, kunt u verificatie toevoegen aan de aanvraag die wordt verzonden door uw logische app. U kunt bijvoorbeeld de volgende verificatie typen selecteren:

  * [Basisverificatie](#basic-authentication)

  * [Verificatie van client certificaten](#client-certificate-authentication)

  * [Active Directory OAuth-verificatie](#azure-active-directory-oauth-authentication)

  * [Verificatie van beheerde identiteit](#managed-identity-authentication)
  
  Zie [verificatie toevoegen aan uitgaande oproepen](#add-authentication-outbound) verderop in dit onderwerp voor meer informatie.

* Beperk de toegang tot IP-adressen van logische apps.

  Alle aanroepen naar eind punten van Logic apps zijn afkomstig van specifieke IP-adressen die zijn gebaseerd op de regio's van uw Logic apps. U kunt filters toevoegen die alleen aanvragen van deze IP-adressen accepteren. Zie [limieten en configuratie voor Azure Logic apps](logic-apps-limits-and-config.md#configuration)om deze IP-adressen op te halen.

* Verbeter de beveiliging van verbindingen met on-premises systemen.

  Azure Logic Apps biedt integratie met deze services om veiligere en betrouw bare on-premises communicatie te bieden.

  * On-premises gegevensgateway

    Veel beheerde connectors in Azure Logic Apps ondersteunen beveiligde verbindingen met on-premises systemen, zoals bestands systeem, SQL, share point en DB2. De Gateway verzendt gegevens via on-premises bronnen op versleutelde kanalen via de Azure Service Bus. Al het verkeer is afkomstig van het beveiligde uitgaande verkeer van de gateway-agent. Meer informatie [over de werking van de on-premises gegevens gateway](logic-apps-gateway-install.md#gateway-cloud-service).

  * Verbinding maken via Azure API Management

    [Azure API Management](../api-management/api-management-key-concepts.md) biedt opties voor on-premises verbindingen, zoals site-naar-site virtueel particulier netwerk en ExpressRoute-integratie voor beveiligde proxy en communicatie met on-premises systemen. Vanuit de werk stroom van uw logische app in de ontwerp functie voor logische apps kunt u een API selecteren die wordt weer gegeven door API Management, die snelle toegang biedt tot on-premises systemen.

<a name="add-authentication-outbound"></a>

## <a name="add-authentication-to-outbound-calls"></a>Verificatie toevoegen aan uitgaande oproepen

HTTP-en HTTPS-eind punten ondersteunen verschillende soorten verificatie. Op basis van de trigger of actie die u gebruikt om uitgaande aanroepen of aanvragen voor toegang tot deze eind punten te maken, kunt u kiezen uit verschillende soorten verificatie typen. Gebruik beveiligde para meters en codeer gegevens als dat nodig is om ervoor te zorgen dat u alle gevoelige informatie beveiligt die uw logische app verzorgt. Zie [toegang tot para meters invoeren](#secure-action-parameters)voor meer informatie over het gebruik en het beveiligen van para meters.

> [!NOTE]
> In de ontwerp functie voor logische apps is de eigenschap **Authentication** mogelijk verborgen voor sommige triggers en acties waarbij u het verificatie type kunt opgeven. Als u wilt dat de eigenschap wordt weer gegeven in deze gevallen, opent u de lijst **nieuwe para meter toevoegen** op de trigger of actie en selecteert u **verificatie**. Zie [toegang verifiëren met beheerde identiteit](../logic-apps/create-managed-service-identity.md#authenticate-access-with-identity)voor meer informatie.

| Verificatietype | Ondersteund door |
|---------------------|--------------|
| [Standaard](#basic-authentication) | Azure API Management, Azure-app Services, HTTP, HTTP + Swagger, HTTP-webhook |
| [Client certificaat](#client-certificate-authentication) | Azure API Management, Azure-app Services, HTTP, HTTP + Swagger, HTTP-webhook |
| [Active Directory OAuth](#azure-active-directory-oauth-authentication) | Azure API Management, Azure-app Services, Azure Functions, HTTP, HTTP + Swagger, HTTP-webhook |
| [Onbewerkt](#raw-authentication) | Azure API Management, Azure-app Services, Azure Functions, HTTP, HTTP + Swagger, HTTP-webhook |
| [Beheerde identiteit](#managed-identity-authentication) | Azure API Management, Azure-app Services, Azure Functions, HTTP, HTTP + Swagger, HTTP-webhook |
|||

<a name="basic-authentication"></a>

### <a name="basic-authentication"></a>Basisverificatie

Als de optie [basis](../active-directory-b2c/secure-rest-api.md) beschikbaar is, geeft u de volgende eigenschaps waarden op:

| Eigenschap (Designer) | Eigenschap (JSON) | Vereist | Waarde | Beschrijving |
|---------------------|-----------------|----------|-------|-------------|
| **Verificatie** | `type` | Ja | Basic | Het te gebruiken verificatie type |
| **Gebruikers** | `username` | Ja | <*gebruikers naam*>| De gebruikers naam voor het verifiëren van de toegang tot het eind punt van de doel service |
| **Wachtwoord** | `password` | Ja | <*wacht woord*> | Het wacht woord voor het verifiëren van de toegang tot het eind punt van de doel service |
||||||

Wanneer u [beveiligde para meters](#secure-action-parameters) gebruikt voor het verwerken en beveiligen van gevoelige informatie, bijvoorbeeld in een [Azure Resource Manager sjabloon voor het automatiseren van de implementatie](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), kunt u expressies gebruiken om toegang te krijgen tot deze parameter waarden tijdens runtime. In dit voor beeld van een HTTP-actie definitie wordt de verificatie opgegeven `type` als `Basic` en wordt de [functie para meters ()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) gebruikt om de parameter waarden op te halen:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "Basic",
         "username": "@parameters('userNameParam')",
         "password": "@parameters('passwordParam')"
      }
  },
  "runAfter": {}
}
```

<a name="client-certificate-authentication"></a>

### <a name="client-certificate-authentication"></a>Verificatie van client certificaten

Als de optie [client certificaat](../active-directory/authentication/active-directory-certificate-based-authentication-get-started.md) beschikbaar is, geeft u de volgende eigenschaps waarden op:

| Eigenschap (Designer) | Eigenschap (JSON) | Vereist | Waarde | Beschrijving |
|---------------------|-----------------|----------|-------|-------------|
| **Verificatie** | `type` | Ja | **Client certificaat** <br>of <br>`ClientCertificate` | Het verificatie type dat moet worden gebruikt voor TLS/SSL-client certificaten <p><p>**Opmerking**: Hoewel zelfondertekende certificaten worden ondersteund, worden zelfondertekende certificaten voor TLS/SSL niet ondersteund. De HTTP-connector biedt geen ondersteuning voor tussenliggende TLS/SSL-certificaten. |
| **Pfx** | `pfx` | Ja | <*encoded-pfx-file-content*> | De met base64 gecodeerde inhoud van een PFX-bestand (Personal Information Exchange) <p><p>Als u het PFX-bestand wilt converteren naar een base64-gecodeerde indeling, kunt u Power shell gebruiken door de volgende stappen uit te voeren: <p>1. Sla de certificaat inhoud op in een variabele: <p>   `$pfx_cert = get-content 'c:\certificate.pfx' -Encoding Byte` <p>2. Converteer de certificaat inhoud met behulp van de `ToBase64String()` functie en sla die inhoud op in een tekst bestand: <p>   `[System.Convert]::ToBase64String($pfx_cert) | Out-File 'pfx-encoded-bytes.txt'` |
| **Wachtwoord** | `password`| Nee | <*wacht woord voor pfx-bestand*> | Het wacht woord voor toegang tot het PFX-bestand |
|||||

Wanneer u [beveiligde para meters](#secure-action-parameters) gebruikt voor het verwerken en beveiligen van gevoelige informatie, bijvoorbeeld in een [Azure Resource Manager sjabloon voor het automatiseren van de implementatie](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), kunt u expressies gebruiken om toegang te krijgen tot deze parameter waarden tijdens runtime. In dit voor beeld van een HTTP-actie definitie wordt de verificatie opgegeven `type` als `ClientCertificate` en wordt de [functie para meters ()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) gebruikt om de parameter waarden op te halen:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "ClientCertificate",
         "pfx": "@parameters('pfxParam')",
         "password": "@parameters('passwordParam')"
      }
   },
   "runAfter": {}
}
```

Zie de volgende onderwerpen voor meer informatie over het beveiligen van services met behulp van verificatie via client certificaten:

* [Verbeter de beveiliging van Api's met behulp van verificatie op basis van client certificaten in azure API Management](../api-management/api-management-howto-mutual-certificates-for-clients.md)
* [Verbeter de beveiliging van back-end-services door gebruik te maken van verificatie van client certificaten in azure API Management](../api-management/api-management-howto-mutual-certificates.md)
* [Verbeter de beveiliging van uw REST-service door gebruik te maken van client certificaten](../active-directory-b2c/secure-rest-api.md)
* [Certificaat referenties voor toepassings verificatie](../active-directory/develop/active-directory-certificate-credentials.md)
* [Gebruik een TLS/SSL-certificaat in uw code in Azure App Service](../app-service/configure-ssl-certificate-in-code.md)

<a name="azure-active-directory-oauth-authentication"></a>

### <a name="azure-active-directory-open-authentication"></a>Verificatie Azure Active Directory openen

Op aanvraag triggers kunt u [Azure Active Directory open-verificatie](../active-directory/develop/about-microsoft-identity-platform.md) (Azure AD OAuth) gebruiken voor de verificatie van binnenkomende oproepen nadat u een [Azure AD-autorisatie beleid hebt ingesteld](#enable-oauth) voor uw logische app. Voor alle andere triggers en acties die het **Active Directory OAuth** -verificatie type voor u selecteren, geeft u de volgende eigenschaps waarden op:

| Eigenschap (Designer) | Eigenschap (JSON) | Vereist | Waarde | Beschrijving |
|---------------------|-----------------|----------|-------|-------------|
| **Verificatie** | `type` | Ja | **Active Directory OAuth** <br>of <br>`ActiveDirectoryOAuth` | Het verificatie type dat moet worden gebruikt. Het [OAuth 2,0-protocol](../active-directory/develop/v2-overview.md)wordt momenteel gevolgd door Logic apps. |
| **Instantie** | `authority` | Nee | <*URL-voor-Authority-token-Uitgever*> | De URL voor de instantie die het verificatie token levert. Deze waarde is standaard ingesteld op `https://login.windows.net` . |
| **Tenant** | `tenant` | Ja | <*Tenant-ID*> | De Tenant-ID voor de Azure AD-Tenant |
| **Doelgroep** | `audience` | Ja | <*resource-naar-autoriseren*> | De resource die u wilt gebruiken voor autorisatie, bijvoorbeeld`https://management.core.windows.net/` |
| **Client-ID** | `clientId` | Ja | <*client-ID*> | De client-ID voor de app die autorisatie aanvraagt |
| **Referentie type** | `credentialType` | Ja | Certificaat <br>of <br>Geheim | Het referentie type dat door de client wordt gebruikt voor het aanvragen van autorisatie. Deze eigenschap en waarde worden niet weer gegeven in de onderliggende definitie van de logische app, maar bepaalt de eigenschappen die worden weer gegeven voor het geselecteerde referentie type. |
| **Geheim** | `secret` | Ja, maar alleen voor het referentie type ' geheim ' | <*client-geheim*> | Het client geheim voor het aanvragen van autorisatie |
| **Pfx** | `pfx` | Ja, maar alleen voor het referentie type ' certificaat ' | <*encoded-pfx-file-content*> | De met base64 gecodeerde inhoud van een PFX-bestand (Personal Information Exchange) |
| **Wachtwoord** | `password` | Ja, maar alleen voor het referentie type ' certificaat ' | <*wacht woord voor pfx-bestand*> | Het wacht woord voor toegang tot het PFX-bestand |
|||||

Wanneer u [beveiligde para meters](#secure-action-parameters) gebruikt voor het verwerken en beveiligen van gevoelige informatie, bijvoorbeeld in een [Azure Resource Manager sjabloon voor het automatiseren van de implementatie](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), kunt u expressies gebruiken om toegang te krijgen tot deze parameter waarden tijdens runtime. In dit voor beeld van een HTTP-actie definitie wordt de verificatie ingesteld `type` als `ActiveDirectoryOAuth` , het referentie type als en `Secret` wordt de [functie para meters ()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) gebruikt om de parameter waarden op te halen:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "ActiveDirectoryOAuth",
         "tenant": "@parameters('tenantIdParam')",
         "audience": "https://management.core.windows.net/",
         "clientId": "@parameters('clientIdParam')",
         "credentialType": "Secret",
         "secret": "@parameters('secretParam')"
     }
   },
   "runAfter": {}
}
```

<a name="raw-authentication"></a>

### <a name="raw-authentication"></a>Onbewerkte authenticatie

Als de **onbewerkte** optie beschikbaar is, kunt u dit verificatie type gebruiken wanneer u [verificatie schema's](https://iana.org/assignments/http-authschemes/http-authschemes.xhtml) moet gebruiken die niet voldoen aan het [OAuth 2,0-protocol](https://oauth.net/2/). Met dit type kunt u hand matig de waarde voor de autorisatie-header maken die u met de uitgaande aanvraag verzendt en die header waarde opgeven in de trigger of actie.

Hier ziet u een voor beeld van een header voor een HTTPS-aanvraag die volgt op het [OAuth 1,0-protocol](https://tools.ietf.org/html/rfc5849):

```text
Authorization: OAuth realm="Photos",
   oauth_consumer_key="dpf43f3p2l4k3l03",
   oauth_signature_method="HMAC-SHA1",
   oauth_timestamp="137131200",
   oauth_nonce="wIjqoS",
   oauth_callback="http%3A%2F%2Fprinter.example.com%2Fready",
   oauth_signature="74KNZJeDHnMBp0EMJ9ZHt%2FXKycU%3D"
```

Geef in de trigger of actie die onbewerkte authenticatie ondersteunt de volgende eigenschaps waarden op:

| Eigenschap (Designer) | Eigenschap (JSON) | Vereist | Waarde | Beschrijving |
|---------------------|-----------------|----------|-------|-------------|
| **Verificatie** | `type` | Ja | Onbewerkt | Het te gebruiken verificatie type |
| **Waarde** | `value` | Ja | <*autorisatie-header-waarde*> | De waarde van de autorisatie-header die moet worden gebruikt voor verificatie |
||||||

Wanneer u [beveiligde para meters](#secure-action-parameters) gebruikt voor het verwerken en beveiligen van gevoelige informatie, bijvoorbeeld in een [Azure Resource Manager sjabloon voor het automatiseren van de implementatie](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), kunt u expressies gebruiken om toegang te krijgen tot deze parameter waarden tijdens runtime. In dit voor beeld wordt de HTTP-actie definitie opgegeven `type` als `Raw` , en wordt de [functie para meters ()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) gebruikt om de parameter waarden op te halen:

```json
"HTTP": {
   "type": "Http",
   "inputs": {
      "method": "GET",
      "uri": "@parameters('endpointUrlParam')",
      "authentication": {
         "type": "Raw",
         "value": "@parameters('authHeaderParam')"
      }
   },
   "runAfter": {}
}
```

<a name="managed-identity-authentication"></a>

### <a name="managed-identity-authentication"></a>Verificatie van beheerde identiteit

Als de optie [beheerde identiteit](../active-directory/managed-identities-azure-resources/overview.md) beschikbaar is, kan uw logische app gebruikmaken van de door het systeem toegewezen identiteit of een *enkele* hand matig gemaakte door de gebruiker toegewezen identiteit voor het verifiëren van toegang tot andere bronnen die worden beveiligd door Azure Active Directory (Azure AD) zonder dat u zich hoeft aan te melden. Azure beheert deze identiteit voor u en helpt u bij het beveiligen van uw referenties omdat u geen geheimen hoeft op te geven of te draaien. Meer informatie over [Azure-Services die ondersteuning bieden voor beheerde identiteiten voor Azure AD-verificatie](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication).

1. Voordat uw logische app een beheerde identiteit kan gebruiken, volgt u de stappen in [toegang tot Azure-resources verifiëren door beheerde identiteiten te gebruiken in azure Logic apps](../logic-apps/create-managed-service-identity.md). Met deze stappen wordt de beheerde identiteit voor uw logische app ingeschakeld en wordt de toegang tot de Azure-doel bron ingesteld.

1. Voordat een Azure-functie een beheerde identiteit kan gebruiken, moet u eerst [verificatie voor Azure functions inschakelen](../logic-apps/logic-apps-azure-functions.md#enable-authentication-for-azure-functions).

1. Geef in de trigger of actie waar u de beheerde identiteit wilt gebruiken, de volgende eigenschaps waarden op:

   | Eigenschap (Designer) | Eigenschap (JSON) | Vereist | Waarde | Beschrijving |
   |---------------------|-----------------|----------|-------|-------------|
   | **Verificatie** | `type` | Ja | **Beheerde identiteit** <br>of <br>`ManagedServiceIdentity` | Het te gebruiken verificatie type |
   | **Beheerde identiteit** | `identity` | Ja | * **Door het systeem toegewezen beheerde identiteit** <br>of <br>`SystemAssigned` <p><p>* <door de *gebruiker toegewezen identiteits naam*> | De beheerde identiteit die moet worden gebruikt |
   | **Doelgroep** | `audience` | Ja | <*doel-Resource-ID*> | De resource-ID voor de doel resource waartoe u toegang wilt krijgen. <p>`https://storage.azure.com/`Maakt bijvoorbeeld de [toegangs tokens](../active-directory/develop/access-tokens.md) voor verificatie geldig voor alle opslag accounts. U kunt echter ook een URL voor de basis service opgeven, zoals `https://fabrikamstorageaccount.blob.core.windows.net` voor een specifiek opslag account. <p>**Opmerking**: de eigenschap **doel groep** kan in sommige triggers of acties worden verborgen. Als u deze eigenschap zichtbaar wilt maken, opent u de lijst **nieuwe para meter toevoegen** in de trigger of actie en selecteert u **doel groep**. <p><p>**Belang rijk**: Zorg ervoor dat deze doel resource-id *precies overeenkomt* met de waarde die door Azure AD wordt verwacht, inclusief alle vereiste afsluitende slashes. De `https://storage.azure.com/` resource-id voor alle Azure Blob Storage-accounts vereist dus een afsluitende slash. De resource-ID voor een specifiek opslag account vereist echter geen afsluitende slash. Zie [Azure-Services die ondersteuning bieden voor Azure AD](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)om deze resource-id's te vinden. |
   |||||

   Wanneer u [beveiligde para meters](#secure-action-parameters) gebruikt voor het verwerken en beveiligen van gevoelige informatie, bijvoorbeeld in een [Azure Resource Manager sjabloon voor het automatiseren van de implementatie](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md), kunt u expressies gebruiken om toegang te krijgen tot deze parameter waarden tijdens runtime. In dit voor beeld van een HTTP-actie definitie wordt de verificatie opgegeven `type` als `ManagedServiceIdentity` en wordt de [functie para meters ()](../logic-apps/workflow-definition-language-functions-reference.md#parameters) gebruikt om de parameter waarden op te halen:

   ```json
   "HTTP": {
      "type": "Http",
      "inputs": {
         "method": "GET",
         "uri": "@parameters('endpointUrlParam')",
         "authentication": {
            "type": "ManagedServiceIdentity",
            "identity": "SystemAssigned",
            "audience": "https://management.azure.com/"
         },
      },
      "runAfter": {}
   }
   ```

## <a name="next-steps"></a>Volgende stappen

* [Implementatie voor Azure Logic Apps automatiseren](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)  
* [Logische apps bewaken](../logic-apps/monitor-logic-apps-log-analytics.md)  
* [Fouten en problemen van logische apps diagnosticeren](../logic-apps/logic-apps-diagnosing-failures.md)  
* [Implementatie van logische apps automatiseren](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)
