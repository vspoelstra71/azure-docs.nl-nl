---
title: Op rollen gebaseerd toegangsbeheer in Azure Cosmos DB
description: Meer informatie over hoe Azure Cosmos DB database beveiliging biedt met Active Directory-integratie (RBAC).
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: mjbrown
ms.openlocfilehash: 4e028e7a5e7e7b8f747d7a1cfb36c553a8113544
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "82583729"
---
# <a name="role-based-access-control-in-azure-cosmos-db"></a>Op rollen gebaseerd toegangsbeheer in Azure Cosmos DB

Azure Cosmos DB biedt ingebouwde op rollen gebaseerd toegangs beheer (RBAC) voor algemene beheer scenario's in Azure Cosmos DB. Een persoon met een profiel in Azure Active Directory kan deze RBAC-rollen toewijzen aan gebruikers, groepen, service-principals of beheerde identiteiten voor het verlenen of weigeren van toegang tot resources en bewerkingen op Azure Cosmos DB-resources. Roltoewijzingen zijn alleen gericht op toegangs beheer, waaronder toegang tot Azure Cosmos-accounts, data bases, containers en aanbiedingen (door Voer).

## <a name="built-in-roles"></a>Ingebouwde rollen

Hier volgen de ingebouwde rollen die door Azure Cosmos DB worden ondersteund:

|**Ingebouwde rol**  |**Beschrijving**  |
|---------|---------|
|[Inzender voor DocumentDB-accounts](../role-based-access-control/built-in-roles.md#documentdb-account-contributor)|Kan Azure Cosmos DB accounts beheren.|
|[Lezer van Cosmos DB-account](../role-based-access-control/built-in-roles.md#cosmos-db-account-reader-role)|Kan gegevens van Azure Cosmos DB-account lezen.|
|[Cosmos-back-upoperator](../role-based-access-control/built-in-roles.md#cosmosbackupoperator)|Kan een herstel aanvraag indienen voor een Azure Cosmos-data base of een container.|
|[Cosmos DB-operator](../role-based-access-control/built-in-roles.md#cosmos-db-operator)|Kan Azure Cosmos-accounts,-data bases en-containers inrichten, maar heeft geen toegang tot de sleutels die nodig zijn voor toegang tot de gegevens.|

> [!IMPORTANT]
> RBAC-ondersteuning in Azure Cosmos DB is alleen van toepassing op beheer vlak bewerkingen. Gegevensvlak bewerkingen worden beveiligd met behulp van hoofd sleutels of bron tokens. Zie [beveiligde toegang tot gegevens in azure Cosmos DB](secure-access-to-data.md) voor meer informatie.

## <a name="identity-and-access-management-iam"></a>Identiteits-en toegangs beheer (IAM)

Het deel venster **toegangs beheer (IAM)** in de Azure portal wordt gebruikt voor het configureren van toegangs beheer op basis van rollen op Azure Cosmos-resources. De rollen worden toegepast op gebruikers, groepen, service-principals en beheerde identiteiten in Active Directory. U kunt ingebouwde rollen of aangepaste rollen gebruiken voor individuen en groepen. De volgende scherm afbeelding toont Active Directory Integration (RBAC) met behulp van toegangs beheer (IAM) in de Azure Portal:

![Toegangs beheer (IAM) in de Azure Portal-demonstrerende database beveiliging](./media/role-based-access-control/database-security-identity-access-management-rbac.png)

## <a name="custom-roles"></a>Aangepaste rollen

Naast de ingebouwde rollen kunnen gebruikers ook [aangepaste rollen](../role-based-access-control/custom-roles.md) maken in Azure en deze rollen Toep assen op service-principals in alle abonnementen binnen hun Active Directory Tenant. Aangepaste rollen bieden gebruikers een manier om RBAC-roldefinities te maken met een aangepaste set bewerkingen van de resource provider. Als u wilt weten welke bewerkingen beschikbaar zijn voor het maken van aangepaste rollen voor Azure Cosmos DB raadpleegt u [Azure Cosmos DB resource provider bewerkingen](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)

## <a name="preventing-changes-from-cosmos-sdk"></a>Wijzigingen van de Cosmos-SDK voor komen

De resource provider Cosmos kan worden vergrendeld om te voor komen dat bronnen worden gewijzigd, zoals Cosmos-account, data bases, containers en door Voer van clients die verbinding maken via account sleutels (bijvoorbeeld toepassingen die verbinding maken via Cosmos SDK). Als deze instelling is ingesteld, moeten wijzigingen aan resources afkomstig zijn van een gebruiker met de juiste RBAC-rol en referenties. Deze mogelijkheid is ingesteld met `disableKeyBasedMetadataWriteAccess` eigenschaps waarde in de resource provider Cosmos. Hieronder vindt u een voor beeld van een Azure Resource Manager sjabloon met de instelling van deze eigenschap.

```json
{
    {
      "type": "Microsoft.DocumentDB/databaseAccounts",
      "name": "[variables('accountName')]",
      "apiVersion": "2019-08-01",
      "location": "[parameters('location')]",
      "kind": "GlobalDocumentDB",
      "properties": {
        "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
        "locations": "[variables('locations')]",
        "databaseAccountOfferType": "Standard",
        "disableKeyBasedMetadataWriteAccess": true
        }
    }
}
```
Als u een bestaande resource manager-sjabloon exporteert en deze bijwerkt met deze eigenschap, kan deze de functionaliteit van uw sjabloon volledig vervangen. Dus als niet alle waarden zijn opgenomen, worden deze teruggezet naar de standaard waarde. Een andere manier om de schrijf toegang op basis van sleutels uit te scha kelen, is door gebruik te maken van Azure CLI, zoals wordt weer gegeven in de volgende opdracht:

```cli
az cosmosdb update  --name CosmosDBAccountName --resource-group ResourceGroupName  --disable-key-based-metadata-write-access true

```

## <a name="next-steps"></a>Volgende stappen

- [Wat is op rollen gebaseerd toegangs beheer (RBAC) voor Azure-resources](../role-based-access-control/overview.md)
- [Aangepaste rollen voor Azure-resources](../role-based-access-control/custom-roles.md)
- [Bewerkingen voor de resource provider Azure Cosmos DB](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)
