---
title: Firewall regels voor Azure Event Hubs | Microsoft Docs
description: Gebruik firewall regels om verbindingen van specifieke IP-adressen toe te staan aan Azure Event Hubs.
services: event-hubs
documentationcenter: ''
author: spelluru
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.custom: seodec18
ms.topic: article
ms.date: 12/20/2019
ms.author: spelluru
ms.openlocfilehash: 2d67ad70ccdf4ce1f88401806700f38bd1d3c11d
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/19/2020
ms.locfileid: "83655882"
---
# <a name="configure-ip-firewall-rules-for-an-azure-event-hubs-namespace"></a>IP-firewall regels configureren voor een Azure Event Hubs-naam ruimte
Event Hubs naam ruimten zijn standaard toegankelijk vanuit Internet zolang de aanvraag een geldige verificatie en autorisatie heeft. Met IP-firewall kunt u dit nog verder beperken tot een aantal IPv4-adressen of IPv4-adresbereiken in CIDR-notatie [(Classless Inter-Domain Routing)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) .

Deze functie is handig in scenario's waarin Azure Event Hubs alleen toegankelijk moet zijn vanaf bepaalde bekende sites. Met firewall regels kunt u regels configureren voor het accepteren van verkeer dat afkomstig is van specifieke IPv4-adressen. Als u bijvoorbeeld Event Hubs met [Azure Express route][express-route]gebruikt, kunt u een **firewall regel** maken om alleen verkeer toe te staan van uw on-premises IP-adressen van de infra structuur. 

>[!WARNING]
> Het inschakelen van IP-filtering kan verhinderen dat andere Azure-Services communiceren met Event Hubs.
>
> Vertrouwde micro soft-services worden niet ondersteund wanneer virtuele netwerken zijn geïmplementeerd.
>
> Algemene scenario's voor Azure die niet met virtuele netwerken werken (Houd er rekening mee dat de lijst **niet** volledig is)-
> - Azure Monitor (diagnostische instelling)
> - Azure Stream Analytics
> - Integratie met Azure Event Grid
> - Azure-IoT Hub routes
> - Azure IoT-Device Explorer
>
> De volgende micro soft-services moeten zich in een virtueel netwerk bevinden
> - Azure Web Apps
> - Azure Functions


## <a name="ip-firewall-rules"></a>IP-firewall regels
De IP-firewall regels worden toegepast op het niveau van de Event Hubs naam ruimte. Daarom gelden de regels voor alle verbindingen van clients die gebruikmaken van elk ondersteund protocol. Een verbindings poging van een IP-adres dat niet overeenkomt met een toegestane IP-regel op de Event Hubs naam ruimte, wordt geweigerd als niet-geautoriseerd. De IP-regel wordt niet vermeld in het antwoord. IP-filter regels worden in volg orde toegepast en de eerste regel die overeenkomt met het IP-adres, bepaalt de accepteren of afwijzen.

## <a name="use-azure-portal"></a>Azure Portal gebruiken
In deze sectie wordt beschreven hoe u de Azure Portal gebruikt om IP-firewall regels voor een Event Hubs naam ruimte te maken. 

1. Navigeer naar uw **Event hubs-naam ruimte** in de [Azure Portal](https://portal.azure.com).
2. Selecteer in het linkermenu **netwerk** optie. Als u de optie **alle netwerken** selecteert, accepteert de Event hub verbindingen van elk IP-adres. Deze instelling komt overeen met een regel die het IP-adres bereik 0.0.0.0/0 accepteert. 

    ![Optie Firewall: alle netwerken geselecteerd](./media/event-hubs-firewall/firewall-all-networks-selected.png)
1. Selecteer de optie **geselecteerde netwerken** om de toegang tot specifieke netwerken en IP-adressen te beperken. Voer de volgende stappen uit in de sectie **firewall** :
    1. Selecteer **de optie uw IP-adres voor client toevoegen** om uw huidige client-IP de toegang tot de naam ruimte te geven. 
    2. Voer bij **adres bereik**een specifiek IPv4-adres of een bereik van IPv4-adres in CIDR-notatie in. 
    3. Geef op of u wilt **toestaan dat vertrouwde micro soft-services deze firewall overs Laan**. 

        > [!WARNING]
        > Als u de optie **geselecteerde netwerken** kiest en geen IP-adres of adres bereik opgeeft, is verkeer van alle netwerken toegestaan door de service. 

        ![Optie Firewall: alle netwerken geselecteerd](./media/event-hubs-firewall/firewall-selected-networks-trusted-access-disabled.png)
3. Selecteer **Opslaan** op de werk balk om de instellingen op te slaan. Wacht een paar minuten totdat de bevestiging op de portal meldingen wordt weer gegeven.


## <a name="use-resource-manager-template"></a>Resource Manager-sjabloon gebruiken

> [!IMPORTANT]
> Firewall regels worden ondersteund in de **standaard** -en **toegewezen** lagen van Event hubs. Deze worden niet ondersteund in de Basic-laag.

Met de volgende Resource Manager-sjabloon kunt u een IP-filter regel toevoegen aan een bestaande Event Hubs naam ruimte.

Sjabloon parameters:

- **ipMask** is een enkel IPv4-adres of een blok met IP-adressen in CIDR-notatie. Bijvoorbeeld, in CIDR-notatie 70.37.104.0/24 staat voor de IPv4-adressen 256 van 70.37.104.0 naar 70.37.104.255, met 24 waarmee het aantal belang rijke voorvoegsel bits voor het bereik wordt aangegeven.

> [!NOTE]
> Hoewel er geen regels kunnen worden geweigerd, is voor de Azure Resource Manager sjabloon de standaard actie ingesteld op **' toestaan '** , waardoor verbindingen niet worden beperkt.
> Wanneer u Virtual Network of firewall regels maakt, moeten we de ***' defaultAction '*** wijzigen
> 
> from
> ```json
> "defaultAction": "Allow"
> ```
> tot
> ```json
> "defaultAction": "Deny"
> ```
>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "eventhubNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Event Hubs namespace"
        }
      },
      "location": {
        "type": "string",
        "metadata": {
          "description": "Location for Namespace"
        }
      }
    },
    "variables": {
      "namespaceNetworkRuleSetName": "[concat(parameters('eventhubNamespaceName'), concat('/', 'default'))]",
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('eventhubNamespaceName')]",
        "type": "Microsoft.EventHub/namespaces",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Standard",
          "tier": "Standard"
        },
        "properties": { }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.EventHub/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.EventHub/namespaces/', parameters('eventhubNamespaceName'))]"
        ],
        "properties": {
          "virtualNetworkRules": [<YOUR EXISTING VIRTUAL NETWORK RULES>],
          "ipRules": 
          [
            {
                "ipMask":"10.1.1.1",
                "action":"Allow"
            },
            {
                "ipMask":"11.0.0.0/24",
                "action":"Allow"
            }
          ],
          "trustedServiceAccessEnabled": false,
          "defaultAction": "Deny"
        }
      }
    ],
    "outputs": { }
  }
```

Als u de sjabloon wilt implementeren, volgt u de instructies voor [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Volgende stappen

Zie de volgende koppeling voor meer informatie over het beperken van toegang tot Event Hubs voor virtuele netwerken van Azure:

- [Service-eind punten Virtual Network voor Event Hubs][lnk-vnet]

<!-- Links -->

[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
[lnk-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[lnk-vnet]: event-hubs-service-endpoints.md
