---
title: Geavanceerd gebruik van authn/autho
description: Meer informatie over het aanpassen van de functie voor verificatie en autorisatie in App Service voor verschillende scenario's en het ophalen van gebruikers claims en verschillende tokens.
ms.topic: article
ms.date: 10/24/2019
ms.custom: seodec18
ms.openlocfilehash: d57b196bf95ebdf31bc459ad4b9d718fd32ca495
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "79280831"
---
# <a name="advanced-usage-of-authentication-and-authorization-in-azure-app-service"></a>Geavanceerd gebruik van verificatie en autorisatie in Azure App Service

In dit artikel wordt beschreven hoe u de ingebouwde [verificatie en autorisatie in app service](overview-authentication-authorization.md)kunt aanpassen en hoe u identiteit kunt beheren vanuit uw toepassing. 

Als u snel aan de slag wilt gaan, raadpleegt u een van de volgende zelf studies:

* [Zelf studie: gebruikers end-to-end verifiëren en autoriseren in Azure App Service (Windows)](app-service-web-tutorial-auth-aad.md)
* [Zelf studie: gebruikers end-to-end verifiëren en autoriseren in Azure App Service voor Linux](containers/tutorial-auth-aad.md)
* [Uw app configureren voor aanmelding met Azure Active Directory](configure-authentication-provider-aad.md)
* [Uw app configureren voor aanmelding met Facebook](configure-authentication-provider-facebook.md)
* [Uw app configureren voor aanmelding met Google](configure-authentication-provider-google.md)
* [Uw app configureren voor aanmelding met een Microsoft Account](configure-authentication-provider-microsoft.md)
* [Uw app configureren voor aanmelding met Twitter](configure-authentication-provider-twitter.md)

## <a name="use-multiple-sign-in-providers"></a>Meerdere aanmeldings providers gebruiken

De Portal configuratie biedt geen mogelijkheid om meerdere aanmeld providers te presen teren aan uw gebruikers (zoals Facebook en Twitter). Het is echter moeilijk om de functionaliteit toe te voegen aan uw app. De stappen worden als volgt beschreven:

Configureer eerst op de pagina **verificatie/autorisatie** in de Azure Portal elke id-provider die u wilt inschakelen.

Selecteer **anonieme aanvragen toestaan (geen actie)** **wanneer de aanvraag niet is geverifieerd in actie om uit te voeren**.

Voeg op de aanmeldings pagina of in de navigatie balk of op een andere locatie van uw app een aanmeldings koppeling toe aan elk van de providers die u hebt ingeschakeld (`/.auth/login/<provider>`). Bijvoorbeeld:

```HTML
<a href="/.auth/login/aad">Log in with Azure AD</a>
<a href="/.auth/login/microsoftaccount">Log in with Microsoft Account</a>
<a href="/.auth/login/facebook">Log in with Facebook</a>
<a href="/.auth/login/google">Log in with Google</a>
<a href="/.auth/login/twitter">Log in with Twitter</a>
```

Wanneer de gebruiker op een van de koppelingen klikt, wordt de betreffende aanmeldings pagina geopend om zich aan te melden bij de gebruiker.

Als u de aanmelding van een gebruiker naar een aangepaste URL wilt door sturen, gebruikt `post_login_redirect_url` u de query teken reeks parameter (niet met de omleidings-URI in de configuratie van de ID-provider). Gebruik bijvoorbeeld de volgende HTML-code om `/Home/Index` door de gebruiker te navigeren naar na het aanmelden:

```HTML
<a href="/.auth/login/<provider>?post_login_redirect_url=/Home/Index">Log in</a>
```

## <a name="validate-tokens-from-providers"></a>Tokens van providers valideren

