# Introduzione
Un **sistema operativo** è un insieme di programmi **software** che gestisce l'**hardware** di un calcolatore, fornisce una piattaforma ai programmi applicativi e agisce da intermediario fra l’utente e il calcolatore.

Un moderno calcolatore  è composto da una o più CPU e da un certo numero di controllori di dispositivi connessi attraverso un canale di comunicazione comune (bus) che permette l’accesso alla memoria condivisa dal sistema.

L'**avvio** dell'elaboratore avviene attraverso un sistema di **bootstrap**, un meccanismo secondo il quale un processo é in grado di avviarne un'altro, a partire dal **BIOS** (Basic I/O System), tipicamente contenuto nella ROM

![[Pasted image 20220131121040.png]]

Il kernel, una volta caricato, é in grado di caricare all'avviamento altri processi eseguiti anche all'esterno del kernel, chiamati **Daemon**(demoni).

### Gestione dei segnali

Inoltre l'SO é un sistema **event driven**, quindi esso rimane in attesa di un segnale che puó essere di due tipi:
- **Interrupt**: causati da dispositivi **hardware** ed inviati alla CPU attraverso il bus. Quando riceve questo tipo di segnale la CPU interrompe l'esecuzione e avvia l'handler immediatamente,effettuando quindi un **context switch**, per poi riprendere l'esecuzione del programma interrotto.
- **Trap**: causati da **programmi**, possono codificare errori o richieste di esecuzioni speciali(system call o supervisor call)

Ad ogni evento corrisponde una routine di gestione predefinita detta **event-handler**, gestire un evento significa effettuare un **dispatch**, cioè individuare ed eseguire la routine associata all'evento.

In generale gli eventi possono essere accumulati in una coda.

Per conssentire un'accesso veloce agli handler, ad ognuno di essi é associato un'indice di un vettore, denominato **vettore delle interruzioni**.

Il vettore delle interruzioni è mantenuto nella **memoria bassa** (es. prime 100 posizioni), l'accesso avviene in modo diretto tramite un indice codificato nello stesso **segnal**e che notifica l'evento, **senza** bisogno di un vero **dispatcher**.

### Gestione della memoria
La CPU può caricare istruzioni **esclusivamente** dalla **memoria**, quindi tutti i programmi da eseguire devono esservi caricati sopra. Gli elaboratori moderni eseguono i programmi in una memoria riscrivibile, la **RAM** (Random Access Memory).

Quando un programma deve essere eseguito, le istruzioni vengono **prelevate** dalla memoria principale(**fetch**), e trasferite nel **registro d'istruzione**. Quí l'istruzione verrá decodificata in attesa dell'esecuzione, e una volta terminata l'esecuzione si puó scrivere il **risultato** sulla **memoria principale**.

Idealmente, si vorrebbe cconservare tutti i programmi e i dati in RAM, ma questo **non** è **possibile** per i seguenti **due** motivi: 
1. La **capacità** della memoria centrale **non** è di solito **sufficiente** a contenere in modo permanente tutti i programmi e i dati richiesti; 
2. La memoria centrale è un dispositivo di **memorizzazione volatile**, che perde il proprio contenuto quando l’alimentazione elettrica viene spenta o si interrompe.

Per queste ragioni la maggior parte dei sistemi elaborativi comprende una **memoria secondaria** come estensione della memoria centrale, per conservare in modo **permanente** grandi quantitá di informazione.

L’ampio ventaglio dei sistemi di memorizzazione può essere ordinato secondo una **scala gerarchica**:

![[Pasted image 20220131145419.png]]

I livelli più alti sono più veloci, ma anche più dispendiosi.

### Struttura di I/O
Un calcolatore general-purpose è composto da una CPU e da un insieme di **controllori** di dispositivi connessi mediante un bus comune.

Ciascun controllore si occupa di un particolare **tipo di dispositivo** e può gestire una o più unità a esso connesse.

