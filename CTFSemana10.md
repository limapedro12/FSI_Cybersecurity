## Processo utilizado para resolver o CTF da Semana 10

Começamos por investigar o código dado no ficheriro `cipherspec.py` para perceber a funcionalidade do mesmo:

![codigo](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_ce_2.png)

Uma ciphersuite é o conjunto de algoritmos utilizado para manter uma comunicação segura, neste caso, para encriptar e desencriptar uma mensagem.

No código dado contém uma ciphersuite composta por 3 funções: gen(), que é usada para gerar uma chave aleatória; enc(k, m, nonce), que é utiliza o algoritmo AES-CTR para encriptar a mensagem "m", com uma chave "k", obtida através da função "gen", e com um "nonce"; dec(k, c, nonce), que é usada para desencriptar a mensagem "c" e retornar a mensagem em bytes.

Analisamos o código da função "gen" de modo a encontrar a vulnerabilidade:

![keygen](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_ce_1.png)

Olhando para esta função que gera a chave de encriptação, conseguimos ver que a chave é vulnerável a ataques de brute-force. A chave começa por ter 13 bytes definidos como 0, e depois 3 bytes são aleatórios, ou seja, se tentarmos um ataque brute-force só tinhamos que verificar no máximo: 3\*8 = 24 bits, 2^24 = 16777216 combinações, em vez do suposto número de combinações se a chave fosse realmente segura que seria 16\*8 = 128 bits, 2^128 = 3.4028237e+38 combinações.

Fazendo `nc ctf-fsi.fe.up.pt 6003`, obtemos a mensagem encriptada e o respetivo nonce:

![msgnonce](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_ce_3.png)

Ou seja, o nosso objetivo será desenvolver um script que tente todas as combinações possíveis de chaves para desencriptar a mensagem com o nonce dado, obtendo assim a flag. Como sabemos que a mensagem desencriptada vai começar com "flag", podemos usar isso como condição para sair do loop e devolver-nos a flag.

Depois de muitas correções de erros, chegamos ao script final. Este começa a chave igual a 0, e vai incrementando-a, até encontrar a chave que desencripta a mensagem.

![script](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_ce_4.png)

Passando à explicação do script:

![initkey](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_ce_5.png)

Começamos com a chave inicial, que é similar ao que acontece na função gen(), só que aqui pretendemos ter todos os bytes incializados a 0, logo multiplicamos `b'\x00'` por KEYLEN, e ficamos com todos os bytes da chave a 0, depois temos que passar a chave de bytearray para bytes.

![desencript](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_ce_6.png)

Nesta secção temos o loop, que corre infinitamente, até a condição à frente demonstrada o quebre. Dentro dete ciclo while, definimos o nonce e a mensagem ecriptada que nos foram dados acima. Como nas funções os argumentos são recebidos em hexadecimal temos que passar os de hexadecimal para bytes. Depois chamamos a função dec, de modo a tentar desencriptar a mensagem com a chave atual, e assim testar se esta é a chave correta.

![condition](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_ce_7.png)

Nesta parte do código o que estamos a fazer é verificar se adivinhamos a chave. Começamos por tentar converter a suposta mensagem desencriptada que obtivemos acima para utf-8, para passarmos de bytes para uma string. 

Um problema que ocorre ao tentarmos fazer isto é que muitas das vezes a mensagem desencriptada não está no formato necessário para conseguirmos passar de bytes para utf-8, então temos de fazer um try/except, em que caso surja um Unicode Decode Error, simplesmente ignora a mensagem e passa para a próxima chave.

Se não ocorrer nenhum erro, verificamos se a string começa por "flag", para termos a certeza que encontramos a chave correta, e damos print à flag.

Se não for a chave correta, imprimimos a chave só para efeitos de ver o nosso progresso, e depois invocamos a função "nextKey(key)", que incrementa a chave em 1.

![nextKey](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_ce_8.png)

Para incrementarmos a key, é necessário passar para inteiro, e depois de volta para bytes. 

Para passar de bytes para inteiro usamos o método `int.from_bytes(key, 'big')`, que converte os o conjunto de bytes dado em inteiro em big-endian order, que significa que o byte mais significativo está no ínicio da byte array, o oposto de little-endian, que foi o que usámos na ctf da format-string, em que nos interessava alterar a ordem dos bytes.

Prosseguindo, após passar para inteiro, incrementamos a variável auxiliar em 1, e depois voltamos a tornar o int em bytes, com o comprimento da key que nos foi dada inicialmente (para saber o número de 0 que temos que acrescentar no inicio).

Correndo o script, esperamos alguns minutos, e obtemos a flag com sucesso:

![flag](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_ce_9.png)
