---
title: Computer Vision-container gebruiken met Kubernetes en helm
titleSuffix: Azure Cognitive Services
description: Implementeer de Computer Vision-container in een Azure-container exemplaar en test deze in een webbrowser.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 9aac374de5af748eafbe4c22e5fc89f64e483c2a
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "80877967"
---
# <a name="use-computer-vision-container-with-kubernetes-and-helm"></a>Computer Vision-container gebruiken met Kubernetes en helm

Een optie voor het on-premises beheren van uw Computer Vision-containers is het gebruik van Kubernetes en helm. Door Kubernetes en helm te gebruiken om een Computer Vision container installatie kopie te definiëren, maken we een Kubernetes-pakket. Dit pakket wordt on-premises geïmplementeerd op een Kubernetes-cluster. Ten slotte verkennen we hoe u de geïmplementeerde Services kunt testen. Zie [Computer Vision containers installeren en uitvoeren](computer-vision-how-to-install-containers.md)voor meer informatie over het uitvoeren van docker-containers zonder Kubernetes-indeling.

## <a name="prerequisites"></a>Vereisten

De volgende vereisten voordat u Computer Vision containers on-premises gebruikt:

| Vereist | Doel |
|----------|---------|
| Azure-account | Als u nog geen abonnement op Azure hebt, maak dan een [gratis account][free-azure-account] aan voordat u begint. |
| Kubernetes CLI | De [KUBERNETES cli][kubernetes-cli] is vereist voor het beheren van de gedeelde referenties in het container register. Kubernetes is ook vereist voordat helm, de Kubernetes Package Manager. |
| Helm CLI | Installeer de [helm cli][helm-install], die wordt gebruikt om een helm-grafiek (container Package Definition) te installeren. |
| Computer Vision resource |Als u de container wilt gebruiken, hebt u het volgende nodig:<br><br>Een Azure **Computer Vision** -resource en de bijbehorende API-sleutel de EINDPUNT-URI. Beide waarden zijn beschikbaar op de pagina overzicht en sleutels voor de resource en zijn vereist om de container te starten.<br><br>**{API_KEY}**: een van de twee beschik bare bron sleutels op de pagina **sleutels**<br><br>**{ENDPOINT_URI}**: het eind punt op de pagina **overzicht**|

[!INCLUDE [Gathering required parameters](../containers/includes/container-gathering-required-parameters.md)]

### <a name="the-host-computer"></a>De hostcomputer

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Container vereisten en aanbevelingen

[!INCLUDE [Container requirements and recommendations](includes/container-requirements-and-recommendations.md)]

## <a name="connect-to-the-kubernetes-cluster"></a>Verbinding maken met het Kubernetes-cluster

Er wordt naar verwachting een beschik bare Kubernetes-cluster op de hostcomputer. Raadpleeg deze zelf studie over het [implementeren van een Kubernetes-cluster](../../aks/tutorial-kubernetes-deploy-cluster.md) voor een conceptuele uitleg over het implementeren van een Kubernetes-cluster op een hostcomputer.

### <a name="sharing-docker-credentials-with-the-kubernetes-cluster"></a>Docker-referenties delen met het Kubernetes-cluster

Als u het Kubernetes-cluster `docker pull` naar de geconfigureerde installatie kopieën in het `containerpreview.azurecr.io` container register wilt toestaan, moet u de docker-referenties naar het cluster overdragen. Voer de [`kubectl create`][kubectl-create] onderstaande opdracht uit om een *docker-REGI ster* te maken op basis van de referenties die zijn verschaft uit de toegangs vereisten voor container register.

Voer de volgende opdracht uit vanaf de opdracht regel interface van Choice. Zorg ervoor dat u de `<username>`, `<password>`en `<email-address>` vervangt door de container register referenties.

```console
kubectl create secret docker-registry containerpreview \
    --docker-server=containerpreview.azurecr.io \
    --docker-username=<username> \
    --docker-password=<password> \
    --docker-email=<email-address>
```

> [!NOTE]
> Als u al toegang hebt tot het `containerpreview.azurecr.io` container register, kunt u in plaats daarvan een Kubernetes-geheim maken met behulp van de generieke vlag. Bekijk de volgende opdracht die wordt uitgevoerd op basis van de JSON van de docker-configuratie.
> ```console
>  kubectl create secret generic containerpreview \
>      --from-file=.dockerconfigjson=~/.docker/config.json \
>      --type=kubernetes.io/dockerconfigjson
> ```

De volgende uitvoer wordt naar de console afgedrukt wanneer het geheim is gemaakt.

```console
secret "containerpreview" created
```

Als u wilt controleren of het geheim is gemaakt, voert [`kubectl get`][kubectl-get] u uit `secrets` met de vlag.

```console
kubectl get secrets
```

Bij het `kubectl get secrets` uitvoeren van worden alle geconfigureerde geheimen afgedrukt.

```console
NAME                  TYPE                                  DATA      AGE
containerpreview      kubernetes.io/dockerconfigjson        1         30s
```

## <a name="configure-helm-chart-values-for-deployment"></a>Helm-grafiek waarden voor implementatie configureren

Maak eerst een map met de naam *Read*en plak de volgende YAML-inhoud in een nieuw bestand met de naam *Chart. yml*.

```yaml
apiVersion: v1
name: read
version: 1.0.0
description: A Helm chart to deploy the microsoft/cognitive-services-read to a Kubernetes cluster
```

Als u de standaard waarden van de helm-grafiek wilt configureren, kopieert en plakt u de `values.yaml`volgende yaml in een bestand met de naam. Vervang de `# {ENDPOINT_URI}` opmerkingen `# {API_KEY}` en door uw eigen waarden.

