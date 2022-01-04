
### Paginazione
Come la segmentazione, anche la paginazione è uno schema di gestione della memoria che permette agli indirizzi fisici di non essere contigui con la differenza che evita la frammentazione esterna, la necessità di compattazione e il dover sistemare blocchi di memoria in memoria ausiliaria.
La paginazione consiste nel dividere la memoria fisica in blocchi di dimensione fissa (frame) e suddividere la memoria logica in blocchi della stessa dimensione (pagine).
Quando si esegue un processo le sue pagine vengono prese dalla memoria ausiliaria/file system e caricate nei frame disponibili.

Ogni indirizzo generato dalla Cpu è diviso in due parti: 
- un numero di pagina (p), serve come indice per la tabella delle pagine contenente l'indirizzo di base in memoria fisica di ogni pagina.
- un offset di pagina (d), se unito con l'indirizzo indicato dal numero di pagina definisce l'inidirizzo della memoria fisica.

![[Pasted image 20211229171435.png]]
La dimensione di una pagina, così come quella di un frame è definita dall'architettura, normalemente è una potenza di 2 compresa tra 512 byte e 1GB.
Se la dimensione dello spazio degli indirizzi logici è $2^m$ e la dimensione di una pagina è di $2^n$ byte, allora gli *m – n* bit più significativi di un indirizzo logico indicano il numero di pagina, e gli n bit meno significativi indicano l’offset di pagina. 

&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp![[Pasted image 20211229172840.png]]

Ogni frame libero può essere assegnato ad un processo che ne abbia bisogno, evitando così la frammentazione esterna ma tuttavia si potrebbe verificare una frammentazione interna. Lo spazio richiesto da un processo non è multiplio delle dimensioni delle pagine, di conseguenza, l'ultimo frame potrebbe non essere completamente pieno causando la frammentazione. E' quindi conveniente usare pagine di piccole dimensioni per minimizzare la frammentazione.
Se la dimensione di un frame è di 4 kb ($2^12$), un sistema con voci della tabella delle pagine di 4 byte può indirizzare $2^44$ byte (o 16 Tb) di memoria fisica. 
La paginazione consente quindi di utilizzare una memoria fisica che è decisamente più grande rispetto a quella che potrebbe essere indirizzata dalla lunghezza del puntatore agli indirizzi della CPU.  

Quando si esegue un processo, il sistema esamina la sua dimensione in pagine, dato che ogni pagina del processo necessita di un frame, se il processo richiede n pagine, devono essere disponibili almeno n frame che, se ci sono, vengono assegnate al processo. Si carica la prima pagina in uno dei frame assegnati e s’inserisce il numero del frame nella tabella delle pagine relativa al processo in questione. Le pagine successive verrano a loro volta caricate in un frame il cui numero verrà inserito nella tabella delle pagine.

In un sistema paginato c'è una netta distinzione tra la memoria vista dal programmatore e l'effettiva memoria fisica. Il programmatore la vede come un unico spazio contiguo contenente i programmi, ma in realtà i programma sono sparsi nella memoria fisica ed è il SO a convertire gli indirizzi fisici negli indirizzi logici generati dai processi utenti.
Poichè la memoria fisica è gestita dal SO, esiste una **tabella dei frame** contenente un elemento per ogni frame, indicante se sia libero oppure assegnato e, se è assegnato, a quale pagina di quale processo.

Inoltre, il SO deve sapere che i processi utenti operano nello spazio utente, e tutti gli indirizzi logici si devono far corrispondere a indirizzi fisici. Il SO conserva  
una copia della tabella delle pagine per ciascun processo che serve per tradurre gli indirizzi logici in indirizzi fisici ogni volta che è necessario associare esplicitamente un indirizzo fisico a un indirizzo logico. Questa copia è usata anche dal dispatcher della Cpu per impostare l’hardware di paginazione quando a un processo sta per essere assegnata la Cpu. La paginazione fa quindi aumentare la durata dei context switch.


