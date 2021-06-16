# Windows Setup
What I usually do (or install) to set up Windows

## How to fix Windows Update problems:
This re-downloads definitely correct updates:

In Admin Powershell
````
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

All the PC's have to be connected via LAN to the router (not sure how to do it via Wi-Fi yet).

For every computer do this:
- Open services via start search
- Activate all services that start with an "F" ("Finding this computer in the network etc").
- Go to Network & Sharing Center (via network menu) -> Advanced 
- Activate Everything (in Network Sharing Center) for all kinds of networks.

For all the folders you want to share do this:
- Properties -> Sharing
- Select Everyone
- Some people say it used to be that you have to have Windows user (on a server PC) with the same username/password as a client, but in my experience it worked fine without it. Just asked for a password for the first time.
