# win11_stop.bat
Win11  Prevent update (Frissítés megakadályozása)
----------------------------------------

Alul Magyar nyelvű leírást is találsz!

----------------------------------------


Windows 11 Update Manager (Update Blocker)
----------------------------------------

This project provides an automated solution to prevent a forced upgrade to Windows 11 on Windows 10 systems. It is especially useful for laptops that are not compatible, but update unsolicited, after which the machine often needs to be reinstalled.

🚀 Key features
----------------------------------------
Automatic version detection: Queries the current Windows 10 version (e.g. 22H2) and fixes the system to it).
Registry-based blocking: Modifies the Registry using the safest method.
Service Management: Temporarily stops the Windows Update (wuauserv) and BITS services (pp. 8-9) for a secure effect.
Reset option (Unlock): You can easily restore the original state at any time.
Logging: Records all operations in the file win11_update_log.txt with date and time.

🛠️ Usage
----------------------------------------
You have two options for running the script:
Download: Download win11_stop_prevent_update.bat.
Manual creation: Copy the code below into a Notepad file and save it with a .bat extension.
Important: The file must always be run as Administrator!

📄 Script code (Batch)
----------------------------------------
batch
@echo off
setlocal enableddelayedexpansion
title Windows 11 Frissites Kezelo + Naplozas

:: Specifying log file location in the script folder
set LOGFILE=%~dp0win11_update_log.txt

:MENU
cls
echo ==============================================
echo Windows 11 Updates Disabled/Allowed
echo ==============================================
echo.
echo 1. DISABLE Windows 11 update (Win 10 update)
echo 2. ALLOW BACK to original base (Frissites permission)
echo 3. Out of order
echo.
set /p choice="Choose an option (1-3): "

if "%choice%"=="1" goto LOCK
if "%choice%"=="2" goto UNLOCK
if "%choice%"=="3" exit
goto MENU

:LOCK
cls
echo [%date% %time%] - DISABLE INITIVA >> "%LOGFILE%"
echo Loyalty of service providers for secure loyalty...
net stop wuauserv >nul 2>&1
net stop bits >zero 2>&1

:: Query current Windows 10 version (e.g. 22H2)
for /f "tokens=3" %%a in ('reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion" /v DisplayVersion 2^>nul') do set WIN_VER=%%a
if "%WIN_VER%"=="" ( 
for /f "tokens=3" %%a in ('reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion" /v ReleaseId 2^>nul') do set WIN_VER=%%a
)

echo Update Windows 10 version: %WIN_VER%
echo Applying Beallitas in the Registry...

:: Downloading registry keys and setting values for the disabled
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate" /v TargetReleaseVersion /t REG_DWORD /d 1 /f >nul
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate" /v ProductVersion /t REG_SZ /d "Windows 10" /f >nul
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate" /v TargetReleaseVersionInfo /t REG_SZ /d "%WIN_VER%" /f >nul

echo [%date% %time%] - SUCCESSFUL: Win 10 installed (%WIN_VER%) >> "%LOGFILE%"
echo.
echo SUCCESS: The system is installed with Windows 10 %WIN_VER% version.
goto REBOOT_PROMPT

: UNLOCK
cls
echo [%date% %time%] - RECALL INDICATED >> "%LOGFILE%"
echo Loyalty of services to return...
net stop wuauserv >nul 2>&1
net stop bits >zero 2>&1

echo Removing limitations...
:: Restricting Registry values
reg delete "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate" /v TargetReleaseVersion /f >nul 2>&1
reg delete "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate" /v ProductVersion /f >nul 2>&1
reg delete "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate" /v TargetReleaseVersionInfo /f >nul 2>&1

echo [%date% %time%] - SUCCESSFUL: Restricted >> "%LOGFILE%"
echo.
echo SUCCESSFUL: Limiters are pushed.
goto REBOOT_PROMPT

:REBOOT_PROMPT
echo.
echo ==============================================
echo ATTENTION: In order to surprise the fans
echo RESET REQUIRED!
echo ==============================================
echo.
set /p rb="Do you want to restart your computer now? (Y/N): "

