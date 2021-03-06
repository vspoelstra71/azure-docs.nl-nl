---
title: 'Zelf studie: integratie Azure Active Directory met Picturepark | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en Picturepark.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 31c21cd4-9c00-4cad-9538-a13996dc872f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/18/2019
ms.author: jeedes
ms.openlocfilehash: 08c5bd8da0dda74156b2d44c8106ed345ef749dd
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "73177012"
---
# <a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Zelf studie: integratie Azure Active Directory met Picturepark

In deze zelf studie leert u hoe u Picturepark integreert met Azure Active Directory (Azure AD).
Het integreren van Picturepark met Azure AD biedt de volgende voor delen:

* U kunt beheren in azure AD die toegang heeft tot Picturepark.
* U kunt ervoor zorgen dat uw gebruikers automatisch worden aangemeld bij Picturepark (eenmalige aanmelding) met hun Azure AD-accounts.
* U kunt uw accounts vanaf één centrale locatie beheren: de Azure-portal.

Zie [What is application access and single sign-on with Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis) (Wat houden toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory in?) als u wilt graag meer wilt weten over de integratie van SaaS-apps met Azure AD.
Als u nog geen abonnement op Azure hebt, [Maak dan een gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u Azure AD-integratie met Picturepark wilt configureren, hebt u de volgende items nodig:

* Een Azure AD-abonnement Als u geen Azure AD-omgeving hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) aanvragen
* Abonnement voor eenmalige aanmelding Picturepark ingeschakeld

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelfstudie gaat u in een testomgeving eenmalige aanmelding van Azure AD configureren en testen.

* Picturepark ondersteunt door **SP** GEÏNITIEERDe SSO

## <a name="adding-picturepark-from-the-gallery"></a>Picturepark toevoegen uit de galerie

Als u de integratie van Picturepark in azure AD wilt configureren, moet u Picturepark uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

**Voer de volgende stappen uit om Picturepark toe te voegen uit de galerie:**

