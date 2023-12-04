## Processo utilizado para resolver o CTF da Semana 11

Começamos por investigar o código dado no ficheiro `challenge.py` para perceber a funcionalidade do mesmo:

```# Python Module ciphersuite
import os
import sys
from rsa_utils import getParams
from binascii import hexlify, unhexlify

FLAG_FILE = '/flags/flag.txt'

def enc(x, e, n):
    int_x = int.from_bytes(x, "little")
    y = pow(int_x,e,n)
    return hexlify(y.to_bytes(256, 'little'))

def dec(y, d, n):
    int_y = int.from_bytes(unhexlify(y), "little")
    x = pow(int_y,d,n)
    return x.to_bytes(256, 'little')

with open(FLAG_FILE, 'r') as fd:
	un_flag = fd.read()

(p, q, n, phi, e, d) = getParams()

print("Public parameters -- \ne: ", e, "\nn: ", n)
print("ciphertext:", hexlify(enc(un_flag.encode(), e, n)).decode())
sys.stdout.flush()```
