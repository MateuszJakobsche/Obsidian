Definicja
---
mechanizm synchronizacji procesór; zmienna całkowita przyjmująca wartości nieujemne, którą można: 
- P - opuszczać (zmniejszyć wartość)
blokując wykonanie instrukcji na sekcji krytycznej przez proces
- V - podnosić (zwiększyć wartść)
umożliwiając oczekującym procesom wykonanie instrukcji

**klasyczna**:
Opuszczenie
	await (S > 0) S = (w zależności od semafora *false* lub *-liczba*)
Podniesienie
	S = (w zależności od semafora *true* lub *+liczba*)

**praktyczna**
Opuszczenie
	if (S > 0) S = (w zależności od semafora *false* lub *-liczba*)
Podniesienie
	if (procesy wstrzymane podczas opuszczania) wznów jeden;
	else S = (w zależności od semafora *true* lub *+liczba*)

jeżeli mamy do czynienia z dwustronie ograniczonym, S należy do przedziału (0, N), a przed podniesieniem sprawdzamy *if (S == N) wait*, zaś przed opuszczeniem *if (S== 0) wait*

Rodzaje
---
**Binarny:** zmienna przyjmuje wartości (t/f), zwykle nie można go dwukrotnie podnieść (error)
**Ogólny:** zmienna przyjmuje wartości naturalne (+1 / -1 > 0)
**Dwustronnie ograniczony:** jak wyżej, ale z również z górną granicą
**Uogólniony:** zmienną można modyfikować o dowolną wartość (+x / -x > 0)