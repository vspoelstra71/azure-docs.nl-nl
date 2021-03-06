---
title: Herstel naar een bepaald tijdstip voor blok-blobs inschakelen en beheren (preview-versie)
titleSuffix: Azure Storage
description: Meer informatie over het gebruik van punt-in-time Restore (preview) voor het herstellen van blok-blobs naar een status op een eerder tijdstip.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 05/28/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: fe98e04c37172dc6b91c86fab8200022ed860d4f
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2020
ms.locfileid: "84170100"
---
# <a name="enable-and-manage-point-in-time-restore-for-block-blobs-preview"></a>Herstel naar een bepaald tijdstip voor blok-blobs inschakelen en beheren (preview-versie)

U kunt punt-in-time Restore (preview) gebruiken om blok-blobs op een eerder tijdstip terug te zetten naar hun status. In dit artikel wordt beschreven hoe u herstel naar een bepaald tijdstip kunt inschakelen voor een opslag account met Power shell. U ziet ook hoe u een herstel bewerking uitvoert met Power shell.

Zie herstel naar een bepaald [tijdstip voor blok-blobs (preview)](point-in-time-restore-overview.md)voor meer informatie en om te leren hoe u zich kunt registreren voor de preview-versie.

> [!CAUTION]
> Herstel naar een bepaald tijdstip biedt alleen ondersteuning voor het herstellen van bewerkingen op blok-blobs. Bewerkingen op containers kunnen niet worden hersteld. Als u een container uit het opslag account verwijdert door de bewerking [Delete container](/rest/api/storageservices/delete-container) aan te roepen tijdens de preview-fase van het herstel punt, kan deze container niet worden hersteld met een herstel bewerking. In plaats van een container te verwijderen, kunt u tijdens de preview-versie de afzonderlijke blobs verwijderen als u deze wilt herstellen.

