###Processo de como resolvemos CTF Semana 3

1 Passo -> Inspecionar o html de http://ctf-fsi.fe.up.pt:5001/, onde encontramos os diversos pulg-ins que a página corria, onde corriam o WooCommerce 2.7.0 e o StoreFront 3.9.1.

2 Passo -> Procurar por exploits relativamente ao WooCommerce (no google "woocommerce wordpress plugin cve login as another user"), depois de alguns sites encontramos https://wpscan.com/vulnerability/b5795f8a-78b8-481e-8522-7bd4689c917c, onde tinha uma vulnerabilidade que explorava o pretendido, sendo ela a CVE-2021-34646.

3 Passo -> Fomos ao site da ctf, inserimos na Semana 3(Wordpress CVE) - Desafio 1 a seguinte flag: flag{CVE-2021-34646}, que estava correta.

4 Passo -> Fomos ao exploitdatabase e procuramos pelo CVE, onde encontramos o script no seguinte link: https://www.exploit-db.com/exploits/50299 e demos download ao exploit.

5 Passo -> Vimos os comentários no script e seguimos os passos, ou seja, fomos ao seguinte link: http://ctf-fsi.fe.up.pt:5001/wp-json/wp/v2/users/1, onde conseguimos confirmar que, de facto, o user 1, era o admin.

6 Passo -> No editor de texto, fizemos "run 50299.py http://ctf-fsi.fe.up.pt:5001/ 1", onde o primeiro argumento era o target, e o segundo era o target id do user.

7 Passo -> Ao correr o comando, no output apareceram links para testar a ver se dava acesso à user account pretendida, e ao inserir este "http://ctf-fsi.fe.up.pt:5001/my-account/?wcj_verified_email=eyJpZCI6IjEiLCJjb2RlIjoiZmQ1OWQzYTY3NGUxZTAyMDUwMWYxNWM1MzViZTg5ZDQifQ, deu acesso à admin account.

8 Passo -> Fomos a http://ctf-fsi.fe.up.pt:5001/wp-admin/edit.php, já loggados como admin, vimos a mensagem privada na post section,clicamos em View, e descobrimos a flag{please don’t bother me}.

9 Passo -> Inserimos a flag na Semana 3 (Wordpress CVE) - Desafio 2, que estava correta.
