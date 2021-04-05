# Introducción a Git y GitHub

## Control de versiones
### Comparando archivos

```python
diff rearrange1.py rearrange2.py

Resultado:

Linea eliminada <
<     result = re.search(r"^([\w .]*), ([\w .]*)$", name) 
---
Linea agregada >
>     result = re.search(r"^([\w .-]*), ([\w .-]*)$", name)

```
Ejemplo con más lineas modificadas y otro formato

```python
diff -u rearrange1.py rearrange2.py

Resultado:

--- rearrange1.py       2021-03-30 12:47:47.274659100 -0500
+++ rearrange2.py       2021-03-30 12:56:38.553055300 -0500
@@ -1,7 +1,10 @@
 import re

 def rearrange_name(name):
-    result = re.search(r"^([\w .]*), ([\w .]*)$", name)
+    result = re.search(r"^([\w .-]*), ([\w .-]*)$", name)
     if result == None:
         return result
+    pass
+    pass
+    pass
     return "{} {}".format(result[2], result[1])
\ No newline at end of file
```

Se puede generar un archivo .diff el cual contiene los cambios capturados por el comando diff-u
```
diff -u rearrange1.py rearrange2.py > diferencias.diff
```

De igual forma, se puede leer un archivo .diff para aplicar los cambios a un archivo específico:
```
patch cpu_usage.py < cpu_usage.diff
```

# Sistemas de control de versiones

## Commit:
Conjunto de cambios en múltiples archivos agrupados en un "único cambio". Se registra que se hizo en el conjunto de cambios.

 # Git

 Configuración global
 ```
 git config --global user.email "me@example.com"
git config --global user.name "myname"
 ```
  --global guarda estas credenciales para todos los repositorios.
(Puede configurarse una credencial por repositorio)


 Iniciando un repositorio de manera local
 ```
 git init
  ``` 
 Crea una carpeta oculta .git. 
 Se verifica su contenido con 
 ls -l .git/


 Working tree:

 Directorio actual donde se inició el repo. Contiene todos los archivos que son y no son rastreados por git.

 Hacer seguimiento a un archivo:

 ```
 git add <file_name>
 ```

 Staging area (index)
 "Area de puesta en escena". Es un archivo manejado por git que contiene la info de que archivos (y sus cambios) van a hacer parte del siguiente commit.

Commit (Confirmación): 

Es momento de guardar los cambios:
 ```
 git commit
 ```
 Se abre un editor de texto (en cmder) para agregar el mensaje del commit. Para guardar el mensaje: Esc -> :wq -> Enter.

```
git commit -m "Short commit message"
```
## Seguimiento de archivos
1. Se modifica el archivo.
2. Se añaden al staging area (git add).
3. Se confirman los cambios (git commit).

## Flujo de trabajo básico con Git
1. Iniciar un repositorio
```
git init
```
2. Revisar la configuración actual de git
```
git config -l
```
Principalmente para ver el nombre y correo configurados.

3. Crear un archivo

```
code all_checks.py
```
4. Revisar el estado actual del repositorio
```
git status
```
5. Hacer seguimiento a un archivo (staging area)
```
git add <filename>
```
6. Confirmar los cambios
```
git commit
git commit -m "commit message"
```

## Consejos para el mensaje del commit
1. Descripción muy breve de los cambios realizados
2. Linea vacia
3. Explicación detallada. Puede incluir enlaces a info adicional, hacer los cambios de linea cada 70 caracteres para que sea legible el mensaje.

Ver el historial de commits realizados
```
git log
```
# Usando Git localmente
## Omitiendo el staging area
```
git commit -a
```
Atajo para poner en el stage area y hacer el commit de cambios dearchivos que ya tienen seguimiento.
```
git commit -a -m "Commit message"
```
Git utiliza el alias **HEAD** para representar la instantánea actual del proyecto.

## Información adicional de los commits
La bandera -p permite ver los cambios generados en el archivo de la misma forma que diff -u
```
git log -p

En cmder al ver : se sale con q
``` 
El comando git log -p muetras mucha información. Existe la siguiente alternativa:

```
git show <commit_id>
```

Información estadística básica de todos los commit:
```
git log --stat
```
### Rastreando diferencias en los archivos
Se puede verificar los cambios en un archivo o conjunto de archivos, **sin necesidad** de haber hecho un commit:
```
git diff
git diff <filename>
```
git diff sólo muestra cambios de archivos que no están en el staging area. Para ver estos cambios se usa --staged:
```
git diff --staged
git diff --staged <filename>
```
Se pueden ver los cambios antes de enviar un archivo al staging area:
```
git add -p
El editor de texto pide confirmación (y)
```
## Eliminando y cambiando el nombre a archivos
Se elimina un archivo del directorio de git:
```
git rm <filename>
```
Debe hacerse un commit para confirmar la eliminación.

Se renombra un archivo en el directorio de git:
```
git mv <filename>
```
Debe hacerse un commit para confirmar el cambio de nombre.

**Verificar 'deleted' y 'renamed' con git status**

