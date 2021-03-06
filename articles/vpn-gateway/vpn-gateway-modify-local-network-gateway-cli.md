---
title: 'VPN Gateway: Wijzig de IP-adres instellingen van de gateway: Azure CLI'
description: In dit artikel wordt stapsgewijs beschreven hoe u IP-adres voorvoegsels voor uw lokale netwerk gateway kunt wijzigen met behulp van de Azure CLI.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 11/29/2017
ms.author: cherylmc
ms.openlocfilehash: bc051a7e0a19dc54431266cfa5f37131868bdc07
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "75864040"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-cli"></a>Instellingen voor lokale netwerk gateway wijzigen met behulp van Azure CLI

Soms worden de instellingen voor het adres voorvoegsel of het IP-adres van de gateway van het lokale netwerk gewijzigd. In dit artikel wordt beschreven hoe u de instellingen van uw lokale netwerk gateway wijzigt. U kunt deze instellingen ook met behulp van een andere methode wijzigen door een andere optie te selecteren in de volgende lijst:

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure-CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <a name="before-you-begin"></a><a name="before"></a>Voordat u begint

Installeer de meest recente versie van de CLI-opdrachten (2,0 of hoger). Zie [Azure CLI 2.0 installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) voor meer informatie over het installeren van de CLI-opdrachten.

[!INCLUDE [CLI-login](../../includes/vpn-gateway-cli-login-include.md)]

## <a name="modify-ip-address-prefixes"></a><a name="ipaddprefix"></a>IP-adres voorvoegsels wijzigen

[!INCLUDE [modify-prefix](../../includes/vpn-gateway-modify-ip-prefix-cli-include.md)]

## <a name="modify-the-gateway-ip-address"></a><a name="gwip"></a>Het IP-adres van de gateway wijzigen

[!INCLUDE [modify-gateway-IP](../../includes/vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

## <a name="next-steps"></a>Volgende stappen

U kunt uw gateway verbinding controleren. Zie [een gateway verbinding controleren](vpn-gateway-verify-connection-resource-manager.md).

