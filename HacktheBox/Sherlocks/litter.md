
**Given:** 
suspicious traffic.pcap



- At a glance, what protocol seems to be suspect in this attack?

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/ac6ee83a-6925-42a9-977e-8bbf759ce959)

Answer: DNS


- There seems to be a lot of traffic between our host and another, what is the IP address of the suspect host?

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/86c3e00b-54b2-4265-8ba7-5d2b855fe0fb)

Answer: 192.168.157.145

- What is the first command the attacker sends to the client?

```
ip.addr == 192.168.157.145 && dns && dns.resp.type == TXT

```
![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/2e4cbb74-f4d5-4aa0-bb80-8f574149ec4d)


After filtering, follow UDP stream and copy all the text into Cyberchef (Hexadecimal) and covert it 
![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/52124f54-8632-4053-bcf8-cd67a9bd3b01)





There's also another udp stream which contained more information. "udp.stream eq 481"

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/0bbbb293-3f74-41b0-88eb-0d54b164dad3)


![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/a4fa8bd2-fb18-403b-a0e8-c0dce5f7b045)



**Note: It's recommended to copy all udp stream from "udp.stream eq 481" because it contains the completed required evidence**


From the Cyberchef output, we can see that the attacker lauched cmd.exe instance followed by using "whoami" command

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/72b39446-fbb9-4f87-b7c1-76c5172793eb)


Answer: whoami


- What is the version of the DNS tunneling tool the attacker is using?

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/5709f3f7-6258-4b17-96e5-26bee72f6e06)


Answer 0.07



- The attackers attempts to rename the tool they accidentally left on the clients host. What do they name it to?

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/5164d4a4-e5a3-4a8d-b381-461776dbec41)

Answer: win_installer.exe



- The attacker attempts to enumerate the users cloud storage. How many files do they locate in their cloud storage directory?

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/3e25730d-3ac2-4f73-9402-9e78bed76bd2)


Answer: 0


- What is the full location of the PII file that was stolen?

Scroll down to the bottom and start search for stolen PII due to the fact that the attacker is most likely to perform the data exfiltration in the last steps

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/e8d2bdcb-d211-494e-8600-30a513bcb9d3)

Answer: C:\Users\test\Documents\client data optimisation\user details.csv


- Exactly how many customer PII records were stolen?

If we pay an attention on each PII record, we can see that there's actually a number Infront of each person's record. Upon pattern  investigation, we can see each individual has their own number that's unique to them and keeps increasing by one.

Start with 0, Chief

![Uploading image.png…]()

Ends with 720, Ambulance person


![Uploading image.png…]()

Answer: 721
