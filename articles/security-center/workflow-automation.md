---
title: Werk stroom automatisering in Azure Security Center | Microsoft Docs
description: Meer informatie over het maken en automatiseren van werk stromen in Azure Security Center
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 5d947cf41e13abdea9a2fd29f8a740d0c101dc6f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80397916"
---
# <a name="workflow-automation"></a>Werkstroomautomatisering

Elk beveiligings programma bevat meerdere werk stromen voor reactie op incidenten. Deze processen kunnen het melden van relevante belanghebbenden, het starten van een wijzigings beheer proces en het Toep assen van specifieke herbemiddelings stappen zijn. Beveiligings experts raden u aan zo veel mogelijk stappen van deze procedures te automatiseren. Automation vermindert de overhead. Het kan ook de beveiliging verbeteren door ervoor te zorgen dat de proces stappen snel, consistent en volgens uw vooraf gedefinieerde vereisten worden uitgevoerd.

In dit artikel wordt de functie werk stroom automatisering van Azure Security Center beschreven. Deze functie kan Logic Apps activeren over beveiligings waarschuwingen en aanbevelingen. U kunt bijvoorbeeld Security Center een e-mail bericht verzenden wanneer er een waarschuwing wordt weer gegeven. U leert ook hoe u Logic Apps maakt met behulp van [Azure Logic apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview).

> [!NOTE]
> Als u eerder de weer gave Playbooks (preview) op de zijbalk hebt gebruikt, kunt u dezelfde functies vinden in combi natie met de uitgebreide functionaliteit op de pagina nieuwe werk stroom automatisering.


## <a name="requirements"></a>Vereisten

