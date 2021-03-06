---
title: Ingangs controller met statisch IP-adres gebruiken
titleSuffix: Azure Kubernetes Service
description: Meer informatie over het installeren en configureren van een NGINX ingress-controller met een statisch openbaar IP-adres in een Azure Kubernetes service-cluster (AKS).
services: container-service
ms.topic: article
ms.date: 04/27/2020
ms.openlocfilehash: a44a41806af30479f06ec4daba936c7aa71ef5d7
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "82561910"
---
# <a name="create-an-ingress-controller-with-a-static-public-ip-address-in-azure-kubernetes-service-aks"></a>Een ingangs controller maken met een statisch openbaar IP-adres in azure Kubernetes service (AKS)

Een controller voor inkomend verkeer is een stukje software dat omgekeerde proxy’s, configureerbare verkeersroutering en TLS-beëindiging voor Kubernetes-services biedt. Kubernetes-resources voor inkomend verkeer worden gebruikt om de regels en routes voor uitgaand verkeer worden geconfigureerd voor individuele Kubernetes-services. Met behulp van een controller en regels voor inkomend verkeer kan er één enkel IP-adres worden gebruikt voor het routeren van verkeer naar meerdere services in een Kubernetes-cluster.

In dit artikel wordt beschreven hoe u de [NGINX ingress-controller][nginx-ingress] implementeert in een Azure Kubernetes service-cluster (AKS). De ingangs controller is geconfigureerd met een statisch openbaar IP-adres. Het [CERT-beheer][cert-manager] project wordt gebruikt om certificaten automatisch te genereren en [te configureren.][lets-encrypt] Ten slotte worden er twee toepassingen uitgevoerd in het AKS-cluster, die allemaal toegankelijk zijn via één IP-adres.

U kunt ook het volgende doen:

- [Een eenvoudige ingangs controller met externe netwerk verbinding maken][aks-ingress-basic]
- [De invoeg toepassing voor het routeren van HTTP-toepassingen inschakelen][aks-http-app-routing]
- [Een ingangs controller maken die gebruikmaakt van uw eigen TLS-certificaten][aks-ingress-own-tls]
- [Een ingangs controller maken die gebruikmaakt van de code ring om automatisch TLS-certificaten te genereren met een dynamisch openbaar IP-adres][aks-ingress-tls]

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u beschikt over een bestaand AKS-cluster. Als u een AKS-cluster nodig hebt, raadpleegt u de AKS Quick Start [met behulp van de Azure cli][aks-quickstart-cli] of [met behulp van de Azure Portal][aks-quickstart-portal].

In dit artikel wordt gebruikgemaakt van [helm 3][helm] voor het installeren van de NGINX ingress-controller en cert-Manager. Zorg ervoor dat u de nieuwste versie van helm gebruikt. Zie [helm install docs][helm-install](Engelstalig) voor upgrade-instructies. Zie [Installing Applications with helm in azure Kubernetes service (AKS) (Engelstalig)][use-helm]voor meer informatie over het configureren en gebruiken van helm.

Voor dit artikel moet u ook de Azure CLI-versie 2.0.64 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Een ingangs controller maken

Standaard wordt een NGINX ingress-controller gemaakt met een nieuwe open bare IP-adres toewijzing. Dit open bare IP-adres is alleen statisch voor de levens duur van de ingangs controller en gaat verloren als de controller wordt verwijderd en opnieuw wordt gemaakt. Een algemene configuratie vereiste is om de NGINX ingress controller een bestaand statisch openbaar IP-adres te bieden. Het statische open bare IP-adres blijft aanwezig als de ingangs controller wordt verwijderd. Met deze aanpak kunt u bestaande DNS-records en netwerk configuraties op consistente wijze gebruiken gedurende de levens cyclus van uw toepassingen.

