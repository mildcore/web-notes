top htop

多核查看： top下按1

### Memory 

Memory Used:
- used (RHEL 7)  
- used - buff/cache (RHEL 6)

```
pi@raspberrypi:~ $ free
              total        used        free      shared  buff/cache   available
Mem:        1858700      268084     1153524       94380      437092     1410332
Swap:        102396           0      102396
```

- total = used + free + buff/cache
- total = used + free (<= RHEL 6)

name|meaning
:--|:--
used |	Memory currently in use by running processes (used= total – free – buff/cache)
free |	Unused memory (free= total – used – buff/cache)
shared |	Memory shared by multiple processes
buffers |	Memory reserved by the OS to allocate as buffers when process need them
cached |	Recently used files stored in RAM
buff/cache |	Buffers + Cache
available |	Estimation of how much memory is available for starting new applications, without swapping.
