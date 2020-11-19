# Bash Help

## `set` switches

```text
-b soubor               Existuje a je to blokový speciální soubor.
-c soubor               Existuje a je to znakový speciální soubor.
-d soubor               Existuje a je to adresář.
-e soubor               Existuje.
-f soubor               Existuje a je to normální soubor.
-g soubor               Existuje a má právo set-group-id.
-k soubor               Existuje a má nastavený sticky bit.
-L soubor               Existuje a je to symbolický odkaz.
-p soubor               Existuje a je to pojmenovaná roura (FIFO).
-r soubor               Existuje a je čitelný.
-s soubor               Existuje a má délku větší než nula.
-S soubor               Existuje a je to soket.
-u soubor               Existuje a má nastaven set-user-id bit.
-w soubor               Existuje a je zapisovatelný.
-x soubor               Existuje a je proveditelný.
-O soubor               Existuje a je vlastněný efektivním user id.
-G soubor               Existuje a je vlastněný efektivním group id.
soubor1 -nt soubor2     Pravda, když je soubor1 novější než soubor2.
soubor1 -ot soubor2     Pravda, když je soubor1 starší než soubor2.
soubor1 -ef soubor2     Pravda, když soubor1 a soubor2 mají shodný inode
                        na stejném disku.
-z řetězec              Pravda, když je řetězec prázdný.
-n řetězec              Pravda, když je délka řetězce nenulová.
řetězec1 = řetězec2     Pravda, když řetězce jsou stejné.
řetězec1 != řetězec2    Pravda, když řetězce nejsou stejné.
! výraz                 Pravda, když výraz je nepravdivý.
výraz1 -a výraz2        Pravda, když jak výraz1 tak výraz2 jsou pravdivé.
výraz1 -o výraz2        Pravda, když je aspoň jeden z výrazů výraz1 nebo výraz2 pravdivý.
```
