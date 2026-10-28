# ACTIVIDAD 1 — GIT

Para crear el directorio `prueba_git`:

```bash
mkdir prueba_git
```
---
Después, accedemos al directorio:

```bash
cd prueba_git/
```
---
Creamos `texto.txt` con el siguiente contenido:
```text
uno
dos
tres 
```
---
Ahora creamos `script.sh` con el siguiente contenido:
```bash
echo Listado completo
ls -l 
```

## 1. INICIALIZACIÓN
### 1.1. Añadir `prueba_git` como repositorio local
Para añadirlo como repositorio local, tenemos que estar dentro del directorio `prueba_git`, por lo tanto, accedemos a él, comprobamos que estamos dentro y creamos el repositorio local:
```bash
$ cd /c/Users/alejandra/Desktop/AFOND/Tema1/prueba_git
$ pwd
$ git init
```
Nos saldrá un mensaje similar a 
`Initialized empty Git repository in C:/Users/alejandra/Desktop/AFOND/Tema1/prueba_git/.git/`.

### 1.2. Comprobar el estado del repositorio
Una vez creado el repositorio local, podemos comprobar su estado mediante el comando:
```bash
$ git status
```
Git nos indica que aunque estos archivos estén en el directorio, **no están gestionados por él**.
```bash
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        script.sh
        texto.txt

nothing added to commit but untracked files present (use "git add" to track)
```
El nombre que se le da por defecto es `master`, pero se lo podremos cambiar con:
```bash
alejandra@DESKTOP-KB9PHSP MINGW64 ~/Desktop/AFOND/Tema1/prueba_git (master)
$ git branch -m main
alejandra@DESKTOP-KB9PHSP MINGW64 ~/Desktop/AFOND/Tema1/prueba_git (main)
```
Si miramos el archivo .git/HEAD veremos el nombre de la rama en la que estamos con el nuevo nombre.
```bash
$ cat .git/HEAD
ref: refs/heads/main
```
De todas formas, restauraremos el nombre a master.
```bash
$ git branch -m master
```

## 2. AÑADIENDO ARCHIVOS
### 2.1. Añadir `script.sh` a los archivos rastreados 
En este momento, Git no está siguiendo ninguno de los archivos de `prueba_git`. Estos archivos se llaman **untracked** (no rastreados). Vamos a añadir `script.sh` a los archivos rastreados y a comprobar su estado:
```bash
$ git add script.sh
$ git status

On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   script.sh

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        texto.txt
```
Con **add** indicamos que queremos preparar un archivo para el próximo **commit**. Esto prepara los cambios para ser confirmados, pero como estos aún no lo están, no se sincronizarán con los equipos remotos.

Si quisiesemos añadir todos los archivos de la carpeta de golpe, podríamos hacerlo con:
```bash
$ git add .
```
Para confirmarlos o para establecer una nueva versión del archivo:
```bash
$ git commit -m "Confirmación inicial"
[master (root-commit) 88e0a75] Confirmación inicial
 1 file changed, 2 insertions(+)
 create mode 100644 script.sh

alejandra@DESKTOP-KB9PHSP MINGW64 ~/Desktop/AFOND/Tema1/prueba_git (master)
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        texto.txt

nothing added to commit but untracked files present (use "git add" to track)
```
Al confirmar se añade un mensaje que queda guardado con cada commit. Ahí se suelen indicar los cambios desde la última versión. Si el mensaje que queremos guardar es muy largo, lo guardaremos en un archivo de texto:
```bash
$ git commit -F mensaje.txt
```

Añadiremos también `texto.txt`:
```bash
$ git add texto.txt

alejandra@DESKTOP-KB9PHSP MINGW64 ~/Desktop/AFOND/Tema1/prueba_git (master)

$ git commit -m "Añadido archivo texto.txt"
[master 9d919e6] Añadido archivo texto.txt
 1 file changed, 3 insertions(+)
 create mode 100644 texto.txt

alejandra@DESKTOP-KB9PHSP MINGW64 ~/Desktop/AFOND/Tema1/prueba_git (master)

$ git status
On branch master
nothing to commit, working tree clean

```

### 2.2. Objetos internos de Git
Git guarda internamente de forma comprimida cada uno de los objetos que está manejando en el directorio `.git/objects`. Estos objetos se identifican mediante un hash: un código que Git utiliza para identificar de forma única cada objeto que guarda.

