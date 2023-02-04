1 proces
---
```c++
//FORK:
int status;
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
		wait(&status); //czeka na zakończenie childa
}

//INNE FUNKCJE:
getpid(); getppid(); //do informacji o id
execv( "./child",arg ); //lub execl() dla określonej z góry il. parametrów
exit(); atexit(func); //natychmiastowe zakończenie procesu oraz wywołanie funkcji przy takowym
```
2 Łącza nienazwane
---
```c++
//UŻYCIE (niskopoziomowe):
//parametry: deskryptor odczyt / zapis, obszar pamięci, rozmiar
write( fd[1], &x, sizeof(double) ); //piszemy
close( fd[1] ); //i zamykamy

//SKOJARZENIE STRUMIENIA Z PLIKIEM (wysokopoziomowe):
int fd[2];
FILE* stream;
//v
close( fd[0] ); //nie czytamy
stream = fdopen( fd[1], "w" ); //"w" od write
fprintf( stream, "\tAaaaa\n" );
fflush( stream );
close( fd[1] );
//v
close( fd[1] ); //nie piszemy
dup2( fd[0], STDIN_FILENO ); //kopia deskryptora na stdin
close(fd[0]);
```
3 Łącza nazwane
---
```c++
//użycie (niskopoziomowe):
char pipe[]="xyz";
//parametry: nazwa łącza, maska uprawnień
mkfifo( pipe, S_IRUSR|S_IWUSR )
//wykorzystanie:
in = open( pipe, O_RDONLY|O_NONBLOCK );
out = open( pipe, O_WRONLY );
write( out,message,PIPE_BUF );
read( in,message,PIPE_BUF );
close( out ); close( in );
//zamknięcie
unlink(pipe);
//też można korzystać z tych wszystkich f-cośtam (wysokopoziomowe), jak powyżej
```
4 Kolejki komunikatów IPC
---
```c++
//utworzenie kolejki
//parametry: ścieżka + id
key_t key = ftok("/tmp",_PROJECT_ID);
int flag = IPC_CREAT | 0x100 | 0x80;
//parametry: działanie + tryb
int msqid = msgget(key, flag);
//utworzenie wiadomości
struct {long type; char text[3]} message;
message.type = (long) pid;
message.text = "abc";
//wysyłanie / odbieranie
msgsnd(msqid,&message,3,IPC_NOWAIT);
msgrcv(msqid,&message,3,pid,IPC_NOWAIT|MSG_NOERROR);
//usunięcie
//parametry: id + akcja + zwrot wyniku
struct msqid_ds buffer;
msgctl(msqid, IPC_RMID, &buffer)
```
5 Pamięć współdzielona IPC
---
```c++
//analogicznie do powyższego:
size = n*sizeof( unsigned );
id = shmget(key, size, flag);
//przyłączenie, operacja i odłączenie
unsigned* array = (unsigned*)shmat( id,NULL,0 );
*(array+i)=1; //gdzie 'i' określa położenie wartości w segm. pamięci
shmdt(array);
//usunięcie
shmctl(id, IPC_RMID, &buffer);
```
6 Wątki
---
```c++
//jakaś funkcja wątku i zmienna dla identyfikatorów:
void o (){} 
pthread_t tid;
//istotne funkcje:
//parametry: identyfikator + sposób dołączania / kolejkowania wątku + funkcja wątku + argumenty dla funkcji startowej
pthread_create( &tid, NULL, &o, NULL );
pthread_exit( NULL );
//parametry: id wątku na którego czekamy + kod powrotu wątku
pthread_join ( tid, NULL );
```
7 Semafory (systemv) - zbiorowe
---
```c++
//tworzenie tablicy z pojedyńczym semaforem:
key_t key = ftok("/tmp", 'a'+'t'+'h');
int nsems = 1;
int semflag = IPC_CREAT | S_IRUSR | S_IWUSR;
//parametry: identyfikator + ilość + maska bitowa
semget(key, nsems, semflag);
//inicjowanie semafora binarnego wartością 1:
union semun control;
constrol.value = 1;
semctl(semid, 0x0, SETVAL, control);
//wykorzystanie semafora:
//parametry: numer + operacja + flaga,
struct sembuf P={0,-1,0};
struct sembuf S={0,+1,0};
semop(semid,&P,nsems)
//(sekcja krytyczna)
semop(semid,&V,nsems)
//usunięcie semafora:
//parametry: id + numer semafora w zbiorze + cmd (i jej ewentualne parametry)
semctl(semid,0x0,IPC_RMID)
```

8 Semafory (posix) - pojedyncze
---
```c++
//NAZWANE:
sem_t *id;
int counter = 7;
char name[] = "km";
//parametry: nazwa, maska bitowa: działanie i uprawnienia, wartość
sem_open(name,O_CREAT,S_IRUSR|S_IWUSR,(unsigned) counter);
sem_wait(id); (zmniejszenie wartości)
//(sekcja krytyczna)
sem_post(id); (zwiększenie wartości)
sem_close(id);
sem_unlink(name);

//NIENAZWANE - analogicznie ale zamiast open i unlink:
//parametry: id, współdzielenie m. procesami 0/1, wartość
sem_init(&id, 0, counter);
sem_destroy(&id);
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


#define MASTER 0
#define TAG 'A'+'B'+'C'
//v
double x;
MPI_Status status;
MPI_Init(&argc, &argv);
//v
x=4;
//parametry: wysyłana tablica, ilość elementów, nazwa typu, cel/źródło, liczba ident. rodzaj danych, komunikator, -/status;
MPI_Send(&x, 1, MPI_DOUBLE, MASTER, TAG, MPI_COMM_WORLD);
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