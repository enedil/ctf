I use recursive relation
```
enc[n+1] = (enc[n] + msg[n] + key[n])%128
```
Note that last bytes of message are filled with flag, so this gives another relation (after inlining key_len and pipe_offset)
```
msg[n%13 + 22] = key[n%13]
```

Using this I start with given pipe character and perform jumping through the bytes of key. Thankfully, `key_len` and `pipe_offset` are coprime, so jumping doesn't mean falling into a small cycle. Therefore all bytes will be restored.

```
#!/usr/bin/env python3

from binascii import unhexlify

encrypted = unhexlify('7c153a474b6a2d3f7d3f7328703e6c2d243a083e2e773c45547748667c1511333f4f745e')

def recover(enc, pipe_offset, key_len):
    msg = [None for _ in enc[1:]]
    key = [None for _ in range(key_len)]

    n = pipe_offset
    msg[n] = ord('|')

    for _ in range(key_len):
        key[n%key_len] = (enc[n+1] - enc[n] - msg[n]) % 128
        msg[n%key_len + pipe_offset + 1] = key[n%key_len]
        n = n%key_len + pipe_offset + 1

    for i in range(len(msg) - 1 - key_len):
        msg[i] = (enc[i+1] - enc[i] - key[i%key_len]) % 128

    return bytes(msg)

if __name__ == '__main__':
    print(recover(encrypted, 21, 13))

```
