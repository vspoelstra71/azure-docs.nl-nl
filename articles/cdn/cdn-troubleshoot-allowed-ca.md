---
title: Toegestane CA voor het inschakelen van aangepaste HTTPS op Azure CDN
description: Als u uw eigen certificaat gebruikt om HTTPS in te scha kelen op een aangepast domein, moet u een toegestane certificerings instantie (CA) gebruiken om deze te maken.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2018
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: faf51dbb1f1c3c0346b1ae9104494538efcc2ee7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "81259967"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-cdn"></a>Toegestane certificerings instanties voor het inschakelen van aangepaste HTTPS op Azure CDN

U moet voldoen aan specifieke certificaat vereisten wanneer u [de https-functie inschakelt met behulp van uw eigen certificaat](cdn-custom-ssl.md?tabs=option-2-enable-https-with-your-own-certificate#tlsssl-certificates) voor een aangepast domein van Azure Content Delivery Network (CDN). Voor de **Azure CDN standaard van micro soft** -profiel is een certificaat van een van de goedgekeurde certificerings instanties (CA) in de volgende lijst vereist. Als een certificaat van een niet-goedgekeurde certificerings instantie of een zelfondertekend certificaat wordt gebruikt, wordt de aanvraag geweigerd. **Azure CDN standaard van Verizon** en **Azure CDN Premium van Verizon** -profielen accepteren een geldig certificaat van een geldige certificerings instantie.

> [!NOTE]
> De optie voor het gebruik van uw eigen certificaat om de HTTPS-functie van het aangepaste domein in te scha kelen is *niet* beschikbaar voor **Azure CDN standaard van Akamai** -profielen. 
>

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]

