---
title: Toegang tot Key Vault achter een firewall-Azure Key Vault | Microsoft Docs
description: Meer informatie over toegang tot Azure Key Vault vanuit een toepassing achter een firewall
services: key-vault
author: amitbapat
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: ambapat
ms.openlocfilehash: 07ae08f87c9a3e788944a48f6d5a24e2b076d16f
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2020
ms.locfileid: "82732322"
---
# <a name="access-azure-key-vault-behind-a-firewall"></a>Toegang tot Azure Key Vault achter een firewall

## <a name="what-ports-hosts-or-ip-addresses-should-i-open-to-enable-my-key-vault-client-application-behind-a-firewall-to-access-key-vault"></a>Welke poorten, hosts of IP-adressen moet ik openen om mijn sleutel kluis-client toepassing achter een firewall in te scha kelen voor toegang tot de sleutel kluis?

Als u toegang wilt krijgen tot een key vault, moet de clienttoepassing voor de key vault toegang hebben tot meerdere eindpunten voor verschillende functies:

* Verificatie via Azure Active Directory (Azure AD).
* Beheer van Azure Key Vault. Dit omvat het maken, lezen, bijwerken, verwijderen en instellen van toegangsbeleid via Azure Resource Manager.
* Toegang tot en beheer van objecten (sleutels en geheimen) die zijn opgeslagen in de Key Vault zelf die via het Key Vault-specifieke eindpunt lopen (bijvoorbeeld `https://yourvaultname.vault.azure.net`).  

Er zijn enkele verschillen, afhankelijk van uw configuratie en omgeving.

## <a name="ports"></a>Poorten

Al het verkeer naar de key vault voor de drie functies (verificatie, beheer en toegang tot gegevenslaag) gaat via HTTPS: poort 443. Voor CRL gaat het verkeer soms echter via HTTP (poort 80). Clients die ondersteuning bieden voor OCSP mogen geen CRL bereiken, maar kunnen [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl)af en toe bereikbaar zijn.  

## <a name="authentication"></a>Verificatie

Key vault-clienttoepassingen moet toegang hebben tot Azure Active Directory-eindpunten voor verificatie. Welk eindpunt wordt gebruikt, hangt af van de configuratie van de Azure AD-tenant en het type principal (gebruikers-principal of service-principal) en het type account, bijvoorbeeld een Microsoft-account of een werk- of schoolaccount.  

| Type principal | Eindpunt:poort |
| --- | --- |
| Gebruiker gebruikt Microsoft-account<br> (bijvoorbeeld, user@hotmail.com) |**Internationaal**<br> login.microsoftonline.com:443<br><br> **Azure China:**<br> login.chinacloudapi.cn:443<br><br>**Azure van de Amerikaanse overheid:**<br> login.microsoftonline.us:443<br><br>**Azure Duitsland:**<br> login.microsoftonline.de:443<br><br> en <br>login.live.com:443 |
| Gebruiker of service-principal gebruikt een werk- of schoolaccount met Microsoft Azure Active Directory (bijvoorbeeld user@contoso.com) |**Internationaal**<br> login.microsoftonline.com:443<br><br> **Azure China:**<br> login.chinacloudapi.cn:443<br><br>**Azure van de Amerikaanse overheid:**<br> login.microsoftonline.us:443<br><br>**Azure Duitsland:**<br> login.microsoftonline.de:443 |
| Gebruiker of service-principal gebruikt een werk- of schoolaccount, plus Active Directory Federation Services (AD FS) of andere federatief eindpunt (bijvoorbeeld user@contoso.com) |Alle eindpunten voor een werk- of schoolaccount, plus AD FS- of andere federatieve eindpunten |

Er zijn andere, mogelijk complexe scenario's. Raadpleeg [Azure Active Directory-verificatiestroom](../../active-directory/develop/authentication-scenarios.md), [Toepassingen integreren met Azure Active Directory](../../active-directory/develop/active-directory-how-to-integrate.md) en [Active Directory-verificatieprotocollen](https://msdn.microsoft.com/library/azure/dn151124.aspx) voor meer informatie.  

## <a name="key-vault-management"></a>Key Vault-beheer

Voor Key Vault-beheer (CRUD en toegangsbeleid instellen), moet de clienttoepassing voor de key vault toegang hebben tot het Azure Resource Manager-eindpunt.  

| Type bewerking | Eindpunt:poort |
| --- | --- |
| Bewerkingen voor de controlelaag van Key Vault<br> via Azure Resource Manager |**Internationaal**<br> management.azure.com:443<br><br> **Azure China:**<br> management.chinacloudapi.cn:443<br><br> **Azure van de Amerikaanse overheid:**<br> management.usgovcloudapi.net:443<br><br> **Azure Duitsland:**<br> management.microsoftazure.de:443 |
| Microsoft Graph API |**Internationaal**<br> graph.microsoft.com:443<br><br> **Azure China:**<br> graph.chinacloudapi.cn:443<br><br> **Azure van de Amerikaanse overheid:**<br> graph.microsoft.com:443<br><br> **Azure Duitsland:**<br> graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>Key Vault-bewerkingen

Voor alle beheer- en cryptografiebewerkingen van het key vault-object (sleutels en geheimen) moet de key vault-client toegang hebben tot het eindpunt van de key vault. Het DNS-achtervoegsel van het eindpunt verschilt afhankelijk van de locatie van de key vault. Het eindpunt van de key vault heeft de indeling *kluisnaam*.*regiospecifiek-dns-achtervoegsel*, zoals beschreven in de onderstaande tabel.  

| Type bewerking | Eindpunt:poort |
| --- | --- |
| Bewerkingen waaronder cryptografiebewerkingen voor sleutels, het maken, lezen, bijwerken en verwijderen van sleutels en geheimen, instellen of ophalen van tags en andere kenmerken voor key vault-objecten (sleutels of geheimen) |**Internationaal**<br> &lt;kluisnaam&gt;.vault.azure.net:443<br><br> **Azure China:**<br> &lt;kluisnaam&gt;.vault.azure.cn:443<br><br> **Azure van de Amerikaanse overheid:**<br> &lt;kluisnaam&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Duitsland:**<br> &lt;kluisnaam&gt;.vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>IP-adresbereiken

De Key Vault-service maakt gebruik van andere Azure-bronnen zoals de PaaS-infrastructuur. Daarom is het niet mogelijk om een specifiek bereik van IP-adressen op te geven die key vault-eindpunten op een bepaald moment hebben. Als uw firewall alleen IP-adresbereiken ondersteunt, raadpleegt u Microsoft Azure Data Center IP-bereiken beschik bare documenten op:
* [Public](https://www.microsoft.com/en-us/download/details.aspx?id=56519)
* [US Gov](https://www.microsoft.com/en-us/download/details.aspx?id=57063)
* [Duitsland](https://www.microsoft.com/en-us/download/details.aspx?id=57064)
* [China](https://www.microsoft.com/en-us/download/details.aspx?id=57062)

Authentication and Identity (Azure Active Directory) is een wereldwijde service waarmee failover naar andere regio's kan worden uitgevoerd of verkeer kan worden verplaatst zonder dit te melden. In dit scenario moeten alle IP-adresbereiken bij [IP-adressen voor Authentication and Identity](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity_ip) worden toegevoegd aan de firewall.

## <a name="next-steps"></a>Volgende stappen

Als u vragen hebt over Key Vault, gaat u naar de [Azure Key Vault forums](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
