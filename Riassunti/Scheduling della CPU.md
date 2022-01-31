## Scheduling della CPU
Ogni volta che la cpu passa nello stato d’inattività, il sistema operativo sceglie di eseguire uno dei processi presenti nella **ready queue**.

In particolare, è lo scheduler a **breve termine**, o **scheduler della cpu** che sceglie quello cui assegnare la cpu tra i processi *ready*

La ready queue non é sempre di tipo FIFO, ma puó essere anche una coda con prioritá o una lista concatenata non ordinata.

### Scheduling con prelazione

Le **decisioni** riguardanti lo **scheduling** della cpu vengono prese nelle seguenti **quattro** circostanze:
1. un processo passa dallo stato di **esecuzione** allo stato ***waiting***
2. un processo passa dallo stato di **esecuzione** allo stato ***ready***
3. un processo passa dallo stato di **attesa** allo stato **pronto**
4. un processo **termina**

Quando lo scheduling interviene solo nelle condizioni **1** e **4**, si dice che lo schema di scheduling è **senza prelazione** (*non preemptive*) ,**altrimenti** lo schema di scheduling è **con prelazione** (*preemptive*)

### Dispatcher
 É il **modulo** che passa effettivamente il controllo della cpu al processo scelto dallo scheduler a breve termine. 
 
 Questa funzione comprende: 
 - Il **Context switch**
 - Il passaggio alla **modalità corretta**
 - Il salto alla giusta posizione del programma per riavviarne l’esecuzione. 

Il tempo richiesto dal dispatcher per fermare un processo e avviare l’esecuzione di un altro è noto come **latenza** di dispatch.

### Criteri di scheduling
Per poter scegliere l'algoritmo di scheduling **piú adatto** da implementare utilizziamo i seguenti criteri:
- **Utilizzo** della **CPU**
	- La CPU deve restare il piú attiva possibile
- **Produttività**(*throughput*)
	- il numero di processi completati nell'unità di tempo
- **Tempo di completamento**
	- la somma non solo del tempo di esecuzione e dei vari I/O ma anche di tutti i tempi di attesa legati allo scheduling
- **Tempo d’attesa**
	- la somma degli intervalli d’attesa passati in questa coda
- **Tempo di risposta**
	- Il tempo che intercorre tra la **effettuazione** di una **richiesta** e la **prima** **risposta** prodotta

È importante aumentare al **massimo** **utilizzo e produttività** della cpu, mentre il tempo di **completamento**, il tempo d’**attesa** e il tempo di **risposta** si devono ridurre al **minimo**.

## Algoritmi di scheduling 
### First-come First-served
É Il più semplice algoritmo di scheduling della cpu.

con questo schema la cpu si assegna al processo che la richiede per primo, basandosi sulla realizzazione di una coda FIFO.

Il tempo medio d’attesa per quest'algoritmo é abbastanza lungo:

prendendo come esempio la seguente tabella

Processo | Durata della sequenza| Tempo d'attesa
--|--|--|
$$P_1$$|$$24$$|$$0$$
$$P_2$$|$$3$$|$$24$$
$$P_3$$|$$3$$|$$27$$

il tempo d'attesa medio sará $(0 + 24 + 27)/3=17ms$, ma con l'ordine dei processi invertito sarebbe stato piú basso, quindi il tempo d'attesa medio con questá lgoritmo dipende dall'ordine dei processi nella coda.

Questo algoritmo puó generare un **effetto convoglio**,in cui tutti i processi **attendono** che un lungo processo liberi la cpu, il che causa una riduzione dell’utilizzo della cpu e dei dispositivi rispetto a quella che si avrebbe se si eseguissero per primi i processi più brevi.

L’algoritmo di scheduling FcFS è **senza prelazione**: una volta che la cpu è assegnata a un processo, questo la trattiene fino al momento del rilascio.

### Shortest Job First
Questo algoritmo **associa** a ogni **processo** la **lunghezza** della successiva **sequenza** di operazioni della cpu(*cpu burst*).

Quando la CPU é disponibile si assegna ad essa il processo con cpu burst piú basso, minimizzando il tempo medio di attesa.

