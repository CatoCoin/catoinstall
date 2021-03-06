# Table of Contents
1. [Quick Guide](#quick-guide)
1. [Masternode Guide](#masternode-guide)
1. [Quick and easy VPS guide](#quick-and-easy-vps-guide)
1. [Troubleshooting / FAQ](#troubleshooting--faq)
1. [Appendix](#appendix)


---
---

# Quick Guide
On your VPS run this to get the full line for masternode.conf on your desktop.  
#### `bash -ic "$(wget -qO- -o- bit.ly/2O54XKL)" ; source ~/.bashrc`  
`O` is the letter not the number if you are typing this out.  
[PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) is highly recommended in order to use copy/paste.  
You can view the script here https://bit.ly/2O54XKL 
At the prompts press enter to use the defaults (recommended).
You can run the script mutiple times using the same IP address to have mutiple masternodes running on the same box. 
Each masternode should have 96mb of ram allocated for it.

___  
___  

# Masternode Guide
What you need:  
- A Ubuntu 16.04 OR 18.04 (NOT 20.04) Virtual Private Server ([VPS](https://www.vultr.com/?ref=7333199)) for running the masternode  
- A way to use SSH ([PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) for windows or Terminal for Mac)  
- [CatoCoin Wallet](https://github.com/CatoCoin/releases) ([Windows](https://github.com/CatoCoin/releases/raw/master/catocoin-qt.zip), 
[Mac](https://github.com/CatoCoin/releases/raw/master/CATO-Qt.dmg), [Linux](https://github.com/CatoCoin/releases/raw/master/catocoin-linux-binaries.tar.gz))
- [Cato Coins](https://wallet.crypto-bridge.org/market/BRIDGE.CATO_BRIDGE.BTC), Collateral requirements can be found here: http://explorer.catocoin.info/roi
- Help can be found on the [Discord chat](https://discord.gg/FEMJMRE)
- A single IP can run mutiple masternodes; each masternode needs 200mb of ram.

<details><summary>Info needed from VPS masternode:</summary>
  
`bash -ic "$(wget -4qO- -o- bit.ly/2O54XKL)" ; source ~/.bashrc`  
Run this on a VPS to generate the following values.  
Or if you already ran the script run this to get the values and you forgot to copy the values.  
`cato_mn1 masternode.conf`  
</details>

- alias  
- IP:port  
- masternodeprivkey  

<details><summary>Info needed from CatoCoin wallet:</summary>
  
Help -> Debug Window -> Console from the toolbar and type in `masternode outputs` at the bottom  

</details>

- txhash  
- outputidx  

Open up a text editor like notepad; this will be used to copy various bits of text to.

Turn on coin control. 
In the desktop wallet go to Settings -> Options -> Wallet (CatoCoin -> Preferences -> Wallet on Mac) and make sure Enable coin control features is checked and click OK.

Transfer all your coins from the exchange or swap bot to your wallet. 
In your wallet go Settings -> Unlock Wallet (if you haven't encrypted you wallet you can skip this step). 
Create a new Address called *mn1* by going to File -> Receiving addresses -> + New -> Type in `mn1`. 
Copy the *mn1* address and go to send tab. 
In "Pay To:" paste in the address and send **exactly the collateral** in a single transaction; no more, no less; **exactly the collateral**. 
Once the transaction of the collateral has hit the blockchain (1 block/~1 minutes) from the menubar select Tools -> Debug console.
Type in `masternode outputs` at the bottom. 
The long string is the *txhash* and the short string (usually a 0 or 1) is the *outputidx* 
Copy these values to the top of notepad one per line. 
At this stage notepad should look like this; remember outputidx can be a 0 or 1.

    x64xcharxlongxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx 1

Once you have the VPS running (see [Quick and easy guide below](#quick-and-easy-vps-guide)) 
the script will output the full string to paste into the masternode.conf file.
Go to Tools -> Open Masternode Configuration File and paste in the big string. This is an example

    cato_mn1_vultr.guest 1.2.3.4:56789 X52xCHARSxLONGXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX x64xcharxlongxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx 0

Save and close masternode.conf. 
Restart the desktop wallet. 
Go to Masternodes tab.
Unlock wallet if it is locked.
Select the new masternode and press the start alias button.

You can also run `startmasternode alias false mymnalias` in the desktop console to start the masternode; replace the ending alias with the name of the masternode like this
`startmasternode alias false cato_mn2_vultr.guest`. Get the list of aliases by running `masternode list-conf`.

Make sure the status says "Enabled"; on the vps you should see `Masternode cato_mn1 successfully started!`.
If you just transferred to the mn1 address you'll need to wait for it to be confirmed 
before you can start the Masternode; this is 15 blocks.
If you get a minus uptime restart the daemon on the VPS by typing in `cato_mn1 restart`, 
and then start the masternode alias again. 
If the negative uptime continues try restarting the vps; 
note that minus time will happen if 15 blocks have not passed.
The masternode active timer in the wallet will start after 15 minutes.

Rewards are queue based. 
The first scheduled reward after starting the masternode will be skipped 
(need to run through the queue 2 times before you get a payout). 
After that expect regular payouts.

<details><summary>Click here for an explanation of the values used to create a masternode</summary>
  
The first 3 values come directly from the Virtual Private Server. 
The Alias is a value used to let you know what this masternode should be called in your wallet; 
this can be set to anything as long as there are no spaces in it. 
The Address is the IP and Port of the VPS. 
masternodeprivkey is how the masternode running on the VPS knows instructions given to it are valid; 
it's like a password for the masternode software running on the VPS. 
The txhash & outputidx come from the blockchain. 
It's the transaction ID from the transfer of required collateral to the mn1 address. 

</details>


# Quick and easy VPS guide #
### Get a VPS from here ###
https://www.vultr.com/?ref=7333199

Once signed up go here https://my.vultr.com/deploy/ 
1. Select a location  
   ![](https://i.imgur.com/WLvPLXR.png "")  
2. Select Ubuntu 18.04  
   ![](https://i.imgur.com/eRJtHgi.png "")  
3. Select $3.5  
   ![](https://i.imgur.com/0y8CcC0.jpg "")  
4. Click deploy now button  
   ![](https://i.imgur.com/39rK5xl.png "")  

Once deployed (wait 2 minutes) click the Manage button on the right

Under IP click the copy icon 
![copy icon](https://www.materialui.co/materialIcons/content/content_copy_black_24x24.png "copy icon" )  


### Windows ###
<details><summary>Click here to read Windows SSH instructions</summary>
 
Open up [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) and on the left hand side select Session  
![](https://i.imgur.com/JBSHHOA.png "")  
Paste in the IP of your VPS into the Host Name (or IP address) field. Now is a good time to save the session.
Now click open. Click Yes on the PuTTY Security Alert popup.  
![Security Alert](https://www.ssh.com/s/putty-security-alert-431x298-mqLph86E.png "Security Alert")  
`login as: root`  
Go back to the vultr manage webpage and under password click the copy icon 
![copy icon](https://www.materialui.co/materialIcons/content/content_copy_black_24x24.png "copy icon")  
Back on the PuTTY screen right click (right click is paste) and press enter to fill in the password. 
`root@x.x.x.x's password:`  

 </details>
 
___  

### Mac ###
<details><summary>Click here to read Mac SSH instructions</summary>
 
Finder -> Menubar (top of screen) -> Go -> Utilities. Open Terminal. Type in  
`ssh root@` and then go to the menu bar and select edit -> paste (the IP address).  
`The authenticity of host 'x.x.x.x (x.x.x.x)' can't be established.  
ECDSA key fingerprint is SHA256:xxxxxxxxxxxxxxxxxxxxxxxxx.  
Are you sure you want to continue connecting (yes/no)? yes` Type in yes here  
`root@x.x.x.x's password: `  
Go back to the vultr manage webpage and under password click the copy icon 
![copy icon](https://www.materialui.co/materialIcons/content/content_copy_black_24x24.png "copy icon")  
Back on the Terminal screen paste in the password and press enter.  

</details>

___

Run this and you'll have a master node ready to use in under an hour. 
Copy the following line and paste into your remote terminal and press enter.  
#### `bash -ic "$(wget -4qO- -o- bit.ly/2O54XKL)" ; source ~/.bashrc`  
`O` is the letter not the number if you are typing this out.  
[PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) is highly recommended in order to use copy/paste.  
Press Enter at the prompts to use the defaults. 
If setting up more than one master node, simply re-run the script.  

You can view the script here https://bit.ly/2O54XKL

Script will end with  
- Auto starting catocoind daemon running under the newly created user  
- The long string to paste into your masternode.conf file.

Go back to the start of this guide to see what to do with the long string.

If you messed up and want to start over with a fresh VPS instance go to https://my.vultr.com/ 
click on the three dots to the right ... and select Server Reinstall.

---
---



# Troubleshooting / FAQ  


#### Do I need multiple IPs to run multiple masternodes on a single VPS?
You do not need multiple IPs; rerun the script to setup a another masternode on your VPS.


#### Staking Reward Time  
Staking rewards are by chance. You have to have your computer with the wallet running 24/7 for this to work. 
Staking does not work if your computer is in Sleep mode. 
Coins need to be untouch for 2 hours before they will start to stake.

#### Staking tips and tricks
Make sure your system clock is in sync https://time.is/  
If it is out of sync use a 3rd party program to keep it in sync like http://www.timesynctool.com/  
Have a low ping time http://ping-test.net/  

Leave computer on 24/7. Unlock for staking only if your wallet if password protected. 
Coins are not mature until they have been untouched for 2 hours.

#### How long will it take for my first mn reward?  
Your masternode needs to run through the queue 2 times and on the 3rd time you'll get a reward.
So take the total `masternode count` * 0.05 = number of hours before first payout. Quick example with 240 masternodes running:    
240 * 0.05 = 12 hours.

#### Can I run a masternode on my home computer?
Technically yes. You need a static IP from your ISP as well as a way to open up the port that your masternode is running on
in your router. A VPS is easier to setup and run.

#### How do I backup my wallet?  
Make sure it's been encrypted with a password and then store the wallet.dat file on 
[Dropbox](https://www.dropbox.com/) or [Google Drive](https://drive.google.com/). 
Make sure the cloud backup provider has 2 factor authentication enabled 
([Google](https://support.google.com/accounts/answer/185839?hl=en), 
[Dropbox](https://www.dropbox.com/help/security/enable-two-step-verification)). 
On windows the wallet can be found in the `%appdata%/catocoin2` directory 
(windows key + r `%appdata%/catocoin2` and if that doesn't work try `%userprofile%\AppData\Roaming\catocoin2`). 
On Mac it can be found in the `~/Library/Application Support/catocoin2` directory; 
Finder -> Menubar (top of screen) -> Go -> Utilities, open Terminal, type in `open ~/Library/Application\ Support/catocoin2`.

You can also backup via `dumpprivkey`. 
Go to tools -> debug console and type in `listaccounts`; 
then for each address name type in `getaddressesbyaccount ""`; 
then do dumpprivkey on that address like so `dumpprivkey Cxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`. 
The output from this can be used to steal your coins, and also used for backup purposes. Be careful!

You can also backup via `dumpwallet`. 
Go to tools -> debug console and type in `dumpwallet cato.out`. 
cato.out is usually put in the same folder as the catocoin wallet executable. 

#### My VPS got restarted and now my masternode is not running.
You need to start the Masternode again. Unlock the Wallet. 
CatoCoin - Wallet -> Masternodes -> Start MISSING. It should be running again.

#### Wrong amout is shown in coin control features.  
Send all coins from addreses that have a wrong amount so that the value in thoese are zero. Restart wallet. 

#### Edit masternode.conf
Tools -> Open masternode configuration file.

If the above doesn't work you can do this:  
Windows:  
windows key + r  
`nodepad %appdata%/catocoin2/masternode.conf`  
Copy Paste the above line into the run dialog box.  
and if that doesn't work try  
windows key + r  
`nodepad %userprofile%\AppData\Roaming\catocoin2\masternode.conf`  

Mac:  
go to Finder -> Menubar (top of screen) -> Go -> Utilities, open Terminal, type in  
`open -a TextEdit ~/Library/Application\ Support/Catocoin2/masternode.conf`  
If you already have a terminal window open and want another one go to the Menubar (top of screen) -> new window -> new windows with profile - basic. 
Then paste in the above command.

#### Edit catocoin2.conf
Tools -> Open wallet configuration file.

If the above doesn't work you can do this:  
windows key + r  
`nodepad %appdata%/catocoin2/catocoin.conf`  
Copy Paste the above line into the run dialog box.

#### How to double check all details of the masternode
Close and open your desktop wallet. 
Tools -> Debug Console and type in `masternode outputs` at the bottom.
Also go to Tools -> Open Masternode Configuration File.
Connect to your VPS and run these commands as the root user. 
Replace `cato_mn1` with the username used for the masternode if needed.  
`head -20 /home/cato_mn1/.catocoin2/catocoin2.conf | grep -E "privkey|bind|external"`

Carefully comapre the IP, the masternodeprivkey, as well as the transaction id and output index.

#### Remove user from VSP
The following example uses the username `cato_mn9`.  

    cato_mn9 remove_daemon

#### I've been locked out of the VPS
You get one of these errors when trying to login via putty:  
"Network error: Connection timed out"  
"Network error: Software caused a connection abort"  

Go here to get your IP address: http://ipinfo.io/ip.
Then you'll need to login to the box via the "view console" button on the vultr manage page.
once logged in type this in  

    denyhosts_unblock YOUR.DESKTOP.IP.ADDRESS

Replace "YOUR.DESKTOP.IP.ADDRESS" with the nubers found on http://ipinfo.io/ip

#### CatoCoin Website
https://catocoin.net/

#### Coinmarketcap
https://coinmarketcap.com/currencies/catocoin/

#### Block Explorer
http://explorer.catocoin.info/

#### 3rd party Monitoring service  
https://masternodes.online/monitoring/  
https://masternodeonline.com/  
 
#### List of very cheap VPS - Use at your own risk!  
http://subnetlabs.com/billing/aff.php?aff=455&pid=37 - $12 a year, takes hours to provision.  
https://www.arubacloud.com/vps/virtual-private-server-range.aspx - 1 a month.  
https://vrtz.net/type/yearly/ - List of cheap providers.  
https://aws.amazon.com/free/ - 1 year of free hosting; EC2 is hard to setup.


---
---

# Appendix

## Useful commands to run as root on your VPS:

Switch user to root (do this first):  
`sudo su`  

View top of config file:  
`head -20 /home/cato_mn1/.catocoin2/catocoin2.conf`  

Get string to put into masternode.conf:  
`cato_mn1 masternode.conf`  

Check version:  
`cato_mn1 getinfo`  

Check if mn has been started remotely:  
`cato_mn1 masternode debug`  

Check mn status (4 is up and running):  
`cato_mn1 masternode status`  

Start daemon:  
`cato_mn1 start`  

Reindex daemon block db:  
`cato_mn1 reindex`  

Stop daemon:   
`cato_mn1 stop`  

Daemon status:  
`cato_mn1 status`  

Get connection count:  
`cato_mn1 getconnectioncount`  

Get block count:  
`cato_mn1 getblockcount`  

Get blockchain info:  
`cato_mn1 getblockchaininfo`  

Verify the block count of your masternode and the explorer are the same:  
`cato_mn1 checkblock`  

Rename the daemon on the VPS:  
`cato_mn1 rename cato_mn3`  

Check system log of the daemon on the VPS:  
`cato_mn1 system_log`  

Check daemon log on the VPS:  
`cato_mn1 daemon_log`  

Run the same command on all masternodes:  
`all_mn_run masternode debug`  

Get all the pubkey's for https://masternodes.online/monitoring/  
`all_mn_run masternode status | grep '"addr"' | awk '{print $2'} | tr -d '",'`  

How to watch a command:  
`watch -x bash -c 'PS1="\\";. /root/.bashrc; cato_mn1 getinfo '`  
`watch -x bash -c 'PS1="\\";. /root/.bashrc; cato_mn1 daemon_log | tail '`  
`watch -n 0.4 -x bash -c 'PS1="\\";. /root/.bashrc; cato_mn1 getblockchaininfo '`  

---
---

## Stake on Linux VPS ##
Run this in putty to get the linxu desktop and catocoin gui wallet installed  
`bash <(wget -4qO- -o- https://gist.githubusercontent.com/mikeytown2/2e66db59a4063be3f0f254ee4f70937e/raw/catocoin-qt.sh)`  

"VNC" Windows client http://code.x2go.org/releases/X2GoClient_latest_mswin32-setup.exe  
Setup is still a little tricky, when adding you need to make sure that the Session type is set to `LXDE`.  


---
---





---
---
