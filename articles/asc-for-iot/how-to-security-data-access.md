---
title: Gegevens van aanbeveling van beveiliging &
description: Meer informatie over het verkrijgen van toegang tot uw beveiligings waarschuwing en aanbevelings gegevens bij het gebruik van Azure Security Center voor IoT.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: fbd96ddd-cd9f-48ae-836a-42aa86ca222d
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: bbea0accc79cafb6fea3f1438a71250dc02f4d62
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "81311010"
---
# <a name="access-your-security-data"></a>Toegang tot uw beveiligings gegevens

Azure Security Center voor IoT worden beveiligings waarschuwingen, aanbevelingen en onbewerkte beveiligings gegevens opgeslagen (als u ervoor kiest om deze op te slaan) in uw Log Analytics-werk ruimte.

## <a name="log-analytics"></a>Log Analytics

Configureren welke Log Analytics-werk ruimte wordt gebruikt:

1. Open uw IoT-hub.
1. Klik op de Blade **overzicht** onder het gedeelte **beveiliging**
1. Klik op **instellingen**en wijzig de configuratie van uw log Analytics-werk ruimte.

Voor toegang tot uw waarschuwingen en aanbevelingen in uw Log Analytics-werk ruimte na de configuratie:

1. Kies een waarschuwing of aanbeveling in Azure Security Center voor IoT.
1. Klik op **nader onderzoek**en klik vervolgens op **de kolom DeviceID om te zien op welke apparaten deze waarschuwing wordt weer gegeven**.

