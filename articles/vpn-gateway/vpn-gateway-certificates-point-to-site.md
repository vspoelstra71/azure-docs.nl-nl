---
title: 'Certificaten genereren en exporteren voor P2S: Power shell'
titleSuffix: Azure VPN Gateway
description: Maak een zelfondertekend basis certificaat, Exporteer de open bare sleutel en Genereer client certificaten met behulp van Power shell op Windows 10 of Windows Server 2016.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: cherylmc
ms.openlocfilehash: f28e76e9dcaf1331fd26a2321cd4deca1027e693
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "79279336"
---
# <a name="generate-and-export-certificates-for-point-to-site-using-powershell"></a>Certificaten voor punt-naar-site genereren en exporteren met Power shell

Punt-naar-site-verbindingen gebruiken certificaten voor verificatie. In dit artikel wordt beschreven hoe u een zelfondertekend basis certificaat maakt en client certificaten genereert met behulp van Power shell op Windows 10 of Windows Server 2016. Zie [certificaten-Linux](vpn-gateway-certificates-point-to-site-linux.md) of [certificaten-makecert](vpn-gateway-certificates-point-to-site-makecert.md)als u andere certificaat instructies zoekt.

U moet de stappen in dit artikel uitvoeren op een computer met Windows 10 of Windows Server 2016. De Power shell-cmdlets die u gebruikt om certificaten te genereren, maken deel uit van het besturings systeem en werken niet op andere versies van Windows. De computer met Windows 10 of Windows Server 2016 is alleen nodig voor het genereren van de certificaten. Zodra de certificaten zijn gegenereerd, kunt u deze uploaden of installeren op elk ondersteund client besturingssysteem.

Als u geen toegang hebt tot een computer met Windows 10 of Windows Server 2016, kunt u [makecert](vpn-gateway-certificates-point-to-site-makecert.md) gebruiken om certificaten te genereren. De certificaten die u met een van beide methoden genereert, kunnen worden geïnstalleerd op elk [ondersteund](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) client besturingssysteem.

[!INCLUDE [generate and export certificates](../../includes/vpn-gateway-generate-export-certificates-include.md)]

## <a name="install-an-exported-client-certificate"></a><a name="install"></a>Een geëxporteerd clientcertificaat installeren

Voor elke client die verbinding maakt met het VNet via een P2S-verbinding, moet een client certificaat lokaal worden geïnstalleerd.

Als u een client certificaat wilt installeren, raadpleegt u [een client certificaat installeren voor punt-naar-site-verbindingen](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="next-steps"></a>Volgende stappen

Ga door met uw punt-naar-site-configuratie.

* Zie [P2S configureren met behulp van systeem eigen Azure-certificaat verificatie](vpn-gateway-howto-point-to-site-resource-manager-portal.md)voor de stappen van het **Resource Manager** -implementatie model.
* Zie [een punt-naar-site-VPN-verbinding naar een VNet (klassiek) configureren](vpn-gateway-howto-point-to-site-classic-azure-portal.md)voor de stappen van het **klassieke** implementatie model.