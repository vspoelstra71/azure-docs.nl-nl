---
title: Voor beelden van JSON-claim transformatie voor aangepaste beleids regels
titleSuffix: Azure AD B2C
description: Voor beelden van JSON-claim transformatie voor het IEF-schema (Identity experience Framework) van Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 04/21/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: b42c2a414333e7ed262441321a808fc45425fc3b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "81756754"
---
# <a name="json-claims-transformations"></a>JSON-claim transformaties

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In dit artikel vindt u voor beelden voor het gebruik van de JSON-claim transformaties van het Framework voor identiteits ervaring in Azure Active Directory B2C (Azure AD B2C). Zie [ClaimsTransformations](claimstransformations.md)voor meer informatie.

## <a name="generatejson"></a>GenerateJson

Gebruik claim waarden of constanten voor het genereren van een JSON-teken reeks. De padtekenreeks volgende punt notatie wordt gebruikt om aan te geven waar de gegevens moeten worden ingevoegd in een JSON-teken reeks. Na splitsing per punt worden alle gehele getallen geïnterpreteerd als de index van een JSON-matrix en worden niet-gehele getallen geïnterpreteerd als de index van een JSON-object.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| Input claim | Wille keurige teken reeks na de punt notatie | tekenreeks | De JsonPath van de JSON waar de claim waarde wordt ingevoegd. |
| Parameter | Wille keurige teken reeks na de punt notatie | tekenreeks | De JsonPath van de JSON waar de constante teken reeks waarde wordt ingevoegd. |
| Output claim | Output claim | tekenreeks | De gegenereerde JSON-teken reeks. |

In het volgende voor beeld wordt een JSON-teken reeks gegenereerd op basis van de claim waarde "email" en "otp" en ook constante teken reeksen.

```XML
<ClaimsTransformation Id="GenerateRequestBody" TransformationMethod="GenerateJson">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="personalizations.0.to.0.email" />
    <InputClaim ClaimTypeReferenceId="otp" TransformationClaimType="personalizations.0.dynamic_template_data.otp" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="template_id" DataType="string" Value="d-4c56ffb40fa648b1aa6822283df94f60"/>
    <InputParameter Id="from.email" DataType="string" Value="service@contoso.com"/>
    <InputParameter Id="personalizations.0.subject" DataType="string" Value="Contoso account email verification code"/>
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="requestBody" TransformationClaimType="outputClaim"/>
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

De volgende claim transformatie voert een JSON-teken reeks claim uit die de hoofd tekst van de aanvraag is die wordt verzonden naar SendGrid (een e-mail provider van derden). De structuur van het JSON-object wordt gedefinieerd door de Id's in punt notatie van de invoer parameters en de TransformationClaimTypes van de InputClaims. Getallen in de punt notatie impliceren matrices. De waarden zijn afkomstig van de waarden van de InputClaims en de waarde van de eigenschap input parameters.

- Invoer claims:
  - **e-mail**, persoonlijke instellingen van het transformatie claim type **. 0. tot. 0. e-mail adres**:someone@example.com
  - persoonlijke instellingen voor **otp**, transformatie claim type **. 0. dynamic_template_data. otp** "346349"
- Invoer parameter:
  - **template_id**: "d-4c56ffb40fa648b1aa6822283df94f60"
  - **van. e-mail adres**:service@contoso.com
  - **persoonlijke instellingen. 0. onderwerp** "e-mail verificatie code contoso-account"
- Uitvoer claim:
  - **requestBody**: JSON-waarde

```JSON
{
  "personalizations": [
    {
      "to": [
        {
          "email": "someone@example.com"
        }
      ],
      "dynamic_template_data": {
        "otp": "346349",
        "verify-email" : "someone@example.com"
      },
      "subject": "Contoso account email verification code"
    }
  ],
  "template_id": "d-989077fbba9746e89f3f6411f596fb96",
  "from": {
    "email": "service@contoso.com"
  }
}
```

## <a name="getclaimfromjson"></a>GetClaimFromJson

Een opgegeven element ophalen uit een JSON-gegevens.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| Input claim | inputJson | tekenreeks | De ClaimTypes die worden gebruikt door de claim transformatie om het item op te halen. |
| Parameter | claimToExtract | tekenreeks | de naam van het JSON-element dat moet worden geëxtraheerd. |
| Output claim | extractedClaim | tekenreeks | Het claim type dat is geproduceerd nadat deze claim transformatie is aangeroepen, is de element waarde die is opgegeven in de invoer parameter _claimToExtract_ . |

In het volgende voor beeld heeft de claim transformatie het `emailAddress` element opgehaald uit de JSON-gegevens:`{"emailAddress": "someone@example.com", "displayName": "Someone"}`

```XML
<ClaimsTransformation Id="GetEmailClaimFromJson" TransformationMethod="GetClaimFromJson">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="customUserData" TransformationClaimType="inputJson" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="claimToExtract" DataType="string" Value="emailAddress" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="extractedEmail" TransformationClaimType="extractedClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoer claims:
  - **inputJson**: {"emailAddress": "someone@example.com", "DisplayName": "iemand"}