La **durata** di una CPU burst **non** é **nota** a priori, ma ne possiamo prevedere la durata sulla base dei burst precedenti, combinati con una media esponenziale:

$$τ_{n+1}=\alpha t_n+(1-\alpha)τ_n$$

dove 
- $0\le \alpha \le 1$
- $t_n$ indica la lunghezza dell'n-esima cpu-burst e contiene le informazioni piú recenti
- $τ_n$ registra la storia passata

Espansa la formula diventa :
$$τ_{n+1}=\alpha t_n+\alpha(1-\alpha)t_{n-1}+...+\alpha(1-\alpha)^jt_{n-j}+...+(1-\alpha)^{n+1}τ_0$$

Di solito α è minore di 1.

L’algoritmo può essere **sia** **con** prelazione **sia** **senza** prelazione. 
La scelta si presenta quando alla ready queue arriva un nuovo processo mentre un altro processo è ancora in esecuzione. 

Il nuovo processo può richiedere cpu burst **più breve** di quella che resta al processo correntemente in esecuzione. Un algoritmo SjF con prelazione **sostituisce** il processo attualmente in esecuzione, mentre un algoritmo SjF senza prelazione **permette** al processo correntemente in esecuzione di portare a **termine** la propria sequenza di operazioni.

Il modello con prelazione é anche definito **shortest remaining time first**.

### Scheduling a Priorità
Questo algoritmo **associa** ad ogni **processo** una **prioritá**, e **assegna** la CPU ad un processo in base a questa. I processi con **prioritá uguale** vengono ordinate secondo il principio **FIFO**.

Le priorità si possono definire:
- **internamente** 
	- Si usano una o più **quantità misurabili** per calcolare la priorità del processo
- **esternamente**
	- Si definiscono le prioritá secondo criteri **esterni** al sistema operativo(l'importanza del processo,...)

Un problema importante relativo agli algoritmi di scheduling con priorità è l’**attesa indefinita** (***starvation***):un processo potrebbe non ottenere mai la CPU perché continuano a passargli davanti nuovi processi a priorità maggiore.

Una soluzione al problema  è costituita dall’**invecchiamento** (***aging***), una tecnica di aumento graduale delle priorità dei processi che attendono nel sistema da parecchio tempo.

### Round Robin
L’algoritmo di **scheduling circolare** (*round robin*) è simile allo scheduling FcFS, ma **aggiunge** la capacità di **prelazione** in modo che il sistema possa commutare fra i vari processi.

Ciascun processo riceve una piccola **quantità fissata** del tempo della CPU, chiamata **quanto di tempo**(*time slice*) e la **ready queue** é trattata come una coda **circolare**

Lo **scheduler** della cpu **scorre** la ready **queue**, assegnando la CPU a ciascun processo per un intervallo di tempo della durata **massima** di un **quanto** di tempo,se **non** è **sufficiente** a concludere, il processo é soggetto a **prelazione** e viene **reinserito** in **coda** ready e la CPU riassegnata al successivo.

Il tempo d’attesa medio per il criterio di scheduling rr è spesso abbastanza lungo.

Ogni processo viene servito ogni (n_proc – 1) * quanto millisecondi.

Le prestazioni dell’algoritmo **dipendono** molto dalla **dimensione** del **quanto** di tempo. 
- Nel caso limite in cui il **quanto** di tempo sia **molto lungo**, il criterio di scheduling rr si riduce al criterio di scheduling **FcFS**. 
- Se il **quanto** di tempo è **molto breve**, il criterio rr può portare a un numero **elevato** di **cambi di contesto**.

Quindi il quanto di tempo deve essere ampio rispetto alla durata del cambio di contesto.

### Scheduling a code multilivello
Un algoritmo di **scheduling a code multilivello** (*multilevel queue scheduling algorithm*) **suddivide** la **ready** queue in code **distinte**. 

I processi si assegnano in modo permanente a una coda, generalmente secondo qualche caratteristica del processo.

Ogni coda ha il proprio algoritmo di scheduling. 

Esiste una priorità fra le code (eventualmente associata a meccanismi di invecchiamento per evitare starvation).

Se ai processi è consentito cambiare coda allora si parla di multilivello con **feedback**.