Als u een statisch openbaar IP-adres wilt maken, moet u eerst de naam van de resource groep van het AKS-cluster ophalen met de opdracht [AZ AKS show][az-aks-show] :

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv
```

Maak vervolgens een openbaar IP-adres met de *statische* toewijzings methode met behulp van de opdracht [AZ Network Public-IP Create][az-network-public-ip-create] . In het volgende voor beeld wordt een openbaar IP-adres gemaakt met de naam *myAKSPublicIP* in de AKS-cluster resource groep die u in de vorige stap hebt verkregen:

```azurecli-interactive
az network public-ip create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP --sku Standard --allocation-method static --query publicIp.ipAddress -o tsv
```

Implementeer nu het *nginx-ingress-* grafiek met helm. Voor toegevoegde redundantie worden er twee replica's van de NGINX-ingangscontrollers geïmplementeerd met de parameter `--set controller.replicaCount`. Om volledig te profiteren van het uitvoeren van replica's van de ingangs controller, moet u ervoor zorgen dat er meer dan één knoop punt in uw AKS-cluster is.

U moet twee extra para meters door geven aan de helm-release, zodat de ingangs controller op de hoogte wordt gesteld van het statische IP-adres van de load balancer dat moet worden toegewezen aan de ingangs controller service en van het DNS-naam label dat wordt toegepast op de open bare IP-adres resource. Voor een juiste werking van de HTTPS-certificaten wordt een DNS-naam label gebruikt voor het configureren van een FQDN voor het IP-adres van de ingangs controller.

1. Voeg de `--set controller.service.loadBalancerIP` para meter toe. Geef uw eigen open bare IP-adres op dat u in de vorige stap hebt gemaakt.
1. Voeg de `--set controller.service.annotations."service\.beta\.kubernetes\.io/azure-dns-label-name"` para meter toe. Geef een DNS-naam label op dat moet worden toegepast op het open bare IP-adres dat in de vorige stap is gemaakt.

De ingangscontroller moet ook worden gepland op een Linux-knooppunt. Windows Server-knooppunten mogen de ingangscontroller niet uitvoeren. Er wordt een knooppuntselector opgegeven met behulp van de parameter `--set nodeSelector` om de Kubernetes-planner te laten weten dat de NGINX-ingangscontroller moet worden uitgevoerd op een Linux-knooppunt.

> [!TIP]
> In het volgende voor beeld wordt een Kubernetes-naam ruimte gemaakt voor de ingangs resources met de naam *ingress-Basic*. Geef waar nodig een naam ruimte op voor uw eigen omgeving. Als op uw AKS-cluster geen RBAC is ingeschakeld `--set rbac.create=false` , voegt u toe aan de helm-opdrachten.

> [!TIP]
> Als u [IP-behoud van client bronnen][client-source-ip] wilt inschakelen voor aanvragen voor containers in uw cluster, voegt `--set controller.service.externalTrafficPolicy=Local` u toe aan de helm-installatie opdracht. Het bron-IP-adres van de client wordt opgeslagen in de aanvraag header onder *X-doorgestuurd-voor*. Bij gebruik van een ingangs controller waarvoor IP-behoud door client bron is ingeschakeld, werkt TLS Pass-Through niet.

Werk het volgende script bij met het **IP-adres** van uw ingangs controller en een **unieke naam** die u wilt gebruiken voor het FQDN-voor voegsel:

```console
# Create a namespace for your ingress resources
kubectl create namespace ingress-basic

# Use Helm to deploy an NGINX ingress controller
helm install nginx-ingress stable/nginx-ingress \
    --namespace ingress-basic \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set controller.service.loadBalancerIP="STATIC_IP" \
    --set controller.service.annotations."service\.beta\.kubernetes\.io/azure-dns-label-name"="demo-aks-ingress"
```

Wanneer de Kubernetes-load balancer service is gemaakt voor de NGINX ingress-controller, wordt uw statische IP-adres toegewezen, zoals wordt weer gegeven in de volgende voorbeeld uitvoer:

```
$ kubectl get service -l app=nginx-ingress --namespace ingress-basic

