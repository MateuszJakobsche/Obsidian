Przetwarzanie współbieżne
---
- iteracyjne: procesy zawierają pętle
- rekurencyjne: niezależne wywołania procedur rekurencyjnych na różnych częściach wspólnych danych
- pipelines: komunikacja zorganizowana w potoki, procesy otrzymują wyjścia poprzedników i produkują wejścia następników
- klient-serwer: serwer czeka > klient wysyła > serwer odpowiada
- interaktywne komunikaty: wymiana informacjami między procesami dla realizacji zadań

Modele współbieżności
---
- Scentralizowany - zmienne globalne (wady i zalety), niepodzielność instrukcji wysokopoziomowych, mechanizmy synchronizacyjne
- Rozproszony - wiele komputerów lub odrębne logicznie jednostki, patrz : [[Komunikacja]]

---

Programy
---
- sekwencyjne: kolejne akcje wykonywane po zakończeniu poprzednich
- współbieżne: różne akcje wykonywane (częściowo) w tym samym czasie:
	- w przeplocie: kolejne wykonywane akcje z *różnych* rozpoczętych czynności
	- równolegle: wiele akcji w tym samym czasie (np. różne procesory)



