---
title: 'Problemen oplossen met een Azure site-naar-site-VPN-verbinding die geen verbinding kan maken met titleSuffix: Azure VPN Gateway'
description: Informatie over het oplossen van problemen met een site-naar-site-VPN-verbinding die plotseling niet meer werkt en niet opnieuw verbinding kan maken.
services: vpn-gateway
author: chadmath
ms.service: vpn-gateway
ms.topic: troubleshooting
ms.date: 09/16/2019
ms.author: genli
ms.openlocfilehash: 4e827c5f6eedc819bc3635cb09a28f65df51312c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "75862574"
---
# <a name="troubleshooting-an-azure-site-to-site-vpn-connection-cannot-connect-and-stops-working"></a>Problemen oplossen: een Azure site-naar-site-VPN-verbinding kan geen verbinding maken en werkt niet meer

Nadat u een site-naar-site-VPN-verbinding tussen een on-premises netwerk en een virtueel Azure-netwerk hebt geconfigureerd, wordt de VPN-verbinding plotseling gestopt en kan deze niet meer worden aangesloten. Dit artikel bevat stappen voor probleem oplossing om dit probleem op te lossen. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing

Om het probleem op te lossen, probeert u eerst [de Azure VPN-gateway opnieuw](vpn-gateway-resetgw-classic.md) in te stellen en de tunnel opnieuw in te stellen vanaf het on-PREMISES VPN-apparaat. Als het probleem zich blijft voordoen, voert u de volgende stappen uit om de oorzaak van het probleem te achterhalen.

### <a name="prerequisite-step"></a>Vereiste stap

Controleer het type van de Azure VPN-gateway.

1. Ga naar de [Azure Portal](https://portal.azure.com).

2. Controleer de **overzichts** pagina van de VPN-gateway op de type-informatie.
    
    ![Overzicht van de gateway](media/vpn-gateway-troubleshoot-site-to-site-cannot-connect/gatewayoverview.png)

### <a name="step-1-check-whether-the-on-premises-vpn-device-is-validated"></a>Step 1. Controleren of het on-premises VPN-apparaat is gevalideerd

1. Controleer of u een [gevalideerd VPN-apparaat en de versie van het besturings systeem](vpn-gateway-about-vpn-devices.md#devicetable)gebruikt. Als het apparaat geen gevalideerd VPN-apparaat is, moet u mogelijk contact opnemen met de fabrikant van het apparaat om te controleren of er een probleem is met de compatibiliteit.

2. Controleer of het VPN-apparaat correct is geconfigureerd. Zie apparaatconfiguratie-voor [beelden van apparaten bewerken](vpn-gateway-about-vpn-devices.md#editing)voor meer informatie.

### <a name="step-2-verify-the-shared-key"></a>Stap 2. De gedeelde sleutel controleren

Vergelijk de gedeelde sleutel voor het on-premises VPN-apparaat met de Azure Virtual Network VPN om te controleren of de sleutels overeenkomen. 

Gebruik een van de volgende methoden om de gedeelde sleutel voor de Azure VPN-verbinding weer te geven:

**Azure Portal**

1. Ga naar de site-naar-site-verbinding van de VPN-gateway die u hebt gemaakt.

2. Klik in de sectie **instellingen** op **gedeelde sleutel**.
    
    ![Gedeelde sleutel](media/vpn-gateway-troubleshoot-site-to-site-cannot-connect/sharedkey.png)

**Azure PowerShell**

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Voor het Azure Resource Manager implementatie model:

    Get-AzVirtualNetworkGatewayConnectionSharedKey -Name <Connection name> -ResourceGroupName <Resource group name>

Voor het klassieke implementatie model:

    Get-AzureVNetGatewayKey -VNetName -LocalNetworkSiteName

### <a name="step-3-verify-the-vpn-peer-ips"></a>Stap 3. De VPN peer Ip's controleren

-   De IP-definitie in het **lokale netwerk gateway** object in azure moet overeenkomen met het on-premises IP-adres van het apparaat.
-   De IP-definitie van de Azure-gateway die is ingesteld op het on-premises apparaat moet overeenkomen met het IP-adres van de Azure-gateway.

### <a name="step-4-check-udr-and-nsgs-on-the-gateway-subnet"></a>Stap 4. Controleer UDR en Nsg's op het gateway-subnet

Controleer of door de gebruiker gedefinieerde route ring (UDR) of netwerk beveiligings groepen (Nsg's) op het gateway-subnet worden verwijderd en het resultaat te testen. Als het probleem is opgelost, controleert u de instellingen die door UDR of NSG zijn toegepast.

### <a name="step-5-check-the-on-premises-vpn-device-external-interface-address"></a>Stap 5. Het externe interface adres van het on-premises VPN-apparaat controleren

- Als het Internet gerichte IP-adres van het VPN-apparaat is opgenomen in de **lokale netwerk** definitie in azure, kunt u problemen ondervinden met de verbinding.
- De externe interface van het apparaat moet rechtstreeks op internet zijn. Er mag zich geen Network Address Translation of firewall tussen internet en het apparaat bevinden.
- Als u Firewall clusters wilt configureren voor een virtueel IP-adres, moet u het cluster verbreekt en het VPN-apparaat rechtstreeks beschikbaar maken op een open bare interface waarmee de gateway een interface kan maken.

### <a name="step-6-verify-that-the-subnets-match-exactly-azure-policy-based-gateways"></a>Stap 6. Controleren of de subnetten exact overeenkomen (op Azure Policy-gebaseerde gateways)

-   Controleer of de adres ruimte van het virtuele netwerk precies overeenkomt tussen het virtuele netwerk van Azure en de on-premises definities.
-   Controleer of de subnetten exact overeenkomen tussen de **lokale netwerk gateway** en de on-premises definities voor het on-premises netwerk.

### <a name="step-7-verify-the-azure-gateway-health-probe"></a>Stap 7. De status test van de Azure-Gateway controleren

1. Open de status test door te bladeren naar de volgende URL:

    `https://<YourVirtualNetworkGatewayIP>:8081/healthprobe`

2. Klik op de certificaat waarschuwing.
3. Als u een antwoord ontvangt, wordt de VPN-gateway als gezond beschouwd. Als u geen antwoord ontvangt, is de gateway mogelijk niet in orde of wordt het probleem veroorzaakt door een NSG op het subnet van de gateway. De volgende tekst is een voorbeeld antwoord:

    ```xml
    <?xml version="1.0"?>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Primary Instance: GatewayTenantWorker_IN_1 GatewayTenantVersion: 14.7.24.6</string>
    ```

### <a name="step-8-check-whether-the-on-premises-vpn-device-has-the-perfect-forward-secrecy-feature-enabled"></a>Stap 8. Controleer of het on-premises VPN-apparaat de perfecte functie voor Forward Secrecy heeft ingeschakeld

De perfecte functie Forward Secrecy kan problemen met de verbinding veroorzaken. Als het VPN-apparaat Perfect Forward Secrecy is ingeschakeld, schakelt u de functie uit. Werk vervolgens het IPsec-beleid van de VPN-gateway bij.

## <a name="next-steps"></a>Volgende stappen

-   [Een site-naar-site-verbinding met een virtueel netwerk configureren](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
-   [Een IPsec/IKE-beleid configureren voor site-naar-site-VPN-verbindingen](vpn-gateway-ipsecikepolicy-rm-powershell.md)