### Supporto hardware alla paginazione 

La tabella delle pagine è memorizzata diversamente a seconda del SO, nel caso più semplice, si usa uno specifico insieme di registri per garantire un’efficiente traduzione degli indirizzi di paginazione. E' importante che questi registri siano veloci perchè ogni accesso alla memoria passa attraverso il sistema di paginazione. Le istruzioni di modifica dei registri sono privilegiate, quindi solo il SO può modificare la mappa della memoria. 

L'uso di registri è efficiente se la tablla è di piccole dimensioni (circa 256 elementi), però i calcolatori moderni usano tabelle da un milione di elementi, i registri veloci non possono quindi essere usati. In questi casi la tabella delle pagine viene mantenuta nella memoria principale e un registro di base della tabella delle pagine (pTbr) punta alla tabella stessa. Il tempo dei context switch diminuisce considerevolmente perchè basta modificare questo registro per modificare la tabella. Questo metodo presenta un problema connesso al tempo necessario di accesso a una locazione della memoria utente. Per accedere alla locazione *i*, occorre far riferimento alla tabella delle pagine usando il valore contenuto nel pTbr aumentato dell’offset relativo alla pagina di *i*, perciò si deve accedere alla memoria. Si ottiene il numero del frame che, associato all’offset rispetto all’inizio della pagina, produce l’indirizzo cercato; a questo punto è possibile accedere alla posizione di memoria desiderata. Con questo metodo, per accedere a un byte occorrono due accessi alla memoria (uno per l’elemento della tabella delle pagine e uno per il byte stesso), quindi l’accesso alla memoria è rallentato di un fattore 2, un ritardo intollerabile.
##### TLB (translation look-aside buffer)
Per risolvere questo problema si usa una cache hardware, TLB, una memoria associativa ad alta velocità in cui ogni elemento è costituito da due parti, una chiave (o tag) e un valore.
Quando arriva un elemento il TLB lo confronta con tutte le chiavi e se trova una corrispondenza ne ritorna il valore associato. La ricerca è molto rapida e negli hardware moderni viene integrata nella pipeline delle istruzioni.

Hardware di paginazione con TLB:
![[Pasted image 20211230154249.png]]
La TLB contiene una piccola parte degli elementi della page table; quando la CPU genera un indirizzo logico, si cerca il suo numero di pagina nel TLB, se è presente, sarà subito disponibile il numero di frame e si usa per accedere alla memoria altrimenti si verifica un TLB miss e verrà consultata la page table. Se si verifica un miss, il numero di pagina e del frame, dopo essere stati presi dalla page table, vengono salvati nel TLB, se è piena, viene un rimosso un elemento, il modo varia a seconda della CPU (elemenro usato meno recentemente, round-robin, scelta casuale). Alcune TLB inoltre consentono a certi elementi di essere vincolati (wired down) ossia non possono essere rimossi. 

Alcune TLB memorizzano gli **identificatori dello spazio d'indirizzi  (ASID)** in ciascun elemento della TLB. Un ASID identifica un processo e si usa per fornirgli una protezione allo spazio d'indirizzi. Quando si tenta di trovare i valori dei numeri delle pagine virtuali, la TLB si assicura che l’ASID per il processo in esecuzione corrisponda all’ASID associato alla pagina virtuale in caso contrario si verifica un miss. 
L'ASID consente alla TLB di contenere nello stesso istante elementi di diversi processi.
Se la TLB non permette l'uso di ASID distinti, ad ogni cambio di page table (es: ad un context switch), si deve cancellare (flush) la TLB per evitare che ci siano errori di traduzione, potrebbero esserci indirizzi virtuali validi con i corrispondenti indirizzi fisici sbagliati.
La percentuale di volte che il numero di pagina di interesse si trova nella TLB è detta tasso di successi (hit ratio).
Calcolo del **tempo effettivo d'accesso alla memoria**:
 - hit ratio = 80%
 
 Caso in cui il numero sia contenuto nel TLB:
