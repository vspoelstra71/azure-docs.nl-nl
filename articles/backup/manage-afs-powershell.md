---
title: Back-ups van Azure-bestands shares beheren met Power shell
description: Informatie over het gebruik van Power shell voor het beheren en bewaken van Azure-bestands shares waarvan een back-up is gemaakt door de Azure Backup service.
ms.topic: conceptual
ms.date: 1/27/2020
ms.openlocfilehash: 6ee5fb92e4a66a9d6db66514f966c3650d3a4f13
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2020
ms.locfileid: "83201981"
---
# <a name="manage-azure-file-share-backups-with-powershell"></a>Back-ups van Azure-bestands shares beheren met Power shell

In dit artikel wordt beschreven hoe u Azure PowerShell gebruikt voor het beheren en bewaken van de Azure-bestands shares waarvan een back-up wordt gemaakt door de Azure Backup-service.

> [!WARNING]
> Zorg ervoor dat de PS-versie is bijgewerkt naar de minimale versie van AZ. Recovery Services 2.6.0 voor AFS-back-ups. Raadpleeg de [sectie](backup-azure-afs-automation.md#important-notice-backup-item-identification) overzicht van de vereiste voor deze wijziging voor meer informatie.

## <a name="modify-the-protection-policy"></a>Het beveiligings beleid wijzigen

Gebruik [Enable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection?view=azps-1.4.0)om het beleid te wijzigen dat wordt gebruikt voor het maken van een back-up van de Azure-bestands share. Geef het relevante back-upitem en het nieuwe back-upbeleid op.

In het volgende voor beeld wordt het **testAzureFS** -beveiligings beleid gewijzigd van **dailyafs** in **monthlyafs**.

```powershell
$monthlyafsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "monthlyafs"
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType AzureFiles -Name "testAzureFS"
Enable-AzRecoveryServicesBackupProtection -Item $afsBkpItem -Policy $monthlyafsPol
```

## <a name="track-backup-and-restore-jobs"></a>Back-up-en herstel taken bijhouden

Back-ups op aanvraag en herstel bewerkingen geven een taak samen met een ID, zoals wordt weer gegeven wanneer u [een back-up op aanvraag uitvoert](backup-azure-afs-automation.md#trigger-an-on-demand-backup). Gebruik de cmdlet [Get-AzRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob?view=azps-1.4.0) om de voortgang en Details van de taak bij te houden.

```powershell
$job = Get-AzRecoveryServicesBackupJob -JobId 00000000-6c46-496e-980a-3740ccb2ad75 -VaultId $vaultID

 $job | fl


IsCancellable        : False
IsRetriable          : False
ErrorDetails         : {Microsoft.Azure.Commands.RecoveryServices.Backup.Cmdlets.Models.AzureFileShareJobErrorInfo}
ActivityId           : 00000000-5b71-4d73-9465-8a4a91f13a36
JobId                : 00000000-6c46-496e-980a-3740ccb2ad75
Operation            : Restore
Status               : Failed
WorkloadName         : testAFS
StartTime            : 12/10/2018 9:56:38 AM
EndTime              : 12/10/2018 11:03:03 AM
Duration             : 01:06:24.4660027
BackupManagementType : AzureStorage

$job.ErrorDetails

 ErrorCode ErrorMessage                                          Recommendations
 --------- ------------                                          ---------------
1073871825 Microsoft Azure Backup encountered an internal error. Wait for a few minutes and then try the operation again. If the issue persists, please contact Microsoft support.
```

## <a name="stop-protection-on-a-file-share"></a>De beveiliging voor een bestandsshare stoppen

Er zijn twee manieren om het beveiligen van Azure-bestandsshares te stoppen:

* Alle toekomstige back-uptaken stoppen en alle herstel punten *verwijderen*
* Alle toekomstige back-uptaken stoppen, maar de herstel punten *behouden*

Er zijn mogelijk kosten verbonden aan het verlaten van de herstel punten in de opslag, omdat de onderliggende moment opnamen die zijn gemaakt door Azure Backup, behouden blijven. Het voor deel van het verlaten van de herstel punten is echter dat u de bestands share later, indien gewenst, kunt herstellen. Zie de [prijs informatie](https://azure.microsoft.com/pricing/details/storage/files/)voor meer informatie over de kosten voor het verlaten van de herstel punten. Als u ervoor kiest om alle herstel punten te verwijderen, kunt u de bestands share niet herstellen.

## <a name="stop-protection-and-retain-recovery-points"></a>Beveiliging stoppen en herstel punten behouden

Als u de beveiliging wilt stoppen tijdens het bewaren van gegevens, gebruikt u de cmdlet [Disable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/disable-azrecoveryservicesbackupprotection?view=azps-3.3.0) .

In het volgende voor beeld wordt de beveiliging van de *afsfileshare* -bestands share gestopt, maar worden alle herstel punten behouden:

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "afstesting" -Name "afstest" | select -ExpandProperty ID
$bkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureStorage -WorkloadType AzureFiles -Name "afsfileshare" -VaultId $vaultID
Disable-AzRecoveryServicesBackupProtection -Item $bkpItem -VaultId $vaultID
```

```output
WorkloadName     Operation         Status         StartTime                 EndTime                   JobID
------------     ---------         ------         ---------                 -------                   -----
afsfileshare     DisableBackup     Completed      1/26/2020 2:43:59 PM      1/26/2020 2:44:21 PM      98d9f8a1-54f2-4d85-8433-c32eafbd793f
```

Het kenmerk taak-ID in de uitvoer komt overeen met de taak-ID van de taak die is gemaakt door de back-upservice voor de bewerking ' Stop beveiliging '. Gebruik de cmdlet [Get-AzRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob?view=azps-3.3.0) om de status van de taak bij te houden.

## <a name="stop-protection-without-retaining-recovery-points"></a>Beveiliging stoppen zonder herstel punten te bewaren

Als u de beveiliging wilt stoppen zonder herstel punten te bewaren, gebruikt u de cmdlet [Disable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/disable-azrecoveryservicesbackupprotection?view=azps-3.3.0) en voegt u de para meter **-RemoveRecoveryPoints** toe.

In het volgende voor beeld wordt de beveiliging van de *afsfileshare* -bestands share gestopt zonder herstel punten te bewaren:

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "afstesting" -Name "afstest" | select -ExpandProperty ID
$bkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureStorage -WorkloadType AzureFiles -Name "afsfileshare" -VaultId $vaultID
Disable-AzRecoveryServicesBackupProtection -Item $bkpItem -VaultId $vaultID -RemoveRecoveryPoints
```

```output
WorkloadName     Operation            Status         StartTime                 EndTime                   JobID
------------     ---------            ------         ---------                 -------                   -----
afsfileshare     DeleteBackupData     Completed      1/26/2020 2:50:57 PM      1/26/2020 2:51:39 PM      b1a61c0b-548a-4687-9d15-9db1cc5bcc85
```

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over](manage-afs-backup.md) het beheren van back-ups van Azure-bestands shares in de Azure Portal.
