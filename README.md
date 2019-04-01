# Turizem v Sloveniji (vmesno poročilo)
### Opis problema

Ukvarjal se bom z analizo turizma v Sloveniji med letoma 2007 in 2017. Ključna vprašanja na katera bom odgovarjal so:
- kako se je število turistov spreminjalo v obdobju 10 let
- v katerih občinah turistična aktivnost raste in v katerih vpada
- kako je turistična obremenitev porazdeljena po mesecih in kako po občinah

### Podatki
Uporabljal bom 3 datoteke:
- turisti po državah (podatki o številu turistov in številu nočitev za vsako državo za obdobje 10 let)
- turisti po občinah (podatki o številu turistov za vsako državo in za vsako občino po mesecih)
- turisti po občinah brez držav (podatki o številu turistov za vsako občino po mesecih)

Podatki so na voljo na:
- https://podatki.gov.si/dataset/surs2164408s
- https://podatki.gov.si/dataset/surs2108102s

### Osnovne vizualizacije
```python
from csv import DictReader
drzave = []
turisti    = [[], [], [], [], [], [], [], [], [], [], []]
prenocitve = [[], [], [], [], [], [], [], [], [], [], []]

reader = DictReader(open('podatki/turisti_po_drzavah.csv', 'rt', encoding='latin1'))
first = True
for row in reader:
    drz = row["drzava"]
    if drz == "Drzava potovanja  - SKUPAJ":
        continue
    drzave.append(drz)
    i = 7
    while i < 18:
        turisti[i-7].append(int(row["tur" + str(i)]))
        prenocitve[i-7].append(int(row["pren" + str(i)]))
        i += 1          
        
import matplotlib.pyplot as plt
import numpy as np
from operator import itemgetter
data = []
for i in range(len(drzave)):
    data.append((drzave[i], turisti[0][i]))

data.sort(key=itemgetter(1))  
a = []
b = []
for id, key in data:
    a.append(id)
    b.append(key)

plt.figure(figsize=(20,23))   
plt.barh(np.arange(len(data)), b, tick_label=a)
plt.show()
```
Zgornja koda prebere .csv datoteko in izriše horizontalni stolpični diagram.



