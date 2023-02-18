## Static Analysis

lets check the given binary with `checksec` & see what protections are there in the binary 

![](images/pwn1.png)

no Canary , no PIE , no NX enabled. so it should be a simple buf. well lets try 

![](images/pwn2.png)

as expected `Segment fault`.



![](images/pwn3.png)

lets see whats happening when given overflow input using `gdb`

![](images/pwn4.png)
