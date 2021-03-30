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