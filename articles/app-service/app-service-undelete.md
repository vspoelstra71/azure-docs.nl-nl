---
title: Verwijderde apps herstellen
description: Meer informatie over het herstellen van een verwijderde app in Azure App Service. Vermijd een per ongeluk verwijderde app.
author: btardif
ms.author: byvinyal
ms.date: 9/23/2019
ms.topic: article
ms.openlocfilehash: d147c87e8fb0fa9e01ba2dbaf732adaae082bcd7
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2020
ms.locfileid: "84167788"
---
# <a name="restore-deleted-app-service-app-using-powershell"></a>Verwijderde App Service-apps herstellen met PowerShell

Als u uw app per ongeluk wilt verwijderen in Azure App Service, kunt u deze herstellen met behulp van de opdrachten uit de [AZ Power shell-module](https://docs.microsoft.com/powershell/azure/?view=azps-2.6.0&viewFallbackFrom=azps-2.2.0).

> [!NOTE]
> Verwijderde apps worden 30 dagen na de eerste verwijdering uit het systeem verwijderd. Zodra een app is leeg gemaakt, kan deze niet meer worden hersteld.
>

## <a name="re-register-app-service-resource-provider"></a>App Service Resource provider opnieuw registreren
Sommige klanten kunnen een probleem ondervinden waarbij het ophalen van de lijst met verwijderde apps mislukt. Voer de volgende opdracht uit om het probleem op te lossen:

```powershell
 Register-AzResourceProvider -ProviderNamespace "Microsoft.Web"
```

## <a name="list-deleted-apps"></a>Verwijderde apps weer geven

Als u de verzameling verwijderde Apps wilt ophalen, kunt u gebruiken `Get-AzDeletedWebApp` .

Voor meer informatie over een specifieke verwijderde app kunt u het volgende gebruiken:

```powershell
Get-AzDeletedWebApp -Name <your_deleted_app> -Location <your_deleted_app_location> 
```

De gedetailleerde informatie omvat:

- **DeletedSiteId**: de unieke id voor de app die wordt gebruikt voor scenario's waarbij meerdere apps met dezelfde naam zijn verwijderd
- **SubscriptionID**: abonnement met de verwijderde resource
- **Locatie**: locatie van de oorspronkelijke app
- **ResourceGroupName**: naam van de oorspronkelijke resource groep
- **Naam**: naam van de oorspronkelijke app.
- **Sleuf**: de naam van de sleuf.
- **Verwijderings tijd**: wanneer is de app verwijderd  

## <a name="restore-deleted-app"></a>Verwijderde app herstellen
>[!NOTE]
> `Restore-AzDeletedWebApp`wordt niet ondersteund voor functie-apps.

Zodra de app die u wilt herstellen is geïdentificeerd, kunt u deze herstellen met `Restore-AzDeletedWebApp` .

```powershell
Restore-AzDeletedWebApp -ResourceGroupName <my_rg> -Name <my_app> -TargetAppServicePlanName <my_asp>
```
> [!NOTE]
> Implementatie sites worden niet teruggezet als onderdeel van uw app. Als u een faserings sleuf moet herstellen, gebruikt u de `-Slot <slot-name>` vlag.
>

De invoer voor opdracht is:

- **Resource groep**: doel resource groep waar de app wordt teruggezet
- **Naam**: de naam voor de app moet wereld wijd uniek zijn.
- **TargetAppServicePlanName**: app service plan dat aan de app is gekoppeld

Standaard `Restore-AzDeletedWebApp` worden zowel de configuratie van uw app als alle inhoud hersteld. Als u alleen inhoud wilt herstellen, gebruikt u de `-RestoreContentOnly` vlag met deze commandlet.

> [!NOTE]
> Als de app is gehost op en vervolgens wordt verwijderd uit een App Service Environment, kan deze alleen worden hersteld als de bijbehorende App Service Environment nog bestaan.
>

U vindt hier de volledige naslag informatie over de commandlet: [Restore-AzDeletedWebApp](https://docs.microsoft.com/powershell/module/az.websites/restore-azdeletedwebapp).
