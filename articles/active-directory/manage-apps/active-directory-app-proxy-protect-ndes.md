---
title: Integreren met AD-toepassingsproxy op een NDES-server
titleSuffix: Azure Active Directory
description: Richt lijnen voor het implementeren van een Azure Active Directory-toepassingsproxy voor het beveiligen van uw NDES-server.
services: active-directory
author: CelesteDG
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2020
ms.author: baselden
ms.reviewer: mimart
ms.openlocfilehash: 4ccd8834671725ace72497391090f81eb197ad6a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "77032255"
---
# <a name="integrate-with-azure-ad-application-proxy-on-a-network-device-enrollment-service-ndes-server"></a>Integreren met Azure AD-toepassingsproxy op een server met een registratie service voor netwerk apparaten (NDES)

Met Azure Active Directory (AD) toepassings proxy kunt u toepassingen in uw netwerk publiceren. Deze toepassingen zijn bijvoorbeeld share point-sites, micro soft Outlook Web app en andere webtoepassingen. Het biedt ook veilige toegang tot gebruikers buiten uw netwerk via Azure.

Als u geen ervaring hebt met Azure AD-toepassingsproxy en meer informatie wilt, raadpleegt u [externe toegang tot on-premises toepassingen via Azure AD-toepassingsproxy](application-proxy.md).