- Invoer parameter:
    - **claimToExtract**: emailAddress
- Uitvoer claims:
  - **extractedClaim**:someone@example.com


## <a name="getclaimsfromjsonarray"></a>GetClaimsFromJsonArray

Een lijst met opgegeven elementen uit de JSON-gegevens ophalen.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| Input claim | jsonSourceClaim | tekenreeks | De ClaimTypes die worden gebruikt door de claim transformatie om de claims op te halen. |
| Parameter | errorOnMissingClaims | booleaans | Hiermee geeft u op of er een fout moet worden gegenereerd als een van de claims ontbreekt. |
| Parameter | includeEmptyClaims | tekenreeks | Geef op of lege claims moeten worden toegevoegd. |
| Parameter | jsonSourceKeyName | tekenreeks | Sleutel naam van element |
| Parameter | jsonSourceValueName | tekenreeks | Naam van element waarde |
| Output claim | Verzameling | teken reeks, int, Boolean en datum/tijd |Lijst met te extra heren claims. De naam van de claim moet gelijk zijn aan die in _jsonSourceClaim_ -invoer claim. |

In het volgende voor beeld worden met de claim transformatie de volgende claims geëxtraheerd: e-mail (teken reeks), displayName (String), membershipNum (int), Active (Boolean) en geboorte datum (datetime) van de JSON-gegevens.

```JSON
[{"key":"email","value":"someone@example.com"}, {"key":"displayName","value":"Someone"}, {"key":"membershipNum","value":6353399}, {"key":"active","value":true}, {"key":"birthdate","value":"1980-09-23T00:00:00Z"}]
```

```XML
<ClaimsTransformation Id="GetClaimsFromJson" TransformationMethod="GetClaimsFromJsonArray">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="jsonSourceClaim" TransformationClaimType="jsonSource" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="errorOnMissingClaims" DataType="boolean" Value="false" />
    <InputParameter Id="includeEmptyClaims" DataType="boolean" Value="false" />
    <InputParameter Id="jsonSourceKeyName" DataType="string" Value="key" />
    <InputParameter Id="jsonSourceValueName" DataType="string" Value="value" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="membershipNum" />
    <OutputClaim ClaimTypeReferenceId="active" />
    <OutputClaim ClaimTypeReferenceId="birthdate" />
  </OutputClaims>
</ClaimsTransformation>
```

- Invoer claims:
  - **jsonSourceClaim**: [{"Key": "e-mail", "waarde": "someone@example.com"}, {"sleutel": "DisplayName", "waarde": "persoon"}, {"sleutel": "membershipNum", "waarde": 6353399}, {"sleutel", "actief", "waarde": True}, {"Key": "geboorte datum", "waarde": "1980-09-23T00:00:00Z"}]
- Invoer parameters:
    - **errorOnMissingClaims**: False
    - **includeEmptyClaims**: False
    - **jsonSourceKeyName**: sleutel
    - **jsonSourceValueName**: waarde
- Uitvoer claims:
  - **e-mail**:someone@example.com
  - **DisplayName**: ' iemand '
  - **membershipNum**: 6353399
  - **actief**: waar
  - **geboorte datum**: 1980-09-23T00:00:00Z

## <a name="getnumericclaimfromjson"></a>GetNumericClaimFromJson

Hiermee wordt een opgegeven numeriek (lang)-element opgehaald uit een JSON-gegevens.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| Input claim | inputJson | tekenreeks | De ClaimTypes die door de claim transformatie worden gebruikt om de claim op te halen. |
| Parameter | claimToExtract | tekenreeks | De naam van het JSON-element dat moet worden uitgepakt. |
| Output claim | extractedClaim | long | Het claim type dat is geproduceerd nadat deze ClaimsTransformation is aangeroepen, is de waarde van het element opgegeven in de _claimToExtract_ -invoer parameters. |

