## Processo utilizado para resolver o CTF da Semana 11


Começamos por procurar o Client Hello onde o número aleatório utilizado era o dado no enunciado (52362c11ff0ea3a000e1b48dc2d99e04c6d06ea1a061d5b8ddbf87b001745a27), para isto utilizamos a função de search do wireshark com a opcão de procura por hexadecimal e encontramos o seguinte Client Hello: 

![CTF](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/CTF13_1.PNG)

Com isto conseguimos 
