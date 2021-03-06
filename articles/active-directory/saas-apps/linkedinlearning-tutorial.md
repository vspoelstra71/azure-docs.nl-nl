---
title: 'Zelf studie: Azure Active Directory-integratie met eenmalige aanmelding (SSO) met LinkedIn Learning | Microsoft Docs'
description: Ontdek hoe u eenmalige aanmelding configureert tussen Azure Active Directory en LinkedIn Learning.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d5857070-bf79-4bd3-9a2a-4c1919a74946
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 15509866980cdf85f54fc03cb77eed36f83c982f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "76983364"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-linkedin-learning"></a>Zelf studie: Azure Active Directory-integratie met eenmalige aanmelding (SSO) met LinkedIn Learning

In deze zelf studie leert u hoe u LinkedIn Learning kunt integreren met Azure Active Directory (Azure AD). Wanneer u LinkedIn Learning integreert met Azure AD, kunt u het volgende doen:

* Controle in azure AD die toegang heeft tot LinkedIn learning.
* Uw gebruikers in staat stellen om automatisch te worden aangemeld bij LinkedIn learning met hun Azure AD-accounts.
* Beheer uw accounts op één centrale locatie: de Azure Portal.

Zie [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)voor meer informatie over SaaS-app-integratie met Azure AD.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende items nodig om aan de slag te gaan:

