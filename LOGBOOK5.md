### Task 1
Primeiro corremos os seguintes comandos no terminal:
 - `sudo sysctl -w kernel.randomize_va_space=0`
 - `sudo ln -sf /bin/zsh /bin/sh`

Compilamos o ficheiro "call_shellcode.c" usando o makefile como foi pedido e dois binarios foram criados "a32.out" e "a64.out".
Ao corrermos estes binarios ambos executaram uma simples shell sem diferença de comportamento entre eles.

### Task 2
Para começar começamos por copiar o conteudo do diretorio "category-software/Buffer_Overflow_Setuid" para um diretorio na área de trabalho. De seguida corremos o ficheiro makefile que estava dentro da pasta "Labsetup/code", que nos gerou 8 ficheiros: stack-L1, stack-L1-dbg, stack-L2, stack-L2-dbg stack-L3, stack-L3-dbg stack-L4, stack-L4-dbg. 
Depois de analisarmos o codigo em stack.c, concluimos que para correm o programa precisariamos de um ficheiro chamado "badfile" no mesmo diretorio que o executavel. Entao criamos um ficheiro vazio com o comando `touch badfile`.
De seguida corremos o debugger com o comando `gdb stack-L1-dbg`. Aqui na consola do gdb, criamos um breakpoint no inicio da função "bof", escrevendo `b bof`. De seguida corremos o programa, com`run` e escrevemos `next` para avançar para a instrução seguinte, que neste caso é `strcpy(buffer, str);`, onde poderá acontecer um buffer overflow. Aqui imprimimos o endereço do "ebp" e do "buffer", correndo `p $ebp`, que nos devolveu `$1 = (void *) 0xffffcab8` e`p &buffer`, que nos devolveu `$2 = (char (*)[100]) 0xffffca4c`. Depois disto calculamos a diferença entre os dois endereços que é 108. Com isto já podemos sair do debugger.
Depois no ficheiro "exploit.py":
 - substituimos o codigo na variavel "shellcode" pelo shellcode de 32 bit presente no ficheiro call_shellcode.c da pasta "Labsetup/shellcode"
 - definimos "start" como 300
 - definimos "ret" como o endereço de "ebp"(0xffffcab8) mais 100
 - definimos offset como a difereça calculada anteriormente mais 4, ou seja, 108 + 4 = 112.
 De seguida corremos o programa "exploit.py" e corremos "stack-L1" e obtivemos:
 ```
 Input size: 517
 Illegal instruction
```
Então no "exploit.py" mudamos "ret" para `0xffffcab8 + 200`. Corremos novamente "exploit.py" e "stack-L1" e obtivemos a mesma resposta. Mudamos novamente "ret" para `0xffffcab8 + 150` e corremos "stack-L1" e obtivemos a mesma resposta. Ao fim de algumas tentativas chegamos a `ret = 0xffffcab8 + 50` e ao correr o "stack-L1" conseguimos acesso à root shell.


### Task 3


### Task 4
