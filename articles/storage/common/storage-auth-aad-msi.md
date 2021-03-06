---
title: Toegang tot gegevens met een beheerde identiteit autoriseren
titleSuffix: Azure Storage
description: Meer informatie over het gebruik van beheerde identiteiten voor Azure-resources om toegang te verlenen tot Blob-en wachtrij gegevens van toepassingen die worden uitgevoerd op virtuele machines van Azure, functie-apps, schaal sets voor virtuele machines en andere.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/04/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: f3bac0d47a53da1ec4d1fa08b5f0933f5f65dc56
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "79255338"
---
# <a name="authorize-access-to-blob-and-queue-data-with-managed-identities-for-azure-resources"></a>Toegang tot Blob-en wachtrij gegevens toestaan met beheerde identiteiten voor Azure-resources

Azure Blob-en Queue Storage ondersteunen Azure Active Directory-verificatie (Azure AD) met [beheerde identiteiten voor Azure-resources](../../active-directory/managed-identities-azure-resources/overview.md). Beheerde identiteiten voor Azure-resources kunnen toegang verlenen tot Blob-en wachtrij gegevens met behulp van Azure AD-referenties van toepassingen die worden uitgevoerd in azure virtual machines (Vm's), functie-apps, schaal sets voor virtuele machines en andere services. Door beheerde identiteiten voor Azure-resources te gebruiken in combi natie met Azure AD-verificatie kunt u voor komen dat referenties worden opgeslagen in uw toepassingen die in de cloud worden uitgevoerd.  

In dit artikel wordt beschreven hoe u toegang kunt verlenen tot BLOB-of wachtrij gegevens van een Azure-VM met beheerde identiteiten voor Azure-resources. Ook wordt beschreven hoe u uw code kunt testen in de ontwikkel omgeving.

## <a name="enable-managed-identities-on-a-vm"></a>Beheerde identiteiten op een virtuele machine inschakelen

Voordat u beheerde identiteiten voor Azure-resources kunt gebruiken om toegang te verlenen tot blobs en wacht rijen van uw VM, moet u eerst beheerde identiteiten voor Azure-resources inschakelen op de VM. Zie een van de volgende artikelen voor meer informatie over het inschakelen van beheerde identiteiten voor Azure-resources:

- [Azure Portal](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure-CLI](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager sjabloon](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Client bibliotheken Azure Resource Manager](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

Zie [beheerde identiteiten voor Azure-resources](../../active-directory/managed-identities-azure-resources/overview.md)voor meer informatie over beheerde identiteiten.

## <a name="authenticate-with-the-azure-identity-library"></a>Verifiëren met de Azure Identity-bibliotheek

De Azure Identity client-bibliotheek biedt ondersteuning voor Azure Azure AD-token verificatie voor de [Azure SDK](https://github.com/Azure/azure-sdk). De nieuwste versies van de Azure Storage-client bibliotheken voor .NET, Java, python en Java script zijn geïntegreerd met de Azure-identiteits bibliotheek om een OAuth 2,0-token te verkrijgen voor autorisatie van Azure Storage aanvragen.

Een voor deel van de Azure Identity client-bibliotheek is dat u dezelfde code kunt gebruiken om te verifiëren of uw toepassing wordt uitgevoerd in de ontwikkel omgeving of in Azure. De Azure Identity client-bibliotheek voor .NET verifieert een beveiligings-principal. Wanneer uw code wordt uitgevoerd in azure, is de beveiligingsprincipal een beheerde identiteit voor Azure-resources. In de ontwikkel omgeving bestaat de beheerde identiteit niet, zodat de client bibliotheek de gebruiker of een Service-Principal verifieert voor test doeleinden.

Na de verificatie krijgt de Azure Identity client-bibliotheek een token referentie. Deze token referentie wordt vervolgens ingekapseld in het service-client object dat u maakt om bewerkingen uit te voeren op basis van Azure Storage. De bibliotheek verwerkt dit voor u probleemloos door de juiste token referentie op te halen.

Zie de [Azure Identity client-bibliotheek voor .net](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/identity/Azure.Identity)voor meer informatie over de Azure Identity client-bibliotheek voor .net. Zie [Azure. Identity naam ruimte](/dotnet/api/azure.identity)voor referentie documentatie voor de Azure Identity client-bibliotheek.

### <a name="assign-role-based-access-control-rbac-roles-for-access-to-data"></a>RBAC-rollen (Role-based Access Control) toewijzen voor toegang tot gegevens

Wanneer een Azure AD-beveiligingsprincipal probeert toegang te krijgen tot BLOB-of wachtrij gegevens, moet die beveiligingsprincipal machtigingen hebben voor de resource. Of de beveiligingsprincipal een beheerde identiteit in azure of een Azure AD-gebruikers account voor het uitvoeren van code in de ontwikkel omgeving is, moet aan de beveiligingsprincipal een RBAC-rol worden toegewezen die toegang verleent tot BLOB-of wachtrij gegevens in Azure Storage. Voor informatie over het toewijzen van machtigingen via RBAC, zie de sectie **RBAC-rollen toewijzen voor toegangs rechten** in [toegang verlenen tot Azure-blobs en-wacht rijen met behulp van Azure Active Directory](../common/storage-auth-aad.md#assign-rbac-roles-for-access-rights).

### <a name="authenticate-the-user-in-the-development-environment"></a>De gebruiker verifiëren in de ontwikkel omgeving

Wanneer uw code wordt uitgevoerd in de ontwikkel omgeving, wordt de verificatie mogelijk automatisch verwerkt of is er mogelijk een browser aanmelding vereist, afhankelijk van de hulpprogram ma's die u gebruikt. Bijvoorbeeld: micro soft Visual Studio ondersteunt eenmalige aanmelding (SSO), zodat het actieve Azure AD-gebruikers account automatisch wordt gebruikt voor verificatie. Zie [eenmalige aanmelding bij toepassingen](../../active-directory/manage-apps/what-is-single-sign-on.md)voor meer informatie over SSO.

Andere ontwikkel hulpprogramma's vragen u mogelijk om u aan te melden via een webbrowser.

### <a name="authenticate-a-service-principal-in-the-development-environment"></a>Een Service-Principal verifiëren in de ontwikkel omgeving

Als uw ontwikkel omgeving geen ondersteuning biedt voor eenmalige aanmelding of aanmelding via een webbrowser, kunt u een Service-Principal gebruiken om te verifiëren vanuit de ontwikkel omgeving.

#### <a name="create-the-service-principal"></a>De service-principal maken

Roep de opdracht [AZ AD SP create-for-RBAC](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) om een service-principal te maken met Azure CLI en een RBAC-rol toe te wijzen. Geef een Azure Storage rol voor gegevens toegang op om toe te wijzen aan de nieuwe service-principal. Daarnaast kunt u het bereik voor de roltoewijzing opgeven. Zie [ingebouwde rollen voor Azure-resources](../../role-based-access-control/built-in-roles.md)voor meer informatie over de ingebouwde rollen voor Azure Storage.

Als u onvoldoende machtigingen hebt om een rol toe te wijzen aan de Service-Principal, moet u mogelijk de eigenaar van het account of de beheerder vragen de roltoewijzing uit te voeren.

In het volgende voor beeld wordt de Azure CLI gebruikt om een nieuwe service-principal te maken en de rol van **BLOB-gegevens lezer voor opslag** toe te wijzen aan het account bereik

```azurecli-interactive
az ad sp create-for-rbac \
    --name <service-principal> \
    --role "Storage Blob Data Reader" \
    --scopes /subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
```

De `az ad sp create-for-rbac` opdracht retourneert een lijst met Service-Principal-eigenschappen in JSON-indeling. Kopieer deze waarden zodat u ze kunt gebruiken om de vereiste omgevings variabelen in de volgende stap te maken.

```json
{
    "appId": "generated-app-ID",
    "displayName": "service-principal-name",
    "name": "http://service-principal-uri",
    "password": "generated-password",
    "tenant": "tenant-ID"
}
```

> [!IMPORTANT]
> RBAC-roltoewijzingen kunnen enkele minuten duren voordat deze wordt door gegeven.

#### <a name="set-environment-variables"></a>Omgevingsvariabelen instellen

De Azure Identity client-bibliotheek leest waarden uit drie omgevings variabelen tijdens runtime om de service-principal te verifiëren. De volgende tabel beschrijft de waarde die moet worden ingesteld voor elke omgevings variabele.

|Omgevingsvariabele|Waarde
|-|-
|`AZURE_CLIENT_ID`|De App-ID voor de Service-Principal
|`AZURE_TENANT_ID`|De Azure AD-Tenant-ID van de Service-Principal
|`AZURE_CLIENT_SECRET`|Het wacht woord dat voor de Service-Principal is gegenereerd

> [!IMPORTANT]
> Nadat u de omgevings variabelen hebt ingesteld, sluit u het console venster en opent u het opnieuw. Als u Visual Studio of een andere ontwikkel omgeving gebruikt, moet u de ontwikkel omgeving mogelijk opnieuw opstarten om de nieuwe omgevings variabelen te registreren.

Zie [identiteit voor Azure-app maken in de portal](../../active-directory/develop/howto-create-service-principal-portal.md)voor meer informatie.

[!INCLUDE [storage-install-packages-blob-and-identity-include](../../../includes/storage-install-packages-blob-and-identity-include.md)]

## <a name="net-code-example-create-a-block-blob"></a>.NET-code voorbeeld: een blok-Blob maken

Voeg de volgende `using` instructies toe aan uw code voor het gebruik van de identiteits-en Azure Storage-client bibliotheken van Azure.

```csharp
using Azure;
using Azure.Identity;
using Azure.Storage.Blobs;
using System;
using System.IO;
using System.Text;
using System.Threading.Tasks;
```

Maak een instantie van de [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential) -klasse om een token referentie op te halen die door uw code kan worden gebruikt om aanvragen voor Azure Storage te autoriseren. In het volgende code voorbeeld ziet u hoe de referenties van de geverifieerde token worden opgehaald en gebruikt om een service-client object te maken. vervolgens gebruikt u de service-client om een nieuwe BLOB te uploaden:

```csharp
async static Task CreateBlockBlobAsync(string accountName, string containerName, string blobName)
{
    // Construct the blob container endpoint from the arguments.
    string containerEndpoint = string.Format("https://{0}.blob.core.windows.net/{1}",
                                                accountName,
                                                containerName);

    // Get a credential and create a client object for the blob container.
    BlobContainerClient containerClient = new BlobContainerClient(new Uri(containerEndpoint),
                                                                    new DefaultAzureCredential());

    try
    {
        // Create the container if it does not exist.
        await containerClient.CreateIfNotExistsAsync();

        // Upload text to a new block blob.
        string blobContents = "This is a block blob.";
        byte[] byteArray = Encoding.ASCII.GetBytes(blobContents);

        using (MemoryStream stream = new MemoryStream(byteArray))
        {
            await containerClient.UploadBlobAsync(blobName, stream);
        }
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

> [!NOTE]
> Als u aanvragen voor BLOB-of wachtrij gegevens met Azure AD wilt autoriseren, moet u HTTPS voor die aanvragen gebruiken.

## <a name="next-steps"></a>Volgende stappen

- [Toegangs rechten voor opslag gegevens beheren met RBAC](storage-auth-aad-rbac.md).
- [Gebruik Azure AD met opslag toepassingen](storage-auth-aad-app.md).
- [Voer Azure CLI-of Power shell-opdrachten uit met Azure AD-referenties om toegang te krijgen tot BLOB-of wachtrij gegevens](authorize-active-directory-powershell.md).
