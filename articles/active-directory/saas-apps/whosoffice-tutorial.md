---
title: 'Zelf studie: Azure Active Directory de integratie van eenmalige aanmelding (SSO) met WhosOffice | Microsoft Docs'
description: Meer informatie over het configureren van eenmalige aanmelding tussen Azure Active Directory en WhosOffice.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e7de0266-312a-4f8a-a78f-16729ebc8c2f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/21/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: dc6eee984f6c6fbdf916d1287d98b921158271af
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "79136394"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-whosoffice"></a>Zelf studie: Azure Active Directory de integratie van eenmalige aanmelding (SSO) met WhosOffice

In deze zelf studie leert u hoe u WhosOffice integreert met Azure Active Directory (Azure AD). Wanneer u WhosOffice integreert met Azure AD, kunt u het volgende doen:

* Controle in azure AD die toegang heeft tot WhosOffice.
* Zorg ervoor dat uw gebruikers automatisch worden aangemeld bij WhosOffice met hun Azure AD-accounts.
* Beheer uw accounts op één centrale locatie: de Azure Portal.

Zie [Wat is toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](/azure/active-directory/manage-apps/what-is-single-sign-on)voor meer informatie over SaaS-app-integratie met Azure AD.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende items nodig om aan de slag te gaan:

* Een Azure AD-abonnement Als u geen abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/)aanvragen.
* WhosOffice-abonnement dat is ingeschakeld voor eenmalige aanmelding (SSO).

## <a name="scenario-description"></a>Scenariobeschrijving

In deze zelf studie configureert en test u Azure AD SSO in een test omgeving.

* WhosOffice ondersteunt SSO die door **SP en IDP** is geïnitieerd
* Nadat u WhosOffice hebt geconfigureerd, kunt u sessie beheer afdwingen, waardoor exfiltration en infiltratie van de gevoelige gegevens van uw organisatie in real-time worden beschermd. Sessie beheer wordt uitgebreid vanuit voorwaardelijke toegang. [Meer informatie over het afdwingen van sessie beheer met Microsoft Cloud app Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

> [!NOTE]
> De id van deze toepassing is een vaste teken reeks waarde zodat slechts één exemplaar in één Tenant kan worden geconfigureerd.

## <a name="adding-whosoffice-from-the-gallery"></a>WhosOffice toevoegen uit de galerie

Als u de integratie van WhosOffice in azure AD wilt configureren, moet u WhosOffice uit de galerie toevoegen aan uw lijst met beheerde SaaS-apps.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) met behulp van een werk-of school account of een persoonlijke Microsoft-account.
1. Selecteer de **Azure Active Directory** -service in het navigatie deel venster aan de linkerkant.
1. Ga naar **bedrijfs toepassingen** en selecteer **alle toepassingen**.
1. Selecteer **nieuwe toepassing**om een nieuwe toepassing toe te voegen.
1. Typ in de sectie **toevoegen vanuit de galerie** **WhosOffice** in het zoekvak.
1. Selecteer **WhosOffice** uit het paneel resultaten en voeg vervolgens de app toe. Wacht een paar seconden wanneer de app aan uw Tenant is toegevoegd.


## <a name="configure-and-test-azure-ad-single-sign-on-for-whosoffice"></a>Eenmalige aanmelding voor Azure AD configureren en testen voor WhosOffice

Azure AD SSO met WhosOffice configureren en testen met behulp van een test gebruiker met de naam **B. Simon**. Voor het werken met SSO moet u een koppelings relatie tot stand brengen tussen een Azure AD-gebruiker en de bijbehorende gebruiker in WhosOffice.

Als u Azure AD SSO wilt configureren en testen met WhosOffice, voltooit u de volgende bouw stenen:

