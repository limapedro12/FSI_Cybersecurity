## Task 1
Começamos por lançar os 3 dockers containers, dois containers que irão trocar mensagens(com IPs "10.9.0.5" e "10.9.0.6") e um terceiro que vai servir como atacante(com IP "10.9.0.1"). Para o atacante também poderiamos utilizar a nossa máquina em vez de fazer um novo container (aliás, como o `network_mode` desse container é igual a `host`, tanto o ip como as permissões sobre a rede da nossa máquina e do container serão as mesmas).

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_01.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_02.png)

De seguida, criamos um novo ficheiro "mycode.py" no diretorio partilhado, e copiamos o conteudo do codigo fornecido para o mesmo, de modo a testar se o scapy estava bem instalado.

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_04.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_06.png)

## Task 1.1

### Task 1.1A

Copiamos o codigo fornecido no enunciado para um ficheiro "sniffer.py" e alteramos o nome da interface da rede do codigo dado, para o nome da interface da nossa rede, que para o descobrimos, corremos o comando `ifconfig` e vimos qual era a interface cujo ip é `10.9.0.1`. Depois mudamos o modo para que este se tornasse executavel, com `chmod a+x sniffer.py`.

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_08.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_09.png)

De seguida, corremos o "sniffer.py" com privilegios root, e o programa correu direito, mas como nao havia pacotes na rede ele nao imprimiu nada. Para testar fomos ao host B e deste ping para o host A, e aqui o o sniffer ja mostrou pacotes.

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_13.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_11.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_16_icmp.png)

Corremos o "sniffer.py", mas desta vez sem privilegios root, o que devoldeu um erro de falta de permissões:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_14.png)


### Task 1.1B

É nos pedido para filtrar pacotes no Scapy utilizando a sintaxe BPF (Berkeley Packet Filter). Para isso procuramos na internet e encontramos o seguinte website que nos ensinou como fazer isso:

https://dl.acm.org/doi/fullHtml/10.5555/2642922.2642925

No enunciado é nos pedido que comecemos por filtrar apenas pelos pacotes ICMP. Iss ja é feiro no codigo dado inicialmente, uma vez que ao chamarmos a função `sniff()`, passamos como argumento `filter='icmp'`. Por isso so voltamos a correr o programa como na task 1.1A(utilizando na mesma o ping do host B para o host A)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_16_underlined.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_16_icmp.png)

A seguir é nos pedido que filteremos pelos pacotes TCP que venham de um determinado ip e que tenha como a porta 23 de destino. Decidimos filtrar por pacotes que venham do host B, ou seja, com ip "10.9.0.6". Para isso alteramos, na chamada à função `sniff()`, o `filter` para `'tcp and host 10.9.0.6 and port 23'`. O "sniffer.py" corre, porém não coneguimos testar.

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_17.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_18.png)

Depois é nos pedido que filteremos por pedidos que venham de uma subnet especifica, aconselham a filtrar pela subnet associada à nossa VM, que neste caso é "10.9.0.0/24". Para isso alteramos, na chamada à função `sniff()`, o `filter` para `'net 10.9.0.0/24'`. Para testar fizemos `ping 10.9.0.5` no host B.

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_19.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_20.png)

## Task 1.2

O objetivo desta task é criar um pedido cujo source address é diferente do nosso. Ao enviar um pedido destes, a resposta ao mesmo será enviada para o source IP que definimos anteriormente. Para isso alteramos um pouco as linhas de codigo dadas no enunciado, adicionando `a.src = '10.9.0.6'`, de modo a definir o source IP como "10.9.0.6"(host B). Para saber se a resposta foi enviada para o source IP, como esperado, criamos um novo programa "sniffer2.py" que está atento a todos os pacotes que cheguem ou saiam do IP 10.9.0.6(host B).

Codigo do Scapy para enviar a mensagem:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_21.png)


Codigo do "sniffer2.py":

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_21_2.png)


Pacote que enviamos:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_22.png)


Pacote de resposta:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_23.png)


Podemos fazer o mesmo utilizando o Wireshark:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_24.png)


## Task 1.3

O objetivo desta task é estimar o numero de routers entre a VM e um destino à escolha.
Para isso criamos o seguinte programa em python:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_25_with_numbers.png)

A ideia é enviar um pacote de qualquer tipo, que no nosso caso é do tipo ICMP, com o seu TTL definido a 1 (ponto 1). TTL ou Time-To-Live é o numero máximo de viagens entre 2 routers que um pacote faz na rede até ser descartado. Este pacote é descartado logo no primeiro router, que nos envia uma mensagem de erro com o seu IP. Para obtermos esta informação enviamos o pacote através da função do Scapy `sr1()`, que envia o pacote e retorna a resposta do mesmo (ponto 2).
Depois, vamos incrementando o TTL e enviando novamente os pacotes até chegar ao destino (ponto 5), isto é feito guardando no `current_router` o ultimo IP obtido e quando `current_router == destination_router` saimos do ciclo (ponto 3). Durante este processo vamos recolhendo os IPs devolvidos, guardamos na lista `ip_list`(ponto 4). Com isto podemos descobrir o numero de routers por onde o pacote passou e quais os seus IPs(ponto 6).

Corremos o programa e obtivemos:
![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_26.png)

