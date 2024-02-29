Sherlock Scenario
Khalid has just logged onto a host that he and his team use as a testing host for many different purposes, it’s off their corporate network but has access to lots of resources in network. The host is used as a dumping ground for a lot of people at the company but it’s very useful, so no one has raised any issues. Little does Khalid know; the machine has been compromised and company information that should not have been on there has now been stolen – it’s up to you to figure out what has happened and what data has been taken.


**Given:**
- meerkat.pcap
- meerkat-alert.json



- We believe our Business Management Platform server has been compromised. Please can you confirm the name of the application running?

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/949db684-e32c-4a7f-8397-a751f1f43abf)

Answer: Bonitasoft

- We believe the attacker may have used a subset of the brute forcing attack category - what is the name of the attack carried out?

OSINT research for a subset of brute forcing attack

Answer: Credential Stuffing

- Does the vulnerability exploited have a CVE assigned - and if so, which one?

Simply search for the keyword "CVE" in alert json file

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/eea32031-3037-4240-9c57-1d12d9096838)

Answer: CVE-2022-25237

- Which string was appended to the API URL path to bypass the authorization filter by the attacker's exploit?


We can see a lot of unauthorized request (failed to login by brute forcing). However,  after the last HTTP 401 (Unauthorized) packet, there were packets followed after and they can been can be seen with HTTP 200 (OK)

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/fe77a203-8fc5-4509-bdda-de4e87f07520)

If we filter for just http in wireshark and follow the http, we can see after the successful logon for "seb.broom", the attacker generate another post request that contained the appended string "i18ntranslation" with the api path: /bonita/API/pageUpload

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/fa3422a1-9caa-4c43-abce-8d1f85cc71be)

Answer: i18ntranslation

- How many combinations of usernames and passwords were used in the credential stuffing attack?


If we inspect the packet closely, we can filter the specific word like "username" out by using the below command

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/d5f1dc3d-58dd-4eef-be8b-953022b5eb72)


Also we have to filter out noise

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/5758b92d-e2b0-4df0-a91f-5cac23601f25)

```
frame contains 75:73:65:72:6e:61:6d:65 && !frame contains 69:6e:73:74:61:6c

```
or alternatively

```
urlencoded-form.key == "username" && !(http contains "install")
```
- Which username and password combination was successful?


![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/69fde73c-c8d1-45e4-9d4a-01bdedfa621c)


Even though the result shows 59, we we inspect the packet closely, there are four duplication of the set of username and password. Therefore, we have to minus it by 3. The username and password that are duplicated is as follow 

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/d1858cd8-8e71-405a-a338-b0488598253a)

Answer: 56


We can see that the attacker attempted various password combination in wireshark and we know that http code 401 means "unauthorized" until the packet no. 3550 where http code 401 is gone. Therefore, it's very likely that we can assume that the packet 3550 is contained the correct username and password

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/88906422-b625-4a6a-804b-52b58dc4cd92)


Answer: seb.broom@forela.co.uk:g0vernm3nt


- If any, which text sharing site did the attacker utilise?

If we filter through DNS in Wireshark, we can see suspicious "pastes.io"site

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/c2ac0ded-121a-4b6b-923f-3773fd10d83f)

Answer: pastes.io


- Please provide the filename of the public key used by the attacker to gain persistence on our host.

Filter for:
```
http.request.method
```

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/d897f65f-09a6-4a8d-b5a6-2b6c0209c373)



Then we have the suspicious URL, if you try to go to this URL, we can see some more information. It seems like this path leads us to something that's related to ssh key path and the suspicious "hffgra4unv" file.

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/98308e34-b477-4b82-b431-b62d11a4b470)


Answer: hffgra4unv


- Can you confirmed the file modified by the attacker to gain persistence?

According the last question. now we also have the modified path

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/8061f7a6-52ed-400d-9012-7553375ddf46)

Answer: /home/ubuntu/.ssh/authorized_keys


- Can you confirm the MITRE technique ID of this type of persistence mechanism?

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/cd5c5a38-9160-46d0-8777-a8731ebcd24c)


Answer: T1098.004
