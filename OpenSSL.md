# OpenSSL

Using openssl we can generate the public and private keys for asymmetric encryption.

To generate private use below command on your computer, where openssl is installed.

```
openssl genrsa
```

To Encrypt the generated public key we can use symmetric key encryption comes with openssl. We need to use the below command.

```bash
samleet@DESKTOP:~$ openssl genrsa -aes256
Generating RSA private key, 2048 bit long modulus (2 primes)
.....................................................+++++
........................................................................................+++++
e is 65537 (0x010001)
Enter pass phrase:
Verifying - Enter pass phrase:
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-256-CBC,C0FF0986BC33240C3B38B6AA24D8D325

hUen6kUaqHvjDcBQK7EmKdpJQAthMLJD9zP+YhPpjZearRbiJ0NBUhW6uRXd6Q5n
MKqS+RkwWJC1F0RbLfYrkf6bjPw/EDxalXHeRpk8PrRU5sI+/gRiZDcDz4ED3/Lp
K4Tv2Ea0/VGWyh4DYvyfO9BjuAkxoaYhDuKPODtdNQTnRqtFwsmm78NnZ6dopEuq
GNRFwTb+TUQwDEepumLDpr8SZ8yYBSjc6S/CcWg8k67jX/oJ6FfTvUrcEarNtDJX
cRSeURCwg3L4QtJ3ustN96f4mZ1KJI7SPS6h76R13RSVqUjaEUJY5mlTjBoT9b7s
c9MBXYXbrAFhP3iJ19q/GKk+FhvK3HgNnoGj8OMeoYpY+X3FYsOTb9aFbhlI/TgU
8D5K4SAfXVOSUWjbK21ZW5AeaWId5GCGNDMjJatXM9Nksl4RCqz61/JBDpstGnJm
JaFLh9uY7ow6XdiVQ4e6QVQ6mE4O4A6swm+tnwlDJWvtFB9IrqBU238ViX9T6n+r
PREFuSHuiC2YW6yu0+FmAXVQcmnagcfT4MbuDFapVy/3MS9rOu7bnGpNgqABi0HM
5Ob6yDnlqbzKQo9rvjinwKT0jAb0H0DAS5LVwBCG9h6nAOEKp+ZYwO86RJxPrZnI
Vxrk3uWsZa70KU/XOf5emvdyuNialHHO6YFAHYNDjr8wzFjYsBNA5nNnYP2u9RS3
bcNyWYuqMa4HTVkBulUwqlnyqDWVnK3DeQ+3N94gkWiDwfT+gvSqe8kLYAQQUo0F
fAv9Oxl3AyrWGBqC3IofPem2OcfzhCZH/cc6Rh2kGpuam2RraP/jZmgxtlSlAKBz
9upwCJND5AAeJ4hhOx91OZWhKcM4n0Pked/XngPCyWyvElQYrrcR/pN2JaSvs9b3
kd7/Q3npJTBEICTpMKiZv5T1olDFO8KCDeEUyeO1CfHdJAMq7zW3Raro+mtGRlfo
9JoCpSWVsB5p5RXuv4iN9mwUnE9KQpLItS9Kjfl64ZIOMiPw4H6EYy6Lb2E6jSz+
mJcA124frLjtXlbgEHmoGglFSNZQsWKYG/x60Fwm5oauFTAwnJKe8Vn8IPG3RETx
e0sv1ERIjj65tDwK6mcNduND9jZtQVe6TPUFZFSWZhrP4XYwyS2j7yQ+xF52Aqlx
s2NkNMrLFeVsOXcUwErKMTITdB+6Hm9BGSiOSb21UCOF6klPmWZXZv5Dt1DyHOHW
1/CWeoZzmeJq8H9lHnc/lkXjsWELJbMCnZD6yRgctgFwCDkh3rN89H5of0dq8spT
FmYDCLOZWaYBSKsHSNWiAm7YCPdSl2smAjyJEzIFzlMXFvjrx7UX1xv/P1LTo15J
KXtVMOZoFZj3TDAYsNxwY6DrX42PDKYI/3weEsNTXolybsIcq/Ifl7HhHZfpUBbw
O+sV1VbJzCfVd+W0NcYr14pr5Mxobg5W3+e5DnZTP3HXyVX5nBlwN23UpkvSmeWj
67H9yWWcAqQw62vphxSr2TfV8xMW3kd2JRbtCEcF+qu7pYfC4rh+UZldwQYMG/VU
5NWA8IWmRjlgbjPlduTZSsMBLeT4qZ5hiOITCOWOgJVtWeq0f/L3HbDRDbX7BLIs
-----END RSA PRIVATE KEY-----
```

In above command we are using aes256 encryption, we can use des encryption as well.

```
openssl genrsa -des
```

To save the generated rsa key on a file directly we can take help of `-out` flag. In below command, the output will be saved into a file with name `private.pem`.

```
openssl genrsa -aes256 -out private.pem
```

So we have generated the private key. We need to generate the public key as well. To do so, we can use the below command. In this command `-outform PEM` specifies the file type of the key and `-pubout` tells to generate the public key.

```
openssl rsa -in private.pem -outform PEM -pubout -out public.pem
```
By default, openssl genrates the private key with 2048 bit length. But to make it 4096, we can specify that.

```
openssl genrsa 4096
```