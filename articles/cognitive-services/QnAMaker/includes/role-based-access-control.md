---
title: bestand opnemen
description: bestand opnemen
ms.topic: include
ms.custom: include file
ms.date: 03/11/2020
ms.openlocfilehash: b34d3ed2481600cde7ffb3275d635b8a57926bd6
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/19/2020
ms.locfileid: "83665947"
---
De volgende rollen zijn aanwezig voor samen werking:

|Rol|Functionaliteiten|API-toegang|API-machtigingen|
|--|--|--|--|
|Eigenaar|Alles|Verificatiesleutel|Alles|
|Inzender|Alles behalve mogelijkheid om nieuwe leden toe te voegen aan rollen|Verificatiesleutel|Alles behalve mogelijkheid om nieuwe leden toe te voegen aan rollen|
|QnA Maker lezen<br>wie|Exporteren/downloaden<br>Test|Bearer-token|1. KB-API downloaden<br>2. lijst Kb's voor gebruikers-API<br>3. Details van de Knowledge Base ophalen<br>4. wijzigingen downloaden<br>Antwoord genereren |
|QnA Maker editor<br>(lezen/schrijven)|Exporteren/downloaden<br>Test<br>KB bijwerken<br>KB exporteren<br>KB importeren<br>KB vervangen<br>KB maken|Bearer-token|1. KB-API maken<br>2. KB-API bijwerken<br>3. de KB-API vervangen<br>4. wijzigingen vervangen<br>5. "Train API" [in New Service model V5]|
|Gebruiker van de cognitieve service<br>(lezen/schrijven/publiceren)|Alles|Bearer-token|Alles|