if /i "%rb%"=="I" ( 
echo [%date% %time%] - System update... >> "%LOGFILE%" 
shutdown /r /t 5 /c "Restart due to termination of Windows Update settings." 
exit
)
if /i "%rb%"=="N" ( 
echo Restarting services... 
net start bits >zero 2>&1 
net start wuauserv >nul 2>&1 
goto MENU
)
exit

🔍 Advanced functions and fault tolerance
----------------------------------------
The used script contains more safety and convenience functions compared to the factory default solutions:
Intelligent version query: If the system does not find the modern DisplayVersion value, it automatically searches for the older ReleaseId key, so it is reliable even on older Windows 10 builds.
Run quietly: By using the >nul 2>&1 switches, the script hides unnecessary error messages, keeping the user interface clean and transparent.
Service coordination: Stops background processes for the duration of the change, so that Registry entries take effect immediately and without conflicts.
Event log: Every run and its outcome (success/error) is recorded in the win11_update_log.txt file.


📂 Technical background
----------------------------------------
The script in HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate sets the ProductVersion value to "Windows 10" so that Windows Update will not offer Windows 11.
Attention: It is recommended to create a system restore point before using the script. Running is at your own risk.

Blog post about it here:
----------------------------------------
https://lordathis.blogspot.com/p/win11-ne.html

----------------------------------------


Windows 11 Frissítés Kezelő (Update Blocker)
----------------------------------------

Ez a projekt egy automatizált megoldást kínál a Windows 11-re való kényszerített frissítés megakadályozására Windows 10-es rendszereken. Különösen hasznos olyan laptopok esetén, amelyek nem kompatibilisek, mégis kéretlenül frissítenek, ami után gyakran újra kell telepíteni a gépet.

🚀 Főbb jellemzők
----------------------------------------
Automatikus verziófelismerés: Lekérdezi az aktuális Windows 10 verziót (pl. 22H2), és ehhez rögzíti a rendszert.
Registry alapú tiltás: A legbiztosabb módszerrel módosítja a Rendszerleíró adatbázist.
Szolgáltatás-menedzsment: A biztos hatás érdekében ideiglenesen leállítja a Windows Update (wuauserv) és a BITS szolgáltatásokat.
Visszaállítási lehetőség (Unlock): Bármikor egyszerűen visszaállítható az eredeti állapot.
Naplózás: Minden műveletet a win11_update_log.txt fájlba rögzít dátummal és időponttal.

🛠️ Használat
----------------------------------------
A szkript futtatásához két lehetőséged van:
Letöltés: Töltsd le a win11_stop_prevent_update.bat fájlt.
Manuális létrehozás: Másold ki az alábbi kódot egy Notepad fájlba, és mentsd el .bat kiterjesztéssel.
Fontos: A fájlt minden esetben Rendszergazdaként kell futtatni!

📄 A szkript kódja (Batch)
----------------------------------------
batch
@echo off
setlocal enabledelayedexpansion
title Windows 11 Frissites Kezelo + Naplozas

:: Naplofajl helyenek meghatarozasa a szkript mappajaban
set LOGFILE=%~dp0win11_update_log.txt

:MENU
cls
echo ==============================================
echo    Windows 11 Frissites Tiltas/Engedelyezes
echo ==============================================
echo.
echo 1. Windows 11 frissites LETILTASA (Win 10 rogzitese)
echo 2. Eredeti allapot VISSZAALLITASA (Frissites engedelyezese)
echo 3. Kilepes
echo.
set /p choice="Valassz egy opciot (1-3): "

if "%choice%"=="1" goto LOCK
if "%choice%"=="2" goto UNLOCK
if "%choice%"=="3" exit
goto MENU

:LOCK
cls
echo [%date% %time%] - LETILTAS INDITVA >> "%LOGFILE%"
echo Szolgaltatasok leallitasa a biztos beallitashoz...
net stop wuauserv >nul 2>&1
net stop bits >nul 2>&1

