A few important things I learnt about file permissions today.

1. It's split in 3 categories: Read, Write and Execute. (rwx)

2. Each permission has a value assigned to it e.g. Read (r) = 4 , Write (w) = 2 and Execute (x) = 1 .

3. There are 3 user groups that these permissions can be assigned to: Owner, Groups and Others.

4. When one user group has more than one permission, the values are tallied up.

    Value    Symbol  Meaning                         Calculation
      0       ---    No access                         0+0+0
      1       --x    Execute only                      0+0+1
      2       -w-    Write only                        0+2+0
      3       -wx    Write + Execute access            0+2+1
      4       r--    Read only                         4+0+0
      5       r--x   Read + Execute access             4+0+1
      6       rw-    Read + Write access               4+2+0
      7       rwx    Read + Write + Execute access     4+2+1 
