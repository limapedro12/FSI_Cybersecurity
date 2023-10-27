## Processo utilizado para resolver o CTF da Semana 6

Começamos por correr `checksec program`:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/fs_ctf_0.PNG)

Qual é a linha do código onde a vulnerabilidade se encontra?
A analisar o "main.c" vimos que linha de código onde a vulnerabilidade se encontra é a linha 27, onde está escrito `printf(buffer);`, ou seja, o programa tem uma vulnerabilidade de "format string".

O que é que a vulnerabilidade permite fazer?
A vulnerabilidade permite imprimir valores da stack. Com isto podemos aceder à variavel "flag" onde está guardada a nossa flag

Qual é a funcionalidade que te permite obter a flag?
Sabendo que a flag se encontra numa variável global ;) e como no checksec percebemos que o endereços do programa são estáticos, utiliza o gdb para descobrir o endereço de memória da variável onde se encontra a flag.

Atrvés do gdb conseguimos o endreço onde está guardada a flag:


Com o endereço da flag e com a vulnerabilidade que encontraste anteriormente, é possível ler a flag da memória e imprimi-la no stdout. 

![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/fs_ctf_1.PNG)
