# SHA256
A Secure Hash Algorithm 256
The SHA256 creates a unique, 64 bit long hash for any input. 

The building blocks of the SHA256 are the RotateRight Function, the upper and lowercase Sigma algorithms (σ0, σ1, Σ0, Σ1), the Choice Algorithm and the Majority Algorithm.

Rotate Right:
Rotates the bits n positions to the right, moving the last ones to the front. 
```
def rr(word, count): 
    return ((word >> count) | (word << (32 - count))) % 2 ** 32
# example: rr(255,4) ---> 4160749575
# bin(255) = 0b00000000000000000000000011111111  bin(4160749575) = 0b11111000000000000000000000000111
```
Choice:
Takes in three numbers (e,f,g). For each bit in the arguments, if e ==1, it adds the bit in f to the result , else it adds the bit in g.
```
def ch(e, f, g):
    return (e & f) ^ ((~e) & g)
# example: ch(157,405,433) ---> 437,  bin(437) = 0b110110101
# bin(157) = 0b010011101, bin(405) = 0b110010101, bin(433) = 0b110110001
```
Majority:
Takes in three numbers (a,b,c). For each bit in the arguments, it returns the bit that is more frequent in that position in the arguments.
```
def maj(a, b, c):
    return (a & b) ^ (a & c) ^ (b & c)
# example: maj(330,405,433) ---> 401,  bin(401) = 0b110010001
# bin(330) = 0b101001010, bin(405) = 0b110010101, bin(433) = 0b110110001
```

