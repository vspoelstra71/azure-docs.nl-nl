---
title: Tokens verkrijgen om een web-API (daemon-app) aan te roepen-micro soft Identity-platform | Azure
description: Meer informatie over het bouwen van een daemon-app die web-Api's aanroept (tokens ophalen)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: d755573b53eb63d85165fb73fe4b97298dbeff09
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "81868999"
---
# <a name="daemon-app-that-calls-web-apis---acquire-a-token"></a>Daemon-app die web-Api's aanroept-een Token ophalen

Nadat u een vertrouwelijke client toepassing hebt gemaakt, kunt u een token voor de app verkrijgen door het `AcquireTokenForClient`bereik aan te roepen, door te geven en eventueel een vernieuwing van het token te forceren.

## <a name="scopes-to-request"></a>Te aanvragen scopes

Het bereik dat moet worden aangevraagd voor een client referentie stroom is de naam van de resource `/.default`gevolgd door. Deze notatie vertelt Azure Active Directory (Azure AD) voor het gebruik van de *machtigingen op toepassings niveau* die statisch zijn gedeclareerd tijdens de registratie van de toepassing. Daarnaast moeten deze API-machtigingen worden verleend door een Tenant beheerder.

# <a name="net"></a>[.NET](#tab/dotnet)

```csharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

# <a name="python"></a>[Python](#tab/python)

In MSAL python ziet het configuratie bestand eruit als het volgende code fragment:

```Json
{
    "scope": ["https://graph.microsoft.com/.default"],
}
```

# <a name="java"></a>[Java](#tab/java)

```Java
final static String GRAPH_DEFAULT_SCOPE = "https://graph.microsoft.com/.default";
```

---

### <a name="azure-ad-v10-resources"></a>Azure AD (v 1.0)-resources

Het bereik dat wordt gebruikt voor client referenties moet altijd de resource-ID `/.default`zijn, gevolgd door.

> [!IMPORTANT]
> Wanneer MSAL een toegangs token aanvraagt voor een resource die een versie 1,0-toegangs token accepteert, parseert Azure AD de gewenste doel groep uit het aangevraagde bereik door alles vóór de laatste slash te nemen en deze als resource-id te gebruiken.
> Als, als Azure SQL Database (**https:\//database.Windows.net**), de resource verwacht dat een doel groep eindigt met een slash (voor Azure SQL database `https://database.windows.net/`), moet u een bereik van `https://database.windows.net//.default`aanvragen. (Let op de dubbele slash.) Zie ook MSAL.NET issue [#747: de afsluitende slash van de resource-URL wordt wegge laten, wat een SQL-verificatie fout heeft veroorzaakt](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747).

## <a name="acquiretokenforclient-api"></a>AcquireTokenForClient-API

Voor het verkrijgen van een token voor de app gebruikt `AcquireTokenForClient` u of het equivalent ervan, afhankelijk van het platform.

# <a name="net"></a>[.NET](#tab/dotnet)

```csharp
using Microsoft.Identity.Client;

// With client credentials flows, the scope is always of the shape "resource/.default" because the
// application permissions need to be set statically (in the portal or by PowerShell), and then granted by
// a tenant administrator.
string[] scopes = new string[] { "https://graph.microsoft.com/.default" };

AuthenticationResult result = null;
try
{
 result = await app.AcquireTokenForClient(scopes)
                  .ExecuteAsync();
}
catch (MsalUiRequiredException ex)
{
    // The application doesn't have sufficient permissions.
    // - Did you declare enough app permissions during app creation?
    // - Did the tenant admin grant permissions to the application?
}
catch (MsalServiceException ex) when (ex.Message.Contains("AADSTS70011"))
{
    // Invalid scope. The scope has to be in the form "https://resourceurl/.default"
    // Mitigation: Change the scope to be as expected.
}
```

# <a name="python"></a>[Python](#tab/python)

```Python
# The pattern to acquire a token looks like this.
result = None

# First, the code looks up a token from the cache.
# Because we're looking for a token for the current app, not for a user,
# use None for the account parameter.
result = app.acquire_token_silent(config["scope"], account=None)

if not result:
    logging.info("No suitable token exists in cache. Let's get a new one from AAD.")
    result = app.acquire_token_for_client(scopes=config["scope"])

if "access_token" in result:
    # Call a protected API with the access token.
    print(result["token_type"])
else:
    print(result.get("error"))
    print(result.get("error_description"))
    print(result.get("correlation_id"))  # You might need this when reporting a bug.
```

# <a name="java"></a>[Java](#tab/java)

Deze code wordt geëxtraheerd uit de [MSAL java dev](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/dev/src/samples/confidential-client/)-voor beelden.

