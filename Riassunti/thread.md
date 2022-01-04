# I THREAD
Proprio come i processi, anche i thread sono delle **esecuzioni**: sono una sorta di sotto processi.
Per esempio se avessimo tale richiesta:
`calcolare il risultato di:`
`f1(x1, x2, x3, ..., xn) + f2(y1, y2, y3, ..., yn)`
potremmo comporre in parallelo f1 ed f2 e poi comporli insieme, avremmo due modi per fare ciò:

 - **applicando le conoscenze apprese fino ad ora**:
		 Utilizzare un processo padre che generi due processi figli P1 e P2 a cui assegnare rispettivamente f1 ed f2 per poi comporre i risultati, ma, soprattutto quando si ha a che fare con richieste contenute, generare un intero processo sarebbe abbastanza inutile ed ecco che viene in soccorso l'alternativa *thread*.
 - **applicando il concetto di thread**
		infatti i thread sono delle articolazioni dell'esecuzione interne al processo P e utilizzarli ci permetterebbe di rendere più efficiente tutto quanto il meccanismo.

## Definizione di thread (filo):

Sono delle articolazioni dell'esecuzione all'interno di un processo: elementi eseguibili in parallelo che però eseguono lo stesso P.
Prendendo n thread che condividono lo stesso processo questi avranno un loro personale pid, un loro personale stack e condivideranno il codice (ovvero le variabili globali utilizzate). Se un thread modifica una di queste variabili globali automaticamente gli altri thread che le condividono se le vedranno modificate 
--> modo per far comunicare i thread tra di loro.
Un thread può essere detto **utente** (se esiste soltanto al livello della libreria) oppure **kernel** (se esiste a livello del Sistema Operativo).

utente | kernel
--|--
| agisce a livello della libreria / processo| agisce a livello del Sistema Operativo
Il Sistema operativo non vede come è strutturato il processo e di conseguenza non vede i thread che esso contiene |il Sistema operativo vede come è strutturato il processo e di conseguenza potrà andare a fare lo scheduling della CPU a livello dei thread
|si possono usare thread su Sistemi Operativi qualsiasi, persino quelli che non sanno cosa sia un thread  | minore portabilità, perché SO mutlithread diversi implementano i thread in modo diverso |
| scheduling dei thread più adattibile ad esigenze particolari (maggiore adattibilità)|  |
| sincronizzazione dei contex switch più veloce perché è una scelta interna allo stesso processo | |
|  | rischio di sovraccarico e calo di prestazioni in caso si debbano gestire tanti thread contemporaneamente  |
| MA... se uno dei thread esegue un'operazione sospensiva automaticamente tutto il processo si blocca nonostante dei thread potrebbero comunque eseguire | INVECE... non si blocca tutto il processo perché ogni thread ha assegnata la propria CPU |
| parallelismo non reale perché il SO assegna sempre e comunque una sola CPU | il SO è in grado di assegnare la CPU ai singoli thread del processo e quindi permette il parallelismo | 



## lo scheduling
Il SO effettua lo scheduling della CPU utilizzando oggetti che può assegnare al processo in varia maniera.
Noi sappiamo che il SO effettua lo scheduling della CPU a partire dai PCB (process control block, che contengono le informazioni essenziali per la gestione del processo) oppure a livello dei thread. 
Per effettuare lo scheduling della CPU a livello dei thread è necessario associare thread utente a thread kernel.
Si possono seguire tre tipi di modelli per fare ciò:
### A) uno a uno (thread kernel):
Ogni thread utente ha una corrispondente unità di computazione che possiamo chiamare thread kernel, questo significa che se uso un thread che invoca una chiamata bloccante posso nel frattempo eseguire un altro thread. (scheduling tutto sul lato del SO).
Tuttavia avere molti thread kernel attivi potrebbe essere uno svantaggio (sovraccarico delle prestazioni), perciò molti SO che applicano questo modello (tra cui Linux stesso) limitano il numero di thread supportabili.
### B) uno a molti (thread utente):
in questo caso molti thread utente hanno assegnata un'unica unità di computazione (thread kernel), perciò la gestione dei thread risulta efficiente, anche se bloccante:
 infatti è impossibile eseguire thread multipli in parallelo e se un thread invoca una chiamata bloccante tutto il processo rimane bloccato (infatti lo scheduling agisce completamente a livello dei processi).
### C) molti a molti (thread utente e thread kernel):
Si tratta di una via intermedia:
a più thread utente viene assegnato un numero minore o uguale di thread kernel.
In questo modo si possono creare liberamente n thread che si ritengono necessari, si possono eseguire i corrispondenti kernel in parallelo e se c'è un thread che effettua una chiamata di sistema bloccante il kernel può fare nel frattempo lo scheduling di un altro thread tramite upcall (guardare pt. 2 per capire meglio).
Per arrivare alla CPU tramite il modello di tipo C lo scheduling è diviso in 2 parti:
1. **a carico del programmatore**: 
il programmatore decide quali dei suoi thread rendere visibili al sistema operativo e quali no, a questo punto il SO li usa e li esegue sulla CPU e una volta che quel thread ha finito il suo compito lo si cambierà con un altro thread.
2. **a carico del Sistema operativo**:
entrano in gioco gli LWP (light weight process): 
delle risorse che il SO assegna ai processi per effettuare lo scheduling dei thread utente al kernel, 
ogni LWP corrisponde ad un thread kernel.
Per esempio, supponiamo che un thread che va in esecuzione richieda una chiamata di sistema bloccante come la read, il SO, tramite l'**upcall**, avvisa il processo che un suo thread in esecuzione è passato allo stato waiting e così il processo può togliere al thread bloccato l'LWP per assegnarlo ad un altro thread fino a quando il primo non finirà la sua read. 
