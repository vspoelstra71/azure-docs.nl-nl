---
title: App-rollen toevoegen en ophalen uit een token | Azure
titleSuffix: Microsoft identity platform
description: Meer informatie over het toevoegen van app-rollen in een toepassing die is geregistreerd in Azure Active Directory, gebruikers en groepen toewijzen aan deze rollen `roles` en ze ontvangen in de claim in het token.
services: active-directory
author: kkrishna
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: kkrishna
ms.reviewer: kkrishna, jmprieur
ms.custom: aaddev
ms.openlocfilehash: 144fad249011d547ac6a8cf2d404cb3f8fe74f96
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80884253"
---
# <a name="how-to-add-app-roles-in-your-application-and-receive-them-in-the-token"></a>Procedure: app-rollen toevoegen in uw toepassing en deze ontvangen in het token

Op rollen gebaseerd toegangs beheer (RBAC) is een populair mechanisme voor het afdwingen van autorisatie in toepassingen. Wanneer u RBAC gebruikt, verleent een beheerder machtigingen aan rollen en niet aan afzonderlijke gebruikers of groepen. De beheerder kan vervolgens rollen toewijzen aan verschillende gebruikers en groepen om te bepalen wie toegang heeft tot de inhoud en functionaliteit.

Met RBAC met toepassings rollen en claim claims kunnen ontwikkel aars veilig autorisaties in hun apps afdwingen met weinig moeite op hun eigen onderdeel.