1. Klik in het linkernavigatievenster in de **[Azure-portal](https://portal.azure.com)** op het **Azure Active Directory**-pictogram.

    ![De knop Azure Active Directory](common/select-azuread.png)

2. Navigeer naar **Bedrijfstoepassingen** en selecteer vervolgens de optie **Alle toepassingen**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

3. Als u de nieuwe toepassing wilt toevoegen, klikt u op de knop **Nieuwe toepassing** boven aan het dialoogvenster.

    ![De knop Nieuwe toepassing](common/add-new-app.png)

4. Typ **Picturepark**in het zoekvak, selecteer **Picturepark** in het resultaten paneel en klik vervolgens op de knop **toevoegen** om de toepassing toe te voegen.

     ![Picturepark in de lijst met resultaten](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD-eenmalige aanmelding configureren en testen

In deze sectie kunt u eenmalige aanmelding voor Azure AD configureren en testen met Picturepark op basis van een test gebruiker met de naam **Julia Simon**.
Voor een goede werking van eenmalige aanmelding moet er een koppelings relatie tussen een Azure AD-gebruiker en de bijbehorende gebruiker in Picturepark tot stand worden gebracht.

Als u eenmalige aanmelding voor Azure AD wilt configureren en testen met Picturepark, moet u de volgende bouw stenen volt ooien:

1. **[Azure AD-eenmalige aanmelding configureren](#configure-azure-ad-single-sign-on)**: als u wilt dat uw gebruikers deze functie kunnen gebruiken.
2. **[Picturepark eenmalige aanmelding configureren](#configure-picturepark-single-sign-on)** : Hiermee configureert u de instellingen voor eenmalige aanmelding aan de kant van de toepassing.
3. **[Een Azure AD-testgebruiker maken](#create-an-azure-ad-test-user)**: als u Azure AD-eenmalige aanmelding wil testen met Britta Simon.
4. **[De testgebruiker van Azure AD-toewijzen](#assign-the-azure-ad-test-user)**: als u wilt dat Britta Simon gebruik kan maken van Azure AD-eenmalige aanmelding.
5. **[Maak een Picturepark-test gebruiker](#create-picturepark-test-user)** -om een equivalent van Julia Simon in Picturepark te hebben dat is gekoppeld aan de Azure AD-representatie van de gebruiker.
6. **[Eenmalige aanmelding testen](#test-single-sign-on)**: als u wilt controleren of de configuratie werkt.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD configureren voor eenmalige aanmelding

In deze sectie gaat u Azure AD-eenmalige aanmelding in de Azure-portal inschakelen.

Voer de volgende stappen uit om eenmalige aanmelding voor Azure AD te configureren met Picturepark:

1. Selecteer in de [Azure Portal](https://portal.azure.com/)op de pagina **Picturepark** Application Integration de optie **eenmalige aanmelding**.

    ![Koppeling Eenmalige aanmelding configureren](common/select-sso.png)

2. In het dialoogvenster **Een methode voor eenmalige aanmelding selecteren** selecteert u de modus **SAML/WS-Federation** om eenmalige aanmelding in te schakelen.

    ![De modus Eenmalige aanmelding selecteren](common/select-saml-option.png)

3. Op de pagina **Eenmalige aanmelding met SAML instellen** klikt u op het pictogram **Bewerken** om het dialoogvenster **Standaard SAML-configuratie** te openen.

    ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

4. In de sectie **Standaard SAML-configuratie** voert u de volgende stappen uit:

    ![Informatie over eenmalige aanmelding voor Picturepark domein en Url's](common/sp-identifier.png)

    a. In het tekstvak **Aanmeldings-URL** typt u een URL met de volgende notatie: `https://<companyname>.picturepark.com`

    b. In het tekstvak **Id (Entiteits-id)** typt u een URL met het volgende patroon: 

    |  |
    |--|
    | `https://<companyname>.current-picturepark.com`|
    | `https://<companyname>.picturepark.com`|
    | `https://<companyname>.next-picturepark.com`|
    | |

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke aanmeldings-URL en -id. Neem contact op met het [ondersteunings team van Picturepark-clients](https://picturepark.com/company/picturepark-customer-support) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

5. Klik in de sectie **SAML-handtekeningcertificaat** op de knop **Bewerken** om het dialoogvenster **SAML-handtekeningcertificaat** te openen.

    ![SAML-handtekeningcertificaat bewerken](common/edit-certificate.png)

6. Kopieer de **vinger afdruk** in het gedeelte **SAML-handtekening certificaat** en sla het op uw computer op.

    ![Waarde van vingerafdruk kopiëren](common/copy-thumbprint.png)

7. Kopieer op de sectie **Picturepark instellen** de gewenste URL ('s) volgens uw vereiste. Gebruik voor **aanmeldings-URL**de waarde met het volgende patroon:`https://login.microsoftonline.com/_my_directory_id_/wsfed`

    > [!Note]
    > _my_directory_id_ is de Tenant-id van het Azure AD-abonnement.

    ![Configuratie-URL's kopiëren](./media/picturepark-tutorial/configurls.png)

    a. Azure AD-id

    b. Afmeldings-URL

### <a name="configure-picturepark-single-sign-on"></a>Eenmalige aanmelding voor Picturepark configureren

1. Meld u in een ander webbrowser venster als beheerder aan bij uw Picturepark-bedrijfs site.

2. Klik in de werk balk bovenaan op **systeem beheer**en klik vervolgens op **beheer console**.
   
    ![Beheer console](./media/picturepark-tutorial/ic795062.png "Beheer console")

3. Klik op **verificatie**en klik vervolgens op **id-providers**.
   
    ![Verificatie](./media/picturepark-tutorial/ic795063.png "Verificatie")

4. Voer de volgende stappen uit in de sectie configuratie van de **identiteits provider** :
   
    ![Configuratie van de ID-provider](./media/picturepark-tutorial/ic795064.png "Configuratie van de ID-provider")
   
    a. Klik op **Add**.
  
    b. Typ een naam voor uw configuratie.
   
    c. Selecteer **als standaard instellen**.
   
    d. Plak in het tekstvak **URI van uitgever** de waarde van de **aanmeldings-URL** die u hebt gekopieerd uit Azure Portal.
   
    e. Plak in het tekstvak **vertrouwde uitgever van miniatuur** de waarde van de **vinger afdruk** die u hebt gekopieerd uit de sectie **SAML-handtekening certificaat** . 

5. Klik op **JoinDefaultUsersGroup**.

6. Als u het kenmerk **EmailAddress** wilt instellen in het tekstvak **claim** , typt `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` u en klikt u op **Opslaan**.

      ![Configuratie](./media/picturepark-tutorial/ic795065.png "Configuratie")

### <a name="create-an-azure-ad-test-user"></a>Een Azure AD-testgebruiker maken 

Het doel van deze sectie is om in de Azure-portal een testgebruiker met de naam Britta Simon te maken.

1. Selecteer in het linkerdeelvenster in de Azure-portal de optie **Azure Active Directory**, selecteer **Gebruikers** en selecteer vervolgens **Alle gebruikers**.

    ![De koppelingen Gebruikers en groepen en Alle gebruikers](common/users.png)

2. Selecteer **nieuwe gebruiker** boven aan het scherm.

    ![Knop Nieuwe gebruiker](common/new-user.png)

3. In Gebruikerseigenschappen voert u de volgende stappen uit.

    ![Het dialoogvenster Gebruiker](common/user-properties.png)

    a. Voer in het veld **Naam****Britta Simon**in.
  
    b. Typ `brittasimon@yourcompanydomain.extension`in het veld **gebruikers naam** . Bijvoorbeeld: BrittaSimon@contoso.com

    c. Schakel het selectievakje **Wachtwoord weergeven** in en noteer de waarde die wordt weergegeven in het vak Wachtwoord.

    d. Klik op **maken**.

### <a name="assign-the-azure-ad-test-user"></a>De Azure AD-testgebruiker toewijzen

In deze sectie schakelt u Julia Simon in om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen aan Picturepark.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**, selecteer **alle toepassingen**en selecteer vervolgens **Picturepark**.

    ![De blade Bedrijfstoepassingen](common/enterprise-applications.png)

2. Selecteer in de lijst toepassingen de optie **Picturepark**.

    ![De koppeling Picturepark in de lijst met toepassingen](common/all-applications.png)

3. Selecteer in het menu aan de linkerkant **Gebruikers en groepen**.

    ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

4. Klik op de knop**Gebruiker toevoegen** en selecteer vervolgens **Gebruikers en groepen** in het dialoogvenster **Toewijzing toevoegen**.

    ![Het deelvenster Toewijzing toevoegen](common/add-assign-user.png)

5. Selecteer in het dialoog venster **gebruikers en groepen** **Julia Simon** in de lijst gebruikers en klik vervolgens op de knop **selecteren** onder aan het scherm.

6. Als u een wille keurige rol verwacht in de SAML-bewering, selecteert u in het dialoog venster **rol selecteren** de juiste rol voor de gebruiker in de lijst en klikt u op de knop **selecteren** onder aan het scherm.

7. Klik in het dialoogvenster **Toewijzing toevoegen** op de knop **Toewijzen**.

### <a name="create-picturepark-test-user"></a>Picturepark-test gebruiker maken

Om ervoor te zorgen dat Azure AD-gebruikers zich kunnen aanmelden bij Picturepark, moeten ze worden ingericht in Picturepark. In het geval van Picturepark is inrichting een hand matige taak.

**Voer de volgende stappen uit als u een gebruikersaccount wilt inrichten:**

1. Meld u aan bij uw **Picturepark** -Tenant.

1. Klik in de werk balk bovenaan op **systeem beheer**en klik vervolgens op **gebruikers**.
   
    ![Gebruikers](./media/picturepark-tutorial/ic795067.png "Gebruikers")

1. Klik op het tabblad **overzicht van gebruikers** op **Nieuw**.
   
    ![Gebruikersbeheer](./media/picturepark-tutorial/ic795068.png "Gebruikersbeheer")

1. Voer in het dialoog venster **gebruiker maken** de volgende stappen uit van een geldige Azure Active Directory gebruiker die u wilt inrichten:
   
    ![Gebruiker maken](./media/picturepark-tutorial/ic795069.png "Gebruiker maken")
   
    a. Typ in het tekstvak **e-mail adres** het **e-mail adres** van de gebruiker `BrittaSimon@contoso.com`.  
   
    b. Typ in het tekstvak **wacht woord** en **wacht woord bevestigen** het **wacht woord** van BrittaSimon. 
   
    c. Typ in het tekstvak **voor de voor naam** de **voor naam** van de gebruiker **Julia**. 
   
    d. Typ in het tekstvak **Achternaam** de **Achternaam** van de gebruiker **Simon**.
   
    e. Typ in het tekstvak **Company** de **Bedrijfs naam** van de gebruiker. 
   
    f. Selecteer in het tekstvak **Country** het **land/de regio** van de gebruiker.
  
    g. Typ in het tekstvak **zip** de **Post code** van de plaats.
   
    h. Typ in het tekstvak **City** de **plaatsnaam** van de gebruiker.

    i. Selecteer een **taal**.
   
    j. Klik op **maken**.

>[!NOTE]
>U kunt alle andere hulpprogram ma's voor het maken van Picturepark-gebruikers accounts of Api's die worden geleverd door Picturepark, gebruiken om Azure AD-gebruikers accounts in te richten.
>

### <a name="test-single-sign-on"></a>Eenmalige aanmelding testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel Picturepark in het toegangs venster klikt, moet u automatisch worden aangemeld bij de Picturepark waarvoor u SSO hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende resources

- [Lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

