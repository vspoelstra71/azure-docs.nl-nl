---
title: Beveiligings beveiliging in hosts van virtuele AKS-machines
description: Meer informatie over de beveiligings beveiliging in AKS VM host-besturings systeem
services: container-service
author: mlearned
ms.topic: article
ms.date: 09/11/2019
ms.author: mlearned
ms.custom: mvc
ms.openlocfilehash: b7552fc083c5ed340dc54c2a31160b0c8b4bd076
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80420906"
---
# <a name="security-hardening-for-aks-agent-node-host-os"></a>Beveiligings beveiliging voor AKS-agent knooppunt host-besturings systeem

Azure Kubernetes service (AKS) is een veilige service die voldoet aan SOC-, ISO-, PCI DSS-en HIPAA-normen. Dit artikel heeft betrekking op de beveiligings beveiliging die wordt toegepast op hosts met virtuele AKS-machines. Zie voor meer informatie over AKS-beveiliging [beveiligings concepten voor toepassingen en clusters in azure Kubernetes service (AKS)](https://docs.microsoft.com/azure/aks/concepts-security).

> [!Note]
> Dit document is alleen van toepassing op Linux-agents in AKS.

AKS-clusters worden geïmplementeerd op virtuele machines van de host, die een door beveiliging geoptimaliseerd besturings systeem uitvoeren dat wordt gebruikt voor containers die worden uitgevoerd op AKS. Dit hostbesturingssysteem is gebaseerd op een **Ubuntu 16.04. LTS** -installatie kopie met extra beveiligings beveiliging en-optimalisaties die zijn toegepast (Zie Details van beveiligings beveiliging).

Het doel van het besturings systeem voor de beveiliging van beveiligde hosts is het verminderen van de surface area van de aanval en het optimaliseren van de implementatie van containers op een veilige manier.

> [!Important]
> Het beveiligde besturings systeem is niet CIS-benchmarkd. Hoewel er overlap pingen met CIS-benchmarks zijn, is het doel geen CIS-compatibel. Het doel van de beveiliging van host-besturings systemen is het convergeren op een beveiligings niveau dat overeenkomt met de interne beveiligings normen van micro soft.

## <a name="security-hardening-features"></a>Functies voor beveiligings beveiliging

* AKS biedt standaard een geoptimaliseerd host-besturings systeem voor beveiliging. Er is geen optie om een ander besturings systeem te selecteren.

* Azure past dagelijkse patches (inclusief beveiligings patches) toe aan AKS van virtuele machines. Voor sommige van deze patches moet de computer opnieuw worden opgestart, terwijl anderen dat niet doen. U bent zelf verantwoordelijk voor het plannen van het opnieuw opstarten van AKS VM-host. Zie [patching AKS nodes](https://docs.microsoft.com/azure/aks/node-updates-kured)(Engelstalig) voor meer informatie over het AUTOMATISEREN van AKS-patches.

## <a name="what-is-configured"></a>Wat is er geconfigureerd?

| CIS  | Beschrijving van controle|
|---|---|
| 1.1.1.1 |Controleren of het koppelen van cramfs-bestands systeem is uitgeschakeld|
| 1.1.1.2 |Controleren of het koppelen van freevxfs-bestands systeem is uitgeschakeld|
| 1.1.1.3 |Controleren of het koppelen van JFFS2-bestands systeem is uitgeschakeld|
| 1.1.1.4 |Controleren of het koppelen van HFS-bestands systeem is uitgeschakeld|
| 1.1.1.5 |Zorg ervoor dat het koppelen van HFS plus bestands systemen is uitgeschakeld|
|1.4.3 |Controleren of de verificatie is vereist voor de modus voor één gebruiker |
|1.7.1.2 |Controleren of de banner van de lokale aanmeldings waarschuwing correct is geconfigureerd |
|1.7.1.3 |Zorg ervoor dat de banner waarschuwing voor externe aanmelding correct is geconfigureerd |
|1.7.1.5 |Zorg ervoor dat de machtigingen voor/etc/issue zijn geconfigureerd |
|1.7.1.6 |Zorg ervoor dat de machtigingen voor/etc/issue.net zijn geconfigureerd |
|versie 2.1.5 |Zorg ervoor dat--streaming-Connection-time-out niet is ingesteld op 0 |
|3.1.2 |Zorgen dat verzenden pakket omleiding is uitgeschakeld |
|3.2.1 |Controleren of de door de bron gerouteerde pakketten niet worden geaccepteerd |
|3.2.2 |Controleren of ICMP-omleidingen niet worden geaccepteerd |
|3.2.3 |Controleren of beveiligde ICMP-omleidingen worden niet geaccepteerd |
|3.2.4 |Controleren of verdachte pakketten worden geregistreerd |
|3.3.1 |Controleren of IPv6-router-advertisements niet worden geaccepteerd |
|3.5.1 |Zorg ervoor dat DCCP is uitgeschakeld |
|3.5.2 |Zorg ervoor dat SCTP is uitgeschakeld |
|3.5.3 |Zorg ervoor dat RDS is uitgeschakeld |
|3.5.4 |Zorg ervoor dat TIPC is uitgeschakeld |
|4.2.1.2 |Controleren of logboek registratie is geconfigureerd |
|5.1.2 |Zorg ervoor dat de machtigingen voor/etc/crontab zijn geconfigureerd |
|5.2.4 |Controleren of het door sturen van SSH-X11 is uitgeschakeld |
|5.2.5 |Zorg ervoor dat de SSH-MaxAuthTries is ingesteld op 4 of minder |
|5.2.8 |Controleren of SSH-basis aanmelding is uitgeschakeld |
|5.2.10 |Controleren of SSH-PermitUserEnvironment is uitgeschakeld |
|5.2.11 |Zorg ervoor dat alleen goedgekeurde maximum algoritmen worden gebruikt |
|5.2.12 |Controleer of het time-outinterval voor SSH-inactiviteit is geconfigureerd |
|5.2.13 |Zorg ervoor dat de SSH-LoginGraceTime is ingesteld op één minuut of minder |
|5.2.15 |Zorg ervoor dat de banner SSH-waarschuwing is geconfigureerd |
|5.3.1 |Zorg ervoor dat vereisten voor het maken van wacht woorden zijn geconfigureerd |
|5.4.1.1 |Controleren of het wacht woord verloopt 90 dagen of minder |
|5.4.1.4 |Controleren of inactieve wachtwoord vergrendeling 30 dagen of minder is |
|5.4.4 |Zorg ervoor dat de umask van de standaard gebruiker 027 of meer beperkend is |
|5,6 |Controleren of de toegang tot de su-opdracht is beperkt|

## <a name="additional-notes"></a>Aanvullende opmerkingen
 
* Om het aanvals surface area verder te verminderen, zijn er overbodige Stuur Programma's voor de kernel-module in het besturings systeem uitgeschakeld.

* Het beveiligde besturings systeem van de beveiliging is speciaal gemaakt en wordt onderhouden specifiek voor AKS en wordt niet ondersteund buiten het AKS-platform.

## <a name="next-steps"></a>Volgende stappen  

Raadpleeg de volgende artikelen voor meer informatie over AKS-beveiliging: 

[Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/intro-kubernetes)

[Beveiligings overwegingen voor AKS](https://docs.microsoft.com/azure/aks/concepts-security)

[Best practices voor AKS](https://docs.microsoft.com/azure/aks/best-practices)
