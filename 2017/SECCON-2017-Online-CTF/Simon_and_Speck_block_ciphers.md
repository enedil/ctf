I realized that since only 4 characters are unknown, instead of analyzing how it works, I could simply brute force it. There's Python implementation of the cipher:
[Simon Speck Ciphers]([https://github.com/inmcm/Simon_Speck_Ciphers)

```
import itertools

from simon import SimonCipher
import tqdm

plain = 0x6d564d37426e6e71
cipher = 0xbb5d12ba422834b5

chars = b'0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ-_!?:;'

for d in tqdm.tqdm(itertools.product(chars, repeat=4)):
    c = SimonCipher(int.from_bytes(b'SECCON{'+bytes(d)+b'}','big'),key_size=96,block_size=64)
    if c.encrypt(plain) == cipher:
        print(d)
        break

```
This gives the flag: `SECCON{6Pz0}`
