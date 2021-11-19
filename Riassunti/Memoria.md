%%Appunti da Finire%%
## Binding degli indirizzi
Un programma per essere eseguito deve essere caricato dal disco alla memoria per essere eseguito, eventualemtne formando una **input queue**.

Una volta caricato, il processó fará molti passi prima di essere eseguito, durante i quali gli indirizzi potranno essere rappresentati in modi diversi.
Il compilatore bloccherá(**bind**) questi indirizzi simbolici a degli indirizzi rilocabili, per permettere al linker di convertirli in indirizzi assoluti.

Solitamente, il binding delle istruzioni e degli indirizzi di memoria avviene durante una delle seguenti fasi:
- **Compile time**
	- Se durante questa fase conosciamo dove il codice sará allocato, possiamo generare degli **indirizzi assoluti**
- **Load Time**
	- Se **non sappiamo** dove il programma sará locato durante la **compilazione** , il compilatore genererá degli **indirizzi rilocabili**, rimandando il binding degli indirizzi. Se l'indirizzo iniziale del codice  viene **alterato**, dobbiamo solamente **ricaricare** il **codice dell'utente**, poiché le librerie non sono state ancora caricate.
- **Execution Time**
	- Se il processo puó essere **spostato** durante la sua esecuzione, gli indirizzi del modulo eseguibile devono rimanere **rilocabili** fino all'esecuzione stessa

![[Pasted image 20211115123337.png]]

### Indirizzi fisici, logici e virtuali

Un indirizzo generato dalla CPU é definito **indirizzo logico**, mente un indirizzo visto dalla memoria(che puó essere caricato nel **MAR**) é definito **indirizzo fisico**, collegato all'indirizzo fisico di riferimento.

Il binding a che avviene a Compile-time e Load-time genera indirzzi fisici e logici identici, mentre quello che avviene a Execution-time no, generando quindi degli **indirizzi virtuali**.

#### Memory-Management unit

La gestione del collegamento tra indirizzi virtuali e fisici é a carico di un dispositivo hardware chiamato **memory-management unit**(**MMU**)

Possiamo scegliere diversi modi per mappare questi collegamenti, un'implementazione di questo consiste nel usare un **registro di riallocazione**, precedentemente chiamato **registro base**, dove era contenuto il piú **piccolo indirizzo fisico utilizzabile** dai processi, e aggiungere il valore contenuto in questo ad ogni registro generato dai processi utente.

![[Pasted image 20211115150857.png]]

Cosí facendo il processo utente non conoscerá mai l'indirizzo fisico reale, poiché avrá a che fare solo con gli indirizzi logici.

Sará quindi compito del **memory-mapping hardware** convertire tali indirizzi logici in indirrizzi fisici, e non é possibille conoscere la posizione finale di un indirizzo di memoria referenziato fino a che questo non é stato allocato.

### Linking e Loading
Con **Loading** intendiamo il processo mediante il quale il programma viene caricato in memoria prima di essere eseguito

Con Linking intendiamo il processo di composizione dei moduli che costituiscono un programma, dove ai nomi utilizzati da ciascun modulo e definiti in esso vengono associate le corrette definizioni.

Questi processi di dicono **statici** quando **precedono l'esecuzione**, mentre si dicono **dinamici** quando avvengono **durante l'esecuzione **.

Il loading dinamico consente una **migliore gestione della memoria**, poiché una routine non viene caricata in memoria fino al momento della sua **prima invocazione**, ma lascia al programmatore il compito di avvantaggiarsi di questa feature.

Il linking dinamico é simile al loading dinamico, **posponendo** il **linking** fino all'**esecuzione** del codice, senza caricare prima il codice in memoria. 
Con il linking dinamico includiamo un pezzo di codice,che ci permette di locare la porzione di memoria dove é presente il codice della routine e come caricarlo, se non é stato giá caricato,definito **stub**,  nella procedura in questione.

Se **aggiorno** una libreria dinamica, automaticamente **tutti i programmi** che usano la libreria faranno riferimento alla nuova versione anche **senza ricompilare**.

Tutto questo peró é possibile solo sui sistemi operativi che lo permettono

## Allocazione contigua
La memoria é solitamente divisa in 2 partizioni:
- una per il **sistema operativo** residente 
-  una per i **processi utente**

