---
title: Geforceerde tunneling Azure Firewall
description: U kunt geforceerde tunneling configureren om Internet-gebonden verkeer te routeren naar een extra firewall of een virtueel netwerk apparaat voor verdere verwerking.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 05/18/2020
ms.author: victorh
ms.openlocfilehash: cf0af93d95c5af56be6168bc8e4f79d3005e2ec2
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/19/2020
ms.locfileid: "83649595"
---
# <a name="azure-firewall-forced-tunneling"></a>Geforceerde tunneling Azure Firewall

Wanneer u een nieuw Azure Firewall configureert, kunt u alle Internet-gebonden verkeer routeren naar een aangewezen volgende hop in plaats van rechtstreeks naar Internet te gaan. Zo hebt u mogelijk een on-premises Edge-firewall of een ander virtueel netwerk apparaat (NVA) om netwerk verkeer te verwerken voordat het wordt door gegeven aan Internet. U kunt echter geen bestaande firewall configureren voor geforceerde tunneling.

Geforceerde tunneling is standaard niet toegestaan op Azure Firewall om ervoor te zorgen dat aan alle uitgaande Azure-afhankelijkheden wordt voldaan. UDR-configuraties (door de gebruiker gedefinieerde route) op de *AzureFirewallSubnet* die een standaard route hebben die niet rechtstreeks naar Internet gaat, worden uitgeschakeld.

## <a name="forced-tunneling-configuration"></a>Configuratie van geforceerde tunneling

Voor de ondersteuning van geforceerde tunneling wordt Service Management-verkeer gescheiden van het verkeer van de klant. Een extra toegewezen subnet met de naam *AzureFirewallManagementSubnet* (minimale subnet-grootte/26) is vereist met een eigen gekoppeld openbaar IP-adres. De enige route die op dit subnet is toegestaan, is een standaard route naar Internet en de doorgifte van BGP-routes moet worden uitgeschakeld.

Als u een standaard route hebt geadverteerd via BGP om verkeer naar de on-premises af te dwingen, moet u de *AzureFirewallSubnet* en *AzureFirewallManagementSubnet* maken voordat u uw firewall implementeert en een UDR met een standaard route naar het Internet hebt en de **gateway route doorgifte van het virtuele netwerk** is uitgeschakeld.

Binnen deze configuratie kan de *AzureFirewallSubnet* nu routes naar een on-premise firewall of NVA voor het verwerken van verkeer, voordat deze wordt door gegeven aan Internet. U kunt deze routes ook via BGP naar *AzureFirewallSubnet* publiceren als de **route doorgifte van de virtuele netwerk gateway** is ingeschakeld op dit subnet.

U kunt bijvoorbeeld een standaard route maken op het *AzureFirewallSubnet* met uw VPN-gateway als de volgende hop om naar uw on-premises apparaat te gaan. U kunt ook de **route doorgifte van de virtuele netwerk gateway** inschakelen om de juiste routes naar het on-premises netwerk op te halen.

![Route doorgifte van virtuele netwerk gateway](media/forced-tunneling/route-propagation.png)

Zodra u Azure Firewall configureert om geforceerde tunneling te ondersteunen, kunt u de configuratie niet ongedaan maken. Als u alle andere IP-configuraties van uw firewall verwijdert, wordt de beheer-IP-configuratie ook verwijderd en wordt de toewijzing van de Firewall ongedaan gemaakt. Het open bare IP-adres dat is toegewezen aan de beheer-IP-configuratie kan niet worden verwijderd, maar u kunt een ander openbaar IP-adres toewijzen.

## <a name="next-steps"></a>Volgende stappen

- [Zelf studie: Azure Firewall implementeren en configureren in een hybride netwerk met behulp van de Azure Portal](tutorial-hybrid-portal.md)