* Een Azure AD-abonnement Als u geen abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/)aanvragen.
* Abonnement op eenmalige aanmelding voor LinkedIn Learning (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelf studie configureert en test u Azure AD SSO in een test omgeving.

* LinkedIn Learning biedt ondersteuning voor door **SP en IDP** geïnitieerde eenmalige aanmelding
* LinkedIn Learning biedt ondersteuning voor **Just In Time**-gebruikersinrichting
* Zodra u LinkedIn Learning hebt geconfigureerd, kunt u sessie beheer afdwingen, waardoor exfiltration en infiltratie van de gevoelige gegevens van uw organisatie in realtime worden beschermd. Sessie beheer is uitgebreid met voorwaardelijke toegang. [Meer informatie over het afdwingen van sessie beheer met Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-linkedin-learning-from-the-gallery"></a>LinkedIn Learning vanuit de galerie toevoegen

Als u de integratie van LinkedIn Learning met Azure AD wilt configureren, moet u LinkedIn Learning vanuit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) met behulp van een werk-of school account of een persoonlijke Microsoft-account.
1. Selecteer de **Azure Active Directory** -service in het navigatie deel venster aan de linkerkant.
1. Ga naar **bedrijfs toepassingen** en selecteer **alle toepassingen**.
1. Selecteer **nieuwe toepassing**om een nieuwe toepassing toe te voegen.
1. In de sectie **toevoegen vanuit de galerie** typt u **LinkedIn Learning** in het zoekvak.
1. Selecteer **LinkedIn Learning** in het paneel resultaten en voeg vervolgens de app toe. Wacht een paar seconden wanneer de app aan uw Tenant is toegevoegd.


## <a name="configure-and-test-azure-ad-single-sign-on-for-linkedin-learning"></a>Eenmalige aanmelding van Azure AD voor LinkedIn Learning configureren en testen

Azure AD SSO met LinkedIn Learning configureren en testen met behulp van een test gebruiker met de naam **B. Simon**. Voor het werken met SSO moet u een koppelings relatie tot stand brengen tussen een Azure AD-gebruiker en de bijbehorende gebruiker in LinkedIn learning.

Als u Azure AD SSO wilt configureren en testen met LinkedIn Learning, voltooit u de volgende bouw stenen:

1. **[Configureer Azure AD SSO](#configure-azure-ad-sso)** -om uw gebruikers in staat te stellen deze functie te gebruiken.
    1. **[Een Azure AD-test gebruiker maken](#create-an-azure-ad-test-user)** : u kunt eenmalige aanmelding voor Azure AD testen met B. Simon.
    1. **[Wijs de Azure AD-test gebruiker](#assign-the-azure-ad-test-user)** toe, zodat B. Simon de eenmalige aanmelding van Azure AD kan gebruiken.
1. **[LinkedIn Learning SSO configureren](#configure-linkedin-learning-sso)** : voor het configureren van de instellingen voor eenmalige aanmelding aan de kant van de toepassing.
    1. **[Maak een LinkedIn Learning test-gebruiker](#create-linkedin-learning-test-user)** -om een soort tegen te brengen van B. Simon in LinkedIn learning dat is gekoppeld aan de Azure AD-representatie van de gebruiker.
1. **[SSO testen](#test-sso)** : om te controleren of de configuratie werkt.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO-configureren

Volg deze stappen om Azure AD SSO in te scha kelen in de Azure Portal.

1. Zoek in het [Azure Portal](https://portal.azure.com/)op de pagina **LinkedIn Learning** Application Integration de sectie **Manage** en selecteer **eenmalige aanmelding**.
1. Selecteer op de pagina **Eén aanmeldings methode selecteren** de optie **SAML**.
1. Klik op de pagina **eenmalige aanmelding met SAML instellen** op het pictogram bewerken/pen voor **eenvoudige SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Als u de toepassing in de gestarte modus **IDP** wilt configureren, voert u in de sectie **basis configuratie van SAML** de waarden voor de volgende velden in:

     a. Voer in het tekstvak **id** de **Entiteits-id** in die u hebt gekopieerd uit LinkedIn Portal. 

    b. Voer in het tekstvak **Antwoord-URL** de **ACS-URL (Assertion Consumer Service)** in die u uit LinkedIn Portal hebt gekopieerd.

    c. Als u de toepassing in de **SP geïnitieerde** modus wilt configureren, klikt u in het gedeelte **Standaard SAML-configuratie** waarin u de aanmeldings-URL opgeeft op **Extra URL's instellen**. Kopieer de **ACS-URL (Assertion Consumer Service)** en vervang /saml/ door /login/ om een kopie van de aanmeldings-URL te maken. Zodra dit is gebeurd, zou de aanmeldings-URL het volgende patroon moeten hebben:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![Informatie over eenmalige aanmelding voor LinkedIn Learning-domein en -URL's](common/metadata-upload-additional-signon.png)

    > [!NOTE]
    > Deze waarden zijn geen werkelijke waarden. U kunt deze waarden bijwerken met de daad werkelijke id en de antwoord-URL, die later wordt beschreven in de sectie **LinkedIn Learning SSO configureren** van de zelf studie.

1. LinkedIn-trainings toepassing verwacht de SAML-beweringen in een specifieke indeling, waarvoor u aangepaste kenmerk toewijzingen moet toevoegen aan uw configuratie van uw SAML-token kenmerken. In de volgende schermafbeelding ziet u de lijst met standaardkenmerken, waarbij **nameidentifier** is toegewezen aan **user.userprincipalname**. In de toepassing LinkedIn Learning wordt verwacht dat **nameidentifier** is toegewezen met **user.mail**. Daarom moet u de toewijzing van het kenmerk bewerken door op het pictogram **Bewerken** te klikken en de toewijzing van het kenmerk te wijzigen.

    ![installatiekopie](common/edit-attribute.png)

1. Zoek op de pagina **eenmalige aanmelding met SAML instellen** , in de sectie **SAML-handtekening certificaat** , de **federatieve meta gegevens-XML** en selecteer **downloaden** om het certificaat te downloaden en op uw computer op te slaan.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

1. Op de sectie **LinkedIn Learning instellen** kopieert u de gewenste URL ('s) op basis van uw vereiste.

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

In deze sectie schakelt u B. Simon in om de eenmalige aanmelding van Azure te gebruiken door toegang te verlenen aan LinkedIn learning.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**en selecteer vervolgens **alle toepassingen**.
1. Selecteer **LinkedIn Learning**in de lijst toepassingen.
1. Ga op de pagina overzicht van de app naar de sectie **beheren** en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in het dialoog venster **toewijzing toevoegen** .

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoog venster **gebruikers en groepen** **B. Simon** van de lijst gebruikers en klik vervolgens op de knop **selecteren** onder aan het scherm.
1. Als u een wille keurige rol verwacht in de SAML-bewering, selecteert u in het dialoog venster **rol selecteren** de juiste rol voor de gebruiker in de lijst en klikt u op de knop **selecteren** onder aan het scherm.
1. Klik in het dialoog venster **toewijzing toevoegen** op de knop **toewijzen** .

## <a name="configure-linkedin-learning-sso"></a>LinkedIn Learning SSO configureren

1. Meld u in een ander browservenster als beheerder aan bij uw LinkedIn Learning-tenant.

2. Klik in **Accountcentrum** onder **Instellingen** op **Algemene instellingen**. Selecteer ook **Learning - standaard** uit de vervolgkeuzelijst.

    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

3. Klik op **Of klik hier om afzonderlijke velden van het formulier te laden en kopiëren** en kopieer **Entiteits-id** en **ACS-URL (Assertion Consumer Service)** en plak deze in het gedeelte **Standaard SAML-configuratie** in Azure Portal.

    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

4. Ga naar het gedeelte **met beheerdersinstellingen voor LinkedIn**. Upload het XML-bestand dat u hebt gedownload uit Azure Portal door op de optie **XML-bestand uploaden** te klikken.

    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

5. Klik op **Aan** om SSO in te schakelen. De SSO-status verandert van **Niet verbonden** naar **Verbonden**

    ![Eenmalige aanmelding configureren](./media/linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="create-linkedin-learning-test-user"></a>Testgebruiker voor LinkedIn Learning maken

De LinkedIn Learning-toepassing biedt ondersteuning voor Just in time-gebruikersinrichting. Na verificatie worden gebruikers automatisch aangemaakt in de toepassing. Ga naar het tabblad met beheerdersinstellingen in de LinkedIn Learning-portal en zet de schakelaar voor **automatisch toewijzen van licenties** op actieve Just in time-inrichting. Hiermee wordt ook een licentie aan de gebruiker toegewezen.

   ![Een Azure AD-testgebruiker maken](./media/linkedinlearning-tutorial/LinkedinUserprovswitch.png)

## <a name="test-sso"></a>SSO testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel LinkedIn Learning in het toegangsvenster klikt, zou u automatisch moeten worden aangemeld bij de instantie van LinkedIn Learning waarvoor u eenmalige aanmelding hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende bronnen

- [Lijst met zelf studies voor het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Probeer LinkedIn learning met Azure AD](https://aad.portal.azure.com/)

- [Wat is sessie beheer in Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)