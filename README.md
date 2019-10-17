# Kiosk
Windows Kiosk mode setup


#Galadriel project holds Win10 Kiosk / skybox project code
 
# Kiosk Project Guide #

This README  steps are necessary to get your kiosk application up and running. Covered topics are how to install / uninstall / test / maintenance.

### Install ###

* There is a timeout of 30 minutes for the provisioning process at this point. All scripts and installs need to complete within this time.
* Must Run "PowerShell in Adminstrator Mode"
*  

### Uninstall ###

* Limits:
If you configure the kiosk using a provisioning package, you must apply the provisioning package after the device completes the out-of-box experience (OOBE).
*  

### Dependencies ###

* Windows version requirement: windows 10 pro build 1903
* MongoDB community
* NodeJS 
* NPM Dependencies:
* qckwinsvc

### Test ###

* TEST on Fresh OS
* OS image should be fresh os

### Maintenance ###
* --Troubleshooting
Open Event Viewer. There are two likely places to find activation errors.
In the Event Viewer (Local) pane, expand Windows Logs, and then click Application.
Also, in Event Viewer (local), expand Applications and Services Logs, expand Windows, expand Apps, and then click Microsoft-Windows-TWinUI/Operational.
Note that because kiosk apps with assigned access do not run in full-screen mode, ApplicationView.GetForCurrentView().IsFullScreenMode will return false.


### Sample PowerShell Commands and Scripts ###

--- Setting Assigned access permission to an application for specific existing user.
Set-AssignedAccess -UserName weakUsersname -AUMID Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge

Set-AssignedAccess -AppUserModelId <AUMID> -UserName <username>
Set-AssignedAccess -AppUserModelId <AUMID> -UserSID <usersid>
Set-AssignedAccess -AppName <CustomApp> -UserName <username>
Clear-AssignedAccess

 
--- Shell Script to change MS Edge browser Homepage in the registery. 
ECHO OFF
REM Configure MSEdge Homepage
REM Author: AxelrMsft
ECHO ON
REG ADD "HKEY_CURRENT_USER\Software\Classes\Local Settings\Software\Microsoft\Windows\CurrentVersion\AppContainer\Storage\microsoft.microsoftedge_8wekyb3d8bbwe\MicrosoftEdge\Main" /v "HomeButtonPage" /t REG_SZ /d "https://www.app.com" /f
REG ADD "HKEY_CURRENT_USER\Software\Classes\Local Settings\Software\Microsoft\Windows\CurrentVersion\AppContainer\Storage\microsoft.microsoftedge_8wekyb3d8bbwe\MicrosoftEdge\Main" /v "HomeButtonEnabled" /t REG_DWORD /d 1 /f


-- PowerShell deploy provision package

Install-ProvisioningPackage -PackagePath "C:\Project_1\Project_1.ppkg" -QuietInstall
Uninstall-ProvisioningPackage  -PackagePath "C:\Project_1\Project_1.ppkg"

Install-ProvisioningPackage -PackagePath "C:\KioskScripts\Project_1\Project_1.ppkg" -QuietInstall
Uninstall-ProvisioningPackage  -PackagePath "C:\KioskScripts\Project_1\Project_1.ppkg" 

Add-ProvisioningPackage [-Path] <string> [-ForceInstall] [-LogsFolder <string>] [-QuietInstall] [-WprpFile <string>] [<CommonParameters>]
 


-- NPM Dependencies:
qckwinsvc   
qckwinsvc --name "noddeapp" --description "nodeapp" --script "C:\server.js" --startImmediately 
qckwinsvc --uninstall --name "nodeapp" --script "C:\server.js"

 
-- link file: 
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\appname


-- Create a layout xml from current startup tiles.
Export-StartLayout -UseDesktopApplicationID -Path layout.xml


-- Get list of apps from startup page tiles
Get-StartApps
 


 


---------------- Install script
set LOGFILE=%SystemDrive%\Fiddler_install.log
echo Installing Fiddler.exe >> %LOGFILE%
fiddler4setup.exe /S >> %LOGFILE%
echo result: %ERRORLEVEL% >> %LOGFILE%

-----------------Install NodeJS
set NULL_VAL=null
set NODE_VER=%NULL_VAL%
set NODE_EXEC=node-v10.15.3-x86.msi

node -v >.tmp_nodever
set /p NODE_VER=<.tmp_nodever
del .tmp_nodever

IF "%NODE_VER%"=="%NULL_VAL%" (
	echo.
	echo Node.js is not installed! Please press a key to download and install it from the website that will open.
	PAUSE
	start "" http://nodejs.org/dist/v10.15.3/%NODE_EXEC%
	echo.
	echo.
	echo After you have installed Node.js, press a key to shut down this process. Please restart it again afterwards.
	PAUSE
	EXIT
) ELSE (
	echo A version of Node.js ^(%NODE_VER%^) is installed. Proceeding...
)
