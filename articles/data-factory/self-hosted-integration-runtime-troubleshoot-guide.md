---
title: Problemen met een zelf-hostende Integration runtime in Azure Data Factory oplossen
description: Meer informatie over het oplossen van problemen met zelf-hostende Integration runtime in Azure Data Factory.
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 11/07/2019
ms.author: abnarain
ms.openlocfilehash: f27132eb21d245d0d26de910abba088ba3b8efde
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2020
ms.locfileid: "84170969"
---
# <a name="troubleshoot-self-hosted-integration-runtime"></a>Problemen met zelf-hostende Integration runtime oplossen

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In dit artikel worden algemene probleemoplossings methoden besproken voor zelf-hostende Integration runtime in Azure Data Factory.

## <a name="common-errors-and-resolutions"></a>Veelvoorkomende fouten en oplossingen

### <a name="error-message-self-hosted-integration-runtime-cant-connect-to-cloud-service"></a>Fout bericht: zelf-hostende Integration runtime kan geen verbinding maken met de Cloud service

![Probleem met de zelf-hosted IR-verbinding](media/self-hosted-integration-runtime-troubleshoot-guide/unable-to-connect-to-cloud-service.png)

#### <a name="cause"></a>Oorzaak 

De zelf-hostende Integration runtime kan geen verbinding maken met de Data Factory Service (back-end). Dit probleem wordt meestal veroorzaakt door netwerk instellingen in de firewall.

#### <a name="resolution"></a>Oplossing

1. Controleer of de service Integration runtime actief is.
    
   ![Zelf-hostende status van de IR-service](media/self-hosted-integration-runtime-troubleshoot-guide/integration-runtime-service-running-status.png)
    
1. Als de service wordt uitgevoerd, gaat u naar stap 3.

1. Als er geen proxy is geconfigureerd voor de zelf-hostende Integration runtime (dit is de standaard instelling), voert u de volgende Power shell-opdracht uit op de computer waarop de zelf-hostende Integration runtime is geïnstalleerd:

    ```powershell
    (New-Object System.Net.WebClient).DownloadString("https://wu2.frontend.clouddatahub.net/")
    ```
        
   > [!NOTE]     
   > De service-URL kan variëren, afhankelijk van de locatie van uw Data Factory. U kunt de service-URL vinden onder **ADF UI**  >  **Connections**  >  **Integration Runtimes**  >  **bewerken zelf-hostende IR**-  >  **knoop punten**  >  **bekijken service-url's**.
            
    Hier volgt de verwachte reactie:
            
    ![Power shell-opdracht antwoord](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)
            
1. Als u het verwachte antwoord niet ontvangt, gebruikt u een van de volgende methoden, afhankelijk van uw situatie:
            
    * Als er een bericht wordt weer gegeven dat de externe naam niet kan worden omgezet, is er een probleem met de Domain Name System (DNS). Neem contact op met uw netwerk team om dit probleem op te lossen.
    * Als er een bericht wordt weer gegeven dat het SSL/TLS-certificaat niet wordt vertrouwd, controleert u of het certificaat voor https://wu2.frontend.clouddatahub.net/ wordt vertrouwd op de computer en installeert u vervolgens het open bare certificaat met behulp van certificaat beheer. Deze actie moet het probleem verminderen.
    * Ga naar **Windows**logboeken  >  toepassingen en services van Windows **(Logboeken)**  >  **Applications and Services Logs**  >  **Integration runtime** en controleer op fouten die worden veroorzaakt door DNS, een firewall regel of instellingen van het bedrijfs netwerk. (Als u een dergelijke fout vindt, sluit u de verbinding af.) Omdat elk bedrijf aangepaste netwerk instellingen heeft, neemt u contact op met uw netwerk team om deze problemen op te lossen.

