---
title: 'Zelf studie: integratie Azure Active Directory met ON24 Virtual Environment SAML-verbinding | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en de SAML-verbinding van de virtuele omgeving van ON24.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d4028fb5-b2ad-4c5d-b123-7b675c509d64
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/13/2019
ms.author: jeedes
ms.openlocfilehash: 801a631b56a11e68c444ede846ff82195cd7627f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "67095731"
---
# <a name="tutorial-azure-active-directory-integration-with-on24-virtual-environment-saml-connection"></a>Zelf studie: integratie Azure Active Directory met ON24 Virtual Environment SAML-verbinding

In deze zelf studie leert u hoe u de SAML-verbinding van ON24 virtual environment integreert met Azure Active Directory (Azure AD).
Het integreren van ON24 Virtual Environment SAML-verbinding met Azure AD biedt u de volgende voor delen:

* U kunt beheren in azure AD die toegang heeft tot de SAML-verbinding van ON24 virtual environment.
* U kunt ervoor zorgen dat uw gebruikers automatisch worden aangemeld voor ON24 virtuele omgeving SAML-verbinding (eenmalige aanmelding) met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u nog geen abonnement op Azure hebt, [Maak dan een gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voor het configureren van Azure AD-integratie met ON24 Virtual Environment SAML-verbinding hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u [hier](https://azure.microsoft.com/pricing/free-trial/) de proefversie van één maand krijgen.
* ON24-abonnement voor SAML-verbinding met eenmalige aanmelding voor de virtuele omgeving

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* De SAML-verbinding van ON24 Virtual Environment ondersteunt door **SP** en **IDP** geïnitieerde SSO

## <a name="adding-on24-virtual-environment-saml-connection-from-the-gallery"></a>De SAML-verbinding van ON24 Virtual Environment toevoegen vanuit de galerie

Als u de integratie van ON24 Virtual Environment SAML-verbinding wilt configureren in azure AD, moet u de SAML-verbinding van ON24 Virtual Environment toevoegen vanuit de galerie aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om de SAML-verbinding van ON24 Virtual Environment toe te voegen vanuit de galerie:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ in het zoekvak **ON24 virtuele omgeving SAML-verbinding**, selecteer **ON24 virtuele omgeving SAML-verbinding** uit het paneel resultaten en klik vervolgens op de knop **toevoegen** om de toepassing toe te voegen.

     ![SAML-verbinding in de lijst met resultaten van ON24 Virtual Environment](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u eenmalige aanmelding voor Azure AD configureren en testen met ON24 Virtual Environment SAML-verbinding op basis van een test gebruiker met de naam **Julia Simon**.
Als u eenmalige aanmelding wilt gebruiken, moet er een koppelings relatie tussen een Azure AD-gebruiker en de gerelateerde gebruiker in ON24 Virtual Environment SAML-verbinding tot stand worden gebracht.

Als u eenmalige aanmelding voor Azure AD wilt configureren en testen met de SAML-verbinding van ON24 Virtual Environment, moet u de volgende bouw stenen volt ooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Eenmalige aanmelding voor de ON24 Virtual Environment configureren](#configure-on24-virtual-environment-saml-connection-single-sign-on)** . de instellingen voor eenmalige aanmelding aan de kant van de toepassing worden geconfigureerd.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak een ON24 voor de SAML-verbindings test van de virtuele omgeving](#create-on24-virtual-environment-saml-connection-test-user)** , zodat u een equivalent van Julia Simon in ON24 Virtual Environment SAML-verbinding hebt die is gekoppeld aan de Azure AD-representatie van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding voor Azure AD met ON24 Virtual Environment SAML-verbinding te configureren:

1. Selecteer in de [Azure Portal](https://portal.azure.com/)op de pagina de integratie van de SAML-verbindings toepassing voor **ON24 virtuele omgeving** de optie **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. Als u de toepassing in de gestarte modus van **IDP** wilt configureren, voert u de volgende stappen uit in de sectie **basis configuratie van SAML** :

    ![Informatie over eenmalige aanmelding voor het SAML-verbindings domein en Url's van ON24 Virtual Environment](common/idp-relay.png)

    a. In het tekstvak **Id** typt u een URL: 

     **URL van productie omgeving**
    
    `SAML-VSHOW.on24.com`

    `SAML-Gateway.on24.com` 

    `SAP PROD SAML-EliteAudience.on24.com` 
                
     **URL van QA-omgeving**
    
    `SAMLQA-VSHOW.on24.com` 

    `SAMLQA-Gateway.on24.com` 

    `SAMLQA-EliteAudience.on24.com`

    b. Typ een URL in het tekstvak **Antwoord-URL**:

     **URL van productie omgeving**
    
    `https://federation.on24.com/sp/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1WU2hvdy5vbjI0LmNvbSJ9/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1HYXRld2F5Lm9uMjQuY29tIn0/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1FbGl0ZUF1ZGllbmNlLm9uMjQuY29tIn0/ACS.saml2`

     **URL van QA-omgeving**
    
    `https://qafederation.on24.com/sp/ACS.saml2`

    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLVZzaG93Lm9uMjQuY29tIn0/ACS.saml2`

    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLUdhdGV3YXkub24yNC5jb20ifQ/ACS.saml2`
     
    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLUVsaXRlQXVkaWVuY2Uub24yNC5jb20ifQ/ACS.saml2` 

    c. Klik op **Extra URL's instellen**. 

    d. In het tekstvak **Relaystatus** typt u een URL: `https://vshow.on24.com/vshow/ms_azure_saml_test?r=<ID>`

5.  Als u de toepassing in de modus door **SP** gestart wilt configureren, voert u de volgende stap uit:

    ![Informatie over eenmalige aanmelding voor het SAML-verbindings domein en Url's van ON24 Virtual Environment](common/both-signonurl.png)

    Typ in het tekstvak **URL voor aanmelding** een URL met het volgende patroon:`https://vshow.on24.com/vshow/<INSTANCENAME>`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke relay-status en aanmeldings-URL. Neem contact op met het [ondersteunings team voor SAML-ON24 van de virtuele omgeving](https://www.on24.com/contact-us/) om deze waarden op te halen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

4. Op de pagina **Eenmalige aanmelding met SAML instellen** in het gedeelte **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **XML-bestand met federatieve metagegevens** te downloaden uit de gegeven opties overeenkomstig met wat u nodig hebt, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

6. Kopieer de gewenste URL ('s) volgens uw vereiste in het gedeelte **ON24 virtuele omgeving instellen voor SAML-verbinding** .

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-on24-virtual-environment-saml-connection-single-sign-on"></a>Eenmalige aanmelding voor ON24 Virtual Environment configureren voor SAML-verbinding

Voor het configureren van eenmalige aanmelding op de **SAML-verbinding van ON24 Virtual Environment** , moet u het gedownloade **XML-bestand met federatieve meta gegevens** en de juiste gekopieerde url's verzenden van Azure Portal naar [ON24 virtuele omgeving SAML Connection ondersteunings team](https://www.on24.com/about-us/support/). Het team stelt de instellingen zo in dat de verbinding tussen SAML en eenmalige aanmelding aan beide zijden goed is ingesteld.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam****Britta Simon**in.
  
    b. Typ in het veld **gebruikers naam** **brittasimon\@yourcompanydomain. extension**  
    Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Julia Simon in om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen tot de SAML-verbinding van ON24 virtual environment.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **ON24 virtuele omgeving SAML-verbinding**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst toepassingen de optie **ON24 virtuele omgeving SAML-verbinding**.

    ![De ON24 voor de SAML-verbinding van de virtuele omgeving in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoog venster **gebruikers en groepen** **Julia Simon** in de lijst gebruikers en klik vervolgens op de knop **selecteren** onder aan het scherm.

6. Als u een wille keurige rol verwacht in de SAML-bewering, selecteert u in het dialoog venster **rol selecteren** de juiste rol voor de gebruiker in de lijst en klikt u op de knop **selecteren** onder aan het scherm.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-on24-virtual-environment-saml-connection-test-user"></a>ON24-test gebruiker voor SAML-verbinding met virtuele omgeving maken

In deze sectie maakt u een gebruiker met de naam Julia Simon in de SAML-verbinding van ON24 virtual environment. Werk met [ON24 Virtual Environment-ondersteunings team voor SAML-verbinding](https://www.on24.com/about-us/support/) om de gebruikers toe te voegen in het SAML-VERBINDINGS platform ON24 virtual environment. Er moeten gebruikers worden gemaakt en geactiveerd voordat u eenmalige aanmelding kunt gebruiken.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel ON24 virtuele omgeving SAML-verbinding in het toegangs venster klikt, moet u automatisch worden aangemeld bij de SAML-verbinding ON24 Virtual Environment waarvoor u SSO hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

