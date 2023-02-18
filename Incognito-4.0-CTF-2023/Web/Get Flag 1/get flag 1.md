### Challenge Description

![](../images/web1.png)

### Homepage

![](../images/web3.png)

it seems like it may be vulnerable to SSRF (Server-Side Request Forgery)! Which means we can try to reach internal services!

so lets try Hacktricks

![](../images/hack.png)

so after few trial & error this payload seemed to be working :
```http
http://127.1:9001/flag.txt
```

![](../images/web4.png)

### Got the flag

![](../images/web5.png)
