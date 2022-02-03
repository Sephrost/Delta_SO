
### Paginazione
La **paginazione** è uno schema di **gestione** della **memoria** che permette agli indirizzi fisici di non essere contigui evitando la frammentazione esterna, la necessità di compattazione e il dover sistemare blocchi di memoria in memoria ausiliaria.

Consiste nel dividere la memoria fisica in blocchi di dimensione fissa (frame) e suddividere la memoria logica in blocchi della stessa dimensione (pagine).
Quando si esegue un processo le sue pagine vengono prese dalla memoria ausiliaria/file system e caricate nei frame disponibili.

Ogni indirizzo generato dalla Cpu è diviso in due parti: 
- un numero di pagina (p), serve come indice per la page table contenente l'indirizzo di base in memoria fisica di ogni pagina.
- un offset di pagina (d), se unito con l'indirizzo indicato dal numero di pagina definisce l'inidirizzo della memoria fisica.

![[Pasted image 20211229171435.png]]

Dimensione dello spazio degli indirizzi logici: $2^m$ byte
dimensione di una pagina : $2^n$ byte
gli *m – n* bit più significativi di un indirizzo logico indicano il numero di pagina, e gli n bit meno significativi indicano l’offset di pagina. 

&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp![[Pasted image 20211229172840.png]]

Per evitare la frammentazione esterna, ad un processo che ne ha bisogno può essere assegnato ogni frame libero. Si potrebbe però verificare una frammentazione interna, poichè lo spazio richiesto da un processo non è multiplio delle dimensioni delle pagine, l'ultimo frame potrebbe non essere completamente pieno.

Quando si esegue un processo, il sistema esamina la sua dimensione in pagine, se il processo richiede *n* pagine, devono essere disponibili almeno *n* frame che, se ci sono, vengono assegnate al processo. Si carica la pagina in uno dei frame assegnati e s’inserisce il numero del frame nella page table relativa al processo. 

Esiste una **tabella dei frame** contenente un elemento per ogni frame, indicante se sia libero oppure assegnato e, se è assegnato, a quale pagina di quale processo. Il SO conserva una copia della page table per ciascun processo, e serve per tradurre gli indirizzi logici in fisici ogni volta che è necessario associare un indirizzo fisico a uno logico. Questa copia è usata anche dal dispatcher della CPU per impostare l’hardware di paginazione quando a un processo sta per essere assegnata la CPU. La paginazione fa quindi aumentare la durata dei context switch.

### Supporto hardware alla paginazione 
In caso di tablle di piccole dimensioni (~ 256 elementi), per avere un’efficiente traduzione degli indirizzi di paginazione, si usa un insieme di registri che devono essere veloci perchè ogni accesso alla memoria passa attraverso il sistema di paginazione.

Nei calcolatori moderni le tabelle hanno molti più elementi e i registri non sono utilizzabili, le tabelle vengono quindi tenute in memoria principale e un Registro di Base della Tabella delle Pagine (PTBR) punta alla tabella stessa. Basta modificare il PTBR per modificare la tabella, così diminuendo il tempo dei context switch.

Problema per il tempo di accesso alla memoria: per accedere alla locazione *i*, occorre far riferimento alla page table usando il valore del PTBR aumentato dell’offset della pagina di *i*. Si ottiene il numero del frame che, associato all’offset produce l’indirizzo cercato; ora è possibile accedere alla memoria desiderata. Per accedere a un byte occorrono due accessi alla memoria (uno per l’elemento della page table e uno per il byte stesso).

##### TLB (translation look-aside buffer)
E' una cache hardware in cui ogni elemento è costituito da una chiave (o tag) e un valore. Quando arriva un elemento il TLB lo confronta con tutte le chiavi e se trova una corrispondenza ne ritorna il valore associato. 

Hardware di paginazione con TLB:
![[Pasted image 20211230154249.png]]
La TLB contiene una parte degli elementi della page table; quando la CPU genera un indirizzo logico, si cerca il suo numero di pagina nel TLB, se è presente, sarà subito disponibile il numero di frame altrimenti si verifica un TLB miss e verrà consultata la page table. In caso di miss, il numero di pagina e del frame, dopo essere stati presi dalla page table, vengono salvati nel TLB, se è piena, viene rimosso un elemento, il modo varia a seconda della CPU.  Alcune TLB inoltre consentono a certi elementi di essere vincolati (wired down) ossia non possono essere rimossi. 

Alcune TLB memorizzano gli **identificatori dello spazio d'indirizzi  (ASID)** in ciascun elemento della TLB. Un ASID identifica un processo e fornisce una protezione allo spazio d'indirizzi. Quando si tenta di trovare i valori dei numeri delle pagine virtuali, la TLB si assicura che l’ASID per il processo in esecuzione corrisponda all’ASID associato alla pagina virtuale, in caso contrario si verifica un miss.

L'ASID consente alla TLB di contenere nello stesso istante elementi di diversi processi. 
Se la TLB non permette l'uso di ASID distinti, ad ogni cambio di page table (es: context switch), si deve cancellare la TLB per evitare che ci siano errori di traduzione.

Calcolo del **tempo effettivo d'accesso alla memoria**:
 - hit ratio (% di volte che il numero di pagina si trova nella TLB)= 80%
 
 Caso in cui il numero sia contenuto nel TLB:
- tempo di accesso alla memoria (TLB): 100 ns

