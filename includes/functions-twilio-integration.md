---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 467e09f9bd46df6d888d82f2961c5aed9cca4ab5
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/27/2020
ms.locfileid: "67175836"
---
In dit voor beeld moet de [Twilio](https://www.twilio.com/) -service worden gebruikt om SMS-berichten te verzenden naar een mobiele telefoon. Azure Functions biedt al ondersteuning voor Twilio via de [Twilio-binding](https://docs.microsoft.com/azure/azure-functions/functions-bindings-twilio). het voor beeld gebruikt deze functie.

Het eerste wat u nodig hebt, is een Twilio-account. U kunt er gratis op https://www.twilio.com/try-twiliomaken. Nadat u een account hebt, voegt u de volgende drie **app-instellingen** toe aan de functie-app.

| Naam van app-instelling | Waarde beschrijving |
| - | - |
| **TwilioAccountSid**  | De SID voor uw Twilio-account |
| **TwilioAuthToken**   | Het verificatie token voor uw Twilio-account |
| **TwilioPhoneNumber** | Het telefoon nummer dat is gekoppeld aan uw Twilio-account. Dit wordt gebruikt om SMS-berichten te verzenden. |