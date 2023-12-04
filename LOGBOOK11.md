O guião da semana 11 tem como objetivo perceber o funcionamento de Public Key Infrastructures, de Cerificate Autorities e de ataques Man in the Middle a ligações HTTPS.

## Task 1

Começamos por copiar o código de `/usr/lib/ssl/openssl.cnf` para um ficheiro chamado `myopen.cnf` no diretório atual e criamos um ficheiro `index.txt` vazio e um fichero `serial` com conteudo 1000 e descomentados o atributo `unique_subject` do ficheiro `myopen.cnf`, como nos é indicado no enunciado.

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki01.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki02.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki03.png)

Agora queremos gerar o certificado e a respetiva chave privada da nossa CA, para depois podermos criar os nossos própios certificados e indicar se um website é seguro ou não. Para isso, corremos os seguintes commandos, de modo a gerar um certificado assinado por nos própios com a password "dees" e a respetiva chave privada:

![openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 -keyout ca.key -out ca.crt](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki04.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki04_5.png)

De seguida, imprimimos as especificações dos mesmos:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki05.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki06.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki06_1.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki06_2.png)

Acima podemos ver as especificações do certificado da CA, e podemos ver que é auto-assinado, uma vez que o "Subject" e o "Issuer" correspondem à mesma entidade.

Como podemos ver nas imagens acima, no ficheiro `ca.key` temos vários números utilizados no algoritmo de encriptação RSA, entre os quais o "modulus" que representa o _n=p*q_, o "prime1" que representa o _p_ e o "prime2" que representa o _q_.


## Task 2

Nesta tarefa, temos que gerar um certificado de chave pública da nossa CA para uma empresa chamada "bank32.com".
Para isso começamos por gerar um Certificate Signing Request (CSR) e a respetiva chave privada através do seguinte comando:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki07.png)

A seguir imprimimos a informação de `server.csr` e de `server.key`:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki08.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki09.png)

Estes ficheiros seriam enviados para a nossa CA, que iria verificar a informação do pedido.

De seguida repetimos os passos, mas desta vez adicionando o seguinte item ao criar o CSR, para permitir 2 alias para o website: "www.bank32A.com" e "www.bank32B.com".
```
-addext "subjectAltName = DNS:www.bank32.com, \
        DNS:www.bank32A.com, \
        DNS:www.bank32B.com"

```

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki10_1.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki10_2.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki10_3.png)


## Task 3 

Nesta tarefa queremos utilizar a nossa própia CA para gerar certificados. Para gerar um certificado X509 (server.crt) correspondente ao certificate signing request da tarefa anterior (server.csr), utilizando "ca.crt" e "ca.key", corremos o seguinte comando:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki11.png)

Como, podemos ver a configuração requere a existência de uma pasta "./demoCA/newcerts/". Então criamos uma nova pasta "demoCA" dentro da pasta atual e uma pasta "newcerts" dentro dessa e voltamos a correr  comando:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki12.png)

Agora diz que o ficheiro "demoCA/index.txt" não existe, logo copiamos o ficheiro "index.txt" da pasta atual para o pasta "demoCA" e voltamos a correr o comando:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki13.png)

Agora diz que o ficheiro "demoCA/serial" não existe, logo copiamos o ficheiro "serial" da pasta atual para o pasta "demoCA" e voltamos a correr o comando:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki14.png)

De modo a que o "myopen.cnf" permita que o comando "openssl ca" copie o extension field, descomentamos a seguinte linha, como indicado no enunciado:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki15.png)

A seguir, imprimimos o conteudo de  server.crt:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki16.png)


## Task 4

Nesta tarefa iremos ver como é os certificados de chave publica são utilizados pelos websites, de modo a permitir uma navegação segura. 
Para isso precisams de montar um container com um servidor Apache. Abrimos um novo terminal na pasta com o ficheiro `docker-compose.yml` e corremos no terminal `dcbuild`, seguido de `dcup`. Depois abrimos outro terminal e corremos `dockps` e vimos que tinhamos que tinhamos que nos conectar a `0f97471292c8  www-10.9.0.80`, logo corremos `docksh 0f`.

