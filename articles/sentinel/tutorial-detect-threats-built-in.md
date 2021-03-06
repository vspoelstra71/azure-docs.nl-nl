---
title: Waarschuwingen onderzoeken met Azure Sentinel | Microsoft Docs
description: Gebruik deze zelf studie om te leren hoe u waarschuwingen met Azure Sentinel kunt onderzoeken.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: df855aa768fdeb279482b8407259be1a644322d9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "77585200"
---
# <a name="tutorial-detect-threats-out-of-the-box"></a>Zelf studie: bedreigingen-out-of-Box detecteren


> [!IMPORTANT]
> Out-of-the-box-bedreigings detectie is momenteel beschikbaar als open bare preview.
> Deze functie wordt zonder service level agreement gegeven en wordt niet aanbevolen voor productie werkbelastingen.
> Zie voor meer informatie [aanvullende gebruiks voorwaarden voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Nadat u [uw gegevens bronnen hebt verbonden](quickstart-onboard.md)met Azure Sentinel, wilt u een melding ontvangen wanneer er iets verdacht is. Om dit mogelijk te maken, biedt Azure Sentinel u de beschikking over out-of-the-box-sjablonen. Deze sjablonen zijn ontworpen door het team van beveiligings experts en analisten van micro soft op basis van bekende bedreigingen, veelvoorkomende aanvals vectoren en escalatie ketens van verdachte activiteiten. Nadat deze sjablonen zijn ingeschakeld, zullen ze automatisch zoeken naar activiteiten die in uw omgeving verdacht zijn. Veel sjablonen kunnen worden aangepast om activiteiten op basis van uw behoeften te zoeken of uit te filteren. De waarschuwingen die door deze sjablonen worden gegenereerd, maken incidenten die u kunt toewijzen en onderzoeken in uw omgeving.

Deze zelf studie helpt u bij het detecteren van bedreigingen met Azure Sentinel:

> [!div class="checklist"]
> * Out-of-the-box-detecties gebruiken
> * Bedreigings reacties automatiseren

## <a name="about-out-of-the-box-detections"></a>Over standaarddetectie

Als u alle opstaande detecties wilt weer geven, gaat u naar **Analytics** en vervolgens op **regel sjablonen**. Dit tabblad bevat alle ingebouwde Azure Sentinel-regels.

   ![Ingebouwde detecties gebruiken om bedreigingen te vinden met Azure Sentinel](media/tutorial-detect-built-in/view-oob-detections.png)

De volgende sjabloon typen zijn beschikbaar:

- **Micro soft-beveiliging** : micro soft-beveiligings sjablonen maken automatisch Azure-Sentinel-incidenten op basis van de waarschuwingen die zijn gegenereerd in andere micro soft-beveiligings oplossingen, in realtime. U kunt micro soft-beveiligings regels als sjabloon gebruiken om nieuwe regels met soort gelijke logica te maken. Zie voor meer informatie over beveiligings regels [automatisch incidenten maken op basis van beveiligings waarschuwingen van micro soft](create-incidents-from-alerts.md).
- **Fusie** -op basis van fusie technologie, geavanceerde aanvals detectie in azure wordt gebruikt schaal bare machine learning algoritmen die een groot aantal waarschuwingen en gebeurtenissen op laag niveau kunnen correleren voor meerdere producten in een hoge mate van betrouw baarheid en bruikbare incidenten. Fusion is standaard ingeschakeld. Omdat de logica verborgen is, kunt u deze niet gebruiken als sjabloon voor het maken van meer dan een regel.
- **Machine learning-gedrags analyse** : deze sjablonen zijn gebaseerd op eigen micro soft machine learning-algoritmen, zodat u niet de interne logica kunt zien van hoe ze werken en wanneer ze worden uitgevoerd. Omdat de logica verborgen is, kunt u deze niet gebruiken als sjabloon voor het maken van meer dan een regel.
-   **Gepland** – geplande analytische regels zijn geplande query's die zijn geschreven door micro soft-beveiligings experts. U kunt de query logica bekijken en er wijzigingen in aanbrengen. U kunt geplande regels als sjabloon gebruiken om nieuwe regels met soort gelijke logica te maken.

## <a name="use-out-of-the-box-detections"></a>Out-of-the-box-detecties gebruiken

1. Als u een ingebouwde sjabloon wilt gebruiken, klikt u op **regel maken** om een nieuwe actieve regel te maken op basis van deze sjabloon. Elk item heeft een lijst met vereiste gegevens bronnen die automatisch worden gecontroleerd. Dit kan ertoe leiden dat het maken van de **regel** is uitgeschakeld.
  
   ![Ingebouwde detecties gebruiken om bedreigingen te vinden met Azure Sentinel](media/tutorial-detect-built-in/use-built-in-template.png)
 
1. Hiermee opent u de wizard regel maken op basis van de geselecteerde sjabloon. Alle details worden automatisch ingevuld en voor **geplande regels** of **micro soft-beveiligings regels**kunt u de logica aanpassen aan uw organisatie of extra regels maken op basis van de ingebouwde sjabloon. Nadat u de stappen in de wizard voor het maken van regels hebt uitgevoerd en een regel hebt gemaakt op basis van de sjabloon, wordt de nieuwe regel weer gegeven op het tabblad **actieve regels** .

Zie [zelf studie: aangepaste analytische regels maken voor het detecteren van verdachte bedreigingen](tutorial-detect-threats-custom.md)voor meer informatie over de velden in de wizard.



## <a name="next-steps"></a>Volgende stappen
In deze zelf studie hebt u geleerd hoe u aan de slag gaat met het detecteren van bedreigingen met behulp van Azure Sentinel. 

Als u wilt weten hoe u uw reacties op bedreigingen kunt automatiseren, [stelt u automatische reacties op dreigingen in azure Sentinel in](tutorial-respond-threats-playbook.md).

