---
title: Learning-beleid-Personaler
description: Leer instellingen bepalen de *Hyper parameters* van de model training. Twee modellen van dezelfde gegevens die worden getraind op verschillende leer instellingen, worden uiteindelijk afwijkend.
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: abe6a2a2ec9b9978230d894c69193469f6e932e6
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "79219344"
---
# <a name="learning-policy-and-settings"></a>Leer beleid en-instellingen

Leer instellingen bepalen de *Hyper parameters* van de model training. Twee modellen van dezelfde gegevens die worden getraind op verschillende leer instellingen, worden uiteindelijk afwijkend.

Het [trainings beleid en de instellingen](how-to-settings.md#configure-rewards-for-the-feedback-loop) zijn ingesteld op uw personaler-resource in de Azure Portal.

## <a name="import-and-export-learning-policies"></a>Leer beleid importeren en exporteren

U kunt leer beleids bestanden importeren en exporteren vanuit het Azure Portal. Gebruik deze methode om bestaande beleids regels op te slaan, te testen, te vervangen en te archiveren in uw broncode beheer als artefacten voor toekomstige Naslag informatie en controle.

Meer informatie [over](how-to-manage-model.md#import-a-new-learning-policy) het importeren en exporteren van een leer beleid in de Azure portal voor uw persoonlijke resource.

## <a name="understand-learning-policy-settings"></a>Informatie over de instellingen van het leer beleid

De instellingen in het trainings beleid zijn niet bedoeld om te worden gewijzigd. Wijzig de instellingen alleen als u begrijpt hoe deze van invloed zijn op persoonlijkere. U kunt zonder deze kennis problemen veroorzaken, waaronder het ongeldig maken van Personaler-modellen.

Personaler gebruikt [vowpalwabbit](https://github.com/VowpalWabbit) om de gebeurtenissen te trainen en te beoordelen. Raadpleeg de [vowpalwabbit-documentatie](https://github.com/VowpalWabbit/vowpal_wabbit/wiki/Command-line-arguments) voor informatie over het bewerken van de leer instellingen met behulp van vowpalwabbit. Zodra u de juiste opdracht regel argumenten hebt, slaat u de opdracht op in een bestand met de volgende indeling (Vervang de waarde van de eigenschap arguments door de gewenste opdracht) en uploadt u het bestand om de leer instellingen te importeren in het deel venster **model-en leer instellingen** in het Azure portal voor uw persoonlijke resource.

Hier volgt `.json` een voor beeld van een leer beleid.

```json
{
  "name": "new learning settings",
  "arguments": " --cb_explore_adf --epsilon 0.2 --power_t 0 -l 0.001 --cb_type mtr -q ::"
}
```

## <a name="compare-learning-policies"></a>Leer beleid vergelijken

U kunt vergelijken hoe verschillende leer beleidsregels worden uitgevoerd op eerdere gegevens in persoonlijke logboeken door [offline-evaluaties](concepts-offline-evaluation.md)uit te voeren.

[Upload uw eigen trainings beleid](how-to-manage-model.md) om ze te vergelijken met het huidige leer beleid.

## <a name="optimize-learning-policies"></a>Leer beleid optimaliseren

Personaler kan een geoptimaliseerd leer beleid maken in een [offline-evaluatie](how-to-offline-evaluation.md). Een geoptimaliseerd leer beleid met betere voor delen in een offline-evaluatie levert betere resultaten op wanneer het online in Personaler wordt gebruikt.

Wanneer u een leer beleid optimaliseert, kunt u dit rechtstreeks op persoonlijker Toep assen zodat het huidige beleid wordt vervangen. Of u kunt het geoptimaliseerde beleid voor verdere evaluatie opslaan en later beslissen of u het wilt verwijderen, opslaan of Toep assen.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [actieve en inactieve gebeurtenissen](concept-active-inactive-events.md).
