---
title: Cluster configuratie in azure Kubernetes Services (AKS)
description: Meer informatie over het configureren van een cluster in azure Kubernetes service (AKS)
services: container-service
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: fe5ce13d9db8f2bc2231f87de7e602e63d239bfa
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2020
ms.locfileid: "83725143"
---
# <a name="configure-an-aks-cluster"></a>Een AKS-cluster configureren

Als onderdeel van het maken van een AKS-cluster moet u mogelijk uw cluster configuratie aanpassen aan uw behoeften. Dit artikel bevat een aantal opties voor het aanpassen van uw AKS-cluster.

## <a name="os-configuration-preview"></a>BESTURINGSSYSTEEM configuratie (preview-versie)

AKS ondersteunt nu Ubuntu 18,04 als het besturings systeem van het knoop punt (OS) in de preview-versie. Tijdens de preview-periode zijn zowel Ubuntu 16,04 als Ubuntu 18,04 beschikbaar.

U moet de volgende resources hebben geïnstalleerd:

- De Azure CLI, versie 2.2.0 of hoger
- De 0.4.35-uitbrei ding AKS-preview

Gebruik de volgende Azure CLI-opdrachten om de 0.4.35-uitbrei ding AKS-preview of hoger te installeren:

```azurecli
az extension add --name aks-preview
az extension list
```

De `UseCustomizedUbuntuPreview` functie registreren:

```azurecli
az feature register --name UseCustomizedUbuntuPreview --namespace Microsoft.ContainerService
```

Het kan enkele minuten duren voordat de status als **geregistreerd**wordt weer gegeven. U kunt de registratie status controleren met behulp van de opdracht [AZ Feature List](https://docs.microsoft.com/cli/azure/feature?view=azure-cli-latest#az-feature-list) :

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/UseCustomizedUbuntuPreview')].{Name:name,State:properties.state}"
```

Wanneer de status wordt weer gegeven als geregistreerd, vernieuwt u de registratie van de `Microsoft.ContainerService` resource provider met behulp van de opdracht [AZ provider REGI ster](https://docs.microsoft.com/cli/azure/provider?view=azure-cli-latest#az-provider-register) :

```azurecli
az provider register --namespace Microsoft.ContainerService
```

### <a name="new-clusters"></a>Nieuwe clusters

Configureer het cluster voor het gebruik van Ubuntu 18,04 wanneer het cluster wordt gemaakt. Gebruik de `--aks-custom-headers` markering om Ubuntu 18,04 in te stellen als het standaard besturingssysteem.

```azure-cli
az aks create --name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804
```

Als u een normaal Ubuntu 16,04-cluster wilt maken, kunt u dit doen door de aangepaste tag weg te laten `--aks-custom-headers` .

### <a name="existing-clusters"></a>Bestaande clusters

Configureer een nieuwe knooppunt groep om Ubuntu 18,04 te gebruiken. Gebruik de `--aks-custom-headers` markering om Ubuntu 18,04 in te stellen als het standaard besturings systeem voor die knooppunt groep.

```azure-cli
az aks nodepool add --name ubuntu1804 --cluster-name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804
```

Als u een reguliere Ubuntu 16,04-knooppunt groepen wilt maken, kunt u dit doen door de aangepaste tag weg te laten `--aks-custom-headers` .


## <a name="custom-resource-group-name"></a>Aangepaste naam van resource groep

Wanneer u een Azure Kubernetes-service cluster in azure implementeert, wordt er een tweede resource groep voor de worker-knoop punten gemaakt. AKS krijgt standaard de naam van de resource groep `MC_resourcegroupname_clustername_location` , maar u kunt ook uw eigen naam opgeven.

Als u de naam van uw eigen resource groep wilt opgeven, installeert u de AKS-preview Azure CLI-extensie versie 0.3.2 of hoger. Gebruik de Azure CLI `--node-resource-group` om de para meter van de `az aks create` opdracht te gebruiken om een aangepaste naam voor de resource groep op te geven. Als u een Azure Resource Manager-sjabloon gebruikt om een AKS-cluster te implementeren, kunt u de naam van de resource groep definiëren met behulp van de `nodeResourceGroup` eigenschap.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --node-resource-group myNodeResourceGroup
```

De secundaire resource groep wordt automatisch gemaakt door de Azure-resource provider in uw eigen abonnement. U kunt alleen de naam van de aangepaste resource groep opgeven wanneer het cluster wordt gemaakt. 

Houd er bij het werken met de knooppunt resource groep voor dat u niet:

- Geef een bestaande resource groep op voor de knooppunt resource groep.
- Geef een ander abonnement op voor de knooppunt resource groep.
- Wijzig de naam van de resource groep van het knoop punt nadat het cluster is gemaakt.
- Geef namen voor de beheerde resources op in de resource groep van het knoop punt.
- Door Azure gemaakte Tags van beheerde resources wijzigen of verwijderen in de resource groep van het knoop punt.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het gebruik `Kured` van om [beveiligings-en kernel-updates toe te passen op Linux-knoop punten](node-updates-kured.md) in uw cluster.
- Zie [een Azure Kubernetes service-cluster (AKS) bijwerken](upgrade-cluster.md) voor meer informatie over het upgraden van uw cluster naar de nieuwste versie van Kubernetes.
- Raadpleeg de lijst met [Veelgestelde vragen over AKS](faq.md) om antwoorden te vinden op enkele veelgestelde vragen over AKS.