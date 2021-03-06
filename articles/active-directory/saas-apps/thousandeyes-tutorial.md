---
title: 'Zelf studie: Azure Active Directory de integratie van eenmalige aanmelding (SSO) met ThousandEyes | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en ThousandEyes.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/04/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: cd179049f88454c62244cf1819cee08ef78d0633
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "72373240"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-thousandeyes"></a>Zelf studie: Azure Active Directory de integratie van eenmalige aanmelding (SSO) met ThousandEyes

In deze zelf studie leert u hoe u ThousandEyes integreert met Azure Active Directory (Azure AD). Wanneer u ThousandEyes integreert met Azure AD, kunt u het volgende doen:

* Controle in azure AD die toegang heeft tot ThousandEyes.
* Zorg ervoor dat uw gebruikers automatisch worden aangemeld bij ThousandEyes met hun Azure AD-accounts.
* Beheer uw accounts op één centrale locatie: de Azure Portal.

Zie [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)voor meer informatie over SaaS-app-integratie met Azure AD.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende items nodig om aan de slag te gaan:

* Een Azure AD-abonnement Als u geen abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/)aanvragen.
* ThousandEyes-abonnement dat is ingeschakeld voor eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelf studie configureert en test u Azure AD SSO in een test omgeving.

* ThousandEyes ondersteunt SSO die door **SP en IDP** is geïnitieerd
* ThousandEyes ondersteunt [ **geautomatiseerde** gebruikers inrichting](https://docs.microsoft.com/azure/active-directory/saas-apps/thousandeyes-provisioning-tutorial)

> [!NOTE]
> De id van deze toepassing is een vaste teken reeks waarde zodat slechts één exemplaar in één Tenant kan worden geconfigureerd.

## <a name="adding-thousandeyes-from-the-gallery"></a>ThousandEyes toevoegen uit de galerie

Als u de integratie van ThousandEyes in azure AD wilt configureren, moet u ThousandEyes uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) met behulp van een werk-of school account of een persoonlijke Microsoft-account.
1. Selecteer de **Azure Active Directory** -service in het navigatie deel venster aan de linkerkant.
1. Ga naar **bedrijfs toepassingen** en selecteer **alle toepassingen**.
1. Selecteer **nieuwe toepassing**om een nieuwe toepassing toe te voegen.
1. Typ in de sectie **toevoegen vanuit de galerie** **ThousandEyes** in het zoekvak.
1. Selecteer **ThousandEyes** uit het paneel resultaten en voeg vervolgens de app toe. Wacht een paar seconden wanneer de app aan uw Tenant is toegevoegd.

## <a name="configure-and-test-azure-ad-single-sign-on-for-thousandeyes"></a>Eenmalige aanmelding voor Azure AD configureren en testen voor ThousandEyes

Azure AD SSO met ThousandEyes configureren en testen met behulp van een test gebruiker met de naam **B. Simon**. Voor het werken met SSO moet u een koppelings relatie tot stand brengen tussen een Azure AD-gebruiker en de bijbehorende gebruiker in ThousandEyes.

Als u Azure AD SSO wilt configureren en testen met ThousandEyes, voltooit u de volgende bouw stenen:

