Zakłada, że część programu można przyspieszyć p-krotnie wykorzystując p procesorów, lecz reszta programu nie ulegnie przyspieszeniu.

Przyspieszenie:
t(s) - całkowity czas sekwencyjnego wykonania programu
f - stosunek czasu fragmentu nie dającego się przyspieszyć do całkowitego czasu
p - ilość procesów

**S(p)** = t(s)/(ft(s) + (1 - f)t(s) / p) = **p / 1 + (p - 1)f**
dla : limS(p) = 1/f : nie można osiągnąć większego przyspieszenia.

W rzeczywistości skalowalność jest jeszcze gorsza, przez:
- koszty komunikacji procesów, 
- niezrównoważenie obciążenia, 
- duplikację obliczeń na różnych procesorach, 
- niespełnienie założenia o stałym rozmiarze problemu (przez różnice sprzętowe).

