## Task 1
Começamos por lançar os 3 dockers containers, dois containers que irão trocar mensagens(com IPs "10.9.0.5" e "10.9.0.6") e um terceiro que vai servir como atacante(com IP "10.9.0.1"). Para o atacante também poderiamos utilizar a nossa máquina em vez de fazer um novo container (aliás, como o `network_mode` desse container é igual a `host`, tanto o ip como as permissões sobre a rede da nossa máquina e do container serão as mesmas).

![](sn_sp_01.png)

![](sn_sp_02.png)

De seguida, criamos um novo ficheiro "mycode.py" no diretorio partilhado, e copiamos o conteudo do codigo fornecido para o mesmo, de modo a testar se o scapy estava bem instalado.

![](sn_sp_04.png)

![](sn_sp_06.png)


## Task 1.1
Copiamos o codigo fornecido no enunciado para um ficheiro "sniffer.py" e alteramos o nome da interface da rede do codigo dado, para o nome da interface da nossa rede, que para o descobrimos, corremos o comando `ifconfig` e vimos qual era a interface cujo ip é `10.9.0.1`. Depois mudamos o modo para que este se tornasse executavel, com `chmod a+x sniffer.py`.

![](sn_sp_08.png)

![](sn_sp_09.png)

De seguida, 

https://dl.acm.org/doi/fullHtml/10.5555/2642922.2642925
