The code is pretty strightforward.
```
def _l(idx, s):
    '''return circular left shift
    return s[idx:] + s[:idx]
```
Then there is 
```
s = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789abcdefghijklmnopqrstuvwxyz_{}"
t = [[_l((i+j) % len(s), s) for j in range(len(s))] for i in range(len(s))]
```
Now $t[a][b][c] = x \Leftrightarrow t[(a+b+c) \pmod{65}][0][0] = x$ (easy to check). Suppose, for the rest of the code, that instead of actual characters, we operate of index in string `s`, for example, `'D'` becomes `3`, `'z'` becomes `61`.


```
for a in p:
    c += t[s.find(a)][s.find(k1[i1])][s.find(k2[i2])]
    i1 = (i1 + 1) % len(k1)
    i2 = (i2 + 1) % len(k2)
```
These lines mean that we add, character by character value $t[a][k_1(i_1)][k_2(i_2)] = t[(a + k_1(i_1) + k_2(i_2))][0][0]$. Keylength is 14. `k2` is just  `k1` reversed, so we know that `k1[i] == k2[13-i]`. Thanks to this, it's not required to know whole key to decode text, since we're only interested in sums of "opposite" values, i.e. `k1[0] + k2[13] == k1[13] + k2[0]`. Recovering half of the key is easy, as we know the beginning of the plaintext and the whole ciphertext. The known beginning turns out to be exactly $7 = 14/2$ characters.

Code to solve:
```
pt = 'SECCON{**************************}'
ct = 'POR4dnyTLHBfwbxAAZhe}}ocZR3Cxcftw9'

s = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789abcdefghijklmnopqrstuvwxyz_{}"

key = [(s.find(c) - s.find(p))% (len(s)) for p, c in zip(pt[:7],ct)]

flagchars = [s[(s.find(c) - e) % (len(s))] for c, e in zip(ct, (key+key[::-1])*30)] # 14 < 34, so I had to repeat the key (as it's done in the source)

print(''.join(flagchars))
# SECCON{Welc0me_to_SECCON_CTF_2017}
```