Un controllore di dispositivo dispone di una **propria memoria** interna (**buffer**), e di un insieme di **registri** specializzati. Il controllore è responsabile del **trasferimento** dei **dati** tra i **dispositivi** periferici a esso connessi e la propria memoria interna.

I sistemi operativi in genere possiedono, per **ogni** controllore di dispositivo, un **driver** del dispositivo che si coordina con il controllore e funge da **interfaccia** con il resto del sistema.

Un'operazione di I/O su un dispositivo é cosí definita:
- Per avviare un’operazione di I/O, il **driver** del dispositivo **carica** gli appropriati **registri** all’interno del controllore, il quale, esamina i contenuti di questi registri per determinare l’azione da intraprendere 
-  Il **controllore** comincia a **trasferire** i dati dal **dispositivo** al proprio **buffer**. 
-  A trasferimento completato, il controllore informa il **driver**, tramite un’interruzione, di avere **terminato** l’operazione. 
-  Il driver passa quindi il controllo al sistema operativo, restituendo i dati  se l’operazione è di lettura
-   per altre operazioni, il driver restituisce delle informazioni di stato.

Questa forma di I/O è adatto al trasferimento di **piccole quantità** di dati, ma in caso di trasferimenti massicci può generare un pesante sovraccarico (**overhead**).

Per risolvere questo problema si utilizza le tecnica dell’accesso diretto alla memoria (**DMA**). 

Una volta impostati i buffer, i puntatori e i contatori necessari al dispositivo di I/O, il controllore trasferisce un **intero blocco** di dati dal proprio buffer **direttamente** nella **memoria centrale** senza alcun intervento da parte della CPU. In questo modo l’operazione richiede una sola interruzione per ogni blocco di dati trasferito.

![[Pasted image 20220131151315.png]]

### Architettura degli elaboratori
- **Monoprocessore**: una CPU. Talvolta presenti processori ausiliari dedicati ad attività specifiche
- **Multiprocessore**:
	- Maggiore capacità elaborativa (**throughput**)
	- Economia di scala
		- Più processori condividono device e risorse
	- Incremento dell’affidabilità
		- un guasto a una CPU non impedisce alle altre di funzionare

I sistemi multiprocessore attualmente in uso sono di **due tipi**:
- **Multielaborazione asimmetrica** (asymmetric multiprocessing), in cui a ogni processore si assegna un compito specifico
- **Multielaborazione simmetrica** (symmetric multiprocessing, SMP), in cui ogni processore è abilitato all’esecuzione di tutte le operazioni del sistema.

e si dividono in 2 ulteriori categorie:
- **Single core**: Ogni CPU è realizzata su un chip separato 
- **Multi core**: Uno stesso chip contiene diverse CPU 
	- la comunicazione all’interno di un singolo chip è più veloce

### Dual mode
Per garantire il corretto funzionamento del sistema è necessario **distinguere** tra l’**esecuzione** di codice del **sistema** operativo e di codice **utente**. 

Il metodo seguito dalla maggioranza dei sistemi operativi consiste nel disporre di un supporto hardware che consente di distinguere differenti modalità di funzionamento.

Sono necessarie almeno due diverse modalità: 
1. **modalità utente** 
2. **modalità kernel**

Per indicare quale sia la **modalità attiva**, l’architettura della CPU deve essere dotata di un bit, chiamato **bit di modalità**, che vale:
- **0** se in modalità kernel 
-  **1** se in modalità utente

Al **bootstrap** il bit di modalità è inizializzato a 0 .

Con la richiesta di esecuzione di processi utente, il bit di modalità cambierà valore, a seconda del processo che richiede l’esecuzione della prossima istruzione.

Le **chiamate di sistema**(system call) sono gli strumenti con cui un programma **utente** **richiede** al sistema operativo di compiere **operazioni** a esso riservate, per conto del programma utente.

L'utente non puó aver accesso all'istruction set del SO in alcun altro modo.

Durante l'esecuzione di una system call il bit di modalità vale 0.

![[Pasted image 20220131153714.png]]
