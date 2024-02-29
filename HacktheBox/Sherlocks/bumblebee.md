**Sherlock Scenario:**
An external contractor has accessed the internal forum here at Forela via the Guest WiFi and they appear to have stolen credentials for the administrative user! We have attached some logs from the forum and a full database dump in sqlite3 format to help you in your investigation.


**Evidence:**
- phpbb - SQLITE3 File
- Access - text document

**Tool:**
- Timeline Explorer
- SQLITE Browser



**What was the username of the external contractor?**

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/58018c0f-43fb-489b-b98d-4f1f2eff9374)


Answer: apoole1

**What IP address did the contractor use to create their account?**


![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/1f9b3156-8ebb-4ecf-8bd9-d163c9c3fb3f)



Answer: 10.10.0.78

**What is the post_id of the malicious post that the contractor made?**

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/f475c0e5-e591-48eb-a68a-39e72579bb21)


Answer: 9
**What is the full URI that the credential stealer sends its data to?**

Upon the previous suspicious code, if we copy all the code and paste it somewhere that's easier to read, there's a suspicious url that the attacker added in their code and then post to the target
![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/f84df133-e8a4-4f48-9b80-151a1a969344)



Answer: http://10.10.0.78/update.php

**When did the contractor log into the forum as the administrator? (UTC)**


![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/ea823513-e6a7-403f-ae79-e2caa7ca6625)




Epoch time converter: https://www.epochconverter.com/

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/9bdb9fb4-8330-4cdf-a1e6-e7488696ed69)



Answer: 26/04/2023 10:53:12



**In the forum there are plaintext credentials for the LDAP connection, what is the password?**


![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/0b1678a5-aef4-4b0f-893c-dd187d46175c)



Answer: Passw0rd1


**What is the user agent of the Administrator user?**

Open access.log with notepad, I tried with the timeline explorer that it didn't print out the full user agent.


![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/4364340d-83a8-4b43-a25d-162452f9b269)


Answer: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36

**What time did the contractor add themselves to the Administrator group? (UTC)**

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/0bdee9a0-4830-44dd-b328-b21af5bed392)



Answer: 26/04/2023 10:53:51

**What time did the contractor download the database backup? (UTC)**

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/462fa8ab-6933-4e6e-999b-4436122ee31f)


Answer: 26/04/2023 11:01:38

**What was the size in bytes of the database backup as stated by access.log?**

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/81d394ef-e3bd-438a-8a5b-22aca036b805)


Answer: 34707