In het volgende voor beeld haalt de claim transformatie het `id` element uit de JSON-gegevens.

```JSON
{
    "emailAddress": "someone@example.com",
    "displayName": "Someone",
    "id" : 6353399
}
```

```XML
<ClaimsTransformation Id="GetIdFromResponse" TransformationMethod="GetNumericClaimFromJson">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="exampleInputClaim" TransformationClaimType="inputJson" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="claimToExtract" DataType="string" Value="id" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="membershipId" TransformationClaimType="extractedClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoer claims:
  - **inputJson**: {"emailAddress": "someone@example.com", "DisplayName": "iemand", "ID": 6353399}
- Invoer parameters
    - **claimToExtract**: id
- Uitvoer claims:
    - **extractedClaim**: 6353399

## <a name="getsingleitemfromjson"></a>GetSingleItemFromJson

Hiermee wordt het eerste element opgehaald uit een JSON-gegevens.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| Input claim | inputJson | tekenreeks | De ClaimTypes die door de claim transformatie worden gebruikt voor het ophalen van het item uit de JSON-gegevens. |
| Output claim | sleutel | tekenreeks | De eerste element sleutel in de JSON. |
| Output claim | waarde | tekenreeks | De eerste element waarde in de JSON. |

In het volgende voor beeld haalt de claim transformatie het eerste element (de opgegeven naam) uit de JSON-gegevens.

```XML
<ClaimsTransformation Id="GetGivenNameFromResponse" TransformationMethod="GetSingleItemFromJson">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="json" TransformationClaimType="inputJson" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="givenNameKey" TransformationClaimType="key" />
    <OutputClaim ClaimTypeReferenceId="givenName" TransformationClaimType="value" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoer claims:
  - **inputJson**: {"opgegeven naam": "Emilty", "LastName": "Smith"}
- Uitvoer claims:
  - **sleutel**: OpgegevenNaam
  - **waarde**: Emilty


## <a name="getsinglevaluefromjsonarray"></a>GetSingleValueFromJsonArray

Hiermee wordt het eerste element opgehaald uit een JSON-gegevens matrix.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| Input claim | inputJsonClaim | tekenreeks | De ClaimTypes die door de claim transformatie worden gebruikt om het item uit de JSON-matrix op te halen. |
| Output claim | extractedClaim | tekenreeks | Het claim type dat is geproduceerd nadat deze ClaimsTransformation is aangeroepen, het eerste element in de JSON-matrix. |

In het volgende voor beeld haalt de claim transformatie het eerste element (e-mail adres) uit de JSON `["someone@example.com", "Someone", 6353399]`-matrix.

```XML
<ClaimsTransformation Id="GetEmailFromJson" TransformationMethod="GetSingleValueFromJsonArray">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userData" TransformationClaimType="inputJsonClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="extractedClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoer claims:
  - **inputJsonClaim**: ["someone@example.com", "iemand", 6353399]
- Uitvoer claims:
  - **extractedClaim**:someone@example.com

## <a name="xmlstringtojsonstring"></a>XmlStringToJsonString

XML-gegevens worden geconverteerd naar de JSON-indeling.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| Input claim | xml | tekenreeks | De ClaimTypes die worden gebruikt door de claim transformatie voor het converteren van de gegevens van XML naar de JSON-indeling. |
| Output claim | json | tekenreeks | Het claim type dat is geproduceerd nadat deze ClaimsTransformation is aangeroepen, is de gegevens in JSON-indeling. |

```XML
<ClaimsTransformation Id="ConvertXmlToJson" TransformationMethod="XmlStringToJsonString">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="intpuXML" TransformationClaimType="xml" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="outputJson" TransformationClaimType="json" />
  </OutputClaims>
</ClaimsTransformation>
```

In het volgende voor beeld worden met de claim transformatie de volgende XML-gegevens geconverteerd naar de JSON-indeling.

#### <a name="example"></a>Voorbeeld
Invoer claim:

```XML
<user>
  <name>Someone</name>
  <email>someone@example.com</email>
</user>
```

Uitvoer claim:

```JSON
{
  "user": {
    "name":"Someone",
    "email":"someone@example.com"
  }
}
```


