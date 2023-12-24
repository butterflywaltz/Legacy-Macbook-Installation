# Installing Windows 7 via Bootcamp on an old macbook (fresh install in 2023)

## Sorting out DVD reader
Bootcamp from MBP5,4 to install windows 7 require a install DVD.

If you DVD looks like dead (i.e. cannot read any content from any DVD), try below steps first:
1. Reset SMC, for old macbook, means hold ctrl+option+shift, while holding the 3 keys, press and hold power and hold 4 keys for 10 seconds and release, google SMC reset for mac. For me nothing changed resetting SMC.  
2. Reset PRAM/NVRAM, for old macbook, means press and hold Option+Command+P+R for about 15sec. For me, it does change the sound at start up for built-in DVD drive, sounded more normal. But still cannot read any content.  
3. (Last Resort) I got a wet baby wipe (99.9% water one) and cut it in half, wrap around a credit card and insert into the DVD drive (about 3cm to 4cm), move gently left to right and pull out. WAIT for drying. This has worked for me, I suspect it's dirt from long-time no usage blocked the laser transmitter.  

If still fail to get built-in DVD drive working, get an external super drive, but models with built-in superdrive will probably say the external one is not supported. Do below:  
1. In terminal, use below to get the current boot args  
`nvram boot-args`
2. Append mbasd=1 to boot args and set nvram, for example if your previous boot args are empty or you just resetted your nvram in steps before, you just simply do  
`sudo nvram boot-args="-v mbasd=1"`
You are then set to use external superdrive to install Windows via Bootcamp.  

## Sorting out internet, activation and updates
After installing Windows 7, you will find yourself with no internet. In fact, your wifi or LAN might work properly, but you just cannot connect to any website, except Google. In that case, via www.google.com, search for Chrome and install it. You can then access web properly using Chrome. But note internet explorer won't work still, including Windows updates and even Windows activation.  

Using chrome, search for "Easy fix 51044", the root cause is because TLS 1.0 got retired and all websites (except a few like Google) rejects your request to connect with them. Use chrome to download that Easy fix 51044. A direct link provided below:  

> https://download.microsoft.com/download/0/6/5/0658B1A7-6D2E-474F-BC2C-D69E5B9E9A68/MicrosoftEasyFix51044.msi

You should then be able to access internet (and therefore able to activate windows the normal way).  

A few updates for reference:  
1. KB976932 - Windows 7 SP1
2. KB3020369 - Windows update client upgrade, requires SP1, Windows update won't work properly without it
3. KB3125574 - Windows 7 roll-up update (unofficially called SP2)

For the rest of update, run Windows update.  
