# Codigo 98: Geo Loc

## Funcionalidad
Da las coordenadas de una imagen con geo localizacion

### **Requerimientos**
Una imagen con datos de geoloc

### **Anotaciones**
Funciono directamente

### **[Codigo 98: Geo Loc](geoLoc98.sh)**

```bash
#!/bin/bash

tempfile="/tmp/geoloc.$$"

trap "`which rm` -f $tempfile" 0 1 15

if [ $# -eq 0 ] ; then
  echo "Usage: $(basename $0) image" >&2 ; exit 1
fi

for filename
do
  identify -format "%[EXIF:*]" "$filename" | grep GPSL > $tempfile

  latdeg=$(head -1 $tempfile | cut -d, -f1 | cut -d= -f2)
  latdeg=$(scriptbc -p 0 $latdeg)
  latmin=$(head -1 $tempfile | cut -d, -f2)
  latmin=$(scriptbc -p 0 $latmin)
  latsec=$(head -1 $tempfile | cut -d, -f3)
  latsec=$(scriptbc $latsec)
  latorientation=$(sed -n '2p' $tempfile | cut -d= -f2)

  longdeg=$(sed -n '3p' $tempfile | cut -d, -f1 | cut -d= -f2)
  longdeg=$(scriptbc -p 0 $longdeg)
  longmin=$(sed -n '3p' $tempfile | cut -d, -f2)
  longmin=$(scriptbc -p 0 $longmin)
  longsec=$(sed -n '3p' $tempfile | cut -d, -f3)
  longsec=$(scriptbc $longsec)
  longorientation=$(sed -n '4p' $tempfile | cut -d= -f2)

  /bin/echo -n "Coords: $latdeg ${latmin}' ${latsec}\" $latorientation, "
  echo "$longdeg ${longmin}' ${longsec}\" $longorientation"

done

exit 0
```

### **Salidas del codigo**

![Salida.png](Salida.png)

**[<- Regresar](../README.md)**