NAME                                        TYPE           CLUSTER-IP    EXTERNAL-IP    PORT(S)                      AGE
nginx-ingress-controller                    LoadBalancer   10.0.232.56   STATIC_IP      80:31978/TCP,443:32037/TCP   3m
nginx-ingress-default-backend               ClusterIP      10.0.95.248   <none>         80/TCP                       3m
```

Er zijn nog geen regels voor binnenkomend verkeer gemaakt, dus de standaard 404-pagina van de NGINX ingress-controller wordt weer gegeven als u naar het open bare IP-adres bladert. Ingangs regels worden in de volgende stappen geconfigureerd.

U kunt controleren of het DNS-naam label is toegepast door de volgende query uit te zoeken naar de FQDN op het open bare IP-adres:

```azurecli-interactive
az network public-ip list --resource-group MC_myResourceGroup_myAKSCluster_eastus --query "[?name=='myAKSPublicIP'].[dnsSettings.fqdn]" -o tsv
```

De ingangs controller is nu toegankelijk via het IP-adres of de FQDN.

## <a name="install-cert-manager"></a>Certificaatbeheer installeren

De NGINX-ingangscontroller ondersteunt TLS-beëindiging. Er zijn verschillende manieren om certificaten voor HTTPS op te halen en te configureren. In dit artikel wordt gedemonstreerd met behulp van [CERT-beheer][cert-manager], waarmee automatisch de functionaliteit voor het genereren en beheren van certificaten [kan worden versleuteld][lets-encrypt] .

> [!NOTE]
> In dit artikel wordt `staging` gebruikgemaakt van de omgeving voor versleutelen. In productie-implementaties gebruikt `letsencrypt-prod` u `https://acme-v02.api.letsencrypt.org/directory` en in de resource definities en bij de installatie van de helm-grafiek.

Gebruik de volgende `helm install` opdracht om de CERT-beheer controller te installeren in een cluster met RBAC-functionaliteit:

```console
# Install the CustomResourceDefinition resources separately
kubectl apply --validate=false -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.13/deploy/manifests/00-crds.yaml

# Label the cert-manager namespace to disable resource validation
kubectl label namespace ingress-basic cert-manager.io/disable-validation=true

# Add the Jetstack Helm repository
helm repo add jetstack https://charts.jetstack.io

# Update your local Helm chart repository cache
helm repo update

# Install the cert-manager Helm chart
helm install \
  cert-manager \
  --namespace ingress-basic \
  --version v0.13.0 \
  jetstack/cert-manager
```

Zie het [project certificaat beheer][cert-manager]voor meer informatie over de configuratie van certificaat beheer.

## <a name="create-a-ca-cluster-issuer"></a>Een certificerings instantie voor een CA-cluster maken

Voordat certificaten kunnen worden verleend, vereist CERT-Manager een [verlener][cert-manager-issuer] -of [ClusterIssuer][cert-manager-cluster-issuer] -resource. Deze Kubernetes-resources zijn identiek in functionaliteit, `Issuer` maar werken in één naam ruimte en `ClusterIssuer` werken in alle naam ruimten. Zie de documentatie van de [certificaat beheerder][cert-manager-issuer] voor meer informatie.

Maak een cluster Uitgever, zoals in het `cluster-issuer.yaml`volgende voor beeld-manifest. Werk het e-mail adres bij met een geldig adres van uw organisatie:

```yaml
apiVersion: cert-manager.io/v1alpha2
kind: ClusterIssuer
metadata:
  name: letsencrypt-staging
spec:
  acme:
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    email: user@contoso.com
    privateKeySecretRef:
      name: letsencrypt-staging
    solvers:
    - http01:
        ingress:
          class: nginx
```

Gebruik de `kubectl apply -f cluster-issuer.yaml` opdracht om de certificaat verlener te maken.

```
$ kubectl apply -f cluster-issuer.yaml --namespace ingress-basic

clusterissuer.cert-manager.io/letsencrypt-staging created
```

## <a name="run-demo-applications"></a>Demo toepassingen uitvoeren

Er is een ingangs controller en een oplossing voor certificaat beheer geconfigureerd. We gaan nu twee demonstratie toepassingen uitvoeren in uw AKS-cluster. In dit voor beeld wordt helm gebruikt om twee exemplaren van een eenvoudige ' Hallo wereld '-toepassing te implementeren.

