## Tarefa 1

Tal como diz no enunciado, começamos por escrever o seguinte comando:

![No randomize Command](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_01.png)

De seguida corremos o contentor docker(escrevendo `dcbuid` e `dcup`) e escrevemos no terminal o seguinte:

![echo hello to the container](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_02.png)

Obtendo:

![echo hello to the container](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_03.png)

De seguida corremos o programa em python, chamado "build_string.py" que nos é dado nos ficheiros do SEED Labs:

![echo hello to the container](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_04.png)

Ao correr isto a função "my_printf()" crasha como refere no enunciado:

![echo hello to the container](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_05.png)

O programa crasha por causa do "%n". O "%n" recebe um argumento que deve ser um apontador para um int, mas em vez de imprimir qualquer coisa ele armazena o número de caracteres impressos até ao momento. No nosso caso, primeiro escrevemos doze "%.8x", que vão lendo o conteúdo da stack. Depois o programa chega ao "%n" e como o conteúdo que está nesse local da stack não é um apontador para um int, o programa crasha. 
 

## Tarefa 2.A

Inicialmente tentamos escrever: 

![Primeiro echo](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_08.png)

E obtivemos:

![Resultado do Primeiro echo](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_09.png)

Despois concluímos que iria ser necessário mais "%x" do que inicialmente achávamos. 
Por isso alteramos o ficheiro "build_string.py" para: 

![build_string.py](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_10.png)

Assim imprimindo o conteúdo dos primeiros 100 endereços da nossa stack atual. Corremos o seguinte: 

![Correr build_string.py](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_11.png)

E obtivemos: 

![Resultado](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_12.png)
![Resultado](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_12_1.png)

Aqui temos procuramos pelo padrão "61616161" corresponde ao início da string("aaaa") em hexadecimal. Como o padrão "616161" tem o número 64, concluímos que necessitamos de colocar 63 "%x" antes de colocarmos o "%x" que irá ler a nossa string. Para comprovar alteramos o "build_string.py" para o seguinte: 

![build_string.py](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_13.png)

Correndo obtivemos: 

![Resultado](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_14.png)
 
## Tarefa 2.B

Sabemos que o segredo está guardado no endereço "0x080b4008", logo temos que escrever este endereço no inicio da string, e depois utilizando vários "%x" avaçamos os elementos na stack, até chegar ao início da string, onde está guardado o endereço do segredo. Lemos este endereço utilizando o "%s", que nos irá levar ao segredo. O "%s" recebe um apontador para o primeiro caracter de uma string e imprime essa string. Assim, ao lermos o endereço do segredo com %s, estaremos a imprimir o segredo.

Para executar esta estratégia, modificamos novamente o ficheiro "build_string.py":

![build_string.py](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_15.png)

Corremos os seguintes comandos:

![Correr build_string.py](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_11.png)

E obtivemos o seguinte:

![Resultado](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_16.png)

## Tarefa 3.A

Nesta task, temos como objetivo alterar o valor da "target variable", a partir do output que obtemos do servidor, conseguimos saber o seu address:

![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_17.png)

Sendo ele: 0x080e5068.

Nós sabemos que conseguirmos aceder ao nosso input na stack, como pudemos ver nas tasks anteriores. O que temos a fazer é aceder ao nosso input na stack, que vai ter de ser o address da variable, e com o %n, que usamos também em tasks anteriores para crashar o programa, alterar o valor da variável.

![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_18.png)

Alterando o "number" para o endereço da variável, e depois fazendo %x*63 + %n, o %n refere-se já ao endereço da variável, alterando o valor da mesma com o número de bytes impressos até aí. Executamos o script que nos dá o badfile:

![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_19.png)

E em seguida enviamos o badfile para o servidor correr. Como podemos observar o valor da target variable foi alterado com sucesso:

![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_20.png)



## Tarefa 3.B

Esta task tem o mesmo conceito que a anterior, no entanto temos que alterar para um valor específico, sendo ele: 0x5000, que traduz para 20480 em decimal. Ao dividir 20480/63 = 325, por efeitos de simplicidade metemos o seguinte:

![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_21.png)

Reparamos então que o valor da variável foi mudado para:

![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_22.png)

Como necessitamos de ajustar o valor, é necessário adicionar um %x em que possamos definir um número mais exato de bytes a adicionar. Chegamos à conclusão que este era o valor para que a variável fosse alterada corretamente:

![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_23.png)

Analisando o código podemos ver que reduzimos de 63 para 62 %x, e em seguida metemos o %x que faltava, mas com um valor de bytes ajustado para que desse 0x5000:

![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_24.png)
