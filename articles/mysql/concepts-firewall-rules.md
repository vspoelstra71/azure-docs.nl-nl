---
title: Firewall regels-Azure Database for MySQL
description: Meer informatie over het gebruik van firewall regels om verbindingen met uw Azure Database for MySQL server in te scha kelen.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 01/15/2020
ms.openlocfilehash: a82d2317314c79a82fe80c5a25afc950fb728815
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "76155193"
---
# <a name="azure-database-for-mysql-server-firewall-rules"></a>Firewall regels van Azure Database for MySQL server
Firewalls verhinderen alle toegang tot uw database server totdat u opgeeft welke computers zijn gemachtigd. De firewall verleent toegang tot de server op basis van het oorspronkelijke IP-adres van elke aanvraag.

Als u een firewall wilt configureren, maakt u firewall regels die bereiken van acceptabele IP-adressen opgeven. U kunt Firewall regels maken op server niveau.

**Firewall regels:** Met deze regels kunnen clients toegang krijgen tot uw hele Azure Database for MySQL server, dat wil zeggen, alle data bases binnen dezelfde logische server. Firewall regels op server niveau kunnen worden geconfigureerd met behulp van de Azure Portal-of Azure CLI-opdrachten. Als u firewall regels op server niveau wilt maken, moet u de eigenaar van het abonnement of een mede werker van het abonnement zijn.

## <a name="firewall-overview"></a>Firewalloverzicht
Alle database toegang tot uw Azure Database for MySQL-server wordt standaard geblokkeerd door de firewall. Als u uw server vanaf een andere computer wilt gebruiken, moet u een of meer firewall regels op server niveau opgeven om toegang tot uw server mogelijk te maken. Gebruik de firewall regels om op te geven welke IP-adresbereiken van Internet moeten worden toegestaan. De firewall regels hebben geen invloed op de Azure Portal website zelf.

Verbindings pogingen via internet en Azure moeten eerst door de firewall worden door gegeven voordat ze uw Azure Database for MySQL-data base kunnen bereiken, zoals in het volgende diagram wordt weer gegeven:

![Voor beeld van de werking van de firewall](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>Verbinding maken vanuit internet
Firewall regels op server niveau zijn van toepassing op alle data bases op de Azure Database for MySQL-server.

Als het IP-adres van de aanvraag binnen een van de bereiken ligt die zijn opgegeven in de firewall regels op server niveau, wordt de verbinding verleend.

Als het IP-adres van de aanvraag buiten het bereik valt dat is opgegeven in een van de firewall regels op database niveau of op server niveau, mislukt de verbindings aanvraag.

## <a name="connecting-from-azure"></a>Verbinding maken vanuit Azure
Het is raadzaam om het uitgaande IP-adres van een toepassing of service te vinden en expliciet toegang tot die afzonderlijke IP-adressen of bereiken toe te staan. U kunt bijvoorbeeld het uitgaande IP-adres van een Azure App Service vinden of een open bare IP gebruiken dat is gekoppeld aan een virtuele machine of een andere bron (zie hieronder voor informatie over het maken van een verbinding met de privé-IP van een virtuele machine via service-eind punten). 

Als er geen vast uitgaand IP-adres beschikbaar is voor uw Azure-service, kunt u overwegen verbindingen van alle IP-adressen van Azure-Data Center in te scha kelen. Deze instelling kan worden ingeschakeld via de Azure Portal door de optie **toegang tot Azure-Services toestaan** in te stellen **op in** het deel venster **verbindings beveiliging** en op **Opslaan**te drukken. Vanuit de Azure CLI wordt een instelling van een firewall regel waarbij het begin-en eind adres gelijk is aan 0.0.0.0 het equivalent. Als de verbindings poging niet is toegestaan, wordt de Azure Database for MySQL server niet bereikt door de aanvraag.

> [!IMPORTANT]
> Met de optie **toegang tot Azure-Services toestaan wordt** de firewall zodanig geconfigureerd dat alle verbindingen van Azure, inclusief verbindingen van de abonnementen van andere klanten, worden toegestaan. Wanneer u deze optie selecteert, zorg dan dat uw aanmeldings- en gebruikersmachtigingen de toegang beperken tot alleen geautoriseerde gebruikers.
> 

![Toegang tot Azure-Services in de portal toestaan configureren](./media/concepts-firewall-rules/allow-azure-services.png)

### <a name="connecting-from-a-vnet"></a>Verbinding maken vanaf een VNet
Als u een beveiligde verbinding wilt maken met uw Azure Database for MySQL-server vanuit een VNet, kunt u gebruikmaken van [vnet-service-eind punten](./concepts-data-access-and-security-vnet.md). 

## <a name="programmatically-managing-firewall-rules"></a>Firewallregels programmatisch beheren
Naast de Azure Portal kunnen Firewall regels programmatisch worden beheerd met behulp van de Azure CLI. Zie ook [Azure database for MySQL firewall regels maken en beheren met behulp van Azure cli](./howto-manage-firewall-using-cli.md)

## <a name="troubleshooting-firewall-issues"></a>Problemen met de firewall oplossen
Houd rekening met de volgende punten wanneer de toegang tot de Microsoft Azure-Data Base voor MySQL-Server service niet naar behoren werkt:

* **Wijzigingen in de acceptatie lijst zijn nog niet doorgevoerd:** Het kan een vertraging van vijf minuten duren voordat wijzigingen in de configuratie van de Azure Database for MySQL Server-firewall van kracht worden.

* **De aanmelding is niet geautoriseerd of er is een onjuist wacht woord gebruikt:** Als een aanmelding geen machtigingen heeft op de Azure Database for MySQL server of het gebruikte wacht woord onjuist is, wordt de verbinding met de Azure Database for MySQL-server geweigerd. Door een firewallinstellingen te maken, krijgen clients alleen de mogelijkheid om te proberen verbinding te maken met de server. Elke client moet alsnog de benodigde beveiligingsreferenties opgeven.

* **Dynamisch IP-adres:** Als u een Internet verbinding hebt met dynamische IP-adres sering en u problemen ondervindt met het verkrijgen van de firewall, kunt u een van de volgende oplossingen proberen:

   * Vraag uw Internet provider (ISP) naar het IP-adres bereik dat is toegewezen aan uw client computers die toegang hebben tot de Azure Database for MySQL server en voeg het IP-adres bereik vervolgens toe als een firewall regel.

   * Vraag in plaats daarvan statische IP-adressen voor uw clientcomputers en voeg de IP-adressen als firewallregels toe.

* **Het IP-adres van de server lijkt openbaar te zijn:** Verbindingen met de Azure Database for MySQL-server worden doorgestuurd via een openbaar toegankelijke Azure-gateway. Het daadwerkelijke IP-adres van de server wordt echter beschermd door de firewall. Ga naar het artikel over de [connectiviteits architectuur](concepts-connectivity-architecture.md)voor meer informatie. 

## <a name="next-steps"></a>Volgende stappen

* [Azure Database for MySQL firewall regels maken en beheren met behulp van de Azure Portal](./howto-manage-firewall-using-portal.md)
* [Azure Database for MySQL firewall regels maken en beheren met Azure CLI](./howto-manage-firewall-using-cli.md)
- [VNet-service-eind punten in Azure Database for MySQL](./concepts-data-access-and-security-vnet.md)