Als u de ingangs controller in actie wilt zien, voert u twee demonstratie toepassingen uit in uw AKS-cluster. In dit voor beeld gebruikt `kubectl apply` u om twee exemplaren van een eenvoudige *Hello World* -toepassing te implementeren.

Maak een *AKS-bestand HelloWorld. yaml* en kopieer het in het volgende voor beeld YAML:

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aks-helloworld
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aks-helloworld
  template:
    metadata:
      labels:
        app: aks-helloworld
    spec:
      containers:
      - name: aks-helloworld
        image: neilpeterson/aks-helloworld:v1
        ports:
        - containerPort: 80
        env:
        - name: TITLE
          value: "Welcome to Azure Kubernetes Service (AKS)"
---
apiVersion: v1
kind: Service
metadata:
  name: aks-helloworld
spec:
  type: ClusterIP
  ports:
  - port: 80
  selector:
    app: aks-helloworld
```

Maak een inkomend *-demo. yaml-* bestand en kopieer het in het volgende voor beeld YAML:

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ingress-demo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ingress-demo
  template:
    metadata:
      labels:
        app: ingress-demo
    spec:
      containers:
      - name: ingress-demo
        image: neilpeterson/aks-helloworld:v1
        ports:
        - containerPort: 80
        env:
        - name: TITLE
          value: "AKS Ingress Demo"
---
apiVersion: v1
kind: Service
metadata:
  name: ingress-demo
spec:
  type: ClusterIP
  ports:
  - port: 80
  selector:
    app: ingress-demo
```

Voer de twee demo toepassingen uit `kubectl apply`met:

```console
kubectl apply -f aks-helloworld.yaml --namespace ingress-basic
kubectl apply -f ingress-demo.yaml --namespace ingress-basic
```

## <a name="create-an-ingress-route"></a>Een ingangs route maken

Beide toepassingen worden nu uitgevoerd op uw Kubernetes-cluster, maar ze zijn geconfigureerd met een service van `ClusterIP`het type. De toepassingen zijn dus niet toegankelijk via internet. Als u ze openbaar beschikbaar wilt maken, maakt u een Kubernetes-ingangs bron. De ingangs resource configureert de regels die verkeer routeren naar een van de twee toepassingen.

In het volgende voor beeld wordt verkeer naar het `https://demo-aks-ingress.eastus.cloudapp.azure.com/` adres gerouteerd naar de service met de `aks-helloworld`naam. Verkeer naar het adres `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` wordt doorgestuurd naar de `ingress-demo` service. Werk de *hosts* en *host* bij naar de DNS-naam die u in een vorige stap hebt gemaakt.

Maak een bestand met `hello-world-ingress.yaml` de naam en kopieer het in het volgende voor beeld YAML.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    cert-manager.io/cluster-issuer: letsencrypt-staging
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  tls:
  - hosts:
    - demo-aks-ingress.eastus.cloudapp.azure.com
    secretName: tls-secret
  rules:
  - host: demo-aks-ingress.eastus.cloudapp.azure.com
    http:
      paths:
      - backend:
          serviceName: aks-helloworld
          servicePort: 80
        path: /(.*)
      - backend:
          serviceName: ingress-demo
          servicePort: 80
        path: /hello-world-two(/|$)(.*)
```

Maak de ingangs resource met behulp van de `kubectl apply -f hello-world-ingress.yaml --namespace ingress-basic` opdracht.

```
$ kubectl apply -f hello-world-ingress.yaml --namespace ingress-basic

