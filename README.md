# kWSL.cmd

- Simplicity - One command to set up KDE in WSL with all the quirks resolved for you
- Runs on Windows Server 2019 or Windows 10 Version 1803 (or newer)
- KDE 5.17 on Devuan Linux (Tracking with what will become Debian Bullseye, without systemd) 
- xRDP Display Server, no additional X Server downloads required
- RDP Audio playback enabled (YouTube playback in browser works)

<img width="641" alt="kWSL1" src="https://user-images.githubusercontent.com/33142753/82766604-ea801680-9df6-11ea-9045-6ab9540a5424.png">

kWSL is accessible from anywhere on your network, you connect via Microsoft's Remote Desktop Client (mstsc.exe) 

**INSTRUCTIONS:  From an elevated CMD.EXE prompt change to your desired install directory and type/paste the following command:**

```
PowerShell -executionpolicy bypass -command "wget https://github.com/DesktopECHO/kWSL/raw/master/kWSL.cmd -UseBasicParsing -OutFile kWSL.cmd ; .\kWSL.cmd"
```

You will be asked a few questions:

```
kWSL for Ubuntu 20.04
Enter a unique name for the distro or hit Enter to use default [kWSL]: 
Enter port number for xRDP traffic or hit Enter to use default [3399]: 
Enter port number for SSHd traffic or hit Enter to use default [3322]: 
kWSL (kWSL) To be installed in: C:\Users\danm\kWSL
```

Near the end of the script you will be prompted to create a non-root user.  This user will be automatically added to sudo'ers.

```
Enter name of kWSL user: danm
Enter password: ********
SUCCESS: The scheduled task "kWSL-Init" has successfully been created.

TaskPath                                       TaskName                          State
--------                                       --------                          -----
\                                              kWSL-Init                         Ready

  Start: Sun 05/24/2020 @ 20:08:00.84
    End: Sun 05/24/2020 @ 20:16:48.87

 Installation Complete.  xRDP server listening on port 3399 and SSH on port 3322
 Links for GUI and Console sessions have been placed on your desktop.
 Auto-launching RDP Desktop Session in 5 seconds...

C:\Users\danm>
```

Upon completion you'll be logged into an attractive and fully functional XFCE4 desktop.  A scheduled task is created that runs at login to start kWSL. 

   **If you prefer to start kWSL at boot (like a service) do the following:**

   - Right-click the task in Task Scheduler, click properties
   - Click the checkboxes for **Run whether user is logged on or not** and **Hidden** then click **OK**
   - Enter your Windows credentials when prompted

   Reboot your PC.  kWSL will automatically start at boot, no need to login to Windows.

**Quirks Addressed and other interesting tidbits:**
- WSL1 Has issues with the latest libc6 library.  The package is being held until fixes from MS are released over Windows Update.  Unmark and update libc6 after MS releases the update.
- WSL1 Doesn't work with PolicyKit.  Pulled-in GKSU and dependencies to allow runing GUI apps with elevated rights.  
- Rolled back and held xRDP until the version shipped in Ubuntu is better-behaved (xrdp-chansrv high CPU %)
- Current version of Chrome or Firefox does not work in WSL1 so Mozilla Seamonkey was included as a stable and maintaned browser
- Installed image consumes less than 2GB of disk
- Symlinked Windows fonts in Linux which make for a very nice looking XFCE4 session using Segoe UI and Consolas
- Password-saving magic for RDP connections performed safely using Windows credential store and Powershell ConvertTo-SecureString 
