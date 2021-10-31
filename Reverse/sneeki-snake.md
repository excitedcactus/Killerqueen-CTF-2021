# sneeki snake

## Challenge Description

![image](https://user-images.githubusercontent.com/56489087/139575137-a4a43ae4-1f2c-49f3-bbe4-fc49238d99ea.png)

We are given a text file named: sneekisnake.txt

## sneekisnake.txt

```
  4           0 LOAD_CONST               1 ('')
              2 STORE_FAST               0 (f)

  5           4 LOAD_CONST               2 ('rwhxi}eomr\\^`Y')
              6 STORE_FAST               1 (a)

  6           8 LOAD_CONST               3 ('f]XdThbQd^TYL&\x13g')
             10 STORE_FAST               2 (z)

  7          12 LOAD_FAST                1 (a)
             14 LOAD_FAST                2 (z)
             16 BINARY_ADD
             18 STORE_FAST               1 (a)

  8          20 LOAD_GLOBAL              0 (enumerate)
             22 LOAD_FAST                1 (a)
             24 CALL_FUNCTION            1
             26 GET_ITER
        >>   28 FOR_ITER                48 (to 78)
             30 UNPACK_SEQUENCE          2
             32 STORE_FAST               3 (i)
             34 STORE_FAST               4 (b)

  9          36 LOAD_GLOBAL              1 (ord)
             38 LOAD_FAST                4 (b)
             40 CALL_FUNCTION            1
             42 STORE_FAST               5 (c)

 10          44 LOAD_FAST                5 (c)
             46 LOAD_CONST               4 (7)
             48 BINARY_SUBTRACT
             50 STORE_FAST               5 (c)

 11          52 LOAD_FAST                5 (c)
             54 LOAD_FAST                3 (i)
             56 BINARY_ADD
             58 STORE_FAST               5 (c)

 12          60 LOAD_GLOBAL              2 (chr)
             62 LOAD_FAST                5 (c)
             64 CALL_FUNCTION            1
             66 STORE_FAST               5 (c)

 13          68 LOAD_FAST                0 (f)
             70 LOAD_FAST                5 (c)
             72 INPLACE_ADD
             74 STORE_FAST               0 (f)
             76 JUMP_ABSOLUTE           28

 14     >>   78 LOAD_GLOBAL              3 (print)
             80 LOAD_FAST                0 (f)
             82 CALL_FUNCTION            1
             84 POP_TOP
             86 LOAD_CONST               0 (None)
             88 RETURN_VALUE
```

This is just bytecode from an interpreted python script. After doing some research its easy to decode what it is into a normal script by hand.

## Before Compile

```python3
from dis import dis

def f():
    f = ''
    a = 'rwhxi}eomr\\^`Y'
    z = 'f]XdThbQd^TYL&\x13g'

    a = a + z

    for i, b in enumerate(a):
        c = ord(b)
        c = c - 7
        c = c + i
        c = chr(c)
        f += c
    print(f)

#dis(f)

f()
```

We use the dis function to spit out the output of this function in bytecode onto our terminal. We do this so we can compare to ensure that the bytecode matches.

![image](https://user-images.githubusercontent.com/56489087/139575282-646473ab-3053-40a8-a4db-dd7404861d3f.png)

A mysterious flag has appear!
