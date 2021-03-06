---
title: Certificaten in Azure Automation beheren
description: In dit artikel leest u hoe u met certificaten werkt voor toegang door runbooks en DSC-configuraties.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: mgoedtel
ms.author: magoedte
ms.date: 04/02/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: ebd4bf0ff91342229edb4295489f0cbd0970699a
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2020
ms.locfileid: "83745027"
---
# <a name="manage-certificates-in-azure-automation"></a>Certificaten in Azure Automation beheren

Azure Automation certificaten veilig opslaat voor toegang door runbooks en DSC-configuraties, met behulp van de cmdlet [Get-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationCertificate?view=azps-3.7.0) voor Azure Resource Manager resources. Met beveiligde certificaat opslag kunt u runbooks en DSC-configuraties maken die gebruikmaken van certificaten voor verificatie, of deze toevoegen aan Azure of bronnen van derden.

>[!NOTE]
>Beveilig assets in Azure Automation referenties, certificaten, verbindingen en versleutelde variabelen bevatten. Deze assets worden versleuteld en opgeslagen in Automation met behulp van een unieke sleutel die wordt gegenereerd voor elk Automation-account. Automation slaat de sleutel op in de door het systeem beheerde Key Vault-service. Voordat u een beveiligde Asset opslaat, laadt Automation de sleutel van Key Vault en gebruikt deze om de Asset te versleutelen. 

## <a name="powershell-cmdlets-to-access-certificates"></a>Power shell-cmdlets voor toegang tot certificaten

