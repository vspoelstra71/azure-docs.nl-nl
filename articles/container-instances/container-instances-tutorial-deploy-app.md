---
title: Zelf studie-container-app implementeren in container exemplaar
description: 'Zelfstudie 3 van 3 voor Azure Container Instances: een containertoepassing implementeren in Azure Container Instances'
ms.topic: tutorial
ms.date: 03/21/2018
ms.custom: seodec18, mvc
ms.openlocfilehash: 757b41bd69d69deb901e3b5b9a633dce3b9e133a
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "78249959"
---
# <a name="tutorial-deploy-a-container-application-to-azure-container-instances"></a>Zelf studie: een container toepassing implementeren in Azure Container Instances

Dit is de laatste zelfstudie in een reeks van drie. U hebt eerder in de reeks [een containerinstallatiekopie gemaakt](container-instances-tutorial-prepare-app.md) en [gepusht naar Azure Container Registry](container-instances-tutorial-prepare-acr.md). In dit artikel wordt de reeks afgerond door de container te implementeren in Azure Container Instances.

In deze zelfstudie hebt u:

> [!div class="checklist"]
> * De Azure Container Registry-container implementeren in Azure Container Instances
> * De actieve toepassing weergeven in de browser
> * De logboeken van de container weergeven

## <a name="before-you-begin"></a>Voordat u begint

[!INCLUDE [container-instances-tutorial-prerequisites](../../includes/container-instances-tutorial-prerequisites.md)]

## <a name="deploy-the-container-using-the-azure-cli"></a>De container implementeren met behulp van de Azure CLI

In deze sectie gebruikt u Azure CLI om de installatiekopie te implementeren die u hebt gebouwd in de [eerste zelfstudie](container-instances-tutorial-prepare-app.md) en die u in de [tweede zelfstudie](container-instances-tutorial-prepare-acr.md) naar Azure Container Registry hebt gepusht. Zorg ervoor dat u deze zelfstudies hebt voltooid voordat u verdergaat.

### <a name="get-registry-credentials"></a>Registerreferenties ophalen

Wanneer u een installatie kopie implementeert die wordt gehost in een persoonlijk Azure-container register, zoals het bestand dat in de [tweede zelf studie](container-instances-tutorial-prepare-acr.md)is gemaakt, moet u referenties opgeven voor toegang tot het REGI ster. 

Een best practice voor veel scenario's is het maken en configureren van een Azure Active Directory Service-Principal met *pull* -machtigingen voor het REGI ster. Zie [verifiëren met Azure container Registry van Azure container instances](../container-registry/container-registry-auth-aci.md) voor voorbeeld scripts om een service-principal met de benodigde machtigingen te maken. Noteer de *Service-Principal-id* en het *Service-Principal-wacht woord*. U gebruikt deze referenties om toegang te krijgen tot het REGI ster wanneer u de container implementeert.

U hebt ook de volledige naam nodig van de aanmeldingsserver van het containerregister (vervang `<acrName>` door de naam van het register):

```azurecli
az acr show --name <acrName> --query loginServer
```

### <a name="deploy-container"></a>Container implementeren

Gebruik nu de opdracht [az container create][az-container-create] om de container te implementeren. Vervang `<acrLoginServer>` door de waarde die u hebt verkregen met de vorige opdracht. Vervang `<service-principal-ID>` en `<service-principal-password>` door de ID en het wachtwoord van de service-principal die u hebt gemaakt voor toegang tot het register. Vervang `<aciDnsLabel>` door een gewenste DNS-naam.

```azurecli
az container create --resource-group myResourceGroup --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-login-server <acrLoginServer> --registry-username <service-principal-ID> --registry-password <service-principal-password> --dns-name-label <aciDnsLabel> --ports 80
```

U ontvangt binnen enkele seconden een eerste reactie van Azure. De waarde `--dns-name-label` moet uniek zijn voor de Azure-regio waar u de containerinstallatiekopie maakt. Wijzig de waarde in de vorige opdracht als u een foutbericht ontvangt over het **DNS-naamlabel** wanneer u de opdracht uitvoert.

### <a name="verify-deployment-progress"></a>Voortgang van implementatie verifiëren

Gebruik [az container show][az-container-show] om de status van de implementatie te bekijken:

```azurecli
az container show --resource-group myResourceGroup --name aci-tutorial-app --query instanceView.state
```

Herhaal de opdracht [az container show][az-container-show] totdat de status verandert van *In behandeling* in *Wordt uitgevoerd*. Als het goed is, duurt dit minder dan een minuut. Wanneer de container de status *Wordt uitgevoerd* heeft, gaat u verder met de volgende stap.

## <a name="view-the-application-and-container-logs"></a>De toepassings- en containerlogboeken bekijken

Wanneer de implementatie is geslaagd, geeft u de volledig gekwalificeerde domeinnaam (FQDN) van de container weer met de opdracht [az container show][az-container-show]:

```azurecli
az container show --resource-group myResourceGroup --name aci-tutorial-app --query ipAddress.fqdn
```

Bijvoorbeeld:
```output
"aci-demo.eastus.azurecontainer.io"
```

Als u de actieve toepassing wilt bekijken, navigeert u in uw browser naar de weergegeven DNS-naam:

![Hello world-app in browser][aci-app-browser]

U kunt ook de logboekuitvoer van de container bekijken:

```azurecli
az container logs --resource-group myResourceGroup --name aci-tutorial-app
```

Voorbeelduitvoer:

```output
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://aci-demo.eastus.azurecontainer.io/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u de resources die u in de zelfstudiereeks hebt gemaakt, niet langer nodig hebt, kunt u de opdracht [az group delete][az-group-delete] uitvoeren om de resourcegroep en alle resources daarin te verwijderen. Met deze opdracht verwijdert u het containerregister dat u hebt gemaakt, evenals de actieve container en alle gerelateerde resources.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het proces voor het implementeren van uw container naar Azure Container Instances voltooid. De volgende stappen zijn voltooid:

> [!div class="checklist"]
> * De container van Azure Container Registry implementeren met behulp van Azure CLI
> * De toepassing in de browser bekijken
> * De containerlogboeken ophalen

Nu u de basisprincipes onder de knie hebt, kunt u meer leren over Azure Container Instances, bijvoorbeeld hoe containergroepen werken:

> [!div class="nextstepaction"]
> [Containergroepen in Azure Container Instances](container-instances-container-groups.md)

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli-install]: /cli/azure/install-azure-cli
[prepare-app]: ./container-instances-tutorial-prepare-app.md