De seguida, dentro do container, fomos à pasta `/etc/apache2/sites-available`, onde estão guardados os ficheiros dos websites:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki17.png)

Aqui, lemos o conteúdo da configuração do website "https://www.bank32.com" que está no ficheiro `bank32_apache_ssl.conf`:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki18.png)

Aqui vemos que o nome do website é "https://www.bank32.com", que os ficheiros do website estão guardados em `/var/www/bank32`, que o website tem 3 alias: "https://www.bank32A.com", "https://www.bank32B.com", "https://www.bank32W.com", e vemos que o certificado SSL está em `/certs/bank32.crt` e que a chave privada está em `/certs/bank32.key`.

Como pedido no enunciado habilitamos o modulo ssl do Apache e depois habilitamos este site:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki19.png)

Depois corremos o servivor Apache, para isso corremos o seguinte comando e introduzimos a password indicada no enunciado("dees"), de modo a desencriptar a nossa chave privada:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki20.png)

Para aceder a "https://www.bank32.com", tivemos que modificar a DNS da nossa maquina, adicionando o nosso servidor a `/etc/hosts`:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki27.png)

Através do browser fomos a "https://www.bank32.com", mas este dizia-nos que não conseguia estabelecer uma ligação segura:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki21_1.png)

Com isto abrimos numa janela privada e obtivemos o seguite:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki21.png)

Como nos é dito no enunciado, não deveremos conseguir abrir o website de forma segura, porque o browser não reconhece o certificado, uma vez que este não foi assinado por uma Certificate Authority que o browser confia. Para isso adicionarmos a Certificate Authority criada nas tasks anteriores à lista de Certificate Authorities em que o nosso browser confia, colocamos no url "about:preferences#privacy" e andamos para baixo até encontrar a secção "Cerificates", clicamos em "View Certificates...", clicamos em "Import..." e selecionamos o ficheiro `ca.crt`. 

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki22.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki23.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki24.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki25.png)

De seguida copiamos o `server.crt` e `server.key` para a pasta partilhada(`Labsetup/volumes`) e depois no terminal do container, alteramos o `bank32_apache_ssl.conf`, de modo a que o certificado passasse a ser `/volumes/server.crt` e a chave privada passasse a ser `/volumes/server.key`.

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki26.png)

Depois reiniciamos o servidor(correndo "service apache2 stop" e "service apache2 restart"). Voltamos a abrir o browser, acedemos a "https://www.bank32.com" e obtivemos:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki28.png)


## Task 5

Esta tarefa tem como intuito mostrar-nos como a PKI nos defende contra a Man in Middle Attacks. 
Começamos por adicionar "www.facebook.com" à DNS da nossa maquina, alterar o url do nosso website lançado pelo servidor Apache e adicionar um novo html para este website:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki29.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki30.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki31.png)

E acedemos a "www.facebook.com", mas apareceu-nos a seguinte mensagem no ecrã:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki32.png)

Este aviso aparece-nos porque o certificado, fornecido pelo nosso "www.facebook.com", embora seja válido e verificado por uma CA que o browser confia(como vimos na task 4), não corresponde ao url "www.facebook.com", mas sim a "www.bank32.com".


## Task 6

Nesta tarefa iremos assumir que o atacante tem controlo sobre a CA, logo este vai criar um certificado para comprovar que o nosso "www.facebook.com" é legitimo.

Para isso, iremos repetir os passos que fizemos nas tarefas anteriores: fazer um CSR à CA para validar o nosso website, gerar um certificado para o respetivo pedido, e substituir o certificado e a chave privada antigos pelo novo certificado e pela nova chave privada.

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki33.png)

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki34.png)

E abrimos novamente o website:

![](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/pki35.png)

E já temos uma conexão "segura" com este website.
