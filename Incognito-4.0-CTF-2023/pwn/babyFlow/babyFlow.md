## Static Analysis

lets check the given binary with `checksec` & see what protections are there in the binary 

![](images/pwn1.png)

no Canary , no PIE , no NX enabled. so it should be a simple buf. well lets try 

![](images/pwn2.png)

as expected `Segment fault`.



![](images/pwn3.png)

First let us check the functions of this binary .

![](images/pwn4.png)

we can see there;s a `get_shell` function. seems interesting & another called `vulnerable_function`

lets see whats happening when given overflow input using `gdb`


![](images/pwn5.png)

we can see $eip is overwritten with `gaaa` . lets find out the offset for it

![](images/pwn6.png)

okay so we got the offset `24` .

let us craft our exploit with offset as 24 + A's & then invoke `get_shell` function to get us a shell.

![](images/pwne7.png)

## Exploit

![](images/final-pwn.png)
