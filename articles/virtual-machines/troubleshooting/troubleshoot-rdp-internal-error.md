---
title: Er treedt een interne fout op wanneer u een RDP-verbinding maakt met Azure Virtual Machines | Microsoft Docs
description: Meer informatie over het oplossen van interne RDP-fouten in Microsoft Azure. | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/22/2018
ms.author: genli
ms.openlocfilehash: 8046e4f42db50db15c840a13b95ae1f3620a8c7f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "79266921"
---
#  <a name="an-internal-error-occurs-when-you-try-to-connect-to-an-azure-vm-through-remote-desktop"></a>Er treedt een interne fout op wanneer u via Extern bureaublad probeert te verbinden met een Azure-VM

In dit artikel wordt een fout beschreven die kan optreden wanneer u probeert verbinding te maken met een virtuele machine (VM) in Microsoft Azure.


## <a name="symptoms"></a>Symptomen

U kunt geen verbinding maken met een virtuele Azure-machine met behulp van Remote Desktop Protocol (RDP). De verbinding wordt verbroken op de sectie extern configureren of het volgende fout bericht wordt weer gegeven:

- Interne RDP-fout
- Er is een interne fout opgetreden
- Deze computer kan niet worden verbonden met de externe computer. Probeer opnieuw verbinding te maken. Als het probleem blijft bestaan, neemt u contact op met de eigenaar van de externe computer of de netwerk beheerder


## <a name="cause"></a>Oorzaak

Dit probleem kan de volgende oorzaken hebben:

- Kan geen toegang krijgen tot de lokale RSA-versleutelings sleutels.
- Het TLS-protocol is uitgeschakeld.
- Het certificaat is beschadigd of verlopen.

## <a name="solution"></a>Oplossing

Voordat u deze stappen volgt, moet u een moment opname maken van de besturingssysteem schijf van de betrokken VM als back-up. Zie [snap shot a disk](../windows/snapshot-copy-managed-disk.md)(Engelstalig) voor meer informatie.

