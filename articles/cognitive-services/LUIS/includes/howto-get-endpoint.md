---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: include file
ms.service: cognitive-services
ms.date: 05/06/2020
ms.subservice: language-understanding
ms.topic: include
ms.author: diberry
ms.openlocfilehash: 2a98e2a97a9154d9e256a4662bb292896c6d1e77
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588775"
---
Kopieer in het gedeelte **beheren** (in de rechter bovenhoek) op de pagina **Azure-resources** (menu links) de **voorbeeld query** -URL en plak deze in een nieuw browser tabblad.

De eind punt-URL ziet eruit als in de volgende notatie, met uw eigen aangepaste subdomein, App-ID en de sleutel van het eind punt, waarbij de APP-ID en de sleutel-ID worden vervangen:

```console
https://YOUR-CUSTOM-SUBDMAIN.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/production/predict?subscription-key=KEY-ID&verbose=true&show-all-intents=true&log=true&query=YOUR_QUERY_HERE
```