In een client gerichte aanmelding meldt de toepassing de gebruiker hand matig aan bij de provider en verzendt het verificatie token naar App Service voor validatie (Zie [verificatie stroom](overview-authentication-authorization.md#authentication-flow)). Met deze validatie wordt u geen toegang verleend tot de gewenste app-resources, maar een geslaagde validatie geeft u een sessie token dat u kunt gebruiken voor toegang tot app-resources. 

Als u het provider token wilt valideren, moet App Service app eerst worden geconfigureerd met de gewenste provider. Tijdens runtime, na het ophalen van het verificatie token van uw provider, plaatst u het `/.auth/login/<provider>` token voor validatie. Bijvoorbeeld: 

```
POST https://<appname>.azurewebsites.net/.auth/login/aad HTTP/1.1
Content-Type: application/json

{"id_token":"<token>","access_token":"<token>"}
```

De token indeling is enigszins afhankelijk van de provider. Raadpleeg de volgende tabel voor meer informatie:

| Provider waarde | Vereist in de hoofd tekst van de aanvraag | Opmerkingen |
|-|-|-|
| `aad` | `{"access_token":"<access_token>"}` | |
| `microsoftaccount` | `{"access_token":"<token>"}` | De `expires_in` eigenschap is optioneel. <br/>Bij het aanvragen van het token van Live Services vraagt u `wl.basic` het bereik altijd aan. |
| `google` | `{"id_token":"<id_token>"}` | De `authorization_code` eigenschap is optioneel. Indien opgegeven, kan deze eventueel ook vergezeld gaan van de `redirect_uri` eigenschap. |
| `facebook`| `{"access_token":"<user_access_token>"}` | Gebruik een geldig [token voor gebruikers toegang](https://developers.facebook.com/docs/facebook-login/access-tokens) van Facebook. |
| `twitter` | `{"access_token":"<access_token>", "access_token_secret":"<acces_token_secret>"}` | |
| | | |

Als het provider token is gevalideerd, wordt de API geretourneerd met een `authenticationToken` in de antwoord tekst, wat uw sessie token is. 

```json
{
    "authenticationToken": "...",
    "user": {
        "userId": "sid:..."
    }
}
```

Zodra u dit sessie token hebt, kunt u toegang krijgen tot beveiligde app-bronnen `X-ZUMO-AUTH` door de header toe te voegen aan uw HTTP-aanvragen. Bijvoorbeeld: 

```
GET https://<appname>.azurewebsites.net/api/products/1
X-ZUMO-AUTH: <authenticationToken_value>
```

## <a name="sign-out-of-a-session"></a>Afmelden bij een sessie

Gebruikers kunnen een afmelding initiëren door een `GET` aanvraag naar het eind punt van `/.auth/logout` de app te verzenden. De `GET` aanvraag doet het volgende:

- Hiermee wist u de verificatie cookies van de huidige sessie.
- Hiermee verwijdert u de tokens van de huidige gebruiker uit de token opslag.
- Voor Azure Active Directory en Google voert een afmelding aan de server zijde uit voor de ID-provider.

Dit is een voorbeeld van een eenvoudige afmeldingskoppeling op een webpagina:

```HTML
<a href="/.auth/logout">Sign out</a>
```

Standaard wordt de client door een geslaagde aanmelding omgeleid naar de URL `/.auth/logout/done`. U kunt de omleidings pagina voor het afmelden van berichten wijzigen `post_logout_redirect_uri` door de query parameter toe te voegen. Bijvoorbeeld:

```
GET /.auth/logout?post_logout_redirect_uri=/index.html
```

Het is raadzaam om de [encode](https://wikipedia.org/wiki/Percent-encoding) waarde van `post_logout_redirect_uri`te coderen.

Wanneer u Fully Qualified Url's gebruikt, moet de URL ofwel worden gehost in hetzelfde domein of worden geconfigureerd als een toegestane externe omleidings-URL voor uw app. In het volgende voor beeld wordt een omleiding naar `https://myexternalurl.com` die niet gehost in hetzelfde domein:

```
GET /.auth/logout?post_logout_redirect_uri=https%3A%2F%2Fmyexternalurl.com
```

Voer de volgende opdracht uit in de [Azure Cloud shell](../cloud-shell/quickstart.md):

```azurecli-interactive
az webapp auth update --name <app_name> --resource-group <group_name> --allowed-external-redirect-urls "https://myexternalurl.com"
```

## <a name="preserve-url-fragments"></a>URL-fragmenten behouden

Wanneer gebruikers zich aanmelden bij uw app, willen ze meestal worden omgeleid naar hetzelfde gedeelte van dezelfde pagina, zoals `/wiki/Main_Page#SectionZ`. Omdat URL- [fragmenten](https://wikipedia.org/wiki/Fragment_identifier) `#SectionZ`echter nooit naar de server worden verzonden, worden ze niet standaard bewaard nadat de OAuth-aanmelding is voltooid en weer omgeleid naar uw app. Gebruikers krijgen dan een optimale ervaring wanneer ze opnieuw naar het gewenste anker moeten navigeren. Deze beperking is van toepassing op alle verificatie oplossingen aan de server zijde.

Bij App Service-verificatie kunt u URL-fragmenten behouden over de OAuth-aanmelding. Als u dit wilt doen, stelt `WEBSITE_AUTH_PRESERVE_URL_FRAGMENT` u een app `true`-instelling in op. U kunt dit doen in de [Azure Portal](https://portal.azure.com)of gewoon de volgende opdracht uitvoeren in de [Azure Cloud shell](../cloud-shell/quickstart.md):

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group <group_name> --settings WEBSITE_AUTH_PRESERVE_URL_FRAGMENT="true"
```

## <a name="access-user-claims"></a>Gebruikers claims openen

App Service gebruikers claims door gegeven aan uw toepassing met behulp van speciale headers. Externe aanvragen zijn niet gemachtigd om deze headers in te stellen, zodat ze alleen aanwezig zijn als deze zijn ingesteld door App Service. Enkele voor beelden van headers zijn:

* X-MS-CLIENT-PRINCIPAL-NAAM
* X-MS-CLIENT-PRINCIPAL-ID

Code die is geschreven in een wille keurige taal of elk Framework, kan de informatie die nodig is van deze headers ophalen. Voor ASP.NET 4,6-apps wordt de **claimsprincipal is** automatisch ingesteld met de juiste waarden. ASP.NET Core biedt echter geen middleware voor authenticatie die kan worden geïntegreerd met App Service gebruikers claims. Zie [MaximeRouiller. Azure. AppService. EasyAuth](https://github.com/MaximRouiller/MaximeRouiller.Azure.AppService.EasyAuth)voor een tijdelijke oplossing.

Uw toepassing kan ook aanvullende details over de geverifieerde gebruiker verkrijgen door `/.auth/me`aan te roepen. De Sdk's van de Mobile Apps server bieden hulp methoden om met deze gegevens te werken. Zie [de Azure Mobile apps node. js SDK gebruiken](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity)en [werken met de .net back-end server sdk voor Azure Mobile apps](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info)voor meer informatie.

## <a name="retrieve-tokens-in-app-code"></a>Tokens ophalen in app-code

Vanuit uw server code worden de providerspecifieke tokens in de aanvraag header ingevoegd, zodat u ze eenvoudig kunt openen. De volgende tabel bevat mogelijke token header-namen:

| Provider | Header namen |
|-|-|
| Azure Active Directory | `X-MS-TOKEN-AAD-ID-TOKEN` <br/> `X-MS-TOKEN-AAD-ACCESS-TOKEN` <br/> `X-MS-TOKEN-AAD-EXPIRES-ON`  <br/> `X-MS-TOKEN-AAD-REFRESH-TOKEN` |
| Facebook-token | `X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN` <br/> `X-MS-TOKEN-FACEBOOK-EXPIRES-ON` |
| Google | `X-MS-TOKEN-GOOGLE-ID-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-ACCESS-TOKEN` <br/> `X-MS-TOKEN-GOOGLE-EXPIRES-ON` <br/> `X-MS-TOKEN-GOOGLE-REFRESH-TOKEN` |
| Microsoft-account | `X-MS-TOKEN-MICROSOFTACCOUNT-ACCESS-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-EXPIRES-ON` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-AUTHENTICATION-TOKEN` <br/> `X-MS-TOKEN-MICROSOFTACCOUNT-REFRESH-TOKEN` |
| Twitter | `X-MS-TOKEN-TWITTER-ACCESS-TOKEN` <br/> `X-MS-TOKEN-TWITTER-ACCESS-TOKEN-SECRET` |
|||

Verzend vanuit uw client code (zoals een mobiele app of een Java script in de browser) een HTTP `GET` -aanvraag `/.auth/me`naar. De geretourneerde JSON heeft de providerspecifieke tokens.

> [!NOTE]
> Toegangs tokens zijn voor toegang tot bronnen van de provider, zodat ze alleen aanwezig zijn als u uw provider configureert met een client geheim. Zie toegangs tokens vernieuwen voor meer informatie over het verkrijgen van vernieuwings tokens.

## <a name="refresh-identity-provider-tokens"></a>ID-provider tokens vernieuwen

Wanneer het toegangs token van uw provider (niet het [sessie token](#extend-session-token-expiration-grace-period)) verloopt, moet u de gebruiker opnieuw verifiëren voordat u het token opnieuw gebruikt. U kunt de verval datum van tokens `GET` voor komen door `/.auth/refresh` een aanroep naar het eind punt van uw toepassing te maken. Wanneer wordt aangeroepen App Service de toegangs tokens in de token opslag voor de geverifieerde gebruiker automatisch vernieuwd. Bij volgende aanvragen voor tokens door uw app-code worden de vernieuwde tokens opgehaald. Het vernieuwen van tokens werkt echter alleen als de token opslag [vernieuwings tokens](https://auth0.com/learn/refresh-tokens/) voor uw provider bevat. De manier om vernieuwings tokens op te halen, wordt door elke provider gedocumenteerd, maar de volgende lijst is een korte samen vatting:

- **Google**: een `access_type=offline` query reeks parameter toevoegen aan de `/.auth/login/google` API-aanroep. Als u de Mobile Apps SDK gebruikt, kunt u de para meter toevoegen aan een `LogicAsync` van de Overloads (Zie [tokens voor Google-vernieuwing](https://developers.google.com/identity/protocols/OpenIDConnect#refresh-tokens)).
- **Facebook**: biedt geen vernieuwings tokens. Tokens met een lange levens duur verlopen over 60 dagen (Zie het [verloop van Facebook en de uitbrei ding van toegangs tokens](https://developers.facebook.com/docs/facebook-login/access-tokens/expiration-and-extension)).
- **Twitter**: toegangs tokens verlopen niet (Zie [Veelgestelde vragen over Twitter OAuth](https://developer.twitter.com/en/docs/basics/authentication/FAQ)).
- **Micro soft-account**: Selecteer het bereik bij het `wl.offline_access` [configureren van verificatie-instellingen voor micro soft-accounts](configure-authentication-provider-microsoft.md).
- **Azure Active Directory**: in [https://resources.azure.com](https://resources.azure.com)voert u de volgende stappen uit:
    1. Selecteer boven aan de pagina **lezen/schrijven**.
    2. Navigeer in de linkernavigatiebalk naar **abonnementen** > **_\<abonnements\_naam_****resourceGroups** > **_\<\_resourceGroups resource\_groep naam>_** **providers** >  > **authsettings****sites** >   >  **config****Microsoft.Web** > soft. web**_sites\<app\_name>config authsettings. _**  >  >  
    3. Klik op **Bewerken**.
    4. Wijzig de volgende eigenschap. Vervang _ \<de\_app-id>_ door de Azure Active Directory-toepassings-id van de service die u wilt openen.

        ```json
        "additionalLoginParams": ["response_type=code id_token", "resource=<app_id>"]
        ```

    5. Klik op **put**. 

Zodra uw provider is geconfigureerd, kunt u [het vernieuwings token en de verloop tijd voor het toegangs token vinden](#retrieve-tokens-in-app-code) in de token opslag. 

Als u uw toegangs token op elk gewenst moment wilt vernieuwen `/.auth/refresh` , kunt u gewoon een wille keurige taal aanroepen. Het volgende code fragment gebruikt jQuery om uw toegangs tokens van een Java script-client te vernieuwen.

```JavaScript
function refreshTokens() {
  let refreshUrl = "/.auth/refresh";
  $.ajax(refreshUrl) .done(function() {
    console.log("Token refresh completed successfully.");
  }) .fail(function() {
    console.log("Token refresh failed. See application logs for details.");
  });
}
```

Als een gebruiker de machtigingen intrekt die aan uw app zijn verleend, `/.auth/me` kan de aanroep mislukken `403 Forbidden` met een antwoord. Raadpleeg uw toepassings logboeken voor meer informatie over het vaststellen van fouten.

## <a name="extend-session-token-expiration-grace-period"></a>Respijt periode van verloop sessie token uitbreiden

De geverifieerde sessie verloopt na 8 uur. Nadat een geverifieerde sessie is verlopen, is er standaard een respijt periode van 72 uur. Binnen deze respijt periode mag u het sessie token vernieuwen met App Service zonder de gebruiker opnieuw te verifiëren. U kunt alleen bellen `/.auth/refresh` wanneer uw sessie token ongeldig wordt en u hoeft niet zelf het token verloop op te volgen. Nadat de respijt periode van 72 uur is vervallen, moet de gebruiker zich opnieuw aanmelden om een geldig sessie token te verkrijgen.

Als 72 uur niet genoeg tijd voor u is, kunt u dit verloop venster uitbreiden. Het verlengen van de verloop tijd voor een lange periode kan aanzienlijke gevolgen hebben voor de beveiliging (zoals wanneer een verificatie token wordt gelekt of gestolen). Daarom moet u de standaard waarde van 72 uur laten staan of de extensie periode instellen op de kleinste.

Als u het standaard vervaldatum venster wilt uitbreiden, voert u de volgende opdracht uit in de [Cloud shell](../cloud-shell/overview.md).

```azurecli-interactive
az webapp auth update --resource-group <group_name> --name <app_name> --token-refresh-extension-hours <hours>
```

> [!NOTE]
> De respijt periode geldt alleen voor de App Service geverifieerde sessie, niet de tokens van de id-providers. Er is geen respijt periode voor de verlopen provider tokens. 
>

## <a name="limit-the-domain-of-sign-in-accounts"></a>Het domein van aanmeldings accounts beperken

Met zowel het micro soft-account als de Azure Active Directory kunt u zich vanuit meerdere domeinen aanmelden. Micro soft-account staat bijvoorbeeld _Outlook.com_-, _live.com_-en _Hotmail.com_ -accounts toe. Azure AD biedt een wille keurig aantal aangepaste domeinen voor de aanmeldings accounts. U kunt uw gebruikers echter ook rechtstreeks naar uw eigen Azure AD-aanmeldings pagina (zoals `contoso.com`) versnellen. Voer de volgende stappen uit om de domein naam van de aanmeldings accounts voor te stellen.

Ga [https://resources.azure.com](https://resources.azure.com)in naar **abonnementen** > **_\<abonnements\_naam_** >  > **authsettings** **providers**  >  **config****resourceGroups**  >  > **sites****_resourceGroups\<resource\_groep\_naam>_** providers**micro soft. Web**sites > **_app\_>config authsettings.\< _** >  >  

Klik op **bewerken**, wijzig de volgende eigenschap en klik vervolgens op **put**. Vervang _ \<de\_domein naam>_ door het domein dat u wilt.

```json
"additionalLoginParams": ["domain_hint=<domain_name>"]
```

Met deze instelling wordt de `domain_hint` query teken reeks parameter toegevoegd aan de omleidings-URL voor aanmelding. 

> [!IMPORTANT]
> Het is mogelijk dat de client de `domain_hint` para meter verwijdert nadat de omleidings-URL is ontvangen en u zich vervolgens aanmeldt met een ander domein. Hoewel deze functie handig is, is het geen beveiligings functie.
>

## <a name="authorize-or-deny-users"></a>Gebruikers autoriseren of weigeren

Hoewel App Service de eenvoudigste autorisatie aanvraag verkrijgt (dat wil zeggen niet-geverifieerde aanvragen afwijzen), is voor uw app mogelijk meer verfijnde autorisatie gedrag vereist, zoals het beperken van de toegang tot een specifieke groep gebruikers. In bepaalde gevallen moet u aangepaste toepassings code schrijven om toegang tot de aangemelde gebruiker toe te staan of te weigeren. In andere gevallen kunnen App Service of uw ID-provider u helpen zonder code wijzigingen te hoeven vereisen.

- [Server niveau](#server-level-windows-apps-only)
- [Niveau van ID-provider](#identity-provider-level)
- [Toepassings niveau](#application-level)

### <a name="server-level-windows-apps-only"></a>Server niveau (alleen Windows-apps)

Voor elke Windows-app kunt u autorisatie gedrag van de IIS-webserver definiëren door het bestand *Web. config* te bewerken. Linux-apps gebruiken geen IIS en kunnen niet worden geconfigureerd via *Web. config*.

1. Ga naar`https://<app-name>.scm.azurewebsites.net/DebugConsole`

1. Ga in de browser Verkenner van uw App Service-bestanden naar *site/wwwroot*. Als er geen *Web. config* bestaat, maakt u het door **+**  >  **nieuw bestand**te selecteren. 

1. Selecteer het potlood voor *Web. config* om het te bewerken. Voeg de volgende configuratie code toe en klik op **Opslaan**. Als *Web. config* al bestaat, kunt u het `<authorization>` element gewoon toevoegen met alles. Voeg de accounts toe die u wilt toestaan in `<allow>` het-element.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
       <system.web>
          <authorization>
            <allow users="user1@contoso.com,user2@contoso.com"/>
            <deny users="*"/>
          </authorization>
       </system.web>
    </configuration>
    ```

### <a name="identity-provider-level"></a>Niveau van ID-provider

De ID-provider kan bepaalde machtigingen voor een schakel sleutel geven. Bijvoorbeeld:

- Voor [Azure app service](configure-authentication-provider-aad.md)kunt u [toegang op ondernemings niveau](../active-directory/manage-apps/what-is-access-management.md) rechtstreeks in azure AD beheren. Zie [de toegang van een gebruiker tot een toepassing verwijderen](../active-directory/manage-apps/methods-for-removing-user-access.md)voor instructies.
- Google API-projecten die deel uitmaken van een [organisatie](https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy#organizations) kunnen worden geconfigureerd om alleen toegang te verlenen aan gebruikers in uw organisatie (Zie [de pagina **OAuth 2,0** -ondersteuning instellen](https://support.google.com/cloud/answer/6158849?hl=en)) voor [Google](configure-authentication-provider-google.md).

### <a name="application-level"></a>Toepassings niveau

Als een van de andere niveaus niet de autorisatie biedt die u nodig hebt, of als uw platform of ID-provider niet wordt ondersteund, moet u aangepaste code schrijven om gebruikers te autoriseren op basis van de [gebruikers claims](#access-user-claims).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelf studie: gebruikers een end-to-end-zelf studie (Windows)](app-service-web-tutorial-auth-aad.md)
> verifiëren en autoriseren[: gebruikers verifiëren en autoriseren-end-to-end (Linux)](containers/tutorial-auth-aad.md)
