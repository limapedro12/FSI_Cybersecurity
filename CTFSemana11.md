## Processo utilizado para resolver o CTF da Semana 11

Começamos por investigar o código dado no ficheiro `challenge.py` para perceber a funcionalidade do mesmo:

```
# Python Module ciphersuite
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
sys.stdout.flush()
```

O código dado 2 funções: enc(x,e,n), que é utiliza o algoritmo RSA para encriptar a mensagem que se encontra no ficheiro "flag.txt", com um expoente púlico "e" e um módulo associado "n"; dec(y,d,n), que é usada para desencriptar a mensagem e retorná-la em bytes.

Desta forma baseamo-nos na função dec para escrever o seguinte código:

```
from binascii import hexlify, unhexlify
from sympy import *

p = nextprime(2**512-1000) # next prime 2**512 - 1000
q = nextprime(2**513-1000) # next prime 2**513 - 1000
e = 65537 # expoente público

enc_flag=b"3966616566643633373731646565303363616466653030386438386465326265653433643936666435303639666436616539343935313035373732346330623236336534633964613636633562383234323631393763353365363266653931623530613136646137313962663933306266383064366634653039623233323265316361306665373539363564366263333434353966333030353432623934396565363135313033376137656232643266353032383535653938363530393436653036626132383265646239393665643039303039663762376133373062623631633534376237306663373265386637643136336461633463623537363463633330313030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030303030"

n=359538626972463181545861038157804946723595395788461314546860162315465351611001926265416954644815072042240227759742786715317579537628833244985694861278964217454780348482876563216377302365230016163598491444682880299355773985644743357397197253370771256309079849312272701366461312751388460974476915135214694510843

def dec(y,d):
	int_y = int.from_bytes(unhexlify(y), "little")
	x = pow(int_y,d,n)
	return x.to_bytes(256, 'little')

while (p<2**512+1000):
    if(p*q==n):
	    phi = (p-1)*(q-1)
	    d = pow(e, -1, phi)
	    c=dec(unhexlify(enc_flag),d).decode(errors='ignore')
	    if("flag" in c):
		    print(c)
		    break
    if q > 2**513+1000:
        p = nextprime(p)
        q = nextprime(2 ** 513-1000)
    q = nextprime(q)
```

No código iteramos por todas a combinações de números primos proximos dos valores 2^512 e 2^513 que estivessem num intervalo de [2^512-1000,2^512+1000] e [2^513-1000,2^513+1000], fizemos isto porque nos foi dito que p e q eram números primos proximos dos valores indicados e achamos este intervalo adequado. Para cada combinação de números primos p e q se p*q fosse igual ao módulo n então era decifrado o ciphertext dado com os valores p e q. Por fim confirmamos que obtivemos o valor correto se no string que obtivemos estivesse a palavra "flag".

Corremos o código e obtivemos a seguinte flag:

![flag](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/CTF11.PNG)