Zie [aan de slag met query's in log Analytics](https://docs.microsoft.com//azure/log-analytics/query-language/get-started-queries)voor meer informatie over het opvragen van gegevens uit log Analytics.

## <a name="security-alerts"></a>Beveiligingswaarschuwingen

Beveiligings waarschuwingen worden opgeslagen in de tabel _AzureSecurityOfThings. SecurityAlert_ in de werk ruimte log Analytics die is geconfigureerd voor de Azure Security Center voor IOT-oplossing.

We hebben een aantal nuttige query's ontvangen waarmee u aan de slag kunt met het verkennen van beveiligings waarschuwingen.

### <a name="sample-records"></a>Voorbeeld records

Selecteer een paar wille keurige records

```
// Select a few random records
//
SecurityAlert
| project
    TimeGenerated,
    IoTHubId=ResourceId,
    DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"]),
    AlertSeverity,
    DisplayName,
    Description,
    ExtendedProperties
| take 3
```

| TimeGenerated           | IoTHubId                                                                                                       | DeviceId      | AlertSeverity | DisplayName                           | Beschrijving                                             | ExtendedProperties                                                                                                                                                             |
|-------------------------|----------------------------------------------------------------------------------------------------------------|---------------|---------------|---------------------------------------|---------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 2018-11-18T18:10:29.000 | /Subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Hoog          | Beveiligings aanval geslaagd           | Er is een beveiligings aanval op het apparaat geslaagd        |    {"Full source Address": "[\"10.165.12.18:\"]", "gebruikers namen": "[\"\"]", "DeviceID": "IOT-Device-Linux"}                                                                       |
| 2018-11-19T12:40:31.000 | /Subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Hoog          | Geslaagde lokale aanmelding op het apparaat      | Er is een geslaagde lokale aanmelding voor het apparaat gedetecteerd     | {"Extern adres": "?", "externe poort": "", "lokale poort": "", "aanmeldings shell": "/bin/su", "aanmeldings proces-id": "28207", "gebruikers naam": "aanvaller", "DeviceId": "IoT-Device-Linux"} |
| 2018-11-19T12:40:31.000 | /Subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Hoog          | De lokale aanmeldings poging op het apparaat is mislukt  | Er is een mislukte lokale aanmeldings poging op het apparaat gedetecteerd |    {"Extern adres": "?", "externe poort": "", "lokale poort": "", "aanmeldings shell": "/bin/su", "aanmeldings proces-id": "22644", "gebruikers naam": "aanvaller", "DeviceId": "IoT-Device-Linux"} |

### <a name="device-summary"></a>Overzicht van apparaten

Het aantal afzonderlijke beveiligings waarschuwingen in de afgelopen week ontvangen, gegroepeerd op IoT Hub, apparaat, ernst van waarschuwing, waarschuwings type.

```
// Get the number of distinct security alerts detected in the last week, grouped by
//   IoT hub, device, alert severity, alert type
//
SecurityAlert
| where TimeGenerated > ago(7d)
| summarize Cnt=dcount(SystemAlertId) by
    IoTHubId=ResourceId,
    DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"]),
    AlertSeverity,
    DisplayName
```

| IoTHubId                                                                                                       | DeviceId      | AlertSeverity | DisplayName                           | Count |
|----------------------------------------------------------------------------------------------------------------|---------------|---------------|---------------------------------------|-----|
| /Subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Hoog          | Beveiligings aanval geslaagd           | 9   |
| /Subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Middelgroot        | De lokale aanmeldings poging op het apparaat is mislukt  | 242 |
| /Subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Hoog          | Geslaagde lokale aanmelding op het apparaat      | 31  |
| /Subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Middelgroot        | Crypto-munten Miner                     | 4   |

### <a name="iot-hub-summary"></a>Overzicht van IoT hub

Selecteer een aantal afzonderlijke apparaten met waarschuwingen in de afgelopen week, per IoT Hub, ernst van waarschuwing, waarschuwings type

```
// Select number of distinct devices which had alerts in the last week, by
//   IoT hub, alert severity, alert type
//
SecurityAlert
| where TimeGenerated > ago(7d)
| extend DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"])
| summarize CntDevices=dcount(DeviceId) by
    IoTHubId=ResourceId,
    AlertSeverity,
    DisplayName
```

| IoTHubId                                                                                                       | AlertSeverity | DisplayName                           | CntDevices |
|----------------------------------------------------------------------------------------------------------------|---------------|---------------------------------------|------------|
| /Subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Hoog          | Beveiligings aanval geslaagd           | 1          |
| /Subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Middelgroot        | De lokale aanmeldings poging op het apparaat is mislukt  | 1          |
| /Subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Hoog          | Geslaagde lokale aanmelding op het apparaat      | 1          |
| /Subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | Middelgroot        | Crypto-munten Miner                     | 1          |

## <a name="security-recommendations"></a>Aanbevelingen voor beveiliging

Beveiligings aanbevelingen worden opgeslagen in de tabel _AzureSecurityOfThings. SecurityRecommendation_ in de werk ruimte log Analytics die is geconfigureerd voor de Azure Security Center voor IOT-oplossing.

We hebben een aantal nuttige query's ontvangen waarmee u kunt beginnen met het verkennen van beveiligings aanbevelingen.

### <a name="sample-records"></a>Voorbeeld records

Selecteer een paar wille keurige records

```
// Select a few random records
//
SecurityRecommendation
| project
    TimeGenerated,
    IoTHubId=AssessedResourceId,
    DeviceId,
    RecommendationSeverity,
    RecommendationState,
    RecommendationDisplayName,
    Description,
    RecommendationAdditionalData
| take 2
```

| TimeGenerated | IoTHubId | DeviceId | RecommendationSeverity | RecommendationState | RecommendationDisplayName | Beschrijving | RecommendationAdditionalData |
|---------------|----------|----------|------------------------|---------------------|---------------------------|-------------|------------------------------|
| 2019-03-22T10:21:06.060 |    /Subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Middelgroot | Actief | Er is een strikte firewall regel in de invoer keten gevonden | Er is een regel in de firewall gevonden die een gebruiks patroon bevat voor een breed scala aan IP-adressen of poorten | {"Rules": "[\"{\"SourceAddress\"\":\",\"SourcePort\"\":\",\"DestinationAddress\"\":\",\"DestinationPort\":\"1337}]"} |
| 2019-03-22T10:50:27.237 | /Subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Middelgroot | Actief | Er is een strikte firewall regel in de invoer keten gevonden | Er is een regel in de firewall gevonden die een gebruiks patroon bevat voor een breed scala aan IP-adressen of poorten | {"Rules": "[\"{\"SourceAddress\"\":\",\"SourcePort\"\":\",\"DestinationAddress\"\":\",\"DestinationPort\":\"1337}]"} |

### <a name="device-summary"></a>Overzicht van apparaten

Haal het aantal verschillende actieve beveiligings aanbevelingen op, gegroepeerd op IoT Hub, apparaat, urgentie ernst en type.

```
// Get the number of distinct active security recommendations, grouped by by
//   IoT hub, device, recommendation severity and type
//
SecurityRecommendation
| extend IoTHubId=AssessedResourceId
| summarize CurrentState=arg_max(RecommendationState, DiscoveredTimeUTC) by IoTHubId, DeviceId, RecommendationSeverity, RecommendationDisplayName
| where CurrentState == "Active"
| summarize Cnt=count() by IoTHubId, DeviceId, RecommendationSeverity
```

| IoTHubId                                                                                                       | DeviceId      | RecommendationSeverity | Count |
|----------------------------------------------------------------------------------------------------------------|---------------|------------------------|-----|
| /Subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Hoog          | 2   |
| /Subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Middelgroot        | 1 |
| /Subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Hoog          | 1  |
| /Subscriptions/<subscription_id>/resourceGroups/<resource_group>/providers/Microsoft.Devices/IotHubs/<iot_hub> | <device_name> | Middelgroot        | 4   |

## <a name="next-steps"></a>Volgende stappen

- Lees het [overzicht](overview.md) van Azure Security Center voor IOT
- Meer informatie over Azure Security Center voor IoT- [architectuur](architecture.md)
- [Azure Security Center voor IOT-waarschuwingen](concept-security-alerts.md) begrijpen en verkennen
- [Azure Security Center voor IOT-aanbeveling](concept-recommendations.md) begrijpen en verkennen