Een andere benadering is het gebruik van Azure AD-groepen en-groepclaims, zoals wordt weer gegeven in [webapp-GroupClaims-DotNet](https://github.com/Azure-Samples/WebApp-GroupClaims-DotNet). Azure AD-groepen en toepassings rollen zijn op geen enkele manier onderling uitsluiten; ze kunnen samen worden gebruikt om nog nauw keuriger toegangs beheer te bieden.

## <a name="declare-roles-for-an-application"></a>Rollen voor een toepassing declareren

Deze toepassings rollen worden gedefinieerd in de [Azure Portal](https://portal.azure.com) in het registratie manifest van de toepassing.  Wanneer een gebruiker zich aanmeldt bij de toepassing, verzendt `roles` Azure AD een claim voor elke rol die de gebruiker afzonderlijk heeft verleend aan de gebruiker en aan het groepslid maatschap.  De toewijzing van gebruikers en groepen aan rollen kan worden uitgevoerd via de gebruikers interface van de portal of via een programma met behulp van [Microsoft Graph](https://developer.microsoft.com/graph/docs/concepts/azuread-identity-access-management-concept-overview).

### <a name="declare-app-roles-using-azure-portal"></a>App-rollen declareren met Azure Portal

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
1. Selecteer het pictogram voor het adres van de **map en het abonnement** op de werk balk van de portal.
1. Kies in de lijst **Favorieten** of **alle directory's** de Active Directory Tenant waar u de toepassing wilt registreren.
1. Zoek in het Azure Portal naar en selecteer **Azure Active Directory**.
1. Selecteer in **Azure Active Directory** het deel venster Azure Active Directory **app-registraties** om een lijst met al uw toepassingen weer te geven.
1. Selecteer de toepassing waarvoor u de app-rollen wilt definiëren. Selecteer vervolgens **manifest**.
1. Bewerk het app-manifest door de `appRoles` instelling te zoeken en alle toepassings rollen toe te voegen.

     > [!NOTE]
     > Elke app-roldefinitie in dit manifest moet een andere geldige GUID hebben in de context van het manifest voor de `id` eigenschap.
     >
     > De `value` eigenschap van elke roldefinitie van de app moet exact overeenkomen met de teken reeksen die worden gebruikt in de code in de toepassing. De `value` eigenschap mag geen spaties bevatten. Als dit het geval is, ontvangt u een fout melding wanneer u het manifest opslaat.

1. Sla het manifest op.

### <a name="examples"></a>Voorbeelden

In het volgende voor beeld `appRoles` ziet u hoe u kunt `users`toewijzen aan.

> [!NOTE]
>De `id` moet een unieke GUID zijn.

```Json
"appId": "8763f1c4-f988-489c-a51e-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "User"
      ],
      "displayName": "Writer",
      "id": "d1c2ade8-98f8-45fd-aa4a-6d06b947c66f",
      "isEnabled": true,
      "description": "Writers Have the ability to create tasks.",
      "value": "Writer"
    }
  ],
"availableToOtherTenants": false,
```

> [!NOTE]
>De `displayName` mag geen spaties bevatten.

U kunt app-rollen definiëren voor `users`het `applications`doel, of beide. Indien beschikbaar voor `applications`, worden app-rollen weer gegeven als toepassings machtigingen op de Blade **vereiste machtigingen** . In het volgende voor beeld ziet u een app- `Application`rol die is gericht op een.

```Json
"appId": "8763f1c4-f988-489c-a51e-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "Application"
      ],
      "displayName": "ConsumerApps",
      "id": "47fbb575-859a-4941-89c9-0f7a6c30beac",
      "isEnabled": true,
      "description": "Consumer apps have access to the consumer data.",
      "value": "Consumer"
    }
  ],
"availableToOtherTenants": false,
```

Het aantal gedefinieerde rollen heeft gevolgen voor de limieten die het toepassings manifest heeft. Deze zijn uitvoerig besproken op de pagina [manifest limieten](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest#manifest-limits) .

### <a name="assign-users-and-groups-to-roles"></a>Gebruikers en groepen toewijzen aan rollen

Wanneer u app-rollen hebt toegevoegd in uw toepassing, kunt u gebruikers en groepen toewijzen aan deze rollen.

1. Selecteer in **Azure Active Directory** het deel venster Azure Active Directory **bedrijfs toepassingen** in het navigatie menu **Azure Active Directory** links.
1. Selecteer **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

     Als u de toepassing die u wilt weer geven hier niet ziet, gebruikt u de diverse filters boven aan de lijst **alle toepassingen** om de lijst te beperken of omlaag te schuiven in de lijst om uw toepassing te vinden.

1. Selecteer de toepassing waarin u gebruikers of beveiligings groep aan rollen wilt toewijzen.
1. Selecteer het deel venster **gebruikers en groepen** in het navigatie menu aan de linkerkant van de toepassing.
1. Klik boven aan de lijst **gebruikers en groepen** op de knop **gebruiker toevoegen** om het deel venster **toewijzing toevoegen** te openen.
1. Selecteer de selector **gebruikers en groepen** in het deel venster **toewijzing toevoegen** .

     Er wordt een lijst met gebruikers en beveiligings groepen weer gegeven samen met een tekstvak om te zoeken en een bepaalde gebruiker of groep te zoeken. In dit scherm kunt u meerdere gebruikers en groepen tegelijk selecteren.

1. Wanneer u klaar bent met het selecteren van de gebruikers en groepen, klikt u op de knop **selecteren** aan de onderkant om naar het volgende deel te gaan.
1. Kies de selector **selecteren** in het deel venster **toewijzing toevoegen** . Alle eerder in het app-manifest gedeclareerde rollen worden weer gegeven.
1. Kies een rol en druk op de knop **selecteren** .
1. Druk op de knop **toewijzen** aan de onderkant om de toewijzingen van gebruikers en groepen aan de app te volt ooien.
1. Controleer of de gebruikers en groepen die u hebt toegevoegd, worden weer gegeven in de lijst met bijgewerkte **gebruikers en groepen** .

## <a name="more-information"></a>Meer informatie

- [Autorisatie toevoegen met behulp van app-rollen & rollen claims aan een ASP.NET Core-web-app](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/5-WebApp-AuthZ/5-1-Roles)
- [Beveiligings groepen en toepassings rollen gebruiken in uw apps (video)](https://www.youtube.com/watch?v=V8VUPixLSiM)
- [Azure Active Directory, nu met groeps claims en toepassings rollen](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-now-with-Group-Claims-and-Application/ba-p/243862)
- [Azure Active Directory app-manifest](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest)
- [AAD-toegangs tokens](access-tokens.md)
- [AAD`id_tokens`](id-tokens.md)
