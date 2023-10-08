### Task 1
Corremos os seguintes comandos na shell:
- _printenv_ e _env_: este comando imprime a lista de todas as variaveis de ambiente da shell
Por exemplo quando corremos _printenv PWD_ a shell imprime "/home/pedropassos/Labsetup", que é diretorio onde estamos a trabalhar.
- _export_ e _unset_: serve para mudar as variaveis de ambiente

### Task 2
Corremos o myprintenv.c como nos foi dado salvando o output para o "file1", e depois corremos comentando o o primeiro _printenv()_ e descomentando o segundo _printenv()_, como pedido no Step 2, salvando o output para o "file2". Por fim  comparamos o "file1" e o "file2", usando o comando _diff_ e descobrimos que não havia diferença entre os 2 ficheiros. Com isto concluimos que ao executar um _fork()_, o processo filho tem as mesmas variaveis de ambiente do seu pai.

### Task 3
Primeiro executamos o programs myenv.c como nos foi dado. A seguir, mudamos a linha pedida no guião para _execve("/usr/bin/env", argv, environ);_. _environ_ é uma variavel externa herdada do processo pai. Ou seja, ao correr o programa alterado, imprimimos as variaveis de ambiente do processo pai, que neste caso é a shell.

### Task 4
Ao compilarmos e corrermos o codigo dado imprimimos as variaveis de ambiente do processo pai. Com isto comfirmamos que as variaveis são passadas para o novo programa.

### Task 5
Em primeiro lugar compilamos (dando o nome de "foo") e corremos na shell o codigo C dado no enunciado. Isto imprimiu as variaveis de ambiente do processo atual.
A seguir, corremos os seguintes comandos:
- `sudo chown root foo` alterando a propriedade para root.
- `sudo chmod 4755 foo` de modo a torná-lo Set-UID.
- `export PATH="/home/FSI/Labsetup:$PATH"` de modo a definir a variável de ambiente PATH para o diretorio "/home/FSI/Labsetup".
- `export LD_LIBRARY_PATH="/your/new/library/path:$LD_LIBRARY_PATH"` de modo a definir a variável de ambiente LD_LIBRARY_PATH para o diretório "/home/FSI/Labsetup".
- `export MY_VARIABLE="Hello World"` de modo a definir a variável de ambiente MY_VARIABLE para "Hello World".

Depois corremos o programa Set-UID ("foo") que imprimiu as variaveis de ambiente. Dentro das variáveis estavam todas as que definimos menos LD_LIBRARY_PATH. Com isto concluimos que as variáveis PATH e MY_VARIABLE foram herdadas pelo processo filho do Set-UID.



### Task 6
Em primeiro lugar compilamos (dando o nome de "task6" ao ficheiro binário) e corremos na shell o codigo C dado no enunciado, sem fazer nenhuma alteração à variavel de ambiente _PATH_. Isto imprimiu os ficheiros do diretorio atual. 
A seguir, corremos os seguintes comandos:
- `sudo ln -sf /bin/zsh /bin/sh` como estava indocado no guião
- `sudo chown root task6`, de modo a definir o _root_ como owner do ficheiro
- `sudo chmod 4755 task6`, de modo a definir o ficheiro como um "Set-UID program"
- `export PATH=/home/seed/Desktop/FSI`, de modo a redifinir a localização do "bin" para a nossa localizacao atual.
Depois corremos o task6 outra vez e a shell devolveu-nos `zsh:1: command not found: ls `.
Por isso criamos um novo ficheiro ao qual chamamos ls.c e escrevemos o seguite codigo:
```c
#include <stdio.h>
#include <unistd.h>

int main() {
    printf("I did it!!! :)\n");
    printf("%d\n", geteuid());
    return 0;
}


```
Depois compilamos o código, dando ao ficheiro binário o nome de "ls".
Corremos novamente o task6 e o programa correu o codigo que acabamos de escrever.
Com isto também descobrimos que o código não está a correr com privilégios _root_, uma vez que imprimimos o effective user ID (com a chamada a geteuid()) e este imprimiu 1000, que é diferente do ID do _root_, que é 0.
