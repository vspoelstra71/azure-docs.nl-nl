---
title: 'Azure-ExpressRoute: een circuit wijzigen: Power shell'
description: Een ExpressRoute-circuit maken, inrichten, controleren, bijwerken, verwijderen en de inrichting ervan opheffen.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 01/08/2020
ms.author: cherylmc
ms.openlocfilehash: ab44d5358154c17622eef68205ac2326e81377a0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "75770965"
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell"></a>Een ExpressRoute-circuit maken en wijzigen met behulp van PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure-CLI](howto-circuit-cli.md)
> * [Azure Resource Manager sjabloon](expressroute-howto-circuit-resource-manager-template.md)
> * [Video-Azure Portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [Power shell (klassiek)](expressroute-howto-circuit-classic.md)
>

Dit artikel helpt u bij het maken van een ExpressRoute-circuit met Power shell-cmdlets en het Azure Resource Manager-implementatie model. U kunt ook de status controleren, bijwerken, verwijderen of de inrichting van een circuit ongedaan maken.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u begint, moet u de [vereisten](expressroute-prerequisites.md) en [werk stromen](expressroute-workflows.md) controleren voordat u begint met de configuratie.

### <a name="working-with-azure-powershell"></a>Werken met Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/hybrid-az-ps.md)]

[!INCLUDE [expressroute-cloudshell](../../includes/expressroute-cloudshell-powershell-about.md)]

## <a name="create-and-provision-an-expressroute-circuit"></a><a name="create"></a>Een ExpressRoute-circuit maken en inrichten
### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Meld u aan bij uw Azure-account en selecteer uw abonnement

[!INCLUDE [sign in](../../includes/expressroute-cloud-shell-connect.md)]

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. de lijst met ondersteunde providers, locaties en band breedten ophalen
Voordat u een ExpressRoute-circuit maakt, hebt u de lijst met ondersteunde connectiviteits providers, locaties en bandbreedte opties nodig.

De Power shell **-cmdlet Get-AzExpressRouteServiceProvider** retourneert deze informatie, die u in latere stappen gaat gebruiken:

```azurepowershell-interactive
Get-AzExpressRouteServiceProvider
```

Controleer of uw connectiviteits provider daar wordt vermeld. Noteer de volgende informatie, die u later nodig hebt bij het maken van een circuit:

* Naam
* PeeringLocations
* BandwidthsOffered

U bent nu klaar om een ExpressRoute-circuit te maken.

### <a name="3-create-an-expressroute-circuit"></a>3. een ExpressRoute-circuit maken
Als u nog geen resource groep hebt, moet u er een maken voordat u uw ExpressRoute-circuit maakt. U kunt dit doen door de volgende opdracht uit te voeren:

```azurepowershell-interactive
New-AzResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"
```

In het volgende voor beeld ziet u hoe u een ExpressRoute-circuit van 200 Mbps maakt met behulp van Equinix in silicone dal. Als u een andere provider en andere instellingen gebruikt, moet u deze informatie vervangen wanneer u uw aanvraag doet. Gebruik het volgende voor beeld om een nieuwe service sleutel aan te vragen:

```azurepowershell-interactive
New-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200
```

Zorg ervoor dat u de juiste SKU-laag en SKU-familie opgeeft:

* SKU-laag bepaalt of een ExpressRoute-circuit [lokaal](expressroute-faqs.md#expressroute-local), standaard of [Premium](expressroute-faqs.md#expressroute-premium)is. U kunt *lokaal*, *standaard* of *Premium*opgeven.
* De SKU-familie bepaalt het facturerings type. U kunt *Metereddata* opgeven voor een data-abonnement met data limiet en *Unlimiteddata* voor een onbeperkt data-abonnement. U kunt het facturerings type wijzigen van *Metereddata* in *Unlimiteddata*, maar u kunt het type niet wijzigen van *Unlimiteddata* in *Metereddata*. Een *lokaal* circuit is altijd *Unlimiteddata*.

> [!IMPORTANT]
> Uw ExpressRoute-circuit wordt gefactureerd vanaf het moment dat een service sleutel wordt uitgegeven. Zorg ervoor dat u deze bewerking uitvoert wanneer de connectiviteits provider gereed is om het circuit in te richten.
>
>

Het antwoord bevat de service sleutel. U kunt gedetailleerde beschrijvingen van alle para meters verkrijgen door de volgende opdracht uit te voeren:

```azurepowershell-interactive
get-help New-AzExpressRouteCircuit -detailed
```


### <a name="4-list-all-expressroute-circuits"></a>4. alle ExpressRoute-circuits weer geven
Voer de opdracht **Get-AzExpressRouteCircuit** uit om een lijst op te halen met alle ExpressRoute-circuits die u hebt gemaakt:

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```

Het antwoord ziet er ongeveer uit als in het volgende voor beeld:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

U kunt deze informatie op elk gewenst moment ophalen met behulp van de `Get-AzExpressRouteCircuit` -cmdlet. Als u de aanroep zonder para meters aanroept, worden alle circuits weer gegeven. Uw service sleutel wordt weer gegeven in het veld *ServiceKey* :

```azurepowershell-interactive
Get-AzExpressRouteCircuit
```


Het antwoord ziet er ongeveer uit als in het volgende voor beeld:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. de service sleutel naar uw connectiviteits provider verzenden voor het inrichten
*ServiceProviderProvisioningState* biedt informatie over de huidige status van de inrichting van de service provider. Status geeft de status aan de kant van micro soft. Zie [werk stromen](expressroute-workflows.md#expressroute-circuit-provisioning-states)voor meer informatie over de inrichtings status van circuits.

Wanneer u een nieuw ExpressRoute-circuit maakt, heeft het circuit de volgende status:

    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



Het circuit verandert in de volgende status wanneer de connectiviteits provider het proces voor u inschakelt:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Als u een ExpressRoute-circuit wilt gebruiken, moet dit een van de volgende statussen hebben:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. Controleer de status en de status van de circuit sleutel regel matig
Door de status en de status van de circuit sleutel te controleren, kunt u weten wanneer uw provider uw circuit heeft ingeschakeld. Nadat het circuit is geconfigureerd, wordt *ServiceProviderProvisioningState* weer gegeven als *ingericht*, zoals wordt weer gegeven in het volgende voor beeld:

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


Het antwoord ziet er ongeveer uit als in het volgende voor beeld:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []

### <a name="7-create-your-routing-configuration"></a>7. de routerings configuratie maken
Zie het artikel [ExpressRoute circuit routerings configuratie](expressroute-howto-routing-arm.md) voor het maken en wijzigen van circuit peerings voor stapsgewijze instructies.

> [!IMPORTANT]
> Deze instructies zijn alleen van toepassing op circuits die zijn gemaakt met service providers die Layer 2-connectiviteits services aanbieden. Als u een service provider gebruikt die Managed Layer 3-services biedt (meestal een IP VPN, zoals MPLS), wordt door uw connectiviteits provider route ring voor u geconfigureerd en beheerd.
>
>

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. een virtueel netwerk koppelen aan een ExpressRoute-circuit
Vervolgens koppelt u een virtueel netwerk aan uw ExpressRoute-circuit. Gebruik het artikel [koppel virtuele netwerken aan ExpressRoute-circuits](expressroute-howto-linkvnet-arm.md) wanneer u werkt met het Resource Manager-implementatie model.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>De status van een ExpressRoute-circuit ophalen
U kunt deze informatie op elk gewenst moment ophalen met behulp van de cmdlet **Get-AzExpressRouteCircuit** . Als u de aanroep zonder para meters aanroept, worden alle circuits weer gegeven.

```azurepowershell-interactive
Get-AzExpressRouteCircuit
```


Het antwoord is vergelijkbaar met het volgende voorbeeld:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                            "ServiceProviderName": "Equinix",
                                            "PeeringLocation": "Silicon Valley",
                                            "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


U kunt informatie over een specifiek ExpressRoute-circuit verkrijgen door de naam van de resource groep en de naam van het circuit als een para meter aan de aanroep door te geven:

```azurepowershell-interactive
Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
```


Het antwoord ziet er ongeveer uit als in het volgende voor beeld:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                            "Tier": "Standard",
                                            "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


U kunt gedetailleerde beschrijvingen van alle para meters verkrijgen door de volgende opdracht uit te voeren:

```azurepowershell-interactive
get-help Get-AzExpressRouteCircuit -detailed
```

## <a name="modifying-an-expressroute-circuit"></a><a name="modify"></a>Een ExpressRoute-circuit wijzigen
U kunt bepaalde eigenschappen van een ExpressRoute-circuit wijzigen zonder dat dit van invloed is op de verbinding.

U kunt de volgende taken zonder uitval tijd doen:

* Een ExpressRoute Premium-invoeg toepassing voor uw ExpressRoute-circuit in-of uitschakelen.
* Verg root de band breedte van uw ExpressRoute-circuit, op voor waarde dat er capaciteit beschikbaar is op de poort. Het downgradeen van de band breedte van een circuit wordt niet ondersteund.
* Wijzig het licentie controle plan van gegevens met data limiet naar onbeperkte gegevens. Het is niet mogelijk om het meet plan van onbeperkte gegevens te wijzigen in gegevens met data limiet.
* U kunt *klassieke bewerkingen toestaan*in-en uitschakelen.

Zie de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md)voor meer informatie over limieten en beperkingen.

### <a name="to-enable-the-expressroute-premium-add-on"></a>De Premium-invoeg toepassing voor ExpressRoute inschakelen
U kunt de ExpressRoute Premium-invoeg toepassing voor uw bestaande circuit inschakelen met behulp van het volgende Power shell-fragment:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Premium"
$ckt.sku.Name = "Premium_MeteredData"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

Het circuit heeft nu de ExpressRoute Premium-invoeg toepassingen ingeschakeld. U wordt gefactureerd voor de Premium-invoeg functie zodra de opdracht is uitgevoerd.

### <a name="to-disable-the-expressroute-premium-add-on"></a>De Premium-invoeg toepassing voor ExpressRoute uitschakelen
> [!IMPORTANT]
> Als u resources gebruikt die groter zijn dan wat is toegestaan voor het standaard circuit, kan deze bewerking mislukken.
>
>

Let op de volgende informatie:

* Voordat u van Premium naar standaard downgradet, moet u ervoor zorgen dat het aantal virtuele netwerken dat is gekoppeld aan het circuit, kleiner is dan 10. Als u dit niet doet, mislukt de update aanvraag en worden de Premium-tarieven in rekening gebracht.
* U moet alle virtuele netwerken in andere geopolitieke regio's ontkoppelen. Als u dit niet doet, mislukt de update aanvraag en worden de Premium-tarieven in rekening gebracht.
* De route tabel moet kleiner zijn dan 4.000 routes voor privé-peering. Als uw route tabel groter is dan 4.000 routes, wordt de BGP-sessie verbroken en wordt deze niet opnieuw ingeschakeld totdat het aantal aangekondigde voor voegsels onder 4.000 komt.

U kunt de ExpressRoute Premium-invoeg toepassing voor het bestaande circuit uitschakelen met behulp van de volgende Power shell-cmdlet:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Tier = "Standard"
$ckt.sku.Name = "Standard_MeteredData"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-update-the-expressroute-circuit-bandwidth"></a>De band breedte van het ExpressRoute-circuit bijwerken
Raadpleeg de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md)voor de ondersteunde bandbreedte opties voor uw provider. U kunt elke grootte kiezen die groter is dan de grootte van uw bestaande circuit.

> [!IMPORTANT]
> Mogelijk moet u het ExpressRoute-circuit opnieuw maken als er onvoldoende capaciteit is op de bestaande poort. U kunt het circuit niet upgraden als er geen extra capaciteit beschikbaar is op die locatie.
>
> U kunt de band breedte van een ExpressRoute-circuit zonder onderbreking niet verkleinen. Voor downgrade-band breedte moet u de inrichting van het ExpressRoute-circuit ongedaan maken en vervolgens een nieuw ExpressRoute-circuit opnieuw inrichten.
>

Nadat u hebt bepaald welke grootte u nodig hebt, gebruikt u de volgende opdracht om het formaat van het circuit te wijzigen:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.ServiceProviderProperties.BandwidthInMbps = 1000

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```


Uw circuit wordt op de micro soft-zijde aangepast. Vervolgens moet u contact opnemen met uw connectiviteits provider om configuraties aan hun zijde bij te werken om deze wijziging aan te passen. Nadat u deze melding hebt gemaakt, wordt u gefactureerd voor de bijgewerkte bandbreedte optie.

### <a name="to-move-the-sku-from-metered-to-unlimited"></a>De SKU verplaatsen van naar een onbeperkte data limiet
U kunt de SKU van een ExpressRoute-circuit wijzigen met behulp van het volgende Power shell-fragment:

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

$ckt.Sku.Family = "UnlimitedData"
$ckt.sku.Name = "Premium_UnlimitedData"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>De toegang tot de klassieke en Resource Manager-omgevingen beheren
Bekijk de instructies in [ExpressRoute-circuits verplaatsen van het klassieke naar het Resource Manager-implementatie model](expressroute-howto-move-arm.md).

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a><a name="delete"></a>Deprovisioning and deleting an ExpressRoute circuit (Inrichting van een ExpressRoute-circuit ongedaan maken en het circuit verwijderen)
Let op de volgende informatie:

* U moet alle virtuele netwerken loskoppelen van het ExpressRoute-circuit. Als deze bewerking mislukt, controleert u of er virtuele netwerken zijn gekoppeld aan het circuit.
* Als de inrichtings status van de ExpressRoute-circuit service provider is **ingericht** of **ingericht** , moet u samen met uw service provider de inrichting van het circuit aan hun zijde opheffen. We blijven resources reserveren en u factureren totdat de service provider de inrichting van het circuit ongedaan heeft gemaakt en ons op de hoogte stelt.
* Als de service provider het circuit heeft ontsteld (de inrichtings status van de service provider is ingesteld op **niet ingericht**), kunt u het circuit verwijderen. Hiermee wordt ook de facturering voor het circuit gestopt.

U kunt het ExpressRoute-circuit verwijderen door de volgende opdracht uit te voeren:

```azurepowershell-interactive
Remove-AzExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"
```

## <a name="next-steps"></a>Volgende stappen

Nadat u het circuit hebt gemaakt, moet u de volgende stappen uitvoeren:

* [Route ring voor uw ExpressRoute-circuit maken en wijzigen](expressroute-howto-routing-arm.md)
* [Uw virtuele netwerk koppelen aan uw ExpressRoute-circuit](expressroute-howto-linkvnet-arm.md)
