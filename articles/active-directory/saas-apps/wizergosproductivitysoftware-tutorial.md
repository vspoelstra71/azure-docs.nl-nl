---
title: 'Zelf studie: integratie Azure Active Directory met Wizergos-productiviteits software | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory-en Wizergos-productiviteits software.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: acc04396-13c5-4c24-ab9a-30fbc9234ebd
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: b6491013cb35f2473eff6c2019fe2a80dd9e9b08
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "67086988"
---
# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a>Zelf studie: integratie Azure Active Directory met Wizergos-productiviteits software

In deze zelf studie leert u hoe u Wizergos Productivity software integreert met Azure Active Directory (Azure AD).
Het integreren van Wizergos-productiviteits software met Azure AD biedt de volgende voor delen:

* U kunt beheren in azure AD die toegang heeft tot Wizergos-productiviteits software.
* U kunt ervoor zorgen dat uw gebruikers automatisch worden aangemeld voor Wizergos-productiviteits software (eenmalige aanmelding) met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u nog geen abonnement op Azure hebt, [Maak dan een gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u Azure AD-integratie met Wizergos-productiviteits software wilt configureren, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) aanvragen
* Abonnement voor eenmalige aanmelding van Wizergos-productiviteits software

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Wizergos Productivity software ondersteunt **IDP** GEÏNITIEERDe SSO

## <a name="adding-wizergos-productivity-software-from-the-gallery"></a>Wizergos-productiviteits software toevoegen uit de galerie

Als u de integratie van Wizergos-productiviteits software wilt configureren in azure AD, moet u Wizergos-productiviteits software van de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Als u Wizergos-productiviteits software wilt toevoegen vanuit de galerie, voert u de volgende stappen uit:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. In het zoekvak typt u **Wizergos Productivity Software**, selecteert u **Wizergos Productivity Software** van result panel en klikt u vervolgens op knop **toevoegen** om de toepassing toe te voegen.

     ![Wizergos-productiviteits software in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u eenmalige aanmelding voor Azure AD configureren en testen met Wizergos-productiviteits software op basis van een test gebruiker met de naam **Julia Simon**.
Voor een goede werking van eenmalige aanmelding moet een koppelings relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Wizergos Productivity software tot stand worden gebracht.

Als u eenmalige aanmelding voor Azure AD wilt configureren en testen met Wizergos-productiviteits software, moet u de volgende bouw stenen volt ooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Wizergos Productivity software configureren eenmalige aanmelding](#configure-wizergos-productivity-software-single-sign-on)** : Configureer de instellingen voor eenmalige aanmelding aan de kant van de toepassing.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak Wizergos Productivity Software Test User](#create-wizergos-productivity-software-test-user)** -als u een tegen hanger van Julia Simon wilt hebben in Wizergos-productiviteits software die is gekoppeld aan de Azure AD-representatie van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding voor Azure AD te configureren met Wizergos-productiviteits software:

1. Selecteer in de [Azure Portal](https://portal.azure.com/)op de pagina **Wizergos Productivity Software** Application Integration de optie **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Wizergos Productivity Software Domain and Url's single sign-on Information](common/idp-identifier.png)

    In het tekstvak **Id** typt u een URL: `https://www.wizergos.net`

5. Op de pagina **Eenmalige aanmelding met SAML instellen** in de sectie **SAML-handtekeningcertificaat** klikt u op **Downloaden** om het **Certificaat (Base64)** te downloaden uit de opgegeven opties overeenkomstig uw behoeften, en slaat u dit op uw computer op.

    ![De link om het certificaat te downloaden](common/certificatebase64.png)

6. Kopieer op de sectie **Wizergos Productivity software instellen** de gewenste URL ('s) volgens uw vereiste.

    ![Configuratie-URL's kopiëren](common/copy-configuration-urls.png)

    a. Aanmeldings-URL

    b. Azure AD-id

    c. Afmeldings-URL

### <a name="configure-wizergos-productivity-software-single-sign-on"></a>Eenmalige aanmelding van Wizergos-productiviteits software configureren

1. Meld u in een ander browser venster aan bij uw Wizergos Productivity Software Tenant als beheerder.

2. Selecteer in het Hamburger-menu de optie **beheerder**.

    ![Eenmalige aanmelding in de app configureren](./media/wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)

3. Selecteer in de pagina beheer in het menu links **verificatie** en klik op **Azure AD**.

    ![Eenmalige aanmelding in de app configureren](./media/wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)

4. Voer de volgende stappen uit in de sectie **authenticatie** .

    ![Eenmalige aanmelding in de app configureren](./media/wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)
    
    a. Klik op de knop **uploaden** om het gedownloade certificaat te uploaden vanuit Azure AD.
    
    b. Plak in het tekstvak **URL van uitgever** de id-waarde van **Azure AD** die u van Azure Portal hebt gekopieerd.
    
    c. Plak in het tekstvak **URL voor eenmalige aanmelding** de waarde voor de **aanmeldings-URL** die u hebt gekopieerd uit Azure Portal.
    
    d. Plak in het tekstvak voor de **eenmalige Afmeldings-** URL de waarde van de **afmelding** die u van Azure Portal hebt gekopieerd.
    
    e. Klik op de knop **Save**.

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam****Britta Simon**in.
  
    b. Typ brittasimon@yourcompanydomain.extensionin het veld **gebruikers naam** . Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Julia Simon in om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen tot de Wizergos-productiviteits software.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Wizergos Productivity Software**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst toepassingen de optie **Wizergos Productivity Software**.

    ![De koppeling Wizergos productiviteits software in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoog venster **gebruikers en groepen** **Julia Simon** in de lijst gebruikers en klik vervolgens op de knop **selecteren** onder aan het scherm.

6. Als u een wille keurige rol verwacht in de SAML-bewering, selecteert u in het dialoog venster **rol selecteren** de juiste rol voor de gebruiker in de lijst en klikt u op de knop **selecteren** onder aan het scherm.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-wizergos-productivity-software-test-user"></a>Wizergos Productivity Software test gebruiker maken

In deze sectie maakt u een gebruiker met de naam Julia Simon in Wizergos Productivity software. Werk samen met het [ondersteunings team van Wizergos Productivity Software](mailTo:support@wizergos.com) om gebruikers toe te voegen aan het platform van de Wizergos-productiviteit.

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel Wizergos productiviteits software in het toegangs venster klikt, moet u automatisch worden aangemeld bij de Wizergos-productiviteits software waarvoor u SSO hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

