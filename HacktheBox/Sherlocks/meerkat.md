**Given:**
- meerkat.pcap
- meerkat-alert.json






1. We believe our Business Management Platform server has been compromised. Please can you confirm the name of the application running?

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/89c315ce-c5dd-4d9e-a5d2-dcaea1018565)


Answer: Bonitasoft


2. We believe the attacker may have used a subset of the brute forcing attack category - what is the name of the attack carried out?

OSINT research for a subset of brute forcing attack

Answer: Credential Stuffing


3. Does the vulnerability exploited have a CVE assigned - and if so, which one?

Simply search for the keyword "CVE" in alert json file

![image](https://github.com/jirayus013t/cybersecurityprojects/assets/49973180/b678aac1-ec90-4168-870a-b49e1930f950)

Answer: CVE-2022-25237


4. Which string was appended to the API URL path to bypass the authorization filter by the attacker's exploit?

We can see a lot of unauthorized request (failed to login by brute forcing). However,  after the last HTTP 401 (Unauthorized) packet, there were packets followed after and they can been can be seen with HTTP 200 (OK)

![[Pasted image 20240206150115.png]]


If we filter for just http in wireshark and follow the http, we can see after the successful logon for "seb.broom", the attacker generate another post request that contained the appended string "i18ntranslation" with the api path: /bonita/API/pageUpload

![[Pasted image 20240206145734.png]]

Answer: i18ntranslation



5. How many combinations of usernames and passwords were used in the credential stuffing attack?


If we inspect the packet closely, we can filter the specific word like "username" out by using the below command

![[Pasted image 20240206142217.png]]


Also we have to filter out noise

![[Pasted image 20240206143111.png]]

```
frame contains 75:73:65:72:6e:61:6d:65 && !frame contains 69:6e:73:74:61:6c

```
or alternatively

```
urlencoded-form.key == "username" && !(http contains "install")
```

6. Which username and password combination was successful?



![[Pasted image 20240206143722.png]]

Even though the result shows 59, we we inspect the packet closely, there are four duplication of the set of username and password. Therefore, we have to minus it by 3. The username and password that are duplicated is as follow 

![[Pasted image 20240206143847.png]]

Answer: 56

7. We can see that the attacker attempted various password combination in wireshark and we know that http code 401 means "unauthorized" until the packet no. 3550 where http code 401 is gone. Therefore, it's very likely that we can assume that the packet 3550 is contained the correct username and password

![[Pasted image 20240206131226.png]]

![[Pasted image 20240206132113.png]]



Answer: seb.broom@forela.co.uk:g0vernm3nt


8. If any, which text sharing site did the attacker utilise?

If we filter through DNS in Wireshark, we can see suspicious "pastes.io"site

![[Pasted image 20240206133111.png]]

Answer: pastes.io


9. Please provide the filename of the public key used by the attacker to gain persistence on our host.

Filter for:
```
http.request.method
```

![[Pasted image 20240206151616.png]]


Then we have the suspicious URL, if you try to go to this URL, we can see some more information. It seems like this path leads us to something that's related to ssh key path and the suspicious "hffgra4unv" file.

![[Pasted image 20240206151924.png]]


Answer: hffgra4unv


10. Can you confirmed the file modified by the attacker to gain persistence?

According the last question. now we also have the modified path

![[Pasted image 20240206151924.png]]

Answer: /home/ubuntu/.ssh/authorized_keys


11. Can you confirm the MITRE technique ID of this type of persistence mechanism?

![[Pasted image 20240206152502.png]]


Answer: T1098.004
