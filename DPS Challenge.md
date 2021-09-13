# This is my first walkthrough, please keep that in mind.

## This room was created by Dead Pixel Sec. These are the nicest group of people I have met on internet.

The room is located at <https://tryhackme.com/room/dpschallenge> 


#### I looked at the second question for information that might be being searched. I used the find string option to look for “Flight” The results were SMB2 traffic from the target and the attacker.

***What is the IP address of the hacker?***  

- *192.168.200.7*

I exported smb objects via file-export objects-smb from the pcap and found 2 files. The first being his flight info and the second being a picture of a goose with his computer flag on it.


***Where is Tom flying to?*** 
- *Phoenix*

***How much did Tom pay (in USD) in total for the flight?***

- *219.30*

***What is the flag on Tom's computer? (format: @@@-@@@-####)***

- *Sky-Leak-2304*

I looked at the traffic between the attacker and the target, using ip.src == attacker. Packet 49180 shows DCERPC protocol which shows us that attackers distribution.

***What is the name of the Linux distribution used by the attacker?***  

- *Kali*

I used the find for “.exe” while I had the packets filtered with ip.src==attacker

***Tom had an executable file in his data volume, what is the name of that file?***

- *wireshark-win64-3.0.6.exe*

Looked for the ntlm information, I filtered via ntImspp and looked for the auth packet and copied the domain  - WORKGROUP and user name Tom_Fedder. I next looked for the NTProofStr and Ntlmv2 response and copied those to a text document.  I then looked for the server challenge and I put them all together with this format username::domain:ServerChallenge:NTproofstring:modifiedntlmv2response

tom_fedder::WORKGROUP:497bb71c637c055b:ed93b7d3ed61c510d74d98fb688476e0:01010000000000006201533f649bd501e17dd2637b53d5b80000000002001e004400450053004b0054004f0050002d0045004a003800310047004e004a0001001e004400450053004b0054004f0050002d0045004a003800310047004e004a0004001e004400450053004b0054004f0050002d0045004a003800310047004e004a0003001e004400450053004b0054004f0050002d0045004a003800310047004e004a00070008006201533f649bd501060004000200000008003000300000000000000000000000000000005b7c87cb37c89eb064a9e7faca6bcf8b7f59aca797b867c2c20a8ff0a559ebfb0a001000000000000000000000000000000000000900240063006900660073002f003100390032002e003100360038002e003200300030002e00350000000000

Then I put it in a text document and then used hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt

***What is Tom's account password?*** 

- *castlepark93*
