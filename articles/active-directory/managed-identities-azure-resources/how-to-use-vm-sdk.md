---
title: Beheerde identiteiten gebruiken op een Azure VM met Azure Sdk's-Azure AD
description: Code voorbeelden voor het gebruik van Azure Sdk's met een Azure-VM met beheerde identiteiten voor Azure-resources.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 422b4c63a86904721fc6910193ffa63a8fe46f0a
ms.sourcegitcommit: b1e25a8a442656e98343463aca706f4fde629867
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/27/2020
ms.locfileid: "74184154"
---
# <a name="how-to-use-managed-identities-for-azure-resources-on-an-azure-vm-with-azure-sdks"></a>Beheerde identiteiten voor Azure-resources gebruiken op een Azure VM met Azure Sdk's 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
In dit artikel vindt u een lijst met SDK-voor beelden, die het gebruik van hun respectieve Azure SDK-ondersteuning voor beheerde identiteiten voor Azure-bronnen illustreren.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

> [!IMPORTANT]
> - In alle voorbeeld code/script in dit artikel wordt ervan uitgegaan dat de-client wordt uitgevoerd op een virtuele machine met beheerde identiteiten voor Azure-resources ingeschakeld. Gebruik de VM-functie ' Connect ' in het Azure Portal om extern verbinding te maken met uw VM. Zie [beheerde identiteiten voor Azure-resources configureren op een virtuele machine met behulp van de Azure Portal](qs-configure-portal-windows-vm.md), of een van de variant artikelen (met behulp van Power shell, CLI, een sjabloon of een Azure SDK), voor meer informatie over het inschakelen van beheerde identiteiten voor Azure-resources op een virtuele machine. 

## <a name="sdk-code-samples"></a>SDK-code voorbeelden

| SDK             | Codevoorbeeld |
| --------------- | ----------- |
| .NET            | [Een Azure Resource Manager-sjabloon implementeren vanaf een Windows-VM met beheerde identiteiten voor Azure-resources](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet) |
| .NET Core       | [Azure-Services aanroepen vanaf een Linux-VM met beheerde identiteiten voor Azure-resources](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/) |
| Node.js         | [Resources beheren met beheerde identiteiten voor Azure-resources](https://azure.microsoft.com/resources/samples/resources-node-manage-resources-with-msi/) |
| Python          | [Beheerde identiteiten gebruiken voor Azure-resources om simpelweg vanuit een VM te verifiëren](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [Resources van een virtuele machine beheren met beheerde identiteiten voor Azure-resources ingeschakeld](https://github.com/Azure-Samples/resources-ruby-manage-resources-with-msi/) |

## <a name="next-steps"></a>Volgende stappen

- Zie [Azure sdk's](https://azure.microsoft.com/downloads/) voor de volledige lijst met Azure SDK-resources, inclusief down loads voor bibliotheken, documentatie en meer.
- Zie [beheerde identiteiten voor Azure-resources configureren op een virtuele machine met behulp van de Azure Portal](qs-configure-portal-windows-vm.md)om beheerde identiteiten voor Azure-resources in te scha kelen op een Azure VM.








