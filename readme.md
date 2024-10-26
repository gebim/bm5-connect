
The Beomaster 5 (BM5) is now a old machine running Windows XP. But for those out there still using it ;- this readme. Tested under Ubuntu 24.04.
## Beomaster 5 configuration
Set a network password on the BM5. First enable the extended settings by select "mode" on BS5, then focus on settings and press RIGHT, LEFT, RIGHT, LEFT, GO. Then go to NETWORK SETTINGS and set a CLIENT PASSWORD. Also remember the ip address of the BM5 as shown in NETWORK INFO.
## Linux preparation
It is essential to have a clean tagging of the music files with embedded cover-art. A great SW for this task is Picard. But Picard sets and uses UTF8 encoded filenames. Hence, these are containing spaces and other special characters which are causing problems with the BM5. To clean up the filenames `detox` is a great little tool to remove all these problematic characters. Just do a  `detox -R dirname/`

The BM5 just supports the (now obsolete and insecure) SMB1 protocol. We need to enable the insecure SMB1 under Linux by editing the samba server configuration file. In case samba is not installed it is sufficient to install only the `smbclient`.
Edit `/usr/share/samba/smb.conf` by adding the line: `client min protokol = NT1` in global setting paragraph: 
```
#======================= Global Settings =======================

[global]

## Browsing/Identification ###

# Change this to the workgroup/NT-domain name your Samba server will part of
   workgroup = WORKGROUP
client min protokol = NT1
```

Technical details are explained in: https://askubuntu.com/questions/1270219/ubuntu-20-04-force-nautilus-to-use-smb1-when-etc-samba-does-not-exist
Restart the samba server. 

Note: following use the ip address of your BM5. In this example we use `192.168.1.106`. 
List the BM5 share with: `smbclient -L 192.168.1.106 -U BM-USER`:
```
Sharename       Type      Comment
	---------       ----      -------
	IPC$            IPC       Remote IPC
	BM-SHARE$       Disk      BeoMaster5 Share

```
After this test we are ready to connect.
### Connecting to the BM5
In the file-manger press "+Other Locations" and in the bottom line type:
![Pasted image 20241026170035](Pasted%20image%2020241026170035.png)
Press Connect and a dialog will appear. Set as Username: BM-USER and type the password in capital letters.

![Pasted image 20241026170225](Pasted%20image%2020241026170225.png)


Finally:

![Pasted image 20241026170717](Pasted%20image%2020241026170717.png)

Alternatively, you can connect from the command line too:

`gio mount -a smb://192.168.1.106/bm-share$/`

