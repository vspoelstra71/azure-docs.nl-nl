---
title: Hoe reserveringskortingen van toepassing zijn op Azure Synapse Analytics | Microsoft Docs
description: Leer hoe reserveringskortingen van toepassing zijn op Azure Synapse Analytics om geld te besparen.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: banders
ms.openlocfilehash: 33a0a0a62814579dee10ae046338ff9f78d667cb
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2020
ms.locfileid: "82627072"
---
# <a name="how-reservation-discounts-apply-to-azure-synapse-analytics"></a>Hoe reserveringskortingen van toepassing zijn op Azure Synapse Analytics

Nadat u gereserveerde capaciteit voor Azure Synapse Analytics hebt gekocht, wordt de reserveringskorting automatisch toegepast op uw ingerichte exemplaren in die regio. De reserveringskorting geldt voor het gebruik dat door de cDWU-meter van Synapse Analytics wordt gegenereerd. Opslag en netwerkgebruik worden in rekening gebracht volgens betalen-per-gebruiktarieven.

## <a name="reservation-discount-application"></a>Toepassing van reserveringskorting

De korting voor gereserveerde capaciteit van Synapse Analytics wordt elk uur toegepast op warehouses die actief zijn. Als u een uur lang geen warehouse hebt geïmplementeerd, gaat de gereserveerde capaciteit voor dat uur verloren. Deze restcapaciteit kan niet worden overgedragen.

Na aankoop wordt de reservering die u aanschaft, vergeleken met het Synapse Analytics-gebruik dat door actieve warehouses op een bepaald moment wordt gegenereerd. Als u bepaalde warehouses afsluit, zijn de reserveringskortingen automatisch van toepassing op andere overeenkomende warehouses.

Voor warehouses die geen volledig uur worden uitgevoerd, wordt de reservering automatisch toegepast op andere overeenkomende instanties in dat uur.

## <a name="discount-examples"></a>Kortingsvoorbeelden

De volgende voorbeelden laten zien hoe de korting voor gereserveerde Azure Synapse Analytics-capaciteit wordt toegepast, afhankelijk van de implementaties.

- **Voorbeeld 1**: U koopt 5 eenheden van 100 cDWU gereserveerde capaciteit. U voert een DW1500c Azure Synapse Analytics-exemplaar een uur uit. In dit geval wordt het gebruik gegenereerd voor 15 eenheden van een gebruik van 100 cDWU. De reserveringskorting geldt voor de vijf eenheden die u hebt gebruikt. Er worden kosten in rekening gebracht op basis van betalen-per-gebruiktarieven voor de resterende 10 gebruikte eenheden van 100 cDWU-gebruik. Met andere woorden, gedeeltelijke dekking is mogelijk voor meerdere reserveringen.

- **Voorbeeld 2**: U koopt 5 eenheden van 100 cDWU gereserveerde capaciteit. U voert twee DW100c Azure Synapse Analytics-exemplaren een uur uit. In dit geval worden twee gebruiksgebeurtenissen gegenereerd voor 1 eenheid 100 cDWU-gebruik. Beide gebruiksgebeurtenissen krijgen kortingen voor gereserveerde capaciteit. De resterende 3 eenheden van 100 cDWU gereserveerde capaciteit gaan verloren en worden niet overgedragen voor toekomstig gebruik. Met andere woorden, een enkele reservering kan worden gekoppeld aan meerdere Synapse Analytics-exemplaren.

- **Voorbeeld 3**: U koopt 1 eenheid van 100 cDWU gereserveerde capaciteit. U voert twee DW100c Azure Synapse Analytics-exemplaren uit. Elk instantie wordt 30 minuten uitgevoerd. In dit geval krijgen beide gebruiksgebeurtenissen kortingen voor gereserveerde capaciteit. Er wordt geen gebruik in rekening gebracht op basis van betalen-per-gebruiktarieven.

## <a name="need-help-contact-us"></a>Hebt u hulp nodig? Contact opnemen

- Als u vragen hebt of hulp nodig hebt, [kunt u een ondersteuningsaanvraag maken](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen voor meer informatie over Azure-reserveringen:

- [Wat zijn Azure-reserveringen?](save-compute-costs-reservations.md)
- [Reserveringstransacties weergeven](view-reservations.md)
- [Reserveringstransacties en gebruik via API ophalen](reservation-apis.md)
- [Reserveringen beheren](manage-reserved-vm-instance.md)
