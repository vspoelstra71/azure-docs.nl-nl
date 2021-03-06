---
title: Een SDK gebruiken om beheerde identiteiten op een virtuele machine te configureren-Azure AD
description: Stapsgewijze instructies voor het configureren en gebruiken van beheerde identiteiten voor Azure-resources op een Azure-VM met behulp van een Azure-SDK.
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
ms.date: 09/28/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: d11cd51984f82bc20c02669e796d9ba21b9ed5d7
ms.sourcegitcommit: b1e25a8a442656e98343463aca706f4fde629867
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/27/2020
ms.locfileid: "74183483"
---
# <a name="configure-a-vm-with-managed-identities-for-azure-resources-using-an-azure-sdk"></a>Een virtuele machine met beheerde identiteiten configureren voor Azure-resources met behulp van een Azure SDK

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Beheerde identiteiten voor Azure-resources biedt Azure-Services met een automatisch beheerde identiteit in Azure Active Directory (AD). U kunt deze identiteit gebruiken voor verificatie bij elke service die ondersteuning biedt voor Azure AD-verificatie, zonder dat u referenties hebt in uw code. 

In dit artikel vindt u informatie over het inschakelen en verwijderen van beheerde identiteiten voor Azure-resources voor een Azure-VM met behulp van een Azure-SDK.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

## <a name="azure-sdks-with-managed-identities-for-azure-resources-support"></a>Azure-Sdk's met beheerde identiteiten voor Azure-bronnen ondersteuning 

Azure ondersteunt meerdere programmeer platforms door middel van een reeks [Azure-sdk's](https://azure.microsoft.com/downloads). Er zijn verschillende van deze updates voor het ondersteunen van beheerde identiteiten voor Azure-resources en het leveren van de bijbehorende voor beelden om gebruik te demonstreren. Deze lijst wordt bijgewerkt als er aanvullende ondersteuning wordt toegevoegd:

| SDK | Voorbeeld |
| --- | ------ | 
| .NET   | [Resource beheren vanaf een VM die is ingeschakeld met beheerde identiteiten voor Azure-resources ingeschakeld](https://azure.microsoft.com/resources/samples/aad-dotnet-manage-resources-from-vm-with-msi/) |
| Java   | [Opslag beheren vanaf een virtuele machine die is ingeschakeld met beheerde identiteiten voor Azure-resources](https://azure.microsoft.com/resources/samples/compute-java-manage-resources-from-vm-with-msi-in-aad-group/)|
| Node.js| [Een virtuele machine maken waarop door het systeem toegewezen beheerde identiteit is ingeschakeld](https://azure.microsoft.com/resources/samples/compute-node-msi-vm/) |
| Python | [Een virtuele machine maken waarop door het systeem toegewezen beheerde identiteit is ingeschakeld](https://azure.microsoft.com/resources/samples/compute-python-msi-vm/) |
| Ruby   | [Een Azure-VM maken met een door het systeem toegewezen identiteit ingeschakeld](https://github.com/Azure-Samples/compute-ruby-msi-vm/) |

## <a name="next-steps"></a>Volgende stappen

- Zie Verwante artikelen onder **identiteit configureren voor een virtuele machine van Azure**voor meer informatie over hoe u ook de Azure Portal, Power shell, CLI en resource sjablonen kunt gebruiken.