1. **[Configureer Azure AD SSO](#configure-azure-ad-sso)** -om uw gebruikers in staat te stellen deze functie te gebruiken.
    * **[Een Azure AD-test gebruiker maken](#create-an-azure-ad-test-user)** : u kunt eenmalige aanmelding voor Azure AD testen met B. Simon.
    * **[Wijs de Azure AD-test gebruiker](#assign-the-azure-ad-test-user)** toe, zodat B. Simon de eenmalige aanmelding van Azure AD kan gebruiken.
1. **[THOUSANDEYES SSO configureren](#configure-thousandeyes-sso)** : voor het configureren van de instellingen voor eenmalige aanmelding aan de kant van de toepassing.
    * **[Maak een ThousandEyes-test gebruiker](#create-thousandeyes-test-user)** -om een equivalent van B. Simon in ThousandEyes te hebben dat is gekoppeld aan de Azure AD-representatie van de gebruiker.
1. **[SSO testen](#test-sso)** : om te controleren of de configuratie werkt.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO-configureren

Volg deze stappen om Azure AD SSO in te scha kelen in de Azure Portal.

1. Zoek in het [Azure Portal](https://portal.azure.com/)op de pagina Toepassings integratie van **ThousandEyes** de sectie **beheren** en selecteer **eenmalige aanmelding**.
1. Selecteer op de pagina **Eén aanmeldings methode selecteren** de optie **SAML**.
1. Klik op de pagina **eenmalige aanmelding met SAML instellen** op het pictogram bewerken/pen voor **eenvoudige SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. In de sectie **basis configuratie van SAML** is de toepassing vooraf geconfigureerd en de benodigde url's zijn al vooraf ingevuld met Azure. De gebruiker moet de configuratie opslaan door te klikken op de knop **Opslaan** .

1. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    Typ een URL in het tekstvak **URL voor aanmelding** :`https://app.thousandeyes.com/login/sso`

1. Zoek op de pagina **eenmalige aanmelding met SAML instellen** , in de sectie **SAML-handtekening certificaat** , naar **certificaat (base64)** en selecteer **downloaden** om het certificaat te downloaden en op uw computer op te slaan.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

1. Op de sectie **ThousandEyes instellen** kopieert u de gewenste URL ('s) op basis van uw vereiste.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken

In deze sectie maakt u een test gebruiker in de Azure Portal met de naam B. Simon.

1. Selecteer in het linkerdeel venster van de Azure Portal **Azure Active Directory**, selecteer **gebruikers**en selecteer vervolgens **alle gebruikers**.
1. Selecteer **nieuwe gebruiker** boven aan het scherm.
1. Voer de volgende stappen uit in de eigenschappen van de **gebruiker** :
   1. Voer in het veld **Naam**`B.Simon` in.  
   1. Voer in het veld **gebruikers naam** het username@companydomain.extensionin. Bijvoorbeeld `B.Simon@contoso.com`.
   1. Schakel het selectie vakje **wacht woord weer geven** in en noteer de waarde die wordt weer gegeven in het vak **wacht woord** .
   1. Klik op **maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u B. Simon in om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen aan ThousandEyes.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst toepassingen de optie **ThousandEyes**.
1. Ga op de pagina overzicht van de app naar de sectie **beheren** en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in het dialoog venster **toewijzing toevoegen** .

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoog venster **gebruikers en groepen** **B. Simon** van de lijst gebruikers en klik vervolgens op de knop **selecteren** onder aan het scherm.
1. Als u een wille keurige rol verwacht in de SAML-bewering, selecteert u in het dialoog venster **rol selecteren** de juiste rol voor de gebruiker in de lijst en klikt u op de knop **selecteren** onder aan het scherm.
1. Klik in het dialoog venster **toewijzing toevoegen** op de knop **toewijzen** .

## <a name="configure-thousandeyes-sso"></a>ThousandEyes SSO configureren

1. Meld u in een ander browser venster aan bij de **ThousandEyes** -bedrijfs site als beheerder.

2. Klik in het menu aan de bovenkant op **Settings**.

    ![Instellingen](./media/thousandeyes-tutorial/ic790066.png "Instellingen")

3. Klik op **account**

    ![Account](./media/thousandeyes-tutorial/ic790067.png "Account")

4. Klik op het tabblad **beveiliging & verificatie** .

    ![Verificatie van beveiligings &](./media/thousandeyes-tutorial/ic790068.png "Verificatie van beveiligings &")

5. Voer de volgende stappen uit in de sectie **eenmalige aanmelding instellen** :

    ![Eenmalige aanmelding instellen](./media/thousandeyes-tutorial/ic790069.png "Eenmalige aanmelding instellen")

    a. Selecteer **eenmalige aanmelding inschakelen**.

    b. Plak in het tekstvak **URL van aanmeldings pagina** de **aanmeldings-URL**die u hebt gekopieerd uit Azure Portal.

    c. Plak in het tekstvak **URL voor afmelding** de **afmeldings-URL**die u van Azure Portal hebt gekopieerd.

    d. Tekstvak voor de uitgever van de **identiteits provider** , plak de **Azure ad-id**, die u van Azure Portal hebt gekopieerd.

    e. Klik in **verificatie certificaat**op **bestand kiezen**en upload het certificaat dat u hebt gedownload van Azure Portal.

    f. Klik op **Opslaan**.

### <a name="create-thousandeyes-test-user"></a>ThousandEyes-test gebruiker maken

Het doel van deze sectie is het maken van een gebruiker met de naam Julia Simon in ThousandEyes. ThousandEyes ondersteunt het automatisch inrichten van gebruikers, dat standaard is ingeschakeld. U kunt [hier](thousandeyes-provisioning-tutorial.md) meer informatie vinden over het configureren van het automatisch inrichten van gebruikers.

**Als u de gebruiker handmatig moet maken, voert u de volgende stappen uit:**

1. Meld u als beheerder aan bij de ThousandEyes-bedrijfs site.

2. Klik op **Instellingen**.

    ![Instellingen](./media/thousandeyes-tutorial/IC790066.png "Instellingen")

3. Klik op **Account**.

    ![Account](./media/thousandeyes-tutorial/IC790067.png "Account")

4. Klik op het tabblad **Accounts & gebruikers** .

    ![Accounts & gebruikers](./media/thousandeyes-tutorial/IC790073.png "Accounts & gebruikers")

5. Voer in de sectie **gebruikers & accounts toevoegen** de volgende stappen uit:

    ![Gebruikers accounts toevoegen](./media/thousandeyes-tutorial/IC790074.png "Gebruikers accounts toevoegen")

    a. Typ in het tekstvak **naam** de naam van de gebruiker zoals **B. Simon**.

    b. Typ in het tekstvak **e-mail** het e-mail adres b.simon@contoso.comvan de gebruiker zoals.

    b. Klik op **nieuwe gebruiker toevoegen aan account**.

    > [!NOTE]
    > De houder van het Azure Active Directory-account krijgt een e-mail bericht met een koppeling om het account te bevestigen en te activeren.

> [!NOTE]
> U kunt alle andere hulpprogram ma's voor het maken van ThousandEyes-gebruikers accounts of Api's die worden geleverd door ThousandEyes, gebruiken om Azure Active Directory gebruikers accounts in te richten.


## <a name="test-sso"></a>SSO testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel ThousandEyes in het toegangs venster klikt, moet u automatisch worden aangemeld bij de ThousandEyes waarvoor u SSO hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende bronnen

- [Lijst met zelf studies voor het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Probeer ThousandEyes met Azure AD](https://aad.portal.azure.com/)

- [Gebruikers inrichten configureren](https://docs.microsoft.com/azure/active-directory/saas-apps/thousandeyes-provisioning-tutorial)