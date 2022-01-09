# Memoria Virtuale
Per realizzare la memoria virtuale sono necessari:
1. Algoritmo di Sostituzione delle Pagine 
1. Algoritmo di allocazione dei frame     

## **Memoria RAM**

- Allocazione con **caricamento totale**
    - VINCOLO sullo spazio occupabile dai processi
    - VINCOLO sul programmatore
    - Il grado di MULTIPROGRAMMAZIONE cresce
    - La Memoria necessaria potrebbe essere inferiroe a quella Caricata
- Allocazione mediante **memoria virtuale**
    - PAGINAZIONE

### PAGINA INVALIDA
2 Definizioni:
- Pagina non caricata in RAM
- Pagina alla quale si effettua accesso in maniera illecita ( *es. OS che assegna ad un processo spazio extra per frammentazione e si effetua la lettura su queste pagine per frammentazione* )

### COPIATURA SU SCRITTURA
In caso di fork il processo figlio clona la tabella delle pagine del padre, P ed F condividono lo stack fino a quando uno dei due non lo modifica. Quando questo avviene si copia lo stack modificato in un frame libero
- Aumento l'efficienza di gestione della RAM

### POOL OF FREE FRAMES
In caso di PAGE FAULT la pagina da caricare viene scritta in un frame libero mentre la pagine vittima diventa un frame libero ( *che può essere sovrascritto* )

Se il processo ha bisogno della pagina vittima questa è già presente in RAM e basta linkarla 
- Evito la copiatura da disco

## **Memoria Secondaria** - *area di swap*
- Vista come estensione della RAM accedibile solo in caso di PAGE FAULT
- Contiene dati ( *stack* ), non codice ( *file system* )



# Algoritmi di Sostituzione delle Pagine
Quando la richiesta di una pagina porta a page fault [^1]:
1. Verifico la validità della pagina
2. Identifico un frame libero
3. Se lo trovo
   1. Avvio caricamento pagine ( *processo in waiting* )
   2. Aggiorno la tabella delle pagine del processo
   3. Processo ready
4. Altrimenti
   1. Trovo una pagina vittima
   2. Effettuo la sostituzione
      1. Salvo la pagina vittima su disco [^2]
      2. Copio la pagina che ha causato page fault nello stesso frame
   
Gestione del page fault:
1. Gestione dell'interruzione $\simeq 10^-6$ secondi
2. Lettura della pagina mancante $\simeq 10^-3$ secondi
3. Riavvio del processo $\simeq 10^-6$ secondi

---
## FIFO - *First In First Out*
Associa ad ogni pagina un timestamp

![FIFO example](img/Memoria%20Virtuale/FIFO.png)

Pro:
+ Facile da implementare

Contro:
- Le prestazioni possono essere molto scarse in certi casi
- I timestamp crescenti possono andare in overflow
- Statisticamente, soffre di anomalia di Belady: il numero di page fault non decresce sempre alláumentare dei frame

## Algoritmo Ottimale - *OPT/MIN*
Si basa sul comportamento futuro dei processi

![OPT example](img/Memoria%20Virtuale/Algoritmo%20Ottimale.png)

Pro:
+ Minimizza il numero di page fault

Contro:
- Non è implementabile

## Least Recently Used
Si parte dall'assunzione che le pagine utlizzate di recente saranno utilizzate anche nell'immediato futuro e sceglie come vittime quelle usate meno recentemente

![LRU example](img/Memoria%20Virtuale/Least%20receently%20used.png)

Pro:
+ Non soffre dell'anomalia di Belady

Contro:
- Possibilità di overflow 
- Necessaria ricerca sequenziale di pagine valide

## Algoritmo con bit supplementare di riferimento 
*Richiede implementazione hardware*

Utilzza un bit supplementare impostato a 1 quando viene effettuato il riferimento a una pagina e 0 quando non viene effettuato nessun riferimento per un certo lasso di tempo

![Algoritmo con BIT supplementare example](img/Memoria%20Virtuale/Algoritmo%20con%20bit%20supplementare%20di%20riferimento.png)

## Algoritmo di seconda chance
*bit di riferimento \+ FIFO*

Le pagine in RAM sono organizate con coda FIFO circolare.

Non definisco nessun marcatore temporale.

Associo ad ogni pagina solamente il bit di riferimento anziché un registro.