Azure AD-toepassingsproxy is gebaseerd op Azure. Het biedt u een enorme hoeveelheid netwerk bandbreedte en server infrastructuur voor betere beveiliging tegen gedistribueerde Denial-of-service-aanvallen (DDOS) en een uitstekende Beschik baarheid. Bovendien is het niet nodig om externe firewall poorten te openen voor uw on-premises netwerk en is er geen DMZ-server vereist. Al het verkeer is afkomstig van binnenkomend. Zie [zelf studie: een on-premises toepassing toevoegen voor externe toegang via toepassings proxy in azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#prepare-your-on-premises-environment)voor een volledige lijst met uitgaande poorten.

> Azure AD-toepassingsproxy is een functie die alleen beschikbaar is als u de Premium-of Basic-edities van Azure Active Directory gebruikt. Zie [Azure Active Directory prijzen](https://azure.microsoft.com/pricing/details/active-directory/)voor meer informatie. 
> Als u licenties voor Enter prise Mobility Suite (EMS) hebt, komt u in aanmerking voor deze oplossing te gebruiken.
> De Azure AD-toepassingsproxy-connector wordt alleen geïnstalleerd op Windows Server 2012 R2 of hoger. Dit is ook een vereiste van de NDES-server.

## <a name="install-and-register-the-connector-on-the-ndes-server"></a>De connector op de NDES-server installeren en registreren

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/) als een toepassingsbeheerder van de map die gebruikmaakt van Application Proxy. Als het domein van de tenant bijvoorbeeld contoso.com is, moet de beheerder admin@contoso.com of een andere beheerdersalias in dat domein zijn.
1. Selecteer uw gebruikers naam in de rechter bovenhoek. Controleer of u bent aangemeld bij een map die gebruikmaakt van de toepassings proxy. Als u mappen wilt wijzigen, selecteert u **Directory activeren** en kiest u een map die toepassings proxy gebruikt.
1. Selecteer in het navigatie venster aan de linkerkant **Azure Active Directory**.
1. Selecteer **toepassings proxy**onder **beheren**.
1. Selecteer **Connector service downloaden**.

    ![Connector service downloaden om de service voorwaarden te bekijken](./media/active-directory-app-proxy-protect-ndes/application-proxy-download-connector-service.png)

1. Lees de servicevoorwaarden. Wanneer u klaar bent, selecteert u **voor waarden accepteren & downloaden**.
1. Kopieer het installatie bestand voor de Azure AD-toepassingsproxy-connector naar de NDES-server. 
   > U kunt de connector installeren op elke server in uw bedrijfs netwerk met toegang tot NDES. U hoeft deze niet te installeren op de NDES-server zelf.
1. Voer het installatie bestand uit, zoals *AADApplicationProxyConnectorInstaller. exe*. Ga akkoord met de licentie voorwaarden van de software.
1. Tijdens de installatie wordt u gevraagd de connector te registreren bij de toepassings proxy in uw Azure AD-adres lijst.
   * Geef de referenties voor een globale of toepassings beheerder op in uw Azure AD-adres lijst. De globale gegevens van de Azure AD-of toepassings beheerder kunnen afwijken van uw Azure-referenties in de portal.

        > [!NOTE]
        > Het globale of toepassings beheerders account dat wordt gebruikt om de connector te registreren, moet deel uitmaken van dezelfde map waar u de Application proxy-service inschakelt.
        >
        > Als het Azure AD-domein bijvoorbeeld *contoso.com*is, moet de beheerder van de globale groep/ `admin@contoso.com` toepassing een andere geldige alias hebben op dat domein.

   * Als verbeterde beveiliging van Internet Explorer is ingeschakeld voor de server waarop u de connector installeert, is het registratie scherm mogelijk geblokkeerd. Als u toegang wilt toestaan, volgt u de instructies in het fout bericht of schakelt u de verbeterde beveiliging van Internet Explorer uit tijdens het installatie proces.
   * Zie [problemen met toepassings proxy oplossen](application-proxy-troubleshoot.md)als de registratie van de connector mislukt.
1. Aan het einde van de installatie wordt een notitie weer gegeven voor omgevingen met een uitgaande proxy. Als u de Azure AD-toepassingsproxy-connector wilt configureren om te werken via de uitgaande proxy, voert u het `C:\Program Files\Microsoft AAD App Proxy connector\ConfigureOutBoundProxy.ps1`meegeleverde script uit, zoals.
1. Op de pagina Toepassings proxy in de Azure Portal wordt de nieuwe connector weer gegeven met de status *actief*, zoals wordt weer gegeven in het volgende voor beeld:

    ![De nieuwe Azure AD-toepassingsproxy-connector wordt weer gegeven als actief in de Azure Portal](./media/active-directory-app-proxy-protect-ndes/connected-app-proxy.png)

    > [!NOTE]
    > Om hoge Beschik baarheid te bieden voor toepassingen die worden geverifieerd via de Azure AD-toepassingsproxy, kunt u connectors op meerdere Vm's installeren. Herhaal de stappen in de vorige sectie om de connector te installeren op andere servers die zijn toegevoegd aan het Azure AD DS beheerde domein.

1. Nadat de installatie is voltooid, gaat u terug naar de Azure Portal.

1. Selecteer **bedrijfs toepassingen**.

   ![Zorg ervoor dat u de juiste belanghebbenden gebruikt](./media/active-directory-app-proxy-protect-ndes/azure-active-directory-enterprise-applications.png)

1. Selecteer **+ nieuwe toepassing**en selecteer vervolgens de optie **on-premises toepassing**. 

1. Configureer de volgende velden in het veld **uw eigen on-premises toepassing toevoegen**:

   * **Naam**: Voer een naam in voor de toepassing.
   * **Interne URL**: Voer de interne URL/FQDN in van de NDES-server waarop u de connector hebt geïnstalleerd.
   * **Pre-authenticatie**: Selecteer **passthrough**. Het is niet mogelijk om een vorm van vooraf-verificatie te gebruiken. Het protocol dat wordt gebruikt voor certificaat aanvragen (SCEP) biedt deze optie niet.
   * Kopieer de meegeleverde **externe URL** naar het klem bord.

1. Selecteer **+ toevoegen** om uw toepassing op te slaan.

1. Test of u toegang hebt tot de NDES-server via de Azure AD-toepassings proxy door de koppeling die u in stap 10 hebt gekopieerd, in een browser te plakken. Er wordt een standaard welkomst pagina van IIS weer geven.

1. Als laatste test voegt u het pad *MSCEP. dll* toe aan de bestaande URL die u in de vorige stap hebt geplakt:

   https://scep-test93635307549127448334.msappproxy.net/certsrv/mscep/mscep.dll

1. Er wordt een **HTTP-fout 403-niet-toegestaan** antwoord weer geven.

1. Wijzig de door de beschik bare NDES-URL (via Microsoft Intune) op apparaten. deze wijziging kan zich in het micro soft-eindpunt configuratie centrum of in de intune-Cloud beontvangen.

   * Ga voor Configuration Center naar het certificaat registratiepunt (CRP) en pas de URL aan. Deze URL is wat apparaten aanroepen en hun uitdaging presen teren.
   * Voor alleen de intune-Cloud, ook wel zelfstandige versie van intune genoemd, kunt u een nieuw SCEP-beleid bewerken of maken en de nieuwe URL toevoegen.

## <a name="next-steps"></a>Volgende stappen

Met Azure AD-toepassingsproxy geïntegreerd met NDES, publiceert u toepassingen voor gebruikers om toegang te krijgen. Zie [toepassingen publiceren met Azure AD-toepassingsproxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application)voor meer informatie.
