## Task 1
Começamos por lançar os 3 dockers containers, dois containers que irão trocar mensagens(com IPs "10.9.0.5" e "10.9.0.6") e um terceiro que vai servir como atacante(com IP "10.9.0.1"). Para o atacante também poderiamos utilizar a nossa máquina em vez de fazer um novo container (aliás, como o `network_mode` desse container é igual a `host`, tanto o ip como as permissões sobre a rede da nossa máquina e do container serão as mesmas).

![](sn_sp_01.png)

![](sn_sp_02.png)

De seguida, criamos um novo ficheiro "mycode.py" no diretorio partilhado, e copiamos o conteudo do codigo fornecido para o mesmo, de modo a testar se o scapy estava bem instalado.

![](sn_sp_04.png)

![](sn_sp_06.png)

## Task 1.1

### Task 1.1A

Copiamos o codigo fornecido no enunciado para um ficheiro "sniffer.py" e alteramos o nome da interface da rede do codigo dado, para o nome da interface da nossa rede, que para o descobrimos, corremos o comando `ifconfig` e vimos qual era a interface cujo ip é `10.9.0.1`. Depois mudamos o modo para que este se tornasse executavel, com `chmod a+x sniffer.py`.

![](sn_sp_08.png)

![](sn_sp_09.png)

De seguida, corremos o "sniffer.py" com privilegios root, e o programa correu direito, mas como nao havia pacotes na rede ele nao imprimiu nada. Para testar fomos ao host B e deste ping para o host A, e aqui o o sniffer ja mostrou pacotes.

![](sn_sp_13.png)

![](sn_sp_11.png)

![](sn_sp_16_icmp.png)

Corremos o "sniffer.py", mas desta vez sem privilegios root, o que devoldeu um erro de falta de permissões:

![](sn_sp_14.png)


### Task 1.1B

É nos pedido para filtrar pacotes no Scapy utilizando a sintaxe BPF (Berkeley Packet Filter). Para isso procuramos na internet e encontramos o seguinte website que nos ensinou como fazer isso:

https://dl.acm.org/doi/fullHtml/10.5555/2642922.2642925

No enunciado é nos pedido que comecemos por filtrar apenas pelos pacotes ICMP. Iss ja é feiro no codigo dado inicialmente, uma vez que ao chamarmos a função `sniff()`, passamos como argumento `filter='icmp'`. Por isso so voltamos a correr o programa como na task 1.1A(utilizando na mesma o ping do host B para o host A)

![](sn_sp_16_underlined.png)

![](sn_sp_16_icmp.png)

A seguir é nos pedido que filteremos pelos pacotes TCP que venham de um determinado ip e que tenha como a porta 23 de destino. Decidimos filtrar por pacotes que venham do host B, ou seja, com ip "10.9.0.6". Para isso alteramos, na chamada à função `sniff()`, o `filter` para `'tcp and host 10.9.0.6 and port 23'`. O "sniffer.py" corre, porém não coneguimos testar.

![](sn_sp_17.png)

![](sn_sp_18.png)

Depois é nos pedido que filteremos por pedidos que venham de uma subnet especifica, aconselham a filtrar pela subnet associada à nossa VM, que neste caso é "10.9.0.0/24". Para isso alteramos, na chamada à função `sniff()`, o `filter` para `'net 10.9.0.0/24'`. Para testar fizemos `ping 10.9.0.5` no host B.

![](sn_sp_19.png)

![](sn_sp_19.png)

## Task 1.2



