---
title: App-wacht woorden beheren-Azure Active Directory | Microsoft Docs
description: Meer informatie over app-wacht woorden en wat ze worden gebruikt voor met betrekking tot verificatie in twee stappen.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: richagi
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 08/13/2019
ms.author: curtand
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: 62723e56bf78a4d80a6e026185d50537fdfa847f
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2020
ms.locfileid: "83738889"
---
# <a name="manage-app-passwords-for-two-step-verification"></a>App-wacht woorden beheren voor verificatie in twee stappen

>[!Important]
>Uw beheerder staat mogelijk niet toe dat u app-wacht woorden gebruikt. Als u app- **wacht woorden** niet ziet als een optie, zijn ze niet beschikbaar in uw organisatie.

Wanneer u app-wacht woorden gebruikt, is het belang rijk om het volgende te onthouden:

- App-wacht woorden worden automatisch gegenereerd en moeten eenmaal per app worden gemaakt en ingevoerd.

- Er is een limiet van 40 wacht woorden per gebruiker. Als u later een van deze limieten probeert te maken, wordt u gevraagd een bestaand wacht woord te verwijderen voordat u de nieuwe kunt maken.

    >[!Note]
    >Office 2013-clients (inclusief Outlook) ondersteunen nieuwe verificatie protocollen en kunnen worden gebruikt met verificatie in twee stappen. Deze ondersteuning betekent dat als u verificatie in twee stappen hebt ingeschakeld, u geen app-wacht woorden meer nodig hebt voor Office 2013-clients. Zie voor meer informatie het artikel [hoe moderne verificatie werkt voor office 2013-en office 2016-client-apps](https://support.office.com/article/how-modern-authentication-works-for-office-2013-and-office-2016-client-apps-e4c45989-4b1a-462e-a81b-2a13191cf517) .

## <a name="create-new-app-passwords"></a>Nieuwe app-wacht woorden maken

Tijdens het eerste registratie proces voor twee ledige verificatie wordt u met één app-wacht woord opgegeven. Als u meer dan één nodig hebt, moet u ze zelf maken. U kunt app-wacht woorden maken op basis van meerdere gebieden, afhankelijk van de instelling van twee ledige verificatie in uw organisatie. Zie [overzicht voor twee ledige verificatie en uw werk-of school account](multi-factor-authentication-end-user-first-time.md) en de bijbehorende artikelen voor meer informatie over het registreren van het gebruik van twee ledige verificatie met uw werk-of school account.

### <a name="where-to-create-and-delete-your-app-passwords"></a>Waar u uw app-wacht woorden kunt maken en verwijderen?

U kunt app-wacht woorden maken en verwijderen op basis van de manier waarop u twee ledige verificatie gebruikt:

- **Uw organisatie gebruikt twee ledige verificatie en de extra beveiligings verificatie pagina.** Als u uw werk-of school account (zoals alain@contoso.com ) gebruikt met twee ledige verificatie in uw organisatie, kunt u uw app-wacht woorden beheren via de [pagina aanvullende beveiligings verificatie](https://account.activedirectory.windowsazure.com/Proofup.aspx). Zie voor gedetailleerde instructies [app-wacht woorden maken en verwijderen met de extra beveiligings verificatie pagina](#create-and-delete-app-passwords-from-the-additional-security-verification-page) in dit artikel.

- **Uw organisatie gebruikt twee ledige verificatie en de Office 365-Portal.** Als u uw werk-of school account gebruikt (zoals, alain@contoso.com ), twee ledige verificatie en Office 365-apps in uw organisatie, kunt u uw app-wacht woorden beheren via de pagina van de [Office 365-Portal](https://www.office.com). Zie voor gedetailleerde instructies [app-wacht woorden maken en verwijderen met behulp van de Office 365-Portal](#create-and-delete-app-passwords-using-the-office-365-portal) in dit artikel.

- **U gebruikt twee ledige verificatie met een persoonlijke Microsoft-account.** Als u een persoonlijke Microsoft-account gebruikt (zoals, alain@outlook.com ) met verificatie op basis van twee factoren, kunt u uw app-wacht woorden beheren op de [pagina met beveiligings beginselen](https://account.microsoft.com/security/). Voor gedetailleerde instructies raadpleegt [u app-wacht woorden gebruiken met apps die geen ondersteuning bieden voor verificatie in twee stappen](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-and-two-step-verification).

## <a name="create-and-delete-app-passwords-from-the-additional-security-verification-page"></a>App-wacht woorden maken en verwijderen op de pagina aanvullende beveiligings verificatie

U kunt app-wacht woorden maken en verwijderen op de pagina **aanvullende beveiligings verificatie** voor uw werk-of school account.

1. Meld u aan bij de [aanvullende beveiligings verificatie pagina](https://account.activedirectory.windowsazure.com/Proofup.aspx)en selecteer vervolgens **app-wacht woorden**.

    ![Pagina app-wacht woorden, met het tabblad app-wacht woorden gemarkeerd](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page.png)

2. Selecteer **maken**, typ de naam van de app waarvoor het app-wacht woord is vereist en selecteer **volgende**.

    ![Pagina app-wacht woorden maken, met de naam van de app waarvoor het wacht woord is vereist](media/multi-factor-authentication-end-user-app-passwords/mfa-create-app-password-page.png)

3. Kopieer het wacht woord van de pagina **uw app-wacht woord** en selecteer vervolgens **sluiten**.

    ![Uw app-wacht woord pagina met het wacht woord voor uw opgegeven app](media/multi-factor-authentication-end-user-app-passwords/mfa-your-app-password-page.png)

4. Controleer op de pagina **app-wacht woorden** of uw app wordt weer gegeven.

     ![Pagina app-wacht woorden, met een nieuwe app die in de lijst wordt weer gegeven](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page-with-new-password.png)  

5. Open de app waarvoor u het app-wacht woord hebt gemaakt (bijvoorbeeld Outlook 2010) en plak het app-wacht woord wanneer u daarom wordt gevraagd. U hoeft dit slechts één keer per app uit te voeren.

### <a name="to-delete-an-app-password-using-the-app-passwords-page"></a>Een app-wacht woord verwijderen met de pagina app-wacht woorden

1. Selecteer op de pagina **app-wacht woorden** **verwijderen** naast het app-wacht woord dat u wilt verwijderen.

   ![Een app-wacht woord verwijderen](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page-delete.png)

2. Selecteer **Ja** om te bevestigen dat u het wacht woord wilt verwijderen en selecteer vervolgens **sluiten**.

    Het app-wacht woord is verwijderd.

## <a name="create-and-delete-app-passwords-using-the-office-365-portal"></a>App-wacht woorden maken en verwijderen met behulp van de Office 365-Portal

Als u verificatie in twee stappen gebruikt met uw werk-of school account en uw Office 365-apps, kunt u uw app-wacht woorden maken en verwijderen met behulp van de Office 365-Portal.

### <a name="to-create-app-passwords-using-the-office-365-portal"></a>App-wacht woorden maken met behulp van de Office 365-Portal

1. Meld u aan bij Office 365 en ga naar de [pagina Mijn account](https://portal.office.com), selecteer **beveiliging & privacy**en vouw vervolgens **extra beveiligings verificatie**uit.

    ![Office-Portal met uitgebreid extra beveiligings verificatie gebied](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-o365-my-account-page.png)

2. Selecteer de tekst waarin wordt vermeld, **Maak en beheer app-wacht woorden** om de pagina **app-wacht woorden** te openen.

    ![Pagina app-wacht woorden, met het tabblad app-wacht woorden gemarkeerd](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page.png)

3. Selecteer **maken**, typ de naam van de app waarvoor het app-wacht woord is vereist en selecteer **volgende**.

    ![Pagina app-wacht woorden maken, met de naam van de app waarvoor het wacht woord is vereist](media/multi-factor-authentication-end-user-app-passwords/mfa-create-app-password-page.png)

4. Kopieer het wacht woord van de pagina **uw app-wacht woord** en selecteer vervolgens **sluiten**.

    ![Uw app-wacht woord pagina met het wacht woord voor uw opgegeven app](media/multi-factor-authentication-end-user-app-passwords/mfa-your-app-password-page.png)

5. Controleer op de pagina **app-wacht woorden** of uw app wordt weer gegeven.

     ![Pagina app-wacht woorden, met een nieuwe app die in de lijst wordt weer gegeven](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page-with-new-password.png)  

6. Open de app waarvoor u het app-wacht woord hebt gemaakt (bijvoorbeeld Outlook 2010) en plak het app-wacht woord wanneer u daarom wordt gevraagd. U hoeft dit slechts één keer per app uit te voeren.

### <a name="to-delete-app-passwords-using-the-app-passwords-page"></a>App-wacht woorden verwijderen met de pagina app-wacht woorden

1. Selecteer op de pagina **app-wacht woorden** **verwijderen** naast het app-wacht woord dat u wilt verwijderen.

   ![Een app-wacht woord verwijderen](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page-delete.png)

2. Selecteer **Ja** in het bevestigings venster en selecteer vervolgens **sluiten**.

    Het app-wacht woord is verwijderd.

## <a name="if-your-app-passwords-arent-working-properly"></a>Als uw app-wacht woorden niet goed werken

Zorg ervoor dat u het wacht woord correct hebt getypt. Als u zeker weet dat u uw wacht woord correct hebt ingevoerd, kunt u proberen zich opnieuw aan te melden en een nieuw app-wacht woord te maken. Als geen van deze opties uw probleem verhelpen, neemt u contact op met de Help Desk van uw organisatie zodat ze uw bestaande app-wacht woorden kunnen verwijderen, zodat u gloed nieuwe maakt.

## <a name="next-steps"></a>Volgende stappen

- [De instellingen voor verificatie in twee stappen beheren](multi-factor-authentication-end-user-manage-settings.md)

- Probeer de [Microsoft Authenticator-app](user-help-auth-app-download-install.md) uit om uw aanmeldingen met app-meldingen te verifiëren in plaats van teksten of aanroepen te ontvangen.
