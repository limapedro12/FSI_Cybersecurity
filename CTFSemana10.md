## Processo utilizado para resolver o CTF da Semana 10

Começamos por analisar o código geral para perceber a funcionalidade do mesmo:

![codigo](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_ce_2.png)

Vemos que é composto por 3 funções: gen(), que é usada para gerar uma random key; enc(k, m, nonce), que é usada para encriptar a mensagem "m", com a key "k" obtida através da função gen, e com o "nonce"(number used once); dec(k, c, nonce), que é usada para desencriptar a mensagem "c" e retornar a mensagem em bytes.

Analisando o código fornecido para encontrar a vulnerabilidade:

![keygen](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_ce_1.png)

Olhando para esta função que gera a key de encriptação, conseguimos ver que a key é muito vulnerável a brute-force attacks. A key começa por ter 13 bytes set to 0, e depois é que 3 bytes são randomized, ou seja, se fossemos por um brute-force attack só tinhamos que verificar no máximo: 3\*8 = 24 bits, 2^24 = 16777216 combinações, em vez do suposto número de combinações se a key fosse segura que seria 16\*8 = 128 bits, 2^128 = 3.4028237e+38.

Fazendo `nc ctf-fsi.fe.up.pt 6003`, obtemos a mensagem encriptada e o nonce:

![msgnonce](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_ce_3.png)

Ou seja, o nosso objetivo será desenvolver um script que tente todas as combinações possíveis de keys para desencriptar a mensagem com o nonce dado, obtendo assim a flag. Como sabemos que a mensagem desencriptada vai começar com "flag", podemos usar isso para quebrar o loop e mostrar a flag.

Depois de muita correção de erros, chegamos ao script final que começa na key 0, e vai incrementando-a, até chegar à key que desencripta a mensagem.

![script](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_ce_4.png)

Passando a explicar o script:

![initkey](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_ce_5.png)

Começamos com a key inicial, que é basicamente o que acontece na função gen(), só que aqui pretendemos ter todos os bytes incializados a 0, logo multiplicamos por KEYLEN, e ficamos com todos os bytes a 0, depois temos de passar a key de bytearray para bytes.

![desencript](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_ce_6.png)

Nesta secção temos o loop, que está por default infinito, até a condição à frente demonstrada ocorrer. De seguida, definimos o nonce e a mensagem ecriptada que nos foram dados acima, e como nas funções os argumentos são recebidos em bytes temos de passar ambos de hexadecimal para bytes. Depois chamamos a função dec, para vermos se acertamos na key.

![condition](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_ce_7.png)

Nesta parte do código o que estamos a fazer é verificar se adivinhamos a key. Começamos por tentar converter a suposta mensagem desencriptada que obtivemos acima para utf-8, para passarmos de bits para uma string. 

Um problema que ocorre ao tentarmos fazer isto é que muitas das vezes a mensagem desencriptada não vai estar no formato necessário para conseguirmos passar de bits para utf-8, então temos de fazer um try/except, em que se gerar um Unicode Decode Error, simplesmente ignora a mensagem e passa para a próxima key.

Se não ocorrer erro nenhum a dar desencode, verificamos se a string começa por "flag", para termos a certeza que encontramos a key correta, e damos print à flag.

Se não for a key, imprimimos a key só por efeitos de ver o nosso progresso, e depois invocamos a função nextKey(key), que incrementa a key em 1.

![nextKey](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_ce_8.png)

Para incrementarmos a key, é necessário passar para um inteiro, e depois de volta para bytes. 

Para passar de bytes para inteiro usamos o método `int.from_bytes(key, 'big')`, que significa que pega nos bytes e converte-os em inteiro em big-endian order, que significa que o byte mais significativo está no ínicio da byte array, o oposto de little, que foi o que usámos na ctf da format-string, em que nos interessava alterar a ordem dos bytes.

Prosseguindo, após passar para inteiro, incrementamos a variável auxiliar por 1, e depois voltamos a tornar o int em bytes, com o comprimento da key que nos foi dada inicialmente (para saber o número de 0 que tem de meter).

Correndo o script, esperamos alguns minutos, e obtemos a flag com sucesso:

![flag](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_ce_9.png)












