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

A seguir é nos pedido que filteremos pelos pacotes TCP que venham de um determinado ip e que tenha como a porta 23 de destino. Decidimos filtrar por pacotes que venham do host B, ou seja, com ip "10.9.0.6". Para isso alteramos, na chamada à função `sniff()`, o `filter` para `'tcp and src host 10.9.0.6 and dst port 23'`. Para testar utilizamos o scapy para gerar e enviar um pacote TCP com IP de origem "10.9.0.6", IP de destino "10.9.0.5" e porta de destino "23".

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_17.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_18.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_18_1.png)

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


## Task 1.4

Nesta task temos que fazer um programa que faça sniff a pacotes ICMP, e gere um echo reply para os mesmos, independentemente dos seus IPs de destino. Logo, quando um dispositivo utilizar o comando `ping` para um IP X, independentemente se o dispositivo X está vivo ou não, o `ping` irá receber sempre uma resposta a indicar que X está vivo. 

Para fazer este programa, começamos por escrever o sguinte codigo, no ficheiro "fake_ping.py":

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_29.png)

Para o testar, corremos o programa "fake_ping.py" num terminal da nossa VM e abrimos o terminal do host B e corremos:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_28.png)

Porém ao contrário do que estavamos à espera, o `ping` não recebeu os pacotes de resposta gerados. Para tentar resolver o problema, corremos novamente o "fake_ping.py" e `ping 1.2.3.4`, mas desta vez realizamos também uma captura no wireshark, e vimos que o programa "fake_ping.py" envia pacotes do tipo "Type: 8 (Echo (ping) request)", quando deveria enviar "Type: 0 (Echo (ping) reply)". Além disso, o ID do "Echo (ping) reply" deve ser igual ao ID do respetivo "Echo (ping) request", o que nao está a acontecer, e como é um pedido de "Echo", a resposta deve enviar a mesma raw data que recebeu.

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_33.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_34.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_35.png)

Alteramos o programa, corrigindo os erros:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_36.png)

Corremos novamente e obtivemos o seguinte:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_37.png)

Como podemos ver nas respostas ping aparece "DUP!". Fomos à procura de uma solução e comparando com outros `ping` verdadeiros, reparamos no wireshark que a "seq" das respostas é sempres "0/0", quando deveria ser igual ao "seq" do "Echo (ping) request" correspondente. Para corrigir adicionamos `b.seq = pkt[ICMP].seq`:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_38.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_39.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_40.png)

De seguida, fizemos o mesmo, mas para o ip "10.9.0.99":

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_41.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_42.png)

Aqui tivemos um erro diferente, onde o dispositivo que está a fazer `ping`, envia um ARP request broadcast, mas não obtem resposta e por isso ele nem sequer envia o pacote ICMP. Nos dispositivos existe uma tabela de ARP, que traduz endereços de IP para endereços MAC. Para preencher esta tabela, utiliza ARP packets. Quando um dispositivo quer comunicar com o dispositivo com um determinado IP, mas este não consta na sua tabela, o dispositivo envia uma ARP request broadcast message com o IP desejado para toda a rede e o dispositivo que tiver o endereço de IP desejado envia um ARP reply packet com o seu endereço MAC. Por isso, tivemos que alterar o nosso programa para quando detetar ARP request broadcast e enviar o nosso MAC address como resposta.

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_43.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_44.png)

Depois fizemos o mesmo, mas para o ip "8.8.8.8":

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_45.png)

Como o IP "8.8.8.8", existe realmente, o ping recebe duas respostas, a verdadeira e a nossa, e por isso o segundo aparece como "DUP!".