:: Aktualis Windows 10 verzio lekerdezese (pl. 22H2)
for /f "tokens=3" %%a in ('reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion" /v DisplayVersion 2^>nul') do set WIN_VER=%%a
if "%WIN_VER%"=="" (
    for /f "tokens=3" %%a in ('reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion" /v ReleaseId 2^>nul') do set WIN_VER=%%a
)

echo Aktualitas Windows 10 verzio: %WIN_VER%
echo Beallitasok alkalmazasa a Registry-ben...

:: Registry kulcsok letrehozasa es ertekek beallitasa a tiltashoz
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate" /v TargetReleaseVersion /t REG_DWORD /d 1 /f >nul
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate" /v ProductVersion /t REG_SZ /d "Windows 10" /f >nul
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate" /v TargetReleaseVersionInfo /t REG_SZ /d "%WIN_VER%" /f >nul

echo [%date% %time%] - SIKERES: Win 10 rogzitve (%WIN_VER%) >> "%LOGFILE%"
echo.
echo SIKERES: A rendszer rogzitve a Windows 10 %WIN_VER% verziojanal.
goto REBOOT_PROMPT

:UNLOCK
cls
echo [%date% %time%] - VISSZAALLITAS INDITVA >> "%LOGFILE%"
echo Szolgaltatasok leallitasa a visszaallitashoz...
net stop wuauserv >nul 2>&1
net stop bits >nul 2>&1

echo Korlatozasok eltavolitasa...
:: Korlatozo Registry ertekek torlese
reg delete "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate" /v TargetReleaseVersion /f >nul 2>&1
reg delete "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate" /v ProductVersion /f >nul 2>&1
reg delete "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate" /v TargetReleaseVersionInfo /f >nul 2>&1

echo [%date% %time%] - SIKERES: Korlatozasok torolve >> "%LOGFILE%"
echo.
echo SIKERES: A korlatozasok torolve.
goto REBOOT_PROMPT

:REBOOT_PROMPT
echo.
echo ==============================================
echo FIGYELEM: A valtozasok ervenybe leptetesehez
echo UJRAINDITAS SZUKSEGES!
echo ==============================================
echo.
set /p rb="Szeretned most ujrainditani a gepet? (I/N): "

if /i "%rb%"=="I" (
    echo [%date% %time%] - Rendszer ujrainditasa... >> "%LOGFILE%"
    shutdown /r /t 5 /c "A Windows Update beallitasok veglegesitese miatti ujrainditas."
    exit
)
if /i "%rb%"=="N" (
    echo Szolgaltatasok ujrainditasa...
    net start bits >nul 2>&1
    net start wuauserv >nul 2>&1
    goto MENU
)
exit

🔍 Fejlett funkciók és hibatűrés
----------------------------------------
Az alkalmazott szkript több biztonsági és kényelmi funkciót tartalmaz a gyári alapmegoldásokhoz képest:
Intelligens verziólekérdezés: Ha a rendszer nem találja a modern DisplayVersion értéket, automatikusan a régebbi ReleaseId kulcsot keresi meg, így régebbi Windows 10 build-eken is üzembiztos.
Csendes futtatás: A >nul 2>&1 kapcsolók használatával a szkript elrejti a felesleges hibaüzeneteket, így a felhasználói felület tiszta és átlátható marad.
Szolgáltatás-összehangolás: A módosítás idejére leállítja a háttérfolyamatokat, hogy a Registry-beírások azonnal és ütközésmentesen érvényesüljenek.
Eseménynapló: Minden futtatásról és annak kimeneteléről (siker/hiba) bejegyzés készül a win11_update_log.txt fájlba.


📂 Technikai háttér
----------------------------------------
A szkript a HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate útvonalon rögzíti a ProductVersion értékét "Windows 10"-re, így a Windows Update nem fogja felajánlani a Windows 11-et.
Figyelem: A szkript használata előtt javasolt rendszer-visszaállítási pont létrehozása. A futtatás saját felelősségre történik.


Blog bejegyzés erről itt:
----------------------------------------
https://lordathis.blogspot.com/p/win11-ne.html


