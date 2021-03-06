---
title: Webhook-abonnee verificatie configureren-Azure Event Grid IoT Edge | Microsoft Docs
description: Verificatie van webhook-abonnee configureren
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/06/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 101dcae5870322878cec48098f2efae32cc68c14
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "76841727"
---
# <a name="configure-webhook-subscriber-authentication"></a>Verificatie van webhook-abonnee configureren

Deze hand leiding bevat voor beelden van de mogelijke configuratie van de webhook-abonnee voor een Event Grid module. Standaard worden alleen HTTPS-eind punten geaccepteerd voor webhook-abonnees. De module Event Grid wordt geweigerd als de abonnee een zelfondertekend certificaat presenteert.

## <a name="allow-only-https-subscriber"></a>Alleen HTTPS-abonnee toestaan

```json
 {
  "Env": [
    "outbound__webhook__httpsOnly=true",
    "outbound__webhook__skipServerCertValidation=false",
    "outbound__webhook__allowUnknownCA=false"
  ]
}
 ```

## <a name="allow-https-subscriber-with-self-signed-certificate"></a>HTTPS-abonnee met zelfondertekend certificaat toestaan

```json
 {
  "Env": [
    "outbound__webhook__httpsOnly=true",
    "outbound__webhook__skipServerCertValidation=false",
    "outbound__webhook__allowUnknownCA=true"
  ]
}
 ```

>[!NOTE]
>Stel de eigenschap `outbound__webhook__allowUnknownCA` in `true` op alleen in test omgevingen, omdat u meestal zelfondertekende certificaten kunt gebruiken. Voor werk belastingen voor productie raden wij aan dat ze worden ingesteld op **Onwaar**.

## <a name="allow-https-subscriber-but-skip-certificate-validation"></a>HTTPS-abonnee toestaan, maar certificaat validatie overs Laan

```json
 {
  "Env": [
    "outbound__webhook__httpsOnly=true",
    "outbound__webhook__skipServerCertValidation=true",
    "outbound__webhook__allowUnknownCA=false"
  ]
}
 ```

>[!NOTE]
>Stel de eigenschap `outbound__webhook__skipServerCertValidation` `true` alleen in op test omgevingen als u mogelijk geen certificaat presenteert dat moet worden geverifieerd. Voor werk belastingen voor productie raden wij aan dat ze worden ingesteld op **Onwaar** .

## <a name="allow-both-http-and-https-with-self-signed-certificates"></a>Zowel HTTP als HTTPS toestaan met zelfondertekende certificaten

```json
 {
  "Env": [
    "outbound__webhook__httpsOnly=false",
    "outbound__webhook__skipServerCertValidation=false",
    "outbound__webhook__allowUnknownCA=true"
  ]
}
 ```

>[!NOTE]
>Stel de eigenschap `outbound__webhook__httpsOnly` `false` alleen in op test omgevingen als u mogelijk eerst een http-abonnee wilt maken. Voor werk belastingen voor productie raden wij aan dat deze worden ingesteld op **waar**
