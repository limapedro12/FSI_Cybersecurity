### Task 1
Primeiro corremos os seguintes comandos no terminal:
 - `sudo sysctl -w kernel.randomize_va_space=0`
 - `sudo ln -sf /bin/zsh /bin/sh`

Compilamos o ficheiro "call_shellcode.c" usando o makefile como foi pedido e dois binarios foram criados "a32.out" e "a64.out".
Ao corrermos estes binarios ambos executaram uma simples shell sem diferença de comportamento entre eles.

### Task 2
No enunciado é nos apresentado um código em C suscetivel a ataques de buffer overflow. Poderemos tirar partido do `strcpy(buffer, str)`, que irá inserir no "buffer" a informação que está em "str", mesmo que "str" seja maior que o "buffer", e se assim for irá ocorrer um buffer overflow. Com isto poderemos escrever por cima do "RET", e assim mudando o endereço para o qual a função, após ser executada, irá retornar, fazendo-a retornar para o nosso código malicioso.


### Task 3
Para começar começamos por copiar o conteudo do diretorio "category-software/Buffer_Overflow_Setuid" para um diretorio na área de trabalho. De seguida corremos o ficheiro makefile que estava dentro da pasta "Labsetup/code", que nos gerou 8 ficheiros: stack-L1, stack-L1-dbg, stack-L2, stack-L2-dbg stack-L3, stack-L3-dbg stack-L4, stack-L4-dbg. 

Depois de analisarmos o codigo em stack.c, concluimos que para correm o programa precisariamos de um ficheiro chamado "badfile" no mesmo diretorio que o executavel. Entao criamos um ficheiro vazio com o comando `touch badfile`.

De seguida corremos o debugger com o comando `gdb stack-L1-dbg`. Aqui na consola do gdb, criamos um breakpoint no inicio da função "bof", escrevendo `b bof`. De seguida corremos o programa, com`run` e escrevemos `next` para avançar para a instrução seguinte, que neste caso é `strcpy(buffer, str);`, onde poderá acontecer um buffer overflow. Aqui imprimimos o endereço do "ebp" e do "buffer", correndo `p $ebp`, que nos devolveu `$1 = (void *) 0xffffcaa8` e`p &buffer`, que nos devolveu `$2 = (char (*)[100]) 0xffffca3c`. Depois disto calculamos a diferença entre os dois endereços que é 108. Com isto já podemos sair do debugger.

Depois alteramos o ficheiro "exploit.py", já fornecido. 

Substituimos o codigo na variavel "shellcode" pelo shellcode de 32 bit presente no ficheiro call_shellcode.c da pasta "Labsetup/shellcode".

Definimos "start" como 300.

Definimos "offset" a diferença entre o &buffer e a localização do RET, que é a instrução acima do $ebp, logo definimos "offset" como a difereça calculada anteriormente mais 4, ou seja, 108 + 4 = 112.

A ideia é fazer com que a função retorne para um endereço entre o local onde está guardado o endereço de retorno(que é o endereço de ebp + 4) e o shellcode, para que assim o programam corra o shellcode fornecido. Por isso decidimos optar por uma abordagem de tentativa e erro. Se o valor for muito baixo obteremos:
 ```
 Input size: 517
 Illegal instruction
```
Se o valor for muito alto obteremos:
 ```
 Input size: 517
 Segmentation fault
```
Com isto, começamos por fazer com que a função retorne para o endereço de "ebp" mais 100, ou seja, definimos no "exploit.py" a variavel "ret" como o endereço de "ebp"(0xffffcab8) mais 100 .
 
 De seguida corremos o programa "exploit.py" e corremos "stack-L1" e obtivemos:
 ```
 Input size: 517
 Illegal instruction
```
Então concluimos que o endereço de retorno é demasiado baixo. Assim mudamos "ret" para `0xffffcab8 + 200`. Corremos novamente "exploit.py" e "stack-L1" e conseguimos acesso à root shell.


### Task 4
Nesta task não sabemos o tamanho exato do buffer, apenas que está entre 100 e 200 bytes.
Primeiro corremos o ficheiro makefile que estava dentro da pasta "Labsetup/code", que nos gera 8 ficheiros: stack-L1, stack-L1-dbg, stack-L2, stack-L2-dbg stack-L3, stack-L3-dbg stack-L4, stack-L4-dbg. 

Depois criamos um ficheiro vazio com o comando `touch badfile`.

De seguida corremos o debugger com o comando `gdb stack-L2-dbg`. Aqui na consola do gdb, criamos um breakpoint no inicio da função "bof", escrevendo `b bof`. De seguida corremos o programa, com`run` e escrevemos `next` para avançar para a instrução seguinte, que neste caso é `strcpy(buffer, str);`, onde poderá acontecer um buffer overflow. Aqui imprimimos o endereço o "buffer", correndo `p &buffer`, que nos devolveu `$1 = (char (*)[160]) 0xffffca10`. Ao contrário dos ataques anteriores, não podemos imprimir o endereço de "ebp". Depois disto calculamos a diferença entre os dois endereços que é 108. Com isto já podemos sair do debugger.

Depois no ficheiro "exploit.py":
 - substituimos o codigo na variavel "shellcode" pelo shellcode de 32 bit presente no ficheiro call_shellcode.c da pasta "Labsetup/shellcode"
 - comentamos a variavel "start" e substituimos a linha seuinte por `content[517 - len(shellcode):] = shellcode`, de modo a colocar o nosso shellcode no fim do ficheiro badfile.
 - definimos "ret" como o endereço do "buffer"(0xffffca10) mais 300
 - comentamos a variavel "offset" e substituimos a linha que contém `content[offset:offset + L] = (ret).to_bytes(L,byteorder='little') ` por:
 ```py
for offset in range(50):
	content[offset*L:offset*4 + L] = (ret).to_bytes(L,byteorder='little') 
 ```
 Fizemos isto porque como não sabemos exatamente o tamanho do buffer, precisamos de utilizar a técnica de Spray, ou seja, temos que colocar o nosso endereço de retorno em vários sitios, de modo a tentar acertar no correto.
 
 De seguida corremos o programa "exploit.py" e corremos "stack-L2" e e conseguimos acesso à root shell.