1. Als "proxy" is geconfigureerd op de zelf-hostende Integration runtime, controleert u of de proxy server toegang heeft tot het service-eind punt. Zie [Power shell, webaanvragen en proxy's](https://stackoverflow.com/questions/571429/powershell-web-requests-and-proxies)voor een voor beeld van een opdracht.    
                
    ```powershell
    $user = $env:username
    $webproxy = (get-itemproperty 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet
    Settings').ProxyServer
    $pwd = Read-Host "Password?" -assecurestring
    $proxy = new-object System.Net.WebProxy
    $proxy.Address = $webproxy
    $account = new-object System.Net.NetworkCredential($user,[Runtime.InteropServices.Marshal]::PtrToStringAuto([Runtime.InteropServices.Marshal]::SecureStringToBSTR($pwd)), "")
    $proxy.credentials = $account
    $url = "https://wu2.frontend.clouddatahub.net/"
    $wc = new-object system.net.WebClient
    $wc.proxy = $proxy
    $webpage = $wc.DownloadData($url)
    $string = [System.Text.Encoding]::ASCII.GetString($webpage)
    $string
    ```

Hier volgt de verwachte reactie:
            
![Power shell-opdracht antwoord 2](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)

> [!NOTE] 
> Overwegingen voor de proxy:
> *    Controleer of de proxy server moet worden geplaatst in de lijst met veilige geadresseerden. Als dit het geval is, moet u ervoor zorgen dat [deze domeinen](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations#firewall-requirements-for-on-premisesprivate-network) zich in de lijst met veilige ontvangers bevinden.
> *    Controleer of het TLS/SSL-certificaat "wu2.frontend.clouddatahub.net/" wordt vertrouwd op de proxy server.
> *    Als u Active Directory verificatie gebruikt op de proxy, wijzigt u het service account in het gebruikers account dat toegang heeft tot de proxy als ' Integration Runtime-service.

### <a name="error-message-self-hosted-integration-runtime-node-logical-shir-is-in-inactive-running-limited-state"></a>Fout bericht: zelf-hostende Integration runtime-knoop punt/logische SHIR is in inactief/' wordt uitgevoerd (beperkt) '

#### <a name="cause"></a>Oorzaak 

Het zelf-hostende geïntegreerde runtime knooppunt heeft mogelijk een **inactieve** status, zoals wordt weer gegeven in de volgende scherm afbeelding:

![Niet-actief zelf-Hostend IR-knoop punt](media/self-hosted-integration-runtime-troubleshoot-guide/inactive-self-hosted-ir-node.png)

Dit gedrag treedt op wanneer knoop punten niet met elkaar kunnen communiceren.

#### <a name="resolution"></a>Oplossing

1. Meld u aan bij de door het knoop punt gehoste VM. Open in de **Logboeken toepassingen en services**  >  **Integration runtime**logboeken en filter alle fouten Logboeken.

1. Controleer of een fouten logboek de volgende fout bevat: 
    
    ```
    System.ServiceModel.EndpointNotFoundException: Could not connect to net.tcp://xxxxxxx.bwld.com:8060/ExternalService.svc/WorkerManager. The connection attempt lasted for a time span of 00:00:00.9940994. TCP error code 10061: No connection could be made because the target machine actively refused it 10.2.4.10:8060. 
    System.Net.Sockets.SocketException: No connection could be made because the target machine actively refused it. 
    10.2.4.10:8060
    at System.Net.Sockets.Socket.DoConnect(EndPoint endPointSnapshot, SocketAddress socketAddress)
    at System.Net.Sockets.Socket.Connect(EndPoint remoteEP)
    at System.ServiceModel.Channels.SocketConnectionInitiator.Connect(Uri uri, TimeSpan timeout)
    ```
       
1. Als u deze fout ziet, voert u het volgende uit op de opdracht regel: 

   ```
   telnet 10.2.4.10 8060
   ```
   
1. Als het volgende fout bericht wordt weer gegeven, neemt u contact op met uw IT-afdeling voor hulp bij het oplossen van dit probleem. Wanneer u met succes kunt Telnet, neemt u contact op met Microsoft Ondersteuning als u nog steeds problemen ondervindt met de status van het integrale runtime-knoop punt.
        
   ![Fout met opdracht regel](media/self-hosted-integration-runtime-troubleshoot-guide/command-line-error.png)
        
1. Controleer of het fouten logboek het volgende bevat:

    ```
    Error log: Cannot connect to worker manager: net.tcp://xxxxxx:8060/ExternalService.svc/ No DNS entries exist for host azranlcir01r1. No such host is known Exception detail: System.ServiceModel.EndpointNotFoundException: No DNS entries exist for host xxxxx. ---> System.Net.Sockets.SocketException: No such host is known at System.Net.Dns.GetAddrInfo(String name) at System.Net.Dns.InternalGetHostByName(String hostName, Boolean includeIPv6) at System.Net.Dns.GetHostEntry(String hostNameOrAddress) at System.ServiceModel.Channels.DnsCache.Resolve(Uri uri) --- End of inner exception stack trace --- Server stack trace: at System.ServiceModel.Channels.DnsCache.Resolve(Uri uri)
    ```
    
1. Probeer een of beide van de volgende methoden om het probleem op te lossen:
    - Plaats alle knoop punten in hetzelfde domein.
    - Voeg het IP-adres toe aan de host toewijzing in alle host-bestanden van de gehoste VM.


## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over het oplossen van problemen kunt u de volgende bronnen proberen:

*  [Data Factory Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory functie aanvragen](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure-video's](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Micro soft Q&een vraag pagina](https://docs.microsoft.com/answers/topics/azure-data-factory.html)
*  [Stack overflow-forum voor Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Twitter-informatie over Data Factory](https://twitter.com/hashtag/DataFactory)
*  [Prestatie gids gegevens stromen toewijzen](concepts-data-flow-performance.md)
