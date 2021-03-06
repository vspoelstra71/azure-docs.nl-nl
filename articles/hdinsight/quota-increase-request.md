---
title: Verhogings aanvraag CPU-kern quota-Azure HDInsight
description: Leer het proces om een toename aan te vragen voor de CPU-kernen die aan uw abonnement zijn toegewezen.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 05/07/2020
ms.openlocfilehash: a708ad9626f4d6750de64097a7e1e9177c350e5e
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2020
ms.locfileid: "83717807"
---
# <a name="requesting-quota-increases-for-azure-hdinsight"></a>Quota aanvragen verhogen voor Azure HDInsight

CPU-kern quota helpen ervoor te zorgen dat het resource gebruik redelijk wordt gedistribueerd tussen alle klanten in een bepaalde Azure-regio. In bepaalde gevallen kunnen uw bedrijfs vereisten echter meer cluster bronnen vereisen dan het huidige quotum toestaat. In dergelijke gevallen kunt u een CPU-kern quotum toename aanvragen zodat u clusters kunt implementeren die overeenkomen met uw vereisten voor gegevens verwerking.

Wanneer u een quotum limiet bereikt, kunt u geen nieuwe clusters implementeren of bestaande clusters uitschalen door meer werk knooppunten toe te voegen. De enige quotum limiet is het quotum voor CPU-kernen dat voor elk abonnement bestaat op het niveau van de regio. Uw abonnement kan bijvoorbeeld een limiet van 30 CPU-kernen hebben in de regio VS-Oost, met nog eens 30 CPU-kernen die zijn toegestaan in het VS-Oost 2.

## <a name="gather-required-information"></a>Vereiste gegevens verzamelen

Als u een fout bericht hebt ontvangen dat aangeeft dat u een quotum limiet hebt bereikt, gebruikt u het proces dat in deze sectie wordt beschreven om belang rijke informatie te verzamelen en een verzoek om een quotum verhoging in te dienen.

1. Bepaal de gewenste grootte, schaal en type van de cluster-VM.
1. Controleer de huidige limieten voor quotum capaciteit van uw abonnement. Voer de volgende stappen uit om de beschik bare kernen te controleren:

    1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).
    1. Ga naar de **overzichts** pagina voor het HDInsight-cluster.
    1. Selecteer in het linkermenu **quotum limieten**. Op de pagina worden het aantal gebruikte kernen, het aantal beschikbare kernen en het totale aantal kernen weergegeven.

Voer de volgende stappen uit om een quotum verhoging aan te vragen:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).
1. Selecteer **Help en ondersteuning** aan de linkerkant van de pagina.

    ![knop Help en ondersteuning](./media/quota-increase-request/help-support-button.png)

1. Selecteer **Nieuwe ondersteuningsaanvraag**.
1. Selecteer op de pagina **Nieuwe ondersteuningsaanvraag** op het tabblad **Basisgegevens** de volgende opties:

   - **Probleem type**: **service-en abonnements limieten (quota's)**
   - **Abonnement**: het abonnement dat u wilt wijzigen
   - **Quotum type**: **HDInsight**

     ![Een ondersteunings aanvraag maken om het HDInsight-kern quotum te verhogen](./media/quota-increase-request/hdinsight-quota-support-request.png)

1. Selecteer **volgende: >>oplossingen **.
1. Voer op de pagina **Details** een beschrijving van het probleem in, selecteer de ernst van het probleem, uw favoriete contact wijze en andere vereiste velden. Gebruik de hieronder vermelde sjabloon om ervoor te zorgen dat u de benodigde informatie opgeeft. Quota verhogen aanvragen worden geëvalueerd door het Azure-capaciteits team en niet door het HDInsight-product team. Hoe meer volledige informatie u verstrekt, hoe waarschijnlijker uw aanvraag wordt goedgekeurd.

    ```
    I would like to request [SPECIFY DESIRED AMOUNT] on [DESIRED SKU] for [SUBSCRIPTION ID].
    
    My current quota on this subscription is [CURRENT QUOTA AMOUNT].
    
    I would like to use the extra cores for [DETAIL REASON].
    ```

    ![Details van probleem](./media/quota-increase-request/problem-details.png)

1. Selecteer **volgende: controleren + >>maken **.
1. Selecteer op het tabblad **controleren en maken** de optie **maken**.

> [!NOTE]  
> Als u het HDInsight core-quotum moet verhogen in een privé gebied, [dient u een white list-aanvraag](https://aka.ms/canaryintwhitelist)in.

U kunt [contact opnemen met de ondersteuning om een quotum verhoging aan te vragen](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request).

Er zijn enkele vaste quota limieten. Eén Azure-abonnement kan bijvoorbeeld Maxi maal 10.000 kernen hebben. Zie [Azure-abonnement en service limieten, quota's en beperkingen](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits)voor meer informatie over deze limieten.

## <a name="next-steps"></a>Volgende stappen

* [Stel clusters in hdinsight in met Apache Hadoop, Spark, Kafka en meer](hdinsight-hadoop-provision-linux-clusters.md): meer informatie over het instellen en configureren van clusters in hdinsight.
* [Prestaties van het cluster bewaken](hdinsight-key-scenarios-to-monitor.md): meer informatie over de belangrijkste scenario's voor het bewaken van uw HDInsight-cluster die van invloed kunnen zijn op de capaciteit van uw cluster.