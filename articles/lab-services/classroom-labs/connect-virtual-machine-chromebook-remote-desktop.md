---
title: Verbinding maken met een Azure Lab Services VM vanuit Chromebook | Microsoft Docs
description: Meer informatie over hoe u verbinding maakt tussen een Chromebook en een virtuele machine in Azure Lab Services.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2020
ms.author: nicolela
ms.openlocfilehash: 581ba32f536728fec88ddf3120637f9347e2e925
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2020
ms.locfileid: "83704469"
---
# <a name="connect-to-a-vm-using-remote-desktop-protocol-on-a-chromebook"></a>Verbinding maken met een virtuele machine met behulp van Remote Desktop Protocol op een Chromebook
In deze sectie wordt uitgelegd hoe een student met behulp van RDP verbinding kan maken met een leslokaal-VM op basis van een Chromebook.

## <a name="install-microsoft-remote-desktop-on-a-chromebook"></a>Microsoft Extern bureaublad installeren op een Chromebook
1. Open de App Store op uw Chromebook en zoek naar **Microsoft extern bureaublad**.

    ![Microsoft Extern bureaublad](../media/how-to-use-classroom-lab/install-ms-remote-desktop-chromebook.png)
1. Installeer de meest recente versie van Microsoft Extern bureaublad. 

## <a name="access-the-vm-from-your-chromebook-using-rdp"></a>Toegang tot de virtuele machine vanuit uw Chromebook met RDP
1. Open het **RDP** -bestand dat op uw computer is gedownload met **Microsoft extern bureaublad** geïnstalleerd. Het moet beginnen met het maken van verbinding met de virtuele machine. 

    ![Verbinding maken met de virtuele machine](../media/how-to-use-classroom-lab/connect-vm-chromebook.png)

1. Voer uw wacht woord in als u hierom wordt gevraagd.
    ![Verbinding maken met de VM](../media/how-to-use-classroom-lab/password-chromebook.png)


1. Selecteer **door gaan** als u de volgende waarschuwing ontvangt. 

    ![Certificaat waarschuwing](../media/how-to-use-classroom-lab/certificate-error-chromebook.png)

1. U ziet nu het bureau blad van de virtuele machine waarmee u verbinding maakt.

## <a name="next-steps"></a>Volgende stappen
Zie [verbinding maken met virtuele Linux-machines](how-to-use-remote-desktop-linux-student.md) voor meer informatie over het maken van verbinding met Linux-vm's.


