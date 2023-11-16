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
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_04.png)
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_05.png)

De seguida para testar ainda mais o mysql decidimos fazer a seguinte query básica:

![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_06.png)

## Task 2.1
![imagem do input introduzido](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_07.png)
![imagem depois de entrar na conta do Admin](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_07.png)

## Task 2.2
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_11.png)
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_10.png)

## Task 2.3
Admin'; delete from credential where Name = 'Ryan'; -- 
https://www.php.net/manual/en/mysqli.quickstart.multiple-statement.php
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_12.png)
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_13.png)


## Task 3
Nota: Colocar prints do que foi feito ;)
