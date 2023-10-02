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

### Task 6
