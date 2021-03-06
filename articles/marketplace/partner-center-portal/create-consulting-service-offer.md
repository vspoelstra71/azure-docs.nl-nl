---
title: Een advies service-aanbieding maken-micro soft Commercial Marketplace
description: Meer informatie over het publiceren van een advies service-aanbieding naar Microsoft AppSource of Azure Marketplace met behulp van partner centrum.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/19/2020
ms.openlocfilehash: bd9d643708515855e81ac6729843388a690b2c45
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2020
ms.locfileid: "83700949"
---
# <a name="create-a-consulting-service-offer"></a>Een adviesservice-aanbieding maken

In dit artikel wordt beschreven hoe u een advies service publiceert naar [Microsoft AppSource](https://appsource.microsoft.com/) of [Azure Marketplace](https://azuremarketplace.microsoft.com/). Lijst met advies services die zijn gebaseerd op micro soft [Dynamics 365](https://dynamics.microsoft.com/) en het Power platform op AppSource. Vermeld de aanbiedingen voor advies Services op basis van Microsoft Azure op Azure Marketplace. Voordat u begint, moet u [een commercieel Marketplace-account maken in Partner Center](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account) als u dit nog niet hebt gedaan. Zorg ervoor dat uw account is inge schreven in het Commercial Marketplace-programma.

Voordat u uw aanbieding maakt, controleert u de vereisten in de [consulting service-vereisten](https://docs.microsoft.com/azure/marketplace/partner-center-portal/consulting-service-prerequisites).

## <a name="publishing-benefits"></a>Publicatie voordelen

Voor delen van het publiceren naar de commerciële Marketplace:

- Promoot uw bedrijf met behulp van het merk micro soft.
- Mogelijk bereikt u meer dan 100.000.000 Office 365-en Dynamics 365-gebruikers op AppSource en meer dan 200.000 organisaties via Azure Marketplace.
- Ontvang leads van hoge kwaliteit van deze markt plaatsen.
- Laat uw services door het veld en de Televerkoop teams van micro soft worden gepromoveerd

## <a name="create-a-new-offer"></a>Een nieuwe aanbieding maken

1. Meld u aan bij [Partner Center](https://partner.microsoft.com/dashboard/home).
2. Selecteer in het menu links de optie **commerciële Marketplace**-  >  **overzicht**.
3. Selecteer op de pagina overzicht **+ nieuwe aanbieding**  >  **Consulting Service**.

    ![Illustreert het navigatie menu.](./media/new-offer-consulting-service.png)

>[!NOTE]
>Nadat een aanbieding is gepubliceerd, worden wijzigingen die in het partner centrum zijn aangebracht, alleen weer gegeven in de winkel wanneer u de aanbieding opnieuw publiceert. Zorg ervoor dat u altijd opnieuw publiceert nadat u wijzigingen hebt aangebracht.

## <a name="new-offer"></a>Nieuwe aanbieding

Voer een **aanbiedings-id**in. Dit is een unieke id voor elke aanbieding in uw account.

- Deze ID is zichtbaar voor klanten in het webadres voor de Marketplace-aanbieding.
- Gebruik alleen kleine letters en cijfers. Dit kan afbreek streepjes en onderstrepings tekens bevatten, maar mag niet langer zijn dan 50. Als u bijvoorbeeld **test-aanbieding-1**invoert, is het webadres van de aanbieding `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1` .
- De aanbiedings-ID kan niet worden gewijzigd nadat u **maken**hebt geselecteerd.

Voer een **alias**voor de aanbieding in. Dit is de naam die wordt gebruikt voor de aanbieding in Partner Center.

- Deze naam wordt niet gebruikt in Marketplace en wijkt af van de naam van de aanbieding en andere waarden die aan klanten worden weer gegeven.
- De aanbiedings alias kan niet worden gewijzigd nadat u **maken**hebt geselecteerd.

Selecteer **maken** om de aanbieding te genereren en door te gaan.

## <a name="offer-setup"></a>Installatie van aanbieding

### <a name="connect-lead-management"></a>Lead beheer verbinden

Wanneer u uw aanbieding naar Marketplace publiceert met het partner centrum, _moet_ u deze verbinden met een CRM-of marketing-automatiserings systeem. Zo kunt u contact gegevens van klanten ontvangen zodra iemand interesse in of gebruikt voor uw product.

1. Selecteer **verbinden** om op te geven waar u leads van klanten wilt verzenden. Het partner Centrum ondersteunt de volgende systemen:

    - [Dynamics 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics) voor klant betrokkenheid
    - [Marketo](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-marketo)
    - [Salesforce](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce)

    > [!NOTE]
    > Als uw CRM-systeem niet hierboven wordt vermeld, gebruikt u [Azure Table](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table) of [https-eind punt](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-https) om klant lead gegevens op te slaan en exporteert u de gegevens vervolgens naar uw CRM-systeem.

2. Verbind uw aanbieding met de doel locatie van de lead wanneer u publiceert in het partner centrum.
3. Controleer of de verbinding met de doel locatie juist is geconfigureerd. Nadat u het in het partner centrum hebt gepubliceerd, valideren we de verbinding en sturen we u een test lead. Wanneer u een voor beeld van de aanbieding bekijkt voordat deze live gaat, kunt u ook uw lead verbinding testen door zelf te proberen de aanbieding aan te schaffen in de preview-omgeving.
4. Zorg ervoor dat de verbinding met de doel locatie van de lead bijgewerkt blijft, zodat u geen leads kwijtraakt.

Hier volgen enkele aanvullende bronnen voor het beheer van leads:

- [Overzicht Lead beheer](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-get-customer-leads)
- [Veelgestelde vragen over leadmanagement](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Algemene leadconfiguratiefouten](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#publishing-config-errors)
- [Overzicht Lead beheer](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf) PDF (zorg ervoor dat de pop-upblokkering is uitgeschakeld)

Selecteer **concept opslaan** voordat u doorgaat.

### <a name="properties"></a>Eigenschappen

Op deze pagina kunt u het primaire product instellen dat door uw consulting service wordt aanbevolen, een advies service type instellen en van toepassing zijnde producten kiezen.

1. Selecteer een **primair product** in de vervolg keuzelijst.
2. Selecteer een **advies service type** in de vervolg keuzelijst:

    - **Beoordeling**: een evaluatie van de omgeving van een klant om de toepas baarheid van een oplossing te bepalen en een schatting te maken van de kosten en timing.
    - Meer **informatie**: een inleiding tot een oplossing of een advies service om de interesse van klanten te tekenen met behulp van frameworks, demo's en klant voorbeelden.
    - **Implementatie**: een volledige installatie die resulteert in een volledig werkende oplossing. Beperken tot oplossingen die in twee weken of minder kunnen worden geïmplementeerd.
    - **Testen van concept**: een implementatie met een beperkt bereik om te bepalen of een oplossing voldoet aan de vereisten van de klant.
    - **Workshop**: een interactieve betrokkenheid die wordt uitgevoerd op de lokale locatie van een klant. Dit kan betrekking hebben op trainingen, korte informatie, beoordelingen of demo's die zijn gebaseerd op de gegevens of omgeving van de klant.

3. Als u een primair product van **Azure**hebt geselecteerd, selecteert u Maxi maal drie **oplossings gebieden**. Dit maakt het gemakkelijker voor klanten in azure Marketplace om uw aanbieding te vinden. Als u Azure niet hebt gekozen, slaat u deze stap over.
4. Als u een _ander_ primair product dan Azure hebt geselecteerd, selecteert u Maxi maal drie **toepasselijke producten**. Zo kunnen klanten in AppSource gemakkelijker uw aanbieding vinden. Zie [Microsoft AppSourceing Consulting Service List guideline](https://go.microsoft.com/fwlink/?LinkId=828734&amp;clcid=0x409) (PDF) (Engelstalig) voor meer informatie.
5. Selecteer Maxi maal zes **branches** waarop uw aanbieding van toepassing is. Dit maakt het voor klanten gemakkelijker om uw aanbieding te vinden.
6. Voeg Maxi maal drie **competenties** toe die uw bedrijf heeft behaald om weer te geven in uw aanbieding voor advies service. Er is ten minste één competentie vereist, met uitzonde ring van Azure expert Msp's en Azure Networking Msp's.

Selecteer **concept opslaan** voordat u doorgaat.

## <a name="offer-listing"></a>Aanbieding weer geven

Hier definieert u de details van het aanbod dat op Marketplace wordt weer gegeven. Dit omvat de naam van de aanbieding, beschrijving, installatie kopieën, enzovoort. Zorg ervoor dat u het beleid volgt dat wordt beschreven op de [pagina beleid voor commerciële Marketplace-certificerings](https://docs.microsoft.com/legal/marketplace/certification-policies#800-consulting-services) instantie tijdens het configureren van deze aanbieding.

> [!NOTE]
> De details van de aanbieding hoeven niet in het Engels te staan als de beschrijving van de aanbieding met de woord groep begint, &quot; is deze toepassing alleen beschikbaar in [niet-Engelse taal]. &quot; Het is ook handig om een nuttige koppeling te bieden om inhoud te bieden in een andere taal dan die wordt gebruikt in de details van de aanbieding.

### <a name="name"></a>Name

De naam die u hier opgeeft, wordt weer gegeven als de titel van uw aanbieding. Dit veld is vooraf ingevuld met de tekst die u hebt ingevoerd in het vak **aanbiedings alias** wanneer u de aanbieding hebt gemaakt. U kunt deze naam later wijzigen.

De naam:

- Kan worden gemerkt (en u kunt symbolen van het handels merk of copyright bevatten).
- Mag niet langer zijn dan 50 tekens.
- Kan geen emojis bevatten.

### <a name="search-results-summary"></a>Samen vatting van zoek resultaten

Geef een korte beschrijving van uw aanbieding. Dit kan Maxi maal 100 tekens lang zijn en wordt gebruikt in Zoek resultaten voor Marketplace.

### <a name="description"></a>Beschrijving

[!INCLUDE [Long description-1](./includes/long-description-1.md)]

[!INCLUDE [Long description-2](./includes/long-description-2.md)]

[!INCLUDE [Long description-3](./includes/long-description-3.md)]

### <a name="keywords"></a>Trefwoorden

Voer Maxi maal drie Zoek trefwoorden in die relevant zijn voor uw primaire product en advies service. Zo kunt u gemakkelijker uw aanbieding vinden.

### <a name="duration"></a>Duur

Stel de verwachte duur van deze afspraak in op uw klant.

### <a name="contact-information"></a>Contactgegevens

U moet de naam, het e-mail adres en het telefoon nummer opgeven voor een **primaire** en **secundaire contact persoon**. Deze informatie wordt niet weer gegeven aan klanten. Het is beschikbaar voor micro soft en kan worden meegeleverd met de Cloud Solution Provider (CSP)-partners.

### <a name="supporting-documents"></a>Ondersteunende documenten

Voeg Maxi maal drie (maar ten minste één) PDF-documenten voor uw aanbieding toe.

### <a name="marketplace-images"></a>Marketplace-installatie kopieën

Bied logo's en installatie kopieën die u kunt gebruiken met uw aanbieding. Alle installatie kopieën moeten de PNG-indeling hebben. Vage afbeeldingen worden geweigerd.

>[!Note]
>Als u een probleem hebt met het uploaden van bestanden, moet u ervoor zorgen dat uw lokale netwerk de door https://upload.xboxlive.com Partner Center gebruikte service niet blokkeert.

#### <a name="store-logos"></a>Winkel logo's

Bied PNG-bestanden van het logo van uw aanbieding in elk van de volgende pixel grootten:

- **Klein (48 x 48)**
- **Groot (216 x 216)**

Alle logo's zijn vereist en worden op verschillende plaatsen in de Marketplace-vermelding gebruikt.

#### <a name="screenshots-optional"></a>Scherm afbeeldingen (optioneel)

Voeg Maxi maal vijf scherm opnamen toe die laten zien hoe uw aanbieding werkt. Elk moet 1280 x 720 pixels groot en in PNG-indeling zijn.

#### <a name="videos-optional"></a>Video's (optioneel)

Voeg Maxi maal vier Video's toe die uw aanbieding aantonen. Voer de naam van de video, het webadres (URL) en een miniatuur afbeelding van de video in op 1280 x 720 pixels.

Selecteer **concept opslaan** voordat u doorgaat.

## <a name="pricing-and-availability"></a>Prijzen en beschikbaarheid

Hier definieert u elementen zoals prijzen, de markt en een persoonlijke sleutel.

1. **Market**: Stel de markt in die uw aanbieding beschikbaar is in. U mag slechts één markt per aanbieding selecteren.
    1. Voor de Verenigde Staten of Canada-markten selecteert u **statussen** (of **provincies**) bewerken om aan te geven waar uw aanbieding beschikbaar is.
2. **Preview doel groep**: Configureer de **sleutel** die wordt gebruikt om de persoonlijke doel groep voor uw aanbieding in te stellen.
3. **Prijzen**: Geef op of uw aanbieding een **gratis** of een **betaalde** aanbieding is.

    > [!NOTE]
    > Aanbiedingen voor advies Services gelden alleen voor de aanbieding. Trans acties worden direct uitgevoerd buiten de commerciële Marketplace.

4. Voor een betaalde aanbieding geeft u de **prijs en valuta** op en bepaalt u of de prijs **vast** of **geraamd**is. Als u een schatting maakt, moet u in de beschrijving opgeven welke factoren van invloed zijn op de prijs.
5. Selecteer **concept opslaan** voordat u doorgaat.

## <a name="review-and-publish"></a>Controleren en publiceren

Nadat u alle vereiste gedeelten van de aanbieding hebt voltooid, kunt u uw aanbieding verzenden om deze te controleren en te publiceren.

1. Wanneer u klaar bent om uw advies service-aanbieding te publiceren, klikt u op **controleren en publiceren**.
2. Bekijk de details op de pagina definitieve verzen ding.
3. Als dat nodig is, schrijft u een opmerking naar het certificerings team als u denkt dat de details van uw aanbieding uitleg vereisen.
4. Wanneer u klaar bent, selecteert u **verzenden**.
5. Op de **overzichts** pagina van de aanbieding ziet u in welke publicatie fase uw aanbieding zich bevindt.

Zie [de publicatie status van uw aanbieding voor commerciële Marketplace controleren](https://docs.microsoft.com/azure/marketplace/partner-center-portal/publishing-status)voor meer informatie over hoe lang uw aanbieding in elke publicatie fase kan worden verwacht.

## <a name="update-your-existing-consulting-service-offers"></a>De aanbiedingen van uw bestaande advies service bijwerken

- [Een bestaande aanbieding bijwerken in de commerciële Marketplace](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer)
