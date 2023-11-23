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

Claro, claro! Vamos lá preencher esses espaços em branco com as explicações adequadas:

### Task 2.1
Sabemos que temos que executar um ataque SQL Injection. No passo anterior vimos que o username do administrador é `Admin`, e ao analisar o codigo em `unsafe_home.php` vimos que as linhas que executam a query de login são suscetíveis a um ataque de SQL Injection, uma vez que não utiliza prepared statements, nem sanitização de inpu:

![imagem de unsafe_home.php](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_13.png)

Com esta informação, para executar o ataque temos que, apos inserir o username do utilizador pretendido, encerrar a string inserida na query com `'`, encerrar a declaração SQL com `;` e adicionar um comentário SQL `--` para comentar a password esperada pelo sistema.
Assim, introduzimos `Admin'; -- ` na caixa de texto do username e não introduzimos nada na caixa de texto da password.

![imagem do input introduzido](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_07.png)

Assim, conseguimos acesso à página de administrador:

![imagem da página de administrador](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_08.png)

### Task 2.2
Em seguida, procedemos a executar o mesmo ataque, mas através da shell. O enunciado indica-nos que podemos fazer um pedido HTTP GET ao servidor para obter o HTML da página de um determinado utilizador, usando o comando:

```
curl 'www.seed-server.com/unsafe_home.php?username=alice&Password=11'
```

Substituímos o username e a password do comando dado pelos usados na task 2.1 (username Admin'; -- e password vazia). Para isso, codificamos os espaços para "%20" e o apóstrofo(') por "%27", porque o URL não pode conter estes caracteres diretamente, já que certos caracteres, como espaços e apóstrofos, podem causar problemas na interpretação do URL, levando a erros na requisição ou até mesmo à interpretação incorreta dos parâmetros enviados. Portanto, a codificação é necessária para garantir que o URL seja interpretado corretamente pelo servidor, evitando falhas na requisição e assegurando que o ataque de SQL Injection seja executada como pretendido.

Depois dessas alterações, obtemos o seguinte comando:

```
curl "www.seed-server.com/unsafe_home.php?username=Admin%27%20;%20--%20"
```

E ao correr o comando:

![HTML recebido parte 1](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_11.png)
![HTML recebido parte 2](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_10.png)

### Task 2.3
Tentamos alterar a base de dados deixando a caixa de texto da password vazia e colocando como username:

```
Admin'; delete from credential where Name = 'Ryan'; --
```

Fizemos isto com a intenção de eliminar registos da base de dados. No entanto, ao submeter, recebemos a seguinte resposta:

![imagem de erro](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_12.png)

Fomos investigar sobre o porquê de isto acontecer e descobrimos que a função `mysqli::query()`, utilizada na aplicação web fornecida, não permite múltiplas declarações numa única linha, a fim de prevenir ataques de SQL Injection, como podemos ver na [documentação do PHP](https://www.php.net/manual/en/mysqli.quickstart.multiple-statement.php):

![documentação php](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_14.png)
![imagem de unsafe_home.php](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/sqli_13.png)


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
