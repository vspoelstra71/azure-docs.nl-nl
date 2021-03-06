---
title: Snelstartgids voor Java-Web-apps voor micro soft Identity platform | Azure
description: Meer informatie over het implementeren van micro soft-aanmelding in een Java-Web-app met behulp van OpenID Connect Connect
services: active-directory
author: sangonzal
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 10/09/2019
ms.author: sagonzal
ms.custom: aaddev, scenarios:getting-started, languages:Java
ms.openlocfilehash: ed105ce6bd1d7d8980799049649b8d5b95dcb761
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "81536111"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-a-java-web-app"></a>Snelstartgids: aanmelden toevoegen met micro soft aan een Java-Web-app

In deze Quick Start leert u hoe u een Java-webtoepassing integreert met het micro soft Identity-platform. Uw app meldt zich aan bij een gebruiker, haalt een toegangs token op om de Microsoft Graph-API aan te roepen en brengt een aanvraag naar de Microsoft Graph-API.

Wanneer u deze Snelstartgids hebt voltooid, accepteert uw toepassing aanmeldingen van persoonlijke micro soft-accounts (waaronder outlook.com, live.com en anderen) en werk-of school accounts van een bedrijf of organisatie die gebruikmaakt van Azure Active Directory. (Zie [hoe het voor beeld werkt](#how-the-sample-works) voor een illustratie.)

## <a name="prerequisites"></a>Vereisten

Als u dit voor beeld wilt uitvoeren, hebt u het volgende nodig:

- [Java Development Kit (JDK)](https://openjdk.java.net/) 8 of hoger en [maven](https://maven.apache.org/).

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>De snelstart-app registreren en downloaden
> U hebt twee opties om uw Quick Start-toepassing te starten: Express (optie 1) of hand matig (optie 2)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Optie 1: de app registreren en automatisch configureren, en vervolgens de voorbeeldcode downloaden
>
> 1. Ga naar de [Azure Portal-app-registraties](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/JavaQuickstartPage/sourceType/docs) Snelstartgids.
> 1. Voer een naam in voor de toepassing en selecteer **Registreren**.
> 1. Volg de instructies in de Snelstartgids van de portal om de automatisch geconfigureerde toepassings code te downloaden.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Optie 2: registreer de toepassing en configureer handmatig de toepassing en het codevoorbeeld
>
> #### <a name="step-1-register-your-application"></a>Stap 1: Uw toepassing registreren
>
> Voer de volgende stappen uit om uw toepassing te registreren en de registratie gegevens van de app hand matig toe te voegen aan uw toepassing:
>
> 1. Meld u aan bij de [Azure Portal](https://portal.azure.com) met behulp van een werk-of school account of een persoonlijke Microsoft-account.
> 1. Als u via uw account toegang tot meer dan één tenant hebt, selecteert u uw account in de rechterbovenhoek en stelt u uw portalsessie in op de gewenste Azure Active Directory-tenant.
>
> 1. Navigeer naar de pagina micro soft-identiteits platform voor ontwikkel aars [app-registraties](https://go.microsoft.com/fwlink/?linkid=2083908) .
> 1. Selecteer **nieuwe registratie**.
> 1. Wanneer de pagina **Een toepassing registreren** verschijnt, voert u de registratiegegevens van de toepassing in:
>    - Voer in de sectie **Naam** een beschrijvende toepassingsnaam in die zichtbaar is voor gebruikers van de app. Bijvoorbeeld: `java-webapp`.
>    - Selecteer **Registreren**.
> 1. Zoek op de pagina **overzicht** de **client-id** van de toepassing en de id van de **Directory (Tenant)** van de toepassing. Kopieer deze waarden voor later.
> 1. Selecteer de **verificatie** in het menu en voeg de volgende gegevens toe:
>    - Voeg de **Web** webplatform configuratie toe.  Deze `https://localhost:8080/msal4jsample/secure/aad` en `https://localhost:8080/msal4jsample/graph/me` als **omleidings-uri's**toevoegen.
>    - Selecteer **Opslaan**.
> 1. Selecteer de **certificaten & geheimen** in het menu en klik in de sectie **client geheimen** op **Nieuw client geheim**:
>
>    - Typ een beschrijving voor de sleutel (bijvoorbeeld het geheim van een app-exemplaar).
>    - Selecteer een sleutel duur **in 1 jaar**.
>    - De sleutel waarde wordt weer gegeven wanneer u **toevoegen**selecteert.
>    - Kopieer de waarde van de sleutel voor later. Deze sleutel waarde wordt niet opnieuw weer gegeven en kan niet worden opgehaald op een andere manier, dus noteer deze zo snel als deze wordt weer gegeven in de Azure Portal.
>
> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Stap 1: uw toepassing configureren in de Azure-portal
>
> Het code voorbeeld voor deze Quick Start werkt alleen als u:
>
> 1. Antwoord-Url's toevoegen `https://localhost:8080/msal4jsample/secure/aad` als `https://localhost:8080/msal4jsample/graph/me`en.
> 1. Maak een client geheim.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Breng deze wijzigingen voor mij aan]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Al geconfigureerd](media/quickstart-v2-aspnet-webapp/green-check.png) Uw toepassing is al geconfigureerd met deze kenmerken.

#### <a name="step-2-download-the-code-sample"></a>Stap 2: het code voorbeeld downloaden
> [!div renderon="docs"]
> [Het code voorbeeld downloaden](https://github.com/Azure-Samples/ms-identity-java-webapp/archive/master.zip)

> [!div class="sxs-lookup" renderon="portal"]
> Down load het project en pak het zip-bestand uit naar een lokale map dichter bij de hoofdmap, bijvoorbeeld **C:\Azure-samples**
>
> Als u HTTPS met localhost wilt gebruiken, vult u de eigenschappen server. SSL. key in. Gebruik het hulp programma voor het maken van een zelfondertekend certificaat (opgenomen in JRE).
>
>  ```
>   Example:
>   keytool -genkeypair -alias testCert -keyalg RSA -storetype PKCS12 -keystore keystore.p12 -storepass password
>
>   server.ssl.key-store-type=PKCS12
>   server.ssl.key-store=classpath:keystore.p12
>   server.ssl.key-store-password=password
>   server.ssl.key-alias=testCert
>   ```
>   Plaats het gegenereerde bestand van de opslag in de map resources.

> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Het code voorbeeld downloaden](https://github.com/Azure-Samples/ms-identity-java-webapp/archive/master.zip)

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div renderon="docs"]
> #### <a name="step-3-configure-the-code-sample"></a>Stap 3: het code voorbeeld configureren
> 1. Pak het zipbestand uit in een lokale map.
> 1. Als u een Integrated Development Environment gebruikt, opent u het voor beeld in uw favoriete IDE (optioneel).
> 1. Open het bestand Application. Properties, dat u kunt vinden in src/main/resources/folder en vervang de waarde van de velden *Aad. clientId*, *Aad. Authority* en *Aad. secretKey* met de respectieve waarden van de **toepassings-id**, **Tenant-id** en **client geheim** als volgt:
>
>    ```file
>    aad.clientId=Enter_the_Application_Id_here
>    aad.authority=https://login.microsoftonline.com/Enter_the_Tenant_Info_Here/
>    aad.secretKey=Enter_the_Client_Secret_Here
>    aad.redirectUriSignin=https://localhost:8080/msal4jsample/secure/aad
>    aad.redirectUriGraph=https://localhost:8080/msal4jsample/graph/me
>    aad.msGraphEndpointHost="https://graph.microsoft.com/"
>    ```
> Waar:
>
> - `Enter_the_Application_Id_here`: de toepassings-id voor de toepassing die u hebt geregistreerd.
> - `Enter_the_Client_Secret_Here`-is het **client geheim** dat u in **certificaten & geheimen** hebt gemaakt voor de toepassing die u hebt geregistreerd.
> - `Enter_the_Tenant_Info_Here`-is de **ID-waarde van de directory (Tenant)** van de toepassing die u hebt geregistreerd.
> 1. Als u HTTPS met localhost wilt gebruiken, vult u de eigenschappen server. SSL. key in. Gebruik het hulp programma voor het maken van een zelfondertekend certificaat (opgenomen in JRE).
>
>  ```
>   Example:
>   keytool -genkeypair -alias testCert -keyalg RSA -storetype PKCS12 -keystore keystore.p12 -storepass password
>
>   server.ssl.key-store-type=PKCS12
>   server.ssl.key-store=classpath:keystore.p12
>   server.ssl.key-store-password=password
>   server.ssl.key-alias=testCert
>   ```
>   Plaats het gegenereerde bestand van de opslag in de map resources.


> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-run-the-code-sample"></a>Stap 3: het code voorbeeld uitvoeren
> [!div renderon="docs"]
> #### <a name="step-4-run-the-code-sample"></a>Stap 4: het code voorbeeld uitvoeren

Als u het project wilt uitvoeren, kunt u het volgende doen:

Voer de toepassing rechtstreeks vanuit uw IDE uit met behulp van de Inge sloten Spring-opstart server of pak deze uit naar een WAR-bestand met behulp van [maven](https://maven.apache.org/plugins/maven-war-plugin/usage.html) en implementeer dit voor een J2EE-container oplossing zoals [Apache Tomcat](http://tomcat.apache.org/).

##### <a name="running-from-ide"></a>Uitvoeren vanaf IDE

Als u de webtoepassing vanuit een IDE uitvoert, klikt u op uitvoeren en navigeert u naar de start pagina van het project. Voor dit voor beeld is https://localhost:8080de URL van de standaard startpagina.

1. Op de voor pagina selecteert u de knop **Aanmelden** om om te leiden naar Azure Active Directory en vraagt u de gebruiker om referenties.

1. Nadat de gebruiker is geverifieerd, wordt deze omgeleid naar *https://localhost:8080/msal4jsample/secure/aad*. Ze zijn nu aangemeld en op de pagina wordt informatie over het aangemelde account weer gegeven. De voor beeld-UI heeft de volgende knoppen:
    - *Afmelden*: meldt de huidige gebruiker uit de toepassing en leidt deze om naar de start pagina.
    - *Gebruikers gegevens weer geven*: verkrijgt een token voor Microsoft Graph en roept Microsoft Graph aan met een aanvraag met het token, waarmee basis informatie over de aangemelde gebruiker wordt geretourneerd.

##### <a name="running-from-tomcat"></a>Uitvoeren vanaf Tomcat

Als u het webvoorbeeld wilt implementeren op Tomcat, moet u een aantal wijzigingen aanbrengen in de bron code.

1. Open MS-Identity-Java-webapp/pom. XML
    - Onder `<name>msal-web-sample</name>` toevoegen`<packaging>war</packaging>`
    - Afhankelijkheid toevoegen:

         ```xml
         <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-tomcat</artifactId>
          <scope>provided</scope>
         </dependency>
         ```

2. Open MS-Identity-Java-webapp/src/main/Java/com. micro soft. Azure. msalwebsample/MsalWebSampleApplication

    - Verwijder alle bron code en vervang door het volgende:

   ```Java
    package com.microsoft.azure.msalwebsample;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.boot.builder.SpringApplicationBuilder;
    import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;

    @SpringBootApplication
    public class MsalWebSampleApplication extends SpringBootServletInitializer {

     public static void main(String[] args) {
      SpringApplication.run(MsalWebSampleApplication.class, args);
     }

     @Override
     protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
      return builder.sources(MsalWebSampleApplication.class);
     }
    }
   ```

3. Open een opdracht prompt, ga naar de hoofdmap van het project en voer`mvn package`
    - Hiermee wordt een `msal-web-sample-0.1.0.war` bestand in de/targets-map gegenereerd.
    - Wijzig de naam van dit bestand in`ROOT.war`
    - Implementeer dit war-bestand met behulp van Tomcat of een andere J2EE-container oplossing.
        - Als u wilt implementeren op een Tomcat-container, kopieert u het. war-bestand naar de map webapps onder uw tomcat-installatie en start u vervolgens de Tomcat-server.

Deze WAR wordt automatisch gehost op https://localhost:8080/.

> [!IMPORTANT]
> Deze quickstarttoepassing gebruikt een clientgeheim om zichzelf te identificeren als vertrouwelijke client. Omdat het client geheim als tekst zonder opmaak wordt toegevoegd aan uw project bestanden, wordt u uit veiligheids overwegingen aangeraden een certificaat te gebruiken in plaats van een client geheim voordat u de toepassing als productie toepassing overweegt. Zie [certificaat referenties voor toepassings verificatie](https://docs.microsoft.com/azure/active-directory/develop/active-directory-certificate-credentials)voor meer informatie over het gebruik van een certificaat.

## <a name="more-information"></a>Meer informatie

### <a name="how-the-sample-works"></a>Hoe het voor beeld werkt
![Toont hoe de voor beeld-app die door deze Quick start is gegenereerd, werkt](media/quickstart-v2-java-webapp/java-quickstart.svg)

### <a name="getting-msal"></a>MSAL ophalen

MSAL for Java (MSAL4J) is de Java-bibliotheek die wordt gebruikt voor het aanmelden van gebruikers en het aanvragen van tokens die worden gebruikt voor toegang tot een API die wordt beveiligd door het micro soft Identity-platform.

Voeg MSAL4J toe aan uw toepassing met behulp van Maven of Gradle om uw afhankelijkheden te beheren door de volgende wijzigingen aan te brengen in het pom. XML-bestand (Maven) of build. Gradle (Gradle) van de toepassing.

In pom. XML:

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>msal4j</artifactId>
    <version>1.0.0</version>
</dependency>
```

In build. gradle:

```$xslt
compile group: 'com.microsoft.azure', name: 'msal4j', version: '1.0.0'
```

### <a name="msal-initialization"></a>MSAL initialiseren

Voeg een verwijzing naar MSAL voor Java toe door de volgende code toe te voegen aan de bovenkant van het bestand waarin u MSAL4J gaat gebruiken:

```Java
import com.microsoft.aad.msal4j.*;
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over machtigingen en toestemming:

> [!div class="nextstepaction"]
> [Machtigingen en toestemming](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent)

Zie voor meer informatie over de verificatie stroom voor dit scenario de OAuth 2,0-autorisatie code stroom:

> [!div class="nextstepaction"]
> [OAuth-stroom van autorisatie code](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-auth-code-flow)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