```yaml
# These settings are deployment specific and users can provide customizations

read:
  enabled: true
  image:
    name: cognitive-services-read
    registry: containerpreview.azurecr.io/
    repository: microsoft/cognitive-services-read
    tag: latest
    pullSecret: containerpreview # Or an existing secret
    args:
      eula: accept
      billing: # {ENDPOINT_URI}
      apikey: # {API_KEY}
```

> [!IMPORTANT]
> Als de `billing` waarden `apikey` en niet worden weer gegeven, verlopen de Services na 15 minuten. De verificatie mislukt ook omdat de services niet beschikbaar zijn.

Maak een map *sjablonen* onder de map *lezen* . Kopieer en plak de volgende YAML in een bestand met `deployment.yaml`de naam. Het `deployment.yaml` bestand moet worden gebruikt als een helm-sjabloon.

> Sjablonen genereren manifest bestanden. Dit zijn YAML bron beschrijvingen die Kubernetes kunnen begrijpen. [-Hand leiding voor helm-grafiek sjablonen][chart-template-guide]

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: read
spec:
  template:
    metadata:
      labels:
        app: read-app
    spec:
      containers:
      - name: {{.Values.read.image.name}}
        image: {{.Values.read.image.registry}}{{.Values.read.image.repository}}
        ports:
        - containerPort: 5000
        env:
        - name: EULA
          value: {{.Values.read.image.args.eula}}
        - name: billing
          value: {{.Values.read.image.args.billing}}
        - name: apikey
          value: {{.Values.read.image.args.apikey}}
      imagePullSecrets:
      - name: {{.Values.read.image.pullSecret}}

--- 
apiVersion: v1
kind: Service
metadata:
  name: read
spec:
  type: LoadBalancer
  ports:
  - port: 5000
  selector:
    app: read-app
```

De sjabloon specificeert een load balancer service en de implementatie van de container/installatie kopie voor lezen.

### <a name="the-kubernetes-package-helm-chart"></a>Het Kubernetes-pakket (helm-grafiek)

Het *helm-diagram* bevat de configuratie van de docker-installatie kopie (n) die `containerpreview.azurecr.io` moet worden opgehaald uit het container register.

> Een [helm-grafiek][helm-charts] is een verzameling bestanden waarin een gerelateerde set Kubernetes-resources wordt beschreven. Eén grafiek kan worden gebruikt om een eenvoudig te implementeren, zoals een memcached Pod, of iets complex, zoals een volledige web-app-stack met HTTP-servers, data bases, caches, enzovoort.

De meegeleverde *helm-grafieken* halen de docker-installatie kopieën van de computer vision-service en de bijbehorende `containerpreview.azurecr.io` service uit het container register.

## <a name="install-the-helm-chart-on-the-kubernetes-cluster"></a>De helm-grafiek op het Kubernetes-cluster installeren

Als u de *helm-grafiek*wilt installeren, moet u de [`helm install`][helm-install-cmd] opdracht uitvoeren. Zorg ervoor dat u de installatie opdracht uitvoert vanuit de map `read` die zich boven de map bevindt.

```console
helm install read ./read
```

Hier volgt een voorbeeld uitvoer die u kunt verwachten van een geslaagde installatie-uitvoering:

```console
NAME: read
LAST DEPLOYED: Thu Sep 04 13:24:06 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                    READY  STATUS             RESTARTS  AGE
read-57cb76bcf7-45sdh   0/1    ContainerCreating  0         0s

==> v1/Service
NAME     TYPE          CLUSTER-IP    EXTERNAL-IP  PORT(S)         AGE
read     LoadBalancer  10.110.44.86  localhost    5000:31301/TCP  0s

==> v1beta1/Deployment
NAME    READY  UP-TO-DATE  AVAILABLE  AGE
read    0/1    1           0          0s
```

Het volt ooien van de implementatie van Kubernetes kan enkele minuten duren. Voer de volgende opdracht uit om te controleren of zowel de peulen als de services goed zijn geïmplementeerd en beschikbaar zijn:

```console
kubectl get all
```

U ziet dat er iets lijkt op de volgende uitvoer:

```console
kubectl get all
NAME                        READY   STATUS    RESTARTS   AGE
pod/read-57cb76bcf7-45sdh   1/1     Running   0          17s

NAME                   TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/kubernetes     ClusterIP      10.96.0.1      <none>        443/TCP          45h
service/read           LoadBalancer   10.110.44.86   localhost     5000:31301/TCP   17s

NAME                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/read   1/1     1            1           17s

NAME                              DESIRED   CURRENT   READY   AGE
replicaset.apps/read-57cb76bcf7   1         1         1       17s
```
<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het installeren van toepassingen met helm in azure Kubernetes service (AKS) [vindt u hier][installing-helm-apps-in-aks].

> [!div class="nextstepaction"]
> [Cognitive Services containers][cog-svcs-containers]

<!-- LINKS - external -->
[free-azure-account]: https://azure.microsoft.com/free
[git-download]: https://git-scm.com/downloads
[azure-cli]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest
[docker-engine]: https://www.docker.com/products/docker-engine
[kubernetes-cli]: https://kubernetes.io/docs/tasks/tools/install-kubectl
[helm-install]: https://helm.sh/docs/using_helm/#installing-helm
[helm-install-cmd]: https://helm.sh/docs/intro/using_helm/#helm-install-installing-a-package
[helm-charts]: https://helm.sh/docs/topics/charts/
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[helm-test]: https://helm.sh/docs/helm/#helm-test
[chart-template-guide]: https://helm.sh/docs/chart_template_guide

<!-- LINKS - internal -->
[vision-container-host-computer]: computer-vision-how-to-install-containers.md#the-host-computer
[installing-helm-apps-in-aks]: ../../aks/kubernetes-helm.md
[cog-svcs-containers]: ../cognitive-services-container-support.md