* Als u met Azure Logic Apps werk stromen wilt werken, moet u over de volgende Logic Apps-rollen/-machtigingen beschikken:

    * De machtigingen voor de [logische app-operator](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#logic-app-operator) zijn vereist of de toegang tot de logische app wordt gelezen/geactiveerd (deze rol kan geen Logic apps maken of bewerken; alleen bestaande *uitvoeren* )

    * De Inzender machtigingen van de [logische app](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#logic-app-contributor) zijn vereist voor het maken en wijzigen van logische apps

* Als u logische app-connectors wilt gebruiken, hebt u mogelijk aanvullende referenties nodig om u aan te melden bij hun respectieve services (bijvoorbeeld uw exemplaren van Outlook/teams/toegestane vertraging)


## <a name="create-a-logic-app-and-define-when-it-should-automatically-run"></a>Een logische app maken en definiëren wanneer deze automatisch moet worden uitgevoerd 

1. Selecteer op de zijbalk van Security Center **werk stroom automatisering**.

    [![Lijst met werk stroom Automatiseringen](media/workflow-automation/list-of-workflow-automations.png)](media/workflow-automation/list-of-workflow-automations.png#lightbox)

    Op deze pagina kunt u nieuwe Automation-regels maken, maar ook bestaande voor het inschakelen, uitschakelen of verwijderen.  
1. Als u een nieuwe werk stroom wilt definiëren, klikt u op **werk stroom automatisering toevoegen**. 

    Er wordt een deel venster met de opties voor uw nieuwe automatisering weer gegeven. Hier kunt u het volgende invoeren:
    1. Een naam en beschrijving voor de automatisering.
    1. De triggers waarmee deze automatische werk stroom wordt gestart. U wilt bijvoorbeeld dat uw logische app wordt uitgevoerd wanneer er een beveiligings waarschuwing wordt gegenereerd die ' SQL ' bevat.
    1. De logische app die wordt uitgevoerd wanneer aan de voor waarden van de trigger wordt voldaan. 

        [![Lijst met werk stroom Automatiseringen](media/workflow-automation/add-workflow.png)](media/workflow-automation/add-workflow.png#lightbox)

1. Klik in de sectie acties op **een nieuw item maken** om het proces voor het maken van de logische app te starten.

    U wordt geAzure Logic Apps.

    [![Een nieuwe logische app maken](media/workflow-automation/logic-apps-create-new.png)](media/workflow-automation/logic-apps-create-new.png#lightbox)

1. Voer een naam, resource groep en locatie in en klik op **maken**.

1. In de nieuwe logische app kunt u kiezen uit ingebouwde, vooraf gedefinieerde sjablonen uit de beveiligings categorie. U kunt ook een aangepaste stroom van gebeurtenissen definiëren die moeten worden uitgevoerd wanneer dit proces wordt geactiveerd.

    In de ontwerp functie voor logische apps worden de volgende triggers van de Security Center-connectors ondersteund:

    * **Wanneer een Azure Security Center aanbeveling wordt gemaakt of geactiveerd**
    * **Wanneer een Azure Security Center waarschuwing wordt gemaakt of geactiveerd** 
    
    > [!TIP]
    > U kunt de trigger aanpassen zodat deze alleen betrekking heeft op waarschuwingen met de ernst niveaus die u interesseren.
    
    > [!NOTE]
    > Als u de verouderde trigger gebruikt wanneer een reactie op een Azure Security Center waarschuwing wordt geactiveerd, wordt uw Logic Apps niet gestart door de functie werk stroom automatisering. Gebruik in plaats daarvan een van de hierboven genoemde triggers. 

    [![Voor beeld van logische app](media/workflow-automation/sample-logic-app.png)](media/workflow-automation/sample-logic-app.png#lightbox)

1. Nadat u uw logische app hebt gedefinieerd, keert u terug naar het deel venster definitie van werk stroom automatisering ("werk stroom automatisering toevoegen"). Klik op **vernieuwen** om ervoor te zorgen dat de nieuwe logische app beschikbaar is voor selectie.

    ![Vernieuwen](media/workflow-automation/refresh-the-list-of-logic-apps.png)

1. Selecteer uw logische app en sla de automatisering op. Houd er rekening mee dat in de vervolg keuzelijst voor logische apps alleen Logic Apps worden weer gegeven met ondersteunende Security Center Connect oren die hierboven worden genoemd.


## <a name="manually-trigger-a-logic-app"></a>Een logische app hand matig activeren

U kunt Logic Apps ook hand matig uitvoeren bij het weer geven van een beveiligings waarschuwing of een aanbeveling met een [snelle herstel oplossing](https://docs.microsoft.com/azure/security-center/security-center-remediate-recommendations#quick-fix-remediation).

Als u een logische app hand matig wilt uitvoeren, opent u een waarschuwing of een aanbeveling die ondersteuning biedt voor het automatisch oplossen van herstel en klikt u op **logische app activeren**:

[![Een logische app hand matig activeren](media/workflow-automation/manually-trigger-logic-app.png)](media/workflow-automation/manually-trigger-logic-app.png#lightbox)

## <a name="data-types-schemas"></a>Gegevens type schema's

Als u de onbewerkte gebeurtenis schema's wilt bekijken van de beveiligings waarschuwingen of aanbevelingen die zijn door gegeven aan de logische app-instantie, gaat u naar de [schema gegevens typen werk stroom automatisering](https://aka.ms/ASCAutomationSchemas). Dit kan handig zijn in gevallen waarin u geen gebruik maakt van Security Center ingebouwde logische app-connectors die hierboven worden genoemd. in plaats daarvan kunt u het JSON-schema van de logische app gebruiken om dit hand matig te parseren.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u Logic Apps maakt, hoe u de uitvoering ervan automatiseert in Security Center en ze hand matig uitvoert. 

Zie voor andere verwante materialen: 

- [De module Microsoft Learn voor het automatiseren van een beveiligings antwoord met werk stroom automatisering](https://docs.microsoft.com/learn/modules/resolve-threats-with-azure-security-center/)
- [Aanbevelingen voor beveiliging in Azure Security Center](security-center-recommendations.md)
- [Beveiligingswaarschuwingen in Azure Security Center](security-center-alerts-overview.md)
- [Over Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview)
- [Logic Apps-connectors](https://docs.microsoft.com/connectors/)
- [Schema's van werk stroom automatiserings gegevens typen](https://aka.ms/ASCAutomationSchemas)