```Java
private static IAuthenticationResult acquireToken() throws Exception {

     // Load token cache from file and initialize token cache aspect. The token cache will have
     // dummy data, so the acquireTokenSilently call will fail.
     TokenCacheAspect tokenCacheAspect = new TokenCacheAspect("sample_cache.json");

     IClientCredential credential = ClientCredentialFactory.createFromSecret(CLIENT_SECRET);
     ConfidentialClientApplication cca =
             ConfidentialClientApplication
                     .builder(CLIENT_ID, credential)
                     .authority(AUTHORITY)
                     .setTokenCacheAccessAspect(tokenCacheAspect)
                     .build();

     IAuthenticationResult result;
     try {
         SilentParameters silentParameters =
                 SilentParameters
                         .builder(SCOPE)
                         .build();

         // try to acquire token silently. This call will fail since the token cache does not
         // have a token for the application you are requesting an access token for
         result = cca.acquireTokenSilently(silentParameters).join();
     } catch (Exception ex) {
         if (ex.getCause() instanceof MsalException) {

             ClientCredentialParameters parameters =
                     ClientCredentialParameters
                             .builder(SCOPE)
                             .build();

             // Try to acquire a token. If successful, you should see
             // the token information printed out to console
             result = cca.acquireToken(parameters).join();
         } else {
             // Handle other exceptions accordingly
             throw ex;
         }
     }
     return result;
 }
```

---

### <a name="protocol"></a>Protocol

Als u nog geen bibliotheek hebt voor de taal die u hebt gekozen, kunt u het protocol het beste rechtstreeks gebruiken:

#### <a name="first-case-access-the-token-request-by-using-a-shared-secret"></a>Eerste geval: toegang tot de token aanvraag met behulp van een gedeeld geheim

```HTTP
POST /{tenant}/oauth2/v2.0/token HTTP/1.1           //Line breaks for clarity.
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865
&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_secret=qWgdYAmab0YSkuL1qKv5bPX
&grant_type=client_credentials
```

#### <a name="second-case-access-the-token-request-by-using-a-certificate"></a>Tweede geval: toegang tot de token aanvraag met behulp van een certificaat

```HTTP
POST /{tenant}/oauth2/v2.0/token HTTP/1.1               // Line breaks for clarity.
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

scope=https%3A%2F%2Fgraph.microsoft.com%2F.default
&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&grant_type=client_credentials
```

Zie de documentatie over het [micro soft-identiteits platform en de OAuth 2,0-client referenties stroom](v2-oauth2-client-creds-grant-flow.md)voor meer informatie.

## <a name="application-token-cache"></a>Toepassings token cache

In MSAL.NET `AcquireTokenForClient` wordt de toepassings token cache gebruikt. (Alle andere AcquireToken*xx* -methoden gebruiken de token cache van de gebruiker.) Roep `AcquireTokenSilent` niet aan voordat u `AcquireTokenForClient`belt, `AcquireTokenSilent` omdat de cache van de *gebruikers* token wordt gebruikt. `AcquireTokenForClient`Hiermee wordt de cache van het *toepassings* token zelf gecontroleerd en bijgewerkt.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="did-you-use-the-resourcedefault-scope"></a>Hebt u het resource/.-standaard bereik gebruikt?

Als er een fout bericht wordt weer gegeven met de melding dat u een ongeldig bereik hebt gebruikt, hebt `resource/.default` u waarschijnlijk geen gebruik gemaakt van het bereik.

### <a name="did-you-forget-to-provide-admin-consent-daemon-apps-need-it"></a>Bent u verg eten om beheerders toestemming te geven? Daemon-apps zijn vereist!

Als u een **ontoereikende bevoegdheid krijgt om de bewerkings fout te volt ooien** wanneer u de API aanroept, moet de Tenant beheerder machtigingen verlenen voor de toepassing. Zie stap 6 van de bovenstaande client-app registreren.
Normaal gesp roken ziet u een fout die er als volgt uitziet:

```json
Failed to call the web API: Forbidden
Content: {
  "error": {
    "code": "Authorization_RequestDenied",
    "message": "Insufficient privileges to complete the operation.",
    "innerError": {
      "request-id": "<guid>",
      "date": "<date>"
    }
  }
}
```

## <a name="next-steps"></a>Volgende stappen

# <a name="net"></a>[.NET](#tab/dotnet)

> [!div class="nextstepaction"]
> [Daemon-app: een web-API aanroepen](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-call-api?tabs=dotnet)

# <a name="python"></a>[Python](#tab/python)

> [!div class="nextstepaction"]
> [Daemon-app: een web-API aanroepen](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-call-api?tabs=python)

# <a name="java"></a>[Java](#tab/java)

> [!div class="nextstepaction"]
> [Daemon-app: een web-API aanroepen](https://docs.microsoft.com/azure/active-directory/develop/scenario-daemon-call-api?tabs=java)

---
