O programa crasha porque causa do "%n". O "%n" recebe um argumento que deve ser um apontador para um int, mas em vez de imprimir qualquer coisa ele armazena o número de caracteres impressos até agora por esta chamada naquele local. No nosso caso escrevemos doze "%.8x", que vão lendo o conteúdo da stack. Depois o programa chega ao "%n" e como o conteúdo que está nesse local da stack não é um apontador para um int, o programa crasha. 
 
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
Aqui temos procuramos pelo padrão "61616161" corresponde ao início da string("aaaa") em hexadecimal. Fomos tentando diminuir o número de "%x" que escrevíamos no "build_string.py", e concluímos que necessitamos de colocar 63 "%x" antes de colocarmos o "%x" que irá ler a nossa string. Para comprovar alteramos o "build_string.py" para o seguinte: 
![build_string.py](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_13.png)
Correndo obtivemos: 
![Resultado](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/format_string_14.png)
 
 