Hay tres tipos principales de objetos: 
- `blob`: Contiene el contenido de un archivo.
- `tree`: Representa la estructura de directorios y relaciona los archivos que contiene.
- `commit`: Representa una versión del proyecto y contiene información como el autor, la fecha, el mensaje y el tree correspondiente.

Podemos comprobar qué tipo de objeto es un determinado hash mediante:
```bash
$ git cat-file -t hash_code
```
Donde hash_code es el hash del objeto. Podemos utilizar las primeras 4 o 6 cifras siempre que sean suficientes para identificarlo de forma única.

## 3. MODIFICANDO ARCHIVOS
Ahora modificaremos el archivo `texto.txt`, añadiendole una línea con la palabra *cuatro*.

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   texto.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Para prepararlo para el siguiente commit, hay que ejecutar de nuevo add. Add
es un comando que además de añadir a rastreo nuevos archivos, los prepara para
el siguiente commit:
```bash
$ git add texto.txt

alejandra@DESKTOP-KB9PHSP MINGW64 ~/Desktop/AFOND/Tema1/prueba_git (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   texto.txt
```
Si lo volvemos a modificar añadiéndole otra línea con la palabra *cinco* y vemos el estado de Git veremos que aparece en stage (preparado para el commit) y en modificado sin preparar:
```bash
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   texto.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   texto.txt
```
Si se hace ahora el commit, confirmaría la versión anterior, no la última modificación. Esto implicaría que al sincronizarlo en otro equipo, se sincronizaría la última versión. 

Por lo tanto, tenemos que hacer un **add** para incluírlo todo:
```bash
$ git add texto.txt

alejandra@DESKTOP-KB9PHSP MINGW64 ~/Desktop/AFOND/Tema1/prueba_git (master)
$ git commit -m "Ampliada la explicación del texto"
[master f47f803] Ampliada la explicación del texto
 1 file changed, 3 insertions(+), 1 deletion(-)

alejandra@DESKTOP-KB9PHSP MINGW64 ~/Desktop/AFOND/Tema1/prueba_git (master)
$ git status
On branch master
nothing to commit, working tree clean
```
Vuelve a estar todo limpio.

## 4. INFORMACIÓN
Cuando trabajamos con archivos que ya están bajo seguimiento (**tracked**), es posible resumir el comando git add y confirmar directamente las modificaciones mediante el parámetro `-a`:
```bash
$ git commit -a -m "Nueva versión"
```
Este atajo solo trabaja con archivos ya rastreados. Si añadimos un archivo nuevo, su estado será **untracked** y será obligatorio ejecutar `git add` de forma previa.

### 4.1. Inspección del último commit
Para ver la información detallada de la última versión confirmada y los cambios introducidos en sus archivos ejecutamos:
```bash
$ git show
commit f47f8030a0b3d98bc5627099ab8392d6b9518852 (HEAD -> master)
Author: alejandra <gonzalezgranaalejandra@gmail.com>
Date:   Wed Oct 7 11:17:53 2026 +0200

    Ampliada la explicación del texto

diff --git a/texto.txt b/texto.txt
index 3664b17..d92e49a 100644
--- a/texto.txt
+++ b/texto.txt
@@ -1,3 +1,5 @@
 uno
 dos
-tres
\ No newline at end of file
+tres
+cuatro
+cinco
\ No newline at end of file
```
Si la salida es extensa, se puede navegar con los cursores y salir pulsando la tecla `q`.

### 4.2. Historial del proyecto
Para consultar el registro cronológico de todas las versiones y confirmaciones realizadas hasta el momento:
```bash
$ git log
commit f47f8030a0b3d98bc5627099ab8392d6b9518852 (HEAD -> master)
Author: alejandra <gonzalezgranaalejandra@gmail.com>
Date:   Wed Oct 7 11:17:53 2026 +0200

    Ampliada la explicación del texto

commit 9d919e62b1c739b1df611b05931c55d166421d36
Author: alejandra <gonzalezgranaalejandra@gmail.com>
Date:   Wed Oct 7 10:28:53 2026 +0200

    Añadido archivo texto.txt

commit 88e0a75146f389ae5c6209389651bd0203a65113
Author: alejandra <gonzalezgranaalejandra@gmail.com>
Date:   Wed Oct 7 09:48:08 2026 +0200

    Confirmación inicial
```
En este listado se comprueba que:
1. Aparece el historial de los tres commits que hemos realizado.
2. El puntero `HEAD -> master`, que indica que nos encontramos en la última versión de la rama maestra.
3. Cada entrada muestra su identificador en hash, autor, fecha y mensaje de confirmación.

