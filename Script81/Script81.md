# Codigo 81: ITunes List

## Funcionalidad
Imprime todas las subcarpetas de tu libreria ITunes

### **Requerimientos**
No tiene requerimientos

### **Anotaciones**
Como yo no tengo ITunes imprimio todo root

### **[Codigo 81: ITunes List](itunesList81.sh)**

```bash
#!/bin/bash

itunehome="$HOME/Music/iTunes"
ituneconfig="$itunehome/iTunes Music Library.xml"

musiclib="/$(grep '>Music Folder<' "$ituneconfig" | cut -d/ -f5- | \
   cut -d\< -f1 | sed 's/%20/ /g')"

echo "Your library is at $musiclib"

if [ ! -d "$musiclib" ] ; then
  echo "$0: Confused: Music library $musiclib isn't a directory?" >&2
  exit 1
fi

exec find "$musiclib" -type d -mindepth 2 -maxdepth 2 \! -name '.*' -print |
   sed "s|$musiclib/||"
```

### **Salidas del codigo**

![Salida.png](Salida.png)

**[<- Regresar](../README.md)**