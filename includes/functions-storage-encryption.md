---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 0bc5977a581006dc760c0435f9d68371ced7e4cd
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/19/2020
ms.locfileid: "83648778"
---
Azure Storage versleutelt alle gegevens in een opslag account in rust. Zie [Azure Storage versleuteling voor Data-at-rest](../articles/storage/common/storage-service-encryption.md)voor meer informatie.

Standaard worden gegevens versleuteld met door micro soft beheerde sleutels. Voor extra controle over versleutelings sleutels kunt u door de klant beheerde sleutels leveren die worden gebruikt voor het versleutelen van BLOB-en bestands gegevens. Deze sleutels moeten aanwezig zijn in Azure Key Vault om functies te kunnen gebruiken om toegang te krijgen tot het opslag account. Zie [versleuteling op rest met door de klant beheerde sleutels](../articles/azure-functions/configure-encrypt-at-rest-using-cmk.md)voor meer informatie.  