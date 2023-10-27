## Processo utilizado para resolver o CTF da Semana 6

Começamos por introduzir uma justificação qualquer:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/xss_csrf_1.PNG)
Fomos redirecionados para a seguinte página:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/xss_csrf_2.PNG)
De seguida fomos à página onde o administrador aceita ou não os pedidos:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/xss_csrf_3.PNG)
Aqui, inspecionamos a página e vimos o código dos botões que o administrador deve clicar:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/xss_csrf_4.PNG)

A ideia é forçar o administrador a clicar no botão para nos dar a flag através de xss, ou seja, inserindo código javascript na caixa de texto.
Com isto, copiamos o código do botão de aceitar o pedido, retirando o disabled, e mudando o url da action, colocando o caminho completo, porque se não o fizéssemos, iriamos ser redirecionados para a porta 5004, e mudamos o id do pedido para o id do novo pedido, resultando em:

`http://ctf-fsi.fe.up.pt:5005/request/ed6748362972777c957ca7f8495555b62bdee3aa/approve`

Além disso, adicionamos um script javascript que clica automaticamente no botão, que acabamos de adicionar. O resultado foi o seguinte:
```html
<form method="POST" action="http://ctf-fsi.fe.up.pt:5005/request/ed6748362972777c957ca7f8495555b62bdee3aa/approve" role="form">     
    <div class="submit">                  
        <input type="submit" id="giveflag" value="Give the flag">              
    </div> 
</form>
<script>document.getElementById("giveflag").click();</script>
```

A seguir, inserimos o código que acabamos de escrever na caixa das justificações:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/xss_csrf_5.PNG)

E fomos redirecionados para a seguinte página:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/xss_csrf_6.PNG)

Isto acontece porque o browser ao interpetar o código anterior, clica automaticamente no botão que adicionamos e somos redirecionados para a página de aprovar o pedido, mas como não temos permissões para entrar nesta página, aparece-nos uma mensagem de "Forbidden". Para resolver este problema que desligamos o javascript do nosso browser:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/xss_csrf_7.PNG)

Depois fomos para a página para ver a nossa justificação, que está no seguinte url:

`http://ctf-fsi.fe.up.pt:5004/request/ed6748362972777c957ca7f8495555b62bdee3aa`

E assim conseguimos obter a flag:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/xss_csrf_8.PNG)
