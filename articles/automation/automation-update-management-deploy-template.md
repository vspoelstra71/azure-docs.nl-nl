---
title: Updatebeheer met Azure Resource Manager sjabloon inschakelen | Microsoft Docs
description: In dit artikel leest u hoe u een Azure Resource Manager sjabloon kunt gebruiken om Updatebeheer in te scha kelen.
ms.service: automation
ms.subservice: update-management
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 04/24/2020
ms.openlocfilehash: 0a83117d6d58f45d6ee1de2b8d61c2157738fc75
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/25/2020
ms.locfileid: "83830988"
---
# <a name="enable-update-management-using-azure-resource-manager-template"></a>Updatebeheer inschakelen met behulp van Azure Resource Manager-sjabloon

U kunt een [Azure Resource Manager sjabloon](../azure-resource-manager/templates/template-syntax.md) gebruiken om de functie Azure Automation updatebeheer in uw resource groep in te scha kelen. Dit artikel bevat een voorbeeld sjabloon waarmee het volgende kan worden geautomatiseerd:

* Het maken van een Azure Monitor Log Analytics werk ruimte.
* Het maken van een Azure Automation-account.
* Koppel het Automation-account aan de Log Analytics-werk ruimte, als dat nog niet is gekoppeld.
* Updatebeheer inschakelen.

Met de sjabloon wordt het inschakelen van een of meer virtuele Azure-of niet-Azure-Vm's niet geautomatiseerd.

Als u al een Log Analytics werk ruimte en een Automation-account hebt geïmplementeerd in een ondersteunde regio in uw abonnement, zijn deze niet gekoppeld. Updatebeheer is nog niet ingeschakeld voor de werk ruimte. Met deze sjabloon maakt u de koppeling en implementeert u Updatebeheer voor uw virtuele machines. 

>[!NOTE]
>De **nxautomation** -gebruiker ingeschakeld als onderdeel van updatebeheer op Linux voert alleen ondertekende runbooks uit.

## <a name="api-versions"></a>API-versies

De volgende tabel geeft een lijst van de API-versies voor de resources die in deze sjabloon worden gebruikt.

| Resource | Resourcetype | API-versie |
|:---|:---|:---|
| Werkruimte | werkruimten | 2017-03-15-preview |
| Automation-account | automation | 2015-10-31 | 
| Oplossing | oplossingen | 2015-11-01-preview |

## <a name="before-using-the-template"></a>Voordat u de sjabloon gebruikt

Als u Power shell lokaal wilt installeren en gebruiken, is voor dit artikel de Azure PowerShell AZ-module vereist. Voer `Get-Module -ListAvailable Az` uit om de versie te bekijken. Zie [De Azure PowerShell-module installeren](/powershell/azure/install-az-ps) als u een upgrade wilt uitvoeren. Als u Power shell lokaal uitvoert, moet u ook [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.7.0) uitvoeren om een verbinding te maken met Azure. Met Azure PowerShell maakt de implementatie gebruik van [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment).

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel de Azure CLI-versie 2.1.0 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Met Azure CLI maakt deze implementatie gebruik van [AZ Group Deployment Create](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create). 

De JSON-sjabloon is zo geconfigureerd dat u wordt gevraagd om:

* De naam van de werk ruimte
* De regio waarin de werk ruimte moet worden gemaakt
* De naam van het Automation-account
* De regio waarin het account moet worden gemaakt

Met de JSON-sjabloon wordt een standaard waarde opgegeven voor de andere para meters die waarschijnlijk zullen worden gebruikt voor een standaard configuratie in uw omgeving. U kunt de sjabloon opslaan in een Azure-opslag account voor gedeelde toegang in uw organisatie. Zie [resources implementeren met Resource Manager-sjablonen en Azure cli](../azure-resource-manager/templates/deploy-cli.md)voor meer informatie over het werken met sjablonen.

De volgende para meters in de sjabloon zijn ingesteld met een standaard waarde voor de Log Analytics-werk ruimte:

* SKU: wordt standaard ingesteld op de nieuwe prijs categorie per GB die is uitgebracht in het prijs model van april 2018
* bewaren van gegevens-standaard ingesteld op dertig dagen
* capaciteits reservering: de standaard instelling is 100 GB

>[!WARNING]
>Als u een Log Analytics-werk ruimte maakt of configureert in een abonnement dat is aangemeld met het nieuwe prijs model van april 2018, is de enige geldige Log Analytics prijs categorie **PerGB2018**.
>

Met de JSON-sjabloon wordt een standaard waarde opgegeven voor de andere para meters die waarschijnlijk worden gebruikt als een standaard configuratie in uw omgeving. U kunt de sjabloon opslaan in een Azure-opslag account voor gedeelde toegang in uw organisatie. Zie [resources implementeren met Resource Manager-sjablonen en Azure cli](../azure-resource-manager/templates/deploy-cli.md)voor meer informatie over het werken met sjablonen.

Het is belang rijk om de volgende configuratie gegevens te begrijpen als u geen ervaring hebt met Azure Automation en Azure Monitor, om fouten te voor komen bij het maken, configureren en gebruiken van een Log Analytics werkruimte die aan uw nieuwe Automation-account is gekoppeld.

* Bekijk [aanvullende informatie](../azure-monitor/platform/template-workspace-configuration.md#create-a-log-analytics-workspace) om de configuratie opties voor de werk ruimte volledig te begrijpen, zoals de toegangs beheer modus, de prijs categorie en de retentie en het capaciteits reserverings niveau.

* Omdat alleen bepaalde regio's worden ondersteund voor het koppelen van een Log Analytics-werk ruimte en een Automation-account in uw abonnement, raadpleegt u [werkruimte toewijzingen](how-to/region-mappings.md) om de ondersteunde regio's inline of in een parameter bestand op te geven.

* Als u geen ervaring hebt met Azure Monitor-logboeken en nog geen werk ruimte hebt geïmplementeerd, raadpleegt u de richt lijnen voor het ontwerpen van de [werk ruimte](../azure-monitor/platform/design-logs-deployment.md) voor meer informatie over toegangs beheer en inzicht in de ontwerp implementatie strategieën die voor uw organisatie worden aanbevolen.

## <a name="deploy-template"></a>Sjabloon implementeren

1. Kopieer en plak de volgende JSON-syntaxis in het bestand:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "Workspace name"
            }
        },
        "sku": {
            "type": "string",
            "allowedValues": [
                "pergb2018",
                "Free",
                "Standalone",
                "PerNode",
                "Standard",
                "Premium"
            ],
            "defaultValue": "pergb2018",
            "metadata": {
                "description": "Pricing tier: perGB2018 or legacy tiers (Free, Standalone, PerNode, Standard or Premium) which are not available to all customers."
            }
        },
        "dataRetention": {
            "type": "int",
            "defaultValue": 30,
            "minValue": 7,
            "maxValue": 730,
            "metadata": {
                "description": "Number of days of retention. Workspaces in the legacy Free pricing tier can only have 7 days."
            }
        },
        "immediatePurgeDataOn30Days": {
            "type": "bool",
            "defaultValue": "[bool('false')]",
            "metadata": {
                "description": "If set to true when changing retention to 30 days, older data will be immediately deleted. Use this with extreme caution. This only applies when retention is being set to 30 days."
            }
        },
        "location": {
            "type": "string",
            "metadata": {
                "description": "Specifies the location in which to create the workspace."
            }
        },
        "automationAccountName": {
            "type": "string",
            "metadata": {
                "description": "Automation account name"
            }
        },
        "automationAccountLocation": {
            "type": "string",
            "metadata": {
                "description": "Specify the location in which to create the Automation account."
            }
        }
    },
    "variables": {
        "Updates": {
            "name": "[concat('Updates', '(', parameters('workspaceName'), ')')]",
            "galleryName": "Updates"
        }
    },
    "resources": [
        {
        "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2017-03-15-preview",
            "location": "[parameters('location')]",
            "properties": {
                "sku": {
                    "Name": "[parameters('sku')]",
                    "name": "CapacityReservation",
                    "capacityReservationLevel": 100
                },
                "retentionInDays": "[parameters('dataRetention')]",
                "features": {
                    "searchVersion": 1,
                    "legacy": 0,
                    "enableLogAccessUsingOnlyResourcePermissions": true
                }
            },
            "resources": [
                {
                    "apiVersion": "2015-11-01-preview",
                    "location": "[resourceGroup().location]",
                    "name": "[variables('Updates').name]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('Updates').name)]",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
                    },
                    "plan": {
                        "name": "[variables('Updates').name]",
                        "publisher": "Microsoft",
                        "promotionCode": "",
                        "product": "[concat('OMSGallery/', variables('Updates').galleryName)]"
                    }
                }
            ]
        },
        {
            "type": "Microsoft.Automation/automationAccounts",
            "apiVersion": "2015-01-01-preview",
            "name": "[parameters('automationAccountName')]",
            "location": "[parameters('automationAccountLocation')]",
            "dependsOn": [],
            "tags": {},
            "properties": {
                "sku": {
                    "name": "Basic"
                }
            },
        },
        {
            "apiVersion": "2015-11-01-preview",
            "type": "Microsoft.OperationalInsights/workspaces/linkedServices",
            "name": "[concat(parameters('workspaceName'), '/' , 'Automation')]",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
                "[concat('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))]"
            ],
            "properties": {
                "resourceId": "[resourceId('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))]"
            }
        }
      ]
    }
    ```

2. Bewerk de sjabloon om te voldoen aan uw vereisten. Overweeg om een [Resource Manager-parameter bestand](../azure-resource-manager/templates/parameter-files.md) te maken in plaats van para meters door te geven als inline-waarden.

3. Sla dit bestand op in een lokale map als **deployUMSolutiontemplate. json**.

4. U kunt deze sjabloon nu implementeren. U kunt Power shell of de Azure CLI gebruiken. Wanneer u wordt gevraagd om een naam voor de werk ruimte en het Automation-account, geeft u een naam op die wereld wijd uniek is voor alle Azure-abonnementen.

    **PowerShell**

    ```powershell
    New-AzResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile deployUMSolutiontemplate.json
    ```

    **Azure-CLI**

    ```cli
    az group deployment create --resource-group <my-resource-group> --name <my-deployment-name> --template-file deployUMSolutiontemplate.json
    ```

    De implementatie kan enkele minuten duren. Wanneer de bewerking is voltooid, ziet u een bericht dat lijkt op het volgende:

    ![Voor beeld van resultaat wanneer de implementatie is voltooid](media/automation-update-management-deploy-template/template-output.png)

## <a name="next-steps"></a>Volgende stappen

* Zie [Updates en patches voor uw Azure-VM's beheren](automation-tutorial-update-management.md) voor het gebruiken van Updatebeheer voor VM's.
* Als u de Log Analytics-werkruimte niet meer nodig hebt, raadpleegt u de instructies in [Werkruimte ontkoppelen van Automation-account voor Updatebeheer](automation-unlink-workspace-update-management.md).
* Als u VM's uit Updatebeheer wilt verwijderen, raadpleegt u [VM's uit Updatebeheer verwijderen](automation-remove-vms-from-update-management.md).
* Zie [Problemen met Updatebeheer oplossen](troubleshoot/update-management.md) voor meer informatie over het oplossen van algemene Updatebeheer-fouten.
* Zie [Problemen met Windows-updateagent oplossen](troubleshoot/update-agent-issues.md)voor informatie over het oplossen van problemen met de Windows Update-agent.
* Zie [problemen met Linux Update-agent oplossen](troubleshoot/update-agent-issues-linux.md)voor informatie over het oplossen van problemen met de Linux Update-agent.