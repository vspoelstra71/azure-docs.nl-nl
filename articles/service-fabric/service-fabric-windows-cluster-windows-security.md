---
title: Een cluster dat wordt uitgevoerd op Windows beveiligen met behulp van Windows-beveiliging
description: Meer informatie over het configureren van knoop punt-naar-knoop punt en client-naar-knoop punt-beveiliging op een zelfstandig cluster waarop Windows wordt uitgevoerd met behulp van Windows-beveiliging.
author: dkkapur
ms.topic: conceptual
ms.date: 08/24/2017
ms.author: dekapur
ms.openlocfilehash: 46be6acc1ef08770826a2e020c8930eba0787791
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "76774451"
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a>Een zelfstandige cluster in Windows beveiligen met behulp van Windows-beveiliging
Als u ongeoorloofde toegang tot een Service Fabric cluster wilt voor komen, moet u het cluster beveiligen. Beveiliging is vooral belang rijk wanneer het cluster productie werkbelastingen uitvoert. In dit artikel wordt beschreven hoe u de beveiliging van knoop punt-naar-knoop punt en client-naar-knoop punt configureert met behulp van Windows-beveiliging in het bestand *ClusterConfig. json* .  Het proces komt overeen met de beveiligings stap configureren van [een zelfstandig cluster maken dat wordt uitgevoerd in Windows](service-fabric-cluster-creation-for-windows-server.md). Zie [cluster beveiligings scenario's](service-fabric-cluster-security.md)voor meer informatie over het gebruik van Windows-beveiliging met Service Fabric.

> [!NOTE]
> U moet de selectie van knoop punt-naar-knoop punt-beveiliging zorgvuldig overwegen, omdat er geen upgrade van het cluster van de ene beveiligings optie naar de andere wordt uitgevoerd. Als u de beveiligings selectie wilt wijzigen, moet u het volledige cluster opnieuw bouwen.
>
>

## <a name="configure-windows-security-using-gmsa"></a>Windows-beveiliging configureren met behulp van gMSA  
Het voorbeeld configuratie bestand *ClusterConfig. gMSA. Windows. multimachine. json* is gedownload met [micro soft. Azure. ServiceFabric. Windowsserver\< . ](https://go.microsoft.com/fwlink/?LinkId=730690)het zelfstandige cluster pakket voor versie>. zip bevat een sjabloon voor het configureren van Windows-beveiliging met behulp van door de [groep beheerde service accounts (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):  

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {  
        "ClustergMSAIdentity": "[gMSA Identity]",
        "ClusterSPN": "[Registered SPN for the gMSA account]",
        "ClientIdentities": [
            {
                "Identity": "domain\\username",
                "IsAdmin": true
            }
        ]
    }
}
```

| **Configuratie-instelling** | **Beschrijving** |
| --- | --- |
| ClusterCredentialType |Stel deze optie in op *Windows* om Windows-beveiliging in te scha kelen voor communicatie tussen knoop punten.  | 
| ServerCredentialType |Ingesteld op *Windows* om Windows-beveiliging in te scha kelen voor communicatie tussen client en knoop punten. |
| WindowsIdentities |Bevat het cluster en de client-id's. |
| ClustergMSAIdentity |Hiermee configureert u de beveiliging tussen knoop punten. Een beheerd service account voor een groep. |
| ClusterSPN |Geregistreerde SPN voor gMSA-account|
| ClientIdentities |Hiermee configureert u beveiliging van client naar knoop punt. Een matrix met client-gebruikers accounts. |
| Identiteit |Voeg de domein gebruiker domein\gebruikersnaam toe voor de client identiteit. |
| IsAdmin |Stel deze waarde in op True om op te geven dat de domein gebruiker beheerders toegang of ONWAAR heeft voor toegang tot de client gebruiker. |

> [!NOTE]
> De waarde van ClustergMSAIdentity moet de indelingmysfgmsa@mydomain' ' hebben.

[Knoop punt-naar-knooppunt beveiliging](service-fabric-cluster-security.md#node-to-node-security) wordt geconfigureerd door **ClustergMSAIdentity** in te stellen wanneer service Fabric moet worden uitgevoerd onder gMSA. Om vertrouwens relaties tussen knoop punten te kunnen bouwen, moeten ze van elkaar op de hoogte worden gesteld. Dit kan op twee verschillende manieren worden uitgevoerd: Geef het beheerde service account van de groep op dat alle knoop punten in het cluster bevat of geef de computer groep van het domein op die alle knoop punten in het cluster bevat. We raden u ten zeerste aan de [gMSA-aanpak (Group Managed Service account)](https://technet.microsoft.com/library/hh831782.aspx) te gebruiken, met name voor grotere clusters (meer dan 10 knoop punten) of voor clusters die waarschijnlijk groter of kleiner zijn.  
Voor deze methode is het maken van een domein groep waarvoor cluster beheerders toegangs rechten hebben gekregen om leden toe te voegen en te verwijderen, niet vereist. Deze accounts zijn ook handig voor automatische wachtwoord beheer. Zie aan de slag [met beheerde service accounts voor groepen](https://technet.microsoft.com/library/jj128431.aspx)voor meer informatie.  
 
[Beveiliging van client naar knoop punt](service-fabric-cluster-security.md#client-to-node-security) wordt geconfigureerd met behulp van **ClientIdentities**. Om een vertrouwens relatie tussen een client en het cluster tot stand te brengen, moet u het cluster configureren om te weten welke client identiteiten het kan vertrouwen. Dit kan op twee verschillende manieren worden gedaan: Geef de gebruikers van de domein groep op die verbinding kunnen maken of geef de gebruikers van het domein knooppunt op waarmee verbinding kan worden gemaakt. Service Fabric ondersteunt twee verschillende typen toegangs beheer voor clients die zijn verbonden met een Service Fabric cluster: beheerder en gebruiker. Toegangs beheer biedt de mogelijkheid van de Cluster beheerder om de toegang tot bepaalde typen cluster bewerkingen voor verschillende groepen gebruikers te beperken, waardoor het cluster beter is beveiligd.  Beheerders hebben volledige toegang tot beheer mogelijkheden (inclusief Lees-en schrijf mogelijkheden). Gebruikers hebben standaard alleen lees toegang tot beheer mogelijkheden (bijvoorbeeld query mogelijkheden) en de mogelijkheid om toepassingen en services op te lossen. Zie [op rollen gebaseerd toegangs beheer voor service Fabric-clients](service-fabric-cluster-security-roles.md)voor meer informatie over besturings elementen voor toegang.  
 
In het volgende voor beeld van een **beveiligings** sectie wordt Windows-beveiliging met gMSA geconfigureerd en wordt aangegeven dat de machines in *ServiceFabric.clusterA.contoso.com* gMSA deel uitmaken van het cluster en dat *CONTOSO\usera* client toegang heeft:  
  
```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClustergMSAIdentity" : "ServiceFabric.clusterA.contoso.com",
        "ClusterSPN" : "http/servicefabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
}
```
  
## <a name="configure-windows-security-using-a-machine-group"></a>Windows-beveiliging configureren met behulp van een computer groep  
Dit model wordt afgeschaft. De aanbeveling is om gMSA te gebruiken zoals hierboven wordt beschreven. Het voorbeeld configuratie bestand *ClusterConfig. Windows. multimachine. json* is gedownload met de [Microsoft. Azure. ServiceFabric. Windowsserver\< . ](https://go.microsoft.com/fwlink/?LinkId=730690)het zelfstandige cluster pakket voor versie>. zip bevat een sjabloon voor het configureren van Windows-beveiliging.  Windows-beveiliging is geconfigureerd in de sectie **Eigenschappen** : 

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "[domain\machinegroup]",
        "ClientIdentities": [{
            "Identity": "[domain\username]",
            "IsAdmin": true
        }]
    }
}
```

