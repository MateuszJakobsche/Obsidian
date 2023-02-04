1 proces
---
```c++
switch( fork() )
{
	case -1:
		//parent, gdy błąd
		break;
	case 0:
		//child
		break;
	default:
		//parent
}
```
2 Łącza nienazwane
---
```c++
//parametry: deskryptor odczyt / zapis, obszar pamięci, rozmiar

write( fd[1],"\n\t[pozdrowienia od potomka]\n\n", 29 );
close( fd[1] );
```
3 Łącza nazwane
---
```c++
//parametry: nazwa łącza, maska uprawnień

char pipe[]="xyz";
mkfifo( pipe,S_IRUSR|S_IWUSR )
open( pipe, O_RDONLY|O_NONBLOCK)
```
4 Kolejki komunikatów IPC
---
```c++
//parametry: [klucz] ścieżka + id, [flaga] działanie + tryb, [kontrola] id + akcja + zwrot wyniku

key_t key = ftok("/tmp",_PROJECT_ID);
int flag = IPC_CREAT | 0x100 | 0x80;
int msqid = msgget(key, flag);

struct msqid_ds buffer;
msgctl(msqid, IPC_RMID, &buffer)
```
5 Pamięć współdzielona IPC
---
```c++
//analogicznie do powyższego:
size = 1024*64;
id = shmget(key, size, flag);
shmctl(id, IPC_RMID, &buffer);
```
6 Wątki
---
```c++
//parametry: [tworzenie wątku] identyfikator + sposób dołączania / kolejkowania wątku + funkcja wątku + argumenty dla funkcji startowej, [łączenie wątków] id wątku na którego czekamy + kod powrotu wątku

pthread_t tid;
pthread_create( &tid, NULL, &o, NULL );

pthread_join ( tid, NULL );
```
7 Semafory (systemv) - zbiorowe
---
```c++
//parametry: [tworzenie tablicy semaforów] identyfikator + ilość + maska bitowa, [sembuf]: numer + operacja + flaga, [usuwanie semfora] id + numer semafora w zbiorze + cmd (i jej ewentualne parametry)

//tworzenie tablicy z pojedyńczym semaforem:
key_t key = ftok("/tmp", 'a'+'t'+'h');
int nsems = 1;
int semflag = IPC_CREAT | S_IRUSR | S_IWUSR;
semget(key, nsems, semflag);

//inicjowanie semafora binarnego wartością 1:
union semun control;
constrol.value = 1;
semctl(semid, 0x0, SETVAL, control);

//wykorzystanie semafora:
struct sembuf P={0,-1,0};
struct sembuf S={0,+1,0};
semop(semid,&P,nsems)
(sekcja krytyczna)
semop(semid,&V,nsems)

//usunięcie semafora:
semctl(semid,0x0,IPC_RMID)
```

8 Semafory (posix) - pojedyncze
---
```c++
//NAZWANE:
//parametry: nazwa, maska bitowa: działanie + uprawnienia, wartość

sem_t *id;
int counter = 7;
char name[] = "km";
id = sem_open(name,O_CREAT,S_IRUSR|S_IWUSR,(unsigned) counter);

sem_wait(id); (zmniejszenie wartości)
(sekcja krytyczna)
sem_post(id); (zwiększenie wartości)

sem_close(id);
sem_unlink(name);

//NIENAZWANE - analogicznie +:
//parametry: id, współdzielenie, wartość

sem_init(&id, 0, counter);
sem_destroy();
```

9 Semafory (mutex) - binarne
---
```c++
//ZWYKŁE:
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
pthread_mutex_lock(&mutex);
(sekcja krytyczna)
pthread_mutex_unlock(&mutex);
pthread_mutex_destroy(&mutex);

//WARUNKOWE - analogicznie +:
int go = 0;
pthread_cond_t cond = PTHREAD_COND_INITIALIZER;

//producent:
pthread_mutex_lock(&mutex);
go = 1;
pthread_mutex_signal(&mutex);
pthread_mutex_unlock(&mutex);

//konsument:
pthread_mutex_lock(&mutex);
while(!go){pthread_cond_wait(&cond,&mutex)}
pthread_mutex_unlock(&mutex);
```
9 OpenMPI
---
```c++
//parametry: wysyłana tablica, ilość elementów, nazwa typu, cel/źródło, liczba ident. rodzaj danych, komunikator, -/status;

#define MASTER 0
#define TAG 'A'+'B'+'C'
//v
double x;
MPI_Status status;
MPI_Init(&argc, &argv);
//v
x=4;
MPI_Send(&x, 1, MPI_DOUBLE, MASTER, TAG, MPI_COMM_WORLD);
//v
MPI_Recv(&x, 1, MPI_DOUBLE, 1, TAG, MPI_COMM_WORLD, &status);
//v
MPI_Finalize();

//pobranie informacji o procesach do określonych zmiennych:
int current,world;
mpi_Comm_size(MPI_COMM_WORLD,&world);
mpi_Comm_rank(MPI_COMM_WORLD,&current);

//wywołanie: mpirun -np 100 ./nazwa
```
10 TPL
---
```cs
//Pętla równoległa:
object _ = new object();

Parallel.For(0, ilosc, i =>
{
	ulong pom = dosmth();
	lock(x)
	{
		wynik += pom;
	}
});
//ALBO
Parallel.For(0, N, () => 0.0, (i, state, local) =>
{
	return local + dosmth();
}, local =>
{
	lock (_)
	{
		sum += local;
	}
});

//Użycie tasków:
var task1 = new Task(() =>
{
	dosmth(1);
});
var task2 = new Task(() =>
{
	dosmth(2);
});
task1.RunSynchronously();
task2.RunSynchronously();
//ALBO
task1.Start();task2.Start();
task1.Wait();task2.Wait();
```