U kunt dit probleem oplossen door de seriële console te gebruiken of [de virtuele machine offline te herstellen](#repair-the-vm-offline) door de besturingssysteem schijf van de virtuele machine aan een herstel-VM te koppelen.


### <a name="use-serial-control"></a>Serieel besturings element gebruiken

Maak verbinding met de [seriële console en open een Power shell-exemplaar](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
). Als de seriële console niet is ingeschakeld op uw virtuele machine, gaat u naar de sectie [de virtuele machine offline herstellen](#repair-the-vm-offline) .

#### <a name="step-1-check-the-rdp-port"></a>Stap: 1 Controleer de RDP-poort

1. Gebruik in een Power shell-exemplaar de [netstat](https://docs.microsoft.com/windows-server/administration/windows-commands/netstat
) om te controleren of poort 8080 wordt gebruikt door andere toepassingen:

        Netstat -anob |more
2. Als met de term. exe 8080 poort wordt gebruikt, gaat u naar stap 2. Als een andere service of andere toepassing dan de term. exe 8080 poort gebruikt, voert u de volgende stappen uit:

    1. Stop de service voor de toepassing die gebruikmaakt van de 3389-service:

            Stop-Service -Name <ServiceName> -Force

    2. Start de Terminal Service:

            Start-Service -Name Termservice

2. Als de toepassing niet kan worden gestopt of als deze methode niet op u van toepassing is, wijzigt u de poort voor RDP:

    1. Wijzig de poort:

            Set-ItemProperty -Path 'HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name PortNumber -value <Hexportnumber>

            Stop-Service -Name Termservice -Force
            
            Start-Service -Name Termservice 

    2. Stel de firewall in voor de nieuwe poort:

            Set-NetFirewallRule -Name "RemoteDesktop-UserMode-In-TCP" -LocalPort <NEW PORT (decimal)>

    3. [Werk de netwerk beveiligings groep bij voor de nieuwe poort](../../virtual-network/security-overview.md) in de Azure Portal RDP-poort.

#### <a name="step-2-set-correct-permissions-on-the-rdp-self-signed-certificate"></a>Stap 2: de juiste machtigingen voor het zelf-ondertekende RDP-certificaat instellen

1.  Voer in een Power shell-exemplaar de volgende opdrachten één voor één uit om het zelf-ondertekende RDP-certificaat te vernieuwen:

        Import-Module PKI 
    
        Set-Location Cert:\LocalMachine 
        
        $RdpCertThumbprint = 'Cert:\LocalMachine\Remote Desktop\'+((Get-ChildItem -Path 'Cert:\LocalMachine\Remote Desktop\').thumbprint) 
        
        Remove-Item -Path $RdpCertThumbprint

        Stop-Service -Name "SessionEnv"

        Start-Service -Name "SessionEnv"

2. Als u het certificaat niet kunt vernieuwen met behulp van deze methode, probeert u het zelf-ondertekende RDP-certificaat op afstand te vernieuwen:

    1. Voer vanuit een werkende VM die verbinding heeft met de virtuele machine die problemen ondervindt, **MMC** in het vak **uitvoeren** in om micro soft Management console te openen.
    2. Selecteer **module toevoegen/verwijderen**in het menu **bestand** , selecteer **certificaten**en selecteer vervolgens **toevoegen**.
    3. Selecteer **computer accounts**, selecteer **een andere computer**en voeg vervolgens het IP-adres van de probleem-VM toe.
    4. Ga naar de map **externe Desktop\Certificates** , klik met de rechter muisknop op het certificaat en selecteer **verwijderen**.
    5. Start in een Power shell-exemplaar van de seriële console de Extern bureaublad Configuration-service opnieuw:

            Stop-Service -Name "SessionEnv"

            Start-Service -Name "SessionEnv"
3. Stel de machtiging voor de map MachineKeys opnieuw in.

        remove-module psreadline icacls

        md c:\temp

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\BeforeScript_permissions.txt 
        
        takeown /f "C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys" /a /r

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\System:(F)"

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\NETWORK SERVICE:(R)"

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "BUILTIN\Administrators:(F)"

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\AfterScript_permissions.txt 
        
        Restart-Service TermService -Force

4. Start de virtuele machine opnieuw op en probeer vervolgens een Extern bureaublad verbinding met de virtuele machine te starten. Als de fout zich blijft voordoen, gaat u naar de volgende stap.

#### <a name="step-3-enable-all-supported-tls-versions"></a>Stap 3: alle ondersteunde TLS-versies inschakelen

De RDP-client gebruikt TLS 1,0 als het standaard protocol. Dit kan echter worden gewijzigd in TLS 1,1, die de nieuwe standaard is geworden. Als TLS 1,1 is uitgeschakeld op de virtuele machine, mislukt de verbinding.
1.  Schakel in een CMD-exemplaar het TLS-protocol in:

        reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWORD /d 1 /f
2.  Als u wilt voor komen dat het AD-beleid de wijzigingen overschrijft, moet u de groeps beleid-Update tijdelijk uitschakelen:

        REG add "HKLM\SYSTEM\CurrentControlSet\Services\gpsvc" /v Start /t REG_DWORD /d 4 /f
3.  Start de VM opnieuw op zodat de wijzigingen van kracht worden. Als het probleem is opgelost, voert u de volgende opdracht uit om het groeps beleid opnieuw in te scha kelen:

        sc config gpsvc start= auto sc start gpsvc

        gpupdate /force
    Als de wijziging wordt teruggedraaid, betekent dit dat er een Active Directory beleid in uw bedrijfs domein is. U moet dat beleid wijzigen om te voor komen dat dit probleem zich opnieuw voordoet.

### <a name="repair-the-vm-offline"></a>De virtuele machine offline herstellen

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>De besturingssysteem schijf koppelen aan een herstel-VM

1. [Koppel de besturingssysteem schijf aan een herstel-VM](../windows/troubleshoot-recovery-disks-portal.md).
2. Nadat de besturingssysteem schijf is gekoppeld aan de herstel-VM, controleert u of de schijf is gemarkeerd als **online** in de schijf beheer-console. Noteer de stationsletter die is toegewezen aan de gekoppelde besturingssysteem schijf.
3. Start een Extern bureaublad verbinding met de virtuele machine voor herstel.

#### <a name="enable-dump-log-and-serial-console"></a>Dump logboek en seriële console inschakelen

Voer het volgende script uit om dump logboek en seriële console in te scha kelen.

1. Open een opdracht prompt sessie met verhoogde bevoegdheden (**als administrator uitvoeren**).
2. Voer het volgende script uit:

    In dit script wordt ervan uitgegaan dat de stationsletter die is toegewezen aan de gekoppelde besturingssysteem schijf F is. Vervang deze stationsletter door de juiste waarde voor uw VM.

    ```
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

    REM Enable Serial Console
    bcdedit /store F:\boot\bcd /set {bootmgr} displaybootmenu yes
    bcdedit /store F:\boot\bcd /set {bootmgr} timeout 5
    bcdedit /store F:\boot\bcd /set {bootmgr} bootems yes
    bcdedit /store F:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    bcdedit /store F:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200

    REM Suggested configuration to enable OS Dump
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    reg unload HKLM\BROKENSYSTEM
    ```

#### <a name="reset-the-permission-for-machinekeys-folder"></a>De machtiging voor de map MachineKeys opnieuw instellen

1. Open een opdracht prompt sessie met verhoogde bevoegdheden (**als administrator uitvoeren**).
2. Voer het volgende script uit. In dit script wordt ervan uitgegaan dat de stationsletter die is toegewezen aan de gekoppelde besturingssysteem schijf F is. Vervang deze stationsletter door de juiste waarde voor uw VM.

        Md F:\temp

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\BeforeScript_permissions.txt
        
        takeown /f "F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys" /a /r

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\System:(F)"

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\NETWORK SERVICE:(R)"

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "BUILTIN\Administrators:(F)"

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\AfterScript_permissions.txt

#### <a name="enable-all-supported-tls-versions"></a>Alle ondersteunde TLS-versies inschakelen

1.  Open een opdracht prompt sessie met verhoogde bevoegdheden (**als administrator uitvoeren**) en voer de volgende opdrachten uit. In het volgende script wordt ervan uitgegaan dat de stationsletter is toegewezen aan de gekoppelde besturingssysteem schijf F. Vervang deze stationsletter door de juiste waarde voor uw VM.
2.  Controleren welke TLS is ingeschakeld:

        reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWO

3.  Als de sleutel niet bestaat of de waarde ervan **0**is, schakelt u het protocol in door de volgende scripts uit te voeren:

        REM Enable TLS 1.0, TLS 1.1 and TLS 1.2

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWORD /d 1 /f

4.  NLA inschakelen:

        REM Enable NLA

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD /d 1 /f reg unload HKLM\BROKENSYSTEM
5.  [Ontkoppel de besturingssysteem schijf en maak de virtuele machine opnieuw](../windows/troubleshoot-recovery-disks-portal.md)en controleer of het probleem is opgelost.





