---
title: Back-up inschakelen wanneer u een Azure-VM maakt
description: Hierin wordt beschreven hoe u back-ups inschakelt wanneer u een Azure VM maakt met Azure Backup.
ms.topic: conceptual
ms.date: 06/13/2019
ms.openlocfilehash: 7739109eb8bad88c9b723e67e13adc78c127499a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80672809"
---
# <a name="enable-backup-when-you-create-an-azure-vm"></a>Back-up inschakelen wanneer u een Azure-VM maakt

Gebruik de Azure Backup-service om een back-up te maken van Azure virtual machines (Vm's). Er wordt een back-up gemaakt van Vm's op basis van een schema dat is opgegeven in een back-upbeleid en herstel punten van back-ups. Herstel punten worden opgeslagen in Recovery Services kluizen.

In dit artikel vindt u informatie over het inschakelen van back-ups bij het maken van een virtuele machine (VM) in de Azure Portal.  

## <a name="before-you-start"></a>Voordat u begint

- [Controleer](backup-support-matrix-iaas.md#supported-backup-actions) welke besturings systemen worden ondersteund als u back-ups inschakelt bij het maken van een virtuele machine.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Als u nog niet bent aangemeld bij uw account, meldt u zich aan bij de [Azure Portal](https://portal.azure.com).

## <a name="create-a-vm-with-backup-configured"></a>Een virtuele machine maken waarvoor een back-up is geconfigureerd

1. Klik in Azure Portal op **een resource maken**.

2. Klik in de Azure Marketplace op **Compute**en selecteer vervolgens een VM-installatie kopie.

3. Stel de virtuele machine in volgens de instructies voor [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) of [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal) .

4. Klik op het tabblad **beheer** in **back-up inschakelen**op **aan**.
5. Azure Backup back-ups naar een Recovery Services kluis. Klik op **nieuwe maken** als u geen bestaande kluis hebt.
6. Accepteer de naam van de voorgestelde kluis of geef uw eigen op.
7. Geef op of maak een resource groep waarin de kluis zich bevindt. De kluis van de resource groep kan afwijken van de resource groep van de virtuele machine.

    ![Back-up voor een virtuele machine inschakelen](./media/backup-during-vm-creation/enable-backup.png)

8. Accepteer het standaard back-upbeleid of wijzig de instellingen.
    - In een back-upbeleid wordt aangegeven hoe vaak back-upmomentopnamen van de virtuele machine moeten worden gemaakt en hoe lang deze back-ups moeten worden bewaard.
    - Het standaard beleid maakt eenmaal per dag een back-up van de VM.
    - U kunt uw eigen back-upbeleid voor een Azure-VM aanpassen om dagelijks of wekelijks back-ups te maken.
    - Meer [informatie](backup-azure-vms-introduction.md#backup-and-restore-considerations) over back-upaandachtspunten voor Azure-vm's.
    - Meer [informatie](backup-instant-restore-capability.md) over de functionaliteit voor direct terugzetten.

      ![Standaard back-upbeleid](./media/backup-during-vm-creation/daily-policy.png)

## <a name="azure-backup-resource-group-for-virtual-machines"></a>Azure Backup resource groep voor Virtual Machines

Met de back-upservice wordt een afzonderlijke resource groep (RG) gemaakt, die afwijkt van de resource groep van de virtuele machine om de herstel punt verzameling (RPC) op te slaan. De RPC-huizen de onmiddellijke herstel punten van beheerde Vm's. De standaard naamgevings indeling van de resource groep die is gemaakt door de back `AzureBackupRG_<Geo>_<number>`-upservice is:. Bijvoorbeeld: *AzureBackupRG_northeurope_1*. U kunt nu de naam van de resource groep aanpassen die is gemaakt door Azure Backup.

Punten om te noteren:

1. U kunt de standaard naam van de RG gebruiken of deze bewerken volgens de vereisten van uw bedrijf.
2. U geeft het naam patroon RG op als invoer tijdens het maken van het back-upbeleid van de VM. De naam van de RG moet de volgende indeling hebben `<alpha-numeric string>* n <alpha-numeric string>`:. ' n ' wordt vervangen door een geheel getal (vanaf 1) en wordt gebruikt om uit te schalen als de eerste RG vol is. Eén RG kan vandaag nog een maximum van 600 Rpc's hebben.
              ![Naam kiezen bij het maken van beleid](./media/backup-during-vm-creation/create-policy.png)
3. Het patroon moet de onderstaande regels voor de naamgeving van RG volgen en de totale lengte mag niet groter zijn dan de Maxi maal toegestane grootte van RG.
    1. Namen van resource groepen mogen alleen bestaan uit alfanumerieke tekens, punten, onderstrepingen, afbreek streepjes en haakjes. Ze kunnen niet eindigen op een punt.
    2. Namen van resource groepen mogen Maxi maal 74 tekens bevatten, waaronder de naam van de RG en het achtervoegsel.
4. De eerste `<alpha-numeric-string>` is verplicht wanneer de tweede na ' n ' optioneel is. Dit geldt alleen als u een aangepaste naam wilt opgeven. Als u niets opgeeft in een van de tekst vakken, wordt de standaard naam gebruikt.
5. U kunt de naam van de RG bewerken door het beleid te wijzigen als en wanneer dat nodig is. Als het naam patroon wordt gewijzigd, wordt er nieuwe RPs in de nieuwe RG gemaakt. De oude RPs blijft echter wel aanwezig in de oude RG en niet worden verplaatst, omdat de RP-verzameling geen ondersteuning biedt voor het verplaatsen van resources. Uiteindelijk krijgt de RPs de garbage verzameld wanneer de punten verlopen.
![Naam wijzigen bij het wijzigen van beleid](./media/backup-during-vm-creation/modify-policy.png)
6. Het is raadzaam de resource groep die is gemaakt voor gebruik door de back-upservice, niet te vergren delen.

Als u de resource groep Azure Backup wilt configureren voor Virtual Machines met behulp van Power shell, raadpleegt u [Azure backup resource groep maken tijdens de retentie van de moment opname](backup-azure-vms-automation.md#creating-azure-backup-resource-group-during-snapshot-retention).

## <a name="start-a-backup-after-creating-the-vm"></a>Een back-up starten na het maken van de VM

De back-up van de virtuele machine wordt uitgevoerd in overeenstemming met uw back-upbeleid. We raden u echter aan een eerste back-up uit te voeren.

Nadat de VM is gemaakt, gaat u als volgt te werk:

1. Klik in de VM-eigenschappen op **back-up**. Voor de VM-status is de eerste back-up in behandeling totdat de eerste back-up wordt uitgevoerd
2. Klik op **Nu back** -up maken om een back-up op aanvraag uit te voeren.

    ![Een back-up op aanvraag uitvoeren](./media/backup-during-vm-creation/run-backup.png)

## <a name="use-a-resource-manager-template-to-deploy-a-protected-vm"></a>Een resource manager-sjabloon gebruiken om een beveiligde virtuele machine te implementeren

In de vorige stappen wordt uitgelegd hoe u de Azure Portal kunt gebruiken om een virtuele machine te maken en te beveiligen in een Recovery Services kluis. Als u een of meer Vm's snel wilt implementeren en wilt beveiligen in een Recovery Services kluis, raadpleegt u de sjabloon [een Windows VM implementeren en back-up inschakelen](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/).

## <a name="next-steps"></a>Volgende stappen

Nu u uw VM hebt beveiligd, leert u hoe u deze kunt beheren en herstellen.

- [Virtuele machines beheren en controleren](backup-azure-manage-vms.md)
- [VM herstellen](backup-azure-arm-restore-vms.md)

Als u problemen ondervindt, [raadpleegt](backup-azure-vms-troubleshoot.md) u de hand leiding voor het oplossen van problemen.
