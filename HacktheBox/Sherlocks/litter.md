
**Given:** 
suspicious traffic.pcap



At a glance, what protocol seems to be suspect in this attack?

==DNS==
![[Pasted image 20240128145523.png]]
Task 2

There seems to be a lot of traffic between our host and another, what is the IP address of the suspect host?

==192.168.157.145==

![[Pasted image 20240128145431.png]]

Task 3

What is the first command the attacker sends to the client?

```
ip.addr == 192.168.157.145 && dns && dns.resp.type == TXT

```
![[Pasted image 20240206105116.png]]


After filtering, follow UDP stream and copy all the text into Cyberchef (Hexadecimal) and covert it 
![[Pasted image 20240206105751.png]]





There's also another udp stream which contained more information. "udp.stream eq 481"

![[Pasted image 20240206112559.png]]


![[Pasted image 20240206113147.png]]


**Note: It's recommended to copy all udp stream from "udp.stream eq 481" because it contains the completed required evidence**


From the Cyberchef output, we can see that the attacker lauched cmd.exe instance followed by using "whoami" command

![[Pasted image 20240206110014.png]]


![[Pasted image 20240206110059.png]]





Answer: whoami

Task 4

What is the version of the DNS tunneling tool the attacker is using?

![[Pasted image 20240206110658.png]]

Answer 0.07



Task 5

The attackers attempts to rename the tool they accidentally left on the clients host. What do they name it to?

![[Pasted image 20240206113101.png]]

Answer: win_installer.exe


Task 6

The attacker attempts to enumerate the users cloud storage. How many files do they locate in their cloud storage directory?

![[Pasted image 20240206113924.png]]


Answer: 0

Task 7

What is the full location of the PII file that was stolen?

Scroll down to the bottom and start search for stolen PII due to the fact that the attacker is most likely to perform the data exfiltration in the last steps

![[Pasted image 20240206115232.png]]

Answer: C:\Users\test\Documents\client data optimisation\user details.csv

Task 8

Exactly how many customer PII records were stolen?

If we pay an attention on each PII record, we can see that there's actually a number Infront of each person's record. Upon pattern  investigation, we can see each individual has their own number that's unique to them and keeps increasing by one.

Start with 0, Chief

![[Pasted image 20240206120849.png]]
Ends with 720, Ambulance person


![[Pasted image 20240206120439.png]]
Answer: 721
