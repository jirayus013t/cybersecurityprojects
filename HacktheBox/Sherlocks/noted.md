**Sherlock Scenario:**
Simon, a developer working at Forela, notified the CERT team about a note that appeared on his desktop. The note claimed that his system had been compromised and that sensitive data from Simon's workstation had been collected. The perpetrators performed data extortion on his workstation and are now threatening to release the data on the dark web unless their demands are met. Simon's workstation contained multiple sensitive files, including planned software projects, internal development plans, and application codebases. The threat intelligence team believes that the threat actor made some mistakes, but they have not found any way to contact the threat actors. The company's stakeholders are insisting that this incident be resolved and all sensitive data be recovered. They demand that under no circumstances should the data be leaked. As our junior security analyst, you have been assigned a specific type of DFIR (Digital Forensics and Incident Response) investigation in this case. The CERT lead, after triaging the workstation, has provided you with only the Notepad++ artifacts, suspecting that the attacker created the extortion note and conducted other activities with hands-on keyboard access. Your duty is to determine how the attack occurred and find a way to contact the threat actors, as they accidentally locked out their own contact information. Warning : This sherlock requires an element of OSINT and players will need to interact with 3rd party services on internet.

- What is the full path of the script used by Simon for AWS operations?

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/4a59859a-4850-4af9-b91d-f449f0f04357)


Answer: C:\Users\Simon.stark\Documents\Dev_Ops\AWS_objects migration.pl


- The attacker duplicated some program code and compiled it on the system, knowing that the victim was a software engineer and had all the necessary utilities. They did this to blend into the environment and didn't bring any of their tools. This code gathered sensitive data and prepared it for exfiltration. What is the full path of the program's source file?


![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/df84f1fa-6eaa-4e81-94f6-fcb511b5fede)


Answer: C:\Users\simon.stark\Desktop\LootAndPurge.java

- What's the name of the final archive file containing all the data to be exfiltrated?

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/e39d9818-119e-4d01-80e7-9b65fc8239fc)



Answer: Forela-Dev-Data.zip

- What's the timestamp in UTC when attacker last modified the program source file?


Code for converting the time into the right format
```
import datetime

timestamp_low = -1354503710
timestamp_high = 31047188

full_timestamp = (timestamp_high << 32) | (timestamp_low & 0xFFFFFFFF)

timestamp_seconds = full_timestamp / 10**7
timestamp = datetime.datetime(1601, 1, 1) + datetime.timedelta(seconds=timestamp_seconds)

formatted_timestamp = timestamp.strftime("%Y-%m-%d %H:%M:%S")

print(formatted_timestamp)

```




![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/07ab5887-1802-434a-ae7c-64f736942e52)


Answer: 2023-07-24 09:53:23



- The attacker wrote a data extortion note after exfiltrating data. What is the crypto wallet address to which attackers demanded payment?


YOU HAVE BEEN HACKED.txt

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/dcf5485b-d7de-41a3-a452-bcd4ee903ce8)



LootAndPurge.txt

Password for pastebin address:

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/06cab822-116b-4d6e-b00e-2f3d5c28585a)


![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/a9711cc7-8191-44a1-bf2a-00bddbf6ed48)




Answer: 0xca8fa8f0b631ecdb18cda619c4fc9d197c8affca

- What's the email address of the person to contact for support?

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/199745bf-7593-4eef-9c46-89dcc94b1c9f)



Answer: CyberJunkie@mail2torjgmxgexntbrmhvgluavhj7ouul5yar6ylbvjkxwqf6ixkwyd.onion
