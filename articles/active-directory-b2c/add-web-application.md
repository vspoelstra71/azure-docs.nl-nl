---
title: Een web-API-toepassing toevoegen-Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over het toevoegen van een web-API-toepassing aan uw Active Directory B2C-Tenant.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 04/16/2019
ms.custom: mvc
ms.topic: conceptual
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: e6dbf3d6fd5a43ab2d075c193c5bc589dc3566a0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "78190174"
---
# <a name="add-a-web-api-application-to-your-azure-active-directory-b2c-tenant"></a>Een web-API-toepassing toevoegen aan uw Azure Active Directory B2C-Tenant

 Registreer Web-API-resources in uw Tenant, zodat ze aanvragen kunnen accepteren en hierop reageren door client toepassingen die een toegangs token aanbieden. In dit artikel wordt beschreven hoe u een web-API registreert in Azure Active Directory B2C (Azure AD B2C).

Als u een toepassing in uw Azure AD B2C-Tenant wilt registreren, kunt u de huidige **toepassingen** gebruiken of onze nieuwe **Preview-ervaring (Unified app-registraties)** . [Meer informatie over de nieuwe ervaring](https://aka.ms/b2cappregintro).

#### <a name="applications"></a>[Toepassingen](#tab/applications/)

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
2. Zorg ervoor dat u de map gebruikt die uw Azure AD B2C-Tenant bevat. Selecteer het filter **Directory + abonnement** in het bovenste menu en kies de map die uw Tenant bevat.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **toepassingen**en selecteer vervolgens **toevoegen**.
5. Voer een naam in voor de toepassing. Bijvoorbeeld *webapi1*.
6. Selecteer voor **Inclusief webtoepassing/ web-API** en **Impliciete stroom toestaan****Ja**.
7. Voer voor de **Antwoord-URL** een eindpunt in waarop Azure AD B2C tokens retourneert die door uw toepassing worden aangevraagd. In uw productie toepassing kunt u de antwoord-URL instellen op een waarde zoals `https://localhost:44332`. Voor test doeleinden, stelt u de antwoord- `https://jwt.ms`URL in op.
8. Voer voor **App-id-URI** de id in die wordt gebruikt voor uw web-API. De volledige id-URI, inclusief het domein, wordt voor u gegenereerd. Bijvoorbeeld `https://contosotenant.onmicrosoft.com/api`.
9. Klik op **maken**.
10. Noteer op de eigenschappenpagina de toepassings-id die u gebruikt wanneer u de web-app configureert.

#### <a name="app-registrations-preview"></a>[App-registraties (preview-versie)](#tab/app-reg-preview/)

1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
1. Selecteer het filter **Directory + abonnement** in het bovenste menu en selecteer vervolgens de map die uw Azure AD B2C Tenant bevat.
1. Selecteer in het linkermenu **Azure AD B2C**. U kunt ook **alle services** selecteren en **Azure AD B2C**zoeken en selecteren.
1. Selecteer **app-registraties (preview)** en selecteer vervolgens **nieuwe registratie**.
1. Voer een **naam** in voor de toepassing. Bijvoorbeeld *webapi1*.
1. Onder **omleidings-URI**selecteert u **Web**en voert u vervolgens een eind punt in, waar Azure AD B2C tokens moet retour neren die door uw toepassing worden aangevraagd. In een productie toepassing kunt u de omleidings-URI instellen als `https://localhost:5000`een eind punt, bijvoorbeeld. Tijdens het ontwikkelen of testen kunt u het instellen op `https://jwt.ms`, een webtoepassing van micro soft die de gedecodeerde inhoud van een token weergeeft (de inhoud van het token verlaat uw browser nooit). U kunt op elk gewenst moment omleidings-Uri's toevoegen en wijzigen in uw geregistreerde toepassingen.
1. Selecteer **Registreren**.
1. Noteer de **id van de toepassing (client)** voor gebruik in de code van uw web-API.

Als u een toepassing hebt die de impliciete toekennings stroom implementeert, bijvoorbeeld een toepassing met één pagina op basis van Java script, kunt u de stroom inschakelen door de volgende stappen uit te voeren:

1. Selecteer onder **beheren**de optie **verificatie**.
1. Selecteer **de nieuwe ervaring uitproberen** (indien weer gegeven).
1. Schakel onder **impliciete toekenning**de selectie vakjes **toegangs tokens** en **id-tokens** in.
1. Selecteer **Opslaan**.

* * *

## <a name="configure-scopes"></a>Bereiken configureren

Bereiken bieden een manier om toegang tot beveiligde resources te reguleren. Bereiken worden door de web-API gebruikt om toegangsbeheer op basis van een bereik te implementeren. Bijvoorbeeld: gebruikers van de web-API kunnen zowel lees- als schrijftoegang hebben of alleen leestoegang. In deze zelfstudie gebruikt u bereiken om lees- en schrijfmachtigingen voor de web-API te definiëren.

[!INCLUDE [active-directory-b2c-scopes](../../includes/active-directory-b2c-scopes.md)]

## <a name="grant-permissions"></a>Machtigingen verlenen

Als u een beveiligde web-API wilt aanroepen vanuit een app, moet u uw toepassing machtigingen geven voor de API. In de [zelf studie: een toepassing registreren in azure Active Directory B2C](tutorial-register-applications.md), een webtoepassing met de naam *webapp1* is geregistreerd in azure AD B2C. U kunt deze toepassing gebruiken om de Web-API aan te roepen.

[!INCLUDE [active-directory-b2c-permissions-api](../../includes/active-directory-b2c-permissions-api.md)]

Uw toepassing is geregistreerd voor het aanroepen van de beveiligde web-API. Een gebruiker voert een verificatie uit bij Azure AD B2C om de toepassing te kunnen gebruiken. De toepassing verkrijgt een autorisatietoekenning van Azure AD B2C voor toegang tot de beveiligde web-API.
