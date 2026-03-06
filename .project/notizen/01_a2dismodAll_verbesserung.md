# a2dismodAll Verbesserungs-Vorschlag

Das aktuell statische Skript `a2dismodAll` deaktiviert hartcodierte PHP-Versionen. 
Um es zukunftssicher und robuster zu machen, kann man dynamisch nach aktivierten PHP-Modulen suchen und diese deaktivieren.

## Aktueller Code
```bash
#!/bin/bash
sudo a2dismod php7.2
sudo a2dismod php7.4
#...
```

## Bessere Methode (Dynamisch)
Man kann `a2query` verwenden, um alle aktuell aktivierten Module auszulesen und nur die PHP-Module zu deaktivieren:

```bash
#!/bin/bash
# Sucht alle aktivierten PHP-Module und deaktiviert sie
a2query -mq | awk '{print $1}' | grep -E '^php[0-9.]+$' | xargs -r sudo a2dismod
```

### Alternative (Über mods-enabled)

```bash
#!/bin/bash
for mod in /etc/apache2/mods-enabled/php*.load; do
    if [ -f "$mod" ]; then
        mod_name=$(basename "$mod" .load)
        sudo a2dismod "$mod_name"
    fi
done
```
Diese Ansätze sind flexibel und erfordern keine Anpassung bei neuen PHP-Versionen.
