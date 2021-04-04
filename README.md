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