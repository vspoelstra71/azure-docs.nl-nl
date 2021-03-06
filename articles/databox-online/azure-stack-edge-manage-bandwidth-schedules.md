---
title: Azure Stack rand bandbreedte schema's beheren | Microsoft Docs
description: Hierin wordt beschreven hoe u de Azure Portal gebruikt voor het beheren van bandbreedte schema's op uw Azure Stack rand.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/22/2019
ms.author: alkohli
ms.openlocfilehash: a21828413b8d8750827fafa077002863439f8643
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "82569587"
---
# <a name="use-the-azure-portal-to-manage-bandwidth-schedules-on-your-azure-stack-edge"></a>Gebruik de Azure Portal om bandbreedte planningen te beheren op uw Azure Stack rand  

In dit artikel wordt beschreven hoe u gebruikers op uw Azure Stack Edge beheert. Met bandbreedteschema's kunt u het gebruik van netwerkbandbreedte beheren over schema's voor meerdere tijdstippen. Deze schema's kunnen worden toegepast op upload- en downloadbewerkingen van uw apparaat naar de cloud.

U kunt de bandbreedte schema's voor uw Azure Stack rand toevoegen, wijzigen of verwijderen via de Azure Portal.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een schema toevoegen
> * Een schema wijzigen
> * Een schema verwijderen


## <a name="add-a-schedule"></a>Een schema toevoegen

Voer de volgende stappen uit in de Azure Portal om een schema toe te voegen.

1. Ga in de Azure Portal voor uw Azure Stack Edge-resource naar **band breedte**.
2. Selecteer **+ schema toevoegen**in het rechterdeel venster.

    ![Band breedte selecteren](media/azure-stack-edge-manage-bandwidth-schedules/add-schedule-1.png)

3. Doe het volgende in **Schema toevoegen**: 

   1. Geef de **Eerste dag**, **Laatste dag**, **Begintijd** en **Eindtijd** van de planning op.
   2. Controleer de optie **alle dagen** als dit schema de hele dag moet worden uitgevoerd.
   3. **Bandbreedtesnelheid** is de bandbreedte in Megabits per seconde (Mbps) die door uw apparaat wordt gebruikt bij bewerkingen die betrekking hebben op de cloud (uploaden en downloaden). Geef voor dit veld een getal tussen 20 en 1.000.000.007 op.
   4. Schakel **Onbeperkte** bandbreedte in als u de datumupload en -download niet wilt regelen.
   5. Selecteer **Toevoegen**.

      ![Schema toevoegen](media/azure-stack-edge-manage-bandwidth-schedules/add-schedule-2.png)

3. Er wordt een schema gemaakt met de opgegeven parameters. Dit schema wordt vervolgens weergegeven in de lijst van bandbreedteschema's in de portal.

    ![Lijst met bandbreedte schema's bijgewerkt](media/azure-stack-edge-manage-bandwidth-schedules/add-schedule-3.png)

## <a name="edit-schedule"></a>Schema bewerken

Voer de volgende stappen uit als u een bandbreedteschema wilt bewerken.

1. Ga in het Azure Portal naar de resource Azure Stack Edge en ga vervolgens naar **band breedte**. 
2. Selecteer en selecteer een schema dat u wilt wijzigen in de lijst met bandbreedte schema's.
    ![Bandbreedte planning selecteren](media/azure-stack-edge-manage-bandwidth-schedules/modify-schedule-1.png)

3. Breng de gewenste wijzigingen aan en sla de wijzigingen op.

    ![Gebruiker wijzigen](media/azure-stack-edge-manage-bandwidth-schedules/modify-schedule-2.png)

4. Wanneer het schema is gewijzigd, wordt de lijst met schema's bijgewerkt met het gewijzigde schema.

    ![Gebruiker wijzigen](media/azure-stack-edge-manage-bandwidth-schedules/modify-schedule-3.png)


## <a name="delete-a-schedule"></a>Een schema verwijderen

Voer de volgende stappen uit om een bandbreedte schema te verwijderen dat is gekoppeld aan uw Azure Stack edge-apparaat.

1. Ga in het Azure Portal naar de resource Azure Stack Edge en ga vervolgens naar **band breedte**.  

2. Selecteer in de lijst met bandbreedteschema's een schema dat u wilt verwijderen. Selecteer **verwijderen**in het **schema bewerken**. Selecteer **Ja**als u om bevestiging wordt gevraagd.

   ![Een gebruiker verwijderen](media/azure-stack-edge-manage-bandwidth-schedules/delete-schedule-2.png)

3. Wanneer de planning is verwijderd, wordt de lijst met schema's bijgewerkt.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [beheren van shares](azure-stack-edge-manage-shares.md).
