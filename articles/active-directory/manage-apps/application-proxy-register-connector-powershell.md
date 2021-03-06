---
title: Installatie op de achtergrond Azure AD-app proxy connector | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een installatie zonder toezicht van Azure AD-toepassingsproxy-connector uitvoert om veilige externe toegang tot uw on-premises apps te bieden.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/24/2020
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro, has-adal-ref
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9ae3cd491db03fd036869a8d86aeb646e3175b59
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2020
ms.locfileid: "82609966"
---
# <a name="create-an-unattended-installation-script-for-the-azure-ad-application-proxy-connector"></a>Een script voor installatie zonder toezicht maken voor de Azure AD-toepassingsproxy-connector

Dit onderwerp helpt u bij het maken van een Windows Power shell-script dat installatie zonder toezicht en registratie voor uw Azure AD-toepassingsproxy-connector mogelijk maakt.

Deze mogelijkheid is nuttig wanneer u het volgende wilt doen:

* Installeer de connector op Windows-servers waarop geen gebruikers interface is ingeschakeld of waartoe u geen toegang hebt met Extern bureaublad.
* Veel connectors tegelijk installeren en registreren.
* Integreer de installatie en registratie van de connector als onderdeel van een andere procedure.
* Maak een standaard installatie kopie van de server die de connector-bits bevat, maar niet is geregistreerd.

De connector van de [toepassings proxy](application-proxy-connectors.md) werkt alleen als deze is geregistreerd bij uw Azure AD-adres lijst met behulp van een toepassings beheerder en wacht woord. Normaal gesp roken wordt deze informatie tijdens de installatie van de connector ingevoerd in een pop-updialoogvenster, maar u kunt Power shell gebruiken om dit proces te automatiseren.

Er zijn twee stappen voor een installatie zonder toezicht. Installeer eerst de connector. Registreer vervolgens de connector bij Azure AD.

## <a name="install-the-connector"></a>De connector installeren
Gebruik de volgende stappen om de connector te installeren zonder deze te registreren:

1. Open een opdrachtprompt.
2. Voer de volgende opdracht uit, waarbij de/q betekent een stille installatie. Bij een stille installatie wordt u niet gevraagd om de gebruiksrecht overeenkomst te accepteren.

        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="register-the-connector-with-azure-ad"></a>De connector registreren bij Azure AD
Er zijn twee methoden die u kunt gebruiken om de connector te registreren:

* De connector registreren met een Windows Power shell-referentie object
* De connector registreren met een token dat offline is gemaakt

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>De connector registreren met een Windows Power shell-referentie object
1. Maak een Windows Power shell- `$cred` referentie object dat een beheerders naam en-wacht woord voor uw directory bevat. Voer de volgende opdracht uit en * \<Vervang\> de gebruikers naam* en * \<het wacht woord\>*:

        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. Ga naar de **map C:\Program Files\Microsoft Aad app proxy connector** en voer het volgende script `$cred` uit met behulp van het object dat u hebt gemaakt:

        .\RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred -Feature ApplicationProxy

### <a name="register-the-connector-using-a-token-created-offline"></a>De connector registreren met een token dat offline is gemaakt
1. Maak een offline token met behulp van de AuthenticationContext-klasse met behulp van de waarden in dit code fragment of Power shell-cmdlets hieronder:

    **Met C#:**

        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.microsoftonline.com/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }

    **Power shell gebruiken:**

        # Locate AzureAD PowerShell Module
        # Change Name of Module to AzureAD after what you have installed
        $AADPoshPath = (Get-InstalledModule -Name AzureAD).InstalledLocation
        # Set Location for ADAL Helper Library
        $ADALPath = $(Get-ChildItem -Path $($AADPoshPath) -Filter Microsoft.IdentityModel.Clients.ActiveDirectory.dll -Recurse ).FullName | Select-Object -Last 1

        # Add ADAL Helper Library
        Add-Type -Path $ADALPath

        #region constants

        # The AAD authentication endpoint uri
        [uri]$AadAuthenticationEndpoint = "https://login.microsoftonline.com/common/oauth2/token?api-version=1.0/"

        # The application ID of the connector in AAD
        [string]$ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489"

        # The reply address of the connector application in AAD
        [uri]$ConnectorRedirectAddress = "urn:ietf:wg:oauth:2.0:oob"

        # The AppIdUri of the registration service in AAD
        [uri]$RegistrationServiceAppIdUri = "https://proxy.cloudwebappproxy.net/registerapp"

        #endregion

        #region GetAuthenticationToken

        # Set AuthN context
        $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $AadAuthenticationEndpoint

        # Build platform parameters
        $promptBehavior = [Microsoft.IdentityModel.Clients.ActiveDirectory.PromptBehavior]::Always
        $platformParam = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.PlatformParameters" -ArgumentList $promptBehavior

        # Do AuthN and get token
        $authResult = $authContext.AcquireTokenAsync($RegistrationServiceAppIdUri.AbsoluteUri, $ConnectorAppId, $ConnectorRedirectAddress, $platformParam).Result

        # Check AuthN result
        If (($authResult) -and ($authResult.AccessToken) -and ($authResult.TenantId) ) {
        $token = $authResult.AccessToken
        $tenantId = $authResult.TenantId
        }
        Else {
        Write-Output "Authentication result, token or tenant id returned are null"
        }

        #endregion

2. Zodra u het token hebt, maakt u een SecureString met behulp van het token:

   `$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`

3. Voer de volgende Windows Power shell-opdracht \<uit en\> Vervang de Tenant-GUID door de map-id:

   `.\RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID> -Feature ApplicationProxy`

## <a name="next-steps"></a>Volgende stappen
* [Toepassingen publiceren met uw eigen domeinnaam (Engelstalig artikel)](application-proxy-configure-custom-domain.md)
* [Eenmalige aanmelding inschakelen](application-proxy-configure-single-sign-on-with-kcd.md)
* [Problemen met toepassingsproxy oplossen (Engelstalig artikel)](application-proxy-troubleshoot.md)
