## Processo utilizado para resolver o CTF da Semana 8

Começamos por analisar o codigo fornecido no enunciado e vimos que o codigo é suscetivel a ataque de SQL Injection, uma vez que não utiliza prepared statements nem sanitização de input:

![imagem](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_sqli_1.png)

De seguida introduzimos o mesmo que introduzimos na task 2 do Guião da Semana 8, ou seja, username `Admin; -- ` e password vazia. A introduzir isto obtivemos a seguinte mensagem:

![imagem](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_sqli_5.png)

Por isso introduzimos uma password qualquer, que neste caso foi `1234`:

![imagem](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_sqli_6.png)

Com isto não conseguimos entrar na página pretendida. Pensamos então que a o utilizador poderia estar tudo em letras minusculas, como é habitual:

![imagem](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_sqli_3.png)

Aconteceu-nos o mesmo erro então voltamos a introduzir a password `1234`:

![imagem](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_sqli_2.png)

Assim conseguimos acesso à página de administrador e consequentemente à flag.

![imagem](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/ctf_sqli_4.png)
