## Processo utilizado para resolver o CTF da Semana 5
### Desafio 1

Analisamos o código em "main.c" e vimos que o programa lê um ficheiro o ficheiro "mem.txt", por isso se conseguirmos mudar este ficheiro para "flag.txt" conseguiremos ter acesso à flag.

A ideia é, no scanf inserirmos uma mensagem maior que o tamanho do buffer e isto resultará no nosso overflow. Como o array com o nome do ficheiro a ser lido(`meme_file`) está a ser instanciado logo acima do `buffer`, ao dar overflow no `buffer`, estaremos a escrever por cima do `meme_file`, e assim podemos mudar o nome do ficheiro que irá ser lido.
Como sabemos que o buffer tem tamanho de 32 bytes, basta inserir uma mensagem com 32 caracteres(não importa quais), seguido de "flag.txt".

Corremos "program" e inserimos "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaflag.txt". O programa devolveu-nos `flag_placeholder`.

Para conseguir a flag, temos que fazer o mesmo mas no servidor, para isso, mudamos a linha 14 do ficheiro "exploit-example.py" para `r.sendline(b"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaflag.txt")`. Corremos o programa e conseguimos a flag para o Desafio 1.

### Desafio 2

Correndo `checksec program` vimos que não houve alterações nas permissões do executável.

Primeiro corremos o ficheiro "program" localmente e inserimos a mensagem utilizada no exercicio anterior e devolveu-nos o seguinte:
```
You gave me this flag.txt and the value was 0xdeadbeef. Disqualified!
```
Depois ao analisar o codigo em main.c vimos que agora existe no programa um canário, ou seja, foi declarado um array chamado de "val" antes de "meme_file" e que o programa verifica se o seu conteudo é 0xfefc2324( em formato de array, [0x24, 0x23, 0xfc, 0xfe]). 

Estas modificações não resolveram o problema do programa.

Para as ultrapassar, mudamos no ficheiro "exploit-example.py" a messagme para "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaflag.txt\0\x24\x23\xfc\xfe" (passando a estar escrito `r.sendline(b"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaflag.txt\0\x24\x23\xfc\xfe")`) e definimos a variavel DEBUG a True. Corremos o "exploit-example.py" em python e obtivemos `flag_placeholder`.

De seguida alteramos o ficheiro "example-exploit.py", mudando a porta para 4000 (passando a estar escrito `r = remote('ctf-fsi.fe.up.pt', 4000)`) e definimos DEBUG a False. Corremos "example-exploit.py", porém foi devolvido o seguinte:
```
You gave me this .txt and the value was 0x67616c66. Disqualified!
```
Com isto pensamos que o canário("val") poderia estar declarado entre "meme_file" e "buffer". Logo tentamos escrever a seguinte mensagem:
"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa\x24\x23\xfc\xfeflag.txt\0"

Corremos "example-exploit.py" e o programa devolveu-nos a flag.