> [!IMPORTANT]
> De preview-versie van het herstel punt is bedoeld voor niet-productie gebruik. Service Level Agreements (Sla's) op het niveau van de productie zijn momenteel niet beschikbaar.

## <a name="install-the-preview-module"></a>De preview-module installeren

Als u Azure Point-in-time Restore met Power shell wilt configureren, installeert u eerst versie [1.14.1-Preview](https://www.powershellgallery.com/packages/Az.Storage/1.14.1-preview) van de module AZ. Storage Power shell. Voer de volgende stappen uit om de preview-module te installeren:

1. Verwijder eerdere installaties van Azure PowerShell van Windows met de optie **Apps & onderdelen** onder **instellingen**.

1. Zorg ervoor dat de meest recente versie van PowerShellGet is geïnstalleerd. Open een Windows Power shell-venster en voer de volgende opdracht uit om de meest recente versie te installeren:

    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```

1. Sluit het Power shell-venster en open het opnieuw nadat u PowerShellGet hebt geïnstalleerd.

1. Installeer de nieuwste versie van Azure PowerShell:

    ```powershell
    Install-Module Az –Repository PSGallery –AllowClobber
    ```

1. Installeer de module AZ. Storage Preview:

    ```powershell
    Install-Module Az.Storage -Repository PSGallery -RequiredVersion 1.14.1-preview -AllowPrerelease -AllowClobber -Force
    ```

Zie [Install Azure PowerShell with PowerShellGet](/powershell/azure/install-az-ps)(Engelstalig) voor meer informatie over het installeren van Azure PowerShell.

## <a name="enable-and-configure-point-in-time-restore"></a>Herstel naar een bepaald tijdstip inschakelen en configureren

Voordat u herstel naar een bepaald tijdstip inschakelt en configureert, moet u de vereisten hiervoor inschakelen: voorlopig verwijderen, feed wijzigen en BLOB-versie beheer. Raadpleeg de volgende artikelen voor meer informatie over het inschakelen van elk van deze functies:

- [Voorlopig verwijderen voor blobs inschakelen](soft-delete-enable.md)
- [De wijzigings feed in-en uitschakelen](storage-blob-change-feed.md#enable-and-disable-the-change-feed)
- [BLOB-versie beheer inschakelen en beheren](versioning-enable.md)

Als u Azure Point-in-time Restore met Power shell wilt configureren, roept u de opdracht Enable-AzStorageBlobRestorePolicy aan. In het volgende voor beeld wordt zacht verwijderen ingeschakeld en wordt de Bewaar periode voor de tijdelijke verwijdering ingesteld, wordt wijzigings invoer ingeschakeld en wordt vervolgens herstel naar een bepaald tijdstip ingeschakeld. Voordat u het voor beeld uitvoert, moet u de Azure Portal-of Azure Resource Manager-sjabloon gebruiken om ook BLOB-versie beheer in te scha kelen.

Wanneer u het voor beeld uitvoert, moet u de waarden tussen punt haken vervangen door uw eigen waarden:

```powershell
# Sign in to your Azure account.
Connect-AzAccount

# Set resource group and account variables.
$rgName = "<resource-group>"
$accountName = "<storage-account>"

# Enable soft delete with a retention of 6 days.
Enable-AzStorageBlobDeleteRetentionPolicy -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -RetentionDays 6

# Enable change feed.
Update-AzStorageBlobServiceProperty -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -EnableChangeFeed $true

# Enable point-in-time restore with a retention period of 5 days.
# The retention period for point-in-time restore must be at least one day less than that set for soft delete.
Enable-AzStorageBlobRestorePolicy -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -RestoreDays 5

# View the service settings.
Get-AzStorageBlobServiceProperty -ResourceGroupName $rgName `
    -StorageAccountName $accountName
```

## <a name="perform-a-restore-operation"></a>Een herstel bewerking uitvoeren

Als u een herstel bewerking wilt starten, roept u de opdracht Restore-AzStorageBlobRange aan, waarbij u het herstel punt opgeeft als een UTC- **datum/tijd** -waarde. U kunt lexicographical-bereiken van blobs opgeven die u wilt herstellen, of een bereik weglaten om alle blobs in alle containers in het opslag account te herstellen. Er worden Maxi maal 10 lexicographical-bereiken ondersteund per herstel bewerking. Het kan enkele minuten duren voordat de herstel bewerking is voltooid.

Houd bij het opgeven van een aantal blobs dat moet worden hersteld de volgende regels in acht:

- Het container patroon dat is opgegeven voor het begin bereik en het eind bereik moet mini maal drie tekens bevatten. De slash (/) die wordt gebruikt om een container naam te scheiden van een BLOB-naam, telt niet mee aan dit minimum.
- Er kan Maxi maal 10 bereiken worden opgegeven per herstel bewerking.
- Joker tekens worden niet ondersteund. Ze worden beschouwd als standaard tekens.
- U kunt blobs in de `$root` containers herstellen `$web` door ze expliciet op te geven in een bereik dat is door gegeven aan een herstel bewerking. De `$root` `$web` containers en worden alleen hersteld als ze expliciet zijn opgegeven. Andere systeem containers kunnen niet worden hersteld.

> [!IMPORTANT]
> Wanneer u een herstel bewerking uitvoert, blokkeert Azure Storage gegevens bewerkingen op de blobs in de bereiken die worden teruggezet voor de duur van de bewerking. Lees-, schrijf-en verwijder bewerkingen worden geblokkeerd op de primaire locatie. Daarom kunnen bewerkingen, zoals het weer geven van containers in het Azure Portal, niet worden uitgevoerd zoals verwacht tijdens het terugzetten.
>
> Lees bewerkingen van de secundaire locatie kunnen door gaan tijdens de herstel bewerking als het opslag account geo-gerepliceerd is.

### <a name="restore-all-containers-in-the-account"></a>Alle containers in het account herstellen

Als u alle containers en blobs in het opslag account wilt herstellen, roept u de opdracht Restore-AzStorageBlobRange aan, waarbij u de `-BlobRestoreRange` para meter weglaat. In het volgende voor beeld worden de containers in het opslag account teruggezet naar de status 12 uur vóór het huidige tijdstip:

```powershell
# Specify -TimeToRestore as a UTC value
Restore-AzStorageBlobRange -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -TimeToRestore (Get-Date).AddHours(-12)
```

### <a name="restore-a-single-range-of-block-blobs"></a>Eén bereik van blok-blobs herstellen

Als u een bereik van blobs wilt herstellen, roept u de opdracht Restore-AzStorageBlobRange aan en geeft u een lexicographical-bereik van container-en BLOB-namen op voor de `-BlobRestoreRange` para meter. Het begin van het bereik is inclusief en het einde van het bereik is exclusief.

Als u bijvoorbeeld de blobs in één container met de naam *sample-container*wilt herstellen, kunt u een bereik opgeven dat begint met voor *beeld-container* en eindigt met *sample-container1*. Er zijn geen vereisten voor de containers met de naam in het begin-en eind bereik. Omdat het einde van het bereik exclusief is, zelfs als het opslag account een container met de naam *sample-container1*bevat, wordt alleen de container met de naam *sample-container* teruggezet:

```powershell
$range = New-AzStorageBlobRangeToRestore -StartRange sample-container -EndRange sample-container1
```

Als u een subset van blobs in een container wilt herstellen, gebruikt u een slash (/) om de container naam te scheiden van het BLOB-patroon. Het volgende bereik selecteert bijvoorbeeld blobs in één container waarvan de naam begint met de letters *d* t/m *f*:

```powershell
$range = New-AzStorageBlobRangeToRestore -StartRange sample-container/d -EndRange sample-container/g
```

Geef vervolgens het bereik op voor de opdracht Restore-AzStorageBlobRange. Geef het herstel punt op door een UTC- **datum** waarde voor de para meter op te geven `-TimeToRestore` . In het volgende voor beeld worden de blobs in het opgegeven bereik tot de status 3 dagen vóór het huidige moment hersteld:

```powershell
# Specify -TimeToRestore as a UTC value
Restore-AzStorageBlobRange -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -BlobRestoreRange $range `
    -TimeToRestore (Get-Date).AddDays(-3)
```

### <a name="restore-multiple-ranges-of-block-blobs"></a>Meerdere bereiken van blok-blobs herstellen

Als u meerdere bereiken van blok-blobs wilt herstellen, geeft u een matrix met bereiken op voor de `-BlobRestoreRange` para meter. Er worden Maxi maal tien bereiken ondersteund per herstel bewerking. In het volgende voor beeld wordt twee bereiken opgegeven voor het herstellen van de volledige inhoud van *container1* en *container4*:

```powershell
$range1 = New-AzStorageBlobRangeToRestore -StartRange container1 -EndRange container2
$range2 = New-AzStorageBlobRangeToRestore -StartRange container4 -EndRange container5

Restore-AzStorageBlobRange -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -TimeToRestore (Get-Date).AddMinutes(-30) `
    -BlobRestoreRange @($range1, $range2)
```

## <a name="next-steps"></a>Volgende stappen

- [Herstel naar een bepaald tijdstip voor blok-blobs (preview-versie)](point-in-time-restore-overview.md)
- [Voorlopig verwijderen](soft-delete-overview.md)
- [Wijzigings feed (preview-versie)](storage-blob-change-feed.md)
- [Versie beheer van BLOB (preview)](versioning-overview.md)
