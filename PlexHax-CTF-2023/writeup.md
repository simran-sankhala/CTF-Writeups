# PlexHax CTF 2023

![plex.jpeg](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/plex.jpeg)

### My Stats :

![Screenshot 2023-08-14 at 3.30.28 PM.png](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Screenshot_2023-08-14_at_3.30.28_PM.png)

## Challenge 1 : (Analyze)

Challenge Description :

![Screenshot 2023-08-14 at 3.33.28 PM.png](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Screenshot_2023-08-14_at_3.33.28_PM.png)

in the challenge description , one pcap file was given. lets open it in Wireshark & see what we got

i applied `http` filter & saw one flag.png file was transfered

![Screenshot 2023-08-14 at 3.35.07 PM.png](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Screenshot_2023-08-14_at_3.35.07_PM.png)

lets export the image & see whats it about

![Screenshot 2023-08-14 at 3.36.31 PM.png](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Screenshot_2023-08-14_at_3.36.31_PM.png)

![Screenshot 2023-08-14 at 3.37.33 PM.png](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Screenshot_2023-08-14_at_3.37.33_PM.png)

& on opening the flag.png , we got the flag :

![Screenshot 2023-08-14 at 3.38.21 PM.png](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Screenshot_2023-08-14_at_3.38.21_PM.png)

**Flag** : squiblypibles

## Challenge 2: (****Looksee)****

lets see the challenge homepage by visiting the given link in the description 

![Screenshot 2023-08-14 at 3.41.47 PM.png](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Screenshot_2023-08-14_at_3.41.47_PM.png)

looks like a `SSRF` vuln to me , lets test it out

![Screenshot 2023-08-14 at 3.43.18 PM.png](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Screenshot_2023-08-14_at_3.43.18_PM.png)

we do see some response , maybe its using filters , so lets try it with some filter bypass SSRF payloads :

![Untitled](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Untitled.png)

this payload worked fine , & we got our flag

![Untitled](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Untitled%201.png)

**Flag :** `falg=yeedlydeet`

## Challenge 3: (****Dis Function Is Cool)****

lets look at the challenge description once:

![Screenshot 2023-08-14 at 3.50.19 PM.png](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Screenshot_2023-08-14_at_3.50.19_PM.png)

its a `JavaScript` reverse Engineering challenge.

i used python to reverse the xor logic to get the input value mapping to this given flag

```python
import sys

def secretFunction(input):
    secret = "CTF{"
    key = ord('K') - 65
    for i in range(len(input)):
        charCode = ord(input[i]) - 65
        secret += chr(((charCode ^ key) % 26) + 65)
    secret += "}"
    print(secret)

secretFunction(sys.argv[1])
```

![Screenshot 2023-08-14 at 3.56.01 PM.png](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Screenshot_2023-08-14_at_3.56.01_PM.png)

**Flag : SNEAKYSNEK**

## Challenge 4: (****hodor)****

from the challenge name it seems , it’s vulnerable to `IDOR` but lets tests it out 

 

![Screenshot 2023-08-14 at 3.59.02 PM.png](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Screenshot_2023-08-14_at_3.59.02_PM.png)

![Screenshot 2023-08-14 at 3.58.46 PM.png](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Screenshot_2023-08-14_at_3.58.46_PM.png)

a pdf link was given in challenge description as : `g1337.pdf` 

so lets fuzz it for more `200` response with the numbering series by bruteforcing the request’s response.

lets use python to automate this process

```python
import requests

base_url = "https://hodor.plexhax.com/"
endpoint = "/g{}.pdf"
start_number = 0
end_number = 10000

for number in range(start_number, end_number + 1):
    url = base_url + endpoint.format(number)
    response = requests.get(url)
    print(f"Request to {url} - Status code: {response.status_code}")
```

lets run this & see which numbers of pdfs gets `200` response apart from 1337.

![Screenshot 2023-08-14 at 4.02.39 PM.png](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Screenshot_2023-08-14_at_4.02.39_PM.png)

we see , we got one more `200` response from `g1450.pdf` , lets visit it & see what’s inside that pdf

![Untitled](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Untitled%202.png)

boom ! we got the flag

**Flag : mahypasforlemon**

## Challenge 5: (****myapp)****

![Screenshot 2023-08-14 at 4.06.04 PM.png](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Screenshot_2023-08-14_at_4.06.04_PM.png)

lets visit the site , & see how its looking……..

![Screenshot 2023-08-14 at 4.07.00 PM.png](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Screenshot_2023-08-14_at_4.07.00_PM.png)

it is having an input bar with some product name value to be parsed

lets dump this request to burpsuite & see the request headers…..

![Untitled](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Untitled%203.png)

okay so its sending our input via some query param, from the name query itself `SQLi` clicked in my head , so lets parse the request to `SQLMap` to see if its vulnerable to `SQLi` or not

```python
❯ sqlmap -r request.txt --dump-all --threads=10
```

![Screenshot 2023-08-14 at 4.17.11 PM.png](PlexHax%20CTF%202023%206d044ed6faf74f6cb93146af1a64dcb7/Screenshot_2023-08-14_at_4.17.11_PM.png)

**Flag : beepboop**

# Thank You !! 💐