ingress.extensions/hello-world-ingress created
```

## <a name="create-a-certificate-object"></a>Een certificaat object maken

Vervolgens moet er een certificaat bron worden gemaakt. De certificaat resource definieert het gewenste X. 509-certificaat. Zie [certificaten certificaat beheer][cert-manager-certificates]voor meer informatie.

CERT-Manager heeft waarschijnlijk automatisch een certificaat object voor u gemaakt met behulp van ingress-Shim, dat automatisch wordt geïmplementeerd met CERT-Manager sinds v 0.2.2. Zie de documentatie van de inkomende [shim][ingress-shim]voor meer informatie.

Gebruik de `kubectl describe certificate tls-secret --namespace ingress-basic` opdracht om te controleren of het certificaat is gemaakt.

Als het certificaat is uitgegeven, ziet u uitvoer die vergelijkbaar is met de volgende:
```
Type    Reason          Age   From          Message
----    ------          ----  ----          -------
  Normal  CreateOrder     11m   cert-manager  Created new ACME order, attempting validation...
  Normal  DomainVerified  10m   cert-manager  Domain "demo-aks-ingress.eastus.cloudapp.azure.com" verified with "http-01" validation
  Normal  IssueCert       10m   cert-manager  Issuing certificate...
  Normal  CertObtained    10m   cert-manager  Obtained certificate from ACME server
  Normal  CertIssued      10m   cert-manager  Certificate issued successfully
```

Als u een extra certificaat bron wilt maken, kunt u dit doen met het volgende voor beeld-manifest. Werk de *dnsNames* en *domeinen* bij naar de DNS-naam die u in een vorige stap hebt gemaakt. Als u een interne ingangs controller gebruikt, geeft u de interne DNS-naam voor uw service op.

```yaml
apiVersion: cert-manager.io/v1alpha2
kind: Certificate
metadata:
  name: tls-secret
  namespace: ingress-basic
spec:
  secretName: tls-secret
  dnsNames:
  - demo-aks-ingress.eastus.cloudapp.azure.com
  acme:
    config:
    - http01:
        ingressClass: nginx
      domains:
      - demo-aks-ingress.eastus.cloudapp.azure.com
  issuerRef:
    name: letsencrypt-staging
    kind: ClusterIssuer
```

Gebruik de `kubectl apply -f certificates.yaml` opdracht om de certificaat bron te maken.

```
$ kubectl apply -f certificates.yaml

certificate.cert-manager.io/tls-secret created
```

## <a name="test-the-ingress-configuration"></a>De ingangs configuratie testen

Open een webbrowser naar de FQDN van uw Kubernetes ingress-controller, zoals *`https://demo-aks-ingress.eastus.cloudapp.azure.com`*.

Zoals deze voor beelden `letsencrypt-staging`worden gebruikt, wordt het uitgegeven TLS/SSL-certificaat niet vertrouwd door de browser. Accepteer de waarschuwing om door te gaan naar uw toepassing. In de certificaat informatie ziet u dat dit *valse Le-tussenliggend x1* -certificaat wordt uitgegeven door de code ring. Dit valse certificaat geeft `cert-manager` aan dat de aanvraag correct is verwerkt en dat er een certificaat van de provider is ontvangen:

![Laten we het faserings certificaat versleutelen](media/ingress/staging-certificate.png)

Wanneer u het versleutelen van een `prod` certificaat wijzigt `staging`voor gebruik in plaats van, wordt het door de versleutelen van de voor keur verleende vertrouwde certificaten gebruikt, zoals u kunt zien in het volgende voor beeld:

![Certificaat versleutelen](media/ingress/certificate.png)

De demo toepassing wordt weer gegeven in de webbrowser:

![Voor beeld van toepassing 1](media/ingress/app-one.png)

Voeg nu het */Hello-World-Two* -pad toe aan de FQDN, *`https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two`* zoals. De tweede demo toepassing met de aangepaste titel wordt weer gegeven:

![Voor beeld van toepassing twee](media/ingress/app-two.png)

## <a name="clean-up-resources"></a>Resources opschonen

In dit artikel wordt helm gebruikt om de ingangs onderdelen, certificaten en voor beeld-apps te installeren. Wanneer u een helm-grafiek implementeert, worden er een aantal Kubernetes-resources gemaakt. Deze resources omvatten peulen, implementaties en services. Als u deze resources wilt opschonen, kunt u de volledige voorbeeld naam ruimte of de afzonderlijke resources verwijderen.

