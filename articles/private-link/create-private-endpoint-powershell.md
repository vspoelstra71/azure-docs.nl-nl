---
title: Een persoonlijk Azure-eind punt maken met behulp van Azure PowerShell | Microsoft Docs
description: Meer informatie over persoonlijke Azure-koppelingen
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: 81dbbeda9d0132de63180cc13f6243761e0ba865
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2020
ms.locfileid: "84171579"
---
# <a name="create-a-private-endpoint-using-azure-powershell"></a>Een persoonlijk eind punt maken met Azure PowerShell
Een persoonlijk eind punt is de fundamentele bouw steen voor privé-koppeling in Azure. Hiermee kunnen Azure-resources, zoals Virtual Machines (Vm's), privé communiceren met persoonlijke koppelings bronnen. 

In deze Quick Start leert u hoe u een virtuele machine maakt in een Azure-Virtual Network, een logische SQL-Server met een Azure-privé-eind punt met behulp van Azure PowerShell. Vervolgens kunt u veilig toegang krijgen tot SQL Database via de virtuele machine.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Voordat u uw resources kunt maken, moet u een resource groep maken die als host fungeert voor de Virtual Network en het persoonlijke eind punt met [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). In het volgende voor beeld wordt een resource groep met de naam *myResourceGroup* gemaakt op de locatie *westus* :

```azurepowershell

New-AzResourceGroup `
  -ResourceGroupName myResourceGroup `
  -Location westcentralus
```

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken
In deze sectie maakt u een virtueel netwerk en een subnet. Vervolgens koppelt u het subnet aan uw Virtual Network.

### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Maak een virtueel netwerk voor uw persoonlijke eind punt met [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork). In het volgende voor beeld wordt een Virtual Network gemaakt met de naam *MyVirtualNetwork*:
 
```azurepowershell

$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location westcentralus `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

### <a name="add-a-subnet"></a>Een subnet toevoegen

Azure implementeert resources in een subnet binnen een Virtual Network, dus u moet een subnet maken. Maak een subnet-configuratie met de naam *mySubnet* met [add-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig). In het volgende voor beeld wordt een subnet met de naam *mySubnet* gemaakt waarbij de vlag voor het particuliere endpoint-netwerk beleid is ingesteld op **uitgeschakeld**.

```azurepowershell
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 10.0.0.0/24 `
  -PrivateEndpointNetworkPoliciesFlag "Disabled" `
  -VirtualNetwork $virtualNetwork
```

> [!CAUTION]
> Het is eenvoudig om de `PrivateEndpointNetworkPoliciesFlag` para meter te verwarren met een andere beschik bare vlag, `PrivateLinkServiceNetworkPoliciesFlag` omdat deze zowel lange woorden als een vergelijk bare vormgeving hebben.  Zorg ervoor dat u de juiste versie gebruikt `PrivateEndpointNetworkPoliciesFlag` .

### <a name="associate-the-subnet-to-the-virtual-network"></a>Het subnet aan de Virtual Network koppelen

U kunt de configuratie van het subnet naar het Virtual Network schrijven met [set-AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork). Met deze opdracht maakt u het subnet:

```azurepowershell
$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Maak een virtuele machine in de Virtual Network met [New-AzVM](/powershell/module/az.compute/new-azvm). Wanneer u de volgende opdracht uitvoert, wordt u gevraagd referenties op te geven. Voer een gebruikersnaam en wachtwoord voor de virtuele machine in:

```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVm" `
    -Location "westcentralus" `
    -VirtualNetworkName "MyVirtualNetwork" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 80,3389 `
    -AsJob  
```

Met de optie `-AsJob` wordt de virtuele machine op de achtergrond gemaakt. U kunt doorgaan met de volgende stap.

Wanneer er op de achtergrond met het maken van de virtuele machine wordt begonnen, wordt er iets dergelijks weergegeven:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
1      Long Running... AzureLongRun... Running       True            localhost            New-AzVM
```

## <a name="create-a-logical-sql-server"></a>Een logische SQL-Server maken 

Maak een logische SQL-Server met behulp van de opdracht New-AzSqlServer. Houd er rekening mee dat de naam van uw server uniek moet zijn in azure, dus Vervang de waarde van de tijdelijke aanduiding tussen vier Kante haken door uw eigen unieke waarde:

```azurepowershell-interactive
$adminSqlLogin = "SqlAdmin"
$password = "ChangeYourAdminPassword1"

$server = New-AzSqlServer -ResourceGroupName "myResourceGroup" `
    -ServerName "myserver" `
    -Location "WestCentralUS" `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminSqlLogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))

New-AzSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName "myserver"`
    -DatabaseName "myda"`
    -RequestedServiceObjectiveName "S0" `
    -SampleName "AdventureWorksLT"
```

## <a name="create-a-private-endpoint"></a>Een privé-eindpunt maken

Persoonlijk eind punt voor de server in uw Virtual Network met [New-AzPrivateLinkServiceConnection](/powershell/module/az.network/New-AzPrivateLinkServiceConnection): 

