---
title: Een virtuele machine maken op basis van een beheerde installatie kopie in azure
description: Maak een virtuele Windows-machine op basis van een gegeneraliseerde beheerde installatie kopie met behulp van Azure PowerShell of de portal.
author: cynthn
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: article
ms.date: 09/17/2018
ms.author: cynthn
ms.openlocfilehash: a028d597c3eb2a1c66df0e40266c2822e5cd7aab
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2020
ms.locfileid: "83726958"
---
# <a name="create-a-vm-from-a-managed-image"></a>Een VM maken op basis van een beheerde installatiekopie

U kunt meerdere virtuele machines (Vm's) maken op basis van een door Azure beheerde VM-installatie kopie met behulp van de Azure Portal of Power shell. Een beheerde VM-installatie kopie bevat de informatie die nodig is om een virtuele machine te maken, met inbegrip van het besturings systeem en de gegevens schijven. De virtuele harde schijven (Vhd's) waaruit de installatie kopie bestaan, met inbegrip van de schijven van het besturings systeem en alle gegevens schijven, worden opgeslagen als beheerde schijven. 

Voordat u een nieuwe virtuele machine maakt, moet u [een beheerde VM-installatie kopie maken](capture-image-resource.md) om te gebruiken als bron installatie kopie en lees toegang verlenen aan de installatie kopie voor alle gebruikers die toegang tot de installatie kopie moeten hebben. 

Eén beheerde installatie kopie ondersteunt Maxi maal 20 gelijktijdige implementaties. Als u probeert om meer dan 20 Vm's gelijktijdig te maken, vanuit dezelfde beheerde installatie kopie, kan dit leiden tot het inrichten van time-outs als gevolg van de opslag prestatie beperkingen van één VHD. Als u meer dan 20 Vm's gelijktijdig wilt maken, gebruikt u een afbeelding voor [gedeelde afbeeldings galerieën](shared-image-galleries.md) die is geconfigureerd met 1 replica voor elke 20 gelijktijdige VM-implementaties.

## <a name="use-the-portal"></a>Gebruik de portal

1. Ga naar de [Azure Portal](https://portal.azure.com) om een beheerde installatie kopie te vinden. Zoek en selecteer **installatie kopieën**.
3. Selecteer de installatie kopie die u wilt gebruiken in de lijst. De pagina **overzicht** van installatie kopie wordt geopend.
4. Selecteer **virtuele machine maken** in het menu.
5. Geef de informatie van de virtuele machine op. De gebruikers naam en het wacht woord die u hier opgeeft, worden gebruikt om u aan te melden bij de virtuele machine. Wanneer u klaar bent, selecteert u **OK**. U kunt de nieuwe virtuele machine in een bestaande resource groep maken of **nieuwe** maken om een nieuwe resource groep te maken voor het opslaan van de virtuele machine.
6. Selecteer een grootte voor de VM. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. 
7. Breng de gewenste wijzigingen aan onder **instellingen**en selecteer **OK**. 
8. Op de pagina samen vatting ziet u de naam van de installatie kopie die wordt vermeld als een **persoonlijke installatie kopie**. Selecteer **OK** om de implementatie van de virtuele machine te starten.


## <a name="use-powershell"></a>PowerShell gebruiken

U kunt Power shell gebruiken om een virtuele machine te maken op basis van een installatie kopie met behulp van de vereenvoudigde para meter set voor de cmdlet [New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) . De afbeelding moet zich in dezelfde resource groep benemen waar u de VM maakt.

 

Voor de vereenvoudigde parameterset voor [New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) is alleen vereist dat u een naam, resource groep en installatie kopie naam opgeeft om een virtuele machine te maken op basis van een installatie kopie. New-AzVm gebruikt de waarde van de para meter **-name** als de naam van alle resources die worden gemaakt, automatisch. In dit voor beeld geven we meer gedetailleerde namen voor elk van de resources op, maar kunnen ze automatisch worden gemaakt met de cmdlet. U kunt ook vooraf resources maken, zoals het virtuele netwerk, en de resource naam door geven aan de cmdlet. New-AzVm maakt gebruik van de bestaande resources als deze kunnen worden gevonden met hun naam.

In het volgende voor beeld wordt een VM gemaakt met de naam *myVMFromImage*in de resource groep *myResourceGroup* van de installatie kopie met de naam *myImage*. 


```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVMfromImage" `
    -ImageName "myImage" `
    -Location "East US" `
    -VirtualNetworkName "myImageVnet" `
    -SubnetName "myImageSubnet" `
    -SecurityGroupName "myImageNSG" `
    -PublicIpAddressName "myImagePIP" `
    -OpenPorts 3389
```



## <a name="next-steps"></a>Volgende stappen
[Virtuele Windows-machines maken en beheren met de module Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