- tempo di accesso alla memoria (TLB): 100 ns

Caso in cui si verifica un TLB miss: 
- tempo di accesso alla page table e al numero di frame = 100 ns
- tempo per accedere al byte desiderato in memoria = 100 ns
- totale in caso di miss: 200 ns

Tempo effettivo di accesso = 0,80 x 100 + 0,20 x 200 = 120 ns

### Protezione
La protezione è assicurata dai bit di **protezione** associati a ogni frame, normalmente si trovano nella page table e un bit può determinare se una pagina si può leggere e scrivere o soltanto leggere. Mentre si calcola l'indirizzo fisico si possono controllare i bit di protezione per verificare che non si scriva in una pagina read-only, dovesse accadere si verificherebbe un'eccezione hardware per il SO, ossia una violazione della protezione della memoria. Usando più bit di protezione in combinazioni divesrse si possono creare diverse combinazioni di protezione (lettura, scrittura, esecuzione ...).

A ciascun elemento della tabella delle pagine viene associato un ulteriore bit detto di **validità** che, se impostato a valido, indica che la pagina è nello spazio d'indirizzi logici del processo, se non è nello spazio degli indirizzi il bit sarà impostato a non valido.
E' quindi possibile riconoscere gli indirizzi illegali e modificarli attraverso un'eccezione.
Il SO modificando questo bit può concedere o impedire l'accesso a una pagina.

Alcune architetture dispongono di registri di lunghezza della tabella delle pagine (PTLR), per indicare le dimensioni della tabella. Questo valore si controlla rispetto a ogni indirizzo logico per verificare che quest’ultimo si trovi nell’intervallo valido per il processo. Un errore causa la generazione di un’eccezione per il SO.

### Pagine Condivise

Con la paginazione è possibile condividere codice comune, funzione importante per il time sharing. Se per esempio aprissimo diversi text editor, il codice rientrare  anche detto codice puro, è lo stesso, ed è anche un codice non automodificante, ossia che non cambia a run time e che può quindi essere condiviso ed eseguito da più processi contemporaneamente, ciascuno dei quali disporrà della propria pagina di dati, dove conterrà i proprio dati, diversi per ogni processo. In memoria fisica è presente una sola copia dell'editor, la tabella delle pagine di ogni processo fa corrispondere gli stessi frame contenenti il text editor, mentre le pagine dei dati si fanno corrispondere a frame diversi.
In questo modo è possibile risparmiare memoria.


### Struttura della Tabella delle Pagine
 
- ##### Paginazione gerarchica.
I moderni calcolatori hanno uno spazio d'indirizzi logici molto grande, questo implica anche una tabella delle pagine molto grande che sarebbe meglio evitare di collocare in memoria centrale in modo contiguo. 
Per risolvere questo problema si può suddividere la page table in parti più piccole.
Un metodo consiste in un algoritmo di paginazione a due livelli (la tabella stessa è paginata). Con una macchina a 32 bit si avranno indirizzi logici suddivisi in offset(12 bit) e numero di pagina(20 bit), quest'ultimo suddiviso a sua volta in numero di pagina(10 bit) e offset(10 bit).
![[Pasted image 20211231185950.png]]
p1 = indice page table esterna o di primo livello
p2 = offset all’interno della pagina indicata dalla page table esterna.
Poiché la traduzione degli indirizzi si svolge dalla page table esterna verso l’interno, questo metodo è anche detto *tabella delle pagine  
ad associazione diretta*.
Nei casi in cui anche la tabella delle pagine esterna risulti troppo grande è possibile paginarla ed ottenere così una paginazione a 3 livelli e, se anche così risultasse troppo grande, si potrebbe paginare ulteriormente. 