```azurepowershell

$privateEndpointConnection = New-AzPrivateLinkServiceConnection -Name "myConnection" `
  -PrivateLinkServiceId $server.ResourceId `
  -GroupId "sqlServer" 
 
$virtualNetwork = Get-AzVirtualNetwork -ResourceGroupName  "myResourceGroup" -Name "MyVirtualNetwork"  
 
$subnet = $virtualNetwork `
  | Select -ExpandProperty subnets `
  | Where-Object  {$_.Name -eq 'mysubnet'}  
 
$privateEndpoint = New-AzPrivateEndpoint -ResourceGroupName "myResourceGroup" `
  -Name "myPrivateEndpoint" `
  -Location "westcentralus" `
  -Subnet  $subnet `
  -PrivateLinkServiceConnection $privateEndpointConnection
``` 

## <a name="configure-the-private-dns-zone"></a>De Privé-DNS zone configureren 
Maak een privé-DNS-zone voor SQL Database domein en maak een koppelings koppeling met het virtuele netwerk: 

```azurepowershell

$zone = New-AzPrivateDnsZone -ResourceGroupName "myResourceGroup" `
  -Name "privatelink.database.windows.net" 
 
$link  = New-AzPrivateDnsVirtualNetworkLink -ResourceGroupName "myResourceGroup" `
  -ZoneName "privatelink.database.windows.net"`
  -Name "mylink" `
  -VirtualNetworkId $virtualNetwork.Id  
 
$networkInterface = Get-AzResource -ResourceId $privateEndpoint.NetworkInterfaces[0].Id -ApiVersion "2019-04-01" 
 
foreach ($ipconfig in $networkInterface.properties.ipConfigurations) { 
foreach ($fqdn in $ipconfig.properties.privateLinkConnectionProperties.fqdns) { 
Write-Host "$($ipconfig.properties.privateIPAddress) $($fqdn)"  
$recordName = $fqdn.split('.',2)[0] 
$dnsZone = $fqdn.split('.',2)[1] 
New-AzPrivateDnsRecordSet -Name $recordName -RecordType A -ZoneName "privatelink.database.windows.net"  `
-ResourceGroupName "myResourceGroup" -Ttl 600 `
-PrivateDnsRecords (New-AzPrivateDnsRecordConfig -IPv4Address $ipconfig.properties.privateIPAddress)  
} 
} 
``` 
  
## <a name="connect-to-a-vm-from-the-internet"></a>Verbinding maken met een virtuele machine via internet

Gebruik [Get-AzPublicIpAddress](/powershell/module/az.network/Get-AzPublicIpAddress) om het openbare IP-adres van een virtuele machine op te halen. In dit voor beeld wordt het open bare IP-adres van de *myVM* -VM geretourneerd:

```azurepowershell
Get-AzPublicIpAddress `
  -Name myPublicIpAddress `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress 
```  
Open een opdrachtprompt op de lokale computer. Voer de opdracht mstsc uit. Vervang <publicIpAddress> door het openbare IP-adres dat in de laatste stap is geretourneerd: 


> [!NOTE]
> Als u deze opdrachten vanuit een PowerShell-prompt op de lokale computer hebt uitgevoerd en u met de Azure PowerShell-module versie 1.0 of hoger werkt, kunt u doorgaan in die interface.
```
mstsc /v:<publicIpAddress>
```

1. Selecteer **Verbinding maken** wanneer hierom wordt gevraagd. 
2. Voer de gebruikersnaam en het wachtwoord in die u hebt opgegeven bij het maken van de virtuele machine.
  > [!NOTE]
  > Mogelijk moet u meer opties selecteren > een ander account gebruiken om de referenties op te geven die u hebt ingevoerd tijdens het maken van de virtuele machine. 
  
3. Selecteer **OK**. 
4. Er kan een certificaatwaarschuwing worden weergegeven. Als dit het geval is, selecteert u **Ja** of **Doorgaan**. 

## <a name="access-sql-database-privately-from-the-vm"></a>SQL Database privé toegang tot de VM

1. Open Power shell in de Extern bureaublad van myVM.
2. Voer `nslookup myserver.database.windows.net` in. Vergeet niet door `myserver` de naam van uw SQL-Server te vervangen.

    U ontvangt een bericht dat er ongeveer als volgt uitziet:
    
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    myserver.privatelink.database.windows.net
    Address:  10.0.0.5
    Aliases:   myserver.database.windows.net
    ```
    
3. Installeer [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15).
4. Typ of Selecteer in **verbinding maken met server**de volgende informatie:

    | Instelling | Waarde |
    | --- | --- |
    | Servertype | Database Engine |
    | Servernaam | myserver.database.windows.net |
    | Gebruikersnaam | Voer de gebruikers naam in die tijdens het maken is opgegeven |
    | Wachtwoord | Voer het wacht woord in dat u hebt opgegeven tijdens het maken |
    | Wacht woord onthouden | Ja |
    
5. Selecteer **Verbinding maken**.
6. Blader door **data bases** in het menu links. 
7. Eventueel Gegevens uit mydatabase maken of er een query op uitvoeren.
8. Sluit de verbinding met extern bureau blad met *myVM*. 

## <a name="clean-up-resources"></a>Resources opschonen 
Wanneer u klaar bent met het persoonlijke eind punt, SQL Database en de virtuele machine, gebruikt u [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) om de resource groep en alle resources te verwijderen die het bevat:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [persoonlijke Azure-koppelingen](private-link-overview.md)