Poiché il vettore di interruzione dele partizioni é solitamente locato nella porzione bassa della memoria, si posiziona il sistema nella medesima porzione.

![[Pasted image 20211115161859.png]]

Possiamo inoltre implementare meccanismi di protezione e mappatura della memoria allocata utilizzando un registro di riallocazione un registro di limite, limitando l'area di memoria disponibile ai processi utente e allocando dinamicamente gli indirizzi logici.

Per poter permettere anche al sistema operativo di alterare lo spazio a sua disposizione rendiamo una parte del suo codice transiente, che puó essere allocato e disallocato in base alla necessitá.

### Gestione dell'allocazione
#### Partizioni della stessa dimensione
Uno dei metodi piú semplici per allocare la memoria é dividerla in partizioni di dimensione statica, le quali possono contenere solo un processo.

Quando una partizione si liberasi seleziona un processo dalla coda e lo si carica su questa.

#### Partizioni di dimensione variabile
In uno schema a partizioni di dimensione dinamica tutta la memoria é disponibile ed é considerata un unico blocco(o **buco**) che puó essere frammentato in piú **buchi**.

Ogni partizione rappresenta un processo che compete per la CPU, che puó avere dimensione diversa da tutti gli altri.

In ogni momento il **SO** conosce **quanti** buchi sono presenti e la loro **dimensione**.
Se due buchi sono **adiacenti** questi vengono **uniti**.


#### Fit

Come allocare i programmi sui buchi rappresenta un problema di allocazione della memoria dinamica e presenta diverse soluzioni:
- **First fit**
	- Carichiamo il programma sul primo buco abbastanza grande.
- **Best fit**
	- Carichiamo il programma sul primo buco abbastanza grande per contenerlo.
- **Worst fit**
	- Carichiamo il programma sul buco piú grande disponibile.

Le simulazioni dimostrano che il Best e il First fit sono meglio del Worst in termini di utilizzo della memoria, e che il First Fit é il piú veloce.

##### Frammentazione
Con frammentazione si intende lo spezzettamento della memoria in tante parti. 

Si dice che si ha::
- **frammentazione esterna**
	-  se queste parti sono abbastanza grandi da essere utilizzabili
- **frammentazione interna**
	-  se sono molto piccoli, praticamente inutilizzabili. 
	-  In questo caso il frammento viene unito alla partizione precedente.

Sia le strategie di First che di Best fit soffrono di **frammentazione esterna**, ma in particolare con il **first** fit per ogni blocco allocato, mezzo blocco viene reso inutilizzabile(**regola del 50%**)

Per ridurre la frammentazione esterna si applicano di tanto in tanto politiche di compattamento, unendo tutta la memoria libera in un'unico blocco.
Ció é possibile solo a tempo di esecuzione.

# Swapping

Per eseguire un processo questo deve trovarsi nella memoria centrale. 

Un processo, tuttavia, può essere **temporaneamente spostato** in una memoria ausiliaria (backing store) e in seguito riportato in memoria per continuare l’esecuzione, attraverso un procedimento chiamato **avvicendamento dei processi in memoria** o **swapping**.

Questo avviene quando non cé abbastanza posto sulla RAM per caricare un processo che deve essere eseguito.

Il **backing store** deve inoltre essere abbastanza ampia da contenere le copie di tutte le immagini di memoria di tutti i processi utenti.

![[Pasted image 20211119154047.png]]

Lo swapping é composto da **2 fasi**:
- Una fase di **swap in**, dove si carica l'immagine del processo dalla memoria secondaria alla principale.
- Una fase di **swap out**, dove si carica l'immagine del processo di cui si interrompe l'esecuzione nella memoria secondaria.

Il tempo necessario al completamento dello swapping è dato dal 

**tempo di swap-out + tempo di swap-in**

quindi dipende dalle **dimensioni** delle immagini dei processi e dal **tempo** di trasferimento. Usiamo inoltre i **millisecondi**(*ms*) come unitá di misura.

La collocazione dipende dal quando viene effettuato il binding delle variabili: 
- se il codice **non è rilocabile** allora l'immagine del processo dovrà rioccupare la **stessa sezione** di RAM 
- se è **rilocabile** (in particolare, se il binding è dinamico) questo non è necessario

Per poter effettuare lo swapping il processo deve essere **completamente inattivo**(soprattutto riguardo le operazioni di I/O).

# Paginazione