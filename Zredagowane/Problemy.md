wzajemne wykluczenie
---
Zsynchronizować procesy, z których każdy w nieskończonej pętli na przemian zajmuje się własnymi zasobami oraz zasobem współdzielonym, w taki sposób, aby podczas tego drugiego, żaden inny proces nie przeprowadzał na takowym operacji

Rozwiązanie powyższego problemu wymaga wprowadzania dodatkowych instrukcji poprzedzających oraz występujących po zakończeniu sekcji krytycznej.

producent - konsument
---
Występują w nim dwa rodzaje procesów: producent i konsument, którzy dzielą wspólny zasób - bufor dla produkowanych/konsumowanych jednostek. Zadaniem producenta jest wytworzenie produktu, umieszczenie go w buforze i rozpoczęcie pracy od nowa. W tym samym czasie konsument ma pobrać produkt z bufora. Problemem jest taka synchronizacja procesów, żeby producent nie dodawał nowych jednostek gdy bufor jest pełny, a konsument nie pobierał gdy bufor jest pusty.

Rozwiązanie - korzystając z semaforów możemy:
Utworzyć dwa: po jednym dla konsumenta i producenta. Producent, kiedy będzie chciał wykonać operację na buforze zgłosi żądanie wyłączności, wykonując wait() na semaforze własnym). Natomiast po zakończeniu tej operacji zwolni (ewentualnie) oczekującego konsumenta wykonując signal() na jego semaforze..

pisarze - czytelnicy
---
Dotyczy on dostępu do jednego zasobu dwóch rodzajów procesów: dokonujących w nim zmian (pisarzy) i nie dokonujących w nim zmian (czytelników). Jednoczesny dostęp do zasobu może uzyskać dowolna liczba czytelników. Pisarz może otrzymać tylko dostęp wyłączny. Równocześnie z pisarzem dostępu do zasobu nie może otrzymać ani inny pisarz, ani czytelnik. 

Rozwiązania - korzystając z semaforów możemy:
- faworyzacja czytelników - Czytelnicy nie czekają na dostęp, jeśli w danym momencie nie otrzymał go pisarz. Ponieważ pisarz może otrzymać tylko dostęp wyłączny, istnieje ryzyko zagłodzenia jego procesu.
- faworyzacja pisarzy – Czytelnicy nie otrzymują dostępu, jeżeli oczekuje na niego pisarz. W tej sytuacji oczekujący pisarz otrzymuje dostęp najwcześniej, jak to jest możliwe, co stwarza ryzyko zagłodzenia czytelników.
Można również wykorzystać inne mechanizmy np. kolejkę FIFO dla znalezienia innych rozwiązań.

ucztujący filozofowie
---
Przyjmijmy scenariusz, w którym filozofowie siedzą przy okrągłym stole i każdy wykonuje jedną z dwóch czynności – albo je, albo rozmyśla. Przed każdym z nich znajduje się miska z ryżem, a pomiędzy każdą sąsiadującą parą filozofów leży pałeczka, a więc każdy z nich ma przy sobie po dwie – po lewej i stronie. Aby jeść, filozof musi korzystać z dwóch pałeczek. Są to zatem współdzielone zasoby, do których ma dostęp kilka procesów. 

Rozwiązania - korzystając z semaforów możemy:
- natychmiastowo przechwycić 'swoją' pałeczkę, następnie dopiero sięgając po drugą; może jednak dojść do *zakleszczenia*, gdy wszyscy postąpią identycznie w tym samym momencie próbując uzyskać dostęp do zasobu
- oczekiwać na moment w którym można podnieść dwie pałeczki naraz; stwarza to jednak ryzyko wystąpienia *zagłodzenia* w oczekiwaniu na taki moment