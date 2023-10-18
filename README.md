# SHA256
The SHA256 creates a unique, 256 bit long hash for any input. 

### Building block functions:

Rotate Right:
Shifts the bits n positions to the right, moving the last ones to the front, as opposed to a shift right (sr) function, which shifts the bits to the right and adds 0s to the front.
```
# example: rr(255,4) ---> 4160749575
# bin(255) =        0b00000000000000000000000011111111
# bin(4160749575) = 0b11110000000000000000000000001111
```
Choice:
Takes in three numbers (e,f,g). For each bit in the arguments, if e ==1, it adds the bit in f to the result, else it adds the bit in g.
```
# example: ch(157,405,433) ---> 437 
# bin(157) = 0b010011101
# bin(405) = 0b110010101
# bin(433) = 0b110110001
# bin(437) = 0b110110101

```
Majority:
Takes in three numbers (a,b,c). For each bit in the arguments, it returns the bit that is more frequent in that position in the arguments.
```
# example: maj(330,405,433) ---> 401  
# bin(330) = 0b101001010
# bin(405) = 0b110010101
# bin(433) = 0b110110001
# bin(401) = 0b110010001

```
The σ0 (lowercase sigma 0) function takes in a word and returns the rr(word,7) XOR rr(word,18) XOR sr(word, 3)
```
def s0(word):
    return rr(word, 7) ^ rr(word, 18) ^ (word >> 3)
```
The rest of the sigma functions are just variations on this idea. 

### Padding the message:
We also need to pad the message so its length is a multiple of 512 bits. To do so, the function appends a 1 to the end of the message, appends enough zeroes to reach the nearest multiple of 512-64 and fills those last 64 bits with the length of the message. 

### Hashing:
The first group of constants are the hexadecimal fractions of the square roots of the first 8 prime numbers, which are irrational. To convert decimal numbers to a hex fraction, we take the irrational part of the number, multiply it by 16, take the hexadecimal of the integer part of the result and repeat the process until we have 8 digits. 
For example:
```
The first constant is 0x6a09e667, which corresponds to the square root of 2.
2**(1/2.)= 1.41421356237....
To convert that to a hex fraction, we take the irrational part (0.41421356237...)
(0.41421356237...)*16 = 6.62741699797...    the int part of the result is 6, hex(6) = 6
We take the new irrational part and repeat the process
(0.62741699797...)*16 = 10.0386719675...    int part is 10, hex(10) = a
(0.0386719675..)*16 = 0.618751480198...     int part is 0, hex(0) = 0
...
```
The second group of constants is calculated in a similar way. They are the cube roots of the first 64 primes and are saved in k. 

The algorithm starts by splitting the message into chunks of 512 bits. It will loop through each chunk. For each chunk, it divides the chunk into 16 words of 32 bits and appends them to an array I named word_array but is also commonly called schedule. The schedule needs to be 64 words long, so the remaining 48 words are calculated using previous words and sigma funcions. 

Then it creates a temporary hash array (length 8) and sets its initial values to the first group of constants. 
For each word in the schedule, it creates two temporary words using the corresponding constant in k, values in temp hash and the basic functions. 
After the words are created, it shifts every temporary hash one position down (and modifies the 5th hash using a temporary word), and sets the first item to the sum of the two temporary words. 
When the loop is done, the eight values in temporary hash have been completely modified many times. The algorithm then loops through the hashes one last time, adding the original values to each of them. It saves that array as the initial values of the temporary hash array for the next chunk.
After looping through every chunk, the algorithm returns the concatenation of the values in the last hash_array.


### Documentation:
To hash a message (string):
```
hash = SHA256(pad_message(message))
```
