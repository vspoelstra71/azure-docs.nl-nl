---
title: DHCP beheren
description: In dit artikel wordt uitgelegd hoe u DHCP beheert in azure VMware-oplossing (AVS)
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 80791dd2041fb9d6fbc7c67f2d7d7b2d0b6c977e
ms.sourcegitcommit: 2721b8d1ffe203226829958bee5c52699e1d2116
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/28/2020
ms.locfileid: "84148358"
---
# <a name="how-to-manage-dhcp-in-azure-vmware-solution-avs-preview"></a>DHCP beheren in de preview-versie van Azure VMWare-oplossing (AVS)

NSX-T biedt de mogelijkheid om DHCP te configureren voor uw privécloud. Als u van plan bent NSX-T te gebruiken om uw DHCP-server te hosten, raadpleegt u [DHCP-server maken](#create-dhcp-server). Als u een externe DHCP-server van derden in uw netwerk hebt en u aanvragen wilt door sturen naar die DHCP-server, raadpleegt u [DHCP Relay-service maken](#create-dhcp-relay-service).

## <a name="create-dhcp-server"></a>DHCP-server maken

Gebruik de volgende stappen om een DHCP-server te configureren op NSX-T.

In NSX Manager gaat u naar het tabblad **netwerken** en selecteert u **DHCP** onder **IP-beheer**. Selecteer de knop **server toevoegen** . Geef vervolgens de server naam en het IP-adres van de server op. Wanneer u klaar bent, selecteert u **Opslaan**.

:::image type="content" source="./media/manage-dhcp/dhcp-server-settings.png" alt-text="DHCP-server toevoegen" border="true":::

### <a name="connect-dhcp-server-to-the-tier-1-gateway"></a>De DHCP-server verbinden met de laag-1-gateway.

Selecteer **Tier 1-gateways**, selecteer de gateway en selecteer **bewerken**

:::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway.png" alt-text="te gebruiken gateway selecteren" border="true":::

Een subnet toevoegen door **geen IP-toewijzings reeks** te selecteren

:::image type="content" source="./media/manage-dhcp/add-subnet.png" alt-text="een subnet toevoegen" border="true":::

Selecteer op het volgende scherm de optie **DHCP local server** in de vervolg keuzelijst **type** . Voor **DHCP-server**selecteert u **standaard DHCP** en selecteert u **Opslaan**.

:::image type="content" source="./media/manage-dhcp/set-ip-address-management.png" alt-text="opties voor DHCP-server selecteren" border="true":::

Selecteer **Opslaan**in het venster **Tier-1-gateway** . Op het volgende scherm ziet u de **wijzigingen**die zijn opgeslagen en selecteert u sluiten om de **bewerking** te volt ooien.

### <a name="add-a-network-segment"></a>Een netwerk segment toevoegen

Wanneer u de DHCP-server hebt gemaakt, moet u er netwerk segmenten aan toevoegen.

Selecteer in NSX-T het tabblad **netwerken** en selecteer **segmenten** onder **connectiviteit**. Selecteer **segment toevoegen**. Noem het segment en de verbinding met de gateway van de laag 1. Selecteer vervolgens **subnetten instellen** om een nieuw subnet te configureren. 

:::image type="content" source="./media/manage-dhcp/add-segment.png" alt-text="een nieuw netwerk segment toevoegen" border="true":::

Selecteer in het venster **subnetten instellen** de optie **subnet toevoegen**. Voer het IP-adres van de gateway en het DHCP-bereik in en selecteer **toevoegen** en vervolgens **Toep assen**

:::image type="content" source="./media/manage-dhcp/add-subnet-segment.png" alt-text="netwerk segment toevoegen" border="true":::

Wanneer u klaar bent, selecteert u **Opslaan** om het toevoegen van een netwerk segment te volt ooien.

:::image type="content" source="./media/manage-dhcp/segments-complete.png" alt-text="segmenten voltooid" border="true":::

## <a name="create-dhcp-relay-service"></a>DHCP Relay-service maken

Selecteer in het venster NXT-T het tabblad **netwerken** en selecteer **DHCP**in het vak **IP-beheer**. Selecteer **server toevoegen**. Kies DHCP-Relay voor het **server type** en voer de server naam en het IP-adres voor de relay-server in. Selecteer **Opslaan** om uw wijzigingen op te slaan.

:::image type="content" source="./media/manage-dhcp/create-dhcp-relay.png" alt-text="DHCP relay-server maken" border="true":::

Selecteer **laag-1 gateways** onder **connectiviteit**. Selecteer de verticale beletsel tekens op de laag-1-gateway en kies **bewerken**.

:::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway-relay.png" alt-text="gateway voor laag 1 bewerken" border="true":::

Selecteer **geen IP-toewijzings set** om de toewijzing van IP-adressen te definiëren.

:::image type="content" source="./media/manage-dhcp/edit-ip-address-allocation.png" alt-text="toewijzing van IP-adres bewerken" border="true":::

Selecteer in het dialoog venster, bij **type**, **DHCP relay-server**. Selecteer uw DHCP-doorstuur server in de vervolg keuzelijst **DHCP Relay** . Als u klaar bent, selecteert u **Opslaan**

:::image type="content" source="./media/manage-dhcp/set-ip-address-management-relay.png" alt-text="IP-adres beheer instellen" border="true":::

Geef een IP-adres voor het DHCP-bereik op in het segment:

> [!NOTE]
> Deze configuratie is vereist voor het realiseren van DHCP-Relay-functionaliteit op het DHCP-client segment. 

Onder **connectiviteit**selecteert u **segmenten**. Selecteer de verticale ellipsen en selecteer **bewerken**. Als u een nieuw segment wilt toevoegen, kunt u in plaats daarvan **segment toevoegen** selecteren om een nieuw segment te maken.

:::image type="content" source="./media/manage-dhcp/edit-segments.png" alt-text="een subnet van het netwerk bewerken" border="true":::

Voeg details over het segment toe. Selecteer de waarde onder **subnetten** of **Stel subnetten** in om het subnet toe te voegen of te wijzigen.

:::image type="content" source="./media/manage-dhcp/network-segments.png" alt-text="netwerk segmenten" border="true":::

Selecteer de verticale ellipsen en kies **bewerken**. Als u een nieuw subnet wilt maken, selecteert u **subnet toevoegen** om een gateway te maken en een DHCP-bereik te configureren. Geef het bereik van de IP-adres groep **op en selecteer Toep assen**en selecteer vervolgens **Opslaan** .

:::image type="content" source="./media/manage-dhcp/edit-subnet.png" alt-text="subnetten bewerken" border="true":::

Er is nu een DHCP-Server groep toegewezen aan het segment.

:::image type="content" source="./media/manage-dhcp/assigned-to-segment.png" alt-text="DHCP-Server groep toegewezen aan segment" border="true":::