Quando cerco una vittima (seguendo l'ordine FIFO):
1. Se il bit vale 1 viene impostato a 0
2. Se trovo un bit 0 scelgo quella pagina come vittima

Può essere migliorato aggiungendo un secondo bit di riferimento che indica la modifica

Con 2 bit (RIFERIMENTO; MODIFICA) l'ordine diventa:

**00 < 01 < 10 < 11**

## Least frequently used 
Assegno a ciascuna pagina un contatore che tiene conto del numero di volte in cui una pagina è stata utilzzata

Pro:
+ Riduco il rischio di overflow in quanto il contatore cresce più lentamente

La pagina vittima viene scelta tra le pagine con contatore più basso

Due page fault consecutivi possono portare all'immediata eliminazione della prima pagina caricata in qaunto possiede un contatore basso, appena inizializato

Contro:
- Sfavorisce le pagine caricate recentemente in RAM

## Most frequently used
Stessa implementazione del precedente ma sceglie come vittima una delle pagine più utlizzate 

*Si suppone che una pagina con contatore olto alto è in RAM da tanto tempo e a breve non servirà più*

+ Si comporta meglio di "Least Frequently Used"

# Algoritmi di Allocazione dei Frame

**TIPOLOGIE**:
- LOCALE/GLOBALE
- STATICA/DINAMICA

### ALLOCAZIONE LOCALE
Normalmente la richiesta di una pagina da un processo con tutti i frame occupati porta a page fault

### ALLOCAZIONE GLOBALE
Invece di effettuare page fault, un processo a priorità più alta di un altro può "rubare" da questi un frame libero

Possibili problemi:
- **Trashing**: processi a priorità bassa con troppi pochi frame per lavorare senza continui page fault, CPU inutilizzata perché si aspetta continuamente la lettura da disco
- **Stallo**: "CPU poco utilizzata" può portare a caricamento in RAM di ulteriori processi, nonostante quelli già caricati siano a corto di memoria
### PAGINAZIONE SU RICHIESTA - *Lazy Swapping/Paginazione On-Demand*
Assegna 0 pagine alla creazione di un processo, quando la CPU fa riferimento a una pagina invalida, questa viene caricata 

### ALTRI MECCANISMI DI PAGINAZIONE
L'OS assegna almeno il numero liberi di frame calcolato sull'architettura per eseguire una singola istruzione

### ALLOCAZIONE RAM
- Processi Utente
- Processi Kernel [^3]

---
## Allocazione Uniforme
Ogni processo riceve la stessa quantità di frame

## Allocazione Proporzionale
- Calcolo [^4] $ V = VM_{1} + VM_{2} + \dots + VM_{i} + VM_{n} $
- Calcolo [^5] $ m_{i} = \frac{VM_{i}}{V}M $

Se nasce un nuovo processo si riesegue l'assegnazione per ridistribuire i frame (possibilmente non utlizzati) - *allocazione dinamica*

---

## Modello del Working Set
*Uso il concetto di pagina attiva, voglio evitare il trashing*

- I processi usano insiemi di pagine diversi all'interno della propria esecuzione
- Il processo esegue senza problemi se gli assegno almeno il numero di pagine attive per ciascun insieme (*working set* [^6])
- Devo memorizzare le pagine alle quali il processo ha effetuato l'accesso nelle ultime $\delta$[^7] richieste 
- L'OS deve "indovinare" il momento in cui il processo cmabia working set
- Se la **PAGE FAULT FEQUENCY** cresce si aumenta il numero di frame assegnati dall'OS

$ |\omega S_{p}| $ : cardinalità del WF di un processo
$ \sum_{i} |\omega S_{p_{i}}| $[^8] : numero complessivo di frame necessari per tutti i processi

### Prepaginazione - *swapping*
Quando un proceso viene "swapped-out" dalla memoria RAM, memorizzo il working set del processo in memoria secondaria. Il processo viene "swapped-in" quando le pagine del working set si liberano

## Mappatura Virtuale 
1. <U>Mappatura dei file</U>: il processo P effettua open/read(file)
![Operazione su un file](img/Memoria%20Virtuale/Mappatura%20virtuale.png)
2. <U>Mappatura dei device</U>: nella RAM c'è una porzione riservata ai device

---

## Allocazione RAM a processi kernel
### SISTEMA BUDDY - *gemellare*
- Inizio con un solo segmento contiguo
- se necessito di ulteriori segmenti divido a metà il segmento e così via
- se un buddy e il suo gemello sono liberi li ricongungo

![Buddy example](img/Memoria%20Virtuale/Buddy%20System.png)

L'efficienza di questo sistema non è ottima, ad esempio un segmento di 65K necessita di un buddy da 128K nell'esempio precedente

### CACHE DI SLAB - *virtuale, non fisica*
Inizializzo un array uniforme, inizialmente con tutti i segmenti della slab con descrittori liberi *(la contiguità dell'array riduce la frammentazione interna)*

- se la **SLAB** si riempie ne creo un'altra 
- L'insieme di SLAB che contengono elemnti dello stesso tipo prende il nome di cache 



[^1]: *durante la fase di fetch o execute, operazione costosa*
[^2]: *solo se ha associato un dirty bit con valore 1, che sta ad indicare che la copia in RAM è diversa da quella su disco*
[^3]: *necessitano di conservare tabelle molto grandi*
[^4]: *virtual memory richiesta da ogni processo, sommata, per eseguire*
[^5]: *per calcolare il numero di frame da assegnare a ciascun processo*
[^6]: *ricalcolato a intervalli regolari*
[^7]: *circa $10^4$*
[^8]: *serve per decidere se è necessario mandare in swap alcuni processi*