- ##### Tabella delle pagine di tipo hash.
L'argomento della funzione hash  è il numero della pagina virtuale. Per la gestione delle collisioni, ogni elemento della tabella hash contiene una lista concatenata di elementi che la funzione hash fa corrispondere alla stessa locazione. 
Ogni elemento è composto da:  
&nbsp&nbsp&nbsp&nbsp1) il numero della pagina virtuale 
&nbsp&nbsp&nbsp&nbsp2) l'indirizzo del frame corrispondente alla pagina virtuale
&nbsp&nbsp&nbsp&nbsp3) un puntatore al successivo elemento della lista
Si applica la funzione hash al numero della pagina virtuale contenuto nell’indirizzo virtuale, identificando un elemento della tabella. Si confronta il numero di pagina virtuale con il campo (1) del primo elemento della lista concatenata corrispondente.  Se i valori coincidono, si usa l’indirizzo del relativo frame (2) per generare l’indirizzo fisico desiderato.
![[Pasted image 20220101170436.png]] 
Esiste una variante per spazi di indirizzamento a 64 bit, tabella delle pagine a gruppi (clustered page table), la differenza è che ciascun elemento della tabella  
hash contiene i riferimenti alle pagine fisiche corrispondenti a un gruppo di pagine  
virtuali contigue. Sono utili per gli spazi d’indirizzi sparsi, in cui i riferimenti alla memoria non sono contigui ma distribuiti per tutto lo spazio d’indirizzi.

- ##### Tabella delle pagine invertita
Normalmente si associa una page table a ogni processo, che facendo riferimento alle pagine attraverso gli indirizzi virtuali che il SO tradurrà poi in indirizzi fisisci. Questo metodo ha il difetto che ogni ciascuna tabella può contenere milioni di elementi e occupare grandi quantità di memoria fisica, necessaria solo per sapere com’è impiegata la rimanente memoria fisica.
Una possibile soluzione è usare una page table invertita.  Questa tabella ha un elemento per ogni pagina reale(frame). Ogni elemento è costituito dell’indirizzo virtuale della pagina memorizzata in quella reale locazione di memoria, con informazioni sul processo 	che possiede la pagina. 
Quindi, nel sistema esiste una sola tabella delle pagine che ha un solo elemento per ciascuna pagina di memoria fisica.
E' necessaria la presenza di un identificatore dello spazio d'indirizzi in ciascun elemento della page table, poichè sono contenuti molti indirizzi diversi assiociati alla memoria fisica, l'identificatore garantisce che una data pagina logica relativa a un certo processo sia associata alla pagina fisica corrispondente.
![[Pasted image 20220101175446.png]]
Esempio: Nell'IBM RT ciascun indirizzo virtuale è una tripla:  
*<id-processo, numero di pagina, offset>*
E ogni elemento è una coppia:
*<id-processo, numero di pagina>*
L' id-processo ha il ruolo di identificatore dello spazio d'indirizzi.
Quando si fa un riferimento alla memoria, si presenta una parte dell’indirizzo virtuale, formato da *<id-processo, numero di pagina>*, al sottosistema di memoria e si cerca una corrispondenza nella page table invertita. Se si trova, per es. sull’elemento i, si genera l’indirizzo fisico *<i, offset>* altrimenti si è tentato l'accesso ad un indirizzo ilegale.
Questo sistema riduce la quantità di memoria occupata però aumenta il tempo di ricerca, la table è ordinata per indirizzi fisici e le ricerche si fanno per indirizzi virtuali.  
Nei sistemi con page table invertita è difficile implementare la memoria condivisa perchè la condivisione si realizza con indirizzi virtuali multipli associati ad un unico indirizzo fisico. In questo caso però c'è un solo elemento indicante la pagina virtuale corrispondente a ogni pagina fisica e questa non può avere più di un indirizzo virtuale associato. Una possibile soluzione è porre nella page table una sola associazione fra un indirizzo virtuale e l’indirizzo fisico condiviso, ciò causerà page fault per ogni riferimento agli indirizzi virtuali non associati.