### <a name="delete-the-sample-namespace-and-all-resources"></a>De voorbeeld naam ruimte en alle resources verwijderen

Als u de volledige voorbeeld naam ruimte wilt verwijderen `kubectl delete` , gebruikt u de opdracht en geeft u de naam van de naam ruimte op. Alle resources in de naam ruimte worden verwijderd.

```console
kubectl delete namespace ingress-basic
```

### <a name="delete-resources-individually"></a>Resources afzonderlijk verwijderen

U kunt ook een nauw keurigere benadering van de gemaakte afzonderlijke resources verwijderen. Verwijder eerst de certificaat resources:

```console
kubectl delete -f certificates.yaml
kubectl delete -f cluster-issuer.yaml
```

Vermeld nu de helm-releases met `helm list` de opdracht. Zoek naar grafieken met de naam *nginx-* inkomend en *certificaat beheer* , zoals wordt weer gegeven in de volgende voorbeeld uitvoer:

```
$ helm list --all-namespaces

NAME                    NAMESPACE       REVISION        UPDATED                        STATUS          CHART                   APP VERSION
nginx-ingress           ingress-basic   1               2020-01-11 14:51:03.454165006  deployed        nginx-ingress-1.28.2    0.26.2
cert-manager            ingress-basic   1               2020-01-06 21:19:03.866212286  deployed        cert-manager-v0.13.0    v0.13.0
```

Verwijder de releases met de `helm uninstall` opdracht. In het volgende voor beeld worden de implementaties van de NGINX ingress-implementatie en certificaat beheerder verwijderd.

```
$ helm uninstall nginx-ingress cert-manager -n ingress-basic

release "nginx-ingress" deleted
release "cert-manager" deleted
```

Verwijder vervolgens de twee voorbeeld toepassingen:

```console
kubectl delete -f aks-helloworld.yaml --namespace ingress-basic
kubectl delete -f ingress-demo.yaml --namespace ingress-basic
```

Verwijder de zelf naam ruimte. Gebruik de `kubectl delete` opdracht en geef de naam van de naam ruimte op:

```console
kubectl delete namespace ingress-basic
```

Ten slotte verwijdert u het statische open bare IP-adres dat is gemaakt voor de ingangs controller. Geef de naam van uw *MC_* cluster resource groep op die u in de eerste stap van dit artikel hebt verkregen, zoals *MC_myResourceGroup_myAKSCluster_eastus*:

```azurecli-interactive
az network public-ip delete --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel zijn enkele externe onderdelen opgenomen in AKS. Zie de volgende project pagina's voor meer informatie over deze onderdelen:

- [Helm CLI][helm-cli]
- [NGINX ingress-controller][nginx-ingress]
- [CERT-beheerder][cert-manager]

U kunt ook het volgende doen:

- [Een eenvoudige ingangs controller met externe netwerk verbinding maken][aks-ingress-basic]
- [De invoeg toepassing voor het routeren van HTTP-toepassingen inschakelen][aks-http-app-routing]
- [Een ingangs controller maken die gebruikmaakt van een intern, privé netwerk en IP-adres][aks-ingress-internal]
- [Een ingangs controller maken die gebruikmaakt van uw eigen TLS-certificaten][aks-ingress-own-tls]
- [Een ingangs controller maken met een dynamisch openbaar IP-adres en configureren laten versleutelen om automatisch TLS-certificaten te genereren][aks-ingress-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm
[cert-manager]: https://github.com/jetstack/cert-manager
[cert-manager-certificates]: https://cert-manager.readthedocs.io/en/latest/reference/certificates.html
[cert-manager-cluster-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/clusterissuers.html
[cert-manager-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/issuers.html
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
[helm]: https://helm.sh/
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm
[ingress-shim]: https://docs.cert-manager.io/en/latest/tasks/issuing-certificates/ingress-shim.html

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-own-tls]: ingress-own-tls.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[client-source-ip]: concepts-network.md#ingress-controllers
[install-azure-cli]: /cli/azure/install-azure-cli
