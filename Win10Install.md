# Installing Windows 10 via Bootcamp on an old macbook (fresh install in 2023)  
For old macs, like my MBP5,4, only Windows 7 is officially supported. But Windows 10 install is possible, I am writing from that.  

I first tried to upgrade from Windows 7 32bit, wasn't successful, it will block saying 'Invalid path in system registry'. This is a vague error, after checking the log, I find it just stopped because activation server didn't respond, which is expected, as the official migration program stopped years ago and unofficial migration, finally stopped Sep 2023. So I went for a fresh install after (and fortunately I did, as Bootcamp 6 longer supports 32bit and Bootcamp 4 will run into problems).

## Creation of a Boot Camp Assistant that allows Win10 installation
You first need a modified copy of Boot Camp Assistant. Follow these steps:  
1. make a copy of Boot Camp Assistant, call it Boot Camp Win10 Assistant  
2. Open terminal, in terminal use below:  
`sudo nano /Applications/Utilities/Boot\ Camp\ Win10\ Assistant.app/Contents/Info.plist`
3. Scroll down and delete <key>Win7OnlyModels</key><array>...</array>
4. Press Ctrl+X, when asked save Y/N, press Y and then press enter to save
You can now run Boot Camp Win10 Assistant to install windows 10.

## Installation via DVD is recommended, EFI boot is too much for a 2009 macbookpro
Get a DVD DL and burn latest Win10 ISO to it and load it in the drive. (I would not recommend USB installation here)  

Check Win7Install.md if DVD reader needs a fix or external superdrive doesn't load.

Open your modified "Boot Camp Win10 Assistant" and follow the process, DO download your mac's own Bootcamp drivers (in my case, BootCamp4) to your USB, but do not install them if they are build 4.0 or 5.0. After restart, the windows 10 logo will briefly appear and then disappear and you only have a cursor blinking for like 15min...this is normal, the installer will load up.  

## Post installation driver install
After installation, we need to install BootCamp 6.0 (earlier versions of bootcamp will cause WUF violation Blue Screen of Death):
1. get brigadier from internet. get BootCamp 6.0 using `brigadier.exe --model=MacBookPro11,3`
2. get 7zip from internet
3. get your latest display drivers (for example in my case, I got it from nVidia website) and install it, do not reboot yet
4. Go to `{BootCamp6 Folder}\Drivers\Apple` and right click BootCamp.msi (you can select and use Shift+F10 for right click), select 'Run as Administrator', follow the steps to finish installation and reboot
5. After reboot, go to device manager, and install drivers for hardware that is having a warning sign
   - (My case) Bluetooth, navigate to `{BootCamp4 Folder}\Drivers\Apple\x64` and unzip AppleBluetoothBroadcomInstaller64 to a folder, then in device manager, right click bluetooth device in question, select update driver and direct to the unzipped folder and let system to search driver
   - (My case) a device called 'CoProcessor', this is related to the nvidia chipset, right click and select update driver, and direct to `{BootCamp4 Folder}\Drivers\NVidia\NvidiaChipset64` and let installer to search for proper drivers
6. Reboot again and everything should function

## Sort out display brightness issue
I have a nVidia 9400M graphic card, and after newest driver installation, Bootcamp can no longer adjust screen brightness. Do the following:
1. Open device manager, locate Display Adapters, within that, right click on your graphic card and select properties, click on 'Driver Details', we need information to locate the registry entry
2. Press Win+R, enter regedit and run
3. Go to `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Video\{id}\0000`, there might be multiple {id} available, find the one with information same as the one shown in device manager, that is the registry for the driver needed
4. Within that 0000, add two DWORD values:
   - EnableBrightnessControl=0x00000001
   - RMBrightnessControlFlags=0x00000320
5. Reboot and brightness control should function as normal

## Run windows update to bring everything up-to-date
All Done.