1. **[Configureer Azure AD SSO](#configure-azure-ad-sso)** -om uw gebruikers in staat te stellen deze functie te gebruiken.
    * **[Een Azure AD-test gebruiker maken](#create-an-azure-ad-test-user)** : u kunt eenmalige aanmelding voor Azure AD testen met B. Simon.
    * **[Wijs de Azure AD-test gebruiker](#assign-the-azure-ad-test-user)** toe, zodat B. Simon de eenmalige aanmelding van Azure AD kan gebruiken.
1. **[WHOSOFFICE SSO configureren](#configure-whosoffice-sso)** : voor het configureren van de instellingen voor eenmalige aanmelding aan de kant van de toepassing.
    * **[Maak een WhosOffice-test gebruiker](#create-whosoffice-test-user)** -om een equivalent van B. Simon in WhosOffice te hebben dat is gekoppeld aan de Azure AD-representatie van de gebruiker.
1. **[SSO testen](#test-sso)** : om te controleren of de configuratie werkt.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO-configureren

Volg deze stappen om Azure AD SSO in te scha kelen in de Azure Portal.

1. Zoek in het [Azure Portal](https://portal.azure.com/)op de pagina Toepassings integratie van **WhosOffice** de sectie **beheren** en selecteer **eenmalige aanmelding**.
1. Selecteer op de pagina **Eén aanmeldings methode selecteren** de optie **SAML**.
1. Klik op de pagina **eenmalige aanmelding met SAML instellen** op het pictogram bewerken/pen voor **eenvoudige SAML-configuratie** om de instellingen te bewerken.

   ![Standaard SAML-configuratie bewerken](common/edit-urls.png)

1. Als u de toepassing in de gestarte modus **IDP** wilt configureren, voert u in de sectie **basis configuratie van SAML** de waarden voor de volgende velden in:

    Typ in het tekstvak **antwoord-URL** een URL met het volgende patroon:`https://<SUBDOMAIN>.my.whosoffice.com/int/azure/consume.aspx`

1. Klik op **Extra URL's instellen** en voer de volgende stap uit als u de toepassing in de door **SP** geïnitieerde modus wilt configureren:

    In het tekstvak **Aanmeldings-URL** typt u een URL met het volgende patroon: `https://<SUBDOMAIN>.my.whosoffice.com/int/azure`

    > [!NOTE]
    > Dit zijn geen echte waarden. Werk deze waarden bij met de werkelijke antwoord-URL en aanmeldings-URL. Neem contact op met het [ondersteunings team van WhosOffice-clients](mailto:support@whosoffice.com) om deze waarden te verkrijgen. U kunt ook verwijzen naar het patroon dat wordt weergegeven in de sectie **Standaard SAML-configuratie** in de Azure-portal.

1. Zoek op de pagina **eenmalige aanmelding met SAML instellen** , in de sectie **SAML-handtekening certificaat** , de **federatieve meta gegevens-XML** en selecteer **downloaden** om het certificaat te downloaden en op uw computer op te slaan.

    ![De link om het certificaat te downloaden](common/metadataxml.png)

1. Op de sectie **WhosOffice instellen** kopieert u de gewenste URL ('s) op basis van uw vereiste.

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

In deze sectie schakelt u B. Simon in om eenmalige aanmelding van Azure te gebruiken door toegang te verlenen aan WhosOffice.

1. Selecteer in het Azure Portal **bedrijfs toepassingen**en selecteer vervolgens **alle toepassingen**.
1. Selecteer in de lijst toepassingen de optie **WhosOffice**.
1. Ga op de pagina overzicht van de app naar de sectie **beheren** en selecteer **gebruikers en groepen**.

   ![De koppeling Gebruikers en groepen](common/users-groups-blade.png)

1. Selecteer **gebruiker toevoegen**en selecteer vervolgens **gebruikers en groepen** in het dialoog venster **toewijzing toevoegen** .

    ![De koppeling gebruiker toevoegen](common/add-assign-user.png)

1. Selecteer in het dialoog venster **gebruikers en groepen** **B. Simon** van de lijst gebruikers en klik vervolgens op de knop **selecteren** onder aan het scherm.
1. Als u een wille keurige rol verwacht in de SAML-bewering, selecteert u in het dialoog venster **rol selecteren** de juiste rol voor de gebruiker in de lijst en klikt u op de knop **selecteren** onder aan het scherm.
1. Klik in het dialoog venster **toewijzing toevoegen** op de knop **toewijzen** .

## <a name="configure-whosoffice-sso"></a>WhosOffice SSO configureren

1. Meld u in een ander webbrowser venster aan bij de WhosOffice-website als beheerder.

1. Klik op **instellingen** en selecteer **bedrijf**.

    ![WhosOffice-configuratie](./media/whosoffice-tutorial/configuration1.png)

1. Klik op **apps/integraties**.

    ![WhosOffice-configuratie](./media/whosoffice-tutorial/configuration2.png)

1. Selecteer **Microsoft Azure** in de vervolg keuzelijst provider en klik op **aanmeldings provider activeren**.

    ![WhosOffice-configuratie](./media/whosoffice-tutorial/configuration3.png)

1. Upload het gedownloade bestand met federatieve meta gegevens van Azure Portal door te klikken op de **Upload** optie.
    
    ![WhosOffice-configuratie](./media/whosoffice-tutorial/configuration4.png)

### <a name="create-whosoffice-test-user"></a>WhosOffice-test gebruiker maken

1. Meld u in een ander webbrowser venster aan bij de WhosOffice-website als beheerder.

1. Klik op **instellingen** en selecteer **gebruikers**.

    ![WhosOffice-configuratie](./media/whosoffice-tutorial/user1.png)

1. Selecteer **nieuwe gebruiker maken**.

    ![WhosOffice-configuratie](./media/whosoffice-tutorial/user2.png)

1. Geef de benodigde gegevens van de gebruiker op volgens uw organisatie vereiste.

    ![WhosOffice-configuratie](./media/whosoffice-tutorial/user3.png)

## <a name="test-sso"></a>SSO testen 

In deze sectie gaat u uw configuratie van Azure AD-eenmalige aanmelding testen via het toegangsvenster.

Wanneer u op de tegel WhosOffice in het toegangs venster klikt, moet u automatisch worden aangemeld bij de WhosOffice waarvoor u SSO hebt ingesteld. Zie [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Inleiding tot het toegangsvenster) voor meer informatie over het toegangsvenster.

## <a name="additional-resources"></a>Aanvullende bronnen

- [Lijst met zelf studies voor het integreren van SaaS-apps met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [What is application access and single sign-on with Azure Active Directory? ](/azure/active-directory/manage-apps/what-is-single-sign-on) (Wat is toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?)

- [Wat is voorwaardelijke toegang in Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Probeer WhosOffice met Azure AD](https://aad.portal.azure.com/)

- [Wat is sessie beheer in Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)