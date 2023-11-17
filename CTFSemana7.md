## Processo utilizado para resolver o CTF da Semana 7

### Desafio 1

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

### Desafio 2

Quando corremos o checksec outra vez no programa, reparamos que continua com a mesma vulnerabilidade.
Olhando para o código do main.c, reparamos que se conseguirmos alterar o valor da variável global "key", para 0xBEEF, ou seja, 48879 em decimal, conseguimos ativar uma backdoor que nos dá controlo do servidor.

![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/fs_ctf_08.png)

Para alterarmos o valor da "key", precisamos de primeiro obter o endereço da variável, por isso fazemos o seguinte:

![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/fs_ctf_09.png)

Recorrendo ao gdb, obtemos o endereço da "key", sendo ele: 0x0804b324. Agora já temos os dados necessários para realizar o exploit. Primeiramente, retiramos a linha de código do exploit_example.py `p.recvuntil("got:")`, de forma a que o script não interrompa a sua execução.

Sabendo que através do %n, nós conseguimos alterar o valor da variável, sabendo o endereço dela, temos agora de calcular quantos bytes é que têm de ser imprimidos para o valor ser igual a 0xBEEF, ou seja, 48879 em decimal.

![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/fs_ctf_10.png)

Dando primeiramente, um garbage address, por motivos de padding, e em seguida o endereço da key, mas com os bytes na ordem inversa, de forma a que seja lido corretamente da stack, calculamos que até ali demos 8 bytes, 4 do garbage address, 4 do address da key, ficamos com: 48879 - 8 = 48871. 

Chegando à conclusão que precisamos ainda de imprimir 48871 bytes, fazendo isso através do %.Nx, com N = 48871 e em seguida, adicionamos o %n, para escrever esse número de bytes no endereço fornecido. 

Correndo o script:

![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/fs_ctf_10.png)

Verificamos que o programa imprime o garbage address, mas reescreve o endereço da key, ativando assim a backdoor. Fazendo `cat flag.txt`, obtemos assim a flag.

