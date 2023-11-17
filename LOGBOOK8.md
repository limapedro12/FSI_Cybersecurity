# Guião Semana 8
Como indicado no enunciado começamos por correr o docker container que está em `category-web/Web_SQL_Injection` utilizando os comandos `dcbuild` e `dcup`:

![dcbuild](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_00.png)
![dcup](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_01.png)

De seguida abrimos uma shell no docker container da base de dados, para podermos realizar as tasks:

![dockps](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_02.png)
![docksh 25](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_03.png)


## Task 1
Na shell que abrimos anteriormente corremos os comandos apresentados no enunciado:

![mysql -u root -pdees](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_045.png)
![use sqllab_users](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_04.png)
![show table](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_05.png)

De seguida para testar ainda mais o MySQL decidimos fazer a seguinte query básica `select * from credential`:

![tabela credential](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_06.png)

## Task 2.1
Sabemos que temos que executar SQL injection. No passo anterior que o username do administrador é `Admin`, por isso introduzimos `Admin'; -- ` na caixa de texto do username e não intruduzimos nada na caixa de texto da password, porque...

![imagem do input introduzido](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_07.png)

E assim consequimos acesso à página de administrador:

![imagem da pagina de administrador](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_08.png)

## Task 2.2

De seguida procedemos a executar o mesmo ataque mas através da shell. No enunciado é nos dito que podemos podemos fazer um pedido HTTP GET ao servidor para obter o HTML da pagina de um determinado utilizador, correndo o comando:
```
curl 'www.seed-server.com/unsafe_home.php?username=alice&Password=11'
```
Com isto subtituimos o username e a password do comando dado pelos utilizados na task 2.1(username `Admin'; -- ` e password vazia). Para fazer isto temos que codificar o espaço para "%20" e o apóstrofo(') por "%27", porque ...
Depois destas alterações obtemos o seguinte comando:
```
curl "www.seed-server.com/unsafe_home.php?username=Admin%27%20;%20--%20"
```
E corremos o comando:

![fomulario de login](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_11.png)
![imagem da pagina de administrador](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_10.png)

## Task 2.3
Tentamos alterar a base de dados colocando deixando a a caixa de texto da passsword vazia e colocando como do username:
```
Admin'; delete from credential where Name = 'Ryan'; --
``` 
Fizemos isto porque ...

Ao submeter obtivemos a seguinte resposta:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_12.png)

Fomos investigar sobre o porquê de isto acontecer e na documentação do php encontramos:
https://www.php.net/manual/en/mysqli.quickstart.multiple-statement.php
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_13.png)


## Task 3.1

Como utilizador Alice tentamos mudar o seu salário usando uma SQL injection usando a opção de Edit Profile da seguinte forma: 
![ChangeSalary](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/SQLI1.PNG)

Como se pode ver abaixo tivemos sucesso e o salário foi alterado para 2000000:
![ChangeSalary](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/SQLI2.PNG)

## Task 3.2

Para modificar o salário de Boby usamos o mesmo método que a task 3.1 mas desta vez da seguinte forma:
![ChangeSalary](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/SQLI4.PNG)

Podemos confirmar que deu certo com a conta do admin verificando na lista de utilizadores que Boby tem agora salário igual a 1:
![ChangeSalary](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/SQLI5.PNG)

## Task 3.3

Para modificar a password de Boby primeiro precisamos de tranformar a nova password ('1234') para a forma SHA1, para isso usamos um ficheiro php:

![ChangePassword](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/SQLI8.PNG)

E obtivemos o seguinte conjunto de caracteres:

![ChangePassword](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/SQLI6.PNG)

A seguir mudados a password usando o seguinte método de SQL injection: 

![ChangePassword](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/SQLI7.PNG)

Por fim conseguimos fazer login com na conta de Boby com o nome 'Boby' e password '1234' e alterar algumas das suas informações como pedido na task:

![ChangePassword](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/SQL10.PNG)