## Ignorando archivos con .gitignore
Se espectifican las reglas para decirle a git que tipo de archivos o de carpetas no tener en cuenta.
```
echo .DS_STORE > .gitignore
```
Debe hacerse confirmación:
```
git add .gitignore
git commit -m "Add a gitignore file, ignoring .DS_STORE files."
```

https://training.github.com/downloads/github-git-cheat-sheet.pdf

## Deshaciendo cambios
### Antes de hacer un commit:
Retomar un archivo a su última versión, sin que esté en el staging area:
```
git checkout <filename>
git checkout -p <filename>
```
-p Muestra cambio por cambio y permite elegir si se retoma a ese punto o no.
### Revertir cambios que están en el staging area:
```
git reset HEAD <filename>
git restore --staged <filename>
```

### Modificando un commit:
Para modificar **el último** commit realizado, bien sea para modificar los archivos o para ajustar el mensaje:
```
git commit --amend
```
Se abre el editor de texto donde se puede modificar el mensaje y ver los archivos que se van a incluir.

**SOLO UTILIZARLO PARA ARREGLAR COMMITS LOCALES, NO PÚBLICOS.**

### Rollbacks - Retroceso:
Existen varias opciones:
```
git revert
git revert HEAD
```
Crea un commit "inverso" que anula al commit erroneo, anulando así sus efectos, esto para mantener intacto el historial de commits.
```
git log -p -2 -> Info detallada de los 2 últimos commit
```

### Identificando los commit:

Utilizando el commit_id, es posible hacer un rollback de un commit que no es el más reciente:
```
git revert <commit_id>
```
Ej: Deshacer el cambio de nombre de un archivo.

## Ramas y fusiones:
Una rama es un apuntador a un commit particular. También representa una linea de desarrollo independiente. La rama creada por defecto se llama Master.

### Creación de nuevas ramas:
Lista de las ramas existentes:
```
git branch
```
Crear una nueva rama:
```
git branch <name_of_the_new_branch>
```
Moverse a la nueva rama:
```
git checkout <name_of_the_new_branch>
```
Crear y moverse a la nueva rama simultáneamente:
```
git checkout -b <new_branch>
```
Al crear un nuevo archivo en una nueva rama, y al hacer su commit, se ve como esta nueva rama está "adelantada"(Ahead) respecto a la rama master:
```
λ git log -2
commit ecd04957fe546e49fe8ea1988d6176cfb839ecfb (HEAD -> even-better-feature)
Author: juangomez9619 <judgomezb@correo.udistrital.edu.co>
Date:   Sun Apr 4 19:15:27 2021 -0500

    Add an empty free_memory.py

commit f9d73f70548af243806d280efdacc7a7b8e035a1 (new-feature, master)
Author: juangomez9619 <judgomezb@correo.udistrital.edu.co>
Date:   Fri Apr 2 11:59:01 2021 -0500

    Add a comment to the function
```
### Trabajando con ramas
Al moverse entre ramas, git actualiza los archivos, espacios de trabajo y el HEAD presente en esa rama. Si se crea un archivo en una rama x y se listan los archivos en la rama master, este nuevo archivo no va a aparecer.

Para borrar una rama:
```
git branch -d <branch_name>
```
Si se quiere forzar el borrado de una rama, debido a que tiene cambios que no han sido fusionados a la rama maestra:
```
git branch -D <branch_name>
```

### Fusiones
Flujo de trabajo típico con ramas:
1. Crear una nueva rama.
2. Realizar la nueva experimentación en esta rama.
3. Fusionar los cambios de esta rama a la rama master.

Para hacer una fusión, estando en la rama maestra:
```
git merge <second-branch-name>
```
Existen dos algoritmos de fusión:
* Fast-forward: El historial de commits de ambas ramas no diverge. Git actualiza los head de ambas ramas y no ocurre como tal una fusión

![](FF_merge_1.PNG)
![](FF_merge_2.PNG)

* Three-way merge: Se da cuanto existe una divergencia entre las ramas que se van a fusionar. La divergencia se da cuando se hace un commit después de haber creado una nueva rama:


![](merge_1.PNG)

Git empata el historial de ambas ramas con un nuevo commit. Si los cambios realizados en ambas ramas son en archivos diferentes o en lugares distintos de un mismo archivo, git los une directamente. De lo contrario, se puede crear un conflicto en la fusión.

### Conflicto en una fusión:

Al hacer una fusión y generar un conflicto se ve el siguiente mensaje en consola:
```
λ git merge even-better-feature
Auto-merging free_memory.py
CONFLICT (content): Merge conflict in free_memory.py
Automatic merge failed; fix conflicts and then commit the result.
```
git status muestra información útil relacionada con el conflicto:
```
λ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   free_memory.py

no changes added to commit (use "git add" and/or "git commit -a")
```
El editor de texto muestra los puntos donde existe el conflicto. Se puede elegir dejar un cambio u otro o los dos. 

Luego de solucionar el conflicto:
```
git add <file_fixed_name>
```
Al ver git status de nuevo:

```
λ git status
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:
        modified:   free_memory.py
```

Por último se hace el commit para realizar la fusión.

```
git log --graph --oneline
```
Permite ver el historial de commits para apreciar como se realiza la fusión.

En caso de querer abortar un intento de fusión:
```
git merge --abort
```