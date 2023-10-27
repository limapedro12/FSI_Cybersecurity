## Processo utilizado para resolver o CTF da Semana 6

Começamos por correr `checksec program`:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/fs_ctf_0.png)

Com isto percebemos que existe um canário na stack(`Canary found`) e que a stack não permissões de execução(`NX enabled`), mas os endereços do programa são estáticos(`No PIE (0x8048000)`), logo como flag é uma variável global, se encontramos o endereço da flag no gdb, saberemos sempre qual será o endereço da flag.

A analisar o "main.c" vimos que linha de código onde a vulnerabilidade se encontra é a linha 27, onde está escrito `printf(buffer);`, ou seja, o programa tem uma vulnerabilidade de "format string".

A vulnerabilidade permite imprimir valores da stack. Com isto podemos aceder à variável "flag" onde está guardada a nossa flag

Através do gdb conseguimos o endereço onde está guardada a flag:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/fs_ctf_01.png)
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/fs_ctf_02.png)

Através do "scanf()" poderemos inserir uma string com o endereço da flag, e a seguir podemos ler esse endereço com "%s" e assim ter acesso à flag.
Para isso, fomos ver quantos "%x" precisamos de inserir para ter acesso que acabamos de escrever. Para isso corremos o seguinte programa:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/fs_ctf_03.png)

E obtivemos:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/fs_ctf_04.png)

Com isto podemos ver que o padrão "616161" aparece logo com o primeiro "%x". Ou seja, podemos colocar logo o "%s" para ler a flag.
Corremos o seguinte programa para obter a flag localmente:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/fs_ctf_05.png)
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/fs_ctf_06.png)

E corremos o mesmo programa mas com `LOCAL = FALSE` e assim conseguimos obter a flag no servidor:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/fs_ctf_07.png)
