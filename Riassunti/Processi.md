# PROCESSI 
##### Concetto di processo<br>

Informalmente un processo è un programma in esecuzione. 
Non comprende solo il codice effettivo del programma,noto come "sezione di testo",
ma  anche: <br>

- l'attività corrente, rappresentata dal valore del _program counter_ e del contenuto dei registri della cpu.<br>

- normalmente comprende un proprio _stack_, che a sua volta contiene i dati temporanei come i parametri di una procedura, gli indirizzi di rientro e le variabiili locali e una _sezione locale_  <br>

- un processo volendo puó includere anche uno _$heap$_ ossia della memoria dinamicamente allocata 



## STATO DEL PROCESSO

Durante l'esecuzione un processo é soggetto a cambiamenti del suo stato:
 - $NUOVO$ -->si crea il processo
 - $RUNNING$ _oppure Esecuzione_ --> Le sue istruzioni vengono eseguite
 - $WAITING$ _oppure Attesa_ --> il processo attende che si verifichi qualche evento, ad esempio il completamento di un'operazione di I/O o la ricezione di un segnale
 - $READY$ _pronto_ --> il processo attende di essere assegnato a un'unitá d'elaborazione
 - $TERMINATO$ il processo ha terminato l'esecuzione
 
 *NB*-->in ciascuna unitá di elaborazione puó essere in esecuzione solo un processo per volta, sebbene molti processi possano essere in stato _ready_ o in stato _waiting_
 
 ---
 ## BLOCCO DI CONTROLLO DEI PROCESSI
 (Blocco di controllo dei processi ==> process control block ==> PCB)
 E' una sorta di deposito per tutte le informazioni relative ai vari processi
 
 Contiene molte informazioni connesse a un processo specifico:
 - Stato del processo (nuovo,running,waiting etc etc)
 - Program Counter --> contiene l'indirizzo dell'istruzione successiva
 - Registri della cpu
 - Informazioni sullo scheduling della cpu
 - informazioni sulla gestione della memoria
 - informazioni di contabilizzazione --> quota di uso della cpu e il tempo d'utilizzo di questa
 - informazioni sullo stato di I/O -->info sulla lista dei dispositivi di I/O assegnati a un determinato processo.
---
## SCHEDULING 
 Lo scheduler dei processi seleziona un processo da eseguire dall’insieme di quelli disponibili
 
 ### CODE
 Ogni processo é inserito in una coda di processi(job queue),I processi presenti in memoria centrale, che
sono pronti e nell’attesa d’essere eseguiti, si trovano in una lista detta coda dei pro-
cessi pronti (coda ready), questa generalmnente viene memorizzata  come una lista che contiene i puntatori al primo e all'ultimo PCB dell'elenco e ogni PCB comprende un campo puntatore che indica il successivo processo contenuto nella coda dei processi pronti.

### SCHEDULER
Nel corso della sua esistenza un processo si trova in varie code di scheduling, il SO incaricato di selezionare i processi dalle suddette code, compie la selezione per mezzo di un opportuno scheduler.
Poiché accade che si sottopongano piú processi di quanti ne possano essere eseguiti immediatamente, questi generalmente si trasferiscono in dispositivi di memoria secondaria, generalmente dischi, dove rimarranno fino al momento dell'esecuzione.<br>
Ci sono 3 tipi di schedulers

_SCHEDULER A LUNGO TERMINE_:
sceglie i processi da questo insieme e li carica in memoria affinché siano eseguiti

_SCHEDULER A BREVE TERMINE_:
Fa la selezione tra i processi pronti per l'esecuzione e assegna la cpu a uno di loro.

_SCHEDULER A MEDIO TERMINE(SWAPPING)_:
I processi che non sono richiesti immediatamente vengono eliminati dalla memoria pricipiale e successivamente ricaricato dallo scheduler a medio termine.

---
### CAMBIO DI CONTESTO
Detto anche context switch, quando il passaggio della CPU a un nuovo processo implica il salvataggio dello stato attuale del processo e il ripristino dello stato del nuovo processo. Quinidi il sistema salva lo stato del processo uscente nel suo PCB e carica il contesto del processo entrante.

---
### CREAZIONE DI UN PROCESSO

Durante la sua esecuzione un processo può creare numerosi nuovi processi.
I creanti si chiama processo "padre", quelli creati "figli", ognuno di questi può creare altri processi, formando una struttura ad albero, detto albero di processi. La maggior parte dei SO identificano un processo per mezzo di un numero detto "pid" (process identifier)

#### TERMINAZIONE DI UN PROCESSO
Quando un processo termina  (finisce l'esecuzione della sua ultima istruzione) e inoltra al SO la richiesta attraverso  sa system call```exit()```
a questo punto il processo figlio può riportare un'informazione di stato al processo genitore, che li riceve attraverso la system call ```wait()```
Ma un processo può terminare anche per altre ragioni, ad esempio, un processo può terminare l'esecuzione di un altro processo attraverso una system call, generalmente solo il proces so padre può invocare una chiamata simile, generalmente un processo padre può terminare l'esecuzione di uno dei suoi processi anche per questi motivi:<br>
- Il processo figlio ha ecceduto nell’uso di alcune tra le risorse che gli sono state
assegnate. ciò richiede che il processo genitore disponga di un sistema che esa-
mini lo stato dei propri processi figli. <br>
- Il compito assegnato al processo figlio non è più richiesto. <br>
•Il processo genitore termina e il sistema operativo non consente a un processo fi-
glio di continuare l’esecuzione in tale circostanza. <br>

NB -->Un processo che è terminato, ma il cui genitore non ha ancora chiamato la
wait(), è detto processo zombie.
