---
title: PowerShell-voorbeelden
description: Zoek Azure PowerShell-voor beelden voor een aantal algemene App Service scenario's. Meer informatie over het automatiseren van uw App Service-implementatie-of beheer taken.
tags: azure-service-management
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.topic: sample
ms.date: 03/08/2017
ms.custom: mvc
ms.openlocfilehash: f289bd453f2387282402394c807fe9700151f221
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "81532422"
---
# <a name="powershell-samples-for-azure-app-service"></a>PowerShell-voorbeelden voor Azure App Service

De volgende tabel bevat koppelingen naar PowerShell-scripts die zijn gemaakt met behulp van Azure PowerShell.

| | |
|-|-|
|**App maken**||
| [Een app maken met implementatie vanuit GitHub](./scripts/powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een App Service-app die code ophaalt van GitHub. |
| [Een app maken met continue implementatie vanuit GitHub](./scripts/powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een App Service-app die doorlopend code implementeert vanuit GitHub. |
| [Een app maken en code implementeren via FTP](./scripts/powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een App Service-app en uploadt u bestanden uit een lokale map met behulp van FTP. |
| [Een app maken en code implementeren vanuit een lokale Git-opslagplaats](./scripts/powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een App Service-app en configureert u het pushen van code vanuit een lokale Git-opslagplaats. |
| [Een app maken en code implementeren in een faseringsomgeving](./scripts/powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een App Service-app met een implementatiesite voor het faseren van codewijzigingen. |
|**App configureren**||
| [Een aangepast domein toewijzen aan een app](./scripts/powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een App Service-app en wijst u er een aangepaste domeinnaam aan toe. |
| [Een aangepast TLS/SSL-certificaat binden aan een app](./scripts/powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een App Service-app en verbindt u het TLS/SSL-certificaat van een aangepaste domein naam. |
|**App schalen**||
| [Een app handmatig schalen](./scripts/powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een App Service-app en schaalt u deze over twee exemplaren. |
| [Een app wereldwijd schalen met een architectuur voor hoge beschikbaarheid](./scripts/powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u twee App Service-apps in twee verschillende geografische regio's en maakt u deze met behulp van Azure Traffic Manager beschikbaar via één eindpunt. |
|**App verbinden met resources**||
| [Een app verbinden met een SQL-database](./scripts/powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een App Service-app en een SQL-database, waarna u de verbindingsreeks van de database toevoegt aan de app-instellingen. |
| [Een app verbinden met een opslagaccount](./scripts/powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee maakt u een App Service-app en een opslagaccount, waarna u de verbindingsreeks van de opslag toevoegt aan de app-instellingen. |
|**Back-up van app maken en terugzetten**||
| [Een back-up maken van een app](./scripts/powershell-backup-onetime.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een App Service-app en maakt u er een eenmalige back-up van. |
| [Een geplande back-up maken voor een app](./scripts/powershell-backup-scheduled.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een App Service-app en stelt u een geplande back-up in voor de app. |
| [Een back-up van een app verwijderen](./scripts/powershell-backup-delete.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee verwijdert u een bestaande back-up voor een app. |
| [Een app terugzetten vanuit de back-up](./scripts/powershell-backup-restore.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee zet u een app terug vanuit een eerder voltooide back-up. |
| [Een back-up terugzetten binnen abonnementen](./scripts/powershell-backup-restore-diff-sub.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee zet u een web-app terug vanuit een back-up in een ander abonnement. |
|**App controleren**||
| [Een app bewaken met webserverlogboeken](./scripts/powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee maakt u een App Service-app, schakelt u logboekregistratie voor de app in en downloadt u de logboeken naar uw lokale computer. |
| | |
