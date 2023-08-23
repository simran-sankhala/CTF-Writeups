# wCTF 2023 (SeaSides GOA)

## Rev

in this challenge one binary was given.

lets open this binary in `Binary-Ninja`

Analysing the functions we got :

![Untitled](wCTF%202023%20(SeaSides%20GOA)%20b518994d10eb40e992fc45cc25c4f84e/Untitled.png)

so its basically taking the flag as password in md5 form ,

the password is `FERf2!4erwr4` so lets get the md5 form using `md5sum` command :

```bash
$ echo -n 'FERf2!4erwr4'|md5sum
abc3efe2baf9d6d73ac088f14888c786
```

so the flag will be : `SEASIDES{abc3efe2baf9d6d73ac088f14888c786}`

## Cloud

in the challenge description , `myfirst-webpage` keyword was given , i understood its a `S3` Bucket , i tried with aws cli but didnt get anything

so tried the webway :

`<bucket-name>.s3.amazonaws.com`  , then just fuzzed the endpoint for flag , which was : `flag.txt`

![Untitled](wCTF%202023%20(SeaSides%20GOA)%20b518994d10eb40e992fc45cc25c4f84e/Untitled%201.png)

## Web

i found lfi & tried array bypass method using  `[]` to get the flag path location

```bash
http://65.2.167.55:12000/search?term=../../../../../../../etc/passwd&sample[]=../../../../../main.js

http://65.2.167.55:12000/search?term=../../../../../../../etc/passwd&sample=../../../../../usr/src/app/index.js
```

after getting the flag path , just used curl with grep to get the exact flag :

![Untitled](wCTF%202023%20(SeaSides%20GOA)%20b518994d10eb40e992fc45cc25c4f84e/Untitled%202.png)

## Crypto

the given description was :

```bash
logix
100
Engage your analytical skills in decoding a hidden message through an intricate encoding technique. Can you decipher the puzzle and uncover the well-guarded information within?

Try this -

K]YKQ]Kc(y!(|.~-.!|(+z)!,,,}/{)+ // ~ ()e
```

it seemed like `XOR`

so i used `Cyberchef` to bruteforce the key 🔑 & got the flag using key `18` :

![Untitled](./Untitled3.png)

it can be solved the scripting way as well using `pwntools` in python :

```python
from pwn import *
ff = 'K]YKQ]Kc(y!(|.~-.!|(+z)!,,,}/{)+ // ~ ()e'
print(xor(ff, 0x18))
```

![Untitled](./Untitled%204.png)

## OSINT

for the osint challenge , in description one image was given `inc.jpg`

![Untitled](wCTF%202023%20(SeaSides%20GOA)%20b518994d10eb40e992fc45cc25c4f84e/Untitled%205.png)

after doing reverse cross image search for a while , when i didnt get any results ,

i thought of checking the photo’s metadata using `exiftool` & luckily the flag was there lol :

![Untitled](wCTF%202023%20(SeaSides%20GOA)%20b518994d10eb40e992fc45cc25c4f84e/Untitled%206.png)%
