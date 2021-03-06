---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows
author: rothja
manager: craigg
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/30/2018
ms.author: jroth
ms.custom: include file
ms.openlocfilehash: 2c7d312910c6d38c54b291da34bfb827246c7dad
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "79504302"
---
## <a name="prepare-for-akv-integration"></a>Voorbereiden op Azure-integratie
Als u Azure Key Vault-integratie wilt gebruiken om uw SQL Server-VM te configureren, zijn er verschillende vereisten: 

1. [Azure Power Shell installeren](#install)
2. [Een Azure Active Directory maken](#register)
3. [Een sleutel kluis maken](#createkeyvault)

In de volgende secties worden deze vereisten beschreven en de informatie die u moet verzamelen om later de Power shell-cmdlets uit te voeren.

[!INCLUDE [updated-for-az](./updated-for-az.md)]

### <a name="install-azure-powershell"></a><a id="install"></a>Azure PowerShell installeren
Zorg ervoor dat u de nieuwste Azure PowerShell-module hebt geïnstalleerd. Zie [Azure PowerShell installeren en configureren](/powershell/azure/install-az-ps) voor meer informatie.

### <a name="register-an-application-in-your-azure-active-directory"></a><a id="register"></a>Een toepassing registreren in uw Azure Active Directory

Eerst moet u een [Azure Active Directory](https://azure.microsoft.com/trial/get-started-active-directory/) (Aad) hebben in uw abonnement. In veel voor delen kunt u hiermee toestemming verlenen aan de sleutel kluis voor bepaalde gebruikers en toepassingen.

Registreer vervolgens een toepassing met AAD. Hiermee krijgt u een Service-Principal-account dat toegang heeft tot uw sleutel kluis, die uw VM nodig heeft. In het Azure Key Vault-artikel vindt u deze stappen in de sectie [een toepassing registreren met Azure Active Directory](../articles/key-vault/key-vault-manage-with-cli2.md#registering-an-application-with-azure-active-directory) , of u kunt de stappen in de **sectie een identiteit ophalen voor de toepassing** in [dit blog bericht](https://blogs.technet.com/b/kv/archive/2015/01/09/azure-key-vault-step-by-step.aspx)zien met scherm afbeeldingen. Voordat u deze stappen uitvoert, moet u tijdens deze registratie de volgende informatie verzamelen die u later nodig hebt wanneer u Azure Key Vault integratie op uw virtuele machine van SQL inschakelt.

* Nadat de toepassing is toegevoegd, zoekt u de **toepassings-id** (ook wel Aad ClientID of AppID genoemd) op de Blade **geregistreerde app** .
    De toepassings-ID wordt later toegewezen aan de para meter **$spName** (Service Principal Name) in het Power shell-script om Azure Key Vault-integratie in te scha kelen.

   ![Toepassings-id](./media/virtual-machines-sql-server-akv-prepare/aad-application-id.png)

* Wanneer u de sleutel maakt, kopieert u het geheim voor uw sleutel, zoals wordt weer gegeven in de volgende scherm afbeelding. Dit sleutel geheim wordt later toegewezen aan de para meter **$spSecret** (Service-Principal Secret) in het Power shell-script.

   ![AAD-geheim](./media/virtual-machines-sql-server-akv-prepare/aad-sp-secret.png)

* De toepassings-ID en het geheim worden ook gebruikt voor het maken van een referentie in SQL Server.

* U moet deze nieuwe toepassings-ID (of client-ID) toestemming geven voor de volgende toegangs machtigingen: **Get**, **wrapKey**, **sleutel uitpakken**. Dit wordt gedaan met de cmdlet [set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) . Zie [Azure Key Vault Overview](../articles/key-vault/key-vault-overview.md)voor meer informatie.

### <a name="create-a-key-vault"></a><a id="createkeyvault"></a>Een sleutel kluis maken
Als u Azure Key Vault wilt gebruiken om de sleutels op te slaan die u wilt gebruiken voor versleuteling in uw VM, moet u toegang hebben tot een sleutel kluis. Als u uw sleutel kluis nog niet hebt ingesteld, maakt u er een door de stappen te volgen in het artikel aan de slag [met Azure Key Vault](../articles/key-vault/key-vault-overview.md) . Voordat u deze stappen uitvoert, moet u tijdens deze installatie enige informatie verzamelen die u later nodig hebt wanneer u Azure Key Vault integratie op uw virtuele machine van SQL inschakelt.

    New-AzKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Wanneer u de stap een sleutel kluis maken ziet, noteert u de eigenschap geretourneerde **vaultUri** , de URL van de sleutel kluis. In het voor beeld in deze stap, zoals hieronder weer gegeven, is de naam van de sleutel kluis ContosoKeyVault. Daarom zou de https://contosokeyvault.vault.azure.net/sleutel kluis-URL zijn.

De sleutel kluis-URL wordt later toegewezen aan de para meter **$akvURL** in het Power shell-script om Azure Key Vault-integratie in te scha kelen.

Nadat de sleutel kluis is gemaakt, moeten we een sleutel toevoegen aan de sleutel kluis. deze sleutel wordt ook wel genoemd als u later een asymmetrische sleutel maakt in SQL Server.