Si queremos examinar el contenido interno de cualquiera de los commits del historial, utilizamos la opción `-p` junto a las primeras cifras de su hash:
```bash
$ git cat-file 88e0 -p
tree 00ce17026e473736d9cdc11f44c097ee6e470e3d
author alejandra <gonzalezgranaalejandra@gmail.com> 1791359288 +0200
committer alejandra <gonzalezgranaalejandra@gmail.com> 1791359288 +0200

Confirmación inicial
```
**NOTA**: Al tratarse del commit inicial, no tiene ningún commit padre, por lo que únicamente referencia a su objeto tree.

## 5. DIFERENCIAS
### 5.1. Primera prueba de diferencias
Para seguir con este punto, debemos editar el archivo texto.txt: añadiremos una nueva línea (seis) y eliminaremos otra (tres).

Para conocer las diferencias entre un archivo modificado en el directorio de trabajo y la última versión confirmada en el repositorio, se utiliza el siguiente comando:
```bash
    $ git diff
1   diff --git a/texto.txt b/texto.txt
2   index d92e49a..ea169a0 100644
3   --- a/texto.txt
4   +++ b/texto.txt
5   @@ -1,5 +1,5 @@
6    uno
7    dos
8  - tres
9    cuatro
10   cinco
11 + seis
```
**LÍNEAS 1-4:** Muestran los archivos comparados (a/ para la versión previa y b/ para la versión actual en trabajo), información de hashes y los permisos del archivo.

**LÍNEA 5:** El signo `-` se lee como "archivo previo" y el `+` como "archivo final". El primer número indica la fila donde comienza el bloque mostrado en cada versión (incluye las líneas intactas que Git añade alrededor como contexto). El segundo número indica la cantidad total de líneas que abarca dicho bloque en esa versión. 

**LÍNEAS 6, 7, 9 y 10:** Las líneas sin prefijo son líneas sin alteraciones. 

**LÍNEA 8:** El prefijo `-` indica que la línea está eliminada: se encontraba en el archivo de origen, pero no en el archivo final.

**LÍNEA 11:** El prefijo `+` indica que es una nueva línea insertada: está presente en el archivo final, pero no en el de origen.

