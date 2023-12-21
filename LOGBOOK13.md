## Task 1

Começamos por lançar os 3 docker containers, dois containers que irão trocar mensagens(com IPs "10.9.0.5" e "10.9.0.6") e um terceiro que vai servir como atacante(com IP "10.9.0.1"). Apesar da recomendação de usar o terceiro docker container como atacante decidimos utilizar o host como atacante.

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_01.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_02.png)

De seguida, criamos um novo ficheiro "mycode.py" no diretório partilhado, e copiamos o conteúdo do código fornecido para o mesmo, de modo a testar se o Scapy estava bem instalado.

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_04.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_06.png)

## Task 1.1

### Task 1.1A

Copiamos o código fornecido no enunciado para um ficheiro "sniffer.py", alterando o nome da interface da rede. Para descobrimos o nome da interface da nossa rede corremos o comando `ifconfig` e vimos qual era a interface cujo ip é "10.9.0.1" Depois mudamos o modo do ficheiro para que este se tornasse executável, com `chmod a+x sniffer.py`.

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_08.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_09.png)

De seguida, corremos o "sniffer.py" com privilégios root, mas como não havia pacotes na rede ele não imprimiu nada. Para testar, fomos ao host B e fizemos `ping` para o host A, e aqui o "sniffer.py" já mostrou pacotes.

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_13.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_11.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_16_icmp.png)

Corremos o "sniffer.py", mas desta vez sem privilégios root, o que devolveu um erro de falta de permissões:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_14.png)


### Task 1.1B

É nos pedido para filtrar pacotes no Scapy utilizando a sintaxe BPF (Berkeley Packet Filter). Para isso procuramos na internet e encontramos o seguinte website que nos ensinou como fazer isso:

https://dl.acm.org/doi/fullHtml/10.5555/2642922.2642925

No enunciado é nos pedido que comecemos por filtrar apenas pelos pacotes ICMP. Isto já é feiro no código dado inicialmente, uma vez que na chamada à função `sniff()` passa-se como argumento `filter='icmp'`. Por isso, apenas voltamos a correr o programa como na task 1.1ª (utilizando na mesma o `ping` do host B para o host A).

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_16_underlined.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_16_icmp.png)

A seguir é nos pedido para filtrar por pacotes TCP que venham de um determinado ip e que tenha como a porta 23 de destino. Decidimos filtrar por pacotes que venham do host B, ou seja, com ip "10.9.0.6". Para isso alteramos, na chamada à função `sniff()`, o `filter` para `'tcp and src host 10.9.0.6 and dst port 23'`. Para testar utilizamos o Scapy para gerar e enviar um pacote TCP com IP de origem "10.9.0.6", IP de destino "10.9.0.5" e porta de destino "23".

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_17.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_18.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_18_1.png)

Por fim, é nos pedido para filtrar por pedidos que venham de uma subnet especifica, aconselhando a filtrar pela subnet associada à nossa VM, que neste caso é "10.9.0.0/24". Para isso alteramos, na chamada à função `sniff()`, o `filter` para `'net 10.9.0.0/24'`. Para testar fizemos `ping` do host B para o host A.

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_19.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_20.png)

## Task 1.2

O objetivo desta task é criar um pedido cujo endereço IP de origem seja diferente do nosso. Ao enviar um pedido destes, a resposta ao mesmo será enviada para o endereço IP que definimos anteriormente. Para isso alteramos um pouco as linhas de código dadas no enunciado, adicionando `a.src = '10.9.0.6'`, de modo a definir o IP de origem como "10.9.0.6"(host B). Para saber se a resposta foi enviada para o IP de origem, como esperado, criamos um novo programa "sniffer2.py" que está atento a todos os pacotes que cheguem ou saiam do IP 10.9.0.6(host B).

Código que utiliza o Scapy para enviar a mensagem:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_21.png)


Código do "sniffer2.py":

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_21_2.png)


Pacote que enviamos:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_22.png)


Pacote de resposta:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_23.png)


Podemos fazer o mesmo utilizando o Wireshark:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_24.png)


## Task 1.3

O objetivo desta task é estimar o número de routers entre a VM e um destino à escolha. 

