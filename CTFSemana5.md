## Processo utilizado para resolver o CTF da Semana 5
### Desafio 1

A ideia é, no scanf inserirmos uma mensagem maior que o tamanho do buffer e isto resultará no nosso overflow. Como o array com o nome do ficheiro a ser lido(`meme_file`) está a ser instanciado logo acima do `buffer`, ao dar overflow no `buffer`, estaremos a escrever por cima do `meme_file`, e assim podemos mudar o nome do ficheiro que irá ser lido.
Como sabemos que o buffer tem tamanho de 32 bytes, basta inserir uma mensagem com 32 caracteres(não importa quais), seguido de "flag.txt".
Para conseguir a flag, mudamos a linha 14 do ficheiro "exploit-example.py" para `r.sendline(b"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaflag.txt")`. Corremos o programa e conseguimos a flag.

###Desafio 2

Primeiro abrimos o ficheiro example-exploit.py e trocamos a parta por ...
Primeiro corremos o ficheiro "program" localmente e inserimos a mensagem utilizada no exercicio anterior e devolveu-nos o seguinte:
....
Depois ao analisar o codigo em ... .c vimos que agora existe no programa um canário, ou seja, foi declarado um array chamado de "val" antes de "meme_file" e que o programa verifica se o seu conteudo é 0xfefc2324, ou seja no array, [0x24, 0x23, 0xfc, 0xfe]. Então escrevemos k seguinte programa em python para nos criar a mensagem necessária:
```
with open('badfile', 'wb') as f:
  f.write(b"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaflag.txt\0\x24\x23\xfc\xfe")
```
Corremos o program em python e obtivemos:
....
Depois corremos "program" e introduzimos a mensagem anterior.

E assim obtivemos a `...`

De seguida alteramos o ficheiro "example-exploit.py", mudando a porta para ... e a mensagem para a mensgem anterior. Porém foi devolvido o seguinte:
....
Com isto pensamos que o canário("val") poderia estar declarado entre "meme_file" e "buffer". Logo tentamos escrever a seguinte mensagem:
"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa\x24\x23\xfc\xfeflag.txt\0". E assim, o programa devolveu-nos a flag.