Caso in cui si verifica un TLB miss: 
- tempo di accesso alla page table e al numero di frame = 100 ns
- tempo per accedere al byte desiderato in memoria = 100 ns
- totale in caso di miss: 200 ns

Tempo effettivo di accesso = 0,80 x 100 + 0,20 x 200 = 120 ns

### Protezione
E' assicurata dai bit di protezione associati a ogni frame. Un bit può determinare se una pagina si può leggere e scrivere o soltanto leggere. Mentre si calcola l'indirizzo fisico si possono controllare i bit di protezione per verificare che non si scriva in una pagina read-only. Usando più bit di protezione in combinazioni divesrse si possono creare diverse combinazioni di protezione (lettura, scrittura, esecuzione ...).

A ogni elemento della page table viene associato un bit detto di **validità** che, se impostato a valido, indica che la pagina è nello spazio d'indirizzi logici del processo, non valido altrimenti. E' quindi possibile riconoscere gli indirizzi illegali e modificarli con un'eccezione. Il SO modificando questo bit può concedere o impedire l'accesso a una pagina.
 
 Bit di validità (v) o non validità (i) in una page table:
&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp![[Pasted image 20220106184944.png|500]]
Alcune architetture dispongono di registri di lunghezza della tabella delle pagine (PTLR), per indicare le dimensioni della tabella. Questo valore si controlla rispetto a ogni indirizzo logico per verificare che quest’ultimo si trovi nell’intervallo valido per il processo. 

### Pagine Condivise
Possibilità di condividere codice comune, torna utile in un ambiente time sharing. Alcuni programmi possiedono del codice rientrante (o codice puro) che non è automodificante (non cambia a runtime) e che può quindi essere condiviso ed eseguito da più processi contemporaneamente.

Ogni processo avrà la propria pagina dati associata ad un frame diverso mentre il codice rientrante sarà unico in memoria fisica e tutte le page table si riferiranno agli stessi frame. E' così possibile risparmiare memoria.

### Struttura della Tabella delle Pagine
 
- ##### Paginazione gerarchica.
In caso di page table di grandi dimensioni, è meglio evitare di collocarle in memoria centrale in modo contiguo. E' possibile però suddividere la page table in parti più piccole usando un algoritmo di paginazione a due o più livelli in cui la tabella stessa è paginata.

Es. con indirizzi logici suddivisi in offset(12 bit) e numero di pagina(20 bit), quest'ultimo suddiviso a sua volta in numero di pagina(10 bit) e offset(10 bit).
![[Pasted image 20211231185950.png]]
 &nbsp&nbsp&nbsp&nbspp1 = indice page table esterna o di primo livello
 &nbsp&nbsp&nbsp&nbspp2 = offset all’interno della pagina indicata dalla page table esterna
Poiché la traduzione degli indirizzi si svolge dalla page table esterna verso l’interno, questo metodo è anche detto *tabella delle pagine  
ad associazione diretta*.
![[Pasted image 20220107213918.png]]

- ##### Tabella delle pagine di tipo hash.
L'argomento della funzione hash  è il numero della pagina virtuale. Ogni elemento della tabella hash contiene una lista concatenata di elementi che la funzione hash fa corrispondere alla stessa locazione. 
Ogni elemento è composto da:  
&nbsp&nbsp&nbsp&nbsp1) il numero della pagina virtuale 
&nbsp&nbsp&nbsp&nbsp2) l'indirizzo del frame corrispondente alla pagina virtuale
&nbsp&nbsp&nbsp&nbsp3) un puntatore al successivo elemento della lista
Si applica la funzione hash al numero della pagina virtuale contenuto nell’indirizzo virtuale, identificando un elemento della table. Si confronta il numero di pagina virtuale con il campo (1) del primo elemento della lista concatenata corrispondente.  Se i valori coincidono, si usa l’indirizzo del relativo frame (2) per generare l’indirizzo fisico desiderato.
![[Pasted image 20220101170436.png]] 

- ##### Tabella delle pagine invertita
Normalmente si associa una page table a ogni processo solo che ciascuna tabella può contenere milioni di elementi e occupare grandi quantità di memoria fisica, necessaria solo per sapere com’è impiegata la rimanente memoria fisica. 

Una soluzione è usare una page table invertita. Questa tabella ha un elemento per ogni pagina reale(frame) ed è costituito dell’indirizzo virtuale della pagina memorizzata in quella reale locazione di memoria, con informazioni sul processo che possiede quella pagina. 

Quindi, nel sistema esiste una sola page table che ha un solo elemento per ciascuna pagina di memoria fisica.

Poichè ci sono molti indirizzi diversi assiociati alla memoria fisica, è presente un identificatore dello spazio d'indirizzi in ogni elemento della table che garantisce che una data pagina logica sia associata alla pagina fisica corrispondente.

![[Pasted image 20220101175446.png]]

La table è ordinata per indirizzi fisici e le ricerche si fanno per indirizzi virtuali, ciò causa una riduzione della memoria occupata ma un aumento del tempo di ricerca.
La memoria condivisa si realizza con indirizzi virtuali multipli associati ad un unico indirizzo fisico. In questo caso c'è un solo elemento indicante la pagina virtuale corrispondente a ogni pagina fisica.

La pagina fisica non può avere più di un indirizzo virtuale associato, risulta quindi diffilcile implementare la memoria condivisa. Una soluzione è porre nella table una sola associazione fra un indirizzo virtuale e l’indirizzo fisico condiviso, ciò causerà page fault per ogni riferimento agli indirizzi virtuali non associati. 

