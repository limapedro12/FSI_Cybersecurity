## Task 1.1
Começamos por lançar os 3 dockers containers, dois containers que irão trocar mensagens(com IPs "10.9.0.5" e "10.9.0.6") e um terceiro que vai servir como atacante(com IP "10.9.0.1"). Para o atacante também poderiamos utilizar a nossa máquina em vez de fazer um novo container (aliás, como o `network_mode` desse container é igual a `host`, tanto o ip como as permissões sobre a rede da nossa máquina e do container serão as mesmas), mas utlizaremos o terceiro container como recomenda no enunciado.

https://dl.acm.org/doi/fullHtml/10.5555/2642922.2642925