Met de cmdlets in de volgende tabel worden Automation-certificaten met Power shell gemaakt en beheerd. Ze worden geleverd als onderdeel van de [AZ-modules](modules.md#az-modules).

|Cmdlet |Beschrijving|
| --- | ---|
|[Get-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationCertificate?view=azps-3.7.0)|Haalt informatie op over een certificaat dat moet worden gebruikt in een runbook of DSC-configuratie. U kunt het certificaat zelf alleen ophalen met behulp van de interne `Get-AutomationCertificate` cmdlet.|
|[New-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationCertificate?view=azps-3.7.0)|Hiermee maakt u een nieuw certificaat in Automation.|
|[Remove-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Remove-AzAutomationCertificate?view=azps-3.7.0)|Hiermee verwijdert u een certificaat uit Automation.|
|[Set-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Set-AzAutomationCertificate?view=azps-3.7.0)|Hiermee stelt u de eigenschappen van een bestaand certificaat in, waaronder het uploaden van het certificaat bestand en het instellen van het wacht woord voor een **PFX** -bestand.|

De cmdlet [add-AzureCertificate](/powershell/module/servicemanagement/azure/add-azurecertificate) kan ook worden gebruikt voor het uploaden van een service certificaat voor de opgegeven Cloud service.

## <a name="internal-cmdlets-to-access-certificates"></a>Interne cmdlets voor toegang tot certificaten

De interne cmdlet in de volgende tabel wordt gebruikt voor toegang tot certificaten in uw runbooks. Deze cmdlet wordt geleverd met de globale module `Orchestrator.AssetManagement.Cmdlets` . Zie [interne cmdlets](modules.md#internal-cmdlets)voor meer informatie.

| Interne cmdlet | Beschrijving |
|:---|:---|
|`Get-AutomationCertificate`|Haalt een certificaat op dat moet worden gebruikt in een runbook-of DSC-configuratie. Retourneert een [System. Security. Cryptography. X509Certificates. X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) -object.|

> [!NOTE] 
> Vermijd het gebruik van variabelen in de `Name` para meter van `Get-AutomationCertificate` in een RUNBOOK of DSC-configuratie. Dergelijke variabelen kunnen de detectie van afhankelijkheden tussen runbooks of DSC-configuraties en Automation-variabelen tijdens ontwerp tijd bemoeilijken.

## <a name="python-2-functions-to-access-certificates"></a>Python 2-functies voor toegang tot certificaten

Gebruik de functie in de volgende tabel voor toegang tot certificaten in een python 2-runbook.

| Functie | Beschrijving |
|:---|:---|
| `automationassets.get_automation_certificate` | Haalt informatie op over een certificaat Asset. |

> [!NOTE]
> U moet de `automationassets` module op het begin van het python-runbook importeren om toegang te krijgen tot de Asset-functies.

## <a name="create-a-new-certificate"></a>Een nieuw certificaat maken

Wanneer u een nieuw certificaat maakt, uploadt u een CER-of PFX-bestand naar Automation. Als u het certificaat als exporteerbaar markeert, kunt u het uit het Automation-certificaat archief overzetten. Als de service niet kan worden geëxporteerd, kan deze alleen worden gebruikt voor ondertekening binnen het runbook of de DSC-configuratie. Voor Automation is vereist dat het certificaat de provider **micro soft Enhanced RSA en AES Cryptographic Provider**heeft.

### <a name="create-a-new-certificate-with-the-azure-portal"></a>Een nieuw certificaat maken met de Azure Portal

1. Selecteer in uw Automation-account **activa**  >  **certificaten**  >  **een certificaat toevoegen**.
1. Typ in het veld **naam** een naam voor het certificaat.
1. Als u wilt bladeren naar een **CER** -of **PFX** -bestand, kiest u onder **een certificaat bestand uploaden**de optie **een bestand selecteren**. Als u een **PFX** -bestand selecteert, geeft u een wacht woord op en geeft u aan of het kan worden geëxporteerd.
1. Selecteer **maken** om het nieuwe certificaat activum op te slaan.

### <a name="create-a-new-certificate-with-powershell"></a>Een nieuw certificaat maken met Power shell

In het volgende voor beeld ziet u hoe u een nieuw Automation-certificaat maakt en dit markeert als exporteerbaar. In dit voor beeld wordt een bestaand **PFX** -bestand geïmporteerd.

```powershell-interactive
$certificateName = 'MyCertificate'
$PfxCertPath = '.\MyCert.pfx'
$CertificatePassword = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
$ResourceGroup = "ResourceGroup01"

New-AzAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certificateName -Path $PfxCertPath –Password $CertificatePassword -Exportable -ResourceGroupName $ResourceGroup
```

### <a name="create-a-new-certificate-with-a-resource-manager-template"></a>Een nieuw certificaat maken met een resource manager-sjabloon

In het volgende voor beeld ziet u hoe u een certificaat implementeert in uw Automation-account met behulp van een resource manager-sjabloon via Power shell:

```powershell-interactive
$AutomationAccountName = "<automation account name>"
$PfxCertPath = '<PFX cert path'
$CertificatePassword = '<password>'
$certificateName = '<certificate name>'
$AutomationAccountName = '<automation account name>'
$flags = [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::Exportable `
    -bor [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::PersistKeySet `
    -bor [System.Security.Cryptography.X509Certificates.X509KeyStorageFlags]::MachineKeySet
# Load the certificate into memory
$PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPath, $CertificatePassword, $flags)
# Export the certificate and convert into base 64 string
$Base64Value = [System.Convert]::ToBase64String($PfxCert.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Pkcs12))
$Thumbprint = $PfxCert.Thumbprint


$json = @"
{
    '`$schema': 'https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#',
    'contentVersion': '1.0.0.0',
    'resources': [
        {
            'name': '$AutomationAccountName/$certificateName',
            'type': 'Microsoft.Automation/automationAccounts/certificates',
            'apiVersion': '2015-10-31',
            'properties': {
                'base64Value': '$Base64Value',
                'thumbprint': '$Thumbprint',
                'isExportable': true
            }
        }
    ]
}
"@

$json | out-file .\template.json
New-AzResourceGroupDeployment -Name NewCert -ResourceGroupName TestAzureAuto -TemplateFile .\template.json
```

## <a name="get-a-certificate"></a>Een certificaat ophalen

Gebruik de interne cmdlet om een certificaat op te halen `Get-AutomationCertificate` . U kunt de cmdlet [Get-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationCertificate?view=azps-3.7.0) niet gebruiken, omdat deze informatie over de certificaat Asset retourneert, maar niet het certificaat zelf.

### <a name="textual-runbook-example"></a>Voor beeld van een tekst-runbook

In het volgende voor beeld ziet u hoe u een certificaat toevoegt aan een Cloud service in een runbook. In dit voor beeld wordt het wacht woord opgehaald uit een versleutelde automatiserings variabele.

```powershell-interactive
$serviceName = 'MyCloudService'
$cert = Get-AutomationCertificate -Name 'MyCertificate'
$certPwd = Get-AzAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert
```

### <a name="graphical-runbook-example"></a>Voor beeld van grafisch runbook

Voeg een activiteit voor de interne `Get-AutomationCertificate` cmdlet toe aan een grafisch runbook door met de rechter muisknop op het certificaat in het deel venster Bibliotheek te klikken en **toevoegen aan canvas**te selecteren.

![Scherm opname van het toevoegen van een certificaat aan het canvas](../media/certificates/automation-certificate-add-to-canvas.png)

In de volgende afbeelding ziet u een voor beeld van het gebruik van een certificaat in een grafisch runbook. 

![Scherm afbeelding van een voor beeld van grafisch ontwerpen](../media/certificates/graphical-runbook-add-certificate.png)

### <a name="python-2-example"></a>Python 2-voor beeld

Het volgende voor beeld laat zien hoe u toegang krijgt tot certificaten in Python 2-runbooks.

```python
# get a reference to the Azure Automation certificate
cert = automationassets.get_automation_certificate("AzureRunAsCertificate")

# returns the binary cert content  
print cert
```

## <a name="next-steps"></a>Volgende stappen

* Zie [modules beheren in azure Automation](modules.md)voor meer informatie over de cmdlets die worden gebruikt voor toegang tot certificaten.
* Zie voor algemene informatie over runbooks [Runbook-uitvoering in azure Automation](../automation-runbook-execution.md).
* Zie [Azure Automation status configuratie Overview](../automation-dsc-overview.md)(Engelstalig) voor meer informatie over DSC-configuraties. 

