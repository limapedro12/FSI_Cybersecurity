## Task 1

Começamos por copiar o codigo de `/usr/lib/ssl/openssl.cnf` para um ficheiro chamado `myopen.cnf` no diretorio atual e criamos um ficheiro `index.txt` vazio e um fichero `serial` com conteudo 1000 e descomentados o atributo `unique_subject` do ficheiro `myopen.cnf`.

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki01.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki02.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki03.png)

Corremos os seguintes commandos, de modo a gerar um certificado assinado por nos propios com a password "codecacao" e a respetiva chave privada:

![openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 -keyout ca.key -out ca.crt](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki04.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki04_5.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki05.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki06.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki06_1.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki06_2.png)

Acima podemos ver as especificações do certificado da CA, e podemos ver que é auto-assinado, uma vez que o "Subject" e o "Issuer" são o mesmo.

Como podemos ver nas imagens acima, no ficheiro `ca.key` temos vários numeros utilizados no algoritmo de encriptação RSA, entre os quais "modulus" representa o _n=p*q_, "prime1" representa o _p_ e "prime2" representa o _q_.

## Task 2
Nesta tarefa, temos que gerar um cerificado de chave publica da nossa CA para uma empresa chamada "bank32.com".
Para isso começamos por gerar um Certificate Signing Request (CSR)através do seguinte comando:
![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki7.png)

O ficheiro csr seria enviado para a nossa CA, qie ira veriicar a informação no pedido e depois gerar um certificado e a resoetiva chave privada com os comandos seguintes:
![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki8.png)
![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki9.png)

De seguida repetimos os passos, mas desta vez adicionando o seguinte ao comando para criar o CSR
```
-addext "subjectAltName = DNS:www.bank32.com, \
        DNS:www.bank32A.com, \
        DNS:www.bank32B.com"

```
![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki10_1.png)
![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki10_2.png)
![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki10_3.png)


## Task 3 
De modo utilizar a nossa propia CA para gerar certificados, corremos o seguinte comando para turnar o certificate signing request (server.csr) num certificado X509 (server.crt), utilizando "ca.crt" e "ca.key":

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki11.png)

Como, podemos ver a configuração pede a existencia de uma pasta './demoCA/newcerts/'. Então criamos uma nova pasta 'demoCA' dentro da pasta atual e uma pasta 'newcerts' dentro dessa e voltamos a correr  comando:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki12.png)

Agora diz que o ficheiro 'demoCA/index.txt' não existe, logo copiamos o ficheiro 'index.txt' da pasta atual para o pasta 'demoCA' e voltamos a correr o comando:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki13.png)

Agora diz que o ficheiro 'demoCA/serial' não existe, logo copiamos o ficheiro 'serial' da pasta atual para o pasta 'demoCA' e voltamos a correr o comando:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki14.png)

De modo a que o "myopen.cnf" permita que o comando "openssl ca" copie o extension field, descomentamos a seguinte linha:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki15.png)

A seguir, imprimimos o conteudo de  server.crt:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki16.png)


## Task 4
Nesta tarefa iremos ver como é os certificados de chave publica são utilizados pelos websites, de modo a permitir uma navegação segura. 
Para isso precisams de montar um container com um servidor Apache. Abrimos um novo terminal na pasta com o ficheiro `docker-compose.yml` e correr `dcbuild` seguido de `dcup`. Depois abrimos outro terminal e corremos `dockps` e vimos que tinhamos que conectar a `0f97471292c8  www-10.9.0.80`, logo corremos `docksh 0f`.

De seguida, dentro do container, fomos à pasta `/etc/apache2/sites-available`, onde estao guardados os ficheiros dos websites:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki17.png)

Aqui, lemos o conteudo da configuração do website "https://www.bank32.com" que está no ficheiro `bank32_apache_ssl.conf`:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki18.png)

Aqui vemos que o nome do website é "https://www.bank32.com", que os ficheiros do website estão guardados em `/var/www/bank32`, que o website tem 3 alias: "https://www.bank32A.com", "https://www.bank32B.com", "https://www.bank32W.com", e vemos que o certificado SSL está em `/certs/bank32.crt` e que a chave privada está em `/certs/bank32.key`.

Como pedido no enunciado habilitamos o modulo ssl do Apache e depois habilitamos este site:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki19.png)

Depois corremos o servivor Apache, para isso corremos o seguinte comando e introduzimos a password indicada no enunciado("dees"), de modo a desencriptar a nossa chave privada:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki20.png)

Para aceder a "https://www.bank32.com", tivemos que adicionar o IP do nosso servidor a `/etc/hosts`:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki27.png)

Através do browser fomos a "https://www.bank32.com", e obtivemos o seguite:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki21.png)

Como nos é dito no enunciado, não deveremos conseguir abrir o website de forma segura, porque o browser nao reconhece o certificado, uma vez que este não foi assinado por uma Certificate Authority que o browser confia. Para isso adicionamos a Certificate Authority criada nas tasks anteriores à lista de Certificate Authorities em que o nosso browser confia, coloncando no url o "about:preferences#privacy" e andamos para baixo até encontrar a secção "Cerificates", clicamos em "View Certificates...", clicamos em "Import..." e selecionamos o ficheiro `ca.crt`. 

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki22.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki23.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki24.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki25.png)

De seguida copiamos o `server.crt` e `server.key` para a pasta partilhada(`Labsetup/volumes`) e depois no terminal do container, alteramos o `bank32_apache_ssl.conf`, de modo a que o certificado passasse a ser `/volumes/server.crt` e a chave privada passasse a ser `/volumes/server.key`.

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki26.png)

Depois reiniciamos o servidor(correndo "service apache2 stop" e "service apache2 restart"). Depois voltamos a abrir o browser, acedemos a "https://www.bank32.com" e obtivemos:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki28.png)