Confirmaremos esta modificación aprovechando el atajo que vimos al inicio del [punto 4](#4-información):
```bash
$ git commit -a -m "Probando diff"
[master 5e68276] Probando diff
 1 file changed, 1 insertion(+), 1 deletion(-)
```
Git nos resume la información indicando que hubo 1 inserción y 1 eliminación en 1 archivo.

### 5.2. Segunda prueba de diferencias
Volveremos a editar el archivo texto.txt añadiendo la línea *siete*. 
```bash
    $ git diff
1   diff --git a/texto.txt b/texto.txt
2   index bf98941..430cdae 100644
3   --- a/texto.txt
4   +++ b/texto.txt
5   @@ -3,3 +3,4 @@ dos
6    cuatro
7    cinco
8    seis
9  + siete
```

**LÍNEA 5:** La primera prueba reflejaba que se empezaba en la fila 1 y abarcaba 5 líneas. En la segunda prueba esto cambia ya que Git omite el inicio del archivo porque no ha cambiado, comenzando a inspeccionar a partir de la línea 3. En la versión previa (-3,3) se toman 3 líneas (cuatro, cinco, seis). En la versión actual (+3,4) se muestran 4 líneas al haberse incorporado la nueva línea + siete.

**LÍNEA 6-8:** Se mantienen intactas sin prefijo, como líneas de contexto.

**LÍNEA 9:** El prefijo `+` indica la nueva línea añadida en el archivo de trabajo.

Volvemos a confirmar todos los cambios mediante el atajo:
```bash
$ git commit -a -m "Probando diff 2"
[master 002fb80] Probando diff 2
 1 file changed, 5 insertions(+)
```

### 5.3 Tercera prueba de diferencias
Haremos una última prueba de diferencias para la que añadiremos la línea *once*.
```bash
    $ git diff
1   diff --git a/texto.txt b/texto.txt
2   index b08c8c3..76778ed 100644
3   --- a/texto.txt
4   +++ b/texto.txt
5   @@ -7,3 +7,4 @@ siete
6    ocho
7    nueve
8    diez
9  + once
10 \ No newline at end of file
```

**LÍNEA 5:** Como las primeras 6 líneas no han cambiado, Git las ignora para resumir la salida y coloca *siete* (la sexta línea del archivo texto.txt) al final de la cabecera como referencia de contexto previa. En la versión previa confirmada (`-7,3`), el bloque comienza en la línea 7 y abarca 3 líneas de contexto (*ocho, nueve* y *diez*). En la versión modificada (`7,4`), el bloque también empieza en la línea 7, pero pasa a tener 4 líneas en total al incorporarse la nueva línea *once*.

**LÍNEAS 6-8:** Representan las 3 líneas de contexto que se mantienen intactas antes de la modificación.

**LÍNEA 9:** Indica la nueva línea insertada al final del archivo.

Volvemos a confirmar todos los cambios mediante el atajo:
```bash
$ git commit -a -m "Probando diff 3"
[master 2e01c6b] Probando diff 3
 1 file changed, 1 insertion(+)
```

## 6. IGNORAR ARCHIVOS
Al desarrollar proyectos se generan archivos que no interesa incluír en el control de versiones, como archivos compilados (por ejemplo, .class en Java). 

Para que Git los descarte automáticamente, se crea un archivo de texto plano llamado `.gitignore` en la raíz del repositorio donde se especificarán las pruebas de exclusión.

### 6.1 Creación del archivo .gitignore
Creamos en el directorio `prueba_git` un archivo de texto plano con el siguiente contenido:
```text
# Para que ignore los archivos terminados en .class
*.class

# Para que ignore los archivos terminados en ~
*~

# Para que no ignore uno concreto que acabe en ~ (aunque en la línea anterior se haya declarado que se ignoren todos)
!importante~
```
Los # indican línea de comentario. 

### 6.2 Creación de los archivos de prueba
El primer archivo de prueba será Hola.java con el siguiente contenido:
```java
class Hola {
    public static void main(String[] args) {
        System.out.println("Welcome to the Java World");
    }
}
```
Lo compilaremos desde la terminal para generar el archivo Hola.class:
```cmd
C:\Users\alejandra\Desktop\AFOND\Tema1\prueba_git>javac Hola.java
```
Crearemos dos archivos adicionales:
```bash
$ touch "temporal~"
$ touch "importante~"
```

Lo podemos comprobar con `$ ls -a` y obtener un resultado similar al siguiente:
```
./  ../  .git/  .gitignore  Hola.class  Hola.java  importante~  script.sh  temporal~  texto.txt
```

### 6. Comprobación del filtro con `git status`
Consultamos el estado del repositorio para verificar la efectividad de las reglas de `.gitignore`:
```bash
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        Hola.java
        importante~

nothing added to commit but untracked files present (use "git add" to track)
```
Si quitamos el archivo `.gitignore` y hacemos de nuevo un status veremos que tiene en cuenta `temporal~` y `Hola.class`.

Con el `.gitignore` puesto, realizamos lo siguiente:
```bash
$ git add .

$ git commit -m "Añadidos fuentes en java y archivos importantes"

[master 977fc31] Añadidos fuentes en java y archivos importantes
 3 files changed, 13 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 Hola.java
 create mode 100644 importante~
```

Con el punto le indicamos a add que añada todo lo pendiente, incluyendo subdirectorios.

Mediante `$ git status` veremos que está todo en regla.

Para ver los archivos que hay en el directorio tras el último commit en la rama principal haremos lo siguiente:
```bash
$ git ls-tree --name-only -r HEAD

.gitignore
Hola.java
importante~
script.sh
texto.txt
```

## 7. TAGS Y GESTIÓN DE VERSIONES
Los identificadores únicos (*hash*) asignados a cada *commit* en `git log` son códigos complejos para referenciar versiones de forma rápida. Para simplificar la gestión del historial, se emplean **etiquetas (tags)** que asignan nombres descriptivos a puntos clave del proyecto.

### 7.1 Creación de etiquetas (`git tag`)
Para etiquetar el estado actual del proyecto (el último *commit* realizado), ejecutamos:
```bash
$ git tag v0.7
```

Si deseamos etiquetar una versión previa del historial, indicamos el nombre de la etiqueta junto a las primeras cifras del hash correspondiente:
```bash
$ git tag v0.3 cifras_del_hash
```
### 7.2 Consulta de versión y registro
Para inspeccionar el historial con los identificadores abreviados y la localización de las etiquetas, utilizamos el parámetro `--oneline`:
```bash
$ git log --oneline

977fc31 (HEAD -> master, tag: v0.3) Añadidos fuentes en java y archivos importantes
2e01c6b Probando diff 3
af41333 Probando diff 2
002fb80 Probando diff 2
5dd19a3 Probando diff
5e68276 Probando diff
f47f803 Ampliada la explicación del texto
9d919e6 Añadido archivo texto.txt
88e0a75 Confirmación inicial
```

Para consultar los detalles y cambios introducidos en una versión etiquetada sin escribir su hash largo:
```bash
$ git show v0.3

commit 977fc3177cdc98509380f5d94566fb695956bb4a (HEAD -> master, tag: v0.3)
Author: alejandra <gonzalezgranaalejandra@gmail.com>
Date:   Mon Oct 26 09:53:43 2026 +0100

    Añadidos fuentes en java y archivos importantes

diff --git a/.gitignore b/.gitignore
new file mode 100644
index 0000000..d8344a1
--- /dev/null
+++ b/.gitignore
@@ -0,0 +1,8 @@
+# Para que ignore los archivos terminados en .class
+*.class
+
+# Para que ignore los archivos terminados en ~
+*~
+
+# Para que no ignore uno concreto que acabe en ~ (aunque en la línea anterior se haya declarado que se ignoren todos)
+!importante~
diff --git a/Hola.java b/Hola.java
new file mode 100644
index 0000000..7f8cda8
--- /dev/null
+++ b/Hola.java
```
Para salir presionamos la tecla `q`.
### 7.3 Navegación entre versiones
Para desplazarnos temporalmente a una versión anterior y revisar el estado del proyecto en ese instante, utilizamos `git checkout` especificando la etiqueta o el hash:
```bash
$ git checkout v0.3
Note: switching to 'v0.3'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 977fc31 Añadidos fuentes en java y archivos importantes
```
Una vez finalizada la revisión, para regresar a la última versión en la rama principal de trabajo ejecutamos:
```bash
$ git checkout master
Switched to branch 'master'
```
**NOTA**: En caso de que la rama principal se haya denominado de otra forma, se indicaría git checkout nombre_rama.

Los tags se almacenan en el directorio `.git/refs/tags`.

## 8. RAMAS
Las ramas permiten desarrollar líneas de trabajo paralelas a la rama principal con el objetivo de realizar pruebas o implementar nuevas funcionalidades sin riesgo de alterar el código estable del proyecto. Posteriormente, las ramas secundarias se pueden abandonar, eliminar o fusionar (*merge*) con la rama principal.

### 8.1. Creación de una nueva rama
Para crear una rama secundaria denominada `Prueba`, ejecutamos:
```bash
\$ git branch Prueba
```
Comprobamos la estructura del historial mediante `git log --oneline`:
```bash
$ git log --oneline

977fc31 (HEAD -> master, tag: v0.3, Prueba) Añadidos fuentes en java y archivos importantes
2e01c6b Probando diff 3
af41333 Probando diff 2
002fb80 Probando diff 2
5dd19a3 Probando diff
5e68276 Probando diff
f47f803 Ampliada la explicación del texto
9d919e6 Añadido archivo texto.txt
88e0a75 Confirmación inicial
```
Para listar todas las ramas del repositorio e identificar la rama activa (marcada con un asterisco *), ejecutamos git branch:
```bash
$ git branch
  Prueba
* master
```
### 8.2 Cambio de contexto a la rama secundaria
Para desplazarnos a la rama `Prueba` y comenzar a trabajar en ella, ejecutamos el comando `git switch` (o en su defecto `git checkout`):
```bash
\$ git switch Prueba
Switched to branch 'Prueba'
```
Al verificar de nuevo con git branch, se observa que el puntero activo HEAD se ha situado sobre la nueva rama:
```bash
$ git branch
* Prueba
  master
```
Ejecuta `git log --online` para asegurarte de que HEAD apunta a Prueba.
```bash
$ git log --oneline
977fc31 (HEAD -> Prueba, tag: v0.3, master) Añadidos fuentes en java y archivos importantes
2e01c6b Probando diff 3
af41333 Probando diff 2
002fb80 Probando diff 2
5dd19a3 Probando diff
5e68276 Probando diff
f47f803 Ampliada la explicación del texto
9d919e6 Añadido archivo texto.txt
88e0a75 Confirmación inicial
```
Ahora `HEAD` apunta a `Prueba`, por lo que todo *commit* que realicemos a partir de este momento se guardará exclusivamente en dicha rama, dejando intacta la rama principal (`master`). 

Puedes comprobar el contenido del archivo `.git/HEAD` con `cat .git/HEAD` y verás que efectivamente apunta a la nueva rama.
```bash
$ cat .git/HEAD
ref: refs/heads/Prueba
```
### 8.3. Desarrollo y confirmación en la rama paralela
Realizamos las modificaciones en la rama `Prueba` y confirmamos los cambios en el repositorio:

```bash
$ git commit -a -m "Rama para pruebas de código"
```
Consultamos el registro abreviado mediante `git log --oneline` para verificar que la rama `Prueba` ha avanzado de forma independiente respecto a `master`:
```bash
$ git log --oneline
977fc31 (HEAD -> Prueba, tag: v0.3, master) Añadidos fuentes en java y archivos importantes
2e01c6b Probando diff 3
af41333 Probando diff 2
002fb80 Probando diff 2
5dd19a3 Probando diff
5e68276 Probando diff
f47f803 Ampliada la explicación del texto
9d919e6 Añadido archivo texto.txt
88e0a75 Confirmación inicial
```
Podemos ver en el log que ahora HEAD apunta a `Prueba` por lo que todos los cambios serán en paralelo con la rama `master` no afectando a esta.  Esto significa que los cambios que hagas en cualquier archivo no afectarán a la rama principal.

Probaremos a cambiar el `Hola.java` añadiéndole la siguiente línea:
```java
System.out.println("I have no more branches to commit, said the Ent");
```
Hacemos el *commit* de la modificación realizada dentro de la rama `Prueba`:
```bash
$ git commit -a -m "Llegan los Ents a Java"
[Prueba d284cf8] Llegan los Ents a Java
 1 file changed, 1 insertion(+)
```
Ahora consultaremos nuevamente el registro con `git log --oneline` para verificar que la rama `Prueba` ha avanzado un *commit* por delante de `master`:
 ```bash
$ git log --oneline
d284cf8 (HEAD -> Prueba) Llegan los Ents a Java
977fc31 (tag: v0.3, master) Añadidos fuentes en java y archivos importantes
2e01c6b Probando diff 3
af41333 Probando diff 2
002fb80 Probando diff 2
5dd19a3 Probando diff
5e68276 Probando diff
f47f803 Ampliada la explicación del texto
9d919e6 Añadido archivo texto.txt
88e0a75 Confirmación inicial
```
### 8.4. Aislamiento y fusión de cambios
Regresamos a la rama principal `master` para comprobar que las modificaciones realizadas en la rama `Prueba` no la han afectado:
```bash
$ git switch master
Switched to branch 'master'
```
Con el cat comprobamos que en la rama `master` el archivo `Hola.java` se mantiene intacto en su versión previa, demostrando el aislamiento del entorno de trabajo.
```bash
$ cat Hola.java
class Hola {
    public static void main(String[] args) {
        System.out.println("Welcome to the Java World");
    }
}
```
Una vez validada la modificación en la rama secundaria, incorporamos los cambios de `Prueba` a la rama principal `master` mediante la instrucción `git merge`:
```bash
$ git merge Prueba
Updating 977fc31..d284cf8
Fast-forward
 Hola.java | 1 +
 1 file changed, 1 insertion(+)
```
**Updating 977fc31..d284cf8** muestra el desplazamiento del puntero desde el commit original en `master` hasta el nuevo commit en `Prueba`.

**Fast-forward** indica que Git ha realizado una fusión rápida avanzando linealmente hasta el puntero de `master` hasta alcanzar a `Prueba`, al no haberse producido *commits* divergentes en la rama principal durante el desarrollo.

## 9. ELIMINAR Y QUITAR DE SEGUIMIENTO
Si deseas eliminar archivos lo mejor es hacerlo a través del comando:
```bash
$ git rm importante~
```
Y luego un *commit*.

Si hemos eliminado el archivo previamente mediante el comando rm normal, será necesario que ejecutemos también `git rm` para registrar la eliminación. Si Git nos indica que el archivo tiene modificaciones que impiden eliminarlo, se puede utilizar `git rm -f`, teniendo en cuenta que esta opción fuerza la eliminación y puede descartar cambios.
```bash
$ rm importante~
$ git rm importante~
```

Si había algún archivo ya en seguimiento que deseamos quitar (por ejemplo los .class), para sacarlos de la zona de seguimiento (HEAD) ejecutaremos:
```bash
$ git reset HEAD *.class
```
Este comando quita esos archivos de la zona de preparación, pero no los elimina del directorio de trabajo. Si los archivos ya estaban confirmados en un commit, seguirán formando parte del historial del repositorio.
## 10. REPOSITORIOS REMOTOS: GITHUB
El control de versiones distribuido nos permite trabajar en equipo mediante repositorios remotos, sincronizando los cambios de los repositorios locales. GitHub es una plataforma web que nos permite alojar repositorios Git públicos y privados.

Para descargar una copia exacta de un proyecto público alojado en GitHub en nuestro equipo local, no es necesario que dispongamos de una cuenta. Basta con ejecutar la orden `git clone` indicando la URL del repositorio:

```bash
$ git clone https://github.com/ColegioVivasCurro/HolaED

Cloning into 'HolaED'...
remote: Enumerating objects: 20, done.
remote: Total 20 (delta 0), reused 0 (delta 0), pack-reused 20 (from 1)
Receiving objects: 100% (20/20), done.
Resolving deltas: 100% (8/8), done.
```
Podemos comprobar que se nos ha creado un directorio denominado `HolaED`.

Esto clona el repositorio remoto en el directorio local, creando una copia para trabajar y probar proyectos de GitHub en nuestro ordenador.

---
**PASOS PARA ENLACE DE REPOSITORIO LOCAL CON REMOTO**

Para disponer de un nuevo repositorio para un proyecto lo primero que debemos hacer es crear el proyecto en nuestra cuenta de GitHub para luego sincronizar los archivos locales con el remoto:

Una vez tengamos creado el repositorio en la plataforma, copiaremos la URL proporcionada y vincularemos nuestro repositorio local con el servidor remoto, asignándole el alias convencional `origin`:

```bash
$ git remote add origin https://github.com/alegglezz/practica-guiada-git
```
Para comprobar que la vinculación con el repositorio remoto se ha realizado correctamente, ejecutamos git remote -v:
```bash
$ git remote -v
origin  https://github.com/alegglezz/practica-guiada-git (fetch)
origin  https://github.com/alegglezz/practica-guiada-git (push)
```

A continuación, subiremos las ramas `main` y `Prueba` al remoto:
```bash
$ git push -u origin master

info: please complete authentication in your browser...
Enumerating objects: 32, done.
Counting objects: 100% (32/32), done.
Delta compression using up to 12 threads
Compressing objects: 100% (23/23), done.
Writing objects: 100% (32/32), 3.10 KiB | 529.00 KiB/s, done.
Total 32 (delta 2), reused 0 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (2/2), done.
remote:
remote: Create a pull request for 'master' on GitHub by visiting:
remote:      https://github.com/alegglezz/practica-guiada-git/pull/new/master
remote:
To https://github.com/alegglezz/practica-guiada-git
 * [new branch]      master -> master
branch 'master' set up to track 'origin/master'.

$ git push -u origin Prueba
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
remote:
remote: Create a pull request for 'Prueba' on GitHub by visiting:
remote:      https://github.com/alegglezz/practica-guiada-git/pull/new/Prueba
remote:
To https://github.com/alegglezz/practica-guiada-git
 * [new branch]      Prueba -> Prueba
branch 'Prueba' set up to track 'origin/Prueba'.
```

---
**OTRAS SITUACIONES:**
1. Si tu rama no fuera main, puedes renombrarla antes del push con:
```bash
$ git branch -M main
```
2. En el caso de que ya hubiera archivos en el repositorio remoto, primero los debemos descargar antes de subirlos todos:
```bash
$ git pull origin main --no-rebase --allow-unrelated-histories
```
Con esto traemos los archivos del remoto y los unimos con los nuestros haciendo un `merge`.

3. Si ya lo tuviéramos enlazado a un repositorio remoto y necesitásemos cambiarlo a otro, podríamos hacer:
```bash
$ git remote set-url origin https://github.com/alegglezz/otro_repositorio.git
```
4. También podríamos borrarlo directamente: 
```bash
$ git remote remove origin
```
5. Podemos ver los enlaces remotos con:
```bash
$ git remote -v
```