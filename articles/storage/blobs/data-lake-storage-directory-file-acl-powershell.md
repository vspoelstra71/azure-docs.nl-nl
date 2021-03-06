---
title: Azure Data Lake Storage Gen2 Power shell voor bestanden & Acl's
description: Gebruik Power shell-cmdlets voor het beheren van mappen en ACL'S (toegangs beheer lijsten) in opslag accounts met een hiërarchische naam ruimte (HNS) ingeschakeld.
services: storage
author: normesta
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.topic: conceptual
ms.date: 04/21/2020
ms.author: normesta
ms.reviewer: prishet
ms.openlocfilehash: c859176857f64559b9a2994c9cfc2d4ec5f61e57
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2020
ms.locfileid: "82691079"
---
# <a name="use-powershell-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2"></a>Power shell gebruiken voor het beheren van mappen, bestanden en Acl's in Azure Data Lake Storage Gen2

In dit artikel leest u hoe u Power shell gebruikt voor het maken en beheren van mappen, bestanden en machtigingen in opslag accounts met een hiërarchische naam ruimte (HNS) ingeschakeld. 

[Gen1 naar Gen2-toewijzing](#gen1-gen2-map) | [geven feedback](https://github.com/Azure/azure-powershell/issues)

## <a name="prerequisites"></a>Vereisten

> [!div class="checklist"]
> * Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
> * Een opslag account met een hiërarchische naam ruimte (HNS) ingeschakeld. Volg [deze](data-lake-storage-quickstart-create-account.md) instructies om er een te maken.
> * .NET Framework is 4.7.2 of hoger geïnstalleerd. Zie [.NET Framework downloaden](https://dotnet.microsoft.com/download/dotnet-framework).
> * Power shell `5.1` -versie of hoger.

## <a name="install-the-powershell-module"></a>De Power shell-module installeren

1. Controleer of de versie van Power shell die is geïnstalleerd `5.1` , of hoger is met behulp van de volgende opdracht.    

   ```powershell
   echo $PSVersionTable.PSVersion.ToString() 
   ```
    
   Zie [bestaande Windows Power shell upgraden](https://docs.microsoft.com/powershell/scripting/install/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell) voor informatie over het bijwerken van uw versie van Power shell
    
2. Installeer **AZ. Storage** -module.

   ```powershell
   Install-Module Az.Storage -Repository PSGallery -Force  
   ```

   Zie [de module Azure PowerShell installeren](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0) voor meer informatie over het installeren van Power shell-modules.

## <a name="connect-to-the-account"></a>Verbinding maken met het account

Open een Windows Power shell-opdracht venster en meld u vervolgens aan bij uw Azure- `Connect-AzAccount` abonnement met de opdracht en volg de instructies op het scherm.

```powershell
Connect-AzAccount
```

Als uw identiteit is gekoppeld aan meer dan één abonnement, stelt u uw actieve abonnement in op het abonnement van het opslag account waarin u directory's wilt maken en beheren. In dit voor beeld vervangt u `<subscription-id>` de waarde van de tijdelijke aanduiding door de id van uw abonnement.

```powershell
Select-AzSubscription -SubscriptionId <subscription-id>
```

Kies vervolgens hoe u wilt dat uw opdrachten autorisatie aanvragen voor het opslag account. 

### <a name="option-1-obtain-authorization-by-using-azure-active-directory-ad"></a>Optie 1: autorisatie verkrijgen met behulp van Azure Active Directory (AD)

Met deze methode zorgt het systeem ervoor dat uw gebruikers account de juiste RBAC-toewijzingen (op rollen gebaseerd toegangs beheer) en ACL-machtigingen heeft. 

```powershell
$ctx = New-AzStorageContext -StorageAccountName '<storage-account-name>' -UseConnectedAccount
```

### <a name="option-2-obtain-authorization-by-using-the-storage-account-key"></a>Optie 2: autorisatie verkrijgen met behulp van de sleutel van het opslag account

Met deze methode controleert het systeem geen RBAC-of ACL-machtigingen.

```powershell
$storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
$ctx = $storageAccount.Context
```

## <a name="create-a-file-system"></a>Een bestandssysteem maken

Een bestands systeem fungeert als een container voor uw bestanden. U kunt er een maken met behulp van de `New-AzDatalakeGen2FileSystem` -cmdlet. 

In dit voor beeld wordt een bestands `my-file-system`systeem gemaakt met de naam.

```powershell
$filesystemName = "my-file-system"
New-AzDatalakeGen2FileSystem -Context $ctx -Name $filesystemName
```

## <a name="create-a-directory"></a>Een map maken

Een directory verwijzing maken met behulp `New-AzDataLakeGen2Item` van de-cmdlet. 

In dit voor beeld wordt een `my-directory` map toegevoegd met de naam van een bestands systeem.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Directory
```

In dit voor beeld wordt dezelfde map toegevoegd, maar worden ook de machtigingen, umask, eigenschaps waarden en meta gegevens waarden ingesteld. 

```powershell
$dir = New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Directory -Permission rwxrwxrwx -Umask ---rwx---  -Property @{"ContentEncoding" = "UDF8"; "CacheControl" = "READ"} -Metadata  @{"tag1" = "value1"; "tag2" = "value2" }
```

## <a name="show-directory-properties"></a>Mapeigenschappen weer geven

In dit voor beeld wordt een Directory opgehaald `Get-AzDataLakeGen2Item` met behulp van de-cmdlet en worden de eigenschaps waarden vervolgens naar de console afgedrukt.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dir =  Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname
$dir.ACL
$dir.Permissions
$dir.Group
$dir.Owner
$dir.Properties
$dir.Properties.Metadata
```

## <a name="rename-or-move-a-directory"></a>Een map een andere naam geven of verplaatsen

Wijzig de naam van een map of verplaats `Move-AzDataLakeGen2Item` deze met behulp van de cmdlet.

In dit voor beeld wordt de naam van een map `my-directory` gewijzigd van de `my-new-directory`naam in de naam.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dirname2 = "my-new-directory/"
Move-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -DestFileSystem $filesystemName -DestPath $dirname2
```

> [!NOTE]
> Gebruik de `-Force` para meter als u wilt overschrijven zonder prompts.

In dit voor beeld wordt een `my-directory` map verplaatst met de naam `my-directory-2` naar `my-subdirectory`een submap met de naam. 

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dirname2 = "my-directory-2/my-subdirectory/"
Move-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname1 -DestFileSystem $filesystemName -DestPath $dirname2
```

## <a name="delete-a-directory"></a>Een map verwijderen

Een directory verwijderen met de `Remove-AzDataLakeGen2Item` cmdlet.

In dit voor beeld wordt een `my-directory`map met de naam verwijderd. 

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
Remove-AzDataLakeGen2Item  -Context $ctx -FileSystem $filesystemName -Path $dirname 
```

U kunt de `-Force` para meter gebruiken om het bestand zonder prompt te verwijderen.

## <a name="download-from-a-directory"></a>Downloaden uit een directory

Down load een bestand vanuit een directory met de `Get-AzDataLakeGen2ItemContent` cmdlet.

In dit voor beeld wordt een `upload.txt` bestand gedownload met de `my-directory`naam van een map met de naam. 

```powershell
$filesystemName = "my-file-system"
$filePath = "my-directory/upload.txt"
$downloadFilePath = "download.txt"
Get-AzDataLakeGen2ItemContent -Context $ctx -FileSystem $filesystemName -Path $filePath -Destination $downloadFilePath
```

## <a name="list-directory-contents"></a>Mapinhoud weergeven

De inhoud van een directory weer geven met behulp van de `Get-AzDataLakeGen2ChildItem` -cmdlet. U kunt de optionele para meter `-OutputUserPrincipalName` gebruiken om de naam (in plaats van de object-id) van de gebruikers op te halen.

In dit voor beeld wordt de inhoud weer gegeven `my-directory`van een map met de naam.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
Get-AzDataLakeGen2ChildItem -Context $ctx -FileSystem $filesystemName -Path $dirname -OutputUserPrincipalName
```

In het volgende voor beeld `ACL`worden `Permissions`de `Group`eigenschappen, `Owner` , en van elk item in de map weer gegeven. De `-FetchProperty` para meter is vereist om waarden op te `ACL` halen voor de eigenschap. 

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$properties = Get-AzDataLakeGen2ChildItem -Context $ctx -FileSystem $filesystemName -Path $dirname -Recurse -FetchProperty
$properties.ACL
$properties.Permissions
$properties.Group
$properties.Owner
```

Als u de inhoud van een bestands systeem wilt weer geven `-Path` , laat u de para meter van de opdracht weg.

## <a name="upload-a-file-to-a-directory"></a>Een bestand uploaden naar een map

Een bestand uploaden naar een map met behulp `New-AzDataLakeGen2Item` van de-cmdlet.

In dit voor beeld wordt een bestand geüpload `upload.txt` met de naam naar `my-directory`een map met de naam. 

```powershell
$localSrcFile =  "upload.txt"
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$destPath = $dirname + (Get-Item $localSrcFile).Name
New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $destPath -Source $localSrcFile -Force 
```

In dit voor beeld wordt hetzelfde bestand geüpload, maar worden vervolgens de machtigingen, umask, eigenschaps waarden en meta gegevens waarden van het doel bestand ingesteld. In dit voor beeld worden deze waarden ook afgedrukt naar de-console.

```powershell
$file = New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $destPath -Source $localSrcFile -Permission rwxrwxrwx -Umask ---rwx--- -Property @{"ContentEncoding" = "UDF8"; "CacheControl" = "READ"} -Metadata  @{"tag1" = "value1"; "tag2" = "value2" }
$file1
$file1.Properties
$file1.Properties.Metadata

```

## <a name="show-file-properties"></a>Bestands eigenschappen weer geven

In dit voor beeld wordt een bestand opgehaald `Get-AzDataLakeGen2Item` met behulp van de-cmdlet en worden de eigenschaps waarden vervolgens naar de console afgedrukt.

```powershell
$filepath =  "my-directory/upload.txt"
$filesystemName = "my-file-system"
$file = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filepath
$file
$file.ACL
$file.Permissions
$file.Group
$file.Owner
$file.Properties
$file.Properties.Metadata
```

## <a name="delete-a-file"></a>Een bestand verwijderen

Een bestand verwijderen met behulp `Remove-AzDataLakeGen2Item` van de-cmdlet.

In dit voor beeld wordt een `upload.txt`bestand met de naam verwijderd. 

```powershell
$filesystemName = "my-file-system"
$filepath = "upload.txt"
Remove-AzDataLakeGen2Item  -Context $ctx -FileSystem $filesystemName -Path $filepath 
```

U kunt de `-Force` para meter gebruiken om het bestand zonder prompt te verwijderen.

## <a name="manage-access-permissions"></a>Toegangs machtigingen beheren

U kunt toegangs machtigingen voor bestands systemen, mappen en bestanden ophalen, instellen en bijwerken. Deze machtigingen worden vastgelegd in toegangs beheer lijsten (Acl's).

> [!NOTE]
> Als u Azure Active Directory (Azure AD) gebruikt om opdrachten te autoriseren, moet u ervoor zorgen dat aan uw beveiligingsprincipal de [rol van BLOB-gegevens eigenaar voor opslag](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)is toegewezen. Zie voor meer informatie over hoe ACL-machtigingen worden toegepast en de gevolgen van het wijzigen van [toegangs beheer in azure data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control).

### <a name="get-an-acl"></a>Een ACL ophalen

De ACL van een map of bestand ophalen met behulp `Get-AzDataLakeGen2Item`van de-cmdlet.

In dit voor beeld wordt de ACL van een **Bestands systeem** opgehaald en wordt de ACL vervolgens naar de console afgedrukt.

```powershell
$filesystemName = "my-file-system"
$filesystem = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName
$filesystem.ACL
```

In dit voor beeld wordt de ACL van een **Directory**opgehaald en wordt de ACL vervolgens naar de console afgedrukt.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dir = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname
$dir.ACL
```

In dit voor beeld wordt de ACL van een **bestand** opgehaald en wordt de ACL vervolgens naar de console afgedrukt.

```powershell
$filePath = "my-directory/upload.txt"
$file = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filePath
$file.ACL
```

In de volgende afbeelding ziet u de uitvoer na het ophalen van de ACL van een directory.

![ACL-uitvoer ophalen](./media/data-lake-storage-directory-file-acl-powershell/get-acl.png)

In dit voor beeld heeft de gebruiker die eigenaar is, de machtigingen lezen, schrijven en uitvoeren. De groep die eigenaar is, heeft alleen lees-en uitvoer machtigingen. Zie [toegangs beheer in azure data Lake Storage Gen2](data-lake-storage-access-control.md)voor meer informatie over toegangs beheer lijsten.

### <a name="set-an-acl"></a>Een ACL instellen

Gebruik de `set-AzDataLakeGen2ItemAclObject` cmdlet om een ACL te maken voor de gebruiker die eigenaar is, de groep die eigenaar is of andere gebruikers. Gebruik vervolgens de `Update-AzDataLakeGen2Item` cmdlet om de ACL door te voeren.

In dit voor beeld wordt de ACL ingesteld op een **Bestands systeem** voor de gebruiker die eigenaar is, de groep die eigenaar is of andere gebruikers, en wordt de ACL vervolgens naar de console afgedrukt.

```powershell
$filesystemName = "my-file-system"
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rw- 
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission rw- -InputObject $acl 
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission -wx -InputObject $acl
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Acl $acl
$filesystem = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName
$filesystem.ACL
```

In dit voor beeld wordt de ACL ingesteld op een **map** voor de gebruiker die eigenaar is van de groep of andere gebruikers, en wordt de ACL vervolgens naar de console afgedrukt.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rw- 
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission rw- -InputObject $acl 
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission -wx -InputObject $acl
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl
$dir = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname
$dir.ACL
```
In dit voor beeld wordt de toegangs beheer lijst voor een **bestand** ingesteld op de gebruiker die eigenaar is van de groep of van andere gebruikers, en wordt de ACL vervolgens naar de console afgedrukt.

```powershell
$filesystemName = "my-file-system"
$filePath = "my-directory/upload.txt"
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rw- 
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission rw- -InputObject $acl 
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission "-wx" -InputObject $acl
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filePath -Acl $acl
$file = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filePath
$file.ACL
```

In de volgende afbeelding ziet u de uitvoer na het instellen van de ACL van een bestand.

![ACL-uitvoer ophalen](./media/data-lake-storage-directory-file-acl-powershell/set-acl.png)

In dit voor beeld hebben de gebruiker die eigenaar is en de groep die eigenaar is alleen lees-en schrijf machtigingen. Alle andere gebruikers hebben machtigingen voor schrijven en uitvoeren. Zie [toegangs beheer in azure data Lake Storage Gen2](data-lake-storage-access-control.md)voor meer informatie over toegangs beheer lijsten.


### <a name="set-acls-on-all-items-in-a-file-system"></a>Acl's instellen voor alle items in een bestands systeem

U kunt de `Get-AzDataLakeGen2Item` en de `-Recurse` para meter samen met de `Update-AzDataLakeGen2Item` CMDLET recursief gebruiken om de ACL voor mappen en bestanden in een bestands systeem in te stellen. 

```powershell
$filesystemName = "my-file-system"
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rw- 
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission rw- -InputObject $acl 
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission -wx -InputObject $acl

$Token = $Null
do
{
     $items = Get-AzDataLakeGen2ChildItem -Context $ctx -FileSystem $filesystemName -Recurse -ContinuationToken $Token    
     if($items.Length -le 0) { Break;}
     $items | Update-AzDataLakeGen2Item -Acl $acl
     $Token = $items[$items.Count -1].ContinuationToken;
}
While ($Token -ne $Null) 
```

### <a name="add-or-update-an-acl-entry"></a>Een ACL-vermelding toevoegen of bijwerken

Haal eerst de ACL op. Gebruik vervolgens de `set-AzDataLakeGen2ItemAclObject` cmdlet om een ACL-vermelding toe te voegen of bij te werken. Gebruik de `Update-AzDataLakeGen2Item` cmdlet om de ACL door te voeren.

In dit voor beeld wordt de ACL voor een gebruiker gemaakt of bijgewerkt voor een **map** .

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$acl = (Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname).ACL
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType user -EntityID xxxxxxxx-xxxx-xxxxxxxxxxx -Permission r-x -InputObject $acl 
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl
```

### <a name="remove-an-acl-entry"></a>Een ACL-vermelding verwijderen

In dit voor beeld wordt een item uit een bestaande ACL verwijderd.

```powershell
$id = "xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

# Create the new ACL object.
[Collections.Generic.List[System.Object]]$aclnew =$acl

foreach ($a in $aclnew)
{
    if ($a.AccessControlType -eq "User"-and $a.DefaultScope -eq $false -and $a.EntityId -eq $id)
    {
        $aclnew.Remove($a);
        break;
    }
}
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $aclnew
```

<a id="gen1-gen2-map" />

## <a name="gen1-to-gen2-mapping"></a>Toewijzing van gen1 naar Gen2

In de volgende tabel ziet u hoe de cmdlets die worden gebruikt voor Data Lake Storage Gen1 worden toegewezen aan de cmdlets voor Data Lake Storage Gen2.

|Data Lake Storage Gen1-cmdlet| Data Lake Storage Gen2-cmdlet| Opmerkingen |
|--------|---------|-----|
|Get-AzDataLakeStoreChildItem|Get-AzDataLakeGen2ChildItem|Standaard worden met de cmdlet Get-AzDataLakeGen2ChildItem alleen de onderliggende items op het eerste niveau weer gegeven. Met de para meter-recursief worden onderliggende items recursief weer gegeven. |
|Get-AzDataLakeStoreItem<br>Get-AzDataLakeStoreItemAclEntry<br>Get-AzDataLakeStoreItemOwner<br>Get-AzDataLakeStoreItemPermission|Get-AzDataLakeGen2Item|De uitvoer items van de cmdlet Get-AzDataLakeGen2Item hebben de volgende eigenschappen: ACL, eigenaar, groep, machtiging.|
|Get-AzDataLakeStoreItemContent|Get-AzDataLakeGen2FileContent|De cmdlet Get-AzDataLakeGen2FileContent laadt bestand inhoud naar een lokaal bestand.|
|Move-AzDataLakeStoreItem|Move-AzDataLakeGen2Item||
|New-AzDataLakeStoreItem|New-AzDataLakeGen2Item|Met deze cmdlet wordt de nieuwe bestands inhoud van een lokaal bestand geüpload.|
|Remove-AzDataLakeStoreItem|Remove-AzDataLakeGen2Item||
|Set-AzDataLakeStoreItemOwner<br>Set-AzDataLakeStoreItemPermission<br>Set-AzDataLakeStoreItemAcl|Update-AzDataLakeGen2Item|Met de cmdlet Update-AzDataLakeGen2Item wordt één enkel item bijgewerkt en niet recursief. Als u recursief wilt bijwerken, kunt u items weer geven met behulp van de cmdlet Get-AzDataLakeStoreChildItem en vervolgens pijp lijn naar de cmdlet Update-AzDataLakeGen2Item.|
|Test-AzDataLakeStoreItem|Get-AzDataLakeGen2Item|Met de cmdlet Get-AzDataLakeGen2Item wordt een fout gerapporteerd als het item niet bestaat.|

## <a name="see-also"></a>Zie ook

* [Bekende problemen](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [PowerShell Storage-cmdlets](/powershell/module/az.storage)
