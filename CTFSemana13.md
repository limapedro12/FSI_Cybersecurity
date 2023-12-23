## Processo utilizado para resolver o CTF da Semana 13


Começamos por procurar o Client Hello onde o número aleatório utilizado era o dado no enunciado (52362c11ff0ea3a000e1b48dc2d99e04c6d06ea1a061d5b8ddbf87b001745a27), para isto utilizamos a função de search do wireshark com a opcão de procura por hexadecimal e encontramos o seguinte Client Hello: 

![ClientHello](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/CTF13_1.PNG)

Com isto conseguimos descobrir qual é o primeiro e o último frame correspondente ao procedimento de handshake do TLS que neste caso foram o 814 e o 819 respetivamente:

![CTF13](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/CTF13_2.PNG)

Depois para decobrir a ciphersuite escolhida para a conexão TLS recorremos novamente a search do wireshark desta vez pesquisando por strings em que pesquisamos por "Chipher Suite" e encontramos a seguite no frame "Server Hello":

![CTF13](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/CTF13_3.PNG)

A seguir para saber o total do tamanho dos dados cifrados trocados neste canal procuramos por pacotes com o nome Application Data em que encontramos dois um com tamanho de 80 bytes e outro 1184 bytes logo temos um tamanho total de 1264 bytes:

![CTF13](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/CTF13_4.PNG)

![CTF13](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/CTF13_5.PNG)

O ultímo dado que tivemos de retirar foi o tamanho da mensagem cifrada no handshake que concluí o procedimento de handshake para isso fomos até ao frame 819 e retiramos o tamanho da mensagem que tinha como protocolo "Encrypted Handshake Message" que era 80 bytes :

![CTF13](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/CTF13_6.PNG)

Juntando tudo na estrutura que nos foi dada no enunciado obtivemos a flag: 

flag{814-819-TLS_RSA_WITH_AES_128_CBC_SHA256-1264-80}
