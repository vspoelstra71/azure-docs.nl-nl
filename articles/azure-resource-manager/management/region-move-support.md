---
title: Ondersteuning voor het verplaatsen van Azure-resources in verschillende regio's
description: Een lijst met de Azure-resource typen die kunnen worden verplaatst over Azure-regio's
author: rayne-wiselman
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 01/20/2020
ms.author: raynew
ms.openlocfilehash: 9bc7dc66ccf3049ac878f7871c816e5ade1afde5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "76760705"
---
# <a name="support-for-moving-azure-resources-across-regions"></a>Ondersteuning voor het verplaatsen van Azure-resources in verschillende regio's

In dit artikel wordt bevestigd of een Azure-resource type wordt ondersteund voor het verplaatsen naar een andere Azure-regio. 

Ga naar de naam ruimte van een resource provider:
> [!div class="op_single_selector"]
> - [Micro soft. AAD](#microsoftaad)
> - [micro soft. aadiam](#microsoftaadiam)
> - [Micro soft. AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Micro soft. AppConfiguration](#microsoftappconfiguration)
> - [Micro soft. AppService](#microsoftappservice)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Micro soft. Azureworden](#microsoftazuredata)
> - [Micro soft. AzureStack](#microsoftazurestack)
> - [Microsoft.Batch](#microsoftbatch)
> - [Micro soft. BatchAI](#microsoftbatchai)
> - [Microsoft.BingMaps](#microsoftbingmaps)
> - [Micro soft. BizTalkServices](#microsoftbiztalkservices)
> - [Micro soft. Block Chain](#microsoftblockchain)
> - [Micro soft. blauw druk](#microsoftblueprint)
> - [Micro soft. BotService](#microsoftbotservice)
> - [Microsoft.Cache](#microsoftcache)
> - [Micro soft. CDN](#microsoftcdn)
> - [Microsoft.CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Micro soft. ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Compute](#microsoftcompute)
> - [Micro soft. container](#microsoftcontainer)
> - [Micro soft. ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.ContentModerator](#microsoftcontentmoderator)
> - [Micro soft. CortanaAnalytics](#microsoftcortanaanalytics)
> - [Micro soft. CostManagement](#microsoftcostmanagement)
> - [Microsoft.CustomerInsights](#microsoftcustomerinsights)
> - [Micro soft. CustomProviders](#microsoftcustomproviders)
> - [Micro soft. DataBox](#microsoftdatabox)
> - [Micro soft. DataBoxEdge](#microsoftdataboxedge)
> - [Micro soft. Databricks](#microsoftdatabricks)
> - [Microsoft.DataCatalog](#microsoftdatacatalog)
> - [Micro soft. DataConnect](#microsoftdataconnect)
> - [Micro soft. DataExchange](#microsoftdataexchange)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Micro soft. DataLake](#microsoftdatalake)
> - [Microsoft.DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft.DataLakeStore](#microsoftdatalakestore)
> - [Micro soft. DataMigration](#microsoftdatamigration)
> - [Micro soft. DataShare](#microsoftdatashare)
> - [Micro soft. DBforMariaDB](#microsoftdbformariadb)
> - [Micro soft. DBforMySQL](#microsoftdbformysql)
> - [Micro soft. DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Micro soft. DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft.Devices](#microsoftdevices)
> - [Micro soft. DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Micro soft. DomainRegistration](#microsoftdomainregistration)
> - [Micro soft. EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Micro soft. EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Micro soft. Genomics](#microsoftgenomics)
> - [Micro soft. HanaOnAzure](#microsofthanaonazure)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Micro soft. HealthcareApis](#microsofthealthcareapis)
> - [Micro soft. HybridCompute](#microsofthybridcompute)
> - [Micro soft. HybridData](#microsofthybriddata)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [micro soft. Insights](#microsoftinsights)
> - [Micro soft. IoTCentral](#microsoftiotcentral)
> - [Micro soft. IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Micro soft. LabServices](#microsoftlabservices)
> - [Micro soft. LocationBasedServices](#microsoftlocationbasedservices)
> - [Micro soft. bestand locationservices](#microsoftlocationservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [Micro soft. MachineLearningCompute](#microsoftmachinelearningcompute)
> - [Micro soft. MachineLearningExperimentation](#microsoftmachinelearningexperimentation)
> - [Micro soft. MachineLearningModelManagement](#microsoftmachinelearningmodelmanagement)
> - [Micro soft. MachineLearningOperationalization](#microsoftmachinelearningoperationalization)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Micro soft. ManagedIdentity](#microsoftmanagedidentity)
> - [Micro soft. Maps](#microsoftmaps)
> - [Micro soft. MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.Media](#microsoftmedia)
> - [Micro soft. Microservices4Spring](#microsoftmicroservices4spring)
> - [Micro soft. migrate](#microsoftmigrate)
> - [Micro soft. NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Micro soft. peering](#microsoftpeering)
> - [Micro soft. Portal](#microsoftportal)
> - [Micro soft. PortalSdk](#microsoftportalsdk)
> - [Micro soft. PowerBI](#microsoftpowerbi)
> - [Micro soft. PowerBIDedicated](#microsoftpowerbidedicated)
> - [Micro soft. ProjectOxford](#microsoftprojectoxford)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.Relay](#microsoftrelay)
> - [Micro soft. ResourceGraph](#microsoftresourcegraph)
> - [Micro soft. SaaS](#microsoftsaas)
> - [Microsoft.Scheduler](#microsoftscheduler)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.ServerManagement](#microsoftservermanagement)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Micro soft. ServiceFabric](#microsoftservicefabric)
> - [Micro soft. ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Micro soft. SignalRService](#microsoftsignalrservice)
> - [Micro soft. Solutions](#microsoftsolutions)
> - [Microsoft.Sql](#microsoftsql)
> - [Micro soft. SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Micro soft. SqlVM](#microsoftsqlvm)
> - [Microsoft.Storage](#microsoftstorage)
> - [Micro soft. StorageCache](#microsoftstoragecache)
> - [Micro soft. StorageSync](#microsoftstoragesync)
> - [Micro soft. StorageSyncDev](#microsoftstoragesyncdev)
> - [Micro soft. StorageSyncInt](#microsoftstoragesyncint)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Micro soft. StreamAnalyticsExplorer](#microsoftstreamanalyticsexplorer)
> - [Micro soft. TerraformOSS](#microsoftterraformoss)
> - [Micro soft. TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Micro soft. token](#microsofttoken)
> - [Micro soft. VirtualMachineImages](#microsoftvirtualmachineimages)
> - [micro soft. Visual Studio](#microsoftvisualstudio)
> - [Micro soft. VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [Microsoft.Web](#microsoftweb)
> - [Micro soft. WindowsIoT](#microsoftwindowsiot)
> - [Micro soft. WindowsVirtualDesktop](#microsoftwindowsvirtualdesktop)

## <a name="microsoftaad"></a>Micro soft. AAD

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- | 
> | domainservices | Nee | 
> | domainservices / replicasets | Nee | 

## <a name="microsoftaadiam"></a>micro soft. aadiam

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | tenants | Nee |

## <a name="microsoftalertsmanagement"></a>Micro soft. AlertsManagement

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | actionrules | Nee | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | Server | Nee |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | service |  Ja | 

## <a name="microsoftappconfiguration"></a>Micro soft. AppConfiguration

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | configurationstores | Nee | 

## <a name="microsoftappservice"></a>Micro soft. AppService

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | apiapps | Nee | 
> | appidentities | Nee | 
> | gateways | Nee | 


## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | policyassignments | Nee |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | automationaccounts | Nee | 
> | automationaccounts/configuraties | Nee | 
> | automationaccounts/runbooks | Nee | 



## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | b2cdirectories | Nee | 

## <a name="microsoftazuredata"></a>Micro soft. Azureworden

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | sqlserverregistrations | Nee |

## <a name="microsoftazurestack"></a>Micro soft. AzureStack

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | registraties | Nee | 

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | batchaccounts | Nee |

## <a name="microsoftbatchai"></a>Micro soft. BatchAI

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | clusters | Nee | 
> | fileservers | Nee | 
> | functies | Nee | 
> | werkruimten | Nee | 

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | mapapis | Nee | 

## <a name="microsoftbiztalkservices"></a>Micro soft. BizTalkServices

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | biztalk | Nee | 

## <a name="microsoftblockchain"></a>Micro soft. Block Chain

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | blockchainmembers | Nee |
> | Volg | Nee | 

## <a name="microsoftblueprint"></a>Micro soft. blauw druk

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | blueprintassignments | Nee | 

## <a name="microsoftbotservice"></a>Micro soft. BotService

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | botservices | Nee | 

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | redis | Nee | 


## <a name="microsoftcdn"></a>Micro soft. CDN

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | cdnwebapplicationfirewallpolicies | Nee |
> | profielen | Nee | 
> | profielen/eind punten | Nee | 

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | certificateorders | Nee | 


## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | domein naam | Nee |  
> | informatie | Nee | 



## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | networksecuritygroups | Nee |
> | reservedips | Nee | 
> | virtualnetworks | Nee | 

## <a name="microsoftclassicstorage"></a>Micro soft. ClassicStorage

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | Storage accounts | Ja |  


## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | accounts | Nee | 

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | Availability sets | Nee | 
> | diskencryptionsets | Nee | 
> | cd's | Nee | 
> | Galerij | Nee | 
> | galerieën/afbeeldingen | Nee | 
> | galerieën/afbeeldingen/versies | Nee | 
> | hostgroups | Nee | 
> | hostgroups/hosts | Nee | 
> | images | Nee | 
> | proximityplacementgroups | Nee | 
> | restorepointcollections | Nee | 
> | sharedvmimages | Nee | 
> | sharedvmimages/versies | Nee | 
> | momentopnamen | Nee | 
> | informatie | Ja | 
> | informatie/extensies | Nee | 
> | virtualmachinescalesets | Nee | 

## <a name="microsoftcontainer"></a>Micro soft. container

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | containergroups | Nee | 

## <a name="microsoftcontainerinstance"></a>Micro soft. ContainerInstance

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | containergroups | Nee | 

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | registers | Nee |  
> | registers/buildtasks | Nee |  
> | registers/replicaties | Nee | 
> | registers/taken | Nee |  
> | registers/webhooks | Nee | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | containerservices | Nee | 
> | managedclusters | Nee | 
> | openshiftmanagedclusters | Nee | 

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | toepassingen | Nee | 

## <a name="microsoftcortanaanalytics"></a>Micro soft. CortanaAnalytics

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | accounts | Nee | 

## <a name="microsoftcostmanagement"></a>Micro soft. CostManagement

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | connectoren | Nee |  

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | hubs | Nee |  

## <a name="microsoftcustomproviders"></a>Micro soft. CustomProviders

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | resourceproviders | Nee | 

## <a name="microsoftdatabox"></a>Micro soft. DataBox

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | functies | Nee | 

## <a name="microsoftdataboxedge"></a>Micro soft. DataBoxEdge

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | databoxedgedevices | Nee | 

## <a name="microsoftdatabricks"></a>Micro soft. Databricks

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | werkruimten | Nee | 

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | catalogi | Nee | 
> | datacatalogs | Nee | 

## <a name="microsoftdataconnect"></a>Micro soft. DataConnect

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | connectionmanagers | Nee | 

## <a name="microsoftdataexchange"></a>Micro soft. DataExchange

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | Pakketten | Nee | 
> | plant | Nee | 

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | datafactories | Nee | 
> | factory's | Nee |  

## <a name="microsoftdatalake"></a>Micro soft. DataLake

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | datalakeaccounts | Nee | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | accounts | Nee | 

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | accounts | Nee | 

## <a name="microsoftdatamigration"></a>Micro soft. DataMigration

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | services | Nee | 
> | Services/projecten | Nee | 
> | sleuf | Nee | 

## <a name="microsoftdatashare"></a>Micro soft. DataShare

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | accounts | Nee | 

## <a name="microsoftdbformariadb"></a>Micro soft. DBforMariaDB

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | Server | Nee |  

## <a name="microsoftdbformysql"></a>Micro soft. DBforMySQL

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | Server | Nee |  

## <a name="microsoftdbforpostgresql"></a>Micro soft. DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | servergroups | Nee | 
> | Server | Nee |  
> | serversv2 | Nee | 

## <a name="microsoftdeploymentmanager"></a>Micro soft. DeploymentManager

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | artifactsources | Nee | 
> | implementaties | Nee |  
> | servicetopologies | Nee | 
> | servicetopologies/Services | Nee |  
> | servicetopologies/Services/serviceunits | Nee | 
> | stappen | Nee | 

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | elasticpools | Nee | 
> | elasticpools / iothubtenants | Nee | 
> | iothubs | Ja | 
> | provisioningservices | Nee | 

## <a name="microsoftdevspaces"></a>Micro soft. DevSpaces

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | fungeren | Nee | 

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | labcenters | Nee | 
> | Labs | Nee | 
> | Labs/omgevingen | Nee |  
> | Labs-servicerunners | Nee | 
> | Labs-informatie | Nee |  
> | schema's | Nee |  

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | databaseaccounts | Nee | 

## <a name="microsoftdomainregistration"></a>Micro soft. DomainRegistration

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | domeinen | Nee | 

## <a name="microsoftenterpriseknowledgegraph"></a>Micro soft. EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | services | Nee |  

## <a name="microsofteventgrid"></a>Micro soft. EventGrid

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | domeinen | Nee |  
> | onderwerp | Nee | 

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | clusters | Nee |  
> | naam ruimten | Nee | 

## <a name="microsoftgenomics"></a>Micro soft. Genomics

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | accounts | Nee | 

## <a name="microsofthanaonazure"></a>Micro soft. HanaOnAzure

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | hanainstances | Nee | 
> | sapmonitors | Nee |  

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | clusters | Nee | 

## <a name="microsofthealthcareapis"></a>Micro soft. HealthcareApis

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | services | Nee |  

## <a name="microsofthybridcompute"></a>Micro soft. HybridCompute

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | apparaten | Nee | 

## <a name="microsofthybriddata"></a>Micro soft. HybridData

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | datamanagers |  Nee | 

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | functies |  Nee | 

## <a name="microsoftinsights"></a>micro soft. Insights

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | accounts | Nee | 
> | actiongroups |  Nee | 
> | activitylogalerts | Nee | 
> | alertrules |  Nee | 
> | autoscalesettings |  Nee | 
> | materialen |  Nee |  
> | guestdiagnosticsettings | Nee | 
> | metricalerts | Nee | 
> | notificationgroups | Nee | 
> | notificationrules | Nee | 
> | scheduledqueryrules |  Nee | 
> | webtests |  Nee | 
> | werkmappen |  Nee |  


## <a name="microsoftiotcentral"></a>Micro soft. IoTCentral

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | iotapps |  Nee |  

## <a name="microsoftiotspaces"></a>Micro soft. IoTSpaces

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | checknameavailability |  Nee |  
> | Graph |  Nee | 

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | hsmpools | Nee | 
> | kluizen |  Nee | 


## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | clusters |  Nee |  

## <a name="microsoftlabservices"></a>Micro soft. LabServices

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | labaccounts | Nee | 

## <a name="microsoftlocationbasedservices"></a>Micro soft. LocationBasedServices

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | accounts | Nee | 

## <a name="microsoftlocationservices"></a>Micro soft. bestand locationservices

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | accounts | Nee | 

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | hostingenvironments | Nee | 
> | integrationaccounts |  Nee |  
> | integrationserviceenvironments | Nee | 
> | isolatedenvironments | Nee | 
> | stroom |  Nee |  

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | commitmentplans |  Nee | 
> | webservices |  Nee | 
> | werkruimten |  Nee | 

## <a name="microsoftmachinelearningcompute"></a>Micro soft. MachineLearningCompute

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | operationalizationclusters |  Nee | 

## <a name="microsoftmachinelearningexperimentation"></a>Micro soft. MachineLearningExperimentation

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | accounts | Nee | 
> | accounts/werk ruimten | Nee | 
> | accounts/werk ruimten/projecten | Nee | 
> | teamaccounts | Nee | 
> | teamaccounts/werk ruimten | Nee | 
> | teamaccounts/werk ruimten/projecten | Nee | 

## <a name="microsoftmachinelearningmodelmanagement"></a>Micro soft. MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | accounts | Nee | 

## <a name="microsoftmachinelearningoperationalization"></a>Micro soft. MachineLearningOperationalization

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | hostingaccounts | Nee | 

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | werkruimten | Nee | 

## <a name="microsoftmanagedidentity"></a>Micro soft. ManagedIdentity

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | userassignedidentities | Nee | 

## <a name="microsoftmaps"></a>Micro soft. Maps

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | accounts |  Nee |  

## <a name="microsoftmarketplaceapps"></a>Micro soft. MarketplaceApps

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | classicdevservices | Nee | 

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | Media Services |  Nee | 
> | Media Services/liveevents |  Nee | 
> | Media Services/streamingendpoints |  Nee | 

## <a name="microsoftmicroservices4spring"></a>Micro soft. Microservices4Spring

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | appclusters | Nee | 

## <a name="microsoftmigrate"></a>Micro soft. migrate

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | assessmentprojects | Nee | 
> | migrateprojects | Nee | 
> | projecten | Nee | 

## <a name="microsoftnetapp"></a>Micro soft. NetApp

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | netappaccounts | Nee | 
> | netappaccounts / capacitypools | Nee | 
> | netappaccounts/capacitypools/volumes | Nee | 
> | netappaccounts/capacitypools/volumes/mounttargets | Nee | 
> | netappaccounts/capacitypools/volumes/moment opnamen | Nee | 

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | applicationgateways | Nee | 
> | applicationgatewaywebapplicationfirewallpolicies | Nee | 
> | applicationsecuritygroups |  Nee |  
> | azurefirewalls |  Nee |  
> | bastionhosts | Nee | 
> | inbel |  Nee | 
> | ddoscustompolicies |  Nee | 
> | ddosprotectionplans | Nee | 
> | dnszones |  Nee | 
> | expressroutecircuits | Nee | 
> | expressroutecrossconnections | Nee | 
> | expressroutegateways | Nee | 
> | expressrouteports | Nee | 
> | frontdoors | Nee | 
> | frontdoorwebapplicationfirewallpolicies | Nee | 
> | loadbalancers | Ja, basis-SKU<br>Geen standaard-SKU | Ja, basis-SKU<br> -Ja standaard-SKU |
> | localnetworkgateways |  Nee | 
> | natgateways |  Nee | 
> | networkintentpolicies |  Nee | 
> | networkinterfaces | Ja | 
> | networkprofiles | Nee | 
> | networksecuritygroups | Ja | 
> | networkwatchers |  Nee |  
> | networkwatchers / connectionmonitors |  Nee | 
> | networkwatchers/lenzen |  Nee | 
> | networkwatchers / pingmeshes |  Nee | 
> | p2svpngateways | Nee | 
> | privatednszones |  Nee |  
> | privatednszones / virtualnetworklinks |  Nee |  
> | privateendpoints | Nee | 
> | privatelinkservices | Nee | 
> | publicipaddresses | Ja, basis-SKU<br>Geen standaard-SKU | Ja, basis-SKU<br>Geen standaard-SKU |
> | publicipprefixes | Nee | 
> | routefilters | Nee | 
> | routetables |  Nee | 
> | serviceendpointpolicies |  Nee | 
> | trafficmanagerprofiles |  Nee | 
> | virtualhubs | Nee | 
> | virtualnetworkgateways |  Nee |  
> | virtualnetworks |  Nee | 
> | virtualnetworktaps | Nee | 
> | virtualwans | Nee | 
> | vpngateways (virtueel WAN) | Nee | 
> | vpnsites (virtueel WAN) | Nee | 
> | webapplicationfirewallpolicies |  Nee | 


## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | naam ruimten |  Nee | 
> | naam ruimten/notification hubs |  Nee |  

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | werkruimten |  Nee | 



## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | managementconfigurations |  Nee | 
> | Weergaven |  Nee | 

## <a name="microsoftpeering"></a>Micro soft. peering

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | Peerings | Nee | 

## <a name="microsoftportal"></a>Micro soft. Portal

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | dashboards | Nee | 

## <a name="microsoftportalsdk"></a>Micro soft. PortalSdk

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | rootresources | Nee | 

## <a name="microsoftpowerbi"></a>Micro soft. PowerBI

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | workspacecollections |  Nee | 

## <a name="microsoftpowerbidedicated"></a>Micro soft. PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | beschikt |  Nee | 

## <a name="microsoftprojectoxford"></a>Micro soft. ProjectOxford

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | accounts | Nee | 

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | kluizen | Nee. [Kluis uitschakelen en opnieuw maken](https://docs.microsoft.com/azure/site-recovery/move-vaults-across-regions) voor site Recovery  | 


## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | naam ruimten |  Nee | 

## <a name="microsoftresourcegraph"></a>Micro soft. ResourceGraph

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | aanvragen |  Nee |  

## <a name="microsoftsaas"></a>Micro soft. SaaS

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | toepassingen |  Nee | 

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | flows |  Nee |  
> | jobcollections |  Nee | 

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | searchservices |  Nee | 


## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | iotsecuritysolutions |  Nee | 
> | playbookconfigurations | Nee | 

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | gateways | Nee | 
> | punt | Nee | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | naam ruimten |  Nee | 

## <a name="microsoftservicefabric"></a>Micro soft. ServiceFabric

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | toepassingen | Nee | 
> | clusters |  Nee | 
> | clusters/toepassingen | Nee | 
> | containergroups | Nee | 
> | containergroupsets | Nee | 
> | edgeclusters | Nee | 
> | netwerken | Nee | 
> | secretstores | Nee | 
> | volumes | Nee | 

## <a name="microsoftservicefabricmesh"></a>Micro soft. ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | toepassingen |  Nee | 
> | containergroups | Nee | 
> | gateways |  Nee | 
> | netwerken |  Nee | 
> | geheimen |  Nee | 
> | volumes |  Nee |  

## <a name="microsoftsignalrservice"></a>Micro soft. SignalRService

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | signalr |  Nee |  

## <a name="microsoftsolutions"></a>Micro soft. Solutions

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | appliancedefinitions | Nee | 
> | uitrusting | Nee | 
> | applicationdefinitions | Nee | 
> | toepassingen | Nee | 
> | jitrequests | Nee | 

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | instancepools | Nee | 
> | managedinstances | Ja | 
> | managedinstances/data bases | Ja | 
> | Server | Ja | 
> | servers/data bases | Ja | 
> | servers/elasticpools | Ja | 
> | virtualclusters | Ja | 

## <a name="microsoftsqlvirtualmachine"></a>Micro soft. SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | sqlvirtualmachinegroups |  Nee |  
> | sqlvirtualmachines |  Nee |  

## <a name="microsoftsqlvm"></a>Micro soft. SqlVM

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | dwvm | Nee | 

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | Storage accounts | Ja | 

## <a name="microsoftstoragecache"></a>Micro soft. StorageCache

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | caches | Nee | 

## <a name="microsoftstoragesync"></a>Micro soft. StorageSync

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | storagesyncservices |  Nee | 

## <a name="microsoftstoragesyncdev"></a>Micro soft. StorageSyncDev

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | storagesyncservices | Nee | 

## <a name="microsoftstoragesyncint"></a>Micro soft. StorageSyncInt

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | storagesyncservices | Nee | 

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | leider | Nee | 

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | streamingjobs |  Nee |  


## <a name="microsoftstreamanalyticsexplorer"></a>Micro soft. StreamAnalyticsExplorer

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | verschillend | Nee | 
> | omgevingen/eventsources | Nee | 
> | vaak | Nee | 
> | exemplaren/omgevingen | Nee | 
> | exemplaren/omgevingen/eventsources | Nee | 

## <a name="microsoftterraformoss"></a>Micro soft. TerraformOSS

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | providerregistrations | Nee | 
> | resources | Nee | 

## <a name="microsofttimeseriesinsights"></a>Micro soft. TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | verschillend |  Nee | 
> | omgevingen/eventsources |  Nee |  
> | omgevingen/referencedatasets |  Nee | 

## <a name="microsofttoken"></a>Micro soft. token

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | opslaat | Nee | 

## <a name="microsoftvirtualmachineimages"></a>Micro soft. VirtualMachineImages

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | imagetemplates | Nee | 

## <a name="microsoftvisualstudio"></a>micro soft. Visual Studio

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | account |  Nee | 
> | account/extensie |  Nee | 
> | account/project |  Nee | 



## <a name="microsoftvmwarecloudsimple"></a>Micro soft. VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | dedicatedcloudnodes | Nee | 
> | dedicatedcloudservices | Nee | 
> | informatie | Nee | 

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | certificaten | Nee | 
> | connectiongateways |  Nee |  
> | inbel |  Nee |  
> | customapis |  Nee | 
> | hostingenvironments | Nee | 
> | server farms |  Nee |  
> | sites |  Nee | 
> | sites/premieraddons |  Nee |  
> | sites/sleuven |  Nee |  


## <a name="microsoftwindowsiot"></a>Micro soft. WindowsIoT

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | deviceservices | Nee | 

## <a name="microsoftwindowsvirtualdesktop"></a>Micro soft. WindowsVirtualDesktop

> [!div class="mx-tableFixed"]
> | Resourcetype | Regio verplaatsen | 
> | ------------- | ----------- |
> | applicationgroups | Nee | 
> | hostpools | Nee | 
> | werkruimten | Nee | 

## <a name="third-party-services"></a>Services van derden

Services van derden bieden momenteel geen ondersteuning voor de verplaatsings bewerking.
