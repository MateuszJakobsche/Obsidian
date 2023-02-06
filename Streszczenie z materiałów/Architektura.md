**skróty**
j - jednostka, u - urządzenia

Von Neuman
---
cztery główne komponenty komputera:
-pamięć (dane, instrukcje programu, adresowane komówki pamięci)
-j. sterująca (pobieranie z pamięci, przetwarzanie sekwencyjne)
-j. arytmetyczno-logiczna (operacje)
-u. wejścia i wyjścia.

---

Flynn
---
instruction - program
SISD (single instruction przetwarza single data)
SIMD (single instruction przetwarza multiple data)
MISD (multiple instruction przetwarza single data)
MIMD (multiple instruction przetwarza multiple data)

---

Architektury systemowe
---
**Systemy wielozadaniowe:** rozkazy wykonywane sekwencyjne (o kolejności decyduje procesor i system operacyjny), u. we/wy
**Multiprocesory ze wspólną pamięcią:** kilka procesorów, każdy ma pamięć lokalną + wspólną globalną, można przypisać do nich procesy (brak przeplotu), przy korzystaniu tylko z pam. lokalnej - możliwe obliczenia równoległe
**Architektura rozproszona:** wiele komputerów bez wspólnych zasobów, z kanałami komunikacyjnymi, rozproszone geograficznie, brak pam. globalnej; topologie: np. pełna, pierścienia etc.