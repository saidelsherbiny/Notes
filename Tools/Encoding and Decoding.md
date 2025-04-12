# base64
`base64 encoded strings are easily spotted since they only contain alpha-numeric characters. However, the most distinctive feature of `base64` is its padding using `=` characters. The length of `base64` encoded strings has to be in a multiple of 4. If the resulting output is only 3 characters long, for example, an extra `=` is added as padding, and so on`
#### Encoding
```shell-session
echo string | base64
```
#### Decoding
```shell-session
echo aHR0cHM6Ly93d3cuaGFja3RoZWJveC5ldS8K | base64 -d
```

# Hex
`Any string encoded in `hex` would be comprised of hex characters only, which are 16 characters only: 0-9 and a-f. That makes spotting `hex` encoded strings just as easy as spotting `base64` encoded strings.
#### Encoding
```shell-session
echo String | xxd -p
```
#### Decoding
```shell-session
echo 68747470733a2f2f7777772e6861636b746865626f782e65752f0a | xxd -p -r
```


# Caesar/Rot13
`Even though this encoding method makes any text looks random, it is still possible to spot it because each character is mapped to a specific character. For example, in `rot13`, `http://www` becomes `uggc://jjj`, which still holds some resemblances and may be recognized as such.
#### Encoding 
```shell-session
echo String | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```
#### Decoding
```shell-session
echo uggcf://jjj.unpxgurobk.rh/ | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```
