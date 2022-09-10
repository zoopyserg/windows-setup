# Windows Setup
What I usually do (or install) to set up Windows

## Reset Trial Period for MS Products:

````
SLMGR -DLV
SLMGR -REARM
````

## How to fix Windows Update problems:
This re-downloads definitely correct updates:

In Admin Powershell
````
Dism /Online /Cleanup-Image /CheckHealth 
Dism /Online /Cleanup-Image /ScanHealth
DISM /ONLINE /CLEANUP-IMAGE /RESTOREHEALTH
````

* Or this (if Windows Update itself is damaged, say you need to source another windows from CD) https://support.microsoft.com/ru-ru/topic/%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5-%D1%81%D1%80%D0%B5%D0%B4%D1%81%D1%82%D0%B2%D0%B0-%D0%BF%D1%80%D0%BE%D0%B2%D0%B5%D1%80%D0%BA%D0%B8-%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%BD%D1%8B%D1%85-%D1%84%D0%B0%D0%B9%D0%BB%D0%BE%D0%B2-%D0%B4%D0%BB%D1%8F-%D0%B2%D0%BE%D1%81%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BB%D0%B5%D0%BD%D0%B8%D1%8F-%D0%BE%D1%82%D1%81%D1%83%D1%82%D1%81%D1%82%D0%B2%D1%83%D1%8E%D1%89%D0%B8%D1%85-%D0%B8%D0%BB%D0%B8-%D0%BF%D0%BE%D0%B2%D1%80%D0%B5%D0%B6%D0%B4%D0%B5%D0%BD%D0%BD%D1%8B%D1%85-%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%BD%D1%8B%D1%85-%D1%84%D0%B0%D0%B9%D0%BB%D0%BE%D0%B2-79aa86cb-ca52-166a-92a3-966e85d4094e
DISM.exe /Online /Cleanup-Image /RestoreHealth /Source:C:\RepairSource\Windows /LimitAccess 

And then run this in Admin CMD to replace corrupt files with correct downloaded ones:
````
sfc /scannow
````

After that just keep installing your updates in Windows Update Center.

Once done, install programs from Program Installers folder.

## Setting up Windows 10 Local Network Sharing (my checklist).

Obviously, it's not optimized for CIS Security or anything, just a memo where all the buttons are.

All the PC's have to be connected via LAN or Wi-Fi to the router.

### For every computer do this:
- Open services via start search
- Activate these services:
- - Function Discovery Resource Publication
- - Function Discovery Provider Host
- Go to Network & Sharing Center (via network menu) -> Advanced 
- Activate Everything (in Network Sharing Center) for all kinds of networks.
- Go to Settings -> Network 
- Under the network you're setting up, click Properties, and make sure it's set to "private" (not "hidden" from other members)

### For all the folders you want to share do this:
- Properties -> Sharing
- Select Everyone
- Some people say it used to be that you have to have Windows user (on a server PC) with the same username/password as a client, but in my experience it worked fine without it. Just asked for a password for the first time.

### For every Printer you want to share do this:
On server (device physically connected to the printer):
- Go to Printers & Scanners
- Click on the name of the printer
- Click Manage
- Click Printer Properties
- Go to Sharing tab
- Check the "Shared Access" checkmark.
- Apply
On client:
- Try to print something
- Click "Find a printer"
- Select the Server from the dropdown
- Select its Printer
- That's it. Print.

### For every member of the network add the Network Drive
(so that you wouldn't have to access it through "network" menu)
- Open Explorer
- On the top (in the Computer tab) there will be a button "Map a Network Drive".
- There you can assign a letter and pick any folder accessible to you on any computer of the network.

For any more details on Network Setup read this official Microsoft guide:
https://docs.microsoft.com/en-us/troubleshoot/windows-client/networking/set-up-your-small-business-network

## Setting up latest MS Office (unofficial)
Turn off Windows Defender.

## Setting up Bartender (industrial program for barcode printing)
- Install latest Windows SQL Server from https://www.microsoft.com/ru-ru/sql-server/sql-server-downloads (basic setup will do).
- Launch Bartender Setup
- When it's Launched, click "Show Advanced Options"
- On Advanced Options tab disable (uncheck) "installing SQL Server automatically" (because it's pulling a buggy installer that is incompatible with Windows 10).
- Proceed installation normally
- When first launched, Bartender will say "Database Connection is not set up" -> hit Connect Manually and connect it to the database you just created (one of the two that will be there - either one or the other, not sure which one yet).

## USER Management:
- See all users: `net user`
- Manage users: `netplwiz`

# 1C Setup:
1C + MSSQL server:
Good tutorial: https://youtu.be/WWDVLdXYRjY

on MSSQL server: 
open incomming 1433

on 1C server:
open 1541, 1540 TCP (incoming? outgoing?)
open 1560-1591 TCP (incoming? outgoing?)

1433, 1540, 1541, 1560-1591

💢 1. Удалите ранее установленную версию сервера 8.3 (если имеется) через панель управления в «Удаление программ».

💢 2. Проверьте свойства пользователя «USR1CV8».

Учётная запись не заблокирована.
Включена в локальные группы «Пользователи» и «Пользователи журналов производительности».
При необходимости — поменяйте пароль на новый.
Пуск – Выполнить – secpol.msc
Пользователю «USR1CV8» делегировано право «Вход в качестве службы» в разделе «Параметры безопасности – Локальные политики – Назначение прав пользователя».
В управлении службой убедитесь, что запуск агента настроен от имени учётной записи «USR1CV8». Запустите службу вручную с учётом изменения пароля.
💢 3. Если используете параллельно несколько агентов различных версий, то сначала удалите регистрацию агента 8.3, вместе с содержимым каталога сервера кластера srvinfo.

Далее зарегистрируйте службу с новыми значениями портов, без пересечений с используемыми в системе.

ragent.exe -rmsrvc
ragent.exe -instsrvc -port 1540 -regport 1541 -range 1560:1591 -d "C:\Program Files\1cv8\srvinfo " -usr .\usr1cv8 -pwd <пароль_пользователя_ usr1cv8>
✅ При правильных настройках служба агента сервера 1С запускается корректно и готова принимать входящие подключения.
_____________________________________

1C_SQL_STARTER
112223

Set Administrator access to MSSQL: 
https://itproguru.com/expert/2014/09/how-to-fix-login-failed-for-user-microsoft-sql-server-error-18456-step-by-step-add-sql-administrator-to-sql-management-studio/

Debug MSSQL Log and Event Log often, they contain exact errors ("login action missing" etc).
