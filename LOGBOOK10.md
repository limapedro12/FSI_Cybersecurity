## Task 1
Começamos por abrir o ficheiro `ciphertext.txt` e depois corremos o `freq.py` e obtivemos:

![conteudo do ciphertext.txt](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/encry0.PNG)

![freq.py](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/encry1.PNG)

![freq.py](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/encry2.PNG)

![freq.py](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/encry3.PNG)

Depois abrimos a [página da wikipedia sobre Frequency analysis](https://en.wikipedia.org/wiki/Frequency_analysis) e encontramos a seguinte informação:
```
For instance, given a section of English language, E, T, A and O are the most common, while Z, Q, X and J are rare.
```

Logo como vimos que as letras mais comuns no `ciphertext.txt` são 'n', 'y', 'v', 'x', assuminmos que esta corresponderiam às letras mais comuns no ingles('e', 't', 'a', 'o', respetivamente), além disso abrimos [página da wikipedia sobre Trigramas](https://en.wikipedia.org/wiki/Trigram) e descobrimos que a palavra mais comum no ingles é "THE" e que o trigrama mais comum no nosso documento é "ytn" que está de acordo a com substituição proposta a cima uma vez que substituindo já ficaria "TtE" que é muito similar ao desejado, logo assumimos também que "t" corresponde a "h". Para começar a decifrar a mensagem, tentamos correr `tr 'nyvxt' 'ETAOH' <ciphertext.txt> out.txt` mas estava sempre a dar erro por isso renomeamos o ficheiro `ciphertext.txt` para `in.txt` e  corremos o commando novamente mas na seguinte forma:

![tr 'nyvxt' 'ETAOH' <in.txt> out.txt](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/encry5.PNG)

![resultado do comando acima](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/encry6.PNG)


Depois abrimos a [página da wikipedia sobre Bigramas](https://en.wikipedia.org/wiki/Bigram) e vimos que já substituimos os 2 bigramas mais comuns("yt" e "tn" para "TH" e "HE"). Além disso, fomos ver quais eram o 3° e 4° bigramas mais comuns, ou seja, "mu" corresponde a "IN" e "nh" corresponde a "ER"(tendo em conta que o 'n' ja correspondia a 'E'). Para fazer isto corremos o seguinte comando:

![tr 'nyvxtmuh' 'ETAOHINR' <in.txt> out.txt](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/encry6.PNG)

Bigrams
yt c
tn c
in -> mu
er -> nh

Corremos:
image

Voltamos a ver os bigramas e trigrams mais comuns, mas começou a ficar mais dificil de tirar conclusões. Por isso, abrimos o ficheiro `out.txt` e reparamos quse algumas palavras faltavam 1 ou 2 letras e com isso conseguiamos inferir as letras ainda por desencriptar:

Reparamos que "TzRN" se parecia "TURN", por isso assumimos que 'z' corresponde a 'U'.
z -> u porque temos "TzRN", que parece "TURN"
Reparamos que "RIrHT" se parecia "RIGHT", por isso assumimos que 'r' corresponde a 'G'.
r -> g porque "RIrHT"
Reparamos que "AbTER" se parecia "AFTER", por isso assumimos que 'b' corresponde a 'F'.
b -> f "AbTER" "AFTER"
E reparamos que "THIq" se parecia "THIS", por isso assumimos que 'q' corresponde a 'S'.
q -> s "THIq" "THIS"
Aqui começou a ficar mais dificil continuar com este processo e por isso corremos o comando novamente, mas com as novas substituiçõoes:

![tr 'nyvxtmuhzrbq' 'ETAOHINRUGFS' <in.txt> out.txt](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/encry7.PNG)

Abrimos novamente o ficheiro `out.txt` e continuamos com o mesmo processo:
Reparamos que "AgOUT" se parecia "ABOUT", por isso assumimos que 'g' corresponde a 'O'.
g -> b "AgOUT" "ABOUT"
Reaparamos que "iONG" se parecia "LONG", por isso assumimos que 'i' corresponde a 'L'.
i -> l "iONG" "LONG"

![tr 'nyvxtmuhzrbqgi' 'ETAOHINRUGFSBL' <in.txt> out.txt](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/encry8.PNG)

Abrimos novamente o ficheiro `out.txt` e continuamos com o mesmo processo:
p -> d "SURROUNpING" "SURROUNDING"
Reparamos que "SURROUNpING" se parecia "SURROUNDING", por isso assumimos que 'p' corresponde a 'D'.
i -> q "jUESTION" "QUESTION"
Reparamos que "jUESTION" se parecia "QUESTION", por isso assumimos que 'j' corresponde a 'Q'.
k -> x "SEkUAL"
Reparamos que "SEkUAL" se parecia "SEXUAL", por isso assumimos que 'k' corresponde a 'X'.
c -> m "HARRASScENT"
Reparamos que "HARRASScENT" se parecia "HARRASSMENT", por isso assumimos que 'c' corresponde a 'M'.
a -> c e d->y "aOUNTRd" "COUNTRY"
E reparamos que "aOUNTRd" se parecia "COUNTRY", por isso assumimos que 'a' corresponde a 'C' e 'd' corresponde a 'Y'.
Corremos o comando novamente, mas com as novas substituiçõoes:

![tr 'nyvxtmuhzrbqgipjkcad' 'ETAOHINRUGFSBLDQXMCY' <in.txt> out.txt](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/encry9.PNG)

Abrimos novamente o ficheiro `out.txt` e continuamos com o mesmo processo:
l -> w "lHICH"
Reparamos que "lHICH" se parecia "WHICH", por isso assumimos que 'l' corresponde a 'W'.
f -> v "ACTIfISM"
Reparamos que "ACTIfISM" se parecia "ACTIVISM", por isso assumimos que 'f' corresponde a 'V'.
e -> p "eOLITICS"
Reparamos que "eOLITICS" se parecia "POLITICS", por isso assumimos que 'e' corresponde a 'P'.
Corremos o comando novamente, mas com as novas substituiçõoes:

![tr 'nyvxtmuhzrbqgipjkcadlfe' 'ETAOHINRUGFSBLDQXMCYWVP' <in.txt> out.txt](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/encry10.PNG)

Abrimos novamente o ficheiro `out.txt` e continuamos com o mesmo processo:
s -> k "THANsS"
Reparamos que "THANsS" se parecia "THANKS", por isso assumimos que 's' corresponde a 'K'.
o -> j "oUST" "JUST"
Reparamos que "oUST" se parecia "JUST", por isso assumimos que 'o' corresponde a 'J'.
Corremos o comando novamente, mas com as novas substituiçõoes:

![tr 'nyvxtmuhzrbqgipjkcadlfeso' 'ETAOHINRUGFSBLDQXMCYWVPKJ' <in.txt> out.txt](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/encry11.PNG)

A mensagem já estava quase toda decifrada, mas ainda faltavam apenas tinhamos substutuido 25 letras, logo fomos ver qual era a letra que faltava e concluimos que 'w' corresponde a 'Z'.
Com isto corremos uma ultima vez o comando:

![tr 'nyvxtmuhzrbqgipjkcadlfesow' 'ETAOHINRUGFSBLDQXMCYWVPKJZ' <in.txt> out.txt](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/encry12.PNG)

E obtivemos a mensagem decifrada:

![Mensagem final](https://git.fe.up.pt/fsi/fsi2324/logs/l06g07/-/raw/main/images/encry13.PNG)


## Task 2




