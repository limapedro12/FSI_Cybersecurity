## Processo utilizado para resolver o CTF da Semana 5
### Desafio 1

A ideia é, no scanf inserirmos uma mensagem maior que o tamanho do buffer e isto resultará no nosso overflow. Como o array com o nome do ficheiro a ser lido(`meme_file`) está a ser instanciado logo acima do `buffer`, ao dar overflow no `buffer`, estaremos a escrever por cima do `meme_file`, e assim podemos mudar o nome do ficheiro que irá ser lido.
Como sabemos que o buffer tem tamanho de 32 bytes, basta inserir uma mensagem com 32 caracteres(não importa quais), seguido de "flag.txt".
Para conseguir a flag, mudamos a linha 14 do ficheiro "exploit-example.py" para `r.sendline(b"aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaflag.txt")`. Corremos o programa e conseguimos a flag.

###Desafio 2

Localmente:
`aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaflag.txt\0\x24\x23\xfc\xfe`
porque o canário("val") está declarado antes de "meme_file" e de "buffer".

No servidor:
`aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa\x24\x23\xfc\xfeflag.txt\0`
porque o canário("val") deve estar declarado entre "meme_file" e "buffer".