Para isso criamos o seguinte programa em Python:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_25_with_numbers.png)

A ideia é enviar um pacote de qualquer tipo, que no nosso caso é do tipo ICMP, com o seu TTL definido a 1 (ponto 1). TTL ou Time-To-Live é o número máximo de viagens entre 2 routers que um pacote faz na rede até ser descartado. Este pacote é descartado logo no primeiro router, que nos envia uma mensagem de erro com o seu IP. Para obtermos esta informação enviamos o pacote através da função do Scapy `sr1()`, que envia o pacote e retorna a resposta do mesmo (ponto 2). Depois, vamos incrementando o TTL e enviando novamente os pacotes até chegar ao destino (ponto 5), isto é feito guardando no `current_router` o último IP obtido e quando `current_router == destination_router` saímos do ciclo (ponto 3). Durante este processo vamos recolhendo os IPs devolvidos e guardando na lista `ip_list`(ponto 4). Com isto podemos descobrir o número de routers por onde o pacote passou e quais os seus IPs(ponto 6).

Corremos o programa e obtivemos:  

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_26.png)


## Task 1.4

Nesta task temos que fazer um programa que faça sniff a pacotes ICMP, e gere um echo reply para os mesmos, independentemente dos seus IPs de destino. Logo, quando um dispositivo utilizar o comando `ping` para um IP X, independentemente se o dispositivo X está vivo ou não, o `ping` irá receber sempre uma resposta a indicar que X está vivo. 

Para fazer este programa, começamos por escrever o seguinte código no ficheiro "fake_ping.py":

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_29.png)

Para o testar, corremos o programa "fake_ping.py" num terminal da nossa VM e abrimos o terminal do host B e corremos:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_28.png)

Porém ao contrário do que estávamos à espera, o `ping` não recebeu os pacotes de resposta gerados. Para tentar resolver o problema, corremos novamente o "fake_ping.py" e `ping 1.2.3.4`, mas desta vez realizamos também uma captura no Wireshark, e vimos que o programa "fake_ping.py" envia pacotes do tipo "Type: 8 (Echo (ping) request)", quando deveria enviar "Type: 0 (Echo (ping) reply)". Além disso, o ID do "Echo (ping) reply" deve ser igual ao ID do respetivo "Echo (ping) request", o que não está a acontecer, e como é um pedido de "Echo", a resposta deve enviar a mesma raw data que recebeu.

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_33.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_34.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_35.png)

Alteramos o programa, corrigindo os erros:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_36.png)

Corremos novamente e obtivemos o seguinte:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_37.png)

Como podemos ver nas respostas ao `ping` aparecem como "DUP!". Fomos à procura de uma solução e comparando com outros `ping` verdadeiros no wireshark, reparamos que a "seq" das respostas é sempres "0/0", quando deveria ser igual ao "seq" do "Echo (ping) request" correspondente. Para corrigir adicionamos `b.seq = pkt[ICMP].seq`:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_38.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_39.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_40.png)

De seguida, fizemos o mesmo, mas para o IP "10.9.0.99":

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_41.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_42.png)

Aqui tivemos um erro diferente, onde o dispositivo que está a fazer `ping`, envia um “ARP request broadcast message”, mas não obtém resposta e por isso ele nem sequer envia os pacotes ICMP. Nos dispositivos existe uma tabela de ARP, que traduz endereços de IP para endereços MAC. Para preencher esta tabela, utiliza pacotes ARP. Quando um dispositivo quer comunicar com outro dispositivo com um determinado IP, mas este não consta na sua tabela ARP, envia um “ARP request broadcast message” com o IP desejado para toda a rede e o dispositivo que tiver o endereço de IP desejado envia um “ARP reply packet” com o seu endereço MAC. Por isso, tivemos que alterar o nosso programa para quando detetar um “ARP request Broadcast”, enviar o nosso endereço MAC como resposta.

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_43.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_44.png)

Depois fizemos o mesmo, mas para o IP "8.8.8.8":

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sn_sp_45.png)

Como o IP "8.8.8.8", existe realmente, o `ping` recebe duas respostas, a verdadeira e a nossa, e por isso o segundo aparece como "DUP!".
