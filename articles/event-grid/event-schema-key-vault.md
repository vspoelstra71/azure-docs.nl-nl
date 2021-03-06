---
title: Azure Key Vault als Event Grid bron
description: Hierin worden de eigenschappen en schema's beschreven die voor Azure Key Vault gebeurtenissen worden gegeven met Azure Event Grid
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: spelluru
ms.openlocfilehash: 40bff9585e64163039a8847ff868c982ffb20414
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "81458246"
---
# <a name="azure-key-vault-as-event-grid-source"></a>Azure Key Vault als Event Grid bron

In dit artikel vindt u de eigenschappen en het schema voor gebeurtenissen in [Azure Key Vault](../key-vault/index.yml)die momenteel als preview-versie beschikbaar zijn. Zie [Azure Event grid-gebeurtenis schema](event-schema.md)voor een inleiding tot gebeurtenis schema's.

## <a name="event-grid-event-schema"></a>Event Grid-gebeurtenisschema

### <a name="available-event-types"></a>Beschik bare gebeurtenis typen

Een Azure Key Vault-account genereert de volgende gebeurtenis typen:

| Volledige naam van de gebeurtenis | Weergave naam van gebeurtenis | Beschrijving |
| ---------- | ----------- |---|
| Micro soft. CertificateNewVersionCreated | De nieuwe versie van het certificaat is gemaakt | Wordt geactiveerd wanneer een nieuw certificaat of nieuwe certificaat versie wordt gemaakt. |
| Micro soft. CertificateNearExpiry | Certificaat bijna verlopen | Wordt geactiveerd wanneer de huidige versie van het certificaat bijna verloopt. (De gebeurtenis wordt geactiveerd 30 dagen voor de verval datum.) |
| Micro soft. CertificateExpired | Certificaat is verlopen | Wordt geactiveerd wanneer het certificaat is verlopen. |
| Micro soft. KeyNewVersionCreated | Nieuwe versie van de sleutel gemaakt | Wordt geactiveerd wanneer een nieuwe sleutel of nieuwe sleutel versie wordt gemaakt. |
| Micro soft. KeyNearExpiry | Sleutel bijna verlopen | Wordt geactiveerd wanneer de huidige versie van een sleutel bijna verloopt. (De gebeurtenis wordt geactiveerd 30 dagen voor de verval datum.) |
| Micro soft. de sleutel kluis. | De sleutel is verlopen | Wordt geactiveerd wanneer een sleutel is verlopen. |
| Micro soft. SecretNewVersionCreated | Nieuwe geheime versie gemaakt | Wordt geactiveerd wanneer een nieuw geheim of nieuwe geheime versie wordt gemaakt. |
| Micro soft. SecretNearExpiry | Geheim bijna verlopen | Wordt geactiveerd wanneer de huidige versie van een geheim verloopt. (De gebeurtenis wordt geactiveerd 30 dagen voor de verval datum.) |
| Micro soft. SecretExpired | Geheim verlopen | Wordt geactiveerd wanneer een geheim is verlopen. |

### <a name="event-examples"></a>Gebeurtenis voorbeelden

In het volgende voor beeld wordt schema voor **micro soft. SecretNewVersionCreated**weer gegeven:

```JSON
[
   {
      "id":"00eccf70-95a7-4e7c-8299-2eb17ee9ad64",
      "topic":"/subscriptions/{subscription-id}/resourceGroups/sample-rg/providers/Microsoft.KeyVault/vaults/sample-kv",
      "subject":"newsecret",
      "eventType":"Microsoft.KeyVault.SecretNewVersionCreated",
      "eventTime":"2019-07-25T01:08:33.1036736Z",
      "data":{
         "Id":"https://sample-kv.vault.azure.net/secrets/newsecret/ee059b2bb5bc48398a53b168c6cdcb10",
         "vaultName":"sample-kv",
         "objectType":"Secret",
         "objectName ":"newsecret",
         "version":" ee059b2bb5bc48398a53b168c6cdcb10",
         "nbf":"1559081980",
         "exp":"1559082102"
      },
      "dataVersion":"1",
      "metadataVersion":"1"
   }
]
```

### <a name="event-properties"></a>Gebeurtenis eigenschappen

Een gebeurtenis heeft de volgende gegevens op het hoogste niveau:

| Eigenschap | Type | Beschrijving |
| ---------- | ----------- |---|
| id | tekenreeks | De ID van het object dat deze gebeurtenis heeft geactiveerd |
| vaultName | tekenreeks | De naam van de sleutel kluis van het object dat deze gebeurtenis heeft geactiveerd |
| Later | tekenreeks | Het type van het object dat deze gebeurtenis heeft geactiveerd |
| objectName | tekenreeks | De naam van het object dat deze gebeurtenis heeft geactiveerd |
| versie | tekenreeks | De versie van het object dat deze gebeurtenis heeft geactiveerd |
| NBF | getal | De niet-voor-datum in seconden sinds 1970-01-01T00:00:00Z van het object dat deze gebeurtenis heeft geactiveerd |
| geldig | getal | De verval datum in seconden sinds 1970-01-01T00:00:00Z van het object dat deze gebeurtenis heeft geactiveerd |

## <a name="tutorials-and-how-tos"></a>Zelfstudies en handleidingen
|Titel  |Beschrijving  |
|---------|---------|
| [Key Vault gebeurtenissen bewaken met Azure Event Grid](../key-vault/general/event-grid-overview.md) | Overzicht van het integreren van Key Vault met Event Grid. |
| [Zelf studie: Key Vault gebeurtenissen maken en bewaken met Event Grid](../key-vault/general/event-grid-tutorial.md) | Meer informatie over het instellen van Event Grid meldingen voor Key Vault. |


## <a name="next-steps"></a>Volgende stappen

* Zie [Wat is Event grid?](overview.md)voor een inleiding tot Azure Event grid.
* Zie [Event grid-abonnements schema](subscription-creation-schema.md)voor meer informatie over het maken van een Azure Event grid-abonnement.
* Zie [Key Vault bewaken met Azure Event grid (preview)](../key-vault/general/event-grid-overview.md)voor meer informatie over de integratie van Key Vault met Event grid.
* Zie voor een zelf studie over Key Vault integratie met Event Grid de [sleutel kluis meldingen ontvangen en erop reageren met Azure Event grid (preview)](../key-vault/general/event-grid-tutorial.md).
* Zie voor aanvullende richt lijnen voor Key Vault en Azure Automation:
    - [Wat is Azure Sleutelkluis?](../key-vault/general/overview.md)
    - [Key Vault bewaken met Azure Event Grid (preview-versie)](../key-vault/general/event-grid-overview.md)
    - [Belang rijke kluis meldingen ontvangen en hierop reageren met Azure Event Grid (preview-versie)](../key-vault/general/event-grid-tutorial.md)
    - [Overzicht van Azure Automation](../automation/index.yml)