| **Configuratie-instelling** | **Beschrijving** |
| --- | --- |
| ClusterCredentialType |Stel deze optie in op *Windows* om Windows-beveiliging in te scha kelen voor communicatie tussen knoop punten.  |
| ServerCredentialType |Ingesteld op *Windows* om Windows-beveiliging in te scha kelen voor communicatie tussen client en knoop punten. |
| WindowsIdentities |Bevat het cluster en de client-id's. |
| ClusterIdentity |Gebruik de naam van een computer groep, domain\machinegroup, om beveiliging tussen knoop punten te configureren. |
| ClientIdentities |Hiermee configureert u beveiliging van client naar knoop punt. Een matrix met client-gebruikers accounts. |  
| Identiteit |Voeg de domein gebruiker domein\gebruikersnaam toe voor de client identiteit. |  
| IsAdmin |Stel deze waarde in op True om op te geven dat de domein gebruiker beheerders toegang of ONWAAR heeft voor toegang tot de client gebruiker. |  

[Knoop punt-naar-knooppunt beveiliging](service-fabric-cluster-security.md#node-to-node-security) wordt geconfigureerd met behulp van **ClusterIdentity** als u een computer groep binnen een Active Directory-domein wilt gebruiken. Zie [een machine groep maken in Active Directory](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx)voor meer informatie.

[Beveiliging van client naar knoop punt](service-fabric-cluster-security.md#client-to-node-security) wordt geconfigureerd met behulp van **ClientIdentities**. Om een vertrouwens relatie tussen een client en het cluster tot stand te brengen, moet u het cluster configureren om de client identiteiten te kennen die het cluster kan vertrouwen. U kunt vertrouwen op twee verschillende manieren tot stand brengen:

- Geef de gebruikers van de domein groep op die verbinding kunnen maken.
- Geef de gebruikers van het domein knooppunt op waarmee verbinding kan worden gemaakt.

Service Fabric ondersteunt twee verschillende typen toegangs beheer voor clients die zijn verbonden met een Service Fabric cluster: beheerder en gebruiker. Met toegangs beheer kan de Cluster beheerder de toegang tot bepaalde typen cluster bewerkingen voor verschillende groepen gebruikers beperken, waardoor het cluster veiliger wordt.  Beheerders hebben volledige toegang tot beheer mogelijkheden (inclusief Lees-en schrijf mogelijkheden). Gebruikers hebben standaard alleen lees toegang tot beheer mogelijkheden (bijvoorbeeld query mogelijkheden) en de mogelijkheid om toepassingen en services op te lossen.  

In het volgende voor beeld van een **beveiligings** sectie wordt Windows-beveiliging geconfigureerd, geeft u op dat de computers in *ServiceFabric/clustera. contoso. com* deel uitmaken van het cluster en geeft u op dat *CONTOSO\usera* toegang heeft tot de beheerder van de client:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
},
```

> [!NOTE]
> Service Fabric moet niet worden geïmplementeerd op een domein controller. Zorg ervoor dat ClusterConfig. json het IP-adres van de domein controller niet bevat wanneer u een machine groep of groep Managed Service account (gMSA) gebruikt.
>
>

## <a name="next-steps"></a>Volgende stappen
Nadat u de Windows-beveiliging in het bestand *ClusterConfig. json* hebt geconfigureerd, hervat u het proces voor het maken van het cluster in [een zelfstandig cluster maken dat wordt uitgevoerd in Windows](service-fabric-cluster-creation-for-windows-server.md).

Zie [scenario's voor cluster beveiliging](service-fabric-cluster-security.md)voor meer informatie over het beveiligen van knoop punten, de beveiliging van client naar knoop punt en toegangs beheer op basis van rollen.

Zie [verbinding maken met een beveiligd cluster](service-fabric-connect-to-secure-cluster.md) voor voor beelden van het maken van verbinding met behulp van Power shell of FabricClient.
