Własności:
---
- **bezpieczeństwa** - nigdy nie dojdzie do niepożądanej sytuacji
- **żywotność** - w końcu dojdzie do pożądanej sytuacji:
	- z jej braku wynikają problemy:
		- zakleszczenia (globalnie) - system oczekuje na sytuacje do której nigdy nie dojdzie
		- zagłodzenia (lokalnie) - proces oczekuje na sytuację do której nigdy nie dojdzie

---

**zasób współdzielony** - obiekt z którego może korzystać kilka procesów
**sekcja krytyczna** - ten fragment procesu, w którym korzysta on z zasobu współdzielonego

---

**Dekompozycja funkcjonalna** - podział na zadania które realizuje przydzielone mu funkcje. np. Sito Erastotenesa
**Dekompozycja spekulatywna** - podział celem znalezienia najbardziej optymalnej ścieżki wykonania zadania. np. Quicksort, pełzający sympleks

**Dekompozycja gruboziarnista** - podział na mało dużych zadań
**Dekompozycja drobnoziarnista** - podział na dużo małych zadań

---
**równoważenie obciążenia** - każdy proces wykonuje tyle samo obliczeń, aby nie oczekiwały na zakończenie swoich działań
