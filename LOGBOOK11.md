## Task 1

Começamos por copiar o codigo de `/usr/lib/ssl/openssl.cnf` para um ficheiro chamado `myopen.cnf` no diretorio atual e criamos um ficheiro `index.txt` vazio e um fichero `serial` com conteudo 1000 e descomentados o atributo `unique_suvject` do ficheiro `myopen.cnf`.

![]()

![]()

![]()

Corremos o seguinte commando, de modo a gerar um certificado assinado por nos propios com a password "codecacao":

![openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 -keyout ca.key -out ca.crt]()

 What part of the certificate indicates this is a CA’s certificate?
• What part of the certificate indicates this is a self-signed certificate?
• In the RSA algorithm, we have a public exponent e, a private exponent d, a modulus n, and two secret
numbers p and q, such that n = pq. Please identify the values for these elements in your certificate
and key files.

## Task 2

